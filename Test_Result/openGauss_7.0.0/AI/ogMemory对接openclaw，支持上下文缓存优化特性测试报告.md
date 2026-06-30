![avatar](../../images/openGauss.png)

版权所有 © 2026 openGauss社区

修订记录

| 日期         | 修订版本 | 修改描述                              | 作者      |
| ---------- | ---- | --------------------------------- | ------- |
| 2026-06-23 | 0.2  | oGMemory对接openclaw，支持上下文缓存优化 测试报告 | l1azzzy |

[TOC]

**Keywords 关键词**：ogMemory、OpenClaw、openGauss、after_turn、compose、Extraction、Commit、Outbox、向量索引、会话压缩

**Abstract 摘要**：本报告总结 ogMemory 功能测试阶段的测试范围、测试结果和质量评价。测试覆盖记忆写入、记忆抽取、主存储落盘、Outbox 异步索引、分层召回、向量后端异常降级、OpenClaw 集成和 openGauss 本地部署等场景。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| ogMemory | openGauss Memory | 面向 Agent 的上下文生命周期和长期记忆管理能力 |
| LLM | Large Language Model | 大语言模型 |
| API | Application Programming Interface | 应用程序接口 |
| Outbox | Outbox Pattern | 主存储写入后异步投递索引事件的机制 |
| L0/L1/L2 | Level 0/1/2 | 分层索引层级，分别用于主题、概览和精确内容 |

# 1 概述

本报告为 oGMemory对接openclaw，支持上下文缓存优化特性测试活动总结，测试对象为 ogMemory + OpenClaw + openGauss 本地部署环境。

ogMemory 的目标是提供可插拔、可持久化、可治理的 Agent 上下文生命周期引擎，覆盖记忆写入、记忆召回、分层索引、会话压缩和部署适配能力。其核心原则是推理前后拦截，不介入模型推理内部；同步路径保持轻量，主存储先行，Outbox 异步索引。

多租户治理相关能力开发未进行设计，本阶段不纳入测试范围。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称            | 软件包名称           | 测试起始时间     | 测试结束时间     | 测试人员    |
| --------------- | --------------- | ---------- | ---------- | ------- |
| ogMemory 功能测试版本 | oGMemory poc_57 | 2026-06-13 | 2026-06-13 | l1azzzy |

### 2.1.2 配套测试的版本

| 版本名称      | 配套版本                         | 版本说明                             |
| --------- | ---------------------------- | -------------------------------- |
| oGMemory  | poc_57                       | 被测组件                             |
| OpenClaw  | poc1_416（OpenClaw 2026.3.24） | 用于推理前 compose 和推理后 after_turn 集成 |
| openGauss | B007                         | 用作 ogMemory 主存储和向量索引后端           |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息   | 硬件型号  | 硬件配置信息                                                                                            | 备注  |
| ------ | ----- | ------------------------------------------------------------------------------------------------- | --- |
| 本地部署环境 | 鲲鹏920 | CPU：2.9GHz，2路64核，256线程，aarch64<br />内存：1TB（1054GB）<br />磁盘：NVMe，多盘，447GB SSD + 2.9TB HDD 混合<br /> |     |

### 2.2.2 虚拟化平台

| 虚拟化平台 | 版本说明 |
| ---------- | -------- |
| Docker | 用于 openGauss 容器故障模拟和恢复验证 |

### 2.2.3 OS版本

| 操作系统 | OS版本 | 版本说明 |
| -------- | ------ | -------- |
| openEuler | 24.03（LTS-SP2） | `NAME="openEuler"`，`VERSION_ID="24.03"`，`PRETTY_NAME="openEuler 24.03 (LTS-SP2)"` |


# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试按需求范围执行 ogMemory 功能验证。用例统计口径如下：

| 设计用例总数 | 本次不测 | 有效测试用例数 | PASS | 执行率 | 通过率 |
| ------------ | -------- | -------------- | ---- | ------ | ------ |
| 30 | 8 | 22 | 22 | 100% | 100% |

说明：多租户隔离未设计，本次不进行测试

测试结论：ogMemory 在 after_turn 轻量同步、达到阈值后台处理、同 session 并发写入、CandidateMemory 字段完整性、ReAct merge、PolicyRouter 路由、ContextWriter 一致落盘、快照归档、Outbox 投递与消费、L0/L1/L2 索引生成、幂等 upsert、向量库失败重试、TypedQuery 生成、分层召回和向量后端异常降级等核心链路上表现符合预期。

## 3.2 约束说明

- 多租户治理相关能力开发未进行设计，本次不测。

## 3.3 关键风险和规避措施

| 风险类型 | 风险描述                 | 风险影响               | 规避措施和计划             | 责任人   | 当前进展 |
| ---- | -------------------- | ------------------ | ------------------- | ----- | ---- |
| 覆盖风险 | 多租户治理能力开发未进行设计       | 多租户隔离相关场景未在本阶段验证   | 待后续需求设计明确后补充测试设计和验证 | 开发/测试 | 本次不测 |


# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性 | 特性价值评估 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估 | 主要风险 |
| ---- | ------------ | -------------------------- | -------------------- | ---------------- | ------------ | -------- |
| ogMemory 上下文生命周期引擎 | 支持 Agent 记忆写入、召回、分层索引和部署适配，可增强长期上下文管理能力 | 依赖 OpenClaw 调用 after_turn/compose，依赖 openGauss 主存储和向量索引后端 | 无 | 有效测试用例 22 条，全部通过 | <font color=green>▮</font> | 多租户治理和 compact 不在本次测试范围 |

## 4.2 产品功能属性测试结论

| 功能点验证                          | 是否覆盖 | 测试结论                                             |
| ------------------------------ | ---- | ------------------------------------------------ |
| profile 写入                     | YES  | PASS，可写入 profile 类型长期记忆并支持召回                     |
| preference 同主题偏好合并             | YES  | PASS，同主题 preference 合并正确，无同 URI 同 level 重复       |
| event 历史事件追加                   | YES  | PASS，历史事件按追加方式保存                                 |
| after_turn 未达阈值同步轻量路径          | YES  | PASS，短对话返回 `accumulating`，未触发后台抽取线程              |
| after_turn 达到阈值后台处理            | YES  | PASS，长对话返回 `processing`，后台抽取线程启动                 |
| 并发 after_turn 同 session 写入     | YES  | PASS，5 个并发请求均 `ok=true`，run_id 唯一，无错误            |
| CandidateMemory 字段完整性          | YES  | PASS，filters 和 metadata 字段完整                     |
| ReAct 读取已有记忆后 merge            | YES  | PASS，preference entries 合并正确                     |
| PolicyRouter 按 category 选择写入策略 | YES  | PASS，7 类 category 路由和层级分布符合预期                    |
| ContextWriter 一致写入             | YES  | PASS，profile L2 条目字段一致落盘                         |
| 写入后快照归档                        | YES  | PASS，后台 snapshot commit 完成，buffer 清空             |
| Outbox 写入后生成 PENDING 事件        | YES  | PASS，vector_index 记录生成，说明事件被消费写入索引               |
| Outbox 消费生成索引                  | YES  | PASS，L0/L1/L2 三层索引完整生成                           |
| 重复消费幂等 upsert                  | YES  | PASS，IndexRecord ID 符合 `sha256(uri:level)[:16]`  |
| 向量库写入失败重试                      | YES  | PASS，DB 故障期间服务不崩溃，恢复后 worker 正常                  |
| 跨会话召回                          | YES  | PASS，可跨会话召回长期记忆                                  |
| QueryPlanner 生成 TypedQuery     | YES  | PASS，compose 返回 episodic、identity、memory 三类 slot |
| SeedRetriever 分层检索             | YES  | PASS，分层检索链路可用                                    |
| L0 摘要命中后展开到 L2                 | YES  | PASS，摘要命中后可展开到精确内容                               |
| L2 精确内容直接命中                    | YES  | PASS，精确内容查询可命中对应 L2                              |
| 同 URI 多层同时命中去重                 | YES  | PASS，多层命中后可去重                                    |
| vector backend 查询失败            | YES  | PASS，compose 不崩溃，降级为 archive-only context        |


## 4.3 产品质量属性目标(DFX)测试结论

### 4.3.1 性能测试结论

| 测试步骤              | 测试结果                                            |
| ----------------- | ----------------------------------------------- |
| Benchmark 综合验证    | PASS，总体结果 63.2%（96/152）                         |
| 单轮 QA             | PASS，结果 65.6%（21/32）                            |
| 多轮 QA             | PASS，结果 67.6%（25/37）                            |
| 跨会话 QA            | PASS，结果 76.9%（10/13）                            |
| 时序 QA             | PASS，结果 57.1%（40/70）                            |
| after_turn 同步轻量路径 | PASS，未达阈值时返回 `accumulating`，同步路径不调用 LLM         |
| 达阈值后台处理           | PASS，返回 `processing`，同步接口不等待抽取和索引完成             |
| token 消耗优化 test1  | PASS，base avg：204927，ov avg：92361，diff：-54.93%  |
| token 消耗优化 test2  | PASS，base avg：171418，ov avg：88415，diff：-48.42%  |
| token 消耗优化 test3  | PASS，base avg：415910，ov avg：148679，diff：-64.25% |
准确率提升：准确率提升40%-50%，高于验收指标30%
token消耗优化：平均优化高于50%，达到验收指标（30%）
### 4.3.2 可靠性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 5 个线程并发提交同 session 消息 | PASS，全部返回 `ok=true`，无错误，消息顺序正确 |
| 向量库写入失败重试 | PASS，停止 openGauss 容器后 after_turn 仍返回 `ok=true`，服务不崩溃；恢复后 worker 正常 |
| vector backend 查询失败 | PASS，compose 不崩溃，retrieved=0，降级为 archive-only context |

