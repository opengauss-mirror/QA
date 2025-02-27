![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者     |
| --------- | -------- | ------------ | -------- |
| 2025.2.25 | V1.0     | 测试报告初稿 | lvlintao |



**Keywords 关键词**：GMS_STATS

**Abstract 摘要**：GMS_STATS是一个基于openGauss的插件。目前支持31个接口，包括：GMS_STATS.CREATE_STAT_TABLE，GMS_STATS.DROP_STAT_TABLE，GMS_STATS.GATHER_DATABASE_STATS，GMS_STATS.GATHER_INDEX_STATS,GMS_STATS.GATHER_TABLE_STATS，GMS_STATS.DELETE_COLUMN_STATS，GMS_STATS.DELETE_INDEX_STATS，GMS_STATS.DELETE_TABLE_STATS，GMS_STATS.DELETE_SCHEMA_STATS，GMS_STATS.SET_COLUMN_STATS，GMS_STATS.SET_INDEX_STATS，GMS_STATS.SET_TABLE_STATS，GMS_STATS.IMPORT_INDEX_STATS，GMS_STATS.IMPORT_TABLE_STATS，GMS_STATS.IMPORT_COLUMN_STATS，GMS_STATS.IMPORT_SCHEMA_STATS，GMS_STATS.EXPORT_INDEX_STATS，
GMS_STATS.EXPORT_TABLE_STATS，GMS_STATS.EXPORT_COLUMN_STATS，GMS_STATS.EXPORT_SCHEMA_STATS，GMS_STATS.GET_STATS_HISTORY_RETENTION，GMS_STATS.GET_STATS_HISTORY_AVAILABILITY，GMS_STATS.RESTORE_TABLE_STATS，GMS_STATS.RESTORE_SCHEMA_STATS，GMS_STATS.PURGE_STATS，GMS_STATS.LOCK_TABLE_STATS，GMS_STATS.LOCK_PARTITION_STATS，GMS_STATS.LOCK_SCHEMA_STATS，GMS_STATS.UNLOCK_TABLE_STATS，GMS_STATS.UNLOCK_PARTITION_STATS，GMS_STATS.UNLOCK_SCHEMA_STATS。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

此特性主要用来支持在A兼容模式下，支持gms_stats高级包，为用户提供查看和修改为数据库对象收集的优化器统计信息，目前支持31个接口。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build eb367cc7(B013) | openGauss  | 2025.1.23    | 2025.2.7     | lvlintao |
| openGauss 7.0.0-RC1 build d98993bb(B014) | openGauss  | 2025.2.7     | 2025.2.14    | lvlintao |
| openGauss 7.0.0-RC1 build 24e89e20(B015) | openGauss  | 2025.2.14    | 2025.2.21    | lvlintao |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注 |
| -------- | ----------------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | aarch64+openEuler | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>MEM: 32GB<br/>DISK: 200GB<br/>OS: openEuler 22.03 (LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

*本章概要性给出版本测试结论、风险和规避措施。*

## 3.1 测试结论总结

openGauss 兼容A的gms_stats特性，共计执行了65个用例，主要覆盖了功能测试、升级测试、兼容性测试、驱动测试、资料测试。功能测试覆盖了插件的创建、删除，插件包的31个接口函数功能，以及接口函数在不同场景下的验证；升级测试在6.0.0版本升级到7.0.0-RC1版本后执行用例验证此功能的正确性；兼容性测试覆盖在不同兼容模式验证插件功能；驱动测试覆盖使用jdbc驱动，测试插件功能是否正常；资料测试验证描述、约束、示例是否完善、有效。测试中发现3个问题，问题均以修复且回归通过，无遗留问题，整体质量良好。

## 3.2 约束说明

- 仅支持`CREATE EXTENSION`命令方式加载，使用`DROP EXTENSION`命令方式卸载。
- 仅支持在A库环境下使用，非A库环境不建议使用该插件
- 部分函数表现和A库不完全兼容

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                            | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                                 | 特性质量评估               | 主要风险 |
| ------------------------------- | -------------------------- | -------------------------- | -------------------- | ------------------------------------------------ | -------------------------- | -------- |
| 【兼容性组】支持GMS_STATS高级包 | 提升兼容能力，方便用户迁移 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试、兼容性测试、升级测试和资料测试 | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.2 可靠性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.3 安全&隐私保护测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.4 可服务性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.5 生命周期管理测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.6 韧性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.7 兼容性测试结论

| 测试步骤                                               | 测试结果                                |
| ------------------------------------------------------ | --------------------------------------- |
| 1. 在兼容"B"、"C"、"PG"类型数据库下创建gms_utility插件 | 执行1条用例，执行结果符合预期，测试通过 |

###  4.2.8 升级测试结论

| 测试步骤                        | 升级路径    | 测试结果                                |
| ------------------------------- | ----------- | --------------------------------------- |
| 1. 6.0.0版本升级到7.0.0-RC1版本 | 6.0.0-7.0.0 | 执行1条用例，执行结果符合预期，测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                   | 测试结论            |
| ---- | ------------------------------------------ | ------------------- |
| 1    | 插件参考>gms_stats Extension>gms_stats概述 | 未发现问题          |
| 2    | 插件参考>gms_stats Extension>gms_stats限制 | 未发现问题          |
| 3    | 插件参考>gms_stats Extension>gms_stats安装 | 未发现问题          |
| 4    | 插件参考>gms_stats Extension>gms_stats使用 | 发现1个问题，已验收 |

# 5 测试对象质量评估

本次测试主要针对gms_stats高级包功能覆盖了31个接口函数的功能验证、复杂使用场景下的功能是否正确，异常场景合理报错

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 1    | 2    | 0      |
| 百分比 | 100%     | 0%   | 33%  | 67%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBKZGL](https://e.gitee.com/opengaussorg/issues/table?issue=IBKZGL) | gather_table_stats接口使用partname参数统计分区信息，报错     | 主要     | 已验收   |
| [IBM3PJ](https://e.gitee.com/opengaussorg/issues/table?issue=IBM3PJ) | 新增的guc参数gms_stats_history_retention没有增加文档说明，其他接口说明和约束也未补充 | 次要     | 已验收   |
| [IBJREG](https://e.gitee.com/opengaussorg/issues/table?issue=IBJREG) | create_stat_table接口使用全局临时表参数，入参使用global_temporary:=true报错，与预期不符 | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性             | 验证策略                                                | 是否按照测试策略执行 |
| ---- | ---------------- | ------------------------------------------------------- | -------------------- |
| 1    | 插件创建和删除   | 在兼容A库下创建、删除插件                               | YES                  |
| 2    | 接口函数功能验证 | 接口函数基础功能及在复杂场景下的验证                    | YES                  |
| 3    | 兼容验证         | 在非兼容A库下执行用例，验证功能是否支持，数据库正常运行 | YES                  |
| 4    | 升级验证         | 从6.0.0升级到7.0.0之后执行用例验证功能是否正确          | YES                  |
| 5    | 驱动测试         | 使用jdbc驱动验证插件相关功能                            | YES                  |
| 6    | 资料验证         | 资料描述与需求相符，示例是否正确无误                    | YES                  |

##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度  |
| ---------------------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | --------- |
| openGauss 7.0.0-RC1 build eb367cc7(B013) | 10               | 60         | 60         | 3          | 3.97kloc       | 0.75/kloc |
| openGauss 7.0.0-RC1 build d98993bb(B014) | 2                | 3          | 3          | 0          | 0              | 0         |
| openGauss 7.0.0-RC1 build 24e89e20(B015) | 1                | 2          | 2          | 0          | 0              | 0         |

###  6.3.2 测试用例执行结果统计数据

| 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 65             | 65               | 65     | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1、源代码PR：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7005

2、文档pr：

https://gitee.com/opengauss/docs/pulls/7214

3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=fbccd046ad7b4007b7d47b88acfff8d2&hideDevcloudHead=true

4、测试用例：

测试用例->openGauss_7.0.0->数据库兼容性->兼容A库特性->支持gms_stats高级包

