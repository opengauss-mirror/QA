![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者       |
| :--------- | -------- | -------------------- | ---------- |
| 2022-12-26 | 1.0      | 特性测试报告初稿完成 | lizhuo9527 |
| 2022-12-28 | 1.1      | 修改检视意见         | lizhuo9527 |

关键词： 

M*、dolphin插件、`show table status`

摘要：

本文档主要基于dolphin插件兼容M*的`show table status`语法，用于查询当前或指定`schema`中的表信息并支持添加筛选条件，针对上述语法进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1	特性概述

基于dolphin插件，实现openGauss对M*`show table status`语法的兼容，其详细参数为`show table status [{from | in} db_name] [like 'pattern' | where expr];`，该语法用于查询当前或指定`schema`中的表信息并支持使用like或where子句进行进一步筛选和匹配。

# 2	特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| ----------------------------- | ------------ | ------------ |
| openGauss 3.0.0 build b924813 | 2022-10-21   | 2022-10-26   |
| openGauss 3.0.0 build 4238d69 | 2022-10-30   | 2022-10-31   |
| dolphin 1.0                   | 2022-10-21   | 2022-10-31   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3	测试结论概述

## 3.1 测试整体结论

基于dolphin插件兼容M*的部分语法测试，详细语法如下：

* `show table status [{from | in} db_name] [like 'pattern' | where expr];`

共计执行30条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下7点：

1. `show table status`展示当前`schema`下所有表的信息；
2. 验证主要的11个结果字段所展示的信息是否与系统表中的信息相匹配；
3. 包含like子句的情形，包括：
   - %和_通配符及混合使用
   - 通配符结合转义字符使用
4. 包含where子句的情形，包括：
   - 等于/不等于 、in/not in条件筛选
   - 结合like子句及通配符、多条件匹配
5. 验证列的comment
6. 验证用户有无相关权限时`show table status`的执行结果
7. 验证视图结合`show table status`

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单2个，所有缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 展示当前`schema`下所有表的信息，执行结果符合预期，通过       |
| 功能测试 | 验证11个结果字段所展示的信息是否与系统表中的信息相匹配，执行结果符合预期，通过 |
| 功能测试 | like子句通配符结合转义字符使用的情形，执行结果符合预期，通过 |
| 功能测试 | like子句结合%和_通配符及混合使用的情形，执行结果符合预期，通过 |
| 功能测试 | where子句等于/不等于 、in/not in条件筛选的情形，执行结果符合预期，通过 |
| 功能测试 | where子句结合like子句及通配符、多条件匹配的情形，执行结果符合预期，通过 |
| 功能测试 | 验证列的comment，执行结果符合预期，通过                      |
| 功能测试 | 验证用户有无相关权限时`show table status`的执行结果，执行结果符合预期，通过 |
| 功能测试 | 验证视图结合`show table status`，执行结果符合预期，通过      |
| 资料测试 | 资料描述正确，示例的执行无误，通过                           |

## 3.2	约束说明

- 兼容M*5.7版本
- openGauss需使用兼容B库且包含dolphin插件
- `show table status`语法实际上展示的是当前`schema`下的表信息

## 3.3    遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                         | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------ | -------- |
| 1    | [I5XMMZ](https://gitee.com/opengauss/Plugin/issues/I5XMMZ?from=project-issue) | 次要     | show table status可以展示列的comment，与预期不符 | 已验收   |
| 2    | [I5XPFY](https://gitee.com/opengauss/docs/issues/I5XPFY?from=project-issue) | 次要     | show table status文档拼写错误修改及内容补充      | 已验收   |

# 4    测试执行

## 4.1 测试执行步骤

###  4.1.1 展示schema中表的信息

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.`show table status`<br>2.`show table status from/in schema` | 执行1条用例，未发现bug<br/>执行通过 |

### 4.1.2 验证结果字段的展示内容

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.结果字段Engine<br/>2.结果字段Row_format<br/>3.结果字段Rows<br/>4.结果字段Data_length<br/>5.结果字段Index_length<br/>6.结果字段Auto_increment<br/>7.结果字段Create_time<br/>8.结果字段Update_time<br/>9.结果字段Collation<br/>10.结果字段Create_options<br/>11.结果字段Comment | 执行11条用例，未发现bug<br/>执行通过 |

### 4.1.3 like子句通配符结合转义字符的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.like子句多个%匹配<br/>2.like子句多个_匹配<br>3.多个通配符结合转义字符 | 执行5条用例，未发现bug<br/>执行通过 |

### 4.1.4 where子句条件筛选的情形

| 测试步骤                                             | 测试结果                            |
| ---------------------------------------------------- | ----------------------------------- |
| 1.where 子句结合 =/!=<br/>2.where 子句结合 in/not in | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.5 where子句结合like子句及多条件匹配的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.where子句结合like子句及通配符<br/>2.where子句结合like子句and/or多条件嵌套 | 执行3条用例，未发现bug<br/>执行通过 |

### 4.1.6 验证列的comment

| 测试步骤                                                   | 测试结果                                       |
| ---------------------------------------------------------- | ---------------------------------------------- |
| 1.建表并添加列的comment<br>2.`show table status`展示表信息 | 执行1条用例，发现1个bug<br/>现已修复且验收通过 |

### 4.1.7 验证用户有无相关权限的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.用户无表权限<br/>2.用户仅有表select权限<br/>3.用户仅有表某一字段权限<br/>4.用户仅无表select权限<br/>5.用户无sequence权限时测试auto_increment | 执行5条用例，未发现bug<br/>执行通过 |

### 4.1.8 验证视图的情形

| 测试步骤                          | 测试结果                            |
| --------------------------------- | ----------------------------------- |
| 1.使用`show table status`查询视图 | 执行1条用例，未发现bug<br/>执行通过 |

### 4.1.9 资料测试

| 测试步骤                             | 测试结果                            |
| ------------------------------------ | ----------------------------------- |
| 1.资料描述是否准确、示例能否正确执行 | 共发现1个bug<br/>现已修复且验收通过 |

## 4.2  测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ----------------------------- | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build b924813 | 30         | Passed：29<br>Failed：1 | 1            |
| openGauss 3.0.0 build 4238d69 | 30         | Passed：30<br>Failed：0 | 0            |

数据项说明：

* 缺陷密度：2(缺陷个数)/0.554kloc(代码行数)=3.61(个/kloc)
* 其中1个问题单为资料单，已回归通过

## 4.3  后续测试建议

-  为数据库指定不同的`Collation`并使用`show table status`进行查询

# 5     附件

##  5.1 `show table status`

```sql
--step1: 创建schema expect: 创建成功
drop schema if exists sch_tabsat_0001;
NOTICE:  schema "sch_tabsat_0001" does not exist, skipping
DROP SCHEMA
create schema sch_tabsat_0001;
CREATE SCHEMA

--step2: 创建表 expect: 创建成功
drop table if exists sch_tabsat_0001.t_tabsat_0001;
NOTICE:  table "t_tabsat_0001" does not exist, skipping
DROP TABLE
create table sch_tabsat_0001.t_tabsat_0001(col1 int,col2 text);
CREATE TABLE

--step3: show table status expect: 查询成功
show table status from sch_tabsat_0001;
     Name      | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment |     Create_time     |     Update_time     | Check_time |
 Collation | Checksum |          Create_options          | Comment
---------------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+
-----------+----------+----------------------------------+---------
 t_tabsat_0001 | ASTORE |         | ROW        |    0 |              0 |           0 |               0 |            0 |         0 |                | 2022-12-26 16:23:18 | 2022-12-26 16:23:18 |            |
 C         |          | {orientation=row,compression=no} |
(1 row)

show table status in sch_tabsat_0001;
     Name      | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment |     Create_time     |     Update_time     | Check_time |
 Collation | Checksum |          Create_options          | Comment
---------------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+
-----------+----------+----------------------------------+---------
 t_tabsat_0001 | ASTORE |         | ROW        |    0 |              0 |           0 |               0 |            0 |         0 |                | 2022-12-26 16:23:18 | 2022-12-26 16:23:18 |            |
 C         |          | {orientation=row,compression=no} |
(1 row)

--step4: 清理环境 expect: 清理成功
drop table if exists sch_tabsat_0001.t_tabsat_0001;
DROP TABLE
drop schema if exists sch_tabsat_0001;
DROP SCHEMA
```
