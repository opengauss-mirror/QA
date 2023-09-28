![avatar](../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改章节   | 修改描述       | 作者        |
| --------- | -------- | ---------- | -------------- | ----------- |
| 2023.9.14 | 1.0      |            | 初稿撰写       | jiexiao1413 |
| 2023.9.27 | 1.1      | 新需求评价 | 修改新需求评价 | jiexiao1413 |

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

**Keywords 关键词**：openGauss 5.1.0 Preview

**Abstract 摘要**：主要是描述了openGauss 5.1.0 Preview版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                             | 中文解释       |
| ------ | ------------------------------------ | -------------- |
| SQL    | Structured Query Language            | 结构化查询语言 |
| CVE    | Common Vulnerabilities and Exposures | 公共漏洞和暴露 |
| DML    | Data Manipulation Language           | 数据操纵语言   |
| DDL    | Data Definition Language             | 数据定义语言   |
| DCL    | Data Control Language                | 数据控制语言   |
| DS     | Data Studio                          | 数据库管理工具 |
| CM     | Cluster Management                   | 集群管理工具   |

 

***


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 5.1.0 Preview版本新增内核场景化、工具链和兼容性等特性（具体特性名可以查看"测试版本说明"章节中的需求列表），并对若干关键缺陷进行了修改。本文主要描述了openGauss 5.1.0 Preview版本整体测试情况，结合[openGauss 5.1.0 Preview版本测试策略](https://gitee.com/opengauss/QA/blob/master/Test_Strategy/openGauss_5.0.0_Release/openGauss%205.0.0%20Release%E7%89%88%E6%9C%AC%E6%B5%8B%E8%AF%95%E7%AD%96%E7%95%A5.md)，展开相应的测试活动。重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 5.1.0 Preview版本整体质量良好。

# 测试版本说明

本章节描述测试版本的基本信息，包括测试对象是什么，以及在什么环境下开展的测试，具体包括被测版本和测试硬件环境。

描述被测对象的版本信息和测试的时间及测试轮次。

| 版本名称                     | 测试起始时间 | 测试结束时间 |
| ---------------------------- | ------------ | ------------ |
| openGauss 5.1.0 Preview RC1  | 2023/4/12    | 2023/4/18    |
| openGauss 5.1.0 Preview RC2  | 2023/4/19    | 2023/4/25    |
| openGauss 5.1.0 Preview RC3  | 2023/5/10    | 2023/5/16    |
| openGauss 5.1.0 Preview RC4  | 2023/5/24    | 2023/5/30    |
| openGauss 5.1.0 Preview RC5  | 2023/5/31    | 2023/6/6     |
| openGauss 5.1.0 Preview RC6  | 2023/6/7     | 2023/6/13    |
| openGauss 5.1.0 Preview RC7  | 2023/6/14    | 2023/6/20    |
| openGauss 5.1.0 Preview RC8  | 2023/6/21    | 2023/6/30    |
| openGauss 5.1.0 Preview RC9  | 2023/7/7     | 2023/7/11    |
| openGauss 5.1.0 Preview RC10 | 2023/7/12    | 2023/7/18    |
| openGauss 5.1.0 Preview RC11 | 2023/7/19    | 2023/7/25    |
| openGauss 5.1.0 Preview RC12 | 2023/7/26    | 2023/8/1     |
| openGauss 5.1.0 Preview RC13 | 2023/8/2     | 2023/8/8     |
| openGauss 5.1.0 Preview RC14 | 2023/8/9     | 2023/8/15    |
| openGauss 5.1.0 Preview RC15 | 2023/8/16    | 2023/8/22    |
| openGauss 5.1.0 Preview RC16 | 2023/8/23    | 2023/9/1     |
| openGauss 5.1.0 Preview RC17 | 2023/9/2     | 2023/9/12    |
| openGauss 5.1.0 Preview RC18 | 2023/9/13    | 2023/9/19    |
| openGauss 5.1.0 Preview RC19 | 2023/9/20    | 2023/9/27    |

描述本次测试的测试环境（包括环境软硬件版本信息，环境组网配置信息， 测试辅助工具等）。

| 硬件型号                 | 硬件配置信息                                                 | 备注 |
| ------------------------ | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280) | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280) | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 22.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2480) | CPU：Kunpeng-920 7260 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| RH2288H V3               | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4*10GE |      |

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

openGauss 5.1.0 Preview版本是openGauss社区继续联合多方力量推出的版本，本次发布的需求列表如下：

| 任务标题                                                     | 开发主体      | 测试主体 |
| ------------------------------------------------------------ | ------------- | -------- |
| 【MySQL兼容性】DataStudio支持新数据类型的映射                | Plugin        | QA       |
| 【MySQL兼容性】支持数据类型显式/隐式转换规则                 | Plugin        | QA       |
| 【MySQL兼容性】导入导出、备份恢复工具支持MySQL兼容性         | Plugin        | QA       |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（增量迁移）     | Tools         | QA       |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（反向迁移）     | Tools         | QA       |
| 【迁移工具】一体化平台集成portal，支持可视化迁移管理         | Tools         | QA       |
| 升级后的数据字典校验                                         | Tools         | QA       |
| ODBC windows版本发布                                         | Tools         | QA       |
| mysql prepare-execute协议兼容                                | Plugin        | QA       |
| 【资源池化】支持XLog按需回放，优化DMS Reform流程，实现RTO<1min | StorageEngine | QA       |
| 【迁移工具】支持源端和目的端都是openGauss的校验              | Tools         | QA       |
| 【京东】【SQL兼容】B兼容模式支持函数                         | Plugin        | QA       |
| 【内核性能】内核GCC版本升级                                  | StorageEngine | QA       |
| 【内核性能】支持shared_buffer按大页内存分配                  | StorageEngine | QA       |
| 【资源池化】支持XLog按需回放                                 | StorageEngine | QA       |
| 【资源池化】支持事务内写转发                                 | StorageEngine | QA       |
| 【迁移工具】数据校验性能提升                                 | Tools         | QA       |
| 【DataKit】DataKit支持兼容性评估                             | Tools         | QA       |
| 【迁移工具】增量、反向迁移支持断点续传                       | Tools         | QA       |
| 【迁移工具】反向迁移支持全量迁移                             | Tools         | QA       |
| 【MySQL兼容性】grant需支持对函数做赋权操作                   | Plugin        | QA       |
| 【迁移工具】全量迁移工具支持读取csv文件用于数据迁移          | Tools         | QA       |
| 【内核】alter表和索引修改压缩属性                            | SQLEngine     | QA       |
| 【资源池化】分布式存储算子卸载加速TPC-H性能提升30%           | StorageEngine | QA       |
| 【内核高可用】主备高可用能力增强                             | StorageEngine | QA       |
| 【资源池化】DMS Reform优化加固                               | StorageEngine | QA       |
| 【资源池化】DSS功能补齐                                      | StorageEngine | QA       |
| 【资源池化】sysbench场景性能优化，2节点 2.5W tps、线性度达到1.4倍 | StorageEngine | QA       |
| 【资源池化】备机支持缓存快照信息，快照有推进时才向主机获取最新的 | StorageEngine | QA       |
| 【内核】存储过程需要支持嵌套调试，需要支持调试匿名块         | StorageEngine | QA       |
| 【内核】子事务并发回滚流程优化                               | StorageEngine | QA       |
| 【内核】支持在A兼容性模式下对空串的处理                      | SQLEngine     | QA       |
| 【内核】SQL引擎解耦化，实现O兼容性基础插件                   | SQLEngine     | QA       |
| 【迁移工具】chameleon解除跨平台的依赖                        | Tools         | QA       |
| 【迁移工具】全量迁移工具能力增强                             | Tools         | QA       |
| 【OM工具】OM工具解耦对OS版本的依赖                           | Tools         | QA       |
| 【资源池化】备机可见性逻辑优化、主机推进oldestxid逻辑优化    | StorageEngine | QA       |
| 【MySQL兼容性】设置字符集字符序、指定常量字符集、多字符集表达式、set类型指定字符序、gbk、gb18030字符序支持创建事务时开启一致性快照、支持存储过程resignal语法和signal的mysql语法兼容性特 | Plugin        | QA       |
| 【内核】GB18030-2022标准支持                                 | StorageEngine | QA       |
| 【内核】极致RTO支持备机可读，且维持一主一备70W tpmC时，RTO<10s的规格不变 | StorageEngine | QA       |
| 【内核】CAS性能优化                                          | StorageEngine | QA       |
| 【中国联通】兼容MySQL 5.7协议增强                            | Plugin        | QA       |
| 【内核】发布订阅支持冲突解决方案                             | StorageEngine | QA       |
| 【云和恩墨】M兼容性：别名带单引号                            | Plugin        | QA-恩墨  |
| 【云和恩墨】cmake的小型化openGauss编译过程支持dolphin        | StorageEngine | QA-恩墨  |
| 【云和恩墨】【特性提交】tassl 支持TLCP                       | StorageEngine | QA-恩墨  |
| 【云和恩墨】【资源池化】资源池化后的性能视图                 | SQLEngine     | QA-恩墨  |
| 【云和恩墨】【主备复制】并行apply优化                        | StorageEngine | QA-恩墨  |
| 【国创】DataKit 业务开发增强                                 | Tools         | QA-国创  |
| 【国创】DataKit 智能运维增强                                 | Tools         | QA-国创  |
| [海量数据：兼容M*]创建视图支持sql security语法               | Plugin        | QA-海量  |
| [海量数据：兼容M*]存储过程和函数只带select语句时，支持call调用 | Plugin        | QA-海量  |
| [海量数据：兼容M*]支持DIAGNOSTICS                            | Plugin        | QA-海量  |
| [海量数据：兼容M*]select语句支持ignore index                 | Plugin        | QA-海量  |
| [海量数据]【资源池化】 DSS支持NoF/NoF+接口                   | StorageEngine | QA-海量  |
| 【移动】（M*兼容）支持trigger建在指定schema中，并校验schema和table schema是否一致 | Plugin        | QA-移动  |
| 【移动】（M*兼容）支持常用的performance_schema视图           | Plugin        | QA-移动  |
| 【移动】（M*兼容）支持show engines等4个语法                  | Plugin        | QA-移动  |
| 【移动】（DFX）支持在switchover超时时，打印数据库内核的堆栈  | Tools         | QA-移动  |
| 【移动】（M*兼容）json支持=、>=、<=、>、<、&等操作符         | Plugin        | QA-移动  |
| 【移动】（M*兼容）支持SHOW REPLICAS和SHOW REPLICA STATUS     | Plugin        | QA-移动  |

openGauss 5.1.0 Preview版本测试活动分工如下：

