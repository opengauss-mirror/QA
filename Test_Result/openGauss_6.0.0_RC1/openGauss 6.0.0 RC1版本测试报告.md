![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改章节     | 修改描述     | 作者   |
| --------- | -------- | ------------ | ------------ | ------ |
| 2024.3.6  | 1.0      | 初稿撰写     |              | wan005 |
| 2024.3.29 | 1.1      | 遗留问题列表 | 修改遗留问题 | wan005 |

目 录

1 概述

2 测试版本说明

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论

5 问题单统计

6 附件

7 致谢

**Keywords 关键词**：openGauss 6.0.0 RC1

**Abstract 摘要**：主要是描述了openGauss 6.0.0 RC1版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                    | 中文解释       |
| ------ | --------------------------- | -------------- |
| SQL    | Structured Query Language   | 结构化查询语言 |
| LTS    | Long Time support           | 长时间维护     |
| DML    | Data Manipulation Language  | 数据操纵语言   |
| DDL    | Data Definition Language    | 数据定义语言   |
| DCL    | Data Control Language       | 数据控制语言   |
| CM     | Cluster Management          | 集群管理工具   |
| DMS    | Distributed Memory Service  | 分布式内存服务 |
| DSS    | Distributed Storage Service | 分布式存储服务 |

 

***


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 6.0.0 RC1版本新增内核场景化、工具链和兼容性等特性（具体特性名可以查看"测试版本说明"章节中的需求列表），并对若干关键缺陷进行了修改。本文主要描述了openGauss 6.0.0 RC1版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 6.0.0 RC1版本整体质量良好。

# 测试版本说明

本章节描述测试版本的基本信息，包括测试对象是什么，以及在什么环境下开展的测试，具体包括被测版本和测试硬件环境。

描述被测对象的版本信息和测试的时间及测试轮次。

| 版本名称                          | 测试起始时间 | 测试结束时间 |
| --------------------------------- | ------------ | ------------ |
| openGauss 6.0.0 RC1 Test round 1  | 2023-10-18   | 2023-10-24   |
| openGauss 6.0.0 RC1 Test round 2  | 2023-10-25   | 2023-10-31   |
| openGauss 6.0.0 RC1 Test round 3  | 2023-11-1    | 2023-11-7    |
| openGauss 6.0.0 RC1 Test round 4  | 2023-11-8    | 2023-11-14   |
| openGauss 6.0.0 RC1 Test round 5  | 2023-11-15   | 2023-11-21   |
| openGauss 6.0.0 RC1 Test round 6  | 2023-11-22   | 2023-11-28   |
| openGauss 6.0.0 RC1 Test round 7  | 2023-11-29   | 2023-12-5    |
| openGauss 6.0.0 RC1 Test round 8  | 2023-12-6    | 2023-12-12   |
| openGauss 6.0.0 RC1 Test round 9  | 2023-12-13   | 2023-12-19   |
| openGauss 6.0.0 RC1 Test round 10 | 2023-12-20   | 2023-12-26   |
| openGauss 6.0.0 RC1 Test round 11 | 2024-1-10    | 2024-1-16    |
| openGauss 6.0.0 RC1 Test round 12 | 2024-1-17    | 2024-1-24    |
| openGauss 6.0.0 RC1 Test round 13 | 2024-1-25    | 2024-1-31    |
| openGauss 6.0.0 RC1 Test round 14 | 2024-2-1     | 2024-2-6     |
| openGauss 6.0.0 RC1 Test round 15 | 2024-2-7     | 2024-2-20    |
| openGauss 6.0.0 RC1 Test round 16 | 2024-2-21    | 2024-2-28    |
| openGauss 6.0.0 RC1 Test round 17 | 2024-2-29    | 2024-3-5     |
| openGauss 6.0.0 RC1 Test round 18 | 2024-3-6     | 2024-3-12    |
| openGauss 6.0.0 RC1 Test round 19 | 2024-3-13    | 2024-3-19    |
| openGauss 6.0.0 RC1 Test round 20 | 2024-3-20    | 2024-3-30    |

描述本次测试的测试环境（包括环境软硬件版本信息，环境组网配置信息， 测试辅助工具等）。

| 硬件型号                              | 硬件配置信息                                                 | 备注 |
| ------------------------------------- | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 22.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2480)              | CPU：Kunpeng-920 7260 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| RH2288H V3                            | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4*10GE |      |
| 服务端<br />Taishan 200（Model 2280） | CPU：Kunpeng 920<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 存储侧<br />OceanStore                | Dorado 18500 V6                                              |      |

虚拟化平台

| 虚拟化平台 | 版本说明                                                     |
| ---------- | ------------------------------------------------------------ |
| KVM虚拟化  | KVM+GuestOS（Centos7.6/OpenEuler20.03（LTS）/openEuler22.03（LTS）） |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| Kylin     | V10 SP1          | Kylin V10 SP1，aarch版本ISO<br />SHA256:e6fffdb9dec030920a33ad4c57b68cce8cd7d0496b4bd04a7b85f8cc5494cf41 |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 6.0.0 RC1版本是openGauss社区继续联合多方力量推出的版本，本次发布的需求列表、分工如下：

