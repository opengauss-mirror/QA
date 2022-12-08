![avatar](../../images/openGauss.png)

版权所有 © 2022 openGauss社区  您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问<https://creativecommons.org/licenses/by-sa/4.0/>
了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode>。

修订记录

| 日期       | 修订版本 | CR号 | 修改  章节              | 修改描述                                | 作者                      |
| ---------- | -------- | ---- | ----------------------- | --------------------------------------- | ------------------------- |
| 2022.10.28 | 1.0      |      |                         | 版本测试策略初稿                        | yansong_lee<br>zhangao_za |
| 2022.11.10 | 1.1      |      | 新增feature测试设计策略 | 不分feature验证策略、重点和设计思路补充 | yansong_lee               |
|            |          |      |                         |                                         |                           |


目 录

1 概述

>   1.1 版本背景

>   1.2 需求范围

2 风险

3 测试分层策略

4 测试分析设计策略

>   4.1 新增feature测试设计策略

>   4.2 继承feature/组件测试设计策略

>   4.3 专项测试策略

5 测试执行策略

6 附件

**Keywords 关键词**：openGauss 3.1.1

Abstract 摘要：本文是openGauss 3.1.1版本的整体测试策略，用于指导该版本测试活动开展

>   缩略语清单：

| 缩略语 | 英文全名                             | 中文解释       |
| ------ | ------------------------------------ | -------------- |
| CVE    | Common Vulnerabilities and Exposures | 公共漏洞和暴露 |
| SQL    | Structured Query Language            | 结构化查询语言 |
| DML    | Data Manipulation Language           | 数据操纵语言   |
| DDL    | Data Definition Language             | 数据定义语言   |
| DCL    | Data Control Language                | 数据控制语言   |

# <br>概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

本文主要描述了openGauss 3.1.1版本测试策略，将于2022年12月30日在openGauss社区发布，按照社区开发模式进行运作，结合社区release-manager团队指定的版本计划规划相应的测试活动。整体策略覆盖新需求、继承需求的测试分析和测试执行，明确各个测试周期的策略及出入口标准，指导后续测试活动。

## 版本背景

openGauss 3.1.1版本瞄准2023年发布的openGauss Release版本，旨在提前合入竞争力需求，以此腾出更多的时间来规划并执行质量加固活动。openGauss 3.1.1版本面向openGauss的场景化、工具链和兼容性场景，提供更多新特性和功能，给开发者和用户带来全新的体验，服务更多的领域和更多的用户。除了修复bug和cve漏洞外，本次发布的有：

1. 【openGauss 3.1.1】支持基于共享存储的资源池化架构
2. 【openGauss 3.1.1】shardingSphere&openGauss 32节点挑战2600W tpmC
3. 【openGauss 3.1.1】存储层实现算子卸载，SMP查询性能提升20%
4. 【openGauss 3.1.1】支持CM的部署和数据库部署解耦
5. 【openGauss 3.1.1】M*兼容性增强

等重大特性。

## 需求范围