| 任务标题                                                     | 开发主体      | 测试主体 | 验证策略                                                     |
| ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 【MySQL兼容性】DataStudio支持新数据类型的映射                | Plugin        | QA       | 1、验证DataStudio支持编辑新增数据类型year, uint1/uint2/uint4/uint8； binary/varbinary; tinyblob/mediumblob/longblob; bool/set/enum 2、验证新增数据类型create、insert、update、delete、alter等操作正常 |
| 【MySQL兼容性】支持数据类型显式/隐式转换规则                 | Plugin        | QA       | 1、验证二进制、数字、浮点、字符串（set、enum）、时间、json等类型支持显式\隐式转换 2、CAST函数入参数据类型支持显式\隐式转换 |
| 【MySQL兼容性】导入导出、备份恢复工具支持MySQL兼容性         | Plugin        | QA       | 1、逻辑导入导出工具（gs_dump、gs_dumpall、gs_restore）支持对象、数据类型、DDL语法等兼容Mysql 2、物理备份恢复工具（gs_basebackup、gs_probackup）支持对象、数据类型、DDL语法等兼容Mysql |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（增量迁移）     | Tools         | QA       | 1、增量迁移支持迁移进度展示（总量、剩余量、剩余时间等） 2、增量迁移支持迁移失败数据、DDL失败语句展示 |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（反向迁移）     | Tools         | QA       | 1、反向迁移支持迁移进度展示（总量、剩余量、剩余时间等） 2、反向迁移支持迁移失败数据、DDL失败语句展示 |
| 【迁移工具】一体化平台集成portal，支持可视化迁移管理         | Tools         | QA       | 1、验证在平台执行全量、反向、增量等迁移任务成功 2、迁移任务可以建、迁移参数可配置 3、验证迁移任务可视化，迁移进度展示、结果展示和性能展示 |
| 升级后的数据字典校验                                         | Tools         | QA       | 1、升级后主机、备机支持新版本数据字典（表、数据类型和cast、视图、函数）等校验 2、不同版本带用户数据、不带用户数据升级后进行校验 3、升级后校验数据数量、结构、内容不丢失、不出现错误 |
| ODBC windows版本发布                                         | Tools         | QA       | 1、ODBC 驱动在windows环境可以安装 2、使用ODBC windows驱动连接数据库成功、操作数据库增删改查数据成功 |
| mysql prepare-execute协议兼容                                | Plugin        | QA       | 1、dolphin插件兼容M*协议的语法特性测试 2、覆盖了功能测试和资料测试 |
| 【资源池化】支持XLog按需回放，优化DMS Reform流程，实现RTO<1min | StorageEngine | QA       | 1、验证新增参数ss_parallel_thread_count配置可正常使用 2、验证Switchover场景下reform流程正常，集群无卡住现象，主备数据一致，无core文件产生，集群可恢复正常 3、验证重启failover、在线failover场景下，集群无卡住现象，主备数据一致，无core文件产生，集群可恢复正常 4、验证reform过程中dss、gaussdb等进程故障、二进制文件丢失、网络闪断等故障场景，集群合理报错，可恢复正常 |
| 【迁移工具】支持源端和目的端都是openGauss的校验              | Tools         | QA       | 1、验证openGauss数据库迁移至openGauss数据库后进行全量校验、增量校验数据类型、表类型正常 2、继承功能mysql迁移后全量校验成功 3、迁移后表丢失、数据丢失、表增加、数据增加等异常场景测试 |
| 【京东】【SQL兼容】B兼容模式支持函数                         | Plugin        | QA       | 1、测试B兼容模式下支持Mysql侧内置函数WEIGHT_STRING、ANTN、NAME_CONST、COMPRESS、UNCOMPRESS特性与Mysql侧一致 2、测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用。 |
| 【内核性能】内核GCC版本升级                                  | StorageEngine | QA       | 1、openeuler/centos x86使用gcc 10.3.0、openeuler arm环境使用 gcc10.3.1功能正常 2、使用新版本gcc安装、卸载、升级、长稳测试执行正常、集群数据一致无core产生 3、使用新版gcc性能测试，相比提升6% |
| 【内核性能】支持shared_buffer按大页内存分配                  | StorageEngine | QA       | 1、验证配置不同大页在开启关闭情况下数据库功能正常、查询结果正常 2、大页开启情况下长稳测试正常，数据一致，无core产生 3、验证不同操作系统开启大页的性能提升情况，在22.03操作系统性达成150W tpmc 4、openEuler、CentOS、红旗、麒麟等不同操作系统上开启大页，数据库功能正常，查询结果正常 |
| 【资源池化】支持XLog按需回放                                 | StorageEngine | QA       | 1、验证按需回放开启情况下100并发、50w tpmC下RTO < 1min 2、按需回放开启情况下进行switchover、failover操作数据一致、集群正常、无core产生 3、按需回放开启情况下进行操作系统、网络、磁盘、内存等可靠性测试，数据库正常恢复 |
| 【资源池化】支持事务内写转发                                 | StorageEngine | QA       | 1、覆盖功能测试，可靠性测试、稳定性测试和资料测试，使用gsql和jdbc连接数据库在事务内外执行DDL、DML等读写操作验证语句是否转发到主机执行 2、可磁盘故障和网络故障测试 3、备机运行24小时tpcc稳定性测试。 |
| 【迁移工具】数据校验性能提升                                 | Tools         | QA       | 1、预置Mysql数据，不同表个数、行数以及不同数据类情况下，数据校验功能正常 2、gs_datacheck工具配置功能，正常使用 3、验证数据校验性能由70MB/s提升为150MB/s |
| 【DataKit】DataKit支持兼容性评估                             | Tools         | QA       | 1、兼容性评估工具合法参数、非法参数测试 2、验证对表、函数、存储过程、触发器、视图等对象评估正确性 3、验证对sql文件、mybatis映射文件等sql评估正确性 |
| 【迁移工具】增量、反向迁移支持断点续传                       | Tools         | QA       | 1、验证增量迁移mysql迁移至openGauss，源端、目的端数据库down、网络异常等不同情，再次迁移支持断点续传 2、验证反向迁移mysql迁移至openGauss，源端、目的端数据库down、网络异常等不同情，再次迁移支持断点续传 3、验证sink端迁移参数有效性 4、验证迁移性能达到3W tps |
| 【迁移工具】反向迁移支持全量迁移                             | Tools         | QA       | 1、验证进度上报参数、断点续传参数等合法、非法配置情况 2、验证覆盖常见数据类型迁移情况，进度上报，断点续传功能 3、迁移过程中kafka、debezium注入进程故障、磁盘故障、网络故障、CPU故障测试 |
| 【MySQL兼容性】grant需支持对函数做赋权操作                   | Plugin        | QA       | 1、验证grant usage语句在无用户、已有用户、不同权限等场景下基本功能 |
| 【迁移工具】全量迁移工具支持读取csv文件用于数据迁移          | Tools         | QA       | 1、变色龙支持直接读取MySQL的csv文件导入到openGauss 2、读取错误格式文件合理报错 |
| 【内核】alter表和索引修改压缩属性                            | SQLEngine     | QA       | 1、验证将已有的非压缩表修改为压缩表、已有的压缩表修改成非压缩表、修改已有的压缩表压缩参数、ustore表压缩属性修改。 |
| 【资源池化】分布式存储算子卸载加速TPC-H性能提升30%           | StorageEngine | QA       | 1、配置文件参数使能，加载NdpPlugin插件、通过插件卸载算子查询，测试NdpPlugin插件正常使能/关闭场景下的数据库查询功能，LibSmartScan进程启动、关闭场景 2、白名单测试，支持Agg、SeqScan+Filter的逻辑在存储节点上运行，所有给予白名单的查询，算子识别下推则卸载对应特性返回查询结果，算子不识别下推则通过原生返回查询结果 3、Agg测试，对查询数据添加avg、count、max、min等聚合条件，并将结果返回 4、SeqScan测试，选取多列、单列的值，所有数据表的值，返回查询结果 |
| 【内核高可用】主备高可用能力增强                             | StorageEngine | QA       | 1、最大可用模式打开，设置不同的事务同步方式，ignore_standby_lsn_window参数在不同取值的功能验证 2、设置hot_standby_feedback取不同值，ignore_feedback_xmin_window在不同值的情况下功能验证 |
| 【资源池化】DMS Reform优化加固                               | StorageEngine | QA       | 1、验证switchover、failover场景下reform正常，集群可恢复正常，数据一致，无core文件产生 2、验证failover场景下RTO时间 3、一定业务压力情况下，增加操作系统、内存、磁盘、CPU、进程、二进制文件等故障操作，reform正常，集群可恢复正常，数据一致，无core文件产生 |
| 【资源池化】DSS功能补齐                                      | StorageEngine | QA       | 1、验证DSS黑匣子日志生成功能，以及新增参数_BLACKBOX_DETAIL_ON、IO_THREADS、WORK_THREADS设置以及取值验证 2、跑tpcc 100仓业务，验证一致性，通过 3、新增参数资料验证，通过 |
| 【资源池化】sysbench场景性能优化，2节点 2.5W tps、线性度达到1.4倍 | StorageEngine | QA       | 1、验证sysbench场景下tps指标                                 |
| 【资源池化】备机支持缓存快照信息，快照有推进时才向主机获取最新的 | StorageEngine | QA       | 1、验证打开及关闭参数主备快照一致等功能 2、验证打开及关闭参数集群可靠性 |
| 【内核】存储过程需要支持嵌套调试，需要支持调试匿名块         | StorageEngine | QA       | 1、覆盖了命令行的匿名块调试和嵌套调试，以及web_ds端的匿名块调试和嵌套调试功能 |
| 【内核】子事务并发回滚流程优化                               | StorageEngine | QA       | 1、验证多并发带子事务正常rollback、commit、abort 2、高并发进行大量带子事务的事务，过程中kill并发进程后再次进行高并发事务提交或者回滚，数据库功能正常，且保障事务ACID正确 3、从ProcArrayLock等锁次数和等锁时间、commit及rollback命令执行时间两个维度验证性能变化 |
| 【内核】支持在A兼容性模式下对空串的处理                      | SQLEngine     | QA       | 1、验证不同方式设置参数 2、空字符串原样显示及空字符串作为null处理下，结合表，作为常量、变量、函数入参及返回值验证、通过jdbc连接测试，copy to及copy from语句测试 |
| 【内核】SQL引擎解耦化，实现O兼容性基础插件                   | SQLEngine     | QA       | 1、覆盖函数功能测试、whale插件安装测试、guc参数测试及资料测试 |
| 【迁移工具】chameleon解除跨平台的依赖                        | Tools         | QA       | 1、chameleon跨平台的全量依赖，包括OS和python版本，出包正常，可正常安装 2、在不同平台进行chameleon安装 |
| 【迁移工具】全量迁移工具能力增强                             | Tools         | QA       | 1、迁移数据类型 2、同时迁移表和索引及分开迁移表和索引 3、通过配置is_skip_completed_tables参数，测试工具异常重启是否跳过已经迁移完成的表。使用chameleon工具将上述操作迁移到openGauss，保证迁移前后数据的一致性、有序性和完整性 4、性能测试 |
| 【OM工具】OM工具解耦对OS版本的依赖                           | Tools         | QA       | 1、配置、不配置osid.conf文件中对应的系统，直接安装数据库 2、资料测试 |
| 【资源池化】备机可见性逻辑优化、主机推进oldestxid逻辑优化    | StorageEngine | QA       | 1、GUC参数ss_txnstatus_cache_size设置以及取值验证 2、跑tpcc 100仓业务，验证一致性 3、验证网络故障，进程类故障 |
| 【MySQL兼容性】设置字符集字符序、指定常量字符集、多字符集表达式、set类型指定字符序、gbk、gb18030字符序支持创建事务时开启一致性快照、支持存储过程resignal语法和signal的mysql语法兼容性特 | Plugin        | QA       | 1、验证对设置字符集字符序SET NAMES、指定常量字符集_charset、多字符集表达式、set类型指定字符序、gbk、gb18030字符序、start transaction with consistent snapshot、存储过程resignal语法和signal语法语法与MySQL的兼容 |
| 【内核】GB18030-2022标准支持                                 | StorageEngine | QA       | GB18030-2022相关使用功能                                     |
| 【内核】极致RTO支持备机可读，且维持一主一备70W tpmC时，RTO<10s的规格不变 | StorageEngine | QA       | 1、验证极致RTO支持备机读 2、tpcc 业务，验证一致性            |
| 【内核】CAS性能优化                                          | StorageEngine | QA       | 1、tpcc 业务，验证一致性                                     |
| 【中国联通】兼容MySQL 5.7协议增强                            | Plugin        | QA       | 1、覆盖了协议下各函数功能测试和资料测试                      |
| 【内核】发布订阅支持冲突解决方案                             | StorageEngine | QA       | 1、功能测试包含GUC参数subscription_conflict_resolution取值验证 2、新增禁用订阅和跳过冲突事务语法测试，发生数据冲突后是否按设置的处理方式解决冲突测试 3、升级测试验证升级后支持本特性，回滚升级能正常执行 4、资料测试验证特性描述是否准确，示例正确 |
| 【云和恩墨】M兼容性：别名带单引号                            | Plugin        | QA-恩墨  | 1、别名带单引号语法测试、在select语句中测试、在delete语句中测试、应用测试以及在不同场景中测 |
| 【云和恩墨】cmake的小型化openGauss编译过程支持dolphin        | StorageEngine | QA-恩墨  | 1、使用build在/script/cmake_package_mini.sh脚本构建openGauss轻量版安装包, 并在脚本中实现dolphin插件基于cmake的轻量版安装包编译，覆盖了功能测试。 |
| 【云和恩墨】【特性提交】tassl 支持TLCP                       | StorageEngine | QA-恩墨  | 1、在客户端和服务器同时开启tlcp开关，配置正确的加密套件值，相关证书和私钥需要为国密算法生成 2、测试由国密算法生成的证书进行安全认证和安全通信，及其建立连接时握手成功或失败的场景 |
| 【云和恩墨】【资源池化】资源池化后的性能视图                 | SQLEngine     | QA-恩墨  | 1、测试不同场景下页面分布信息、节点reform信息、MES等待事件和DSS IO统计信息的查询功能测试 2、测试版本升级兼容性。 |
| 【云和恩墨】【主备复制】并行apply优化                        | StorageEngine | QA-恩墨  | 1、控制参数enable_time_report开关，测试wal回放的统计信息，开启或关闭回放优化功能测试 2、自定义starup线程移交批次数量测试 3、自定义startup线程隔多长时间移交一次缓存的wal记录测试 |
| 【国创】DataKit 业务开发增强                                 | Tools         | QA-国创  | 1、覆盖了调试函数/过程、覆盖率统计、创建表、管理表、管理表数据、模式 (创建、编辑、删除)、导出数据和DDL（导出函数/过程 DDL、导出表DDL、导出表 DDL 和数据、导出表数据、导出序列 DDL、导出序列 DDL 和数据、导出视图 DDL、导出模式DDL、导出模式DDL和数据）特性 |
| 【国创】DataKit 智能运维增强                                 | Tools         | QA-国创  | 1、覆盖了实例监控、历史数据诊断、告警监控的特性              |
| [海量数据：兼容M*]创建视图支持sql security语法               | Plugin        | QA-海量  | 1、视图、函数体和存储过程使用中支持sql security语法，不同权限的用户创建 |
| [海量数据：兼容M*]存储过程和函数只带select语句时，支持call调用 | Plugin        | QA-海量  | 1、存储过程中进行各种表类型的查询和dml操作，存储过程中对表同义词、视图、物化视图、视图同义词查询，存储过程中使用聚合函数、窗口函数、类型转换函数、日期处理函数、字符处理函数，存储过程中label与loop、while、repeat结合查询 2、自定义用户测试，在join、union、子查询、group by、connect by中使用@变量 |
| [海量数据：兼容M*]支持DIAGNOSTICS                            | Plugin        | QA-海量  | 1、直接调用测试，存储过程测试，自定义函数测试，导入导出测试，其他兼容模式下测试，自定义用户测试，大小写敏感场景 |
| [海量数据：兼容M*]select语句支持ignore index                 | Plugin        | QA-海量  | 1、各种索引类型中支持ignore index语法，DML操作中支持ignore index语法，各种分区表中支持ignore index语法 |
| [海量数据]【资源池化】 DSS支持NoF/NoF+接口                   | StorageEngine | QA-海量  | 1、dsscmd各项操作，建表使用段页式等，导入导出备份还原 2、高并发测试覆盖TPCC 100仓300并发，1000仓800并发等 |
| 【移动】（M*兼容）支持trigger建在指定schema中，并校验schema和table schema是否一致 | Plugin        | QA-移动  | 1、测试triger特性与Mysql侧一致 2、测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用。 |
| 【移动】（M*兼容）支持常用的performance_schema视图           | Plugin        | QA-移动  | 1、测试performance_schema视图与Mysql侧一致 2、测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用。 |
| 【移动】（M*兼容）支持show engines等4个语法                  | Plugin        | QA-移动  | 1、测试show engines等4个语法与Mysql侧一致 2、测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用。 |
| 【移动】（DFX）支持在switchover超时时，打印数据库内核的堆栈  | Tools         | QA-移动  | 1、测试在不同情况下switchover超时打印信息是否完整            |
| 【移动】（M*兼容）json支持=、>=、<=、>、<、&等操作符         | Plugin        | QA-移动  | 1、测试兼容MySQL的json支持不同操作符功能 2、覆盖参数测试、json支持的数据类型进行操作符测试 3、json与其他数据类型支持操作符测试 4、json中某个字段进行操作比较测试，在不同语法中执行多场景测试 |
| 【移动】（M*兼容）支持SHOW REPLICAS和SHOW REPLICA STATUS     | Plugin        | QA-移动  | 1、测试兼容MySQL的SHOW REPLICAS功能，显示有关作为副本连接的服务器的信息，测试结果的每一行对应于一个副本服务器 |

