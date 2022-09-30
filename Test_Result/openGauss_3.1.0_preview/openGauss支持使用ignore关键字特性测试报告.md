![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述         | 作者  |
| ---------- | -------- | ---------------- | ----- |
| 2022/09/17 | 1.0      | 特性报告初稿完成 | lixin |
| 2022/09/27 | 1.1      | 根据评审意见修改 | lixin |
| 2022/09/29 | 1.2      | 根据评审意见修改 | lixin |

关键词： `dolphin`、`ignore`、`/*+ ignore_error */`、`MySQL`、兼容性

摘要：本文档主要介绍了基于`B`库、兼容`B`库插件不同数据库下，对`insert/ update`语句中增加`ignore`关键字使用的测试，包含常见约束情况的测试，及与原有`insert/ update`语法兼容的测试，并给出最终测试结论。

缩略语清单：

| 缩略语    | 英文全名  | 中文解释          |
| --------- | --------- | ----------------- |
|NA | | |

# 1     特性概述

实现`insert/ update`语句中对`ignore`的支持，若带有`ignore`关键字的`insert/update`语句执行时在指定场景引发了Error，则会将Error降级为Warning，且继续语句的执行，不会影响其他数据的操作。 ignore关键字在对`insert/ update`语句作用时，可能会忽略当前SQL语句的更改，也可能插入当前列类型的默认零值。

# 2     特性测试信息

| 版本名称                                        | 测试起始时间 | 测试结束时间 |
| ----------------------------------------------- | ------------ | ------------ |
| openGauss 3.0.0 build a8da82a <br />dolphin 1.0 | 2022-06-20   | 2022-06-28   |
| openGauss 3.0.0 build 03cf837<br /> dolphin 1.0 | 2022-07-19   | 2022-07-29   |
| openGauss 3.1.0 build 4c9602a<br /> dolphin 1.0 | 2022-09-19   | 2022-09-22   |

| 环境信息 | 配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br/>内存：32GB<br/>硬盘：1040GB<br/>OS：CentOS Linux release 7.6.1810 (core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

共设计120个用例，主要覆盖了`insert ignore`可屏蔽错误的场景、`insert /*+ ignore_error */`可屏蔽错误的场景、`update ignore`可屏蔽错误的场景、`update /*+ ignore_error */`可屏蔽错误的场景，以及与原有语法兼容场景的测试。共计发现10个问题，9个已修复验收，1个确认非问题取消，本轮测试无遗留问题，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 测试`insert ignore`可屏蔽错误的场景                          |
| 功能测试   | 测试`insert /*+ ignore_error */`可屏蔽错误的场景             |
| 功能测试   | 测试`update ignore`可屏蔽错误的场景                          |
| 功能测试   | 测试`update /*+ ignore_error */`可屏蔽错误的场景             |
| 兼容性测试 | 测试与原有语法结合使用，可兼容                               |
| 资料测试   | 开发者指南`insert/ update`新增`ignore`使用说明，更新关键字`ignore`说明等，<br />描述准确合理，整体质量良好 |

## 3.2   约束说明

- 仅`B`库下使用，不使用dolphin插件时，使用方式为：`insert /*+ ignore_error */`、`update /*+ ignore_error */`
- `B`库下使用，且使用dolphin插件时，使用方式为：`insert ignore`、`update ignore`
-  违反非空约束时， 根据GUC参数sql_ignore_strategy的值采用以下策略的一种继续执行： 
  - sql_ignore_startegy为ignore_null时，忽略违反非空约束的行的`inser/update`操作，并继续执行剩余数据操作
  -  sql_ignore_startegy为overwrite_null时，将违反约束的null值覆写为目标类型的默认零值，并继续执行剩余数据操作

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 10       | 0    | 0    | 9    | 1      |
| 百分比 |   100%       | 0%   | 0%   | 90%  | 10%    |

### 3.3.3 问题单汇总

|序号| issue号                                                      | 级别 | 问题简述                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------- | ------ |
| 1    | [I5KC91](https://gitee.com/opengauss/openGauss-server/issues/I5KC91?from=project-issue) | 次要 | 向列表分区表使用/* + ignore_null */更新为<br />无法匹配到分区表的值，报错 duplicate key value <br />violates unique constraint in table "xxx" | 已验收 |
| 2 | [I5JEDM](https://gitee.com/opengauss/openGauss-server/issues/I5JEDM?from=project-issue) | 次要 | 使用/*+ ignore_error */向列存表中insert/update，报错未降级 | 已验收 |
| 3 | [I5HKCC](https://gitee.com/opengauss/openGauss-server/issues/I5HKCC?from=project-issue) | 次要 | 列存范围分区表含主键，主键列为分区键，<br />insert /*+ ignore_error */ into 插入分区范围内数据报错 | 已验收 |
| 4 | [I5J3F9](https://gitee.com/opengauss/openGauss-server/issues/I5J3F9?from=project-issue) | 次要 | 当sql_ignore_strategy=overwrite_null时，<br />向json类型的非空列插入null,应插入json类型的默认零值 | 已验收 |
| 5 | [I5IBOV](https://gitee.com/opengauss/openGauss-server/issues/I5IBOV?from=project-issue) | 次要 | 使用/*+ ignore_error */插入值和column类型不一致，<br />且不可转换时，应插入列类型对应的默认零值，<br />并将error降级warning | 已验收 |
| 6 | [I5HSHT](https://gitee.com/opengauss/openGauss-server/issues/I5HSHT) | 次要 | 向二级分区表insert /*+ ignore_error */ into插入<br />超过分区表范围的值，未降级error为warning | 已验收 |
| 7 | [I5ETDB](https://gitee.com/opengauss/openGauss-server/issues/I5ETDB?from=project-issue) | 次要 | 当sql_ignore_strategy=ignore_null，insert ignore <br />向非空约束的表中插入null值，实际未插入，应回显insert 0 0 | 已验收 |
| 8 | [I5ERBF](https://gitee.com/opengauss/openGauss-server/issues/I5ERBF?from=project-issue) | 次要 | 使用ignore向间隔分区表中插入null报错 | 已验收 |
| 9 | [I5CJ7S](https://gitee.com/opengauss/Plugin/issues/I5CJ7S)   | 次要   | 使用插件dolphin，sql_mode=sql_mode_strict时，<br />向int列插入“数字+字符”不应插入成功 | 已验收 |
| 10 | [I5KCGT](https://gitee.com/opengauss/Plugin/issues/I5KCGT?from=project-issue) | 不重要 | 当sql_ignore_strategy=overwrite_null时，<br />向datetime非空字段使用ignore插入null，<br />实际插入的列默认零值不正确 | 已取消 |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 测试insert使用ignore关键字可屏蔽错误的场景

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. 使用`insert ignore`、`insert /*+ ignore_error */`分别覆盖场景包括：<br />向有非空约束的列，插入`NULL`值<br />向有唯一约束的列，插入已存在的重复值<br />向有主键的列，插入`NULL`、已存在的重复值等<br />插入值无法匹配分区表<br />插入值与列类型不匹配<br />2. 查看执行结果与预期一致<br />3. 清理环境 | 执行58条用例，发现5个bug，现已修复且验收通过 |

### 4.1.2测试update使用ignore关键字可屏蔽错误的场景

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. 使用`update ignore`、`update /*+ ignore_error */`分别覆盖场景包括：<br />有非空约束的列，更新为`NULL`值<br />有唯一约束的列，更新为已存在的重复值<br />有主键的列，更新为`NULL`、已存在的重复值等<br />更新值无法匹配分区表<br />更新值与列类型不匹配<br />2. 查看执行结果与预期一致<br />3. 清理环境 | 执行42条用例，发现5个bug，现已修复且验收通过 |

### 4.1.3测试ignore关键字与原有语法兼容

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 使用ignore关键字遍历不同的表、设置不同`enable_opfusion`参数值、prepare语句、与原语法结合等，预期兼容原有语法<br />2. 清理环境 | 执行20条用例，未发现bug |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 3.0.0 build a8da82a | 120        | Passed：108<br>Failed：12 | 10           |
| openGauss 3.0.0 build 03cf837 | 回归issue：5   |issue Passed：5 | 0           |
| openGauss 3.0.0 build 4c9602a |  11<br>回归issue：4  | Passed：11<br>Failed：0<br>issue Passed：4 | 0          |


*数据项说明：*

* 累计发现缺陷单10个，9个已解决且回归通过，1个非问题已取消
* 缺陷密度为 9(缺陷个数)/9.391k(代码行数)=0.96(个/kloc)

## 4.3   后续测试建议

无

# **5     附件**

无

****

