![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

 

| 日期      | 修订版本 | 修改章节   | 修改描述     | 作者        |
| --------- | -------- | ---------- | ------------ | ----------- |
| 2022.3.15 | 1.0      | 初稿撰写   |              | yansong_lee |
| 2022.3.22 | 1.1      | 问题单统计 | 刷新遗留问题 | zhangao     |

目 录

1 概述

2 测试版本说明

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论

5 问题单统计

6 附件

**Keywors 关键词**：openGauss 3.0.0 Release

**Abstract 摘要**：主要是描述了openGauss 3.0.0 Release版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                             | 中文解释       |
| ------ | ------------------------------------ | -------------- |
| SQL    | Structured Query Language            | 接口化查询语言 |
| LTS    | Long time support                    | 长时间维护     |
| CVE    | Common Vulnerabilities and Exposures | 公共漏洞和暴露 |
| DML    | Data Manipulation Language           | 数据操纵语言   |
| DDL    | Data Definition Language             | 数据定义语言   |
| DCL    | Data Control Language                | 数据控制语言   |
| DS     | Data Studio                          | 数据库管理工具 |
| CM     | Cluster Management                   | 集群管理工具   |

 

***
\***

# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 3.0.0 Release版本新增CM集群管理、Data Studio、openGauss分布式解决方案和支持小型化部署等特性，并对若干关键缺陷进行了修改。本文主要描述了openGauss 3.0.0 Release版本整体测试情况，结合[openGauss 3.0.0 Release版本测试策略](https://gitee.com/yansong_lee/QA/blob/master/Test_Strategy/openGauss_3.0.0_Release/openGauss%203.0.0%20Release%E7%89%88%E6%9C%AC%E6%B5%8B%E8%AF%95%E7%AD%96%E7%95%A5.md)，展开相应的测试活动。重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 3.0.0 Release版本新特性交付质量较好，支持5220鲲鹏服务器达成100w tpmC等关键指标达成，问题单也趋于收敛，版本整体质量较好。

# 测试版本说明

本章节描述测试版本的基本信息，包括测试对象是什么，以及在什么环境下开展的测试，具体包括被测版本和测试硬件环境。

描述被测对象的版本信息和测试的时间及测试轮次。

| 版本名称                    | 测试起始时间 | 测试结束时间 |
| --------------------------- | ------------ | ------------ |
| openGauss 3.0.0 Release RC1 | 2022-1-20    | 2022-1-26    |
| openGauss 3.0.0 Release RC2 | 2022-1-27    | 2022-1-30    |
| openGauss 3.0.0 Release RC3 | 2022-2-10    | 2022-2-16    |
| openGauss 3.0.0 Release RC4 | 2022-2-17    | 2022-2-19    |
| openGauss 3.0.0 Release RC5 | 2022-2-20    | 2022-2-27    |
| openGauss 3.0.0 Release RC6 | 2022-2-28    | 2022-3-9     |
| openGauss 3.0.0 Release RC7 | 2022-3-10    | 2022-3-12    |
| openGauss 3.0.0 Release RC8 | 2022-3-13    | 2022-3-15    |

描述本次测试的测试环境（包括环境软硬件版本信息，环境组网配置信息, 测试辅助工具等）。

| 硬件型号                 | 硬件配置信息                                                 | 备注 |
| ------------------------ | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280) | CPU：Kunpeng-920 5220 2p 64核<br />内存：512G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280) | CPU：Kunpeng-920 6426 2p 64核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2480) | CPU：Kunpeng-920 7260 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| RH2288H V3               | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4*10GE |      |

虚拟化平台

| 虚拟化平台 | 版本说明                                       |
| ---------- | ---------------------------------------------- |
| KVM虚拟化  | KVM+GuestOS（Centos7.6/OpenEuler20.03（LTS）） |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| Kylin     | V10 SP1          | Kylin V10 SP1，aarch版本ISO<br />SHA256:e6fffdb9dec030920a33ad4c57b68cce8cd7d0496b4bd04a7b85f8cc5494cf41 |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 3.0.0 Release版本是openGauss第二个长期支持版本（LTS），维护周期为3.5年，本次发布的需求列表如下：

