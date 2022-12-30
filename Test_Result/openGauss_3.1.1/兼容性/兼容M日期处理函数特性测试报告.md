![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2022-12-6 |   1.0    |           特性测试报告初稿           | zou_jialiang050 |
| 2022-12-29 | 2.0 | 修改检视意见 | zou_jialiang050 |
| 2022-12-30 | 3.0 | 修改检视意见 | zou_jialiang050 |

 关键词： 日期处理函数

摘要：

本文对openGauss实现兼容M*日期处理函数特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

openGauss在兼容b库下，可以实现对`curdate()`、`current_time`、`current_time()`、`current_time(n)`、`curtime()`、`current_timestamp`、`current_timestamp()`、`current_timestamp(n)`、`localtime`、`localtime()`、`localtime(n)`、`localtimestamp`、`localtimestamp()`、`localtimestamp(n)`、`now()`、`sysdate()`、`date()`、`day()`、`dayname()`、`last_day()`、`month()`、`monthname()`、`time_to_sec()`、`week()`、`yearweek()`、`adddate()`、`addtime()`、`convert_tz()`、`date_add()`、`date_sub()`、`datediff()`、`from_days()`、`timestampdiff()`、`date_format()`、`extract()`、`get_format()`、`str_to_date()`、`from_unixtime()`函数的兼容，其执行结果与M*侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :----------- |
| M* 5.7                        | 2022-11-16   | 2022-12-6    |
| dolphin 1.0                   | 2022-11-16   | 2022-12-29   |
| openGauss3.1.0 build 51da41f7 | 2022-11-16   | 2022-11-28   |
| openGauss3.1.0 build 448e8551 | 2022-12-1    | 2022-12-20   |
| openGauss3.1.0 build fc654ee8 | 2022-12-27   | 2022-12-29   |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz 8核<br />内存：32GB<br />硬盘：100G<br />OS：openEluer release 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss兼容M*日期处理函数特性测试中，共计执行用例513个，主要覆盖功能测试及资料测试，共发现16个问题，其中14个问题单已回归通过，遗留2个问题单。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | curdate()、current_time、current_time()、current_time(n)、curtime()、current_timestamp、current_timestamp()、current_timestamp(n)、localtime、localtime()、localtime(n)、localtimestamp、localtimestamp()、localtimestamp(n)、now()、sysdate()函数功能测试，通过 |
| 功能测试 | date()、day()、dayname()、last_day()、month()、monthname()、time_to_sec()、week()、yearweek()函数功能测试，通过 |
| 功能测试 | date_format()、extract()、get_format()、str_to_date()、from_unixtime()函数功能测试，通过 |
| 功能测试 | adddate()、convert_tz()、date_add()、date_sub()、datediff()、timestampdiff()函数功能测试，通过 |
| 资料测试 | 资料叙述准确无误，示例正确，通过                             |

## 3.2   约束说明

- 兼容M* 5.7
- openGauss需在兼容b库下执行
- `last_day()`、`timestampdiff()`、`extract()`函数需设置`dolphin.b_compatibility_mode` = `True`后执行结果与M*侧执行结果一致
- `datediff()`、`timestampdiff()`函数涉及两运算日期区间包含'0000-02-28'时运算结果会比M*侧运算结果多一天或少一天
- `adddate()`、`date_add()`、`date_sub()`、函数运算结果小于'0000-01-01 00:00:00'时在严格模式下报错，非严格模式下返回null并报warning
- `INTERVAL`后不支持算数运算表达式

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    [I63EP7](https://e.gitee.com/opengaussorg/issues/table?issue=I63EP7)    | addtime函数第一参数输入time、date类型返回结果与M*侧不一致 | 次要 | 问题影响：addtime函数第一参数输入time、date类型入参运算结果与M*侧不一致<br />规避措施：无 | 已确认 |
| [I66B67](https://e.gitee.com/opengaussorg/issues/table?issue=I66B67) | adddate函数第一参数输入time类型报错 | 次要 | 问题影响：addtime函数第一参数输入time类型入参报错<br />规避措施：无 | 已确认 |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    16    |  0  |  0   |  16  |   0    |
| 百分比  (%) |   100   |  0  |  0   |  100  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
|  1  | [I62LZC](https://e.gitee.com/opengaussorg/issues/table?issue=I62LZC) | 次要 | get_format()函数第一参数输入timestamp报错 | 已验收 |
| 2 | [I62V9L](https://e.gitee.com/opengaussorg/issues/table?issue=I62V9L) | 次要 | str_to_date函数前提条件不完整 | 已验收 |
| 3 | [I62WGS](https://e.gitee.com/opengaussorg/issues/table?issue=I62WGS) | 次要 | from_unixtime函数第二参数输入%l返回错误 | 已验收 |
| 4 | [I633BQ](https://e.gitee.com/opengaussorg/issues/table?issue=I633BQ) | 次要 | extract中from后参数、adddate第一参数不带引号输入0、101报错、date_format函数第一参数不带引号、extra第二参数不带引号输入date类型边界值外结果未报错 | 已验收 |
| 5 | [I63EW6](https://e.gitee.com/opengaussorg/issues/table?issue=I63EW6) | 次要 | date_format函数第二参数输入%j、%U、%u、%V、%v、%X、%x返回值错误 | 已验收 |
| 6 | [I61YDF](https://e.gitee.com/opengaussorg/issues/table?issue=I61YDF) | 次要 | adddate(exper, interval exper unit)、date_add(exper, interval exper unit)、date_sub(exper, interval exper unit)函数第二参数不支持算数运算表达式 | 已验收 |
| 7 | [I62YR2](https://e.gitee.com/opengaussorg/issues/table?issue=I62YR2) | 次要 | extract函数第一参数为hour、minute、second、microsecond时，第二参数不带引号输入time类型报错 | 已验收 |
| 8 | [I63EP7](https://e.gitee.com/opengaussorg/issues/table?issue=I63EP7) | 次要 | addtime函数第一参数输入time、date类型返回结果与M*侧不一致 | 已确认 |
| 9 | [I649ES](https://e.gitee.com/opengaussorg/issues/table?issue=I649ES) | 次要 | convert_tz函数第二第三参数输入数字时间边界值未限制 | 已验收 |
| 10 | [I649F7](https://e.gitee.com/opengaussorg/issues/table?issue=I649F7) | 次要 | convert_tz函数第第二第三参数输入错误时区编码报错 | 已验收 |
| 11 | [I649XP](https://e.gitee.com/opengaussorg/issues/table?issue=I649XP) | 次要 | extract函数第一参数输入组合参数，第二参数输入date或time格式返回错误 | 已验收 |
| 12 | [I64AWG](https://e.gitee.com/opengaussorg/issues/table?issue=I64AWG) | 次要 | datediff函数指定time类型参数报错 | 已验收 |
| 13 | [I64AZ9](https://e.gitee.com/opengaussorg/issues/table?issue=I64AZ9) | 次要 | timestampdiff函数不带引号输入datetime类型报错 | 已验收 |
| 14 | [I64D0P](https://e.gitee.com/opengaussorg/issues/table?issue=I64D0P) | 次要 | get_format函数说明未添加timestamp支持 | 已验收 |
| 15 | [I66B2P](https://e.gitee.com/opengaussorg/issues/table?issue=I66B2P) | 次要 | extract函数第一参数输入hour minute second microsecond，第二参数不带引号输入datetime格式报错 | 已验收 |
| 16 | [I66B67](https://e.gitee.com/opengaussorg/issues/table?issue=I66B67) | 次要 | adddate函数第一参数输入time类型报错 | 已确认 |

# 4     测试执行

## 4.1   功能测试

### 4.1.1   curdate()、current_time、current_time()、current_time(n)、curtime()、current_timestamp、current_timestamp()、current_timestamp(n)、localtime、localtime()、localtime(n)、localtimestamp、localtimestamp()、localtimestamp(n)、now()、sysdate()函数功能测试

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.函数结合入参边界值、入参为算数运算表达式、dolphin.b_db_timestamp时间偏移参数、函数输出精度、函数返回值算术运算等对此部分日期函数进行测试 | 共执行测试用例34条<br />未发现问题。 |

### 4.1.2   date()、day()、dayname()、last_day()、month()、monthname()、time_to_sec()、week()、yearweek()函数功能测试

| 测试步骤                                                     | 测试结果                              |
| :----------------------------------------------------------- | :------------------------------------ |
| 1.函数结合不同入参类型、入参边界值、入参为算数运算表达式、函数嵌套、严格模式与非严格模式下函数返回表现、dolphin.default_week_format参数设置、dolphin.lc_time_names语言参数等对此部分日期函数进行测试 | 共执行测试用例208条<br />未发现问题。 |

### 4.1.3   date_format()、extract()、get_format()、str_to_date()、from_unixtime()函数功能测试

| 测试步骤                                                     | 测试结果                                           |
| :----------------------------------------------------------- | :------------------------------------------------- |
| 1.函数结合不同入参类型、入参边界值、入参为算数运算表达式、函数嵌套、严格模式与非严格模式下函数返回表现、不同unit参数、不同日期格式化参数等对此部分日期函数进行测试 | 共执行测试用例69条<br />发现9个bug，现已回归通过。 |

### 4.1.4   adddate()、convert_tz()、date_add()、date_sub()、datediff()、timestampdiff()、addtime()、from_days()函数功能测试

| 测试步骤                                                     | 测试结果                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1.函数结合不同入参类型、入参边界值、不同入参形式、函数嵌套、严格模式与非严格模式下函数返回表现、不同unit参数、不同时区转换测试等对此部分日期函数进行测试 | 共执行测试用例202条<br />发现5个bug，其中3个已回归通过，遗留2个。 |

## 4.2   资料测试

| 测试步骤                                   |                                                     |
| :------------------------------------------ | :--------------------------------------------------- |
| 1.确认资料描述是否准确无误，示例是否正确。 | 发现2个bug，现已回归通过。 |

## 4.3   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| :----------------------------- | :---------- | :----------------------- | :------------ |
| openGauss3.1.0 build 51da41f7 | 513 | Passed:497<br />Failed:16 | 16      |
| openGauss3.1.0 build 448e8551 | 9 | Passed:9<br />Failed:0 | 0 |
| openGauss3.1.0 build fc654ee8 | 5 | Passed:5<br />Failed:0 | 0 |

缺陷单共16个，项目初次提交修改代码量为11.59kloc， 代码缺陷密度为1.21个/kloc，共提交20个PR，共提交代码24.9kloc，新增文档3篇，新增开发自测用例1210条

## 4.4   后续测试建议

无

# 5     附件

## 5.1   示例

```sql
test=# select curdate();
  curdate
------------
 2022-12-28
(1 row)
test=# select localtime(6);
        localtime(6)
----------------------------
 2022-12-28 10:54:36.665232
(1 row)
test=# select date('2022-01-01');
    date
------------
 2022-01-01
(1 row)

test=# set dolphin.b_compatibility_mode = true;
SET
test=# select extract(year from '2022-01-01');
 extract
---------
    2022
(1 row)
test=# select adddate('2022-01-01', 3);
  adddate
------------
 2022-01-04
(1 row)

```