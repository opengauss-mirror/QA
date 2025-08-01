![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改章节     | 修改描述     | 作者   |
| ---------- | -------- | ------------ | ------------ | ------ |
| 2024.9.11  | 1.0      | 初稿撰写     |              | zhanbiao.chen |

目 录

1 概述

2 测试版本说明

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论

5 问题单统计

6 附件

7 致谢

**Keywords 关键词**：openGauss 6.0.0 release

**Abstract 摘要**：主要是描述了openGauss 6.0.0 release版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                    | 中文解释       |
| ------ | --------------------------- | -------------- |
| 无    |    |  |

 

***


# 概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 6.0.0 release版本新增A兼容性特性（具体特性名可以查看"测试版本说明"章节中的需求列表），并对若干关键缺陷进行了修改。本文主要描述了openGauss 6.0.0 release版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 6.0.0 release版本整体质量良好。

# 测试版本说明

本章节描述测试版本的基本信息，包括测试对象是什么，以及在什么环境下开展的测试，具体包括被测版本和测试硬件环境。

描述被测对象的版本信息和测试的时间及测试轮次。

| 版本名称                          | 测试起始时间 | 测试结束时间 |
| --------------------------------- | ------------ | ------------ |
| openGauss 6.0.0 release   | 2024-07-15   | 2024-08-15   |

描述本次测试的测试环境（包括环境软硬件版本信息，环境组网配置信息， 测试辅助工具等）。

| 环境信息 | 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Centos_x86 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

openGauss 6.0.0 release版本是openGauss社区继续联合多方力量推出的版本，本次发布的需求列表、分工如下：

| 序号 | 需求                                                         | 开发主体      | 测试主体 | 验证策略                                                     |
| ---- | ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 1    | 【openGauss 6.0.0 release】【兼容性】支持gms_output包             | SQLEngine     | QA       | 1、验证接口函数功能<br/>2、验证接口函数在函数、存储过程、匿名块中的使用<br/> |

# 版本概要测试结论

openGauss 6.0.0 release版本按照测试策略完成了需求功能验证，测试任务按计划完成。本版本计划交付需求1个，实际交付1个，交付率100%，所有发布需求均验证通过。

openGauss 6.0.0 release版本共发现问题0个，有效问题0个。版本整体质量良好。无遗留问题。

# 版本详细测试结论

openGauss 6.0.0 release版本详细测试内容包括：


1、在A\*兼容性方面，测试新增A兼容下gms_output包需求，A\*兼容性进一步提升。


## 特性测试结论

### 继承特性评价

无

### 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估，给出特性质量评估结论。

| 特性名称                                                     | 测试情况说明                                                 | 约束                                                         | 质量电灯                    | 遗留问题 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | -------- |
| 【openGauss 6.0.0 release】【兼容性】A兼容性增强             | A兼容性增强特性，共计执行26个用例，主要覆盖功能测试、升级测试和资料测试，通过手动执行用例，未发现功能问题，升级后执行用例，未发现功能问题，资料验证通过，无遗留风险，整体质量良好； | 特性使用时涉及到的约束及限制条件。需安装gms_output插件 | <font color=green>▮</font>  | 无       |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 专项测试结论

### 安全测试

无

### 可靠性测试

无

### 性能测试

无

### 兼容性测试

无

#### 升级兼容

无

#### 硬件兼容

无

#### 软件兼容

无

### 资料测试

无

# 问题单统计

openGauss 6.0.0 release版本共发现问题21个，有效问题21个，无效问题0个。修复问题回归测试结果正常，版本整体质量良好。无遗留问题。 

| 版本名称                         | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| -------------------------------- | ------------ | ------------ | ---------- | ---------- |
| openGauss 6.0.0 release Test round 1 | 2024-07-15   | 2023-08-15   | 21        | 0         |



# 附件

## 附件1：遗留问题列表

无

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@justbk](https://gitee.com/justbk)
+ [@wan005](https://gitee.com/wan005)
+ [@peilinqian](https://gitee.com/peilinqian)
+ [@liu-tong-8848](https://gitee.com/liu-tong-8848)
+ [@li-xin12345](https://gitee.com/@li-xin12345)