| no   | feature                                                      | status   | sig           | owner |
| ---- | ------------------------------------------------------------ | -------- | ------------- | ----- |
| 1    | 【openGauss 3.0.0 LTS】CM集群管理开源                        | Accepted | Tools         |       |
| 2    | 【openGauss 3.0.0 LTS】openGauss分布式解决方案               | Accepted | Tools         |       |
| 3    | 【openGauss 3.0.0 LTS】openGauss支持行表、索引压缩           | Accepted | StorageEngine |       |
| 4    | 【openGauss 3.0.0 LTS】openGauss支持发布订阅的多地多活       | Accepted | StorageEngine |       |
| 5    | 【openGauss 3.0.0 LTS】Data Studio开源                       | Accepted | Tools         |       |
| 6    | 【openGauss 3.0.0 LTS】支持小型化部署，空载内存底噪<250M，安装包<50M | Accepted | StorageEngine |       |
| 7    | 【openGauss 3.0.0 LTS】支持当前会话统计中加入归一化SQL处理，提升慢SQL历史信息诊断 | Accepted | SQLEngine     |       |
| 8    | 【openGauss 3.0.0 LTS】openGauss支持5220鲲鹏服务器达成100w tpmC | Accepted | StorageEngine |       |
| 9    | 【openGauss 3.0.0 LTS】openGauss支持Global system cache      | Accepted | StorageEngine |       |

openGauss 3.0.0 Release版本测试活动分工如下：

| 需求                                                         | 开发主体      | 测试主体 | 验证策略                                                     |
| ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 【openGauss 3.0.0 LTS】CM集群管理开源                        | Tools         | QA       | 1、验证CM集群管理工具的功能和可靠性<br>2、部署openGauss+CM进行长稳测试，关注其仲裁能力 |
| 【openGauss 3.0.0 LTS】openGauss分布式解决方案               | Tools         | QA       | 1、验证shardingSphere对接openGauss数据库的功能、可靠性和性能<br>2、关注分布式关键性能指标达成情况 |
| 【openGauss 3.0.0 LTS】openGauss支持行表、索引压缩           | StorageEngine | QA       | 1、验证行表压缩的功能、可靠性、性能、资料和长时间稳定性<br>2、关注行表压缩的不同压缩率下的空间表现，同时关注升级兼容性 |
| 【openGauss 3.0.0 LTS】openGauss支持发布订阅的多地多活       | StorageEngine | QA       | 1、验证发布订阅的功能、可靠性、资料和长时间稳定性<br/>2、关注发布端断连后的恢复措施，以及订阅端避免重复订阅。同时关注升级兼容性 |
| 【openGauss 3.0.0 LTS】Data Studio开源                       | Tools         | QA       | 1、验证Data Studio工具的功能、可靠性和资料<br/>2、关注Data Studio源码的编译和构建，关注软件包和其他发布件的验收 |
| 【openGauss 3.0.0 LTS】支持小型化部署，空载内存底噪<250M，安装包<50M | StorageEngine | QA       | 1、验证小型化安装部署以及关键指标的达成情况（空载内存底噪<250M，安装包<50M）<br/>2、关注小型化包的编译和构建，关注软件包和其他发布件的验收 |
| 【openGauss 3.0.0 LTS】支持当前会话统计中加入归一化SQL处理，提升慢SQL历史信息诊断 | SQLEngine     | QA       | 1、验证本需求的功能和可靠性<br/>2、关注升级兼容性            |
| 【openGauss 3.0.0 LTS】openGauss支持5220鲲鹏服务器达成100w tpmC | StorageEngine | QA       | 1、验证在5220鲲鹏服务器上对openGauss进行tpcc性能测试<br/>2、验收openGauss支持5220鲲鹏服务器达成100w tpmC |
| 【openGauss 3.0.0 LTS】openGauss支持Global system cache      | StorageEngine | QA       | 1、验证支持系统表syscache的全局缓存的功能、可靠性、资料和长时间稳定性<br/>2、关注升级兼容性 |

# 版本概要测试结论

openGauss 3.0.0 Release版本整体测试按照release-manager团队的计划，共完成了5轮系统测试+3轮集成验证测试：其中前2轮测试重点关注新特性的验收，并对继承特性进行了自动化看护验收，同时对关键性能数据进行了摸底，旨在发现阻塞性问题。第三轮和第四轮测试除了对新特性进行迭代测试外，还投入到CM和DS的开源整改后版本规格验收上，保证这2款工具的如期开源。第五轮测试对新特性进行发散测试，并启动了可靠性、安全、兼容性和资料等专项测试。第六轮测试关注所有交付新特性的验收进展，进行系统集成验证，检查系统集成的完整性和正确性。并启动了7*24H长稳测试和CVE漏洞扫描等安全测试，目的是保证版本的稳定性。第七轮测试主要精力放在编译安装和版本构建上，保证社区安装包的可靠可用。同时对关键性能指标进行再次验收，旨在保证发布时对外的承诺达成。最后一轮测试通过自动化进行了版本发布前的验收测试。openGauss 3.0.0 Release版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全），所有测试任务均按计划完成。本版本计划交付需求9个，实际交付9个，交付率100%，所有发布需求均验证通过。

