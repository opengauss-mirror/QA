![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2025.12.29 | v1.0     | 测试报告初稿 | czy |

**Keywords 关键词**：黑匣子，cm，restapi

**Abstract 摘要**：本文档主要介绍基于oGRecorder与CM提供的API，可以支持集群管理的部分能力，对此提供相关测试方案以及测试结论。



**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告主要测试oGRecorder与CM对外提供restAPI功能，包含通过本地连接、远程连接、异常请求等方式验证restAPI的基础功能、可靠稳定性。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                         | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------------------------------- | ---------- | ------------ | ------------ | -------- |
| grcmd  | oGRecorder         | 2025-12-12     | 2025-12-17    | czy  |
| cm  | openGauss-CM         | 2025-12-12     | 2025-12-17    | czy  |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试基于restAPI功能，验证服务端和客户端HTTPS接口的测试，主要包含基本功能、可靠性测试，输出测试用例30个，执行测试1轮，发现6个异常问题，总体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                           | 应用说明及关键约束假设依赖            | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | -------------------------------------- | ------------------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 黑匣子特性增强-API优化 | 对外提供可靠的集群运维接口，方便服务使用以及集成到上层应用，扩大服务应用范围 | 无 | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 功能测试结论

| 测试场景 | 测试点                            | 测试结果 |
| -------- | --------------------------------- | -------- |
| 接口测试 | HTTPS、HTTP协议   | 预期合理 |
|          | 白名单测试   | 预期合理 |
|          | 请求类型：GET、POST、其他   | 预期合理 |
| 无参接口 | 无参请求&带参请求  | 预期合理 |
| 可选入参接口 | 无参请求&错误带参请求 | 预期合理 |
|          | 带参请求参数值：正常值&异常值   | 预期合理 |
| 无参接口 | 无参请求&错误带参请求 | 预期合理 |
|          | 带参请求参数值：正常值&异常值   | 预期合理 |
|          | 多参数：参数顺序排列组合   | 预期合理 |



### 4.2.2 可靠性测试结论

| 测试场景 | 测试点                                   | 测试结果         |
| -------- | ---------------------------------------- | ---------------- |
| 进程异常  | restAPI进程                 | 预期合理         |
|          | oGRecorder进程                      | 预期合理         |
|          | cm_agent                                 | 预期合理         |
|          | cm_server                             | 预期合理         |
| 网络异常   | 网络中断            | 预期合理         |
|          | 网络闪断             | 预期合理         |

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

无

### 4.2.10 资料测试结论
| 测试场景 | 测试点                            | 测试结果 |
| -------- | --------------------------------- | -------- |
| 资料     | oGRecorder+CM集群搭建   | 验收通过 |
|          | cmRestAPI进程部署   | 验收通过 |
|          | 接口定义文档   | 验收通过 |

[数据保险柜部署介绍](https://docs.opengauss.org/zh/docs/latest/tool_and_commandreference/ogrecorder/black_box_deployment_introduction.html#6-%E5%88%87%E6%8D%A2%E5%88%B0-root-%E7%94%A8%E6%88%B7%E6%89%A7%E8%A1%8C%E9%A2%84%E5%AE%89%E8%A3%85)</br>
[CMRestAPI](https://docs.opengauss.org/zh/docs/latest/tool_and_commandreference/features.html)

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖涉及HTTPS接口的测试、参数测试、可靠性场景测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 11        | 0    | 1    | 10    | 0      |
| 百分比 | 100%     | 0%   | 9%   | 91% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                         | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [CM-RestAPI:14](https://gitcode.com/opengauss/CM-RestAPI/issues/14) | [Bug]: 【黑匣子api】获取黑匣子指定参数的结果，接口入参含有特殊字符-单引号、双引号，接口未识别错误参数，请求正常  | 次要     | 待办的   |
| [CM-RestAPI:11](https://gitcode.com/opengauss/CM-RestAPI/issues/11) | [Bug]: 【黑匣子】stopRestApi接口测试新增传入异常参数，执行成功          | 次要     | 已验收   |
| [CM-RestAPI:10](https://gitcode.com/opengauss/CM-RestAPI/issues/10) | [Bug]: 【黑匣子api】停止对应节点restapi服务，接口未传参数请求成功，返回shutdown成功，查看状态显示在线 | 次要     | 已验收   |
| [CM-RestAPI:9](https://gitcode.com/opengauss/CM-RestAPI/issues/9) | [Bug]: 【黑匣子api】获取黑匣子指定参数的结果，接口返回值异常 | 次要     | 已验收   |
| [CM-RestAPI:8](https://gitcode.com/opengauss/CM-RestAPI/issues/8) | [Bug]: 【黑匣子api】获取gr日志文件信息接口maxResults参数边界值未处理 | 次要     | 已验收   |
| [CM-RestAPI:7](https://gitcode.com/opengauss/CM-RestAPI/issues/7) | [Bug]: 【黑匣子api】GET请求拼接参数含有双引号时处理时将双引号缺省 | 次要     | 已验收   |
| [CM-RestAPI:6](https://gitcode.com/opengauss/CM-RestAPI/issues/6) | [Bug]: 【黑匣子api】查看指定节点黑匣子实例状态错误 | 次要     | 已验收   |
| [oGRecorder:59](https://gitcode.com/opengauss/oGRecorder/issues/59) | [Bug]: 黑匣子按照文档部署安装报错，不能正常部署 | 主要     | 已验收   |
| [docs:7052](https://gitcode.com/opengauss/docs/issues/7052) | [Bug]: 【资料】官网数据保险柜缺失RESTAPI部署搭建指南 | 次要     | 已验收   |
| [CM:254](https://gitcode.com/opengauss/CM/issues/254) | [Bug]: 【黑匣子】cm_agent参数enable_fence_dn设置字符串成功 | 次要     | 已验收   |
| [CM:253](https://gitcode.com/opengauss/CM/issues/254) | [Bug]: 【黑匣子】cm_server参数默认值为2，修改参数恢复到默认值报错 | 次要     | 已验收   |


# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                             | 是否按照测试策略执行 |
| ---- | ---------- | ---------------------------------------------------- | -------------------- |
| 1    | 功能测试   | 覆盖流程、正常&异常接口场景测试                               | YES                  |
| 2    | 可靠性测试 | 覆盖进程异常、网络故障以及其他场景 | YES                  |


##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC3 B009 | 5                | 30         | 30         | 6          | 3.4K | 1.8KLOC        |

* 缺陷密度说明：本次测试共发现问题11个，其中2个CM相关问题（CM:254、CM:253）为CM功能历史问题，1个资料问题（docs：7052），oGRecorder部署问题（oGRecorder:59）为其他pr影响，RestAPI问题中有一个问题（CM-RestAPI:8）在资料中添加说明规避。故DI计算问题单数目为：6个

缺陷密度：6/3.4K = 1.8KLOC

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ |
| 30           | 30               | 22     | 8      | 0       | 0           | 100%   |

本次测试共输出测试用例30个，执行测试共1轮，发现6个异常问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitcode.com/opengauss/CM-RestAPI/pull/23
https://gitcode.com/opengauss/CM/pull/446
https://gitcode.com/opengauss/oGRecorder/pull/79

资料pr：

https://gitcode.com/opengauss/docs/pull/8305

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=33bdac9dbee243778dbdc889bcdf5c13
