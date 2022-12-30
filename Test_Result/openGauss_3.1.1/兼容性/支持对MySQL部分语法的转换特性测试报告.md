![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者      |
| ---------- | ----------- | -------------------- | --------- |
| 2022-12-15 | 1.0         | 特性测试报告初稿完成 | zhanghang |
| 2022-12-30 | 1.1         | 修改检视意见 | zhanghang |

 关键词： 

VARCHAR、NATIONAL、like、 table()

摘要：

本文档主要支持对M*部分语法的转换进行测试，并给出测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| N/A    |          |          |

# 1 特性概述

本次特性为支持对*部分语法的转换，实现对以下语法转换：

1. VARCHAR(M)支持[NATIONAL]选项
2. 支持对分区表的create table like语法，感知源表是否为分区表
3. 支持insert into table() values(...)，table后括号为空，等价于table不带括号，默认全部字段，顺序为声明时顺序

用户在兼容B库下加载dolphin插件时可以使用以上特性。

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build 6e6fb419 | 2022-11-09   | 2022-11-16   |
| openGauss 3.1.0 build a8dd3ece | 2022-12-15   | 2022-12-16   |
| M* 5.7.36                      | 2022-09-23   | 2022-11-29   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：32GB<br />硬盘：100GB<br />OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3  测试结论概述

## 3.1 测试整体结论

支持对MySQL部分语法的转换共计执行用例45条，主要覆盖了功能测试和资料测试。功能测试主要覆盖以下方面：

1.验证NATIONAL VARCHAR(M)与其他数据类型转换

2.验证create table like语法，不加参数，感知源表为分区表

3.验证insert into table() values(...)语法等价于table不带括号的语法

4.在表、视图、存储过程、自定义函数中使用NATIONAL VARCHAR(M)类型

5.在存储过程、自定义函数中验证create table like语法和insert into table() values(...)语法

资料测试覆盖资料描述是否准确、约束覆盖是否全面以及示例是否正确。

共计发现缺陷单3个，1个问题单确认非问题后已取消，2个问题单已修复且回归通过，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 验证NATIONAL VARCHAR(M)与其他数据类型转换，通过              |
| 功能测试 | 验证create table like语法，不加参数，感知源表为分区表，通过  |
| 功能测试 | 验证insert into table() values(...)语法等价于table不带括号的语法，通过 |
| 功能测试 | 在表、视图、存储过程、自定义函数中使用NATIONAL VARCHAR(M)类型，通过 |
| 功能测试 | 在存储过程、自定义函数中验证create table like语法和insert into table() values(...)语法，通过 |
| 资料测试 | 资料描述是否准确、约束覆盖是否全面以及示例是否正确，通过     |

## 3.2 约束说明

- 兼容M* 5.7版本。
- openGauss需要在兼容B库下加载dolphin插件才能使用。
- like后不能添加普通建表的额外可选语句。
- table前不能添加foreign选项，包括外表、mot表的创建。
- 默认复制源表的索引，若不希望复制索引，需要手动指定EXCLUDING INDEXES。
- 默认复制源分区表的分区，若不希望复制分区，需要手动指定EXCLUDING PARTITION。
- 对于含索引的分区表，若只指定EXCLUDING PARTITION，由于默认复制分区，将会报错，因为普通表不支持分区索引。
- 只支持复制range分区表的分区，对于hash、list分区表，由于默认复制分区，会直接报错，需要手动指定EXCLUING PARTITION。二级分区只支持复制range-range分区，处理方法同上。
- 如果源表是本地临时表，则新表也必须是本地临时表，否则会报错。
- ATUO_INCREMENT列需要为主键或唯一约束的第一个字段，若复制包含AUTO_INCREAMENT列的表时指定EXCLUDING INDEX，将会报错。其中AUTO_INCREAMENT只在B库中生效。

## 3.3  遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 0    | 3    | 0      |
| 百分比 | 100%     | 0    | 0    | 100% | 0      |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I612NT](https://gitee.com/opengauss/Plugin/issues/I612NT?from=project-issue) | 次要     | list分区表、hash分区表使用create table like语句建表时使用excluding all参数，建表失败 | 已验收   |
| 2    | [I612TG](https://gitee.com/opengauss/Plugin/issues/I612TG?from=project-issue) | 次要     | create table like语法源表为普通表，新表指定为分区表，新表创建失败 | 已取消   |
| 3    | [I61RFR](https://gitee.com/opengauss/docs/issues/I61RFR?from=project-issue) | 次要     | create table like语法不支持部分选项，资料未说明              | 已验收   |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 验证NATIONAL VARCHAR(M)与其他数据类型转换

| 测试步骤                                                   | 测试结果                              |
| ------------------------------------------------------------ | ------------------------------------- |
| 1.验证NATIONAL VARCHAR(M)与其他数据类型显式转换<br />2.验证NATIONAL VARCHAR(M)与其他数据类型阴式转换 | 执行17条用例，未发现bug，结果符合预期 |

### 4.1.2 验证create table like语法，不加参数，感知源表为分区表

| 测试步骤                                              | 测试结果                                                     |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| 1.验证create table like语法，不加参数，感知源表为分区表 | 执行16条用例，发现2个bug，1个非问题取消，1个已修复且验收通过 |

### 4.1.3 验证insert into table() values(...)语法等价于table不带括号的语法

| 测试步骤                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.验证insert into table() values(...)语法等价于table不带括号的语法 | 执行5条用例，未发现bug，结果符合预期 |

### 4.1.4 在表、视图、存储过程、自定义函数中使用NATIONAL VARCHAR(M)类型

| 测试步骤                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.修改表中相应列类型为NATIONAL VARCHAR(M)类型<br />2.在视图中含NATIONAL VARCHAR(M)类型<br />3.在存储过程中使用NATIONAL VARCHAR(M)类型<br />4.在自定义函数中使用NATIONAL VARCHAR(M)类型 | 执行5条用例，未发现bug，结果符合预期 |

### 4.1.5 在存储过程、自定义函数中验证create table like语法和insert into table() values(...)语法

| 测试步骤：                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.分别在存储过程、自定义函数中验证create table like语法和insert into table() values(...)语法 | 执行2条用例，未发现bug，结果符合预期 |

## 4.2  资料测试

| 测试步骤：                                           | 测试结果                        |
| ---------------------------------------------------- | ------------------------------- |
| 1.资料描述是否准确、约束覆盖是否全面以及示例是否正确 | 发现1个bug，1个已修复且验收通过 |

## 4.3 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build 6e6fb419 | 45         | Passed：41<br />Failed：4 | 3            |
| openGauss 3.1.0 build a8dd3ece | 45         | Passed：45<br />Failed：0 | 0            |

数据说明

1.  共计发现缺陷单3个，1个确认非问题后已取消，2个问题单已修复且回归通过（1个为功能单，1个为资料单）
2.  缺陷密度：1个（缺陷个数）/1.459kloc（代码行数）=0.7个/kloc

## 4.4 后续测试建议

无

# 5 附件

## 5.1 在表中验证create table like语法示例

```sql
--step1:建表; expect:运算成功
dbtest=# drop table if exists t_table_0001;
NOTICE:  table "t_table_0001" does not exist, skipping
DROP TABLE
dbtest=# create table t_table_0001 (
dbtest(# col01 int auto_increment primary key comment '首列',
dbtest(# col02 int not null,
dbtest(# col03 date default '2000-01-01 00:00:01',
dbtest(# col04 varchar(255) unique,
dbtest(# col05 varchar(10) check(length(col05)>=5),
dbtest(# col06 int);
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_col01_seq" for serial column "t_table_0001.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_pkey" for table "t_table_0001"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_col04_key" for table "t_table_0001"
CREATE TABLE
dbtest=# create index i_table_0001_01 on t_table_0001(col05);
CREATE INDEX
      
--step2:create table like语法创建普通表; expect:部分测试点合理报错
dbtest=# create table t_table_0001_01 like t_table_0001;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_01_col01_seq" for serial column "t_table_0001_01.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_01_pkey" for table "t_table_0001_01"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_01_col04_key" for table "t_table_0001_01"
CREATE TABLE

dbtest=# create table t_table_0001_02 like t_table_0001 including partition;
ERROR:  could not specify "INCLUDING PARTITION" for non-partitioned-table relation:"t_table_0001"

dbtest=# create table t_table_0001_03 like t_table_0001 excluding partition;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_03_col01_seq" for serial column "t_table_0001_03.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_03_pkey" for table "t_table_0001_03"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_03_col04_key" for table "t_table_0001_03"
CREATE TABLE

dbtest=# create table t_table_0001_04 like t_table_0001 including all;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_04_col01_seq" for serial column "t_table_0001_04.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_04_pkey" for table "t_table_0001_04"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_04_col04_key" for table "t_table_0001_04"
CREATE TABLE

dbtest=# create table t_table_0001_05 like t_table_0001 excluding all;
CREATE TABLE


dbtest=# create table t_table_0001_06 like t_table_0001 including defaults;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_06_col01_seq" for serial column "t_table_0001_06.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_06_pkey" for table "t_table_0001_06"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_06_col04_key" for table "t_table_0001_06"
CREATE TABLE

dbtest=# create table t_table_0001_07 like t_table_0001 excluding defaults;
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_07_pkey" for table "t_table_0001_07"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_07_col04_key" for table "t_table_0001_07"
CREATE TABLE
       
dbtest=# create table t_table_0001_08 like t_table_0001 including comments;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_08_col01_seq" for serial column "t_table_0001_08.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_08_pkey" for table "t_table_0001_08"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_08_col04_key" for table "t_table_0001_08"
CREATE TABLE
       
dbtest=# create table t_table_0001_09 like t_table_0001 excluding comments;
NOTICE:  CREATE TABLE will create implicit sequence "t_table_0001_09_col01_seq" for serial column "t_table_0001_09.col01"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_table_0001_09_pkey" for table "t_table_0001_09"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_table_0001_09_col04_key" for table "t_table_0001_09"
CREATE TABLE

dbtest=# select count(relname) from pg_class where relname like 't_table_0001___';
 count
-------
     8
(1 row)
```