![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改章节 | 修改描述 | 作者      |
| -------- | -------- | -------- | -------- | --------- |
| 2026.9.8  | 1.0      | 初稿撰写     |          | liuzhen001 |

目 录

1 概述

2 测试版本说明

> 2.1 测试版本信息

> 2.2 测试环境
>
> 2.3 需求清单

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论
>
> 4.3 DBV伙伴测试结论

5 问题单统计

> 5.1 问题单统计

> 5.2 问题单分析
>
> 5.3 执行用例与问题单数量对应关系
>
> 5.4 继承测试迭代版本基线

6 附件

7 致谢

**Keywords 关键词**：openGauss 7.0.0 LTS

**Abstract 摘要**：主要是描述了openGauss 7.0.0 LTS版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语     | 英文全名                           | 中文解释                                     |
| ---------- | ---------------------------------- | -------------------------------------------- |
| SQL        | Structured Query Language          | 结构化查询语言                               |
| LTS        | Long Time support                  | 长时间维护                                   |
| DML        | Data Manipulation Language         | 数据操纵语言                                 |
| DDL        | Data Definition Language           | 数据定义语言                                 |
| DCL        | Data Control Language              | 数据控制语言                                 |
| CM         | Cluster Management                 | 集群管理工具                                 |
| DMS        | Distributed Memory Service         | 分布式内存服务                               |
| DSS        | Distributed Storage Service        | 分布式存储服务                               |
| RAG        | Retrieval-Augmented Generation     | 检索增强生成                                 |
| HNSW       | Hierarchical Navigable Small World | 基于分层导航小世界（HNSW）算法的图索引       |
| OGAI       | openGauss AI                       | openGauss智能向量化框架                      |
| BM25       | Best Matching 25                   | 全文检索相关性打分算法                       |
| oGMemory   | openGauss Memory                   | 面向Agent的上下文生命周期和长期记忆管理能力  |
| OpenViking | OpenViking                         | 面向Agent的记忆系统与向量检索框架            |
| RTO        | Recovery Time Objective            | 恢复时间目标                                 |



***


# 1、概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 7.0.0 LTS版本在7.0.0 RC1/RC2/RC3基础上，新增内核性能优化、oGRAC能力构建、工具链、资源池化、AI与兼容性等特性，并对若干关键缺陷进行了修改。本文主要描述了openGauss 7.0.0 LTS版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 7.0.0 LTS版本整体质量良好。

# 2、测试版本说明

## 2.1 版本测试信息

| 版本名称                 | 测试起始时间 | 测试结束时间 |
| ------------------------ | ------------ | ------------ |
| openGauss 7.0.0 LTS B001 | 2026/4/1     | 2026/4/15    |
| openGauss 7.0.0 LTS B002 | 2026/4/16    | 2026/4/22    |
| openGauss 7.0.0 LTS B003 | 2026/4/23    | 2026/4/29    |
| openGauss 7.0.0 LTS B004 | 2026/4/30    | 2026/5/13    |
| openGauss 7.0.0 LTS B005 | 2026/5/14    | 2026/5/20    |
| openGauss 7.0.0 LTS B006 | 2026/5/21    | 2026/5/27    |
| openGauss 7.0.0 LTS B007 | 2026/5/28    | 2026/6/3     |
| openGauss 7.0.0 LTS B008 | 2026/6/4     | 2026/6/10    |
| openGauss 7.0.0 LTS B009 | 2026/6/11    | 2026/6/17    |
| openGauss 7.0.0 LTS B010 | 2026/6/18    | 2026/6/24    |
| openGauss 7.0.0 LTS B011 | 2026/6/25    | 2026/7/1     |
| openGauss 7.0.0 LTS B012 | 2026/7/2     | 2026/7/8     |
| openGauss 7.0.0 LTS B013 | 2026/7/9     | 2026/7/15    |
| openGauss 7.0.0 LTS B014 | 2026/7/16    | 2026/7/22    |
| openGauss 7.0.0 LTS B015 | 2026/7/23    | 2026/7/29    |
| openGauss 7.0.0 LTS B016 | 2026/7/30    | 2026/8/5     |
| openGauss 7.0.0 LTS B017 | 2026/8/6     | 2026/8/12    |
| openGauss 7.0.0 LTS B018 | 2026/8/13    | 2026/8/19    |
| openGauss 7.0.0 LTS B019 | 2026/8/20    | 2026/8/26    |
| openGauss 7.0.0 LTS B020 | 2026/8/27    | 2026/9/2     |
| openGauss 7.0.0 LTS B021 | 2026/9/3     | 2026/9/9     |
| openGauss 7.0.0 LTS B022 | 2026/9/10    | 2026/9/12    |
| openGauss 7.0.0 LTS B023 | 2026/9/13    | 2026/9/16    |

## 2.2 测试环境

| 硬件型号                         | 硬件配置信息                                                 | 备注 |
| -------------------------------- | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280)         | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280)         | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 22.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280)         | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 24.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2480)         | CPU：Kunpeng-920 7260 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| TaiShan 200 (Model 2280)         | CPU：Kunpeng-920 V200 7270z 2p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| RH2288H V3                       | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4*10GE |      |
| 服务端 Taishan 200（Model 2280） | CPU：Kunpeng 920<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 鲲鹏920 7280Z                    | CPU：Kunpeng 920 7280Z<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 鲲鹏950                          | CPU：--<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 存储侧<br />OceanStore           | Dorado 18500 V6                                              |      |
| DCS/ECS虚拟机                    | 8核30G                                                       |      |
| 公有云虚拟机                     | 8核16G/8核32G/16核32G                                        |      |

虚拟化平台

| 虚拟化平台 | 版本说明                                                     |
| ---------- | ------------------------------------------------------------ |
| KVM虚拟化  | KVM+GuestOS（Centos7.6/OpenEuler20.03(LTS)/openEuler22.03(LTS)/openEuler24.03(LTS)） |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| openEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| openEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| openEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| openEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| openEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，aarch版本ISO<br />SHA256:9967f955f5b7637cf77bc4ecb69cb6ac3816d591357c374a53c0dbbfd5402ac9 |
| openEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，x86-64版本ISO<br />SHA256:786b9683659512e71c978c34aea806a97ed6cacf04e1b0a22017a50eec582cbe |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

## 2.3 需求清单

openGauss 7.0.0 LTS版本是openGauss社区继续联合多方力量推出的长期支持版本，本次发布的需求列表、分工如下：

