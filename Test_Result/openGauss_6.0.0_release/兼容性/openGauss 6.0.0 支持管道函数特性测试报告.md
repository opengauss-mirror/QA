![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期     | 修订版本 | 修改描述     | 作者            |
| -------- | -------- | ------------ | --------------- |
| 2024.6.8 | 1.0      | 初稿         | zou_jialiang050 |
| 2024.8.8 | 2.0      | 修改缺陷描述 | zou_jialiang050 |

[TOC]

**Keywords 关键词**：支持管道函数

**Abstract 摘要**：本文对支持A兼容性管道函数特性进行测试，并给出最终结论。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

***


# 1 概述

openGauss实现对A兼容管道函数创建及调用进行支持，实现了通过create function方式及create package方式对管道函数进行创建，支持返回顺序数组及嵌套表类型，支持使用pipe row进行输出，并支持gs_dump导出备份功能。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                       | 软件包名称                              | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------------ | --------------------------------------- | ------------ | ------------ | -------- |
| openGauss 6.0.0 build ada68ffa | openGauss_6.0.0_PACKAGES_RELEASE.tar.gz | 2024-6-27    | 2024-7-10    | 邹佳良   |
| openGauss 6.0.0 build 8d33e9eb | openGauss_6.0.0_PACKAGES_RELEASE.tar.gz | 2024-7-12    | 2024-7-12    | 邹佳良   |
| openGauss 6.0.0 build f9b53221 | openGauss_6.0.0_PACKAGES_RELEASE.tar.gz | 2024-8-8     | 2024-8-8     | 邹佳良   |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| NA       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号   | 硬件配置信息                                                 | 备注 |
| -------- | ---------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | x86_CentOS | CPU：Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br />内存：62GB<br />硬盘：100GB<br />OS：CentOS Linux release 7.9.2009 (Core)<br />文件系统：dos<br />网卡：eth0 |      |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

支持管道函数特性测试中，共执行用例30条，覆盖功能测试、性能测试、兼容性测试及资料文档验证，测试执行覆盖率达到100%，共发现问题3条，，缺陷密度为1.1个/kloc，已全部解决，回归测试结果正常，无遗留问题，整体质量良好。

## 3.2 约束说明

在openGauss侧兼容A库下验证

管道函数返回类型必须是schema级或package内定义的集合类型

管道函数功能实现与openGauss原生return next、query类似，故返回大量数据时函数性能较差且受work_mem参数影响

不支持返回嵌套嵌套表类型，及以end func_name结尾创建pipelined函数

openGauss侧目前TableOf实现是数组形式，因此管道函数作为targetlist返回时，返回结果虽为TableOf类型，但实际访问能力缺失

## 3.3 关键风险和规避措施

| 风险类型 | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| -------- | -------- | -------- | -------------- | ------ | -------- |
| NA       |          |          |                |        |          |

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                    | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                     | 特性质量评估               | 主要风险 |
| ----------------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ------------------------------------ | -------------------------- | -------- |
| 支持A兼容性管道函数特性 | 管道函数可以返回嵌套表、顺序数组类型，通过pipe row返回数据，作为targetlist返回，通过create function及create package方式创建pipelined函数，作为table子句查询返回结果，且支持使用gs_dump对pipelined函数对象进行导出 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试、性能测试和资料测试 | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.修改work_mem为64Kb后调用pipelined函数返回大量数据，逐渐增加返回数据量，观测函数返回时间并增加work_mem值继续增加数据查看返回时间变化 | 1.函数返回时间随数据量增加而增加，增加work_mem后函数返回时间减少 |

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

| 测试步骤                                                | 升级路径       | 测试结果                                                     |
| ------------------------------------------------------- | -------------- | ------------------------------------------------------------ |
| 1.测试升级确认前、升级回退、升级确认后创建pipelined函数 | 不涉及特定路径 | 1.升级确认后创建及调用pipelined函数成功，升级确认前、升级回滚创建pipelined函数均失败。 |

## 4.3 资料测试结论

| 序号 | 测试章节                                          | 测试结论                 |
| ---- | ------------------------------------------------- | ------------------------ |
| 1    | SQL参考>SQL语言结构和语法>SQL语法>CREATE FUNCTION | 整体质量良好，未发现问题 |
| 2    | SQL参考>存储过程>控制语句>返回语句>PIPE ROW       | 整体质量良好，未发现问题 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试主要涉及pipelined函数返回类型，主要包括顺序数组类型及嵌套表类型，通过pipe row返回集合类型，自治事务打开及关闭情况下执行DML操作，函数是否作为targetlist返回，通过create package方式创建pipelined函数，作为table子句查询及相关异常场景，其中正常场景中验证功能与需求相符，异常场景有合理报错，需求无遗留问题，整体质量良好。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 2    | 1    | 0      |
| 百分比 | 100%     | 0%   | 66%  | 33%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IA9XOF](https://e.gitee.com/opengaussorg/issues/table?issue=IA9XOF) | 【测试类型：SQL功能】【测试版本：6.0.0】pipelined函数关闭自制事务执行DML操作成功 | 次要     | 已验收   |
| [IAALCP](https://e.gitee.com/opengaussorg/issues/table?issue=IAALCP) | 【测试类型：SQL功能】【测试版本：6.0.0】减少work_mem后大量调用pipelined函数，数据库core | 主要     | 已验收   |
| [IAA1FI](https://e.gitee.com/opengaussorg/issues/table?issue=IAA1FI) | 【测试类型：SQL功能】【测试版本：6.0.0】使用create package创建pipelined函数包体中赋值错误数据库core | 主要     | 已验收   |
| [IA9O1L](https://e.gitee.com/opengaussorg/issues/table?issue=IA9O1L) | 【测试类型：SQL功能】【测试版本：6.0.0】pipe row返回未declare的变量失败、嵌套表嵌套嵌套表类型、创建函数end指定函数名** | 次要     | 待办的   |

IA9O1L非本次需求引入，不计入代码缺陷密度

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性     | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | -------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试 | 主要包括顺序数组类型及嵌套表类型，通过pipe row返回集合类型，自制事务打开及关闭情况下执行DML操作，函数是否作为targetlist返回，通过create package方式创建pipelined函数，作为table子句查询，使用gs_dump工具进行导入导出及相关异常场景 | YES                  |
| 2.   | 性能测试 | 验证pipelined函数返回大量数据时与wok_mem参数关系             | YES                  |
| 3.   | 升级测试 | 验证升级确认前、升级后回滚、升级后提交创建pipelined函数      | YES                  |
| 4.   | 资料测试 | 验证资料描述是否准确                                         | YES                  |

##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称             | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 缺陷密度   | 代码量   |
| -------------------- | ---------------- | ---------- | ---------- | ---------- | ---------- | -------- |
| openGauss 6.0.0 B021 | 13               | 30         | 30         | 3          | 1.1个/kloc | 3.3kloc  |
| openGauss 6.0.0 B023 | 0.5              | 2          | 2          | 0          | 0个/kloc   | 0.12kloc |
| openGauss 6.0.0 B026 | 0.5              | 1          | 1          | 0          | 0个/kloc   | 0.32kloc |

说明：资料单不计入缺陷密度。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 30           | 30               | 28     | 0      | 2       | 0           | 100%   | 93.3%      |

| 异常用例情况                           | 影响分析                                          | 规避措施 | 后续计划                       |
| -------------------------------------- | ------------------------------------------------- | -------- | ------------------------------ |
| openGauss不支持嵌套创建嵌套表类型      | 不支持嵌套创建嵌套表类型为返回类型的pipelined函数 | 暂无     | 待后续规划                     |
| openGauss不支持以end func_name创建函数 | 不支持以end func_name方式创建pipelined函数        | 暂无     | 待后续规划                     |
| 不支持创建varry类型                    | 不能创建返回varry类型pipelined函数                | 暂无     | 待支持object类型需求合入后解决 |



##  7.1 附件1：遗留问题列表

| 序号 | 问题单号 | 问题描述 | 分类 | 问题级别 | 问题分析与影响 | 规避措施 |
| ---- | -------- | -------- | ---- | -------- | -------------- | -------- |
| NA   |          |          |      |          |                |          |

##  7.2 附件2：特性相关PR

1.源代码PR:

https://gitee.com/opengauss/openGauss-server/pulls/5553

2.资料PR:

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6400

3.测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=7f251b3fa6c94371b34f8ff78f66a8a4&hideDevcloudHead=true