# 版本概要测试结论

openGauss 5.1.0 Preview版本整体测试按照release-manager团队的计划，版本测试规划采取14+4+1的测试方式，即14轮系统测试+4轮集成验证+1轮回归测试的策略，实际完成了14轮系统测试+4集成验证+1轮回归测试：

阶段一：

第一轮~第六轮集中评审了新特性测试设计，同时关注内核场景化、工具链、资源池化和兼容性三个方面的新特性验收，并对关键性能数据进行了摸底，旨在发现阻塞性问题，而且对继承特性进行了自动化看护。并针对630阶段性目标简单进行第七轮集成测试，主要覆盖可靠性、性能、安全等集成测试。关注所有阶段一交付新特性的验收进展，对每个特性的测试报告进行了评审。

阶段二：

第八轮~第十五轮集中测试930阶段内核场景化、工具链、资源池化和兼容性新特性验收。第十六轮启动集成测试与最后一轮转测的需求验收，展开性能、安全、升级、可靠性、长稳、内存测试等专项测试，意在检查系统集成的完整性和正确性。并启动7*24H长稳测试。第十八轮测试紧接着又覆盖了一轮7\*24H长稳测试，保证社区版本的稳定性。此外，针对特性的测试报告测试人员也开始撰写。第十九轮转测后主要集中与问题单回归与版本发布准备，并例行展开自动化测试，对关键指标、功能进行看护，防止修改引入新的问题。

openGauss 5.1.0 Preview版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本计划交付特性65个，实际交付65个，交付率100%，所有发布需求均验证通过。针对长稳、可靠性等场景，后续需要持续测试以增强竞争力。

openGauss 5.1.0 Preview版本共发现问题1436个，有效问题1160个。修复问题回归测试结果正常，版本整体质量符合创新版本要求。遗留7个问题，见附件1遗留问题列表。

# 版本详细测试结论

openGauss 5.1.0 Preview版本详细测试内容包括：

1、通过自动化看护，从接入层、SQL层、存储层、管理、安全、可靠可用和工具等7个维度进行openGauss继承特性测试，继承功能无丢失；

2、在内核场景化的竞争力构建上，主要是对资源池化场景进行增强，如reform优化、DSS功能补齐、按需回放、sysbench场景性能优化等......测试覆盖上述需求，关注功能的实现和规格的达成。但后续仍需从资源池化可靠性、竞争力角度出发，持续进行加固测试和关键指标验证；

3、在工具链方面，覆盖Datakit、CM和OM新特性测试，同时迁移工具也在此次发布版本有了更高的规格表现：全量迁移300MB/S、增量迁移3w tps、反向迁移1w tps以及全量校验70MB/S。不过后续仍需对工具的易用性和稳定性提高要求；

4、在M\*兼容性方面，测试新增数据类型、系统函数、DDL、DML、PL/SQL、SHOW语法和其他语法等需求，M\*兼容性进一步提升；

5、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试等，基本满足7\*24H正常运行，在增加业务模型、部署节点、压力、兼容性模式后可能存在问题，后续需要从这些方面增加测试以增强竞争力；

6、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 特性测试结论

### 继承特性评价

对产品所有继承特性进行评价，用表格形式评价，包括特性列表（与特性清单保持一致），验证质量评估