| 序号 | 需求                                     | 开发主体      | 测试主体 | 验证策略                                                     |
| ---- | ---------------------------------------- | ------------- | -------- | ------------------------------------------------------------ |
| 1    | pgstat优化                               | SQL           | QA       | 功能测试：验证20+个pg_stat_*系列视图统计信息收集功能；INSERT/UPDATE/DELETE后统计准确性；主备库、重启前后一致性；空库/临时表/分区表/数据库退出等异常场景<br/>升级测试：升级前后统计数据一致性，升级后统计视图功能可用<br/>资源占用：对比优化前后CPU、IO、内存利用率变化 |
| 2    | 传统主备性能优化-cache-miss优化          | SQL           | QA       | 功能测试：4个优化项新增功能及继承功能不受影响；pg_session_wlmstat视图新增rowdesc_a/b_store_count、rowdesc_a/b_hit_count；memcheck编译安装<br/>性能测试：优化后平均QPS提升5%<br/>可靠性测试：switchover/failover/关键worker故障恢复，集群状态正常<br/>升级测试：7.0.0 RC3升级至7.0.0 LTS后视图功能验证及回退<br/>资料测试：pg_session_wlmstat视图相关资料 |
| 3    | 索引膨胀优化                             | SQL           | QA       | 功能测试：创建索引指定indexsplit为insertpt策略；验证ubtree和ubtreepcr已有功能；递增写入/重复键密集场景索引页分裂路径<br/>性能测试：索引膨胀优化相关指标<br/>资料测试：CREATE INDEX相关资料 |
| 4    | 向量索引构建性能优化                     | StorageEngine | QA       | 功能测试：插件创建与调用；索引创建；表数据查看、新增、删除等操作回显符合预期<br/>性能测试：覆盖数据导入、高维向量索引构建；100万数据构建1024维向量索引在2.1min内完成 |
| 5    | to_char函数性能优化                      | SQL           | QA       | 功能测试：to_char(numeric, format)常用格式；"999,999.99"及"999,999,999.99"正确执行；隐式/显式/DML操作；非目标to_char不受影响<br/>性能测试：format为"999,999.99"和"999,999,999.99"时性能提升35%<br/>兼容性测试：B库及轻量版/企业版<br/>可靠性测试：数据库异常中断后可正确执行 |
| 6    | 宽表null列占比高场景更新性能优化         | SQL           | QA       | 功能测试：512/1413列宽表更新；分区表、索引、跨字节宽表更新；高列数、大量null、少量列更新的行存宽表场景<br/>性能测试：全null表更新性能提升35%以上，其他场景无劣化<br/>可靠性测试：30分钟多次重复稳定执行，内存无溢出<br/>兼容性测试：A库/B库及轻量版/企业版 |
| 7    | 资源池化failover适配在线reform           | StorageEngine | QA       | 功能/可靠性测试：默认参数、并行回放（参数开到8）、极致RTO、按需回放、按需回放+实时构建五种配置下，TPCC压测10min后stop主节点、kill gaussdb/dss 6次、reboot/panic、网络中断/闪断、进程进入T/D/Z状态、主节点ReadOnly、存储网络中断；二次故障（promoting时kill gaussdb、拉起原故障节点后kill待升主）<br/>性能测试：各配置各故障场景升主时间对比旧版本不劣化<br/>长稳：7*24h后failover稳定性及数据一致性 |
| 8    | CM支持订阅sysSentry                      | StorageEngine | QA       | 功能测试：reboot（正常重启/快速重启）和panic（SysRq/内核模块）故障检测上报；故障节点与CMS主同节点/非同节点；通知延迟≤5s；xalarm_node_map映射；enable_xalarm_event_check开关<br/>资料测试：资料描述完整准确 |
| 9    | 黑匣子加固                               | StorageEngine | QA       | 功能测试：CM-Rest API内存/CPU/磁盘IO监控接口，覆盖HTTPS/HTTP、GET/POST及带参请求、白名单、异常节点、文件权限故障；oGRecorder优化gr_vfs_query_file_info：正常分页查询、空目录、文件名顺序、多次续传终止、多线程并发；异常场景续传参数错误、无效vfs_handle、接收数组长度不足 |
| 10   | 鲲鹏超节点加速资源池化clog、csnlog       | StorageEngine | QA       | 功能测试：enable_ub、ubs_mem_path、ss_init_clog_size、ss_enable_ondemand_recovery等参数配置、reload/restart及冲突拦截；CLOG/CSNLOG共享内存初始化、复用、清理及ubsectl可观测；事务提交/回滚、并发DML、主备读写一致性；长事务/VACUUM下Snapshot与OldestXmin<br/>可靠性测试：主备切换、在线failover、节点stop/start、异常恢复<br/>性能测试：300+300、500+100压测对比TCP基线 |
| 11   | oGRAC兼容性框架                          | oGRAC         | QA       | 功能测试：A/B/C兼容库创建及parse、系统函数、操作符、系统视图、系统表和类型转换规则分支；基本语法在不同兼容性场景验证<br/>连接驱动：odbc/jdbc在不同兼容性库验证<br/>资料测试：兼容性框架资料 |
| 12   | oGRAC常用数据类型兼容                    | oGRAC         | QA       | 功能测试：N开头字面量DML/DDL与不带N前缀结果一致；f/d结尾写法与不带后缀一致；nclob与clob一致；NATIONAL CHARACTER/CHAR VARYING/NCHAR VARYING(n)<br/>资料测试：参数功能及用法描述准确 |
| 13   | oGRAC-DFX增强                            | oGRAC         | QA       | 功能测试：dv_dss_time_stats设备执行次数/等待时间；dv_sql_excution_plan历史SQL执行计划；dv_slow_sql慢SQL字段（需开启slowsql）；dv_drc_buf_info全局缓冲区页面；dv_drc_local_lock_info本地锁；dv_buf_ctrl_info缓冲区控制块；新增字段采集准确性 |
| 14   | oGRAC三方库优化                          | oGRAC         | QA       | 功能测试：编译安装、三方库变更组件确认及功能可用<br/>兼容性测试：x86/ARM多操作系统及容器编译安装<br/>性能测试：合入前后TPCC无劣化<br/>资料测试：单节点本地、容器和双节点编译安装资料 |
| 15   | oGRAC支持rowid数据类型                   | oGRAC         | QA       | 功能测试：ROWID列DDL/ALTER/约束/类型转换；ROWIDTOCHAR、ROWIDTONCHAR、CHARTOROWID及互逆；隐式转换；DML与事务COMMIT/ROLLBACK；唯一/复合/B-tree索引；长度边界/极值/NULL/重复值；存储过程/触发器/匿名块；函数耦合<br/>资料测试：rowid相关资料 |
| 16   | oGRAC parser重构                         | oGRAC         | QA       | 功能测试：use_bison_parser开关对照，覆盖SELECT、INSERT、UPDATE、DELETE、MERGE、REPLACE、PREPARE TRANSACTION、ALTER SESSION、SET TRANSACTION、COMMIT、ROLLBACK、SAVEPOINT、LOCK、ALTER、CHECKPOINT、BACKUP、RESTORE、BUILD、SYNCPOINT、OGRAC RECOVER、SHUTDOWN、VALIDATE、RECOVER、REPAIR等25类语法点 |
| 17   | oGRAC-bison/flex框架重构-part2           | oGRAC         | QA       | 功能测试：use_bison_parser=true下DDL（CREATE、CREATE OR REPLACE、ALTER、DROP、TRUNCATE、FLASHBACK、PURGE、COMMENT、GRANT、REVOKE、ANALYZE）及PL/SQL（body外call/exec/execute/declare/begin；body内自定义类型、控制语句、assign、游标；包和触发器） |
| 18   | MES SHM共享内存传输通道优化              | oGRAC         | QA       | 功能测试：GUC参数与SHM/TCP兼容；SHM初始化与共享内存对象；对端映射和启动时序（retry/active/timeout）；DMS透传、消息收发与回调、心跳、故障注入<br/>性能测试：资源池化jTPCC对比，记录主备并发组合下tpmC、tpmTOTAL<br/>资料测试：《MES灵衢内存语义通信优化》与设计说明书一致性 |
| 19   | oGRAC RTO加速优化                        | oGRAC         | QA       | 功能测试：RBP新增9个GUC参数、rbps_ctl/cms管理rbps、rbps.conf、页缓存与恢复数据<br/>性能测试：Kunpeng 920双节点50w tpmC场景RTO小于10s<br/>可靠性测试：0/1节点rbps断连后不缓存、恢复后缓存、rbps进程故障恢复<br/>资料测试：RBPS加速恢复说明、GUC参数及rbps使用说明 |
| 20   | oGRAC-CM支持磁盘阈值检测                 | oGRAC         | QA       | 功能/可靠性测试：配置加载默认值/边界值/非法值；状态保护失效（本地盘/共享盘）；自动状态保护；只读/读写切换冷却；手动恢复读写；高并发场景 |
| 21   | ogsql Readline行编辑与补全               | oGRAC         | QA       | 功能测试：关键字/对象名/函数补全、行编辑快捷键、历史命令、UTF-8字符、敏感命令过滤<br/>文档对照：按oGRAC SQL参考文档语句清单逐条探测Tab补全（能补/不能补/错误补全）<br/>兼容性测试：多国语言<br/>安全测试：敏感命令不计入HISTORY<br/>资料测试：对照ogsql readline文档核对 |
| 22   | Datakit迁移优化                          | Tools         | QA       | 功能测试：MySQL全量迁移同名用户；迁移模式/对象选项前台后台；卸载时同步卸载prometheus/portal<br/>前向兼容：历史迁移任务、历史后台表字段正常<br/>资料测试：随需求验证 |
| 23   | MySQL全量迁移支持断点续传                | Tools         | QA       | 功能测试：源库到迁移工具到目标库各阶段中断后续传数据完整性<br/>性能测试：10表×1000w无主键场景迁移速率≥300MB/s<br/>前向兼容：其他迁移过程功能正常<br/>资料测试：随需求验证 |
| 24   | 从OpenSearch迁移至openGauss              | Tools         | QA       | 功能测试：数据类型映射、迁移数据一致性；正确指令正常迁移、错误参数报错；参数文件正确/错误场景<br/>性能测试：10万条数据迁移速度优于1MB/s<br/>兼容性测试：目标库A/B/PG模式<br/>资料测试：参数功能及用法描述准确 |
| 25   | Oracle2oGRAC                             | Tools         | QA       | 功能测试：字符/数字/日期时间/二进制/ROWID/BOOLEAN等数据类型迁移；主键表/分区表/外键/检查约束/唯一约束/备注/默认值；序列（基本/缓存/循环/最大值不循环）；单一及复合索引（B-tree/UNIQUE/Bitmap/Reverse Key/Function-based）；简单/复杂/物化视图；存储过程/函数/触发器<br/>工具易用性：参数配置校验及默认值/异常值；进度上报（isDumpJson、statusDir）<br/>性能测试：源端20张表每表100万条，迁移速率≥300MB/s |
| 26   | 智能运维增强                             | Tools         | QA       | 功能测试：配置文件配置项报错检查；GaussMaster+DBMind接口连接；优先级降级；模块注册<br/>可靠性测试：LLM失效、Token过期、配置热重载<br/>安全测试：日志中无api key打印<br/>性能测试：正常并发执行<br/>资料测试：参数功能及用法描述准确 |
| 27   | BM25支持自定义词典、索引空间优化         | AI            | QA       | 功能测试：配置dict_path自定义词典正常和异常场景；索引空间优化<br/>性能测试：索引空间优化下性能<br/>升级测试：7.0.0 RC3升级至7.0.0后功能正常<br/>稳定性/资料测试 |
| 28   | BM25分布式global idf                     | AI            | QA       | 功能测试：enable_bm25_global_idf参数；开启后bm25_global_stat、bm25_table_stat及多参数组合<br/>性能测试：1CN+2DN环境下5000万数据QPS与召回率<br/>可靠性测试：线程并发提交session、向量库写入失败、vector backend查询失败 |
| 29   | openGauss支持数据分支增强                | AI            | QA       | 功能测试：并行回放多wal_redo进程在多租户间合理分配；LFC+Prefetch开启后预取和查询耗时降低；diff差异对比及三种策略merge；新增函数和视图<br/>性能测试：并行回放性能提升5%<br/>异常/资料测试 |
| 30   | ogMemory对接OpenClaw，支持上下文缓存优化 | AI            | QA       | 功能测试：after_turn同步写入profile/preference/event；CandidateMemory抽取、ReAct merge、PolicyRouter；ContextWriter/Outbox/L0/L1/L2索引及幂等upsert；compose/TypedQuery分层召回<br/>异常降级：停止openGauss和vector backend后服务不崩溃<br/>性能测试：Benchmark及同步轻量路径 |
| 31   | OpenViking对接openGauss                  | AI            | QA       | 功能测试：验证 OpenViking 使用 openGauss 作为向量数据库后，服务启动、健康检查、记忆写入、记忆检索链路是否可用；验证 OpenClaw -> OpenViking -> openGauss 链路是否连通；验证 OpenViking storage.vectordb.backend=opengauss、embedding 模型、OpenClaw 远端 OpenViking 插件配置是否生效<br/>评测：使用 LoCoMo small 数据集验证记忆写入、压缩、问答、Judge 评分流程<br/>兼容测试：验证 OpenViking openGauss backend 在 standalone 模式下连接 openGauss / SPQ CN 的基本兼容性<br/>性能测试：鲲鹏与海光向量检索对比 |
| 32   | 【海量】支持JSON/XML函数及表达式         | SQL         | QA       | 功能测试：A兼容模式下GMS_XMLDOM/GMS_XMLPARSER对象类型与方法函数；权限、系统表变更等关联场景<br/>升级测试：6.0.5升级至7.0.0 后功能可用<br/>资料测试：随需求验证 |
| 33   | 【海量】建表时属性GENERATED AS IDENTITY  | SQL       | QA       | 功能测试：PG/A/B/D模式GENERATED ALWAYS/BY DEFAULT AS IDENTITY；序列选项（START WITH/MINVALUE/MAXVALUE/INCREMENT/CYCLE/SEQUENCE NAME）；INSERT/OVERRIDING、UPDATE/MERGE、ALTER、LIKE/继承、TRUNCATE RESTART、权限/PBE、gs_dump/gs_restore；B模式MySQL语法、D模式SQL Server identity<br/>升级测试：6.0.5升级至7.0.0 LTS后语法可用<br/>资料测试：随需求验证 |
| 34   | 【海量】MySQL兼容性-dual表支持子查询     | SQL       | QA       | 功能测试：B模式dolphin扩展下DUAL虚拟表；SELECT FROM DUAL、表达式与函数、查询子句、EXISTS/IN/ANY/标量/FROM子查询、DML、CTE、视图与对象、not_used列、异常边界、遮蔽/权限/事务/临时表<br/>兼容性测试：仅B模式且需创建dolphin扩展<br/>升级测试：6.0.5升级至7.0.0 LTS后语法可用<br/>资料测试：随需求验证 |
| 35   | 【海量】PG兼容性-支持pg_stat_progress_copy视图 | SQL  | QA       | 功能测试：COPY FROM FILE/STDIN、COPY TO、行存/列存/分区表进度；relid、权限、track_activities开关、清理机制、并发与边界<br/>兼容性测试：仅PG模式<br/>升级测试：6.0.5升级至7.0.0 LTS后功能可用<br/>资料测试：随需求验证 |
| 36   | 【海量】PG兼容性-pg_locks新增waitstart字段 | SQL    | QA       | 功能测试：pg_locks.waitstart时间戳；granted=false有值/granted=true为NULL；多等待者队列、各锁类型、等待结束、与pg_stat_activity交互<br/>兼容性测试：仅PG模式<br/>升级测试：6.0.5升级至7.0.0 LTS后功能可用<br/>资料测试：随需求验证 |
| 37   | 【恩墨】支持INSERT ON CONFLICT           | SQL       | QA       | 功能测试：A库/PG库覆盖ON CONFLICT DO NOTHING/UPDATE，与PG对比执行结果<br/>兼容性测试：BCD库不支持报错<br/>升级测试：6.0.0 LTS升级至7.0.0 LTS后语法可用，回退成功<br/>资料测试：INSERT ON CONFLICT相关SQL语法资料 |
| 38   | 【恩墨】支持Global Sequence Cache        | SQL       | QA       | 功能测试：GLOBAL/SESSION序列缓存语法与DDL、序列接口、边界与切换、并发连续性<br/>兼容性测试：A/PG/B/C/D模式GLOBAL基础功能<br/>升级测试：含序列数据的upgrade/rollback/commit及重启可靠性<br/>耦合测试：事务、匿名块、游标、PBE、触发器、存储过程、DDL组合<br/>资料测试：SQL语法sequence、系统表/类型资料核对<br/ |
| 39   | 【神通】支持notify/listen功能            | StorageEngine | QA       | 功能测试：NOTIFY/LISTEN/UNLISTEN正常功能、事务行为、异常与边界；pg_listening_channels()；读写分离组合<br/>兼容性测试：仅PG模式<br/>升级/资料测试：随需求验证 |
| 40   | 【神通】gsql支持gset元命令               | Tools     | QA       | 功能测试：\\gset在A/B/PG/D库下功能；gsql脚本断言（\\echo输出比对、SQL变量替换）<br/>约束测试：0行/多行/NULL/无列名报错<br/>异常测试：错误命令拼写、分号末尾、特殊控制变量赋值<br/>交互测试：与FETCH_COUNT、游标DECLARE/FETCH/CLOSE、事务、\\set/\\unset交互 |
| 41   | 【神通】gsql -c参数增强                  | Tools     | QA       | 功能测试：多个-c参数及与-f混用，不同元命令、与不同参数组合，结果按序输出<br/>兼容性测试：A/B/PG库及轻量版/企业版<br/>性能测试：并发、大数据量<br/>可靠性测试：数据库异常中断后可正确执行<br/>资料测试：参数功能及用法描述准确 |
| 42   | 【神通】支持函数gen_random_uuid生成uuid  | StorageEngine | QA       | 功能测试：合法/非法入参、权限；与其他函数/子句结合、存储过程/匿名块/事务；UUID随机性<br/>升级回滚：B012→B015后函数功能可用<br/>资料测试：随需求验证 |
| 43   | 【神通】TRUNCATE语句支持RESTART IDENTITY | SQL       | QA       | 功能测试：TRUNCATE RESTART IDENTITY清空表数据并将关联序列重置为初始值；权限（表TRUNCATE+序列ALTER）；SERIAL/IDENTITY耦合<br/>兼容性/资料测试：随需求验证 |
| 44   | 【移动】python驱动兼容opensearch接口     | Plugin    | QA       | 功能测试：索引管理、文档操作、查询检索、向量/混合检索及迁移兼容 |
| 45   | 【南大】支持select 别名 = 列名 from 表名的语 | SQL | QA       | 功能测试：默认按别名语法解析；d_format_behavior_compat_options=disable_target_alias时等号按原语法解析；带模式名、不同表类型、别名使用<br/>异常测试：异常操作验证是否合理<br/>兼容性测试：仅D模式且需手动创建shark插件<br/>升级/资料测试：随需求验证 |
| 46   | 【南大】支持CREATE FORCE VIEW语法        | SQL         | QA       | 功能测试：CREATE FORCE VIEW/CREATE OR REPLACE FORCE VIEW正向及基表不存在强制创建、补齐后重解析；异常场景（权限、对象不存在、字段不一致）；视图依赖基表/其他对象；跨Schema、事务、gs_dump导入导出<br/>兼容性测试：仅A库支持，B/PG/D库不支持 |

