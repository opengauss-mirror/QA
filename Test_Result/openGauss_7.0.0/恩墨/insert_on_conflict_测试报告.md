![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述                         | 作者         |
| ---------- | -------- | -------------------------------- | ------------ |
| 2026.07.27 | v1.0     | 初版：PR8765 INSERT ON CONFLICT 特性测试报告 | liuzhen0625 |

[TOC]

**Keywords 关键词**：openGauss，INSERT ON CONFLICT，UPSERT，唯一约束冲突，PostgreSQL 兼容

**Abstract 摘要**：本报告总结 openGauss `INSERT ... ON CONFLICT ... DO NOTHING/UPDATE`（MR !8765）特性测试活动

**缩略语清单：**无

# 1 概述

openGauss支持INSERT ON CONFLICT DO UPDATE/NOTHING语法及其功能，ON CONFLICT子句指定引发唯一约束冲突时，执行ON CONFLICT后面的conflict_action，将INSERT行为变更为UPDATE或DO NOTHING以避免报错，让兼容PG语法的应用程序更容易处理插入数据时可能发生的唯一性约束冲突。

本报告主要测试insert on conflict语法，共计执行测试用例51个，主要覆盖了功能测试、可靠性测试、升级测试、资料测试等。升级测试在6.0.0版本升级到7.0.0-LTS版本后执行用例验证此功能的正确性；测试输出用例51个，共发现功能问题5个，资料问题1个。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称 | 软件包名称 / build | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------- | ------------------ | ------------ | ------------ | -------- |
| openGauss 7.0.0-LTS.B012 | openGauss 7.0.0-LTS.B012 | 2026-06-26 | 2026-07-07 | liuzhen |

### 2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       | 无       | 无       |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号 | 硬件配置信息 | 备注 |
| -------- | -------- | ------------ | ---- |
| 服务器 | aarch64 + openEuler | OS：openEuler 24.03（arm） | 无   |

不涉及（物理/裸金属测试机环境，本章节 NA）。

### 2.2.2 OS版本

| 操作系统 | OS版本 | 版本说明 |
| -------- | ------ | -------- |
| openEuler | 24.03 aarch64 |  |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试insert on conflict语法，共计执行测试用例51个，主要覆盖了功能测试、可靠性测试、升级测试、资料测试等。升级测试在6.0.0版本升级到7.0.0-LTS版本后执行用例验证此功能的正确性；测试输出用例51个，共发现功能问题5个，资料问题1个。整体测试质量优秀。

## 3.2 约束说明

- 设计仅承诺 **A / PG** 兼容模式；B/Dolphin 模式不在范围。
- 目标表 **不支持** 外部表、列存表、视图（应明确 ERROR）。
- `ON CONFLICT DO UPDATE` **必须**提供 conflict_target（推断列或 `ON CONSTRAINT`）。
- `DO NOTHING` 可省略 conflict_target（处理全部唯一约束冲突）。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性 | 特性价值评估 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估 | 主要风险 |
| ---- | ------------ | -------------------------- | -------------------- | ---------------- | ------------ | -------- |
| INSERT ON CONFLICT | 满足：降低唯一冲突插入失败成本，让兼容PG语法的应用程序更容易处理插入数据时可能发生的唯一性约束冲突。 | 详见3.2章节描述 | 无 | 覆盖了功能测试、可靠性测试、升级测试、资料测试等 | <font color=green>▮</font> | 无 |

*特性质量评估说明*：

<font color=red>●</font>：表示特性不稳定，风险高

<font color=yellow>▲</font>：表示特性基本可用，遗留少量问题

<font color=green>▮</font>：表示特性质量良好

## 4.2 产品功能属性测试结论

| 功能点验证 | 是否覆盖 | 测试结果摘要 |
| ---------- | -------- | ------------ |
| DO NOTHING（含省略 conflict_target） | YES | 冲突行忽略，原行不变 |
| DO UPDATE SET / EXCLUDED / WHERE | YES | 冲突行按表达式更新；条件更新生效 |
| 批量 VALUES 部分冲突 | YES | 不冲突行插入，冲突行按策略处理 |
| conflict_target 单列/复合/表达式/部分索引/ON CONSTRAINT | YES | 可推断则成功，否则明确 ERROR |
| INSERT…SELECT + ON CONFLICT | YES | 归档/同步场景可用 |
| 分区表 upsert | YES | 分区内 DO NOTHING/UPDATE 正常 |
| 存储过程 / 匿名块 / 游标内调用 | YES | 过程内 upsert 可用 |
| PBE（PREPARE/EXECUTE） | YES | 多次 EXECUTE 更新同一键 |
| 负向：无匹配唯一索引 / DO UPDATE 缺 target | YES | 语义层 ERROR（非 syntax） |
| 负向：外表 / 列存 / 视图目标表 | YES | 按设计拒绝 |
| 与 `ON DUPLICATE KEY UPDATE` 共存 | YES | MySQL 风格 upsert 仍可用 |
| A / PG 兼容库 | YES | 主路径在 A 库验证；设计要求两模式一致 |

## 4.3 产品质量属性目标(DFX)测试结论

### 4.3.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA | 本阶段不涉及专项性能对比 |

### 4.3.2 可靠性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 事务 COMMIT/ROLLBACK 与 upsert | 持久化/回滚行为符合预期（设计用例覆盖） |
| 触发器在 DO UPDATE 分支 | 可触发，行为符合用例预期 |

### 4.3.3 安全&隐私保护测试结论

本需求不涉及。

### 4.3.4 可服务性测试结论

本需求不涉及。

### 4.3.5 生命周期管理测试结论

本需求不涉及。

### 4.3.6 韧性测试结论

本需求不涉及。

### 4.3.7 兼容性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| A 库兼容性功能 | 通过 |
| PG库兼容性 功能 | 通过 |
| BCD库兼容性 | 不支持，报错 |

### 4.3.8 升级测试结论

| 测试步骤 | 升级路径 | 测试结果 |
| -------- | -------- | -------- |
| 安装6.0.0LTS版本，升级到7.0.0-LTS版本 | 6.0.0LTS---7.0.0-LTS | 通过，升级前不支持insert on conflict，升级后支持 |

## 4.4 资料测试结论

| 序号 | 测试章节 | 测试结论 |
| ---- | -------- | -------- |
| 1 | SQL语法：insert | 发现1个问题单 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试主要针对新增的insert on conflict语法，覆盖功能验证、兼容性测试、支持主流驱动，异常场景合理报错

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重  | 主要  | 次要 | 不重要 |
| ------ | -------- | ----- | ----- | ---- | ------ |
| 数目   | 6        | 1     | 2     | 3    | 0      |
| 百分比 | 100%     | 16.7% | 33.3% | 50%  | 0%     |

### 5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| https://gitcode.com/opengauss/openGauss-server/issues/8254 | insert on conflict不支持视图，报错信息需要优化 | 次要 | 待办的 |
| https://gitcode.com/opengauss/openGauss-server/issues/8260 | insert on conflict do update 新插入更新值与同时新插入的数据一致，openGauss执行结果与pg不一致 | 严重 | 待办的 |
| https://gitcode.com/opengauss/openGauss-server/issues/8261 | insert on conflict do update 新插入的本身存在冲突，opengauss执行结果与pg不一致 | 主要 | 待办的 |
| https://gitcode.com/opengauss/openGauss-server/issues/8263 | insert on conflict 复合唯一索引列序推断，opengauss执行结果与pg不一致 | 主要 | 待办的 |
| https://gitcode.com/opengauss/openGauss-server/issues/8275 | insert on conflict需求文档说明只支持A、PG兼容性，目前在BCD兼容性报错信息不一样，需要优化，并给出兼容性支持结论 | 次要 | 待办的 |
| https://gitcode.com/opengauss/openGauss-server/issues/8340 | on conflict 不支持B库，资料示例SQL是在B库 | 次要 | 待办的 |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性 | 验证策略 | 是否按照测试策略执行 |
| ---- | ---- | -------- | -------------------- |
| 1 | 功能测试                 | A库/PG库覆盖 与PG数据库对比执行结果 | YES |
| 2 | 兼容性测试 | BCD库兼容性，不支持报错 | YES |
| 3 | 资料验证   | 验证SQL语法：insert on conflict相关资料 | YES |
| 4 | 升级验证 | 安装6.0.0LTS版本，升级到7.0.0-LTS版本验证conflict语法可用，回退成功 | YES |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-RC3 B012 | 8 | 51（功能设计项） | 51 | 5 | 3.5k | 1.43 |

### 6.3.2 测试用例执行结果统计数据

本需求功能设计用例 51项。

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 功能设计轮（IOC） | 51 | 51 | 46 | 5 | 0 | 0 | 100% | 90% |

# 7 附件

## 

## 7.1 附件1：特性相关PR与材料

1. 源代码 PR：https://gitcode.com/opengauss/openGauss-server/merge_requests/8765
2. 文档PR：https://gitcode.com/opengauss/docs/pull/7901
3. 测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=e2bd23b7eed544648c3d1153ee097563
5. 测试用例：测试用例→openGauss_7.0.0LTS→兼容性→insert_on_conflict