### 4.3.3 安全&隐私保护测试结论

多租户治理相关能力开发未进行设计，本次不测。

### 4.3.4 可服务性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| after_turn 返回状态检查 | PASS，可通过 `accumulating`、`processing` 判断同步路径和后台处理状态 |
| docker logs 检查 snapshot commit | PASS，可观察 `after_turn background snapshot commit done` |
| DB 查询 vector_index/context 数据 | PASS，可验证 filters、metadata、level、parent_uri、context_type 等字段 |
| stats 检查重试情况 | PASS，可见失败重试与成功处理统计指标 |

### 4.3.5 生命周期管理测试结论



### 4.3.6 韧性测试结论

| 测试步骤                       | 测试结果                                 |
| -------------------------- | ------------------------------------ |
| openGauss 停止时 after_turn   | PASS，接口仍返回 `ok=true`，服务不崩溃           |
| openGauss 恢复后 OutboxWorker | PASS，worker 恢复消费                     |
| compose 向量检索失败             | PASS，返回有效响应，降级为 archive-only context |

### 4.3.7 兼容性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| ogMemory + OpenClaw 本地部署 | PASS，已覆盖基础写入和召回 |
| ogMemory + openGauss 主存储/向量后端 | PASS，已覆盖 DB 写入、查询、故障和恢复 |
| Docker 容器故障模拟 | PASS，已覆盖停止/恢复 openGauss 容器场景 |

## 4.4 资料测试结论

| 序号  | 测试章节                | 测试结论 |
| --- | ------------------- | ---- |
| 1   | 文档中心-向量数据库-oGMemory | 测试通过 |


# 5 测试对象质量评估

## 5.1 覆盖率分析


| 覆盖项                           | 设计覆盖 | 执行覆盖 | 说明                                 |
| ----------------------------- | ---- | ---- | ---------------------------------- |
| 记忆写入 profile/preference/event | YES  | YES  | PASS                               |
| after_turn 同步/后台路径            | YES  | YES  | PASS                               |
| 并发写入                          | YES  | YES  | PASS                               |
| Extraction 字段完整性和 ReAct merge | YES  | YES  | PASS                               |
| Commit 写入策略和一致落盘              | YES  | YES  | PASS                               |
| Outbox 投递、消费、索引生成、幂等和重试       | YES  | YES  | PASS                               |
| compose 分层召回                  | YES  | YES  | PASS                               |
| vector backend 异常降级           | YES  | YES  | PASS                               |
| 部署适配                          | YES  | YES  | PASS                               |
| 性能                            | YES  | YES  | PASS，token 消耗对比降幅为 48.42% 到 64.25% |

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目 | 0 | 0 | 0 | 0 | 0 |
| 百分比 | 0% | 0% | 0% | 0% | 0% |

### 5.2.2 缺陷列表

无。

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号  | 特性     | 验证策略                                                      | 是否按照测试策略执行 |
| --- | ------ | --------------------------------------------------------- | ---------- |
| 1   | 记忆写入   | 通过 after_turn、DB 查询、Benchmark 验证 profile/preference/event | YES        |
| 2   | 记忆抽取   | 验证 CandidateMemory 字段、ReAct merge、PolicyRouter            | YES        |
| 3   | 主存储和索引 | 验证 ContextWriter、Outbox、L0/L1/L2 索引、幂等 upsert             | YES        |
| 4   | 记忆召回   | 通过 compose、TypedQuery、分层检索验证跨会话和分层召回                      | YES        |
| 5   | 异常降级   | 停止 openGauss 和 vector backend，验证服务不崩溃和降级响应                | YES        |
| 6   | 性能     | Benchmark 和同步轻量路径验证                                       | YES        |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称            | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| --------------- | --------- | ----- | ----- | ----- | --------- | ---- |
| oGMemory poc_57 | 4         | 22    | 22    | 0     | 200k      | 0    |

### 6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | PASS | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ---- | ------ | ---------- |
| 第一轮 | 22 | 22 | 22 | 100% | 100% |

# 7 附件

## 7.1 附件1：遗留问题列表

无。

## 7.2 附件4：特性相关PR

[测试设计](https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=bb37fcc60e8a4d63b2da32b09cd2d63b)

测试用例：testplan-oGMemory/openViking-oGmemory对接openclaw
资料：https://gitcode.com/opengauss/oGMemory/pull/68
PR:https://gitcode.com/opengauss/oGMemory/pull/66
https://gitcode.com/opengauss/oGMemory/pull/65
https://gitcode.com/opengauss/oGMemory/pull/64
https://gitcode.com/opengauss/oGMemory/pull/63
https://gitcode.com/opengauss/oGMemory/pull/39
https://gitcode.com/opengauss/oGMemory/pull/35
https://gitcode.com/opengauss/oGMemory/pull/23
https://gitcode.com/opengauss/oGMemory/pull/22
https://gitcode.com/opengauss/oGMemory/pull/18