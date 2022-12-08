![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-10-07 | 1.0 | 报告初稿完成 | xue.xiao |
| 2022-12-08 | 1.1 | 修改报告初稿 | xue.xiao |

关键词：兼容B库，dolphin插件，rename_table

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL中的rename_table函数的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|   NA   |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL的rename_table函数。rename_table函数可以重命名一个或多个表。该语法在MySQL中支持跨库修改表名，而在opengauss中的模式与MySQL中的跨库是同一个级别，所以在opengauss中支持跨模式修改表名。

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-10-07 | 2022-11-21 |
| openGauss 3.1.0 build 79495a60 | 2022-10-07 | 2022-10-26 |
| openGauss 3.1.0 build ea7d1170 | 2022-10-26 | 2022-11-14 |
| openGauss 3.1.0 build f2a034c2 | 2022-11-14 | 2022-11-17 |
| openGauss 3.1.0 build 79495a60 | 2022-11-17 | 2022-12-05 |
| openGauss 3.1.0 build be9cfb7d | 2022-12-05 | 2022-12-05 |
| dolphin 1.0 | 2022-10-07 | 2022-12-05 |

特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 |系统版本:openEuler 20.03 (LTS)<br />CPU:aarch64 64-bit Little Endian 8 Kunpeng-920<br />MEM:30Gi ||

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容rename_table函数，共计执行73个用例，主要覆盖了功能测试、约束测试、资料测试。功能测试覆盖了同模式和跨模式修改表名、特性测试以及多场景测试；约束测试验证了rename_table功能的测试用例在非兼容“B”库下执行数据库可稳定运行。测试中发现16个问题，16个已解决且回归通过。

|  测试活动 |  活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 同模式修改表名的测试，执行结果符合预期，通过 |
| 功能测试 | 跨模式修改表名的测试，执行结果符合预期，通过 |
| 功能测试 | rename_table具备的特性测试，执行结果符合预期，通过 |
| 功能测试 | rename_table在多场景中的测试，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. rename_table函数需在openGauss兼容B库下安装dolphin插件验证。
2. 兼容MySQL5.7版本

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| [I5V5A8]( https://gitee.com/opengauss/Plugin/issues/I5V5A8?from=project-issue)        |      rename table重命名后没有更改内部生成的和用户定义的CHECK约束名称    |     次要     |         不要参考内部生成的CHECK约束名称           |     挂起     |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 16        | 0    | 0    | 16     | 0      |
| 百分比 | 100%     | 0%    | 0%    | 100%  | 0%      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I5V5A8]( https://gitee.com/opengauss/Plugin/issues/I5V5A8?from=project-issue) | rename table重命名后没有更改内部生成的和用户定义的CHECK约束名称 | 次要 | 挂起 |
| [I5V5A1](https://gitee.com/opengauss/Plugin/issues/I5V5A1?from=project-issue) | rename table中重命名之后new table表没有属于自己的type而是依赖于old table表的type | 次要 | 已验收 |
| [I5V59O](https://gitee.com/opengauss/Plugin/issues/I5V59O?from=project-issue) | rename table中对视图的重命名，在不指定模式的情况下，对视图进行重命名会报错 | 次要 | 已验收 |
| [I5V59D](https://gitee.com/opengauss/Plugin/issues/I5V59D?from=project-issue) | rename table应该不支持全局临时表的重命名，但是目前利用ename table是可以成功对全局临时表改名 | 次要 | 已验收 |
| [I5V58W](https://gitee.com/opengauss/Plugin/issues/I5V58W?from=project-issue) | 旧表名必须存在而新表名必须不存在，而使用rename table 对当前old name改名为old name成功 | 次要 | 已验收 |
| [I5WBXL](https://gitee.com/opengauss/Plugin/issues/I5WBXL?from=project-issue) | 重命名同义词之后出现的错误信息未明确指出不支持同义词的改名 | 次要 | 已验收 |
| [I5Y1Y6](https://gitee.com/opengauss/Plugin/issues/I5Y1Y6?from=project-issue) | 给普通用户赋予某一个表所有的权限，但重命名失败 | 次要 | 已验收 |
| [I5Y1XX](https://gitee.com/opengauss/Plugin/issues/I5Y1XX?from=project-issue) | 利用user创建用户，重命名时无法识别存在的表 | 次要 | 已验收 |
| [I5ZVRV](https://gitee.com/opengauss/openGauss-connector-jdbc/issues/I5ZVRV?from=project-issue?from=project-issue) | 在测试改表名时new_table_name存在的情景时，gsql执行相同sql成功，但运行YAT通过jdbc执行用例会导致宕机 | 次要 | 已验收 |
| [I61A2Y](https://gitee.com/opengauss/Plugin/issues/I61A2Y?from=project-issue) | rename table中表名不可以命名为$ | 次要 | 已验收 |
| [I61S5X](https://gitee.com/opengauss/Plugin/issues/I61S5X?from=project-issue) | 跨模式修改表名，利用cascade直接删除存在跨模式修改表名的模式名，第一个模式会删除成功，第二个会失败 | 次要 | 已验收 |
| [I61S5Y](https://gitee.com/opengauss/Plugin/issues/I61S5Y?from=project-issue) | 跨模式修改带触发器的表名会成功，其结果与MySQL不同 | 次要 | 已验收 |
| [I61Y9V](https://gitee.com/opengauss/Plugin/issues/I61Y9V?from=project-issue) | 一个用户拥有对某个表操作的所有权限，但重命名不成功 | 次要 | 已验收 |
| [I61YA4](https://gitee.com/opengauss/Plugin/issues/I61YA4?from=project-issue) | 不能实现修改后的新表名，不具备老表的权限，必须手动赋新的权限这个功能 | 次要 | 已验收 |
| [I64KZT](https://gitee.com/opengauss/Plugin/issues/I64KZT?from=project-issue) | 跨模式重命名存在外键的表不成功，与MySQL结果不一致 | 次要 | 已验收 |
| [I64L0P](https://gitee.com/opengauss/Plugin/issues/I64L0P?from=project-issue) | 对多个表用一条语句重命名，但重命名顺序是从右到左边，会宕机 | 次要 | 已验收 |
# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 入参测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 同模式修改表名的测试<br />2.跨模式修改表名的测试<br />3.rename_table具备的特性测试<br />4.rename_table在多场景中的测试<br /> | 执行73条用例，发现16个问题，16个已修复且验收通过，其他测试通过 |

### 4.1.2 约束测试

|  测试步骤 |  测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行73条用例，执行结果符合预期，测试通过 |

### 4.1.3 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br />2. 文档示例<br /> | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 3.1.0 build 79495a60 | 73 | Passed: 48<br />Failed: 25 | 16 | 21% |
| openGauss 3.1.0 build ea7d1170 | 73 | Passed: 54<br />Failed: 19 | 14 | 19% |
| openGauss 3.1.0 build f2a034c2 | 73 | Passed: 63<br />Failed: 10 | 8 | 13% |
| openGauss 3.1.0 build 79495a60 | 73 | Passed: 70<br />Failed: 3 | 3 | 4% |
| openGauss 3.1.0 build be9cfb7d | 73 | Passed: 73<br />Failed: 0 | 0 | 0% |

## 4.3   后续测试建议

无
# 5     附件

无
 