| 序号 | 需求                                                         | 开发主体      | 测试主体 | 验证策略                                                     |
| ---- | ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 1    | 【openGauss 6.0.0 RC1】【兼容性】MySQL兼容性增强             | Plugin        | QA       | 1、验证RANDOM_BYTES(len)、rand(N)函数兼容<br/>2、验证列支持binary/ASCII属性<br/>3、支持16进制格式的数字输入<br/> |
| 2    | 【openGauss 6.0.0 RC1】【兼容性】walsender                   | Plugin        | QA       | 1、参数验证<br/>2、单机、主备及主备级联下进行验证<br/>3、升级验证 |
| 3    | 【openGauss 6.0.0 RC1】【兼容性】视图解耦测                  | Plugin        | QA       | 1、验证普通视图、临时视图、物化视图、普通表、临时表、分区表、全局临时表相同模式（不同模式）下创建表、视图，修改表字段、名称及约束<br/>2、验证修改当前视图状态为失效状态，将失效视图的表恢复为原状态，并查询视图状态；<br/>3、验证视图依赖一张表或多张表下，视图的不同构建方式 |
| 4    | 【openGauss 6.0.0 RC1】【兼容性】openGauss支持忽略PLSQL间的依赖进行创建 | Plugin        | QA       | 1、验证设置参数behavior_compat_options为plpgsql_dependency，用户可以在修改存储过程依赖的自定义type或表字段类型后，通过对存在依赖的存储过程进行重编译，调用存储过程不报错以及运行结果的准确<br/>（2）验证用户在创建存在互相依赖关系的PLSQL对象时，可以忽略依赖关系的约束进行创建 |
| 5    | 【openGauss 6.0.0 RC1】【兼容性】支持部分关键字转为非保留字  | Plugin        | QA       | 1、验证关键字降级后作为数据库中表、列、索引、角色、序列、表空间、外部数据源、用户、用户组、函数、存储过程、数据库、触发器、视图等对象名称及降级后替换语法 |
| 6    | 【openGauss 6.0.0 RC1】【内核】openGauss内核pg_log日志支持空间轮转 | SQLEngine     | QA       | 1、验证log_max_size参数设置，覆盖设置方式及有效参数值、无效参数值<br/>2、验证pg_log日志空间轮转功能 |
| 7    | 【openGauss 6.0.0 RC1】【内核DFX】pg_prepared_statement视图支持查询数据库会话和实例的prepare statement | SQLEngine     | QA       | 1、验证原函数pg_prepared_statement()功能正常，重载pg_prepared_statement(sessionid)函数传入0、session_id、无效值等场景以及多session创建预备语句<br/>2、验证升级前原函数功能正常，升级后原函数和重载函数功能正常 |
| 8    | 【openGauss 6.0.0 RC1】【内核DFX】打印工具进度条             | SQLEngine     | QA       | 1、验证逻辑备份、恢复，物理备份、恢复，数据库主机对备机的构建以及数据库om升级操作，场景覆盖单机、主备 |
| 9    | 【openGauss 6.0.0 RC1】【资源池化】支持多机并行计划生成，并在多节点间执行 | StorageEngine | QA       | 1.验证SPQPlugin插件使能/关闭场景下的数据库查询功能<br/>2、验证白名单，适合多机执行的查询语句能否正常多机执行，执行结果与原生的一致<br/>3、性能、稳定性、一致性测试 |
| 10   | 【openGauss 6.0.0 RC1】【资源池化】资源池化适配gs_probackup，支持备份恢复 | StorageEngine | QA       | 1、验证全量、增量备份和恢复，备份集的删除、合并等功能；<br/>2、可靠性测试：在备份恢复过程中对DN进程、gs_probackup进程注入故障测试<br/>3、性能测试使用1000仓tpcc数据进行备份恢复，备份恢复时间保持在4-5分钟以内 |
| 11   | 【openGauss 6.0.0 RC1】【内核】支持复杂存储过程中SQL语句的分析特性 | SQLEngine     | QA       | 1、验证系统表pg_stat_activity定位存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数正在执行的sql语句。配置track_stmt_stat_level参数，查询系统表statement_history记录的存储过程的慢sql。配置参数log_min_duration_statement，查询系统表statement_history记录的存储过程的慢sql。执行存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数，查询系统表statement_history记录的慢sql |
| 12   | 【openGauss 6.0.0 RC1】【工具链】DataKit支持兼容性评估(二)   | Tools         | QA       | 1、验证基础功能正常，开展性能测试、可靠性测试                |
| 13   | 【openGauss 6.0.0 RC1】【工具链】MySQL迁移工具适配内核兼容性特性 | Tools         | QA       | 1、验证迁移普通表，表字段类型覆盖整数类型、日期类型、二进制类型、浮点类型，适配最新数据类型映射。<br/>2、验证迁移DDL语句覆盖建表、函数、触发器、视图、存储过程的内核及dolphin插件已兼容的MySQL语法 |
| 14   | 【openGauss 6.0.0 RC1】【工具链】benchmark工厂插件化，支持集成到datakit | Tools         | QA       | 1、验证创建测试任务，查看任务报告，查看编排信息，删除benchmark任务结果，测试各输入框、弹框、可选项等可用性，黑夜模式下各功能以及显示情况，以及易用性等 |
| 15   | 【openGauss 6.0.0 RC1】【工具链】openGauss 5.1.1版本数据校验工具支持对oracle进行校验特性 | Tools         | QA       | 1、验证oracle数据库中的数据分别进行全量迁移、增量迁移以及反向迁移至opengauss数据库后，两库间的数据全量、增量校验 |
| 16   | 【openGauss 6.0.0 RC1】【工具链】timescaledb适配openGauss与性能优化第一阶段 | Tools         | QA       | 1、 验证TimescaleDB一阶段已实现的11个接口是否连接通          |
| 17   | 【openGauss 6.0.0 RC1】【工具链】【DataKit】业务开发平台     | Tools         | QA       | 1、验证数据库管理设置自动断连时间、超时自动重连，连接增加是否保存口令、加密算法改成AES256CBC(32位)、编辑用户/角色、修改密码、外表管理、创建/删除外部服务器、创建/删除触发器、重命名触发器、创建/编辑/删除表空间、复制数据库对象、sql终端支持导出选中SQL语句、查看视图数据支持分页、IntarkDB替换SQLite，以及继承功能<br/>2、验证相关接口性能 |
| 18   | 【openGauss 6.0.0 RC1】【工具链】【DataKit】智能运维平台测试报告（SQLite版） | Tools         | QA       | 1、验证SQL诊断、实例监控、历史数据诊断、告警通知、集群监控功能<br/>2、验证实例监控、智能诊断、集群监控、告警监控继承接口 |
| 19   | 【openGauss 6.0.0 RC1】【工具链】【迁移工具】portal配置优化和【DataKit】MySQL迁移过程增加预操作和切换前检查 | Tools         | QA       | 1、验证portal安装后修改全量迁移参数配置文件、增量迁移参数配置文件配置文件、反向迁移参数配置文件、portal统一参数配置文件后等级迁移、迁移后参数配置生效，修改后的参数配置文件是否保留等功能<br/>2、验证服务可用性检查、数据库连接检查、数据库权限检查、大小写参数配置检查、磁盘空间检查和切换迁移前检查以及迁移前是否选择开启调整内核参数等功能 |
| 20   | 【openGauss 6.0.0 RC1】【工具链】反向迁移工具能力增强特性    | Tools         | QA       | 1、验证不同数据类型映射的数据，是否迁移成功且数据一致<br/>2、验证配置迁移source端与sink端新增参数生效。配置MySQL与openGauss的超时参数，启动迁移并长时间无操作，之后再插入数据查看是否正常迁移<br/>3、在高性能机器下，开启反向迁移，通过sysbench工具给openGauss压测数据，待数据回放完成，计算迁移效率<br/>4、可靠性测试覆盖迁移中，在sink端制造异常导致迁移终止，再次启动迁移后是否从中断处正常迁移，数据是否完整，已迁移的数据是否跳过，迁移的数据不重复 |
| 21   | 【openGauss 6.0.0 RC1】【迁移工具】增量迁移工具能力增强测试  | Tools         | QA       | 1、验证source端和sink端参数有效性，手动kill sink端和source端进程再恢复后数据是否能完成迁移且不重复，以及增量迁移后数据类型兼容性<br/>2、在高性能机器下，设置按表回放，开启增量迁移，通过sysbench工具压测数据，待数据回放完成，计算迁移效率 |
| 22   | 【openGauss 6.0.0 RC1】【工具链】可视化运维特性              | Tools         | QA       | 1、验证集群管理、一键安装、集群安装、批量升级、集群监控、日志分析、WDR报告、SQL诊断、安装包管理、自定义控制台、备份恢复模块<br/>2、兼容性测试验证，在多个浏览器上测试<br/>3、易用性测试，覆盖检查web页面的布局、UI设计是否合理，操作顺序是否符合逻辑 |
| 23   | 【openGauss 6.0.0 RC1】【工具链】支持O_全量迁移              | Tools         | QA       | 1、验证已支持的O*常用数据类型、表类型、索引、约束、序列、视图、表空间、同义词等对象迁移 |
| 24   | 【openGauss 6.0.0 RC1】【工具链】支持O_反向迁移              | Tools         | QA       | 1、验证已支持的O*常用数据类型、表类型等测试，以及不同的dml语法对于反向迁移的影响 |
| 25   | 【openGauss 6.0.0 RC1】【工具链】支持O_增量迁移              | Tools         | QA       | 1、验证已支持的O*常用数据类型、表类型以及不同的dml、ddl语法对于增量迁移的影响 |
| 26   | 【openGauss 6.0.0 RC1】【工具链】数据库向导式一站式安装工具  | Tools         | QA       | 1、验证一键式安装传统单机/主备环境、一键式安装资源池化环境，以及普通用户一键式安装在 |
| 27   | 【openGauss 6.0.0 RC1】【工具链】数据库安装流程解除对root用户的依赖 | Tools         | QA       | 1、验证omm用户（非root用户）进行预安装、安装不带cm和带cm的数据库，使用omm用户执行gs_sshexkey、gs_checkos、gs_upgradectl、gs_expansion等功能 |
| 28   | 【openGauss 6.0.0 RC1】【工具链】数据校验能力增强            | Tools         | QA       | 1、验证Mysql与openGauss分片数不相同的场景，openGauss表对象缺失、新增场景，待校验的表的数据条数小于、等于以及大于1个分片的数据量的场景<br/>2、开展性能、可靠性测试 |
| 29   | 【openGauss 6.0.0 RC1】【内核】允许修改序列                  | SQLEngine     | QA       | 1、验证序列步长、是否循环、最大值、最小值、起始值、当前值、缓存值和属主的修改 |
| 30   | 【openGauss 6.0.0 RC1】【内核】切换后台日志为中文日志        | SQLEngine     | QA       | 1、验证参数enable_nls设置为on/off，查看日志信息为中文或英文<br/>2、设置参数log_min_messages=log，查看日志信息为中文或英文 |
| 31   | 【openGauss 6.0.0 RC1】【资源池化】gs_collector适配资源池化  | StorageEngine | QA       | 1、验证gs_collector的采集基础功能，以及指定配置文件采集指定信息，资源池化特定采集配置 |
| 32   | 【资源池化】CM pause支持资源池化                             | StorageEngine | QA       | 1、验证cm_ctl pause暂停集群，CM对DN的自动仲裁和故障处理功能暂停，恢复后CM的功能正常可用<br/>2、验证CM集群暂停后，磁盘、网络、集群双主等异常时，CM的功能可用 |
| 33   | 【资源池化】主备双集群支持通过网络进行主备日志同步           | StorageEngine | QA       | 1、验证使用gs_sdr工具在不同操作系统搭建主备双集群，备集群的首备和主集群的主节点使用流式复制方式同步日志<br/>2、验证主备集群间和主、备集群内的节点切换正常，构造单节点故障、整集群故障 |
| 34   | 【内核】支持异步备升主时旧主数据找回功能                     | SQLEngine     | QA       | 1、验证gs_retrieve工具参数校验、gs_retrieve工具功能、pg_create_logical_replication_slot函数功能 |
| 35   | 【内核】新增MySQL协议hot_standby模式                         | Connectors    | QA       | 1、验证dolphin_hot_standby参数的三种设置方式及参数生效后的结果<br/>2、验证hot_standby、dolphin_hot_standby参数设置为on/off，JDBC、gsql客户端连接openGauss、mysql的备机连接状况符合预期 |
| 36   | 【openGauss 6.0.0 RC1】【工具链】portal执行流程优化（一）    | Tools         | QA       | 1、验证Datakit启动portal功能，进行在线、离线迁移，对迁移后数据进行校验<br/>2、验证命令行启动portal，对mysql全量迁移工具、增量迁移工具、反向迁移工具进行在线安装、离线安装、卸载、启动测试 |
| 37   | 【openGauss 6.0.0 RC1】【工具链】datakit工具支持资源池化双集群部署 | Tools         | QA       | 1、验证双击群安装功能正常，磁阵创建、修改、删除功能正常使用，容灾集群安装、集群管理、主备切换、集群状态显示、解除容灾功能正常<br/>2、易用性验证，测试执行流程的逻辑合理性，UI界面显示正常、合理 |
| 38   | 【openGauss 6.0.0 RC1】【工具链】扩容工具易用性提升          | Tools         | QA       | 1、 验证去除配置xml操作扩容，支持子用户进行扩容<br/>2、验证root、omm用户下的om、cm集群单机扩容、主备扩容 |
| 39   | 【openGauss 6.0.0 RC1】【兼容性】B兼容性下字段模糊匹配支持走索引扫描 | Plugin        | QA       | 1、验证char、varchar、text类型在utf8、gbk、gb18030编码及字符序下右模糊匹配支持走索引验证<br/>2、验证schema、table、column字符集及字符序优先级 |
| 40   | 【openGauss 6.0.0 RC1】【资源池化】索引创建支持预扩展，堆表支持预读 | StorageEngine | QA       | 1、验证参数heap_bulk_read_size、heap_bulk_read_size取值和设置方式验证<br/>2、开启预读，线性扫描不同类型的表、视图，执行结果与不开启预读的一致，vacuum执行成功<br/>3、对比开启和关闭预读场景下的堆表线性扫描时间和vacuum时间，预期开启预读性能提升15%及以上 |
| 41   | 【openGauss 6.0.0 RC1】【工具链】DataKit支持兼容性评估（三） | Tools         | QA       | 1、验证DataKit采集任务管理、评估工具、查看评估结果模块功能正常，参数输入、输出符合预期<br/>2、验证工具可维护性、易用性，UI界面操作流程合乎逻辑，页面布局合理 |
| 42   | 【openGauss 6.0.0 RC1】【迁移工具】MySQL数据迁移完整方案打通（校验工具） | Tools         | QA       | 1、验证M*数据迁移完整方案打通，测试验证数据类型，表对象、数据异常场景(新增、缺失、更改)<br/>2、验证原有的过滤规则 |
| 43   | 【openGauss 6.0.0 RC1】【资源池化】在线reform增强            | StorageEngine | QA       | 1、验证一个集群，增加、移除、重启备机，集群中的主机和其他备机的业务不中断<br/>2、验证等待事件相关视图、相关特性 |
| 44   | 【openGauss 6.0.0 RC1】【内核】分区表性能优化                | SQLEngine     | QA       | 1、验证tpcc导入时间缩短、并发性能提升<br/>2、验证分区表执行查询、插入、指定分区插入数据等操作性能提升 |
| 45   | 【openGauss 6.0.0 RC1】【资源池化】新增DMS、CBB等待时间统计  | StorageEngine | QA       | 1、验证功能场景、特性耦合，故障场景                          |
| 46   | 【openGauss 6.0.0 RC1】【资源池化】支持在线reform            | StorageEngine | QA       | 1、验证一个集群，增加、移除、重启备机，集群中的主机和其他备机的业务不中断<br/>2、验证等待事件相关视图、相关特性 |
| 47   | 【openGauss 6.0.0 RC1】【资源池化】XLog按需回放性能优化      | StorageEngine | QA       | 1、验证在主机故障后，保证failover成功，以及按需回放功能正常<br/>2、一主一备环境部署，验证主机在100并发、TPCC负载50WtpmC、RTO<10秒<br/>3、验证参数设置，参数赋予正确值错误值情况下特性功能正常 |
| 48   | 【openGauss 6.0.0 RC1】【资源池化】CBB MES支持线程池         | StorageEngine | QA       | 1、验证功能场景、特性耦合，故障场景                          |

