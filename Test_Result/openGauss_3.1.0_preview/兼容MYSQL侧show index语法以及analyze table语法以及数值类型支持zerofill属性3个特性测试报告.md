![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者        |
| :--------- | ----------- | -------------------- | ----------- |
| 2022-09-12 | 1.0         | 特性测试报告初稿完成 | zhanghuan96 |
| 2022-09-29 | 1.1         | 修改评审意见         | zhanghuan96 |

 关键词： 

M*、dolphin插件、部分语法

摘要：

本文档主要基于插件dolphin兼容M*的部分语法，包含三个特性，第一：兼容SHOW INDEX语法及ALTER TABLE相关语法；第二：兼容ANALYZE TABLE语法；第三：数值类型int/tinyint/smallint/bigint支持zerofill属性，对该三个特性进行测试，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

基于插件dolphin，实现openGauss对M*部分语法的兼容，包括查询索引信息SHOW INDEX语法、ALTER TABLE相关语法（含禁用/启用索引；删除索引；重命名索引；删除主键；删除外键；重建表）；ANALYZE TABLE相关语法；数值类型支持zerofill属性进行测试。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build b07c959b | 2022-8-22    | 2022-8-26    |
| openGauss 3.0.0 build cad66d01 | 2022-9-1     | 2022-9-1     |
| dolphin 1.0                    | 2022-8-22    | 2022-8-26    |
| openGauss 3.0.0 build cad66d01 | 2022-9-1     | 2022-9-2     |
| openGauss 3.0.0 build b4215eeb | 2022-9-9     | 2022-9-12    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300GB<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于插件dolphin兼容M*的部分语法测试，含如下三个特性

* 实现SHOW INDEX和ALTER TABLE部分语法兼容
* ANALYZE TABLE语法兼容
* 支持数值类型按照范围填0，实现对zerofill属性的支持

其中实现SHOW INDEX和ALTER TABLE部分语法兼容进行测试，共计执行51条用例，主要覆盖了功能测试和资料测试。功能测试覆盖查询索引信息，显示结果是否正确，删除索引功能可用，删除主键及外键约束功能可用，禁用/启用非唯一索引功能可用等。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

ANALYZE TABLE语法测试，共计执行10条用例，主要覆盖了功能测试和资料测试。功能测试覆盖语法是否可用、执行结果是否正确。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。未发现bug。

支持数值类型按照范围填0，实现对zerofill属性的支持测试，共计执行18条用例，主要覆盖了功能测试和资料测试，功能测试覆盖不同表类型使用数值类型并添加zerofill属性；其他数据类型添加zerofill属性等。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。未发现bug。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 编译数据库，编译dolphin插件，执行结果符合预期，通过          |
| 功能测试 | om方式安装数据库，拷贝插件依赖文件，执行结果符合预期，通过   |
| 功能测试 | SHOW INDEX/INDEXES/KEYS语法，查询索引，通过                  |
| 功能测试 | ALTER TABLE tbl_name {DISABLE\|ENABLE} KEYS禁用及启用非唯一索引，通过 |
| 功能测试 | ALTER TABLE tbl_name DROP {INDEX\|KEY} index_name删除索引，通过 |
| 功能测试 | ALTER TABLE tbl_name DROP PRIMARY KEY删除主键约束，通过      |
| 功能测试 | ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol删除外键约束，通过 |
| 功能测试 | ALTER TABLE tbl_name  FORCE重建表，通过                      |
| 功能测试 | ALTER TABLE tbl_name RENAME {INDEX\|KEY}old_index_name TO new_index_name修改索引名，通过 |
| 功能测试 | ANALYZE TABLE tbl_name收集表统计信息，通过                   |
| 功能测试 | 数值型int/tinyint/smallint/bigint/mediumint添加zerofill属性测试，通过 |
| 资料测试 | 资料描述正确，示例的执行结果成功，通过                       |

## 3.2   约束说明

#####        特性1 实现SHOW INDEX和ALTER TABLE部分语法兼容

- 兼容M*5.7版本

- openGauss需使用兼容B库且含dolphin插件

- ALTER TABLE tbl_name {DISABLE|ENABLE} KEYS语法用于禁用或启用非唯一索引，如果用于唯一索引或主键索引中不会报错，但不生效

- ALTER TABLE tbl_name {DISABLE|ENABLE} KEYS语法禁用或启用表中所有索引，无法指定某个索引

- SHOW INDEX语法查询索引，如果是表达式索引，查询结果column_name列不显示索引列名

- SHOW INDEX语法查询索引，返回结果中sub_part列和packed列目前不支持，显示为空

- ALTER TABLE tbl_name DROP {INDEX|KEY} index_name语法删除索引，若索引为主键约束或唯一约束生成的索引信息，则无法删除

- 修改索引名，旧索引名和新索引名不能一致

  ##### 特性2 ANALYZE TABLE语法兼容

- ANALYZE TABLE语句中，参数NO_WRITE_TO_BINLOG和LOCAL只用于语法兼容，无实际作用

- ANALYZE语句支持多个表，若某个表不存在，返回结果会有合理提示

- ANALYZE语句用于重建表，若对象不是表，则会弹出告警提示

  ##### 特性3 支持数值类型按照范围填0，实现对zerofill属性的支持

- 数值型int/tinyint/smallint/bigint支持zerofill属性，只做语法兼容，无实际作用

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5OJI9](https://gitee.com/opengauss/Plugin/issues/I5OJI9?from=project-issue) | 次要     | 创建ubtree索引并且添加include参数，使用SHOW INDEX查询索引信息，include字段的值也显示为索引 | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 SHOW INDEX语法测试

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.建表<br>2.创建索引，覆盖各种索引类型<br>3.使用SHOW INDEX/INDEXES/KEYS from tbl_name语句查询索引信息 | 执行21条用例，发现1个bug，现已修复且验收通过 |

### 4.1.2 ALTER TABLE tbl_name {DISABLE|ENABLE} KEYS

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表<br/>2.创建索引<br>3.执行ALTER TABLE tbl_name DISABLE KEYS禁用索引<br>4.查询索引是否禁用<br>5.执行ALTER TABLE tbl_name ENABLE KEYS启用索引<br>6.查询索引是否启用 | 执行10条用例，未发现bug，执行通过 |

### 4.1.3 ALTER TABLE tbl_name DROP {INDEX|KEY} index_name

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表<br/>2.创建索引<br/>3.执行ALTER TABLE tbl_name DROP INDEX index_name删除索引<br/>4.查询索引是否删除 | 执行10条用例，未发现bug，执行通过 |

### 4.1.4 ALTER TABLE tbl_name DROP PRIMARY KEY

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.建表，含主键约束<br/>2.执行ALTER TABLE tbl_name DROP PRIMARY KEY删除主键<br/>3.插入数据，验证主键是否删除 | 执行4条用例，未发现bug，执行通过 |

### 4.1.5 ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.建表，含外键约束<br/>2.执行ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol删除外键<br/>3.查询外键约束是否删除 | 执行2条用例，未发现bug，执行通过 |

### 4.1.6 ALTER TABLE tbl_name  FORCE

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.建表并插入数据<br>2.查询表大小<br>3.执行DML操作<br>4.执行ALTER TABLE tbl_name  FORCE语句重建表<br>5.查询表大小 | 执行1条用例，未发现bug，执行通过 |

### 4.1.7 ALTER TABLE tbl_name RENAME {INDEX|KEY} old_index_name TO new_index_name

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.建表<br/>2.创建索引<br/>3.执行ALTER TABLE tbl_name RENAME {INDEX\|KEY}old_index_name TO new_index_name修改索引名 | 执行3条用例，未发现bug，执行通过 |

### 4.1.8 ANALYZE  TABLE tbl_name

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表<br/>2.执行analyze  table [no_write_to_binglog\|local] tbl_name重建表 | 执行10条用例，未发现bug，执行通过 |

### 4.1.9 int/tinyint/smallint/bigint类型添加zerofill属性

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表，int/tinyint/smallint/bigint/mediumint类型，添加zerofill属性<br>2.建表，其他类型（不含上述类型），添加zerofill属性 | 执行18条用例，未发现bug，执行通过 |

## 4.2  测试执行统计数据

#### 特性1 实现SHOW INDEX和ALTER TABLE部分语法兼容

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build b07c959b | 51         | Passed：50<br>Failed：1 | 1            |
| openGauss 3.0.0 build cad66d01 | 1          | Passed：1<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单1个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：1(缺陷个数)/0.645kloc(代码行数)=1.55(个/kloc)

#### 特性2 ANALYZE TABLE语法兼容

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build cad66d01 | 10         | Passed：10<br>Failed：0 | 0            |

*数据项说明：*

* 未发现缺陷

#### 特性3 支持数值类型按照范围填0，实现对zerofill属性的支持

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build b4215eeb | 18         | Passed：18<br>Failed：0 | 0            |

*数据项说明：*

* 未发现缺陷

## 4.3  后续测试建议

无

# 5     附件

##  5.1 SHOW INDEX语法使用

```sql
--建表
db_replica=# create table t_grammar0001(id int not null);
CREATE TABLE
--创建唯一索引
db_replica=# create unique index id_grammar0001 on t_grammar0001(id);
CREATE INDEX
--索引添加注释
db_replica=# comment on index id_grammar0001 is '唯一索引';
COMMENT
--使用SHOW INDEX语句查询
db_replica=# SHOW INDEX from t_grammar0001;
     table     | non_unique |    key_name    | seq_in_index | column_name | collation | cardinality | sub_part | packed | null | index_type | comment | index_comment
---------------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------
 t_grammar0001 | f          | id_grammar0001 |            1 | id          | A         |             |          |        |      | btree      |         | 唯一索引
(1 row)
--使用SHOW INDEXes语句查询
db_replica=# SHOW INDEXES from t_grammar0001;
     table     | non_unique |    key_name    | seq_in_index | column_name | collation | cardinality | sub_part | packed | null | index_type | comment | index_comment
---------------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------
 t_grammar0001 | f          | id_grammar0001 |            1 | id          | A         |             |          |        |      | btree      |         | 唯一索引
(1 row)
--使用show keys语句查询
db_replica=# show keys from t_grammar0001;
     table     | non_unique |    key_name    | seq_in_index | column_name | collation | cardinality | sub_part | packed | null | index_type | comment | index_comment
---------------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------
 t_grammar0001 | f          | id_grammar0001 |            1 | id          | A         |             |          |        |      | btree      |         | 唯一索引
(1 row)

```

## 5.2 ANALYZE TABLE语法使用

```sql
--建表
xxx=# create table t_analyze0001(id int primary key);
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_analyze0001_pkey" for table "t_analyze0001"
CREATE TABLE
--执行analyze
xxx=# analyze no_write_to_binlog table t_analyze0001;
        Table         |   Op    | Msg_type | Msg_text
----------------------+---------+----------+----------
 public.t_analyze0001 | analyze | status   | OK
(1 row)
xxx=# analyze local table t_analyze0001;
        Table         |   Op    | Msg_type | Msg_text
----------------------+---------+----------+----------
 public.t_analyze0001 | analyze | status   | OK
(1 row)

```

## 5.3 zerofill属性使用

```sql
--建表，类型含zerofill属性
xxx=# create table t_zerofill0001(a int zerofill,b integer zerofill,c tinyint zerofill,d smallint zerofill,e bigint zerofill,f mediumint zerofill);
CREATE TABLE
--查询，添加zerofill属性，会自动加上unsigned标识
xxx=# \d+ t_zerofill0001
                   Table "public.t_zerofill0001"
 Column | Type  | Modifiers | Storage | Stats target | Description
--------+-------+-----------+---------+--------------+-------------
 a      | uint4 |           | plain   |              |
 b      | uint4 |           | plain   |              |
 c      | uint1 |           | plain   |              |
 d      | uint2 |           | plain   |              |
 e      | uint8 |           | plain   |              |
 f      | uint4 |           | plain   |              |
Has OIDs: no
Options: orientation=row, compression=no
--插入数据
xxx=# insert into t_zerofill0001 values(1,2,3,4,5,6);
INSERT 0 1
--查询，数值前不会补0
xxx=# select * from t_zerofill0001;
 a | b | c | d | e | f
---+---+---+---+---+---
 1 | 2 | 3 | 4 | 5 | 6
(1 row)

```

