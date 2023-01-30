![avatar](../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述         | 作者       |
| ---------- | ----------- | ---------------- | ---------- |
| 2022-10-25 | 1.0         | 特性测试初稿完成 | lichunlong |
| 2023-1-17  | 1.1         | 问题单修改测试   | lichunlong |

 关键词： 

 openLooKeng，openGauss，tidRangescan，OLAP

摘要：本文档描述openLooKeng使用openGauss的tidRangescan插件，对openGauss数据库中的数据进行并行分析，补充openGauss的OLAP能力的测试。

 缩略语清单：

| 缩略语 | 英文全名                     | 中文解释OG                                                   |
| ------ | ---------------------------- | ------------------------------------------------------------ |
| OLK    | openLooKeng                  | 大数据分析引擎                                               |
| TID    | tidRangescan                 | 一个openGauss的插件，能实现按照      进行数据扫描            |
| OLAP   | Online Analytical Processing | 联机分析处理，大数据分析的应用技术，提供复杂的分析操作、侧重决策支持。 |
| TPC-H  | TPC-H                        | TPC-H是事务处理性能委员会（ Transaction ProcessingPerformance Council ）制定的基准程序之一，TPC- H 主要目的是评价特定查询的决策支持能力。 |

# 1     特性概述

通过openLooKeng配合使用openGauss的tidRangescan插件，并发分析openGauss数据，补充openGauss的OLAP分析能力，增强数据分析竞争力。

openLookeng的singleData Connector从openGauss获取数据表的大小，根据配置生成分片，openLooKeng Coordinator把分片调度到openLooKeng Worker，openLooKeng Worker通过JDBC从openGauss获取数据进行分析。

目前openGauss不具备并发分析数据的能力，并且备机不能参与分析数据，此特性能够使备机参与数据分析，并且是多并发的。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称               | 测试起始时间 | 测试结束时间 |
| ---------------------- | ------------ | ------------ |
| openLooKeng CLI 1.8.0  | 2022-10-9    | 2022-10-20   |
| openLooKeng CLI 1.10.0 | 2023-1-12    | 2023-1-16    |

基本测试的硬件环境信息

| 环境信息                                       | 配置信息                                                     | 备注                                                      |
| ---------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------- |
| ARM+openEuler 2p<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920-4826 2p 96核<br />内存：256GB<br />硬盘：300G<br />OS：openEuler release 20.03 (LTS)<br />文件系统：EXT4<br />网卡：1GE | 2台主机组合配置部署<br />主备部署openGauss、openLooKeng*1 |

性能测试的硬件环境信息：

| 硬件型号                                       | 配置信息                                                     | 备注                  |
| ---------------------------------------------- | ------------------------------------------------------------ | --------------------- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T <br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | OLK节点               |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280) | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T <br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE | openGauss数据库节点*3 |

| 插件名       | 源码路径                                                     |
| ------------ | ------------------------------------------------------------ |
| tidRangescan | 获取地址：https://gitee.com/opengauss/Plugin/tree/master/contrib/tidrangescan |



# 3     测试结论概述

## 3.1   测试整体结论

共计执行用例29条，主要覆盖了功能测试、可靠性测试、性能测试。发现问题8个，遗留问题3个，整体质量一般。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | （1）验证OLK通过配合openGauss TID插件，具备数据分析的能力；<br/>（2）验证不同的配置参数对执行sql的影响；<br />（3）验证tpch 10G数据量级别下21条SQL查询语句功能验证（说明：tpch SQL语句共22条，其中一条涉及create view操作，openLooKeng暂不支持该语法），测试结果与直连openGauss执行sql的结果进行对比，结果一致。<br/>**整体结论：存在部分问题，遗留处理，后续优化解决。验收通过。** |
| 可靠性测试 | 在sql执行前，对OLK和openGauss数据库进行故障注入，验证openLooKeng tid模式在故障场景下正常提示报错，故障恢复后，功能正常、可正常查询。<br />**整体结论：验收通过。** |
| 性能测试   | （1）验证tpch 100G数据量下，对比直连openGauss、OLK直连openGauss执行tpch查询sql，OLK配合使用TID插件，并发执行tpch查询sql的性能提升；<br />（2）验证tpch 100G数据量下，OLK配合使用TID插件，openGauss部署单机、1主1备和1主2备，并发执行tpch查询sql，随着备机增加性能提升；<br/>（3）验证增加分片数时，OLK配合使用TID插件，并发执行tpch查询sql，随着分片数的增加性能提升；<br/>（4）验证增加连接数，OLK配合使用TID插件，并发执行tpch查询sql，随着并发度的增加性能提升；<br/>**整体结论：对比直连openGauss、OLK直连openGauss，OLK配合TID插件性能有很大提升；但随着备机数、分片数以及连接数的增加，性能没有达到预期的提升，遗留处理，后续优化解决。** |