| Domain   | Feature                | 质量评估                   | 备注                                                         |
| -------- | ---------------------- | -------------------------- | ------------------------------------------------------------ |
| 接入层   | 应用开发接口           | <font color=green>▮</font> | 继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动             |
|          | 分布式中间件           | <font color=green>▮</font> | 继承已有测试能力，支持应用侧分布式解决方案shardingsphere     |
| SQL层    | 支持SQL语言版本SQL2003 | <font color=green>▮</font> | 继承已有测试能力，支持SQL 2003语言版本                       |
|          | 支持SQL基本语法        | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句                    |
|          | 存储过程               | <font color=green>▮</font> | 继承已有测试能力，关注声明语法、基本语句、动态语句、控制语句、事务管理和调试等 |
|          | 系统表和系统视图       | <font color=green>▮</font> | 继承已有测试能力                                             |
|          | DBE_PERF内视图         | <font color=green>▮</font> | 继承已有测试能力，关注OS、Instance、Memory等多个维度划分组织视图，DBE_PERF内视图主要用来诊断性能问题 |
|          | SQL关键字              | <font color=green>▮</font> | 继承已有测试能力，关注保留字和非保留字之分。根据标准，保留字决不能用做其他标识符。非保留字只是在特定的环境里有特殊的含义，而在其他环境里是可以用做标识符的 |
| 存储层   | 索引                   | <font color=green>▮</font> | 继承已有测试能力，支持B Tree索引，Hash索引等                 |
|          | 数据分区               | <font color=green>▮</font> | 继承已有测试能力，支持对分区的组合、交换、拆分、合并等功能；支持范围、列表、哈希等分区类型；支持二级分区 |
|          | 行列混合存储           | <font color=green>▮</font> | 继承已有测试能力，支持行存、列存两种方式，可以在不同的引用场景下选择相应的存储方式 |
|          | 内存表                 | <font color=green>▮</font> | 继承已有测试能力，支持MOT                                    |
|          | 物化视图               | <font color=green>▮</font> | 继承已有测试能力，支持物化视图，物化视图是一种特殊的物理表，物化视图是相对普通视图而言的 |
|          | 全文索引               | <font color=green>▮</font> | 继承已有测试能力，支持单字及词查询，长句子检索；支持中英文混排检索；支持and、or及not的条件组合检索 |
| 管理     | 命令行安装             | <font color=green>▮</font> | 继承已有测试能力，支持OM安装，极简安装和编译安装             |
|          | 版本升级               | <font color=green>▮</font> | 继承已有测试能力，支持数据库软件版本的升级功能，包括就地升级和灰度升级 |
|          | 数据库对象管理         | <font color=green>▮</font> | 继承已有测试能力，支持图形化的方式对模式、表、视图、用户、角色、数据库对象、存储过程、约束、函数、触发器、自增列、大对象等的管理 |
|          | 诊断分析报告           | <font color=green>▮</font> | 继承已有测试能力，支持生成WDR等报告                          |
|          | 智能运维               | <font color=green>▮</font> | 继承已有测试能力，具备智能化运维能力，如智能参数调优、智能索引推荐、慢SQL诊断分析、存储空间智能预测等 |
| 安全     | 身份鉴别               | <font color=green>▮</font> | 继承已有测试能力，支持定义鉴别失败的次数，用户具有唯一标识性 |
|          | 访问控制               | <font color=green>▮</font> | 继承已有测试能力，登录密码存储支持高级别密码存储加密         |
|          | 安全接入               | <font color=green>▮</font> | 继承已有测试能力，具备基于RFC5802机制的口令加密认证方法。加密认证过程中采用单项Hash不可逆加密算法PBKDF2，可防止彩虹攻击 |
|          | 强制访问控制           | <font color=green>▮</font> | 继承已有测试能力，支持强制访问控制：主体粒度达到用户级，客体粒度达到表、视图、触发器等数据库对象级或记录级及列级 |
|          | 安全审计               | <font color=green>▮</font> | 继承已有测试能力，支持安全审计，支持检测安全侵害事件，能将审计数据计入审计日志中 |
|          | 三权分立               | <font color=green>▮</font> | 继承已有测试能力，支持系统管理员、安全管理员和审计管理员角色，三种角色计入审计日志中 |
|          | 数据存储加密           | <font color=green>▮</font> | 继承已有测试能力，支持对用户数据的加密存储或其他有效性的保护措施 |
|          | 全密态数据库           | <font color=green>▮</font> | 继承已有测试能力，支持存储，分析密文数据的数据库系统技术，数据在传输、存储以及计算态均以加密形态存在，整个查询分析过程对用户无感知 |
|          | 动态脱敏               | <font color=green>▮</font> | 继承已有测试能力，支持数据库管理员在数据列上创建动态数据脱敏策略 |
|          | 防篡改功能             | <font color=green>▮</font> | 继承已有测试能力，具备多方共识防篡改、高效防篡改算法、账本数据分布式存储 |
|          | 恶意攻击防范           | <font color=green>▮</font> | 继承已有测试能力，支持system防命令注入，可防止缓冲区溢出；输入有效性检查 |
| 可靠可用 | 物理备份恢复           | <font color=green>▮</font> | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|          | 全量PITR物理恢复       | <font color=green>▮</font> | 继承已有测试能力，全量PITR物理恢复                           |
|          | 逻辑导入导出           | <font color=green>▮</font> | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|          | 延迟备份               | <font color=green>▮</font> | 继承已有测试能力，支持延迟备份                               |
|          | 逻辑复制               | <font color=green>▮</font> | 继承已有测试能力，支持异构数据库间的数据同步                 |
|          | 极致RTO                | <font color=green>▮</font> | 继承已有测试能力，支持快速故障倒换能力，实现RTO<10s          |
| 工具     | 客户端工具             | <font color=green>▮</font> | 继承已有测试能力，支持gsql工具、DS工具                       |
|          | 服务端工具             | <font color=green>▮</font> | 继承已有测试能力，关注gs_cgroupgs_check、gs_checkos、gs_checkperf、gs_collector、gs_dump、gs_dumpall、gs_guc、gs_encrypt、gs_om、gs_plan_simulator、gs_restore、gs_ssh等工具 |
|          | 系统内部使用工具       | <font color=green>▮</font> | 继承已有测试能力，关注gaussdb、gs_backup、gs_basebackup、gs_ctl、gs_expansion、gs_initdb、gs_install、gs_postuninstall、gs_preinstall、gs_dropnode、gs_sshexkey、gs_tar、gs_uninstall、gs_upgradectl、gstrace、pg_recvlogical、gs_probackup 等工具 |
|          | 迁移工具               | <font color=green>▮</font> | 继承已有测试能力，支持全量、增量迁移以及反向迁移             |
|          | 校验工具               | <font color=green>▮</font> | 继承已有测试能力，支持全量、增量数据校验能力                 |
| 兼容     | 数据类型兼容           | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的数据类型 |
|          | 函数兼容               | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的系统函数 |
|          | DDL兼容                | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的DDL语法  |
|          | DML兼容                | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的DML语法  |
|          | DCL兼容                | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的DCL语法  |
|          | PL/SQL兼容             | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的PL/SQL语法 |
|          | 其他语法兼容           | <font color=green>▮</font> | 继承已有测试能力，关注在对应兼容模式下，支持已兼容的其他语法 |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估，给出特性质量评估结论。

