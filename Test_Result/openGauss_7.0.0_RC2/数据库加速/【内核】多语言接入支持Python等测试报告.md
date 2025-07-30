![avatar](../../images/openGauss.png)

版权所有 © 2025  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2025.7.16 |v1.0 | 测试报告初稿 | nanyang |

**Keywords 关键词**：Python，Java，Node.js，Go，C#，C++，SDK，向量数据库

**Abstract 摘要**：本文档是对多语言接入支持Python等特性进行的测试。测试Python，Java，Node.js，Go，C#，C++等6种语言对接向量数据库，主要进行功能测试、资料测试。

1、已实现Python，Java，Node.js，Go，C#，C++等语言对接向量数据库。

2、已补充多语言对接向量数据库资料。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|  NA    |          |          |

***


# 1 概述

该测试需求的目的是适配多种语言SDK对接openGauss向量数据库。本需求完成适配Python，Java，Node.js，Go，C#，C++等6种语言对接向量数据库，并在官方文档中增加相关资料，资料中的代码示例，可以指导多语言用户更方便的使用openGauss向量数据库。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

*本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段*

| 版本名称              | 软件包名称                                  | 测试起始时间   | 测试结束时间  | 测试人员  |
| ---------------------| -------------------------------------------| ------------ | ------------ | --------- |
| openGauss 7.0.0-RC2 B012 | openGauss-All-7.0.0-RC2-openEuler20.03-x86_64.tar.gz | 2025.7.2  | 2025.7.8   | nanyang |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | --------- | ------------------------------------------------------------ | ---- |
| 物理机 | x86_64+openEuler20.03<br /> | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：200G<br />硬盘：NVME 3.7T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 | |


# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

openGauss多语言SDK对接向量数据库特性，测试项包含Python SDK对接向量数据库，Java SDK对接向量数据库，Node.js SDK对接向量数据库，Go SDK对接向量数据库，C# SDK对接向量数据库，C++ SDK对接向量数据库等，并对相关资料进行验证。发现资料问题6个，整体质量一般。

## 3.2 约束说明

无

## 无论

无

# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 【内核】多语言接入支持Python等测试报告 |本需求完成适配Python，Java，Node.js，Go，C#，C++等6种语言对接向量数据库，并在官方文档中增加相关资料，资料中的代码示例，可以指导多语言用户更方便的使用openGauss向量数据库。 |详见3.2章节描述                        | 无                  | 资料测试         | <font color=green>▮</font> |  |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

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

无

###  4.2.8 升级测试结论

无

## 4.3 资料测试结论

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationPython.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationJava.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationJavaScript.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationGo.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationCsharp.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationCpp.html

本需求主要对多语言对接向量数据库6个页面资料进行测试，包含Python SDK对接向量数据库，Java SDK对接向量数据库，Node.js SDK对接向量数据库，Go SDK对接向量数据库，C# SDK对接向量数据库，C++ SDK对接向量数据库等页面资料测试，发现资料问题6个。

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次需求覆盖本需求主要对多语言对接向量数据库6个页面资料进行测试，包含Python SDK对接向量数据库，Java SDK对接向量数据库，Node.js SDK对接向量数据库，Go SDK对接向量数据库，C# SDK对接向量数据库，C++ SDK对接向量数据库等页面资料测试。主要包含资料测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 6    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
|    | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】C++ SDK对接向量数据库页面，部分函数定义错误跑不通、部分拼写错误](https://gitcode.com/opengauss/docs/issues/6968)sig/Community | 次要 | 已验收 |
| | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】C# SDK对接向量数据库页面，部分语法错误、拼写错误](https://gitcode.com/opengauss/docs/issues/6962) | 次要 | 已验收 |
| | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】Java SDK对接向量数据库页面，部分示例语法错误](https://gitcode.com/opengauss/docs/issues/6954) | 次要 | 已验收 |
| | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】Python SDK对接向量数据库资料中，限制python版本为3.11及以上，但在OM安装openGauss时，除了openEuler24.03环境外，其余安装环境仅支持python3.6-3.10版本，需要适配低版本Python](https://gitcode.com/opengauss/docs/issues/6953) | 次要 | 已验收 |
| | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】Go SDK对接向量数据库资料中，安装go SDK部分，用gitcode仓库有问题，gitee正常。以及部分资料描述错误需要更新](https://gitcode.com/opengauss/docs/issues/6952) | 次要 | 已验收 |
| | [[Bug\]: 【测试类型：资料】【测试版本：7.0.0-RC2】Node.js SDK对接向量数据库页面，部分示例语法错误](https://gitcode.com/opengauss/docs/issues/6961) | 次要 | 已验收 |

# 6 测试过程评估

##  6.1 测试策略回顾


| 编号 | 特性      | 验证策略                                         | 是否按照测试策略执行 |
| ---- | ----     | ------------------------------------------------ | -------------------- |
| 1    | 资料测试 | 主要对多语言对接向量数据库6个页面资料进行测试，包含Python SDK对接向量数据库，Java SDK对接向量数据库，Node.js SDK对接向量数据库，Go SDK对接向量数据库，C# SDK对接向量数据库，C++ SDK对接向量数据库等页面资料测试 | YES             |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ |
| openGauss 7.0.0-RC2 B012 | 5                | 0          | 0          | 6          | 0k     |

本次测试共发现6个issue均为资料单，缺陷密度为0/0k=0，整体质量一般。

###  6.3.2 测试用例执行结果统计数据

|        | 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked |
| ------ | ------------ | ---------------- | ------ | ------ | ------- |
| 第一轮 | 0            | 0                | 0      | 0      | 0       |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1 特性代码PR：

无

2 测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=6f8772f9f8154389a3e2f2cfd6f6645a&hideDevcloudHead=true

3 测试用例：

无

4.资料地址：

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationPython.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationJava.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationJavaScript.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationGo.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationCsharp.html

https://docs.opengauss.org/zh/docs/latest/docs/DataVec/integrationCpp.html
