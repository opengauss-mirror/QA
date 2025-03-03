![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期        | 修订版本 | 修改描述 | 作者  |
|-----------|------| -------- |-----|
| 2025.2.27 | 1.0  |        | 姚雨驰 |

[TOC]

**Keywords 关键词**：ipv6/datakit

**Abstract 摘要**：本次测试主要测试datakit中所有功能模块对ipv6的数据库实例的使用
**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述
本报告为Datakit支持添加ipv6实例特性测试报告，被测对象为datakit 7.0.0-RC1 B014版本

# 2 测试版本说明


## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称           | 软件包名称   | 测试起始时间   | 测试结束时间    | 测试人员 |
|----------------|---------|----------|-----------|------|
| 7.0.0-RC1 B014 | Datakit-7.0.0-RC1.tar.gz | 2025.2.7 | 2025.2.20 | 姚雨驰  |

###  2.1.2 配套测试的版本

不涉及

## 2.2 测试环境描述


###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号       | 硬件配置信息                                                                                                        | 备注 |
|------|------------|---------------------------------------------------------------------------------------------------------------| ---- |
| 虚拟机  | CentOS_x86 | CPU：Intel(R) Xeon(R) Platinum 8378A CPU @ 3.00GHz<br />内存：15G<br />硬盘：100G<br />OS：CentOS Linux release 7.6.1810 |      |



# 3 版本概要测试结论、关键风险和规避措施



## 3.1 测试结论总结


datakit 7.0.0-RC1版本测试完成，本次测试执行两轮，完成了datakit所有插件的功能测试和资料测试，发现问题3个，开发质量一般。所有被测特性的测试用例的累计执行覆盖率达到100%，上述测试发现的问题都得到了修正，回归测试结果正常。

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

datakit所有插件均能正确的显示ipv6地址，能够管理和使用ipv6实例和集群，能正常监控ipv6实例和集群

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性 | 特性价值评估       | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ---- |--------------|---------------|------------|----------| -------------------------- |------|
| Datakit支持添加ipv6实例  | 满足中移在线提出的的需求 | 无             | 无          | 功能全覆盖    | <font color=yellow><font color=yellow>▲</font></font> | 无    |


## 4.2 产品质量属性目标(DFX)测试结论


###  4.2.1 性能测试结论

无
###  4.2.2 可靠性测试结论

无

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

*按照总体测试策略中制定的兼资料测试策略，给出验证结论。（如本测试阶段涉及则填写）*

*如下表格所示。下表仅供参考，不作为强制输出方式*

| 序号 | 测试章节                | 测试结论         |
| ---- |---------------------|--------------|
|      | workbench仓库-》readme | 整体质量良好，未发现问题 |
|      |                     |              |

# 5 测试对象质量评估


##  5.1 覆盖率分析

本次测试覆盖datakit资源中心、安装部署、基础运维、智能运维、告警监控、数据迁移、业务开发、兼容性评估工具、智能参数调优，功能模块覆盖率为100%
##  5.2 缺陷统计和分析


###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要  | 不重要 |
| ------ |------|----|----|-----|-----|
| 数目   | 3    | 0  | 0  | 3   | 0   |
| 百分比 | 100  | 0  | 0  | 100 | 0   |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- |------|------|
| IBLPG3        |【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】导入ipv6服务器，网络显示异常          | 次要   | 已验收  |
| IBLMQ5      |【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】ipv6服务器基本信息栏格式需要优化          | 次要   | 已验收  |
|IBM329|【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】使用datakit安装ipv6数据库，默认配置的白名单任然是ipv4| 次要   | 已验收  |
# 6 测试过程评估



##  6.1 测试策略回顾



| 编号 | 特性                 | 验证策略                            | 是否按照测试策略执行 |
|----|--------------------|---------------------------------|------------|
| 1  | datakit管理ipv6数据库集群 | ipv6数据库安装-》导入-》管理-》监控       | yes        |
| 2  | datakit工具使用ipv6数据库 | 数据迁移-》兼容性评估-》业务开发-》录制回放-》智能参数调优 | yes        |
|    |                    |                                 |            |

##  6.2 测试设计评估



| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- |---------|------|----------|
|      | 无       | 无    | 无        |
|      |         |      |          |
|      |         |      |          |

##  6.3 测试执行评估



###  6.3.1 测试执行统计数据

| 版本名称       | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| 7.0.0-RC1 B014 | 12               | 55         | 55         | 3          | 0.496          | 6        |
|                |                  |            |            |            |                |          |

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 55             | 55               | 47     | 7      | 0       | 1           | 100%   | 85%        |
| 第一轮 | 54             | 54               | 54     | 0      | 0       | 0           | 100%   | 100%       |


| 异常用例情况          | 影响分析                                           | 规避措施       | 后续计划 |
|-----------------|------------------------------------------------|------------|------|
| Unavailable用例1个 | 不可以用例受限于windows环境不支持ipv6，所有不能使用ipv6地址登录datakit | 使用ipv4地址登录 | 无    |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR
代码PR：<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/986<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-tools-chameleon/pulls/265<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-migration-portal/pulls/177<br />
https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/273<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/1029<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/1036<br />资料PR
https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/7220<br />
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/1046
<br />测试设计：<br />
https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=335a3bec50324fb9a8edcce84123af4d&hideDevcloudHead=true<br />
<br />用例：<br />
openGauss_7.0.0-》特性目录-》工具链-》Datakit支持添加IPV6实例
https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg<br />