| 任务标题                                                     | 测试情况说明                                                 | 约束                                                         | 质量点灯                    | 遗留问题 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | -------- |
| 【MySQL兼容性】DataStudio支持新数据类型的映射                | DataStudio支持新数据类型的映射特性测试中，共计执行用例204个，主要涉及常用类型的修改权限验证及新增兼容类型的建表及修改权限验证，共发现4个问题，其中1个问题单经评审后取消，其余问题单均已回归通过，无遗留风险，整体质量良好。 | 兼容M* 5.7版本 支持新增类型为兼容M*侧数据类型，需要连接兼容b库执行 | <font color=green>▮</font>  |          |
| 【MySQL兼容性】支持数据类型显式/隐式转换规则                 | 支持数据类型隐式显式转换规则特性测试中，共计执行用例329个，主要涉及函数及语法的功能验证、及文档验证，共发现12个问题，其中2个问题单经评审已取消，剩余问题单已全部解决并回归通过，整体质量良好，无遗留风险。 | 需在openGauss中兼容B库下验证 部分类型间转换规则需设置b_compatibility_mode = on 部分类型间转换不带引号输入负数数据结果与M*侧不同 | <font color=green>▮</font>  |          |
| 【MySQL兼容性】导入导出、备份恢复工具支持MySQL兼容性         | 导入导出、备份恢复工具支持M兼容特性，共计执行70个用例，主要覆盖了功能测试和资料测试。功能测试覆盖了逻辑导入导出工具（gs_dump、gs_dumpall、gs_restore）和物理备份恢复工具（gs_basebackup、gs_probackup）支持M兼容特性，导入导出数据一致；资料测试验证描述是否准确，示例的执行结果是否正确。测试发现6个问题，6个已解决且回归通过，整体质量较好。 | B兼容性数据库+dolphin插件 show create procedure/function等语法的database_collation和collation_connection与数据库lc_collate相同，由于InitSession会重新初始化lc_collate参数，所以show create procedure/function的database_collation和collation_connection两列值不稳定 临时表，预备语句等不支持导入导出 | <font color=green>▮</font>  |          |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（增量迁移）     | MySQL数据迁移完整方案打通_增量迁移，共执行用例110个，主要覆盖了功能测试、性能测试、资料测试。功能测试覆盖展示迁移进度文件和不展示迁移进度文件开启增量迁移，在MySQL侧执行DDL及DML，使用增量迁移工具迁移到openGauss，主要观察是否有迁移进度文件生成。性能测试，在高性能机器下，开启增量迁移，通过sysbench工具给MySQL压测数据，待数据回放完成，计算迁移效率。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单7个，7个缺陷已解决，回归通过，整体质量良好。 | MySQL参数设置： log_bin=ON， binlog_format=ROW,，binlog_row_image=FULL,，gtid_mode = ON，若未设置gtid_mode = OFF，则sink端回放退化为串行回放，会降低在线迁移性能 3、增量迁移是直接透传DDL的，对于openGauss侧语法和MySQL侧语法不一样的，迁移失败 4、启动增量迁移，可同时启动source端和sink端，也可以分开启动，先启动source端，再启动sink端，confluent/confluent-5.5.1/etc/schema-registry/connect-avro-standalone.properties文件rest.port需配置不同端口 5、为保证事务的顺序性，kafka_2.13-3.2.3/config/server.properties文件num.partitions参数必须设置为1 6、性能测试对硬件系统有较高要求，仅insert和混合场景迁移性能达到3w tps且必须进行性能调优 mysql高性能配置，binlog位置、安装目录、数据目录分别部署在3个不同的NVME盘 openGauss侧openGauss高性能配置，pg_xlog、安装目录、数据目录分别部署在3个不同的NVME盘 mysql-source.properties配置文件parallel.parse.event=true 在java11环境上运行在线迁移工具 为减少日志刷屏，可设置confluent/confluent-5.5.1/etc/kafka/connect-log4j.properties文件日志级别为WARN | <font color=green>▮</font>  |          |
| 【迁移工具】迁移工具可调试性增强、可靠性增强（反向迁移）     | 迁移工具可调试性增强、可靠性增强（反向迁移），共执行用例115个，主要覆盖了功能测试、性能测试、可靠性测试、资料测试。功能测试覆盖在迁移进度打开与关闭时，开启反向迁移，在openGauss数据库分别执行增删改操作，使用反向迁移工具将上述DML操作迁移到MySQL，保证迁移前后数据的一致性、有序性和完整性。反向迁移工具支持迁移进度展示，反向迁移支持迁移失败的数据展示。性能测试，在高性能机器下，开启反向迁移，通过sysbench工具给openGauss压测数据，待数据回放完成，计算迁移效率。可靠性测试覆盖迁移中，是否支持修改迁移进度文件。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单8个，8个缺陷均已解决且回归通过，整体质量良好。 | 1、MySQL 5.7版本，数据库除初始用户外的数据库用户均有复制权；配置文件添加以下参数： MySQL参数设置： log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON 2、openGauss侧的目标库为兼容B库，仅限初始用户和拥有REPLICATION权限的用户进行操作 3、迁移前需要开启数据库的逻辑复制相关配置：wal_level = logical，ssl=on并配置pg_hba.conf文件的白名单 4、支持openGausss的多个schema下的数据迁移到指定的MySQL的多个库 5、Sink端支持数据按表进行并发回放 6、反向迁移connector端配置连接数据库的用户需要有对应数据库下所有schema以及所有表的操作权限 7、支持openGauss端对schema下的数据的dml操作同步到MySQL端，不支持迁移ddl操作的迁移 8、仅支持在线迁移，不支持离线全量迁移 9、source端的xlog.location参数使用前提用户已在源端数据库建立逻辑复制槽和发布 10、source端和sink端配置文件中，迁移进度若打开，其他相关参数配置不合法，迁移成功，上报迁移进度，不合法的参数值将会置为默认值 11、性能测试对硬件系统有较高要求，必须进行性能调优 MySQL高性能配置，binlog位置、安装目录、数据目录分别部署在3个不同的NVME盘 openGauss侧高性能参数配置 反向迁移进程绑核启动 | <font color=green>▮</font>  |          |
| 【迁移工具】一体化平台集成portal，支持可视化迁移管理         | 共计执行637条用例，主要覆盖了功能测试、可靠性测试、兼容性测试、易用性测试、资料测试。功能测试覆盖首页、资源中心、安装部署、基础运维、数据迁移、插件管理、安全中心、日志中心、平台基础功能，资料测试覆盖校验资料的描述及示例的执行结果是否成功。累计发现issue98个，91个已验收通过，7个缺陷未解决，整体质量一般。 | 1、平台运行依赖于openJdk11 2、仅支持openGauss数据库，需要提前创建database 3、将部署服务器IP配置在平台使用的数据库（openGauss）的白名单列表中 4、使用平台默认的登录账号密码，首次登录后及时修改密码 5、源数据库需要开启复制功能，在配置中增加以下配置参数，并重启。 | <font color=yellow>▲</font> |          |
| 升级后的数据字典校验                                         | gs_upgradechk升级校验工具，共计执行21条用例，主要覆盖了功能测试和资料测试。功能测试覆盖gs_upgradechk的导出元数据校验地图功能，以及基于元数据校验并生成报告的功能，还在升级过程中测试gs_upgradechk的相关功能。累计发现缺陷单5个，5个问题单均已修复，整体质量良好。 | gs_upgradechk工具目前只支持openGauss版本：3.0.3+，5.0.X，5.1.X。 不指定vmap参数时，会自动检测目标数据库版本并在网上下载和使用我们生成好的vmap，需要机器网络正常。 | <font color=green>▮</font>  |          |
| ODBC windows版本发布                                         | 共设计22个测试用例，主要包含Windows环境下，ODBC安装及配置、ODBC连接openGauss数据库参数校验、连接成功后常见DDL/DML/DCL语句执行等。共计发现6个问题，其中4个问题开发分析和linux下odbc表现一致，非问题取消，其它2个问题均已修复且回归验证通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| mysql prepare-execute协议兼容                                | 基于dolphin插件兼容M*协议的语法特性测试，共计执行50条用例，主要覆盖了功能测试和资料测试。累计发现缺陷单5个，其中1个缺陷单已取消，剩余所有缺陷已解决，回归通过，无遗留风险，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【资源池化】支持XLog按需回放，优化DMS Reform流程，实现RTO<1min | 优化DMS reform流程共计执行用例192条，主要覆盖功能测试，可靠性测试，性能、稳定性测试和资料测试。功能测试包含新增GUC参数设置测试，开启reform并行后reform时间验证；可靠性测试复用执行现有资源池化可靠性用例，性能测试为比对开启与关闭reform并行对性能无影响；开启reform并行后7*24小时稳定性测试。测试发现问题4个，均已回归通过，无遗留风险，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【迁移工具】支持源端和目的端都是openGauss的校验              | gs_datacheck数据校验工具支持源端和目的端都是openGauss的校验特性，共计执行66条用例，主要覆盖了功能测试和资料测试。功能测试覆盖源端为openGauss、目的端为M*以及源端和目的端均为openGauss的全量校验和增量校验，覆盖数据校验工具配置文件校验参数验证。可靠性测试覆盖校验中注入故障、kill校验相关进程。资料测试覆盖校验资料的描述与实际的功能是否一致，是否可按照示例正确执行校验。累计发现缺陷单6个，6个缺陷已解决且回归通过。 | 无                                                           | <font color=green>▮</font>  |          |
| 【京东】【SQL兼容】B兼容模式支持函数                         | B兼容模式支持M*内置函数，共计执行75条用例，主要覆盖了功能测试和资料测试。功能测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用等。资料测试验证资料描述及示例是否正确。累计发现缺陷单7个，缺陷已解决，回归通过，无遗留风险，整体质量良好。 | openGauss需使用兼容B库且含dolphin插件 atan(y,x)函数返回结果为double precison，因和M*侧精度处理不同，返回结果不一样 M*侧和openGauss侧对0x00的处理显示不相同，因此weight_string函数建议加hex函数输出结果 | <font color=green>▮</font>  |          |
| 【内核性能】内核GCC版本升级                                  | 共计执行测试用例34个，覆盖涉及功能、性能、资料测试，发现问题3个，均已验收通过，可靠性、长稳测试未发现问题，整体质量良好。 | 性能测试需在ARM+openEuler系统上执行，使用autfdo工具实现openGauss自动反馈调优 | <font color=green>▮</font>  |          |
| 【内核性能】支持shared_buffer按大页内存分配                  | 共计执行10个用例，主要覆盖了功能测试、可靠性测试、性能测试，发现issue4个，已验收通过3个，待办1个（openEuler22.03操作系统，大页内存2M场景下，性能无法达到150w），暂未解决，整体质量一般； | 无                                                           | <font color=yellow>▲</font> |          |
| 【资源池化】支持XLog按需回放                                 | 支持XLog按需回放特特性攻击执行用例47条，主要覆盖功能测试和性能测试。功能测试主要为参数测试、failover测试、数据一致性测试以及升级测试，参数测试正常赋值和异常赋值情况下特性功能正常；failover测试在主机故障后，保证failover成功，以及按需回放功能正常；数据一致性测试对比并行回放与按需回放后的文件checksum是否一致；升级测试完成就地升级和灰度升级，并且升级后按需回放功能可以正常使用。性能测试在一主一备部署，达成主机在100并发、TPCC负载50WtpmC、RTO<1min。本次测试发现问题8个，已解决8个，均回归通过，压力场景下可能存在可靠性、性能问题 | 无                                                           | <font color=yellow>▲</font> |          |
| 【资源池化】支持事务内写转发                                 | 共计执行用例81条，测试发现3个问题，均已验收通过，整体质量良好。 | 1、参数enable_remote_excute=on时，该特性才能生效 2、不支持备机事务內执行DDL语句、lock table、truncate table 3、如果备机事务內包含子事务，那么事务內的读也会转发到主机。 | <font color=green>▮</font>  |          |
| 【迁移工具】数据校验性能提升                                 | 共计执行用例45个，测试发现issue3个,已全部验收通过，整体质量一般。 | 1、openGauss侧的目标库为兼容B库，除初始用户外的数据库用户均有复制权 2、表须含主键 3、仅对数据的差异性及表字段名称的差异性进行数据校验，不对表结构及字段类型的差异性做校验 | <font color=green>▮</font>  |          |
| 【DataKit】DataKit支持兼容性评估                             | 实现兼容性语法评估特性测试中，共计执行用例46个，主要涉及mysqldump导出文件评估、用户自定义文件评估、jdbc连接mysql数据库语法及对象评估、mybatis映射文件评估、general_log、slow_log文件评估等，共发现7个问题，除一个文档问题单为待回归状态，其余问题单已回归通过，无遗留风险，整体质量良好。 | 评估存储过程、自定义函数等需使用delimiter将分隔符指定为//并在创建结束后使用delimiter将分隔符恢复为分号 输入自定义文件时，sql应以分号及空格进行分隔 自定义文件、general_log、slow_log、mybatis映射文件应手动指定.sql、.general、.slow、.xml后缀 输出文件只能制定.html格式，若不指定则默认输出至report.html中 | <font color=green>▮</font>  |          |
| 【迁移工具】增量、反向迁移支持断点续传                       | 迁移工具增量、反向迁移支持断点续传，共执行用例55个，主要覆盖了功能测试、性能测试、资料测试。功能测试主要覆盖sink端参数有效性，sink端和source端数据库挂掉、手动kill sink端和sourc端进程再恢复后数据是否能完成迁移且不重复。性能测试，在高性能机器下，开启增量迁移，通过sysbench工具压测数据，待数据回放完成，计算迁移效率。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单4个，4个缺陷已解决，回归通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【迁移工具】反向迁移支持全量迁移                             | 反向迁移支持全量迁移特性，共计执行84条用例，主要覆盖了功能测试和资料测试。功能测试覆盖验证迁移不同数据类型，验证迁移数据的有序性、一致性，验证迁移工具配置文件参数有效性，验证反向全量迁移与反向增量迁移是否正常衔接，验证进度上报文件准确性。可靠性测试覆盖迁移中注入故障、kill迁移相关进程后日志中是否正常执行或反馈相关异常，停止数据库或迁移进程后再启动，迁移是否可以断点续传。资料测试覆盖资料的描述与实际的功能是否一致，是否可按照示例正确执行校验。累计发现有效缺陷单6个，5个缺陷已解决且回归通过，1个缺陷未解决，除性能指标未达成外，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【MySQL兼容性】grant需支持对函数做赋权操作                   | grant需支持对函数做赋权操作特性测试中，共计执行用例25个，主要涉及语法中不同入参格式校验及密码是否脱敏等测试，未发现问题，无遗留风险，整体质量良好。 | 兼容M* 5.7版本 新增兼容语法为兼容M*侧，需要在兼容b库下执行 用户名输入为user@host格式需打开b_compatibility_user_host_auth = on | <font color=green>▮</font>  |          |
| 【迁移工具】全量迁移工具支持读取csv文件用于数据迁移          | 共计执行27条用例，主要覆盖了功能测试、性能测试、资料测试。功能测试验证了参数的有效性，能够通过配置迁移csv文件；性能测试验证，通过csv文件迁移速率高于原始迁移速率（10张表5000w数据测试数据），共计发现1个问题，已修复完成，整体质量良好。 | M*使用5.7版本，openGauss为兼容B库                            | <font color=green>▮</font>  |          |
| 【内核】alter表和索引修改压缩属性                            | 共设计91个测试用例，主要包含将已有的非压缩表修改为压缩表、将已有的压缩表修改成非压缩表、修改已有的压缩表压缩参数、ustore普通/分区/二级分区表压缩属性修改、结合其他业务场景的测试等，覆盖普通表、分区表、二级分区表。共计发现4个问题，均已修复且回归验证通过，整体质量良好。 | 本特性依赖已有的行存压缩特性，对于行存压缩的适用范围、行存压缩参数的使用范围与行存压缩特性相同，只支持对行存表修改压缩参数，并且必须满足已有的参数间的约束。 分区表不支持分区级别的存储属性，因此只能修改主表的压缩属性，修改后会对所有分区生效。 修改压缩属性时会重写整个表，期间对表加八级锁，此时该表不可读不可写。 修改压缩属性过程中会重建表，如果表的数据量较大，该过层可能花费较长时间。 修改压缩属性期间会重建表，openGauss会先生成新的数据文件再删除旧的数据文件，需要事先保证有足够的空闲物理空间。 | <font color=green>▮</font>  |          |
| 【资源池化】分布式存储算子卸载加速TPC-H性能提升30%           | 共计执行363个用例，主要覆盖了功能测试、性能测试；发现问题12个，已解决12个，回归通过，无遗留风险，整体质量良好； | （1）通过NdpPlugin插件的方式提供数据库内核算子卸载功能；  （2）计算侧依赖ceph集群环境&LibSmartScan服务实现算子下推；  （3）决定适合算子卸载的场景只有收益/可以卸载的场景才进行卸载，不能算子卸载的场景按照内核原有流程执行； | <font color=green>▮</font>  |          |
| 【内核高可用】主备高可用能力增强                             | 共计执行45个用例，主要覆盖了功能测试和可靠性测试，发现issue1个，已验收通过，整体质量良好。 | 改造点一：ignore_standby_lsn_window参数，需在最大可用模式most_available_sync配置为on场景下生效。 改造点二：ignore_feedback_xmin_window参数，需在hot_standby_feedback参数配置为on场景下生效。 | <font color=green>▮</font>  |          |
| 【资源池化】DMS Reform优化加固                               | 共计执行用例192条，测试发现问题4个，均已验收通过，整体质量良好。 | 资源池化架构下支持本特性                                     | <font color=green>▮</font>  |          |
| 【资源池化】DSS功能补齐                                      | 实现资源池化备机DSS黑匣子，DSS线程池相关特性测试特性测试中，主要涉及新增参数设取值为边界值异常值的功能验证、文档验证，一致性测试共发现3个问题，回归通过，无遗留风险，质量良好 | 无                                                           | <font color=yellow>▲</font> |          |
| 【资源池化】sysbench场景性能优化，2节点 2.5W tps、线性度达到1.4倍 | 单表6400w，256并发，主节点读写，备节点空载，单节点20min， TPS：2.8w 单表6400w，256并发，主节点读写，备节点只读，单节点20min 主节点TPS：2.3w，备节点TPS：3.6w | 无                                                           | <font color=green>▮</font>  |          |
| 【资源池化】备机支持缓存快照信息，快照有推进时才向主机获取最新的 | 共设计16个测试用例，主要包含ss_enable_bcast_snapshot参数的测试、验证主备快照一致等功能测试，打开及关闭ss_enable_bcast_snapshot对比性能测试，以及可靠性测试、资料测试等。未发现问题，整体质量良好。 | 开启该参数，会提升备机读性能，当集群中节点数比较少时，对主机性能也不会有太大影响，但是当集群中节点数量较多，比如超过3个，主机由于广播的节点数量变多会有性能影响；当某一个备机网络不通或故障时，会阻塞主机的事务提交。建议在读多写少的场景中选择开启该参数，提升备机读的性能。 | <font color=green>▮</font>  |          |
| 【内核】存储过程需要支持嵌套调试，需要支持调试匿名块         | 共计执行93个用例，主要覆盖了命令行的匿名块调试和嵌套调试，以及web_ds端的匿名块调试和嵌套调试功能，发现issue12个，其中2个问题为非问题，已取消，文档问题汇总在一个问题单，提示信息语法错误汇总在一个问题单，其中有两个问题待修复，经开发解释这两个问题需要到国创那边修改，且这两个问题是之前原函数/过程调试就存在的缺陷，不是本需求新引入的问题，其余问题已验收通过，整体质量良好； | 无                                                           | <font color=green>▮</font>  |          |
| 【内核】子事务并发回滚流程优化                               | 共计执行20个用例，主要覆盖了功能测试、可靠性测试、性能测试，发现问题0，整体质量良好。 | 1、本特性用于优化子事务并发回滚时，对于ProcArrayLock的锁竞争问题，对于同一事务内多个子事务的串行回滚没有优化 2、本特性对于子事务并发提交流程没有优化，在存在大量子事务并发提交/回滚的场景中，通过优化子事务回滚流程的锁竞争问题，减少子事务回滚对事务提交的影响。 | <font color=green>▮</font>  |          |
| 【内核】支持在A兼容性模式下对空串的处理                      | 在A兼容模式下支持对空串的处理，共计执行55条用例，主要覆盖了功能测试和资料测试。累计发现缺issue1个，已验收通过，整体质量良好。 | 兼容A模式                                                    | <font color=green>▮</font>  |          |
| 【内核】SQL引擎解耦化，实现O兼容性基础插件                   | 共执行107条用例，主要覆盖函数功能测试、whale插件安装测试、guc参数测试及资料测试。函数功能测试包括：gsql连接openGauss兼容A库，连接A库创建插件并执行相关SQL语句结果与postgres库orafce插件执行相关SQL语句结果是否一致。whale插件安装测试包括：OM方式安装的数据库是否可以正常创建插件，编译方式安装后的数据库，whale源码拷贝到openGauss-server源码的contrib目录下make install 后，连接数据库是否可以创建whale插件create extension whale。GUC参数测试包括：参数开启关闭后，插件语法功能是否有差异。资料测试：资料描述是否准确、示例能否正确执行。累计发现缺陷单5个，剩余所有缺陷已解决，回归通过，无遗留风险，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【迁移工具】chameleon解除跨平台的依赖                        | chameleon 解除跨平台依赖，共执行用例19个，主要覆盖了Centos7、openEuler20.03_x86、openEuler20.03_aarch64、openEuler22.03_x86、openEuler22.03_aarch64环境的安装和使用。资料测试覆盖chameleon 和portal资料的描述是否完整。累计发现缺陷单1个，1个缺陷已解决，回归通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【迁移工具】全量迁移工具能力增强                             | 全量迁移工具能力增强特性，共执行用例42个，主要覆盖了功能测试、性能测试、资料测试。功能测试覆盖以下特性。1、迁移数据类型；2、同时迁移表和索引及分开迁移表和索引；3、通过配置is_skip_completed_tables参数，测试工具异常重启是否跳过已经迁移完成的表。使用chameleon工具将上述操作迁移到openGauss，保证迁移前后数据的一致性、有序性和完整性。性能测试，在高性能机器下，通过sysbench工具给M*侧生成数据（表无主键），然后使用chameleon工具迁移至openGauss，待数据迁移完成，计算迁移效率。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单5个，4个缺陷均已解决且回归通过，1个待办的，整体质量较好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【OM工具】OM工具解耦对OS版本的依赖                           | OM工具解除对OS版本的依赖，共执行用例10个，功能测试主要覆盖了Centos和openEuler环境以及暂不支持的系统比如bcLinux上使用OM方式安装数据库，资料测试资料的描述是否完整。累计发现缺陷单0个，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【资源池化】备机可见性逻辑优化、主机推进oldestxid逻辑优化    | 实现资源池化备机可见性逻辑优化，主机推进oldest_xmin 相关特性测试特性测试中，主要涉及新增参数设置方式以及取值为边界值异常值的功能验证、文档验证，一致性测试，故障测试，共发现12个问题，回归通过，有遗留风险，质量一般。 | 无                                                           | <font color=yellow>▲</font> |          |
| 【MySQL兼容性】设置字符集字符序、指定常量字符集、多字符集表达式、set类型指定字符序、gbk、gb18030字符序支持创建事务时开启一致性快照、支持存储过程resignal语法和signal的mysql语法兼容性特 | 实现设置字符集字符序、指定常量字符集、多字符集表达式、set类型指定字符序、gbk、gb18030字符序支持创建事务时开启一致性快照、支持存储过程resignal语法和signal的mysql语法兼容性特性测试中，主要涉及函数及语法的功能验证、文档验证，共发现9个问题，现回归通过，无遗留风险，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【内核】GB18030-2022标准支持                                 | 涉及功能验证、文档验证，质量良好                             | 无                                                           | <font color=green>▮</font>  |          |
| 【内核】极致RTO支持备机可读，且维持一主一备70W tpmC时，RTO<10s的规格不变 | 涉及功能验证、文档验证，一致性测试共发现5个问题，现未回归通过，有遗留风险 | 无                                                           | <font color=yellow>▲</font> |          |
| 【内核】CAS性能优化                                          | 涉及功能验证、文档验证，质量良好                             | 无                                                           | <font color=green>▮</font>  |          |
| 【中国联通】兼容MySQL 5.7协议增强                            | 基于dolphin插件兼容M*协议增强的语法特性测试，共计执行60条用例，主要覆盖了功能测试和资料测试。累计发现缺陷单1个，所有缺陷已解决，回归通过，无遗留风险，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【内核】发布订阅支持冲突解决方案                             | 发布订阅支持冲突解决共计执行用例37条，主要覆盖功能测试，升级测试和资料测试。功能测试包含GUC参数subscription_conflict_resolution取值验证，新增禁用订阅和跳过冲突事务语法测试，发生数据冲突后是否按设置的处理方式解决冲突测试；升级测试验证升级后支持本特性，回滚升级能正常执行；资料测试验证特性描述是否准确，示例正确等。测试发现缺陷1个，已修复且回归通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【云和恩墨】M兼容性：别名带单引号                            | 共进行1轮测试，覆盖50测试点,发现问题2个，已解决0个，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【云和恩墨】cmake的小型化openGauss编译过程支持dolphin        | 共进行1轮测试，覆盖361测试点,发现问题0个，已解决0个，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【云和恩墨】【特性提交】tassl 支持TLCP                       | 共进行1轮测试，覆盖12个测试点，发现问题0个，已解决0个，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【云和恩墨】【资源池化】资源池化后的性能视图                 | 共进行2轮测试，覆盖15个测试点，其中功能测试12个，升级兼容测试3个，发现问题1个，已解决问题1个，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【云和恩墨】【主备复制】并行apply优化                        | 共进行1轮测试，覆盖78个测试点，发现问题0个，已解决0个，无遗留问题，整体质量良好 | 无                                                           | <font color=yellow>▲</font> |          |
| 【国创】DataKit 业务开发增强                                 | 进行了2轮测试，覆盖了调试函数/过程、覆盖率统计、创建表、管理表、管理表数据、模式 (创建、编辑、删除)、导出数据和DDL（导出函数/过程 DDL、导出表DDL、导出表 DDL 和数据、导出表数据、导出序列 DDL、导出序列 DDL 和数据、导出视图 DDL、导出模式DDL、导出模式DDL和数据）特性，设计了474个测试用例，用例经过社区的评审，整个测试过程覆盖了功能测试、接口测试、性能测试。 总共发现82个问题，目前遗留了3个问题，整体质量良好，可供DBA日常使用。 | 无                                                           | <font color=yellow>▲</font> |          |
| 【国创】DataKit 智能运维增强                                 | 进行了2轮测试，覆盖了实例监控、历史数据诊断、告警监控的特性，设计了526个测试用例，用例经过社区的评审，整个测试过程覆盖了功能测试、接口测试、性能测试。 总共发现139个问题，目前遗留了3个问题，整体质量良好。 | 无                                                           | <font color=yellow>▲</font> |          |
| [海量数据：兼容M*]创建视图支持sql security语法               | openGauss兼容mysql，支持sql security语法，测试用例54条，主要聚焦于功能测试。功能测试覆盖：视图、函数体和存储过程使用中支持sql security语法，不同权限的用户创建，然后通过赋权交叉调用查看是否支持sql security语法，校验输出结果。累计发现问题单1个，特性质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| [海量数据：兼容M*]存储过程和函数只带select语句时，支持call调用 | openGauss兼容mysql，可以使用call关键字调用存储过程和只包含select语句的函数，测试用例35条，主要覆盖了功能测试。功能测试覆盖：存储过程中进行各种表类型的查询和dml操作，存储过程中对表同义词、视图、物化视图、视图同义词查询，存储过程中使用聚合函数、窗口函数、类型转换函数、日期处理函数、字符处理函数，存储过程中label与loop、while、repeat结合查询，自定义用户测试，在join、union、子查询、group by、connect by中使用@变量，存储过程中的多结果集中有部分报错，返回结果0条和多条结合，不支持多结果集，嵌套调用，校验输出结果。累计发现问题单1个，特性质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| [海量数据：兼容M*]支持DIAGNOSTICS                            | openGauss兼容mysql，支持diagnostic用法，测试用例20条，主要覆盖了功能测试。功能测试覆盖：直接调用测试，存储过程测试，自定义函数测试，导入导出测试，其他兼容模式下测试，自定义用户测试，大小写敏感场景，校验输出结果。累计发现问题单1个，特性质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| [海量数据：兼容M*]select语句支持ignore index                 | openGauss兼容mysql，支持ignore index语法，测试用例30条，主要聚焦于功能测试。功能测试覆盖：各种索引类型中支持ignore index语法，DML操作中支持ignore index语法，各种分区表中支持ignore index语法，校验输出结果。累计发现问题单0个，特性质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| [海量数据]【资源池化】 DSS支持NoF/NoF+接口                   | 资源池化提供NoF+接口支持，测试用例39条，主要覆盖了功能测试及高并发测试。功能测试覆盖：dsscmd各项操作，建表使用段页式等，导入导出备份还原，高并发测试覆盖TPCC 100仓300并发，1000仓800并发等。累计发现问题单5个，特性质量良好。 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（M*兼容）支持trigger建在指定schema中，并校验schema和table schema是否一致 | 共进行2轮测试，覆盖86个测试用例，发现问题3个，均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（M*兼容）支持常用的performance_schema视图           | 共进行1轮测试，覆盖74个测试用例，问题均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（M*兼容）支持show engines等4个语法                  | 共进行2轮测试，覆盖96个测试用例，发现问题1个，均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（DFX）支持在switchover超时时，打印数据库内核的堆栈  | 共进行1轮测试，覆盖27个测试用例，问题均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（M*兼容）json支持=、>=、<=、>、<、&等操作符         | 共进行2轮测试，覆盖31个测试用例，发现问题2个，均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |
| 【移动】（M*兼容）支持SHOW REPLICAS和SHOW REPLICA STATUS     | 共进行1轮测试，覆盖21个测试用例，问题均已解决，无遗留问题，整体质量良好 | 无                                                           | <font color=green>▮</font>  |          |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 专项测试结论

