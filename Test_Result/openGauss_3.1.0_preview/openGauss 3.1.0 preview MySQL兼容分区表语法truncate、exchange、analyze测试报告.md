![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-09-05 | 1.0 | 报告初稿 | zhanbiao.chen |
| 2022-09-16 | 1.1 | 完善问题状态、测试结论 | zhanbiao.chen |
| 2022-09-22 | 1.2 | 根据检视意见修改 | zhanbiao.chen |

关键词：兼容B库，dolphin插件，truncate，exchange，analyze

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表truncate、exchange、analyze语法的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA       |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表的truncate、exchange、analyze语法，使openGauss原语法和MySQL兼容语法均可正常执行。

- Truncate分区操作会删除当前分区对应的所有数据并重置数据文件

  MySQL TRUNCATE语法：

    ALTER TABLE t1 truncate PARTITION p0, p1;

    ALTER TABLE t1 truncate PARTITION all; 

  openGauss TRUNCATE语法：

    ALTER TABLE t1 truncate partition p0, truncate partition p1; 

- 交换分区可以用来交换分区表和普通表的数据，普通表和分区的数据被置换，同时普通表和分区的表空间信息被置换。

  MySQL EXCHANGE语法：

    alter table t1 exchange partition p0 with table t2 (without/with validation);

  openGauss EXCHANGE语法：

    alter table t1 exchange partition (p0) with table t2 (without/with validation);

- 分析分区语法用于收集与表内容相关的统计信息。执行计划生成器会使用这些统计数据，以确定最有效的执行计划。

  MySQL语法：

    alter table t1 analyze partition p0,p1;

    alter table t1 analyze partition all;

  openGauss语法：

    analyze wxq_part partition (p1);

# 2     特性测试信息

被测对象的版本信息和测试的时间

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-09-01 | 2022-09-16  |
| openGauss 3.1.0 build 0b50eccc |  2022-09-01 | 2022-09-16  |
| dolphin 1.0 | 2022-09-01 | 2022-09-16  |

特性测试的环境信息

| 环境信息 | 配置信息     | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容分区表truncate、exchange、analyze语法，共计执行165个用例，主要覆盖了功能测试、约束测试、备份恢复测试和资料测试。功能测试覆盖了truncate、exchange、analyze语法对合理报错场景，和对一级分区表、二级分区表执行的场景测试；约束测试验证了测试用例在非兼容"B"库下执行数据库可稳定运行；备份恢复测试验证了数据恢复后数据的完整性、正确性；资料测试验证手册是否完善、有效。测试中发现5个问题,已解决且回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | TRUNCATE语法测试，执行结果符合预期，通过 |
| 功能测试 | EXCHANGE语法测试，执行结果符合预期，通过 |
| 功能测试 | ANALYZE语法测试，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 兼容MySQL分区truncate、exchange、analyze语法需在openGauss兼容B库下安装dolphin插件验证。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA         |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 1    | 0    | 4    | 0      |
| 百分比 |          | 20%  | 0    | 80%  | 0      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I5OQZN](https://gitee.com/opengauss/Plugin/issues/I5OQZN?from=project-issue) | MySQL分区语法兼容ANALYZE导致数据库core | 严重 | 已验收 |
| [I5OQWU](https://gitee.com/opengauss/Plugin/issues/I5OQWU?from=project-issue) | MySQL分区语法兼容ANALYZE对二级分区功能支持缺陷 | 次要 | 已验收 |
| [I5OIT5](https://gitee.com/opengauss/Plugin/issues/I5OIT5?from=project-issue) | MySQL分区语法兼容EXCHANGE不支持二级分区 | 次要 | 已验收 |
| [I5OE32](https://gitee.com/opengauss/Plugin/issues/I5OE32?from=project-issue) | MySQL分区语法兼容TRUNCATE 二级分区时语法不兼容MySQL | 次要 | 已验收 |
| [I5RAMY](https://gitee.com/opengauss/Plugin/issues/I5RAMY?from=project-issue) | MySQL分区语法兼容ANALYZE对二级分区非法操作时报错信息不准确 | 次要 | 已验收 |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 TRUNCATE语法测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.测试合理报错场景<br />2.对一级分区表interval、range、list、hash执行truncate操作<br />3.对二级分区表range-range、range-list、range-hash执行truncate操作<br />4.对二级分区表list-range、list-list、list-hash执行truncate操作<br />5.对二级分区表hash-range、hash-list、hash-hash执行truncate操作 | 执行58条用例，执行结果符合预期，测试通过 |

#### 4.1.1.3 EXCHANGE语法测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.测试合理报错场景<br />2.对一级分区表interval、range、list、hash执行exchange操作<br />3.对二级分区表range-range、range-list、range-hash执行exchange操作<br />4.对二级分区表list-range、list-list、list-hash执行exchange操作<br />5.对二级分区表hash-range、hash-list、hash-hash执行exchange操作 | 执行49条用例，执行结果符合预期，测试通过 |

#### 4.1.1.4 ANALYZE语法测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.测试合理报错场景<br />2.对一级分区表interval、range、list、hash执行analyze操作<br />3.对二级分区表range-range、range-list、range-hash执行analyze操作<br />4.对二级分区表list-range、list-list、list-hash执行analyze操作<br />5.对二级分区表hash-range、hash-list、hash-hash执行analyze操作 | 执行58条用例，执行结果符合预期，测试通过 |

### 4.1.2 约束测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行165条用例，执行结果符合预期，测试通过 |

### 4.1.3 备份恢复测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.对表做truncate、exchange、analyze操作，备份数据，删除数据<br />2.恢复数据后验证数据完整性、正确性 | 结果符合预期，测试通过 |

### 4.1.4 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
| openGauss 3.1.0 build 0b50eccc | 165 | Passed: 165<br />Failed: 0 | 5 |


## 4.2   后续测试建议

无

# 5     附件

无
 
