![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改章节 | 修改描述 | 作者      |
| -------- | -------- | -------- | -------- | --------- |
| 2025.2.8 | 1.0      | 初稿撰写 |          | sungang14 |

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

**Keywords 关键词**：openGauss 6.0.1

**Abstract 摘要**：主要是描述了openGauss 6.0.1版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                   | 中文解释       |
| ------ | -------------------------- | -------------- |
| SQL    | Structured Query Language  | 结构化查询语言 |
| DML    | Data Manipulation Language | 数据操纵语言   |
| DDL    | Data Definition Language   | 数据定义语言   |
| DCL    | Data Control Language      | 数据控制语言   |


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 6.0.1版本回合了修复的若干关键缺陷代码。本文主要描述了openGauss 6.0.1版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，oopenGauss 6.0.1版本整体质量良好。

# 测试版本说明

| 版本名称             | 测试起始时间 | 测试结束时间 |
| -------------------- | ------------ | ------------ |
| openGauss 6.0.1 B001 | 2025-1-4     | 2025-1-17    |
| openGauss 6.0.1 B002 | 2025-1-18    | 2025-1-26    |

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

openGauss 6.0.1版本整体测试按照release-manager团队的计划，版本测试规划采取2轮测试方式，完成了1轮系统测试+1轮回归测试：

第1轮完成所有回合的特性测试，同时对继承特性开展第1轮的全量测试，覆盖功能、可靠性、安全、性能、长稳、升级等测试，同步全量CI连跑。第2轮验收回合版本的问题单，并对特性测试、专项测试给出结论。

openGauss 6.0.1版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本验收需求3个，问题72个，完成率100%。

# 版本详细测试结论

openGauss 6.0.1版本详细测试内容包括：

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

| 特性名称                                    | 测试情况说明                                                 | 约束                                                         | 质量电灯                   | 遗留问题 |
| ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------- |
| 主备同步与延时回放机制优化                  | 主备同步与延时回放机制优化测试，共执行用例2个。目前配置延迟备库后，备库重启时间不受延迟参数的影响，备库重启后，主备延迟时间配置依然生效，符合预期。发现issue2个，均已解决回归通过，整体质量良好。 | 无                                                           | <font color=green>▮</font> | 无       |
| 【DFX】添加gs_lwlock_status函数统计轻量级锁 | 验证在管理员用户，普通用户下多个操作系统安装portal的功能，整体质量良好，issue0个 | 无                                                           | <font color=green>▮</font> | 无       |
| 备库日志回放，新增一种日志分发算法          | 备库日志回放新增日志分发算法，通过benchmark工具构造备机日志堆积的场景，验证新算法开启时备机日志回放速度，和原算法同场景下回放速度进行对比，回放速度有一定提升。执行测试共1轮，未发现问题，整体质量良好。 | parallel_recovery_dispatch_algorithm参数设置为2，enable_batch_dispatch参数设置为on | <font color=green>▮</font> | 无       |

### 问题单验收结论

对版本所有回合的问题验收

| 序号 | issue ID | 问题描述                                                     | 验收状态         |
| ---- | -------- | ------------------------------------------------------------ | ---------------- |
| 11   | I8H74U   | opengauss丢失csnlog文件【M】                                 | 验收通过验收通过 |
| 22   | I9QOUM   | odbc支持设定连接的主机的类型【M】                            | 验收通过         |
| 32   | I9T4EC   | 客户端将sslmode设置为disable，仍然加载ssl相关lib，导致客户端core【M】 | 验收通过         |
| 4    | IA87WO   | 创表之后，gs_dump导出的时候上报告警，提示WARNING:archive items not in correct section order cursection | 验收通过         |
| 5    | IAR25T   | Python3.11导入报错                                           | 验收通过         |
| 6    | IASAHY   | 【测试类型：工具功能】【测试版本：7.0.0】gs_checkperf检查SSD盘报错 | 验收通过         |
| 7    | IATC2X   | 【测试类型：功能测试】【测试版本：7.0.0】【自动化】扩容时存在xml文件，先与-h指定的ip地址进行校验通过后在创建互信 | 验收通过         |
| 8    | IATG0R   | 5.0.3到master升级失败                                        | 验收通过         |
| 9    | IATKRM   | 当/etc/hosts存在冗余项时, cm_ctl query 可以成功执行, gs_om -t query 可能会失败 | 验收通过         |
| 10   | IATU9M   | 【测试类型：接口功能】【测试版本：7.0.0-RC1】【MS】开启M*协议兼容后，使用jdbc连接参数allowMultiQueries=true，查询语句查询多个结果返回结果集与M*不一致 | 验收通过         |
| 11   | IATYXE   | 【测试类型：接口功能】【测试版本：7.0.0-RC1】【MS】开启M*协议兼容后，使用jdbc连接参数useOldAliasMetadataBehavior=false，查询语句查询结果别名与M*不一致 | 验收通过         |
| 12   | IATYZY   | 【测试类型：接口功能】【测试版本：7.0.0-RC1】【MS】开启M*协议兼容后，使用jdbc连接参数transformedBitIsBoolean=true，执行查询语句返回结果与M*不一致 | 验收通过         |
| 13   | IATZ4E   | 【测试类型：接口功能】【测试版本：7.0.0-RC1】【MS】开启M*协议兼容后，在spring-boot项目中使用多数据源，在切换时，druid和hikari连接线程池获取连接查询数据库默认的数据隔离级别，opengauss查询默认隔离级别位default无法映射正常的数据库隔离级别报错 | 验收通过         |
| 14   | IAVXKM   | Mysql中\"被转义成“, 但是openguass却报错【YJ】                | 验收通过         |
| 15   | IAW8G8   | gs_dump带-c -t参数时，导出导入表后，触发器信息丢失【YJ】     | 验收通过         |
| 16   | IAX9A9   | 【质量加固】Assert处理错误修改                               | 验收通过         |
| 17   | IAZ7LL   | 【测试类型：SQL功能】【测试版本：7.0.0】【sql】replace into一张表的400W数据到另一张表，数据库异常退出【YJ】 | 验收通过         |
| 18   | IAZAUI   | 使用Jdbc6.0驱动连5.0版本数据库报错                           | 验收通过         |
| 19   | IAZK5X   | 【测试类型：社区CI】【测试版本：6.0.0】单节点stop后 start节点报超时 | 验收通过         |
| 20   | IAZL4P   | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】兼容A库下，getObject接口获取numeric列NAN报错Bad value for type BigDecimal : NaN | 验收通过         |
| 21   | IAZOV2   | 【测试类型：工具功能】【测试版本：7.0.0】 同一服务器上不同用户同时安装报错 | 验收通过         |
| 22   | IAZTHA   | 【测试类型：社区CI】【测试版本：6.0.0】gs_dropnode 掉备机  查询synchronous_standby_names 预期是ANY 1(dn_6003)  但现在实际查询是空 | 验收通过         |
| 23   | IB1NMX   | gs_checkperf无法创建输出文件夹                               | 验收通过         |
| 24   | IB2NY5   | 【测试类型：工具功能】【测试版本：7.0.0】【自动化】 om安装数据库预安装报错 | 验收通过         |
| 25   | IB327C   | psycopg2驱动建立连接过程中加锁程序exit之后没有释放锁         | 验收通过         |
| 26   | IB3O0M   | 6.0 代码中几处代码问题                                       | 验收通过         |
| 27   | IB3U11   | sum(int)的性能比sum(bigint)性能差很多                        | 验收通过         |
| 28   | IB53OU   | gs_check -i CheckRXTX 执行非预期,报错 name 'backIp' is not defined | 验收通过         |
| 29   | IB6RLT   | 【mysql_fdw】访问外部表出现宕机                              | 验收通过         |
| 30   | IB6WB1   | 【测试类型：工具功能】【测试版本：7.0.0-RC1】日构建版本预安装阶段报错/tmp/gauss_xxx临时文件不存在 | 验收通过         |
| 31   | IB7LHQ   | gs_dump在导入备份数据，如果触发器对应的表不存在，删除触发器的时候会报错【YJ】 | 验收通过         |
| 32   | IB7LKX   | 当表有唯一性约束时，导致的表信息再导入的时候会报错【YJ】     | 验收通过         |
| 33   | IB7LSW   | gs_dump导出导入表之间如果schema 有变更会导致无法导入【YJ】   | 验收通过         |
| 34   | IB7T6K   | timestamp(3)类型占位符传参进行范围查询数据为空，实际有数据   | 验收通过         |
| 35   | IB7W0G   | 主备切换超时，主机降备卡了几个小时无法结束[MS]               | 验收通过         |
| 36   | IB8ASK   | 【测试类型：工具功能】【测试版本：7.0.0-RC1】【ci】gs_dropnode使用-h指定多个节点进行缩容，执行报错 | 验收通过         |
| 37   | IB9EFE   | 【测试类型：社区CI】【测试版本：7.0.0】逻辑复制 报错发生了变更 | 验收通过         |
| 38   | IB9MEU   | 【测试类型：驱动接口功能】【测试版本：7.0.0】 psycopg2驱动在使用eventlet的sqlalchemy引擎连接数据库时，报错segment fault | 验收通过         |
| 39   | IBA6U6   | 【测试类型：功能测试】【测试版本：7.0.0-RC1】【资源池化】debug版本，stop主节点场景，待升主节点产生The Page's LSN[xxx] bigger than want set LSN[xxx]相关core | 验收通过         |
| 40   | IBBACH   | select count(*) from (select * from testa union all select * from testa) where now() is not null; 导致数据库core | 验收通过         |
| 41   | IBBPW4   | 存在后台线程空跑                                             | 验收通过         |
| 42   | IBCJHH   | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】 B模式下，blob类型和json类型的不兼容问题 | 验收通过         |
| 43   | IBCLLO   | 【测试类型：功能测试】【测试版本：7.0.0-RC1】enable_ustore在系统表pg_settings中显示的默认值是off，实际是数据库应用的是on | 验收通过         |
| 44   | IBD4F8   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（一） | 验收通过         |
| 45   | IBD4GL   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（二） | 验收通过         |
| 46   | IBD4KV   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（三） | 验收通过         |
| 47   | IBD5HI   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（七） | 验收通过         |
| 48   | IBD6JW   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（九） | 验收通过         |
| 49   | IBD6NJ   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（十一） | 验收通过         |
| 50   | IBD6VW   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（十五） | 验收通过         |
| 51   | IBD6WT   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（十七） | 验收通过         |
| 52   | IBDFU2   | 适配银河麒麟系统安装数据库                                   | 验收通过         |
| 53   | IBECUE   | B兼容下，关闭auto_recompile_function，触发器下insert插入出现re-compile后会导致数据库core掉【YJ】 | 验收通过         |
| 54   | IBEVKM   | 存储过程使用set_variables传参报different character set data is not allowed错误【YJ】 | 验收通过         |
| 55   | IB77UJ   | 【回合master】修复ad vg导致clean的问题                       | 验收通过         |
| 56   | IBGY1P   | 【6.0.1补丁版本】更新版本为6.0.1                             | 验收通过         |
| 57   | IB1MSJ   | 【回合】限制3.0版本上面带有压缩表升级到新版本                | 验收通过         |
| 58   | IBGHM7   | 修复带CM集群3.0.6到6.0.0指定节点升级提交/回滚core问题        | 验收通过         |
| 59   | IBDGNW   | 修复hash index回放问题                                       | 验收通过         |
| 60   | IBD6RR   | 【测试类型：SQL功能】【测试版本：7.0.0-Rc1】【升级】5.0.3升级7.0.0RC1，元数据不一致问题（十四） | 验收通过         |
| 61   | IBGHZ3   | 修复5.0.x升级到6.0.1失败的问题                               | 验收通过         |
| 62   | IBAWJ3   | 【回合6.0.0】fix spaces expansion of uheap                   | 验收通过         |
| 63   | IBGN3E   | fix bug that recoveryInstId update when recovery and checkpoint done | 验收通过         |
| 64   | IBGNDF   | 处理issue: 【升级】3.0.5升级至6.0.1后，gs_upgradechk工具执行报错 | 验收通过         |
| 65   | IBG867   | 修复5.0.3升级至6.0.1元数据不一致问题                         | 验收通过         |
| 66   | IATX32   | 【回合6.0.0】修复interval运算结果精度丢失的问题              | 验收通过         |
| 67   | IAU4ZR   | 【回合6.0.0】修复interval运算带引号导致结果错误的问题        | 验收通过         |
| 68   | IB4BQV   | recovery_min_apply_delay修改后无需重启才能生效为新设置值【M】 | 验收通过         |
| 69   | IAWLMP   | 修复2p主备性能劣化问题                                       | 验收通过         |
| 70   | IAGUMO   | 【6.0.1补丁版本】适配BCLinux使用openeuler镜像包安装          | 验收通过         |
| 71   | IBI3XD   | 【回合】修复带子事务的存储过程的执行计划core掉               | 验收通过         |
| 72   | IBD7IQ   | 修复5.0.3升级至6.0.1元数据不一致问题                         | 验收通过         |

## 专项测试结论

### 可靠性测试

openGauss 6.0.1 版本可靠性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及1个长时间负载测试。

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
|              | 2P（Taishan 200 2280 7260）单节点 非分区表   | 145万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为**152.4**W+   |
|              | 2P（Taishan 200 2280 7260）一主一备 分区表   | 120万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为**120.0**W  |
|              | 2P（Taishan 200 2280 7260）一主一备 非分区表 | 120万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为**121.5**W+ |
|              | 4P（Taishan 200 2480 7260）单节点 1H         | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为**240.9**W+     |
| RTO高可用    | 一主两同步备failover                         | 10s        | 此即为release基线 | **3**s                                      |

### 安全测试

openGauss 6.0.1 版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、数据库安全用例CI连跑；针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库权限管理、数据库连接配置、数据库文件目录安全、数据库安全认证配置、数据库账号口令管理、数据库审计、数据库日志配置、数据库运行环境配置和数据库其他配置进行安全测试

openGauss 6.0.1版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 兼容性测试

#### 升级测试

针对openGauss 6.0.1版本，规划的升级路径如下表所示。共进行2轮测试，测试整体情况：

1、openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0升级到openGauss 6.0.1成功，升级失败或者升级未提交，可以成功回滚；

2、openGauss 6.0.1升级到openGauss master成功，除带cm升级回滚失败外，其余场景升级失败或者升级未提交，可以成功回滚；

3、升级成功后，除行存压缩特性，其余特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0不带cm灰度升级到openGauss 6.0.1不带cm | 测试通过 |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0不带cm灰度升级到openGauss 6.0.1带cm | 测试通过 |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0带cm灰度升级到openGauss 6.0.1带cm | 测试通过 |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0不带cm灰度升级到openGauss 6.0.1不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0不带cm灰度升级到openGauss 6.0.1带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 3.0.6/openGauss 5.0.1/openGauss 5.0.3/openGauss 6.0.0带cm灰度升级到openGauss 6.0.1带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.1不带cm升级到openGauss master不带cm            | 测试通过 |
| openGauss 6.0.1不带cm升级到openGauss master带cm              | 测试通过 |
| openGauss 6.0.1带cm升级到openGauss master带cm                | 测试通过 |
| openGauss 6.0.1不带cm升级到openGauss master不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.1不带cm升级到openGauss master带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 6.0.1带cm升级到openGauss master带cm，回滚后，再升级提交 | 测试通过 |

#### 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 6.0.1数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 6.0.1数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/环境下，安装部署openGauss 6.0.1数据库 | 测试通过 |

# 问题单统计

openGauss 6.0.1版本共发现问题13个，无效问题0个，该版本规划解决的问题均已合入，验收通过：

| 版本名称             | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| -------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 6.0.1 B001 | 2025-1-4     | 2025-1-17    | 13         | 0          |
| openGauss 6.0.1 B002 | 2025-1-18    | 2025-1-26    | 0          | 0          |

本次测试共2轮，2025.1.4正式进入openGauss 6.0.1测试，B001合入问题单验收、全量测试，暴露出问题多。B002对修复问题单点验收，无特性测试，未发现问题。

# 附件

## 附件1：遗留问题列表

无

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@wan005](https://gitee.com/wan005)
+ [@sungang14](https://gitee.com/sungang14)
+ [@peilinqian](https://gitee.com/peilinqian)
+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)
+ [@li-xin12345](https://gitee.com/@li-xin12345)
+ [@zhanghuan96](https://gitee.com/@zhanghuan96)