# 版本概要测试结论

openGauss 6.0.0 RC1版本整体测试按照release-manager团队的计划，版本测试规划采取16+3+1的测试方式，即16轮系统测试+3轮集成验证+1轮回归测试的策略，实际完成了16轮系统测试+3轮集成验证+1轮回归测试：

阶段一：

第一轮至第五轮进行迭代1的需求验收及问题单验收，主要涉及工具链的新特性验收，同时对继承测试开展第一阶段第一轮的全量CI测试，覆盖可靠性、安全、性能、长稳、升级等测试。第六轮至第九轮集中评审迭代2新特性测试设计，并对内核、兼容性、工具链三个方面的新特性验收，同时开展第一阶段第一轮的集成测试，对继承特性进行第二轮自动化看护。第十轮集中验收第一阶段修复完成的问题单，并对每个特性的测试报告进行了评审。

阶段二：

第十一轮至第十六轮集中评审阶段二新特性测试设计，同时关注内核场景化、工具链和资源池化三个方面的新特性验收，并对关键性能数据进行了摸底，旨在发现阻塞性问题，而且对继承特性进行了自动化看护。关注交付特性的验收进展，对每个特性的测试报告进行评审。第十七轮至第十八轮开始第二阶段的第一轮集成测试，展开性能、安全、可靠性、升级、长稳、资料等专项测试，意在检查系统集成的完整性和正确性。此外，针对特性的测试报告测试人员也开始撰写。第十九轮开始第二阶段的第二轮集测试，开展每日全量自动化测试，再次覆盖一轮7*24H长稳测试，保证社区版本基础功能正常、稳定性良好。第二十轮转测后主要集中问题单回归与版本发布准备，并例行展开自动化测试，对关键指标、功能进行看护，防止修改引入新的问题。

openGauss 6.0.0 RC1版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本计划交付需求48个，实际交付48个，交付率100%，所有发布需求均验证通过。

openGauss 6.0.0 RC1版本共发现问题1344个，有效问题1143个。修复问题回归测试结果正常，版本整体质量良好。遗留18个问题，见附件1遗留问题列表。

# 版本详细测试结论

openGauss 6.0.0 RC1版本详细测试内容包括：

1、通过自动化看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、系统性能、系统可靠性6个维度进行openGauss继承特性测试，继承功能无丢失；

2、在内核场景化的竞争力构建上，主要是对资源池化场景进行增强，如XLog按需回放性能优化、在线reform增强、主备双集群支持通过网络进行主备日志同步等......测试覆盖上述需求，关注功能的实现和规格的达成。但后续仍需从资源池化可靠性、竞争力的角度出发，持续进行加固测试和关键指标验证；

3、在工具链方面，覆盖Datakit继承特性、新特性测试，对portal执行流程进行优化、扩容工具易用性提升，同时Datakit工具支持了资源池化双集群部署。后续仍需对工具的易用性和稳定性提高要求。

4、在M\*兼容性方面，测试新增B兼容下字段模糊匹配支持走索引扫描需求，M\*兼容性进一步提升；

5、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试和sqlsmith测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

6、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 特性测试结论

### 继承特性评价

对产品所有继承特性进行评价，用表格形式评价，包括特性列表（与特性清单保持一致），验证质量评估

| Domain           | Feature            | 质量评估                   | 备注                                                         |
| ---------------- | ------------------ | -------------------------- | ------------------------------------------------------------ |
| DataKit          | 插件管理           | <font color=green>▮</font> | 测试插件管理模块，关注该模块下各子特性功能正常及界面显示正常 |
|                  | 安全中心           | <font color=green>▮</font> | 测试安全中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 日志中心           | <font color=green>▮</font> | 测试日志中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 资源管理           | <font color=green>▮</font> | 测试资源管理模块，验证资源管理功能，界面显示正常             |
|                  | 安装部署           | <font color=green>▮</font> | 测试安装部署模块，验证安装部署流程正常，界面显示正常         |
|                  | 业务建模           | <font color=green>▮</font> | 测试业务建模模块，关注各子特性功能正常及界面显示正常         |
|                  | 基础运维           | <font color=green>▮</font> | 测试基础运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 智能运维           | <font color=green>▮</font> | 测试智能运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 告警监控           | <font color=green>▮</font> | 测试告警监控模块，关注各子特性功能正常及界面显示正常         |
|                  | 数据迁移           | <font color=green>▮</font> | 测试迁移功能，迁移前后数据一致性校验                         |
|                  | 监控插件工具       | <font color=green>▮</font> | 测试监控插件功能，界面显示正常                               |
|                  | 业务开发           | <font color=green>▮</font> | 测试业务开发模块，关注各子特性功能正常及界面显示正常         |
|                  | 兼容性评估         | <font color=green>▮</font> | 测试兼容性评估模块，关注各子特性功能正常及界面显示正常       |
|                  | 录制回放           | <font color=green>▮</font> | 测试录制回放模块，关注各子特性功能正常及界面显示正常         |
|                  | datakit_安装部署   | <font color=green>▮</font> | 测试安装部署、启停、兼容性                                   |
|                  | datakit_系统可靠性 | <font color=green>▮</font> | 测试系统级长稳、性能、可靠性、内存                           |
|                  | datakit_升级       | <font color=green>▮</font> | 测试平台与含插件升级                                         |
| 数据库服务       | SQL语法            | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句，不同特性组合下用户、权限的验证(含资源池化、兼容性)(表/视图/索引等基础对象，fdw、postgis、物化视图) (含资源池化、兼容性) |
|                  | 功能SQL            | <font color=green>▮</font> | 继承已有测试能力，vacuum、analyze、explain、事务(含自治事务)、审计、安全&加密、AI特性、密态等值查询、账本数据库、逻辑复制 |
|                  | 主备管理           | <font color=green>▮</font> | 继承已有测试能力，极致RTO、switchover、failover等            |
|                  | guc参数控制        | <font color=green>▮</font> | 继承已有测试能力，不同参数影响sql的执行效果，应该放到各个sql模块云设计；这里仅验证参数生效和组合场景 |
|                  | 内核工具链         | <font color=green>▮</font> | 继承已有测试能力，gs_ctl/gstrace/perctrl/pg_config/pagehack/pg_recvlogic/pg_controldata/pg_xlogdump/pg_resetxlog/gs_restore等 |
|                  | 资源负载管理       | <font color=green>▮</font> | 继承已有测试能力，gs_cgroup验证                              |
| 数据库备份恢复   | 物理备份/恢复      | <font color=green>▮</font> | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|                  | 逻辑备份/恢复      | <font color=green>▮</font> | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|                  | PITR、日志归档     | <font color=green>▮</font> | 继承已有测试能力，全量PITR物理恢复                           |
|                  | 延时备份           | <font color=green>▮</font> | 继承已有测试能力，支持延迟备份                               |
| 数据库管理与运维 | 安装卸载           | <font color=green>▮</font> | 继承已有测试能力，测试数据库安装、卸载全流程                 |
|                  | 升级               | <font color=green>▮</font> | 测试带业务操作下多升级路径覆盖，升级成功后，特性功能运行正常 |
|                  | 实例管理           | <font color=green>▮</font> | 测试主备高可用(switchover/failover)、重启、启停              |
|                  | 运维视图           | <font color=green>▮</font> | 测试系统表与系统视图、系统schema(如dbe_perf、information_schema、WDR、pldebugger、db4ai等) |
| 数据库兼容性     | 环境兼容           | <font color=green>▮</font> | 测试硬件兼容、操作系统兼容、依赖软件版本                     |
|                  | 驱动兼容           | <font color=green>▮</font> | 测试jdbc/odbc/libpq/psycopg2等 mysql兼容(协议兼容、类型兼容)继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动 |
|                  | 生态兼容           | <font color=green>▮</font> | 测试ORM(mybatis)，连接池(druid)                              |
|                  | mysql兼容性        | <font color=green>▮</font> | 测试SQL语法（单双引号、反引号、关键字、类型、函数、操作符等）、通信协议 |
|                  | oracle兼容性       | /                          | /                                                            |
|                  | pg兼容性           | /                          | /                                                            |
|                  | 资源池化兼容性     | <font color=green>▮</font> | 测试资源池化特性，双集群极致RTO、网络复制双集群、refrom、主备实时一致性 |
| 系统性能         | 系统性能           | <font color=green>▮</font> | 测试2P/4P性能、主备、RTO(含兼容性、资源池化)                 |
| 系统可靠性       | 系统可靠性         | <font color=green>▮</font> | 故障注入测试，包含MOT、兼容性、资源池化等内存专项、长稳、    |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估，给出特性质量评估结论。

