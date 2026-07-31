![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述                                      | 作者         |
| ---------- | -------- | --------------------------------------------- | ------------ |
| 2026.07.28 | v1.0     | 初版：PR8647 Global Sequence Cache 特性测试报告 | liuzhen0625 |

[TOC]

**Keywords 关键词**：Global Sequence Cache，序列连续性

**Abstract 摘要**：主要测试openGauss global sequence支持情况。

**缩略语清单：**无

# 1 概述

本报告主要测试全局序列缓存（GLOBAL/SESSION）语法与行为，共计执行测试用例 **42** 个，主要覆盖功能测试、兼容性测试、可靠性相关场景（事务/重启）、升级测试、资料测试等，共发现2个issue。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称 | 软件包名称 / build | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------- | ------------------ | ------------ | ------------ | -------- |
| openGauss 7.0.0-LTS | openGauss 7.0.0-LTS（含 B013） | 2026-07-20 | 2026-07-25 | liuzhen |

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
| openEuler | 24.03 aarch64 | 无 |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试全局序列缓存（GLOBAL/SESSION）语法与行为，共计执行测试用例 **42** 个，主要覆盖功能测试、兼容性测试、可靠性相关场景（事务/重启）、升级测试、资料测试等。升级/回滚路径在用例 Case0042 中覆盖；功能主路径（CREATE/ALTER GLOBAL|SESSION、nextval 连续、级别切换 cache 丢弃、老版本拦截等）验证通过。测试共发现功能问题 **1** 个、资料问题 **1** 个。整体测试质量良好。

## 3.2 约束说明

- 未指定级别时默认为 **SESSION**；GLOBAL 走全局 cache，SESSION 保持原有会话级 cache。
- 老版本序列（relkind `S`/`L`）**不可** ALTER 为 GLOBAL；新版本（`z`/`Z`）可双向切换，切换后 nextval 从 `last_value` 续接，未用完的 cache 丢弃。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性 | 特性价值评估 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估 | 主要风险 |
| ---- | ------------ | -------------------------- | -------------------- | ---------------- | ------------ | -------- |
| Global Sequence Cache | 兼容Oracle，实现一套全局sequence cache方案，以解决在高并发且下cache不为1时，序列值不连续的问题。 | 详见 3.2 章节描述 | 无                   | 覆盖语法与 DDL、序列接口、边界/切换、并发、兼容模式、事务上下文、老版本、升级/重启等 | <font color=green>▮</font> | 资料待补齐 |

*特性质量评估说明*：

<font color=red>●</font>：表示特性不稳定，风险高

<font color=yellow>▲</font>：表示特性基本可用，遗留少量问题

<font color=green>▮</font>：表示特性质量良好

## 4.2 产品功能属性测试结论

| 功能点验证 | 是否覆盖 |
| ---------- | -------- |
| CREATE SEQUENCE … GLOBAL / SESSION | YES |
| CREATE LARGE SEQUENCE … GLOBAL | YES |
| ALTER → GLOBAL / SESSION 及 cache 丢弃 | YES |
| 同级别 ALTER 幂等 / RESTART / DROP 重建 | YES |
| OWNED BY 分区表 / serial·largeserial | YES |
| ALTER 非级别属性及 LARGE 级别变更 | YES |
| 小范围 cycle 连续性 | YES |
| nextval/currval/setval/lastval 综合 | YES |
| 元数据函数与 LARGE 接口 | YES |
| 边界与 cycle 综合 | YES |
| 大规模并发无重复 / 会话隔离 | YES |
| SESSION 行为不回退 | YES |
| 扩展与 DDL 组合（shark/RENAME/B 库等） | YES |
| A/PG/B/C/D 兼容模式 GLOBAL 基础功能 | YES |
| 事务 COMMIT/ROLLBACK、DO、游标、PBE、触发器、存储过程 | YES |
| 老版本序列 relkind / ALTER 拦截 / nextval | YES |

## 4.3 产品质量属性目标(DFX)测试结论

### 4.3.1 性能测试结论

本需求不涉及。

### 4.3.2 可靠性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 事务 COMMIT/ROLLBACK 与 nextval | 持久化/不回收行为符合预期 |
| 反复重启后 last_value 续接 | 重启后从 last_value 续接 |

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
| A 模式（Oracle）GLOBAL 基础功能 | 通过 |
| PG 模式 GLOBAL 基础功能 | 通过 |
| B 模式（MySQL）GLOBAL / auto_increment 组合 | 通过 |
| C 模式（TD）GLOBAL 基础功能 | 通过 |
| D 模式（SQL Server）GLOBAL / shark 元数据 | 通过 |

### 4.3.8 升级测试结论

| 测试步骤 | 升级路径 | 测试结果 |
| -------- | -------- | -------- |
| 含 GLOBAL/SESSION 序列数据的升级/回滚/commit | 6.0.2 基线 → 7.0.0 LTS | 通过，upgrade/rollback/commit 各阶段序列可用 |

## 4.4 资料测试结论

| 序号 | 测试章节 | 测试结论 |
| ---- | -------- | -------- |
| 1 | 系统表 / `pg_class` 类型说明（序列新增类型） | 发现1个问题单 |
| 2 | SQL语法 create sequence | 测试通过 |
| 3 | SQL语法 alter sequence | 测试通过 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试针对新增 GLOBAL/SESSION 序列缓存能力，需求用例 **42** 条，覆盖：覆盖功能验证、多兼容模式、事务/过程/PBE 耦合、升级与重启可靠性场景；异常场景合理报错。

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重  | 主要  | 次要 | 不重要 |
| ------ | -------- | ----- | ----- | ---- | ------ |
| 数目   | 2        | 0     | 1     | 1    | 0      |
| 百分比 | 100%     | 0%    | 50%   | 50%  | 0%     |

### 5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| https://gitcode.com/opengauss/openGauss-server/issues/8285 | `DROP SEQUENCE` 报错 `ERROR: could not open relation with OID 0` | 主要 | 已验收 |
| https://gitcode.com/opengauss/openGauss-server/issues/8342 | globalSequenceCache 需求：`pg_class` 新增两个类型，资料未体现 | 次要 | 待办的 |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性 | 验证策略 | 是否按照测试策略执行 |
| ---- | ---- | -------- | -------------------- |
| 1 | 功能测试 | 语法/DDL、序列接口、边界与切换、并发连续性 | YES |
| 2 | 兼容性测试 | A/PG/B/C/D 模式 GLOBAL 基础功能 | YES |
| 3 | 升级测试 | 含序列数据的 upgrade / rollback / commit | YES |
| 4 | 耦合测试 | 事务、匿名块、游标、PBE、触发器、存储过程、DDL 组合 | YES |
| 5 | 资料测试 | SQL语法sequence相关测试、系统表/类型资料核对 | YES |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-LTS | 6 | 42 | 42 | 1（功能，不含资料） | 10k | 0.1      |

### 6.3.2 测试用例执行结果统计数据

本需求功能设计用例 **42** 项。

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 功能设计轮（GSC） | 42 | 42 | 41 | 1 | 0 | 0 | 100% | 97.6% |

# 7 附件

## 7.1 附件1：特性相关 PR 与材料

1. 源代码 MR：https://gitcode.com/opengauss/openGauss-server/merge_requests/8647
2. 资料PR：https://gitcode.com/opengauss/docs/pull/8422
3. 测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=1d4f177276404cfa82643ee49a6432bc
6. 测试用例：测试用例→openGauss_7.0.0LTS→兼容性→global_sequence cache
