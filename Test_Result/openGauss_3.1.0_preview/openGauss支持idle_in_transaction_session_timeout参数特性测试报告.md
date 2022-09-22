![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述             | 作者       |
| :-------- | ----------- | -------------------- | ---------- |
| 2022-9-22 | 1.0         | 特性测试报告初稿完成 | chengyao25 |

 关键词： 

 idle_in_transaction_session_timeout  、session_timeout、 idle、idle_in_transaction、idle_in_transaction_block 

摘要：

本文档主要介绍新增 idle_in_transaction_session_timeout 参数，在线程池场景与非线程池场景下设置idle_in_transaction_session_timeout 与session_timeout参数 控制 idle、idle_in_transaction、idle_in_transaction_block三种场景下会话超时时间， 并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

idle_in_transaction_session_timeout是一个guc参数，其值是用于控制idle_in_transaction和idle_in_transaction_block两种场景的会话超时时间 。该参数为userset类型参数，拥有三种设置方式，设置成功后与session_timeout结合使用，在线程池与非线程池场景下控制idle、idle_in_transaction、idle_in_transaction_block三种状态的会话超时时间。只有idle_in_transaction_session_timeout生效时，只控制idle_in_transaction和idle_in_transaction_block场景的会话超时，idle_in_transaction_session_timeout与session_timeout同时生效，以设置时长较短的参数控制会话超时时间(idle状态只受session_timeout参数控制)。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 801d945a | 2022-9-7     | 2022-9-13    |
| openGauss 3.1.0 build 88ca9bcd | 2022-9-22    | 2022-9-22    |

| 硬件型号   | 硬件配置信息                                                 | 备注 |
| ---------- | ------------------------------------------------------------ | ---- |
| x86+centOS | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

新增guc参数idle_in_transaction_session_timeout支持openGauss，共计执行51条用例，主要覆盖功能测试和资料测试。功能测试覆盖通过gs_guc set 方式、gs_guc reload方式、指定数据库、用户、会话级别参数的方式进行参数设置，在线程场景与非线程场景下设置idle_in_transaction_session_timeout与session_timeout参数，控制idle、idle_in_transaction、idle_in_transaction_block三种状态的会话超时时间，通过jdbc连接数据库进行设置观察数据库断开时间，会话超时会断开连接并且提示由哪个参数影响造成的超时。资料测试覆盖资料的描述是否正确，示例的执行是否成功。累计发现缺陷单1个，缺陷均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 通过gs_guc set方式设置参数idle_in_transaction_session_timeout，重启并连接数据库后，查看参数设置成功。 |
| 功能测试 | 通过gs_guc reload方式设置参数idle_in_transaction_session_timeout，连接数据库，查看参数设置成功。 |
| 功能测试 | 指定数据库、用户设置idle_in_transaction_session_timeout，断开数据库连接，重新连接后查看参数设置成功；指定会话设置idle_in_transaction_session_timeout后直接查看设置成功。 |
| 功能测试 | 在线程池场景设置idle_in_transaction_session_timeout与session_timeout参数后，进行会话监控，在idle_in_transaction、idle_in_transaction_block状态下受设置数值较小的参数控制断开会话，idle状态受session_timeout控制断开会话。 |
| 功能测试 | 在非线程池场景设置idle_in_transaction_session_timeout与session_timeout参数后，进行会话监控，在idle_in_transaction、idle_in_transaction_block状态下受设置数值较小的参数控制断开会话，idle状态受session_timeout控制断开会话。 |
| 功能测试 | 通过jdbc连接数据库后，设置idle_in_transaction_session_timeout参数比session_timeout数值小，在在idle_in_transaction、idle_in_transaction_block状态下会话断开时间以数值小的为准，idle状态下以session_timeout为准。 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好               |

## 3.2   约束说明

1. idle_in_transaction_session_timeout参数的取值范围为：0 - 86400。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1      | 0    | 0    | 1   | 0      |
| 百分比 | 100% | 0%   | 0%  | 100% | 0%     |

###  3.3.3 问题单汇总

| issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| [I5QEY](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5QEY?from=project-issue) | 次要     | 设置idle_in_transaction_session_timeout为30s，session_timeout为10s，idle_in_transaction与idle_in_transaction_block状态，会话等待10s没有断开，而是等待30S后断开 | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 使用gs_guc set设置idle_in_transaction_session_timeout参数

| 测试步骤：                                                   | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. gs_guc set 设置 idle_in_transaction_session_timeout<br/>2. 重启数据库 | 共执行5条用例，执行结果符合预期 |

### 4.1.2 使用gs_guc reload设置idle_in_transaction_session_timeout参数

| 测试步骤：                                                   | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. gs_guc reload 设置 idle_in_transaction_session_timeout<br/>2. 重启数据库 | 共执行5条用例，执行结果符合预期 |

### 4.1.3 数据库、用户、会话级别idle_in_transaction_session_timeout参数修改

| 测试步骤：                                                   | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. 设置idle_in_transaction_session_timeout<br/>2. show  idle_in_transaction_session_timeout; | 共执行5条用例，执行结果符合预期 |

### 4.1.4 线程池场景设置idle_in_transaction_session_timeout与session_timeout，监测会话超时时间

| 测试步骤：                                                   | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1. 开启线程池<br/>2. 设置idle_in_transaction_session_timeout与session_timeout<br/>3. 监测会话超时 | 共执行15条用例，执行结果符合预期 |

### 4.1.5 非线程池场设置idle_in_transaction_session_timeout与session_timeout，监测会话超时时间

| 测试步骤：                                                   | 测试结果                                     |
| :----------------------------------------------------------- | -------------------------------------------- |
| 1. 关闭线程池<br/>2. 设置idle_in_transaction_session_timeout与 session_timeout<br/>3. 监测会话超时 | 执行18条用例，发现2个bug，现已修复且验收通过 |

### 4.1.6 jdbc连接数据库设置idle_in_transaction_session_timeout与session_timeout，监测会话超时

| 测试步骤：                                                   | 测试结果                      |
| ------------------------------------------------------------ | ----------------------------- |
| 1.jdbc连接数据库<br/>2.设置idle_in_transaction_session_timeout参数<br/>3.监测会话超时时间 | 执行3条用例，执行结果符合预期 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 3.0.0 build 801d945a | 51         | Passed：50  Failed : 2   | 1            |
| openGauss 3.1.0 build 88ca9bcd | 51         | Passed : 51   Failed : 0 | 0            |

*数据项说明：*

1.openGauss 3.0.0 build 801d945a版本测试时发现1个问题。

2.openGauss 3.1.0 build 88ca9bcd版本回归1个问题单，验收通过。

3.缺陷密度为 1(缺陷个数)/0.254k(代码行数)=3.9(个/kloc)。

## 4.3   后续测试建议

无

# 5     附件

无

 



 

 
