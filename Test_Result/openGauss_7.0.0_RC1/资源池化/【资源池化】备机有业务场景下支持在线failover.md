![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者    |
| ---------- | -------- | ------------ | ------- |
| 2024.12.26 | v1.0     | 测试报告初稿 | haomeng |

**Keywords 关键词**：备机，在线failover

**Abstract 摘要**：本文档主要介绍openGauss资源池化主备节点在大量业务背景下，主节点发生故障时，集群可以快速恢复到功能正常的测试方案及测试结果。

**缩略语清单：**

| 缩略语 | 英文全名                  | 中文解释       |
| ------ | ------------------------- | -------------- |
| DMS    | Distribute Memory Service | 分布式内存服务 |
| RTO    | recovery time objective   | 恢复时间目标   |
|        | Failover                  | 故障恢复       |


# 1 概述

本报告主要测试资源池化集群下，主备节点进行大量业务时，主节点发生故障后，整个集群failover能够正常进行，集群可对外支持业务。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build 0ea58f71 | openGauss  | 2024-11-14   | 2024-11-21   | haomeng  |
| openGauss 7.0.0-RC1 build d9c302e7 | openGauss  | 2024-11-22   | 2024-11-29   | haomeng  |
| openGauss 7.0.0-RC1 build 466faceb | openGauss  | 2024-12-5    | 2024-12-12   | haomeng  |
| openGauss 7.0.0-RC1 build f0bf6e56 | openGauss  | 2024-12-13   | 2024-12-20   | haomeng  |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |
| 磁阵     | Dorado 18500 V6                                              |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试资源池化集群内主节点故障后集群内failover流程可以正常进行，主要包含可靠性场景测试，输出测试用例34个，执行测试4轮，发现issue共6个，部分问题已修复并回归通过，存在少量待解决问题，整体质量一般。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                         | 特性价值评估                                   | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                          | 主要风险 |
| -------------------------------------------- | ---------------------------------------------- | -------------------------- | -------------------- | ---------------- | ----------------------------------------------------- | -------- |
| 【资源池化】备机有业务场景下支持在线failover | 资源池化集群内主节点故障后集群failover功能优化 | 资源池化集群               | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

无

### 4.2.2 可靠性测试结论

分别针对不同failover场景、二次故障、特性融合、硬件故障等故障点进行验证，验收通过。

| 测试步骤                            | 测试结果                           |
| ----------------------------------- | ---------------------------------- |
| 并行回放模式下，构造failover 场景   | 集群切换成功，备机支持在线failover |
| 极致RTO回放模式下，构造failover场景 | 集群切换成功，备机支持在线failover |
| 按需回放模式下，构造failover场景    | 集群切换成功，备机支持在线failover |

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

无

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖主节点异常之后集群内failover流程切换正常等可靠性测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6        | 0    | 4    | 2    | 0      |
| 百分比 | 100%     | 0%   | 67%  | 33%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IB4GPB](https://e.gitee.com/opengaussorg/issues/table?issue=IB4GPB) | 【测试类型：功能测试】kill主节点dn进程构造failover场景，备节点worker&TrackStmtClean线程无法退出导致升主失败集群状态卡在starting | 主要     | 已验收   |
| [IB4MAD](https://e.gitee.com/opengaussorg/issues/table?issue=IB4MAD) | 【测试类型：功能测试】【测试版本：7.0.0-RC1】【资源池化】【备机在线failover】mv + kill进程场景，待升主节点产生SegmentCheck failed相关core | 主要     | 已验收   |
| [IB7817](https://e.gitee.com/opengaussorg/issues/table?issue=IB7817) | 【测试类型：功能测试】【测试版本：7.0.0-RC1】【资源池化】【备机在线failover】kill主节点dn进程构造failover场景，待升主节点产生LWLockAcquire相关core | 主要     | 已验收   |
| [IB7BN5](https://e.gitee.com/opengaussorg/issues/table?issue=IB7BN5) | 【测试类型：功能测试】【测试版本：7.0.0-RC1】【资源池化】【备机在线failover】kill主节点dn进程构造failover场景，待升主节点因TrackStmtWorker线程持锁未释放致worker线程无法退出导致升主失败状态卡在promoting | 主要     | 已验收   |
| [IB7ZA0](https://e.gitee.com/opengaussorg/issues/table?issue=IB7ZA0) | 【测试类型：功能测试】【测试版本：7.0.0-RC1】【资源池化】【备机在线failover】kill主节点dn进程构造failover场景，回放线程持有页面未释放导致升主节点卡住 | 次要     | 已验收   |
| [IBAH9S](https://e.gitee.com/opengaussorg/issues/table?issue=IBAH9S) | 【测试类型：故障注入】【测试版本：7.0.0-RC1】【资源池化】【备机在线failover】down掉主节点网卡构造faiover场景，某个备节点存在worker线程无法退出导致集群状态一直卡在starting | 次要     | 已验收   |

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

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC1 B004 | 1                | 4          | 4          | 2          | 0.025K | 2        |
| openGauss 7.0.0-RC1 B007 | 5                | 10         | 10         | 2          | 0.025K | 2        |
| openGauss 7.0.0-RC1 B008 | 3                | 10         | 10         | 1          | 0.025K | 1        |
| openGauss 7.0.0-RC1 B009 | 5                | 10         | 10         | 1          | 0.025K | 1        |

本次测试共发现6个issue，全部已修复并回归通过，缺陷密度为6/1k=6，整体质量良好，后续可进行多次复测发现潜在问题。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 4            | 4                | 2      | 2      | 0       | 0           | 100%   | 50%        |
| 10           | 10               | 4      | 4      | 2       | 0           | 100%   | 60%        |
| 10           | 10               | 8      | 2      | 0       | 0           | 100%   | 80%        |
| 10           | 10               | 9      | 1      | 0       | 0           | 100%   | 90%        |

本次测试共输出测试用例34个，执行测试共4轮，发现issue共6个，全部已修复并回归通过，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitee.com/opengauss/openGauss-server/pulls/6637

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=bf38bc61406d4b4d87b58366d8a0444e&hideDevcloudHead=true

测试用例：

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg