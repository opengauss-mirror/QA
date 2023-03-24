![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|    日期    | 修订版本 |       修改描述       |   作者    |
| :--------: | :------: | :------------------: | :-------: |
| 2023-03-13 |   1.0    | 特性测试报告初稿完成 | zhanghang |

 关键词： ALGORITHM、插件dolphin

 摘要：

本文档主要基于插件dolphin实现支持视图、索引等语法带ALGORITHM选项的功能进行测试，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| :----- | :------- | -------- |
| NA     |          |          |

# 1     特性概述

基于插件dolphin，openGauss在兼容B模式下支持view的创建、修改以及index的创建、删除操作带ALGORITHM选项，仅实现语法兼容，不需要做功能兼容。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| MySQL 5.7<br/>openGauss 5.0.0 build 5a69c469<br/>dolphin 1.0 | 2023-03-06   | 2023-03-10   |

| 环境信息 | 配置信息                                                     | 备注 |
| :------: | :----------------------------------------------------------- | ---- |
|  虚拟机  | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br />内存：32GB<br />硬盘：100GB<br />OS：CentOS Linux release 7.6.1908 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

支持视图、索引等语法带ALGORITHM选项共计执行用例60条，主要覆盖了功能测试和资料测试，功能测试覆盖在行存表、列存表、分区表、临时表、函数、存储过程等场景下分别验证create view、alter view、create index、drop index语法带ALGORITHM选项。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。发现问题单1个，为资料单，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证create view语法带ALGORITHM选项，通过 |
| 功能测试 | 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证alter view语法带ALGORITHM选项，通过 |
| 功能测试 | 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证create index语法带ALGORITHM选项，通过 |
| 功能测试 | 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证drop index语法带ALGORITHM选项，通过 |
| 资料测试 | 资料描述完整准确，整体质量良好                               |

## 3.2   约束说明

- M*使用5.7版本。
- openGauss需使用兼容B库且有dolphin插件。
- create view、alter view、create index、drop index语法中ALGORITHM选项仅实现语法兼容，无实际功能。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :---------: | :------: | :--: | :--: | :--: | :----: |
|  数目 (个)  |    1     |  0   |  0   |  1   |   0    |
| 百分比  (%) |   100%   |  0%   |  0%   | 100% |   0%   |

### 3.3.3 问题详情

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| :--: | ------------------------------------------------------------ | :------: | ------------------------------------------------------------ | :------: |
|  1   | [I6LMKP](https://gitee.com/opengauss/docs/issues/I6LMKP?from=project-issue) |   次要   | dolphin语法介绍>SQL参考>DDL语法一览表中修改视图关联的相关SQL和跳转后的相关SQL不一致 |  已验收  |

# 4     测试执行

## 4.1   测试执行统计数据

### 4.1.1  验证create view语法带ALGORITHM选项

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 创建兼容B库并加载dolphin插件 <br />2. 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证create view语法带ALGORITHM选项 | 执行15条用例，未发现bug |

### 4.1.2  验证alter view语法带ALGORITHM选项

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 创建兼容B库并加载dolphin插件 <br />2. 在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证alter view语法带ALGORITHM选项 | 执行15条用例，未发现bug |

### 4.1.3  验证create index语法带ALGORITHM选项

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1.创建兼容B库并加载dolphin插件 <br />2.在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证create index语法带ALGORITHM选项 | 执行15条用例，未发现bug |

### 4.1.4  验证drop index语法带ALGORITHM选项

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1.创建兼容B库并加载dolphin插件 <br />2.在行存表、列存表、分区表、临时表、函数、存储过程等场景下验证drop index语法带ALGORITHM选项 | 执行15条用例，未发现bug |

## 4.2   资料测试

| 测试步骤           | 测试结果           |
| ------------------ | ------------------ |
| 资料的描述是否正确 | 发现1个bug，已回归通过 |

## 4.3   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题 |
| ------------------------------ | ---------- | ------------------------- | -------- |
| openGauss 5.0.0 build 5a69c469 | 60         | Passed：60<br />Failed：0 | 0        |

数据项说明：

- 累计发现缺陷单1个，为资料缺陷，已完成
- 缺陷密度为0个/kloc

## 4.4 后续测试建议

无

# 5     附件

## 5.1  执行结果示例

```sql
--step1：建表；expect：成功
dbtest=# drop table if exists t_view0001 cascade;
NOTICE:  table "t_view0001" does not exist, skipping
DROP TABLE
dbtest=# create table t_view0001(c1 int auto_increment primary key,c2 json not null);
NOTICE:  CREATE TABLE will create implicit sequence "t_view0001_c1_seq" for serial column "t_view0001.c1"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_view0001_pkey" for table "t_view0001"
CREATE TABLE

--step2：创建视图，添加algorithm选项；expect：成功 
dbtest=# create algorithm = undefined view v_view0001_01 as select * from t_view0001;
CREATE VIEW
dbtest=# create algorithm = merge view v_view0001_02 as select * from t_view0001;
CREATE VIEW
dbtest=# create algorithm = temptable view v_view0001_03 as select * from t_view0001;
CREATE VIEW
dbtest=# \d+
                                                       List of relations
 Schema |             Name              |      Type      | Owner |    Size    |             Storage              | Description
--------+-------------------------------+----------------+-------+------------+----------------------------------+-------------
 public | index_statistic               | view           | zhang | 0 bytes    |                                  |
 public | pg_type_nonstrict_basic_value | view           | zhang | 0 bytes    |                                  |
 public | t_view0001                    | table          | zhang | 8192 bytes | {orientation=row,compression=no} |
 public | t_view0001_c1_seq             | large sequence | zhang | 8192 bytes |                                  |
 public | v_view0001_01                 | view           | zhang | 0 bytes    |                                  |
 public | v_view0001_02                 | view           | zhang | 0 bytes    |                                  |
 public | v_view0001_03                 | view           | zhang | 0 bytes    |                                  |
(7 rows)

--step3：清理环境；expect：成功
dbtest=# drop table if exists t_view0001 cascade;
NOTICE:  drop cascades to 3 other objects
DETAIL:  drop cascades to view v_view0001_01
drop cascades to view v_view0001_02
drop cascades to view v_view0001_03
DROP TABLE
```