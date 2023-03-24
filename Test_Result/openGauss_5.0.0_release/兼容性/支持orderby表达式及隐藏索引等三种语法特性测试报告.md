![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者       |
| :--------- | -------- | -------------------- | ---------- |
| 2023-03-07 | 1.0      | 特性测试报告初稿完成 | lizhuo9527 |
| 2023-03-09 | 1.1      | 修改特性测试报告     | lizhuo9527 |

关键词： 

M*、dolphin插件、`distinct`、`order by`、`invisible index`、操作符

摘要：

本文档主要基于dolphin插件兼容M*的三种语法，第一，支持`distinct`结合`order by`表达式；第二，支持隐藏索引`invisible index`；第三，支持操作符行为和M\*兼容；针对上述语法进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1	特性概述

基于dolphin插件，实现openGauss对M*的三种语法的兼容。

- 第一，支持`distinct`结合`order by`表达式，当`dolphin.sql_mode`参数中不包含`sql_mode_full_group`或设置GUC参数`behavior_compat_option`为`allow_orderby_undistinct_column`时，可以支持`distinct`结合`order by`表达式语法，支持`order by`的排序字段不在`distinct`表达式中；


- 第二，支持隐藏索引`invisible index`特性，索引可被设置为`visible`和`invisible`两种状态，参数`dolphin.optimizer_switch`可设置为`use_invisible_indexes=on/off`，当且仅当`dolphin.optimizer_switch`=`use_invisible_indexes=off`同时索引为`invisible index`时，查询计划不使用该索引；


- 第三，支持操作符行为和M\*兼容，当参数`dolphin.b_compatibility_mode`=`on`时，可以对二进制、数字类型、浮点型、字符串(set/enum)、时间日期类型及`json`类型的数据进行加减乘除运算，其运算结果的精度大于等于M\*；


# 2	特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build 8ecf040d | 2023-02-23   | 2023-03-03   |
| openGauss 5.0.0 build e94d3b43 | 2023-03-08   | 2023-03-09   |
| dolphin build 1a5da341         | 2023-02-23   | 2023-03-09   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3	测试结论概述

## 3.1 测试整体结论

基于dolphin插件兼容M*的三种语法测试，包含如下三个特性：

* 支持`distinct`结合`order by`表达式
* 支持隐藏索引`invisible index`特性
* 支持操作符行为和M\*兼容

特性一，共计执行27条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下7点：

1. `distinct`支持`order by`(单列/多列)结合表达式及`asc/desc`排序；
2. `distinct`支持`order by`排序字段不在`distinct`列表中；
3. `distinct on`不支持`order by`排序字段不在`distinct on`列表中；
4. `order by`结合`group by`/`having`等子句、嵌套查询及子查询语句等；
5. GUC参数`behavior_compat_option`为`allow_orderby_undistinct_column`时特性功能验证；
6. `dolphin.sql_mode`参数中不包含`sql_mode_full_group`时特性功能验证；
7. 行列不唯一对应的特殊情形验证；

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

未发现缺陷单，无遗留风险，整体质量良好。

特性二，共计执行32条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下9点：

1. `create table`同时指定`invisible index`或先`create table`后`create invisible index`验证；
2. `alter table`修改索引属性或添加`invisible index`；
3. 验证参数`dolphin.optimizer_switch`=`use_invisible_indexes=on/off`控制是否使用`invisible index`；
4. `select hint`设置参数值；
5. 验证多种数据类型字段上创建`invisible index`；
6. 反复更改`index`的`visible/invisible`属性，观察SQL查询计划是否执行该索引；
7. 验证系统表`pg_index`的新增字段`indisvisible`；
8. 验证`gs_dump`工具导出`invisible index`的SQL中是否包含`invisible`选项；
9. 升级测试；

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单2个，所有缺陷已解决，回归通过，无遗留风险，整体质量良好。

特性三，共计执行72条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下9点：

1. 数字、字符串、时间日期和json同类数据两两之间进行加减乘除简单四则运算；
2. 数字、字符串、时间日期和json同类数据之间进行加减乘除混合四则运算；
3. 四种数据类型交互进行多类型多符号的混合运算；
4. 混合运算与数学函数进行交互运算；
5. 混合运算与表类型交互并验证输出结果；
6. 混合运算与function、procedure、事务、视图等类型交互；
7. 开启/关闭严格模式下验证运算输出结果；
8. 验证参数`dolphin.div_precision_increment`的取值范围；
9. 验证参数`dolphin.div_precision_increment`取值对除法运算结果的影响；

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单1个，所有缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | `distinct`支持`order by`(单列/多列)结合表达式及`asc/desc`排序，执行结果符合预期，通过 |
| 功能测试 | `distinct`支持`order by`排序字段不在`distinct`列表中，执行结果符合预期，通过 |
| 功能测试 | `distinct on`不支持`order by`排序字段不在`distinct on`列表中，执行结果符合预期，通过 |
| 功能测试 | `order by`结合`group by`/`having`等子句、嵌套查询及子查询语句等，执行结果符合预期，通过 |
| 功能测试 | GUC参数`behavior_compat_option`为`allow_orderby_undistinct_column`时特性功能验证，执行结果符合预期，通过 |
| 功能测试 | `dolphin.sql_mode`参数中不包含`sql_mode_full_group`时特性功能验证，执行结果符合预期，通过 |
| 功能测试 | 行列不唯一对应的特殊情形验证，执行结果符合预期，通过         |
| 功能测试 | `create table`同时指定`invisible index`或先`create table`后`create invisible index`验证，执行结果符合预期，通过 |
| 功能测试 | `alter table`修改索引属性或添加`invisible index`，执行结果符合预期，通过 |
| 功能测试 | 验证参数`dolphin.optimizer_switch`=`use_invisible_indexes=on/off`控制是否使用`invisible index`，执行结果符合预期，通过 |
| 功能测试 | `select hint`设置参数值，执行结果符合预期，通过              |
| 功能测试 | 验证多种数据类型字段上创建`invisible index`，执行结果符合预期，通过 |
| 功能测试 | 反复更改`index`的`visible/invisible`属性，观察SQL查询计划是否执行该索引，执行结果符合预期，通过 |
| 功能测试 | 验证系统表`pg_index`的新增字段`indisvisible`，执行结果符合预期，通过 |
| 功能测试 | 验证`gs_dump`工具导出`invisible index`的SQL中是否包含`invisible`选项，执行结果符合预期，通过 |
| 功能测试 | 数字、字符串、时间日期和json同类数据两两之间进行加减乘除简单四则运算，执行结果符合预期，通过 |
| 功能测试 | 数字、字符串、时间日期和json同类数据之间进行加减乘除混合四则运算，执行结果符合预期，通过 |
| 功能测试 | 四种数据类型交互进行多类型多符号的混合运算，执行结果符合预期，通过 |
| 功能测试 | 混合运算与数学函数进行交互运算，执行结果符合预期，通过       |
| 功能测试 | 混合运算与表类型交互并验证输出结果，执行结果符合预期，通过   |
| 功能测试 | 混合运算与function、procedure、事务、视图等类型交互，执行结果符合预期，通过 |
| 功能测试 | 开启/关闭严格模式下验证运算输出结果，执行结果符合预期，通过  |
| 功能测试 | 验证参数`dolphin.div_precision_increment`的取值范围，执行结果符合预期，通过 |
| 功能测试 | 验证参数`dolphin.div_precision_increment`取值对除法运算结果的影响，执行结果符合预期，通过 |
| 功能测试 | 升级测试，执行结果符合预期，通过                             |
| 资料测试 | 资料描述正确，示例的执行无误，通过                           |

## 3.2	约束说明

- 特性1 支持`distinct`结合`order by`表达式
  - 兼容M*5.7版本
  - openGauss需使用兼容B库
  - 兼容B库不包含dolphin插件时，由GUC参数`behavior_compat_option`是否包含`allow_orderby_undistinct_column`选项控制是否支持该特性，默认关闭
  - 兼容B库包含dolphin插件时，由参数`dolphin.sql_mode`参数中是否包含`sql_mode_full_group`选项控制是否支持该特性，默认关闭
  - 当被查询表的行列不唯一对应时，执行器会先进行去重再排序，可能导致查询结果不唯一
- 特性2 支持隐藏索引`invisible index`特性
  - 兼容M*5.7版本
  - openGauss需使用兼容B库且包含dolphin插件
  - 当且仅当`dolphin.optimizer_switch`=`use_invisible_indexes=off`同时索引为`invisible index`时，查询计划不使用该索引
- 特性3 支持操作符行为和M\*兼容
  - 兼容M*5.7版本
  - openGauss需使用兼容B库且包含dolphin插件
  - 当运算结果精度低于M\*时，可以通过调整参数`extra_float_digits`的值，获得更高的计算精度

## 3.3    遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重  | 主要 | 次要  | 不重要 |
| ------ | -------- | ----- | ---- | ----- | ------ |
| 数目   | 3        | 1     | 0    | 2     | 0      |
| 百分比 | 100%     | 33.3% | 0%   | 66.7% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                    | 问题级别 | 问题简述                                                  | 问题状态 |
| ---- | ---------------------------------------------------------- | -------- | --------------------------------------------------------- | -------- |
| 1    | [I6HQB4](https://gitee.com/opengauss/Plugin/issues/I6HQB4) | 次要     | hash分区表创建local invisible索引后查询计划仍然执行该索引 | 已验收   |
| 2    | [I6HQWZ](https://gitee.com/opengauss/Plugin/issues/I6HQWZ) | 严重     | 设置参数dolphin.optimizer_switch非法取值后数据库core      | 已验收   |
| 3    | [I6J1PE](https://gitee.com/opengauss/Plugin/issues/I6J1PE) | 次要     | A兼容性下自定义运算符后创建视图报错                       | 已验收   |

# 4    测试执行

## 4.1 测试执行步骤

###  4.1.1 `distinct`支持`order by`结合表达式及`asc/desc`排序测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`order by`单列/多列结合数学函数<br>2.`order by`单列/多列结合排序函数nlssort()、convert_to()<br>3.`order by`单列/多列结合asc/desc排序 | 执行8条用例<br/>未发现bug<br/>执行通过 |

### 4.1.2 `distinct`支持`order by`排序字段不在`distinct`列表测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`order by`指定单列/多列结合数学函数<br/>2.`order by`结合数学函数及数学运算<br/>3.`order by`结合排序函数及asc/desc排序 | 执行4条用例<br/>未发现bug<br/>执行通过 |

### 4.1.3 `distinct on`不支持`order by`排序字段不在`distinct on`列表

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`order by`字段在`distinct on`中<br/>2.`order by`字段不在`distinct on`中 | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.4 `order by`结合`group by`/`having`等子句、嵌套查询等

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`order by`指定单列/多列且字段包含在`distinct`表达式中<br>2.`order by`指定asc/desc排序<br>3.`order by`指定position<br>4.`order by`结合联合查询、嵌套查询<br>5.`order by`结合`group by`/`having`/`limit`子句查询<br>6.`order by`结合group_concat()函数<br>7.`order by`结合事务、`select into` | 执行9条用例<br/>未发现bug<br/>执行通过 |

### 4.1.5 验证GUC参数`behavior_compat_option`

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.参数值包含`allow_orderby_undistinct_column`<br/>2.参数值不包含`allow_orderby_undistinct_column` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.6 验证`dolphin.sql_mode`参数

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.参数值包含`sql_mode_full_group`<br/>2.参数值不包含`sql_mode_full_group`<br>3.同时设置参数`behavior_compat_option`和`dolphin.sql_mode` | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.7 验证行列不唯一对应的特殊情形

| 测试步骤                                   | 测试结果                               |
| ------------------------------------------ | -------------------------------------- |
| 1.构造行列不唯一对应的情形，如同名不同学号 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.8 `create table`同时指定`invisible index`或先`create table`后`create invisible index`

| 测试步骤                                                     | 测试结果                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| 1.`create table`指定`primary key`/`unique`同时主键结合`auto_increment`<br>2.`create table`指定`invisible index`<br/>3.`create table`后`create index`结合多种表类型 | 执行14条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.9 `alter table`修改索引属性或添加`invisible index`

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`alter table alter index`修改索引可见性<br/>2.`alter table add constraint`添加主键和唯一约束<br/>3.`alter table add index`设置索引可见性<br>4.`alter index`修改索引可见性，合理报错 | 执行7条用例<br/>未发现bug<br/>执行通过 |

### 4.1.10 验证参数`dolphin.optimizer_switch`控制是否使用`invisible index`

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1.参数为on且`invisible index`<br/>2.参数为on且`visible index`<br/>3.参数为off且`invisible index`<br/>4.参数为off且`visible index`<br>5.参数`dolphin.optimizer_switch`的非法取值 | 执行3条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.11 `select hint`设置参数值

| 测试步骤                                            | 测试结果                               |
| --------------------------------------------------- | -------------------------------------- |
| 1.验证`select hint`设置参数值<br>2.验证参数生效范围 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.12 多种数据类型字段上创建`invisible index`

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.数字类型字段上创建`invisible index`<br/>2.字符类型字段上创建`invisible index`<br>3.结合事务创建`invisible index` | 执行3条用例<br/>未发现bug<br/>执行通过 |

### 4.1.13 更改`index`的`visible/invisible`属性，观察SQL查询计划

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.索引属性设置为`invisible`<br/>2.索引属性设置为`visible`<br/>3.索引属性设置为`invisible` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.14 验证系统表`pg_index`的新增字段`indisvisible`

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.设置`invisible index`查询系统表相应字段<br/>2.修改索引属性为`visible`查询系统表相应字段 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.15 验证`gs_dump`工具导出`invisible index`的SQL

| 测试步骤                                               | 测试结果                               |
| ------------------------------------------------------ | -------------------------------------- |
| 1.设置`invisible index`<br/>2.使用`gs_dump`工具导出SQL | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.16 升级测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1. 3.0.0版本升级至5.0.0版本<br/>2. 3.1.0版本升级至5.0.0版本<br/>3. 3.1.1版本升级至5.0.0版本<br/>4. 灰度升级<br/>5. 就地升级 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.17 同类数据两两之间进行加减乘除简单四则运算

| 测试步骤                                                     | 测试结果                                |
| ------------------------------------------------------------ | --------------------------------------- |
| 1.数字类型两两之间进行简单四则运算<br/>2.字符类型两两之间进行简单四则运算<br/>3.时间日期和json类型两两之间进行简单加减乘除运算 | 执行40条用例<br/>未发现bug<br/>执行通过 |

### 4.1.18 同类数据之间进行加减乘除混合四则运算

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.数字类型之间进行加减乘除混合四则运算<br/>2.字符类型之间进行加减乘除混合四则运算<br/>3.时间日期和json类型之间进行加减乘除混合四则运算 | 执行3条用例<br/>未发现bug<br/>执行通过 |

### 4.1.19 四种数据类型交互进行多类型多符号的混合运算

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1. 数字类型、字符类型、时间日期类型和json类型数据交互进行<br>多类型多符号的混合四则运算<br>2.使用括号改变运算优先级 | 执行5条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.20 混合运算与数学函数进行交互

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1. 多类型多符号的混合四则运算与数学函数交互运算<br/>2.使用括号改变运算优先级 | 执行6条用例<br/>未发现bug<br/>执行通过 |

### 4.1.21 混合运算与表类型交互

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1. 多类型多符号的混合四则运算与表类型交互<br>列存表、一级二级分区表、unlogged表、ustore表、segment表 | 执行6条用例<br/>未发现bug<br/>执行通过 |

### 4.1.22 混合运算与function、procedure、事务、视图等类型交互

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.混合运算结合function计算<br/>2.混合运算结合procedure计算<br/>3.混合运算结合事务输出<br/>4.混合运算结合视图输出 | 执行4条用例<br/>未发现bug<br/>执行通过 |

### 4.1.23 开启/关闭严格模式下验证运算输出结果

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.严格模式开启<br/>2.严格模式关闭<br/>3.`dolphin.b_compatibility_mode='off'` | 执行4条用例<br/>未发现bug<br/>执行通过 |

### 4.1.24 验证参数`dolphin.div_precision_increment`的取值范围

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.参数`dolphin.div_precision_increment`边界值<br/>2.参数`dolphin.div_precision_increment`其他正确取值<br/>3.参数`dolphin.div_precision_increment`错误取值 | 执行3条用例<br/>未发现bug<br/>执行通过 |

### 4.1.25 验证参数`dolphin.div_precision_increment`取值对除法运算结果的影响

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.调整参数`dolphin.div_precision_increment`取值<br/>2.验证除法运算结果的保留小数位 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.26 资料测试

| 测试步骤                             | 测试结果               |
| ------------------------------------ | ---------------------- |
| 1.资料描述是否准确、示例能否正确执行 | 未发现bug<br/>执行通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 5.0.0 build 8ecf040d | 27         | Passed：27<br>Failed：0  | 0            |
| openGauss 5.0.0 build 8ecf040d | 32         | Passed：28<br>Failed：4  | 2            |
| openGauss 5.0.0 build 8ecf040d | 72         | Passed：71<br>Failed：1  | 1            |
| openGauss 5.0.0 build e94d3b43 | 131        | Passed：131<br>Failed：0 | 0            |

数据项说明：

* 特性一缺陷密度：0(缺陷个数)/1.125kloc(代码行数)=0(个/kloc)
* 特性二缺陷密度：2(缺陷个数)/1.115kloc(代码行数)=1.79(个/kloc)
* 特性三缺陷密度：1(缺陷个数)/1.980kloc(代码行数)=0.51(个/kloc)

## 4.3  后续测试建议

-  验证其他场景下数据类型四则运算的结果精度

# 5     附件

## 5.1 支持`distinct`结合`order by`表达式

```sql
--step1: 设置参数 expect: 设置成功
set dolphin.sql_mode = 'sql_mode_strict,pipes_as_concat,ansi_quotes,no_zero_date';
test=# set dolphin.sql_mode = 'sql_mode_strict,pipes_as_concat,ansi_quotes,no_zero_date';
SET

--step2: 创建表 expect: 建表成功
drop table if exists t_orderby_0003;
test=# drop table if exists t_orderby_0003;
NOTICE:  table "t_orderby_0003" does not exist, skipping
DROP TABLE

create table t_orderby_0003
(id int,name text,subject text,score int);
test=# create table t_orderby_0003
test-# (id int,name text,subject text,score int);
CREATE TABLE

--step3: insert数据 expect: insert成功
insert into t_orderby_0003 values
(1,'Tom','English',85),(1,'Tom','Math',93),
(2,'Jack','English',74),(2,'Jack','Math',86),
(3,'John','English',94),(3,'John','Math',65),
(4,'Tim','English',63),(4,'Tim','Math',71);
test=# insert into t_orderby_0003 values
test-# (1,'Tom','English',85),(1,'Tom','Math',93),
test-# (2,'Jack','English',74),(2,'Jack','Math',86),
test-# (3,'John','English',94),(3,'John','Math',65),
test-# (4,'Tim','English',63),(4,'Tim','Math',71);
INSERT 0 8

--step4：order by表达式结合nlssort排序 expect：执行成功
select distinct name from t_orderby_0003 order by nlssort(name,'nls_sort=schinese_pinyin_m') desc;
 name
------
 Tom
 Tim
 John
 Jack
(4 rows)

--step5：order by字段不在distinct中 expect：执行成功
select distinct name from t_orderby_0003 order by score;
 name
------
 Tim
 Jack
 Tom
 John
(4 rows)

--step6：清理环境 expect：执行成功
drop table if exists t_orderby_0003;
test=# drop table if exists t_orderby_0003;
DROP TABLE
reset dolphin.sql_mode;
test=# reset dolphin.sql_mode;
RESET
```

## 5.2 支持隐藏索引`invisible index`特性

```sql
--step1: 设置参数 expect: 设置成功
set dolphin.optimizer_switch = 'use_invisible_indexes=off';
test=# set dolphin.optimizer_switch = 'use_invisible_indexes=off';
SET

--step2: create table设置单列index expect: 执行成功
drop table if exists t_invisible_0004;
create table t_invisible_0004
(col1 int,col2 numeric,col3 text
,index idx_col1(col1) invisible);
test=# drop table if exists t_invisible_0004;
NOTICE:  table "t_invisible_0004" does not exist, skipping
DROP TABLE
test=# create table t_invisible_0004
test-# (col1 int,col2 numeric,col3 text
test(# ,index idx_col1(col1) invisible);
CREATE TABLE

--step3: insert数据 expect: 执行成功
insert into t_invisible_0004 values
(generate_series(1,100000),random()*100,'abc');
test=# insert into t_invisible_0004 values
test-# (generate_series(1,100000),random()*100,'abc');
INSERT 0 100000
        
--step4: explain查询计划 expect: 不执行该索引
explain(costs false) select col1 from t_invisible_0004 where col1>=7778 and col2 < 12;
test=# explain(costs false) select col1 from t_invisible_0004 where col1>=7778 and col2 < 12;
                     QUERY PLAN
-----------------------------------------------------
 Seq Scan on t_invisible_0004
   Filter: ((col1 >= 7778) AND (col2 < 12::numeric))
(2 rows)
        
--step5: 清理环境 expect: 清理成功
drop index if exists idx_col1;
drop table if exists t_invisible_0004;
reset dolphin.optimizer_switch;
test=# drop index if exists idx_col1;
DROP INDEX
test=# drop table if exists t_invisible_0004;
DROP TABLE
test=# reset dolphin.optimizer_switch;
RESET
```

## 5.3 支持操作符行为和M\*兼容

```sql
--step1: 设置参数 expect: 设置成功
set dolphin.b_compatibility_mode = 'on';
test=# set dolphin.b_compatibility_mode = 'on';
SET

--step2: create table expect: 执行成功
drop table if exists test;
create table test
(col1 float(8),col2 numeric,col3 bit(3)
,col4 text,col5 json,col6 blob,col7 date,col8 year);
test=# drop table if exists test;
NOTICE:  table "test" does not exist, skipping
DROP TABLE
test=# create table test
test-# (col1 float(8),col2 numeric,col3 bit(3)
test(# ,col4 text,col5 json,col6 blob,col7 date,col8 year);
CREATE TABLE

--step3: insert数据 expect: 执行成功
insert into test values
(57.13,2.223,B'101','297.556','37.13','21'::blob,'1997-03-21','2022'),
(10.31,0.275,B'011','8.57','92.6','44'::blob,'2025-12-31','1985');
test=# insert into test values
test-# (57.13,2.223,B'101','297.556','37.13','21'::blob,'1997-03-21','2022'),
test-# (10.31,0.275,B'011','8.57','92.6','44'::blob,'2025-12-31','1985');
INSERT 0 2

--step4: 执行运算 expect: 执行成功
select 
col1+col8*col3/col7-col5+col4 c1,
col6-col4*col5*col8-col2/col3 c2 
from test;
test=# select
test-# col1+col8*col3/col7-col5+col4 c1,
test-# col6-col4*col5*col8-col2/col3 c2
test-# from test;
        c1         |       c2
-------------------+-----------------
  317.556507319367 | -22339549.55416
 -73.7197055241849 |     -1575216.27
(2 rows)
        
--step5: 清理环境 expect: 清理成功
drop table if exists test;
reset dolphin.b_compatibility_mode;
test=# drop table if exists test;
DROP TABLE
test=# reset dolphin.b_compatibility_mode;
RESET
```

