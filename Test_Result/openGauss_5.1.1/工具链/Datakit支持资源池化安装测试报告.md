![](../../images/openGauss.png#crop=0&crop=0&crop=1&crop=1&id=d4u0Z&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=#crop=0&crop=0&crop=1&crop=1&id=Amfc1&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2024-03-20 | 1.0 | 测试报告初稿完成 | roxanne315 |


关键词：

资源池化安装

摘要：

本文档主要介绍DataKit支持资源池化安装特性。

# 1     特性概述

DataKit通过界面安装资源池化，支持资源池化的启动、停止、重启、主备切换、删除操作，支持界面化选择LUN路径，并自动创建软链接。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| openGauss 5.1.1  | 2023-12-20 | 2024-12-22 |
| openGauss 6.0.0  | 2024-01-25 | 2024-03-15 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 物理机 | CPU：aarch64 96核 Kunpeng-920 OS：OpenEuler | Dorado存储设备 |


# 3     测试结论概述

## 3.1   测试整体结论
DataKit支持资源池化安装，测试用例52条，主要测试功能的正确性。功能测试覆盖：一主一备安装，一主两备安装，正常和异常测试。由于是在传统集群安装的基础上新增了一些功能，原有页面和功能只做回归测试，不新增用例。且当前仅支持openEuler系统，其他系统暂均不支持，因此不涉及多系统的适配测试。累计发现问题单19个，特性质量一般。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 资源池化单集群一主一备和一主两备安装，通过 |
| 功能测试 | 资源池化的集群管理，如启动，停止等，通过 |


## 3.2   约束说明
1. 需提前准备磁阵，在DeviceManager管理平台上对预安装的各节点划分lun
2. 只能企业版安装
3. 不能在线安装openGauss6.0.0
4. 只支持openEuler

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| N/A |  |  |  |  |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 19 | 0 | 0 | 3 | 16 |
| 百分比 | 100% | 0% | 0% | 0% | 100% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I8QKQ6](https://gitee.com/opengauss/openGauss-workbench/issues/I8QKQ6?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.1.1】【DataKit】资源池化安装，主备节点应该只能设置一个cm端口号  | 已验收 |
| 2 | [I8QLJ7](https://gitee.com/opengauss/openGauss-workbench/issues/I8QLJ7?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.1.1】【DataKit】资源池化安装，主备节点的用户名只能选择，且两者不一致，也能执行安装 | 已验收 |
| 3 | [I8QLZX](https://gitee.com/opengauss/openGauss-workbench/issues/I8QLZX?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.1.1】【DataKit】资源池化安装，安装过程中失败了，不能下一步清理环境 | 已验收 |
| 4 | [I8QM8Q](https://gitee.com/opengauss/openGauss-workbench/issues/I8QM8Q?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：5.1.1】【DataKit】资源池化安装，不选择环境分离，没有提示  | 已验收 |
| 5 | [I8QMQO](https://gitee.com/opengauss/openGauss-workbench/issues/I8QMQO?from=project-issue) | 不重要 |【测试类型：工具功能】【测试版本：5.1.1】【DataKit】集群名称意义不大，应该限制长度，显示在展示范围内 | 已验收 |
| 6 | [I8YUU1](https://gitee.com/opengauss/openGauss-workbench/issues/I8YUU1?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【DataKit】环境监测中不影响安装的报错不应该是红色失败 | 已验收 |
| 7 | [I8YX09](https://gitee.com/opengauss/openGauss-workbench/issues/I8YX09?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】点击多个备节点后再回退到一个，校验ip不能相同失效 | 已验收 |
| 8 | [I8YXYT](https://gitee.com/opengauss/openGauss-workbench/issues/I8YXYT?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【DataKit资源池化】创建dss共享卷名超长，界面无限制，安装报错 | 已验收 |
| 9 | [I8YY21](https://gitee.com/opengauss/openGauss-workbench/issues/I8YY21?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【DataKit资源池化】dss卷含特殊符号，安装失败 | 已验收 |
| 10 | [I8ZR3A](https://gitee.com/opengauss/openGauss-workbench/issues/I8ZR3A?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】集群信息字段含非法字符，安装失败 | 已验收 |
| 11 | [I900FJ](https://gitee.com/opengauss/openGauss-workbench/issues/I900FJ?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】更换用户后，环境变量中的用户名没有修改，导致安装失败 | 已验收 |
| 12 | [I900HW](https://gitee.com/opengauss/openGauss-workbench/issues/I900HW?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】port输入4，没有校验，安装失败 | 已验收 |
| 13 | [I900RM](https://gitee.com/opengauss/openGauss-workbench/issues/I900RM?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】dn和cm的端口号一致，没有校验，安装失败 | 已验收 |
| 14 | [I900UH](https://gitee.com/opengauss/openGauss-workbench/issues/I900UH?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】安装等路径填写null等非法路径，没有校验，安装失败 | 已验收 |
| 15 | [I900YH](https://gitee.com/opengauss/openGauss-workbench/issues/I900YH?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】dss实例目录与安装目录相同，没有校验，安装失败 | 已验收 |
| 16 | [I900YR](https://gitee.com/opengauss/openGauss-workbench/issues/I900YR?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】共享存储的配置，没有在安装前回显 | 已验收 |
| 17 | [I9010Q](https://gitee.com/opengauss/openGauss-workbench/issues/I9010Q?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】一主两备，备节点的ip相同，没有校验 | 已验收 |
| 18 | [I9011A](https://gitee.com/opengauss/openGauss-workbench/issues/I9011A?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】xlog卷两个名字相同没有校验，安装失败 | 已验收 |
| 19 | [I9012G](https://gitee.com/opengauss/openGauss-workbench/issues/I9012G?from=project-issue) | 不重要 | 【测试类型：工具功能】【测试版本：6.0.0】【Datakit资源池化】xlog和dss卷，设置相同，没有校验 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 安装openGauss资源池化单集群，执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 部署datakit安装包|
| 2. 执行安装用例|共执行52条用例，共发现19个bug。





## 4.2   测试执行统计数据
| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| opengauss 6.0.0  | 52 | Passed：33 Failed：19 | 19 |

数据说明

1. 累计发现问题单19个
2. 缺陷密度为19（缺陷个数)/2.2k(代码行数)=8.6(个/kloc)
## 4.3   后续测试建议
无
# 5     附件
