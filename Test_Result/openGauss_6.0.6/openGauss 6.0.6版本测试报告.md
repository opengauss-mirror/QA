![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改章节 | 修改描述 | 作者      |
| -------- | -------- | -------- | -------- | --------- |
| 2026.08.29 | 1.0      | 初稿撰写 |          | zhangxubo |

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

**Keywords 关键词**：openGauss 6.0.6

**Abstract 摘要**：主要是描述了openGauss 6.0.6版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                   | 中文解释       |
| ------ | -------------------------- | -------------- |
| SQL    | Structured Query Language  | 结构化查询语言 |
| DML    | Data Manipulation Language | 数据操纵语言   |
| DDL    | Data Definition Language   | 数据定义语言   |
| DCL    | Data Control Language      | 数据控制语言   |


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 6.0.6版本回合了主干分支的部分需求和若干关键缺陷代码。本文主要描述了openGauss 6.0.6版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 6.0.6版本整体质量良好。

# 测试版本说明

| 版本名称                  | 测试起始时间 | 测试结束时间 |
| ------------------------- | ------------ | ------------ |
| openGauss 6.0.6 B001      |  2026-07-07      |   2026-07-28       |
| openGauss 6.0.6 B002 |  2026-07-29           |   2026-08-08       | 
| openGauss 6.0.6 B003 |  2026-08-10           |    2026-08-24      | 
| openGauss 6.0.6 B004 |  2026-08-27           |      2026-08-30    | 


测试环境：

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：HDD 1T <br />OS：CentOS Linux 7.6.1810（Core）<br />文件系统：xfs<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 20.03 (LTS)<br />文件系统：ext4<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 22.03 (LTS)<br />文件系统：ext4<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 24.03 (LTS)<br />文件系统：ext4<br /> |      |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| OpenEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，aarch版本ISO |
| OpenEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，x86-64版本ISO |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 6.0.6版本整体测试按照release-manager团队的计划，版本测试规划采取多轮测试方式，完成了1轮系统测试+多轮回归测试：

第1轮（B001）完成所有回合的特性测试，同时对继承特性开展第1轮的全量测试，覆盖功能、可靠性、安全、性能、长稳、升级等测试，同步全量CI连跑。第2-4轮（B002-B004）验收回合版本的问题单，并对特性测试、专项测试给出结论。

openGauss 6.0.6版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本验收需求14个，问题120个（B001 85个，B002-B004 35个），完成率100%。

# 版本详细测试结论

openGauss 6.0.6版本详细测试内容包括：

1、通过自动化看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、系统性能、系统可靠性6个维度进行openGauss继承特性测试，继承功能无丢失；

2、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

3、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 特性测试结论

### 继承特性评价

| Domain           | Feature        | 质量评估    +              | 备注                                                         |
| ---------------- | -------------- | -------------------------- | ------------------------------------------------------------ |
| 数据库服务       | SQL语法        | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句，不同特性组合下用户、权限的验证(兼容性)(表/视图/索引等基础对象，fdw、postgis、物化视图) (兼容性) |
|                  | 功能SQL        | <font color=green>▮</font> | 继承已有测试能力，vacuum、analyze、explain、事务(含自治事务)、审计、安全&加密、AI特性、密态等值查询、账本数据库、逻辑复制 |
|                  | 主备管理       | <font color=green>▮</font> | 继承已有测试能力，极致RTO、switchover、failover等            |
|                  | guc参数控制    | <font color=green>▮</font> | 继承已有测试能力，不同参数影响sql的执行效果，应该放到各个sql模块云设计；这里仅验证参数生效和组合场景 |
|                  | 内核工具链     | <font color=green>▮</font> | 继承已有测试能力，gs_ctl/gstrace/perctrl/pg_config/pagehack/pg_recvlogic/pg_controldata/pg_xlogdump/pg_resetxlog/gs_restore等 |
|                  | 资源负载管理   | <font color=green>▮</font> | 继承已有测试能力，gs_cgroup验证                              |
| 数据库备份恢复   | 物理备份/恢复  | <font color=green>▮</font> | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|                  | 逻辑备份/恢复  | <font color=green>▮</font> | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|                  | PITR、日志归档 | <font color=green>▮</font> | 继承已有测试能力，全量PITR物理恢复                           |
|                  | 延时备份       | <font color=green>▮</font> | 继承已有测试能力，支持延迟备份                               |
| 数据库管理与运维 | 安装卸载       | <font color=green>▮</font> | 继承已有测试能力，测试数据库安装、卸载全流程                 |
|                  | 升级           | <font color=green>▮</font> | 测试带业务操作下多升级路径覆盖，升级成功后，特性功能运行正常 |
|                  | 实例管理       | <font color=green>▮</font> | 测试主备高可用(switchover/failover)、重启、启停              |
|                  | 运维视图       | <font color=green>▮</font> | 测试系统表与系统视图、系统schema(如dbe_perf、information_schema、WDR、pldebugger、db4ai等) |
| 数据库兼容性     | 环境兼容       | <font color=green>▮</font> | 测试硬件兼容、操作系统兼容、依赖软件版本                     |
|                  | 驱动兼容       | <font color=green>▮</font> | 测试jdbc/odbc/libpq/psycopg2等 mysql兼容(协议兼容、类型兼容)继承已有测试能力，支持JDBC、ODBC、、GDBC驱动 |
|                  | 生态兼容       | <font color=green>▮</font> | /                                                            |
|                  | mysql兼容性    | <font color=green>▮</font> | 测试SQL语法（单双引号、反引号、关键字、类型、函数、操作符等）、通信协议 |
|                  | oracle兼容性   | /                          | /                                                            |
|                  | pg兼容性       | /                          | /                                                            |
| 系统性能         | 系统性能       | <font color=green>▮</font> | 测试2P/4P性能、主备、RTO(含兼容性)                           |
| 系统可靠性       | 系统可靠性     | <font color=green>▮</font> | 故障注入测试                                                 |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 需求测试结论

| 特性名称 | 测试情况说明 | 约束 | 质量电灯 | 遗留问题 |
| -------- | ------------ | ---- | -------- | -------- |
| 支持openEuler2403操作系统 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| DiskANN索引支持在线创建，增加向标混合多列索引构建和查询能力 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 支持LSG索引 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| IVF_RABITQ支持索引构建和查询 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| HNSW_RABITQ 增加vacuum和halfvec支持 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| HNSW_RABITQ支持以及增加fp32和sq8精排 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| BM25索引空间优化 + BM25自定义分词器 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 支持halfvec | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 支持ADIO | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 压缩支持兼容升级到7.0.0 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 支持rack_mem、dlopen libmemfabric.so | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| Clog/CSNLog/Oldestxmin传输优化 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| 小包RPC优化页面传输 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |
| SysSentry故障检测 | 转测无问题 | 无 | <font color=green>▮</font> | 无 |


### 问题单验收结论

对版本所有回合的问题验收，测试发现35个均已验收：

| 序号 | 问题描述 | ISSUE链接 | 验收状态 |
| ---- | -------- | --------- | -------- |
| 1 | [Bug]: CM 两节点安装， 配置第三方网管ip做高可用，在频繁杀掉主机做高可用测试时候会偶发备升主失败【HK】 | https://gitcode.com/opengauss/DCF/issues/20 | 已验收 |
| 2 | [Bug]: 【测试类型：功能测试】【测试版本：6.0.6】搭建容灾集群时报错 | https://gitcode.com/opengauss/openGauss-server/issues/8296 | 已验收 |
| 3 | [Bug]: dolphin.lower_case_table_names为1的 情况下， 开启ansi_quotes创建schema之后无法用use schema选择 | https://gitcode.com/opengauss/Plugin/issues/1615 | 已验收 |
| 4 | [Bug]: 【测试类型：资料测试】【测试版本：6.0.0LTS】ADIO已回合6.0.0.资料需同步修改 | https://gitcode.com/opengauss/docs/issues/7252 | 已验收 |
| 5 | [Bug]: 【测试类型：资料测试】【测试版本：6.0.0LTS】BM25支持自定义词典、索引空间优化已回合6.0.0 资料需同步修改 | https://gitcode.com/opengauss/docs/issues/7253 | 已验收 |
| 6 | [Bug]: 【测试类型：功能测试】【资源池化】【升级】6.0.0版本升级到6.0.6版本，元数据校验失败 | https://gitcode.com/opengauss/openGauss-server/issues/8301 | 已验收 |
| 7 | [Bug]: 【测试类型：资料测试】【测试版本：6.0.0LTS】RabitQ相关资料同步修改 | https://gitcode.com/opengauss/docs/issues/7254 | 已验收 |
| 8 | [Bug]: 【测试类型：功能测试】【ADIO】设置enable_adio_debug = on成功，查询结果为off | https://gitcode.com/opengauss/openGauss-server/issues/8302 | 已验收 |
| 9 | [Bug]: 【测试类型：功能测试】【资源池化】【升级】6.0.3版本升级到6.0.6版本后回滚，执行gs_upgradechk verify报错 | https://gitcode.com/opengauss/openGauss-server/issues/8303 | 已验收 |
| 10 | [Bug]: 【6.0.6】parallel_workers范围为[1，128]，当前为[1，32]，未更新 | https://gitcode.com/opengauss/openGauss-server/issues/8307 | 已验收 |
| 11 | [Bug]: 【测试类型：功能测试】【升级】5.0.0/5.0.5版本升级到6.0.6版本，元数据校验失败 | https://gitcode.com/opengauss/openGauss-server/issues/8310 | 已验收 |
| 12 | [Bug]: 【测试类型：功能测试】【升级】6.0.0/6.0.5版本升级到6.0.6版本，元数据校验失败 | https://gitcode.com/opengauss/openGauss-server/issues/8311 | 已验收 |
| 13 | [Bug]: 在6.0.6上的A库，执行模零操作不报错 | https://gitcode.com/opengauss/openGauss-server/issues/8312 | 已验收 |
| 14 | [Bug]: 【测试类型：功能测试】【传统主备】【升级】6.0.0版本升级到6.0.6版本场景，执行gs_upgradechk verify报错 | https://gitcode.com/opengauss/openGauss-server/issues/8313 | 已验收 |
| 15 | [Bug]: magicset开启或关闭导致了结果集不一致，致命bug | https://gitcode.com/opengauss/openGauss-server/issues/8315 | 已验收 |
| 16 | [Bug]: 6.0.6  opensslCVE | https://gitcode.com/opengauss/openGauss-third_party/issues/275 | 已验收 |
| 17 | [Bug]: libcurl的几个CVE漏洞：CVE-2026-5773 CVE-2026-6276 CVE-2026-5545 CVE-2026-4873 CVE-2026-6429 CVE-2026-7168 CVE-2026-9547 | https://gitcode.com/opengauss/openGauss-third_party/issues/276 | 已验收 |
| 18 | [Bug]: 当表格类型为UNLOGGED表时，创建LSG索引导致数据库崩溃 | https://gitcode.com/opengauss/openGauss-server/issues/8319 | 已验收 |
| 19 | [Bug]: 【测试类型：功能测试】【测试版本：6.0.6】gs_check 在高版本linux OS中使用时报错，和ifconfig相关 | https://gitcode.com/opengauss/openGauss-server/issues/8325 | 已验收 |
| 20 | [Bug]: 【测试类型：工具功能】【测试版本：6.0.6】gs_checkperf 检查SSD性能报错不符合预期 | https://gitcode.com/opengauss/openGauss-server/issues/8326 | 已验收 |
| 21 | [Bug]: 【测试类型：SQL功能】【测试版本：6.0.6】B库format函数报错 | https://gitcode.com/opengauss/openGauss-server/issues/8327 | 已验收 |
| 22 | [Bug]: 【测试类型：SQL功能】【测试版本：6.0.6】B库设置dolphin.lower_case_table_names=0;执行show events语法报错 | https://gitcode.com/opengauss/openGauss-server/issues/8328 | 已验收 |
| 23 | [Bug]: 【测试类型：SQL功能】【测试版本：6.0.6】B库left函数报错 | https://gitcode.com/opengauss/openGauss-server/issues/8329 | 已验收 |
| 24 | [Bug]: 【测试类型：SQL功能】【测试版本：6.0.6】B库创建列存表，不插入数据查询表数据总数为null | https://gitcode.com/opengauss/openGauss-server/issues/8332 | 已验收 |
| 25 | [Bug]: 【测试类型：工具功能】【测试版本：6.0.6】收集日志信息报错（gs_collector ） | https://gitcode.com/opengauss/openGauss-OM/issues/772 | 已验收 |
| 26 | [Bug]: 【测试类型：SQL功能】【测试版本：6.0.6】创建timescaledb插件报错 | https://gitcode.com/opengauss/openGauss-server/issues/8334 | 已验收 |
| 27 | [Bug]: [6.0.6]发布端备机switchover升主，订阅端未同步数据 | https://gitcode.com/opengauss/openGauss-server/issues/8339 | 已验收 |
| 28 | [Bug]: 开启"ss_interconnect_type = 'SHM'"数据库重启备机启动失败 | https://gitcode.com/opengauss/openGauss-server/issues/8346 | 已验收 |
| 29 | [Bug]: 【测试类型：功能测试】【资源池化】6.0.6版本安装出现dss相关core，导致集群不能拉起 | https://gitcode.com/opengauss/openGauss-server/issues/8373 | 已验收 |
| 30 | [Bug]: guc set 未校验 ss_interconnect_type 参数的合法值范围 | https://gitcode.com/opengauss/openGauss-server/issues/8378 | 已验收 |
| 31 | [Bug]: openEuler24.03机器存在RemoteMemTotal，om安装配置shared_buffer失败 | https://gitcode.com/opengauss/openGauss-OM/issues/776 | 已验收 |
| 32 | [Bug]: 606版本漏洞 CVE-2026-30922 & CVE-2026-11850 & CVE-2026-34073 | https://gitcode.com/opengauss/openGauss-third_party/issues/289 | 已验收 |
| 33 | [Bug]: 【测试类型：功能测试】【升级】6.0.0升级到6.0.6B003存在元数据校验失败项 | https://gitcode.com/opengauss/openGauss-server/issues/8386 | 已验收 |
| 34 | [Bug]: 6.0.6资源池化sysbench不达标 | https://gitcode.com/opengauss/openGauss-server/issues/8390 | 已验收 |
| 35 | [Bug]: 资源池化CI测试6.0.6，GSDROPNODE用例执行失败，gs_dropnode工具检测到重复进程无法执行 | https://gitcode.com/opengauss/openGauss-OM/issues/778 | 已验收 |



## 专项测试结论

### 可靠性测试

openGauss 6.0.6 版本可靠性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及1个长时间负载测试。

1、故障注入类测试：共计测试1个版本，在x86+Centos，x86openEuler环境下共计执行707个用例，测试用例累计执行率100%，测试发现1个问题，整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：共计测试1个版本，共执行1个用例，测试用例累计执行率100%，未发现问题，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | TPCC连跑，insert/update/delete事务，200仓+100并发，虚拟机测试7*24H连跑 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |

### 性能测试

对关键性能指标进行摸底和测试，主要覆盖了传统主备的TPC-C测试及RTO测试，性能多轮测试稳定。

| **指标大项** | **指标小项**                                 | **指标值** | **说明**          | 测试结论                                    |
| ------------ | -------------------------------------------- | ---------- | ----------------- | ------------------------------------------- |
| TPCC         | 2P（Taishan 200 2280 7260）单节点 分区表     | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为**153.3**W+   |
|              | 2P（Taishan 200 2280 7260）单节点 非分区表   | 150万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为**150.3**W+   |
|              | 2P（Taishan 200 2280 7260）一主一备 分区表   | 120万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为**131.3**W  |
|              | 2P（Taishan 200 2280 7260）一主一备 非分区表 | 120万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为**134.2**W+ |
|              | 4P（Taishan 200 2480 7260）单节点 1H         | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为**232.9**W+       |
| RTO高可用    | 一主两同步备failover                         | 10s        | 此即为release基线 | **3**s                                      |


资源池化性能测试：(测试数据见 https://gitcode.com/opengauss/openGauss-server/issues/8390)

| **指标大项** | **指标小项**                                 | **指标值** | **说明**          | 测试结论                                    |
| ------------ | -------------------------------------------- | ---------- | ----------------- | ------------------------------------------- |
| TPCC         | 2P（Taishan 200 2280 7260）主机读写备机只读  | 220万      | 此即为release基线 |  **317.5**W                     |
|              | 2P（Taishan 200 2280 7260）主机读写备机空载  | 120万      | 此即为release基线 |  **123**W                       |
| sysbench     | 2P（Taishan 200 2280 7260）一主一备 分区表   | 37887      | 此即为release基线 |  **69146.32**     |
|              | 2P（Taishan 200 2280 7260）一主一备 非分区表 | 35000      | 此即为release基线 |  **43253.56**	                      |


### 安全测试

openGauss 6.0.6 版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、数据库安全用例CI连跑；针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库权限管理、数据库连接配置、数据库文件目录安全、数据库安全认证配置、数据库账号口令管理、数据库审计、数据库日志配置、数据库运行环境配置和数据库其他配置进行安全测试

openGauss 6.0.6版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 兼容性测试

#### 升级测试

针对openGauss 6.0.6版本，规划的升级路径如下表所示。共进行多轮测试，测试整体情况：

1、openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 升级到openGauss 6.0.6成功，升级失败或者升级未提交，可以成功回滚；

2、openGauss 6.0.6升级到openGauss master成功，回滚成功；

3、升级成功后，特性功能运行正常，压缩特性支持兼容升级到7.0.0。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 不带cm灰度升级到openGauss 6.0.6不带cm | 测试通过 |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 不带cm灰度升级到openGauss 6.0.6带cm | 测试通过 |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 带cm灰度升级到openGauss 6.0.6带cm | 测试通过 |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 不带cm灰度升级到openGauss 6.0.6不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 不带cm灰度升级到openGauss 6.0.6带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 5.0.*/openGauss 6.0.0 - 6.0.5 带cm灰度升级到openGauss 6.0.6带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.6不带cm升级到openGauss master不带cm            | 测试通过 |
| openGauss 6.0.6不带cm升级到openGauss master带cm              | 测试通过 |
| openGauss 6.0.6带cm升级到openGauss master带cm                | 测试通过 |
| openGauss 6.0.6不带cm升级到openGauss master不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.6不带cm升级到openGauss master带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.6带cm升级到openGauss master带cm，回滚后，再升级提交 | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 6.0.6数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 6.0.6数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/X86+openEuler 24.03 LTS环境下，安装部署openGauss 6.0.6数据库 | 测试通过 |

# 问题单统计

openGauss 6.0.6版本共回合问题120个（B001 85个，B002-B004 35个），无效问题0个，该版本规划解决的问题均已合入，验收通过：

| 版本名称                  | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| ------------------------- | ------------ | ------------ | ------------ | ------------ |
| openGauss 6.0.6 B001      |  2026-07-07  |   2026-07-28 | 18  | 0 |
| openGauss 6.0.6 B002      |  2026-07-29  |   2026-08-08 |  13 | 0 |
| openGauss 6.0.6 B003      |  2026-08-10  |   2026-08-24 |  5  | 0 |
| openGauss 6.0.6 B004      |  2026-08-27  |   2026-08-30 |  0  | 0 |

本次测试共多轮。B001合入问题单验收、全量测试，B002-B004对测试发现问题全部解决并完成验收。6.0.5遗留的元数据问题在本版本闭环。

# 附件

## 附件1：遗留问题列表

无

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@lvlintao666](https://gitee.com/lvlintao666)
+ [@sungang14](https://gitcode.com/sungang14)
+ [@zhangxubo](https://gitcode.com/zhangxubo)
