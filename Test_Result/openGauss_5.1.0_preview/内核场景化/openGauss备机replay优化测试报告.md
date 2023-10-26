![avatar](../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2023-09-20|	1.0	|特性测试报告初稿完成|	shaohua.guan     |
| 2023-09-25|	1.1	|补充测试 关闭并行apply优化功能和大压力下的场景22个|	shaohua.guan     |

关键词： 并行apply


摘要： 本文档主要介绍对openGauss主备并行apply优化进行的测试，并给出测试结论。



缩略语清单：
| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| N/A    |          |          |

# 1     特性概述

## 1.1 新增GUC参数enable_time_report
bool值，on表示统计wal回放信息，off表示关掉wal回放的统计信息

## 1.2 新增GUC参数enable_batch_dispatch
开启或关闭回放优化功能。

Redo vacuum日志时，需要等待当前redo不对备库当前快照产生影响。这样一个有利影响就是，备库获取快照不再需要等待快照有效性。如果发现这个快照等待机制有问题，可以用这个参数关掉本优化。

## 1.3 新增GUC参数parallel_recovery_batch
用户可自定义starup线程移交批次数量。


## 1.4 新增GUC参数parallel_recovery_timeout，startup线程隔一段时间移交一次移交缓存wal记录
用户可自定义startup线程隔多长时间移交一次缓存的wal记录

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
|openGauss 5.1.0 build 24d9203f |  2023/09/19 | 2023/09/20 |
|openGauss 5.1.0 build fa0f975f |  2023/09/21 | 2023/09/25 |

描述特性测试的硬件环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | -------- | ---- |
| 虚拟机   |CPU： HUAWEI Kunpeng 920 @ 2.60GHz <br />内存：64GB<br/>硬盘：1.1TB<br/> OS：    Kylin Linux Advanced Server release V10 (Sword)  |  
| 虚拟机   |CPU： HUAWEI Kunpeng 920 @ 2.60GHz <br />内存：64GB<br/>硬盘：1.1TB<br/> OS：    Kylin Linux Advanced Server release V10 (Sword)  |  

# 3     测试结论概述
## 3.1   测试整体结论
openGauss备机replay速度优化特性，共计执行用例100个，主要覆盖了功能测试、可靠性测试和文档测试。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 |  验证并行apply优化 GUC参数名称和有效值范围的设置，通过 |
| 功能测试 |  验证并行apply优化 功能关闭时，无压力情况下，parallel_recovery_batch、parallel_recovery_timeout配置在有效值的最小、最大、默认值下，执行简单的CRUD，验证主备回放是否正常，通过|
| 功能测试 |  验证并行apply优化 功能开启时，无压力情况下，parallel_recovery_batch、parallel_recovery_timeout配置在有效值的最小、最大、默认值下，执行简单的CRUD，验证主备回放是否正常，通过|
| 功能测试 |  验证并行apply优化 功能开启时，TPCC 3000仓600并发情况下，parallel_recovery_batch、parallel_recovery_timeout配置在有效值的最小、最大、默认值下，验证主备回放是否正常，通过
| 功能测试 |  验证并行apply优化 功能关闭时时，TPCC 3000仓600并发情况下，parallel_recovery_batch、parallel_recovery_timeout配置在有效值的最小、最大、默认值下，验证主备回放是否正常，通过|
| 可靠性测试| 验证并行apply优化 开启时，在大压力（TPCC 3000仓600并发）和不同场景（switchover/kill -9/节点启停)下，能否正常拉起、回放正常，通过|
| 可靠性测试| 验证并行apply优化 关闭时，在大压力（TPCC 3000仓600并发）和不同场景（switchover/kill -9/节点启停)下，能否正常拉起、回放正常，通过|
| 可靠性测试| 验证并行apply优化 关闭时，在小压力（少量insert和select）和不同场景（switchover/kill -9/节点启停)下，能否正常拉起、回放正常，通过|
| 可靠性测试| 验证并行apply优化 开启时，在小压力（少量insert和select）和不同场景（switchover/kill -9/节点启停)下，能否正常拉起、回放正常，通过|
| 文档测试 |  验证文档描述是否准，通过 |

## 3.2   约束说明
无

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|  N/A     |   N/A     |   N/A     |   N/A               |   N/A     |

### 3.3.2 问题统计
|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   |     1     |   0   |  0    |  0    |  1      |
| 百分比 |       100%   |   0   |   0   |    0  |    100%    |

# 4     测试执行

## 4.1   测试执行统计数据
| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
|   openGauss 5.1.0 build 24d9203f       |      78      |     Passed: 78 <br/>Failed : 1         |        1      |
|   openGauss 5.1.0 build fa0f975f       |      100      |     Passed: 100 <br/>Failed : 0         |        0      |



## 4.2   后续测试建议

无

# 5     附件

无