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

 ShardingSphere-Proxy、性能提升、async-profile 、jdbc

摘要：

本文档主要介绍了openGauss分布式方案后端jdbc线程池模型优化改造内容，及针对改造进行性能提升比对测试，并给出最终测试结论。

缩略语清单：

| 缩略语         | 英文全名                                   | 中文解释                                                     |
| -------------- | ------------------------------------------ | ------------------------------------------------------------ |
| TPC            | Transaction Processing Performance Council | 事务处理性能委员会。                                         |
| TPC-C          | Trade Promotion Coordination Committee     | 在线事务处理（OLTP）的基准程序。                             |
| tpmC           | transaction per minute                     | TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试。 |
| ss-proxy       | ShardingSphere-Proxy                       | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务。 |
| async-profiler | async-profiler                             | async-profiler 是一个性能开销小、不受保存点偏差影响的采样分析器，基于 HotSpot 定义的 API 实现堆栈跟踪与内存分配采样，可以用于 OpenJDK、Oracle JDK 及其他基于 HotSpot JVM 的 JRE。能够跟踪不限于以下几类性能相关的事件：CPU 周期、硬件和软件性能计数器、Java 堆中的分配、锁跟踪。 |

# 1     特性概述

ShardingSphere 使用 openGauss JDBC 驱动与 openGauss 数据库通讯。JDBC 是同步阻塞模型的 API，ShardingSphere 的后端会存在一定数量的线程用于执行 JDBC 方法。由于目前已有的协程技术方案不成熟，且在性能上并不理想，本次后端 JDBC 优化不使用协程。

ss-proxy 前端 Netty 是事件驱动的异步 I/O 框架，前端接收到的请求会被投入后端线程池，由后端线程执行，在这一过程中会存在跨线程的开销。在 ss-proxy 客户端数量相对明确的场景下，可以通过统一前后端线程池，使用同一条线程完成前后端逻辑，减少线程切换的开销。

ShardingSphere 在使用 JDBC API 的过程中，为尽量减少资源的占用，会在每次请求前创建 Statement 与 PreparedStatement，并在请求结束后释放当次请求所使用的 Statement / PreparedStatement。即使 JDBC 驱动内部会有缓存实现，频繁的创建、销毁还是会存在一定的开销。对此，本次优化会在ShardingSphere 内部增加对应的 Statement / PreparedStatement 维护逻辑，减少频繁创建、销毁 Statement / PreparedStatement 的开销。

对于同一个 PreparedStatement，SQL 是固定不变的，只有参数会改变。对此，本次优化将尽量复用如 SQLStatementContext 这类对象，减少频繁创建对象的开销。

特性改造后实现如下目标：

- 1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后 Proxy 的 CPU 使⽤率下降 10% 
- 6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 优化后 Proxy 的 CPU 使⽤率下降 10%

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

共计执行2个用例，主要覆盖了优化前后的性能比对测试，未发现问题，整体质量良好。

| 测试活动     | 活动评价                                                     |
| ------------ | ------------------------------------------------------------ |
| 性能比对测试 | （1）1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化后Proxy 的 CPU 使⽤率下降 54.6%；符合需求预期。<br />（2）6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 优化后Proxy 的 CPU 使⽤率下降 56.8%；符合需求预期。 |

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

### 4.2.1  1 节点 openGauss + 1 节点 ss-proxy + 1 节点 BenchmarkSQL 优化前后Proxy 的 CPU 使⽤率比对

**测试步骤**

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与1 节点 config-sharding.yaml 

（2）启动 ss-proxy 进程

（3）启动 async-profiler 连接 ss-proxy 进程采样： 

```
 ./profiler.sh start -e cpu --lock 10ms -f shardingsphere-proxy-opengauss-%t.jfr $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}') 
```

（4）启动 BenchmarkSQL 发压 30 分钟。限制每分钟事务数为 1,066,666（约 480,000 tpmC），关键配置如下： 

```
terminals=500 
runTxnsPerTerminal=0 
runMins=30 
limitTxnsPerMin=1066666 
terminalWarehouseFixed=true 
```

（5）结束 async-profiler 采样

```
./profiler.sh stop $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}') 
```

（6）停⽌ ss-proxy 进程