| 特性名称                                                     | 测试情况说明                                                 | 约束                                                         | 质量电灯                    | 遗留问题 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | -------- |
| 【openGauss 6.0.0 RC1】【兼容性】MySQL兼容性增强             | MySQL兼容性增强特性，共计执行22个用例，主要覆盖功能测试和资料测试，通过手动执行用例，未发现功能问题，资料测试发现问题已解决，回归通过，无遗留风险，整体质量良好； | 特性使用时涉及到的约束及限制条件。RANDOM_BYTES函数入参取值范围为[1,1024]，超范围直接报错，入参为NULL时返回NULL。RAND函数返回值范围为[0，1)。参数取值范围为[-2^63,2^64-1]。入参为NULL时等价于入参为0。BINARY/ASCII属性实现，ASCII属性实际为“character set latin1”，但当前实际上指定列的字符集为“latin1”会报错（原始设计如此），本需求不处理此问题，所以如果给列赋ASCII属性，虽然语法能通过，但实际会报错。支持16进制输入，不同于MySQL。当数字个数为奇数时，不会自动补0。当前openGauss的XCONST表现与MySQL的0x不完全一致，需要通过SQL_MODE控制（其他同事开发，具体表现不一致不在本需求覆盖）。需要注意，MySQL侧不支持0x、0x123G、0xG123这种非法输入，但是在openGauss侧，由于在词法分析时，两个词可以不用空格分隔，所以对于上述三种特殊场景，openGauss实际会识别成select 0 x; select 0x123 G;select 0 xG123;而不会直接报错 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【兼容性】walsender                   | 累计执行42个用例，主要覆盖了功能测试、升级测试、资料测试。功能测试覆盖了单机下进行验证(参数验证 开关开启/重置/关闭下验证 数据库重启后进行验证)、主备下进行验证(在主机下进行验证 在备机下进行验证 主机变成备机后在主机以及备机下分别验证 关闭备机后进行验证 关闭主机后进行验证)、主备级联下进行验证。测试中发现0个问题。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【兼容性】视图解耦测                  | dolphin插件以及兼容A库下实现视图解耦功能，共计执行80个用例，主要覆盖了功能测试、约束测试、资料测试。功能测试主要包括普通视图、临时视图、物化视图、普通表、临时表、分区表、全局临时表相同模式下创建或不同模式下创建表以及视图，修改表字段、名称以及约束可以成功修改并且当前视图状态为失效状态；将失效视图的表恢复为以前状态，查询视图状态；视图依赖一张表或多张表下进行验证；视图的不同构建方式。测试中发现1个问题，已回归通过。 | 需在openGauss兼容B库下安装dolphin插件验证                    | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【兼容性】openGauss支持忽略PLSQL间的依赖进行创建 | openGauss支持忽略PLSQL间的依赖进行创建，共计执行50条用例，主要覆盖了功能测试和资料测试。功能测试覆盖设置behavior_compat_options='plpgsql_dependency'及behavior_compat_options=''下测试该需求。资料测试验证资料描述及示例是否正确。累计发现缺陷单3个，缺陷已解决，回归通过，无遗留风险，整体质量良好。 | openGauss的A兼容模式下使用。本需求实现需设置guc参数behavior_compat_options值为'plpgsql_dependency'，默认不设置该参数。rename类型和rename类型的属性，若存在依赖时，不允许操作 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【兼容性】支持部分关键字转为非保留字  | 关键字降级特性测试中，共计执行用例454个，主要涉及关键字降级后作为数据库中表、列、索引、角色、序列、表空间、外部数据源、用户、用户组、函数、存储过程、数据库、触发器、视图等对象名称及降级后替换语法进行测试，未发现问题，无遗留风险，整体质量良好。 | 本次关键字降级作用域为兼容B库，需要在兼容b库下执行           | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核】openGauss内核pg_log日志支持空间轮转 | openGauss内核pg_log日志支持空间轮转特性，共设计22个测试用例，主要包含log_max_size参数设置测试、pg_log日志空间轮转功能验证，及与其他业务交互场景测试等。发现2个问题，一个非问题已取消，另一个已修复并验收通过，整体质量良好。 | log_max_size当前仅对pg_log日志生效                           | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核DFX】pg_prepared_statement视图支持查询数据库会话和实例的prepare statement | pg_prepared_statement视图支持查询数据库会话和实例的prepare statement特性，共计执行54个用例，主要覆盖了功能测试、兼容性测试、性能测试、资料测试，发现issue5个，已验收通过，整体质量良好； | 只有超级用户和monitor admin才可访问函数pg_prepared_statement(sessionid) | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核DFX】打印工具进度条             | 内核工具打印进度条特性，共执行用例18个，主要覆盖了功能测试、资料测试。功能测试覆盖逻辑备份、恢复，物理备份、恢复，数据库主机对备机的构建以及数据库om升级操作，场景覆盖单机、主备。资料测试覆盖工具相关资料是否准确、完整。累计发现缺陷单1个，已回归通过，整体质量良好。 | GUC参数、操作约束集成内核备份、恢复、构建、升级工具，无新增约束。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】支持多机并行计划生成，并在多节点间执行 | SPQ多机并行执行特性，共计执行78个用例，主要覆盖了功能测试、一致性测试，稳定性、性能测试；发现问题21个，已全部修复并回归通过，整体质量一般 。 | 本特性仅支持在资源池化部署下使用。暂时仅支持shared_preload_libraries方式加载插件。加载SPQ的节点所配置的节点名称pgxc_node_name互不相同。开启spq插件功能后，QE节点pg_hba.conf中配置QC节点trust方式的白名单，否则非初始用户查询时报错。分区表、系统表、系统视图、伪列、自定义函数、存储过程的查询暂不支持，走原生流程。事务中查询走原生处理流程。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】资源池化适配gs_probackup，支持备份恢复 | 资源池化适配gs_probackup，支持备份恢复特性，共执行83个用例，主要覆盖了功能测试、可靠性测试、长稳测试、性能测试以及文档验证。功能测试覆盖：全量、增量备份和恢复，备份集的删除、合并以及验证等功能；可靠性测试覆盖：在备份恢复过程中对DN进程、gs_probackup进程注入故障测试；长稳测试在长稳流水线中增加备份任务；性能测试使用1000仓tpcc数据进行备份恢复，备份恢复时间保持在4-5分钟以内。测试过程中累计发现4个问题，特性质量良好。 | 需在资源池化模式下使用，在配置文件中指定enable_dss参数并配置磁阵环境。增量备份需设置参数enable_cbm_tracking=on。需在进行增量备份、备份合并、增量恢复之前需要有完整的全量备份，且两者压缩算法一致。其余约束见gs_probackup文档。 | <font color=yellow>▮</font> | 无       |
| 【openGauss 6.0.0 RC1】【内核】支持复杂存储过程中SQL语句的分析特性 | 支持复杂存储过程中SQL语句的分析特性，共执行用例47个，主要覆盖了功能测试、资料测试。功能测试覆盖查询系统表pg_stat_activity定位存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数正在执行的sql语句。配置track_stmt_stat_level参数，查询系统表statement_history记录的存储过程的慢sql。配置参数log_min_duration_statement，查询系统表statement_history记录的存储过程的慢sql。执行存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数，查询系统表statement_history记录的慢sql。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单1个，1个缺陷均已解决且回归通过，整体质量良好。 | 需设置参数track_activities=on，enable_resource_track=on，instr_unique_sql_track_type=all | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】DataKit支持兼容性评估(二)   | DataKit支持兼容性评估，实现抽取jdbc执行的SQL语句及堆栈信息并输出到文本文件中。累计发现缺陷单4个，已完成4个，整体质量良好。 | jdk版本11.0.20以上。mysql数据库版本8.0.x,5.0.x。scheduled的startTime必须设置在当前时间之后，否则会无法启动。conf文件下的yml信息,linux的config信息,默认用户为root用户,该用户需要有创建directory,上传文件的权限,运行启动命令的权限,否则会导致失败。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】MySQL迁移工具适配内核兼容性特性 | MySQL迁移工具适配内核兼容性，共执行用例39个，主要覆盖了功能测试、资料测试。功能测试覆盖迁移普通表，表字段类型覆盖整数类型、日期类型、二进制类型、浮点类型，适配最新数据类型映射。迁移DDL语句覆盖建表、函数、触发器、视图、存储过程的内核及dolphin插件已兼容的MySQL语法。覆盖配置参数lower_case_table_names迁移大小写名称的对象。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单2个，2个缺陷均已解决且回归通过，整体质量良好。 | MySQL5.7及以上版本。MySQL参数设置：log_bin=on、binlog_format=row、binlong_row_image=full、gtid_mode=on。迁移工具目标端openGauss的数据库应设置为使用dolphin插件的b库，b库下建议打开的参数：dolphin.b_compatibility_mode=on、enable_set_variable_b_format=on。为保证大小写一致性，迁移时需保证MySQL的大小写参数lower_case_table_names和openGauss的大小写参数dolphin.lower_case_table_names保持一致。sql_mode设置openGauss与MySQL保持一致。对象的在线DDL操作，DDL语句是直接透传到openGauss端的，对于MySQL与openGauss不兼容的语法将报错。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】benchmark工厂插件化，支持集成到datakit | benchmark工厂插件化，支持集成到DataKit，共执行用例10个，主要覆盖了功能测试、及资料测试，功能测试主要覆盖创建测试任务，查看任务报告，查看编排信息，删除benchmark任务结果，测试各输入框、弹框、可选项等可用性，黑夜模式下各功能以及显示情况，以及易用性等。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单5个，5个缺陷已解决，回归通过，整体质量良好。 | DataKit环境已搭建                                            | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】openGauss 5.1.1版本数据校验工具支持对oracle进行校验特性 | gs_datacheck数据校验工具支持对oracle进行检验特性，共计执行26条用例，主要覆盖了功能测试和可靠性测试。功能测试覆盖范围包括oracle数据库中的数据分别进行全量迁移、增量迁移以及反向迁移至opengauss数据库后，对两库间的数据进行全量校验和增量校验。可靠性测试主要场景为迁移后的数据不同，或人为进行数据构造执行校验，验证校验结果的可靠和准确性。累计发现缺陷单4个，4个缺陷皆已解决，整体质量良好。 | 校验的表必须包含主键。校验内容差距过大（200行及以上），校验后不会出修复报告，此时建议重新迁移或检查文件配置。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】timescaledb适配openGauss与性能优化第一阶段 | timescaledb适配openGauss与性能优化第一阶段实现接口合计11个,其中6个与第二阶段实现的chunk功能存在交互（资料中已加约束），待第二阶段转测后测试涉及到与chunk交互的功能。第一阶段累计发现缺陷17个，其中已验收16个、修复中1个(遗留问题)，总体质量一般。 | 在兼容pg库下创建插件。不支持非编译安装版本。目前TimescaleDB安装之后，不支持删除TimescaleDB插件。TimescaleDB插件依赖于public schema，因此不支持使用drop schema的方式删除public schema 。chunk功能不支持。在不同数据库创建插件需要重启数据库 | <font color=yellow>▮</font> | 无       |
| 【openGauss 6.0.0 RC1】【工具链】【DataKit】业务开发平台     | 业务开发平台特性，进行了2轮测试，覆盖了连接管理、函数过程、表管理、数据库管理、用户管理、视图、序列、同义词特性，本版本新增了213个测试用例，整个测试过程覆盖了功能测试、接口测试、性能测试。 本次测试总共发现52个问题（其中46个为开发bug，6个为功能建议），目前遗留了1个问题，整体质量良好，可供DBA日常使用。 | 函数/过程 （1）在“SQL 终端”或“创建函数/过程”向导创建的函数/过程须以“/”结尾，表示函数/过程的结尾。调试 （1）安装openGauss3.1.0及以上（2）安装 pldebugger 插件3.0及以上 （3）仅支持plpgsql语言的调试<br/>项目运行 （1）本项目依赖主平台的web-socket，若需要使用本项目所有功能，只能通过编译成 jar 包的形式作为插件运行在主平台上，若需单独运行将影响 SQL 终端语句执行、调试函数/过程等功能的使用。<br/>（2）部署安装时需设置心跳时间小于平台token过期时间， 调整心跳方法：在plugins/data-studio/web-ui/src/config/index.ts修改配置心跳时间（单位：毫秒）<br/>// websocket心跳时间<br/>export const wsHeartbeatTime = 1000 * 30;<br/>// http心跳时间<br/>export const httpHeartbeatTime = 1000 * 30;<br/>（3）外表管理功能需要当前操作数据库为企业版，外部服务器创建需要当前操作用户具有管理权限。<br/>（4）替换sqlite后无配置可便捷切换回sqlite<br/> | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】【DataKit】智能运维平台测试报告（SQLite版） | 智能运维平台特性，覆盖了SQL诊断、实例监控、历史数据诊断、告警通知、集群监控特性，共设计了843个测试用例，整个测试过程覆盖了功能测试。 总共发现82个问题，4个功能建议，目前遗留了14个问题，整体质量一般。 | 日志检索：<br/>（1）FileBeat版本由8.3.3进行二次开发新增openGauss Module<br/>（2）需要在openGauss实例所在服务器安装FileBeat，以及FileBeat配置文件要求<br/>（3）ES版本与FileBeat版本保持一致<br/>（4）用户需要自行搭建ElasticSearch环境；ElasticSearch服务器性能要求、ElasticSearch配置文件要求<br/>（5）Java 11及以上版本<br/>（6）需要根据约定规则设计ES索引名称与插入具体字段值<br/>（7）ElasticSearch需要集中存储多实例的日志，对存储容量要求较高，实际根据数据库实例数量、实例日志数量、ElasticSeach部署方式决定；<br/>实例监控:<br/>（1）使用平台插件安装部署exporter和Prometheus<br/>（2）集群数据库使用内存监控时需要设置对应参数：max_process_memory=12G, shared_buffers=64M, cstore_buffers=16M,enable_memory_limit=on<br/>SQL诊断-性能分析:<br/>（1）仅支持Linux版本在4.1或以上版本<br/>（2）BCC版本需与内核版本匹配<br/>（3）Python3或以上版本<br/>（4）java 11或以上版本<br/>（5）agent工具使用Java语言编写，但eBPF需与Linux内核交互，故只支持Linux平台<br/>（6）需与Linux内核交互对性能有较高的要求，因此对于openGauss数据库的硬件系统有较高的要求。<br/>SQL诊断与慢SQL:<br/>（1）需要在openGauss实例所在服务器安装bcc，以及安装诊断数据采集服务<br/>（2）Java 11或以上版本<br/>（3）个人开发者最低配置2核4G，推荐配置4核8G。预留50%以上的磁盘剩余空间用于数据存储。<br/>（4）服务器需配置：echo vm.max_map_count = 262144 >> /etc/sysctl.conf &&sysctl -p<br/>（5）SQL诊断使用explain需配置参数：track_stmt_parameter=on<br/>（6）慢sql使用时，数据库需配置：<br/>track_stmt_stat_level='L1,L1'<br/>enable_stmt_track=on<br/>track_stmt_parameter=on<br/>log_min_duration_statement=10s<br/>如果track_stmt_stat_level参数通过guc参数修改不生效，则执行ALTER DATABASE postgres SET track_stmt_stat_level TO 'L1,L1'<br/>自动部署:<br/>（1）需要先在服务器上安装yum命令<br/>（2）安装后如若无数据显示，可做如下操作：卸载Agent、重装Agent、停用插件、启动插件<br/> | <font color=yellow>▮</font> | 无       |
| 【openGauss 6.0.0 RC1】【工具链】【迁移工具】portal配置优化和【DataKit】MySQL迁移过程增加预操作和切换前检查 | portal配置优化，共执行用例19个，主要覆盖了功能测试、资料测试以及易用性测试。功能测试主要覆盖了portal安装后修改全量迁移参数配置文件、增量迁移参数配置文件配置文件、反向迁移参数配置文件、portal统一参数配置文件后等级迁移，以及迁移后参数配置是否生效，修改后的参数配置文件是否保留等功能。资料测试主要覆盖操作手册是否完整，易用性主要覆盖操作流程是否符合，按键布局及UI是否涉及合理，弹框大小及位子是否布局合理等功能。累计发现缺陷单4个，3个缺陷已解决，回归通过，整体质量良好。<br/>MySQL迁移过程增加预操作和切换前检查，共执行用例20个，主要覆盖功能测试、资料测试以及易用性测试，功能测试主要覆盖服务可用性检查、数据库连接检查、数据库权限检查、大小写参数配置检查、磁盘空间检查和切换迁移前检查以及迁移前是否选择开启调整内核参数等功能。资料测试主要覆盖惭怍手册是否完整，易用性主要覆盖操作流程是否符合，按键布局及UI是否涉及合理，弹框大小及位子是否布局合理等功能。累计发现缺陷1个，1个缺陷已解决，回归通过，整体质量良好。 | MySQL 5.7及以上版本。Java为11及其以上。                      | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】反向迁移工具能力增强特性    | 反向迁移工具能力增强，共执行用例43个，主要覆盖了功能测试、性能测试、可靠性测试、资料测试。功能测试覆盖迁移不同数据类型映射的数据，是否迁移成功且数据一致。配置迁移source端与sink端新增参数后，参数是否生效，参数配置错误，是否合理报错。配置MySQL与openGauss的超时参数，启动迁移并长时间无操作，之后再插入数据查看是否正常迁移。性能测试，在高性能机器下，开启反向迁移，通过sysbench工具给openGauss压测数据，待数据回放完成，计算迁移效率。可靠性测试覆盖迁移中，在sink端制造异常导致迁移终止，再次启动迁移后是否从中断处正常迁移，数据是否完整，已迁移的数据是否跳过，迁移的数据不重复。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单4个，4个缺陷均已解决且回归通过，整体质量良好。 | 该特性继承原有反向迁移的约束。启动迁移进度上报时，继承原有迁移进度上报功能的约束。该特性无新增约束。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【迁移工具】增量迁移工具能力增强测试  | 增量迁移工具能力增强，共执行用例42个，主要覆盖了功能测试、性能测试、资料测试以及常稳测试。功能测试主要覆盖source端和sink端参数有效性，手动kill sink端和sourc端进程再恢复后数据是否能完成迁移且不重复，以及增量迁移后数据类型兼容性。性能测试，在高性能机器下，设置按表回放，开启增量迁移，通过sysbench工具压测数据，待数据回放完成，计算迁移效率。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单5个，5个缺陷已解决，回归通过，整体质量良好。 | 1、MySQL 5.7及以上版本<br/><br/>2、MySQL参数设置： log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON，若未设置gtid_mode = OFF，则sink端回放退化为串行回放，会降低在线迁移性能<br/><br/>3、增量迁移是直接透传DDL的，对于openGauss侧语法和MySQL侧语法不一样的，迁移失败<br/><br/>4、启动增量迁移，先启动source端，再启动sink端，confluent/confluent-5.5.1/etc/schema-registry/connect-avro-standalone.properties文件rest.port需配置不同端口<br/><br/>5、为保证事务的顺序性，kafka_2.12-3.6.0/config/server.properties文件num.partitions参数必须设置为1<br/><br/>6、性能测试对硬件系统有较高要求，仅insert和混合场景迁移性能达到3w tps且必须进行性能调优<br/><br/>mysql高性能配置，binlog位置、安装目录、数据目录分别部署在3个不同的NVME盘<br/><br/>openGauss侧openGauss高性能配置，pg_xlog、安装目录、数据目录分别部署在3个不同的NVME盘<br/><br/>在java11环境上运行在线迁移工具<br/> | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】可视化运维特性              | 可视化运维共计执行240条用例，主要覆盖了功能测试、兼容性测试、易用性测试、资料测试。功能测试覆盖集群管理、一键安装、集群安装、批量升级、集群监控、日志分析、WDR报告、SQL诊断、安装包管理、自定义控制台、备份恢复。兼容性测试验证在chrome浏览器、firefox浏览器上的使用，网页元素位置是否混乱、错位，页面的格式，字体，输入框，下拉框，复选框， 按钮是否正常显示。易用性测试覆盖检查web页面的布局、UI设计是否合理，操作顺序是否符合逻辑。资料测试覆盖资料的一致性、准确性。累计发现缺陷单12个，11个缺陷均已解决且回归通过，1个缺陷未解决。整体质量良好。 | 当前平台运行依赖于openJdk11。平台使用的数据库，当前仅支持openGauss数据库，并且需要提前创建database。需要将部署服务器IP配置在平台使用的数据库（openGauss）的白名单列表中。平台默认的登录账号密码：admin/admin123，请在首次登录后及时修改密码。注意，如果是openEuler操作系统，需要手动先设置umask值为022，设置命令： umask 022。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】支持O_全量迁移              | 全量迁移特性，共计执行58个用例，主要覆盖了功能测试和资料测试，功能测试覆盖已支持的O*常用数据类型、表类型、索引、约束、序列、视图、表空间、同义词等对象测试。资料测试覆盖校验资料的描述及示例的执行结果是否通过。累计发现issue4个，已验收通过，整体质量良好。 | 需要配置O*和openGauss数据库用户的连接权限、执行sql的权限<br/>O*：<br/>CREATE USER test IDENTIFIED BY 'password123';<br/>GRANT CONNECT TO test;<br/>openGauss：<br/>create user test sysadmin password 'password123';<br/>需要将执行全量迁移的机器IP配置在openGauss数据库的白名单列表中<br/>gs_guc reload -Z datanode -N all -I all -h "host all all 0.0.0.0/0 sha256" gs_guc reload -Z datanode -N all -I all -c "password_encryption_type=1" | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】支持O_反向迁移              | 反向迁移特性，共计执行55个用例，主要覆盖了功能测试和资料测试，功能测试覆盖已支持的O*常用数据类型、表类型等测试，以及不同的dml语法对于反向迁移的影响。校验资料的描述及示例的执行结果是否通过。累计发现issue2个，已验收通过，整体质量良好。 | 需要将执行反向迁移任务的机器ip设置到openGauss数据库的白名单内<br/>需要为openGauss数据库用户开启replication相关权限<br/>host replication 【user】 0.0.0.0/0 sha256<br/>调整guc参数：<br/>alter system set wal_level to logical;<br/>在启动反向迁移任务之前，需要启动zookeeper、kafka等工具<br/>cd kafka_2.13-3.2.3 ./bin/zookeeper-server-start.sh ./config/zookeeper.properties<br/>./bin/kafka-server-start.sh ./config/server.properties | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】支持O_增量迁移              | 增量迁移特性，共计执行48个用例，主要覆盖了功能测试、易用性测试、可靠性测试、性能测试、稳定性测试和资料测试，功能测试覆盖已支持的O*常用数据类型、表类型等测试，以及不同的dml、ddl语法对于增量迁移的影响。校验资料的描述及示例的执行结果是否通过。累计发现issue3个，已验收通过，整体质量良好。 | 1.本次测试对O*到openGauss的增量迁移进行测试<br/>2.需要为O*开启归档参数archive log，并赋予相关权限，操作步骤如下：<br/>1）查看是否处于归档模式：SELECT log_mode FROM v$database;如果现实数据库为归档模式（ARCHIVELOG），则后续步骤不用做。<br/>2）开启归档模式：<br/>--设置归档目录位置（注：此目录需要在执行sql前手动创建）<br/>SQL>ALTER SYSTEM SET db_recovery_file_dest='/opt/O/oradata/recovery_area' scope=spfile;<br/>--重启数据库<br/>SQL> SHUTDOWN IMMEDIATE;<br/>SQL> STARTUP MOUNT;<br/>--开启归档日志<br/>SQL> ALTER DATABASE ARCHIVELOG;<br/>--开启数据库<br/>SQL> ALTER DATABASE OPEN;<br/>--如果为pdb库，执行<br/>SQL> ALTER PLUGGABLE DATABASE 【库名】 OPEN;<br/>3）为待同步的用户授权，此处用test进行说明，如用其他用户请进行替换<br/>SQL> grant connect to test;<br/>SQL> grant execute on dbms_logmnr to test;<br/>SQL> grant select any transaction to test;<br/>SQL> grant create table to test;<br/>SQL> grant select on v_$logmnr_contents to test;<br/>SQL> grant select on v_$log to test;<br/>SQL> grant select on v_$archived_log to test;<br/>SQL> grant select on v_$database to test;<br/>SQL> grant select on v_$logfile to test;<br/>SQL> grant select on gv_$log to test;<br/>SQL> grant select on gv_$archived_log to test;<br/>SQL> grant select on gv_$logfile to test;<br/>4）为数据库和特定的表启用补充日志记录<br/>alter databse SID add supplemental log data;<br/>alter table schema_name.table_name add supplemental log data (all) columns;<br/>3.在启动增量迁移任务之前，需要启动zookeeper、kafka等工具<br/>cd kafka_2.13-3.2.3 ./bin/zookeeper-server-start.sh ./config/zookeeper.properties<br/>./bin/kafka-server-start.sh ./config/server.properties<br/>4.dml中仅对数据的insert、update、delete操作进行增量迁移，dml操作可以不在事务中；ddl必须在事务中才可以进行增量同步。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】数据库向导式一站式安装工具  | 数据库向导式一站式安装工具特性，共设计 37 个测试用例，包含一键式安装传统单机/主备环境、一键式安装资源池化环境，以及一键式安装在普通用户下执行测试。发现 27 个问题，3 个非问题已取消，其余 24 个已修复并验收通过，整体质量良好。 | 需要安装 pyhton3 版本                                        | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】数据库安装流程解除对root用户的依赖 | 数据库安装流程解除对root用户的依赖特性，共执行用例11个，主要覆盖了功能测试及资料测试，功能测试主要覆盖了使用omm用户进行预安装和安装不带cm和带cm的数据库，以及使用omm用户执行gs_sshexkey、gs_checkos、gs_upgradectl、gs_expansion等功能，资料测试主要测试安装指南对使用omm方式安装数据库的描述以及工具参考中对之前使用root用户执行的工具，添加可以使用omm用户执行的描述。 | 需要将omm用户写入到定时任务。创建多节点数据库时，须提前在各节点使用root用户创建好用户和用户组 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】数据校验能力增强            | 数据校验能力增强特性，以分片粒度进行数据校验。累计发现缺陷单4个，其中已验收4个，整体质量良好。 | M*5.7版本。jdk11以上。kakfa 2.13-2.81                        | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核】允许修改序列                  | 支持对序列的修改特性，共执行107条用例，主要覆盖修改各属性的功能测试，累计发现缺陷单2个，均已解决并验收通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核】神舟通用_切换后台日志为中文日志 | 后台及工具支持切换运行日志为中文日志特性，共执行4个用例，主要覆盖修改各属性的功能测试，累计发现缺陷单4个，3个缺陷已解决并验收通过，1个缺陷为遗留问题，整体质量良好。 | 数据库已正确安装。后台版本编译开启ENABLE_NLS宏。数据库实例编码为zh_CN.UTF-8。配置conf参数 lc_messages=zh_CN.UTF-8 enable_nls=on。环境变量 LANG=zh_CN.UTF-8 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】gs_collector适配资源池化  | gs_collector适配资源池化特性，共计执行44条用例，主要覆盖了功能测试和资料测试。功能测试覆盖gs_collector的采集基础功能，以及指定配置文件采集指定信息，资源池化特定采集配置。累计发现缺陷单4个，4个问题单均已修复，整体质量良好。 | gs_collector工具适配资源池化目前只支持openGauss版本：5.1.1。只能数据库用户执行（非root）。目前仅能收集用户设定时间范围内存在的备节点日志信息。目前仅能收集共享磁盘中已有内容的文件信息 | <font color=green>▮</font>  | 无       |
| 【资源池化】CM pause支持资源池化                             | CM pause支持资源池化，共执行49个用例，主要覆盖了功能测试、可靠性和资料测试。功能测试覆盖对CM集群执行暂停和恢复后CM的功能。可靠性测试覆盖在暂停后磁盘、网络、集群双主等异常时，CM的功能是否可用。资料测试覆盖校验资料的描述是否完整无误。测试过程中未发现问题单，整体质量良好。 | 集群各节点间的互信网络正常。暂停操作和扩缩容操作不能同时进行，否则会出现扩缩容失败。在执行cm_ctl pause命令前，需要先停掉业务，防止此时出现故障导致业务异常。 | <font color=green>▮</font>  | 无       |
| 【资源池化】主备双集群支持通过网络进行主备日志同步           | 主备双集群支持通过网络进行主备日志同步，累计执行66个用例，主要覆盖了功能测试、可靠性测试，性能&稳定性测试；发现问题9个，已解决9个，回归通过，无遗留风险，整体质量一般，后续因特性融合场景，禁掉了部分功能，删减测试用例2个； | 至少2+2计算节点。2套Dorado存储设备。                         | <font color=yellow>▮</font> | 无       |
| 【内核】支持异步备升主时旧主数据找回功能                     | 支持异步备升主时旧主数据找回功能，共执行46个测试用例，主要包含gs_retrieve工具参数校验测试、gs_retrieve工具功能验证、pg_create_logical_replication_slot函数测试，及性能测试等。其中1个用例不适配，其余用例皆执行通过，未发现问题，整体质量好。 | gs_retrieve工具仅支持在旧主所在机器上使用，新主可在不同机器。 旧主恢复之后建议将autovacuum设置为off，防止元数据的历史版本被回收清理。 解码范围内存在DDL的情况下，可能由于元数据的历史版本被清理而无法正常解码。通过force方式解码，可能会丢失数据。 继承原有逻辑解码的约束。 | <font color=green>▮</font>  | 无       |
| 【内核】新增MySQL协议hot_standby模式                         | 新增MySQL协议hot_standby模式，累计执行9个用例，主要覆盖了dolphin_hot_standby参数是否可以正常设置，该参数的基础功能测试和主备切换情况下是否可用，共发现0个问题单，整体质量良好。 | MySQL JDBC driver应为5.1.47版本，MySQL命令行客户端版本号应在5.7.26~5.7.42之间。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】portal执行流程优化（一）    | portal执行流程优化，累计执行测试用例21个，主要覆盖了功能及资料测试。功能测试主要覆盖Datakit启动portal进行离线、在线迁移，使用Datakit在线、离线、导入安装portal是否能正常安装以及能否卸载portal，命令行启动portal、脚本启动portal进行数据迁移。资料测试覆盖校验资料的描述是否完整。累计发现问题7个，均已解决，验收通过，整体质量良好。 | MySQL 5.7及以上版本。MySQL参数设置：log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON，若未设置gtid_mode = OFF，则sink端回放退化为串行回放，会降低在线迁移性能。增量迁移是直接透传DDL的，对于openGauss侧语法和MySQL侧语法不一样的，迁移失败。在java11环境上运行Datakit和portal迁移工具。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】datakit工具支持资源池化双集群部署 | datakit工具支持资源池化双集群部署，累计执行用例38个，覆盖了功能测试、资料测试。功能测试覆盖新增、修改、删除磁阵信息，安装资源池化容灾双集群，查看容灾双集群信息，集群间进行主备切换、解除容灾关系操作，验证容灾集群单集群启停、主备切换操作，解除容灾关系后卸载、删除集群。累计发现问题单3个，均已解决，验收通过，整体质量良好。 | 每个单集群需关联不同的Dorado磁阵，且磁阵之间需要能够相互访问。 每个单集群的xlog卷只能有一个，并且需要建立好多个集群上xlog盘之间的同步复制关系。上述操作均需要在DeviceManager上操作完成。 具有同步关系的xlog盘的容量要相同。 |                             |          |
| 【openGauss 6.0.0 RC1】【工具链】扩容工具易用性提升          | 扩容工具易用性提升，累计执行测试用例32个，验证root、omm用户下的om、cm集群单机、主备扩容。gs_om -t generate_xml工具生成单机的xml文件以及om、cm集群的xml文件生成。累计发现问题3个，均已验收通过，整体质量良好。 | 扩容前需要在做preinstall操作，需要依赖建立互信。当前集群和新增节点在/etc/hosts配置所有ip的节点名称映射关系。新节点需要建立好同集群的子用户。配置部署的相关目录权限子用户可读写。cm安装句柄数目至少640000（建议配置1000000）。/etc/cron.allow配置子用户。升级未完成不能扩容以及扩容未结束不能升级。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【兼容性】B兼容性下字段模糊匹配支持走索引扫描 | B兼容性下字段模糊匹配支持索引扫描，累计执行测试用例33个，主要涉及char、varchar、text类型在utf8、gbk、gb18030编码集及字符序下的右模糊匹配是否走索引及文档验证，共发现2个问题，现已回归通过，无遗留风险，整体质量良好。 | 需在openGauss中兼容B库下验证。库中存在多种字符集时需要设置b_format_behaviour_compat_options = enable_multi_charset。模糊查询强制走索引需要设置enable_seqscan = off。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】索引创建支持预扩展，堆表支持预读 | 索引创建支持预扩展，堆表支持预读，累计执行29个测试用例，主要覆盖了功能测试、稳定性测试、性能测试、升级测试及资料测试。共发现问题3个，已全部修复并回归通过，整体质量良好 。 | 本特性在资源池化部署下不生效。堆表线性扫描场景才有性能提升。列存表，段页式表，ustore表全表扫描不走预读块流程。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【工具链】DataKit支持兼容性评估（三） | DataKit支持兼容性评估，共执行用例16个，主要覆盖了功能测试和资料测试，功能测试覆盖插件的安装，卸载，不同场景下获取SQL语句进行评估，评估报告的格式以及结果准确性进行测试，资料测试覆盖校验资料的描述是否完整。累计发现缺陷单4个，4个缺陷均已解决且回归通过，整体质量良好。 | MySQL 5.7.27版本，数据库除初始用户外的数据库用户均有复制权；配置文件添加以下参数：MySQL参数设置： general_log = 1，slow_query_log = on。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【迁移工具】MySQL数据迁移完整方案打通（校验工具） | MySQL数据迁移完整方案打通（校验工具），累计执行16个用例，功能测试验证数据类型，表对象、数据异常场景(新增、缺失、更改)，累计发现缺陷单4个，均已验收，整体质量良好。 | M* 5.7版本。不支持表级对象实时校验(例如新增表、表丢失等) 协同使用时chameleon工具。reader、writer配置4线程左右。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【内核】分区表性能优化                | 分区表性能优化，累计执行5个用例，主要覆盖了tpcc模型导入时间缩短、并发测试性能提升，以及对分区表执行查询、插入、指定分区插入等操作性能提升，共发现0个问题单，整体质量良好。 | 本报告中的各场景测试结果中的处理时间，指的是openGauss的处理时间，不包含其他第三方业务的处理时间。本次性能测试是在实验室网络环境下，搭建测试环境并进行的性能测试，网络条件较稳定，不考虑现网与实验室的网络条件差异。本报告中，使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力。本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异，本次测试环境具体情况，请参考第二章节。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】支持在线reform            | 支持在线reform特性，等待事件相关视图 相关特性测试特性测试中，主要涉及功能验证、一致性长跑测试，故障测试共发现12个问题，回归通过，无遗留风险，质量一般。 | 无                                                           | <font color=yellow>▮</font> | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】XLog按需回放性能优化      | XLog按需回放性能优化，累计执行用例47个，主要覆盖功能测试和性能测试。功能测试主要为参数测试、failover测试以及升级测试，参数测试正常赋值和异常赋值情况下特性功能正常；failover测试在主机故障后，保证failover成功，以及按需回放功能正常。性能测试在一主一备部署，主机在100并发、TPCC负载50WtpmC、RTO<10秒。本次测试发现问题8个，已解决8个，均回归通过，无遗留风险，整体质量良好。 | 本特性仅支持在资源池化部署下使用。在对外提供服务后，数据库仍在后台进行回放，直到全部内容完成后退出回放，该阶段称为“按需回放阶段”。按需回放阶段仅支持部分类型SQL语法（INSERT/UPDATE/DELETE/SELECT/SET/SHOW），该状态请使用pg_controldata工具查询。按需回放阶段禁用autovacuum。实时构建能力仅在备机failover场景生效。 当前不支持多备机情况下，优先选择实时构建节点升主。不支持双集群。 | <font color=green>▮</font>  | 无       |
| 【openGauss 6.0.0 RC1】【资源池化】CBB MES支持线程池，新增DMS、CBB等待时间统计 | 实现资源池化mes_worker线程池化特性，主要涉及功能验证、特性耦合测试，故障场景，性能场景测试共发现10个问题，回归通过，无遗留风险，质量一般。 | 无                                                           | <font color=yellow>▮</font> | 无       |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 专项测试结论

