![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2022-9-14 |   1.0    |           特性测试报告初稿           | zou_jialiang050 |
| 2022-9-21 | 1.0 | 修改bug状态 | zou_jialiang050 |

 关键词： tablespace、rollup、create table、rename、collate、charset、index、row_format、charset

摘要：

本文对openGauss实现对MYSQL中create table table_name select、create tablespace space_name add datafile 'file_path' engine = engine_name、alter table [if exists] old_table_name rename {to | as} new_table_name、create index index_name [using {btree | hash} on table_name(key_part,...)、group  by with rollup语法、表级、字段级charset、collate，表级row_format语法支持特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  无  |          |          |

# 1     特性概述

openGauss在兼容b库下，可以实现对create table table_name select、create tablespace space_name add datafile 'file_path' engine = engin_name、alter table old_table_name rename {to | as} new_table_name、create index index_name [using {btree | hash} on table_name(key_part,...)、group  by with rollup语法、表级、字段级charset、collate，表级row_format语法的兼容，其执行结果与MYSQL侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :----------- |
| MYSQL 5.7                     | 2022-9-1     | 2022-9-14    |
| openGauss3.1.0 build b4215eeb | 2022-9-1     | 2022-9-14    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C 8核<br />内存：32GB<br />硬盘：100G<br />OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss实现对MYSQL部分语法的支持特性测试中，共计执行用例96个，主要涉及create table table_name select、create tablespace space_name add datafile 'file_path' engine = engine_name、alter table old_table_name rename {to | as} new_table_name、create index index_name [using {btree | hash} on table_name(key_part,...)、兼容表级、字段级charset、collate，表级row_format语法、兼容group  by with rollup语法，剩余1条问题待确认。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 兼容create table table_name select语法测试通过 |
| 功能测试 | 兼容create tablespace space_name add datafile 'file_path' engine = engine_name语法测试通过 |
| 功能测试 | 兼容alter table [if exists] old_table_name rename {to \| as} new_table_name语法测试通过 |
| 功能测试 | 兼容group  by with rollup语法测试通过 |
| 功能测试 | 兼容create index index_name [using {btree \| hash} on table_name(key_part,...)语法测试通过 |
| 功能测试 | 兼容表级、字段级charset、collate，表级row_format语法测试通过 |
| 资料测试 | 资料叙述准确无误，示例正确，通过                             |

## 3.2   约束说明

- 兼容MYSQL 5.7
- openGauss需在兼容b库下执行

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    [I5QHIQ](https://gitee.com/opengauss/Plugin/issues/I5QHIQ)    | 兼容b库下执行create tablespace add datafile不支持.ibd文件 | 次要 | 待评审后确认是否支持ibd文件 | 待确认 |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    1    |  0  |  0   |  1   |   0    |
| 百分比  (%) |   100    |  0  |  0   |  100  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
|  1   |  [I5QHIQ](https://gitee.com/opengauss/Plugin/issues/I5QHIQ)  |   次要   | 兼容b库下执行create tablespace add datafile不支持.ibd文件 |  待确认  |

# 4     测试执行

## 4.1   功能测试

### 4.1.1   兼容create table table_name select 语法功能验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.结合多种表类型及字段类型对create table table_name select语法进行功能验证 | 共执行测试用例18条<br />未发现bug。 |

###  4.1.2   兼容create tablespace space_name add datafile 'file_path' engine = engine_name语法功能验证

| 测试步骤                                                     | 测试结果                                                |
| :----------------------------------------------------------- | :------------------------------------------------------ |
| 1.结合不同权限datafile及.ibd文件等对create tablespace space_name add datafile 'file_path' engine = engine_name进行创建表空间功能验证 | 共执行测试用例7条<br />发现1条bug，现已验收并回归通过。 |

###  4.1.3   兼容alter table [if exists] old_table_name rename {to | as} new_table_name语法功能验证

| 测试步骤                                                     | 测试结果                            |
| :------------------------------------------------------------ | :----------------------------------- |
| 1.结合多种表类型对alter table [if exists] old_table_name rename {to \| as} new_table_name进行表重命名功能验证 | 共执行测试用例16条<br />未发现bug。 |

###  4.1.4   兼容group  by with rollup语法功能验证

| 测试步骤                                                     | 测试结果                            |
| :------------------------------------------------------------ | :----------------------------------- |
| 1.筛选表数据结合聚合函数对group by with rollup语法进行功能验证 | 共执行测试用例11条<br />未发现bug。 |

###  4.1.5   兼容create index index_name [using {btree | hash}] on table_name(key_part,...)语法功能验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.结合多种表类型及字段类型对create index index_name [using {btree \| hash}] on table_name(key_part,...)语法进行功能验证 | 共执行测试用例15条<br />未发现bug。 |

###  4.1.6   兼容表级、字段级charset、collate，表级row_format语法功能验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.结合多种字段类型及参数值验证字段级charset、collate功能正确性<br />2.结合多种表类型及参数值验证表级charset、collate、row_format功能正确性 | 共执行测试用例30条<br />未发现bug。 |

## 4.2   资料测试

| 测试步骤                                   |                                                     |
| :------------------------------------------ | :--------------------------------------------------- |
| 1.确认资料描述是否准确无误，示例是否正确。 | 未发现bug。 |

## 4.3   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| :----------------------------- | :---------- | :----------------------- | :------------ |
| openGauss3.1.0 build b4215eeb | 96      | Passed:96<br />Failed:1 | 1           |

累计发现缺陷单1个，剩余1个问题单待确认。

缺陷单共1个，修改代码量为0.93kloc， 缺陷密度为1.08个/kloc

## 4.4   后续测试建议

1.表级collate、charset、row_format对mysql中参数的兼容

# 5     附件

```
openGauss=# DROP TABLE IF EXISTS test;
NOTICE:  table "test" does not exists, skipping
DROP TABLE
openGauss=# CREATE TABLE test(c1 VARCHAR(10));
CREATE TABLE
openGauss=# CREATE TABLE test1 SELECT * FROM TSET;
INSERT 0 0

openGauss=# DROP TABLE IF EXISTS test;
NOTICE:  table "test" does not exists, skipping
DROP TABLE
openGauss=# CREATE TABLE test(c1 VARCHAR(10));
CREATE TABLE
openGauss=# ALTER TABLE test RENAME TO test1;
ALTER TABLE

openGauss=# CREATE TABLESPACE space_01 ADD DATAFILE 'test_file' ENGINE = test_engine;
CREATE TABLESPACE

openGauss=# DROP TABLE IF EXISTS test;
NOTICE:  table "test" does not exists, skipping
DROP TABLE
openGauss=# CREATE TABLE test(c1 VARCHAR(10),c2 INT);
CREATE TABLE
openGauss=# INSERT INTO TEST VALUES('A', 120),('A', 120),('B', 128),('C', 140);
INSERT 0 4
openGauss=# SELECT c1,sum(c2) from test GROUP BY c1 WITH ROLLUP;
 c1 | sum 
----+-----
 A  | 240
 B  | 125
 C  | 140
    | 505
(4 rows)

openGauss=# DROP TABLE IF EXISTS test;
NOTICE:  table "test" does not exists, skipping
DROP TABLE
openGauss=# CREATE TABLE test(c1 TEXT CHARSET "sql_text" COLLATE "default") CHARSET="test_charset" COLLATE="test_collate" ROW_FORMAT="test_fromat";
WARNING:  CHARSET for TABLE is not supported for current version. skipped
WARNING:  COLLATE for TABLE is not supported for current version. skipped
WARNING:  ROW_FORMAT for TABLE is not supported for current version. skipped
CREATE TABLE
```