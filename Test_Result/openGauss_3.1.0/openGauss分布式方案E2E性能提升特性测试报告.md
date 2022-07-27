![avatar](../../images/openGauss.png)

版权所有 © 2021  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述                     | 作者       |
| --------- | ----------- | ---------------------------- | ---------- |
| 2022-7-22 | 1.0         | 特性测试报告初稿完成         | peilinqian |
| 2022-7-27 | 1.1         | 修改测试组网信息和缩略词信息 | peilinqian |
|           |             |                              |            |

 关键词： 

 ShardingSphere-Proxy、性能提升、async-profile 

摘要：

本文档主要介绍了ShardingSphere-Proxy针对openGauss进行E2E性能提升改造内容，及针对改造进行性能提升比对测试，并给出最终测试结论。

缩略语清单：

| 缩略语         | 英文全名                                   | 中文解释                                                     |
| -------------- | ------------------------------------------ | ------------------------------------------------------------ |
| TPC            | Transaction Processing Performance Council | 事务处理性能委员会。                                         |
| TPC-C          | Trade Promotion Coordination Committee     | 在线事务处理（OLTP）的基准程序。                             |
| tpmC           | transaction per minute                     | TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试。 |
| ss-proxy       | ShardingSphere-Proxy                       | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务。 |
| async-profiler | async-profiler                             | async-profiler 是一个性能开销小、不受保存点偏差影响的采样分析器，基于 HotSpot 定义的 API 实现堆栈跟踪与内存分配采样，可以用于 OpenJDK、Oracle JDK 及其他基于 HotSpot JVM 的 JRE。能够跟踪不限于以下几类性能相关的事件：CPU 周期、硬件和软件性能计数器、Java 堆中的分配、锁跟踪。 |

# 1     特性概述

ss-proxy 实现了 openGauss 协议，支持 openGauss 特有的批量协议等。ss-proxy 在前端支持了批量协议，但后端执行 SQL 的时候还是会逐个请求执行。本次优化将充分利用 openGauss 特有的批量协议，优化 ss-proxy openGauss 的批量操作性能。

ShardingSphere代码中存在通过捕获异常控制流程的逻辑。由于异常产生时Throwable的fillInStackTrace方法开销较大，对性能产生一定的影响，需要在代码主流程中避免不必要的异常实例创建。

另外，本次优化将借助 async-profiler 工具追溯 ShardingSphere 内核、数据分片的代码逻辑，对热点代码中不必要的逻辑、锁进行优化，确保热点代码不存在因锁导致的并发性能受限的情况。

特性改造后实现如下目标：

- 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后单线程 E2E 延时减少 20%、性能提升 15% 
- 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后 250 线程 E2E 延时减少 10%、 性能提升 15%
- 6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 优化后 250 线程 E2E 延时减少 10%、 性能提升 15%
- 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 连跑30min，ss-proxy ⽆热点锁 
- 1 节点 openGauss + 1 节点 ss-proxy（5.1.1版本） + 1 节点 BenchmarkSQL 连跑24H ，正常无报错



# 2     特性测试信息

| 版本名称                 | 测试起始时间 | 测试结束时间 |
| ------------------------ | ------------ | ------------ |
| ss-proxy 5.0.0（优化前） | 2022-7-20    | 2022-7-21    |
| ss-proxy 5.1.1（优化后） | 2022-7-22    | 2022-7-26    |

| 硬件型号                                       | 硬件配置信息                                                 | 备注                   |
| ---------------------------------------------- | ------------------------------------------------------------ | ---------------------- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 3<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | openGauss数据库节点    |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | ss-proxy节点、tpcc节点 |

| 软件名称       | 软件版本                                                     | 备注                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BenchmarkSQL   | 定制版本<br />commit：27b4eef1b6d499af68dccd33a9a1fd758f47a7db | 修改随机参数使完美sharding，增加负载均衡参数。<br />取包地址：<br />https://gitee.com/opengauss_sharding/benchmarksql |
| aarch64 jdk    | 17.0.1                                                       |                                                              |
| ss-proxy       | ShardingSphere-Proxy 5.0.0 <br />ShardingSphere-Proxy 5.1.1  | sharding官网发布包                                           |
| zookeeper      | 3.7.0                                                        |                                                              |
| openGauss      | openGauss 3.0.1                                              |                                                              |
| async-profiler | async-profiler-2.7-linux-arm64                               | 工具链接：<br />https://github.com/jvm-profiling-tools/async-profiler |