### 安全测试

openGauss 5.1.0 Preview版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库越权风险管理、数据库注入攻击、数据库秘钥管理、数据库辅助工具安全、数据库攻击及审计、数据库内存问题、数据库安全框架设计问题和数据库操作系统提权8大方向进行安全测试。

openGauss 5.1.0 Preview版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 可靠性测试

+ 故障注入类测试

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

+ 长时间负载测试

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景   | TPCC+sysbench连跑insert/update/delete事务，1000仓+800并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL创建表删表事务；cpu、内存和io等系统资源使用均正常；7*24H满足；极致RTO压力场景下可能存在一致性问题 |

### 性能测试

*性能测试对关键性能指标进行摸底和测试。*

| 测试活动                                             | 活动评价                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| 2P(Taishan 200 2280 7260 ARM)环境下单节点1H          | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，在696并发下进行1H的单节点压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280 7260)测试，单节点1H，tpmC158.3w，达到标准150w。 |
| 2P(Taishan 200 2280 7260 ARM)环境下单节点8H          | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，在696并发下进行8H的单节点压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280 7260)测试，单节点8H，tpmC151.6w，达到标准145w。 |
| 2P(Taishan 200 2280 7260 ARM)环境下一主一备1H        | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，在696并发下进行1H的一主一备压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280 7260)测试，单节点1H，tpmC124.4w，达到标准120w。 |
| 2P(Taishan 200 2280 7260 ARM)环境下一主一备8H        | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，在696并发下进行8H的一主一备压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280 7260)测试，单节点8H，tpmC123.8w，达到标准110w。 |
| 4P(TaiShan 200 2480 7260 ARM)环境下单节点1H          | 性能测试tpmC指标验收，使用benchmarksql工具，生成1000仓库的测试数据，在812并发下进行1H的单节点压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2480)，单节点1H，tpmC 233.2W，达到标准230w。 |
| RTO高可用特性测试                                    | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，在696并发下进行模拟数据库一主两备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求。多轮测试下测试结果稳定。<br />极致RTO恢复时间平均为8s，达到标准<10s。 |
| 资源池化sysbench特性测试                             | sysbench(tps)验收，利用sysbench工具在数据库使用指定的数据模型生成数据并进行压测，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />主机读写备机空载场景下，主机使用oltp_write_only，tps2.8w。主机读写备机只读场景下，主机使用oltp_write_only，备机使用oltp_read_write，主机tps2.3w，备机tps3.6w。 |
| 资源池化RTO高可用特性测试                            | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库一主一备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />故障前25wtpmC，30并发场景下，RTO恢复时间为8s，达到标准<10s。故障前50wtpmC，100并发场景下，RTO恢复时间为29s，达到标准<30s。 |
| M*向openGauss数据库全量迁移                          | 迁移速度(MB/s)验收，利用sysbench工具在源端生成数据并使用chamelon工具向目标端进行数据迁移，保证数据一致性的前提下测试迁移速度，测试过程符合指标规定的要求，测试结果达标300MB/s。多轮测试下测试结果稳定。<br />源端存在10张表，单表数据量3000万，线程数20场景下，M*向openGauss全量迁移速度为325MB/s。 |
| M*向openGauss数据库增量迁移                          | 迁移性能tps指标验收，利用sysbench工具在源端生成数据并使用chamelon工具同步源端/目标端数据，使用debezium工具开启在线迁移，利用sysbench工具在源端进行数据压力测试，测试结果达标3w tps。多轮测试下测试结果稳定。<br />源端存在10张表，单表数据量10万，线程数50，混合IUD场景下，M向openGauss增量迁移速度为 30763 tps。 |
| openGauss向M*数据库反向迁移                          | 迁移性能tps指标验收，利用sysbench工具分别在源端/目标端生成相同数据，使用debezium工具开启反向迁移，再次利用sysbench工具在源端进行数据压力测试，测试结果达标1w tps。多轮测试下测试结果稳定。<br />源端存在50张表，单表数据量10万，线程数50，混合IUD场景下，openGauss向M*反向迁移速度为 20837 tps。 |
| M*向openGauss数据库全量数据迁移，datachecker进行校验 | 迁移过程数据校验速度(MB/s)验收，更改sysbench的lua脚本，使用自定义表结构对M*数据库预置50张表每张表1000w条数据，通过chameleon全量迁移工具将数据迁移至openGauss，利用gs_datachecker工具对迁移数据的一致性进行测试，多轮测试下测试结果稳定，校验结果准确。<br />50张表，单表数据量1000万场景下，总size/time为518MB/s，达到标准150M/s，总条数/time为26w条/s，达到标准25w条/s。<br /> |

