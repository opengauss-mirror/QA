![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者        |
| :--------- | ----------- | -------------------- | ----------- |
| 2022-11-30 | 1.0         | 特性测试报告初稿完成 | zhanghuan96 |
| 2022-12-2  | 1.1         | 修改评审意见         | zhanghuan96 |

 关键词： 

mysql、dolphin插件、时间函数

摘要：

本文档主要基于插件dolphin兼容mysql的时间函数，包括MAKEDATE()、MAKETIME()、PERIOD_ADD()、PERIOD_DIFF()、SEC_TO_TIME()、SUBDATE()、SUBTIME()、TIMEDIFF()、TIME()、TIME_FORMAT()、TIMESTAMP()、TIMESTAMPADD()、TO_DAYS()、TO_SECONDS()、UNIX_TIMESTAMP()、UTC_DATE()、UTC_TIME()、UTC_TIMESTAMP()，对上述18个函数的功能、性能、资料进行测试，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容B库下，加载dolphin插件，实现对mysql的时间函数进行兼容，执行结果和mysql侧一致。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build la8d2288 | 2022-10-28   | 2022-11-6    |
| openGauss 3.1.0 build 6c83a22a | 2022-11-7    | 2022-11-16   |
| openGauss 3.1.0 build 0e2086be | 2022-11-17   | 2022-11-17   |
| openGauss 3.1.0 build 78779a1b | 2022-11-19   | 2022-11-19   |
| openGauss 3.1.0 build cd1244c5 | 2022-11-24   | 2022-11-24   |
| openGauss 3.1.0 build 28c2f32e | 2022-11-29   | 2022-11-29   |
| openGauss 3.1.0 build f23a81e7 | 2022-12-2    | 2022-12-2    |
| dolphin 1.0                    | 2022-10-28   | 2022-12-1    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300GB<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于插件dolphin兼容mysql时间函数，共计执行用例177条，主要覆盖了功能测试、性能测试及资料测试。功能测试覆盖不同入参类型、参数取值范围测试、不同时间日期输入格式测试、结合其他函数及运算符、各种类型的表及存储过程等进行测试，与mysql侧执行结果进行比对。性能测试每个函数执行十万条select语句，获取执行时间，执行5次，取平均时间。因环境存在差异，函数执行时间和mysql基本持平或比mysql更优。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现缺陷单19个，其中2个问题经plugin sig组评审不予修改，添加资料说明即可。所有缺陷均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 函数基本功能测试，包括合法与非法入参测试，通过               |
| 功能测试 | 函数参数取值范围测试，覆盖边界值测试，溢出边界的处理，通过   |
| 功能测试 | 函数入参个数测试，通过                                       |
| 功能测试 | 函数入参为时间日期类型，不同输入格式测试                     |
| 功能测试 | 函数特殊入参类型测试，通过                                   |
| 功能测试 | 函数在表中结合insert使用，通过                               |
| 功能测试 | 函数名作为关键字测试，通过                                   |
| 性能测试 | 每个函数执行十万条select语句，获取执行时间，执行五次，去除最大值和最小值，取平均执行时间。函数执行时间和mysql基本持平或比mysql更优 |
| 资料测试 | 资料描述正确，示例的执行结果成功，通过                       |

## 3.2   约束说明

- 兼容mysql5.7版本
- openGauss需使用兼容B库且含dolphin插件
- 本函数中支持的interval单位与openGauss支持的interval单位保持一致，mysql侧的interval不兼容
- 本函数中异常入参报错同mysql侧insert行为表现一致
- 返回时间表达式涉及到小数位，最多保留6位小数且小数末尾的0不会显示

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 19       | 0    | 0    | 19   | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5ZJ63](https://gitee.com/opengauss/Plugin/issues/I5ZJ63?from=project-issue) | 次要     | SUBTIME函数，入参为time类型，返回值超出[-838:59:59, 838:59:59]，预期根据符号返回极值，实际报错 | 已验收   |
| 2    | [I5ZHK4](https://gitee.com/opengauss/Plugin/issues/I5ZHK4?from=project-issue) | 次要     | subdate函数，第一参数为date或datetime及time时，第2个参数的表达式是小数，返回结果有误 | 已验收   |
| 3    | [I5ZFW4](https://gitee.com/opengauss/Plugin/issues/I5ZFW4?from=project-issue) | 次要     | sec_to_time函数，入参second为负数，超过返回TIME类型值[-838:59:59, 838:59:59]，返回边界值未带负号 | 已验收   |
| 4    | [I5ZF5B](https://gitee.com/opengauss/Plugin/issues/I5ZF5B?from=project-issue) | 次要     | period_add，period_diff函数入参均为小数，返回结果有误        | 已验收   |
| 5    | [I5ZC1K](https://gitee.com/opengauss/Plugin/issues/I5ZC1K?from=project-issue) | 次要     | maketime函数，第一个参数hour为2^32-1，返回结果和mysql不一致，预期和mysql一致返回极限值 | 已验收   |
| 6    | [I5ZATZ](https://gitee.com/opengauss/Plugin/issues/I5ZATZ?from=project-issue) | 次要     | timediff函数取值超过9999-12-31 23:59:59.999999依然支持，预期报错 | 已验收   |
| 7    | [I5ZAYD](https://gitee.com/opengauss/docs/issues/I5ZAYD?from=project-issue) | 次要     | doc仓库，dolphin-时间和日期处理函数和操作符.md文档中TIMEDIFF()函数，返回值超出[-838:59:59, 838:59:59]，函数将会报错，实际示例执行结果是返回极限值 | 已验收   |
| 8    | [I5ZVXV](https://gitee.com/opengauss/Plugin/issues/I5ZVXV?from=project-issue) | 次要     | timediff函数，入参取值超过mysql范围，仍然支持，请确认        | 已验收   |
| 9    | [I5ZV49](https://gitee.com/opengauss/Plugin/issues/I5ZV49?from=project-issue) | 次要     | timediff函数，入参为time类型，返回结果有误或不支持，请确认   | 已验收   |
| 10   | [I5ZR9W](https://gitee.com/opengauss/Plugin/issues/I5ZR9W?from=project-issue) | 次要     | TIME_FORMAT函数，时间为负数，格式化后，返回时间没有负号      | 已验收   |
| 11   | [I5ZQV4](https://gitee.com/opengauss/Plugin/issues/I5ZQV4?from=project-issue) | 次要     | 小数位超过6位且后含多个0，进位规则与mysql侧不一样，请确认    | 已验收   |
| 12   | [I5ZQHE](https://gitee.com/opengauss/Plugin/issues/I5ZQHE?from=project-issue) | 次要     | 部分函数，入参为date类型，超过msyql侧取值，使用opengauss侧最大值，未报错 | 已验收   |
| 13   | [I60GDZ](https://gitee.com/opengauss/Plugin/issues/I60GDZ?from=project-issue) | 次要     | timestampadd函数，当参数3为time类型，参数1为年、月等单位时，预期报错，实际返回time类型的极限值 | 已验收   |
| 14   | [I60G4B](https://gitee.com/opengauss/Plugin/issues/I60G4B?from=project-issue) | 次要     | timestampadd函数，第一个参数为microsecon成功，预期应该是microsecond，目前microsecon和microsecond单位均支持，请确认 | 已验收   |
| 15   | [I60PWV](https://gitee.com/opengauss/docs/issues/I60PWV?from=project-issue) | 次要     | 时间和日期处理函数中，若干处描述错误或未说明                 | 已验收   |
| 16   | [I625L9](https://gitee.com/opengauss/Plugin/issues/I625L9?from=project-issue) | 次要     | makedate函数，参数取值BIGINT类型最大值，返回结果有误         | 已验收   |
| 17   | [I61NB6](https://gitee.com/opengauss/Plugin/issues/I61NB6?from=project-issue) | 次要     | timediff函数，解析datetime类型规则和mysql不一致导致返回结果不一样 | 已验收   |
| 18   | [I60KLM](https://gitee.com/opengauss/Plugin/issues/I60KLM?from=project-issue) | 次要     | sec_to_time函数，入参为date,datetime，time类型报错，预期和mysql一致，支持隐式转换为numeric | 已验收   |
| 19   | [I62ONC](https://gitee.com/opengauss/Plugin/issues/I62ONC?from=project-issue) | 次要     | sec_to_time函数，入参为加引号的负数，返回结果有误            | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

####  4.1.1.1 函数入参取值范围及返回值测试

| 测试步骤                                                     | 测试结果                                |
| ------------------------------------------------------------ | --------------------------------------- |
| 1.兼容b库下创建表<br>2.使用函数给表插入数据，函数入参覆盖有效及无效取值<br>3.函数返回值范围验证 | 执行68条用例，发现11个bug，现已验收通过 |

#### 4.1.1.2 函数特殊入参类型测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.函数入参类型依次为布尔值，二进制，reltime，abstime类型等，如select makedate(true); | 执行20条用例，发现6个bug，现已验收通过 |

#### 4.1.1.3 函数参数个数测试

| 测试步骤                                   | 测试结果                          |
| ------------------------------------------ | --------------------------------- |
| 1.入参个数等于，小于，大于入参个数要求测试 | 执行18条用例，未发现bug，执行通过 |

#### 4.1.1.4 函数入参为时间日期类型，不同输入格式测试

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.入参为date类型，插入不同格式的date，如'YYYY-MM-DD'，'YYYYMMDD'等<br>2.入参为time类型，插入不同格式的time，如'HHMMSS'，'HH:MM:SS'，HHMMSS等<br>3.入参为datetime类型，插入不同格式的datetime，如'YYYY-MM-DD hh:mm:ss'，'YYYYMMDDhhmmss'等 | 执行25条用例，未发现bug，执行通过 |

#### 4.1.1.5 函数结合操作符测试

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.函数结合操作符或函数嵌套使用测试，如select to_days(now()); | 执行10条用例，未发现bug，执行通过 |

#### 4.1.1.6 函数结合表使用

| 测试步骤                         | 测试结果                          |
| -------------------------------- | --------------------------------- |
| 1.表中使用insert语句结合函数使用 | 执行18条用例，未发现bug，执行通过 |

### 4.1.2 性能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.构造十万条select函数查询语句<br>2.openGauss侧每个函数每轮执行十万条select语句，使用explain analyze统计执行时间，共执行五次，去除最大值和最小值，计算平均执行时间<br>3.mysql侧通过performance_schema下的event_statements_history_long表统计执行时间<br>4.比较每个函数openGauss侧与mysql侧的平均执行时间 | 执行18条用例，因环境存在差异，如CPU占用过多影响函数性能，总体性能比值不超过20%，验收通过 |

### 平均执行时间对比结果

##### 说明：数据量10万条select语句

| 函数名         | openGauss侧(s) | mysql侧(s) | 时间比值(openGauss:mysql) |
| -------------- | -------------- | ---------- | ------------------------- |
| makedate       | 1.525429       | 2.024358   | 75.35%                    |
| maketime       | 2.470498       | 2.203376   | 112.12%                   |
| period_add     | 1.493516       | 2.016556   | 74.06%                    |
| period_diff    | 1.481287       | 1.962059   | 75.5%                     |
| sec_to_time    | 1.514488       | 2.015433   | 75.14%                    |
| subdate        | 1.891649       | 2.021151   | 93.59%                    |
| subtime        | 2.001769       | 2.089783   | 95.79%                    |
| time           | 1.723120       | 1.862194   | 92.53%                    |
| time_format    | 2.014389       | 2.315681   | 87%                       |
| timediff       | 1.737437       | 2.343722   | 74.13%                    |
| timestamp      | 1.790201       | 2.008880   | 89.11%                    |
| timestampadd   | 1.866673       | 2.084010   | 89.57%                    |
| to_days        | 1.697278       | 1.851053   | 91.70%                    |
| to_seconds     | 1.757422       | 1.856587   | 94.66%                    |
| unix_timestamp | 1.898533       | 2.186371   | 86.83%                    |
| utc_date       | 1.568020       | 1.691080   | 92.72%                    |
| utc_time       | 1.603170       | 1.866107   | 85.91%                    |
| utc_timestamp  | 1.604044       | 1.739627   | 92.21%                    |

### 4.1.3 资料测试

| 测试步骤                                             | 测试结果                  |
| ---------------------------------------------------- | ------------------------- |
| 测试函数的说明文档描述是否正确，示例执行结果是否正确 | 发现2个问题，现已验收通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build la8d2288 | 70         | Passed：60<br>Failed：10 | 9            |
| openGauss 3.1.0 build 6c83a22a | 89         | Passed：80<br/>Failed：9  | 7            |
| openGauss 3.1.0 build 0e2086be | 30         | Passed：28<br/>Failed：2  | 2            |
| openGauss 3.1.0 build 78779a1b | 20         | Passed：19<br/>Failed：1 | 1            |
| openGauss 3.1.0 build f23a81e7 | 5          | Passed：5<br/>Failed：0   | 0            |

*数据项说明：*

* 累计发现缺陷单19个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：19(缺陷个数)/4.998kloc(代码行数)=3.80(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

```sql
--makedate函数示例
xxx=# select makedate(9999,31);
  makedate
------------
 9999-01-31
(1 row)

```

