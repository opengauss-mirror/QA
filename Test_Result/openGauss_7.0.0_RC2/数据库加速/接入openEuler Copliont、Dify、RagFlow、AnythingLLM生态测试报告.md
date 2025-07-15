![avatar](https://gitee.com/opengauss/QA/raw/master/images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2025.7.7 |v1.0 | 测试报告初稿 | chen-czywj |

**Keywords 关键词**：openGauss DataVec、RAG

**Abstract 摘要**：本报告主要介绍opengauss向量数据库作为RAG引擎语料库，对接搭建出高效智能的助手平台的测试情况。

**缩略语清单： **

| 缩略语 | 英文全名                   | 中文解释                                                     |
| ------ | -------------------------- | ------------------------------------------------------------ |
| LLM    | Large Language Model | 大语言模型：就像 ChatGPT 这样的 AI 模型，拥有强大的语言理解和生成能力，但它们的知识局限于训练数据，且可能产生“幻觉” |
| RAG | Retrieval-Augmented Generation | 检索增强生成：一种将信息检索与 LLM 结合的框架，可以实时从外部知识库中检索相关信息，并利用 LLM 生成更准确、可靠的回答 |

***


# 1 概述

LLM (Large Language Model，大语言模型): 就像 ChatGPT 这样的 AI 模型，拥有强大的语言理解和生成能力，但它们的知识局限于训练数据，且可能产生“幻觉”（即生成不准确的信息）。
RAG (Retrieval-Augmented Generation，检索增强生成): 一种将信息检索与 LLM 结合的框架，可以实时从外部知识库中检索相关信息，并利用 LLM 生成更准确、可靠的回答。
使用openGauss DataVec向量数据库作为RAG引擎语料库，从而搭建出高效智能的助手平台。本次测试针对资料中适配的组合方案测试openGauss RAG的应用。
# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称              | 软件包名称                                  | 测试起始时间   | 测试结束时间  | 测试人员  |
| ---------------------| -------------------------------------------| ------------ | ------------ | --------- |
| openGauss 7.0.0-RC2 B012 | openGauss-All-7.0.0-RC2-openEuler22.03-aarch64.tar.gz | 2025.6.26 | 2025.7.7 | chen-czywj |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Kunpeng-920<br/>内存：30GB<br/>硬盘：56GB<br/>OS：openEuler release 22.03(LTS) |      |


# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试覆盖五个场景：openGauss DataVec + Dify 、openGauss+openEuler Intelligence 、openGauss + Ragflow 、openGauss + AnythingLLM 、openGauss + LlamaIndex
测试过程按照文档指南可以搭建示例成功，发现文档缺陷3个，总体质量良好。

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | -------------------------- | ---------------------- | ---------------------- |
| 接入openEuler Copliot、Dify、RagFlow、AnythingLLM生态 |将openGauss与RAG流程集成，不仅可以充分发挥openGauss的高效数据存储与检索优势，还能借助RAG技术实现智能化的知识问答、数据分析与内容生成，从而构建更强大的企业级数据智能解决方案。 |无                        | 无                  | 100%      | <font color=green>▮</font> | 无 |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论


###  4.2.2 可靠性测试结论

无
###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

无

###  4.2.7 兼容性测试结论

无

###  4.2.8 升级测试结论

无

## 4.3 资料测试结论

对官网相关资料进行测试，测试通过

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试主要针对openGauss作为底座对接openEuler copliont、Dify、RagFlow、AnythingLLM、LlamaIndex生态，搭建不同应用，验证搭建使用整个流程完整可用。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 0    | 3    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100%   | 0%   |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前 |
| -------- | -------- | -------- | -------- |
| [6960](https://gitcode.com/opengauss/docs/issues/6960) | 【测试类型：资料】【测试版本：7.0.0-RC2】使用openGauss部署AnythingLLM文档指南需要增加异常处理 | 次要 | 待办的 |
| [6946](https://gitcode.com/opengauss/docs/issues/6946) | 【测试类型：资料】【测试版本：7.0.0-RC2】使用openGauss部署LlamaIndex文档中拉取python依赖包缺失模型相关指导 | 次要 | 待办的 |
| [6945](https://gitcode.com/opengauss/docs/issues/6945) | 【测试类型：资料】【测试版本：7.0.0-RC2】openGauss+openEuler Intelligence的RAG架构部署按照脚本步骤部署导入镜像失败，需要增加异常处理 | 次要 | 待办的 |

# 6 测试过程评估

##  6.1 测试策略回顾

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

本次测试共发现3个issue，issue为资料问题不计入缺陷率，缺陷密度为0/KLOC，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1 特性代码PR：

无

2 测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=2493976fbcef4c1bb63fdf3983031e87&hideDevcloudHead=true

3 测试用例

4.资料连接
https://docs.opengauss.org/zh/docs/latest/docs/DataVec/llama_index.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/anythingllm.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/openGauss-Dify.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/openGauss-eulercopilot.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/openGauss-Ragflow.html
