![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者       |
| :--------- | ----------- | -------------------- | ---------- |
| 2022-09-16 | 1.0         | 特性测试报告初稿完成 | lizhuo9527 |
| 2022-09-21 | 1.1         | 修改特性测试报告     | lizhuo9527 |

 关键词： 

M*、dolphin插件、comment、大小写敏感

摘要：

本文档主要基于dolphin插件兼容M*的部分语法进行测试，并给出最终测试结论。包含以下两个特性，第一：由参数lower_case_table_names控制支持表名、用户名大小写敏感；第二：支持create/alter table/index/function/procedure添加注释comment。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

基于dolphin插件，实现openGauss对M*部分语法的兼容，第一，支持表名、用户名大小写敏感且受参数lower_case_table_names值控制，当lower_case_table_names=0时，表名及用户名大小写敏感，当lower_case_table_names>0时，表名及用户名大小写不敏感；第二，支持create/alter table/index/function/procedure添加注释comment。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 0128e74a | 2022-9-6     | 2022-9-16    |
| openGauss 3.0.0 build 075791e0 | 2022-9-19    | 2022-9-21    |
| dolphin 1.0                    | 2022-9-6     | 2022-9-21    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于dolphin插件兼容M*的部分语法测试，含下述两个特性：

* 支持表名、用户名大小写敏感且受参数lower_case_table_names值控制
* 支持create/alter table/index/function/procedure添加注释comment

特性一，支持表名、用户名大小写敏感特性的语法兼容测试，共计执行41条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下4点：

1. 参数控制用户名支持大小写敏感，包括create/alter/drop/grant/revoke user以及用户名大小写混合使用、引用使用的情形；
2. 参数控制表名支持大小写敏感，包括：
   - DML语句：select/insert/update/delete/truncate/merge into
   - DDL语句：create/alter table、create index、create trigger、reindex、analyze等
   - 表类型的交互：行存列存表、一级二级分区表、临时表、压缩表等
   - create table与事务、函数、存储过程的交互
   - 表名的大小写混合使用及引用使用
3. 参数的取值测试
4. 列名、索引名大小写不敏感，不受参数控制

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单2个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

特性二，支持create/alter table/index/function/procedure添加注释comment语法兼容测试，共计执行68条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下5点：

1. create table
   - 列的comment和表的comment
   - 一级、二级分区表指定comment
   - 事务、存储过程、函数中建表指定comment
2. create index指定comment
3. alter table
   - 修改表注释
   - alter table add constraint [using index]
   - alter table add column
   - alter table modify column
   - alter table add partition/subpartition
4. create table包含约束添加comment
5. create/alter function/procedure

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单4个，其中1个非问题取消，剩余3个缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 参数控制用户名支持大小写敏感，执行结果符合预期，通过         |
| 功能测试 | 表名支持大小写敏感结合DML语句，执行结果符合预期，通过        |
| 功能测试 | 表名支持大小写敏感结合DDL语句，执行结果符合预期，通过        |
| 功能测试 | 表名支持大小写敏感结合表类型的交互，执行结果符合预期，通过   |
| 功能测试 | create table与事务、函数、存储过程的交互，执行结果符合预期，通过 |
| 功能测试 | 表名的大小写混合使用及引用使用，执行结果符合预期，通过       |
| 功能测试 | 参数lower_case_table_names有效值、无效值测试，执行结果符合预期，通过 |
| 功能测试 | 列名、索引名大小写不敏感且不受参数控制，执行结果符合预期，通过 |
| 功能测试 | create table指定comment，执行结果符合预期，通过              |
| 功能测试 | create index指定comment，执行结果符合预期，通过              |
| 功能测试 | alter table修改表注释，执行结果符合预期，通过                |
| 功能测试 | alter table add constraint [using index]结合comment，执行结果符合预期，通过 |
| 功能测试 | alter table add/modify cloumn，执行结果符合预期，通过        |
| 功能测试 | alter tableadd partition/subpartition，执行结果符合预期，通过 |
| 功能测试 | create table包含约束添加comment，执行结果符合预期，通过      |
| 功能测试 | create/alter function/procedure添加comment，执行结果符合预期，通过 |
| 资料测试 | 资料描述正确，示例的执行无误，通过                           |

## 3.2   约束说明

- 兼容M*5.7版本
- openGauss需使用兼容B库且包含dolphin插件
- 参数lower_case_table_names取值为1或2（即>0）时，表名和用户名均大小写不敏感
- create/alter分区表（含二级分区表）指定comment仅做语法支持，并有warning信息告警，无实际作用
- alter table tab_name modify partition part_name add subpartition...语句因M*不支持该语法故不予实现指定comment的兼容

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6        | 0    | 0    | 6    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5R0P9](https://gitee.com/opengauss/Plugin/issues/I5R0P9?from=project-issue) | 次要     | 表名大小写敏感不支持删除行访问控制策略                       | 已验收   |
| 2    | [I5R5YD](https://gitee.com/opengauss/Plugin/issues/I5R5YD?from=project-issue) | 次要     | 调用存储过程及函数时未识别表名大小写                         | 已验收   |
| 3    | [I5REC6](https://gitee.com/opengauss/openGauss-server/issues/I5REC6?from=project-issue) | 次要     | alter table using index指定comment，<br>系统表中查询不到comment信息 | 已验收   |
| 4    | [I5RE6M](https://gitee.com/opengauss/openGauss-server/issues/I5RE6M?from=project-issue) | 次要     | create/alter table时为外键约束指定comment，<br>系统表中查询不到相关信息 | 已验收   |
| 5    | [I5RESL](https://gitee.com/opengauss/Plugin/issues/I5RESL?from=project-issue) | 次要     | 包含dolphin插件时部分指定comment的语句执行失败               | 已验收   |
| 6    | [I5REGG](https://gitee.com/opengauss/openGauss-server/issues/I5REGG?from=project-issue) | 次要     | alter table modify添加二级分区指定comment报错                | 已取消   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 用户名大小写敏感

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.create/alter/drop/grant/revoke user<br>2.用户名大小写混合使用<br>3.用户名引用使用 | 执行5条用例，未发现bug<br/>执行通过 |

### 4.1.2 表名大小写结合DML语句

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.select<br/>2.insert<br/>3.update<br/>4.delete<br/>5.truncate<br/>6.merge into | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.3 表名大小写结合DDL语句

| 测试步骤                                                     | 测试结果                                        |
| ------------------------------------------------------------ | ----------------------------------------------- |
| 1.create/alter table<br/>2.create index<br/>3.create trigger<br/>4.reindex<br/>5.analyze<br/>6.cluster...on...<br/>7.vacuum<br/>8.rule | 执行12条用例，发现1个bug<br/>现已修复且验收通过 |

### 4.1.4 表名大小写结合表类型交互

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.行存、列存表<br/>2.全局、本地临时表<br/>3.一级、二级分区表<br/>4.压缩表<br/>5.ustore表 | 执行8条用例，未发现bug<br/>执行通过 |

### 4.1.5 表名大小写结合事务、函数、存储过程

| 测试步骤                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.事务中建表结合表名大小写敏感<br/>2.函数中建表结合表名大小写敏感<br/>3.存储过程中建表结合表名大小写敏感 | 执行4条用例，发现1个bug<br/>现已修复且验收通过 |

### 4.1.6 表名的大小写混合使用及引用使用

| 测试步骤                                               | 测试结果                            |
| ------------------------------------------------------ | ----------------------------------- |
| 1.表名全小写、全大写及大小写混合使用<br>2.表名引用使用 | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.7 参数lower_case_table_names取值

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.参数取有效值，测试表名用户名是否大小写敏感<br/>2.参数取无效值 | 执行4条用例，未发现bug<br/>执行通过 |

### 4.1.8 列名、索引名大小写不敏感

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.设置参数lower_case_table_names=0/1/2<br/>2.建表指定列名大小写混合使用<br>3.create index索引名大小写混合使用 | 执行4条用例，未发现bug<br/>执行通过 |

### 4.1.9 create table指定comment

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.create table指定表注释<br>2.create table指定列注释<br>3.分区表指定comment<br>4.事务、存储过程、函数中create table指定comment | 执行20条用例，未发现bug<br/>执行通过 |

### 4.1.10 create index指定comment

| 测试步骤                                      | 测试结果                            |
| --------------------------------------------- | ----------------------------------- |
| 1.create index idx_name comment...指定comment | 执行4条用例，未发现bug<br/>执行通过 |

### 4.1.11 alter table修改表注释

| 测试步骤                                    | 测试结果                            |
| ------------------------------------------- | ----------------------------------- |
| 1.alter table tab_name comment...修改表注释 | 执行1条用例，未发现bug<br/>执行通过 |

### 4.1.12 alter table add constraint [using index]结合comment

| 测试步骤                                                     | 测试结果                                        |
| ------------------------------------------------------------ | ----------------------------------------------- |
| 1.alter table tab_name add constraint comment...<br>2.alter table tab_name add constraint using index comment... | 执行12条用例，发现2个bug<br/>现已修复且验收通过 |

### 4.1.13 alter table add/modify column结合comment

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.alter table tab_name add column comment...<br>2.alter table tab_name modify column comment...修改列注释 | 执行10条用例，未发现bug<br/>执行通过 |

### 4.1.14 alter table add partition/subpartition

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.alter table add partition<br>2.alter table modify partition add subpartition | 执行4条用例，发现1个bug<br/>非问题取消 |

### 4.1.15 create table包含约束添加comment

| 测试步骤                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.create table包含约束，为约束指定comment<br>2.create table建索引指定comment | 执行9条用例，发现1个bug<br/>现已修复且验收通过 |

### 4.1.16 create/alter function/procedure

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.create function指定comment<br>2.alter function修改comment<br>3.create procedure指定comment<br>4.alter procedure修改comment | 执行8条用例，未发现bug<br/>执行通过 |

## 4.2  测试执行统计数据

#### 特性1 表名用户名大小写敏感

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 3.0.0 build 0128e74a | 41         | Passed：38<br>Failed：3  | 2            |
| openGauss 3.0.0 build 075791e0 | 41         | Passed：41<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单2个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：2(缺陷个数)/18.639kloc(代码行数)=0.11(个/kloc)

#### 特性2 实现对comment的支持

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build 0128e74a | 68         | Passed：64<br>Failed：4 | 4            |
| openGauss 3.0.0 build 075791e0 | 68         | Passed：68<br>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单4个，其中3个已验收通过，1个非问题取消
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：3(缺陷个数)/0.712kloc(代码行数)=4.21(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

##  5.1 表名大小写敏感

```sql
--step1：create table except：创建成功
set lower_case_table_names=0;
SET
drop table if exists T_LETTER_0008;
NOTICE:  table "T_LETTER_0008" does not exist, skipping
DROP TABLE
create table T_LETTER_0008
(col1 int
,col2 varchar(64));
CREATE TABLE

--setp2：create table as except：创建成功
drop table if exists T_LETTER_0008_01;
NOTICE:  table "T_LETTER_0008_01" does not exist, skipping
DROP TABLE
create table T_LETTER_0008_01 as select col1 from T_LETTER_0008;
INSERT 0 0

--step3：系统表查询 except：成功
select relname from pg_class where relname = 'T_LETTER_0008_01';
     relname
------------------
 T_LETTER_0008_01
(1 row)

--step4 ：create table like except：创建成功
drop table if exists T_LETTER_0008_02;
NOTICE:  table "T_LETTER_0008_02" does not exist, skipping
DROP TABLE
create table T_LETTER_0008_02 (like T_LETTER_0008);
CREATE TABLE

--step5：系统表查询 except：成功
select relname from pg_class where relname = 'T_LETTER_0008_02';
     relname
------------------
 T_LETTER_0008_02
(1 row)

--step6：清理环境 except：成功
drop table if exists T_LETTER_0008;
DROP TABLE
drop table if exists T_LETTER_0008_01;
DROP TABLE
drop table if exists T_LETTER_0008_02;
DROP TABLE
set lower_case_table_names=1;
SET
```

## 5.2 comment语法使用

```sql
--step1：create table添加注释 except：创建成功
drop table if exists t_comment_0023;
NOTICE:  table "t_comment_0023" does not exist, skipping
DROP TABLE
create table t_comment_0023
(id int
,name varchar(64))
comment 'user_info';
CREATE TABLE

--setp2：查看注释 as except：注释存在
select description from pg_description  pd join pg_class pc
on pd.objoid = pc.oid where pc.relname = 't_comment_0023';
 description
-------------
 user_info
(1 row)

--step3：修改表注释 except：成功
alter table t_comment_0023 
comment='company_info';
ALTER TABLE

--setp4：查看注释 as except：注释存在
select description from pg_description  pd join pg_class pc
on pd.objoid = pc.oid where pc.relname = 't_comment_0023';
 description
--------------
 company_info
(1 row)

--step5：修改表注释 except：成功
alter table t_comment_0023 
comment 'company_information';
ALTER TABLE

--setp6：查看注释 as except：注释存在
select description from pg_description  pd join pg_class pc
on pd.objoid = pc.oid where pc.relname = 't_comment_0023';
     description
---------------------
 company_information
(1 row)

--step7：清理环境 except：成功
drop table if exists t_comment_0023;
DROP TABLE
```
