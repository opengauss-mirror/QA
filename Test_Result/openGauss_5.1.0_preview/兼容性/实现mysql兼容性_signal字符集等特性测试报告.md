![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2023-8-9 |   1.0    |           特性测试报告初稿           | liumin30023837 |

 关键词： 字符集、字符序、gbk、gb18030、 set names、_charset、signal、resignal、start transaction

摘要：

本文对实现设置字符集字符序、指定常量字符集、多字符集表达式、set类型支持指定字符序、支持gbk、gb18030相关字符序、支持创建事务时开启一致性快照、支持存储过程resignal语法和signal、mysql语法兼容性特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

openGauss实现对设置字符集字符序SET NAMES、指定常量字符集_charset、多字符集表达式、set类型指定字符序、gbk、gb18030字符序、start transaction with consistent snapshot、存储过程resignal语法和signal语法语法与MySQL的兼容。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| :----------------------------- | :----------- | :----------- |
| openGauss 5.1.0 build 30e029fa | 2023-7-21    | 2023-8-1     |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz 8核<br />内存：32GB<br />硬盘：200G<br />OS： openEuler 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

实现设置字符集字符序、指定常量字符集、多字符集表达式、set类型指定字符序、gbk、gb18030字符序支持创建事务时开启一致性快照、支持存储过程resignal语法和signal的mysql语法兼容性特性测试中，主要涉及函数及语法的功能验证、文档验证，共发现9个问题，现回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 兼容语法主要功能验证，通过 |
| 资料测试 | 兼容语法资料验证，通过 |

## 3.2   约束说明

- 此需求需在兼容B库下验证
- 参数b_format_behavior_compat_options含有”enable_multi_charset“选项时，支持_charset语法和多字符集转换、字符序合并规则
- 兼容XML、JSON、TSVECTOR相关、RECORD、HLL相关数据类型不支持多字符集转换，必须为server_encoding
- set不指定字符序，保持原set行为；指定字符集、字符序后，行为保持与MySQL一致
- 不支持在用户创建的扩展、数据类型等对象中使用多字符集数据，可能会产生错误结果
- 当字符串向不支持字符序的数据类型转换，或向没有pg_cast中没有转换路径的数据类型转换时，会丢失字符集和字符序属性
- cstore表和tsstore表中字段的字符集必须与server_encoding相同
- 分区表的分区键字段的字符集必须与server_encoding相同

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |  |          |  |  |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    9    |  2  |  4   |  3  |   0    |
| 百分比  (%) |   100   |  22.22  |  44.44  |  33.33  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
| 1 | [I7PTWT](https://e.gitee.com/opengaussorg/dashboard?issue=I7PTWT) | 严重 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：支持返回值为字符串、binary的函数支持字符序】使用pbe 带字符序查询的语句  和直接查询结果 不一致 | 已验收 |
| 2 | [I7KU6D](https://e.gitee.com/opengaussorg/dashboard?issue=I7KU6D) | 严重 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：openGauss兼容start transaction with consistent snapshot】创建事务时开启一致性快照语法需要在代码层面 限制在mysql兼容性数据库 | 已验收 |
| 3 | [I7OP3F](https://e.gitee.com/opengaussorg/dashboard?issue=I7OP3F) | 主要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：支持gbk、gb18030字符集的字符序】SQL_ASCII库下字符集行为不正确 | 已验收 |
| 4 | [I7O2YY](https://e.gitee.com/opengaussorg/dashboard?issue=I7O2YY) | 主要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：SET NAMES】设置collation_connection为不是字符集的默认字符序的时候部分设置成功，部分设置报错 | 已验收 |
| 5 | [I7NU0M](https://e.gitee.com/opengaussorg/dashboard?issue=I7NU0M) | 主要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：存储过程实现signal和resignal】RESIGNAL SQLSTATE '01012'时候，show warnings结果有误 | 已验收 |
| 6 | [I7NOPB](https://e.gitee.com/opengaussorg/dashboard?issue=I7NOPB) | 主要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：存储过程实现signal和resignal】resignal后 参数mysql_errno 结果异常 | 已验收 |
| 7 | [I7PJP7](https://e.gitee.com/opengaussorg/dashboard?issue=I7PJP7) | 次要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：支持返回值为字符串、binary的函数支持字符序】limit 后带字符序binary  未报错 也未生效 | 已验收 |
| 8 | [I7O12H](https://e.gitee.com/opengaussorg/dashboard?issue=I7O12H) | 次要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：兼容MySQL设置字符集字符序语法：SET NAMES】设置character_set_connection为binary 时候报语法错误 | 已验收 |
| 9 | [I7NDO9](https://e.gitee.com/opengaussorg/dashboard?issue=I7NDO9) | 次要 | 【测试类型：功能测试】【测试版本：5.1.0】【需求名称：存储过程实现signal和resignal】condition_information_item_name 参数CLASS_ORIGIN和SUBCLASS_ORIGIN  实际操作时候为空  资料上写默认为ISO 9075 | 已验收 |

# 4     测试执行

## 4.1   功能测试

| 测试步骤                                                     | 测试结果   |
| ------------------------------------------------------------ | ---------- |
| 1.设置不同字符集字符序，测试是否设置成功，异常值是否正常报错；设置字符集字符序跑dml基础功能。<br />2.测试常量指定不同字符集字符序。<br />3.多字符集在不同表达式中的使用。<br />4.set数据类型在schema、表、列级别设置以及修改不同字符序；select update时候对set类型指定字符序；字段指定set数据类型字符序，对字段进行操作包括select update insert等操作；观察结果正确性。<br />5.在schema、表、列级别设置字符序为gbk、gb18030；select update时候指定字符序为gbk、gb18030；对设置字符序为gbk、gb18030的表进行select update insert等操作；观察结果正确性。<br />6.使用start transaction with consistent snapshot，功能正常，可以正确在开启事务时候开启快照。<br />7.验证signal、resignal语法各个位置参数正常异常取值，异常取值正常报错，正常取值结果正确；验证signal、resignal和get diagnostics交互测试。 | 发现8个bug |

## 4.2   资料测试

| 测试步骤                           | 测试结果   |
| :--------------------------------- | :--------- |
| 1.验证资料语法描述及示例是否正确。 | 发现1个bug |

## 4.3   测试执行统计数据

缺陷单共9个，其中1个为资料单

## 4.4   后续测试建议

增大数据量进一步加固测试；在复杂的场景中调用signal、resignal进一步加固测试。

# 5     附件

```sql
charset_biao1=# set names gb18030;
SET
charset_biao1=# show server_encoding;
 server_encoding
-----------------
 UTF8
(1 row)

charset_biao1=# show client_encoding;
 client_encoding
-----------------
 GB18030
(1 row)

charset_biao1=# show character_set_connection;
 character_set_connection
--------------------------
 GB18030
(1 row)

charset_biao1=# show collation_connection;
 collation_connection
----------------------
 gb18030_chinese_ci
(1 row)

charset_biao1=# SELECT CONCAT(_utf8mb4'高斯DB' , _binary'高斯DB') result, collation for(result);
           result           |  pg_collation_for
----------------------------+--------------------
 高斯DB\xe9ab98e696af4442 | gb18030_chinese_ci
(1 row)
charset_biao1=# create table tb_set7(c1 set('all','ALL','aLL') CHARSET gb18030 COLLATE gb18030_bin,c2 int);
NOTICE:  CREATE TABLE will create implicit set "tb_set7_c1_set" for column "tb_set7.c1"
CREATE TABLE
charset_biao1=# insert into tb_set7 values('all',1),('ALL',2),('aLL',3),('all',4);
INSERT 0 4
charset_biao1=# \d+ tb_set7
                                           Table "public.tb_set7"
 Column |      Type      |                 Modifiers                 | Storage | Stats target | Description
--------+----------------+-------------------------------------------+---------+--------------+-------------
 c1     | tb_set7_c1_set | character set GB18030 collate gb18030_bin | plain   |              |
 c2     | integer        |                                           | plain   |              |
Has OIDs: no
Options: orientation=row, compression=no

charset_biao1=# select * from tb_set7;
 c1  | c2
-----+----
 all |  1
 ALL |  2
 aLL |  3
 all |  4
(4 rows)

charset_biao1=# set b_format_behavior_compat_options = set_session_transaction;
SET
charset_biao1=# set SESSION transaction isolation level repeatable read;
SET
charset_biao1=# START TRANSACTION with consistent snapshot;
START TRANSACTION

charset_biao1=# set enable_set_variable_b_format=on;
SET
charset_biao1=# CREATE OR REPLACE PROCEDURE p_sig4() IS
charset_biao1$# BEGIN
charset_biao1$# SIGNAL SQLSTATE '02123' set CLASS_ORIGIN='aa';
charset_biao1$# END;
charset_biao1$# /
CREATE PROCEDURE
charset_biao1=# call p_sig4();
ERROR:  Unhandled user-defined not found condition
CONTEXT:  PL/pgSQL function p_sig4() line 2 at SIGNAL
charset_biao1=# get diagnostics condition 1 @p1 = CLASS_ORIGIN;
GET DIAGNOSTICS
charset_biao1=# select @p1;
 @p1
-----
 aa
(1 row)
```