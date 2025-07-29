![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述 | 作者   |
| --------- | -------- | -------- | ------ |
| 2025/7/28 | 1.0.0    |          | 姚雨驰 |

[TOC]

**Keywords 关键词**：数据迁移，Datakit，openDesign

**Abstract 摘要**：本次测试的测试范围为datakit迁移插件的迁移任务中心前端界面，主要覆盖功能测试和ui测试。本次测试发现问题11个，开发质量良好

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告为迁移插件前端交互界面按openDesign UI组件库优化特性测试报告，主要测试对象为datakit的迁移插件，测试版本为7.0.0-RC2

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称  | 软件包名称                   | 测试起始时间 | 测试结束时间 | 测试人员 |
| --------- | ---------------------------- | ------------ | ------------ | -------- |
| 7.0.0-RC2 | Datakit-All-7.0.0-RC2.tar.gz | 2025/7/4     | 2025/7/16    | 1        |

###  2.1.2 配套测试的版本

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 硬件型号                      | 硬件配置信息                                                 | 备注 |
| ----------------------------- | ------------------------------------------------------------ | ---- |
| openEuler release 20.03 (LTS) | CPU：Intel(R) Xeon(R) Platinum 8378C CPU @ 2.80GHz<br />内存：30G<br />硬盘：100G<br />OS：openEuler x86_64 |      |

### 2.2.2 虚拟化平台

### 2.2.3 OS版本

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试完成了功能测试和资料测试，需求100%实现，所有被测特性的测试用例累计执行覆盖率达到100%，发现问题11个，回归测试均已通过，开发质量良好

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                          | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                                     | 特性质量评估               | 主要风险               |
| --------------------------------------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------------------------------------------- | -------------------------- | ---------------------- |
| 迁移插件前端交互界面按openDesign UI组件库优化 | 当前Datakit与其他项目前端设计风格不一致，各插件内部使用组件不同，页面风格不统一。在Datakit上集成openDesign组件库，以提高插件前端页面的统一性 | 无                         | 无                   | 覆盖原有迁移功能和新ui的表现形式，保证迁移正常进行。 | <font color=green>▮</font> | 如：可靠性、性能无保证 |

## 4.2 产品质量属性目标(DFX)测试结论

无

###  4.2.1 性能测试结论

无

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

| 测试步骤                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 1.旧版datakit上创建迁移任务，其中包含多个子任务。2.datakit更新到最新版，迁移任务能正常显示所有子任务 | 成功     |

###  4.2.8 升级测试结论

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论   |
| ---- | ------------------------------------------------------------ | ---------- |
|      | openGauss-workbench仓库：plugins/data-migration/doc/DataKit Product Manual - Data Migration .md | 开发修改中 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试覆盖数据迁移的全流程，包括离线迁移，在线迁移，portal的安装及任务创建的普通场景及异常场景

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 11       | 0    | 1    | 10   | 0      |
| 百分比 | 100%     | 0    | 9%   | 91%  | 0      |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1052 | [Bug]: datakit离线迁移任务完成后任务无法重置                 | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1043 | [Bug]: datakit全量迁移一张空表时，迁移速率显示为undefinded   | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1032 | [Bug]: 迁移任务中心使用任务状态进行搜索，已成功和已停止的搜索结果异常 | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1028 | [Bug]: datakit迁移任务中心把自动刷新关闭后启动任务，再次打开自动刷新后，子任务状态不会刷新 | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1058 | [Bug]: datakit迁移任务详情中，进行到反向迁移时，需要添加结束任务的按钮 | 次要     | 待办的   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1026 | [Bug]: datakit增量迁移停止后，需要将一键修复按钮隐藏或禁用   | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1024 | [Bug]: datakit创建迁移任务时，配置自定义参数的高级参数设置页面异常 | 主要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1021 | [Bug]: datakit迁移详情界面文本描述有误                       | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1018 | [Bug]: datakit创建迁移任务时，任务名为全空格，创建任务未报错 | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1016 | [Bug]: datakit创建迁移任务时，填写完迁移信息后，将窗口滑动至最下方，点击下一步，页面跳转后窗口任然在最下方 | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1014 | [Bug]: datakit创建迁移任务时，搜索迁移表时，搜索结果第二页的表前端未加载 | 次要     | 已验收   |

# 6 测试过程评估

*本章节对测试过程进行评估，包括测试策略、测试设计和测试执行，目的是审视测试过程的执行情况，分析有待进一步开展测试和需要改进的地方。*

##  6.1 测试策略回顾

| 编号 | 特性     | 验证策略                                   | 是否按照测试策略执行 |
| ---- | -------- | ------------------------------------------ | -------------------- |
| 1    | 功能测试 | 针对迁移任务创建，迁移详情页面功能进行测试 | YES                  |
| 2    | ui测试   | 针对报错提示、文本描述以及ui合理性进行测试 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称              | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| --------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| Datakit-All-7.0.0-RC2 | 10               | 104        | 104        | 11         | 7.118          | 1.54     |
|                       |                  |            |            |            |                |          |

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 104            | 104              | 92     | 12     | 0       | 0           | 100%   | 88%        |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

代码pr:

https://gitcode.com/opengauss/openGauss-workbench/pull/1119

https://gitcode.com/opengauss/openGauss-workbench/pull/1135

https://gitcode.com/opengauss/openGauss-workbench/pull/1136

https://gitcode.com/opengauss/openGauss-workbench/pull/1137

https://gitcode.com/opengauss/openGauss-workbench/pull/1141

https://gitcode.com/opengauss/openGauss-workbench/pull/1154

https://gitcode.com/opengauss/openGauss-workbench/pull/1158

https://gitcode.com/opengauss/openGauss-workbench/pull/1159

https://gitcode.com/opengauss/openGauss-workbench/pull/1163

测试用例：

![image-20250728165121619](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250728165121619.png)

测试设计：

![image-20250728165043862](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250728165043862.png)

