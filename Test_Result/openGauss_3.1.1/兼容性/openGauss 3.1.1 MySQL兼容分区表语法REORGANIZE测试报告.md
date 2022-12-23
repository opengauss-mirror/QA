![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-12-16 | 1.0   | 报告初稿 | zhanbiao.chen |
| 2022-12-22 | 1.1   | 补充完善 | zhanbiao.chen |


关键词：兼容B库，dolphin插件，REORGANIZE

 
摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表重组织分区语法(REORGANIZE)的测试报告。


缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表重组织分区的语法，对一级、二级分区表进行分区重组织的功能。

- MySQL语法：
```sql
  ALTER TABLE members
    REORGANIZE PARTITION p0 INTO (
    PARTITION n0 VALUES LESS THAN (1970),
    PARTITION n1 VALUES LESS THAN (1980)
  );
```
# 2     特性测试信息

被测对象的版本信息和测试的时间

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-11-10 | 2022-12-22  |
| openGauss 3.1.0 build 152c968e | 2022-11-10 | 2022-12-16 |
| openGauss 3.1.0 build 7924440a | 2022-12-17 | 2022-12-22 |
| dolphin 1.0 | 2022-11-10 | 2022-12-22  |

特性测试的环境信息

| 环境信息 |  配置信息    | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容分区表重组织分区语法，共计执行了68个用例，主要覆盖了功能测试、约束测试、备份恢复测试和资料测试。功能测试覆盖了合理报错场景测试，一级分区表（4种）和二级分区表（9种）的分区重组织测试；约束测试验证了功能测试用例在非兼容"B"库下执行数据库可稳定运行；备份恢复测试验证了数据恢复后数据的完整性、正确性；资料测试手册是否完善、有效。测试中发现8个问题,已解决且回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 合理报错场景测试，执行结果符合预期，通过 |
| 功能测试 | 不支持的分区类型执行重组织合理报错场景测试，执行结果符合预期，通过 |
| 功能测试 | 一级分区表range、list重组织分区测试，执行结果符合预期，通过 |
| 功能测试 | 二级分区表range-hash、list-hash重组织分区测试，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 备份恢复测试 | 逻辑备份逻辑恢复、物理备份以及物理恢复，符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 兼容MySQL分区重组织语法需在openGauss兼容B库下安装dolphin插件验证。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA         |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 8        | 2    | 2    |  3   | 1      |
| 百分比 | 100%     | 25%  | 25%  | 38%  | 12%    |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I627LM](https://gitee.com/opengauss/Plugin/issues/I627LM?from=project-issue) | MySQL兼容分区语法REORGANIZE，不支持REORGANIZE的二级分区类型执行结果与期望不一致 | 次要 | 已验收 |
| [I61YXD](https://gitee.com/opengauss/Plugin/issues/I61YXD?from=project-issue) | MySQL兼容分区语法REORGANIZE，重组织hash二级分区为其它类型分区应报错，实际可执行成功| 主要 | 已验收 |
| [I61WBW](https://gitee.com/opengauss/Plugin/issues/I61WBW?from=project-issue) | MySQL兼容分区语法REORGANIZE，二级分区表带local索引时执行REORGANIZE失败 | 主要 | 已验收 |
| [I61H3M](https://gitee.com/opengauss/Plugin/issues/I61H3M?from=project-issue) | MySQL兼容分区语法REORGANIZE，不支持range分区列存表、压缩表 | 次要 | 已验收 |
| [I61FNX](https://gitee.com/opengauss/Plugin/issues/I61FNX?from=project-issue) | MySQL兼容分区语法REORGANIZE，间隔分区表报错信息歧义 | 不重要 | 已验收 |
| [I64QX8](https://gitee.com/opengauss/Plugin/issues/I64QX8?from=project-issue) | MySQL兼容分区语法REORGANIZE，二级分区REORGANIZE指定tablespace为pg_global应合理报错，实际可执行成功 | 次要 | 已验收 |
| [I64M9Z](https://gitee.com/opengauss/Plugin/issues/I64M9Z?from=project-issue) | MySQL兼容分区语法REORGANIZE，二级分区REORGANIZE指定tablespace为pg_global时db core (segment=on) | 严重 | 已验收 |
| [I66UFT](https://gitee.com/opengauss/Plugin/issues/I66UFT?from=project-issue) | MySQL兼容分区语法REORGANIZE，重组织为其它类型的分区时数据库core | 严重 | 已验收 |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 合理报错场景测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.对普通表、临时表、全局临时表、unlogged表使用重组织分区语法，合理报错 | 执行4条用例，结果符合预期，测试通过 |

#### 4.1.1.3 不支持重组织分区的分区表

| 测试步骤 | 测试结果 |
| -------- | -------- |
|1.对间隔分区表、hash分区表执行重组织分区操作<br />2.对range-range、range-list、list-range、list-list、hash-range、hash-list、hash-hash分区表执行重组织分区操作 | 执行36条用例，结果符合预期，测试通过 |

#### 4.1.1.4 支持重组织分区的分区表

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.对range、list分区表执行重组织分区操作<br />2.对range-hash、list-hash分区表执行重组织分区操作 | 执行28条用例，结果符合预期，测试通过 | 

### 4.1.2 约束测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行68条用例，执行结果符合预期，测试通过 |

### 4.1.3 备份恢复测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.对表做增删分区操作，备份数据，删除数据<br />2.恢复数据后验证数据完整性、正确性 | 结果符合预期，测试通过 | 

### 4.1.4 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
| openGauss 3.1.0 build 152c968e | 68 | Passed: 52<br />Failed: 16 | 8 |
| openGauss 3.1.0 build 7924440a | 68 | Passed: 68<br />Failed: 0 | 0 |

## 4.2   后续测试建议

无

# 5     附件

无
 
