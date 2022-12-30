![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者        |
| :--------- | ----------- | -------------------- | ----------- |
| 2022-11-1  | 1.0         | 特性测试报告初稿完成 | zhanghuan96 |
| 2022-12-21 | 1.1         | 补充特性测试         | zhanghuan96 |
| 2022-12-28 | 1.2         | 修改检视意见         | zhanghuan96 |
| 2022-12-30 | 1.3         | 修改评审意见         | zhanghuan96 |

 关键词： 

M*、dolphin插件、prepare/execute、tinyint、show privileges

摘要：

本文档主要基于插件dolphin兼容M*的四个特性测试报告，第一，兼容prepare/execute语法；第二，tinyint支持有符号，同时支持unsigned tinyint；第三，兼容show privileges语法；第四，支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型。对上述特性进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

基于插件dolphin，实现openGauss对M*部分特性兼容，包括兼容prepare/execute语法，主要实现preapre stmt from后的语句带单引号；prepare绑定参数支持`?`；execute支持using传递参数。tinyint支持有符号，同时支持unsigned tinyint，该特性主要实现tinyint类型在无符号和有符号时取值范围不同，同时该类型支持部分索引和聚集函数使用等。兼容show privileges语法主要实现使用该语句能查询到当前数据库权限相关信息。支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型，满足用户使用。

# 2     特性测试信息

| 版本名称                             | 测试起始时间 | 测试结束时间 |
| ------------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build a31f86e5       | 2022-10-28   | 2022-11-2    |
| dolphin 1.0                          | 2022-10-28   | 2022-11-2    |
| openGauss 3.1.0 build 6c83a22a       | 2022-8-22    | 2022-8-26    |
| openGauss 3.1.0 build cd1244c5       | 2022-11-7    | 2022-11-9    |
| openGauss 3.1.0 build 78779a1b       | 2022-11-24   | 2022-11-24   |
| dolphin 1.0                          | 2022-11-7    | 2022-11-9    |
| openGauss 3.1.0 build 78779a1b       | 2022-11-19   | 2022-11-20   |
| openGauss 3.1.0 build 54c43934       | 2022-12-14   | 2022-12-21   |
| openGauss 3.1.0 build 708502e9       | 2022-12-28   | 2022-12-28   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300GB<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于插件dolphin兼容M*部分特性，包含如下四个特性

* 特性1，兼容prepare/execute语法
* 特性2，tinyint支持有符号，同时支持unsigned tinyint
* 特性3，兼容show privileges语法
* 特性4，支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型

其中特性1，兼容prepare/execute语法特性测试，共计执行32条用例，主要覆盖了功能测试和资料测试。功能测试prepare stmt from 语句带引号测试；prepare绑定参数支持```?```测试；execute支持using语句测试。资料测试验证资料描述及示例。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

特性2， tinyint支持有符号，同时支持unsigned tinyint特性，共计执行35条用例，主要覆盖了功能测试和资料测试。功能测试包括tinyint取值测试；结合操作符、聚集函数、索引等测试；tinyint类型结合unsigned、zerofill属性测试；数据类型转换测试。资料测试验证资料描述及示例。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

特性3，兼容show privileges语法，共计执行7条用例，主要覆盖了功能测试和资料测试。功能测试包括不同用户执行show privileges权限测试；show privileges展示结果测试。资料测试验证资料描述及示例。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

特性4，支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型，共计执行70条用例，主要覆盖了功能测试和资料测试。功能测试包括，精度和小数位数取值范围及不同数据类型测试；类型转换测试；插入不同数据类型验证；不同表使用上述类型测试；资料测试验证资料描述及示例。累计发现缺陷单2个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | prepare stmt from带引号，测试select/insert/update/delete/merge场景，功能正常，通过 |
| 功能测试 | prepare stmt from不带引号，测试select/insert/update/delete/merge场景，功能正常，通过 |
| 功能测试 | prepare stmt from带引号，测试引号中包括多个sql，报错是否正常，通过 |
| 功能测试 | prepare stmt from带引号，测试非select/insert/update/delete/merge场景，报错是否正常，通过 |
| 功能测试 | 测试dolphin.b_compatibility_mode参数打开与关闭时，prepare绑定参数支持```?```，正常执行与合理报错，通过 |
| 功能测试 | 测试dolphin.b_compatibility_mode参数打开与关闭时,含```?```的操作符验证，通过 |
| 功能测试 | prepare绑定参数支持```?```，对比将```?```换成```$n```结果是否一致，通过 |
| 功能测试 | prepare语句结合验证单引号和```?```场景，通过                 |
| 功能测试 | prepare stmt 语句嵌套验证，通过                              |
| 功能测试 | execute支持using，测试将using的内容换成execute stmt (param)的方式对比验证，报错和结果都应保持一致，通过 |
| 功能测试 | tinyint类型带unsigned属性及不带取值范围验证，通过            |
| 功能测试 | tinyint类型结合操作符 = ，<>，<，<=，>，>=等验证，通过       |
| 功能测试 | tinyint类型结合聚合函数验证，通过                            |
| 功能测试 | tinyint类型结合索引验证，通过                                |
| 功能测试 | tinyint类型结合索引验证，通过                                |
| 功能测试 | tinyint数据类型转换验证，通过                                |
| 功能测试 | show privileges语法用户权限验证，通过                        |
| 功能测试 | show privileges显示权限验证，通过                            |
| 功能测试 | double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型取值范围测试，通过 |
| 功能测试 | double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型插入不同数据类型测试，通过 |
| 功能测试 | double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型转换测试，通过 |
| 资料测试 | 验证资料描述完整且正确，资料示例执行结果正确                 |

## 3.2   约束说明

#####        特性1，兼容prepare/execute语法

- 兼容M*5.7版本
- openGauss需使用兼容B库且含dolphin插件
- prepare中的绑定参数支持使用`?`，需要先将参数`dolphin.b_compatibility_mode`设置为`on`，且不能同时在一个语句中同时使用`$`和`?`占位符
- 参数`dolphin.b_compatibility_mode`设置为`on`后，`?`将不能作为操作符使用
- preapre stmt from后的语句带单引号的场景只能是一个query，不支持多个query
-  preapre stmt from的query，如果给的是非SELECT/INSERT等语句，和在不带引号的场景下，报错提示不一样    

##### 特性2，tinyint支持有符号，同时支持unsigned tinyint

- 兼容M*5.7版本
- openGauss需使用兼容B库且含dolphin插件

##### 特性3，兼容show privileges语法

* 兼容M*5.7版本
* openGauss需使用兼容B库且含dolphin插件

##### 特性4，支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型

* 兼容M*5.7版本
* openGauss需使用兼容B库且含dolphin插件
* 其中，double(M,D)类型，M，D支持小数，采取四舍五入的方式，float(M,D)、real(M,D)、double precision(M,D）类型不支持M，D为小数，必须为整数
* double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型均显示为numeric(M,D)类型，其中M和D的取值同numeric(M,D)一致，和mysql不同

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

##### 特性1， 兼容prepare/execute语法

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

##### 特性2，tinyint支持有符号，同时支持unsigned tinyint

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

##### 特性3，兼容show privileges语法

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

##### 特性4，支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5Y120](https://gitee.com/opengauss/docs/issues/I5Y120?from=project-issue) | 次要     | 增加描述prepare语句加单引号支持show语句，不加单引号show语句报错 | 已验收   |
| 2    | [I618SZ](https://gitee.com/opengauss/Plugin/issues/I618SZ?from=project-issue) | 次要     | tinyint有符号类型修改为无符号类型，报错提s示有误             | 已验收   |
| 3    | [I61WKA](https://gitee.com/opengauss/Plugin/issues/I61WKA?from=project-issue) | 次要     | SHOW PRIVILEGES语句显示的delete权限有误及type权限未显示内容和注释 | 已验收   |
| 4    | [I66F75](https://gitee.com/opengauss/docs/issues/I66F75?from=project-issue) | 次要     | latest文档，dolphin-数值类型章节中，double(p[,s],real(p[,s],double precision(p[,s]，类型描述错误，小数位数s不加报错 | 已验收   |
| 5    | [I66HL1](https://gitee.com/opengauss/Plugin/issues/I66HL1?from=project-issue) | 次要     | timestamp类型显式转换为smalldatetime不支持                   | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 preapre stmt from后的语句带单引号并使用execute支持using执行预备语句

| 测试步骤                                                     | 测试结果                           |
| ------------------------------------------------------------ | ---------------------------------- |
| 1.建表<br>2.创建prepare语句，语句加单引号<br>3.execute带using执行预备语句 | 执行20条用例，发现1个bug，验收通过 |

### 4.1.2 prepare绑定参数支持`?`

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.设置dolphin.b_compatibility_mode为on<br/>2.创建含`?`占位符的prepare语句<br>3.执行预备语句 | 执行10条用例，未发现bug，执行通过 |

### 4.1.3 dolphin.b_compatibility_mode参数打开与关闭验证含?的操作符

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.分别设置dolphin.b_compatibility_mode为on和off<br/>2.使用`?`相关操作符 | 执行2条用例，未发现bug，执行通过 |

### 4.1.4 tinyint/uint1类型取值范围验证

| 测试步骤                                                     | 测试结果                           |
| ------------------------------------------------------------ | ---------------------------------- |
| 1.建表，字段含tinyint及uint1类型<br>2.插入边界值及内点值验证 | 执行15条用例，发现1个bug，验证通过 |

### 4.1.5 tinyint类型结合索引、操作符、聚合函数验证

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表，tinyint类型字段创建索引<br>2.结合操作符插入数据<br>3.结合聚合函数查询 | 执行18条用例，未发现bug，执行通过 |

### 4.1.6 tinyint类型转换验证

| 测试步骤                     | 测试结果                         |
| ---------------------------- | -------------------------------- |
| 1.tinyint及uint1类型转换测试 | 执行2条用例，未发现bug，执行通过 |

### 4.1.7 show privileges显示权限验证

| 测试步骤                             | 测试结果                          |
| ------------------------------------ | --------------------------------- |
| 1.连接兼容b库执行show privileges语句 | 执行7条用例，发现1个bug，验收通过 |

### 4.1.8 double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型取值验证

| 测试步骤                                                 | 测试结果                         |
| -------------------------------------------------------- | -------------------------------- |
| 1.建表，M和D有效取值及无效取值验证<br>2.插入不同类型数据 | 执行53个用例，发现1个bug，已验收 |

### 4.1.9 double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型转换验证

| 测试步骤                         | 测试结果                        |
| -------------------------------- | ------------------------------- |
| 1.使用::或者cast函数验证类型转换 | 执行4个用例，发现1个bug，已验收 |

### 4.1.10 double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型结合索引，约束验证

| 测试步骤                                                 | 测试结果                          |
| -------------------------------------------------------- | --------------------------------- |
| 1.建表，M和D有效取值及无效取值验证<br>2.插入不同类型数据 | 执行10个用例，未发现bug，执行通过 |

## 4.2  测试执行统计数据

#### 特性1 兼容prepare/execute语法

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.1.0 build a31f86e5 | 32         | Passed：31<br>Failed：1 | 1            |
| openGauss 3.1.0 build cd1244c5 | 1          | Passed：1<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单1个，已验收通过，缺陷为资料单
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：0(缺陷个数)/1.182kloc(代码行数)=0(个/kloc)

#### 特性2 tinyint支持有符号，同时支持unsigned tinyint

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.1.0 build 6c83a22a | 35         | Passed：33<br>Failed：2 | 1            |
| openGauss 3.1.0 build cd1244c5 | 2          | Passed：2<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单1个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：1(缺陷个数)/4.407kloc(代码行数)=0.23(个/kloc)

#### 特性3 兼容show privileges语法

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.1.0 build 78779a1b | 7          | Passed：6<br>Failed：1  | 1            |
| openGauss 3.1.0 build f23a81e7 | 7          | Passed：7<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单1个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：1(缺陷个数)/0.173kloc(代码行数)=5.78(个/kloc)

#### 特性4 支持double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.1.0 build 54c43934 | 70         | Passed：62<br>Failed：8 | 2            |
| openGauss 3.1.0 build 708502e9 | 8          | Passed：8<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单2个，已验收通过，一个为资料单，一个为其他特性引入的缺陷
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：0(缺陷个数)/0.197kloc(代码行数)=0(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

##  5.1 prepare/execute语法使用

```sql
--设置参数
xxx=# set dolphin.b_compatibility_mode=on;
SET
--建表
xxx=# create table t_prepare0001(a int,b text,c bool,d numeric);
CREATE TABLE
--创建预备语句
xxx=# prepare pre_grammar0001 as 'insert t_prepare0001 values(?,?,?,?)';
PREPARE
--执行
xxx=# execute pre_grammar0001 using 10,'hi',true,12.566;
INSERT 0 1
--查询表
xxx=# select * from t_prepare0001;
 a  | b  | c | d
----+----+---+----
 10 | hi | t | 13
(1 row)
```

## 5.2 tinyint支持有符号，同时支持unsigned tinyint

```sql
--建表
xxx=# create table t_tinyint0001(c1 tinyint,c2 uint1);
CREATE TABLE
--插入数据
xxx=# insert t_tinyint0001 values(-128,0);
INSERT 0 1
xxx=# insert t_tinyint0001 values(127,255);
INSERT 0 1
--超过取值范围，报错
xxx=# insert t_tinyint0001 values(255,-128);
ERROR:  tinyint out of range
CONTEXT:  referenced column: c1
--查询
xxx=# select * from t_tinyint0001;
  c1  | c2
------+-----
 -128 | 0
 127  | 255
(2 rows)
```

## 5.3 兼容show privileges语法

```sql
--查询当前数据库权限信息
xxx=# show privileges;
      Privilege       |                                                  Context                                                   |                              Comment

----------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------------------
------------------
 Alter                | Large object,Sequence,Database,Foreign Server,Function,Node group,Schema,Tablespace,Type,Directory,Package | To alter the 'objects'
 Alter any index      | Index                                                                                                      | To alter any index
 Alter any sequence   | Sequence                                                                                                   | To alter any sequence
 Alter any table      | Table                                                                                                      | To alter any table
 Alter any trigger    | Trigger                                                                                                    | To alter any trigger
 Alter any type       | Type                                                                                                       | To alter any type
 Comment              | Table                                                                                                      | To comment on table

```

## 5.4 兼容double(M,D)、float(M,D)、real(M,D)、double precision(M,D)类型

```sql
--建表
xxx=# create table test_1(a float(20, 2), b double(20, 2), c real(20, 2), d double precision(20, 2));
CREATE TABLE
--查询表结构
xxx=# desc test_1;
 Field |     Type      | Null | Key | Default | Extra
-------+---------------+------+-----+---------+-------
 a     | numeric(20,2) | NO   |     | NULL    |
 b     | numeric(20,2) | NO   |     | NULL    |
 c     | numeric(20,2) | NO   |     | NULL    |
 d     | numeric(20,2) | NO   |     | NULL    |
(4 rows)
--插入数据
xxx=# insert test_1 values(0.014, 0.014, 0.014, 0.014), (0.015, 0.015, 0.015, 0.015), (0.0151, 0.0151, 0.0151, 0.0151),(0.016, 0.016, 0.016, 0.016);
INSERT 0 4
--查询
xxx=# select * from test_1;
  a   |  b   |  c   |  d
------+------+------+------
 0.01 | 0.01 | 0.01 | 0.01
 0.02 | 0.02 | 0.02 | 0.02
 0.02 | 0.02 | 0.02 | 0.02
 0.02 | 0.02 | 0.02 | 0.02
(4 rows)
```