### 安全测试

openGauss 6.0.0 RC1版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。--待确认

3、从数据库越权风险管理、数据库注入攻击、数据库秘钥管理、数据库辅助工具安全、数据库攻击及审计、数据库内存问题、数据库安全框架设计问题和数据库操作系统提权8大方向进行安全测试。

openGauss 6.0.0 RC1版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 可靠性测试

openGauss 6.0.0 RC1 版本可靠性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及2个长时间负载测试。

1、故障注入类测试：共计测试4个版本，在x86+Centos，x86openEuler，Kylin环境下共计执行715个用例，测试用例累计执行率100%，测试发现4个问题（每个版本问题单已去重），已优化闭环2个，待解决1个（优化类问题单，不影响功能），遗留规避1个，整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：共计测试9个版本，共执行1个用例，测试用例累计执行率100%，未发现问题，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | TPCC+sysbench连跑insert/update/delete事务，500仓+100并发，7*24H测试，包括虚拟机、物理机测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景2  | 在TPCC 500仓+100并发压力的背景下，执行特性测试，共计覆盖可商用特性17个 | 各特性测试符合预期，主备机无core，cpu、io、内存等资源波动符合预期，TPCC业务不中断 |

### 性能测试

对关键性能指标进行摸底和测试，主要覆盖了传统主备的TPC-C测试及RTO测试、资源池化的sysbench性能测试及RTO测试、迁移工具的sysbench性能测试，性能多轮测试稳定。

