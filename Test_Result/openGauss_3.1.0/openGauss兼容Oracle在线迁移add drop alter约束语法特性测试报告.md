

![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者       |
| ---------- | -------- | -------------------- | ---------- |
| 2022-07-15 | 1.0      | 特性测试报告初稿完成 | lizhuo9527 |
| 2022-07-29 | 1.1      | 修改报告描述         | lizhuo9527 |
| 2022-08-02 | 1.2      | 修改报告描述         | lizhuo9527 |

关键词： 

debezium；kafka；onlineMigration；Oracle；openGauss；在线迁移add/drop/alter约束语法

摘要：

在线迁移工具由Oracle（生产端），debezium，kafka，onlineMigration及openGauss（消费端）这几部分组成。本次测试针对迁移工具支持DDL语句的add/drop/alter约束语法，本文档主要介绍Oracle-openGauss在线迁移工具支持DDL语句add/drop/alter约束语法特性测试结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容A库情形下，依次开启kafka、kafka_connector、onlineMigration工具，由debezium监控Oracle中DDL及DML语句的变化并把记录捕捉到kafak的topic中，然后通过onlineMigration消费topic中的记录并迁移到openGauss数据库中，这样就完成从Oracle到openGauss的在线迁移。

本次特性支持DDL语句的add/drop/alter约束语法，即在create table迁移完成后，通过alter table add/drop constraint语句执行添加、删除primary key、foreign key、unique、check、not null、default约束操作后，可以顺利的由Oracle端迁移至openGauss端，并使得约束在openGauss端生效，且不影响insert、update、delete语句的迁移。

# 2     特性测试信息

特性测试的版本信息及测试时间、测试轮次

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 02c14696 | 2022-06-20   | 2022-07-29   |
| Oracle version 19.3.0.0.0      | 2022-06-20   | 2022-07-29   |
| kafka v2.13-2.8.1              | 2022-06-20   | 2022-07-29   |
| debezium v1.8.1                | 2022-06-20   | 2022-07-29   |
| onlineMigration v1.0           | 2022-06-20   | 2022-07-29   |

特性测试的硬件环境信息

| 硬件型号       | 硬件配置信息                                                 | 备注 |
| -------------- | ------------------------------------------------------------ | ---- |
| OpenStack Nova | CPU:  Intel(R) Xeon(R) Platinum 8378A CPU @ 3.00GHz<br>内存：32GB<br>硬盘：1000G<br>OS：Linux version 3.10.0-1160.53.1.el7.x86_64 (Red Hat 4.8.5-44)<br>文件系统：XFS |      |

# 3     测试结论概述

## 3.1   测试整体结论

Oracle在线迁移支持DDL约束语句alter table add/drop/alter约束语法共计执行134条用例，主要覆盖了功能测试及资料测试。功能测试主要包括以下7个方面：

 	1. 各种约束类型，如单一、复合的主外键、unique、check约束，外键的on delete cascade、on delete set null及no action参数，default约束的不同数据类型，check约束的表达式等；
 	2. 同一张表的同一字段及不同字段add/drop/alter多个约束；
 	3. openGauss端存在同名表（表结构相同和不同时）的情况；
 	4. 在openGauss端手动修改表结构（增删改字段）的情况；
 	5. openGauss端表中手动添加违反约束的数据的情况；
 	6. 各对象名大小写混合使用情况；
 	7. DML语句的支持情况验证。

资料测试主要验证资料的描述的准确性以及示例的正确性。

发现问题共计21条，其中19条问题已解决并回归通过，1条非问题取消，1条经CCB评审后挂起。该特性遗留风险点1个，即在线迁移过程中，Oracle的对象名（表名、列名、约束名）在未加双引号且大小写混合使用的情形下，onlineMigration工具因对象名的大小写识别问题导致添加约束的DDL语句迁移失败。因此在该工具使用中，需要将SQL语句中各对象名两端添加双引号，以此规避该问题，保证迁移结果准确无误。其余项无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 单一、复合的主外键、unique、check约束，外键的on delete cascade、on delete set null<br>及no action参数，default约束的不同数据类型，check约束的表达式验证，通过 |
| 功能测试 | 同一张表的同一字段及不同字段add/drop/alter多个约束的情形验证，通过 |
| 功能测试 | openGauss端存在同名表（表结构相同）的情况验证，通过          |
| 功能测试 | openGauss端存在同名表（表结构不同）的情况验证，通过          |
| 功能测试 | 在openGauss端手动修改表结构（如增删改字段）的情形验证，通过  |
| 功能测试 | 在openGauss端表中手动添加违反约束的数据的情况验证，通过      |
| 功能测试 | 已包含约束的表add/drop/alter约束情形验证，通过               |
| 功能测试 | 对象名大小写混合使用验证，通过                               |
| 功能测试 | DML语句的验证，通过                                          |
| 资料测试 | 资料描述准确，示例正确无误，整体质量良好                     |

## 3.2   约束说明

1. openGauss为兼容A库的情形，Oracle版本为Oracle version 19.3.0.0.0；
2. Oracle及openGauss两端数据库需有同名schema，如“C##ROMA_LOGMINER”；
3. 依次开启zookeeper、kafka、kafka_connector、onlineMigration工具并保证正常运行；
4. 启动onlineMigration时，参数--smartConversionOfObjectNames表示大小写转换开关，启动命令包含该参数则大小写转换开关开启，否则开关关闭。目前，约束（create table包含约束及add/drop/alter约束）暂不支持大小写转换开关；
5. 各对象名（表名、字段名、约束名等）两端需加上双引号，保证数据库迁移结果的准确；
6. 关闭迁移工具后，如需清空历史迁移记录并重新开始迁移，则需更改kafka_connector连接名或清空/tmp目录下的kafka_logs及zookeeper_logs日志，重新开启迁移工具；如需断点续传，则需要在onlineMigration工具的启动命令中，不使用参数--from-begining。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                     | 问题级别 | 问题影响和规避措施                                           | 当前状态 |
| ------------------------------------------------------------ | -------------------------------------------- | -------- | ------------------------------------------------------------ | -------- |
| [I5G8LB](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5G8LB?from=project-issue) | 在线迁移中，添加联合外键时列名大小写识别有误 | 次要     | 问题影响：create table时column name大小写混合使用且不加引号，迁移成功后使用alter table语句添加外键约束时，onlineMigration将column name识别为全部小写，外键约束添加失败<br>规避措施：将所有对象名（包括但不限于表名、字段名、约束名）两端加双引号，可以保证迁移结果的准确性 | 挂起     |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 21       | 0    | 0    | 21   | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3  问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I5DARU]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DARU?from=project-issue) ) | 次要 | 在线迁移因大小写识别问题，drop unique约束迁移失败            | 已验收 |
| 2    | [I5AW85](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5AW85?from=project-issue) | 次要 | 建表添加约束后，insert语句迁移失败                           | 已验收 |
| 3    | [I5AD5I](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5AD5I?from=project-issue) | 次要 | 建表时外键约束不支持外键参数迁移                             | 已验收 |
| 4    | [I5ACQ6](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5ACQ6?from=project-issue) | 次要 | 一张表不同字段添加表级主外键，drop外键online报错             | 已验收 |
| 5    | [I59YT9](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59YT9?from=project-issue) | 次要 | 在线迁移对于check约束的not in关键字识别有误                  | 已验收 |
| 6    | [I59W4P](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59W4P?from=project-issue) | 次要 | 已存在主键约束的表迁移完成后无法删除主键                     | 已验收 |
| 7    | [I59O8E](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59O8E?from=project-issue) | 次要 | 在线迁移当主键与检查约束在同一字段时无法drop检查约束         | 已验收 |
| 8    | [I59NWI](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59NWI?from=project-issue) | 次要 | 在线迁移，同一字段同时存在主外键，先drop主键后drop外键，online报错，openGauss侧主键删除，外键未删除 | 已验收 |
| 9    | [I59MGO](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59MGO?from=project-issue) | 次要 | 在线迁移drop非空约束后，online报错，openGauss侧约束未删除    | 已验收 |
| 10   | [I59GCN](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59GCN?from=project-issue) | 次要 | 在线迁移drop联合主键后，online报错，openGauss侧主键未删除    | 已验收 |
| 11   | [I59FM1](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I59FM1?from=project-issue) | 次要 | 在线迁移drop主键后，openGauss侧无法插入null                  | 已验收 |
| 12   | [I591GM](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I591GM?from=project-issue) | 次要 | 在线迁移已包含约束的表，drop/add迁移结果与期望结果不符       | 已验收 |
| 13   | [I58WWS](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I58WWS?from=project-issue) | 次要 | 在线迁移时不包含任何约束的表也无法迁移                       | 已验收 |
| 14   | [I5957A](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5957A?from=project-issue) | 次要 | 在线迁移时使用modify语句添加外键约束online报错               | 已验收 |
| 15   | [I5HUIA](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5HUIA?from=project-issue) | 次要 | 在线迁移中，表名未加引号，insert语句迁移失败                 | 已验收 |
| 16   | [I5H8FQ](https://gitee.com/opengauss/debezium/issues/I5H8FQ?from=project-issue) | 次要 | 在线迁移表中包含各种约束后insert迁移失败                     | 已验收 |
| 17   | [I5DKZ1](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DKZ1?from=project-issue) | 次要 | 在线迁移表中包含各种约束后insert迁移失败                     | 已验收 |
| 18   | [I5GXCT](https://gitee.com/opengauss/debezium/issues/I5GXCT?from=project-issue) | 次要 | 在线迁移中表名不添加引号则online不能识别，添加约束失败       | 已验收 |
| 19   | [I5HUDR](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5HUDR?from=project-issue) | 次要 | 在线迁移中，建两张同名表，一张表名加引号一张表名不加引号，第二张表无法创建 | 已验收 |
| 20   | [I5G8NE](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5G8NE?from=project-issue) | 次要 | 在线迁移中表名不添加引号则online不能识别，添加约束失败       | 已取消 |
| 21   | [I5G8LB](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5G8LB?from=project-issue) | 次要 | 在线迁移中，添加联合外键时列名大小写识别有误                 | 挂起   |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启两端数据库并设置同名schema<br />2. 开启kafka、onlineMigration等工具 | 共执行134条用例<br />共发现21个bug，19条现已修复且验收通过<br />1条非问题取消，1条经CCB评审挂起 |

#### 4.1.1.1 约束类型测试

| 测试步骤：                                                   | 测试结果                                                |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| 1. 单一、复合的主外键、unique、check约束<br>2. 外键的on delete cascade、set null及no action参数<br>3. default约束的不同数据类型<br>4. check约束的表达式 | 共执行23条用例<br />共发现9个bug，9条现已修复且验收通过 |

#### 4.1.1.2 同一张表的同一字段及不同字段add/drop/alter多个约束测试

| 测试步骤：                                                   | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1.  验证同一字段add/drop多个约束<br >2.  验证同一张表的不同字段add drop多个相同约束 | 共执行20条用例，<br />共发现3个bug，3条现已修复且验收通过 |

#### 4.1.1.3 openGauss端存在同名表（手动建表）add/drop/alter约束测试

| 测试步骤：                                                   | 测试结果                                      |
| ------------------------------------------------------------ | --------------------------------------------- |
| 1. openGauss端手动创建同名表<br>2. 表结构相同、表结构不同<br>3. add/drop/alter约束 | 执行20条用例，未发现bug<br />执行结果符合预期 |

#### 4.1.1.4 在openGauss端手动修改表结构（增删改字段）的情形测试

| 测试步骤：                                            | 测试结果                                      |
| ----------------------------------------------------- | --------------------------------------------- |
| 1. 增删改字段名、数据类型 <br />2. 手动添加、删除约束 | 执行30条用例，未发现bug<br />执行结果符合预期 |

#### 4.1.1.5 openGauss端表中手动添加违反约束的数据测试

| 测试步骤：                                             | 测试结果                                        |
| ------------------------------------------------------ | ----------------------------------------------- |
| 1. 插入违反约束的数据<br>2. 在线迁移add/drop/alter约束 | 共执行11条用例，未发现bug<br />执行结果符合预期 |

#### 4.1.1.6 已包含约束的表add/drop/alter约束情形测试

| 测试步骤：                                            | 测试结果                                                |
| ----------------------------------------------------- | ------------------------------------------------------- |
| 1. openGauss端建表后手动添加约束<br />2. 执行迁移操作 | 共执行10条用例<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.7 对象名大小写混合使用测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 表名未加引号且大小写混用 <br />2. 列名、约束名未加引号且大小写混用 | 共执行10条用例<br />共发现4个bug，2条现已修复且验收通过<br />1条经CCB评审后挂起<br />1条非问题取消 |

#### 4.1.1.8 DML迁移测试

| 测试步骤：                             | 测试结果                                                |
| -------------------------------------- | ------------------------------------------------------- |
| 1. 建表并添加约束 <br />2. DML语句迁移 | 共执行10条用例<br />共发现4个bug，4条现已修复且验收通过 |

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果                                     | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------------------------------ | ------------ |
| openGauss 3.0.0 build 02c14696 | 134        | Passed：133  <br />Failed：1<br />Unavailable：0 | 21           |

数据说明

1. 共发现21个问题，19条已修改并验证通过，1条非问题取消，1条经CCB评审挂起
2. 1条用例因问题单挂起执行失败，其余失败用例已在后续issue回归中执行通过

## 4.3   后续测试建议

1. 关注各对象名大小写混合使用情况下，工具的迁移结果
2. 关注各对象名未加双引号情况下，工具的迁移结果
3. 关注onlineMigration工具启动命令中未加--from-begining参数时，工具的断点续传功能

# 5     附件

### 5.1 Oracle-openGauss在线迁移add/drop/alter约束语句执行过程示例

```sql
--step1:创建普通行存表 expect:创建表成功
create table "C##ROMA_LOGMINER"."t_const_0001"(
	"pid" int,
	"pname" varchar(20)
);
Table created.

--step2:add添加单一主键约束 expect:约束迁移成功
alter table "C##ROMA_LOGMINER"."t_const_0001" add constraint "pid_pk" primary key("pid");
Table altered.

--step3:插入数据 expect:成功
insert into "C##ROMA_LOGMINER"."t_const_0001" values(1,'张三');
1 row created.
insert into "C##ROMA_LOGMINER"."t_const_0001" values(2,'李四');
1 row created.
insert into "C##ROMA_LOGMINER"."t_const_0001" values(3,'王五');
1 row created.
commit;
Commit complete.

--step4:在openGauss侧插入数据验证约束 expect:插入非法数值约束生效 合理报错
insert into "C##ROMA_LOGMINER"."t_const_0001" values(1,'赵六');
ERROR:  duplicate key value violates unique constraint "pid_pk"
DETAIL:  Key (id)=(1) already exists.
insert into "C##ROMA_LOGMINER"."t_const_0001" values(null,'赵六');
ERROR:  null value in column "id" violates not-null constraint
DETAIL:  Failing row contains (null,'赵六').

--step5:drop删除单一主键约束 expect:迁移成功
alter table "C##ROMA_LOGMINER"."t_const_0001" drop constraint pid_pk;
Table altered.

--step6:在openGauss侧插入数据验证无约束 expect:插入成功
insert into t_const_0001 values(1,'张三'),(2,'李四'),(3,'王五'),(null,'王五');
INSERT 0 4

--step7:清理环境 expect:清理环境成功
drop table "C##ROMA_LOGMINER"."t_const_0001" cascade constraint purge;
```
