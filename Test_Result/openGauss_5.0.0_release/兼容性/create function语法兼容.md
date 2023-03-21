![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2023-3-5 |   1.0    |           特性测试报告初稿           | zou_jialiang050 |
| 2023-3-18 | 2.0 | 增加缺陷描述 | zou_jialiang050 |

 关键词： create function

摘要：

本文对openGauss实现对兼容create function语法特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

openGauss在兼容b库下，可以实现对create function语法的兼容，其执行结果与M*侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :----------- |
| M* 5.7                        | 2023-2-16    | 2023-3-5     |
| dolphin 1.0                   | 2023-2-16    | 2023-3-5     |
| openGauss5.0.0 build 52dd7bc7 | 2023-2-16    | 2023-3-5     |
| openGauss5.0.0 build 78f2bb65 | 2023-3-13    | 2023-3-17    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz 8核<br />内存：32GB<br />硬盘：100G<br />OS：openEluer release 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss兼容create function语法的支持特性测试中，共计执行用例43个，主要涉及单个流控制语句、begin-end场景中不同流程控制语句、兼容M*侧函数选项等，共发现3个问题，现已全部回归并通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 兼容create function语法测试，通过 |
| 资料测试 | 资料叙述准确无误、示例正确，通过                            |

## 3.2   约束说明

- 兼容M* 5.7
- openGauss需在兼容b库下执行
- 使用兼容M*侧语法创建函数不支持openGauss侧原有函数选项
- 使用兼容M*侧语句创建函数后通过show create function查询得到创建语句为openGauss侧语法格式

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |  |          |  |  |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    3    |  0  |  0   |  3  |   0    |
| 百分比  (%) |   100   |  0  |  0   |  100  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
|  1  | [I6FGHH](https://e.gitee.com/opengaussorg/issues/table?issue=I6FGHH) | 次要 | 【测试类型：SQL功能】【测试版本：5.0.0】create function兼容语法创建函数主体为单个流控制语句时不支持label | 已验收 |
| 2 | [I6FR1R](https://e.gitee.com/opengaussorg/issues/table?issue=I6FR1R) | 次要 | 【测试类型：SQL功能】【测试版本：5.0.0】使用兼容create function语句创建函数指定重复函数选项报错 | 已验收 |
| 3 | [I6GXHP](https://e.gitee.com/opengaussorg/issues/table?issue=I6GXHP) | 次要 | 【测试类型：SQL功能】【测试版本：5.0.0】兼容M*侧函数语法调用错误情况下严格与非严格模式下均报错 | 已验收 |

# 4     测试执行

## 4.1   功能测试

### 4.1.1   兼容create function语法单个流控制语句验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.单个函数主体语句结合return、repeat、loop、while、case、if-else等流程控制语句进行验证 | 共执行测试用例7条<br />未发现1个bug。 |

### 4.1.2   兼容create function语法begin-end场景验证

| 测试步骤                                                     | 测试结果                          |
| :----------------------------------------------------------- | :-------------------------------- |
| 1.函数主体中包含begin-end场景结合repeat、loop、while、case、if-else等流程控制语句进行验证 | 共执行测试用例20条 未发现1个bug。 |

### 4.1.3   兼容create function语法函数选项验证

| 测试步骤                                                     | 测试结果                          |
| :----------------------------------------------------------- | :-------------------------------- |
| 1.结合M*侧函数选项comment、language sql、no sql、sql security、deterministic等参数进行验证 | 共执行测试用例16条 未发现1个bug。 |

## 4.2   资料测试

| 测试步骤                                   |                                                     |
| :------------------------------------------ | :--------------------------------------------------- |
| 1.确认资料描述是否准确无误，示例是否正确。 | 未发现bug。 |

## 4.3   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| :----------------------------- | :---------- | :----------------------- | :------------ |
| openGauss5.0.0 build 52dd7bc7 | 43   | Passed:40<br />Failed:3 | 3         |
| openGauss5.0.0 build 78f2bb65 | 3 | Passed:3<br />Failed:0 | 0 |

缺陷单共3个，修改代码量为1.25kloc， 缺陷密度为2.4个/kloc

## 4.4   后续测试建议

1.show create function支持查询创建语句在M*l侧执行

# 5     附件

## 5.1   示例

```sql
gs_testdb=# create function f_create_function_0001(s char(20)) returns char(50) return concat('Hello, ', s, '!');
CREATE FUNCTION
gs_testdb=# select f_create_function_0001('world');
 f_create_function_0001
------------------------
 Hello, world!
(1 row)

gs_testdb=# delimiter |
SET
gs_testdb=# create function f_create_function_0011(num int) returns int
gs_testdb-# no sql
gs_testdb-# begin
gs_testdb-# if num < 10 then
gs_testdb-# return num + 1;
gs_testdb-# else
gs_testdb-# return num + 20;
gs_testdb-# end if;
gs_testdb-# end|
CREATE FUNCTION
gs_testdb=# delimiter ;
SET
gs_testdb=# select f_create_function_0011(20);
 f_create_function_0011
------------------------
                     40
(1 row)

gs_testdb=# create function f_create_function_0016(s int)
gs_testdb-# returns char(50)
gs_testdb-# comment '测试函数'
gs_testdb-# deterministic
gs_testdb-# return s;
CREATE FUNCTION
gs_testdb=# select f_create_function_0016(10);
 f_create_function_0016
------------------------
 10
(1 row)

gs_testdb=# \df+ f_create_function_0016
                                                                                 List of functions
 Schema |          Name          | Result data type | Argument data types |  Type  | Volatility | Owner | Language | Source code | Description | fencedmode | propackage |
prokind
--------+------------------------+------------------+---------------------+--------+------------+-------+----------+-------------+-------------+------------+------------+-
--------
 public | f_create_function_0016 | character        | s integer           | normal | volatile   | zjl   | plpgsql  | BEGIN      +| 测试函数    | f          | f          |
f
        |                        |                  |                     |        |            |       |          | RETURN s;  +|             |            |            |
        |                        |                  |                     |        |            |       |          | END        +|             |            |            |
        |                        |                  |                     |        |            |       |          |             |             |            |            |
(1 row)
```