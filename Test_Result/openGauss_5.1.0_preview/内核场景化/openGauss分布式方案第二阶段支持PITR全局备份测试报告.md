![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者       |
| ---------- | ----------- | -------------------- | ---------- |
| 2023-12-22 | 1.0         | 特性测试报告初稿完成 | peilinqian |
| 2024-1-2   | 2.0         | 优化测试报告         | peilinqian |

关键词： 

openGauss 分布式、ShardingSphere-Proxy、PITR物理备份及恢复、CDC逻辑复制

摘要：

本文档主要验证ShardingSphere + openGauss 在分布式场景下的全局物理备份恢复（PITR）、逻辑复制（CDC）特性，并给出最终测试结论。

缩略语清单：

| 缩略语   | 英文全名                 | 中文解释                                                     |
| -------- | ------------------------ | ------------------------------------------------------------ |
| ss-proxy | shardingsphere-Proxy     | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务端版本，用于完成对异构语言的支持。 目前提供 MySQL 和 PostgreSQL（兼容 openGauss 等基于 PostgreSQL 的数据库）版本，它可以使用任何兼容 MySQL/PostgreSQL 协议的访问客户端（如：MySQL Command Client, MySQL Workbench, Navicat 等）操作数据，对 DBA 更加友好。 |
| ZK       | zookeeper                | ZooKeeper 是 Apache 软件基金会的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册。 |
| Redis    | Remote Dictionary Server | 远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。 |
| GLT      | global logical time      | 全局逻辑时钟                                                 |
| CDC      | Change Data Capture      | 实时数据同步。CDC技术允许在数据源发生变化时，实时地捕获这些变化，并将其应用到目标系统中，从而保持数据的同步性。 |
| PITR     | Point-In-Time-Recover    | 时间点恢复                                                   |

# 1    特性概述

基于openGauss 的物理备份和恢复、全局事务及全局时钟组件GLT，实现 ShardingSphere + openGauss 在分布式场景下的全局物理备份恢复（PITR）、逻辑复制（CDC）能力。

- PITR：基于openGauss的gs_probackup物理备份及恢复工具，设计PITR脚本工具，通过脚本工具连接到ShardingProxy进行分布式集群备份、恢复备份、查看备份。
- CDC：ShardingSphere CDC 分为两个部分，一个是 CDC Server，另一个是 CDC Client。 CDC Server 和 ShardingSphere-Proxy 目前是集成部署。CDC Client是jar包工具，用户可以在自己的项目中引入 CDC Client，实现数据的消费逻辑。

# 2    CDC特性

## 2.1    特性测试信息

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| ------------------------------------------------------------ | ------------ | ------------ |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:152f56c1195f324d310e794c443bec2b3be7190d | 2023/11/6    | 2023/11/21   |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:dc9d8bef0e5696e38b9dec665fb4a39cd29c1062 | 2023/12/1    | 2023/12/4    |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:eb4dc319b97aba2133073446fa310763c4f0a9fb | 2023/12/9    | 2023/12/15   |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:1ac5eaecf2b044fe9a1e1d9d91dd48941d301c6e | 2023/12/22   | 2023/12/22   |

| 硬件型号                                       | 硬件配置信息                                                 | 备注                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 1<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | 1. 功能测试不需要如此高的配置，可根据实际情况调整；<br />2. 功能测试2分片，实际需要3台机器；2个dn节点，1个sharding-proxy+CDC server+CDC Client客户端应用<br />3. 性能测试16分片，实际需要18台机器；16个dn节点，1个sharding-proxy+CDC server，1个sysbench+CDC Client客户端应用 |

| 软件名称                                   | 软件版本                                                     | 备注                                                         |
| ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| shardingsphere-Proxy+CDC server +CDCclient | **ShardingSphere-5.4.2-SNAPSHOT**<br/>152f56c1195f324d310e794c443bec2b3be7190d<br />dc9d8bef0e5696e38b9dec665fb4a39cd29c1062<br />eb4dc319b97aba2133073446fa310763c4f0a9fb<br />1ac5eaecf2b044fe9a1e1d9d91dd48941d301c6e | ShardingSphere官网源码包[github地址](https://github.com/apache/shardingsphere)<br />编译方式：见[CDC官网资料](https://shardingsphere.apache.org/document/current/cn/user-manual/shardingsphere-proxy/cdc/) |
| zookeeper                                  | 3.8.0                                                        |                                                              |
| openGauss                                  | openGauss 3.1.0 build 5e70d87b                               |                                                              |
| redis                                      | 7.2.0                                                        |                                                              |

## 2.2    测试结论概述

### 2.2.1    测试整体结论

共设计25个用例，主要覆盖了功能测试、可靠性测试、性能测试，发现问题19个，已解决15个，4个遗留；整体质量一般；

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 1. 正常业务场景覆盖如下，正常同步：<br />（1）同步表类型覆盖：单表、分片表、广播表<br />（2）同步数据类型覆盖：见CDC支持数据类型<br />（3）同步事务覆盖：UID、批量超大事务<br />（4）同步时选择存量数据是否同步<br />（5）多个cdc任务并发同步<br />2. 异常场景覆盖如下，正常抛错：<br />（1）cdc同步时，源端、目标端表不存在<br />（2）cdc server端、目标端的IP、端口、用户名、密码错误<br />3. CDC client jar包功能覆盖如下：<br />（1）CDCClientConfiguration功能验证，CDC地址、端口、超时参数验证正常生效<br />（2）CDCClient方法验证，包括await(), close(), conneciton(), login(), startStreaming(), stopStreaming(), restartStreaming(), dropStreaming()<br />4. CDC 任务distsql语法验证： <br />（1）查询订阅任务列表，正常场景结果正确，错误场景报错信息正确<br />（2）删除订阅任务，active状态删除异常<br />（3）查询订阅状态，包括所有参数正确性验证，正常场景结果正确，错误场景报错信息正确<br />（4）cdc 同步规则修改验证，修改后可按照最新配置进行同步<br />**存在两个遗留问题单；见遗留问题2、3，其他场景验收通过；** |
| 可靠性测试 | 1. CDC Client端连接server端网络故障；Proxy进程kill -19<br />2. 同步目标端网络故障<br />3. 基础数据、增量数据同步过程中，取消同步任务、kill 同步client任务<br />4. 基础数据、增量数据同步过程中，kill 同步cdc serve进程<br />5. 同步过程中构造发布端某个实际节点故障<br />测试点：所有故障执行时，可正常抛错，故障恢复后，同步任务可再次执行，执行完成后，目标库与源库数据一致<br />**存在1个遗留问题单；见遗留问题1，其他场景验收通过；** |
| 性能测试   | 16节点10个分片表场景下，基础数据同步、增量数据同步性能验证，影响同步性能的不同配置项摸底验证。详细测试情况见写性能测试说明章节；验收通过；<br />**存在1个遗留问题单；见遗留问题4，其他场景验收通过；** |

### 2.2.2    约束说明

（1）部署模式：

- ShardingSphere Proxy

  a. ZK+GlT配置+Redis部署，提供全局 CSN

  b. 带CDC Server模块，且开启

- OpenGauss

  a. 目前仅3.1.0分支支持GLT功能，提供全局 CSN

  b. 设置GUC参数wal_level=logical

  c. 设置GUC参数max_replication_slots>=每个节点所需的（物理流复制槽数+逻辑复制槽数）

  d. 设置cdc server端IP的replication权限白名单配置

（2）CDC 只同步数据，不会同步表结构，目前也不支持 DDL 语句同步

（3）CDC 增量阶段会按照事务的维度消费数据， 要保证全局事务一致性，需要开启 XA 事务的兼容，openGauss 和 ShardingSphere-Proxy 都需要配置使用 GLT 模块

（4）在存在超大事务的情况下，目前处理逻辑可能会导致 CDC 所在的进程 OOM，后续可能会强制截断。

（5）满足同步推送条件是满足了一定大小的数据量或者到了一定的时间间隔（目前是 300ms），在处理 XA 事务时，收到的多个分库增量事件超过了 300ms，可能会导致 XA 事务被拆开推送。

### 2.2.3    遗留问题分析

#### 2.2.3.1    遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 问题影响和规避措施                                           | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| [29259](https://github.com/apache/shardingsphere/issues/29259) | cdc server端异常，比如kill-9 ss-proxy后，cdc任务状态active仍为true，异常后检测更新job状态需要考虑 | 次要     | 此故障场景涉及sharding集群ZK数据更新，后续考虑如何优化；规避措施：可手工更新ZK的同步任务状态 | 打开     |
| [29276](https://github.com/apache/shardingsphere/issues/29276) | 同一cdcjob 多次并发执行异常，两个客户端进程均都异常（全量数据未同步阶段报错概率高） | 次要     | 问题根因：elasticjob 启动和停止需要一点时间（依赖zk）；规避措施：不要使用多个client并发的订阅同一个CDC任务 | 打开     |
| [29508](https://github.com/apache/shardingsphere/issues/29508) | 目前cdc增量，如果xa事务量较大，读取时间超过300ms，则无法保障事务完整性。目前隐式单库事务场景下csn无法推进，导致无法无限制等待 | 次要     | 目前隐式单库事务场景下csn无法推进，同步过程以csn事务为单位会导致事务扩大，无超时推进截断的话会导致超大事务。待GLT隐式事务功能完善后，优化该场景 | 打开     |
| [29554](https://github.com/apache/shardingsphere/issues/29554) | 目前仅支持mppdb_decoding解码方式，需要适配并行解码方式，以提高性能 | 次要     | 目前整体流程无问题，并行解码适配可以提高性能。后续以需求跟踪优化 | 打开     |

#### 2.2.3.2    问题统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 19       | 0    | 7     | 12    | 0      |
| 百分比 | 100%     | 0%   | 36.8% | 63.2% | 0%     |

#### 2.2.3.3    问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [28958](https://github.com/apache/shardingsphere/issues/28958) | 次要     | cdc增量同步bytea字段报错                                     | 已关闭   |
| 2    | [28960](https://github.com/apache/shardingsphere/issues/28960) | 主要     | cdc drop streaming jobId成功，但是实际逻辑数据源逻辑复制槽未清理 | 已关闭   |
| 3    | [28979](https://github.com/apache/shardingsphere/issues/28979) | 次要     | cdc任务执行过程中，proxy侧删除streamingjob需要进行限制，不允许删除 | 已关闭   |
| 4    | [28980](https://github.com/apache/shardingsphere/issues/28980) | 主要     | cdcclient jar包未提供drop streaming操作功能                  | 已关闭   |
| 5    | [29035](https://github.com/apache/shardingsphere/issues/29035) | 主要     | cdc任务同步记录数量统计存在问题，非实际同步的记录条数        | 已关闭   |
| 6    | [29096](https://github.com/apache/shardingsphere/issues/29096) | 次要     | cdc server端配置规则功能缺少，无法自定义                     | 已关闭   |
| 7    | [29097](https://github.com/apache/shardingsphere/issues/29097) | 次要     | show streaming list，stop_time时间未记录                     | 已关闭   |
| 8    | [29098](https://github.com/apache/shardingsphere/issues/29098) | 次要     | 已开启cdc任务，再次开启，前一次CDC任务取消退出无任何打印信息 | 已关闭   |
| 9    | [29249](https://github.com/apache/shardingsphere/issues/29249) | 次要     | cdc streaming list，状态为active为true时，stop_time应该为空  | 已关闭   |
| 10   | [29259](https://github.com/apache/shardingsphere/issues/29259) | 次要     | cdc server端异常，比如kill-9 ss-proxy后，cdc任务状态active仍为true，如何在异常后检测job状态需要考虑 | 打开     |
| 11   | [29250](https://github.com/apache/shardingsphere/issues/29250) | 次要     | 取消cdc客户端任务时，cdc server端报错                        | 已关闭   |
| 12   | [29258](https://github.com/apache/shardingsphere/issues/29258) | 主要     | 已经同步的数据，逻辑复制槽未进行确认后消费，导致逻辑日志无法回收 | 已关闭   |
| 13   | [29274](https://github.com/apache/shardingsphere/issues/29274) | 主要     | cdcClient. dropStreaming (cdcjob) 方法调用报错               | 已关闭   |
| 14   | [29276](https://github.com/apache/shardingsphere/issues/29276) | 次要     | 同一cdcjob 多次执行异常，两个客户端进程均都异常（全量数据未同步阶段报错概率高） | 打开     |
| 15   | [29416](https://github.com/apache/shardingsphere/issues/29416) | 主要     | 20张表，每张表4个分表，总计20*10000条数据进行全量同步时异常，存在部分分表未同步情况；偶现问题，后续未复现 | 取消     |
| 16   | [29420](https://github.com/apache/shardingsphere/issues/29420) | 次要     | 相同表进行同步，生成的jobID不同                              | 已关闭   |
| 17   | [29508](https://github.com/apache/shardingsphere/issues/29508) | 次要     | 目前cdc增量，如果xa事务量较大，读取时间超过300ms，则无法保障事务完整性。目前隐式单库事务场景下csn无法推进，导致无法无限制等待 | 打开     |
| 18   | [29506](https://github.com/apache/shardingsphere/issues/29506) | 主要     | cdc全量同步物理表数量较多，大于线程数量后，等待时间比较长，需要优化 | 已完成   |
| 19   | [29554](https://github.com/apache/shardingsphere/issues/29554) | 次要     | 目前仅支持mppdb_decoding解码方式，需要适配并行解码方式，以提高性能 | 打开     |

## 2.3    测试执行

### 2.3.1    测试组网图

```
                                             +------------------+
                                             | DN 1:            |
+------------------------------+             | OpenGauss Server |
| Apache ShardingSphere Proxy  |             +------------------+  
| Apache Zookeeper             | ---------->        ……
| CDC Server                   |             +------------------+
+------------------------------+             | DN n:            |
              ^                              | OpenGauss Server |
              |                              +------------------+
              |
+------------------------------+
| Sysbench(performance)        |
| CDC client                   |
+------------------------------+           
```

### 2.3.2    测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ----------------------- | ------------ |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:152f56c1195f324d310e794c443bec2b3be7190d | 10         | Pass：6<br />Failed：4  | 8            |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:eb4dc319b97aba2133073446fa310763c4f0a9fb | 15         | Pass：10<br />Failed：5 | 6            |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:bcde6f374c4a3a025173fbc9f6d0e66ed686a042 | 10         | Pass：5<br />Failed：5  | 5            |
| ShardingSphere-5.4.2-SNAPSHOT Commit ID:1ac5eaecf2b044fe9a1e1d9d91dd48941d301c6e | 5          | Pass：3<br />Failed：2  | 0            |

*数据项说明：*

- 用例总数25个，24个功能可靠性用例，1个性能用例；
- 第一轮冒烟测试，第二、三轮全量功能、可靠性测试，第四轮回归问题单+性能测试。
- 最终测试用例执行通过23个，不通过2个（见遗留问题单1、2）；
- 缺陷密度为19(缺陷个数)/23.147k(代码行数)=0.82(个/kloc)。

### 2.3.3    性能测试说明

#### 2.3.3.1    测试步骤：

（1）环境准备：

- 配置部署 1 个 Proxy & zookeeper 
- 配置部署 16 节点 openGauss 
- 部署下游消费程序
- 通过 Sysbench 向 Proxy 灌入 10 个表(单表分 16 个库)，每个表 6 千 4 百万行数据，总量 6 亿 4 千万行数据，数据总量约 135 G

（2） 全量数据消费性能测试：

-  启动下游消费程序，记录全量数据的消费耗时，计算 TPS 记作性能

（3） 增量数据消费性能测试：

- 启动 Sysbench 发压至 Proxy，模拟增量；同时下游启动消费程序，消费数据
- 记录增量数据的消费耗时，计算 TPS 记作性能指标
- 记录 Sysbench 的并发度、压测时长、TPS 用于对比

#### 2.3.3.2    测试说明：

（1）目前Java堆内存配置较大，Xmx=256g，以便于排除 JVM 相关的限制；

（2）涉及CDC 同步规则配置最优如下，内存队列值 block-queue-size 和 batchSize 会影响到消费速率。测试不同维度性能趋势时，只修改测试项配置，其他参数默认如下：

```
ALTER STREAMING RULE (READ( WORKER_THREAD=20, BATCH_SIZE=100, SHARDING_SIZE=10000000), WRITE( WORKER_THREAD=10, BATCH_SIZE=100), STREAM_CHANNEL ( TYPE(NAME='MEMORY',PROPERTIES('block-queue-size'='10000'))) );
```

（3）性能测试仅验收至消费阶段，后续实际往目标库写入依赖目标端og配置，不考虑；

#### 2.3.3.3    测试结论：

##### （1）全量同步性能验证结论：

| 测试轮次 | 数据量                                       | 执行时间            |
| -------- | -------------------------------------------- | ------------------- |
| 1        | 10个分片表，每个分片表16个分片，共135G数据量 | 25分47秒            |
| 2        | 10个分片表，每个分片表16个分片，共135G数据量 | 26分10秒            |
| 3        | 10个分片表，每个分片表16个分片，共135G数据量 | 24分05秒            |
| 4        | 10个分片表，每个分片表16个分片，共135G数据量 | 25分                |
|          | 平均执行tps：55W                             | 平均执行时长：25min |

##### （2）增量同步性能验证结论：

###### a.  batchSize 配置对性能的影响

说明：保证 sysbench 的 tps 保持在 33000 左右，内存队列 block-queue-size 设置较大值10000，验证 batchSize 值对性能的影响

| Read batchSize | Write batchSize | 数据延迟(16 DN 总和), 统计间隔 4min, 共20m     |
| -------------- | --------------- | ---------------------------------------------- |
| 10             | 10              | 10mb, 6gb, 11.8gb, 16.8gb,  22.6gb,  28.4gb    |
| 100            | 100             | 0.2mb,  0.08gb,  0.27gb,  0.5gb, 0.3gb,  0.5gb |
| 500            | 500             | 80mb, 3.3gb, 6.5gb, 9.4gb, 14gb, 19gb          |
| 1000           | 1000            | 20mb,  2.4gb, 4.6gb, 6.7gb,  8.6gb, 11gb       |
| 2000           | 6000            | 12mb, 1.8gb, 3.1gb, 4.6gb, 5.7gb, 7.2gb        |
| 3000           | 10000           | 0.01mb, 1.2gb, 2.5gb, 4.1gb,  5.5gb, 7.5gb     |
| 1000           | 15000           | 13mb, 1.8gb, 2.6gb, 3.4gb,  3.9gb, 4gb         |

结论：batchSize 对性能有一定的影响，过大或者过小的话会导致性能下降，配置合适的读写参数，可以适当提升消费性能，需要根据实际进行调优，实际性能和每条数据的大小也有关系。除此之外，性能会受到网络层面的一些限制，以及逻辑复制插槽的解析性能。

######  b.  block-queue-size 配置对性能的影响

说明：保证 sysbench 的 tps 保持在 33000 左右，batchSize 都等于 100，验证 block-queue-size 内存队列对性能的影响

因为读写都通过内存队列，所以内存队列的值时比较关键的，在性能测试中如果给的太小可能会限制读的性能，从而影响到写。

| block-queue-size | 数据延迟(16 DN 总和),统计间隔 5min，共20min  | 备注                                     |
| ---------------- | -------------------------------------------- | ---------------------------------------- |
| 1                | 0.1mb, 11.7gb, 24.7gb, 37.7gb, 50.8gb, 64.5g | Client消费速率非常差，每秒消费数据个位数 |
| 100              | 17mb,  12.7gb,  25.7g, 38.4gb, 50.4gb, 64.gb | 每秒消费数据100左右                      |
| 500              | 12mb, 0.29gb, 0.26gb, 0.29gb, 0.26gb, 0.24gb | 正常消费速率                             |
| 1000             | 10mb, 0.05g,  0.13g, 0.36g, 0.48gb, 0.06gb   | 正常消费速率                             |
| 10000            | 0.04mb, 0.1g, 0.29gb, 0.3gb, 0.17gb, 0.69g   | 正常消费速率                             |

结论：内存队列 block-queue-size 的值必须大于 batchSize，否则对性能会有很大的影响，当大于 5 倍以上，再增加效果不大。

###### c. Sysbench TPS 对性能的影响

说明：batchSize 设置 100，内存队列 block-queue-size 设置 10000，验证 Sysbench TPS 值对性能的影响

| 线程数（参考价值不大，可以只看TPS） | TPS   | 数据延迟(16 DN 总和),统计间隔 5min，共20min    |
| ----------------------------------- | ----- | ---------------------------------------------- |
| 50                                  | 19000 | 013mb, 0.05gb, 0.04gb, 0.04gb, 0.04gb          |
| 100                                 | 32000 | 0.2mb,  0.08gb,  0.27gb,  0.5gb, 0.3gb,  0.5gb |
| 120                                 | 35000 | 0mb,  1.3gb, 2gb,  2.9gb, 3.6gb, 4.2gb         |
| 150                                 | 40000 | 0.01mb, 0.6gb,  3gb, 6.1gb, 10.7gb,  14.7gb    |

结论：在 TPS 32000 及以下，CDC 基本没有数据堆积；当 TPS 达到 35000 的时候，消费速率逐渐跟不上了；当 TPS 达到 40000 的时候的时候，会出现消息堆积的情况。

### 2.3.4    后续测试建议

- 后续支持并行解码后，进行相应功能及性能验证；

## 2.4    附件

无

# 3    PITR特性

## 3.1    特性测试信息

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| ------------------------------------------------------------ | ------------ | ------------ |
| shardingsphere-on-cloud Commit ID:78d680f16e99b073275d3d40ef13ef468d4d06a0 | 2023/9/4     | 2023/9/5     |
| shardingsphere-on-cloud Commit ID:4ea3d1b35c796e5f668a6555163adf5e1fdc7e9e | 2023/11/12   | 2023/11/15   |
| shardingsphere-on-cloud Commit ID:829f4b8195121c9386b1ff1fcc5849f507358a58 | 2023/12/25   | 2023/12/25   |

| 硬件型号                                       | 硬件配置信息                                                 | 备注                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 1<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | 1. 功能测试不需要如此高的配置，可根据实际情况调整；<br />2. 功能测试2分片，实际需要3台机器；2个dn节点+pitr-agent，1个sharding-proxy+gs_pitr<br />3. 稳定性测试16分片，实际需要17台机器；16个dn节点+pitr-agent，1个sharding-proxy+gs_pitr |

| 软件名称  | 软件版本                                                     | 备注                                                         |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| PITR      | **shardingsphere-on-cloud**<br/>78d680f16e99b073275d3d40ef13ef468d4d06a0<br />4ea3d1b35c796e5f668a6555163adf5e1fdc7e9e<br />829f4b8195121c9386b1ff1fcc5849f507358a58 | 编译使用方式：[PITR官网资料](https://github.com/apache/shardingsphere-on-cloud/blob/main/pitr/README-zh.md) |
| zookeeper | 3.8.0                                                        |                                                              |
| openGauss | openGauss 3.1.0 build 5e70d87b                               |                                                              |
| redis     | 7.2.0                                                        |                                                              |

## 3.2    测试结论概述

### 3.2.1    测试整体结论

共设计19个用例，主要覆盖了功能测试、可靠性测试、稳定性测试，发现问题13个，已解决12个，遗留待解决1个，可进行规避，不影响整体功能，整体质量良好；

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 1. pitr-agent工具验证：<br />（1）参数正常启动、参数异常启动、帮助功能、日志打印功能验证正常，异常及错误日志可正常打印；<br />2. gs_pitr backup 备份功能验证：<br />（1）参数正常，备份功能正常（备份模式全量、增量覆盖；备份线程数不同覆盖）；<br />（2）参数异常，备份可正常抛错（连接ss-proxy参数不正确、连接pirt-agent参数不正确等）<br />（3）备份业务场景覆盖，多次全量备份、多次增量备份、备份前dn节点未进行gs_probackup init操作、备份过程中是否继续选择n；<br />（4）help功能打印正确<br />3. gs_pitr restore 恢复备份功能验证：<br />（1）参数正常，恢复功能正常（备份线程数不同、通过csn恢复、通过id恢复、全量备份恢复，增量备份恢复）；<br />（2）参数异常，恢复可正常抛错（连接ss-proxy参数不正确、连接pirt-agent参数不正确、恢复id不存在、恢复csn不存在等）<br />（3）备份业务场景覆盖，恢复csn存在多条备份记录、恢复过程中是否继续选择n等<br />（4）help功能打印正确<br />4. gs_pitr delete 删除备份功能验证：<br />（1）参数正常，删除功能正常（通过csn删除、通过id删除、删除后实际dn节点备份记录清理）；<br />（2）参数异常，恢复可正常抛错（连接ss-proxy参数不正确、连接pirt-agent参数不正确、恢复id不存在、恢复csn不存在等）<br />（3）help功能打印正确<br />5. gs_pitr show 查询备份功能验证<br />（1）备份记录信息正确、备份、备份删除后，备份记录数据正确<br />（2）按备份id查询正确、按备份csn查询正确<br />（3）无备份记录、按备份id、csn查询不正确，正常提示<br />**以上基础功能模块功能正常，验收通过；** |
| 可靠性测试 | 备份、恢复、删除时，注入如下故障，可以正常抛错处理，恢复故障可以重新执行通过：<br />（1）sharding-proxy未启动、端口网络不通、进程挂住<br />（2）某个节点pitr-agent未启动、端口网络不通、进程挂住<br />（3）dn节点库未启动、磁盘满<br />**以上可靠性场景功能正常，验收通过；** |
| 稳定性测试 | pitr工具性能主要是各dn节点备份恢复性能，与工具本身无关，所以仅验证测试大数据量下全量，增量备份稳定性正常即可<br />**存在1个遗留问题单；见遗留问题2；** |

### 3.2.2    约束说明

（1）部署模式：

- ShardingSphere Proxy

  a. ZK+GlT配置+Redis部署，提供全局 CSN

- OpenGauss

  a. 目前仅3.1.0分支支持GLT功能，提供全局 CSN

  b. 设置enable_cbm_tracking为on

  c. 设置环境变量

  `export PGDATABASE=****`

  `export PGPORT=****`

- PITR工具

  a. 服务端工具 gs_pitr ：备份、恢复、删除备份操作发起端；对部署节点及用户无要求，可以访问 proxy 及pitr-agent 端口即可

  b. 客户端工具 pitr-agent ：部署在 opengauss 数据库用户下，包括SSL密钥；服务端发起时，客户端 pitr-agent 需要是启动状态

（2）业务限制：

- 目前PITR工具仅实现了openGauss gs_probackup工具的3个参数，–pgdata、–threads、–backup-mode；如果集群的dn节点涉及数据目录外部的脚本、sql转储和配置文件、xlog等分盘日志，则无法满足需求

（3）其他使用限制：

- Pitr 备份恢复功能依赖 GLT，通过部署 Redis 实现。如果没有 GLT，那么生成的 CSN 会为空，导致恢复无法根据 CSN 保证一致性，此时恢复命令执行只能使用备份 ID进行恢复
- 全局备份任务需要在没有进行中的事务的时间点进行开启，由 ShardingSphere 来加锁保证
- 备份开始后 ShardingSphere 会一直持有锁，当备份结束后才会释放锁
- 多个 Pitr cli 客户端同时操作，只有一个 Pitr cli 客户端可执行成功
- 恢复前后 OpenGauss 数据节点的 IP 地址和端口需保持不变，即和 ShardingSphere 中逻辑库注册的数据源保持一致
- 恢复时，保证 ShardingSphere 在备份时和恢复时使用的版本一致，确保元数据兼容
- 恢复操作需要停机，为同步操作，用户需保证完全恢复成功
- 当恢复失败时，如果 OpenGauss 数据节点存在状态不一致（比如一台成功另一台失败），需用户处理异常后重新发起恢复操作，保证最终恢复成功
- 当执行备份后，会在当前用户的 `$HOME` 下创建 `.gs_pitr/backup` 目录，并在该目录下存放备份元数据文件
- 如果需要另一台设备上需要恢复，需要复制路径 `$HOME/.gs_pitr/backup` 下的备份数据到对应设备的相同路径
- 当执行删除备份后，当前用户的 `$HOME/.gs_pitr/backup` 下的相应备份文件将被删除
- 当命令行执行过程中，`Ctrl C` 取消或者 `kill` 中止进程，不会同时停止服务端正在执行的任务

### 3.2.3    遗留问题分析

#### 3.2.3.1    遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 问题影响和规避措施                                           | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| [29574](https://github.com/apache/shardingsphere/issues/29574) | 恢复工具执行，实际dn节点恢复步骤后，元数据drop后重新import 时发生异常报错；<br />问题根因：集群规模大、元数据较多的情况下（16分片），在执行恢复的时候可能会出现元数据不一致的情况，用户需要等待一段时间后才能进行正常访问。该问题为偶发问题 | 次要     | 目前仅影响工具前端打印，及sharding短暂无法正常查询表结构，实际恢复功能已完成；<br />可通过等待足够时间，或者重启 Proxy + refresh metadata 可以进行规避掉 | 打开     |

#### 3.2.3.2    问题统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 13       | 0    | 4     | 9     | 0      |
| 百分比 | 100%     | 0%   | 30.8% | 69.2% | 0%     |

#### 3.2.3.3    问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [440](https://github.com/apache/shardingsphere-on-cloud/issues/440) | 次要     | PITR README.md存在说明不清晰部分，需要优化                   | 已关闭   |
| 2    | [442](https://github.com/apache/shardingsphere-on-cloud/issues/442) | 次要     | PITR README.md文件中的描述不清楚，缺少opengauss的预置操作    | 已关闭   |
| 3    | [443](https://github.com/apache/shardingsphere-on-cloud/issues/443) | 次要     | PITR README.md文件中的描述不清楚。opengauss需要配置dbname和port环境变量 | 已关闭   |
| 4    | [444](https://github.com/apache/shardingsphere-on-cloud/issues/444) | 主要     | 在gs_pitr工具易用测试中，错误提示不清晰、准确，无法指导用户分析和定位故障 | 已关闭   |
| 5    | [455](https://github.com/apache/shardingsphere-on-cloud/issues/455) | 次要     | PITR backup时threads透传不正常，--dn-threads-num设置为5，dn实际节点执行--threads=1 | 已关闭   |
| 6    | [458](https://github.com/apache/shardingsphere-on-cloud/issues/458) | 主要     | PITR 删除指定备份功能不支持                                  | 已关闭   |
| 7    | [459](https://github.com/apache/shardingsphere-on-cloud/issues/459) | 主要     | PITR gs_pitr备份失败场景，报错信息不明确，需要优化           | 已关闭   |
| 8    | [463](https://github.com/apache/shardingsphere-on-cloud/issues/463) | 次要     | PITR show按csn号查询，多个备份csn记录相同，查询结果只展示1条 | 已关闭   |
| 9    | [462](https://github.com/apache/shardingsphere-on-cloud/issues/462) | 次要     | PITR show展示"start_time""end_time"未按日期格式展示          | 已关闭   |
| 10   | [477](https://github.com/apache/shardingsphere-on-cloud/issues/477) | 次要     | PITR 说明下备份记录文件默认是保存在gs_pitr工具所在目录的隐藏文件夹下。以备更换主机执行恢复操作等执行 | 已关闭   |
| 11   | [476](https://github.com/apache/shardingsphere-on-cloud/issues/476) | 次要     | PITR 存在多个相同CSN的备份记录，恢复时，需要进行校验，只能通过ID进行恢复 | 已关闭   |
| 12   | [483](https://github.com/apache/shardingsphere-on-cloud/issues/483) | 次要     | PITR 各节点进行备份路径创建、gs_probackup init操做需要手工去实际节点执行，未集成至备份流程中 | 已关闭   |
| 13   | [29574](https://github.com/apache/shardingsphere/issues/29574) | 次要     | 集群规模大、元数据较多的情况下（16分片），在执行恢复的时候可能会出现元数据不一致的情况，用户需要等待一段时间后才能进行正常访问。该问题为偶发问题 | 打开     |

## 3.3    测试执行

### 3.3.1    测试组网图

```
                                             +------------------+
                                             | DN 1:            |
                                             | OpenGauss Server |
+------------------------------+             | Pitr Agent       |
| Apache ShardingSphere Proxy  |             +------------------+
| Apache Zookeeper             | ---------->        ……
| Pitr Cli (aka `gs_pitr`)     |             +------------------+
+------------------------------+             | DN n:            |
                                             | OpenGauss Server |
                                             | Pitr Agent       |
                                             +------------------+
```

备注：
功能测试：2个DN节点（2分片）
稳定性测试：16个DN节点（16分片）

### 3.3.2    测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------------------ | ------------ |
| shardingsphere-on-cloud Commit ID:78d680f16e99b073275d3d40ef13ef468d4d06a0 | 5          | Pass：4 <br />Failed：1  | 4            |
| shardingsphere-on-cloud Commit ID:4ea3d1b35c796e5f668a6555163adf5e1fdc7e9e | 18         | Pass：10 <br />Failed：8 | 8            |
| shardingsphere-on-cloud Commit ID:829f4b8195121c9386b1ff1fcc5849f507358a58 | 9          | Pass：9 <br />Failed：0  | 1            |

*数据项说明：*

- 用例总数19个，18个功能可靠性用例，1个稳定性用例；
- 第一轮冒烟测试，第二轮全量功能、可靠性测试，第三轮回归问题单+稳定性测试。
- 最终测试用例执行通过18个，不通过1个（见遗留问题单）；
- 缺陷密度为13(缺陷个数)/18.448k(代码行数)=0.7(个/kloc)。

### 3.3.3    稳定性测试说明

#### 3.3.3.1    测试步骤

（1）环境准备：

- 16 节点 openGauss & Pitr Agent 配置、部署
- Proxy & zookeeper & Pitr Cli  & Sysbench配置、部署

（2）全量数据准备：

- 通过 Sysbench 向 Proxy 灌入 10 个表(单表分 16 个库)，每个表 6 千 4 百万行数据，总量 6 亿 4 千万行数据，数据总量约 135 G


（3）全量备份：

- 通过 Pitr Cli 启动全量备份，调用 16 节点 Pitr Agent 进行对应 openGauss 的全量数据备份

（4）增量数据准备：

- Proxy 创建新的逻辑库，注册 openGauss 对应新的物理库
- 通过 Sysbench 灌入总量 1,000w 行数据至 Proxy & 后端挂载的 openGauss，分片策略同上。

（5）增量备份：

- 通过 Pitr Cli 启动全量备份，调用 16 节点 Pitr Agent 进行对应 openGauss 的增量数据备份

（6）全量恢复：

- 通过 Pitr Cli 启动全量备份恢复，调用 16 节点 Pitr Agent 进行对应 openGauss 的全量数据恢复
- 通过 Proxy & openGauss 进行数据量查询，验证全量恢复成功

（7）增量恢复:

- 通过 Pitr Cli 启动全量备份恢复，调用 16 节点 Pitr Agent 进行对应 openGauss 的增量数据恢复
- 通过 Proxy & openGauss 进行数据量查询，验证增量恢复成功

#### 3.3.3.2    测试结论

（1）PITR 工具测试全量备份 —— 16 openGauss 节点全量备份成功

（2）PITR 工具测试增量备份 —— 16 openGauss 节点增量备份成功

（3）PITR 工具测试全量恢复 —— 全量恢复成功，验证整体数据行数与全量备份前一致

（4）PITR 工具测试增量恢复 —— 增量恢复成功，验证增量数据行数与增量备份前一致

### 3.3.4    后续测试建议

无

## 3.4    附件

无