# 3、版本概要测试结论

openGauss 7.0.0 LTS版本整体测试按照release-manager团队的计划，在7.0.0 RC3发布后启动LTS系统测试。版本测试采取系统测试+集成验证+回归测试的策略，实际完成了B001至B017的需求转测与系统测试，以及B018至B023的SIT测试及问题单回归。

版本迭代1：对应openGauss 7.0.0 LTS B001版本至openGauss 7.0.0 LTS B003版本。主要进行oGRAC能力补齐增强需求，以及内核pgstat、BM2分词器扩展等需求验收，开展一轮全量CI测试和分析优化。

版本迭代2至版本迭代4：对应openGauss 7.0.0 LTS B004版本至openGauss 7.0.0 LTS B016版本。该阶段主要进行新需求测试设计、验收和issue回归，重点覆盖oG-Memory/Open-Viking记忆框架及AI向量数据库相关需求，鲲鹏超节点适配，数据库可靠性及性能提升，工具链及运维能力增强，ograc多写相关需求，伙伴兼容性需求；持续进行专项能力测试，并对已修复的问题单进行回归和对已验收完成特性测试报告进行评审。该阶段主要进行新需求验收，评审和优化已完成需求测试报告；同时完成了自动化套件、专项测试流水线、测试环境从黄区到蓝区的迁移、调试。

openGauss 7.0.0 LTS B017版本主要转测联合伙伴需求和少量内部需求，该阶段进行历史需求收尾和新需求验收，同时评审和优化已完成需求测试报告。

