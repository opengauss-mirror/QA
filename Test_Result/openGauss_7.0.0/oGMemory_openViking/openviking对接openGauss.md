![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者   |
| ---------- | -------- | ------------ | ------ |
| 2026.6.10 | v1.0     | 测试报告     |  孙德超 |


[TOC]

**Keywords 关键词**：OpenViking、openGauss、向量数据库、OpenClaw、LoCoMo、RAG

**Abstract 摘要**

| 项目 | 内容 |
|------|------|
| 功能验证 | 验证 OpenViking 使用 openGauss 作为向量数据库后，服务启动、健康检查、记忆写入、记忆检索链路是否可用 |
| 对接验证 | 验证 OpenClaw -> OpenViking -> openGauss 链路是否连通 |
| 配置验证 | 验证 OpenViking storage.vectordb.backend=opengauss、embedding 模型、OpenClaw 远端 OpenViking 插件配置是否生效 |
| 评测验证 | 使用 LoCoMo small 数据集验证记忆写入、压缩、问答、Judge 评分流程 |
| 兼容性验证 | 验证 OpenViking openGauss backend 在 standalone 模式下连接 openGauss / SPQ CN 的基本兼容性 |
| 风险验证 | 识别分布式模式、连接复用、任务查询鉴权等已观察到的风险点 |

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| OV | OpenViking | 记忆系统 |
| OC | OpenClaw | 调用 OpenViking 的上层 Agent / Gateway 服务 |
| OG | openGauss | openGauss 数据库 |
| SPQ | Streaming / Scalable Postgres Query | openGauss 分布式能力相关组件 |
| CN | Coordinator Node | 分布式数据库协调节点 |
| DN | Data Node | 分布式数据库数据节点 |
| RAG | Retrieval-Augmented Generation | 检索增强生成 |
| LLM | Large Language Model | 大语言模型 ||


# 1 概述

本次测试围绕 OpenViking 对接 openGauss 向量数据库后端展开，验证 OpenViking 能否将记忆数据写入 openGauss，并通过 OpenClaw 调用 OpenViking 完成 LoCoMo small 记忆评测流程。


# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------ | ---------- | ------------ | ------------ | -------- |
| OpenViking openGauss backend | `swr.cn-north-4.myhuaweicloud.com/kunpeng-ai/openviking-opengaussbackend:0526` | 2026.6.08 | 2026.6.10 | 孙德超 |
| OpenClaw OpenViking 插件镜像 | `swr.cn-north-4.myhuaweicloud.com/kunpeng-ai/openclaw-openviking:remote-0526` | 2026.6.08 | 2026.6.10 | 孙德超 |
| openGauss distributed | `swr.cn-north-4.myhuaweicloud.com/kunpeng-ai/opengauss-distributed:0422` | 2026.6.08 | 2026.6.10 | 孙德超 |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注 |
| -------- | ----------------- | ------------------------------------------------------------ | ---- |
| 物理机 | aarch64 + openEuler | CPU: Kunpeng-920，128 核<br/>MEM: 477GiB<br/>DISK: `/data1` 3.6T<br/>OS: openEuler 20.03 (LTS) | Docker 容器化部署 |




# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试完成了 OpenViking 对接 openGauss 的部署、配置、服务健康检查、OpenClaw 集成和 LoCoMo small 评测。
LoCoMo small 跑测完成，35 个问题全部完成评分，无 `[ERROR]` 响应，整体准确率为 82.86%。从链路角度看，OpenClaw 能够调用 OpenViking，OpenViking 能够在 openGauss backend 配置下完成记忆流程。向量检索在鲲鹏优于HG超30%。测试中未发现问题，整体质量良好。

## 3.2 约束说明

无


## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性         | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                       | 特性质量评估               | 主要风险 |
| ------------ | -------------------------- | -------------------------- | -------------------- | -------------------------------------- | -------------------------- | -------- |
| OpenViking 对接 openGauss backend | 支持使用 openGauss 作为 OpenViking 向量存储后端，降低外部向量库依赖 | 依赖 openGauss 可连接、embedding 维度与表结构一致、OpenViking 配置正确 | 无 | 覆盖部署、配置、健康检查、LoCoMo small、DB 表计数 | <font color=green>▮</font>  | 无 |


*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品功能属性测试结论

| 被测功能点 | 是否覆盖 |
|----------|------------|
| OpenViking 服务启动 | YES | 通过 |
| OpenClaw 服务启动 | YES | 通过 |
| OpenViking 健康检查 | YES | 通过，`/health` 返回 `healthy=true` |
| OpenClaw 健康检查 | YES | 通过，`/health` 返回 `ok=true` |
| OpenViking 使用 openGauss backend | YES | 通过配置验证 |
| OpenClaw 连接 OpenViking | YES | 通过，插件 `baseUrl=http://openviking:1933` |
| 记忆写入 openGauss | YES | 通过，`public.context` 存在数据 |
| LoCoMo small ingest | YES | 通过，4 个 session compact 完成 |
| LoCoMo small QA | YES | 通过，35 个问题执行完成 |
| LoCoMo small Judge | YES | 通过，35 个问题完成评分 |
| SPQ 分布式分片表 | YES | 通过，`standalone` 模式下分布式openGauss可正常进行记忆存储和召回 |

## 4.3 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

# 920B VS HG7490 向量检索性能测试结果

## 环境信息

| 项目 | 920B | HG7490 |
|---|---|---|
| OS 版本 | OG 7.0.0 | OG 7.0.0 |
| 硬件 | 7280Z 920B 2P，80 * 2，2.9GHz，4 NUMA，160 CPUs | HG 7490，64 * 2 * 2，3.1GHz，8 NUMA，Threads per Core: 2，256 CPUs |
| 内存 | 500GB DDR5，4800 MT/s | 751GB DDR5，4800 MT/s |
| 磁盘 | 2.9T * 2 NVME | 3.0T * 2 NVME |
| 测试方式 | 单次数据 | 单次数据 |

## 跑分对比

> 对比口径：`920B 整机 PQ + mmap + 早停` VS `7490 整机 + 早停 + 绑核单 NUMA`

| 规格 | ef | 并发数 | 920B QPS | 7490 QPS | QPS 提升 | 920B recall | 7490 recall | 920B latency | 7490 latency |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Cohere 1M * 768dim | 400 | 1 | 213.4411 | 98.4803 | **116.73%** | 0.9909 | 0.9886 | 0.008 | 0.016 |
| Cohere 1M * 768dim | 400 | 4 | 759.029 | 366.5071 | **107.10%** | 0.9909 | 0.9886 | 0.0098 | 0.0177 |
| Cohere 1M * 768dim | 400 | 8 | 1391.187 | 673.8784 | **106.44%** | 0.9909 | 0.9886 | 0.0109 | 0.0194 |
| Cohere 1M * 768dim | 400 | 16 | 2439.1509 | 1211.9636 | **101.26%** | 0.9909 | 0.9886 | 0.0109 | 0.0227 |
| Cohere 1M * 768dim | 400 | 32 | 4772.175 | 1771.8974 | **169.33%** | 0.9909 | 0.9886 | 0.013 | 0.03053 |
| Cohere 10M * 768dim | 400 | 1 | 165.66965 | 98.5069 | **68.18%** | 0.9787 | 0.9823 | 0.0129 | 0.0175 |
| Cohere 10M * 768dim | 400 | 4 | 726.82735 | 373.6582 | **94.52%** | 0.9787 | 0.9823 | 0.01135 | 0.0181 |
| Cohere 10M * 768dim | 400 | 8 | 1148.69405 | 696.0906 | **65.02%** | 0.9787 | 0.9823 | 0.0145 | 0.0198 |
| Cohere 10M * 768dim | 400 | 16 | 2307.127 | 1132.3973 | **103.74%** | 0.9787 | 0.9823 | 0.0151 | 0.0274 |
| Cohere 10M * 768dim | 400 | 32 | 4174.0164 | 1710.2825 | **144.05%** | 0.9787 | 0.9823 | 0.0164 | 0.0333 |

## locomo测试结果
| 指标 | 数值 |
| ---- | ---- |
| 数据集 | `data/locomo_small.json` |
| 样本数 | 1 个 conversation |
| Session 数 | 4 |
| 问题数 | 35 |
| 已评分问题数 | 35 |
| 正确数 | 29 |
| 错误数 | 6 |
| `[ERROR]` 响应数 | 0 |
| 总体准确率 | 82.86% |

分类准确率：

| Category | Correct | Total | Accuracy |
| -------- | ------- | ----- | -------- |
| 1 | 4 | 5 | 80.00% |
| 2 | 5 | 9 | 55.56% |
| 3 | 2 | 2 | 100.00% |
| 4 | 18 | 19 | 94.74% |

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

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.8 升级测试结论


| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

## 4.3 资料测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
|https://gitee.com/sundechao2/KunpengRAG/blob/master/OpenViking_OpenGauss/OpenViking%20+%20openGauss%20Docker%E9%83%A8%E7%BD%B2%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md|    通过指导文档可以正常部署ov+og     |

# 5 测试对象质量评估

本次测试主要对索引膨胀优化进行测试，覆盖功能验证、复杂使用场景下的功能是否正常，异常场景合理报错

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%    | 0%   | 0%   | 0% | 0%     |

###   5.2.2 缺陷列表
无


# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性 | 验证策略 | 是否按照测试策略执行 |
|------|------|----------|----------------------|
| 1 | 服务部署 | 拉起 OpenViking、OpenClaw、openGauss 容器并检查状态 | YES |
| 2 | 配置验证 | 检查 OpenViking `ov.conf` 和 OpenClaw `openclaw.json` | YES |
| 3 | 链路验证 | 调用 OpenClaw Gateway，触发 OpenViking 记忆流程 | YES |
| 4 | 数据验证 | 查询 openGauss `public.context` 表统计 | YES |
| 5 | 评测验证 | 执行 LoCoMo small 的 health_check、ingest、qa、judge、stats | YES |
| 6 | 风险验证 | 观察日志和分布式表元数据 | YES |


##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据 

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（k） | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ----------- | -------- |
| openGauss 7.0.0-RC3 B009 | 5               | 6        | 6         | 0          | NA          | NA  |
| OpenViking openGauss backend 0526 | 5 | 6 | 6 | 0 | NA | NA |
###  6.3.2 测试用例执行结果统计数据

| 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 6 | 6 | 6 | 0 | 0 | 0 | 100% | 100% |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1、源代码PR：

https://gitee.com/kunpeng_compute/KunpengRAG/pulls/60



2、文档pr：

https://gitee.com/kunpeng_compute/KunpengRAG/pulls/57

3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=64f8f98762e441cbac89e1e2852dac44

4、测试用例：

openGauss_7.0.0LTS->内核->openviking对接openGauss