结合社区release-manager团队制定的[版本计划](https://gitee.com/opengauss/release-management/blob/master/openGauss%203.1.1/release-plan.md)，openGauss 3.1.1版本发布的需求列表如下：

1. **内核场景化**：涉及15个新特性，发布共享存储新特性，增强分布式能力，推出算子卸载、多节点写入等其他内核新特性

   | no   | feature                                                      | status     | sig           | owner                |
   | ---- | ------------------------------------------------------------ | ---------- | ------------- | -------------------- |
   | 1    | 【openGauss 3.1.1】支持全局逻辑时钟组件GLT，满足分布式事务管理要求 | Developing | StorageEngine |                      |
   | 2    | 【openGauss 3.1.1】对shardingSphere+openLookeng+openGauss统一的安装部署能力 | Developing | StorageEngine | sphereEx+openLookeng |
   | 3    | 【openGauss 3.1.1】shardingSphere&openGauss 32节点挑战2600W tpmC | Developing | StorageEngine | sphereEx             |
   | 4    | 【openGauss 3.1.1】openGauss-分布式数据库支持事务下多种模式读写分离 | Developing | StorageEngine | sphereEx             |
   | 5    | 【openGauss 3.1.1】openGauss分布式方案适配federation以支持跨库查询 | Developing | StorageEngine | sphereEx             |
   | 6    | 【openGauss 3.1.1】openGauss分布式方案第二阶段15节点1300万性能提升 | Developing | StorageEngine | sphereEx             |
   | 7    | 【openGauss 3.1.1】DMS适配OCK分布式锁                        | Developing | StorageEngine |                      |
   | 8    | 【openGauss 3.1.1】存储层实现算子卸载，SMP查询性能提升20%    | Developing | StorageEngine |                      |
   | 9    | 【openGauss 3.1.1】支持对dbe_perf.file_iostat统计的LRU淘汰算法 | Developing | StorageEngine |                      |
   | 10   | 【openGauss 3.1.1】支持多节点写入                            | Developing | StorageEngine |                      |
   | 11   | 【openGauss 3.1.1】支持数据库物理读写块大小的配置功能        | Developing | StorageEngine |                      |
   | 12   | 【openGauss 3.1.1】postgresql_fdw执行性能提升，支持聚合和条件下推到远端执行功能 | Developing | SQLEngine     |                      |
   | 13   | 【openGauss 3.1.1】提升特定情况下sort的性能                  | Developing | SQLEngine     | Enmotech             |
   | 14   | 【openGauss 3.1.1】支持垂直分区                              | Developing | SQLEngine     | Vastdata             |
   | 15   | 【openGauss 3.1.1】强制访问控制                              | Developing | SQLEngine     | Vastdata             |

2. **工具链**：涉及17个新特性，主要是迁移工具、校验工具以及OM/CM等工具增强

   | no   | feature                                                      | status     | sig           | owner    |
   | ---- | ------------------------------------------------------------ | ---------- | ------------- | -------- |
   | 1    | 【openGauss 3.1.1】支持CM的部署和数据库部署解耦              | Developing | CM            |          |
   | 2    | 【openGauss 3.1.1】OM支持在XML中对同步备机的设置             | Developing | OM            |          |
   | 3    | 【openGauss 3.1.1】支持openEuler 22.03 LTS系统               | Developing | OM            |          |
   | 4    | 【openGauss 3.1.1】提升数据校验性能，满足数据校验性能达到70MB/s | Developing | Tools         |          |
   | 5    | 【openGauss 3.1.1】数据校验支持行级、列级和全量比较规则      | Developing | Tools         |          |
   | 6    | 【openGauss 3.1.1】debezium全量迁移支持对M\*对象的迁移       | Developing | Tools         |          |
   | 7    | 【openGauss 3.1.1】M\*数据迁移完整方案打通                   | Developing | Tools         |          |
   | 8    | 【openGauss 3.1.1】增量、反向迁移支持断点续传                | Developing | Tools         |          |
   | 9    | 【openGauss 3.1.1】迁移工具可调试性增强、可靠性增强          | Developing | Tools         |          |
   | 10   | 【openGauss 3.1.1】支持一站式迁移平台                        | Developing | Tools         |          |
   | 11   | 【openGauss 3.1.1】openGauss可视化运维项目                   | Developing | Tools         | 众智     |
   | 12   | 【openGauss 3.1.1】openGauss-DataStudio 适配存储过程覆盖率工具开发合作项目 | Developing | Tools         | 众智     |
   | 13   | 【openGauss 3.1.1】openGauss数据建模项目                     | Developing | Tools         | 众智     |
   | 14   | 【openGauss 3.1.1】openGauss工具一体化平台                   | Developing | Tools         | 众智     |
   | 15   | 【openGauss 3.1.1】支持CM两节点特性                          | Developing | Tools         | Enmotech |
   | 16   | 【openGauss 3.1.1】gstrace工具新增特性，支持数据库运行中追踪 | Developing | Tools         | Enmotech |
   | 17   | 【openGauss 3.1.1】openGauss周边工具适配DMS&DSS              | Developing | StorageEngine |          |

3. **兼容性**：涉及48个新特性，从数据类型兼容、系统函数兼容、DDL兼容、DML兼容、PL/SQL兼容、SHOW语法兼容和其他语法兼容方面提升openGauss对openGauss对M*的兼容率

   | no   | feature                                  | combination                                                  | status     | sig    | owner    |
   | ---- | ---------------------------------------- | ------------------------------------------------------------ | ---------- | ------ | -------- |
   | 1    | 【openGauss 3.1.1】数据类型兼容 **2个**  | 支持用户在M\*兼容性下对bool类型的输出统一转成tinyint         | Developing | Plugin |          |
   |      |                                          | tinyint支持有符号，同时支持unsigned tinyint                  | Developing | Plugin |          |
   | 2    | 【openGauss 3.1.1】系统函数兼容 **9个**  | openGauss兼容M\*日期处理函数                                 | Developing | Plugin | 众智     |
   |      |                                          | 兼容性时间类型相关函数                                       | Developing | Plugin | 众智     |
   |      |                                          | 兼容性JSON类型相关函数及操作符                               | Developing | Plugin | 众智     |
   |      |                                          | any_value聚合函数                                            | Developing | Plugin | Enmotech |
   |      |                                          | 支持row_count函数                                            | Developing | Plugin | Enmotech |
   |      |                                          | 支持database()函数                                           | Developing | Plugin | Enmotech |
   |      |                                          | CAST语法的类型补全                                           | Developing | Plugin | Enmotech |
   |      |                                          | 支持default函数用于SQL语句                                   | Developing | Plugin | Enmotech |
   |      |                                          | 支持found_rows特性                                           | Developing | Plugin | Enmotech |
   | 3    | 【openGauss 3.1.1】DDL兼容 **7个**       | 支持指定FOREIGN DATA WRAPPER为M\*                            | Developing | Plugin |          |
   |      |                                          | 支持CREATE USER带if not exists选项                           | Developing | Plugin |          |
   |      |                                          | drop trigger支持不指定表名的级联删除                         | Developing | Plugin | Vastdata |
   |      |                                          | 支持分区语法值指定 less than maxvalue时候，无需 less than maxvalue()如此指定括号 | Developing | Plugin | Enmotech |
   |      |                                          | 支持分区表使用函数作为分区条件                               | Developing | Plugin | Enmotech |
   |      |                                          | drop trigger支持不指定表名的级联删除                         | Developing | Plugin | Vastdata |
   |      |                                          | 兼容MySQL创建存储过程的语法格式                              | Developing | Plugin | Vastdata |
   | 4    | 【openGauss 3.1.1】DML兼容 **5个**       | 视图支持插入、删除和更新                                     | Developing | Plugin |          |
   |      |                                          | insert支持set后面的表达式右值带字段名                        | Developing | Plugin |          |
   |      |                                          | prepare、execute语法兼容                                     | Developing | Plugin |          |
   |      |                                          | 实现prepare stmt、rename user、alter function...SQL SECURITY等M\*兼容性 | Developing | Plugin | 众智     |
   |      |                                          | 二级分区支持reorginze                                        | Developing | Plugin | Enmotech |
   | 5    | 【openGauss 3.1.1】PL/SQL兼容 **7个**    | 存储过程支持RETURN                                           | Developing | Plugin | Vastdata |
   |      |                                          | 支持create procedure select语法                              | Developing | Plugin | Vastdata |
   |      |                                          | 存储过程支持REPEAT                                           | Developing | Plugin | Vastdata |
   |      |                                          | 存储过程condition处理                                        | Developing | Plugin | Vastdata |
   |      |                                          | 支持IF_THEN控制语法                                          | Developing | Plugin | Vastdata |
   |      |                                          | 支持CASE WHEN condition控制语法                              | Developing | Plugin | Vastdata |
   |      |                                          | 支持WHILE循环带标签                                          | Developing | Plugin | Vastdata |
   | 6    | 【openGauss 3.1.1】SHOW语法兼容 **4个**  | 支持对M\*列类型的映射                                        | Developing | Plugin |          |
   |      |                                          | 兼容SHOW PRIVILEGES语法                                      | Developing | Plugin | Unicom   |
   |      |                                          | 兼容SHOW TABLE STATUS语法                                    | Developing | Plugin | Unicom   |
   |      |                                          | 支持show warnings，show errors语法                           | Developing | Plugin | Enmotech |
   | 7    | 【openGauss 3.1.1】其他语法兼容 **16个** | 实现对Load DATA语法的兼容，支持使用load语句导入数据          | Developing | Plugin |          |
   |      |                                          | 支持对M\*部分语法的转换                                      | Developing | Plugin |          |
   |      |                                          | 支持查看系统可支持的字符集和字符序                           | Developing | Plugin |          |
   |      |                                          | 支持使用双引号表示字符串                                     | Developing | Plugin |          |
   |      |                                          | 支持explain和desc保持一致，互为同义词                        | Developing | Plugin |          |
   |      |                                          | 指定多个分区查询数据                                         | Developing | Plugin |          |
   |      |                                          | 支持autocommit=0模式                                         | Developing | Plugin |          |
   |      |                                          | 支持!操作符                                                  | Developing | Plugin |          |
   |      |                                          | 对M*数据库参数适配                                           | Developing | Plugin |          |
   |      |                                          | 兼容FLUSH BINARY LOGS语法                                    | Developing | Plugin | Unicom   |
   |      |                                          | 兼容M*协议                                                   | Developing | Plugin | Unicom   |
   |      |                                          | 支持DO statement语法                                         | Developing | Plugin | Vastdata |
   |      |                                          | 支持cursor相关语法                                           | Developing | Plugin | Vastdata |
   |      |                                          | 支持反引号作为标识符                                         | Developing | Plugin | Enmotech |

# 风险

这里可以呈现已识别的影响版本交付的风险，根据版本需求调查、技术分析和物料状况进行汇总，识别出当前版本风险&障碍列举如下：

| 问题类型 | 问题描述                                | 问题等级 | 应对措施           | 责任人      | 状态 |
| -------- | --------------------------------------- | -------- | ------------------ | ----------- | ---- |
| 物料风险 | 分布式32节点环境还未ready               | 中       | 已采购环境所缺物料 | yansong_lee | open |
| 物料风险 | 缺少2P（Taishan 200 2280 5220）性能环境 | 高       | 求助上层，协调物料 | yansong_lee | open |

# 测试分层策略

| 序号 | 需求                                                         | 开发主体      | 测试主体                             | 验证策略                                                     |
| ---- | ------------------------------------------------------------ | ------------- | ------------------------------------ | ------------------------------------------------------------ |
| 1    | 【openGauss 3.1.1】支持全局逻辑时钟组件GLT，满足分布式事务管理要求 | StorageEngine | QA                                   | 1.验证分布式事务实时一致性<br/>2.验证全局csn能正确单调递增<br/>3.验证opengauss端和ShardingSphere-Proxy端GLT开关功能正确<br/>4.验证GLT组件功能和可靠性 |
| 2    | 【openGauss 3.1.1】对shardingSphere+openLookeng+openGauss统一的安装部署能力 | StorageEngine | QA                                   | 1.根据安装部署指南验证安装部署功能及资料                     |
| 3    | 【openGauss 3.1.1】shardingSphere&openGauss 32节点挑战2600W tpmC | StorageEngine | QA                                   | 1.验证32节点性能能否达到2600W tpmC                           |
| 4    | 【openGauss 3.1.1】openGauss-分布式数据库支持事务下多种模式读写分离 | StorageEngine | QA                                   | 1.不同事务类型（XA、LOCAL）分别在显式、隐式场景下，配置不同读写分离策略，验证读写操作是否正常进行分离路由 |
| 5    | 【openGauss 3.1.1】openGauss分布式方案适配federation以支持跨库查询 | StorageEngine | QA                                   | 1.不同类型表、不同数据类型、不同事务类型场景下，验证不同跨库SQL查询，查询结果正确，功能正常 |
| 6    | 【openGauss 3.1.1】openGauss分布式方案第二阶段15节点1300万性能提升 | StorageEngine | QA                                   | 1.验证15节点的性能是否达到1300万tpmc                         |
| 7    | 【openGauss 3.1.1】DMS适配OCK分布式锁                        | StorageEngine | QA                                   | 1.验证在资源池化场景下，对表空间、数据库、表等非页面及元组对象进行DDL操作加分布式锁，保证以上操作在节点间的协同<br/>2.验证openGauss同时适配DMS分布式锁与OCK分布式锁 |
| 8    | 【openGauss 3.1.1】存储层实现算子卸载，SMP查询性能提升20%    | StorageEngine | QA                                   | 1.验证算子下推是否影响数据一致性 <br/>2.实现算子下推后性能是否有提升并达到要求 |
| 9    | 【openGauss 3.1.1】支持对dbe_perf.file_iostat统计的LRU淘汰算法 | StorageEngine | QA                                   | 1.验证针对dbe_perf.file_iostat统计设计的LRU淘汰算法，满足最近最久不用的条目及时淘汰 |
| 10   | 【openGauss 3.1.1】支持多节点写入                            | StorageEngine | QA                                   | 1. 备机执行DML和DDL写入语句，只需要简单SQL转发到主机执行<br/>2. 针对PBE场景，验证是否成功转发到主机执行<br/>3. 备机执行批量写入，验证是否成功转发到主机执行<br/>4. 备机执行事务，验证是否全部转发给主机执行 |
| 11   | 【openGauss 3.1.1】支持数据库物理读写块大小的配置功能        | StorageEngine | QA                                   | 1.可修改的物理块大小的尺寸标准<br/>2.不同物理块大小的适配对象<br/>3.验证物理块的大小对数据库读取速度的影响 |
| 12   | 【openGauss 3.1.1】postgresql_fdw执行性能提升，支持聚合和条件下推到远端执行功能 | SQLEngine     | QA                                   | 1.验证Join下推路径搜索与生成<br/>2.验证Non-spj算子(仅agg、sort、limit、lockrows) 下推路径搜索与生成<br/>3.验证Fdw 计划生成与执行<br/>4.验证Explain适配，增加支持下推的表达式检查 |
| 13   | 【openGauss 3.1.1】提升特定情况下sort的性能                  | SQLEngine     | QA-Enmotech                          | 1. 验证char、varchar、date、timestamp、int、uuid类型在单列查询的场景下排序的性能有提升<br/>2. 验证符合增量排序触发条件时可以使用增量排序算子执行并有性能提升 |
| 14   | 【openGauss 3.1.1】支持垂直分区                              | SQLEngine     | QA-Vastdata                          |                                                              |
| 15   | 【openGauss 3.1.1】强制访问控制                              | SQLEngine     | QA-Vastdata                          | 1.验证敏感标记的创建<br />2.验证表和表列强制访问控制策略的正确性，测试增删查改的场景<br />3.验证存储过程和触发器强制访问控制策略的正确性，测试调用存储过程和触发器的场景 |
| 16   | 【openGauss 3.1.1】支持CM的部署和数据库部署解耦              | CM            | QA                                   | 1.验证支持在OM集群中部署CM<br/>2.验证支持在CM集群中解耦CM<br/>3.关注CM部署和解耦后数据库能否正常使用 |
| 17   | 【openGauss 3.1.1】OM支持在XML中对同步备机的设置             | OM            | QA                                   | 1.测试ANY、FIRST方式在XML文件中设置同步备可以成功安装并对异常场景进行验证，例如错误的主机名等<br/>2.安装成功后测试同步备是否生效，通过查看系统表、gs_ctl等方式验证<br/>3.测试XML文件中指定同步备安装成功后，可以通过gs_guc参数修改同步备 |
| 18   | 【openGauss 3.1.1】支持openEuler 22.03 LTS系统               | OM            | QA                                   | 1. 验证数据库内核、OM、CM、JDBC/ODBC/python驱动、libpq以及迁移工具在openEuler 22.03 LTS可编译、可运行<br/>2. 进行TPCC性能测试，验证TPCC性能满足180W tpmC |
| 19   | 【openGauss 3.1.1】提升数据校验性能，满足数据校验性能达到70MB/s | Tools         | QA                                   | 1.数据准备，在1~10张表下，递增数据总量，通过迁移工具迁移至M\*后通过校验工具查看全量校验性能是否满足70M/S<br/>2.查看结果文件，测试在满足速率的情况下，数据校验工具校验结果准确 |
| 20   | 【openGauss 3.1.1】数据校验支持行级、列级和全量比较规则      | Tools         | QA                                   | 1.表字段类型支持可配置，针对配置以及未配置的表字段覆盖不同数据类型测试<br/>2.未配置、已配置表字段类型同名区分大小写等常见场景覆盖测试 |
| 21   | 【openGauss 3.1.1】debezium全量迁移支持对M\*对象的迁移       | Tools         | QA                                   | 1.验证是否支持使用Debezium对表、索引、约束、存储过程、函数、触发器、schema的全量迁移<br/>2.验证是否支持对M\*对象的迁移 |
| 22   | 【openGauss 3.1.1】M\*数据迁移完整方案打通                   | Tools         | QA                                   | 1.验证全量迁移、增量迁移、反向迁移、数据校验完整方案的功能<br/>2.关注数据迁移完整方案打通流程和数据的一致性 |
| 23   | 【openGauss 3.1.1】增量、反向迁移支持断点续传                | Tools         | QA                                   | 1.验证增量/反向迁移的断点续传功能<br/>2.关注迁移数据的一致性 |
| 24   | 【openGauss 3.1.1】迁移工具可调试性增强、可靠性增强          | Tools         | QA                                   | 1.验证全量迁移、增量迁移、数据校验、反向迁移支持迁移进度展示（总量、剩余量、剩余时间等）<br/>2.验证校验失败的数据展示是否完整,增量、反向迁移支持迁移失败数据、DDL失败语句展示，全量迁移支持迁移失败对象的展<br/>3.验证资料完整性和可操作性 |
| 25   | 【openGauss 3.1.1】支持一站式迁移平台                        | Tools         | QA                                   | 1.验证系统是否支持一站式迁移平台,对接debezium+kafka工具，并且具有全量迁移、增量迁移、数据校验能力 |
| 26   | 【openGauss 3.1.1】openGauss可视化运维项目                   | Tools         | QA                                   | 1.验证系统是否支持可视化一键式安装部署数据库，并提供运维、管理功能<br/>2.验证其他工具通过预留的集成接口是否可以正常运行 |
| 27   | 【openGauss 3.1.1】openGauss-DataStudio 适配存储过程覆盖率工具开发合作项目 | Tools         | QA                                   | 1.从用户体验方面验证工具菜单命名是否合理，排列是否整齐，功能是否生效。<br/>2.验证调试过程中覆盖语句是否合理<br/>3.验证调试结束覆盖率报告是否完整合理，导出后格式是否正<br/>4.检查资料可操作性和完整性 |
| 28   | 【openGauss 3.1.1】openGauss数据建模项目                     | Tools         | QA                                   | 1.验证系统本身的易用性，能否一键部署<br/>2.验证建模过程中关键点可以算子化，并支持编辑与可视化预<br/>3.验证链接数据库、导入、导出、写入数据库等基础算子支<br/>4.验证产品界面与UI原型图一致 |
| 29   | 【openGauss 3.1.1】openGauss工具一体化平台                   | Tools         | QA                                   | 1.从用户体验方面验证页面布局是否合理，操作是否方便<br/>2.验证基于可视化运维平台集成openGauss的工具链的功能 |
| 30   | 【openGauss 3.1.1】支持CM两节点特性                          | Tools         | QA-Enmotech                          |                                                              |
| 31   | 【openGauss 3.1.1】gstrace工具新增特性，支持数据库运行中追踪 | Tools         | QA-Enmotech                          | 1. 通过解析出来的结果文件验证是否包含应有的模块信息<br/>2. 验证给定非法指令时可以识别并做相应错误处理 |
| 32   | 【openGauss 3.1.1】openGauss周边工具适配DMS&DSS              | StorageEngine | QA                                   | 1.验证在DMS&DSS中opengauss周边工具( gs_probackup、xlogdump、pagehack、gs_install、gs_restore 等)的功能是否正常 |
| 33   | 【openGauss 3.1.1】数据类型兼容                              | Plugin        | QA                                   | 1.验证tinyint数据类型取值范围，精度，对无效值的处理<br/>2.验证类型转换功能是否正常<br/>3.验证类型和函数、操作符之间的交互使用<br/>4.tinyint类型列上创建索引、约束是否正常 |
| 34   | 【openGauss 3.1.1】系统函数兼容                              | Plugin        | QA<br />QA-Enmotech                  | 1.验证时间日期类型函数的取值范围，参数类型，对无效值的处理，返回值超范围的处理<br/>2.验证时间日期类型不同输入格式<br/>3.验证JSON类型函数的取值范围，参数类型，对无效值的处理<br/>4.函数参数个数验证<br/>5.结合其他函数嵌套使用<br/>6.验证any_value()函数在select场景下的功能是否正确<br/>7.验证default()函数能否正确返回对应输入列的默认值<br/>8.验证database()函数能否返回正确结果<br/>9.验证不同场景下row_count()函数返回结果是否正确<br/>10.验证cast()函数对类型转换结果的正确性 |
| 35   | 【openGauss 3.1.1】DDL兼容                                   | Plugin        | QA<br />QA-Enmotech<br />QA-Vastdata | 1.验证加载插件后是否能够对M\*对象进行增删改<br/>2.验证加载插件后查询M\*侧不同类型数据以及表进行查询能否正确显示<br/>3. 验证使用create user if exists语法创建多种不同权限用户后权限是否正确<br/>4. 验证兼容语法文档描述及示例是否正确<br/>5.验证分区表REORGANIZE语法功能，对不支持的分区场景合理报错<br/>6.验证分区表中less than maxvalue语法支持"maxvalue"使用和不使用括号<br/>7.验证在分区表中表达式及函数可用于分区键，在支持列表之外的表达式和函数合理报错<br />8.验证删除触发器时不指定表名是否能级联删除成功<br />9.验证兼容MySQL创建存储过程语法是否正确，创建存储过程能否正常使用<br />10.验证兼容MySQL创建触发器语法是否正确，创建触发器能否正常使用 |
| 36   | 【openGauss 3.1.1】DML兼容                                   | Plugin        | QA<br />QA-Enmotech                  | 1.验证视图的insert、update、delete、truncate操作及与表类型、表约束的交互<br/> 2.验证insert语句的set表达式与算术、逻辑运算符及函数的交互 3.验证prepare语句与select insert、存储过程等语句的结合使用以及与单双引号的结合使用4.验证rename user用户名已存在或非法、alter function结合函数、表类型、存储过程、视图、事务等操作<br/>5.验证DML语句兼容M*语法后的资料描述及示例 |
| 37   | 【openGauss 3.1.1】PL/SQL兼容                                | Plugin        | QA-Vastdata                          | 1.验证新增PL/SQL兼容语法的使用和正确性<br />2.验证新增PL/SQL兼容语法与常量运算、调用函数等的使用<br />3.验证PL/SQL兼容语法与各种查询的使用<br />4.验证使用不同LANGUAGE类型 |
| 38   | 【openGauss 3.1.1】SHOW语法兼容                              | Plugin        | QA<br />QA-Enmotech                  | 1.支持对M\*列类型的映射：验证对数字，时间，字符串等数据类型进行合法入参、非法入参、返回值正确性、边界值等验证；函数嵌套使用验证<br/>2. 兼容SHOW PRIVILEGES语法： 验证显示当前数据库所支持的不同权限，显示权限是否正确<br/>3.兼容SHOW TABLE STATUS语法：查看不同表类型的信息，验证返回信息是否正确 <br/>4.支持show warnings，show errors语法： 验证设置sql_mode参数与不设置，进行非法sql操作后使用show语句查询，提示是否正确 |
| 39   | 【openGauss 3.1.1】其他语法兼容                              | Plugin        | QA<br />QA-Enmotech<br />QA-Vastdata | 1.兼容load data语法：验证每个参数是否合法，验证返回值是否正确<br/>2.支持对M\*部分语法的转换：验证VARCHAR(M)有无national参数功能正常，可以进行增删改查基本操作<br/>3.支持查看字符集和字符序：验证返回值是否为opengauss支持的字符集和字符序<br/>4.双引号表示字符串：校验ansi_quotes参数设置方法，验证双引号原有功能、对元命令、引用字符串常量功能是否正常<br/>5.支持explain和desc保持一致：验证explain/desc 不同的表，结果显示是否正确<br/> 6.指定多个分区查询数据：验证不同分区表/视图，结合聚合函数，数学函数进行验证<br/>7.支持autocommit=0模式：验证不同事物，不同sql语句，在commit/rollback之后是否正确<br/>8.支持!操作符：验证开关设置方式，验证 ! 在不同sql语句中，结合操作符、函数等验证<br/>9.对M\*数据库参数适配：验证参数默认值，修改参数是否生效<br/>10.兼容FLUSH BINARY LOGS语法：验证执行该语句后，pg_xlog归档是否正确<br/>11.兼容M*协议：是否支持M\*5.7不同协议，协议的异常处理是否正确<br/>12.验证反引号在非管理员可创建的opengauss语法中，涉及到名称的情况是否可以正常使用,反引号加入各个语法中可以识别反引号中的名称且不会影响结果的准确性<br />13.验证在函数或存储过程中利用declare声明游标并进行使用<br />14.验证兼容Do expr_list语法的使用 |

# 测试分析设计策略

## 新增feature测试设计策略

| 序号 | Feature                                                      | 重点                                                         | 设计思路                                                     | 备注 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| 1    | 【openGauss 3.1.1】支持全局逻辑时钟组件GLT，满足分布式事务管理要求 | 验证分布式事务实时一致性                                     | 两套一主两备数据库+ShardingSphere+redis的环境上验证GLT功能是否开启， 开启显式事务提交，全局csn能正确推进，验证事务的实时一致性是否满足 |      |
| 2    | 【openGauss 3.1.1】对shardingSphere+openLookeng+openGauss统一的安装部署能力 | 验证安装部署过程是否具备个性化配置、部署工具是否正常且易用   | 根据资料，对不同分片场景进行不同配置部署，整体功能正常可用   |      |
| 3    | 【openGauss 3.1.1】shardingSphere&openGauss 32节点挑战2600W tpmC | 验证性能指标是否达成                                         | 测试分布式32节点性能达到2600W tpmC                           |      |
| 4    | 【openGauss 3.1.1】openGauss-分布式数据库支持事务下多种模式读写分离 | 不同读写分离策略是否可正常配置且生效、结合读写分离场景下可保证事务一致性的前提下性能有所提升 | 1.覆盖不同类型读写分离策略，验证yaml、DistSql配置方式配置功能<br />2.不同事务类型读写分离场景下，验证事务一致性及性能。 |      |
| 5    | 【openGauss 3.1.1】openGauss分布式方案适配federation以支持跨库查询 | 覆盖不同表类型、查询语法、数据类型、事务类型的不同跨库查询场景 | 设计查询SQL，包括跨库表查询、子查询、聚合查询和跨库视图查询等复杂SQL语句、覆盖不同数据类型，在ss-proxy、ss-jdbc场景下进行验证 |      |
| 6    | 【openGauss 3.1.1】openGauss分布式方案第二阶段15节点1300万性能提升 | 验证性能指标是否达成                                         | 测试分布式15节点的性能是否有1300W tpmC                       |      |
| 7    | 【openGauss 3.1.1】DMS适配OCK分布式锁                        | 1.验证资源池化场景下，加分布式锁对表空间、数据库、表等进行DDL操作<br />2.验证openGauss适配DMS与OCK分布式锁是否满足基本功能 | 1.部署openGauss系统集群，验证在资源池化场景加分布式锁，对数据库、表空间、表等进行DDL操作，保证个节点的协同，避免出现在读写节点删除对象时，只读节点还在对该对象进行读操作<br />2.通过配置参数控制，openGauss在适配DMS和OCK分布式锁后对集群已有功能是否影响 |      |
| 8    | 【openGauss 3.1.1】存储层实现算子卸载，SMP查询性能提升20%    | 1.对比同一份数据在没有做算子下推的情况下和算子下推情况下数据是否一致<br />2.SMP查询性能提升20% | 部署一主多备集群，验证算子卸载不影响其他功能的场景，验证SMP查询性能提升20% |      |
| 9    | 【openGauss 3.1.1】支持对dbe_perf.file_iostat统计的LRU淘汰算法 | 验证dbe_perf.file_iostat统计设计LRU淘汰算法，满足最近最久不用的条目及时淘汰 | 主要进行功能测试，结合系统函数，验证dbe_perf.file_iostat统计设计LRU淘汰算法，满足最近最久不用的条目及时淘汰 |      |
| 10   | 【openGauss 3.1.1】支持多节点写入                            | 验证备机支持接收写入语句，转发到主机执行，满足多节点写入     | 一主多备架构下，针对简单写入场景、PBE场景、批量写入场景、事务场景进行测试 |      |
| 11   | 【openGauss 3.1.1】支持数据库物理读写块大小的配置功能        | 验证物理块大小可被修改，修改后可提升数据库的读取速度         | 针对物理块可修改的范围，不同标准尺寸的物理块支持的存储对象以及不同大小物理块的读取速度多场景展开测试 |      |
| 12   | 【openGauss 3.1.1】postgresql_fdw执行性能提升，支持聚合和条件下推到远端执行功能 | Postgres_fdw (postgres foreign data wrap) 功能增强，验证支持join、agg、sort、 limit、lockrows算子的下推执行，实现提升外表查询性能。 | 创建外表，本地使用不同查询算子对外表进行重新查询，要求实现将涉及的计算操作挪到远端数据库进行，增强本地数据库查询性能 |      |
| 13   | 【openGauss 3.1.1】提升特定情况下sort的性能                  | 1. 验证char、varchar、date、timestamp、int、uuid类型在单列查询的场景下排序的性能有提升<br/>2. 验证符合增量排序触发条件时可以使用增量排序算子执行并有性能提升 | 创建包含char、varchar、date、timestamp、int、uuid数据类型的表，加载大量数据，分别记录不同场景下排序算子的执行时间，并与上一版本对比其性能提升 |      |
| 14   | 【openGauss 3.1.1】支持垂直分区                              |                                                              |                                                              |      |
| 15   | 【openGauss 3.1.1】强制访问控制                              | 1.验证敏感标记的创建<br />2.验证表和表列强制访问控制策略的正确性，测试增删查改的场景<br />3.验证存储过程和触发器强制访问控制策略的正确性，测试调用存储过程和触发器的场景 | 主要进行功能测试<br />1.验证敏感标记的创建。<br />2.验证表和表列强制访问控制策略的正确性，测试增删查改的场景。<br />3.验证存储过程和触发器强制访问控制策略的正确性，测试调用存储过程和触发器的场景 |      |
| 16   | 【openGauss 3.1.1】支持CM的部署和数据库部署解耦              | 验证CM部署和数据库部署解耦的功能及资料描述                   | 1.验证在OM集群中部署CM的功能正常，与资料描述一致 <br/>2.验证在CM集群中解耦CM的功能正常，与资料描述一致<br/>3.关注CM部署和解耦后数据库能否正常使用 |      |
| 17   | 【openGauss 3.1.1】OM支持在XML中对同步备机的设置             | 1.指定同步备后可以安装成功<br/>2.安装成功后同步备实例名写入postgresql.conf文件<br/>3.安装成功后可通过gs_guc修改同步备 | 1.测试安装前对XML新增的参数内容及格式合法性的校验及通过ANY/FIRST方式并存逆向场景验证<br/>2.测试安装成功后将主机名转化为实例名写入了postgresql.conf文件<br/>3.测试安装成功后可以通过gs_guc对同步备机进行修改 |      |
| 18   | 【openGauss 3.1.1】支持openEuler 22.03 LTS系统               | 验证数据库内核和工具在openEuler 22.03LTS可编译、可运行，同时TPCC性能满足180W tpmC | 验证数据库内核、OM、CM、JDBC/ODBC/python驱动、libpq以及迁移工具在openEuler 22.03LTS可编译、可运行，同时TPCC性能满足180W tpmC |      |
| 19   | 【openGauss 3.1.1】提升数据校验性能，满足数据校验性能达到70MB/s | 1.设定单表至多表(10个)，表字段20个左右，表字段的内容占字段指定范围的2/3<br/>2.将表数量与数据总量作为变量，分别测试在该变量下的数据校验速度 | 1.数据准备，在1~10张表下，递增数据总量，通过迁移工具迁移至M\*后通过校验工具查看全量校验性能是否满足70M/S<br/>2.查看结果文件，测试在满足速率的情况下，数据校验工具校验结果准确 |      |
| 20   | 【openGauss 3.1.1】数据校验支持行级、列级和全量比较规则      | 验证配置后的表字段的过滤的有效性                             | 1.表字段类型支持可配置，针对已配置以及未配置的表字段覆盖不同数据类型进行校验测试，如果int，varchar等<br/>2.未配置、已配置表字段类型同名区分大小写等常见场景覆盖。如表字段为aa、AA等的校验情况<br/>3.表字段为关键字，配置后可正确进行校验 |      |
| 21   | 【openGauss 3.1.1】debezium全量迁移支持对M\*对象的迁移       | 验证Debezium支持表、索引、约束、存储过程、函数、触发器、schema的全量迁移 | 1.验证是否支持使用Debezium对表、索引、约束、存储过程、函数、触发器、schema的全量迁移<br/>2.验证是否支持对M\*对象的迁移 |      |
| 22   | 【openGauss 3.1.1】M\*数据迁移完整方案打通                   | 验证数据迁移完整打通流程及资料描述                           | 验证全量迁移、增量迁移、反向迁移、数据校验完整方案打通；关注数据迁移完整方案打通流程和数据的一致性 |      |
| 23   | 【openGauss 3.1.1】增量、反向迁移支持断点续传                | 验证迁移工具的断点续传功能及资料描述                         | 验证增量/反向迁移的断点续传功能正常；关注迁移数据的一致性    |      |
| 24   | 【openGauss 3.1.1】迁移工具可调试性增强、可靠性增强          | 验证迁移和反向迁移进度以及报错提示信息的展示                 | 1.验证全量迁移、增量迁移、数据校验、反向迁移支持迁移进度展示（总量、剩余量、剩余时间等）<br/>2.验证校验失败的数据展示是否完整,增量、反向迁移支持迁移失败数据、DDL失败语句展示，全量迁移支持迁移失败对象的展<br/>3.验证资料完整性和可操作性 |      |
| 25   | 【openGauss 3.1.1】支持一站式迁移平台                        | 验证支持一站式迁移平台                                       | 1.是否支持一站式迁移平台<br/>2.是否可以对接debezium+kafka工具<br/>3.是否具有集中全量迁移、增量迁移、数据校验能力 |      |
| 26   | 【openGauss 3.1.1】openGauss可视化运维项目                   | 验证系统支持对openGauss进行安装、配置、运维、升级            | 1.是否一键化安装部署工具<br/>2.是否能以三种方式：CM、OM、gs_ctl可视化安装openGauss（单机和主备），一键化部署、卸载、组件化安装、多版本升级<br/>3.是否支持集群展示和管理，数据库运维参数<br/>4.是否触发WDR的收集，提供快照的设置，并且可以基于两个快照生成报告（html格式）参考orl的em<br/>5.系统是否提供执行SQL<br/>6.是否预留出集成其他工具的接口<br/>7.数据库参数配置是否以guc参数为准 |      |
| 27   | 【openGauss 3.1.1】openGauss-DataStudio 适配存储过程覆盖率工具开发合作项目 | 验证调试功能和调试后的覆盖率结果是否正确以及资料测试         | 1.验证调试过程中被调试语句是否有明显标志<br/>2.验证调试结束后覆盖率结果是否正确，覆盖率导出后格式是否正确<br/>3.验证功能和操作是否与资料描述一致 |      |
| 28   | 【openGauss 3.1.1】openGauss数据建模项目                     | 1.验证数据建模系统的易用性<br/>2.算子的可视化、编辑以及自由搭配<br/>3.产品界面与UI原型图一致 | 1.验证系统本身的易用性，能否一键部署<br/>2.验证建模过程中关键点可以算子化，并支持编辑与可视化预览<br/>3.验证链接数据库、导入、导出、写入数据库等基础算子支持<br/>4.验证产品界面与UI原型图一致 |      |
| 29   | 【openGauss 3.1.1】openGauss工具一体化平台                   | 验证基于可视化运维平台集成openGauss的工具链的功能            | 1.验证基于可视化运维平台集成openGauss工具链如数据建模平台、数据可视化平台，联调M\*数据迁移工具、数据校验工具、O\*数据迁移工具、分布式以及普罗米修斯监控的功能是否支持<br/>2.验证功能和操作是否与资料描述一致 |      |
| 30   | 【openGauss 3.1.1】支持CM两节点特性                          |                                                              |                                                              |      |
| 31   | 【openGauss 3.1.1】gstrace工具新增特性，支持数据库运行中追踪 | 1. 通过解析出来的结果文件验证是否包含应有的模块信息<br/>2. 验证给定非法指令时可以识别并做相应错误处理 | 在数据库执行sql语句时开始追踪，重点验证解析出来的文件有相应的模块信息 ||
| 32   | 【openGauss 3.1.1】openGauss周边工具适配DMS&DSS              | 验证DMS&DSS中工具的功能及资料描述                            | 验证在DMS&DSS中opengauss周边工具( gs_probackup、xlogdump、pagehack、gs_install、gs_restore 等)的功能正常使用 |      |
| 33   | 【openGauss 3.1.1】数据类型兼容                              | 重点关注取值范围、类型转换功能                               | 从取值范围，类型转换，数据类型结合操作符等方面验证           |      |
| 34   | 【openGauss 3.1.1】系统函数兼容                              | 1.重点关注函数取值及对无效值的处理<br/>2.any_value()函数select返回的查询结果是否正确<br/>3.default()函数在数据表中各场景下结果是否正确<br/>4.database()函数在功能上是否与MySQL测对齐<br/>5.row_count()函数在SQL操作后返回的结果是否与MySQL测对齐<br/>6.cast()函数与MySQL测对齐的类型转换返回结果的正确性 | 1.从函数参数个数，参数类型，参数取值等方面验证<br/>2.any_value()函数:验证在group by及其它合法场景返回结果是否与MySQL测对齐，验证参数非法场景的合理报错<br/>3.default()函数:验证在insert、select等场景下default取值是否正确，验证非法场景下的合理报错<br/>4.database()函数:结合use语法验证返回结果是否正确<br/>5.row_count()函数:验证DML、DDL等SQL执行之后，在新SQL中执行row_count()函数返回结果是否正确<br/>6.cast()函数:验证已支持的类型转换及新添加的类型转换结果正确性 |      |
| 35   | 【openGauss 3.1.1】DDL兼容                                   | 1.重点关注加载插件后能否对mysql侧对象进行增、删、改、查等操作<br>2.分区兼容REORGANIZE语法支持的分区类型，重组织之后数据的正确性<br/>3.less than maxvalue语法"maxvalue"支持使用或不适用括号<br/>4.分区键支持表达式、函数，重点关注表达式和函数是否生效，及后续功能是否正确<br />5.重点关注触发器时不指定表名是否能级联删除成功<br />6.重点关注兼容MySQL创建存储过程语法是否正确，创建存储过程能否正常使用<br />7.重点关注兼容MySQL创建触发器语法是否正确，创建触发器能否正常使用 | 1.从语法功能和资料方面验证<br/>2.REORGANIZE语法:验证支持的分区类型可以重组织成功，数据正确，不支持的分区类型合理报错<br/>3.less than maxvalue语法:验证使用或不适用括号语法均可执行通过，功能正确<br/>4.分区键支持表达式、函数:验证支持列表内的表达式和函数功能正确，分区功能正常，支持列表之外的合理报错<br />5.验证触发器时不指定表名是否能级联删除成功<br />6.验证兼容MySQL创建存储过程语法是否正确，创建存储过程能否正常使用<br />7.验证兼容MySQL创建触发器语法是否正确，创建触发器能否正常使用 |      |
| 36   | 【openGauss 3.1.1】DML兼容                                   | 重点关注DML语法是否实现，和已有语法交互使用及资料测试        | 从语法功能和资料方面验证                                     |      |
| 37   | 【openGauss 3.1.1】PL/SQL兼容                                | 重点关注新增PL/SQL兼容语法是否实现，和已有语法交互使用及资料测试 | 1.验证新增PL/SQL兼容语法的使用和正确性<br />2.验证新增PL/SQL兼容语法与常量运算、调用函数等的使用<br />3.验证PL/SQL兼容语法与各种查询的使用<br />4.验证使用不同LANGUAGE类型 |      |
| 38   | 【openGauss 3.1.1】SHOW兼容                                  | 重点关注证show语法功能实现是否正确及查询结果是否正确         | 从用户权限方面验证show语句是否支持查询且查询结果是否正确     |      |
| 39   | 【openGauss 3.1.1】其他语法兼容                              | 1.重点关注语法功能是否正确，和其他语法交互使用是否有影响<br/>2.反引号加入各个语法中，可正确运行且不影响结果<br />3.重点关注在函数或存储过程中利用declare声明游标并进行使用<br />4.重点关注兼容Do expr_list语法的使用 | 1.从语法功能和资料方面验证<br/>2.根据opengauss开发者指南中关于SQL语法部分，对不同语法利用反引号进行测试，整体功能正常运行<br />3.验证在函数或存储过程中利用declare声明游标并进行使用<br />4.验证兼容Do expr_list语法的使用 |      |

## 继承feature/组件测试设计策略

| Domain   | Feature                | 测试策略                                                     |
| -------- | ---------------------- | ------------------------------------------------------------ |
| 接入层   | 应用开发接口           | 继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动             |
|          | 分布式中间件           | 继承已有测试能力，支持应用侧分布式解决方案shardingsphere     |
| SQL层    | 支持SQL语言版本SQL2003 | 继承已有测试能力，支持SQL 2003语言版本                       |
|          | 支持SQL基本语法        | 继承已有测试能力，支持DDL/DML/DCL/DQL语句                    |
|          | 存储过程               | 继承已有测试能力，关注声明语法、基本语句、动态语句、控制语句、事务管理和调试等 |
|          | 系统表和系统视图       | 继承已有测试能力                                             |
|          | DBE_PERF内视图         | 继承已有测试能力，关注OS、Instance、Memory等多个维度划分组织视图，DBE_PERF内视图主要用来诊断性能问题 |
|          | SQL关键字              | 继承已有测试能力，关注保留字和非保留字之分。根据标准，保留字决不能用做其他标识符。非保留字只是在特定的环境里有特殊的含义，而在其他环境里是可以用做标识符的 |
| 存储层   | 索引                   | 继承已有测试能力，支持B Tree索引，Hash索引等                 |
|          | 数据分区               | 继承已有测试能力，支持对分区的组合、交换、拆分、合并等功能；支持范围、列表、哈希等分区类型；支持二级分区 |
|          | 行列混合存储           | 继承已有测试能力，支持行存、列存两种方式，可以在不同的引用场景下选择相应的存储方式 |
|          | 内存表                 | 继承已有测试能力，支持MOT                                    |
|          | 物化视图               | 继承已有测试能力，支持物化视图，物化视图是一种特殊的物理表，物化视图是相对普通视图而言的 |
|          | 全文索引               | 继承已有测试能力，支持单字及词查询，长句子检索；支持中英文混排检索；支持and、or及not的条件组合检索 |
| 管理     | 命令行安装             | 继承已有测试能力，支持OM安装，极简安装和编译安装             |
|          | 版本升级               | 继承已有测试能力，支持数据库软件版本的升级功能，包括就地升级和灰度升级 |
|          | 数据库对象管理         | 继承已有测试能力，支持图形化的方式对模式、表、视图、用户、角色、数据库对象、存储过程、约束、函数、触发器、自增列、大对象等的管理 |
|          | 诊断分析报告           | 继承已有测试能力，支持生成WDR等报告                          |
|          | 智能运维               | 继承已有测试能力，具备智能化运维能力，如智能参数调优、智能索引推荐、慢SQL诊断分析、存储空间智能预测等 |
| 安全     | 身份鉴别               | 继承已有测试能力，支持定义鉴别失败的次数，用户具有唯一标识性 |
|          | 访问控制               | 继承已有测试能力，登录密码存储支持高级别密码存储加密         |
|          | 安全接入               | 继承已有测试能力，具备基于RFC5802机制的口令加密认证方法。加密认证过程中采用单项Hash不可逆加密算法PBKDF2，可防止彩虹攻击 |
|          | 强制访问控制           | 继承已有测试能力，支持强制访问控制：主体粒度达到用户级，客体粒度达到表、视图、触发器等数据库对象级或记录级及列级 |
|          | 安全审计               | 继承已有测试能力，支持安全审计，支持检测安全侵害事件，能将审计数据计入审计日志中 |
|          | 三权分立               | 继承已有测试能力，支持系统管理员、安全管理员和审计管理员角色，三种角色计入审计日志中 |
|          | 数据存储加密           | 继承已有测试能力，支持对用户数据的加密存储或其他有效性的保护措施 |
|          | 全密态数据库           | 继承已有测试能力，支持存储，分析密文数据的数据库系统技术，数据在传输、存储以及计算态均以加密形态存在，整个查询分析过程对用户无感知 |
|          | 动态脱敏               | 继承已有测试能力，支持数据库管理员在数据列上创建动态数据脱敏策略 |
|          | 防篡改功能             | 继承已有测试能力，具备多方共识防篡改、高效防篡改算法、账本数据分布式存储 |
|          | 恶意攻击防范           | 继承已有测试能力，支持system防命令注入，可防止缓冲区溢出；输入有效性检查 |
| 可靠可用 | 物理备份恢复           | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|          | 全量PITR物理恢复       | 继承已有测试能力，全量PITR物理恢复                           |
|          | 逻辑导入导出           | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|          | 延迟备份               | 继承已有测试能力，支持延迟备份                               |
|          | 逻辑复制               | 继承已有测试能力，支持异构数据库间的数据同步                 |
|          | 极致RTO                | 继承已有测试能力，支持快速故障倒换能力，实现RTO<10s          |
| 工具     | 客户端工具             | 继承已有测试能力，支持gsql工具、DS工具                       |
|          | 服务端工具             | 继承已有测试能力，关注gs_cgroupgs_check、gs_checkos、gs_checkperf、gs_collector、gs_dump、gs_dumpall、gs_guc、gs_encrypt、gs_om、gs_plan_simulator、gs_restore、gs_ssh等工具 |
|          | 系统内部使用工具       | 继承已有测试能力，关注gaussdb、gs_backup、gs_basebackup、gs_ctl、gs_expansion、gs_initdb、gs_install、gs_postuninstall、gs_preinstall、gs_dropnode、gs_sshexkey、gs_tar、gs_uninstall、gs_upgradectl、gstrace、pg_recvlogical、gs_probackup 等工具 |
|          | 迁移工具               | 继承已有测试能力，支持全量、增量迁移以及反向迁移             |
|          | 校验工具               | 继承已有测试能力，支持全量、增量数据校验能力                 |
| 兼容     | 数据类型兼容           | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的数据类型 |
|          | 系统函数兼容           | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的系统函数 |
|          | DDL兼容                | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的DDL语法  |
|          | DML兼容                | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的DML语法  |
|          | PL/SQL兼容             | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的PL/SQL语法 |
|          | SHOW语法兼容           | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的SHOW语法 |
|          | 其他语法兼容           | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的其他语法 |

## 专项测试策略

### 安全测试

openGauss社区开源版本需要满足安全基本要求，以达到安全合规，避免安全危机，减少安全事故的目标。openGauss 3.1.1版本的安全测试主要从新特性安全测试、渗透测试和二进制扫描三方面展开，保证openGauss发布版本的安全，提升社区的安全测试能力。

1. 新特性安全测试
2. 渗透测试
3. 二进制测试

| Domain         | 测试策略                                                     |
| -------------- | ------------------------------------------------------------ |
| 新特性安全测试 | 针对交付的新特性进行安全白盒检视，对新特性识别出的安全测试点进行覆盖验收 |
| 渗透测试       | 利用工具进行漏洞扫描、端口扫描、安全编码扫描、敏感信息扫描和Fuzz测试等 |
| 二进制扫描     | 对社区三方件CVE漏洞挖掘，并对新增漏洞确认，修复分支并编译验证；安全编译选项扫描；三方件优选版本扫描；病毒扫描 |

### 可靠性测试

可靠性测试建议覆盖硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具/长时间负载等7个场景，针对7个场景所需要用到的资源有针对性注入关键配置异常、软件故障和硬件故障，关注大并发下的频繁操作以及长时间的负载压力测试。

在模拟业务压力下，采用故障注入工具模拟故障的注入，在观测系统在故障时的反应，集群监控、告警提示、日志输出是否符合预期。在已出现故障的场景下，观测故障恢复后，数据库集群是否可正常运行，数据有无丢失，主备数据是否一致。

在故障注入后检查故障是否影响数据库运行、检查故障后主备数据库状态是否正常、是否能正常处理SQL、主备服务器数据是否一致，若进程异常退出是否有定位信息；故障恢复后数据库服务是否正常工作、SQL语句是否正常执行，主备数据是否一致。

| Domain         | 测试策略                                                     |
| -------------- | ------------------------------------------------------------ |
| 硬件故障       | 注入CPU、内存、网络和磁盘故障，关注数据库的表现，是否有有效提示，故障消除后数据库是否可回复正常，数据有无丢失，主备数据是否一致 |
| 操作系统故障   | 注入系统时间、端口、文件句柄、信号量、文件目录等故障，关注数据库的表现，是否有有效提示，故障消除后数据库是否可回复正常，数据有无丢失，主备数据是否一致 |
| 数据库系统故障 | 模拟双机故障、事务管理、数据库进程异常退出、高并发大数据量、数据库参数调整等场景，观察数据库的表现，消除故障后数据库可恢复正常，数据有无丢失，主备数据是否一致 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库等操作，观察是否有对应日志记录，并且消除故障后数据库可恢复正常，数据有无丢失，主备数据是否一致 |
| RTO            | 开启极致RTO后，构造业务背景，进行数据库停止启动、数据库重启和主备切换等操作，观察数据库是否正常，数据有无丢失，主备数据是否一致 |
| 工具           | 结合备机重建、主备机同步、备份恢复和导入导出等特性，进行硬件/软件故障注入关注数据库表现，是否有有效提示，消除故障后，观察数据库是否正常，数据有无丢失，主备数据是否一致 |
| 长时间负载测试 | 在一主一同步备部署方式下，进行7*24小时的TPC-C稳定性测试，观察系统资源的使用情况，数据有无丢失，主备数据是否一致 |

### 性能测试

性能测试除了关注关键性能指标不劣化外，还需要对新的性能指标进行摸底和测试。利用benckmarksql等工具辅助完成性能测试。

| **指标大项** | **指标小项**                                                 | **指标值** | **说明**                              |
| ------------ | ------------------------------------------------------------ | ---------- | ------------------------------------- |
| TPCC         | 2P（Taishan 200 2280 7260）单节点 1H                         | 150万      | 与Release基线数据差异小于5%以内可接受 |
|              | 2P（Taishan 200 2280 7260）单节点 8H                         | 150万      | 与Release基线数据差异小于5%以内可接受 |
|              | 2P（Taishan 200 2280 7260）一主一备 1H                       | 120万      | 与Release基线数据差异小于5%以内可接受 |
|              | 4P（Taishan 200 2280 7260）单节点 1H                         | 230万      | 与Release基线数据差异小于5%以内可接受 |
|              | 2P（Taishan 200 2280 5220）单节点 1H                         | 100万      | 与Release基线数据差异小于5%以内可接受 |
|              | 分布式OLTP，TPCC线性度0.8，2P（Taishan 200 2280 7260）16节点 1H | 1000万     | 与Release基线数据差异小于5%以内可接受 |
|              | 分布式OLTP，2P（Taishan 200 2280 7260）15节点 1H             | 1300万     | 此即为release基线                     |
|              | 分布式OLTP，2P（Taishan 200 2280 7260）32节点 1H             | 2600万     | 此即为release基线                     |
| RTO          | 一主两同步备failover                                         | 10s        | 与Release基线数据差异小于5%以内可接受 |
| 工具链       | 全量迁移                                                     | 300MB/s    | 此即为release基线                     |
|              | 增量迁移                                                     | 3w tps     | 此即为release基线                     |
|              | 反向迁移                                                     | 1w tps     | 此即为release基线                     |
|              | 全量数据校验                                                 | 70 MB/s    | 此即为release基线                     |

### 兼容性测试

#### 升级兼容

| Domain     | 测试策略                                                     |
| ---------- | ------------------------------------------------------------ |
| 升级路径   | 1.升级路径<br>--支持openGauss 2.0.0升级到openGauss 3.1.1；<br/>--支持openGauss 2.0.1升级到openGauss 3.1.1；<br/>--支持openGauss 3.0.0升级到openGauss 3.1.1<br/>2.升级失败或者升级未提交，可以回滚到原版本<br>3.故障场景下，环境恢复后升级可以重入并升级成功 |
| 升级兼容性 | 关注升级后特性功能运行正常                                   |

#### 硬件兼容

| Domain       | 测试策略                                                     |
| ------------ | ------------------------------------------------------------ |
| 服务器兼容   | 继承已有测试能力，支持在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上部署 |
| 存储设备兼容 | 继承已有测试能力，支持在本地盘、云盘上部署，支持在SAS，SATA，SSD部署 |

#### 软件兼容

| Domain       | 测试策略                                                     |
| ------------ | ------------------------------------------------------------ |
| 支持云化部署 | 继承已有测试能力，支持容器化部署                             |
| 操作系统兼容 | 继承已有测试能力，支持在X86+openEuler 20.03 LTS、X86+CentOS 7.6、ARM+麒麟V10和ARM+openEuler 20.03 LTS操作系统上部署。新增支持在openGauss 22.03 LTS上部署 |

### 资料测试

明确发布资料的测试策略，明确资料中英文测试策略，如下表格样式供参考：

| **手册名称**   | **覆盖策略**                                                 | **中英文测试策略**                                           |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 《发行说明》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《产品描述》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《技术白皮书》 | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《特性描述》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《编译指导书》 | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《快速入门》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《安装指南》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《管理员指南》 | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《开发者指南》 | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |
| 《工具参考》   | 1、可操作性：按照命令描述进行操作。<br>2、文字逻辑：检查其描述的上下文逻辑，有无目标不清，逻辑混乱，信息冗余，导致读者不知所云的问题。<br>3、网页中超链接是否可正常跳转。 | 1、按照资料描述是否可正确操作。<br>2、结合产品实际情况，描述资料的正确性和合理性。<br>3、检查格式问题以及链接跳转是否正常。 |

# 测试执行策略

openGauss 3.1.1版本按照社区release-management团队既定的版本计划，共有9轮测试，按照社区研发模式，所有的需求在2022年11月24日前完成合入。版本计划采取5+4的测试方式，即5轮系统测试+4轮集成验证的策略。

### 测试计划

openGauss 3.1.1版本按照社区开发模式进行运作，结合社区release-management团队指定的版本计划规划相应的测试活动。

| Stage name | Begin time | End time |
| :---- | :---- | :---- |
| Test round 1 | 2022-10-19 | 2022-10-25 |
| Test round 2 | 2022-10-26 | 2022-11-1 |
| Test round 3 | 2022-11-2 | 2022-11-8 |
| Test round 4 | 2022-11-9 | 2022-11-15 |
| Test round 5 | 2022-11-16 | 2022-11-23 |
| Test round 6 | 2022-11-24 | 2022-11-30 |
| Test round 7 | 2022-12-1 | 2022-12-7 |
| Test round 8 | 2022-12-8 | 2022-12-14 |
| Test round 9 | 2022-12-15 | 2022-12-16 |

### 测试重点

#### 测试阶段1：

1. 评审新特性测试设计，重点关注内核场景化、工具链和兼容性新特性的规格验收
2. 通过qa sig组对伙伴同步openGauss 3.1.1测试节奏
3. 继承功能通过自动化看护

#### 测试阶段2：

1. 评审新特性测试设计，重点关注内核场景化、工具链和兼容性新特性的规格验收
3. 回归问题单

#### 测试阶段3：

1. 评审新特性测试设计，重点关注兼容性、内核和分布式解决方案新特性的规格验收
2. 专项测试：性能指标测试，防止指标大幅劣化在集成验收阶段才发现，同时有助于界定性能问题出现的阶段
3. 回归问题单

#### 测试阶段4：

1. 评审新特性测试设计，重点关注兼容性、内核和分布式解决方案新特性的规格验收
2. 回归问题单
3. 专项测试：安全测试，资料测试

#### 测试阶段5：

1. 评审新特性测试设计，重点关注兼容性、内核和分布式解决方案新特性的规格验收
2. 专项测试：兼容性测试（不含升级兼容性测试）
3. 本阶段为集成验证的最后一轮，需要统一审视新特性的规格是否达成

#### 测试阶段6：

1. 进入系统验收阶段，评审新特性测试设计，重点关注兼容性、工具、内核新特性的规格验收
2. 回归问题单
3. 专项测试：可靠性测试（7*24H 负载测试）、资料测试

#### 测试阶段7：

1. 回归问题单
2. 专项测试：可靠性测试（7*24H 负载测试），兼容性测试（升级测试），性能测试，安全测试（二进制扫描），资料测试

#### 测试阶段8：

1. 关注所有交付新特性的验收进展，检查系统集成的完整性和正确性
2. 回归问题单
3. 专项：可靠性测试（7*24H 负载测试），安全测试（CVE漏洞扫描）
4. 启动编写openGauss 3.1.1版本、专项和新特性测试报告

#### 测试阶段9：

1. 关注版本构建，包括数据库的编译安装，社区三方件的编译和其他组件的适配
2. 通过自动化进行版本发布前的验收测试
3. 回归问题单
4. 专项：资料测试
5. openGauss 3.1.1新特性、专项测试报告编写、评审并合入社区。openGauss 3.1.1版本测试报告评审

### 入口标准

1. 所有新需求均按照社区运作方式进入版本
2. 上个测试阶段无阻塞问题遗留
3. 转测版本通过自动化流水线测试，且无阻塞性问题

### 出口标准

1. 测试策略规划的测试活动涉及测试用例100%执行完毕
2. 新特性关键规格达成，继承功能不丢失
3. 可靠性/性能/兼容性等基线数据满足版本规划目标
4. 版本无阻塞性问题遗留，其他问题要有影响分析和相应的规避措施

# 附件

*附件为本测试策略的补充部分*

无
