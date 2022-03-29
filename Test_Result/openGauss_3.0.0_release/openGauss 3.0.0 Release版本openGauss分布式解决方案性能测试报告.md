![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述         | 作者       |
| --------- | ----------- | ---------------- | ---------- |
| 2022-3-12 | 1.0         | 特性测试报告初稿 | peilinqian |
|           |             |                  |            |
|           |             |                  |            |

关键词： ss-proxy；ss-jdbc；tpmC；分布式性能；分布式ss-jdbc性能损耗

摘要：

 本文对openGauss 3.0.0 Release版本，分布式场景下单ss-jdbc1分片性能损耗和8分片最高性能指标测试，并给出最终测试结论。

 

缩略语清单：

| 缩略语   | 英文全名                                   | 中文解释                                                     |
| -------- | ------------------------------------------ | ------------------------------------------------------------ |
| TTPC     | Transaction Processing Performance Council | 事务处理性能委员会。                                         |
| TPC-C    | Trade Promotion Coordination Committee     | 在线事务处理（OLTP）的基准程序。                             |
| tpmC     | transaction per minute                     | TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试。 |
| ss-jdbc  | shardingsphere-JDBC                        | 定位为轻量级 Java 框架，在 Java 的 JDBC 层提供的额外服务。 它使用客户端直连数据库，以 jar 包形式提供服务，无需额外部署和依赖，可理解为增强版的 JDBC 驱动，完全兼容 JDBC 和各种 ORM 框架。 |
| ss-proxy | shardingsphere-Proxy                       | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务端版本，用于完成对异构语言的支持。 目前提供 MySQL 和 PostgreSQL（兼容 openGauss 等基于 PostgreSQL 的数据库）版本，它可以使用任何兼容 MySQL/PostgreSQL 协议的访问客户端（如：MySQL Command Client, MySQL Workbench, Navicat 等）操作数据，对 DBA 更加友好。 |

# 1     特性概述

针对sharding和benchmarksql进行优化改造，以达到分布式ss-jdbc性能损耗15%以内，8分片ss-jdbc性能达标1000万+。

# 2     特性测试信息

| 版本名称                    | 测试起始时间 | 测试结束时间 |
| --------------------------- | ------------ | ------------ |
| openGauss 3.0.0 Release RC7 | 2022-3-10    | 2022-3-12    |
| openGauss 3.0.0 Release RC8 | 2022-3-13    | 2022-3-15    |

| 硬件型号                                       | 硬件配置信息                                                 | 备注                            |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 3<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | 数据库节点                      |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | ss-jdbc、ss-proxy节点、tpcc节点 |

| 软件名称             | 软件版本                                                     | 备注                                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| benchmarksql         | 定制版本<br />commit：27b4eef1b6d499af68dccd33a9a1fd758f47a7db | 修改随机参数使完美sharding，增加负载均衡参数。<br />取包地址：<br />https://gitee.com/opengauss_sharding/benchmarksql |
| aarch64 jdk          | 17.0.1                                                       |                                                              |
| shardingsphere-proxy | master分支 <br />commit：bc2cefa536a538bb66e9317467b9774b95d1db3d | 合入性能优化措施<br />sharding官网取包地址：<br />https://github.com/apache/shardingsphere |
| zookeeper            | 3.7.0                                                        |                                                              |
| openGauss            | openGauss 3.0.0                                              |                                                              |



# 3     测试结论概述

## 3.1   测试整体结论

| 测试活动                                                     | 活动评价                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2P 128核环境下，分布式16节点（8pcs+7ss-jdbc+1ss-proxy）1H    | 性能测试tpmC指标验收，使用benchmarksql工具，生成8000仓库的测试数据，在7*700并发下进行1H的8节点压力测试，测试过程符合指标规定的要求，测试结果达标1000W。多轮测试下测试结果稳定。<br />测试结果：基于TaiShan 200 (Model 2280)，8节点1H，tpmC 1073.3W。 |
| 2P 128核环境下，分布式ss-jdbc性能损耗场景（1pcs+1ss-jdbc）1H功能测试 | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，ss-jdbc在700并发下进行1H的1节点压力测试，测试过程符合指标规定的要求，测试结果性能损耗15%以内。多轮测试下测试结果稳定。<br />测试结果：基于TaiShan 200 (Model 2280)，ss-jdcbc1节点1H，tpmC 136W（数据库直连1节点1H，tpmC 150W），性能损耗(150-136)/150=9.3%。 |

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

## 4.1   测试结果

### 4.1.1 2P环境下，分布式16节点（8pcs+7ss-jdbc+1ss-proxy）1H

测试步骤：

（1） 部署数据库*8；

（2） 部署ss-proxy*1，配置proxy 数据源、分库分表及server等配置文件，启动ss-proxy；

（3） 连接benchmarksql工具连接ss-proxy生成8000仓测试数据；

（4） 配置数据库极限场景参数*8；

（5） 配置ss-jdbc config文件、jar包等信息；ss-jdbc节点配置tpcc参数*7 （700并发）；

（6） 启动tpcc*7对8节点数据库进行1H压力测试；

（7） 观察输出结果。

测试结果：

​	8分片在极限场景配置下tpmC为1073.3W。

### 4.1.2 2P环境下，分布式ss-jdbc性能损耗场景（1pcs+1ss-jdbc）1H

测试步骤：

场景1：单节点数据库性能基准测试：

（1）部署数据库；

（2）连接benchmarksql工具生成1000仓测试数据；

（3）配置极限场景参数；

（4）以700并发数启动benchmarksql工具对数据库进行1H压力测试；

（5）观察输出结果。

场景2：单ss-jdbc连接单节点数据库性能测试：

（1） 部署数据库*1；

（2） 部署ss-proxy*1，配置proxy 数据源、分库分表及server等配置文件，启动ss-proxy；

（3） 连接benchmarksql工具连接ss-proxy生成1000仓测试数据；

（4） 配置数据库极限场景参数*1；

（5） 配置ss-jdbc config文件、jar包等信息；ss-jdbc节点配置tpcc参数*1 （700并发）；

（6） 启动tpcc*1对1节点数据库进行1H压力测试；

（7） 观察输出结果。

测试结果：

​	单节点数据库tpmC性能150W，ss-jdbc+单节点tpmC性能136W，损耗(150W-136W)/150W 约9.3%。

## 4.2 测试执行统计数据

| 版本名称                    | 测试用例数 | 用例执行结果 | 发现问题单数 |
| --------------------------- | ---------- | ------------ | ------------ |
| openGauss 3.0.0 Release RC7 | 2          | 通过         | 0            |
| openGauss 3.0.0 Release RC8 | 2          | 通过         | 0            |

## 4.2   后续测试建议

无

# 5     附件

无