集成测试阶段：对应openGauss 7.0.0 LTS B018版本至openGauss 7.0.0 LTS B023版本开展集成测试与问题回归，覆盖工具链、兼容性、内核加速、资源池化、oGRAC等继承能力测试，涉及功能、可靠性、安全、性能、长稳、升级、资料领域，累计执行7.5w+用例，保证社区LTS版本基础功能正常、稳定性良好。

openGauss 7.0.0 LTS版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、稳定性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本计划交付需求46个，实际交付46个，交付率100%，所有发布需求均验证通过。openGauss 7.0.0 LTS版本共发现有效问题630个。修复问题回归测试结果正常，版本整体质量良好。

# 4、版本详细测试结论

openGauss 7.0.0 LTS版本详细测试内容包括：

1、通过自动化和手工用例测试看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、dataKit功能看护、资源池化存储能力、黑匣子安全能力、内核加速数据库优势特性、向量数据库、oGRAC、系统性能、系统可靠性等维度进行openGauss继承特性测试，继承7.0.0 RC1/RC2/RC3功能无丢失。

2、在内核场景化的竞争力构建上，持续优化传统主备cache-miss、to_char、宽表更新、索引膨胀和向量索引构建性能；在数据库高可用方面，资源池化failover适配在线reform、oGRAC RTO加速；在AI适配上，BM25词典与global idf、数据分支增强、ogMemory/OpenViking记忆链路持续发力。测试覆盖上述需求，重点关注功能完备性及性能指标的达成。但后续仍需从产品可靠性、竞争力的角度出发，持续进行加固测试和关键指标验证。

3、在oGRAC能力构建上，交付兼容性框架、数据类型、parser/bison重构、DFX、三方库、rowid、MES SHM、CM磁盘阈值和ogsql Readline等能力，语法覆盖面显著提升。在工具链方面，gsql增强、Datakit迁移优化、MySQL断点续传、OpenSearch/Oracle迁移和智能运维等能力，后续仍需对工具的易用性和稳定性提高要求。

4、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试以及资源池化failover长稳等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

5、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 4.1 特性测试结论

### 4.11 继承特性评价

对产品所有继承特性进行评价，包含9大项51小项，根据集成测试结果和问题遗留，进行质量评估。本表继承openGauss 7.0.0 RC3版本已发布能力。

| Domain           | Feature                 | 质量评估                   | 备注                                                         |
| ---------------- | ----------------------- | -------------------------- | ------------------------------------------------------------ |
| DataKit          | 插件管理                | <font color=green>▮</font> | 测试插件管理模块，关注该模块下各子特性功能正常及界面显示正常 |
|                  | 安全中心                | <font color=green>▮</font> | 测试安全中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 日志中心                | <font color=green>▮</font> | 测试日志中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 资源管理                | <font color=green>▮</font> | 测试资源管理模块，验证资源管理功能，界面显示正常             |
|                  | 安装部署                | <font color=green>▮</font> | 测试安装部署模块，验证安装部署流程正常，界面显示正常         |
|                  | 业务建模                | <font color=green>▮</font> | 测试业务建模模块，关注各子特性功能正常及界面显示正常         |
|                  | 基础运维                | <font color=green>▮</font> | 测试基础运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 智能运维                | <font color=green>▮</font> | 测试智能运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 告警监控                | <font color=green>▮</font> | 测试告警监控模块，关注各子特性功能正常及界面显示正常         |
|                  | 数据迁移                | <font color=green>▮</font> | 测试迁移功能，迁移前后数据一致性校验                         |
|                  | 监控插件工具            | <font color=green>▮</font> | 测试监控插件功能，界面显示正常                               |
|                  | 业务开发                | <font color=green>▮</font> | 测试业务开发模块，关注各子特性功能正常及界面显示正常         |
|                  | 兼容性评估              | <font color=green>▮</font> | 测试兼容性评估模块，关注各子特性功能正常及界面显示正常       |
|                  | 录制回放                | <font color=green>▮</font> | 测试录制回放模块，关注各子特性功能正常及界面显示正常         |
|                  | datakit_安装部署        | <font color=green>▮</font> | 测试安装部署、启停、兼容性                                   |
|                  | datakit_系统可靠性      | <font color=green>▮</font> | 测试系统级长稳、性能、可靠性、内存                           |
|                  | datakit_升级            | <font color=green>▮</font> | 测试平台与含插件升级                                         |
| 数据库服务       | SQL语法                 | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句，不同特性组合下用户、权限的验证(含资源池化、兼容性)(表/视图/索引等基础对象，fdw、postgis、物化视图) (含资源池化、兼容性) |
|                  | 功能SQL                 | <font color=green>▮</font> | 继承已有测试能力，vacuum、analyze、explain、事务(含自治事务)、审计、安全&加密、AI特性、密态等值查询、账本数据库、逻辑复制 |
|                  | 主备管理                | <font color=green>▮</font> | 继承已有测试能力，极致RTO、switchover、failover等            |
|                  | guc参数控制             | <font color=green>▮</font> | 继承已有测试能力，不同参数影响sql的执行效果，应该放到各个sql模块云设计；这里仅验证参数生效和组合场景 |
|                  | 内核工具链              | <font color=green>▮</font> | 继承已有测试能力，gs_ctl/gstrace/perctrl/pg_config/pagehack/pg_recvlogic/pg_controldata/pg_xlogdump/pg_resetxlog/gs_restore等 |
|                  | 资源负载管理            | <font color=green>▮</font> | 继承已有测试能力，gs_cgroup验证                              |
| 数据库备份恢复   | 物理备份/恢复           | <font color=green>▮</font> | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|                  | 逻辑备份/恢复           | <font color=green>▮</font> | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|                  | PITR、日志归档          | <font color=green>▮</font> | 继承已有测试能力，全量PITR物理恢复                           |
|                  | 延时备份                | <font color=green>▮</font> | 继承已有测试能力，支持延迟备份                               |
| 数据库管理与运维 | 安装卸载                | <font color=green>▮</font> | 继承已有测试能力，测试数据库安装、卸载全流程                 |
|                  | 升级                    | <font color=green>▮</font> | 测试带业务操作下多升级路径覆盖，升级成功后，特性功能运行正常 |
|                  | 实例管理                | <font color=green>▮</font> | 测试主备高可用(switchover/failover)、重启、启停              |
|                  | 运维视图                | <font color=green>▮</font> | 测试系统表与系统视图、系统schema(如dbe_perf、information_schema、WDR、pldebugger、db4ai等) |
| 数据库兼容性     | 环境兼容                | <font color=green>▮</font> | 测试硬件兼容、操作系统兼容、依赖软件版本                     |
|                  | 驱动兼容                | <font color=green>▮</font> | 测试jdbc/odbc/libpq/psycopg2等 mysql兼容(协议兼容、类型兼容)继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动 |
|                  | 生态兼容                | <font color=green>▮</font> | 测试ORM(mybatis)，连接池(druid)                              |
|                  | mysql兼容性             | <font color=green>▮</font> | 测试SQL语法（单双引号、反引号、关键字、类型、函数、操作符等）、通信协议 |
|                  | A库兼容性               | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | pg兼容性                | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | D库兼容性               | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | 资源池化兼容性          | <font color=green>▮</font> | 测试资源池化特性，双集群极致RTO、网络复制双集群、refrom、主备实时一致性 |
| 系统性能         | 系统性能                | <font color=green>▮</font> | 测试2P/4P性能、主备、RTO(含兼容性、资源池化)                 |
| 系统可靠性       | 系统可靠性/长时间稳定性 | <font color=green>▮</font> | 故障注入测试，包含MOT、兼容性、资源池化等内存专项、长稳测试  |
| 资源池化         | reform                  | <font color=green>▮</font> | 测试集群对外提供服务高可用能力                               |
|                  | 极致RTO\按需回放        | <font color=green>▮</font> | 测试主机故障后集群不可用情况快速恢复提供服务的能力           |
|                  | 双集群                  | <font color=green>▮</font> | 测试资源池化双集群容灾能力测试，保障主备集群切换功能正常     |
|                  | 非池化场景能力继承      | <font color=green>▮</font> | 测试包含语法兼容性、工具等用例，保证资源池化场景下非池化能力不丢失 |
|                  | 黑匣子                  | <font color=green>▮</font> | 测试API单接口，API组合场景提供快速恢复数据零丢失能力；覆盖可靠性测试：性能测试 |
| 内核加速         | ustore                  | <font color=green>▮</font> | 测试ustore相关存储、索引、闪回等特性用例，保障ustore功能正常 |
|                  | HTAP                    | <font color=green>▮</font> | 测试行列转换功能及相关GUC参数生效                            |
|                  | 向量索引                | <font color=green>▮</font> | 测试DataVec向量数据库HNSW、IVFFLAT、PQ、DiskANN、RabitQ索引创建、修改、重建及检索模块，覆盖所有索引选项参数及查询检索参数 |
|                  | 全文检索索引            | <font color=green>▮</font> | 测试BM25索引和GIN索引创建、修改、重建及检索模块，覆盖所有索引选项参数及查询检索参数 |
|                  | OGAI                    | <font color=green>▮</font> | 测试在数据库内部直接调用AI模型进行文本向量化、文本生成、文档重排序等操作 |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 4.12 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估，给出特性质量评估结论。

