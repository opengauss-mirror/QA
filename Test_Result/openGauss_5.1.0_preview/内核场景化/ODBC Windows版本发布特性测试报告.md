![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述         | 作者  |
| ---------- | -------- | ---------------- | ----- |
| 2023/05/18 | 1.0      | 特性报告初稿完成 | lixin |
| 2023/07/10 | 2.0      | 修改缺陷描述 | lixin |

关键词：ODBC、Windows

摘要：本文档主要介绍测试Windows环境下，ODBC驱动安装配置、通过ODBC连接openGasuss数据库、常见DDL/DML/DCL语句执行及资料测试，并给出最终测试结论。

缩略语清单：

| 缩略语    | 英文全名  | 中文解释          |
| --------- | --------- | ----------------- |
|NA |  | |

# 1     特性概述

 ODBC Windows驱动是一种使用ODBC标准的数据库驱动程序，它允许应用程序通过统一接口与openGauss数据库交互。目前ODBC只支持linux，不支持Windows，本特性输出Windows ODBC驱动程序包，后续在Windows操作系统中，可通过控制面板中的ODBC数据源管理器配置和管理，允许程序通过ODBC接口来访问openGauss数据库。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.0 build 338028e0 | 2023-05-11   | 2023-05-17   |

| 环境信息 | 配置信息                                                     | 备注               |
| -------- | ------------------------------------------------------------ | ------------------ |
| 虚拟机   | Inter(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 4核<br/>内存：32GB<br/>硬盘：220GB<br/>OS：openEuler release 20.03(LTS) | 安装openGauss      |
| 虚拟机   | Inter(R) Xeon(R) CPU ES-2690 v4 @ 2.60GHz 2.59GHz<br/>内存：16GB<br/>硬盘：300GB<br/>OS：Windows 10 企业版 LTSC | 安装ODBC、odbct32w |

| 软件名称            | 软件版本     | 备注 |
| ------------------- | ------------ | ---- |
| odbc data source 32 | 10.0.17763.1 |      |
| odbct32w            | 02.70.8019   |      |

# 3     测试结论概述

## 3.1   测试整体结论

共设计22个测试用例，主要包含Windows环境下，ODBC安装及配置、ODBC连接openGauss数据库参数校验、连接成功后常见DDL/DML/DCL语句执行等。共计发现6个问题，其中4个问题开发分析和linux下odbc表现一致，非问题取消，其它2个问题均已修复且回归验证通过，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | Windows环境下，ODBC安装及配置测试                            |
| 功能测试 | Windows环境下，ODBC连接openGauss数据库参数校验               |
| 功能测试 | Windows环境下，通过ODBC连接openGauss数据库，常见DDL/DML/DCL语句执行等场景测试 |
| 资料测试 | ODBC相关资料，描述清晰，示例正确                           |

## 3.2   约束说明

预先安装好ODBC应用程序，并配置好需要的odbc.ini和odbcinst.ini

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6   | 0    | 0  | 3 | 3    |
| 百分比 |   100%       | 0%   | 0%  | 50% | 50% |

### 3.3.3 问题单汇总

|序号| issue号                                                      | 级别 | 问题简述                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------- | ------ |
| 1 | [I76LHO](https://gitee.com/opengauss/openGauss-connector-odbc/issues/I76LHO?from=project-issue) | 次要 | 【测试类型：接口功能】【测试版本：5.1.0】 主机宕机，gsql备节点查询正常，windows环境下，通过ODBC连接备节点报错server is in hot standby mode | 已取消 |
| 2 | [I73V2O](https://gitee.com/opengauss/openGauss-connector-odbc/issues/I73V2O?from=project-issue) | 次要 | 【测试类型：接口功能】【测试版本：5.1.0】 windows环境，odbc数据源管理配置其他信息正确，database为空时，连接报错信息不合理 | 已取消 |
| 3 | [I73IMX](https://gitee.com/opengauss/openGauss-connector-odbc/issues/I73IMX?from=project-issue) | 次要 | 【【测试类型：接口功能】【测试版本：5.1.0】 windows环境使用odbc连接数据库，数据源配置时server为空，报错信息重复 | 已取消 |
| 4 | [I73ITJ](https://gitee.com/opengauss/openGauss-connector-odbc/issues/I73ITJ?from=project-issue) | 不重要 | 【测试类型：接口功能】【测试版本：5.1.0】 windows环境，odbc数据源管理配置时，其他信息正确，Server为未加白名单ip，连接报错信息指向不清晰 | 已取消 |
| 5    | [I73P5T](https://gitee.com/opengauss/docs/issues/I73P5T?from=project-issue) | 不重要 | 【测试类型：资料】【测试版本：5.1.0】需求ODBC windows版本发布相关资料未更新至社区 | 已验收 |
| 6 | [I7F620](https://gitee.com/opengauss/openGauss-connector-odbc/issues/I7F620?from=project-issue) | 不重要 | 【测试类型：资料】【测试版本：5.1.0】应用开发指南>基于ODBC开发章节，点击“odbc文档”404 | 已验收 |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 Windows环境下，ODBC安装及配置测试

| 测试步骤                                                     | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. Windows环境下，下载安装ODBC驱动<br />2. linux环境安装openGauss数据库，创建用户，配置白名单<br />3. 打开windows自带的数据管理，选择odbc data source 32，弹窗中配置数据源信息<br />4.点击“Test”按钮，测试是否连接成功 | 执行1条用例，执行通过未发现缺陷 |

### 4.1.2 Windows环境下，ODBC连接openGauss数据库参数校验

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1. Windows环境下，下载安装ODBC驱动<br />2. linux环境安装openGauss数据库，创建用户，配置白名单<br />3. 打开windows自带的数据管理，选择odbc data source 32，弹窗中配置数据源信息，不同参数值组合校验<br />4.点击“Test”按钮，测试是否连接成功，若失败是否有合理提示信息 | 执行13条用例，执行通过未发现缺陷 |

### 4.1.3 Windows环境下，通过ODBC连接openGauss数据库，常见DDL/DML/DCL语句执行等场景测试

| 测试步骤                                                     | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. Windows环境下，下载安装ODBC驱动、odbct32w<br />2. linux环境安装openGauss数据库，创建用户，配置白名单<br />3. 打开windows自带的数据管理，选择odbc data source 32，弹窗中配置数据源信息并保存<br />4.打开odbct32w，选择步骤3中配置的数据源，弹窗中分别输入常见的DDL/DML/DCL语句并执行 | 执行9条用例，执行通过未发现缺陷 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 5.1.0 build 338028e0 | 22     | Passed：22<br>Failed：0<br />Unavaliable：0 | 0         |

*数据项说明：*

* 累计发现问题单6个，4个非问题取消，2个资料但不计入代码缺陷密度计算，代码缺陷密度为0(缺陷个数)/0.38k(代码行数)=0(个/kloc)

## 4.3   后续测试建议

无

# **5     附件**

无

****

