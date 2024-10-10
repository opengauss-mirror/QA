![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2024.9.11 | v1.0     | 测试报告初稿 | haomeng |

**Keywords 关键词**：switchover

**Abstract 摘要**：主要使CM支持双集群中备集群通过switchover命令切换首备和从备节点。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

本报告主要测试CM支持双集群中备集群首备和从备的switchover，保证切换后的首备节点和主集群建联、日志同步状态正常。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                       | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------------ | ---------- | ------------ | ------------ | -------- |
| openGauss 6.0.0 build 4ef8fd3b | openGauss  | 2024-4-26    | 2024-5-7     | haomeng  |
| openGauss 6.0.0 build 6c3d81df | openGauss  | 2024-6-11    | 2024-6-17    | haomeng  |
| openGauss 6.0.0 build 7bf1cd53 | openGauss  | 2024-6-25    | 2024-6-28    | haomeng  |
| openGauss 6.0.0 build 71e3b71d | openGauss  | 2024-8-12    | 2024-8-19    | haomeng  |
| openGauss 6.0.0 build fb09a5d8 | openGauss  | 2024-8-22    | 2024-8-29    | haomeng  |
| openGauss 6.0.0 build 5f620997 | openGauss  | 2024-9-4     | 2024-9-11    | haomeng  |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |
| 磁阵     | Dorado 18500 V6                                              |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试资源池化双集群（含存储复制和网络复制）备集群内CM支持首备和从备的switchover切换操作，以及主备集群间切换之后新备集群内首备和从备的switchover，包含支持参数的基本功能、可靠性场景以及资料测试，输出测试用例15个，执行测试6轮，发现issue共13个，部分问题已修复并回归通过，存在少量待解决问题，整体质量一般。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                   | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                          | 主要风险 |
| ------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | ----------------------------------------------------- | -------- |
| 【资源池化】CM支持双集群中备集群首备和从备的switchover | 支持备集群中cm_ctl switchover，保证切换后和主集群建联、日志同步状态正常 | 资源池化双集群             | 无                   | 100%             | <font color=yellow><font color=yellow>▲</font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

无

### 4.2.2 可靠性测试结论

分别针对切换过程中数据库系统故障、节点二次切换等故障点进行验证，验收通过。

| 测试步骤                                 | 测试结果                             |
| ---------------------------------------- | ------------------------------------ |
| switchover切换过程中，待升首备节点异常   | 首备切换失败                         |
| switchover切换过程中，待降从备节点异常   | 首备切换成功                         |
| switchover切换过程中，switchover进程异常 | switchover操作终止，首备切换可能成功 |

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

社区文档已增加cm_server参数，且部分涉及到参数配置的已修改，与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [ss_double_cluster_mode](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/cm_server%E5%8F%82%E6%95%B0.html) | 测试通过 |
| 2    | [存储复制双集群部署](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/DatabaseAdministrationGuide/%E8%B5%84%E6%BA%90%E6%B1%A0%E5%8C%96DORADO%E5%8F%8C%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2.html) | 测试通过 |
| 3    | [网络复制双集群部署](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/DatabaseAdministrationGuide/%E8%B5%84%E6%BA%90%E6%B1%A0%E5%8C%96%E7%BD%91%E7%BB%9C%E5%8F%8C%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2.html) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖cm_ctl switchover支持备集群内首备和从备切换以及主备集群间切换之后新备集群内switchover切换的功能、可靠性测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 13       | 0    | 8     | 5     | 0      |
| 百分比 | 100%     | 0%   | 61.5% | 38.5% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IANG6C](https://e.gitee.com/opengaussorg/issues/table?issue=IANG6C) | 【资源池化】网络复制双集群，备集群内switchover进行AZ级别切换时，待升首备节点System id not matched | 次要     | 已验收   |
| [IAL0B7](https://e.gitee.com/opengaussorg/issues/table?issue=IAL0B7) | 【资源池化】存储复制双集群，部分switchover操作超时异常导致待降备节点产生StopRecoveryWorkers wait too long的core | 主要     | 已验收   |
| [IAJT0X](https://e.gitee.com/opengaussorg/issues/table?issue=IAJT0X) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群执行switchover -a恢复初始集群状态期间产生now lsn(xxx) is less than past lsn(xxx)的core | 次要     | 已验收   |
| [IAGUPA](https://e.gitee.com/opengaussorg/issues/table?issue=IAGUPA) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】存储复制双集群，备集群内执行switchover -A切换操作时，待升首备节点产生memory lsn is less than lsn_on_disk相关core | 主要     | 已验收   |
| [IAFBXC](https://e.gitee.com/opengaussorg/issues/table?issue=IAFBXC) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群内执行switchover进行AZ级别切换，待升首备节点产生invalid max offset number相关core | 主要     | 已验收   |
| [IAF9DA](https://e.gitee.com/opengaussorg/issues/table?issue=IAF9DA) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群内switchover切换AZ级别时，偶现dw相关core | 主要     | 已验收   |
| [IA94F2](https://e.gitee.com/opengaussorg/issues/table?issue=IA94F2) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群执行switchover -z AZ时，偶现因page work线程退出过程因校验lsn小于落盘lsn，导致待升首备节点coredump | 主要     | 已验收   |
| [IA8NW8](https://e.gitee.com/opengaussorg/issues/table?issue=IA8NW8) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群执行switchover指定节点切换失败，待降备节点产生parallel_recovery::CheckAlivePageWorkers相关core | 主要     | 已验收   |
| [IA82T5](https://e.gitee.com/opengaussorg/issues/table?issue=IA82T5) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】存储复制双集群，备集群首备stop之后start再次拉起，switchover -a恢复初始集群状态时待降备节点产生BufValidateDrc的core | 主要     | 已验收   |
| [I9MPY9](https://e.gitee.com/opengaussorg/issues/table?issue=I9MPY9) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，备集群内执行switchover指定节点切换，待降为从备节点产生[REDO_LOG_TRACE]DispatchRedoRecord could not be here config相关core | 主要     | 已验收   |
| [I9KS74](https://e.gitee.com/opengaussorg/issues/table?issue=I9KS74) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】网络复制双集群，首备节点执行switchover -n指定节点升主超时失败，待升首备节点core | 主要     | 已验收   |
| [IAKL6S](https://e.gitee.com/opengaussorg/issues/table?issue=IAKL6S) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】存储复制双集群，备集群停止首备后再拉起恢复为集群初始状态，流复制关系断连，日志回放卡住 | 次要     | 已验收   |
| [I9MPPH](https://e.gitee.com/opengaussorg/issues/table?issue=I9MPPH) | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】存储复制双集群，备集群内执行switchover -A后产生SSSyncOldestXminWhenReform相关core | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                     | 是否按照测试策略执行 |
| ---- | ---------- | -------------------------------------------- | -------------------- |
| 1    | 功能测试   | 基本切换功能验证，覆盖switchover所有支持参数 | YES                  |
| 2    | 可靠性测试 | 切换过程中进程故障                           | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称             | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| -------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 6.0.0 B014 | 4                | 3          | 3          | 2          | 1K     | 2        |
| openGauss 6.0.0 B016 | 1                | 5          | 5          | 3          | 1K     | 3        |
| openGauss 6.0.0 B020 | 2                | 5          | 5          | 2          | 1K     | 2        |
| openGauss 6.0.0 B027 | 3                | 3          | 3          | 2          | 1K     | 2        |
| openGauss 6.0.0 B029 | 3                | 10         | 10         | 3          | 1K     | 3        |
| openGauss 6.0.0 B030 | 3                | 3          | 3          | 1          | 1K     | 1        |

本次测试共发现13个issue，部分已修复并回归通过，缺陷密度为13/1k=13，整体质量一般，后续可进行多次复测发现潜在问题。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 3            | 3                | 1      | 2      | 0       | 0           | 100%   | 30%        |
| 5            | 5                | 3      | 1      | 1       | 0           | 100%   | 0%         |
| 5            | 5                | 3      | 0      | 2       | 0           | 100%   | 60%        |
| 3            | 3                | 1      | 2      | 0       | 0           | 100%   | 33%        |
| 10           | 10               | 7      | 3      | 0       | 0           | 100%   | 70%        |
| 3            | 3                | 2      | 1      | 0       | 0           | 100%   | 67%        |

本次测试共输出测试用例15个，执行测试共6轮，发现issue共13个，部分已修复并回归通过，整体质量一般。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitee.com/opengauss/CM/pulls/197

资料pr：

https://gitee.com/opengauss/docs/pulls/6588

https://gitee.com/opengauss/docs/pulls/6687

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=b4f6aecd66404b99b37a568556768fd9&hideDevcloudHead=true