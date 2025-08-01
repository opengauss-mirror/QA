![avatar](https://gitee.com/opengauss/QA/raw/master/images/openGauss.png)

版权所有 © 2024 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2025.2.10 | v1.0     | 测试报告初稿 | xiaqi14 |

**Keywords 关键词**： **GMS_RAW、GMS_MATCH** 

**Abstract 摘要**：gms_raw是一个基于openGauss的插件，用于RAW类型的sql函数。gms_match比较两个字符串的相似度。目前支持的接口有：
GMS_RAW.BIT_AND
GMS_RAW.BIT_COMPLEMENT
GMS_RAW.BIT_OR
GMS_RAW.BIT_XOR
GMS_RAW.CAST_FROM_BINARY_DOUBLE
GMS_RAW.CAST_FROM_BINARY_FLOAT
GMS_RAW.CAST_FROM_BINARY_INTEGER
GMS_RAW.CAST_FROM_NUMBER
GMS_RAW.CAST_TO_BINARY_DOUBLE
GMS_RAW.CAST_TO_BINARY_FLOAT
GMS_RAW.CAST_TO_BINARY_INTEGER
GMS_RAW.CAST_TO_NUMBER
GMS_RAW.CAST_TO_NVARCHAR2
GMS_RAW.CAST_TO_RAW
GMS_RAW.CAST_TO_VARCHAR2
GMS_RAW.COMPARE
GMS_RAW.CONCAT
GMS_RAW.CONVERT
GMS_RAW.COPIES
GMS_RAW.TRANSLATE
GMS_RAW.TRANSLITERATE
GMS_RAW.XRANGE
GMS_RAW.REVERSE
GMS_match.edit_distance   
GMS_match.edit_distance_similarity

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 概述

此特性主要用来支持在A兼容模式下，支持gms_raw高级包，这主要用于RAW类型的sql函数。gms_match用于比较两个字符串的相似度。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build 96ee5da3 | openGauss  | 2025-1-6     | 2024-1-24    | xiaqi14  |

### 2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注 |
| -------- | ----------------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | aarch64+openEuler | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>MEM: 32GB<br/>DISK: 200GB<br/>OS: openEuler 20.03 (LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

##  3.1 测试结论总结

openGauss 兼容A的gms_raw特性，共计执行了205个用例，主要覆盖了功能测试、升级测试、兼容性测试、资料测试。功能测试覆盖了插件的创建、删除，插件包的25个接口函数功能，以及接口函数在不同场景下的验证；升级测试在6.0.0版本升级到7.0.0-RC1.B008版本后执行用例验证此功能的正确性；资料测试验证描述、约束、示例是否完善、有效。无遗留问题，整体质量良好。

## 3.2 约束说明

- 仅支持在A模式数据库创建。
- 仅支持`create extension`命令方式加载插件。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性                          | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                                 | 特性质量评估 | 主要风险 |
| ----------------------------- | -------------------------- | -------------------------- | -------------------- | ------------------------------------------------ | ------------ | -------- |
| 【兼容性组】支持GMS_RAW高级包 | 提升兼容能力，方便用户迁移 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试、兼容性测试、升级测试和资料测试 | ▮            | 无       |

*特性质量评估说明*：

●： *表示特性不稳定，风险高*

▲： *表示特性基本可用，遗留少量问题*

▮： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.3 安全&隐私保护测试结论

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

### 4.2.7 兼容性测试结论

| 测试步骤                                              | 测试结果                                |
| ----------------------------------------------------- | --------------------------------------- |
| 1. 在兼容"B"、"C"、"PG"类型数据库下创建gms_xmlgen插件 | 执行3条用例，执行结果符合预期，测试通过 |

### 4.2.8 升级测试结论

| 测试步骤                             | 升级路径    | 测试结果                                |
| ------------------------------------ | ----------- | --------------------------------------- |
| 1. 6.0.0版本升级到7.0.0-RC1.B008版本 | 6.0.0-7.0.0 | 执行2条用例，执行结果符合预期，测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                              | 测试结论   |
| ---- | ------------------------------------- | ---------- |
| 1    | 插件参考>gms_rawExtension>gms_raw概述 | 未发现问题 |
| 2    | 插件参考>gms_rawExtension>gms_raw限制 | 未发现问题 |
| 3    | 插件参考>gms_rawExtension>gms_raw安装 | 未发现问题 |
| 4    | 插件参考>gms_rawExtension>gms_raw使用 | 未发现问题 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试主要针对gms_raw高级包功能覆盖了25个接口函数的功能验证、复杂使用场景下的功能是否正确，异常场景合理报错

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                  | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------- | -------- | -------- |
| [IB4NFN](https://gitee.com/opengauss/openGauss-server/issues/IB4NFN?from=project-issue&search_text=gms_xmlgen) | gsm_raw相关约束资料需添加 | 次要     | 已验收   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性             | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------------- | ------------------------------------------------------------ | -------------------- |
| 1    | 插件创建和删除   | 在兼容A库下创建、删除插件                                    | YES                  |
| 2    | 接口函数功能验证 | 接口函数基础功能及在复杂场景下的验证                         | YES                  |
| 3    | 兼容验证         | 在非兼容A库下执行用例，验证是否有合理报错提示，数据库正常运行 | YES                  |
| 4    | 升级验证         | 从6.0.0升级到7.0.0之后执行用例验证功能是否正确               | YES                  |
| 5    | 资料验证         | 资料描述与需求相符，示例是否正确无误                         | YES                  |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度   |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | ---------- |
| openGauss 7.0.0-RC1 build 96ee5da3 | 16               | 205        | 205        | 1          | 6.112  | 0.163/kloc |

### 6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 205          | 205              | 205    | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

## 7.1 附件1：遗留问题列表

无

## 7.2 附件2：特性相关PR

1、源代码PR：https://gitee.com/opengauss/openGauss-server/pulls/6775/files

2、文档pr：https://gitee.com/opengauss/docs/pulls/6944

3、测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=280512c63e3d43bb9739a487e4fcb774&hideDevcloudHead=true