| 特性名称                                       | 测试情况说明                                                 | 约束                                                         | 质量点灯                    | 遗留问题 |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | -------- |
| pgstat优化                                     | 围绕pg_stat_*系列视图开展功能、准确性、一致性、稳定性、升级及资源占有率优化验证，覆盖表级/索引/数据库/函数/活动会话/复制/IO/冲突/后台写入/坏块统计及临时表、分区表、事务内统计、主备一致性。发现1个问题，整体质量良好。 | 不涉及性能有关测试                                           | <font color=green>▮</font>  | 无       |
| 传统主备性能优化cache-miss优化                 | 共计执行26个用例，覆盖功能、性能、可靠性、升级和资料测试。功能验证4个优化项不影响继承功能；性能测试优化后平均QPS提升5%；升级覆盖7.0.0 RC3→7.0.0 LTS及回退；可靠性覆盖switchover/failover/关键worker故障。发现2个有效问题，整体质量良好。 | 1、track_activities=off只关闭统计观测，不关闭rowdesc缓存功能<br/>2、shared GPC plan场景禁止继续使用共享row_desc_buf作为rowdesc宿主 | <font color=green>▮</font>  | 无       |
| 索引膨胀优化                                   | 共计执行6个用例，覆盖功能、性能、资料测试。insertpt路径下IO速率下降约4.52倍，大于验收目标4倍。发现1个资料问题，整体质量良好。 | 仅当页空间不足触发分裂时进入该路径；普通插入路径不变         | <font color=green>▮</font>  | 无       |
| 向量索引构建性能优化                           | 覆盖功能测试、性能测试。功能验证插件创建与调用正常；100万数据构建1024维向量索引在2.1min完成，性能满足要求。整体质量良好。 | openGauss-server编译必须使用cmake编译                        | <font color=green>▮</font>  | 无       |
| to_char函数性能优化                            | 共计执行11个用例，覆盖功能、兼容性、性能测试。format为"999,999.99"和"999,999,999.99"时性能提升35%，其他to_char场景无劣化。需求100%实现，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 宽表null列占比高场景更新性能优化               | 共计执行12个用例，覆盖功能、可靠性、兼容性、性能测试。全null表列更新性能提升35%以上，其他场景无劣化。需求100%实现，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 资源池化failover适配在线reform                 | 设计并执行28条用例，覆盖默认参数、并行回放、极致RTO、按需回放、按需回放+实时构建五种配置下的故障注入、二次故障、升主时间对比和7*24h长稳后failover。所有用例通过，未发现缺陷，整体质量良好。 | 仅在资源池化部署模式下生效；不同回放模式需通过对应GUC配置启用；TPCC压测10min后注入故障；kill gaussdb/dss连续6次验证反复故障稳定性 | <font color=green>▮</font>  | 无       |
| CM支持订阅sysSentry                            | 覆盖reboot/panic故障检测（故障节点与CMS主同节点/非同节点）、通知及时性、xalarm节点映射、开关功能及资料完整性准确性检查。共发现2个缺陷。 | 不涉及性能有关测试                                           | <font color=green>▮</font>  | 无       |
| 黑匣子加固                                     | 覆盖CM-Rest API新增内存/CPU/磁盘IO监控接口及oGRecorder的gr_vfs_query_file_info接口优化，验证正常、异常及多线程并发。输出用例26个全部通过，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 鲲鹏超节点加速资源池化clog、csnlog             | 测试覆盖 GUC 参数、共享内存初始化、事务一致性、Snapshot/OldestXmin、主备切换、节点启停、在线 failover、资料与性能等场景。性能测试：300+300与500+100场景总性能分别提升14.24%/20.43%。输出测试用例39个共发现2个问题单，整体质量良好。 | 依赖 `enable_ub=on`、`ubs_mem_path` 正确配置、DSS/DMS 正常，且不与按需回放冲突 | <font color=green>▮</font>  | 无       |
| oGRAC兼容性框架                                | 共计执行9个用例，覆盖功能、资料测试，验证A/B/C兼容库解析器、系统函数、操作符、系统视图、系统表及类型转换规则。发现1个功能问题，整体质量良好。 | parse语法解析和操作符兼容时需要开启USE_BISON_PARSER          | <font color=green>▮</font>  | 无       |
| oGRAC常用数据类型兼容                          | 共计执行28个用例，覆盖N开头字面量、f/d结尾写法、nclob及NATIONAL CHARACTER等类型，需求100%实现，整体质量良好。 | 不涉及性能有关测试                                           | <font color=green>▮</font>  | 无       |
| oGRAC-DFX增强                                  | 覆盖dv_dss_time_stats、dv_sql_excution_plan、dv_drc_buf_info、dv_drc_local_lock_info、dv_buf_ctrl_info及dv_slow_sql新增字段采集准确性，输出用例35个，发现本特性问题1个、资料问题1个，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| oGRAC三方库优化                                | 共计执行22个用例，覆盖编译安装、变更组件功能、x86/ARM及容器兼容、TPCC无劣化和资料测试。发现2个有效问题（含1个资料问题），整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| oGRAC支持rowid数据类型                         | 共计执行10个用例，覆盖ROWID列DDL/函数/隐式转换/索引/DML/事务/存储过程与触发器及资料测试。发现1个问题，整体质量良好。 | 不涉及性能有关测试                                           | <font color=green>▮</font>  | 无       |
| oGRAC parser重构                               | 覆盖25类语法点，用例319条。B004 bison-on结果PASS 255条，非PASS 64条，发现9个问题。功能基本正常，整体质量良好。 | use_bison_parser=true                                        | <font color=green>▮</font>  | 无       |
| oGRAC-bison/flex框架重构-part2                 | use_bison_parser=true下覆盖DDL及PL/SQL语法，输出文本用例359条，发现19个问题。功能基本正常，整体质量良好。 | use_bison_parser=true                                        | <font color=green>▮</font>  | 无       |
| MES SHM共享内存传输通道优化                    | 输出功能用例37条，覆盖GUC、兼容性、CPU绑核、DMS透传、共享内存对象、对端映射、消息收发、心跳、清理释放、统计日志及故障注入。发现2个问题，整体质量良好。 | SHM模式仅在ss_interconnect_type=SHM时启用；依赖灵衢超节点UBS Memory与ub_dist_comm_queue；SHM模式下不启动mes_start_receivers；ss_shm_ub_comm_cpu_bind仅SHM模式生效 | <font color=green>▮</font>  | 无       |
| oGRAC RTO加速优化                              | oGRAC RTO优化特性共计执行39个用例，主要覆盖功能测试、性能测试、可靠性测试和资料测试。功能测试验证RBP新增9个GUC参数、rbps_ctl和cms管理rbps命令、rbps.conf配置验证、rbps缓存数据页；性能测试验证50w tpmc场景下RTO小于10s；可靠性验证0节点和1节点的rbps断连和rbps进程故障恢复；资料验证RBPS加速恢复说明、GUC参数使用介绍和rbps使用说明。用例执行率100%，发现5个有效问题（含1个资料问题），整体质量良好。 | 运行期实时分析在USE_RBP、RBP_RT_ANALYSIS均开启、双节点且未启动DBS时生效；RBPS以独立进程部署，由cms管理；页镜像发送须建立在WAL可见基础上 | <font color=green>▮</font>  | 无       |
| oGRAC-CM支持磁盘阈值检测                       | 覆盖特性功能测试、可靠性相关测试，包含配置加载、状态保护失效、自动状态保护、只读/读写状态切换冷却、手动恢复读写、高并发等场景，测试结果符合预期。输出文本用例63个，发现提交问题2个，整体质量良好 | oGRAC架构需包含cms组件                                       | <font color=green>▮</font>  | 无       |
| ogsql Readline行编辑与补全                     | 设计功能用例37个，覆盖功能、可靠性、兼容性和资料测试。关键字/对象补全、行编辑快捷键、历史命令主路径通过。发现功能缺陷14个，多为特殊分支场景，需继续迭代，整体测试质量良好。 | 仅Linux下stdin/stdout均为TTY且SET HISTORY ON时启用，Windows暂不启用；空行Ctrl+D停止读取，非空行Ctrl+D删除光标处字符；未识别ESC序列需被消费 | <font color=green>▮</font>  | 无       |
| Datakit迁移优化                                | 功能测试覆盖：1.MySQL全量迁移中，数据库对象迁移时同名用户进行测试，覆盖函数、存储过程、视图、触发器场景，验证目标库迁移后同名用户创建情况。2.在Datakit迁移流程中，涉及迁移模式、迁移对象选项，对前台页面显示、后台数据库数据进行测试。3.支持Datakit卸载时通过选项同步卸载prometheus、portal等组件，正常和异常场景下的卸载功能测试。以及历史任务前向兼容和资料测试。总共输出测试用例3个，发现1个问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| MySQL全量迁移支持断点续传                      | 共计执行30个用例，覆盖功能测试、性能测试、前向兼容性测试、资料测试。功能测试主要覆盖MySQL全量迁移从源端数据库到迁移工具到目标数据库中各阶段中断场景下，进入续传阶段数据完整性、准确性的功能测试。发现5个问题，整体质量一般。 | 只在迁移失败情况下可用断点续传，手动结束任务后不可续传       | <font color=yellow>▲</font> | 无       |
| 从OpenSearch迁移至openGauss                    | 共执行了45个用例，主要覆盖了功能验证，可靠性测试，资料测试、兼容性测试、性能测试五个方面。功能测试主要覆盖数据迁移类型映射正确，包括keyword、byte、integer、long、ip等语类型， 涵盖基本opensearch数据类型，迁移速度优于1MB/s。共发现4个问题，整体质量良好。 | 数据库需要配置白名单                                         | <font color=green>▮</font>  | 无       |
| Oracle2oGRAC                                   | 主要覆盖Oracle数据对象迁移到oGRAC数据库，主要包括：表、索引、约束、视图、触发器、函数、存储过程、类型、序列等。 不同数据类型迁移到oGRAC数据库，数据一致性、数据完整性验证。 分区表、主键表、表外键、表约束等复杂表结构迁移到oGRAC数据库，数据一致性、数据完整性验证。 简单视图、复杂视图、简单触发器、复杂触发器、简单函数、复杂函数简单存储过程、复杂存储过程等迁移到oGRAC数据库，数据一致性、数据完整性验证。 以及迁移工具易用性测试，包含参数配置、进度上报等功能测试；测试源端数据迁移到目标端性能测试，性能满足迁移指标300M/s。测试输出用例70个，发现问题14个，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 智能运维增强                                   | 共计执行65个用例，覆盖GaussMaster+DBMind连接与接口、可靠性、资料、安全和性能，共发现2个问题单，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| BM25支持自定义词典、索引空间优化               | 共计执行25个用例，覆盖功能、性能、升级、稳定性、资料测试。索引空间占用优化59.48%，大于验收目标40%。发现4个问题（2个功能、2个资料），整体质量良好。 | dict_path仅支持绝对路径；词典目录须满足固定文件名集合；缓存key使用realpath；更新词典后已有index失效，不允许alter dict_path | <font color=green>▮</font>  | 无       |
| BM25分布式global idf                           | 覆盖enable_bm25_global_idf、bm25_global_stat、bm25_table_stat及多参数组合；1CN+2DN、5000万数据场景性能劣化10%以内、召回率劣化2%以内。发现2个功能问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| openGauss支持数据分支增强                      | 三个子特性共计执行60个用例，覆盖并行回放、LFC+Prefetch、分支合并的功能/异常/性能/资料测试。并行回放tpmc提升11.55%，大于验收目标5%。发现3个问题，整体质量良好。 | LFC+Prefetch仅处理永久表；分支合并要求同源tenant且存在running endpoint，当前以schema为粒度；双方共有表合并要求字段签名一致且目标表须有主键 | <font color=green>▮</font>  | 无       |
| ogMemory对接OpenClaw，支持上下文缓存优化       | 有效测试用例22条全部通过（执行率/通过率100%），覆盖after_turn同步、CandidateMemory、ReAct merge、PolicyRouter、ContextWriter/Outbox、L0/L1/L2索引及分层召回等核心链路，表现符合预期。 | 多租户治理相关能力开发未进行设计，本次不测                   | <font color=green>▮</font>  | 无       |
| OpenViking对接openGauss                        | 完成部署、配置、健康检查、OpenClaw集成及LoCoMo small评测，35个问题全部完成平分，准确率82.86%；鲲鹏向量检索优于海光超30%。未发现问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【海量】支持JSON/XML函数及表达式               | 共计执行92个用例，覆盖A兼容模式下GMS_XMLDOM/GMS_XMLPARSER对象与函数、权限及系统表变更。发现2个问题，整体质量良好。 | 仅A兼容模式                                                  | <font color=green>▮</font>  | 无       |
| 【海量】建表时属性GENERATED AS IDENTITY        | 共计执行42个用例，覆盖PG/A/B/D模式建表定义、序列选项、INSERT/OVERRIDING、UPDATE/MERGE、ALTER、LIKE/继承、TRUNCATE RESTART、gs_dump/gs_restore及B/D模式兼容语法。发现3个缺陷（TRUNCATE RESTART丢START WITH、B库ON UPDATE静默放行、B库nocycle达上限静默复用），总体测试质量良好。 | PG/A/B/D四种兼容模式均支持，核心语义一致                     | <font color=green>▮</font>  | 无       |
| 【海量】MySQL兼容性-dual表支持子查询           | 共计执行14个用例，覆盖DUAL虚拟表结构、基本行为、表达式与函数、EXISTS/IN/ANY/标量/FROM子查询、DML、CTE、视图、异常边界及遮蔽/权限/事务。未发现问题，整体质量良好。 | 仅B兼容模式，需创建dolphin扩展                               | <font color=green>▮</font>  | 无       |
| 【海量】PG兼容性-支持pg_stat_progress_copy视图 | 共计执行11个用例，覆盖COPY FROM FILE/STDIN、COPY TO、行存/列存/分区表进度、权限、track_activities、清理与并发。发现1个问题，整体质量良好。 | 仅PG兼容模式                                                 | <font color=green>▮</font>  | 无       |
| 【海量】PG兼容性-pg_locks新增waitstart字段     | 共计执行9个用例，覆盖waitstart视图结构、时间语义、多等待者队列、各锁类型、等待结束及与pg_stat_activity交互。发现1个问题，整体质量良好。 | 仅PG兼容模式                                                 | <font color=green>▮</font>  | 无       |
| 【恩墨】支持INSERT ON CONFLICT                 | 共计执行51个用例，覆盖功能、可靠性、升级和资料测试。升级路径覆盖6.0.0→7.0.0 LTS。发现功能问题5个、资料问题1个，整体质量良好。 | 仅承诺A/PG兼容模式；目标表不支持外部表、列存表、视图；DO UPDATE必须提供conflict_target；DO NOTHING可省略conflict_target | <font color=green>▮</font>  | 无       |
| 【恩墨】支持Global Sequence Cache              | 共计执行42个用例，覆盖GLOBAL/SESSION序列缓存语法与DDL、兼容性、可靠性、升级和资料测试。功能主路径验证通过。发现功能问题1个、资料问题1个，整体质量良好。 | 未指定级别时默认为SESSION；老版本序列（relkind S/L）不可ALTER为GLOBAL；新版本可双向切换，切换后未用完cache丢弃 | <font color=green>▮</font>  | 无       |
| 【神通】支持notify/listen功能                  | 完成功能、可服务性、资料、特性耦合分析测试。共执行75个用例，发现4个问题单均已修正，回归测试结果正常，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【神通】gsql支持gset元命令                     | 验证\\gset在A/B/PG/D库下功能正确性、约束正确性、异常健壮性，以及与特殊控制变量、FETCH_COUNT、游标交互。输出用例27个，提交1个文档issue，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【神通】gsql -c参数增强                        | 共计执行112个用例，覆盖多个-c及与-f/-1/-l/ON_ERROR_STOP组合、兼容性、性能、可靠性和资料测试，共发现1个问题单，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【神通】支持函数gen_random_uuid生成uuid        | 覆盖合法/非法入参、权限、与其他函数/子句结合、存储过程/匿名块/事务、企业版/轻量版、升级回滚和资料测试。共执行15个用例，发现1个问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【神通】TRUNCATE语句支持RESTART IDENTITY       | 完成功能、可服务性、资料、特性耦合分析测试。共执行28个用例，发现1个问题单，回归测试结果正常，无遗留问题。 | 无                                                           | <font color=green>▮</font>  | 无       |
| 【移动】python驱动兼容opensearch接口           | 覆盖索引管理、文档操作、查询检索、向量检索和迁移兼容等核心场景，需求范围内关键功能均已验证。发现6个问题单，回归测试结果正常，无遗留问题。 | 依赖Python 3.6+及psycopg2；向量字段需正确配置dims/similarity/index_options；索引创建和删除建议在低峰期执行 | <font color=green>▮</font>  | 无       |
| 【南大】支持select 别名 = 列名 from 表名的语法 | 共计执行57个用例，覆盖功能、其他表类型、异常、兼容性、其他场景、升级和资料测试。默认按别名语法解析，开启disable_target_alias后等号按原语法解析。发现2个问题，整体质量良好。 | 仅D模式且需手动创建shark插件；别名不支持单引号包裹，可用双引号或方括号 | <font color=green>▮</font>  | 无       |
| 【南大】支持CREATE FORCE VIEW语法              | 共计执行40个用例，覆盖基础语法、异常场景、视图依赖基表/其他对象、扩展功能及兼容性测试。基表不存在时可强制创建，补齐后重解析恢复正常。发现2个问题，整体质量良好。 | 仅A兼容库；B/PG/D库不支持                                    | <font color=green>▮</font>  | 无       |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，缺陷密度大于3