（7）获取 shardingsphere-proxy-opengauss-%t.jfr ⽂件，其中 %t 会被替换为⽇期时间，通过以下命令累计

CPU 使⽤率： 

```
jfr print --events CPULoad shardingsphere-proxy-opengauss-${⽇期时间}.jfr | awk '/jvmUser/ { us += $3; jvm += $3 } ; /jvmSystem/ { sy += $3; jvm += $3 } ; /machineTotal/ { total += $3 } END { print "JVM us:",us,"\nJVM sy:",sy,"\nJVM total:",jvm,"\nMachine Total:",total }' 
```

**测试结果：**

| 版本  | 测试结果                                                     |
| ----- | ------------------------------------------------------------ |
| 5.0.0 | JVM us: 1285.64<br/>JVM sy: 208.633<br/>JVM total: **1494.27** |
| 5.1.1 | JVM us: 465.351<br/>JVM sy: 212.804<br/>JVM total: **678.155** |
| 结论  | (1494.27-678.155)/1494.27 = 54.6% >10%                       |

### 4.2.2 6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 优化前后Proxy 的 CPU 使⽤率比对

**测试步骤**

以下测试步骤分别对 ss-proxy 5.0.0 和 ss-proxy 5.1.1 各执⾏⼀次： 

（1）配置 ss-proxy 的 server.yaml 与6节点 config-sharding.yaml

（2） 启动 ShardingSphere-Proxy 进程 

（3）启动 async-profiler 连接 ss-proxy 进程采样： 

```
./profiler.sh start -e cpu --lock 10ms -f shardingsphere-proxy-opengauss-%t.jfr $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}') 
```

（4） 启动同时启动 3 节点 BenchmarkSQL 发压 30 分钟。 限制每分钟事务数为 2,133,332（约 960,000 tpmC），关键配置如下： 

```
terminals=1000 
runTxnsPerTerminal=0 
runMins=30 
limitTxnsPerMin=2133332 
terminalWarehouseFixed=true 
```

（5）结束 async-profiler 采样： 

```
./profiler.sh stop $(jps -l | grep 'org.apache.shardingsphere.proxy.Bootstrap' | awk '{print $1}') 
```

（6） 停⽌ ss-proxy 进程 

（7） 获取所有 ss-proxy 所在机器 shardingsphere-proxy-opengauss-%t.jfr ⽂件，通过以下命令累计 CPU 使⽤率： 

```
jfr print --events CPULoad shardingsphere-proxy-opengauss-${⽇期时间}.jfr | awk '/jvmUser/ { us += $3; jvm += $3 } ; /jvmSystem/ { sy += $3; jvm += $3 } ; /machineTotal/ { total += $3 } END { print "JVM us:",us,"\nJVM sy:",sy,"\nJVM total:",jvm,"\nMachine Total:",total }' 
```

**测试结果：**

| 版本  | 测试结果                                                     |
| ----- | ------------------------------------------------------------ |
| 5.0.0 | proxy1 JVM total:	1635.31<br/>proxy2 JVM total:	1625.47<br/>proxy3 JVM total:	1635.94<br/>proxy4 JVM total:	1626.45<br/>proxy5 JVM total:	1624.77<br/>proxy6 JVM total:	1621.81<br/>JVM total:	**9769.75** |
| 5.1.1 | proxy1 JVM total:	707.499<br/>proxy2 JVM total:	704.303<br/>proxy3 JVM total:	685.555<br/>proxy4 JVM total:	699.31<br/>proxy5 JVM total:	708.924<br/>proxy6 JVM total:	713.407<br/>JVM total:	**4218.998** |
| 结论  | (9769.75-4218.998)/9769.75 = 56.8% >10%                      |

## **4.3 测试执行统计数据**

| **版本名称**               | **测试用例数** | **用例执行结果**        | **发现问题单数** |
| -------------------------- | -------------- | ----------------------- | ---------------- |
| ShardingSphere-Proxy 5.0.0 | 2              | Passed：2<br/>Failed：0 | 0                |
| ShardingSphere-Proxy 5.1.1 | 2              | Passed：2<br/>Failed：0 | 0                |

数据项说明：

测试用例数：2个，测试通过：2个；

发现问题单数：0个。

## **4.4   后续测试建议**

无

# **5     附件**



****



****

****
