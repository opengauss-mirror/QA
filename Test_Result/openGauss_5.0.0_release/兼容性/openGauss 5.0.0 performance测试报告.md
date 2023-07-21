![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2023-06-30 | 1.0 | 报告初稿 | xue.xiao |

关键词：兼容B库，dolphin插件，Performance schema

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现Performance schema的部分视图的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现Performance schema的部分视图。兼容如下视图：
performance_schema.events_statements_current、
performance_schema.events_statements_history、
performance_schema.events_statements_summary_by_digest、
performance_schema.events_waits_current、
performance_schema.events_waits_summary_global_by_event_name、
performance_schema.file_summary_by_instance、
performance_schema.table_io_waits_summary_by_table、
performance_schema.table_io_waits_summary_by_index_usage

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2023-05-12 | 2023-05-18 |
| openGauss 5.1.0 build f3e0cb3d | 2023-06-18 | 2023-06-30 |
| dolphin 1.0 | 2023-05-12 | 2023-06-30 |

特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 |系统版本:openEuler 20.03 (LTS)<br />CPU:aarch64 64-bit Little Endian 8 Kunpeng-920<br />MEM:30GB<br />DISK:200GB ||

# 3     测试结论概述

## 3.1   测试整体结论

openGauss安装dolphin插件实现MySQL兼容Performance schema的部分视图，包括以下八种视图：1:performance_schema.events_statements_current、2:performance_schema.events_statements_history、3:performance_schema.events_statements_summary_by_digest、4:performance_schema.events_waits_current、5:performance_schema.events_waits_summary_global_by_event_name、6:performance_schema.file_summary_by_instance、
7:performance_schema.table_io_waits_summary_by_table、8:performance_schema.table_io_waits_summary_by_index_usage，共计执行74个用例，主要覆盖了功能测试、约束测试、资料测试。功能测试覆盖了该模式不支持ddl,dml等操作；每个视图对应的结构不允许修改；对视图中每个字段进行功能验证；不可以删除当前模式中的任意视图。约束测试验证了指定模式创建触发器功能的测试用例在非兼容“B”库下执行数据库可稳定运行。测试中发现4个问题，4个已解决且回归通过。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 该模式不支持ddl,dml等操作，执行结果符合预期，通过 |
| 功能测试 | 每个视图对应的结构不允许修改，执行结果符合预期，通过 |
| 功能测试 | 对视图中每个字段进行功能验证，执行结果符合预期，通过 |
| 功能测试 | 不可以删除当前模式中的任意视图，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 需在openGauss兼容B库下安装dolphin插件验证。
2. 兼容MySQL5.7版本

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4    | 0      |
| 百分比 | 100%     | 0%    | 0%  | 100%  | 0%      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I7G25D](https://gitee.com/opengauss/Plugin/issues/I7G25D?from=project-issue) | performance模式 performance_schema.table_io_waits_summary_by_index_usage;只会在阻塞完后，执行insert语句会出现有索引的表，而执行select读语句不会在此视图中出现。与doc文件描述不相符 | 次要 | 已验收 |
| [I7G1KA](https://gitee.com/opengauss/Plugin/issues/I7G1KA?from=project-issue) | performance模式 EVENTS_STATEMENTS_SUMMARY_BY_DIGEST当执行数据到100条时，再次增加执行语句则只显示之前的100条，不会显示当前执行语句 | 次要 | 已验收 |
| [I7CAAY](https://gitee.com/opengauss/Plugin/issues/I7CAAY?from=project-issue) |  doc文档中EVENTS_STATEMENTS_CURRENT视图中query_start字段以及EVENTS_STATEMENTS_SUMMARY_BY_DIGEST视图中的table,MAX_TIMMER_WAIT字段并没有没有实现出来 | 次要 | 已验收 |

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
| 1. 该模式不支持ddl,dml等操作 <br />2. 每个视图对应的结构不允许修改 <br />3. 对视图中每个字段进行功能验证 <br />4. 不可以删除当前模式中的任意视图 <br /> | 执行74条用例，发现0个问题，其他测试通过 |

### 4.1.2 约束测试

|  测试步骤 |  测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行74条用例，执行结果符合预期，测试通过 |

### 4.1.3 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br />2. 文档示例<br /> | 文档描述发现4个问题，4个已修复且验收通过 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 5.1.0 build f3e0cb3d | 74 | Passed: 74<br />Failed: 0 | 0 | 0% |

## 4.3   后续测试建议

无
# 5     附件

无
 
