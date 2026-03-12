![avatar](../../images/openGauss.png)

版权所有 © 2025  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者 |
| --------- | -------- | ------------ | ---- |
| 2026-3-2 | V1.0     | 测试报告 | 徐达标 |

[TOC]

**Keywords 关键词**：Python，多模态融合检索

**Abstract 摘要**：本报告是对Python支持多模态融合检索特性进行测试的结论总结, 主要对如下功能点进行测试: (1) 统一多模数据治理测试. (2) 多模态融合检索测试. (3) AI 模型服务集成测试.

**缩略语清单： **

| 缩略语  | 英文全名   | 中文解释     |
| ------- | --------- | ------------ |
|         |           |              |

# 1 概述

本报告主要测试Python支持多模态融合检索特性，版本为7.0.0-RC3 B018，主要从功能测试进行特性验证。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                           | 软件包名称       | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC3 B018           | openGauss-Python | 2026/02/24   | 2026/02/28   | 徐达标    |

### 2.1.2 配套测试的版本

无

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号    | 硬件配置信息                                                                   | 备注   |
| -------- | -----------| ----------------------------------------------------------------------------- | -----  |
| 物理机   | ARM+openEuler | Kunpeng-920<br/>内存：477GB<br/>硬盘：3.6TB<br/>OS：openEuler release 20.03(LTS) |       |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试Python支持多模态融合检索特性功能，共计执行了用例174个，主要覆盖了功能测试。
功能测试主要覆盖了统一多模数据治理接口，其中包括表相关接口：create_table、drop_table、list_tables、describe_table；索引相关接口：create_index、drop_index、list_indexes；
DML接口insert、update、delete、query；多模态融合检索接口：vector_search、fulltext_search、hybrid_search；AI 模型服务接口：embed、rerank、chat.

测试中发现了1个问题，代码量3K，缺陷密度0.33，相关问题单正在修改中，整体质量良好.

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性           | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                                 | 主要风险 |
| -------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | ------------------------------------------------------------ | -------- |
| Python支持多模态融合检索特性 | 通过Python包提供openGauss数据库表、索引数据的统一管理与多模数据检索能力，提供调用第三方AI模型的能力。 | 见3.2章节                  | 无        | 100%             | <font color=green><font color=yellow><font color=green>▮</font></font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 功能测试结论

| 功能点验证                          | 是否覆盖 |
| --------------------------------- | -------- |
|  create_table                     | YES      |
|  drop_table                       | YES      |
|  list_tables                      | YES      |
|  describe_table                   | YES      |
|  create_index                     | YES      |
|  drop_index                       | YES      |
|  list_indexes                     | YES      |
|  insert                           | YES      |
|  update                           | YES      |
|  delete                           | YES      |
|  query                            | YES      |
|  vector_search                    | YES      |
|  fulltext_search                  | YES      |
|  hybrid_search                    | YES      |
|  embed                            | YES      |
|  rerank                           | YES      |
|  chat                             | YES      |


### 4.2.2 性能测试结论

不涉及

### 4.2.3 可靠性测试结论

不涉及

### 4.2.4 安全&隐私保护测试结论

不涉及

### 4.2.5 生命周期管理测试结论

不涉及

### 4.2.6 韧性测试结论

不涉及

### 4.2.7 兼容性测试结论

不涉及

### 4.2.8 升级测试结论

不涉及

## 4.3 资料测试结论

| 序号      | 测试章节                                                        | 测试结论     |
| --------- |----------------------------------------------------------------|-------------|
| 官网资料   | 文档中心>向量数据库>工具编排使用>API reference>Python SDK 多模检索 | 整体质量良好 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

| 问题总数 | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| -------- | -------- | ---- | ---- | ---- | ------ |
| 数目     | 1         | 0    | 0    | 1    | 0      |
| 百分比   | 100%      | 0%    | 0%    | 100%  | 0%    |

### 5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                    | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------ | -------- | -------- |
| [7988](https://gitcode.com/opengauss/openGauss-server/issues/7988) | [Bug]: create_table添加表注释无效      | 次要     | 待办的   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性                    | 验证策略                              | 是否按照测试策略执行 |
| ---- | ----------------------- | -------------------------------------| ------------------ |
| 1    | 基本功能验证             | 接口功能在基础使用场景下是否生效        | YES                |
| 2    | 资料验证                 | 文档资料是否完整                      | YES                |
## 6.2 测试设计评估

无

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-RC3 B018           | 5                | 174        | 174        | 1          | 3              | 0.33     |

### 6.3.2 测试用例执行结果统计数据

| 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 174            | 174              | 173    | 1      | 0       | 0           | 100%   | 99.4%      |

# 7 附件

## 7.1 附件1：遗留问题列表

无

## 7.2 附件2：特性相关PR

1.源代码PR:

https://gitcode.com/opengauss/openGauss-connector-python-psycopg2/pull/64

https://gitcode.com/opengauss/openGauss-connector-python-psycopg2/pull/65

https://gitcode.com/opengauss/openGauss-connector-python-psycopg2/pull/66

https://gitcode.com/opengauss/openGauss-connector-python-psycopg2/pull/68

https://gitcode.com/opengauss/openGauss-connector-python-psycopg2/pull/69

2.文档PR：

https://gitcode.com/opengauss/docs/pull/8426


3.测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=e496e4fbe6ae4f32979f88756917ab4a

4.测试用例：

openGauss_7.0.0RC3->内核加速->Python支持多模态融合检索特性