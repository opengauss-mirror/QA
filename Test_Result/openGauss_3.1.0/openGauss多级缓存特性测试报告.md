![avatar](../images/openGauss.png)

版权所有 © 2021  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-8-30  |    1.0         |   特性测试报告初稿完成       |  quemingjian    |
|      |             |          |      |
|      |             |          |      |

 关键词： 多级缓存、 性能提升

 

摘要：

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|   TPC-C        |    Trade Promotion Coordination Committee      |    在线事务处理（OLTP）的基准程序。      |
|   tpmC     |   transaction per minute       |    TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试。      |
|   NVM     |   non volatile memory       |    非易失性存储。      |

# 1     特性概述

现有数据库BufferPool是基于DRAM-DISK设计:磁盘中的数据首先会被加载至内存中作为缓存供使用者读写。

在实际数据量大于shared_buffer的场景下,共享内存淘汰处会成为瓶颈。

本需求特性將BufferPool划分为三层DRAM-NVM-DISK设计：利用成本相对较低的NVM对BufferPool进行扩容，当实际数据量是shared_buffers的多倍，同时访问的数据量也超过shared_buffers时，減少共享内存淘汰流程的触发，形成性能优势：

1.利用成本相对较低的NVM作为内存扩展，物理上形成DRAM+NVM+DISK的结构，增大整体shared_buffers，优化数据量 > shared_buffers场景下的性能问题，减少淘汰流程的触发。

2.通过访问频率控制各缓存层次间的页面迁移，实现热数据在DRAM，温数据在NVM，冷数据在DISK中。

特性改造后实现如下目标：

等成本对比：（例如1000wh，内存40G换算成20G内存 + 40GNVM）

使用该特性，1 节点 openGauss + 1 节点 BenchmarkSQL 连跑30min，优化后最高性能提升30%+

使用该特性，1 节点 openGauss + 1 节点 BenchmarkSQL 连跑24H ，正常无报错

使用该特性，1 节点 openGauss + 1 节点 Sysbench oltp_read_only 连跑30min，优化后最高性能提升50%+

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
|    OpenGauss（优化前）          |      2022-8-30        |      2022-8-30        |
|    OpenGauss（优化后）          |      2022-8-31        |      2022-9-1         |
|          |              |              |

描述特性测试的硬件环境信息

| 硬件型号 | 硬件配置信息 | 备注 |
| -------- | ------------ | ---- |
|    X86 + Centos7.6<br /> 2288H V5      |    CPU：Intel(R) Xeon(R) Gold 6248R CPU @ 3.00GHz<br />内存：12*32GB<br />硬盘：SATA SDD/NVMe<br /> OS：Centos7.6 x86<br /> 文件系统XFS<br /> 网卡：10GE<br />  NVM：12 x 128 Intel AEP<br />        |  openGauss数据库节点    |
|    X86 + Centos7.6<br /> 2288H V5      |    CPU：Intel(R) Xeon(R) Gold 6248R CPU @ 3.00GHz<br />内存：12*32GB<br /> OS：Centos7.6 x86<br /> 文件系统XFS<br /> 网卡：10GE<br />         |  tpcc客戶端节点    |

| 软件型号 | 软件配置信息 | 备注 |
| -------- | ------------ | ---- |
|   BenchmarkSQL       |    标准版本          |      |
|   openGauss       |    openGauss 3.0.0 合入该改动          |      |
|   openGauss       |    openGauss 3.0.1 commit ID: 979b916d          |      |

# 3     测试结论概述

## 3.1   测试整体结论
共设计新增3个用例，主要覆盖了优化前后的性能对比测试和稳定性测试。其中，性能对比测试用例2个，稳定性测试用例1个；性能对比测试用例分别在 OpenGauss优化前和OpenGauss优化后版本各执行一次，稳定性测试用例在OpenGauss优化后执行一次；未发现问题，整体质量良好。

以及开启该特性后的全量用例覆盖：包括fastcheck\memfastcheck\hacheck\yat目前cover的全量用例，执行结果与优化前保持一致。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 性能对比测试 | 1 节点 openGauss + 1 节点 BenchmarkSQL 连跑30min，优化后最高性能提升30%+，验收通过。<br />      |
| 稳定性测试 |   1 节点 openGauss + 1 节点 BenchmarkSQL 连跑24H ，正常无报错；验收通过。       |
| YAT测试 |    YAT全量用例跑完 ，結果与优化前保持一致；验收通过。      |