openGauss 3.0.0 Release版本版本共发现问题213个，有效问题186个，遗留问题3个（详见遗留问题章节），其他问题均已修复，回归测试结果正常。版本整体质量较好。

# 版本详细测试结论

openGauss 3.0.0 Release版本详细测试内容包括：

1、通过自动化看护，从接入层、SQL层、存储层、管理、安全、可靠可用和工具等7个维度进行openGauss继承特性测试，继承功能无丢失；

2、完成CM、DS、支持小型化部署、支持分布式方案、支持行存压缩和支持发布订阅多地多活等新特性的全量功能验证，测试较为充分，特性质量良好；

3、针对系统的稳定性，进行长稳测试，包括sqlsmith语法组合攻击测试、事务并发测试、benchmarksql+sysbench加压测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

4、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 特性测试结论

### 继承特性评价

对产品所有继承特性进行评价，用表格形式评价，包括特性列表（与特性清单保持一致），验证质量评估

| Domain   | Feature                | 质量评估                   | 备注                                                       |
| -------- | ---------------------- | -------------------------- | ---------------------------------------------------------- |
| 接入层   | 应用开发接口           | <font color=green>▮</font> | 支持JDBC、ODBC、PDBC和GDBC应用开发接口                     |
| SQL层    | 支持SQL语言版本SQL2003 | <font color=green>▮</font> | 支持SQL2003语言                                            |
|          | 支持SQL基本语法        | <font color=green>▮</font> | 支持SQL基础语法DML/DQL/DCL/DDL                             |
|          | 存储过程               | <font color=green>▮</font> | 支持存储过程                                               |
|          | 系统表和系统视图       | <font color=green>▮</font> | 支持系统表和系统视图                                       |
|          | DBE_PERF内视图         | <font color=green>▮</font> | 支持DBE_PERF内视图                                         |
|          | SQL关键字              | <font color=green>▮</font> | 包含保留关键字及非保留关键字                               |
| 存储层   | 索引                   | <font color=green>▮</font> | 支持btree/ubtree/psort/hash/gin/gist索引                   |
|          | 数据分区               | <font color=green>▮</font> | 支持一级分区、二级分区                                     |
|          | 行列混合存储           | <font color=green>▮</font> | 支持行/列存储                                              |
|          | 内存表                 | <font color=green>▮</font> | 支持MOT                                                    |
|          | 物化视图               | <font color=green>▮</font> | 支持物化视图                                               |
|          | 全文索引               | <font color=green>▮</font> | 支持常见单字及词查询，长句子检索，中英文混排检索           |
| 管理     | 命令行安装             | <font color=green>▮</font> | 支持命令行安装                                             |
|          | 版本升级               | <font color=green>▮</font> | 支持常见数据库软件版本升级                                 |
|          | 数据库对象管理         | <font color=green>▮</font> | 支持使用Data Studio工具进行操作                            |
|          | 诊断分析报告           | <font color=green>▮</font> | 支持WDR Snapshot生成性能诊断分析报告                       |
|          | 智能运维               | <font color=green>▮</font> | 支持使用不同算子创建mode，为表创建虚拟索引、推荐查询索引列 |
| 安全     | 身份鉴别               | <font color=green>▮</font> | 支持定义鉴别失败次数，用户具有唯一标识性                   |
|          | 访问控制               | <font color=green>▮</font> | 支持高级别密码存储加密                                     |
|          | 安全接入               | <font color=green>▮</font> | 具备基于SHA256/SM3机制的口令加密认证方法                   |
|          | 强制访问控制           | <font color=green>▮</font> | 支持强制访问控制，主题粒度达用户级                         |
|          | 安全审计               | <font color=green>▮</font> | 支持常见安全审计，支持检测安全侵害事件                     |
|          | 三权分立               | <font color=green>▮</font> | 支持常见系统管理员、安全管理员和审计管理员角色             |
|          | 数据存储加密           | <font color=green>▮</font> | 支持数据存储加密                                           |
|          | 全密态数据库           | <font color=green>▮</font> | 支持全密态数据库功能                                       |
|          | 动态脱敏               | <font color=green>▮</font> | 支持动态脱敏功能                                           |
|          | 防篡改功能             | <font color=green>▮</font> | 支持防篡改功能                                             |
|          | 恶意攻击防范           | <font color=green>▮</font> | 支持恶意攻击防范功能                                       |
| 可靠可用 | 物理备份恢复           | <font color=green>▮</font> | 支持对数据库数据进行备份恢复操作                           |
|          | 全量PITR物理恢复       | <font color=green>▮</font> | 支持恢复到备份归档数据之后的任意时间点                     |
|          | 逻辑导入导出           | <font color=green>▮</font> | 支持对数据库进行逻辑备份还原                               |
|          | 延迟备份               | <font color=green>▮</font> | 支持备机延迟回放                                           |
|          | 逻辑复制               | <font color=green>▮</font> | 支持逻辑复制                                               |
|          | 极致RTO                | <font color=green>▮</font> | 支持极致RTO，具备极致RTO场景下故障恢复能力                 |
| 工具     | 客户端工具             | <font color=green>▮</font> | 支持gsql数据库连接工具                                     |
|          | 服务端工具             | <font color=green>▮</font> | 支持数据库导入导出/系统检查等工具                          |
|          | 系统内部使用工具       | <font color=green>▮</font> | 支持安装卸载/备份/缩扩容/升级等工具                        |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估,给出特性质量评估结论。

