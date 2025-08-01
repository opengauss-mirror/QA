![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者       |
| ---------- | -------- | ------------ | ---------- |
| 2025-01-03 | v1.0     | 测试报告初稿 | lichunlong |

**Keywords 关键词**：multipath

**Abstract 摘要**：本文主要验证openGauss资源池化集群是否支持使用multipath多链路。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

为了进一步提高openGauss存储系统的可用性及可靠性，使用ISCSI软件建立主、备机与磁阵之间的多链路，并使用multipath软件管理建立的多链路。本次测试主要验证使用ISCSI软件建立多链路以及multipath对资源池化的支持情况。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员   |
| ---------------------------------- | ---------- | ------------ | ------------ | ---------- |
| openGauss 7.0.0-RC1 build 96ee5da3 | openGauss  | 2024-12-12   | 2024-12-28   | lichunlong |

###  2.1.2 配套测试的版本

无

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                             | 备注 |
| -------- | -------------------------------------------------------- | ---- |
| 物理机   | CPU：kunpeng-920 2600MHz<br/>内存：765GB<br/>硬盘：3.0TB |      |
| 磁阵     | Dorado 18500 V6                                          |      |

### 2.2.2 OS版本

| 操作系统  | OS版本                               |
| --------- | ------------------------------------ |
| CentOS    | CentOS Linux release 7.9.2009 (Core) |
| openEuler | openEuler release 20.03 (LTS)        |
| openEuler | openEuler release 22.03 (LTS-SP4)    |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试multipath支持资源池化的情况，主要包含功可靠性、性能、稳定性测试以及资料验证，输出测试用例41个，测试过程中发现资料issue1个，证明multipath多链路可以支持资源池化集群。

## 3.2 约束说明

所有设备处于同一网段。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                              | 特性价值评估                        | 应用说明及关键约束假设依赖               | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| --------------------------------- | ----------------------------------- | ---------------------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【资源池化】multipath支持资源池化 | multipath多链路可以支持资源池化集群 | 资源池化集群，多链路磁阵网络处于同一网段 | 无                   |                  | <font color=green>▮</font> |          |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

| 测试步骤                           | 测试结果                 |
| ---------------------------------- | ------------------------ |
| 多链路集群对比单路径集群性能无劣化 | 性能对比无劣化，测试通过 |

###  4.2.2 可靠性测试结论

| 测试步骤                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 资源池化多链路集群磁阵网络隔断测试                           | 验证通过 |
| 资源池化多链路集群磁阵网络延迟、闪断、丢包故障测试           | 验证通过 |
| 资源池化多链路集群磁阵网络在switchover、failover以及节点加入过程中的影响验证 | 验证通过 |

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
| 在不同OS（CentOS、openEuler 20.03、openEuler22.03）系统下，对multipath支持资源池化集群的验证 | 验证通过 |

###  4.2.8 升级测试结论

无

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论            |
| ---- | ------------------------------------------------------------ | ------------------- |
| 1    | 文档地图>数据库管理指南>数据库部署方案>资源池化架构>资源池化multipath多链路冗余环境搭建指南 | 发现1个问题，已解决 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

覆盖多链路网路对于集群的影响验证。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        |      |      |      |        |
| 百分比 | NA       |      |      |      |        |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBB3UI](https://e.gitee.com/opengaussorg/issues/table?issue=IBB3UI) | 【测试类型：资料】【测试版本：7.0.0】资源池化multipath多链路冗余环境搭建指南中缺少对multipath配置的介绍 | 不重要   | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                        | 是否按照测试策略执行 |
| ---- | ---------- | ----------------------------------------------- | -------------------- |
| 1    | 可靠性测试 | multipath多链路网络故障对资源池化集群的影响验证 | YES                  |
| 2    | 长稳测试   | multipath多链路资源池化集群的稳定性测试         | YES                  |
| 3    | 兼容性测试 | 在不同的OS上验证multipath多链路支持资源池化集群 | YES                  |
| 4    | 资料测试   | 资料描述完整，示例正确无误                      | YES                  |

##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-RC1 build 96ee5da3 | 14               | 41         | 83         | 1          | 0              | 0        |

本次测试为软硬件设施对资源池化集群的支持验证，无代码量。总用例41个，openEuler 20.03系统执行了41条用例、openEuler 20.03系统执行了15条用例，CentOS系统执行了27条用例。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 41           | 83               | 83     | 0      | 0       | 0           | 100%   | 100%       |



# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码PR：

无

资料PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6970?tab=files

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=c8ac35746e0c45e4899cdc1b0b3d0e2f&hideDevcloudHead=true

测试用例：

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg

multipath相关资料：

https://docs.redhat.com/zh-cn/documentation/red_hat_enterprise_linux/9/html-single/configuring_device_mapper_multipath/index#configuration-file-multipaths-section_modifying-the-dm-multipath-configuration-file