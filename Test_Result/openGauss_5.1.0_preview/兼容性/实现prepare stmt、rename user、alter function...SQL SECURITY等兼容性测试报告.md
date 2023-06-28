![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2023-6-20 |   1.0    |           特性测试报告初稿           | zou_jialiang050 |
| 2023-6-25 | 2.0 | 增加缺陷描述 | zou_jialiang050 |

 关键词： prepare stmt、rename user、alter function

摘要：

本文对实现prepare stmt、rename user、alter function...SQL SECURITY等兼容性特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

openGauss实现对prepare stmt from、rename user、alter function sql security、export_set、from_base64、to_base64、oct、ord、substring_index、unhex、bit_count、current_user、schema、session_user、system_user、user、connection_id、bit_xor、sleep、uuid_short语法及函数的兼容、其执行结果与M*侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :----------- |
| M* 5.7                        | 2023-4-24    | 2023-6-19    |
| openGauss5.1.0 build 1c922789 | 2023-4-24    | 2023-5-12    |
| openGauss5.1.0 build 68d1772f | 2023-5-24    | 2023-5-26    |
| openGauss5.1.0 build 39f999f9 | 2023-6-6     | 2023-6-19    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br />内存：32GB<br />硬盘：100G<br />OS：CentOS Linux release 7.6.1908 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

实现prepare stmt、rename user、alter function...SQL SECURITY等兼容性特性测试中，共计执行用例255个，主要涉及函数及语法的功能验证、性能验证及文档验证，共发现18个问题，现已回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 兼容语法及函数主要功能验证，通过 |
| 性能测试 | 兼容函数性能测试，通过 |
| 资料测试 | 兼容语法及函数资料验证，通过 |

## 3.2   约束说明

- 兼容M* 5.7版本
- 此需求需在兼容B库下验证
- unhex函数在非严格模式下入参为非法制或返回结果为不可见值时算数运算不报warning
- prepare stmt from为语法兼容
- unhex函数对于不可见字符返回值默认转换为16进制
- 部分函数性能在同等条件下差于M*侧，经过sig组评定后不进行优化

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |  |          |  |  |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    18    |  0  |  0   |  18  |   0    |
| 百分比  (%) |   100   |  0  |  0   |  100  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
|  1  | [I6YFMY](https://gitee.com/opengauss/Plugin/issues/I6YFMY) | 次要 | 【测试类型：资料】【测试版本：5.1.0】bit_xor资料中描述与实际执行结果不符 | 已验收 |
|  2  | [I6YI8K](https://gitee.com/opengauss/Plugin/issues/I6YI8K) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】bit_count函数输入小数及日期类型参数处理方式错误 | 已验收 |
|  3  | [I6YNKS](https://gitee.com/opengauss/Plugin/issues/I6YNKS) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】bit_xor函数调用边界值与M*侧不同 | 已验收 |
|  4  | [I6YXOV](https://gitee.com/opengauss/Plugin/issues/I6YXOV) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】schema函数部分表现与M*侧不一致 | 已验收 |
|  5  | [I76NLB](https://gitee.com/opengauss/Plugin/issues/I76NLB) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】部分函数性能不达标 | 已验收 |
|  6  | [I70E2G](https://gitee.com/opengauss/Plugin/issues/I70E2G) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】unhex输入非法值严格模式未报错，非严格模式未报warning | 已验收 |
|  7  | [I70HEP](https://gitee.com/opengauss/Plugin/issues/I70HEP) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】sleep函数不支持date类型入参 | 已验收 |
|  8  | [I713H9](https://gitee.com/opengauss/Plugin/issues/I713H9) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】bit_xor函数入参带引号处理方式方式错误且严格模式错非严格模式未报warning | 已验收 |
|  9  | [I71FW2](https://gitee.com/opengauss/Plugin/issues/I71FW2) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】oct、ord函数边界值与M*侧不一致 | 已验收 |
|  10  | [I71HRC](https://gitee.com/opengauss/Plugin/issues/I71HRC) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】oct、ord函数输入二进制返回结果与M*侧不一致 | 已验收 |
|  11  | [I71T1G](https://gitee.com/opengauss/Plugin/issues/I71T1G) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】substring_index第三参数输入汉字、符号等严格模式下未报错，非严格模式下未报warning | 已验收 |
|  12  | [I71Z58](https://gitee.com/opengauss/Plugin/issues/I71Z58) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】from_base64函数入参输入非base64参数返回空未返回null | 已验收 |
|  13  | [I738HH](https://gitee.com/opengauss/Plugin/issues/I738HH) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】export_set函数第二、三、四参数不支持输入二进制类型 | 已验收 |
|  14  | [I73HNY](https://gitee.com/opengauss/Plugin/issues/I73HNY) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】export_set函数第一参数输入非法值严格模式下未报错，非严格模式下未报warnings | 已验收 |
| 15 | [I73WZB](https://gitee.com/opengauss/Plugin/issues/I73WZB) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】export_set函数第五参数输入小数未四舍五入处理 | 已验收 |
| 16 | [I75OFS](https://gitee.com/opengauss/Plugin/issues/I75OFS) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】export_set函数第二三四参数不支持输入布尔类型 | 已验收 |
| 17 | [I76SXJ](https://gitee.com/opengauss/Plugin/issues/I76SXJ) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】多线程向表中插入connection_id有重复id产生 | 已验收 |
| 18 | [I78IAN](https://gitee.com/opengauss/Plugin/issues/I78IAN) | 次要 | 【测试类型：SQL功能】【测试版本：5.1.0】bit_count函数入参带引号输入小数严格与非严格模式下处理不同 | 已验收 |

# 4     测试执行

## 4.1   功能测试

| 测试步骤                                                     | 测试结果                                                 |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| 1.对函数入参个数、入参类型、与其他函数交互、在存储过程及自定义函数中使用语法及函数、结合条件筛选语句等方面对语法及函数进行测试。 | 共执行测试用例239条<br />发现16个bug，现已全部回归通过。 |

## 4.2   性能测试

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.对函数在openGauss侧执行与M*侧执行进行性能对比。 | 共执行测试用例16条<br />发现1个bug，现已全部回归通过。 |

各函数性能对比数据如下表（单机数据库200000次调用结果）：

| 函数              | openGauss(s) | M*(s) | 时间比值 |
| ----------------- | ------------ | ----- | -------- |
| export_set()      | 4.50         | 5.66  | 0.80     |
| from_base64()     | 3.20         | 4.59  | 0.70     |
| oct()             | 3.32         | 4.36  | 0.76     |
| ord()             | 3.28         | 4.08  | 0.80     |
| substring_index() | 4.03         | 4.68  | 0.86     |
| to_base64()       | 3.16         | 4.18  | 0.76     |
| unhex()           | 7.97         | 4.72  | 1.69     |
| bit_count()       | 3.06         | 4.29  | 0.71     |
| current_user()    | 0.95         | 1.54  | 0.62     |
| schema()          | 1.48         | 1.54  | 0.96     |
| session_user()    | 1.10         | 1.50  | 0.73     |
| system_user()     | 1.41         | 1.42  | 0.99     |
| user()            | 1.04         | 1.49  | 0.70     |
| connection_id()   | 1.40         | 1.42  | 0.99     |
| bit_xor()         | 19.73        | 19.33 | 1.02     |
| uuid_short()      | 1.00         | 1.72  | 0.58     |

注：部分函数性能时间比值大于1，经过plugin sig例会评审后确定问题原因不在于函数，故保持现状，暂不解决。

## 4.3   资料测试

| 测试步骤                                 | 测试结果                       |
| :--------------------------------------- | :----------------------------- |
| 1.验证语法及函数资料描述及示例是否正确。 | 发现1个bug，现已全部回归通过。 |

## 4.4   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| :----------------------------- | :---------- | :----------------------- | :------------ |
| openGauss5.1.0 build 1c922789 | 255 | Passed:229<br />Failed:26 | 15      |
| openGauss5.1.0 build 68d1772f | 26 | Passed:23<br />Failed:3 | 3 |
| openGauss5.1.0 build 39f999f9 | 255 | Passed:255<br />Failed:0 | 0 |

缺陷单共18个，其中一个为资料单，修改代码量为4.54kloc， 缺陷密度为3.74个/kloc

## 4.3   后续测试建议

无

# 5     附件

```sql
gs_testdb=# select unhex(hex('123'));
 unhex
-------
 123
(1 row)

gs_testdb=# select sleep(1);
 sleep
-------
     0
(1 row)

gs_testdb=# select current_user();
 current_user
--------------
 test
(1 row)

gs_testdb=# select user();
 current_user
--------------
 test
(1 row)

gs_testdb=# select session_user();
 session_user
--------------
 test
(1 row)

gs_testdb=# select uuid_short();
    uuid_short
-------------------
 10118354740183040
(1 row)

gs_testdb=# select schema();
 schema
--------
 public
(1 row)

gs_testdb=# select to_base64(123);
 to_base64
-----------
 MTIz
(1 row)

gs_testdb=# select from_base64('MTIz');
 from_base64
-------------
 123
(1 row)

gs_testdb=# prepare test_stmt from select '1';
PREPARE
gs_testdb=# execute test_stmt;
 ?column?
----------
 1
(1 row)
```