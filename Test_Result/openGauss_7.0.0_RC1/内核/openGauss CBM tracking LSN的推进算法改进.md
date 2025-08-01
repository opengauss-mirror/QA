![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改描述     | 作者    |
| -------- | -------- | ------------ | ------- |
| 2025.1.6 | v1.0     | 测试报告初稿 | haomeng |

**Keywords 关键词**：CBM

**Abstract 摘要**：本文档主要介绍openGauss在传统主备、资源池化环境下，开启CBM tracking功能后，集群内业务正常执行以及备份功能的验证测试结果；

**缩略语清单：**

| 缩略语 | 英文全名          | 中文解释 |
| ------ | ----------------- | -------- |
| CBM    | changed block map |          |


# 1 概述

本报告主要测试2种不同架构（传统集群、资源池化集群）下，开启CBM tracking增量备份功能下，全量备份，增量备份，以及xlog日志解析&回收流程的功能测试。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                            | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ----------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build eb367cc7  | openGauss  | 2024-12-26   | 2024-12-30   | haomeng  |
| openGauss 7.0.0-RC1 build  1d325dd9 | openGauss  | 2024-12-30   | 2025-1-6     | haomeng  |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |
| 磁阵     | Dorado 18500 V6                                              |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试传统集群&资源池化集群内开启enable_cbm_tracking参数，打开增量备份功能，在业务运行中进行全量、增量备份，主要包含功能场景测试，输出测试用例10个，执行测试2轮，发现issue共3个，所有问题已修复并回归通过，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【资源池化】CBM tracking LSN的推进算法改进，使之更平滑，减少对XLog清理的影响 | 对开启CBM功能后，在原有逻辑中能够与加快Xlog Read and Parse速度 | 无                         | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

无

### 4.2.2 功能测试结论

分别针对两种不同架构下，开启enable_cbm_tracking增量备份功能进行验证，以及新增cbm相关参数的功能和参数取值等，验收通过。

| 测试步骤                                                     | 测试结果                           |
| ------------------------------------------------------------ | ---------------------------------- |
| cbm_xlog_files_epoch、cbm_threads_num参数值验证              | 取值与资料描述保持一致，可正常设置 |
| cbm_threads_num功能验证                                      | cbm并发线程与实际配置值保持一致    |
| 传统集群模式下，开启enable_cbm_tracking，运行跑批业务，进行定时全量&增量备份 | 业务运行正常，备份正常             |
| 资源池化模式下，开启enable_cbm_tracking，运行跑批业务，进行定时全量&增量备份 | 业务运行正常，备份正常             |
| cbm相关DFX函数                                               | 执行正常                           |

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

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [cbm_xlog_files_epoch](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseReference/%E5%A4%87%E4%BB%BD%E6%81%A2%E5%A4%8D.html#cbm_xlog_files_epocha-namesection1232751104711a) | 测试通过 |
| 2    | [cbm_threads_num](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseReference/%E5%A4%87%E4%BB%BD%E6%81%A2%E5%A4%8D.html#cbm_xlog_files_epocha-namesection1232751104711a) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖传统集群和资源池化集群2种架构模式下，开启cbm功能之后全量/增量备份测试以及大批量业务下xlog回收验证测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBE11G](https://e.gitee.com/opengaussorg/issues/table?issue=IBE11G) | 【CBM tracking LSN】cbm_xlog_files_epoch参数，不同设置方式下有效值范围提示不一致 | 次要     | 已验收   |
| [IBEKSD](https://e.gitee.com/opengaussorg/issues/table?issue=IBEKSD) | 【CBM tracking LSN】增量备份因invalid CBM page header偶现失败 | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | cbm参数功能，参数设置方式，参数取值范围验证，DFX函数功能验证 | YES                  |
| 2    | 稳定性测试 | 执行tpcc业务，7*24H，开启定时全量&增量备份任务               | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC1 B010 | 4                | 12         | 12         | 1          | 0.956K | 1        |
| openGauss 7.0.0-RC1 B011 | 5                | 8          | 8          | 1          | 0.956K | 1        |

本次测试共发现2个issue，部分已修复并回归通过，缺陷密度为2/1.292k=1.5，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 12           | 12               | 11     | 1      | 0       | 0           | 100%   | 91.7%      |
| 8            | 8                | 6      | 2      | 0       | 0           | 100%   | 75%        |

本次测试共输出测试用例20个，执行测试共2轮，发现issue共2个，所有问题已修复并回归通过，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitee.com/opengauss/openGauss-server/pulls/6837

资料pr：

https://gitee.com/opengauss/docs/pulls/7084

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=bf38bc61406d4b4d87b58366d8a0444e&hideDevcloudHead=true

测试用例：

testplan _ openGauss_7.0.0 _ 数据库服务 _ CBM Tracking

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg&case_id=vb1n000103m5iuh0&detail=base