## 3.2   约束说明

本报告中，使用benchmarkSQL工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力；
本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异，本次测试环境具体情况，请参考第二章节。

目前测试暂时依赖于Intel Optane AEP作为NVM。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|    N/A      |          |          |                    |          |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   |     0     |   0   |   0   |   0   |    0    |
| 百分比 |     0     |   0   |   0   |   0   |    0    |

# 4     测试执行

## 4.1   测试组网
**1 节点 openGauss + 1 节点 BenchmarkSQL 测试组⽹：**

![opengauss_test](images/openGauss测试组网.png)


*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
|          |            |              |              |
|          |            |              |              |
|          |            |              |              |

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*发现问题单数－－本测试活动总共发现的问题单数。*

## 4.2   测试步骤&结果

### 4.2.1  1 节点 openGauss + 1 节点 BenchmarkSQL 本特性优化前后tpmC比对

**测试步骤:**

以下测试步骤分别在openGauss优化后 和 优化前 版本各执⾏⼀次。注:步骤1在优化后版本执行，步骤1'在优化前版本执行

（1）openGauss优化后版本，配置如下guc：
```
enable_nvm = on
shared_buffers = 20GB
nvm_buffers = 60GB
bypass_nvm = 0.75
bypass_dram = 0.01
nvm_file_path = '/mnt/pmem0/q00421813/bufferExt'
```

（1'）openGauss优化前版本，配置如下guc：
```
shared_buffers = 50GB
```

（2）启动 gaussdb 进程

（3）启动 BenchmarkSQL 预热1min后，压测 30min，BenchmarkSQL 关键配置如下： 

```
terminals=192 
runTxnsPerTerminal=0 
runMins=30 
limitTxnsPerMin=0 
terminalWarehouseFixed=false
```

（5）收集 BenchmarkSQL tpmC

**测试结果：**

| 版本                       | 测试结果                                                     |
| -------------------------- | ------------------------------------------------------------ |
| OpenGauss优化前 | tpmc：391523.2                 |
| OpenGauss优化后 | tpmc：524813.4                 |
| 结论                       | tpmc性能提升：(524813.4-391523.2)/391523.2 ≈ 34.0% > 30% |

### 4.2.2 1 节点 openGauss +1 节点 BenchmarkSQL 连跑24H

**测试步骤**

以下测试步骤在openGauss优化后版本执⾏⼀次。 ： 

（1）openGauss优化后版本，配置如下guc：
```
enable_nvm = on
shared_buffers = 20GB
nvm_buffers = 60GB
bypass_nvm = 0.75
bypass_dram = 0.01
nvm_file_path = '/mnt/pmem0/q00421813/bufferExt'
```

（2）启动 gaussdb 进程

（3）启动 BenchmarkSQL 压测 24H，BenchmarkSQL 关键配置如下： 

```
terminals=192 
runTxnsPerTerminal=0 
runMins=1440
limitTxnsPerMin=0 
terminalWarehouseFixed=false 
```

### 4.2.3 YAT环境全量用例跑openGauss优化后版本

**测试步骤**

以下测试步骤在openGauss优化后版本执⾏⼀次。 ： 

（1）openGauss优化后版本，配置如下guc：
```
enable_nvm = on
shared_buffers = 20GB
nvm_buffers = 60GB
bypass_nvm = 0.75
bypass_dram = 0.01
nvm_file_path = '/mnt/pmem0/q00421813/bufferExt'
```

（2）测试yat用例

（3）比对测试结果

**测试结果**

测试结果与openGauss优化前版本保持一致

## **4.3 测试执行统计数据**

| **版本名称**               | **测试用例数** | **用例执行结果**        | **发现问题单数** |
| -------------------------- | -------------- | ----------------------- | ---------------- |
| openGauss优化前 | 1              | Passed：1<br/>Failed：0 | 0                |
| openGauss优化后 | 3              | Passed：3<br/>Failed：0 | 0                |

数据项说明：

测试用例数：3个；其中1个用例需要在OpenGauss优化前、OpenGauss优化后各执行一次进行性能结果对比。1个长稳用例在OpenGauss优化后版本测试1次；一个yat用例在OpenGauss优化后版本全量执行;测试通过：3个；

发现问题单数：0个。

## **4.4   后续测试建议**

按照上述思路，补测各种业务场景性能对比。

# 5     附件

*此处可粘贴各类专项测试数据或报告*

 



 

 