# 3     测试结论概述

## 3.1   测试整体结论

共计执行5个用例，主要覆盖了优化前后的性能比对测试和稳定性测试，未发现问题，整体质量良好。

| 测试活动     | 活动评价                                                     |
| ------------ | ------------------------------------------------------------ |
| 性能比对测试 | （1）1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后单线程 E2E 延时减少 30.9%、性能提升32.5%；符合需求预期。<br />（2）1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后 250 线程 E2E 延时减少 60.1%、 性能提升105.47%；符合需求预期。<br />（3）6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 优化后 250 线程 E2E 延时减少 50.0%、 性能提升 66.38%；符合需求预期。<br />（4）1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 连跑30min，ss-proxy ⽆热点锁；符合需求预期。 |
| 稳定性测试   | 1 节点 openGauss + 1 节点 ss-proxy（5.1.1版本） + 1 节点 BenchmarkSQL 连跑24H ，正常无报错。 |

## 3.2   约束说明

- 本报告中，使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力；且benchmarkSQL为定制包（针对分布式进行改造，进行完美sharding），如果用其他版本包，性能会有所降低。
- 本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异，本次测试环境具体情况，请参考第二章节。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0        | 0    | 0    | 0    | 0      |

# 4     测试执行

## 4.1  测试组网

**1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 测试组⽹：**

• 1 节点 openGauss：IP1 

• 1 节点 ss-proxy 5.0.0 / 5.1.1：IP2 

• 1 节点 BenchmarkSQL：IP3 

• 1 节点 ZooKeeper：IP3 

**6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL测试组⽹**：

• 6 节点 openGauss：IP1~IP6 

• 6 节点 ss-proxy 5.0.0 / 5.1.1：IP7~IP12 

• 3 节点tpcc：IP13~IP15

• 1 节点zookeeper：IP15 

## 4.2  测试步骤&结果

### 4.2.1  1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL ，单线程优化前后NEW_ORDER延时及tpmc比对

**测试步骤:**

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与1节点config-sharding.yaml 

（2）启动 ss-proxy 进程

（3）启动 BenchmarkSQL 预热10min后待tpmc稳定后，发压 5min，BenchmarkSQL 关键配置如下： 

```
terminals=1 
runTxnsPerTerminal=0 
runMins=5 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
useProxy=true   //设置为true，收集SQL延时信息
```

（4）停⽌ ss-proxy 进程

（5）收集 BenchmarkSQL tpmC

**测试结果：**

| 版本  | 测试结果                                                     |
| ----- | ------------------------------------------------------------ |
| 5.0.0 | 延时(avg NEW_ORDER)：9.873<br/>tpmc：4251.57                 |
| 5.1.1 | 延时(avg NEW_ORDER)：6.818<br/>tpmc：5632.86                 |
| 结论  | 延时下降百分比：(9.873-6.818)/9.873 ≈ 30.9%<br />tpmc性能提升：(5632.86-4251.57)/4251.57 ≈ 32.5% |

### 4.2.2  1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL ， 250 线程优化前后NEW_ORDER延时及tpmc比对

**测试步骤:**

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与1节点的config-sharding.yaml 

（2）启动 ss-proxy 进程

（3）启动 BenchmarkSQL 预热10min后待tpmc稳定后，发压 5min，BenchmarkSQL 关键配置如下： 

```
terminals=250 
runTxnsPerTerminal=0 
runMins=5 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
useProxy=true   //设置为true，收集SQL延时信息
```

（4）停⽌ ss-proxy 进程 

（5）收集 BenchmarkSQL tpmC

**测试结果：**

| 版本  | 测试结果                                                     |
| ----- | ------------------------------------------------------------ |
| 5.0.0 | 延时(avg NEW_ORDER)：39.434<br/>tpmc：270146.46              |
| 5.1.1 | 延时(avg NEW_ORDER)：15.708<br/>tpmc：555074.93              |
| 结论  | 延时下降百分比：(39.434-15.708)/39.434≈ 60.1%   >10%<br />tpmc性能提升：(555074.93-270146.46)/270146.46≈ 105.47%   >15% |