| 序号 | 特性名称                                                     | 测试情况说明                                                 | 约束依赖说明                                                 | 遗留问题单 | 特性质量评估                | 备注                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------- | --------------------------- | ------------------------------------------------------------ |
| 1    | 【openGauss 3.0.0 LTS】CM集群管理开源                        | CM共执行4轮测试，执行用例340条，此版本为首次开源，整改后代码符合开源规范，并对开源整改后版本进行了独立编译、OM适配CM组网安装、基本功能（停启集群、主备切换、安装卸载、参数设置获取等）和仲裁能力等方面的验证，保证CM开源质量 | 1.安装xml需要配置cm参数                                      | 无         | <font color=green>▮</font>  |                                                              |
| 2    | 【openGauss 3.0.0 LTS】openGauss分布式解决方案               | 分布式解决方案共执行3轮测试，共执行用例6个，通过配置数据库极限场景，通过测试1分片和8分片场景，保证ss-jdbc的性能损耗15％以内和16节点性能达标1000W | 1.需部署ShardingSphere-Proxy、ShardingSphere-JDBC来对接openGauss数据库，实现数据分片 | 无         | <font color=green>▮</font>  | 8分片完美sharding场景下tpmc性能可达1000万+<br/>单SS-JDBC直连数据库性能损耗为9.3% |
| 3    | 【openGauss 3.0.0 LTS】openGauss支持行表、索引压缩           | 共执行3轮测试，共执行用例67条，测试覆盖功能、可靠性和安全方面。重点验证不同数据量下行存压缩表的压缩率，并覆盖正常、故障及压力场景下，对行存压缩数据在主备节点上数据一致性的验证 | 1.需要开启支持压缩特性参数；<br />2.仅支持堆组织形式数据表的压缩，即行存表、Btree索引等；<br />3.系统要设置足够大的max_map_count。 | I4UDYB     | <font color=yellow>▲</font> | 支持对行存表、索引等进行压缩，通过压缩可大大减少存储空间，最大可达50+%压缩率；支持pglz与zstd两种压缩算法 |
| 4    | 【openGauss 3.0.0 LTS】openGauss支持发布订阅的多地多活       | 发布订阅共执行3轮测试，共执行219个用例，通过yat测试套覆盖基础功能，通过cfe故障注入等手段保证可靠性及安全 | 1.发布端wal_level=logical<br>2.max_replication_slots>当前物理流复制槽+所需的逻辑复制槽数<br/>3.不支持列存，支持行存ustore和段页式<br/>4.不支持DDL，不支持TRUNCATE<br />5.发布表需要满足以下两个条件之一：a) 有主键 b)有REPLICA IDENTITY FULL权限 若以上两个条件均不满足，则只能进行插入，无法进行跟新和删除数据操作<br/>6. 单条元组大小不超过1GB，考虑解码结果可能大于插入数据，因此建议单条元组大小不超过500MB<br/>7.不复制序列数据。<br/>8.复制只能从基表到基表<br/>9. 不支持级联订阅<br/>10.支持部分触发器。仅支持ROW TIGGER<br/>11.发布端设置enable_slot_log=on，发布端主备切换不断开，订阅端无需人工更新订阅信息<br/>12. 不支持同步基础数据，只能同步创建订阅后的增量数据<br/> | 无         | <font color=green>▮</font>  |                                                              |
| 5    | 【openGauss 3.0.0 LTS】Data Studio开源                       | 执行3轮测试，共计执行用例4568条，覆盖安全、基础功能和资料测试。重点验证升级全新框架后的安全特性，并保证开源后的数据库连接工具核心功能正常稳定 | 无                                                           | 无         | <font color=green>▮</font>  |                                                              |
| 6    | 【openGauss 3.0.0 LTS】支持小型化部署，空载内存底噪<250M，安装包<20M | 小型化共执行了1轮测试，执行了336条用例，通过yat测试套和手工执行覆盖基础功能，通过故障注入等手段保证基本功能稳定 | 1.普通用户必须有数据库包解压路径、安装路径的读、写和执行权限，并且安装路径必须为空，安装路径和解压路径以及配置的日志路径交叉<br />2.普通用户对下载的openGauss小型化压缩包有执行权限<br />3.环境变量：新安装时请确保GAUSSHOME、GAUSSDATA、GAUSSLOG、GAUSSENV不存在，如果是执行配置启动等后续操作，确保这些环境变量对应的配置正确<br />4.小型化安装时端口取值1024-65535，安装前需确保端口没有被占用<br />5.主备安装需要在各个节点分别执行安装指令 | 无         | <font color=green>▮</font>  | 安装包22M<50M，空载内存底噪为163M<250M，会话内存空载<4M      |
| 7    | 【openGauss 3.0.0 LTS】支持当前会话统计中加入归一化SQL处理，提升慢SQL历史信息诊断 | 共覆盖1轮测试，共执行33条用例，通过yat测试套和手工执行的方式覆盖基础功能，通过设置不同的guc参数验证Full SQL和Slow SQL的信息诊断，确保特性功能稳定 | 1.依赖当前的Slow Query机制来实现Full Sql，通过guc参数log_min_duration_statment控制Full Sql或者Slow Sql | 无         | <font color=green>▮</font>  |                                                              |
| 8    | 【openGauss 3.0.0 LTS】openGauss支持5220鲲鹏服务器达成100w tpmC | 共执行2轮测试，第一轮进行性能摸底，第二轮性能验收，tpcc均能达到100w | 1.支持鲲鹏5220服务器，硬件规格需满足：<br />CPU：Kunpeng-920 5220*64<br />内存：512G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 | 无         | <font color=green>▮</font>  | tpmc=1000080.35w                                             |
| 9    | 【openGauss 3.0.0 LTS】openGauss支持Global system cache      | gsc全局系统缓存需求共执行2轮测试，共执行用例120个，通过CI功能和可靠性全量测试套覆盖功能及可靠性的原有功能不受影响。通过该需求设计测试用例（包括高并发，主备切换，guc参数交互等场景）保证主备系统缓存及淘汰功能正常。通过长稳及性能测试保证gsc压力场景下数据库功能正常稳定。 | 1.enable_global_sycache设置为on                              | 无         | <font color=green>▮</font>  |                                                              |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 专项测试结论