### 兼容性测试

#### 升级兼容

针对openGauss 5.1.0 版本，规划的升级路径如下表所示。测试情况：

1. openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1升级到openGauss 5.1.0.成功，升级失败或者升级未提交，可以成功回滚；
2. openGauss 2.0.0/openGauss 3.0.0/openGauss3.1.0升级到中间版本，间接升级到openGauss 5.1.0.成功，升级失败或者升级未提交，可以成功回滚；
3. 升级成功后，特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1带cm灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0不带cm灰度升级到openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0带cm，再灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0不带cm灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0不带cm，再灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0不带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0不带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0不带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0带cm就地升级到openGauss 5.0.1带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.1.0不带cm就地升级到openGauss 5.0.1带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.1.0不带cm就地升级到openGauss 5.0.1不带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 5.1.0数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 5.1.0数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论                      |
| ------------ | ------------------------------------------------------------ | ----------------------------- |
| 支持云化部署 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS环境下，通过Docker（18.03.1-ce, build 9ee9f40、18.09.0, build 4f81b51、18.09.0, build d1134d1）安装部署openGauss 5.1.0数据库 | 发现1个问题，已修复且回归通过 |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 5.1.0数据库 | 测试通过                      |

### 资料测试

| 序号 | **手册名称**       | **测试结论** |
| ---- | ------------------ | ------------ |
| 1    | 《发行说明》       | PASS         |
| 2    | 《关于openGauss》  | PASS         |
| 3    | 《快速入门》       | PASS         |
| 4    | 《安装指南》       | PASS         |
| 5    | 《简易教程》       | PASS         |
| 6    | 《应用开发指南》   | PASS         |
| 7    | 《编译指南》       | PASS         |
| 8    | 《数据库管理指南》 | PASS         |
| 9    | 《数据库运维指南》 | PASS         |
| 10   | 《性能调优指南》   | PASS         |
| 11   | 《AI特性指南》     | PASS         |
| 12   | 《数据迁移指南》   | PASS         |
| 13   | 《插件参考》       | PASS         |
| 14   | 《SQL参考》        | PASS         |
| 15   | 《工具与命令参考》 | PASS         |
| 16   | 《数据库参考》     | PASS         |