| **指标大项**      | **指标小项**                                               | **指标值** | **说明**          | 测试结论                                                     |
| ----------------- | ---------------------------------------------------------- | ---------- | ----------------- | ------------------------------------------------------------ |
| TPCC              | 2P（Taishan 200 2280 7260）单节点 1H                       | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***156.7***W+                  |
|                   | 2P（Taishan 200 2280 7260）单节点 8H                       | 145万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为***153.1***W+                  |
|                   | 2P（Taishan 200 2280 7260）一主一备 1H                     | 120万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***122.6***W+                |
|                   | 2P（Taishan 200 2280 7260）一主一备 8H                     | 110万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为***122.3***W+                |
|                   | 4P（Taishan 200 2480 7260）单节点 1H                       | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***242.0***W+                    |
| RTO高可用         | 一主两同步备failover                                       | 10s        | 此即为release基线 | **2**s                                                       |
| 多分区表性能      | 2P（Taishan 200 2280 7260）单节点 3000分区 list-list分区表 | /          | /                 | 单节点1H在1000仓800并发的极限场景配置下tpmC为***55.0***W+<br/>分区表插入数据时间master版本耗时***9s***，小于标准15s<br/>分区表指定分区插入数据时间master版本耗时***9s***，小于标准15s<br/>通过pgbench对分区表进行数据查询，查询数据100000条，单分区键场景耗时***67s***，多分区键场景耗时***74s*** |
| 资源池化sysbench  | 2P（Taishan 200 2280 7260）主机读写备机空载                |            |                   | 主机读写备机空载场景下，主机使用oltp_read_write，性能为***3.5w tps***。 |
|                   | 2P（Taishan 200 2280 7260）主机读写备机只读                |            |                   | 主机读写备机只读场景下，主机使用oltp_read_write，备机使用oltp_read_only，主机性能***2.39w tps***，备机性能***3.86w tps***，总计***6.1w tps***。 |
| 资源池化TPCC      | 2P（Taishan 200 2280 7260）主机读写备机空载 1H             | 120万      | 此即为release基线 | 主机读写备机空载 1H在极限场景配置下tpmC为***122.9***W+       |
|                   | 2P（Taishan 200 2280 7260）主机读写备机只读 1H             | 220万      | 此即为release基线 | 主机读写备机只读 1H在极限场景配置下tpmC为***220.1***W+       |
| 资源池化RTO高可用 | 单集群                                                     |            |                   | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库主备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br/>单集群故障前25wtpmC，30并发场景下，RTO恢复时间为***8s***，达到标准<10s。故障前50wtpmC，100并发场景下，RTO恢复时间为***29s***，达到标准<30s。 |
|                   | 双集群                                                     |            |                   | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库主备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br/>双集群故障前30wtpmC，160并发场景下，RTO恢复时间为***2s***，达到标准<20s。 |
| 工具链            | M*向openGauss数据库全量迁移                                | 300MB/s    | 此即为release基线 | 源端存在10张表，单表数据量3000万，线程数20场景下，M\*向openGauss全量迁移速度为***407MB/s*** |
|                   | M*向openGauss数据库增量迁移                                | 3w tps     | 此即为release基线 | 源端存在10张表，单表数据量10万，线程数50，混合IUD场景下，M\*向openGauss增量迁移速度为***3.02w tps*** |
|                   | openGauss向M*数据库反向迁移                                | 1w tps     | 此即为release基线 | 源端50张表，单表数据量10万，线程数50，混合IUD场景下，openGauss向M\*反向迁移速度为***2.02w tps*** |
|                   | openGauss向M*数据库反向全量迁移                            | 80MB/s     | 此即为release基线 | 源端10张表，单表数据量3000万，线程数50场景下，openGauss向M\*反向迁移速度为***92 MB/s*** |
|                   | M*向openGauss数据库全量数据迁移，datachecker进行校验       | 20wtps     | 此即为release基线 | 50张表，单表数据量1000万场景，线程池设定为30，数据校验速度为***20.6w tps*** |