### 安全测试

openGauss 3.0.0 Release版本安全测试覆盖：

1、 新特性安全加固测试：openGauss 3.0.0 Release版本涉及发布订阅和行表压缩两大特性，均已执行相关安全加固测试。

2、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

3、针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

4、利用开源测试工具sqlsmith对数据库进行FUZZ测试。

openGauss 3.0.0 Release版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 可靠性测试

故障注入类测试

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 符合预期 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 符合预期 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 符合预期 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 符合预期 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且清楚故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 符合预期 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 符合预期 |

长时间负载测试

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | TPCC+sysbench连跑instert/update/delete事务，1000仓+800并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、dml正删改查并发事务、ddl创建表删表事务；cpu、内存和io等系统资源使用均正常；一致性检查通过；7*24H满足 |
| 场景2  | sqlsmith工具随机生成SQL语句，执行7*24H测试                   | 连跑7*24小时，数据库未产生core，无异常宕机、卡死等问题，随机生成的sql语句均正常执行 |
| 场景3  | TPCC+持续周期性kill主进程，恢复后校验数据一致性，开启RTO，异步备，7*24H测试 | 连跑7*24小时，数据库未产生core，kill主进程后能正常恢复，数据一致性校验成功 |
| 场景4  | TPCC并发，PITR测试半小时增量备一次，dml/ddl综合事务测试和特性混合测试，开启RTO，异步备，7*24H测试 | 连跑7*24小时，数据库未产生core，PITR备份正常，dml/ddl语句执行正常 |