<font color=green>▮</font>： 表示特性质量良好，缺陷密度小于3

## 4.2 专项测试结论

### 4.21 安全测试

openGauss 7.0.0 LTS版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、数据库安全用例CI连跑；针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库权限管理、数据库连接配置、数据库文件目录安全、数据库安全认证配置、数据库账号口令管理、数据库审计、数据库日志配置、数据库运行环境配置和数据库其他配置进行安全测试。

4、完成安全编码规范扫描，PR满足合入要求；开发白盒安全检视，问题修复后测试回归。

5、使用ICSL官方策略对数据库、工具链进行BAS扫描并分析整改，问题修复后测试回归。

6、使用SQLSmith对转测版本进行安全测试。

7、通过openGauss白盒扫描规则集扫描sql注入、仿冒对象、越权、敏感信息、命令注入等安全问题，并分析整改。

8、通过内部AI扫描等方式，对openGauss代码仓进行安全扫描，识别疑似安全问题626个，其中修复完善相关问题点381个，均已合入7.0.0版本。

openGauss 7.0.0 LTS版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 4.22 可靠性\稳定性测试

openGauss 7.0.0 LTS版本可靠性\稳定性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及6个长时间负载测试，并在LTS阶段补充资源池化failover适配在线reform的7*24h长稳验证。