### 兼容性测试

#### 升级兼容

针对openGauss 6.0.0 RC1版本，规划的升级路径如下表所示。测试整体情况：

1. openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/penGauss 5.1.0升级到openGauss 6.0.0 成功，升级失败或者升级未提交，可以成功回滚；
2. 故障场景下，环境恢复后升级可以重入并升级成功
3. 升级成功后，特性功能运行正常

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0灰度升级到openGauss 6.0.0 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0灰度升级到openGauss 6.0.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0灰度升级到openGauss 6.0.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 6.0.0就地升级到openGauss 6.0.0 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0就地升级到openGauss 6.0.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0就地升级到openGauss 6.0.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0不带cm灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0带cm灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 2.0.5不带cm灰度/就地升级到openGauss 3.0.5不带cm，再灰度/就地升级到openGauss 6.0.0不带cm | 测试通过 |
| openGauss 2.0.5不带cm灰度/就地升级到openGauss 3.0.5不带cm，再灰度/就地升级到openGauss 6.0.0不带cm，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5不带cm灰度升级到openGauss 3.0.5带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0带cm灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.0不带cm灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.0不带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0不带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.5灰度升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.0.5灰度升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5灰度升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5就地升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.0.5就地升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5就地升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5带cm灰度升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.5不带cm灰度升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.0.5不带cm就地升级到openGauss 5.0.0/openGauss 5.0.1/openGauss 5.1.0不带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1/openGauss 5.1.0，再灰度升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1/openGauss 5.1.0，再就地升级到openGauss 6.0.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0带cm灰度升级到openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.1.0不带cm灰度升级到openGauss 5.0.1/openGauss 5.1.0带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |
| openGauss 3.1.0不带cm灰度升级到openGauss 5.0.1/openGauss 5.1.0不带cm，再灰度升级到openGauss 6.0.0带cm | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 6.0.0 RC1数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 6.0.0 RC1数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论    |
| ------------ | ------------------------------------------------------------ | ----------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 6.0.0 RC1数据库 | 测试   通过 |

### 资料测试

| 序号 | **手册名称**       | **测试结论** |
| ---- | ------------------ | ------------ |
| 1    | 《法律声明》       | PASS         |
| 2    | 《发行说明》       | PASS         |
| 3    | 《关于openGauss》  | PASS         |
| 4    | 《快速入门》       | PASS         |
| 5    | 《安装指南》       | PASS         |
| 6    | 《简易教程》       | PASS         |
| 7    | 《应用开发指南》   | PASS         |
| 8    | 《编译指南》       | PASS         |
| 9    | 《数据库管理指南》 | PASS         |
| 10   | 《数据库运维指南》 | PASS         |
| 11   | 《性能调优指南》   | PASS         |
| 12   | 《AI特性指南》     | PASS         |
| 13   | 《数据迁移指南》   | PASS         |
| 14   | 《插件参考》       | PASS         |
| 15   | 《SQL参考》        | PASS         |
| 16   | 《工具与命令参考》 | PASS         |
| 17   | 《数据库参考》     | PASS         |
| 18   | 《附录》           | PASS         |