压力测试

| Domain | 测试内容                                                     | 测试结论               |
| ------ | ------------------------------------------------------------ | ---------------------- |
| 场景1  | gsc关闭场景下，tpcc+sysbench，1000仓+10000并发，1小时压力测试 | 测试通过               |
| 场景2  | gsc开启场景下，tpcc+sysbench，1000仓+10000并发，1小时压力测试 | 测试通过               |
| 场景3  | 备份恢复（gs_dump/gs_basebackup/gs_backup）,1000仓+100000并发 | 备份恢复正常，测试通过 |

### 性能测试

性能测试除了关注关键性能指标不劣化外，还需要对新的性能指标（如kunpeng-5220 2P单机1H 100W）进行摸底和测试。利用benckmarksql等工具辅助完成性能测试。

| **指标大项** | **指标小项**                                                 | **指标值** | **说明**                              | 测试结论                                                     |
| ------------ | ------------------------------------------------------------ | ---------- | ------------------------------------- | ------------------------------------------------------------ |
| TPCC         | 2P（Taishan 200 2280 6426）单节点 1H                         | 150万      | 与Release基线数据差异小于5%以内可接受 | 单节点1H在极限场景配置下tpmC为***150.33***W+，验证通过       |
|              | 2P（Taishan 200 2280 6426）单节点 8H                         | 150万      | 与Release基线数据差异小于5%以内可接受 | 单节点8H在极限场景配置下tpmC为***144.96***W+。在5%浮动范围内，验证通过 |
|              | 2P（Taishan 200 2280 6426）一主一备 1H                       | 120万      | 与Release基线数据差异小于5%以内可接受 | 一主一备1H在极限场景配置下tpmC为***117.3***W+。在5%浮动范围内，验证通过 |
|              | 4P（Taishan 200 2280 7260）单节点 1H                         | 230万      | 与Release基线数据差异小于5%以内可接受 | 单节点在极限场景配置下tpmC为***219***W+。在5%浮动范围内，验证通过 |
|              | 2P（Taishan 200 2280 5220）单节点 1H                         | 100万      | 此即为Release基线数据                 | 单节点在极限场景配置下tpmC为***100***W+。验证通过            |
|              | ss-jdbc直连性能损耗比<50%                                    | 1000万     | 此即为Release基线数据                 | ss-jdbc 1节点1H，tpmC 136W（数据库直连1节点1H，tpmC 150W），性能损耗(150-136)/150=***9.3%***。单SS-JDBC直连数据库性能损耗小于50%，验证通过 |
|              | 分布式OLTP，TPCC线性度0.8，2P（Taishan 200 2280 6426）16节点（8pcs+7ss-jdbc+1ss-proxy） 1H | 1000万     | 此即为Release基线数据                 | 8分片完美sharding场景下tpmc性能可达***1073***W+，验证通过    |
| RTO          | 一主两同步备failover                                         | 10s        | 与Release基线数据差异小于5%以内可接受 | 1.025s，验证通过                                             |

### 兼容性测试

#### 升级兼容

针对openGauss 3.0.0 Release版本，规划的升级路径如下表所示。测试整体情况：

1. openGauss 1.1.0/2.0.0/2.0.1/2.1.0升级到openGauss 3.0.0.成功，升级失败或者升级未提交，可以成功回滚
2. 故障场景下，环境恢复后升级可以重入并升级成功
3. 升级成功后，特性功能运行正常

