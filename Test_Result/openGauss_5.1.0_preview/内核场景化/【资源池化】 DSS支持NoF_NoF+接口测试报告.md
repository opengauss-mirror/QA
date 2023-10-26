![](../../images/openGauss.png#crop=0&crop=0&crop=1&crop=1&id=d4u0Z&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=#crop=0&crop=0&crop=1&crop=1&id=Amfc1&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2023-09-26 | 1.0 | 测试报告初稿完成 | roxanne315 |


关键词：

资源池化，NoF+

摘要：

本文档主要介绍openGauss在资源池化提供NoF+接口支持的特性。

# 1     特性概述

资源池化提供NoF+接口支持，提供基于Nvme over Fabirc接口的存储访问能力，提升集群整体利用底层闪存存储带宽与速率的效率。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| openGauss 5.1.0  | 2023-08-01 | 2023-09-26 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 物理机 | CPU：aarch64 96核 Kunpeng-920 OS：Kylin Linux Advanced Server V10(Tercel) | Dorado存储设备 |


# 3     测试结论概述

## 3.1   测试整体结论
资源池化提供NoF+接口支持，测试用例39条，主要覆盖了功能测试及高并发测试。功能测试覆盖：dsscmd各项操作，建表使用段页式等，导入导出备份还原，高并发测试覆盖TPCC 100仓300并发，1000仓800并发等。累计发现问题单5个，特性质量良好。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 安装集群，查看集群状态，执行结果符合预期，通过 |
| 功能测试 | 各种数据库兼容模式下建表，视图，存储过程等对象，执行结果符合预期，通过 |
| 功能测试 | dsscmd各种操作，如显示卷组，创建卷组，查看LUN/注册信息等，执行结果符合预期，通过 |
| 功能测试 | gs_dump,gs_dumpall,gs_restore等，导入导出备份还原，执行结果符合预期，通过 |
| 功能测试 | TPCC 100仓/1000仓导入数据删除数据，并发测试，通过 |

## 3.2   约束说明
1.openGauss需使用资源池化部署

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| N/A |  |  |  |  |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 5 | 1 | 0 | 0 | 4 |
| 百分比 | 100% | 20% | 100% | 0% | 80% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I7TL0Y](https://gitee.com/opengauss/openGauss-server/issues/I7TL0Y?from=project-issue) | 不重要 | 【资源池化】【nof+】dsscmd inq -t lun失败 | 已验收 |
| 2 | [I83MX8](https://gitee.com/opengauss/openGauss-server/issues/I83MX8?from=project) | 严重 | 【资源池化】TPCC装载1000仓数据，集群连接不上has_server | 已验收 |
| 3 | [I7IJKV](https://gitee.com/opengauss/CM/issues/I7IJKV?from=project-issue?from=project-issue) | 不重要 | NoF+协议，安装后启动集群失败 | 已验收 |
| 4 | [I7I5OG](https://gitee.com/opengauss/openGauss-OM/issues/I7I5OG?from=project-issue) | 不重要 | NoF+协议，gs_preinstall预安装失败，Failed to obtain the uuid of the '' | 已验收 |
| 5 | [I7I66R](https://gitee.com/opengauss/openGauss-OM/issues/I7I66R?from=project-issue) | 不重要 | NoF+协议，gs_preinstall命令执行报错openssl问题 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 安装openGauss资源池化单集群，执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 获取openGauss转测包，使用OM工具进行安装|
| 2. 执行用例|共执行39条用例，共发现5个bug。


#### 4.1.1.1 安装测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 按照文档使用OM工具进行安装
2. 执行用例 | 共执行1条用例，共发现3个bug。 |

#### 4.1.1.2 各种数据库兼容模式下建表，视图，存储过程等对象测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 按照文档使用OM工具进行安装
2. 执行用例 | 共执行12条用例，共发现1个bug。 |

#### 4.1.1.3 dsscmd各种操作测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 按照文档使用OM工具进行安装
2. 执行用例 | 共执行21条用例，共发现1个bug。 |

#### 4.1.1.4 gs_dump,gs_dumpall,gs_restore测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 按照文档使用OM工具进行安装
2. 执行用例 | 共执行3条用例，共发现0个bug。 |

#### 4.1.1.5 TPCC 100仓/1000仓导入数据删除数据，并发测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 按照文档使用OM工具进行安装
2. 执行用例 | 共执行2条用例，共发现1个bug。 |


## 4.2   测试执行统计数据
| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| opengauss 5.1.0  | 39 | Passed：34 Failed：5 | 5 |

数据说明

1. 累计发现问题单5个
## 4.3   后续测试建议
无
# 5     附件
