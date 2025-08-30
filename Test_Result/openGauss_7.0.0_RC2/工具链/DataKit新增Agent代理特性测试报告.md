![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| XX   | XX       | XX       | XX   |

[TOC]

**Keywords 关键词**：agent

**Abstract 摘要**：本此测试为datakit服务器管理新增agent代理功能测试，主要涉及datakit前端功能测试，接口测试，易用性测试，升级测试，兼容性测试以及后端自定义采集任务测试，开发质量良好，无遗留风险

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告为datakit服务器管理新增agent代理特性测试报告，主要测试对象为datakit-7.0.0-RC2及agent-7.0.0-RC2

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

*本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段*

| 版本名称  | 软件包名称                   | 测试起始时间 | 测试结束时间 | 测试人员 |
| --------- | ---------------------------- | ------------ | ------------ | -------- |
| 7.0.0-RC2 | Datakit-All-7.0.0-RC2.tar.gz | 2025.8.12    | 2025.8.22    | 1        |

###  2.1.2 配套测试的版本

| 版本名称  | 配套版本                           | 版本说明 |
| --------- | ---------------------------------- | -------- |
| 7.0.0-RC2 | datakit-agent-7.0.0-RC2-runner.jar |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 硬件型号                             | 硬件配置信息                                                 | 备注 |
| ------------------------------------ | ------------------------------------------------------------ | ---- |
| CentOS Linux release 7.9.2009 (Core) | CPU：Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br />内存：48G<br />硬盘：200G<br />OS：CentOS x86_64 |      |

### 2.2.2 虚拟化平台

### 2.2.3 OS版本

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试完成了datakit前端功能测试，完成了agent后端功能测试，完成升级测试，完成了前端易用性测试，需求100%实现，所有被测特性的测试用例的累计执行覆盖率达到100%，测试发现问题6个，无版本遗留问题，开发质量良好

## 3.2 约束说明

1.datakit配置文件中的server.ssl.enabled设置为false

2.环境java版本>=17

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                 | 特性价值评估                                                 | 应用说明及关键约束假设依赖                         | 关键遗留事项如缺陷等 | 测试整体覆盖情况               | 特性质量评估               | 主要风险 |
| -------------------- | ------------------------------------------------------------ | -------------------------------------------------- | -------------------- | ------------------------------ | -------------------------- | -------- |
| DataKit新增Agent代理 | DataKit服务器管理新增Agent代理，通过Agent代理实现业务数据收集，并推送到DataKit | 1.datakit配置文件中的server.ssl.enabled设置为false | 无                   | 功能测试、易用性测试、升级测试 | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

不涉及

###  4.2.1 性能测试结论

不涉及

###  4.2.2 可靠性测试结论

不涉及

###  4.2.3 安全&隐私保护测试结论

不涉及

### 4.2.4 可服务性测试结论

| 测试步骤                                  | 测试结果 |
| ----------------------------------------- | -------- |
| 查看前端文字是否正确                      | 通过     |
| 查看ui缩放过程中，界面显示是否正常        | 通过     |
| 查看agent安装流程中提示信息是否详细、正确 | 通过     |

### 4.2.5 生命周期管理测试结论

不涉及

### 4.2.6 韧性测试结论

不涉及

###  4.2.7 兼容性测试结论

| 测试步骤                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 基座数据库使用intarkdb，安装agent并进行启动、停止、卸载操作  | 通过     |
| 基座数据库使用opengauss，安装agent并进行启动、停止、卸载操作 | 通过     |

###  4.2.8 升级测试结论

*按照测试策略中制定的升级路径及具体版本号（包括补丁），给出验证结论。（如本测试阶段涉及则填写）*

| 测试步骤                                       | 升级路径              | 测试结果 |
| ---------------------------------------------- | --------------------- | -------- |
| 1.更新代理服务jar包，重启代理服务2.重启datakit | 不同的agent日构建包   | 通过     |
| 2.升级datakit版本，重启datakit                 | 不同的datakit日构建包 | 通过     |

## 4.3 资料测试结论

*按照总体测试策略中制定的兼资料测试策略，给出验证结论。（如本测试阶段涉及则填写）*

*如下表格所示。下表仅供参考，不作为强制输出方式*

| 序号 | 测试章节                   | 测试结论 |
| ---- | -------------------------- | -------- |
|      | 资料pr暂未合入，已提单跟踪 |          |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试主要覆盖datakit前端功能测试，接口测试，易用性测试，升级测试，兼容性测试以及后端自定义采集任务测试

##  5.2 缺陷统计和分析

*给出各特性或模块缺陷的分布或分类统计以及缺陷走势分析。建议通过表格说明，下表仅供参考，不作为强制输出方式*

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 7        | 0    | 0    | 7    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1100 | [Bug]: datakit前端点击启动/停止agent，后端启动/停止失败，但前端无报错提示 | 次要     | 修复中   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1099 | [Bug]: datakit安装机器ip和agent安装机器ip网段不同时，agent的配置文件中agent.server配置为空 | 次要     | 待办的   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1097 | [Bug]: datakit在删除服务器用户时需要检测该用户是否安装了agent | 次要     | 待回归   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1096 | [Bug]: datakit安装服务器代理页面安装路径需要添加文字提示     | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1095 | [Bug]: agent安装的安装路径识别待优化                         | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1094 | [Bug]: datakit安装服务器agent代理，输入框识别处理需要优化    | 次要     | 已验收   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1112 | [Bug]: datakit资源中心资料未更新                             | 次要     | 待办的   |

# 6 测试过程评估

##  6.1 测试策略回顾

*回顾本阶段的测试策略，建议以表格的方式检查测试策略规定的活动是否都已经落实。*

| 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---------- | ------------------------------------------------------------ | -------------------- |
| 功能测试   | 1.前端agent的安装、停止、启动、卸载功能                      | YES                  |
| 接口测试   | 1.前端agent的安装、停止、启动、卸载功能                      | YES                  |
| 易用性测试 | 1.ui显示是否合理2.异常场景是否有合理报错3.操作流程过程中文字提示是否详细 | YES                  |
| 兼容性测试 | 1.使用opengauss作为基座数据库2.使用intarkdb作为基座数据库    | YES                  |
| 升级测试   | 1.升级agent版本2.升级datakit版本                             | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| Datakit-All-7.0.0-RC2              | 8                | 47         | 47         | 6          | 20.481         | 0.3      |
| datakit-agent-7.0.0-RC2-runner.jar |                  |            |            |            |                |          |

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 47             | 47               | 41     | 6      | 0       | 0           | 100%   | 87%        |

# 7 附件

##  7.1 附件1：遗留问题列表

##  7.2 附件2：特性相关PR

代码pr：

[需求-资源中心Agent优化: Socket后台线程任务删除接口优化-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1144)

[基座插件JDK版本升级-JDK17-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1151)

[需求-资源中心Agent优化: 建表语句-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1161)

[需求-资源中心Agent优化: Agent Proxy-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1165)

[需求-资源中心Agent优化: Agent Proxy2-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1167)

[需求-资源中心Agent优化: Agent Proxy3-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1171)

[需求-资源中心Agent优化: Agent Proxy 项目初始化 http client-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1173)

[需求-资源中心Agent优化: Agent Proxy Task Group-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1179)

[需求-资源中心Agent优化: Agent Proxy TaskExecutor-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1180)

[需求-资源中心Agent优化: Agent Proxy os command utils-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1185)

[需求-资源中心Agent优化: Agent Server-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1190)

[datakit 资源中心Agent优化: Agent Server 超大MR合入备案-community-GitCode](https://gitcode.com/opengauss/community/pull/424)

[agent前端适配-修改服务器/agent安装页面-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1206)

[agent前端适配-样式修改/标签管理弹窗修改风格-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1207)

[agent前端适配-新增服务器页面-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1209)

[agent前端适配-agent管理页面-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1210)

[解除agent安装编辑弹窗屏蔽-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1211)

[需求-资源中心Agent优化: 删除host逻辑增加Agent安装状态检查-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1215)

[优化Agent构建打包-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1220)

[agent页面-intarkdb系统表增加新页面-openGauss-workbench-GitCode](https://gitcode.com/opengauss/openGauss-workbench/pull/1222)

测试设计：

[测试设计-TestPlan](https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=939813cea6d14340816a87c98bf7548f&hideDevcloudHead=true)

测试用例：

[测试用例-TestPlan](https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb2100010cof2bn7)

![image-20250829113934880](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250829113934880.png)
