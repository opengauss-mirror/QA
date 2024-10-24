![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改章节 | 修改描述 | 作者              |
| --------- | -------- | -------- | -------- | ----------------- |
| 2024.9.18 | 1.0      | 初稿撰写 |          | wan005，sungang14 |
| 2024.9.22 | 1.1      | 遗留修正 |          | sungang14，wan005 |

目 录

1 概述

2 测试版本说明

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论
>
> 4.3 DBV伙伴测试结论

5 问题单统计

6 附件

7 致谢

**Keywords 关键词**：openGauss 6.0.0 LTS

**Abstract 摘要**：主要是描述了openGauss 6.0.0 LTS版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

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

openGauss 6.0.0 LTS版本新增内核场景化、工具链、资源池化和兼容性等特性（具体特性名可以查看"测试版本说明"章节中的需求列表），并对若干关键缺陷进行了修改。本文主要描述了openGauss 6.0.0 LTS版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 6.0.0 LTS版本整体质量良好。

# 测试版本说明

本章节描述测试版本的基本信息，包括测试对象是什么，以及在什么环境下开展的测试，具体包括被测版本和测试硬件环境。

描述被测对象的版本信息和测试的时间及测试轮次。

| 版本名称                          | 测试起始时间 | 测试结束时间 |
| --------------------------------- | ------------ | ------------ |
| openGauss 6.0.0 LTS Test round 1  | 2024/4/1     | 2024/4/10    |
| openGauss 6.0.0 LTS Test round 2  | 2024/4/11    | 2024/4/17    |
| openGauss 6.0.0 LTS Test round 3  | 2024/4/18    | 2024/4/24    |
| openGauss 6.0.0 LTS Test round 4  | 2024/4/25    | 2024/5/8     |
| openGauss 6.0.0 LTS Test round 5  | 2024/5/9     | 2024/5/15    |
| openGauss 6.0.0 LTS Test round 6  | 2024/5/16    | 2024/5/29    |
| openGauss 6.0.0 LTS Test round 7  | 2024/5/30    | 2024/6/5     |
| openGauss 6.0.0 LTS Test round 8  | 2024/6/6     | 2024/6/12    |
| openGauss 6.0.0 LTS Test round 9  | 2024/6/13    | 2024/6/19    |
| openGauss 6.0.0 LTS Test round 10 | 2024/6/20    | 2024/6/26    |
| openGauss 6.0.0 LTS Test round 11 | 2024/6/27    | 2024/7/10    |
| openGauss 6.0.0 LTS Test round 12 | 2024/7/11    | 2024/7/17    |
| openGauss 6.0.0 LTS Test round 13 | 2024/7/18    | 2024/7/24    |
| openGauss 6.0.0 LTS Test round 14 | 2024/7/25    | 2024/7/31    |
| openGauss 6.0.0 LTS Test round 15 | 2024/8/1     | 2024/8/7     |
| openGauss 6.0.0 LTS Test round 16 | 2024/8/8     | 2024/8/14    |
| openGauss 6.0.0 LTS Test round 17 | 2024/8/15    | 2024/8/21    |
| openGauss 6.0.0 LTS Test round 18 | 2024/8/22    | 2024/8/28    |
| openGauss 6.0.0 LTS Test round 19 | 2024/8/29    | 2024/9/4     |
| openGauss 6.0.0 LTS Test round 20 | 2024/9/5     | 2024/9/11    |
| openGauss 6.0.0 LTS Test round 21 | 2024/9/12    | 2024/9/18    |
| openGauss 6.0.0 LTS Test round 22 | 2024/9/19    | 2024/9/25    |

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
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 6.0.0 LTS版本是openGauss社区继续联合多方力量推出的版本，本次发布的需求列表、分工如下：

| 序号 | 需求                                                         | 开发主体      | 测试主体 | 验证策略                                                     |
| ---- | ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 1    | 【openGauss 6.0.0 LTS】【工具链】openEuler 22.03支持默认集成openGauss 5.0.1 Lite版本 | Tools         | QA       | 1.验证yum安装卸载openGauss数据库，以及将openGauss-lite 2.1.0升级至openGauss-lite 5.0.1，兼容B库功能验证，yat连跑server仓支持的5.0.1用例<br/>2.资料介绍与实现功能一致 |
| 2    | 【openGauss 6.0.0 LTS】【工具链】Portal支持依赖包离线安装    | Tools         | QA       | 1.验证在管理员用户，普通用户下多个操作系统安装portal的功能   |
| 3    | 【openGauss 6.0.0 LTS】【工具链】DataKit迁移插件支持连接到数据库集群 | Tools         | QA       | 1.功能测试与需求相符，对于边界值等异常情况给出合理报错，执行结果符合预期 |
| 4    | 【openGauss 6.0.0 LTS】【工具链】DataKit支持指定表迁移       | Tools         | QA       | 1.指定表迁移，全量、增量、反向迁移成功，校验通过<br/>2.kill掉sink端进程后重启断点续传 |
| 5    | 【openGauss 6.0.0 LTS】【工具链】DataKit支持数据库集群安装易用性提升 | Tools         | QA       | 1.测试文件格式/后缀合法性检查，数据检查，用户信息测试，标签信息测试等 |
| 6    | 【openGauss 6.0.0 LTS】【工具链】DataKit支持对服务器统一导入管理 | Tools         | QA       | 1.测试功能，覆盖文件格式/后缀合法性检查，数据检查，用户信息测试，标签信息测试等<br/>2.资料测试覆盖校验资料的描述是否完整，可读性高 |
| 7    | 【openGauss 6.0.0 LTS】【工具链】DataKit支持对已安装的数据库集群导入管理 | Tools         | QA       | 1.cm、om集群导入，单机、主备导入，企业版、轻量版、极简版数据库导入<br/>2.资料介绍与实现功能一致 |
| 8    | 【openGauss 6.0.0 LTS】【工具链】录制回放工具支持SQL采集<br/>【openGauss 6.0.0 LTS】【工具链】录制回放工具支持流量回放或压测 | Tools         | QA       | 1.验证参数配置及基础功能、三种录制mysql的sql语句的功能、对tcp方式录制的网络文件的解析、对采集到的sql语句的进行回放等场景 |
| 9    | 【openGauss 6.0.0 LTS】【工具链】OM和CM支持安装管理IPV6的数据库集群 | Tools         | QA       | 1.全量功能测试，资料测试                                     |
| 10   | 【openGauss 6.0.0 LTS】【工具链】数据库安装流程解除对root用户的依赖 | Tools         | QA       | 1.普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，覆盖原有的root用户下安装流程，关注普通用户建立互信情况，以及不同句柄数、信号量资源情况下的安装部署现象<br/>2.覆盖arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7等操作系统下普通用户安装流程<br/>3.覆盖官网latest关于普通用户安装部署流程相关资料验证 |
| 11   | 【openGauss 6.0.0 LTS】【工具链】支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | Tools         | QA       | 1.测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE、IS [NOT] NAN条件表达式入参校验 2.测试TO_BINARY_FLOAT函数作为select列字段，结合where condition/ order by/ group by/ having/ join/ distinct/ 子查询/ 函数嵌套/ 嵌套自身/ 索引列等场景，并覆盖带default子句场景测试 3.测试IS [NOT] INFINITE和IS [NOT] NAN条件表达式作为select列字段/ where条件等场景测试 4.测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE和IS [NOT] NAN条件表达式，结合prepare语句、自定义函数、匿名函数、视图、导入导出及通过jdbc执行该函数等场景 5.测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE、IS [NOT] NAN条件表达式，带索引及不带索引场景下，对比A库执行10w次耗时少，验证其性能达标 |
| 12   | 【openGauss 6.0.0 LTS】【工具链】发布安全配置基线以及开源的安全巡检工具 | Tools         | QA       | 1.gs_checkse工具覆盖连接配置、文件目录安全、安全认证配置、账号口令管理、权限管理、数据库审计、错误报告和日志配置、备份配置、运行环境配置、其他配置的工具支持项扫描与修复 |
| 13   | 【openGauss 6.0.0 LTS】【资源池化】CM支持双集群中备集群首备和从备的switchover | StorageEngine | QA       | 1.基本切换功能验证，覆盖switchover所有支持参数 2.切换过程中进程故障测试 |
| 14   | 【openGauss 6.0.0 LTS】【资源池化】dsscmd支持连接到DSS Server中，直接执行读写文件的命令 | StorageEngine | QA       | 1.测试新增参数校验，已有cmd执行结果与非交互式保持一致； 2.进程故障，实例切换验证； |
| 15   | 【openGauss 6.0.0 LTS】【资源池化】CM支持选择实时回放节点，在主机故障时优先升主 | StorageEngine | QA       | 1.主要验证开启实时构建特性后参数是否能使CM选择指定的节点升主以及构造各种异常场景，CM能否正确的选择对应的节点升主 <br/>2.验证集群RTO性能是否有所提升 3.验证资料描述是否准确 |
| 16   | 【openGauss 6.0.0 LTS】【资源池化】按需回放redo阶段支持DDL回放 | StorageEngine | QA       | 1.验证redo阶执行单个ddl或者并行执行ddl，以及确认数据正确性和报错的合理性<br/> 2.验证在redo阶段执行ddl时注入故障，确认集群正常恢复及数据正确的测试 <br/>3.验证是否因为该特性代码导致RTO性能、redo时长劣化 <br/>4.资料描述与需求相符，实例是否正确无误 |
| 17   | 【openGauss 6.0.0 LTS】【资源池化】数据库内核日志分级，对关键流程补充日志打印，支持不开debug级别，支撑问题定位<br/>【openGauss 6.0.0 LTS】【资源池化】资源池化补充关键流程校验 | StorageEngine | QA       | 1.功能测试：开启业务之后，执行各种reform操作（重启、switchover、failover），reform过后数据库功能正常，dn日志有相对应的日志<br/>2.性能测试，性能下降幅度小于3%<br/>3.长稳测试，执行tpcc业务 |
| 18   | 【openGauss 6.0.0 LTS】【资源池化】资源池化支持扩缩容        | StorageEngine | QA       | 1.测试扩缩容功能以及集群节点变化后业务能力 <br/>2.硬盘故障、服务故障等可靠性测试 |
| 19   | 【openGauss 6.0.0 LTS】【内核】针对鲲鹏920 V200进行性能调优  | SQLEngine     | QA       | 1.根据调优文档对测试环境进行参数调优，之后对数据库进行1000仓861并发的tpcc性能测试 |
| 20   | 【openGauss 6.0.0 LTS】【内核】CM选主逻辑优化，多数派备机未回放完时，及时选出旧主 | SQLEngine     | QA       | 1.分别构造一主一备、一主两备、一主三备在旧主故障后又恢复和故障后未恢复的场景，观察是否可以快速选出新主，并且在选主完成后进行正常场景下的测试，且新主再次故障后，仍可以重新选出新主。cm选主过程中，通过注入磁盘满故障、修改实例权限、stop节点等构造主机故障 |
| 21   | 【openGauss 6.0.0 LTS】【内核】SyncRepLock机制重构           | SQLEngine     | QA       | 1.测试原issue场景SyncRepLock等待事件导致性能场景不再复<br/>2.SyncRepLock机制重构后同步配置异常场景处理与预期一致 <br/>3.性能对比性能出口标准不劣化 4.长稳测试无异常中断 |
| 22   | 【openGauss 6.0.0 LTS】【内核】支持全链路跟踪能力            | SQLEngine     | QA       | 1.测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE、IS [NOT] NAN条件表达式入参校验 <br/>2.驱动进程故障测试 |
| 23   | 【openGauss 6.0.0 LTS】【内核】支持慢SQL统计默认开启执行计划统计 | SQLEngine     | QA       | 1.配置track_stmt_stat_level='L0,OFF'，该配置为统计全量SQL，相应的查询计划可以被记录<br/>2.配置track_stmt_stat_level='OFF,L0'，该配置为统计慢SQL，组合相应的log_min_duration_statement边界值，大于边界值的查询计划可以被记录<br/>3.配置track_stmt_stat_level='OFF,L1'，该配置为统计慢SQL，组合相应的log_min_duration_statement边界值，大于边界值的查询计划可以被记录<br/>4.配合数据库安装方式，不同的兼容性数据库，是否中断，大于边界值的查询计划可以被记录 |
| 24   | 【openGauss 6.0.0 LTS】【内核】函数/PACKAGE支持指定并行相关参数 | SQLEngine     | QA       | 1.功能测试与需求相符，对于边界值等异常情况给出合理报错，执行结果符合预期 <br/>2.资料描述与需求基本相符，在涉及游标和函数的相关章节添加相关语法以及说 <br/>3.验证新增特性是否影响已有功能 |
| 25   | 【openGauss 6.0.0 LTS】【内核】支持管道函数                  | SQLEngine     | QA       | 1.主要包括顺序数组类型及嵌套表类型，通过pipe row返回集合类型，自制事务打开及关闭情况下执行DML操作，函数是否作为targetlist返回，通过create package方式创建pipelined函数，作为table子句查询，使用gs_dump工具进行导入导出及相关异常场景 <br/>2.验证pipelined函数返回大量数据时与wok_mem参数关系 <br/>3.验证升级确认前、升级后回滚、升级后提交创建pipelined函数 <br/>4.验证资料描述是否准确 |
| 26   | 【openGauss 6.0.0 LTS】【内核】支持游标嵌套                  | SQLEngine     | QA       | 1.功能测试与需求相符，对于边界值等异常情况给出合理报错，执行结果符合预期 <br/>2.资料描述与需求基本相符，在涉及游标的相关章节添加相关语法以及说明 <br/>3.验证新增特性是否影响已有功能 |
| 27   | 【openGauss 6.0.0 LTS】【内核】SELECT语句支持sample子句，实现数据采样 | SQLEngine     | QA       | 1.功能测试与需求相符，对于边界值等异常情况给出合理报错，执行结果符合预期 <br/>2.资料描述与需求基本相符，在select语句章节添加相关语法以及说明 |
| 28   | 【openGauss 6.0.0 LTS】【内核】数据备份需支持OSS             | SQLEngine     | QA       | 1.参数校验、s3远程备份恢复功能验证、远程操作s3上的备份集功能验证<br/>2.测试大数据量备份/恢复至s3所需的时间，并以三次测试结果的平均值作为基准数据，继续线性增大数据量，观察性能是否劣化<br/>3.备份恢复过程中注入磁盘满故障、s3服务故障 |
| 29   | 【openGauss 6.0.0 LTS】【内核】支持以f/d结尾作为浮点数；支持特殊的 'NaN' 和 'infinity'表示浮点数 | SQLEngine     | QA       | 1.通过手工执行‘f/F’或‘d/D’结尾的浮点数和特殊的‘NaN’和‘infinity’的基础识别、与函数结合使用、自定义函数、表达式等的结合场景的用例<br/>2.覆盖opengauss与A库的性能对比<br/>3.覆盖官网latest关于支持特殊浮点数相关流程，对支持特殊浮点数增加特性的描述相关资料验证 |
| 30   | 【openGauss 6.0.0 LTS】【内核】float精度支持范围调整为(1~126)；numeric精度设置允许precision大于scale，允许scale为负数 | SQLEngine     | QA       | 1.验证float、numeric的精度准确性、验证有效值、无效值以及边界值，验证在表、存储过程、包、自定义数据类型等数据库对象中的使用 <br/>2.验证在python、jdbc和odbc中的使用 <br/>3.验证导入导出是否成功并数据准确 <br/>4.从低版本升级到支持该特性的版本，验证功能是否正确 <br/>5.资料描述与需求相符，实力是否正确无误 |
| 31   | 【openGauss 6.0.0 LTS】【内核】使用用户态网络优化北向网络，TPCC性能提升15% | SQLEngine     | QA       | 1.对用户态guc参数enable_gazelle_performance值使用各种方式进行修改<br/>2.使用gsql和jdbc连接方式均可以连接用户态数据库<br/>3.在2p单机场景下执行tpcc性能测试，查看用户态是否比内核态性能提升超过15%<br/>4.在2p单机场景下执行长时间稳定性测试，查看用户态是否能够完成长稳测试<br/>5.在2p主备场景下执行tpcc性能测试，查看用户态参数是否会导致性能劣化<br/>6.多次停止数据库重启 |
| 32   | 【openGauss 6.0.0 LTS】【兼容性】MySQL兼容性增强             | Plugin        | QA       | 1.创建视图，视图引用表，直接删除表，删除成功，视图处于失效状态，查询报错。重新创建基表，视图再次可以被查询 <br/>2.支持straight_join语法，内连接中强制以左表驱动右表 <br/>3.interval支持表达式、支持绑定参数、支持列引用语法。返回结果无误<br/> 4.performance、shrink关键字由保留关键字降级至非保留关键字，关键字支持作为对象名 |
| 33   | 【openGauss 6.0.0 LTS】【兼容性】支持类似O*的SQL语句带外层PRIOR关键字的CONNECT BY语句 | Plugin        | QA       | 1.在start with ... connect by... 语句中使用Prior进行递归查询<br/>2.在connect by... 语句中使用Prior进行递归查询3.在where后使用Prior进行递归查询4.在group by后使用Prior进行分组查询5.在order by后使用Prior对查询结果进行排序6.对Prior关键字降级进行测试7.测试Prior关键字的递归查询性能 |
| 34   | 【openGauss 6.0.0 LTS】【兼容性】支持游标作为插入值          | Plugin        | QA       | 支持游标作为插入值按需求描述实现                             |
| 35   | 【openGauss 6.0.0 LTS】【兼容性】gms_output包特性            | Plugin        | QA       | 1.在兼容A库下创建、删除插件 <br/>2.接口函数基础功能及在复杂场景下的验证 <br/>3.在非兼容A库下执行用例，验证数据库是否稳定运行 <br/>4.从5.0.1升级到6.0.0之后执行用例验证功能是否正确 <br/>5.资料描述与需求相符，示例是否正确无误 |
| 36   | 【openGauss 6.0.0 LTS】【兼容性】使用游标创建type支持RETURN  | Plugin        | QA       | 1.支持定义type为REF CURSOR时使用RETURN按需求描述实现         |
| 37   | 【openGauss 6.0.0 LTS】【兼容性】MySQL 8.0协议兼容           | Plugin        | QA       | 1.M8.0客户端连接openGauss数据库，使用不同登录认证set_native_password及set_caching_sha2_password均连接成功且执行SQL正常 <br/>2.M8.0驱动连接openGauss数据库，使用不同登录认证set_native_password及set_caching_sha2_password均连接成功且执行SQL正常 <br/>3.Prepare协议及Execute协议相关功能测试 <br/>4.缓存用户密码测试 <br/>5.线程池管理工具使用M驱动连接 <br/>6.验证资料描述是否准确 <br/>7.执行业务过程中，openGauss数据库异常 |
| 38   | 【openGauss 6.0.0 LTS】【内核】支持 表名/列名 等标识符加双引号后也忽略大小写 | Plugin        | QA       | 1.打开开关enable_ignore_case_in_dquotes，测试标识符包含双引号的情况下也忽略大小写 <br/>2.新增参数enable_ignore_case_in_dquotes，性能测试 |
| 39   | 【openGauss 6.0.0 LTS】【兼容性】支持GMS_STATS包             | Plugin        | QA       | 1.在A模式下创建插件和删除 <br/>2.验证函数入参正常 <br/>3.验证各种场景下函数执行统计结束；包含同名表、临时表、同义词表、特殊字符表、表数量多、列存表、分区表、gis表、unlogged表、ustore表、压缩表、物化视图等 <br/>4.验证函数插入表同时并行执行统计命令的结果 <br/>5.验证异常值，错误入参等异常场景，报错准确（如不存在的schema以及函数名相同的schema、schema无访问权限、schema下部分表无访问权限的场景） <br/>6.验证兼容性场景测试，在B/C/PG库中分别执行用例 <br/>7.验证在动态语句/begin等场景使用是否正常 <br/>8.资料描述准确无误 |
| 40   | 【openGauss 6.0.0】【兼容性】select支持rotate/not rotate子句 | Plugin        | QA       | 1.验证只在兼容A库下可执行及其A库下的基本功能 <br/>2.验证各种不同的数据类型下进行单列/多列行列转换，表类型包含普通表、临时表、分区表、ustore表等；数据类型包含空值、特殊字符、中文等 <br/>3.验证聚合函数、子查询、并行查询中使用行列转换<br/>4.验证opengauss与A库性能相比无明显劣化 |
| 41   | 【openGauss 6.0.0】【兼容性】支持通过alter trigger方式启用禁用单个触发器 | Plugin        | QA       | 1.验证只在兼容A库下可执行及其A库下的基本功能 <br/>2.验证新语法对原有的触发器功能和系统表不受影响 <br/>3.并发场景下，验证多条语句插入时，触发器的触发情况；以及一条语句触发多个触发器情况下的可靠性验证 <br/>4.跨会话执行，验证触发器是否生效 |
| 42   | 【openGauss 6.0.0 】【兼容性】支持启用禁用tables约束测试报告 | Plugin        | QA       | 1.验证基本功能正常 <br/>2.验证日志信息以及资源统计、sql日志显示正常 <br/>3.长稳验证，验证长期在数据库对表约束增删改查操作，数据库无异常<br/>4.验证增加表约束后，对操作表的性能无明显影响 |
| 43   | 【openGauss 6.0.0】【兼容性】插件GMS_PROFILER的功能          | Plugin        | QA       | 1.验证函数入参、函数功能、存储过程中收集的信息等正常 <br/>2.验证异常值、错误入参等异常场景下报错准确 <br/>3.验证动态语句/begin等场景下正常使用 <br/>4.验证函数组合使用功能以及在其他高级包中使用正常 |
| 44   | 【openGauss 6.0.0】【兼容性】去除LargeObject 使用限制        | Plugin        | QA       | 1.验证大对象类型可读写，读写不超过范围                       |
| 45   | 【openGauss 6.0.0】【兼容性】支持array和record嵌套           | Plugin        | QA       | 1.验证array嵌套array时，赋值、取值、插入表数、打印输出正常，异常情况报错清晰 |
| 46   | 【openGauss 6.0.0】【兼容性】支持游标参数默认值              | Plugin        | QA       | 1.验证游标默认值生效，异常情况报错清晰                       |
| 47   | 【openGauss 6.0.0】【兼容性】支持用ROWTYPE给游标赋值         | Plugin        | QA       | 1.验证游标可使用rowtype类型赋值、取值、插入表数据、打印输出，异常情况报错清晰 |
| 48   | 【openGauss 6.0.0 LTS】【内核】金融版本：安全审计增强 <br/>【openGauss 6.0.0 LTS】【内核】审计日志支持完整性校验 <br/>【openGauss 6.0.0 LTS】【内核】CM支持对软链接目录空间管理 <br/>【openGauss 6.0.0 LTS】【内核】支持查询升级状态 | Plugin        | QA       | 1.测试不同场景下查询升级状态 <br/>2.软连接目录下磁盘达到阈值后的数据库状态 <br/>3.gs_dump,gs_dumpall,gs_restore,gs_basebackup,gs_probackup备份恢复工具命令执行并记录审计日志 <br/>4.数据库服务启动20次，均正确记录审计日志 <br/>5.审计日志新增两个参数，以及不同并发下的新老版本性能指标对比 |
| 49   | 【openGauss 6.0.0 LTS】【内核】walwriteraux线程支持预分配XLog文件 | SQLEngine     | QA       | 1.覆盖功能测试、可靠性测试和性能测试。                       |
| 50   | DataKit 业务开发- 消息窗口功能优化                           | Tools         | QA       | 测试SQL执行成功（插入/非插入）、执行错误的情况。执行成功时显示：开始执行时间、执行结束时间、执行时长、操作影响的记录行数 执行失败时显示：执行状态、错误代码、报错提示 每条SQL执行的消息之间间隔一行 |
| 51   | DataKit 业务开发- 对象窗口功能优化                           | Tools         | QA       | 无数据库连接时不可新建终端按钮及连接选择下拉框隐藏。有数据库连接时显示按钮及下拉框，下拉框列表显示已经建立的连接。 测试所有可新建终端的入口：新建终端按钮点击新建、左侧数据库名节点右键创建终端、终端Tab右键点击”新建终端“ |
| 52   | DataKit 业务开发- 执行窗口功能优化                           | Tools         | QA       | 测试插件各类窗口悬停显示，悬停标签分层级显示。 （1）数据库对象的显示格式为： ”数据库对象类型“（例如普通表/外表/视图）：xxx 模式：xxx 数据库：xxx 连接：xxx （2）终端窗口显示格式为： 终端：xxx 数据库：xxx 连接：xxx （3）其他类型的窗口显示格式为： "功能页面名称"（例如表空间/定时任务）：xxx 连接：xxx |
| 53   | DataKit 业务开发- 实例连接状态功能优化                       | Tools         | QA       | 测试连接成功及连接失败的情况。连接成功：提示成功及耗时 连接失败：提示对应的失败内容 (1)用户密码错误 (2)端口号错误 (3)ip错误 (4)数据库错误 |
| 54   | DataKit 业务开发- 新建连接功能优化                           | Tools         | QA       | 测试左侧数据库导航列表中一级节点的状态图标显示。存在至少一个数据库连接时显示绿色连接图标。断开所有库的连接显示红色未连接图标。连接超时状态不更新。 |
| 55   | 【国创】实例监控插件性能与稳定性优化                         | Tools         | QA       | 测试实例监控接口性能、资源占用。测试长期监控时是否有掉点。测试安装多个prometheus时，prometheus启动/停止后，采集时间段内的数据情况。测试代理或prometheus异常情况下及恢复后的数据采集 |
| 56   | 【国创】智能诊断插件SQL诊断功能支持Schema                    | Tools         | QA       | 测试Schema为public时的SQL诊断任务执行，测试 Schema为其他模式时的SQL诊断任务执行 |
| 57   | 【国创】智能诊断插件SQL诊断功能支持非ROOT用户                | Tools         | QA       | 测试使用ROOT用户安装/启动/停止/卸载，并执行SQL诊断任务。测试 使用非ROOT用户安装/启动/停止/卸载，并执行SQL诊断任务 |
| 58   | 【国创】支持慢SQL统计功能                                    | Tools         | QA       | 测试慢SQL统计列表、趋势图的查询/排序/统计数据结果正确性，测试慢SQL明细列表查询/排序/统计数据结果正确性 |
| 59   | 【国创】告警监控插件提供插件告警接口                         | Tools         | QA       | 测试插件告警规则的新建/编辑/查看/删除，测试插件告警模板的新建/编辑/查看/删除，测试插件告警配置，测试插件告警记录的查询/导出，测试插件告警通知 |
| 60   | 【国创】支持告警屏蔽                                         | Tools         | QA       | 测试告警屏蔽规则的 新建/编辑/查看/删除，测试告警屏蔽生效/失效。测试告警屏蔽生效时、若告警状态恢复，告警记录可正常结束。 |

# 版本概要测试结论

openGauss 6.0.0 LTS版本整体测试按照release-manager团队的计划，版本测试规划采取15+4+3的测试方式，即15轮系统测试+4轮集成验证+3轮回归测试的策略，实际完成了15轮系统测试+4轮集成验证+3轮回归测试：

Test round 1版本至Test round 7版本进行需求验收及问题单验收，主要涉及资源池化、兼容性的新特性验收，同时对继承测试开展第一轮的全量CI测试，覆盖可靠性、安全、性能、长稳、升级等测试。Test round 8版本至Test round 15版本评审工具链、内核特性测试设计，并对内核、工具链、兼容性、资源池化四个方面的新特性验收，同时开展第6轮的全量CI测试，重点覆盖性能、长稳、可靠性测试，同时在Test round 1至版本集中验收已修复的问题单。并对已验收完成特性测试报告进行评审。Test round 17版本至Test round 20版本开展4轮集成测试，覆盖工具链、兼容性、内核、资源池化等特性测试，涉及功能、可靠性、安全、性能、长稳、升级领域，累计执行测试用例6.9w+个，并覆盖两轮7*24H长稳测试，保证社区版本基础功能正常、稳定性良好，期间对每个特性的测试报告进行评审。Test round 21版本至Test round 22版本集中验收问题单，例行展开自动化测试，对关键指标、功能进行看护，防止修改引入新的问题。

openGauss 6.0.0 LTS版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、ustore场景、安全和资料等），所有测试任务均按计划完成。本版本计划交付需求64个，实际交付64个，交付率100%，所有发布需求均验证通过。

openGauss 6.0.0 LTS版本共发现有效问题1027个。修复问题回归测试结果正常，版本整体质量良好。遗留6个问题，见附件1遗留问题列表。

# 版本详细测试结论

openGauss 6.0.0 LTS版本详细测试内容包括：

1、通过自动化看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、系统性能、系统可靠性6个维度进行openGauss继承特性测试，继承功能无丢失；

2、在内核场景化的竞争力构建上，主要是对资源池化场景进行增强，如CM支持双集群中备集群首备和从备的switchover、CM支持选择实时回放节点，在主机故障时优先升主、资源池化支持扩缩容等......测试覆盖上述需求，关注功能的实现和规格的达成。但后续仍需从资源池化可靠性、竞争力的角度出发，持续进行加固测试和关键指标验证；

3、在工具链方面，覆盖Datakit继承特性、新特性测试，对DatiKit数据迁移、集群管理等易用性提升，同时portal支持依赖包离线安装。后续仍需对工具的易用性和稳定性提高要求。

4、在M\*兼容性方面，测试MySQL兼容性增强、MySQL 8.0协议兼容等需求，M\*兼容性进一步提升。并在A库兼容性做了提升，验证支持类似O*的SQL语句带外层PRIOR关键字的CONNECT BY语句、gms_output包特性、支持通过alter trigger方式启用禁用单个触发器等需求。

5、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试和sqlsmith测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

6、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试、ustore场景测试和资料测试。

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

| 特性名称                                                     | 测试情况说明                                                 | 约束                                                         | 质量电灯                                              | 遗留问题 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- | -------- |
| 【openGauss 6.0.0 LTS】【工具链】openEuler 22.03支持默认集成openGauss 5.0.1 Lite版本 | 通过yum安装卸载openGauss数据库，以及将openGauss-lite 2.1.0升级至openGauss-lite 5.0.1，兼容B库功能验证。包含功能测试、资料测试，输出测试用例12个，执行测试共2轮，发现issue共2个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】Portal支持依赖包离线安装    | 验证在管理员用户，普通用户下多个操作系统安装portal的功能，整体质量良好，issue0个 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】DataKit迁移插件支持连接到数据库集群 | DataKit迁移插件支持连接到数据库集群需求测试累计发现4个缺陷，已验收完成，总体质量良好。 | 1.要求设置openGauss的guc参数`enable_slot_log=on`，以支持主备数据之间之间逻辑复制槽同步。否则可能会出现历史主节点数据因数据量过大，导致主节点发生切换前，未全部反向迁移，新的主节点启动后，原主节点未来得及迁移的数据丢失的情况<br/>2.由于需要保证主备切换时主备间逻辑复制槽的一致性，需要配置debezium的`opengauss-source.properties`配置文件中`slot.drop.on.stop=false`，此参数用于控制重连时是否删除上一次连接时的逻辑复制槽，默认为true。当然使用datakit数据迁移时无需手动配置此参数，datakit会自动配置。但是使用portal和debezium进行迁移时，需要手动配置。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】DataKit支持指定表迁移       | 全量迁移指定表的迁移成功，校验通过。增量迁移对指定表增删改数据、更改表结构迁移至目标端成功，增量校验通过。反向迁移对指定的表增删改数据迁移至目标端成功。包含功能、可靠性、性能和资料测试，输出测试用例35个，执行测试共2轮，发现issue共7个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】DataKit支持数据库集群安装易用性提升 | 增量迁移工具能力增强，共执行用例18个，主要覆盖了功能测试及资料测试。功能测试主要覆盖文件格式/后缀合法性检查，数据检查，用户信息测试，标签信息测试等。资料测试覆盖校验资料的描述是否完整，可读性高。累计发现缺陷单3个，3个缺陷已解决，回归通过，整体质量良好。 | 1.在java11环境上运行Datakit<br/>2.文件后缀格式仅支持excel    | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】DataKit支持对服务器统一导入管理 | 服务器管理易用性增强，共执行用例18个，主要覆盖了功能测试及资料测试。功能测试主要覆盖文件格式/后缀合法性检查，数据检查，用户信息测试，标签信息测试等。资料测试覆盖校验资料的描述是否完整，可读性高。累计发现缺陷单3个，3个缺陷已解决，回归通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】DataKit支持对已安装的数据库集群导入管理 | 过datakit对已安装的企业版、轻量版、极简版数据单机、主备通过模板文件批量导入。om、cm集群导入。包含功能测试、资料测试，输出测试用例18个，执行测试共2轮，发现issue共6个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】录制回放工具支持SQL采集<br/>【openGauss 6.0.0 LTS】【工具链】录制回放工具支持流量回放或压测 | 需求共计执行32个用例，主要覆盖了参数配置及基础功能、三种录制mysql的sql语句的功能、对tcp方式录制的网络文件的解析、对采集到的sql语句的进行回放等场景，共发现8个问题单，7个缺陷单，1个资料单，整体质量良好。 | 1.各场景测试结果中的处理时间，指的是openGauss的处理时间，不包含其他第三方业务的处理时间<br/>2.性能测试是在实验室网络环境下，搭建测试环境并进行的性能测试，网络条件较稳定，不考虑现网与实验室的网络条件差异<br/>3.使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力<br/>4.本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】OM和CM支持安装管理IPV6的数据库集群 | 覆盖功能测试、资料测试，需求功能实现，累计执行用例54个，用例执行率100%，发现问题9个，开发质量良好 | 1.目标机器需要支持ipv6通信，能够使用ssh+ipv6地址连接         | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】数据库安装流程解除对root用户的依赖 | 主要覆盖arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7操作系统下普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，覆盖原有的root用户下安装流程，包含功能、兼容性和资料测试等，共计执行28个用例，发现问题8个，回归通过6个，2个问题正在解决，整体质量良好。 | 1.文件句柄数不满足最小值64w的情况下，带CM的数据库无法安装，只能报错提示用户，仍需要依赖root权限设置文件句柄数<br/>2.系统可用信号量不满足10000，数据库无法安装成功，仍依赖root进行信号量参数设置<br/>3.gs_postuninstall仍需要root用户权限执行<br/>4.自定义定时任务保活ssh进程未实现，如果服务器发生重启等情况，互信会丢失，需要手动执行Python3 /（数据库安装目录）/tool/script/local/CheckSshAgent.py脚本重新建立互信<br/>5.普通用户安装的数据库，gs_guc不支持-N，-I参数 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | 覆盖功能测试和性能测试，输出测试用例79个，执行测试共3轮，发现issue共6个，包含2个资料单，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【工具链】发布安全配置基线以及开源的安全巡检工具 | gs_checkse对工具支持的安全项进行检查。包含功能测试、资料测试，输出测试用例80个，执行测试共2轮，发现issue共9个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】CM支持双集群中备集群首备和从备的switchover | 测试资源池化双集群（含存储复制和网络复制）备集群内CM支持首备和从备的switchover切换操作，以及主备集群间切换之后新备集群内首备和从备的switchover，包含支持参数的基本功能、可靠性场景以及资料测试，输出测试用例15个，执行测试6轮，发现issue共13个，部分问题已修复并回归通过，存在少量待解决问题，整体开发质量一般。 | 资源池化双集群                                               | <font color=yellow><font color=yellow>▲</font></font> | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】dsscmd支持连接到DSS Server中，直接执行读写文件的命令 | 主要测试dsscmd工具交互式运行，测试范围包括只支持交互式输入命令，上下键翻取历史命令，退格键删除字符以及支持cd、pwd命令，包含功能、可靠性和资料测试，输出测试用例13个，执行测试共2轮，发现issue共2个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】CM支持选择实时回放节点，在主机故障时优先升主 | 共执行用例25条，覆盖功能测试、性能测试、可靠性测试及资料文档验证，测试执行覆盖率达到100%，共发现问题3条，，缺陷密度为18.75kloc，已全部解决，回归测试结果正常，无遗留问题，开发质量一般。 | 1.资源池化集群<br/>2.集群中存在节点正常且开启了实时构建特性<br/>3.GUC参数：ss_enable_ondemand_realtime_build | <font color=yellow><font color=yellow>▲</font></font> | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】按需回放redo阶段支持DDL回放 | 测试共执行用例34条，主要覆盖ddl语法功能测试、redo阶段故障测试、性能测试以及资料测试。功能测试覆盖度支持ddl和不支持ddl执行，确认数据正确性以及报错合理性的验证，通过；以及在redo阶段执行ddl后故障测试，验证集群是否可以正常恢复，测试通过；性能测试保证RTO性能无劣化、redo回放时长不能比旧版本劣化，验证通过；资料的准确性测试，通过。测试过程中发现问题2个，无遗留风险，整体质量良好。 | 1.依赖于已有的按需回放、实时构建特性，因此以上两个特性的约束适用于本特性<br/>2.本特性适配库、模式、表、索引、视图、存储过程的create、drop、alter<br/>3.本特性适配redo阶段执行存储过程，存储过程的语法必须在redo阶段支持语法范围内<br/>4.本特性需要在执行DDl期间完成检索该表所有未回放日志，并对所有日志进行回放或清理，可能存在DDL执行时间较长的情况 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】数据库内核日志分级，对关键流程补充日志打印，支持不开debug级别，支撑问题定位<br/>【openGauss 6.0.0 LTS】【资源池化】资源池化补充关键流程校验 | 覆盖功能测试、长稳测试及性能测试，开启业务之后，执行各种reform操作，reform过后数据库功能正常，dn日志有相对应的日志，累计执行78个用例，发现问题2个，均验收通过。 | 1.本特性仅支持在资源池化部署下使用<br/>2.不支持双集群        | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【资源池化】资源池化支持扩缩容        | 测试资源池化扩缩容能力、集群节点发生变化后对外提供服务的能力，包含功能、可靠性和资料测试输出测试用例78个，执行测试共3轮，发现issue共8个，一个问题修复中，剩余问题均以验收通过，整体开发质量一般。 | 无                                                           | <font color=yellow>▲</font>                           | 无       |
| 【openGauss 6.0.0 LTS】【内核】针对鲲鹏920 V200进行性能调优  | 共计执行2个用例，主要覆盖了6.0.0及release版本的tpcc性能测试，性能均达到标准230w，共发现0个问题单，整体质量良好。 | 1.本报告中的各场景测试结果中的处理时间，指的是openGauss的处理时间，不包含其他第三方业务的处理时间<br/>2.本次性能测试是在实验室网络环境下，搭建测试环境并进行的性能测试，网络条件较稳定，不考虑现网与实验室的网络条件差异<br/>3.本报告中，使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力<br/>4.本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】CM选主逻辑优化，多数派备机未回放完时，及时选出旧主 | 主要测试在主机发生故障时，如果备机始终回放无法完成，一直无法选出主节点，而此时主机已经恢复，可以及时让旧主重新归位，再次作为主节点，从而达到优化选主逻辑，降低不必要的等待备机回放时间。测试包括功能测试，测试执行共2轮，未发现issue，无遗留问题，整体质量良好。 | 继承cm选主原有的约束条件                                     | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】SyncRepLock机制重构           | SyncRepLock机制重构后同步配置异常场景处理与预期一致，性能不劣化及常稳测试无异常中断等场景的测试，共执行用例5个，需求未引入问题单，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】金融版本：安全审计增强<br/>【openGauss 6.0.0 LTS】【内核】审计日志支持完整性校验<br/>【openGauss 6.0.0 LTS】【内核】CM支持对软链接目录空间管理<br/>【openGauss 6.0.0 LTS】【内核】支持查询升级状态 | 本次特性测试支持升级状态查询，审计日志支持完整性校验，备份和恢复⼯具审计⽇志，修复数据库主节点审计日志丢失bug，软连接管理，包含功能测试和技术测试，输出测试用例148个，执行测试共2轮，发现issue共2个，1个未修复待修改后回归，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持全链路跟踪能力            | 测试SQL语句从jdbc驱动到数据库服务端的网络耗时，并记录在数据库unique_sql(dbe_perf.statement视图)、慢sql视图（statement_history视图）和WDR报告中，形成全链路时间打印能力，包含功能、可靠性、兼容性、升级、稳定性、性能和资料测试，以及jdbc端不同接口，输出测试用例26个，执行测试共3轮，发现issue共2个，均已修复并回归通过，无遗留问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持慢SQL统计默认开启执行计划统计 | 支持慢SQL统计默认开启执行计划统计特性，共计执行测试用例35个，主要覆盖功能测试，资料测试和性能测试。测试执行覆盖率达到100%。在功能测试中，通过手动执行用例，发现一个问题，经与开发沟通为查询走SQL bypass逻辑时没有相关执行计划，结论为非问题。在资料测试中，相关资料均已补充完整。在性能测试中，在开启统计慢SQL相关配置后，整体性能下降在5%以内，符合预期，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】函数/PACKAGE支持指定并行相关参数 | 覆盖功能测试和资料测试和兼容性测试。执行已有用例150个，测试新增功能不影响已有特性。新增27个用例，主要在函数的创建，调用，以及工具和系统表的适配方面进行测试，通过手工执行单个游标入参，多个游标入参，嵌套游标入参场景的用例，发现问题3个，目前均已回归，无遗留风险，整体质量良好。 | 1.smp总开关打开：set query_dop >1<br/>2.游标中的查询支持smp计划<br/>3.cmd中游标声明为no scroll | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持管道函数                  | 共执行用例30条，覆盖功能测试、性能测试、兼容性测试及资料文档验证，测试执行覆盖率达到100%，共发现问题3条，，缺陷密度为1.1个/kloc，已全部解决，回归测试结果正常，无遗留问题，整体质量良好。 | 1.在openGauss侧兼容A库下验证<br/>2.管道函数返回类型必须是schema级或package内定义的集合类型<br/>3.管道函数功能实现与openGauss原生return next、query类似，故返回大量数据时函数性能较差且受work_mem参数影响<br/>4.不支持返回嵌套嵌套表类型，及以end func_name结尾创建pipelined函数<br/>5.openGauss侧目前TableOf实现是数组形式，因此管道函数作为targetlist返回时，返回结果虽为TableOf类型，但实际访问能力缺失 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持游标嵌套                  | 覆盖功能测试和资料测试和兼容性测试。执行已有用例197个，测试新增功能不影响已有特性。新增18个用例，主要在存储过程，匿名块，事务，函数中使用嵌套游标，通过手工执行用例，发现问题2个，已解决且已经回归通过，无遗留风险，整体质量良好。 | 1.该功能在openGauss的A兼容模式下使用<br/>2.支持在存储过程中定义和访问嵌套游标<br/>3.支持在事务中定义嵌套游标 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】SELECT语句支持sample子句，实现数据采样 | 共计执行62个用例，主要覆盖功能测试和资料测试和接口测试，通过手工执行用例，发现问题1个，已解决且已经回归通过，无遗留风险，整体质量良好。 | 1.该功能在openGauss的A兼容模式下使用<br/>2.原有常规表（包括行存、列存和ustore表）接受sample子句的功能不受影响，新增对物化视图、键保留视图和分区表的支持<br/>3.采样优先于其他过滤条件 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】数据备份需支持OSS             | 覆盖测试gs_probackup工具支持将备份的数据直接传输到远端，不落本地盘，同时也是openGauss高可用竞争力特性的补齐。本次测试包括功能、性能、可靠性和资料测试，输出用例37个，执行测试共2轮，发现issue共2个，均已修复并回归通过，无遗留问题，整体质量良好。 | 1.gs_probackup原有的约束条件<br/>2.S3对象存储服务本身不支持文件编辑功能，对已上传文件的修改需要本地编辑后重新上传，因此对于merge等涉及文件编辑操作的子命令可能存在性能和计费方面的问题<br/>3.远程备份到S3目前支持的子命令：add-instance、backup、restore、delete以及查看备份集信息和配置的子命令 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持以f/d结尾作为浮点数；支持特殊的 'NaN' 和 'infinity'表示浮点数 | 通过支持以‘f/F’或‘d/D’结尾的浮点数以及支持特殊的‘NaN’和‘infinity’表示浮点数功能特性，提升openGaussA库兼容性，共计执行67个用例，主要在函数的创建，调用方面进行测试，通过手工执行‘f/F’或‘d/D’结尾的浮点数和特殊的‘NaN’和‘infinity’的基础识别、与函数结合使用、自定义函数、表达式等的结合场景的用例，发现问题2个，无遗留风险，整体质量良好。 | 1.支持以f/d结尾作为浮点数功能需要打开参数float_suffix_acceptance<br/>2.支持以f/d结尾作为浮点数功能需要打开参数enable_binary_special_o_format<br/>3.支持以f/d结尾作为浮点数功能需要在A兼容库下进行 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】float精度支持范围调整为(1~126)；numeric精度设置允许precision大于scale，允许scale为负数 | 覆盖功能测试、接口测试、导入导出、升级测试以及资料测试。功能测试覆盖创建、删除、修改数据类型以及对存储数据的增删改查，确认数据类型存储打印精度正确性的验证，通过；以及再python、jdbc、odbc等接口中的表现验证，通过；导入导出成功与数据准确性的验证，通过；升级提交前和提交后的使用验证以及资料的准确性测试，通过。测试过程中发现问题2个，资料问题1个，无遗留风险，整体质量良好。 | 1.限定在A兼容性数据库下使用<br/>2.float精度拓展必须打开float_as_numeric<br/>3.numeric的scale范围设定为-84~1000 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】使用用户态网络优化北向网络，TPCC性能提升15% | 共计执行6个用例，主要覆盖了参数功能正常、gsql和jdbc连接数据库、tpcc性能提升、长时间稳定性、主备性能测试等场景，共发现4个问题单，整体质量良好。 | 1.本报告中的各场景测试结果中的处理时间，指的是openGauss的处理时间，不包含其他第三方业务的处理时间<br/>2.本次性能测试是在实验室网络环境下，搭建测试环境并进行的性能测试，网络条件较稳定，不考虑现网与实验室的网络条件差异<br/>3.本报告中，使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力<br/>4.本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】MySQL兼容性增强             | 共计执行116个用例，主要覆盖功能测试、兼容性测试、特性耦合测试、资料测试，测试执行覆盖率达到100%，通过手工执行用例，发现问题5个，5个已验收通过，整体质量良好。 | 1.该功能在openGauss的B兼容模式下使用<br/>2.视图依赖的表删除后，使用\d查询视图和desc查询结果保持一致<br/>3.修改基表表名或列名，查询视图成功，M查询视图报错。此处不完全兼容<br/>4.特性1删除模式为restrict（缺省模式） | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】支持类似O*的SQL语句带外层PRIOR关键字的CONNECT BY语句 | 目标列支持Prior表达式特性，共计执行10个用例，主要覆盖功能测试和资料测试，在功能测试中，通过手动执行用例，发现一个问题，经与开发沟通数据库原始设计导致，结论为非问题。该部分手动用例均以自动化，已上传至仓库中。在资料测试中，发现一个问题。回归通过，无遗留风险，整体质量良好； | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】支持游标作为插入值          | 共计执行55个用例，主要覆盖游标作为插入值时的source表和target表的操作，测试source表提供数据集时的操作，包括但不限于表达式、操作符、join、带函数等，测试target表插入数据后的完整性，包括但不限于结合变量、触发器、存储过程、函数、匿名块、事务等。结合常用测试场景，通过手工执行用例，发现问题0个，无遗留风险，整体质量良好。 | 1.该功能在openGauss的A兼容模式下使用<br/>2.游标作为插入值需解析变量的具体类型时，需要设置参数behavior_compat_options='allow_procedure_compile_check'。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】gms_output包特性            | 覆盖了功能测试、约束测试、升级测试和资料测试。功能测试覆盖了插件的创建、删除，插件包的5个接口函数功能，以及接口函数在不同场景下的验证；约束测试验证了功能测试用例在非兼容"A"库下执行数据库可稳定运行；升级测试在低版本升级到6.0.0版本后执行用例验证此功能的正确性；资料测试验证手册是否完善、有效。测试中未发现问题。 | 1.兼容A gms_output包功能,需在openGauss兼容A库下安装gms_output插件验证 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】使用游标创建type支持RETURN  | 共计执行43个用例，主要覆盖定义type为REF CURSOR时带return定义及不带return语法的覆盖，测试游标定义时返回类型为record类型、rowtype类型，子属性包括但不限于数值类型、字符类型、时间类型等，rowtype基表包括但不限于astore表、ustore表、cstore表、普通视图、物化视图等。结合常用测试场景，如使用bulk collect into传递游标值，跨包调用类型等场景，通过手工执行用例，发现问题0个，无遗留风险，整体质量良好。 | 1.该功能在openGauss的A兼容模式下使用<br/> 2.强游标实际作为弱游标实现。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】MySQL 8.0协议兼容           | 共计执行73个用例，主要覆盖功能测试、兼容性测试、资料测试，测试执行覆盖率达到100%，通过手工执行用例，发现问题4个，2个已验收通过，剩余2个待修复，整体质量良好。 | 1.该功能在openGauss的B兼容模式下使用<br/>2.不支持游标<br/>3.M* JDBC driver： 8.0.28及其以下<br/>4.M* 命令行客户端： 8.0.28及其以下 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】支持 表名/列名 等标识符加双引号后也忽略大小写 | 共执行118个用例，主要覆盖功能测试，发现3个主要缺陷，1个次要缺陷。发现问题已解决，回归通过，无遗留风险，整体质量良好。该特性新增数据库参数enable_ignore_case_in_dquotes，已验证对性能几乎无影响。 | 1.PL中对标识符的引用不受开关影响，包括游标名、变量名、数组名、类型名、集合名 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【兼容性】支持GMS_STATS包             | 共计执行23个用例，主要覆盖了功能测试、兼容性测试和资料测试，发现1个问题，整体质量良好。 | 1.删除扩展时需要设定参数support_extended_features = on<br/>2.不统计空表<br/>3.不支持收集临时表信息<br/>4.仅支持A模式库 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 】【兼容性】select支持rotate/not rotate子句 | select支持rotate/not rotate子句特性，共计执行26个用例，主要覆盖了功能测试、性能测试和资料测试，发现1个问题，遗留风险0个，整体质量良好；本次测试主要覆盖各种sql场景下，使用rotate/not rotate子句的方式进行行转列、列转行功能验证、异常场景下的合理报错、复杂场景的正常使用，发现issue1个，已验收通过，整体质量良好； | 1.仅支持数据库模式为A模式                                    | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 】【兼容性】支持通过alter trigger方式启用禁用单个触发器 | alter trigger方式启用禁用单个触发器特性，共计执行31个用例，主要覆盖了功能测试、语法测试、耦合测试、可靠性测试和资料测试，本次测试主要覆通过alter trigger的方式的启停功能验证、异常场景下的合理报错、以及不影响原有功能交互。测试未发现问题，无遗留风险，整体质量良好； | 1.仅支持数据库模式为A模式                                    | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 】【兼容性】支持启用禁用tables约束测试     | 共计执行46个用例，主要覆盖了功能测试、性能测试、安全测试、资料测试，稳定性测试；发现issue2个，整体质量良好；测试中发现2个问题，都已验收通过，整体质量良好。 | 1.enable/disable novalidate对primary key，unique不生效       | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 】【兼容性】插件GMS_PROFILER的功能         | 共计执行32个用例，主要覆盖了功能测试、资料测试，发现issue1个，已验收通过，整体质量良好； | 1.仅支持Create extension命令方式加载插件<br/>2.不支持存储过程中存在异常处理的场景，会导致收集信息不准确<br/>3.如果测试过程调用了flush_data接口，不支持其后调用ROOLBACK操作，会报错。如需使用ROOLBACK, 建议统一通过stop_profiler接口完成收集信息写表 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0】【兼容性】去除LargeObject 使用限制        | opengauss解除大对象使用限制，共计执行85个用例，支持了大对象功能使用，完成了性能测试，完成了接口测试，完成了兼容性测试，完成了可靠性测试。功能问题1个，资料问题4个，问题均已解决，回归验收通过，无遗留风险，整体质量良好； | 1.限制只有超级用户才能使用lo_import和lo_export               | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0】【兼容性】支持array和record嵌套           | 本需求在测试阶段完成了功能测试、可靠性测试、可服务性测试、资料测试、特性耦合分析测试。共执行26个测试用例，发现功能问题2个，问题均已解决，回归验收通过，无遗留风险，整体质量良好； | 1.在A库之外或在分布式数据库或匿名块中，不允许声明array的元素类型为record，也不允许record和record嵌套。 嵌套深度上限为6。 record类型成员不允许为嵌套array类型。 不支持向嵌套数组的一维数组赋值。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0】【兼容性】支持游标参数默认值              | 本需求在测试阶段完成了功能测试、性能测试、可靠性、可测试性测试、可服务性测试，共计执行96个用例。未发现功能问题和资料问题，无遗留风险，整体质量良好； | 1.需要按顺序先声明没有默认值的参数，再声明有默认值的参数。先于最后一个无默认值的参数声明的参数的默认值无效。例如在如下所示的例子中，游标c的参数arg1的默认值无效，只有arg3的默认值会生效。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0】【兼容性】支持用ROWTYPE给游标赋值         | 本需求在测试阶段完成了功能测试、可靠性测试、可服务性测试、资料测试、特性耦合分析测试。共执行49个测试用例，发现功能问题8个，无资料问题，问题均已解决，回归验收通过，无遗留风险，整体质量良好； | 1.ROWTYPE不支持引用复合类型或RECORD类型变量的类型、跨PACKAGE cursor的类型。 PL对象受预编译参数控制，例如 **set** behavior_compat_options='allow_procedure_compile_check'; 开启预编译开关时可以检测游标所涉及的表是否存在、表中是否存在对应列，能够在创建函数、存储过程、包时抛出错误。 关闭预编译开关时无论游标所涉及的表、或者表中的列是否存在，都可以成功创建函数、存储过程、包。 目前未考虑在PL执行过程中修改表的情况，将在后续提出其他需求进一步完善。 嵌套游标不支持使用%ROWTYPE。 | <font color=green>▮</font>                            | 无       |
| 【openGauss 6.0.0 LTS】【内核】walwriteraux线程支持预分配XLog文件 | 支持根据阈值预分配xlog文件共计执行14个用例，主要覆盖功能测试、可靠性测试和性能测试，通过手工执行用例，发现问题1个，已修复待验证，无遗留风险，整体质量良好。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| DataKit 业务开发- 实例连接状态功能优化                       | 测试连接状态显示，执行4个用例，未发现问题。                  | 当用户长时间未操作，连接超时后，状态不会自动更新，需用户操作发起请求后才会更新状态 | <font color=green>▮</font>                            | 无       |
| DataKit 业务开发- 新建连接功能优化                           | 测试新建编辑连接时，进行测试连接操作。执行3个用例，未发现问题。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| DataKit 业务开发- 对象窗口功能优化                           | 测试查看标签名称显示。执行1个用例，未发现问题。              | 无                                                           | <font color=green>▮</font>                            | 无       |
| DataKit 业务开发- 消息窗口功能优化                           | 测试SQL执行信息、历史执行SQL的错误信息。执行5个用例，未发现问题。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| DataKit 业务开发- 执行窗口功能优化                           | 测试已打开的SQL终端窗口右键菜单增加新建SQL终端，测试无连接时无创建SQL终端按钮。执行3个用例，未发现问题。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【国创】实例监控插件性能与稳定性优化                         | 测试采集数据稳定性。 执行5个用例，发现2个问题，遗留1个问题。 | 无                                                           | <font color=yellow>▲</font>                           | 无       |
| 【国创】智能诊断插件SQL诊断功能支持Schema                    | 测试选择不同Schema进行诊断。执行5个用例，发现1个问题，问题已修复。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【国创】智能诊断插件SQL诊断功能支持非ROOT用户                | 测试使用不同用户安装诊断Agent并执行SQL诊断任务。执行6个用例，未发现问题。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【国创】支持慢SQL统计功能                                    | 测试慢SQL统计列表及趋势图。执行14个用例，发现1个问题，问题已修复。 | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【国创】告警监控插件提供插件告警接口                         | 测试插件告警与实例告警。执行7个用例，未发现问题。            | 无                                                           | <font color=green>▮</font>                            | 无       |
| 【国创】支持告警屏蔽                                         | 测试按实例屏蔽实例告警。执行15个用例，发现1个问题，问题已修复。 | 只支持屏蔽实例的告警，不支持屏蔽插件告警。                   | <font color=green>▮</font>                            | 无       |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 专项测试结论

### 安全测试

openGauss 6.0.0 LTS版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库越权风险管理、数据库注入攻击、数据库秘钥管理、数据库辅助工具安全、数据库攻击及审计、数据库内存问题、数据库安全框架设计问题和数据库操作系统提权8大方向进行安全测试。

openGauss 6.0.0 LTS版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 可靠性\稳定性测试

openGauss 6.0.0 LTS版本可靠性\稳定性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及3个长时间负载测试。

1、故障注入类测试：共计测试6个版本，在x86+Centos，x86openEuler，Kylin环境下共计执行707个用例，测试用例累计执行率100%，测试发现3个问题（每个版本问题单已去重），已优化闭环2个，遗留1个（IAO53D极低概率偶现，经评审遗留至7.0.0解决），整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：共计测试9个版本，共执行3个用例，测试用例累计执行率100%，未发现问题，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | astore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试，包括虚拟机、物理机测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景2  | ustore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试，包括虚拟机、物理机测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景3  | ustore场景，极致RTO、普通回放和并行回放测试，7*24H测试       | 业务正常运行，主备机无core                                   |

### 性能测试

对关键性能指标进行摸底和测试，主要覆盖了astore、ustore和资源池化场景下，2P单机/主备、4P单机的TPC-C测试及RTO测试，性能多轮测试稳定。

| **指标大项**      | **指标小项**                                   | **指标值** | **说明**          | 测试结论                                                     |
| ----------------- | ---------------------------------------------- | ---------- | ----------------- | ------------------------------------------------------------ |
| TPCC              | 2P（Taishan 200 2280 7260）单节点 1H           | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***156.9***W+                  |
|                   | 2P（Taishan 200 2280 7260）单节点 8H           | 145万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为***152.0***W+                  |
|                   | 2P（Taishan 200 2280 7260）一主一备 1H         | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***135.4***W+                |
|                   | 2P（Taishan 200 2280 7260）一主一备 8H         | 130万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为***134.3***W+                |
|                   | 4P（Taishan 200 2480 7260）单节点 1H           | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***233.3***W+                    |
|                   | 2P (鲲鹏920 V200 320 core ) 单节点 1H          | 230万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***239.3***W+                  |
| ustore TPCC       | 2P（Taishan 200 2280 7260）单节点 1H           |            | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***152.6***W+                  |
|                   | 2P（Taishan 200 2280 7260）一主一备 1H         |            | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***131.7***W+                |
|                   | 4P（Taishan 200 2480 7260）单节点 1H           |            | 此即为release基线 | 单节点在极限场景配置下tpmC为***226.0***W+                    |
| RTO高可用         | 一主一备failover                               | 小于10s    | 此即为release基线 | **3**s                                                       |
| 资源池化sysbench  | 2P（Taishan 200 2280 7260）主机读写备机空载    |            |                   | 主机读写备机空载场景下，主机使用oltp_read_write，性能为***4.5w tps***。 |
|                   | 2P（Taishan 200 2280 7260）主机读写备机只读    |            |                   | 主机读写备机只读场景下，主机使用oltp_read_write，备机使用oltp_read_only，总计***1.9w tps***。（IAARRQ） |
| 资源池化TPCC      | 2P（Taishan 200 2280 7260）主机读写备机空载 1H | 120万      | 此即为release基线 | 主机读写备机空载 1H在极限场景配置下tpmC为***120.3***W+       |
|                   | 2P（Taishan 200 2280 7260）主机读写备机只读 1H | 220万      | 此即为release基线 | 主机读写备机只读 1H在极限场景配置下tpmC为***232.8***W+       |
| 资源池化RTO高可用 | 单集群                                         | 小于10s    |                   | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库主备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br/>单集群故障前25wtpmC，30并发场景下，RTO恢复时间为***8s***，达到标准<10s。故障前50wtpmC，100并发场景下，RTO恢复时间为***29s***，达到标准<30s。（IAQ2C8） |

### 兼容性测试

#### 升级兼容

针对openGauss 6.0.0 LTS版本，规划的升级路径如下表所示。测试整体情况：

1. openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1升级到openGauss 6.0.0 成功，升级失败或者升级未提交，可以成功回滚；
3. 升级成功后，特性功能运行正常

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM灰度升级到6.0.0带CM版本 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM灰度升级到6.0.0带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1带CM灰度升级到6.0.0带CM版本 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1带CM灰度升级到6.0.0带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM灰度升级到6.0.0不带CM版本 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM灰度升级到6.0.0不带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM指定节点升级到6.0.0带CM版本 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM指定节点升级到6.0.0带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1带CM指定节点升级到6.0.0带CM版本 | IAR6T9   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1带CM指定节点升级到6.0.0带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM指定节点升级到6.0.0不带CM版本 | 测试通过 |
| openGauss 2.0.5/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1不带CM指定节点升级到6.0.0不带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1容器化升级到6.0.0版本 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2/openGauss 5.0.3/openGauss 6.0.0 RC1容器化升级到6.0.0版本，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3不带CM版本，再灰度升级到openGauss 6.0.0不带CM版本 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3不带CM版本，再灰度升级到openGauss 6.0.0不带CM版本，回滚openGauss 5.0.2/openGauss 5.0.3版本，再升级 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3不带CM版本，再灰度升级到openGauss 6.0.0带CM版本 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3不带CM版本，再灰度升级到openGauss 6.0.0带CM版本，回滚openGauss 5.0.2/openGauss 5.0.3版本，再升级 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3带CM版本，再灰度升级到openGauss 6.0.0带CM版本 | 测试通过 |
| openGauss 3.0.5不带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3带CM版本，再灰度升级到openGauss 6.0.0带CM版本，回滚openGauss 5.0.2/openGauss 5.0.3版本，再升级 | 测试通过 |
| openGauss 3.0.5带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3带CM版本，再灰度升级到openGauss 6.0.0带CM版本 | 测试通过 |
| openGauss 3.0.5带CM灰度升级到openGauss 5.0.2/openGauss 5.0.3带CM版本，再灰度升级到openGauss 6.0.0带CM版本，回滚openGauss 5.0.2/openGauss 5.0.3版本，再升级 | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 6.0.0 LTS数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 6.0.0 LTS数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论    |
| ------------ | ------------------------------------------------------------ | ----------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 6.0.0 LTS数据库 | 测试   通过 |

### ustore测试

| Domain            | 测试活动                                                     | 测试结论   |
| ----------------- | ------------------------------------------------------------ | ---------- |
| 功能测试          | 功能CI连跑两轮；高斯用例自动化GRT工具连跑一轮；重点测试闪回功能；配置默认建表类型为ustore，连跑极致RTO配置、并行回放配置、极致RTO备机读配置场景下主备同步用例；完成一轮集成测试ustore用例执行 | 测试  通过 |
| 可靠性\稳定性测试 | 覆盖硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及2个7*24H稳定测试。 | 测试  通过 |
| 性能测试          | 测试2P单机、2P主备、4P单机场景TPCC满足性能指标               | 测试  通过 |
| 升级测试          | 进行3轮测试，覆盖2.0.x(无ustore)\3.0.x\5.0.x\6.0.0 RC1版本升级至6.0.0 LTS版本，回滚再升级成功 | 测试  通过 |
| 资料测试          | 测试整体介绍页、GUC参数和视图函数                            | 测试  通过 |
| 备份恢复          | 验证ustore场景1000仓数据备份恢复功能正常                     | 测试  通过 |

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
| 14   | 《安全加固指南》   | PASS         |
| 15   | 《插件参考》       | PASS         |
| 16   | 《SQL参考》        | PASS         |
| 17   | 《工具与命令参考》 | PASS         |
| 18   | 《数据库参考》     | PASS         |
| 19   | 《附录》           | PASS         |

## DBV伙伴测试结论

| 伙伴名称 | 主导新需求数量 | 需求缺陷密度                                                 | 问题单数量 | 贡献用例数量                   | 测试内容                                                     | 遗留问题个数 | 版本测试结论 |
| -------- | -------------- | ------------------------------------------------------------ | ---------- | ------------------------------ | ------------------------------------------------------------ | ------------ | ------------ |
| 海量     | 3              | 使用****：0个/kloc<br />支持****：0个/kloc<br />逻辑****：0.375个/kloc | 53         | 文本用例：528                  | 1、需求测试<br />2、继承特性验证：数据库服务（SQL语法、主备管理、GUC参数控制、内核工具链）、数据库管理与运维（实例管理）、数据库兼容性（资源池化兼容性）和可靠性测试 | 0            | 通过         |
| 南大     | 5              | select支持****：1个/kloc<br />启用禁用****：2.86个/kloc<br />支持通过****：0个/kloc<br />插件****：0.67个/kloc<br />支持GMS_STATS包：3个/kloc | 6          | 文本用例：439                  | 1、需求测试<br />2、继承特性验证：数据库服务（SQL语法、主备管理、内核工具链）、数据库管理与运维（安装卸载、升级、运维视图）、数据库兼容性（环境/驱动兼容、A/B/PG兼容性）<br />3、安全测试：日志信息、新特性和越权风险等<br />4、性能测试：单标1W/10W行列转换耗时<br />5、兼容性测试：升级兼容<br />6、资料测试：插件参考和SQL参考章节 | 0            | 通过         |
| 神通     | 5              | 支持用****：2.67个/kloc<br />去除****：1.7个/kloc<br />支持游标****：0个/kloc<br />MySQL8.0****：0.9个/kloc<br />支持array****：10个/kloc | 24         | 文本用例：625<br />自动化：188 | 1、需求测试<br />2、安全测试：神通自研语句模糊测试工具RandOMTest模糊测试<br />3、资料测试：随新需求验证，验证通过 | 0            | 通过         |
| 国创     | 11             | DataKit业务开发-****功能优化：1.43个/kloc<br />实例监控****：3.33个/kloc<br />支持慢****和智能诊断****：1.41个/kloc<br />智能诊断****：5.03个/kloc<br />告警监控****：3.85个/kloc<br />支持告警****：3.05个/kloc | 15         | /                              | 1、需求测试<br />2、继承特性验证：DataKit智能运维和业务开发<br />3、性能测试：覆盖http接口性能测试、分析CPU和内存占用<br />4、兼容性测试：智能运维插件支持监控openGauss多版本验证 | 2            | 通过         |
| 恩墨     | 1              | A兼容****：0个/kloc                                          | 21         | 文本用例：33<br />自动化：26   | 1、需求测试                                                  | 0            | 通过         |



# 问题单统计

## 问题单统计

openGauss 6.0.0 LTS版本共发现问题1227个，有效问题1032个，无效问题195个。修复问题回归测试结果正常，版本整体质量良好。遗留6个问题详细分布见下表: 

| 版本名称                          | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| --------------------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 6.0.0 LTS Test round 1  | 2024-04-01   | 2024-04-10   | 49         | 5          |
| openGauss 6.0.0 LTS Test round 2  | 2024-04-11   | 2024-04-17   | 27         | 8          |
| openGauss 6.0.0 LTS Test round 3  | 2024-04-18   | 2024-04-24   | 18         | 6          |
| openGauss 6.0.0 LTS Test round 4  | 2024-04-25   | 2024-05-08   | 34         | 4          |
| openGauss 6.0.0 LTS Test round 5  | 2024-05-09   | 2024-05-15   | 19         | 2          |
| openGauss 6.0.0 LTS Test round 6  | 2024-05-16   | 2024-05-29   | 53         | 7          |
| openGauss 6.0.0 LTS Test round 7  | 2024-05-30   | 2024-06-05   | 16         | 6          |
| openGauss 6.0.0 LTS Test round 8  | 2024-06-06   | 2024-06-12   | 25         | 12         |
| openGauss 6.0.0 LTS Test round 9  | 2024-06-13   | 2024-06-19   | 39         | 18         |
| openGauss 6.0.0 LTS Test round 10 | 2024-06-20   | 2024-06-26   | 46         | 10         |
| openGauss 6.0.0 LTS Test round 11 | 2024-06-27   | 2024-07-10   | 71         | 13         |
| openGauss 6.0.0 LTS Test round 12 | 2024-07-11   | 2024-07-17   | 45         | 15         |
| openGauss 6.0.0 LTS Test round 13 | 2024-07-18   | 2024-07-24   | 67         | 8          |
| openGauss 6.0.0 LTS Test round 14 | 2024-07-25   | 2024-07-31   | 37         | 4          |
| openGauss 6.0.0 LTS Test round 15 | 2024-08-01   | 2024-08-07   | 61         | 8          |
| openGauss 6.0.0 LTS Test round 16 | 2024-08-08   | 2024-08-14   | 69         | 16         |
| openGauss 6.0.0 LTS Test round 17 | 2024-08-15   | 2024-08-21   | 71         | 16         |
| openGauss 6.0.0 LTS Test round 18 | 2024-08-22   | 2024-08-28   | 75         | 10         |
| openGauss 6.0.0 LTS Test round 19 | 2024-08-29   | 2024-09-04   | 81         | 9          |
| openGauss 6.0.0 LTS Test round 20 | 2024-09-05   | 2024-09-11   | 66         | 16         |
| openGauss 6.0.0 LTS Test round 21 | 2024-09-12   | 2024-09-18   | 51         | 2          |
| openGauss 6.0.0 LTS Test round 22 | 2024-09-19   | 2024-09-25   | 12         | 0          |

本次测试共22轮，从Test round 1持续到Test round 22。

2024.04.01正式进入openGauss 6.0.0 LTS第一阶段测试，直至2024.06.30 openGauss 6.0.0 LTS第二阶段测试结束，Test round 1 至Test round 10部分兼容性、工具链和资源池化需求转测，问题单新增较为平缓，占比问题单总量1/3，兼容性问题最多，工具链由于转测需求少问题最少。

2024.07.01正式进入openGauss 6.0.0 LTS第三阶段测试，版本需求大批量转测试，问题单增长较为迅猛，约占比问题单总量1/3，Test round 16也达到该阶段问题单数量峰值；第三阶段兼容性、工具链和资源池化问题单占比大致相同，新增少量内核加速问题单。

2024.08.15 Test round17开始进入集成测试阶段，Test round 17 至Test round 19部分延迟转测需求和版本集成测试并行开展，问题单处于上升趋势并于Test round 19达到整个版本峰值。Test round20完成一轮集成测试，涉及长稳、可靠性、性能、安全、资源池化等场景。随后进入Test round21集成测试第二阶段，问题单数下降，集成测试问题单主要集中在工具链和兼容性。Test round22为最后一个集成验证周期，测试活动主要为问题回归。

## 执行用例与问题单数对应关系

| 领域             | 执行用例数量 | 问题单数量 |
| ---------------- | ------------ | ---------- |
| 数据库服务       | 50577        | 215        |
| 工具链           | 6574         | 266        |
| 数据库管理与运维 | 970          | 120        |
| 兼容性           | 8776         | 193        |
| 可靠性\稳定性    | 1162         | 20         |
| 资源池化         | 1304         | 185        |
| 性能             | 36           | 8          |
| 数据库备份恢复   | 194          | 17         |



# 附件

## 附件1：遗留问题列表

| 序号 | issue号 | 问题简述                                                     | 分类            | 问题级别 | 问题分析与影响                                               | 规避措施                                                     |
| ---- | ------- | ------------------------------------------------------------ | --------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | IAO53D  | 故障注入场景20并发执行含大量子事务回滚的自定义函数偶现core   | 可靠性          | 次要     | 分析：该问题属于踩内存问题，当对并发执行的回滚任务进行kill -9操作时，对应的事务会终止并清理相关内存，在清理过程中，由于指针指向了不合法的内存地址，导致内存释放异常，进而导致数据库core。<br />影响：（1）该问题测试场景属于故障注入的暴力测试，用kill -9模拟客户场景的极限场景，比如掉电闪退等，该场景在客户场景几乎不会出现，及时出现，core的影响相较于掉电闪退带来的影响可忽略不计，因此评估客户侧影响风险整体较小（2）当前测试同学重复执行100次以上，同时开发同学已连续进行16天以上的复现尝试，但是该问题均未再次出现，因此评估该问题出现概率极低。 | （1）增加对内存释放的合法性校验、如空指针和函数指针的判断，来规避core的发生。（2）在core发生后，对含cm的集群场景，cm在主机core掉后会自动进行主备切换，因此可保证业务正常运行；对不含cm的集群场景和单机场景，可通过脚本监控手段，在异常发生后，自动重启服务，来保证业务正常运行。 |
| 2    | IALVGL  | 【资源池化】稳定性测试备份过程中，偶现增量备份失败           | 备份恢复        | 次要     | 分析：该系统运行期间，开启了DMS、DSS的debug日志（生产场景不推荐开启），导致系统运行速度较慢。同时在主备机运行同一数据库的TPCC测试程序（生产场景不推荐用户在备机频繁查询主机正在频繁修改的数据），进一步加剧了系统性能劣化，主备机日志中存在大量的等锁超时告警。此时运行备份程序时，主机信号槽位耗尽，本轮备份失败。不开启debug日志情况下，不复现该问题。<br />影响：较低，偶现失败后，再次执行备份即可；并不影响用户使用次功能 | 生产环境下用户需要正确配置数据库 参数，基本不会出现问题单中系统负载极高的情况，此时执行备份不会失败。如果极端情况出现因系统负责较高导致的备份失败问题，重新执行备份即可。 |
| 3    | IAMIAL  | 数据迁移偶现两个增量迁移框                                   | 迁移            | 不重要   | 分析：多次复现没有复现成功，代码没有找到重复原因<br />影响：界面出现两个迁移框，对功能没有影响 | 关闭侧边子任务详情页面，再次点击详情，手动刷新问题部分页面4  |
| 4    | IAQ4YI  | 资源池化偶现启动失败，出现第三方开源软件openssl函数内部空指针问题 | 内核安装&三方库 | 次要     | 分析：资源池化启动或运行时偶现失败，堆栈显示在三方库openssl内CRYPTO_THREAD_write_lock()函数<br />影响：资源池化每日CI环境中没有出现，累计至少1000次没有出现，测试人员持续测试至少1000次没有出现；只有开发环境上偶现两次启动时失败，出现概率低 | 资源池化启动失败后重新启动可以解决。                         |
| 5    | IAS2WG  | 【测试类型：工具功能】【测试版本：6.0.0】DataKit-智能运维_告警性能问题 | 工具链          | 不重要   | 分析：DataKit本身功能正常，接口在100并发时，告警模板接口的性能没有达到<=3s要求，20并发时平均响应时长（500ms）符合要求      <br />影响：告警接口并发过高，响应会变慢，影响用户体验，但不会影响实际功能。 | 不影响功能，并发高的时候等待响应即可。                       |
| 6    | IAS2G9  | 【测试类型：工具功能】【测试版本：6.0.0】DataKit-智能运维_【掉点相关】实例数据采集刷新问题 | 工具链          | 次要     | 分析：由于网络等原因，采集数据超过代码中设定的采集超时时间（2秒），导致数据未采集到，出现掉点。   <br />影响：在界面显示的折线图会出现值为0的点，这些点的数据无效，其他的点位不影响。改进措施要考虑网络延时情况，增加对于超时时间的可配置选项，动态修改采集超时时间。 | 1、用户需确认网络延时<br>2、可考虑安装实例监控代理时和被监控的库安装在一个服务器上<br>3、也可考虑联系开发人员修改实例代理代码中的超时时间再重新安装agent |

优先级和DI对应关系说明：

严重/主要：DI 3分

次要：DI 1分

不重要/无级别：DI 0.1分

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@sungang14](https://gitee.com/sungang14)

+ [@wan005](https://gitee.com/wan005)

+ [@peilinqian](https://gitee.com/peilinqian)

+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)

+ [@li-xin12345](https://gitee.com/@li-xin12345)

+ [@zhanghuan96](https://gitee.com/@lzhanghuan96)

+ [@feelingpeng](https://gitee.com/@feelingpeng)

+ [@szoscar55](https://gitee.com/@szoscar55)

+ [@cloudsbreak](https://gitee.com/@cloudsbreak)

+ [@xue-xinyi610](https://gitee.com/@xue-xinyi610)

+ [@JusticeArbiter](https://gitee.com/@JusticeArbiter)

  

