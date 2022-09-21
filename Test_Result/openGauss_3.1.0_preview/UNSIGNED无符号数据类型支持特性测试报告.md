![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者      |
| ---------- | ----------- | -------------------- | --------- |
| 2022-09-21 | 1.0         | 特性测试报告初稿完成 | zhanghang |
|            |             |                      |           |

 关键词： 

int；bigint；tinyint；smallint；unsigned

摘要：

本文档主要对openGauss实现对int/bigint/tinyint/smallint四种数据类型的unsigned属性进行测试，并给出测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| N/A    |          |          |

# 1 特性概述

本次特性实现对int/bigint/tinyint/smallint四种数据类型的unsigned属性的支持，内部使用uint1 、uint2、uint4、uint8，对应mysql数据库中tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned，用户在使用dolphin插件时可以使用这些特性。

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build 0b50eccc | 2022-09-07   | 2022-09-20   |
| openGauss 3.1.0 build 075791e0 | 2022-09-21   | 2022-09-21   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：32GB<br />硬盘：100GB<br />OS：CentoOS Linux release 7.6.1810 (Core) |      |

# 3  测试结论概述

## 3.1 测试整体结论

实现对int/bigint/tinyint/smallint四种数据类型的unsigned属性的支持共计执行用例30条，主要覆盖了功能测试和资料测试。功能测试主要覆盖以下方面：

1.验证tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型的取值范围、有效值、无效值和特殊值

2.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与其他数据类型之间的显式转换和隐式转换

3.在表、视图、存储过程、自定义函数中使用tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型

4.在tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型上创建索引

5.验证和操作符的交互

6.验证和查询语句及聚集函数的交互

7.验证和sql_mode的交互

8.验证是否为关键字

资料测试覆盖资料描述是否准确、约束覆盖是否全面以及示例是否正确。累计发现缺陷单5个，2个问题单已修复且回归通过，3个问题单目前状态已完成，未转测。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 验证tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型的取值范围、有效值、无效值和特殊值，通过 |
| 功能测试 | tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与其他数据类型之间的显式转换和隐式转换，通过 |
| 功能测试 | 在表、视图、存储过程、自定义函数中使用tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型，通过 |
| 功能测试 | 在tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型上创建索引 |
| 功能测试 | 验证和操作符的交互，通过                                     |
| 功能测试 | 验证和查询语句及聚集函数的交互，通过                         |
| 功能测试 | 验证和sql_mode的交互，通过                                   |
| 功能测试 | 验证是否为关键字，通过                                       |
| 资料测试 | 资料描述是否准确、约束覆盖是否全面以及示例是否正确，通过     |

## 3.2 约束说明

- 需要在兼容B库下加载dolphin插件才能使用
- tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型仅支持行存模式，不支持列存模式和MOT
- openGauss插入null值时显示为空，可以通过is null判断是否为null
- tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型仅支持btree、hash索引

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
| 1    | [I5RAVQ](https://gitee.com/opengauss/Plugin/issues/I5RAVQ?from=project-issue) | 次要     | BIGINT UNSIGNED类型插入空值，返回结果报错；使用ignore参数后error未降级为warning | 已验收   |
| 2    | [I5RC16](https://gitee.com/opengauss/Plugin/issues/I5RC16?from=project-issue) | 次要     | BIGINT UNSIGNED类型插入浮点数，返回结果与预期结果不符        | 已验收   |
| 3    | [I5RSK0](https://gitee.com/opengauss/Plugin/issues/I5RSK0?from=project-issue) | 次要     | 将负数显式转换为text类型后插入UNSIGNED类型列，结果和预期不符 | 已完成   |
| 4    | [I5RTAI](https://gitee.com/opengauss/Plugin/issues/I5RTAI?from=project-issue) | 次要     | 将不同超过最大位数的负数显式转换为float类型后再转换为BIGINT UNSIGNED（uint8）类型，结果不一致 | 已完成   |
| 5    | [I5RVPO](https://gitee.com/opengauss/Plugin/issues/I5RVPO?from=project-issue) | 次要     | 使用ignore参数将超范围的数显式转换为text类型后插入BIGINT UNSIGNED类型列，结果和预期不符 | 已完成   |



# 4 测试执行

## 4.1 功能测试

#### 4.1.1 验证tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型的取值范围、有效值、无效值和特殊值

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.验证tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型的取值范围、有效值、无效值和特殊值 | 执行4条用例，发现2个bug，2个已修复且验收通过 |

#### 4.1.2 tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与其他数据类型之间的显式转换和隐式转换

| 测试步骤                                                     | 测试结果                                           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 1.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与数值类型之间的显式转换和隐式转换<br />2.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与货币类型之间的显式转换和隐式转换<br />3.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与布尔类型之间的显式转换和隐式转换<br />4.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与二进制类型之间的显式转换和隐式转换<br />5.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与日期/时间类型之间的显式转换和隐式转换<br />6.tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned与浮点类型之间的显式转换和隐式转换 | 执行9条用例，发现3个bug，3个目前状态已完成，未转测 |

#### 4.1.3 在表、视图、存储过程、自定义函数中使用tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型

| 测试步骤：                                                   | 测试结果                              |
| ------------------------------------------------------------ | ------------------------------------- |
| 1.在表、视图、存储过程、自定义函数中使用tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型 | 执行10条用例，未发现bug，结果符合预期 |

#### 4.1.4 在tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型上创建索引

| 测试步骤：                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.创建包含tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned数据类型的表<br />2.在tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned列上分别创建不同类型的索引 | 执行1条用例，未发现bug，结果符合预期 |

### 4.1.5 验证和操作符的交互

| 测试步骤：           | 测试结果                             |
| -------------------- | ------------------------------------ |
| 1.验证和操作符的交互 | 执行3条用例，未发现bug，结果符合预期 |

### 4.1.6 验证和查询语句及聚集函数的交互

| 测试步骤：                       | 测试结果                             |
| -------------------------------- | ------------------------------------ |
| 1.验证和查询语句及聚集函数的交互 | 执行1条用例，未发现bug，结果符合预期 |

### 4.1.7 验证和sql_mode的交互

| 测试步骤：             | 测试结果                             |
| ---------------------- | ------------------------------------ |
| 1.验证和sql_mode的交互 | 执行1条用例，未发现bug，结果符合预期 |

### 4.1.8 验证是否为关键字

| 测试步骤：         | 测试结果                             |
| ------------------ | ------------------------------------ |
| 1.验证是否为关键字 | 执行1条用例，未发现bug，结果符合预期 |

## 4.2  资料测试

| 测试步骤：                                           | 测试结果        |
| ---------------------------------------------------- | --------------- |
| 1.资料描述是否准确、约束覆盖是否全面以及示例是否正确 | 未发现bug，通过 |

## 4.3 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build 0b50eccc | 30         | Passed：24<br />Failed：6 | 5            |
| openGauss 3.1.0 build 075791e0 | 30         | Passed：30<br />Failed：0 | 0            |

数据说明

1.  测试过程中共发现5个bug，2个问题单均已修复且回归通过，3个问题单目前状态已完成，未转测
2.  缺陷密度：5个（缺陷个数）/35.103kloc（代码行数）=0.1个/kloc

## 4.4 后续测试建议

1. 关注后续是否实现以字符串形式的数值进行显式转换和隐式转换

# 5 附件

## 5.1 验证tinyint unsigned、smallint unsigned、int unsigned、bigint unsigned类型的取值范围示例

```
--step1:兼容B库中建表包含四种unsigned数据类型; expect:建表成功
testdb=# drop table if exists t_unsigned_0001;
NOTICE:  table "t_unsigned_0001" does not exist, skipping
DROP TABLE
testdb=# create table t_unsigned_0001 (
testdb(# col01 tinyint unsigned,
testdb(# col02 smallint unsigned,
testdb(# col03 int unsigned,
testdb(# col04 bigint unsigned
testdb(# );
CREATE TABLE
--step2:各列分别插入0和相应数据类型的最大值; expect:数据插入成功
testdb=# insert into t_unsigned_0001 values(0,0,0,0);
INSERT 0 1
testdb=# insert into t_unsigned_0001 values(255,65535,4294967295,18446744073709551615);
INSERT 0 1
--step3:各列分别依次插入负值; expect:合理报错，相应unsigned类型超范围
ERROR:  tinyint unsigned out of range
CONTEXT:  referenced column: col01
testdb=# insert into t_unsigned_0001 values(255,-5,4294967295,18446744073709551615);
ERROR:  smallint unsigned out of range
CONTEXT:  referenced column: col02
testdb=# insert into t_unsigned_0001 values(128,561,-51,98495946);
ERROR:  int unsigned out of range
CONTEXT:  referenced column: col03
testdb=# insert into t_unsigned_0001 values(255,397,4294967295,-184467);
ERROR:  bigint unsigned out of range
CONTEXT:  referenced column: col04
--step4:各列分别依次插入大于相应数据类型最大值的数据; expect:合理报错，相应unsigned类型超范围
testdb=# insert into t_unsigned_0001 values(256,0,0,0);
ERROR:  tinyint unsigned out of range
CONTEXT:  referenced column: col01
testdb=# insert into t_unsigned_0001 values(0,65536,0,0);
ERROR:  smallint unsigned out of range
CONTEXT:  referenced column: col02
testdb=# insert into t_unsigned_0001 values(0,0,4294967296,0);
ERROR:  int unsigned out of range
CONTEXT:  referenced column: col03
testdb=# insert into t_unsigned_0001 values(0,0,0,18446744073709551616);
ERROR:  bigint unsigned out of range
CONTEXT:  referenced column: col04
--step5:清理环境; expect:清理成功
testdb=# drop table if exists t_unsigned_0001;
DROP TABLE
```