| 升级路径                               | 测试结论 |
| -------------------------------------- | -------- |
| openGauss1.1.0灰度升级到openGauss3.0.0 | 测试通过 |
| openGauss2.0.0灰度升级到openGauss3.0.0 | 测试通过 |
| openGauss2.0.1灰度升级到openGauss3.0.0 | 测试通过 |
| openGauss2.1.0灰度升级到openGauss3.0.0 | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 3.0.0数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 3.0.0数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 支持云化部署 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.0.3 LTS环境下，通过Docker（18.09.0 build 62eb848）安装部署openGauss 3.0.0数据库 | 测试通过 |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.0.3 LTS/ARM+Kylin V10环境下，安装部署openGauss 3.0.0数据库 | 测试通过 |

### 资料测试

| 序号 | **手册名称**   | **测试结论** |
| ---- | -------------- | ------------ |
| 1    | 《发行说明》   | PASS         |
| 2    | 《产品描述》   | PASS         |
| 3    | 《技术白皮书》 | PASS         |
| 4    | 《特性描述》   | PASS         |
| 5    | 《编译指导书》 | PASS         |
| 6    | 《快速入门》   | PASS         |
| 7    | 《安装指南》   | PASS         |
| 8    | 《管理员指南》 | PASS         |
| 9    | 《开发者指南》 | PASS         |
| 10   | 《工具参考》   | PASS         |

# 问题单统计

openGauss 3.0.0 Release版本共发现问题单213个，有效问题186个，其中修复问题单183，回归均通过。详细分布见下表: 

| 版本名称                    | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| --------------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 3.0.0 Release RC1 | 2022-1-20    | 2022-1-26    | 56         | 6          |
| openGauss 3.0.0 Release RC2 | 2022-1-27    | 2022-1-30    | 8          | 1          |
| openGauss 3.0.0 Release RC3 | 2022-2-10    | 2022-2-16    | 39         | 3          |
| openGauss 3.0.0 Release RC4 | 2022-2-17    | 2022-2-19    | 7          | 0          |
| openGauss 3.0.0 Release RC5 | 2022-2-20    | 2022-2-27    | 24         | 2          |
| openGauss 3.0.0 Release RC6 | 2022-2-28    | 2022-3-9     | 32         | 5          |
| openGauss 3.0.0 Release RC7 | 2022-3-10    | 2022-3-12    | 17         | 8          |
| openGauss 3.0.0 Release RC8 | 2022-3-14    | 2022-3-15    | 3          | 2          |

本次测试共8轮，前两轮测试覆盖继承特性的自动化看护验收和新特性测试，并开展性能摸底测试，共发现问题71个，无效问题7个；第三、四轮除版本迭代测试外，重点对CM和DS开源工具进行测试验证，共发现问题49个，无效问题3个；第五轮启动可靠性、安全、兼容性及资料等专项测试，同时开展针对新特性的发散测试，共发现问题26个，无效问题2个；第六轮测试，针对版本进行系统集成验证，并启动长稳、安全、升级等专项测试，共发现问题37个，无效问题5个；第七轮对版本构建、编译安装等工具进行测试，同时启动性能专项测试，共发现问题25个，无效问题8个；第八轮测试通过自动化连跑工程对版本进行整体测试验收，共发现问题5个，无效问题2个。

整体看版本问题单趋于收敛。

# 附件

## 附件1：遗留问题列表

| 序号 | issue号                                                      | 问题简述                                                     | 问题级别 | 问题分析与影响                                               | 规避措施                                   | 备注 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ------------------------------------------ | ---- |
| 1    | [I4TRFY](https://gitee.com/opengauss/openGauss-server/issues/I4TRFY?from=project-issue) | gs_basebackup备份后，使用备份目录启动成功，然后gs_ctl stop失败 | 次要     | 问题分析：停止数据库时部分线程还未完全初始化完成<br />影响：数据库停止失败 | 可以通过在stop时指定immediate模式立即停止  |      |
| 2    | [I4PSHT](https://gitee.com/opengauss/openGauss-server/issues/I4PSHT?from=project-issue) | 数据库实例初始化后，系统表pg_proc的provariadic字段为0        | 次要     | 问题分析：系统表对应属性未做赋值，实际使用时从其他文件中读取该值<br />影响：不影响数据库正常使用 | 可执行\df 函数名查看参数是否带VARIADIC属性 |      |
| 3    | [I4UDYB](https://gitee.com/opengauss/openGauss-server/issues/I4UDYB?from=project-issue) | 存在行存压缩表时，备机重建指定参数-b full，重建后行存压缩表数据丢失 | 次要     | 问题分析：原因暂不明确，低概率出现，后六轮测试均未复现<br />影响：备机数据丢失 | 数据异常时，可通过build重新构造数据        |      |



