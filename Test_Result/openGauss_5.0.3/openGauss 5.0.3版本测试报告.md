![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改章节 | 修改描述 | 作者   |
| -------- | -------- | -------- | -------- | ------ |
| 2024.8.2 | 1.0      | 初稿撰写 |          | wan005 |

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

**Keywords 关键词**：openGauss 5.0.3

**Abstract 摘要**：主要是描述了openGauss 5.0.3版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                   | 中文解释       |
| ------ | -------------------------- | -------------- |
| SQL    | Structured Query Language  | 结构化查询语言 |
| DML    | Data Manipulation Language | 数据操纵语言   |
| DDL    | Data Definition Language   | 数据定义语言   |
| DCL    | Data Control Language      | 数据控制语言   |


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 5.0.3版本回合了修复的若干关键缺陷代码。本文主要描述了openGauss 5.0.3版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，oopenGauss 5.0.3版本整体质量良好。

# 测试版本说明

| 版本名称             | 测试起始时间 | 测试结束时间 |
| -------------------- | ------------ | ------------ |
| openGauss 5.0.3 B001 | 2024-7-19    | 2024-7-31    |
| openGauss 5.0.3 B002 | 2024-8-1     | 2024-8-5     |

测试环境：

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：HDD 1T <br />OS：CentOS Linux 7.6.1810（Core）<br />文件系统：xfs<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 20.03 (LTS)<br />文件系统：ext4<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 22.03 (LTS)<br />文件系统：ext4<br /> |      |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 5.0.3版本整体测试按照release-manager团队的计划，版本测试规划采取2轮测试方式，完成了1轮系统测试+1轮回归测试：

第1轮完成所有回合的特性测试，同时对继承特性开展第1轮的全量测试，覆盖功能、可靠性、安全、性能、长稳、升级等测试，同步全量CI连跑。第2轮验收回合版本的问题单，并对特性测试、专项测试给出结论。

openGauss 5.0.3版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本验收问题31个，完成率100%。

# 版本详细测试结论

openGauss 5.0.3版本详细测试内容包括：

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

### 问题单验收结论

对版本所有回合的问题验收

| 序号 | issue ID                                                | 问题描述                                                     | 验收状态 |
| ---- | ------------------------------------------------------- | ------------------------------------------------------------ | -------- |
| 1    | I91XDR                                                  | dolphin插件下，对于null值的分区与mysql不一致                 | 验收通过 |
| 2    | I944DT                                                  | 【测试类型：SQL功能】【测试版本：6.0.0】show create table分区表结果有误 | 验收通过 |
| 3    | I95N4J                                                  | 【测试类型：工具功能】【测试版本：6.0.0】【自动化】gs_probackup -j 16 在备机远程备份主机，线程无法退出，长时间打印keepalive message is received | 验收通过 |
| 4    | I99QVU                                                  | 【测试类型：接口功能】【测试版本：6.0.0】 JDBC调用getString()获取兼容B库cast(负数 as time)值与mysql不一致 | 验收通过 |
| 5    | I9CKN9                                                  | 【测试类型：SQL功能】【测试版本：6.0.0】【自动化】 兼容B库模式下，now， locatime ，sysdate等算数运算使用pg_typeof()函数返回类型不合理 | 验收通过 |
| 6    | I9D629                                                  | ModifyTable不支持SMP导致CORE                                 | 验收通过 |
| 7    | I9DMZC                                                  | vacuum analyze table导致数据库coredump                       | 验收通过 |
| 8    | I9DVKZ                                                  | 【测试类型：接口功能】【测试版本：6.0.0】 getObject()获取bit(10)类型值报错 | 验收通过 |
| 9    | I9HVTA                                                  | 【测试类型：接口功能】【测试版本：6.0.0】 bool/boolean/bit(1)/bit(10)几种类型处理不合理 | 验收通过 |
| 10   | I9JKUC                                                  | 【测试类型：接口功能】【测试版本：6.0.0】 JDBC连接目前不支持characterEncoding设置为SQL_ASCII | 验收通过 |
| 11   | I9L0FB                                                  | 【测试类型：接口功能】【测试版本：6.0.0】【自动化】 jdbc通过getObject方式进行time字段数据查询，秒以后的精度缺失 | 验收通过 |
| 12   | I9N07G                                                  | 【测试类型：SQL功能】【测试版本：6.0.0】 兼容B库执行sqlsmith产生core | 验收通过 |
| 13   | I9NDXN                                                  | 整数左移31位存在溢出                                         | 验收通过 |
| 14   | I9O68X                                                  | 【测试类型：接口功能】【测试版本：6.0.0】 A库、B库getObject()获取bit类型都不正确 | 验收通过 |
| 15   | I9O7RU                                                  | 执行bit_and函数core                                          | 验收通过 |
| 16   | I9R6XB                                                  | mysql模式下执行count() over(partition by)报错语法错误        | 验收通过 |
| 17   | I9RDKY                                                  | B兼容，hour函数，秒后超过6位小数，未作四舍五入               | 验收通过 |
| 18   | I9VUGG                                                  | 内存泄露（statctl.cpp）                                      | 验收通过 |
| 19   | IA6R56                                                  | form子查询带大写别名时报错                                   | 验收通过 |
| 20   | IA71X3                                                  | 【测试类型：接口功能】【测试版本：6.0.0】【自动化】yat使用opengauss-jdbc-6.0.0.jar包执行多维数组替换结果显示报错 | 验收通过 |
| 21   | IA7ET2                                                  | b兼容模式下，order by跟索引键无法走索引[zyzx]                | 验收通过 |
| 22   | IA7XZ2                                                  | 数据库使用阻塞                                               | 验收通过 |
| 23   | IA8DH7                                                  | timestamp数据类型使用between and时不走索引[zyzx]             | 验收通过 |
| 24   | IA9N5L                                                  | 【测试类型：功能】【测试版本：5.0.2】在B库执行update语句产生core | 验收通过 |
| 25   | IA9YRX                                                  | char数据使用cast函数转signed失败，B模式库【zyzx】            | 验收通过 |
| 26   | IAACU0                                                  | timestampdiff函数处理带时区的数据报错【zyzx】                | 验收通过 |
| 27   | https://gitee.com/opengauss/openGauss-server/pulls/5676 | 修复gs_xlog_keepers在5.0.0升级5.0.2时，系统表中数据存在不同的问题。 | 验收通过 |
| 28   | I9E0B9                                                  | 回放 & debug 模式下，当从候选队列拿到的 buffer 又被写脏时，降低日志级别 | 验收通过 |
| 29   | I8NFWY                                                  | 【masterPR回归5.0】gs_basebackup对用户权限提示信息需优化_5.0.0 | 验收通过 |
| 30   | IAF9GW                                                  | 设置behavior_compat_options为accept_empty_str，执行select replace('abc', 'abc') is null;报错 | 验收通过 |
| 31   | IABEN0                                                  | 修复sequence大小写忽略问题                                   | 验收通过 |

## 专项测试结论

### 可靠性测试

openGauss 5.0.3 版本可靠性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及1个长时间负载测试。

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

| **指标大项** | **指标小项**                           | **指标值** | **说明**          | 测试结论                                    |
| ------------ | -------------------------------------- | ---------- | ----------------- | ------------------------------------------- |
| TPCC         | 2P（Taishan 200 2280 7260）单节点 1H   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为**156.4**W+   |
|              | 2P（Taishan 200 2280 7260）单节点 8H   | 145万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为**152.2**W+   |
|              | 2P（Taishan 200 2280 7260）一主一备 1H | 120万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为**122.4**W+ |
|              | 2P（Taishan 200 2280 7260）一主一备 8H | 110万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为**119.4**W+ |
|              | 4P（Taishan 200 2480 7260）单节点 1H   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为**237.8**W+     |
| RTO高可用    | 一主两同步备failover                   | 10s        | 此即为release基线 | **8**s                                      |

### 兼容性测试

针对openGauss 5.0.3版本，规划的升级路径如下表所示。共进行1轮测试，发现issue测试整体情况：

1、openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2升级到openGauss 5.0.3成功，升级失败或者升级未提交，可以成功回滚；

2、openGauss 5.0.3升级到openGauss 6.0.0成功，除带cm升级回滚失败外，其余场景升级失败或者升级未提交，可以成功回滚；

3、升级成功后，除行存压缩特性，其余特性功能运行正常。

| 升级路径                                                     | 测试结论   |
| ------------------------------------------------------------ | ---------- |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2不带cm灰度升级到openGauss 5.0.3不带cm | 测试通过   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2不带cm灰度升级到openGauss 5.0.3带cm | 测试通过   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2带cm灰度升级到openGauss 5.0.3带cm | 测试通过   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2不带cm灰度升级到openGauss 5.0.3不带cm，回滚后，再升级提交 | 测试通过   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2不带cm灰度升级到openGauss 5.0.3带cm，回滚后，再升级提交 | 测试通过   |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1/openGauss 5.0.2带cm灰度升级到openGauss 5.0.3带cm，回滚后，再升级提交 | 测试通过   |
| openGauss 5.0.3不带cm升级到openGauss 6.0.0不带cm             | 测试通过   |
| openGauss 5.0.3不带cm升级到openGauss 6.0.0带cm               | 测试通过   |
| openGauss 5.0.3带cm升级到openGauss 6.0.0带cm                 | 测试通过   |
| openGauss 5.0.3不带cm升级到openGauss 6.0.0不带cm，回滚后，再升级提交 | 测试通过   |
| openGauss 5.0.3不带cm升级到openGauss 6.0.0带cm，回滚后，再升级提交 | 测试通过   |
| openGauss 5.0.3带cm升级到openGauss 6.0.0带cm，回滚后，再升级提交 | 测试不通过 |

#### 硬件兼容

无

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/环境下，安装部署openGauss 5.0.3数据库 | 测试通过 |

# 问题单统计

openGauss 5.0.3版本共发现问题8个，无效问题0个，该版本规划解决的问题均已合入，验收通过：

| 版本名称             | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| -------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 5.0.3 B001 | 2024-7-19    | 2024-7-31    | 8          | 0          |
| openGauss 5.0.3 B002 | 2024-8-1     | 2024-8-5     | 0          | 0          |

本次测试共2轮，2024.7.19正式进入openGauss 5.0.3测试，B001合入问题单验收、全量测试，暴露出问题多。B002对修复问题单点验收，无特性测试，未发现问题。

# 附件

## 附件1：遗留问题列表

| 序号 | issue号                                                      | 问题简述                                                     | 分类     | 问题级别 | 问题分析与影响                       | 规避措施 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- | ------------------------------------ | -------- |
| 1    | [IA56OP](https://e.gitee.com/opengaussorg/issues/table?issue=IA56OP) | 【测试类型：工具功能】【测试版本：6.0.0】【升级】3.0.5/5.0.2带cm升级至6.0.0带cm，回滚过程中启库失败 | 升级     | 次要     | 升级后回滚失败，主要问题在6.0.0      |          |
| 2    | [IAF4NL](https://e.gitee.com/opengaussorg/issues/table?issue=IAF4NL) | 【测试类型：SQL功能】【测试版本：5.0.3】substring函数指定入参为int类型上边界，执行报错 | sql功能  | 次要     | 执行报错                             |          |
|      | [IAEZZF](https://e.gitee.com/opengaussorg/issues/table?issue=IAEZZF) | 【测试类型：SQL功能】【测试版本：5.0.3】 dblink相关功能dblink_open、dblink_close如果指定连接名场景下，连接名错误不存在时，数据库core | 插件功能 | 次要     | 指定错误连接名，dblink_close执行报错 |          |
|      | [I7YEVF](https://e.gitee.com/opengaussorg/issues/table?issue=I7YEVF) | 【测试类型：SQL功能】【测试版本：5.1.0】【自动化】 问题描述：部分函数入参是point类型，预期结果发生变更 | sql功能  | 次要     | 预期结果不一致                       |          |

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@wan005](https://gitee.com/wan005)
+ [@peilinqian](https://gitee.com/peilinqian)
+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)
+ [@li-xin12345](https://gitee.com/@li-xin12345)
+ [@zhanghuan96](https://gitee.com/@zhanghuan96)
