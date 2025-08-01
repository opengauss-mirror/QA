![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2025.6.20 | v1.0     | 测试报告初稿 | chen-czywj |

**Keywords 关键词**：黑匣子，WR

**Abstract 摘要**：本文档主要介绍基于WalRecorder服务框架防止写入信息被篡改，引入hash校验能力，实现客户端与服务端数据准确性校验，实现数据防篡改能力，提供相关测试方案以及测试结论。



**缩略语清单：**

| 缩略语 | 英文全名    | 中文解释 |
| ------ | ----------- | -------- |
| WR     | WalRecorder |          |


# 1 概述

本报告主要测试WR工具的hash校验能力，以及在WR单节点模式下，通过本地连接、远程连接等方式验证相关API的基础功能、可靠稳定性，以及关键性能满足指标要求，涉及特性如下：

* 协同机密计算、存储支持黑匣子组合方案，快速恢复数据零丢失——链式hash校验

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                         | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员                |
| -------------------------------- | ---------- | ------------ | ------------ | ----------------------- |
| wrserver | WR         | 2025-6-11     | 2025-6-13     | chen-czywj  |
| wrserver | WR         | 2025-6-16    | 2025-6-20    | chen-czywj |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试基于WR单节点服务管理本地存储模式，各API组合场景下的测试，主要包含基本功能、可靠性测试，性能测试，输出测试用例18个，执行测试2轮，发现issue 1个，因存在部分特性正在规划问题，部分流程会在后续整体方案推进过程中完善，当前基础服务能力满足基本要求。
1个issue经过分析为非问题

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 黑匣子链式hash校验 | 支持对日志进行链式hash校验，数据在传输过程中防篡改 | 无             | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 功能测试结论

功能api组合实现特性场景测试，包含同节点请求，跨节点请求等场景

* 涉及API列表

| API                     | 接口描述                            |
| ----------------------- | ----------------------------------- |
| `wr_file_create`        | 创建文件并获取句柄                  |
| `wr_file_delete`        | 删除文件                            |
| `wr_file_open`          | 打开文件并获取句柄                  |
| `wr_file_close`         | 关闭文件句柄                        |
| `wr_file_pread`         | 同步读操作                          |
| `wr_file_stat`          | 获取文件信息                        |
| `wr_file_pwrite`        | 同步写操作                          |

### 4.2.2 可靠性测试结论

可靠性场景测试包含以下场景</br>
网络中断后文件操作、重新建连、恢复网络中断故障后文件读写等场景测试，预期合理</br>
注入网络闪断故障，文件操作，预期合理</br>
hash值的校验以及篡改场景测试，预期合理

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
| 不同VFS下的文件操作     | 在线数据落盘时延<2ms | 验收通过 |
| 相同VFS下的文件操作     | 在线数据落盘时延<2ms | 验收通过 |
| 相同VFS下的不同文件操作 | 在线数据落盘时延<2ms | 验收通过 |

### 4.2.10 资料测试结论

适配黑匣子简介、相关API、配置参数以及错误码等相关文档说明，验收通过。

[黑匣子](https://docs.opengauss.org/zh/docs/latest/docs/ToolandCommandReference/oGRecorder/%E9%BB%91%E5%8C%A3%E5%AD%90.html)

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖所有涉及API的单接口基本功能、组合场景基本功能、组合场景可靠性功能、单接口性能测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0       | 0    | 0    | 0    | 0      |
| 百分比 | 100%     | 0%   | 0%  | 0%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                         | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [1](https://gitcode.com/opengauss/WR/issues/17)   | hash链式校验，服务端与客户端做网络中断5s内恢复，文件写入接口报错              | 次要     | 非问题    |


# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | API组合场景的验证                             | YES                  |
| 2    | 可靠性测试 | 覆盖网络故障、hash值校验以及篡改场景的验证                 | YES                  |
| 3    | 性能测试   | 覆盖一对多场景，含不同VFS下的文件操作，同VFS下的文件操作，同VFS下不同文件操作 | YES                  |


##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- |
| openGauss 7.0.0-RC2 B009 | 2                | 10         | 10         | 0          |
| openGauss 7.0.0-RC2 B010 | 5                | 8         | 8         | 1          |

开发代码行数：444行
本次测试共发现1个issue，经分析为非问题，缺陷密度为0/0.5k=0，整体质量良好。

备注：缺陷密度计算，剔除资料单，剔除非本特性引入问题 。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 18           | 18               | 18     | 0      | 0       | 0           | 100%   | 100%     |

本次测试共输出测试用例17个，执行测试共2轮，发现issue共0个，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitcode.com/opengauss/oGRecorder/pull/35

资料pr：

https://gitcode.com/opengauss/docs/pull/7747

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=26a8629528194eefa2fe138ba9c0c2af&hideDevcloudHead=true

