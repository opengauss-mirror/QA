![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2023-12-07 | 1.0 | 报告初稿 | xue.xiao |

关键词：查询阻止wal清理的因素

摘要：本文档为在出现xlog堆积的情况下，用来定位是否有一些东西阻止了wal的清理的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA |          |          |

# 1     特性概述

openGauss在生产环境中，经常发现xlog堆积的问题，很大情况下是数据库由于一些因素比如归档、复制槽等阻止wal清理。因此需要提供一个视图，在出现xlog堆积的情况下，用来定位是否有一些东西阻止了wal的清理。 gs_xlog_keepers主要包括keeptype keepsegment describe三列返回结果。keeptype为阻止wal清理的类型描述、keepsegment此列意义为当前限制项保护的wal文件名、describe意义为此列意义解释。

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| openGauss 5.1.1 build 6aa32485 | 2023-12-04 | 2023-12-07 |

特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 |PRETTY_NAME：CentOS Linux 7 (Core)<br />Architecture:x86_64  CPU op-mode(s):32-bit, 64-bit<br />MEM:30GB ||

# 3     测试结论概述

## 3.1   测试整体结论

openGauss执行在出现xlog堆积的情况下，用来定位是否有一些东西阻止了wal的清理，共计执行20个用例，覆盖了功能测试主要包括以下二种：1.参数验证，其主要包括无参数、有一个参数、有多个参数 2.阻止wal清理类型的验证，其主要包括Base Keep、Segments Keep、Slots Keep、Basebackup Keep、Build Keep、Unactived Wal Send Keep、Invalid Slot Keep、CBM Keep、Standby Checkpoint Keep、Archive Keep、Resist Archive。测试中发现0个问题。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 参数验证，其主要包括无参数、有一个参数、有多个参数，执行结果符合预期，通过 |
| 功能测试 | 阻止wal清理类型的验证，其主要包括Base Keep、Segments Keep、Slots Keep、Basebackup Keep、Build Keep、Unactived Wal Send Keep、Invalid Slot Keep、CBM Keep、Standby Checkpoint Keep、Archive Keep、Resist Archive，执行结果符合预期，通过 |

## 3.2   约束说明

1. 需要开启data目录下的一些参数、部分类型涉及到主备场景

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%     | 0%    | 0%  | 0%  | 0%      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
|  |   |   |   |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 安装兼容性测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 参数验证，其主要包括无参数、有一个参数、有多个参数<br />2. 阻止wal清理类型的验证，其主要包括Base Keep、Segments Keep、Slots Keep、Basebackup Keep、Build Keep、Unactived Wal Send Keep、Invalid Slot Keep、CBM Keep、Standby Checkpoint Keep、Archive Keep、Resist Archive | 执行20条用例，发现0个问题  |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 5.1.1 build 6aa32485 | 0 | Passed: 0<br />Failed: 0 | 0 | 0% |

## 4.3   后续测试建议

无
# 5     附件

无
 