## 3.2   约束说明

（1）singleData连接器目前只提供对openGauss的查询功能，暂时不支持INSERT/UPDATE/DELETE等会修改数据或者数据结构的语句；

（2）openLookeng的decimal精度最高支持为38，当openGauss的decimal、numeric类型精度超过38时则无法支持；

（3）openGauss版本支持3.0.0以上；

（4）Java 8 Update 161或更高版本(8u161+) (64位)。同时支持Oracle JDK和Open JDK；AArch64 (Bisheng JDK 1.8.262 或者更高版本)；

（5）当查询的表中存在索引时，将不会启用tidrange功能；

（6）其他测试过程中发现的约束见5.2.

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 问题影响和规避措施   | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------------------- | -------- |
| [I5XEXZ](https://gitee.com/openlookeng/hetu-core/issues/I5XEXZ) | etc/config.properties配置文件中query.max-memory-per-node值配置低，内存不足导致查询失败 | 主要     | 查询失败             | 打开     |
| [I5WNUY](https://gitee.com/openlookeng/hetu-core/issues/I5WNUY?from=project-issue) | OLK for opengauss tidrange性能问题，备机增多，性能下降       | 严重     | 增加备机不能提升性能 | 打开     |
| [I6AFNT](https://gitee.com/openlookeng/hetu-core/issues/I6AFNT) | tidRange模式连接openGauss，tidrange.max-lifetime无作用       | 次要     | 配置                 | 打开     |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 8        | 2    | 3     | 3     | 0      |
| 百分比 | 100%     | 25%  | 37.5% | 37.5% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5XEXZ](https://gitee.com/openlookeng/hetu-core/issues/I5XEXZ) | 主要     | etc/config.properties配置文件中query.max-memory-per-node值配置低，内存不足导致查询失败 | 打开     |
| 2    | [I5WNUY](https://gitee.com/openlookeng/hetu-core/issues/I5WNUY?from=project-issue) | 严重     | OLK for opengauss tidrange性能问题，备机增多，性能下降       | 打开     |
| 3    | [I5W2WR](https://gitee.com/openlookeng/hetu-core/issues/I5W2WR?from=project-issue) | 主要     | openguass主备部署，OLK tidrange方式连接数据库l执行执行查询sql，主机或者备机故障，查询失败 | 关闭     |
| 4    | [I5VU6L](https://gitee.com/openlookeng/hetu-core/issues/I5VU6L?from=project-issue) | 严重     | 提升并发度为100，tpch 10G数据，执行查询sql 9，报错：FATAL: Too many clients already, current: 203, max_connections/reserved: 200/3 | 关闭     |
| 5    | [I5VFAL](https://gitee.com/openlookeng/hetu-core/issues/I5VFAL?from=project-issue) | 次要     | tidrange.page-size 与openGauss的block_size不一致时，启动未报错 | 关闭     |
| 6    | [I5VC4Q](https://gitee.com/openlookeng/hetu-core/issues/I5VC4Q?from=project-issue) | 主要     | openLooKeng for openGauss tidRange，分片大小不在规定范围内时（1MB-1GB）时未报错，OLK启动成功 | 关闭     |
| 7    | [I5WNT3](https://gitee.com/openlookeng/hetu-core/issues/I5WNT3?from=project-issue) | 次要     | OLK for opengauss tidrange特性资料问题                       | 关闭     |
| 8    | [I6AFNT](https://gitee.com/openlookeng/hetu-core/issues/I6AFNT) | 次要     | tidRange模式连接openGauss，tidrange.max-lifetime无作用       | 打开     |



# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称               | 测试用例数           | 用例执行结果            | 发现问题单数 |
| ---------------------- | -------------------- | ----------------------- | ------------ |
| openLooKeng CLI 1.8.0  | 23                   | Passed:16<br>Failed: 7  | 7            |
| openLooKeng CLI 1.10.0 | 29<br />回归issue：5 | Passed:27<br/>Failed: 2 | 1            |

数据项说明：

- 全量功能覆盖两轮测试测试执行用例29个，不通过7个，通过16个；发现问题8个，均已提单。

  第一轮测试openLooKeng适配openGauss master包，执行用例23个，通过16个，不通过7个；发现问题7个；

  第二轮测试openLooKeng适配openGauss master包，执行用例29个，通过27个，不通过2个；发现问题1个；

- 累计发现缺陷8个，遗留3个。

- 缺陷密度为8(缺陷个数)/2.9k(代码行数)=2.76(个/kloc)。

## 4.2   测试过程

### 4.2.1 性能基线测试步骤

1）部署opengauss单机数据库，设置shared_buffers=350GB，并插入100GB tpch数据；

2）直连openGauss数据库，执行21条tpch查询sql，并记录每个sql的执行时间；

3）使用OLK直连openGauss单机数据库，执行21条查询sql，并记录每个sql的执行时间；

4）openGauss安装TID插件，OLK使用 TidRange模式连接openGauss数据库，执行21条查询sql，并记录每个sql执行时间；

5）对比以上2-4步骤的测试数据，验证OLK使用openGauss的TID插件，性能提升。

### 4.2.2 增加备机性能测试步骤

1）部署openGauss1主2备数据库，设置shared_buffers=350GB，并插入100GB tpch数据；

2）OLK使用 TidRange模式连接openGauss主机数据库，执行21条查询sql，并记录每个sql的执行时间；

3）OLK使用 TidRange模式连接openGauss1主1备数据库，执行21条查询sql，并记录每个sql的执行时间；

4）OLK使用 TidRange模式连接openGauss1主2备数据库，执行21条查询sql，并记录每个sql的执行时间；

5）对比以上2-4步骤的测试数据，验证OLK使用openGauss的TID插件，openGauss数据库备机增加，性能提升。

### 4.2.3 提升分片数、增加连接数性能测试步骤

1）部署opengauss单机数据库，设置shared_buffers=350GB，并插入100GB tpch数据；

2）openGauss安装TID插件，OLK使用 TidRange模式连接openGauss数据库，默认配置下执行21条查询sql，并记录每个sql执行时间；

3）openGauss安装TID插件，OLK使用 TidRange模式连接openGauss数据库，设置分片数为100，执行21条查询sql，并记录每个sql执行时间；

4）openGauss安装TID插件，OLK使用 TidRange模式连接openGauss数据库，设置连接数为150，执行21条查询sql，并记录每个sql执行时间；

5）对比以上2-4步骤的测试数据，验证OLK使用openGauss的TID插件，性能提升。

## 4.3 性能测试统计

### 4.3.1 性能测试配置

| 组件        | 文件                            | 配置值                                     | 属性说明                                                     |
| ----------- | ------------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| openGauss   | postgresql.conf                 | shared_buffers=350GB                       | openGauss使用的共享内存大小                                  |
| openLooKeng | etc/jvm.config                  | -Xmx500G                                   | 服务器可用内存的70%（建议值，availableMem*70%)               |
| openLooKeng | etc/config.properties           | query.max-memory=350GB                     | 查询可能使用的最大分布式内存量。该参数为N*query.max-memory-per-node，其中N为工作节点数量 |
| openLooKeng | etc/config.properties           | query.max-total-memory=350GB               | 查询在任何一台机器上可能使用的最大用户内存量。该参数为JVM配置中Xmx的70%（建议值） |
| openLooKeng | etc/config.properties           | query.max-memory-per-node=350GB            | 一个查询在任意一台机器上可以使用的最大用户和系统内存量，其中系统内存是读取器、写入器和网络缓冲区等在执行期间使用的内存。该参数为JVM配置中Xmx的70%（建议值） |
| openLooKeng | etc/catalog/tidrange.properties | tidrange.max-connection-count-per-node=100 | 每个openGauss节点的最大连接数，最小值为1，各Worker的该值之和不能大于openGauss的max_connections，默认值100 |
| openLooKeng | etc/catalog/tidrange.properties | tidrange.max-table-split-count-per-node=50 | 每个表为每个openGauss节点切分的最大分片数量，最小值为1，默认值50 |
| openLooKeng | etc/catalog/tidrange.properties | tidrange.connection-timeout=0              | 获取连接最大等待时间，单位毫秒，当为0时，则永不超时，不为0时最小值为250ms |

### 4.3.2 性能基线测试数据统计（单位：秒）

| queries      | openGauss  | OLK直连openGauss | OLK for openGauss TID | 对比openGauss提升比例 | 对比OLK直连openGauss提升比例 |
| ------------ | ---------- | ---------------- | --------------------- | --------------------- | ---------------------------- |
| tpch_sql1    | 2748.86    | 3386.38          | 362.37                | 658.58%               | 834.51%                      |
| tpch_sql2    | 149.32     | 162.31           | 35.96                 | 315.24%               | 351.36%                      |
| tpch_sql3    | 501        | 1437.01          | 224.17                | 123.49%               | 541.04%                      |
| tpch_sql4    | 1250.94    | 2693.59          | 208.7                 | 499.40%               | 1190.65%                     |
| tpch_sql5    | 600.5      | 1321.75          | 279.48                | 114.86%               | 372.93%                      |
| tpch_sql6    | 279.3      | 512.88           | 185.92                | 50.23%                | 175.86%                      |
| tpch_sql7    | 509.9      | 1247.24          | 253.74                | 100.95%               | 391.54%                      |
| tpch_sql8    | 692.68     | 1475.02          | 287.33                | 141.07%               | 413.35%                      |
| tpch_sql9    | 1318.19    | 1730.03          | 317.85                | 314.72%               | 444.29%                      |
| tpch_sql10   | 448.91     | 732.87           | 203.32                | 120.79%               | 260.45%                      |
| tpch_sql11   | 71.95      | 159.65           | 35.14                 | 104.75%               | 354.33%                      |
| tpch_sql12   | 435.3      | 642.36           | 198.54                | 119.25%               | 223.54%                      |
| tpch_sql13   | 367.63     | 419.11           | 55.94                 | 557.19%               | 649.21%                      |
| tpch_sql14   | 277.29     | 399.48           | 193.07                | 43.62%                | 106.91%                      |
| tpch_sql16   | 125.09     | 126.01           | 21.36                 | 485.63%               | 489.93%                      |
| tpch_sql17   | 424.93     | 1247.53          | 350.01                | 21.41%                | 256.43%                      |
| tpch_sql18   | 1691.04    | 1057.74          | 321.79                | 425.51%               | 228.71%                      |
| tpch_sql19   | 344.73     | 599.4            | 178.33                | 93.31%                | 236.12%                      |
| tpch_sql20   | 518.76     | 807.22           | 197.84                | 162.21%               | 308.02%                      |
| tpch_sql21   | 1160.8     | 3752.13          | 369.08                | 214.51%               | 916.62%                      |
| tpch_sql22   | 123.16     | 197.54           | 40.81                 | 201.79%               | 384.05%                      |
| **totalAVG** | **668.58** | **1147.96**      | **205.75**            | **224.95%**           | **457.94%**                  |

### 4.3.3 增加备机性能测试数据统计（单位：秒）

| queries      | 单机       | 1主1备     | 1主2备     | 单机对比1主1备 | 单机对比1主2备 |
| ------------ | ---------- | ---------- | ---------- | -------------- | -------------- |
| tpch_sql1    | 362.37     | 400.54     | 447.88     | -9.53%         | -19.09%        |
| tpch_sql2    | 35.96      | 39.58      | 43.25      | -9.15%         | -16.86%        |
| tpch_sql3    | 224.17     | 270.03     | 310.82     | -16.98%        | -27.88%        |
| tpch_sql4    | 208.7      | 249.81     | 266.57     | -16.46%        | -21.71%        |
| tpch_sql5    | 279.48     | 332.23     | 330.26     | -15.88%        | -15.38%        |
| tpch_sql6    | 185.92     | 225.11     | 257.86     | -17.41%        | -27.90%        |
| tpch_sql7    | 253.74     | 294.17     | 324.37     | -13.74%        | -21.77%        |
| tpch_sql8    | 287.33     | 335.06     | 384.99     | -14.25%        | -25.37%        |
| tpch_sql9    | 317.85     | 371.11     | 380.06     | -14.35%        | -16.37%        |
| tpch_sql10   | 203.32     | 254.65     | 269.53     | -20.16%        | -24.56%        |
| tpch_sql11   | 35.14      | 32.88      | 35.94      | 6.87%          | -2.23%         |
| tpch_sql12   | 198.54     | 237.17     | 268.01     | -16.29%        | -25.92%        |
| tpch_sql13   | 55.94      | 59.05      | 63.65      | -5.27%         | -12.11%        |
| tpch_sql14   | 193.07     | 237.67     | 244.53     | -18.77%        | -21.04%        |
| tpch_sql16   | 21.36      | 26.43      | 28.64      | -19.18%        | -25.42%        |
| tpch_sql17   | 350.01     | 373.24     | 387.15     | -6.22%         | -9.59%         |
| tpch_sql18   | 321.79     | 350.2      | 404.52     | -8.11%         | -20.45%        |
| tpch_sql19   | 178.33     | 211.43     | 234.92     | -15.66%        | -24.09%        |
| tpch_sql20   | 197.84     | 228.03     | 272.34     | -13.24%        | -27.36%        |
| tpch_sql21   | 369.08     | 448.55     | 450.44     | -17.72%        | -18.06%        |
| tpch_sql22   | 40.81      | 48.29      | 42.44      | -15.49%        | -3.84%         |
| **totalAVG** | **205.75** | **239.30** | **259.44** | **-14.02%**    | **-20.69%**    |

### 4.3.3 提升分片数、增加连接数性能测试数据统计

| queries      | 默认配置   | 分片数100  | 连接数150  | 默认配置对比分片数100 | 默认配置对比连接数150 |
| ------------ | ---------- | ---------- | ---------- | --------------------- | --------------------- |
| tpch_sql1    | 362.37     | 354.11     | 255.23     | 2.33%                 | 41.98%                |
| tpch_sql2    | 35.96      | 30.2       | 36.33      | 19.07%                | -1.02%                |
| tpch_sql3    | 224.17     | 262.56     | 216.38     | -14.62%               | 3.60%                 |
| tpch_sql4    | 208.7      | 256.99     | 236.13     | -18.79%               | -11.62%               |
| tpch_sql5    | 279.48     | 274.13     | 234.05     | 1.95%                 | 19.41%                |
| tpch_sql6    | 185.92     | 224.52     | 193.53     | -17.19%               | -3.93%                |
| tpch_sql7    | 253.74     | 281.67     | 256.39     | -9.92%                | -1.03%                |
| tpch_sql8    | 287.33     | 297.47     | 265.36     | -3.41%                | 8.28%                 |
| tpch_sql9    | 317.85     | 313.92     | 274.31     | 1.25%                 | 15.87%                |
| tpch_sql10   | 203.32     | 241.48     | 200.65     | -15.80%               | 1.33%                 |
| tpch_sql11   | 35.14      | 28.61      | 33.78      | 22.82%                | 4.03%                 |
| tpch_sql12   | 198.54     | 267.79     | 196.1      | -25.86%               | 1.24%                 |
| tpch_sql13   | 55.94      | 58.69      | 54.96      | -4.69%                | 1.78%                 |
| tpch_sql14   | 193.07     | 214.69     | 184.21     | -10.07%               | 4.81%                 |
| tpch_sql16   | 21.36      | 28.98      | 28.21      | -26.29%               | -24.28%               |
| tpch_sql17   | 350.01     | 493.68     | 264.72     | -29.10%               | 32.22%                |
| tpch_sql18   | 321.79     | 307.81     | 295.98     | 4.54%                 | 8.72%                 |
| tpch_sql19   | 178.33     | 203.3      | 171.36     | -12.28%               | 4.07%                 |
| tpch_sql20   | 197.84     | 240.47     | 182.06     | -17.73%               | 8.67%                 |
| tpch_sql21   | 369.08     | 299.32     | 279        | 23.31%                | 32.29%                |
| tpch_sql22   | 40.81      | 37.89      | 38.98      | 7.71%                 | 4.69%                 |
| **totalAVG** | **205.75** | **224.68** | **185.61** | **-8.43%**            | **10.85%**            |

### 

## 4.4 后续测试建议

1、等待openGauss的TID插件支持列存表查询后，应对列存表进行测试，并与行存表性能进行对比；

# 5     附件

## 5.1 openLookeng官网资料链接

-  openLooKeng部署文档：

  https://gitee.com/openlookeng/hetu-core/blob/master/hetu-docs/zh/installation/deployment.md

- openLooKeng+tidRangescan配置说明文档：

  https://gitee.com/openlookeng/hetu-core/blob/master/hetu-docs/zh/connector/singledata.md

- tidRangescan部署文档：

  https://gitee.com/opengauss/Plugin/blob/master/contrib/tidrangescan/README.md

## 5.2 测试过程中发现的限制

-  avg(DECIMAL[(p[,s])])结果精度按字段本身精度计算，例如DECIMAL字段a精度为0，avg(a)结果精度也为0；
- tidRangescan插件不支持列存表查询，导致OLK查询列存表失败。

 