### 4.2.3 6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL，250 线程优化前后NEW_ORDER延时及tpmc比对

**测试步骤** 

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与6节点的config-sharding.yaml 

（2） 同时启动 6 节点 ss-proxy 进程

（3）同时启动 3 节点 BenchmarkSQL 预热10min后待tpmc稳定后，发压 5min，BenchmarkSQL 关键配置如下： 

```
terminals=250 
runTxnsPerTerminal=0 
runMins=5 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
useProxy=true //设置为true，收集SQL延时信息
```

（4）停⽌所有 ss-proxy 进程

（5）收集所有 BenchmarkSQL tpmC

**测试结果：**

| 版本  | 中间数据                                                     | 测试结果                                                     |
| ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 5.0.0 | job1 avg NEW_ORDER 18.502 <br />job2 avg NEW_ORDER 18.908 <br />job3 avg NEW_ORDER 19.599 <br />total avg NEW_ORDER 19.003<br /><br />job1 tpmC 	540479.54 <br />job2 tpmC 	537228.26 <br />job3 tpmC 	515467.31 <br />total tpmC	1593175.11 | 延时(avg NEW_ORDER)：19.003<br/>tpmc：1593175.11             |
| 5.1.1 | job1 avg NEW_ORDER 9.436<br/>job2 avg NEW_ORDER 9.6<br/>job3 avg NEW_ORDER 9.455<br/>total avg NEW_ORDER 9.497<br /><br />job1 tpmC 	897867.86<br/>job2 tpmC 	879339.59<br/>job3 tpmC 	873438.27<br/>total tpmC	2650645.72 | 延时(avg NEW_ORDER)：9.497<br/>tpmc：2650645.72              |
| 结论  |                                                              | 延时下降百分比：<br />(19.003-9.497)/19.003≈ 50.0%   >10%<br />tpmc性能提升：<br />(2650645.72-1593175.11)/1593175.11≈ 66.38%   >15% |

### 4.2.4 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 连跑30min，ss-proxy热点锁统计

**测试步骤**

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与1节点的config-sharding.yaml 

（2）启动 ss-proxy 进程

（3） 启动 async-profiler 连接 ss-proxy 进程采样

```
./profiler.sh start -e cpu --lock 10ms -f shardingsphere-proxy-opengauss-%t.jf r $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}')
```

（4） 启动 BenchmarkSQL 发压 30 分钟，BenchmarkSQL 关键配置如下： 

```
terminals=250 
runTxnsPerTerminal=0 
runMins=30 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
```

（5） 结束 async-profiler 采样： 

```
./profiler.sh stop $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}')
```

（6）停⽌ ss-proxy 进程

（7）收集 shardingsphere-proxy-opengauss-%t.jfr ⽂件，其中 %t 会被替换为⽇期时间。 通过 jfr 命令统计进⼊临界区事件数： 

```
jfr summary shardingsphere-proxy-opengauss-${⽇期时间}.jfr | grep JavaMonitorEn ter | awk '{print $2}'
```

**测试结果**

5.0.0统计进⼊临界区事件数：8 < 10000

5.1.1统计进⼊临界区事件数：0 < 10000

注：查询统计数值⼩于 10000结果均正常

### 4.2.5 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 连跑24H

**测试步骤**

以下测试步骤在 ss-proxy 5.1.1 执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与1节点的config-sharding.yaml 

（2）启动 ss-proxy 进程

（3）启动 BenchmarkSQL 发压 24H，BenchmarkSQL 关键配置如下： 

```
terminals=500 
runTxnsPerTerminal=0 
runMins=1440 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
```

**测试结果**

连跑结果正常，ss-proxy、tpcc均无异常日志



## **4.3 测试执行统计数据**

| **版本名称**               | **测试用例数** | **用例执行结果**        | **发现问题单数** |
| -------------------------- | -------------- | ----------------------- | ---------------- |
| ShardingSphere-Proxy 5.0.0 | 4              | Passed：4<br/>Failed：0 | 0                |
| ShardingSphere-Proxy 5.1.1 | 5              | Passed：5<br/>Failed：0 | 0                |

数据项说明：

测试用例数：5个，测试通过：5个；

发现问题单数：0个。

## **4.4   后续测试建议**

无

# **5     附件**



****



****

****
