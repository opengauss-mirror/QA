![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述     | 作者       |
| ---------- | ----------- | ------------ | ---------- |
| 2024-02-06 | 1.0         | 特性测试初稿 | lichunlong |

 关键词： 按需回放，实时构建，failover



摘要：本文档主要验证Xlog按需回放功能，主要面向资源池化failover和故障恢复场景；该功能适用于openGauss资源池化架构，能够帮助客户降低故障恢复和failover时间。

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

按需回放极致RTO开启后，故障恢复时仅构建恢复所必须的内容，不进行实际回放，之后立即对外提供服务，降低RTO。

本次优化特性在原有的按需回放的基础上，增加一种新的数据库状态阶段：实时构建阶段。当启动实时构建备机后，将从主机当前的redo点开始进行HashMap构建，不进行具体内容回放；收到集群内failover指令后，实时构建备机将立即对外提供服务，并切换状态至按需回放阶段；当全部日志回放完成后，切换状态至正常服务阶段。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 6.0.0 build 4614510e | 2024-01-17   | 2024-01-26   |
| openGauss 6.0.0 build cbe0c0e  | 2024-02-05   | 2024-02-09   |
| openGauss 6.0.0 build a73bec27 | 2024-02-21   | 2024-02-29   |
| openGauss 6.0.0 build 5e29c538 | 2024-03-01   | 2024-03-08   |

描述特性测试的硬件环境信息

| 环境信息      | 配置信息                                                     | 备注 |
| ------------- | ------------------------------------------------------------ | ---- |
| ARM+openEuler | CPU：Kunpeng 920 7260 2p 128核<br />内存：24*32GB<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：10GE |      |

# 3     测试结论概述

## 3.1   测试整体结论

支持XLog按需回放特特性总共执行用例47条，主要覆盖功能测试和性能测试。功能测试主要为参数测试、failover测试以及升级测试，参数测试正常赋值和异常赋值情况下特性功能正常；failover测试在主机故障后，保证failover成功，以及按需回放功能正常。性能测试在一主一备部署，主机在100并发、TPCC负载50WtpmC、RTO<10秒。本次测试发现问题11个，已解决10个，遗留1个后续版本解决，整体质量中等。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | （1）新增参数设置赋值测试，参数设置后主备机的实时构建状态正常，测试通过；<br />（2）failover测试，基本的主机故障failover，多次failover，failover和备机故障组合场景，failover完成，测试通过；<br />（3）语法测试，在打开实时构建时、按需回放阶段执行DML、DDL和DCL语句，测试通过；<br />（4）测试reform完成之后，主备的实时构建状态正常，测试通过。 |
| 性能测试 | 环境信息：Kunpeng-920  128core  openEuler 20.03 (LTS)<br />测试结果：warehouse=1000，terminals=100，tpmc=50w，RTO=11s，不满足RTO<10s的要求。 |
| 长稳测试 | 打开按需回放参数，执行tpcc业务24小时未发现问题。             |

## 3.2   约束说明

- 本特性仅支持在资源池化部署下使用。
- 在对外提供服务后，数据库仍在后台进行回放，直到全部内容完成后退出回放，该阶段称为“按需回放阶段”。按需回放阶段仅支持部分类型SQL语法（INSERT/UPDATE/DELETE/SELECT/SET/SHOW），该状态请使用pg_controldata工具查询。
- 按需回放阶段禁用autovacuum。
- 实时构建能力仅在备机failover场景生效。
- 当前不支持多备机情况下，优先选择实时构建节点升主。
- 不支持双集群。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 问题影响和规避措施 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | ------------------ | -------- |
| [I9A6Q1](https://e.gitee.com/opengaussorg/issues/table?issue_type_category=bug&issue=I9A6Q1) | 测试类型：性能】【测试版本：6.0.0】【资源池化】一主一备环境下，按需回放实时构建RTO性能不达标 | 主要     | 后续版本修复       | 待办的   |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 11       | 0    | 3    | 6    | 2      |
| 百分比 | 100%     | 0%   | 33%  | 58%  | 9%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I8Y77L](https://e.gitee.com/opengaussorg/issues/list?issue=I8Y77L) | 主要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】开启按需回放+实时构建，多次kill主节点构建failover，redo阶段kill备机，新主机coredump，关键字：HandlePageRedoInterruptsImpl；ondemand_extreme_rto::TrxnManagerDistributeItemsBeforeEnd | 已回归   |
| 2    | [I8YHE1](https://e.gitee.com/opengaussorg/issues/list?issue=I8YHE1) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】开启按需回放+实时构建，主备机执行tpcc，kill主机进程构建failover，新主coredump，关键字：ondemand_extreme_rto::TrxnWorkMain | 已回归   |
| 3    | [I8Y6WE](https://e.gitee.com/opengaussorg/issues/list?issue=I8Y6WE) | 主要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】开启按需回放+实时构建，主备执行tpcc业务，多次kill主节点构建failover，build阶段卡住 | 已回归   |
| 4    | [I8XABQ](https://e.gitee.com/opengaussorg/issues/table?issue=I8XABQ) | 不重要   | 【测试类型：资料】【测试版本：6.0.0】【资源池化】 按需回放优化函数ondemand_recovery_status()拼写错误 | 已回归   |
| 5    | [I8YUA2](https://e.gitee.com/opengaussorg/issues/table?issue=I8YUA2) | 不重要   | 【测试类型：功能】【测试版本：6.0.0】【资源池化】缺少设置BUILD_TO_REDO的日志 | 已回归   |
| 6    | [I8XEVI](https://e.gitee.com/opengaussorg/issues/table?issue=I8XEVI) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】kill主机构建failover，产生新主后立即kill新主，备机coredump，关键字：StopRecoveryWorkers | 已回归   |
| 7    | [I91C5J](https://e.gitee.com/opengaussorg/issues/table?issue=I91C5J) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，开启按需回放实时构建，failover过程中build阶段新主coredump，关键字：OndemandXLogParseBufferRelease | 已回归   |
| 8    | [I91BV9](https://e.gitee.com/opengaussorg/issues/table?issue=I91BV9) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】debug版本，开启按需回放实时构建，failover过程中备机coredump，关键字：ondemand_extreme_rto::DispatchXactRecord | 已回归   |
| 9    | [I96MEW](https://e.gitee.com/opengaussorg/issues/table?issue=I96MEW) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】开启按需回放实时构建，failover过程redo阶段集群卡住 | 已回归   |
| 10   | [I930AX](https://e.gitee.com/opengaussorg/issues/table?issue=I930AX) | 次要     | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】开启按需回放实时构建，备机执行select \* from ondemand_recovery_status();，realtime_build_status状态为DISABLED | 已回归   |
| 11   | [I9A6Q1](https://e.gitee.com/opengaussorg/issues/table?issue_type_category=bug&issue=I9A6Q1) | 主要     | 【测试类型：性能】【测试版本：6.0.0】【资源池化】一主一备环境下，按需回放实时构建RTO性能不达标 | 待办的   |

# 4     测试执行

## 4.1   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 6.0.0 build 4614510e | 43         | Passed：38  Failed：6   | 6            |
| openGauss 6.0.0 build cbe0c0e  | 10         | Passed：7    Failed：3  | 3            |
| openGauss 6.0.0 build a73bec27 | 20         | Passed：19  Failed：1   | 1            |
| openGauss 6.0.0 build 5e29c538 | 20         | Passed：19    Failed：1 | 1            |

数据项说明：

- 发现问题单数：本测试活动总共发现11个问题单，其中有1个是资料单。

- 用例执行结果：第一轮执行全量用例、后续三轮执行部分并回归问题单。
- 缺陷密度：10(缺陷个数)/2.541kloc(代码行数)=3.93(个/kloc).

## 4.2   后续测试建议

无

# 5     附件

XLog按需回放性能优化代码修改PR：

https://e.gitee.com/opengaussorg/repos/opengauss/DMS/pulls/355

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4748

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4754

XLog按需回放性能优化文档修改PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6100