1、故障注入类测试：在x86+Centos，x86openEuler环境下共计执行879个用例多轮次，测试用例累计执行率100%，测试发现问题已全部优化闭环，整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：执行6个用例多轮次，测试用例累计执行率100%，测试发现问题已解决并回归验证通过，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | astore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景2  | ustore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景3  | 分区表场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core；cpu、内存、io等系统资源使用均正常 |
| 场景4  | 压缩表场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core；cpu、内存、io等系统资源使用均正常 |
| 场景5  | 资源池化，一主一备主机读写备机只读场景，TPCC连跑insert/update/delete事务，主机800仓+500并发，备机800仓+200并发，7*24H测试 | 业务正常运行，主备机无core                                   |
| 场景6  | 资源池化，7*24H内，每12H一次全量备份，每6H一次增量备份；开启xlog归档长稳运行 | 备份功能正常；xlog归档运行正常                               |

### 4.23 性能测试

对关键性能指标进行摸底和测试，主要覆盖了A库、B库和资源池化场景下，2P单机/主备、4P单机的TPC-C测试及RTO测试。LTS阶段内核cache-miss优化验证平均QPS提升5%，to_char指定format场景性能提升35%，向量索引构建百万1024维达标；资源池化/oGRAC超节点SHM相关性能见4.12约束说明。继承性能基线多轮测试稳定。

| **指标大项**      | **指标小项**                   | **指标值** | **说明**          | 测试结论                                                     |
| ----------------- | ------------------------------ | ---------- | ----------------- | ------------------------------------------------------------ |
| TPCC              | A库astore 2P单节点 1H 普通表   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***166.5***W+                  |
|                   | A库astore 2P单节点 1H 分区表   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***163.9***W+                  |
|                   | A库astore 2P一主一备 1H 普通表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***144.2***W+                |
|                   | A库astore 2P一主一备 1H 分区表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***144.2***W+                |
|                   | A库astore 4P单节点 1H 分区表   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***255***W+                      |
|                   | B库astore 2P单节点 1H 普通表   | 145万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***162.1***W+                  |
|                   | B库astore 2P单节点 1H 分区表   | 145万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***163***W+                    |
|                   | B库astore 2P一主一备 1H 普通表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***143.6***W+                |
|                   | B库astore 2P一主一备 1H 分区表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***143.1***W+                |
|                   | B库astore 4P单节点 1H 分区表   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***250***W+                      |
|                   |                                |            |                   |                                                              |
| RTO高可用         | 一主一备failover               | 小于10s    | 此即为release基线 | **4**s                                                       |
| 资源池化sysbench  | 2P主机读写备机空载             | 3.5万      |                   | 主机读写备机空载场景下，主机使用oltp_read_write，性能为***4.2w tps***。 |
|                   | 2P主机读写备机只读             | 6万        |                   | 主机读写备机只读场景下，主机使用oltp_read_write，备机使用oltp_read_only，总计***6.5w tps***。 |
| 资源池化TPCC      | 2P主机读写备机空载 1H          | 120万      | 此即为release基线 | 主机读写备机空载 1H在极限场景配置下tpmC为***129.5***W+       |
|                   | 2P主机读写备机只读 1H          | 220万      | 此即为release基线 | 主机读写备机只读 1H在极限场景配置下tpmC为***379.6***W+       |
| 资源池化RTO高可用 | 单集群                         | 小于10s    |                   | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库主备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br/>故障前50wtpmC，85并发场景下，RTO恢复时间为***6.2s***，达到标准<10s。 |

### 4.24 兼容性测试

#### 4.241 升级兼容

针对openGauss 7.0.0 LTS版本，规划的升级路径如下表所示。测试整体情况：

1. openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3（包含容器化场景）/openGauss 6.0.5/openGauss 6.0.6（包含容器化场景）/openGauss 7.0.0 RC3灰度升级和指定节点升级到openGauss 7.0.0 LTS版本成功，升级失败或者升级未提交，可以成功回滚；
2. 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3灰度升级到openGauss 7.0.0 LTS版本成功，升级失败或者升级未提交，可以成功回滚；
3. 升级成功后，特性功能运行正常；
4. 升级成功后，元数据校验通过

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM灰度升级到openGauss 7.0.0 LTS不带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM灰度升级到openGauss 7.0.0 LTS不带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM指定节点升级到openGauss 7.0.0 LTS不带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM指定节点升级到openGauss 7.0.0 LTS不带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM灰度升级到openGauss 7.0.0 LTS带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM灰度升级到openGauss 7.0.0 LTS带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3不带CM指定节点升级到openGauss 7.0.0 LTS带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 不带CM指定节点升级到openGauss 7.0.0 LTS带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 带CM灰度升级到openGauss 7.0.0 LTS带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 带CM灰度升级到openGauss 7.0.0 LTS带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 带CM指定节点升级到openGauss 7.0.0 LTS带CM版本 | 测试通过 |
| openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 带CM指定升级到openGauss 7.0.0 LTS带CM版本，再回滚，再升级提交 | 测试通过 |
| openGauss 6.0.3/openGauss 6.0.6容器化升级到openGauss 7.0.0 LTS版本 | 测试通过 |
| openGauss 6.0.3/openGauss 6.0.6容器化升级到openGauss 7.0.0 LTS版本，再回滚，再升级提交 | 测试通过 |
| 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 灰度升级到openGauss 7.0.0 LTS版本 | 测试通过 |
| 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 6.0.5/openGauss 6.0.6/openGauss 7.0.0 RC3 灰度升级到openGauss 7.0.0 LTS版本，再回滚，再升级提交 | 测试通过 |

#### 4.242 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏920/鲲鹏920新型号/鲲鹏950服务器/虚拟机上安装部署openGauss 7.0.0 LTS数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）、云盘上和磁阵上安装部署openGauss 7.0.0 LTS数据库 | 测试通过 |

#### 4.243 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS/X86+openEuler 24.03 LTS/ARM+openEuler 24.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 7.0.0 LTS数据库 | 测试通过 |

### 4.25 资料测试

| 序号 | **手册名称**                   | **测试结论** |
| ---- | ------------------------------ | ------------ |
| 1    | 《法律声明》                   | PASS         |
| 2    | 《关于openGauss》              | PASS         |
| 3    | 《新手入门》                   | PASS         |
| 4    | 《安装部署》                   | PASS         |
| 5    | 《使用与运维》- SQL教程        | PASS         |
| 6    | 《使用与运维》- 数据库管理指南 | PASS         |
| 7    | 《使用与运维》- 数据库运维指南 | PASS         |
| 8    | 《使用与运维》- 性能调优指南   | PASS         |
| 9    | 《使用与运维》- 数据迁移指南   | PASS         |
| 10   | 《重要特性》 - 向量数据库      | PASS         |
| 11   | 《重要特性》 - 资源池化        | PASS         |
| 12   | 《开发指南》- 应用开发指南     | PASS         |
| 13   | 《开发指南》- 编译指南         | PASS         |
| 14   | 《参考指南》- 插件参考         | PASS         |
| 15   | 《参考指南》- 数据库参考       | PASS         |
| 16   | 《参考指南》- 工具和命令参考   | PASS         |
| 17   | 《参考指南》- 特性参考         | PASS         |
| 18   | 《参考指南》- 附录             | PASS         |

## 4.3 DBV伙伴测试结论

| 伙伴名称 | 主导新需求数量 | 问题单数量 | 测试设计篇数 | 用例数量      | 测试报告 | 测试内容                                                     | 遗留问题个数 | 版本测试结论 |
| -------- | -------------- | ---------- | ------------ | ------------- | -------- | ------------------------------------------------------------ | ------------ | ------------ |
| 海量     | 5              | 7          | 5            | 文本用例：168 | 5        | 1、需求测试：XML数据操作函数（GMS_XMLDOM/GMS_XMLPARSER）、GENERATED AS IDENTITY、MySQL DUAL虚拟表、pg_stat_progress_copy视图、pg_locks.waitstart<br />2、继承特性验证：数据库服务（SQL语法、主备管理、GUC参数控制、内核工具链）、数据库管理与运维（实例管理）、数据库兼容性（资源池化兼容性）和可靠性测试 | 0            | 测试通过     |
| 恩墨     | 2              | 8          | 2            | 文本用例：93  | 2        | 1、需求测试：INSERT ON CONFLICT、Global Sequence Cache<br />2、问题单回归 | 0            | 测试通过     |
| 神通     | 5              | 8          | 5            | 文本用例：257 | 5        | 1、需求测试：notify/listen、gsql gset元命令、gsql -c参数增强、gen_random_uuid、TRUNCATE RESTART IDENTITY<br />2、问题单回归 | 0            | 测试通过     |
| 移动     | 1              | 6          | 1            | 文本用例：240 | 1        | 1、需求测试：python驱动兼容opensearch接口（PanWeiDB Python SDK）覆盖索引管理、文档操作、查询检索、向量/混合检索及迁移兼容<br />2、问题单回归 | 0            | 测试通过     |
| 南大     | 2              | 4          | 2            | 文本用例：97  | 2        | 1、需求测试：支持select 别名 = 列名 from 表名的语法、支持CREATE FORCE VIEW语法<br />2、继承特性验证：上个版本负责需求自动化实现和测试<br />3、兼容性测试：升级兼容 | 0            | 测试通过     |



