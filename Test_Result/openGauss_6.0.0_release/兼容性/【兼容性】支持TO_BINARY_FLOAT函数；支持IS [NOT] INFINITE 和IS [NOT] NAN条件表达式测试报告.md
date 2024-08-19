![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者  |
| --------- | -------- | ------------ | ----- |
| 2024.8.2  | v1.0     | 测试报告初稿 | lixin |
| 2024.8.19 | v1.1     | 完善执行记录 | lixin |

**Keywords 关键词**：to_binary_float、is [not] nan、is [not] infinite

**Abstract 摘要**：主要测试to_binary_float函数、is [not] nan及is [not] infinite表达式，包含功能测试和性能测试，整体质量良好。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

本报告主要测试to_binary_float函数、is [not] nan及is [not] infinite表达式，包含功能测试和性能测试，输出测试用例79个，执行测试共3轮，发现issue共6个，包含2个资料单，均已修复并回归通过，无遗留问题，整体质量良好。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                       | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------------ | ---------- | ------------ | ------------ | -------- |
| openGauss 6.0.0 build e427efb2 | openGauss  | 2024-7-19    | 2024-8-1     | lixin    |
| openGauss 6.0.0 build eecf4e10 | openGauss  | 2024-8-2     | 2024-8-2     | lixin    |
| openGauss 6.0.0 build a04eaf40 | openGauss  | 2024-8-19    | 2024-8-19    | lixin    |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Inter(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 4核<br/>内存：8GB<br/>硬盘：220GB<br/>OS：openEuler release 20.03(LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本次特性测试to_binary_float函数、is [not] nan及is [not] infinite表达式，包含功能测试和性能测试，输出测试用例79个，执行测试共3轮，发现issue共6个，包含2个资料单，均已修复并回归通过，无遗留问题，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【兼容性】支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | 支持`TO_BINARY_FLOAT`函数功能、`IS [NOT] NAN`表达式功能和 IS [NOT] INFINITE 表达式功能 | 无                         | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

该特性对性能要求为：对比A库不劣化。测试结果如下，性能达标。

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.openGauss数据库建表并插入数据，覆盖带索引及不带索引场景，查询sql分别包含to_binary_float函数、is [not] nan及is [not] infinite表达式，循环执行10w次，记录耗时<br />2.A库数据库建表插入数据与步骤1相同，覆盖步骤1所有相同场景，循环执行10w，记录耗时，与步骤1对比 | 1.执行成功<br />2.openGauss数据库执行耗时均小于A库数据库，性能达标。 |

性能测试结果详细如下：

|   类型    | 执行次数 | to_binary_float- 不带索引 | to_binary_float- 带索引 | is [not ]nan- 不带索引 | is [not ]nan- 带索引 | is [not ]infinite- 不带索引 | is [not ]infinite- 带索引 |
| :-------: | :------: | :-----------------------: | :---------------------: | :--------------------: | :------------------: | :-------------------------: | :-----------------------: |
| opengauss |   10w    |          13.24s           |         13.38s          |         5.29s          |        5.36s         |            5.16s            |           5.46s           |
|    A库    |   10w    |          57.48s           |          59.2s          |         15.96s         |        15.76s        |           16.09s            |           16.3s           |

###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

无

###  4.2.7 兼容性测试结论

无

###  4.2.8 升级测试结论

从无该特性版本升级至该特性所在版本，升级成功；升级后数据库，执行该特性函数及表达式成功，测试通过。

## 4.3 资料测试结论

社区已增加该特性函数及表达式文档，与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [IS [NOT] NAN](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/BriefTutorial/IS-NOT-NAN%E8%AF%AD%E5%8F%A5.html) | 测试通过 |
| 2    | [IS-NOT-INFINITE](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/BriefTutorial/IS-NOT-INFINITE%E8%AF%AD%E5%8F%A5.html) | 测试通过 |
| 3    | [关键字](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/SQLReference/%E5%85%B3%E9%94%AE%E5%AD%97.html) | 测试通过 |
| 4    | [类型转换函数](https://docs-opengauss.osinfra.cn/zh/docs/latest-lite/docs/SQLReference/%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E5%87%BD%E6%95%B0.html) | 测试通过 |
| 5    | [高级数据管理](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/BriefTutorial/%E9%AB%98%E7%BA%A7%E6%95%B0%E6%8D%AE%E7%AE%A1%E7%90%86.html) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖to_binary_float函数、is [not] nan及is [not] infinite表达式的入参校验及功能实现、与其他特性交互场景及性能测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6        | 0    | 0    | 5    | 1      |
| 百分比 | 100%     | 0%   | 0%   | 83%  | 17%    |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IAEXK7](https://gitee.com/opengauss/openGauss-server/issues/IAEXK7?from=project-issue) | 【测试类型：SQL功能】【测试版本：6.0.0】兼容A库下，to_binary_float入参为0时不应返回负无穷大 | 次要     | 已验收   |
| [IAF064](https://gitee.com/opengauss/openGauss-server/issues/IAF064?from=project-issue) | 【测试类型：SQL功能】【测试版本：6.0.0】兼容A库下，to_binary_float入参为超出float8范围的值，子句默认值非法时，应报错超出float8范围 | 次要     | 已验收   |
| [IAEZ9V](https://gitee.com/opengauss/openGauss-server/issues/IAEZ9V?from=project-issue) | 【测试类型：SQL功能】【测试版本：6.0.0】兼容A库下，to_binary_float入参为float4的负边界值时不应返回负无穷大 | 次要     | 已验收   |
| [IAH9LN](https://gitee.com/opengauss/openGauss-server/issues/IAH9LN?from=project-issue) | 【测试类型：SQL功能】【测试版本：6.0.0】to_binary_float入参范围溢出，且return_expr为字符，报错与A库不一致 | 次要     | 已验收   |
| [IAH7OF](https://gitee.com/opengauss/docs/issues/IAH7OF?from=project-issue) | 【测试类型：资料】【测试版本：6.0.0】is [not] nan/infinite文档点击后404 | 次要     | 已验收   |
| [IAIQ1E](https://gitee.com/opengauss/docs/issues/IAIQ1E?from=project-issue) | 【测试类型：资料】【测试版本：6.0.0】文档地图->简易教程->高级数据管理->高级数据库管理界面，缺失is [not] nan、is [not] infinite | 不重要   | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                                                         | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| 1    | 支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | 测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE、IS [NOT] NAN条件表达式入参校验 | YES                  |
| 2    | 支持TO_BINARY_FLOAT函数                                      | 测试TO_BINARY_FLOAT函数作为select列字段，结合where condition/ order by/ group by/ having/ join/ distinct/ 子查询/ 函数嵌套/ 嵌套自身/ 索引列等场景，并覆盖带default子句场景测试 | YES                  |
| 3    | 支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式                | 测试IS [NOT] INFINITE和IS [NOT] NAN条件表达式作为select列字段/ where条件等场景测试 | YES                  |
| 4    | 支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | 测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE和IS [NOT] NAN条件表达式，结合prepare语句、自定义函数、匿名函数、视图、导入导出及通过jdbc执行该函数等场景 | YES                  |
| 5    | 支持TO_BINARY_FLOAT函数；支持IS [NOT] INFINITE和IS [NOT] NAN条件表达式 | 测试TO_BINARY_FLOAT函数、IS [NOT] INFINITE、IS [NOT] NAN条件表达式，带索引及不带索引场景下，对比A库执行10w次耗时少，验证其性能达标 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称             | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数     | 代码量 | 缺陷密度 |
| -------------------- | ---------------- | ---------- | ---------- | -------------- | ------ | -------- |
| openGauss 6.0.0 B023 | 7                | 78         | 78         | 3              | 4.237  | 0.71     |
| openGauss 6.0.0 B025 | 1                | 13         | 13         | 3（2个资料单） | 4.237  | 0.23     |
| openGauss 6.0.0 B027 | 1                | 1          | 1          | 0              | 4.237  | 0        |

本次测试共发现6个issue，包含2个资料单，均已修复并回归通过，缺陷密度为4/4.237k=0.94，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 78           | 78               | 68     | 10     | 0       | 0           | 100%   | 87%        |
| 13           | 13               | 12     | 1      | 0       | 0           | 100%   | 92%        |
| 1            | 1                | 1      | 0      | 0       | 0           | 100%   | 100%       |

本次测试共输出测试用例79个，执行测试共3轮，发现issue共6个，包含2个资料单，均已修复并回归通过，无遗留问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：https://gitee.com/opengauss/openGauss-server/pulls/5714

资料pr：https://gitee.com/opengauss/docs/pulls/6503

测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=b155ae1b338e46d6b1fa45532bf0145c&hideDevcloudHead=true