# 问题单统计

openGauss 6.0.0 RC1版本共发现问题1344个，有效问题1143个，无效问题201个。修复问题回归测试结果正常，版本整体质量良好。遗留18个问题详细分布见下表: 

| 版本名称                         | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| -------------------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 6.0.0 RC1 Test round 1 | 2023-10-18   | 2023-10-24   | 125        | 33         |
| openGauss 6.0.0 RC1Test round 2  | 2023-10-25   | 2023-10-31   | 49         | 14         |
| openGauss 6.0.0 RC1Test round 3  | 2023-11-1    | 2023-11-7    | 55         | 11         |
| openGauss 6.0.0 RC1Test round 4  | 2023-11-8    | 2023-11-14   | 51         | 9          |
| openGauss 6.0.0 RC1Test round 5  | 2023-11-15   | 2023-11-21   | 54         | 4          |
| openGauss 6.0.0 RC1Test round 6  | 2023-11-22   | 2023-11-28   | 56         | 13         |
| openGauss 6.0.0 RC1Test round 7  | 2023-11-29   | 2023-12-5    | 59         | 14         |
| openGauss 6.0.0 RC1Test round 8  | 2023-12-6    | 2023-12-12   | 55         | 10         |
| openGauss 6.0.0 RC1Test round 9  | 2023-12-13   | 2023-12-19   | 82         | 13         |
| openGauss 6.0.0 RC1Test round 10 | 2023-12-20   | 2023-12-26   | 71         | 14         |
| openGauss 6.0.0 RC1Test round 11 | 2024-1-10    | 2024-1-16    | 48         | 4          |
| openGauss 6.0.0 RC1Test round 12 | 2024-1-17    | 2024-1-24    | 66         | 9          |
| openGauss 6.0.0 RC1Test round 13 | 2024-1-25    | 2024-1-31    | 68         | 7          |
| openGauss 6.0.0 RC1Test round 14 | 2024-2-1     | 2024-2-6     | 62         | 16         |
| openGauss 6.0.0 RC1Test round 15 | 2024-2-7     | 2024-2-20    | 29         | 4          |
| openGauss 6.0.0 RC1Test round 16 | 2024-2-21    | 2024-2-28    | 67         | 10         |
| openGauss 6.0.0 RC1Test round 17 | 2024-2-29    | 2024-3-5     | 43         | 2          |
| openGauss 6.0.0 RC1Test round 18 | 2024-3-6     | 2024-3-12    | 44         | 11         |
| openGauss 6.0.0 RC1Test round 19 | 2024-3-13    | 2024-3-19    | 19         | 0          |
| openGauss 6.0.0 RC1Test round 20 | 2024-3-20    | 2024-3-30    | 16         | 0          |

本次测试共20轮，从Test round 1持续到Test round 20。2023.10.18正式进入openGauss 6.0.0 RC1第一阶段测试，Test round 1转测工具链需求，问题单急剧上升，达到峰值。Test round 2 至Test round 10，问题单处于缓慢增长趋势。2024.1.10正式进入openGauss 6.0.0 RC1第二阶段测试，Test round11至Test round13转测部分预埋需求，问题单达到高峰值，随后进入下降趋势。随着330需求开发相继完成、转测，问题单在Test round16进入上升期，工具链、资源池化、内核发现较多问题。Test round17开始进入集成测试阶段，需求验收逐渐完成，问题单处于下降趋势。Test round18完成一轮集成测试，涉及长稳、可靠性、性能、安全、资源池化等场景。随后进入Test round19集成测试第二阶段，问题单数下降过半，集成测试问题单主要集中在内核、工具模块。Test round20为最后一个集成验证周期，测试活动主要为问题回归。

# 附件

## 附件1：遗留问题列表

| 序号 | issue号 | 问题简述                                                     | 分类     | 问题级别 | 问题分析与影响                                               | 规避措施                                                     |
| ---- | ------- | ------------------------------------------------------------ | -------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | I960C3  | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，tpcc插入数据时备机coredump，报错信息：unexpected blocknum 2 >= spc nblocks 0 | 资源池化 | 次要     | 定位进展：备机重启场景下，校验主动core。该校验认为，节点有一个页面，该页面应处于段页式记录的文件的范围内。 影响：该校验在debug版本，release没有。即使core，因为段页式记录只是内存信息，core之后，内存恢复后，节点仍可正常跑 | 恢复手段：集群在core仍可以恢复正常                           |
| 2    | I9369O  | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，主节点读写业务频繁报错ERROR: seg_read blocknum exceeds segment size | 资源池化 | 次要     | 日志观察疑似同一张表extend并发了，没加锁，但走读代码并未发现问题。目前未能复现。 | 重启恢复                                                     |
| 3    | I94H78  | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，开启按需回放实时构建，failover build期间，备机core，关键字：MarkSegPageRedoChildPageDirty | 资源池化 | 次要     | 出现条件：仅在DEBUG下，存在大量DDL与IUD时偶尔出现，重启后可恢复；release下不会出现。由于seg扩页面时的segheader的首条日志与后续页面回放顺序不一致导致，在DDL时偶现，但是并不会导致页面会放出现错误，此处仅是一个置脏检查。 | 恢复手段：debug下偶尔出现，重启恢复；release下不会出现。     |
| 4    | I8WGZR  | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本,开启多个特性,switchover场景超时,备节点产生WAL contains references to invalid pages的core | 资源池化 | 次要     | 定位进展：主机宕机，备节点failover，reform完成进入running状态后，会报invalid page，发现该页面为全零页面，回放这条日志的时候不应该是全零页。追溯往上首先报错I94H78，再报错invalid page 影响：复现概率不高，core之后，后续多次启动恢复，次数不确定 | 规避手段：尽量减少switchover和failover连续切换的次数。 恢复手段：多次重启后可恢复，次数不确定 |
| 5    | I8OIRF  | 【测试类型：功能测试】【测试版本：5.1.1】【资源池化】reform后非初始状态集群，failover场景新主节点读写业务报错主键冲突ERROR: duplicate key value violates unique constraint | 资源池化 | 次要     | 定位进展：在备节点被踢出的场景，主机的业务在reform后有概率出现该报错。具体是执行了一条update语句，where条件涉及主键，update字段不涉及主键；但存在插入index的行为 影响：上层报error，后续继续跑无问题 | 恢复手段：多次重启后可恢复，次数不确定"                      |
| 6    | I9A6Q1  | 【测试类型：性能】【测试版本：6.0.0】【资源池化】一主一备环境下，按需回放实时构建RTO性能不达标 | 资源池化 | 主要     | 怀疑磁阵非独占影响                                           | 不影响使用                                                   |
| 7    | I96UTH  | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，开启按需回放+实时构建，stop&&start之后待升主节点core | 资源池化 | 次要     | 定位进展：备机回放页面时，某一条段页式头页的扩页面日志refcount !=0，怀疑该日志由于由于并发问题导致跳过回放，目前添加日志复现中。 | CM自动重启可恢复                                             |
| 8    | I9A2CR  | 【测试类型：SQL功能】【测试版本：6.0.0】【资源池化】存储复制备集群配置极致RTO，升主后运行tpcc读写产生core文件 | 资源池化 | 次要     | 因为是性能环境，没开日志，现在初步分析到dss alloc ft node失败，导致了dss回滚，进一步分析需要开启debug日志复现，现7天暂未复现 | CM自动重启可恢复                                             |
| 9    | I8VQ6J  | 【测试类型：工具功能】【测试版本：6.0.0】【升级】灰度/就地升级过程中偶现dn节点切换问题 | 工具链   | 次要     | 集群升级后概率偶现主备切换，影响客户的业务接续               | 方案一：升级前，先查询主节点，将节点停止 gs_om -t stop;然后手动拉起，各个节点；如果有cm，停止om_monitor,手动拉起om_monitor， gs_om -t start 重新启动。 方案二：停止定时om_monitor任务，杀掉所有进程，包括fenced ，手动拉起dn节点，重新启动om_monitor。 |
| 10   | I9CFVY  | 【测试类型：功能测试】【测试版本：6.0.0-RC1】csv协同chameleon进行数据校验，提示openGauss少数据。实际openGauss并未丢失数据 | 工具链   | 次要     | csv协同校验不准确                                            | 暂无                                                         |
| 11   | I926ER  | gs_ctl页面缺少部分参数介绍                                   | 工具链   | 不重要   | 资料描述有缺少-Z和-L描述                                     | 暂无                                                         |
| 12   | I90Q0S  | gs_ctl工具， query功能，指定不存在的用户名和密码时，detail_information不显示为FATAL | 工具链   | 不重要   | gs_ctl通过不存在的用户连接，显示的detail_information状态不正确；影响小，不会影响用户的实际使用，只影响查询结果。 | 暂无                                                         |
| 13   | I99A47  | 一主4备双AZ集群存在多cm primary现象[xy]                      | 工具链   | 不重要   | 主备集群出现多个CM主；本地没有复现                           | 暂无                                                         |
| 14   | I99QVU  | 【测试类型：接口功能】【测试版本：6.0.0】 JDBC调用getString()获取兼容B库cast(负数 as time)值与mysql不一致 | 兼容性   | 不重要   | cast(负数 as time)的场景与mysql不一致；特殊场景下影响小      | 暂无                                                         |
| 15   | I9B6QV  | 【测试类型：接口功能】【测试版本：6.0.0】 getObject()/getColumnType()获取tinyint(1)类型和值兼容B库与mysql不一致 | 兼容性   | 不重要   | tinyint(1)类型和值与mysql不一致；特殊场景影响可控            | 暂无                                                         |
| 16   | I950QN  | 【测试类型：SQL功能】【测试版本：6.0.0】 兼容B库year/hour/minute/second/microsecond 函数部分类型返回值和mysql不一致或报错 | 兼容性   | 不重要   | year/hour/minute/second/microsecond函数作为时间函数，在常用场景下，应该使用时间类型作为入参，二进制、bool、json、set、enum等数据类型不是常见的作为时间函数入参的情况，实际使用场景有限 | 手动将对应类型转换成时间类型后再执行对应的函数               |
| 17   | I97AHY  | 【测试类型：性能】【测试版本：6.0.0】interval分区，插入数据自动扩展分区性能差 | 兼容性   | 次要     | 对于分区数过多，需要自动扩展分区的场景，有性能影响           | 对于分区数过多，需要自动扩展分区的场景，有性能影响           |
| 18   | I97KN8  | connect by prior多表关联结果集不对                           | 兼容性   | 不重要   | 影响O兼容性，SWCB结果和O不一样                               | 不在多表join场景下使用swcb                                   |

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@justbk](https://gitee.com/justbk)
+ [@wan005](https://gitee.com/wan005)
+ [@peilinqian](https://gitee.com/peilinqian)
+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)
+ [@li-xin12345](https://gitee.com/@li-xin12345)
