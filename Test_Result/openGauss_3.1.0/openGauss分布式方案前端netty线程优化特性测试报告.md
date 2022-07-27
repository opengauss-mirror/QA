![avatar](../../images/openGauss.png)

版权所有 © 2021  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述             | 作者       |
| --------- | ----------- | -------------------- | ---------- |
| 2022-7-22 | 1.0         | 特性测试报告初稿完成 | peilinqian |
|           |             |                      |            |
|           |             |                      |            |

 关键词： 

 ShardingSphere-Proxy、性能提升、并发度提升

摘要：

本文档主要介绍了ShardingSphere-Proxy针对openGauss进行netty线程优化改造内容，及针对改造进行性能提升比对测试，并给出最终测试结论。

缩略语清单：

| 缩略语   | 英文全名                                   | 中文解释                                                     |
| -------- | ------------------------------------------ | ------------------------------------------------------------ |
| TTPC     | Transaction Processing Performance Council | 事务处理性能委员会。                                         |
| TPC-C    | Trade Promotion Coordination Committee     | 在线事务处理（OLTP）的基准程序。                             |
| tpmC     | transaction per minute                     | TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试。 |
| ss-proxy | ShardingSphere-Proxy                       | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务 |

# 1     特性概述

ShardingSphere-Proxy 所使用的 Netty 是一个高性能的事件驱动异步 I/O 框架。由于 Netty 本身是事件驱动的异步框架，线程数量可控，在 Linux 下 Netty 还会使用 JNI 调用 Epoll API 优化性能。因此，在 Netty 框架本身的线程模型我们无须进行额外调整，不需要使用协程（目前也没有可用于 Netty 的协程方案）对 Netty 线程进行改造。

在openGauss的扩展协议中，客户端的请求及响应可能会同时由多个连续的消息组成。根据这一协议特点，本次优化将减少ShardingSphere Netty 前端发送连续的消息时flush的次数，以减少网络I/O层面的开销。

特性改造后实现如下目标：

- 1 节点 openGauss + 1 节点ss-proxy + 1 节点 BenchmarkSQL 优化后并发度提升 10% 
- 6 节点 openGauss + 6 节点ss-proxy + 3 节点 BenchmarkSQL 优化后并发度提升 10% 

# 2     特性测试信息

| 版本名称                 | 测试起始时间 | 测试结束时间 |
| ------------------------ | ------------ | ------------ |
| ss-proxy 5.0.0（优化前） | 2022-7-20    | 2022-7-21    |
| ss-proxy 5.1.1（优化后） | 2022-7-22    | 2022-7-22    |

| 硬件型号                                       | 硬件配置信息                                                 | 备注                   |
| ---------------------------------------------- | ------------------------------------------------------------ | ---------------------- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 3<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | openGauss数据库节点    |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | ss-proxy节点、tpcc节点 |

| 软件名称     | 软件版本                                                     | 备注                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BenchmarkSQL | 定制版本<br />commit：27b4eef1b6d499af68dccd33a9a1fd758f47a7db | 修改随机参数使完美sharding，增加负载均衡参数。<br />取包地址：<br />https://gitee.com/opengauss_sharding/benchmarksql |
| aarch64 jdk  | 17.0.1                                                       |                                                              |
| ss-proxy     | ShardingSphere-Proxy 5.0.0 <br />ShardingSphere-Proxy 5.1.1  | sharding官网发布包                                           |
| zookeeper    | 3.7.0                                                        |                                                              |
| openGauss    | openGauss 3.0.1                                              |                                                              |



# 3     测试结论概述

## 3.1   测试整体结论

共计执行2个用例，主要覆盖了优化前后的性能比对测试，未发现问题，整体质量良好。

| 测试活动     | 活动评价                                                     |
| ------------ | ------------------------------------------------------------ |
| 性能比对测试 | （1）1 节点 openGauss + 1 节点ss-proxy + 1 节点 BenchmarkSQL 优化后并发度提升 69%；符合需求预期。<br />（2）6 节点 openGauss + 6 节点ss-proxy + 3 节点 BenchmarkSQL 优化后并发度提升 75%；符合需求预期。 |

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

• 1 节点 openGauss：20.20.20.56； 

• 1 节点 ss-proxy 5.0.0 / 5.1.1：20.20.20.67； 

• 1 节点 BenchmarkSQL：20.20.20.88； 

• 1 节点 ZooKeeper：20.20.20.88

**6 节点 openGauss + 6 节点 ss-proxy + 3 节点 BenchmarkSQL 测试组⽹**：

• 6 节点 openGauss：54\56\58\62\82\85 

• 6 节点 ss-proxy 5.0.0 / 5.1.1：52\60\70\73\76\79 