# 问题单统计

openGauss 5.1.0 Preview版本共发现问题1436个，有效问题1160个。详细分布见下表: 

| 版本名称                     | 测试起始时间 | 测试结束时间 | 有效问题数 |
| ---------------------------- | ------------ | ------------ | ---------- |
| openGauss 5.1.0 Preview RC1  | 2023/4/12    | 2023/4/18    | 70         |
| openGauss 5.1.0 Preview RC2  | 2023/4/19    | 2023/4/25    | 70         |
| openGauss 5.1.0 Preview RC3  | 2023/5/10    | 2023/5/16    | 57         |
| openGauss 5.1.0 Preview RC4  | 2023/5/24    | 2023/5/30    | 43         |
| openGauss 5.1.0 Preview RC5  | 2023/5/31    | 2023/6/6     | 41         |
| openGauss 5.1.0 Preview RC6  | 2023/6/7     | 2023/6/13    | 49         |
| openGauss 5.1.0 Preview RC7  | 2023/6/14    | 2023/6/20    | 96         |
| openGauss 5.1.0 Preview RC8  | 2023/6/21    | 2023/6/30    | 67         |
| openGauss 5.1.0 Preview RC9  | 2023/7/7     | 2023/7/11    | 16         |
| openGauss 5.1.0 Preview RC10 | 2023/7/12    | 2023/7/18    | 19         |
| openGauss 5.1.0 Preview RC11 | 2023/7/19    | 2023/7/25    | 36         |
| openGauss 5.1.0 Preview RC12 | 2023/7/26    | 2023/8/1     | 27         |
| openGauss 5.1.0 Preview RC13 | 2023/8/2     | 2023/8/8     | 33         |
| openGauss 5.1.0 Preview RC14 | 2023/8/9     | 2023/8/15    | 24         |
| openGauss 5.1.0 Preview RC15 | 2023/8/16    | 2023/8/22    | 31         |
| openGauss 5.1.0 Preview RC16 | 2023/8/23    | 2023/9/1     | 58         |
| openGauss 5.1.0 Preview RC17 | 2023/9/2     | 2023/9/12    | 84         |
| openGauss 5.1.0 Preview RC18 | 2023/9/13    | 2023/9/20    | 55         |
| openGauss 5.1.0 Preview RC19 | 2023/9/21    | 2023/9/27    | 0          |

![5.1.0_issue](.\images\5.1.0_issue.png)

本次测试共19轮。从RC1持续到RC19。2023.4.12正式进入openGauss 5.1.0测试阶段，RC1至RC3转测部分预埋需求，问题单处在一个较高峰值，随后进入一个下降趋势。随着630需求开发逐步完成、转测，问题单在RC6开始进入一个上升期，资源池化、内核、兼容性发现较多问题，直到RC7问题单达到一个顶峰，随后进入一个下降趋势，RC8展开一轮简单集成测试，涉及长稳、可靠性、安全、性能等场景。随后进入930阶段，需求未大量转测，问题处在一个低位状态。随着RC14开始需求大量转测，问题开始爆发，主要集中于资源池化、性能等方面。RC16进入集成验证阶段，启动长稳、性能、安全和可靠性等专项测试，问题主要集中于资源池化、极致RTO备机读等特性，发现问题数又达到一个高峰，无效问题主要集中伙伴特性测试时对场景的不熟悉。三轮集成测试过后，问题单逐步收敛，直到RC19最后一个版本，本轮测试活动主要为问题回归，本轮未新增问题。

# 附件

## 附件1：遗留问题列表

| 遗留问题                                                     | 优先级 | 负责团队  | 根因分析                                                     | 影响分析                                                     | 规避措施                                                     |
| ------------------------------------------------------------ | ------ | --------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 【测试类型：性能】【测试版本：5.1.0】 openEuler22.03操作系统，大页内存2M场景下，性能无法达到150w https://e.gitee.com/opengaussorg/issues/table?issue=I7J4EM | 主要   | openGauss | 22.03-4k版本操作系统升级，默认为4K页面，且调度、网络有不同程度变更，性能相比20.03-64k有劣化。且部分5.1.0新特性如OgRecord在22.03性能影响更大 | 22.03-4k场景tpcc性能相比20.03-64k的150W劣化                  | 通过调整调度、网络参数，可以一定程度提升22.03-4k场景性能，目前能做到146W Tpmc。后续需要针对部分特性在22.03的表现做优化 |
| 扫表查询调用到GetMultiXactIdMembers，长时间没有返回结果      | 次要   | openGauss | 备机的扫描查询，主机会处理会调用到GetMultiXactIdMembers ，除此之外主机侧其中大量的业务线程也调用GetMultiXactIdMembers ，在这个函数中会请求MultiXactMemberControlLock，这个锁是全局，只有一个，但是每个线程都在请求就会排队，其中部分线程会反复请求，造成业务进行也会变慢。 同样的DMS的消息处理线程调用到该函数，同样会卡住。因为消息处理线程不工作，造成其他消息无法处理，影响其他场景。 | 目前openGasus主线也有该问题，业务会变慢，线程都在排队等锁。过一段时间后能够自动恢复。 该问题并不是扫表查询就必现。需要一定概率才会出现 | 暂无消减或规避措施 后续解决方案：方向：避免DMS消息处理线程因为该场景卡住，影响其他流程 |
| 资源池化下在线Failover失败导致RTO超时                        | 不重要 | openGauss | 其中发生两轮failover，第一轮在线failover进行了30s然后失败，第二轮重启failover成功，耗时近30s 耗时长有主要因素1）在线failover的失败，且耗时30s。因为在线failover需要将业务线程全部退出，如果30s对应业务线程没有退出，则认为线程无法退出，在线failover失败。 因素2）重启failover，因为重启，本身需要完成一系列初始化动作，以及reform重新组织工作，也增加了近10s的耗时 | 当前想避免在线failover失败的话，需要备机没有对应的业务       | 1、不影响集群功能，在线failover失败后还有重启failover，集群还会恢复正常。 2、当前备机只有进程卡在DMS的页面请求中，无法响应信号才会无法退出，如果没有的话，业务线程可以正常退出。  后续技术解决方案：方向：在线failover成功。方法：进程在LockBuffer中可以感知退出信号，释放对应资源，内存资源等，事务相关需要回滚之类。然后退出。 |
| tpcc建仓过程中kill主节点gaussdb进程，节点状态一直卡在unknown 次要 openGauss | 次要   | openGauss | api所在的客户端进程退出时候，某个客户端api所在的线程的链接的session上设置了vg_latch的spinlock_t类型的lock。 dss服务端在发现api所在的客户端链接断链的时候，需要session上做vg_latch等latch信息的清理。 清理过程是断链事件触发从线程池里分配线程处理。断链事件跟新建联事件是可并发的。清理latch的过程是按照链接即session级别处理的。清理过程需要获取并独占vg_latch的lock。 已经分配到线程待清理的session都不是占有vg_latch的lock的session，需要等待持有vg_latch的lock的session的先清理完成才能继续，而线程池里的线程已经全部分配完了，没有新线程分配给持有vg_latch的lock的session做latch清理了，就卡住了。 | 如果发生，会导致集群状态异常，需要手动重启恢复集群。         | 1. 手动重启集群即可。                                        |
| 【测试类型：性能】【测试版本：5.1.0】 反向全量迁移，迁移性能达不到300m/s https://e.gitee.com/opengaussorg/issues/table?issue=I7U5ZX | 次要   | openGauss | Mysql端导入性能不达标，导致整个迁移链路不达标                | 发现全量迁移性能不达300M/S，功能正常                         | 无                                                           |
| I7Y8CG 安装集群，输入错误的密码后，Oops， unhandled type 3 ('unimplemented') | 不重要 | openGauss | 1.目前只有当操作系统是麒麟才有这个问题，并且复现的概率是50% 2.该问题是在创建互信时，已经建立连接，但是在打开会话的失败了，目前还不清楚具体原因，建议将该问题放到下个版本 | 1.只有麒麟环境并且是在第一次输错密码的情况下才可能会失败     | 1.只麒麟系统操作时，一次输对密码                             |
| chameleon迁移日期时间 类型及二进制类型，日期时间类型迁移成功，但迁移日志有报错，二进制类型，迁移后数据有误 | 次要   | openGauss | 变色龙对类型tinyblob、 mediumblob、 longblob未适配兼容性     | mysql端数据迁移到opengauss后 数据不一致                      | 可以通过类型映射覆写，把tinyblob、 mediumblob、 longblob覆写为原来的blob |

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@jiexiao1413](https://gitee.com/jiexiao1413)

+ [@liao-shiting](https://gitee.com/liao-shiting)

+ [@min-sun](https://gitee.com/min-sun)

  

