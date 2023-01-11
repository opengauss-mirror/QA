![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者       |
| ---------- | ----------- | -------------------- | ---------- |
| 2022-12-17 | 1.0         | 特性测试报告初稿完成 | peilinqian |
| 2022-1-11  | 1.1         | 修改检视意见         | peilinqian |

关键词： 

openGauss 分布式、ShardingSphere-Proxy、ShardingSphere-JDBC、federation、跨库查询

摘要：

本文档主要验证openGauss分布式方案ShardingSphere SQL Federation引擎功能，包括：跨库场景下表查询、子查询、聚合查询和跨库视图查询等复杂 SQL 语句，并给出最终测试结论。

缩略语清单：

| 缩略语   | 英文全名             | 中文解释                                                     |
| -------- | -------------------- | ------------------------------------------------------------ |
| ss-proxy | shardingsphere-Proxy | 定位为透明化的数据库代理端，提供封装了数据库二进制协议的服务端版本，用于完成对异构语言的支持。 目前提供 MySQL 和 PostgreSQL（兼容 openGauss 等基于 PostgreSQL 的数据库）版本，它可以使用任何兼容 MySQL/PostgreSQL 协议的访问客户端（如：MySQL Command Client, MySQL Workbench, Navicat 等）操作数据，对 DBA 更加友好。 |
| ss-jdbc  | shardingsphere-JDBC  | 定位为轻量级 Java 框架，在 Java 的 JDBC 层提供的额外服务。 它使用客户端直连数据库，以 jar 包形式提供服务，无需额外部署和依赖，可理解为增强版的 JDBC 驱动，完全兼容 JDBC 和各种 ORM 框架。 |
| ZK       | zookeeper            | ZooKeeper 是 Apache 软件基金会的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册。 |



# 1     特性概述

openGauss分布式方案中，当涉及非分片键的关联查询和跨库join查询时，会提示语法错误而停止执行；针对此问题，shardingSphere已经开发federation模块以扩大分布式语法适配范围，但是目前ShardingSphere SQL Federation 引擎对于openGauss 没有特别适配，导致很多openGauss 特有的语法无法支持，为了提升ShardingSphere + openGauss分布式数据库方案的体验，本需求对openGauss 数据库进行适配，支持openGauss特有语法，同时完美支持跨库表查询、子查询、聚合查询和跨库视图查询等复杂 SQL 语句。在支持这些复杂查询的同时，需要保证事务功能能够正常运行。

# 2     特性测试信息

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| ------------------------------------------------------------ | ------------ | ------------ |
| ShardingSphere-5.2.1-SNAPSHOT Commit ID: 20bf595dfeced4dd8ffee2f6d95de52fdf3e569d | 2022/11/14   | 2022/11/14   |
| ShardingSphere-5.2.2-SNAPSHOT Commit ID: 753c0cee8ee6fd3db00536da55b64bc5198a3758 | 2022/11/23   | 2022/12/7    |
| ShardingSphere-5.2.2-SNAPSHOT Commit ID: 631fdf40f87223e176abe5c851a51b3287b4d6de | 2022/12/12   | 2022/12/13   |
| ShardingSphere-5.3.1-SNAPSHOT Commit ID: 0cbb5fb100de531e7e27382904197c1249baf2d6 | 2022/12/16   | 2022/12/16   |
| ShardingSphere-5.3.1-SNAPSHOT Commit ID: fcb0e1ea635accbf34cf491ae3f5ad463a4cfbb9 | 2022/1/5     | 2022/1/5     |

| 环境信息   | 配置信息                                                     | 备注                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| x86+centOS | Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz 8核<br />内存：32GB<br/>硬盘：100G<br/>OS：CentOS Linux release 7.6.1810 (Core) | 3台主机组合配置部署<br />至少2分片opengauss（一主两备）、1 ss-proxy、1 zookeeper |

| 软件名称             | 软件版本                                                     | 备注                                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| shardingsphere-Proxy | **ShardingSphere-5.2.1-SNAPSHOT Commit ID:** 20bf595dfeced4dd8ffee2f6d95de52fdf3e569d<br/>**ShardingSphere-5.2.2-SNAPSHOTCommit ID:** 753c0cee8ee6fd3db00536da55b64bc5198a3758<br/>631fdf40f87223e176abe5c851a51b3287b4d6de<br/>**ShardingSphere-5.3.1-SNAPSHOT Commit ID:**  0cbb5fb100de531e7e27382904197c1249baf2d6 <br />fcb0e1ea635accbf34cf491ae3f5ad463a4cfbb9 | 官网源码包github地址：<br />https://github.com/apache/shardingsphere |
| zookeeper            | 3.8.0                                                        |                                                              |
| openGauss            | openGauss 3.1.0<br />226d0461                                |                                                              |

# 3     测试结论概述

## 3.1   测试整体结论

共设计28个用例，主要覆盖了功能测试和可靠性测试，发现问题19个（本需求相关缺陷11个，非本需求的select语法缺陷8个），已解决11个（本需求相关缺陷8个，非本需求的select语法缺陷3个），待解决8个（本需求相关缺陷3个，非本需求的select语法缺陷5个），整体需求目标达成。目前存在部分待解决问题中由于sharding内核本身不支持的语法场景，遗留8个问题后续解决优化。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | （1）通过yaml、DistSQL两种方式进行验证federation开关功能。<br />（2）查询语法覆盖；主要验证federation场景下绑定表不带分片键、非绑定表且分片规则不同等场景下多表关联（join）查询、子查询、分组聚合查询、UNION/INTERSECT/EXCEPT/MINUS查询等语法。<br />（3）视图语法覆盖；主要验证federation场景下视图创建包含绑定表不带分片键、非绑定表且分片规则不同等场景下多表关联（join）查询、子查询、分组聚合查询、UNION/INTERSECT/EXCEPT/MINUS查询等语法。<br />（4）视图及表组合场景覆盖；主要验证视图及表关联查询、子查询、分组聚合查询、UNION/INTERSECT/EXCEPT/MINUS查询等场景。<br />（5）federation打开关闭场景下tpcc功能验证。由于目前federation场景仅支持select 语法，导致tpcc非完美sharding场景无法支持。目前仅验证完美sharding场景。<br />（6）事务场景验证，验证LOCAL及XA事务基本功能正常。<br />（7）读写分离场景下，验证select查询以上所有场景脚本，查询结果正确一致。<br />结论：federation场景下，跨库关联查询功能正常，整体需求功能达成，验证通过。 |
| 可靠性测试 | 一个session循环进行federation跨库关联查询，一个session循环进行federation开关修改，开关打开查询正常，开关关闭后正常报错。<br />结论：验收通过 |

## 3.2   约束说明

（1）需要通过server.yaml配置`sql-federation-type: ADVANCED`或通过DistSQL语法`set dist variable sql_federation_type="ADVANCED"`进行federation开关打开，才能使用federation跨库查询功能。

（2）目前cursor语法未进行改造，不支持federation。

（3）目前federation仅支持select语法，insert、update涉及跨库关联查询不支持。

（4）目前打开关闭federation开关、创建修改视图操作会异步刷新Zookeeper元数据，需要等待刷新后进行相应select操作，或者通过执行强制刷新DistSQL命令`refresh database metadata from governance center`强制刷新后进行相应select操作。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 问题影响和规避措施                                           | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| [22375](https://github.com/apache/shardingsphere/issues/22375) | group by 语法，查询字段不能包含除groupby列以外的其他列。     | 次要     | group by以外的其他列查询会受影响。                           | 打开     |
| [22388](https://github.com/apache/shardingsphere/issues/22388) | GROUPING SETS、ROLLUP、CUBE语法不支持。                      | 次要     | GROUPING SETS、ROLLUP、CUBE SQL无法执行。                    | 打开     |
| [22412](https://github.com/apache/shardingsphere/issues/22412) | sharding-proxy使用未适配过sharding的tpcc执行tpcc异常，主键冲突；目前涉及的insert、update跨库操作不支持。 | 次要     | 会影响涉及跨库select的insert、update操作。                   | 打开     |
| [22514](https://github.com/apache/shardingsphere/issues/22514) | 使用distinct on语法查询结果不正确，未进行去重。              | 主要     | distinct on 查询结果不正确。                                 | 打开     |
| [22553](https://github.com/apache/shardingsphere/issues/22553) | 创建视图完成后，查询视图的结果概率性是数据源单个库的结果，而非所有数据源的查询结果；目前仅yat框架执行自动化SQL脚本存在问题。 | 次要     | 进行视图创建需要刷新元数据，创建视图后，可能会导致短暂查询视图不存在或者结果不正确。可以通过sleep等待1s左右再进行查询。后续优化。 | 打开     |
| [22569](https://github.com/apache/shardingsphere/issues/22569) | 创建视图完成后，查询视图的结果概率性报错视图不存在，目前仅yat框架执行自动化SQL脚本存在问题。 | 次要     | 进行视图创建需要刷新元数据，创建视图后，可能会导致短暂查询视图不存在或者结果不正确。可以通过sleep等待1s左右再进行查询。后续优化。 | 打开     |
| [22823](https://github.com/apache/shardingsphere/issues/22823) | 不区分大小写排序（order by nlssort）功能不支持，相关视图创建成功，查询报错。 | 次要     | 不区分大小写排序语法不支持。                                 | 打开     |
| [22841](https://github.com/apache/shardingsphere/issues/22841) | 在federation场景下，视图与表关联查询，group by场景下，如果where条件有视图相关字段，sql语句执行异常。 | 主要     | group by场景下，视图关联查询时，where条件不能包含视图字段。  | 打开     |

### 3.3.2 问题统计

|        | 问题总数 | 严重  | 主要   | 次要   | 不重要 |
| ------ | -------- | ----- | ------ | ------ | ------ |
| 数目   | 19       | 0     | 11     | 8      | 0      |
| 百分比 | 100%     | 0.00% | 57.89% | 42.11% | 0.00%  |

**是否本需求缺陷统计信息：**

| 问题分类           | 问题总数 | 已关闭 | 打开 |
| ------------------ | -------- | ------ | ---- |
| 本需求相关缺陷     | 11       | 8      | 3    |
| 原sharding内核缺陷 | 8        | 3      | 5    |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 是否需求缺陷 | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ------------ | -------- |
| 1    | [22173](https://github.com/apache/shardingsphere/issues/22173) | 主要     | sql-federation-type未进行有效值校验，每次执行sql都会进行初始化federation引擎。 | 是           | 关闭     |
| 2    | [22170](https://github.com/apache/shardingsphere/issues/22170) | 主要     | select group by非分片键（列存在空值）查询结果不正确。        | 是           | 关闭     |
| 3    | [22355](https://github.com/apache/shardingsphere/issues/22355) | 主要     | join with USING(column)' 的查询结果存在问题；列名称和值不匹配。 | 是           | 关闭     |
| 4    | [22359](https://github.com/apache/shardingsphere/issues/22359) | 主要     | select关联查询场景‘natural join’语法不支持，报错ERROR: The column index is out of range: 10, number of columns: 9。 | 是           | 关闭     |
| 5    | [22375](https://github.com/apache/shardingsphere/issues/22375) | 次要     | group by 语法，查询字段不能包含除groupby列以外的其他列。     | 否           | 打开     |
| 6    | [22388](https://github.com/apache/shardingsphere/issues/22388) | 次要     | GROUPING SETS、ROLLUP、CUBE语法不支持。                      | 否           | 打开     |
| 7    | [22412](https://github.com/apache/shardingsphere/issues/22412) | 次要     | sharding-proxy使用未适配过sharding的tpcc执行tpcc异常，主键冲突；目前涉及的insert、update跨库操作不支持。 | 否           | 打开     |
| 8    | [22432](https://github.com/apache/shardingsphere/issues/22432) | 主要     | sharding-proxy使用适配过sharding的tpcc（完美sharding的场景）进行建仓成功，runBenchmark失败。 | 是           | 关闭     |
| 9    | [22438](https://github.com/apache/shardingsphere/issues/22438) | 主要     | select语法fetch offset等场景异常‘java.lang.NullPointerException’。 | 否           | 关闭     |
| 10   | [22480](https://github.com/apache/shardingsphere/issues/22480) | 主要     | 使用orderby的视图进行查询使用使用limit 5,3或者limit 3 offset 5，查询结果顺序不正确。 | 是           | 关闭     |
| 11   | [22514](https://github.com/apache/shardingsphere/issues/22514) | 主要     | 使用distinct on语法查询结果不正确，未进行去重。              | 否           | 打开     |
| 12   | [22553](https://github.com/apache/shardingsphere/issues/22553) | 次要     | 创建视图完成后，查询视图的结果概率性是数据源单个库的结果，而非所有数据源的查询结果；目前仅yat框架执行自动化SQL脚本存在问题。 | 是           | 打开     |
| 13   | [22569](https://github.com/apache/shardingsphere/issues/22569) | 次要     | 创建视图完成后，查询视图的结果概率性报错视图不存在，目前仅yat框架执行自动化SQL脚本存在问题。 | 是           | 打开     |
| 14   | [22576](https://github.com/apache/shardingsphere/issues/22576) | 次要     | 在federation场景下，‘UNION/EXCEPT/INTERSECT’的优先级不正确。 | 是           | 关闭     |
| 15   | [22823](https://github.com/apache/shardingsphere/issues/22823) | 次要     | 不区分大小写排序（order by nlssort）功能不支持，相关视图创建成功，查询报错。 | 否           | 打开     |
| 16   | [22820](https://github.com/apache/shardingsphere/issues/22820) | 主要     | 在federation场景下，视图创建子句为多个分片表进行union/intersect/except/minus，创建视图报错“ERROR: String index out of range: -7”。 | 是           | 关闭     |
| 17   | [22822](https://github.com/apache/shardingsphere/issues/22822) | 主要     | fetch子句查询结果不正确。Fetch count预期应该为fetch count，实际结果为偏移count。 | 否           | 关闭     |
| 18   | [22826](https://github.com/apache/shardingsphere/issues/22826) | 次要     | “select order by null first/last”或者“create view order by null first/last”结果不正确；并未按空值先序。 | 否           | 关闭     |
| 19   | [22841](https://github.com/apache/shardingsphere/issues/22841) | 主要     | 在federation场景下，视图与表关联查询，group by场景下，如果where条件有视图相关字段，sql语句执行异常。 | 是           | 打开     |

# 4     测试执行

## 4.1   测试组网图

![pic1](../images/openGauss分布式方案2分片读写分离组网图.png)

## 4.2   测试执行统计数据

| 版本名称                                                     | 测试用例数             | 用例执行结果                              | 发现问题单数 |
| ------------------------------------------------------------ | ---------------------- | ----------------------------------------- | ------------ |
| ShardingSphere-5.2.1-SNAPSHOT Commit ID: 20bf595dfeced4dd8ffee2f6d95de52fdf3e569d | 2                      | Passed：0 Failed：2                       | 2            |
| ShardingSphere-5.2.2-SNAPSHOT Commit ID: 753c0cee8ee6fd3db00536da55b64bc5198a3758 | 28<br />回归issue：2个 | Passed：22 Failed：6<br />issue Passed：2 | 13           |
| ShardingSphere-5.2.2-SNAPSHOT Commit ID: 631fdf40f87223e176abe5c851a51b3287b4d6de | 回归issue：6个         | issue Passed：6                           | 4            |
| ShardingSphere-5.3.1-SNAPSHOT Commit ID: 0cbb5fb100de531e7e27382904197c1249baf2d6 | 回归issue：2个         | issue Passed：2                           | 0            |
| ShardingSphere-5.3.1-SNAPSHOT Commit ID: fcb0e1ea635accbf34cf491ae3f5ad463a4cfbb9 | 回归issue：1个         | issue Passed：1                           | 0            |

*数据项说明：*

- 首轮版本测试冒烟测试，第二轮全量用例执行，第三至五轮回归问题单；
- 最终测试用例执行28个，不通过6个，缺陷见全部记录遗留问题单；
- 本需求缺陷密度为11(缺陷个数)/62.74k(代码行数)=0.175(个/kloc)。

## 4.3   后续测试建议

遗留缺陷解决后，需要覆盖验证。

# 5     附件

**select不支持情况如下：**

- 不支持 WITH 语句，例如： WITH temp_t(name,isdba) AS (SELECT usename,usesuper FROM pg_user) SELECT * FROM temp_t ；
- 不支持 plan_hint 子句；
- 不支持 START WITH .. CONNECT BY 子句；
- 不支持 WINDOW 子句；
- 不支持 FOR UPDATE，FOR NO KEY UPDATE，FOR SHARE 或 FOR KEY SHARE 加锁语句；
- 不支持 复杂GROUP BY 语法，比如：GROUPING SETS、ROLLUP、CUBE语法。



 

 
