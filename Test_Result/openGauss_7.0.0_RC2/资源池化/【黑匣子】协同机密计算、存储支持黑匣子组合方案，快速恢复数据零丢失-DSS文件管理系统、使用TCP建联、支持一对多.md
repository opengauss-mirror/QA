![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2025.5.20 | v1.0     | 测试报告初稿 | haomeng |

**Keywords 关键词**：黑匣子，WR

**Abstract 摘要**：本文档主要介绍基于WalRecorder服务框架第一阶段，实现文件隔离管理与读写，支持TCP链接，远程控制黑匣子进行文件操作，并且支持一对多对接多个客户端的能力，提供相关测试方案以及测试结论。



**缩略语清单：**

| 缩略语 | 英文全名    | 中文解释 |
| ------ | ----------- | -------- |
| WR     | WalRecorder |          |


# 1 概述

本报告主要测试WR工具的编译安装流程，以及在WR单节点模式下，通过本地连接、远程连接等方式验证相关API的基础功能、可靠稳定性，以及关键性能满足指标要求，涉及特性如下：

* 协同机密计算、存储支持黑匣子组合方案，快速恢复数据零丢失——DSS文件管理系统修改

* 协同机密计算、存储支持黑匣子组合方案，快速恢复数据零丢失——使用TCP建联
* 协同机密计算、存储支持黑匣子组合方案，快速恢复数据零丢失——支持一对多

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                         | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员                |
| -------------------------------- | ---------- | ------------ | ------------ | ----------------------- |
| wrcmd (openGauss build b57f2f6e) | WR         | 2025-5-6     | 2025-5-9     | chenziyang<br> haomeng  |
| wrcmd (openGauss build be4c23ad) | WR         | 2025-5-12    | 2025-5-16    | chenziyang<br/> haomeng |
| wrcmd (openGauss build 542b4b3c) | WR         | 2025-5-19    | 2025-5-20    | chenziyang<br/> haomeng |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试基于WR单节点服务管理本地存储模式，不同API在合理与不合理入参、各API组合场景下的测试，主要包含基本功能、可靠性测试，性能测试，输出测试用例77个，执行测试3轮，发现issue 10个，其中资料问题3个，已知DSS问题1个，当前所有问题待转测回归验收，因存在部分特性正在规划问题，部分流程会在后续整体方案推进过程中完善，当前基础服务能力满足基本要求。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 协同机密计算、Dorado存储构建安全、高效、可靠的数据黑匣子方案 | 挑战RPO≈0，在线数据落盘<1.5ms，落地G行等金融客户防数据勒索场景 | 依赖Dorado存储             | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 功能测试结论

| 测试场景    | 测试点                                                 | 测试结果 |
| ----------- | ------------------------------------------------------ | -------- |
| API         | WR实例管理，含合理入参和异常入参                       | 预期合理 |
|             | VFS管理，含合理入参和异常入参                          | 预期合理 |
|             | 文件管理及读写，含合理入参和异常入参                   | 预期合理 |
| API组合场景 | WR实例涉及API组合验证，含正常流程和异常流程            | 预期合理 |
|             | VFS实例涉及API组合验证，含正常流程和异常流程           | 预期合理 |
|             | 文件管理实例涉及API组合验证，含正常流程和异常流程      | 预期合理 |
| 一对多      | 正常建联配置和错误建联配置                             | 预期合理 |
|             | 并发创建多个WR实例连接，验证创建相同/不同VFS、FILE场景 | 预期合理 |

* API列表

| API                     | 接口描述                            |
| ----------------------- | ----------------------------------- |
| `wr_init`               | 初始化 SDK 实例，包括日志和通信模块 |
| `wr_exit`               | 退出 SDK 实例                       |
| `wr_create_inst`        | 获取 WalRecorder 实例句柄           |
| `wr_delete_inst`        | 删除实例句柄                        |
| `wr_get_err`            | 获取错误码信息                      |
| `wr_set_conf`           | 设置配置参数                        |
| `wr_get_conf`           | 获取配置参数内容                    |
| `wr_vfs_create`         | 创建 VFS                            |
| `wr_vfs_delete`         | 删除 VFS                            |
| `wr_vfs_mount`          | 挂载 VFS 获取句柄                   |
| `wr_vfs_unmount`        | 卸载 VFS                            |
| `wr_vfs_query_file_num` | 查询 VFS 内文件数量                 |
| `wr_file_create`        | 创建文件并获取句柄                  |
| `wr_file_delete`        | 删除文件                            |
| `wr_file_open`          | 打开文件并获取句柄                  |
| `wr_file_close`         | 关闭文件句柄                        |
| `wr_file_pread`         | 同步读操作                          |
| `wr_file_stat`          | 获取文件信息                        |
| `wr_file_pwrite`        | 同步写操作                          |
| `wr_file_truncate`      | 调整文件大小                        |

### 4.2.2 可靠性测试结论

| 测试场景   | 测试点         | 测试结果             |
| ---------- | -------------- | -------------------- |
| WR实例管理 | 建联/断联      | 预期合理             |
|            | 网络异常       | 预期合理             |
| VFS管理    | 流程异常       | 预期合理             |
|            | 网络断连       | 预期合理             |
|            | 磁盘空间不足   | 预期合理             |
| 文件管理   | 流程异常       | 预期合理             |
|            | 网络断连       | 预期合理             |
|            | 磁盘空间不足   | 预期合理             |
|            | 权限不足等场景 | 此阶段方案中暂不涉及 |

###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 稳定性测试结论

无

###  4.2.7 兼容性测试结论

无

###  4.2.8 升级测试结论

无

### 4.2.9 性能测试结论

| 测试步骤                | 性能指标               | 测试结论 |
| ----------------------- | ---------------------- | -------- |
| 不同VFS下的文件操作     | 在线数据落盘时延<1.5ms | 验收通过 |
| 相同VFS下的文件操作     | 在线数据落盘时延<1.5ms | 验收通过 |
| 相同VFS下的不同文件操作 | 在线数据落盘时延<1.5ms | 验收通过 |

### 4.2.10 资料测试结论

补充黑匣子简介、相关API、配置参数以及错误码等相关文档说明，验收通过。

[黑匣子](https://docs.opengauss.org/zh/docs/latest/docs/ToolandCommandReference/oGRecorder/%E9%BB%91%E5%8C%A3%E5%AD%90.html)

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖所有涉及API的单接口基本功能、组合场景基本功能、组合场景可靠性功能、单接口性能测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 10       | 0    | 1    | 9    | 0      |
| 百分比 | 100%     | 0%   | 10%  | 90%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                         | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [1](https://gitcode.com/opengauss/WR/issues/1)   | 【资料】按照WR指导资料不能编译安装WR服务                     | 次要     | 已开启   |
| [2](https://gitcode.com/opengauss/WR/issues/2)   | 【黑匣子】wr_file_create接口重复创建同名文件成功             | 次要     | 已开启   |
| [3](https://gitcode.com/opengauss/WR/issues/3)   | 【资料】wr_ini接口初始化时未对参数数值做校验                 | 次要     | 已开启   |
| [5](https://gitcode.com/opengauss/WR/issues/5)   | 【黑匣子】wr_vfs_mount接口校验不完善，导致请求接口异常       | 次要     | 已开启   |
| [6](https://gitcode.com/opengauss/WR/issues/6)   | 【资料】部分接口文档说明与代码不一致                         | 次要     | 已开启   |
| [7](https://gitcode.com/opengauss/WR/issues/7)   | 【黑匣子】wr_file_pread接口读取到小于文件大小的偏移时报错信息不明确 | 次要     | 已开启   |
| [8](https://gitcode.com/opengauss/WR/issues/8)   | 【黑匣子】wr_file_stat接口功能对于入参没有做校验拦截         | 次要     | 已开启   |
| [9](https://gitcode.com/opengauss/WR/issues/9)   | 【黑匣子】wr_create_file接口未接收到合理值，导致vfs卸载之后仍可以创建file | 次要     | 已开启   |
| [10](https://gitcode.com/opengauss/WR/issues/10) | 【黑匣子】建连后注入网络中断故障后请求接口卡住一段时间后进程直接退出 | 次要     | 已开启   |

* 非本特性引入问题：

| 问题单号                                       | 问题描述                                                     | 问题级别 | 当前状态 |
| ---------------------------------------------- | ------------------------------------------------------------ | -------- | -------- |
| [4](https://gitcode.com/opengauss/WR/issues/4) | 【黑匣子】wr_set_conf接口参数设置空字符串导致wrserver coredump | 主要     | 已开启   |



# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | 覆盖API单接口，API组合场景的验证                             | YES                  |
| 2    | 可靠性测试 | 覆盖异常流程、网络故障、磁盘满故障场景的验证                 | YES                  |
| 3    | 性能测试   | 覆盖一对多场景，含不同VFS下的文件操作，同VFS下的文件操作，同VFS下不同文件操作 | YES                  |

* 备注：因文件权限在当前方案阶段未接入，需等worm文件系统合入后再进行验证；

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量  | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------- | -------- |
| openGauss 7.0.0-RC2 B005 | 2                | 77         | 48         | 3          | 36.535K | 0.08     |
| openGauss 7.0.0-RC2 B006 | 5                | 77         | 29         | 7          | 36.535K | 0.19     |
| openGauss 7.0.0-RC2 B007 | 2                | 77         | 10         | 0          | 36.535K | 0        |

本次测试共发现10个issue，已修复并回归通过，缺陷密度为6/36.353k=0.165，整体质量良好。

备注：缺陷密度计算，剔除资料单，剔除非本特性引入问题 。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 77           | 48               | 45     | 3      | 0       | 0           | 100%   | 93.75%     |
| 77           | 15               | 10     | 5      | 0       | 0           | 100%   | 67%        |
| 77           | 14               | 12     | 2      | 0       | 0           | 100%   | 85.7%      |

本次测试共输出测试用例77个，执行测试共3轮，发现issue共10个，所有问题单待回归验收，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitcode.com/opengauss/WR/pull/5

https://gitcode.com/opengauss/WR/pull/16

https://gitcode.com/opengauss/WR/pull/20

https://gitcode.com/opengauss/WR/pull/23

https://gitcode.com/opengauss/WR/pull/11

https://gitcode.com/opengauss/WR/pull/1

https://gitcode.com/opengauss/WR/pull/2

https://gitcode.com/opengauss/WR/pull/9

https://gitcode.com/opengauss/WR/pull/15

https://gitcode.com/opengauss/WR/pull/18

https://gitcode.com/opengauss/WR/pull/19

https://gitcode.com/opengauss/WR/pull/21

https://gitcode.com/opengauss/WR/pull/22

https://gitcode.com/opengauss/WR/pull/31

资料pr：

https://gitcode.com/opengauss/docs/pull/7668/diffs

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=16f3f560002b4cc8bd0add53b6be0b3a&hideDevcloudHead=true

测试用例：

testplan _ openGauss7.0.0 RC2 _ 黑匣子 _ 基础服务能力 

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb2100010cof2bn7