• 3 节点tpcc：64\67\88 

• 1 节点zookeeper：88 

## 4.2  测试步骤&结果

### 4.2.1  1 节点 openGauss + 1 节点ss-proxy + 1 节点 BenchmarkSQL 优化前后并发度比对

**测试步骤:**

以下测试步骤分别对 ShardingSphere-Proxy 5.0.0 执⾏⼀次，找出 5.0.0 tpmc 最⼤值时的压测并发数：

（1）配置 ShardingSphere-Proxy 的 server.yaml 与 1 节点 config-sharding.yaml

（2）启动 ShardingSphere-Proxy 进程

（3）启动 BenchmarkSQL 使⽤ 500 terminals 发压 5 分钟预热后，分别压测 150、200、250、300、 

325、350 terminals 各 3 分钟，BenchmarkSQL 关键配置如下： 

```
terminals=150 #150、200、250、300、325、350
runTxnsPerTerminal=0 
# runMins=5 # 预热 5 分钟 
runMins=3 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
```

（4）停⽌ ShardingSphere-Proxy 进程

（5）收集所有 BenchmarkSQL tpmC 

（6）分别记录 ShardingSphere 优化前版本5.0.0达到最⾼ tpmC 所需最⾼并发数，

（7）验证5.1.1版本tpmc，从50并发开始，后进行递增验证，测试出优化后达到优化前5.0.0最⾼ tpmC 的并发数

**测试结果** 

| termimal | 150       | 200       | 250       | 300       | 325           | 350       |
| -------- | --------- | --------- | --------- | --------- | ------------- | --------- |
| 5.0.0    | 266931.24 | 283289.24 | 287318.95 | 301556.66 | **308310.69** | 306667.99 |

| termimal | 50        | 80        | 100           |
| -------- | --------- | --------- | ------------- |
| 5.1.1    | 221395.88 | 304388.62 | **346721.54** |

结论：5.1.1版本100 并发 tpmC 346721.54远高于5.0.0 325并发结果，结果符合预期：

 (325 - 100) / 325 = 0.69 > 0.1 



### 4.2.2  6 节点 openGauss + 6 节点ss-proxy + 3 节点 BenchmarkSQL 优化前后并发度比对

**测试步骤:**

以下测试步骤分别对 ShardingSphere-Proxy 5.0.0 执⾏⼀次，找出 5.0.0 tpmc 最⼤值时的压测并发数：

（1）配置 ShardingSphere-Proxy 的 server.yaml 与 6 节点 config-sharding.yaml

（2）启动 ShardingSphere-Proxy 进程

（3）同时启动 3 节点 BenchmarkSQL 使⽤ 1000 terminals 发压 5 分钟预热后，再同时启动 3 节点分别 

压测 400、500、600、700、800 terminals 各 3 分钟，BenchmarkSQL 关键配置如下： 

```
terminals=400 # 400、500、600、700、800
runTxnsPerTerminal=0 
# runMins=5 # 预热 5 分钟 
runMins=3 
limitTxnsPerMin=0 
terminalWarehouseFixed=true 
```

（4）停⽌ ShardingSphere-Proxy 进程

（5）收集所有 BenchmarkSQL tpmC ，并计算平均值

（6）分别记录 ShardingSphere 优化前版本5.0.0平均 tpmC 达到最⾼所需最⾼并发数

（7）验证5.1.1版本tpmc，从100并发开始，后进行递增验证，测试出优化后达到优化前最⾼ tpmC 的并发数

**测试结果** 

| termimal           | 400       | 500       | 600           | 700       | 800       |
| ------------------ | --------- | --------- | ------------- | --------- | --------- |
| 5.0.0（3节点均值） | 627410.25 | 652606.64 | **656430.82** | 653737.15 | 654225.02 |

| termimal           | 100       | 150           |
| ------------------ | --------- | ------------- |
| 5.1.1（3节点均值） | 508209.02 | **741882.40** |

结论：5.1.1版本150并发 tpmC 得到 741882.40远高于5.0.0 600并发结果，结果符合预期：

 (600- 150) / 600= 0.75> 0.1 



## **4.3 测试执行统计数据**

| **版本名称**               | **测试用例数** | **用例执行结果**        | **发现问题单数** |
| -------------------------- | -------------- | ----------------------- | ---------------- |
| ShardingSphere-Proxy 5.0.0 | 4              | Passed：4<br/>Failed：0 | 0                |
| ShardingSphere-Proxy 5.1.1 | 5              | Passed：5<br/>Failed：0 | 0                |

数据项说明：

测试用例数：2个，测试通过：2个；

发现问题单数：0个。

## **4.4   后续测试建议**

无

# **5     附件**



****



****

****
