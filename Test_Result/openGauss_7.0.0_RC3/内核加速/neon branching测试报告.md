![avatar](D:\l30072648\QA\images\openGauss.png)

版权所有 © 2025  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者 |
| --------- | -------- | ------------ | ---- |
| 2026-1-14 | V1.0     | 测试报告初版 | 林强 |

[TOC]

**Keywords 关键词**：neon branching，分支，计算节点

**Abstract 摘要**：本文档主要对neon branching特性功能、可靠性等测试内容及结论，以及测试过程发现issue等情况进行说明。

**缩略语清单： **

| 缩略语             | 英文全名               | 中文解释                                                     |
| ------------------ | ---------------------- | ------------------------------------------------------------ |
| neon branching特性 | Neon Branching Feature | 以存算分离为核心架构，实现类似Git分支管理的数据库分支管理能力 |

# 1 概述

本报告主要测试neon branching特性，包括neon branching功能测试，数据库功能测试。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC3 build 3cfd8054 | openGauss  | 2025/12/31   | 2026/1/14    | 林强     |

### 2.1.2 配套测试的版本

无

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号                                          | 硬件配置信息                                                 | 备注                                         |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- |
| 物理机   | ARM+openEuler <br />TaiShan 200 (Model 2280 7260) | Kunpeng-920<br/>内存：1006GB<br/>硬盘：7.3TB<br/>OS：openEuler release 22.03(LTS) | 需求对环境配置信息要求不高，低配置环境也可以 |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试neon branching特性功能，测试详情如下：

- neon branching功能测试：输出测试用例20个
- 数据库功能测试：执行测试用例5728条

需求覆盖100%，用例执行100%，总计发现issue共8个，8个问题待解决；开发质量一般。

## 3.2 约束说明

- 不支持Ustore。
- 当前仅支持单机。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性           | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                                 | 主要风险 |
| -------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | ------------------------------------------------------------ | -------- |
| neon branching | 解决开发测试环境搭建低效，传统数据库备份时间长，存储成本高的痛点，AI应用与多开发者协作开发对并行试验以及数据库隔离以及数据快速回溯需求激增。 | 见3.2章节                  | 待解决问题8个        | 100%             | <font color=green><font color=yellow><font color=yellow>▲</font></font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 功能测试结论

| 测试步骤                                                   | 测试结果               |
| ---------------------------------------------------------- | ---------------------- |
| neon branching分支创建以及管理，覆盖异常场景的分支创建     | pass                   |
| neon branching租户创建以及管理，覆盖不同租户的数据隔离     | pass                   |
| neon branching计算节点创建以及管理，覆盖不同分支的数据隔离 | pass                   |
| openGauss数据库功能测试，覆盖openGauss常用的功能           | fail(部分用例没有通过) |

### 4.2.2 性能测试结论

不涉及

### 4.2.3 可靠性测试结论

| 测试步骤                           | 测试结果 |
| ---------------------------------- | -------- |
| 重复启停计算节点，可以正常查询数据 | pass     |

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

不涉及

# 5 测试对象质量评估

## 5.1 覆盖率分析

需求测试涉及覆盖率100%，测试执行覆盖率100%。

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

| 问题总数 | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| -------- | -------- | ---- | ---- | ---- | ------ |
| 数目     | 8        | 0    | 4    | 4    | 0      |
| 百分比   | 8        | 0%   | 50%  | 50%  | 0%     |

### 5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 | 发现问题版本                       | 解决版本 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- | ---------------------------------- | -------- |
| [7871](https://gitcode.com/opengauss/openGauss-server/issues/7871) | [Bug]: [neon branching特性]gms_utility.analyze_schema功能，ESTIMATE_ROWS值过大，计算节点crash | 主要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7870](https://gitcode.com/opengauss/openGauss-server/issues/7870) | [Bug]: [neon branching特性]回收站功能启用之后，执行sql ，计算节点crash | 主要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7869](https://gitcode.com/opengauss/openGauss-server/issues/7869) | [Bug]: [neon branching特性]分区表插入数据失败（smgrextend错误） | 次要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7867](https://gitcode.com/opengauss/openGauss-server/issues/7867) | [Bug]: [neon branching特性]D兼容数据库创建后无法连接         | 次要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7866](https://gitcode.com/opengauss/openGauss-server/issues/7866) | [Bug]: [neon branching特性] 创建增量物化视图、普通物化视图，计算节点crash | 主要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7845](https://gitcode.com/opengauss/openGauss-server/issues/7845) | [Bug]: neon branching创建段页式表时，计算节点crash           | 主要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7841](https://gitcode.com/opengauss/openGauss-server/issues/7841) | [Bug]: neon branching 开启LFC参数，但是实际本地目录并没有file.cache文件生成 | 次要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |
| [7840](https://gitcode.com/opengauss/openGauss-server/issues/7840) | [Bug]: [测试类型 功能测试] neon branching 使用创建main计算节点跑TPCC测试失败，多线程导入数据超时 | 次要     | 待办的   | openGauss 7.0.0-RC3 build 3cfd8054 | latest   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性                    | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ----------------------- | ------------------------------------------------------------ | -------------------- |
| 1    | neon branching功能      | neon branching 分支、租户、计算节点管理功能验证，数据隔离性验证 | YES                  |
| 2    | openGauss数据库功能验证 | 覆盖数据库常用模块的Yat用例测试                              | YES                  |
| 3    | 可靠性                  | 重复启停计算节点，启动计算节点后数据查询正常                 | YES                  |

## 6.2 测试设计评估

无

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-RC3 build 3cfd8054 | 12               | 5748       | 5748       | 8          | 16             | 0.5      |

*数据项说明：*

- 缺陷数总计8个，计算缺陷密度缺陷数总计8个。

### 6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 5748           | 5748             | 4762   | 986    | 0       | 0           | 100%   | 82.9%      |

# 7 附件

## 7.1 附件1：遗留问题列表

无

## 7.2 附件2：特性相关PR

代码PR：

​	https://gitcode.com/wlff234/neon_branch_dev/commit/8fecf347901d963f5bde37b54186680d0d7d6438?ref=neon_release_1230

​	https://gitcode.com/wlff234/neon_branch_dev/commit/23dedec6757c79ba78237b09a4a4795ad4f4bfac?ref=neon_release_1230

​	https://gitcode.com/wlff234/neon_branch_dev/commit/62a6df668e3aadae9a43498418a87b5f617e150f?ref=neon_release_1230

​	https://gitcode.com/wlff234/neon_branch_dev/commit/add47b650d474272329d687164329af3b0338254?ref=neon_release_1230

​	https://gitcode.com/wlff234/neon_branch_dev/commit/b9e9eee8703d1bd3c3de2896ae5a9cda081883ac?ref=neon_release_1230

​	https://gitcode.com/wlff234/openGauss-server/commit/9dad1014382cd45c3e51c749e8489edebac4f16e?ref=og_neon_release_1230

​	https://gitcode.com/wlff234/openGauss-server/commit/c40a1e278fe6a9e4e8afc2334a8bbdc9240434ea?ref=og_neon_release_1230

​	https://gitcode.com/wlff234/openGauss-server/commit/e8f23e99bed920f44f9bf7358d5db13d99e6bf24?ref=og_neon_release_1230

​	https://gitcode.com/wlff234/openGauss-server/commit/21a4d89c223fac9e991bf938679b4129853e600a?ref=og_neon_release_1230

测试设计归档testplan：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=bc7ad5268b484be98ddd5514b5234f19

测试用例归档testplan：https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb2100010u4mjc78

目录：/特性目录/内核加速/neon branching特性
