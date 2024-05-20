![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改章节 | 修改描述 | 作者   |
| --------- | -------- | -------- | -------- | ------ |
| 2024.5.18 | 1.0      | 初稿撰写 |          | wan005 |

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

**Keywords 关键词**：openGauss 5.0.2

**Abstract 摘要**：主要是描述了openGauss 5.0.2版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                   | 中文解释       |
| ------ | -------------------------- | -------------- |
| SQL    | Structured Query Language  | 结构化查询语言 |
| DML    | Data Manipulation Language | 数据操纵语言   |
| DDL    | Data Definition Language   | 数据定义语言   |
| DCL    | Data Control Language      | 数据控制语言   |
| CM     | Cluster Management         | 集群管理工具   |

***


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 5.0.2版本回合部分兼容性特性，并对若干关键缺陷进行了修改。本文主要描述了openGauss 5.0.2版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，oopenGauss 5.0.2版本整体质量良好。

# 测试版本说明

| 版本名称             | 测试起始时间 | 测试结束时间 |
| -------------------- | ------------ | ------------ |
| openGauss 5.0.2 B001 | 2024-4-17    | 2024-4-20    |
| openGauss 5.0.2 B002 | 2024-4-21    | 2024-4-28    |
| openGauss 5.0.2 B003 | 2024-4-29    | 2024-5-13    |
| openGauss 5.0.2 B004 | 2024-5-14    | 2024-5-15    |

测试环境：

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：HDD 1T <br />OS：CentOS Linux 7.6.1810（Core）<br />文件系统：xfs<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 20.03 (LTS)<br />文件系统：ext4<br /> |      |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：SSD 100G <br />OS：openEuler release 22.03 (LTS)<br />文件系统：ext4<br /> |      |
| 虚拟机   | CPU：Kunpeng-920 16核<br />内存：32GB<br />硬盘：HDD 200G <br />OS：Kylin Linux Advanced Server V10 (Sword)<br />文件系统：xfs<br /> |      |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| Kylin     | V10 (sword)      | Kylin V10 (sword)，aarch版本ISO<br />SHA256:e6fffdb9dec030920a33ad4c57b68cce8cd7d0496b4bd04a7b85f8cc5494cf41 |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 5.0.2版本是openGauss社区继续联合多方力量推出的版本，本次发布的特性列表、分工如下：

| 序号 | 特性                                                         | 测试主体 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | PinBuffer Cas优化                                            | QA       |
| 2    | probackup 性能优化                                           | QA       |
| 3    | MySQL兼容性-rand/random_bytes函数、ASCII/BINARY列属性、16进制输入 | QA       |
| 4    | B兼容性下字段模糊匹配支持走索引扫描                          | QA       |
| 5    | 关键字降级                                                   | QA       |
| 6    | 兼容M* select语句支持ignore index                            | QA       |
| 7    | @变量递归调用                                                | QA       |
| 8    | m* 兼容，视图支持sql security 语法                           | QA       |
| 9    | 兼容MySQL多字符集                                            | QA       |
| 10   | ATAN函数支持atan(y, x)语法                                   | QA       |
| 11   | AND两侧支持timestamp数据类型                                 | QA       |
| 12   | m* 兼容，视图支持sql security 语法                           | QA       |
| 13   | 支持单引号用作列的别名                                       | QA       |
| 14   | 支持在不带as，不带反引号情况下，关键字作为表别名与列别名     | QA       |
| 15   | grant需支持对函数做赋权操作                                  | QA       |
| 16   | 兼容MySQL的字符串+字符串用法 select 'a' 'b';                 | QA       |
| 17   | 导入导出、备份恢复工具支持MySQL兼容性                        | QA       |
| 18   | 【内核】新增MySQL协议hot_standby模式                         | QA       |
| 19   | 驱动支持批量插入时候处理returnning 语句                      | QA       |
| 20   | \d和\d+无法显示表字段的字符集信息                            | QA       |
| 21   | 【移动】（M*兼容）支持trigger建在指定schema中，并校验schema和table schema是否一致 | QA       |

openGauss 5.0.2版本整体测试按照release-manager团队的计划，版本测试规划采取4轮测试方式，，完成了2轮系统测试+2轮回归测试：

第1轮至第2轮完成所有回合的特性测试，同时对继承特性开展第1轮的全量测试，覆盖功能、可靠性、安全、性能、长稳、升级等测试。第3轮至第4轮验收回合版本的问题单，分别在第三轮、第四轮开展全量CI测试，同时覆盖可靠性、安全、性能、升级、长稳等测试，并对特性测试、专项测试给出结论。

openGauss 5.0.2版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本计划交付特性21个，实际交付21个，交付率100%，所有发布特性均验证通过。

openGauss 5.0.2版本共验收问题115个，均回归测试结果正常，版本整体质量良好。版本遗留4个问题，见附件1遗留问题列表。

# 版本详细测试结论

openGauss 5.0.2版本详细测试内容包括：

1、通过自动化看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、系统性能、系统可靠性6个维度进行openGauss继承特性测试，继承功能无丢失；

2、在M\*兼容性方面，测试B兼容性下字段模糊匹配支持走索引扫描、兼容M*多字符集等特性，M\*兼容性进一步提升；

3、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

4、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 特性测试结论

### 继承特性评价

| Domain           | Feature        | 质量评估                   | 备注                                                         |
| ---------------- | -------------- | -------------------------- | ------------------------------------------------------------ |
| 数据库服务       | SQL语法        | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句，不同特性组合下用户、权限的验证(含资源池化、兼容性)(表/视图/索引等基础对象，fdw、postgis、物化视图) (含资源池化、兼容性) |
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
|                  | 驱动兼容       | <font color=green>▮</font> | 测试jdbc/odbc/libpq/psycopg2等 mysql兼容(协议兼容、类型兼容)继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动 |
|                  | 生态兼容       | <font color=green>▮</font> | 测试ORM(mybatis)，连接池(druid)                              |
|                  | mysql兼容性    | <font color=green>▮</font> | 测试SQL语法（单双引号、反引号、关键字、类型、函数、操作符等）、通信协议 |
|                  | oracle兼容性   | /                          | /                                                            |
|                  | pg兼容性       | /                          | /                                                            |
|                  | 资源池化兼容性 | <font color=green>▮</font> | 测试资源池化特性，双集群极致RTO、网络复制双集群、refrom、主备实时一致性 |
| 系统性能         | 系统性能       | <font color=green>▮</font> | 测试2P/4P性能、主备、RTO(含兼容性、资源池化)                 |
| 系统可靠性       | 系统可靠性     | <font color=green>▮</font> | 故障注入测试，包含MOT、兼容性、资源池化等内存专项、长稳、    |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 问题单验收结论

对版本所有回合的问题验收

| 序号 | issue ID | 问题描述                                                     | 验收状态 |
| ---- | -------- | ------------------------------------------------------------ | -------- |
| 1    | I6KIBM   | 【测试类型：SQL功能】【测试版本：3.1.0】不支持跨schema移动表 | 验收通过 |
| 2    | I6PN4N   | 【海量】\h select帮助语法没有更新                            | 验收通过 |
| 3    | I6QWI9   | 使用COMMENT ON对存储过程进行注释后，gs_dump导出内容导入失败  | 验收通过 |
| 4    | I6S0LB   | MySQL风格存储过程/自定义函数使用gs_dump导出后，使用gs_restore导入失败 | 验收通过 |
| 5    | I6VSZ2   | 【测试类型：SQL功能】【测试版本：5.1.0】 问题描述：兼容b库下，number类型导出前和导入后类型不一样导致数据不一致 | 验收通过 |
| 6    | I6WOW7   | 【测试类型：工具功能】【测试版本：5.1.0】 问题描述:列名使用反引号含空格，gs_dump导出报错 | 验收通过 |
| 7    | I76NF2   | 【移动】【测试类型：SQL功能】【测试版本：5.1.0】 兼容M特性在指定模式下创建、删除触发器。利用if exists删除表上不存在的触发器会报错 | 验收通过 |
| 8    | I7I7LK   | 【兼容MYSQL字符序】分区键字段使用MYSQL字符序，分区剪枝、路由结果集不对 | 验收通过 |
| 9    | I7IW4X   | datcompatibility='B' 创建分区表，使用gs_dump导出报错         | 验收通过 |
| 10   | I7J61X   | proc_outparam_override打开后，函数参数中带有多个refcursor类型的out参数，call方式调用函数宕机 | 验收通过 |
| 11   | I7JB6R   | 【测试类型：SQL功能】【测试版本：5.1.0】 【自动化】regexp_substr，reverse，substrb和substring_inne函数入参为中文时，执行结果不正确 | 验收通过 |
| 12   | I7L155   | 使用interval强制指定时间间隔单位失效                         | 验收通过 |
| 13   | I7PIAI   | 【B兼容模式】【测试版本：5.1.0】B兼容模式CTAS+审计日志场景报错 | 验收通过 |
| 14   | I7PJP7   | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：支持返回值为字符串、binary的函数支持字符序】limit 后带字符序binary 未报错 也未生效 | 验收通过 |
| 15   | I7PL63   | 【测试类型：SQL功能】【测试版本：5.1.0】select @@查询bool类型GUC参数值错误 | 验收通过 |
| 16   | I7PTWT   | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：支持返回值为字符串、binary的函数支持字符序】使用pbe 带字符序查询的语句 和直接查询结果 不一致 | 验收通过 |
| 17   | I7RDH7   | PostgresInitializer::InitExtensionVariable 没有对 file_list 上锁 | 验收通过 |
| 18   | I7VCZI   | 【测试类型：SQL功能】【测试版本：5.1.0】资源池化环境 创建存储过程正常，调用存储过程报错 | 验收通过 |
| 19   | I810S5   | MySQL兼容模式下同时使用ON UPDATE CURRENT_TIMESTAMP语法和触发器时，执行update会报错，偶发性宕机 | 验收通过 |
| 20   | I82STY   | catchup2normal_wait_time设置为0，TPS降低【M】                | 验收通过 |
| 21   | I82ULX   | 裸主备，进行tpcc压测过程中报ERROR:seg_read blocknum exceeds segment size错误 | 验收通过 |
| 22   | I83D1P   | 【B模式】创建视图支持sql security语法-存储过程、函数中创建，修改视图——权限问题 | 验收通过 |
| 23   | I83QV0   | 【测试类型：SQL功能】【测试版本：5.1.0】右值引用场景存在内存访问越界问题 | 验收通过 |
| 24   | I83S8E   | 【测试类型：SQL功能】【测试版本：5.1.0】多表更新+join场景出现core | 验收通过 |
| 25   | I83SYR   | 【测试类型：SQL功能】【测试版本：5.1.0】prepare+自定义变量出现core | 验收通过 |
| 26   | I84A4S   | 【测试类型：升级】【测试版本：5.1.1】轻量版3.1.0升级到5.1.0版本报错，Exec sql on postgres failed.Error: upgrade post sql failed | 验收通过 |
| 27   | I84URC   | jdbc驱动中，对SQL语句中的"/"除法运算符处理有问题             | 验收通过 |
| 28   | I85X19   | 启动容器设置GS_PORT为非5432时，容器启动失败                  | 验收通过 |
| 29   | I864MZ   | 【测试类型：SQL功能】【测试版本：5.1.1】insert ... select ... from ... where ..场景，列引用错误导致的core问题 | 验收通过 |
| 30   | I869P7   | B兼容性数据库逻辑解码有误                                    | 验收通过 |
| 31   | I86YJY   | gs_dump与dolphin.sql_mode配置冲突                            | 验收通过 |
| 32   | I88WP9   | 【mysql兼容性】INSERT…ON DUPLICATE KEY UPDATE语法向表插入数据有误 | 验收通过 |
| 33   | I898UO   | 【测试类型：SQL功能】【测试版本：5.1.1】 问题描述：dayofmonth/dayofyear/quarter等函数入参是time类型，返回结果同M侧不一样 | 验收通过 |
| 34   | I89AMO   | 通过copy命令导入set类型数据default''报错出现违背非空约束     | 验收通过 |
| 35   | I89STU   | 【测试类型：SQL功能】【测试版本：5.1.1】release版本下，连续执行create trigger，数据库进程直接退出，无core产生 | 验收通过 |
| 36   | I8A8DK   | 分区表包含全局索引时使用BitmapHeapscan报错                   | 验收通过 |
| 37   | I8AD19   | 【测试类型：SQL功能】【测试版本：5.1.1】一主四备ANY1 ANY1同步备配置模式下，当后面整个AZ2 6004 6005的实例挂掉，不管是否开启最大可用模式，都会阻塞主机写业务【Y】 | 验收通过 |
| 38   | I8B47R   | memcheck版本forkname_to_number传入非预期参数时触发FailedAsserion | 验收通过 |
| 39   | I8B89Q   | UTF8Encoding存在缺陷问题                                     | 验收通过 |
| 40   | I8E7EG   | autovacuum触发比较频繁的情况下，toast表数据无法清理，数据表空间一直涨【M】 | 验收通过 |
| 41   | I8EK76   | alter table convert to修改表的字符序从utf8_bin到utf8_general_ci，实际修改表字符序成功，修改表列字符集失败，大小写仍敏感 | 验收通过 |
| 42   | I8EVYQ   | 二级分区表分区数3000个，drop表时需要四五十秒                 | 验收通过 |
| 43   | I8G677   | 在/openGauss-server/src/gausskernel/storage/access/common/heaptuple.cpp:765文件处出现段错误 | 验收通过 |
| 44   | I8GN47   | 存储过程中存在select into语句时，为单列时，创建失败          | 验收通过 |
| 45   | I8GPYZ   | 在/openGauss-server/src/common/backend/utils/adt/varlena.cpp:711文件处出现段错误 | 验收通过 |
| 46   | I8GQHY   | 在/openGauss-server/src/common/backend/parser/gram.y:31745文件处出现段错误 | 验收通过 |
| 47   | I8HANI   | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：cas性能优化】跑tpcc主机读写备机读，备机读业务12小时+报错ERROR: invalid memory alloc | 验收通过 |
| 48   | I8I5NS   | MySql兼容性 upsert回归用例宕机                               | 验收通过 |
| 49   | I8J17C   | 【测试类型：SQL引擎】【测试版本：5.0.0】openGauss带子查询时，性能劣于M*[zyzx] | 验收通过 |
| 50   | I8KN8M   | gs_dump导出库的时候，创表携带using语句中出现乱码。           | 验收通过 |
| 51   | I8L5ZO   | 【测试类型：SQL功能】【测试版本：5.1.1】 关闭ansi_quotes后，ctas语句包含convert报错 | 验收通过 |
| 52   | I8M1HO   | gs_dump对表进行备份，\i恢复时报错                            | 验收通过 |
| 53   | I8MBED   | 【测试类型：功能测试】【测试版本：5.1.1】社区版本，MYSQL兼容性下，执行with语句 core | 验收通过 |
| 54   | I8MJNF   | 【测试类型：功能测试】【测试版本：5.1.1】社区版本，MYSQL兼容性下，执行updata多表更新 core | 验收通过 |
| 55   | I8NG0R   | 非pg_catalog模式的表的表名与pg_catalog中已有表表名相同时，gs_dump导入导出有问题 | 验收通过 |
| 56   | I8NR8P   | 【测试类型：SQL功能】【测试版本：5.1.1】 执行SQL语句内存不足，导致数据库core | 验收通过 |
| 57   | I8NTNE   | 设置xloginsert_locks不是numa节点的倍数，数据库启动core       | 验收通过 |
| 58   | I8OFSX   | 创建带point类型表，使用point列创建spgist索引，插入超过10w条记录时失败 | 验收通过 |
| 59   | I8OSGC   | 【测试类型：SQL功能】【测试版本：5.0.0】 问题描述:gsql不同命令对MySQL语句兼容性不同 | 验收通过 |
| 60   | I8PNTP   | makeEmptyPGconn函数出现内存泄漏                              | 验收通过 |
| 61   | I8PZ2T   | 【测试类型：SQL功能】【测试版本：5.1.1】 兼容B库调用存储过程插入数据与mysql结果不一致 | 验收通过 |
| 62   | I8Q1FZ   | 【测试类型：故障注入】【测试版本：3.0.3】 故障注入服务器重启后openGauss启动异常 | 验收通过 |
| 63   | I8QY8C   | gs_dump导出rename过的schema下的表的auto_increment失败        | 验收通过 |
| 64   | I8RUMN   | 【测试类型：功能测试】【测试版本：5.1.1】社区版本，MYSQL兼容性下，执行insert 语句 core | 验收通过 |
| 65   | I8SS5J   | og 3.0.0 range interval自动分区存在多个相同时间范围分区子表[XY] | 验收通过 |
| 66   | I8SVA6   | gsql执行命令报错和pg9.2.4，通过echo $?捕获的命令状态不一致   | 验收通过 |
| 67   | I8SZS3   | 修改列位置和数据类型出现了core                               | 验收通过 |
| 68   | I8TABG   | 【测试类型：SQL功能】【测试版本：6.0.0】执行plan hint后报错降级warning | 验收通过 |
| 69   | I8TJ3I   | 基于多表的视图 做update view where current of，数据库断开连接 | 验收通过 |
| 70   | I8U01Q   | gs_dump备份sql_mode不含有ansi_quotes参数的B模式数据库会报错  | 验收通过 |
| 71   | I8URAR   | 【测试类型：SQL功能】【测试版本：6.0.0】【自动化】statement_history无法记录脚本执行的慢sql | 验收通过 |
| 72   | I8V36N   | 【测试类型：SQL功能】【测试版本：master】 视图创建支持自定义用户变量，与mysql行为不一致 | 验收通过 |
| 73   | I8WAUD   | 【测试类型：SQL功能】【测试版本：6.0.0】JDBC使用getObject()获取YEAR类型与mysql不一致 | 验收通过 |
| 74   | I8WBKN   | 【测试类型：SQL功能】【测试版本：6.0.0】JDBC使用getDate()获取YEAR类型的值报错ERROR:Bad value for type timestamp/date/time | 验收通过 |
| 75   | I8WC0R   | 存储过程调用default值传给了其他入参                          | 验收通过 |
| 76   | I8WGXU   | 【测试类型：SQL功能】【测试版本：6.0.0】JDBC调用getObject()/getColumnTypeName()/getBytes()接口获取cast转换为binary的类型/值与mysql不同 | 验收通过 |
| 77   | I8WYXH   | 【测试类型：接口功能】【测试版本：6.0.0】 JDBC连接兼容B库使用getObject()方法获取blob类型报错 | 验收通过 |
| 78   | I8XGOC   | 线程池模式下使用postgres_fdw宕机                             | 验收通过 |
| 79   | I8XITB   | 数据库日志不断刷：too many history info in the memory，current history tecord is 79438【M】 | 验收通过 |
| 80   | I8XXN6   | 【动态分区裁剪】数据类型varchar在in（array[]）时不走动态分区裁剪 | 验收通过 |
| 81   | I8YEFP   | opengauss B模式数据库参数dolphin.b_compatibility_mode设置为on后在匿名块中通过information_schema.columns表查询列长度会报错 | 验收通过 |
| 82   | I8ZOIM   | 【测试类型：SQL功能】【测试版本：6.0.0】严格模式失效         | 验收通过 |
| 83   | I904J1   | 逻辑复制订阅端创建增量物化视图，视图刷新同步过来的数据失败   | 验收通过 |
| 84   | I90JHX   | 【测试类型：并发/压力长稳】【测试版本：5.0.1】发现一处段错误导致的异常退出 | 验收通过 |
| 85   | I90QQL   | 多表更新功能，部分包含同名别名和列名语法场景下，行为与mysql存在差异 | 验收通过 |
| 86   | I91HUU   | 兼容b库中，创建自定义函数时core                              | 验收通过 |
| 87   | I9228X   | 分区表达式分区表，插入计算结果为null值的数值时，插入分区错误 | 验收通过 |
| 88   | I922UN   | 【测试类型：SQL功能】【测试版本：6.0.0】创建event导致core    | 验收通过 |
| 89   | I927PM   | 【测试类型：接口功能】【测试版本：6.0.0】 兼容B库，jdbc接口通过setBinaryStream、setBytes方法插入blob类型报错，“ERROR: invalid hexadecimal digit: ""” | 验收通过 |
| 90   | I92BC3   | 【备份恢复】恢复参数-C 和-t一起使用时无法恢复新库            | 验收通过 |
| 91   | I96QR7   | openGauss5.0.0 memcheck版本 异常崩溃                         | 验收通过 |
| 92   | I9C0LY   | 分区表删除分区会使全局索引失效                               | 验收通过 |
| 93   | I9C345   | B模式下添加字段后执行SQL或查看执行计划操作导致数据库重启[zyzx] | 验收通过 |
| 94   | I9DJZ5   | 字符串截断后utf8字符序like匹配hang住                         | 验收通过 |
| 95   | I9FH2V   | 【测试类型：SQL功能】【测试版本：6.0.0】 使用表达式作为分区键，更新分区键列时数据库宕机 | 验收通过 |
| 96   | I9FMKZ   | 5.0.2补丁版本插件升级至1.3                                   | 验收通过 |
| 97   | I9HCCQ   | 安装opengauss数据库后，无pg_archivecleanup命令               | 验收通过 |
| 98   | I9I2GO   | 【测试类型：工具功能】【测试版本：5.0.2】5.0.2jdbc包解压出来为5.0.1 | 验收通过 |
| 99   | I9IM10   | 【测试类型：工具功能】【测试版本：5.0.2】【升级】3.0.5创建B库，灰度升级到5.0.2，不带CM，报错ERROR: operator ~~ already exists | 验收通过 |
| 100  | I9IMB7   | 【测试类型：工具功能】【测试版本：5.0.2】【升级】3.0.5创建B库，就地升级到5.0.2，不带CM，报错FATAL: cannot execute CREATE EXTENSION in a read-only transaction | 验收通过 |
| 101  | I9IRH3   | B模式，字符串按照数字顺序排序报错，order by(id+‘’）执行报错  | 验收通过 |
| 102  | I9IYDQ   | 【测试类型：工具功能】【测试版本：5.0.2】【升级】3.0.5/5.0.0/5.0.1带cm灰度升级到5.0.2带cm，升级过程中启库失败 | 验收通过 |
| 103  | I9JTVM   | 【测试类型：SQL功能】【测试版本：5.0.0】 问题描述:modify仅修改列注释导致字符乱码且会引发数据库宕机 | 验收通过 |
| 104  | I9JTZA   | 【测试类型：工具功能】【测试版本：5.0.2】【升级】5.0.0/5.0.1~5.0.2 灰度升级_回滚报错ERROR: cannot cast type bigint to year | 验收通过 |
| 105  | I9JZJ6   | 【测试类型：SQL功能】【测试版本：5.0.2】3.0.5/5.0.0/5.0.1、5.0.2带cm，gs_om -t status --all结果主节点sender_sent_location等为0/0 | 验收通过 |
| 106  | I9L0Y1   | datakit安装5.1.0版本失败                                     | 验收通过 |
| 107  | I9IZ7A   | om执行预安装提示The valid return item number[0] does not match with host number[2]. The return result: | 验收通过 |
| 108  | I9GV2J   | gs_restore恢复带主键的表时报错执行删除主键失败，因为表不存在 | 验收通过 |
| 109  | I9F70M   | 使用concat函数效率下降问题[zyzx]                             | 验收通过 |
| 110  | I9BUSY   | 使用gs_dropnode命令后成功删除备节点，但集群整体无法启动      | 验收通过 |
| 111  | I9JWCD   | 5.0.0物理复制槽为f，xlog不回收问题                           | 验收通过 |
| 112  | I95H80   | springboot + Mybatis架构，blob字符类型读写报错。             | 验收通过 |

## 专项测试结论

### 可靠性测试

openGauss 5.0.2 版本可靠性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及2个长时间负载测试。

1、故障注入类测试：共计测试3个版本，在x86+Centos，x86openEuler，Kylin环境下共计执行715个用例，测试用例累计执行率100%，测试发现2个问题（优化类问题单，不影响整体功能），整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：共计测试4个版本，共执行1个用例，测试用例累计执行率100%，未发现问题，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | TPCC连跑，insert/update/delete事务，500仓+100并发，虚拟机测试7*24H连跑 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |

### 性能测试

对关键性能指标进行摸底和测试，主要覆盖了传统主备的TPC-C测试及RTO测试，性能多轮测试稳定。

| **指标大项** | **指标小项**                           | **指标值** | **说明**          | 测试结论                                    |
| ------------ | -------------------------------------- | ---------- | ----------------- | ------------------------------------------- |
| TPCC         | 2P（Taishan 200 2280 7260）单节点 1H   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为**158.6**W+   |
|              | 2P（Taishan 200 2280 7260）单节点 8H   | 145万      | 此即为release基线 | 单节点8H在极限场景配置下tpmC为**154.5**W+   |
|              | 2P（Taishan 200 2280 7260）一主一备 1H | 120万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为**122.8**W+ |
|              | 2P（Taishan 200 2280 7260）一主一备 8H | 110万      | 此即为release基线 | 一主一备8H在极限场景配置下tpmC为**110.0**W+ |
|              | 4P（Taishan 200 2480 7260）单节点 1H   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为**240.0**W+     |
| RTO高可用    | 一主两同步备failover                   | 10s        | 此即为release基线 | **6**s                                      |

### 兼容性测试

#### 升级兼容

针对openGauss 5.0.2版本，规划的升级路径如下表所示。共进行4轮测试，发现issue测试整体情况：

1. openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1升级到openGauss 5.0.2成功，升级失败或者升级未提交，可以成功回滚；
2. openGauss 5.0.2升级到openGauss 6.0.0成功，升级失败或者升级未提交，可以成功回滚；
3. 升级成功后，除行存压缩特性，其余特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.0.2不带cm | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.0.2带cm | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1带cm灰度升级到openGauss 5.0.2带cm | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.0.2不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.0.2带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1带cm灰度升级到openGauss 5.0.2带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 5.0.2不带cm升级到openGauss 6.0.0不带cm             | 测试通过 |
| openGauss 5.0.2不带cm升级到openGauss 6.0.0带cm               | 测试通过 |
| openGauss 5.0.2带cm升级到openGauss 6.0.0带cm                 | 测试通过 |
| openGauss 5.0.2不带cm升级到openGauss 6.0.0不带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 5.0.2不带cm升级到openGauss 6.0.0带cm，回滚后，再升级提交 | 测试通过 |
| openGauss 5.0.2带cm升级到openGauss 6.0.0带cm，回滚后，再升级提交 | 测试通过 |

#### 硬件兼容

| Domain     | 测试活动                                                     | 测试结论 |
| ---------- | ------------------------------------------------------------ | -------- |
| 服务器兼容 | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 5.0.2数据库 | 测试通过 |

#### 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 5.0.2数据库 | 测试通过 |

# 问题单统计

openGauss 5.0.2版本共发现问题22个，有效问题17个，无效问题5个。修复问题回归测试结果正常，版本整体质量良好。遗留4个问题详细分布见下表: 

| 版本名称             | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| -------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 5.0.2 B001 | 2024-4-17    | 2024-4-20    | 6          | 1          |
| openGauss 5.0.2 B002 | 2024-4-21    | 2024-4-28    | 10         | 3          |
| openGauss 5.0.2 B003 | 2024-4-29    | 2024-5-13    | 0          | 1          |
| openGauss 5.0.2 B004 | 2024-5-14    | 2024-5-15    | 1          | 0          |

本次测试共4轮，从B001持续到B004。2024.4.17正式进入openGauss 5.0.2测试，B001至B002合入特性测试，问题单呈上升趋势，达到峰值。B003 特性测试结束，重点验收问题单，开展第3轮、第4轮全量CI测试、可靠性及升级等专项测试，因此B003至B004新增问题单较少。

# 附件

## 附件1：遗留问题列表

| 序号 | issue号                                                      | 问题简述                                                     | 分类   | 问题级别 | 问题分析与影响                                               | 规避措施                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | [I90ZQ1](https://e.gitee.com/opengaussorg/issues/table?issue=I90ZQ1) | 【测试类型：工具功能】【测试版本：6.0.0】【可靠性】 进行扩容时，扩容的备节点注入ioWait高故障，进行动态配置刷新步骤超时导致扩容操作失败。 | 可靠性 | 次要     | 扩容操作，在执行生成配置文件时候存在默认超时时间90s，主备节点存在故障情况会导致超时。 | 扩容时保障主备环境可用资源充足                               |
| 2    | [I9BGM3](https://e.gitee.com/opengaussorg/issues/table?issue=I9BGM3) | 【测试类型：故障注入】【测试版本：6.0.0】【可靠性】 进行两个备节点扩容，其中先扩容的节点因为故障扩容失败，清除故障后，再次扩容失败的节点，扩容后，两个备节点的application_name都是dn_6002，共用一个物理复制槽。 | 可靠性 | 次要     | 列表扩容多个节点列表，如果前面扩容的节点失败后，再次扩容失败的节点，会导致application_name重复 | 扩容失败的节点再次扩容后，需要检查application_name是否与其他节点重复，并手工修改为不同值 |
| 3    | [I9JZ8M](https://e.gitee.com/opengaussorg/issues/table?issue=I9JZ8M) | 【测试类型：SQL功能】【测试版本：5.0.2】【升级】3.0.5行存压缩方案和5.0.2行存压缩方案不一致，导致升级后行存压缩表找不到文件 | 升级   | 次要     | 3.0.5 到 5.0.0以上版本均有问题，为方案性问题，待评估， 502版本不做合入 | 无                                                           |
| 4    | [I9KUT0](https://e.gitee.com/opengaussorg/issues/table?issue=I9KUT0) | 【测试类型：工具功能】【测试版本：6.0.0】【升级】3.0.5升级到6.0.0后core | 升级   | 次要     | 该问题在3.0.5-5.0.2升级过程中偶现，偶现概率低于1/10          | 无                                                           |

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@justbk](https://gitee.com/justbk)
+ [@wan005](https://gitee.com/wan005)
+ [@peilinqian](https://gitee.com/peilinqian)
+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)
+ [@li-xin12345](https://gitee.com/@li-xin12345)