# 5、问题单统计分析

## 5.1 问题单统计

openGauss 7.0.0 LTS版本截止目前转测23个B版本，共发现问题692个，有效问题630个，无效问题62个。修复问题回归测试结果正常，版本整体质量良好。详细分布见下表:

| 版本名称                 | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| ------------------------ | ------------ | ------------ | ---------- | ---------- |
| openGauss 7.0.0 LTS B001 | 2026/4/1     | 2026/4/15    | 63         | 5          |
| openGauss 7.0.0 LTS B002 | 2026/4/16    | 2026/4/22    | 26         | 2          |
| openGauss 7.0.0 LTS B003 | 2026/4/23    | 2026/4/29    | 16         | 2          |
| openGauss 7.0.0 LTS B004 | 2026/4/30    | 2026/5/13    | 36         | 8          |
| openGauss 7.0.0 LTS B005 | 2026/5/14    | 2026/5/20    | 25         | 4          |
| openGauss 7.0.0 LTS B006 | 2026/5/21    | 2026/5/27    | 15         | 0          |
| openGauss 7.0.0 LTS B007 | 2026/5/28    | 2026/6/3     | 35         | 3          |
| openGauss 7.0.0 LTS B008 | 2026/6/4     | 2026/6/10    | 30         | 3          |
| openGauss 7.0.0 LTS B009 | 2026/6/11    | 2026/6/17    | 14         | 1          |
| openGauss 7.0.0 LTS B010 | 2026/6/18    | 2026/6/24    | 22         | 1          |
| openGauss 7.0.0 LTS B011 | 2026/6/25    | 2026/7/1     | 28         | 7          |
| openGauss 7.0.0 LTS B012 | 2026/7/2     | 2026/7/8     | 42         | 8          |
| openGauss 7.0.0 LTS B013 | 2026/7/9     | 2026/7/15    | 25         | 2          |
| openGauss 7.0.0 LTS B014 | 2026/7/16    | 2026/7/22    | 18         | 0          |
| openGauss 7.0.0 LTS B015 | 2026/7/23    | 2026/7/29    | 43         | 2          |
| openGauss 7.0.0 LTS B016 | 2026/7/30    | 2026/8/5     | 20         | 2          |
| openGauss 7.0.0 LTS B017 | 2026/8/6     | 2026/8/12    | 31         | 2          |
| openGauss 7.0.0 LTS B018 | 2026/8/13    | 2026/8/19    | 21         | 2          |
| openGauss 7.0.0 LTS B019 | 2026/8/20    | 2026/8/26    | 33         | 3          |
| openGauss 7.0.0 LTS B020 | 2026/8/27    | 2026/9/2     | 28         | 4          |
| openGauss 7.0.0 LTS B021 | 2026/9/3     | 2026/9/9     | 56         | 1          |
| openGauss 7.0.0 LTS B022 | 2026/9/10    | 2026/9/12    | 3          | 0          |
| openGauss 7.0.0 LTS B023 | 2026/9/13    | 2026/9/16    | 0          | 0          |

## 5.2 问题单分析

本次测试共23轮，从openGauss 7.0.0 LTS B001持续到openGauss 7.0.0 LTS B023。问题单提单数量同需求转测周期对应，集成测试和需求测试少量并行。

2026.4.1启动openGauss 7.0.0 LTS版本迭代1测试，对应B001到B003版本，有效问题105个，约占有效问题总量17%。该阶段需求转测以oGRAC底座能力（兼容性框架、数据类型、DFX、三方库、rowid）和内核pgstat等为主，并同步开展全量CI分析优化；问题单主要来源于oGRAC（25）、数据库服务（24）、内核加速（15）和工具链（14），整体变化趋势相对平缓。

2026.4.30进入迭代2，对应B004到B007版本，有效问题111个，约占18%。该阶段继续内核/资源池化优化及工具链看护，B004有效问题36个、B007有效问题35个为该阶段高峰；问题单主要来源于数据库服务（25）、工具链（24）、资料（22）和oGRAC（20）。

2026.6.4进入迭代3，对应B008到B011版本，有效问题94个，约占15%。该阶段MES SHM、鲲鹏超节点、向量索引构建及AI相关需求转测，oGRAC（39）提单占比最高，B008有效问题30个、B011有效问题28个。

2026.7.2进入迭代4，对应B012到B016版本，有效问题148个，约占24%，为需求集中交付高峰。该阶段覆盖oGRAC bison重构、RTO加速、failover在线reform、伙伴INSERT ON CONFLICT，以及gsql/ogsql客户端增强、CM磁盘阈值、内核性能优化和BM25 global idf；问题单主要来源于oGRAC（47）、数据库服务（24）和工具链（20）。其中B012有效问题42个、B015有效问题43个，与需求交付高峰对应。迭代2至迭代4合计约占有效问题总量56%，需求转测、测试验证和问题发现整体能相互对应。

openGauss 7.0.0 LTS B017版本起进入集成验证与问题回归阶段，B017版本有少量内部和伙伴未完成需求继续测试活动；B018到B021SIT测试涉及全量用例和专项测试，包含CI连跑、手工用例执行、长稳、可靠性、性能、安全、资料等场景测试，过程中同步进行问题回归，B019-~B021问题单增多，与全量CI连跑和问题集中提交有关。B022到B023主要进行问题单回归和集成测试工作收尾，新增问题单较少。通过B版本和逐日分析提单数量，提单数量呈收敛趋势；对应B017到B023，有效问题172个，约占27%。SIT覆盖全量用例和专项测试，数据库服务（41）、工具链（31）、oGRAC（27）和内核加速（24）问题较为突出；。

按领域看，有效问题主要分布在oGRAC（150）、数据库服务（122）、工具链（93）、资料（87）、内核加速（72）、资源池化（45）和兼容性（36）、可靠性\稳定性\性能（25），前三类合计约占有效问题总量57.9%。

## 5.3 执行用例与问题单数对应关系

| 领域               | 执行用例数量 | 有效问题单数量（CI+需求+集成测试+开发自提） |
| ------------------ | ------------ | ------------------------------------------- |
| 数据库服务         | 50000+       | 122                                         |
| 工具链             | 5500+        | 93                                          |
| 资源池化           | 4000+        | 45                                          |
| oGRAC              | 1700+        | 150                                         |
| 兼容性             | 11000+       | 36                                          |
| 内核加速           | 2000+        | 72                                          |
| 可靠性\稳定性\性能 | 1100+        | 25                                          |
| 资料               | 50+          | 87                                          |

新需求文本/自动化用例合计约2600+。继承特性继续沿用RC3阶段7.5w+全量用例看护。

## 5.4 集成测试迭代版本基线

| 迭代版本 | 转测试B版本                                       | 测试项                                                       |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| 迭代1    | openGauss 7.0.0-LTS.B001~openGauss 7.0.0-LTS.B003 | oGRAC能力补齐增强相关需求测试；低频率全量CI、性能/可靠性/长稳测试 |
| 迭代2    | openGauss 7.0.0-LTS.B004~openGauss 7.0.0-LTS.B007 | 内核加速和黑匣子加固相关需求测试；低频率全量CI、性能/可靠性/长稳测试 |
| 迭代3    | openGauss 7.0.0-LTS.B008~openGauss 7.0.0-LTS.B011 | 向量数据库和智能运维相关需求测试；低频率全量CI、性能/可靠性/长稳测试 |
| 迭代4    | openGauss 7.0.0-LTS.B012~openGauss 7.0.0-LTS.B016 | ograc多写相关需求，伙伴兼容性需求、可靠性、性能优化相关需求测试；周力度全量CI、性能/可靠性/常稳/升级/安全测试 |
| 集成验证 | openGauss 7.0.0-LTS.B017~openGauss 7.0.0-LTS.B023 | 全量CI连跑、专项测试、集成测试、问题单回归                   |



# 6、附件

无

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@sungang14](https://gitcode.com/sungang14)                                               

+ [@czywj](https://gitcode.com/czywj)

+ [@u012280419](https://gitcode.com/u012280419)

+ [@l1azzzy](https://gitcode.com/l1azzzy)

+ [@szoscar55](https://gitcode.com/szoscar55)

+ [@cloudsbreak](https://gitcode.com/cloudsbreak)

+ [@xiaobai-181](https://gitcode.com/xiaobai-181)

+ [@yc-jianda](https://gitcode.com/yc-jianda)

+ [@liuzhen001](https://gitcode.com/liuzhen001)

 
