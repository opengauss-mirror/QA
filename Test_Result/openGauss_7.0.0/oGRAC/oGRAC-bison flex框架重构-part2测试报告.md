![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者   |
| ---------- | -------- | ------------ | ------ |
| 2026.7.22 | v1.0     | 测试报告     | chen-czywj |


[TOC]

**Keywords 关键词**：oGRAC、bison解释器

**Abstract 摘要**

本报告针对oGRAC DDL语法在bison解析器下面的表现是否合理进行测试总结，覆盖DDL语法、PL SQL语法等。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

 本报告针对oGRAC DDL语法在bison解析器下执行是否合理进行测试验证，测试通过设置use_bison_parser=true开关打开bison解析器覆盖所有支持的DDL语法，包含PL SQL语法覆盖测试。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------ | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-LTS B012 | oGRAC | 2026.7.1 | 2026.7.15  | chen-czywj |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注                                |
| -------- | ----------------- | ------------------------------------------------------------ | ----------------------------------- |
| 物理机   | aarch64+openEuler | Kunpeng/aarch64，openEuler 20.03 LTS，节点 openGauss104/openGauss106，DSS 共享存储 |  |



# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试针对oGRAC DDL语法在bison解析器下执行是否合理进行测试验证，测试通过设置use_bison_parser=true开关打开bison解析器覆盖所有支持的DDL语法，包含PL SQL语法覆盖测试。输出文本用例 359 条。测试中共发现19个问题，功能基本正常，整体质量良好。


## 3.2 约束说明

- use_bison_parser=true 


## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性         | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                       | 特性质量评估               | 主要风险 |
| ------------ | -------------------------- | -------------------------- | -------------------- | -------------------------------------- | -------------------------- | -------- |
| oGRAC-bison/flex框架重构-part2 | oGRAC parser重构，减少语法逻辑与流程控制代码耦合，提升开发效率 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试 | <font color=green><font color=yellow><font color=yellow><font color=green>▮</font></font></font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品功能属性测试结论

| 功能点验证          | 是否覆盖 |
| :------------------ | :------- |
| DDL语法-CREATE创建对象    | YES      |
| DDL语法-CREATE OR REPLACE创建对象    | YES      |
| DDL语法-ALTER修改对象     | YES      |
| DDL语法-DROP删除对象     | YES      |
| DDL语法-TRUNCATE清空对象 | YES      |
| DDL语法-FLASHBACK闪回 | YES      |
| DDL语法-PURGE清理回收站 | YES      |
| DDL语法-COMMIENT添加注释 | YES      |
| DDL语法-GRANT授权 | YES      |
| DDL语法-REVOKE撤销权限 | YES      |
| DDL语法-ANALYZE优化存储空间 | YES      |
| PL SQL语法-body外：call、exec、execute、declare、begin | YES      |
| PL SQL语法-body内：自定义类型、控制语句、其他语句（assign）、游标等  | YES      |
| 包和触发器             | YES      |

## 4.3 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.2 可靠性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.3 安全&隐私保护测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.4 可服务性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.5 生命周期管理测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.6 韧性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.7 兼容性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.8 升级测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

## 4.3 资料测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 以官方资料为基准测试语法是否描述正确。约束、指导、注意事项等描述是否正确         |  YES        |

# 5 测试对象质量评估

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 19        | 0    | 2     | 17     | 0      |
| 百分比 | 100%     | 0%   | 10.5% | 89.5% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                               | 问题描述                                                     | 问题级别 | 当前状态 |
| :----------------------------------------------------- | :----------------------------------------------------------- | :------- | :------- |
| [#1](https://gitcode.com/opengauss/docs/issues/7251) | 【测试类型：资料测试】oGRAC资料缺失大部分DDL语法     | 次要     | 待办的   |
| [#2](https://gitcode.com/opengauss/oGRAC/issues/280) | 【测试类型：SQL语法】use_bison_parser = true; alter system set recyclebin = true; 执行报错 | 次要     | 已验收   |
| [#3](https://gitcode.com/opengauss/oGRAC/issues/279) | 【测试类型：SQL语法】use_bison_parser = true;DDL创建对象，异常场景对象名称超长时执行挂库         | 主要     | 待办的   |
| [#4](https://gitcode.com/opengauss/oGRAC/issues/278) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法，游标属性-FOUND验证执行报错    | 次要     | 已验收   |
| [#5](https://gitcode.com/opengauss/oGRAC/issues/277) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法，游标名%NOTFOUND报错 | 次要     | 已验收   |
| [#6](https://gitcode.com/opengauss/oGRAC/issues/276) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法显式游标报错 | 次要     | 已验收   |
| [#7](https://gitcode.com/opengauss/oGRAC/issues/275) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法SQL%FOUND执行报错   | 次要     | 已验收   |
| [#8](https://gitcode.com/opengauss/oGRAC/issues/274) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法SQL%ROWCOUNT执行报错 | 次要     | 已验收   |
| [#9](https://gitcode.com/opengauss/oGRAC/issues/272) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-匿名块中RETURN 带值，挂库 | 主要     | 已验收   |
| [#10](https://gitcode.com/opengauss/oGRAC/issues/270) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-GRANT ALL...to ROLE报错 | 次要     | 已验收   |
| [#11](https://gitcode.com/opengauss/oGRAC/issues/269) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-COMMENT-异常-不存在的列名执行成功 | 次要     | 已验收   |
| [#12](https://gitcode.com/opengauss/oGRAC/issues/268) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-COMMENT-异常-不存在的表名执行成功 | 次要     | 已验收   |
| [#13](https://gitcode.com/opengauss/oGRAC/issues/267) | 【测试类型：SQL语法】DDL语法-PURGE TABLE执行报错 | 次要     | 待办的   |
| [#14](https://gitcode.com/opengauss/oGRAC/issues/266) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-CREATE trigger-INSTEAD OF执行报错 | 次要     | 已验收   |
| [#15](https://gitcode.com/opengauss/oGRAC/issues/263) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-CREATE trigger-AFTER UPDATE执行报错 | 次要     | 已验收   |
| [#16](https://gitcode.com/opengauss/oGRAC/issues/262) | 【测试类型：SQL语法】use_bison_parser = true;DDL语法-CREATE trigger-BEFORE INSERT执行报错 | 次要     | 已验收   |
| [#17](https://gitcode.com/opengauss/oGRAC/issues/261) | 【测试类型：SQL语法】use_bison_parser = true; DDL语法-CREATE procedure-END带schema名报错 | 次要     | 已验收   |
| [#18](https://gitcode.com/opengauss/oGRAC/issues/260) | 【测试类型：SQL语法】CREATE function 返回值类型和实际返回数据不匹配创建成功 | 次要     | 待办的   |
| [#19](https://gitcode.com/opengauss/oGRAC/issues/259) | 【测试类型：SQL语法】use_bison_parser = true; DDL语法-CREATE function-END带schema函数名报错 | 次要     | 已验收   |
| [#20](https://gitcode.com/opengauss/oGRAC/issues/258) | 【测试类型：SQL语法】use_bison_parser = true; CREATE table-分区表-复合分区执行报错 | 次要     | 已验收   |


# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 语法点              | 验证策略                                                     | 是否按照测试策略执行 |
| :--- | :------------------ | :----------------------------------------------------------- | :------------------- |
| 1 | DDL语法-CREATE创建对象    | YES      |
| 2 | DDL语法-CREATE OR REPLACE创建对象    | YES      |
| 3 | DDL语法-ALTER修改对象     | YES      |
| 4 | DDL语法-DROP删除对象     | YES      |
| 5 | DDL语法-TRUNCATE清空对象 | YES      |
| 6 | DDL语法-FLASHBACK闪回 | YES      |
| 7 | DDL语法-PURGE清理回收站 | YES      |
| 8 | DDL语法-COMMIENT添加注释 | YES      |
| 9 | DDL语法-GRANT授权 | YES      |
| 10 | DDL语法-REVOKE撤销权限 | YES      |
| 11 | DDL语法-ANALYZE优化存储空间 | YES      |
| 12 | PL SQL语法-body外：call、exec、execute、declare、begin | YES      |
| 13 | PL SQL语法-body内：自定义类型、控制语句、其他语句（assign）、游标等  | YES      |
| 14 | 包和触发器             | YES      |


##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据 

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（k） | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ----------- | -------- |
| openGauss 7.0.0-RC3 B012 | 14              | 359      | 359      | 19    | 18      | 1.06 |

> 说明：缺陷列表中 #1 为资料问题单，不计入缺陷密度。

###  6.3.2 测试用例执行结果统计数据

| 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 359          | 359            | 300  | 47     | 12       | 0           | 100%   | 83.5% |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1、源代码PR：

https://gitcode.com/opengauss/oGRAC/pull/276

https://gitcode.com/opengauss/oGRAC/pull/262

https://gitcode.com/opengauss/oGRAC/pull/261

https://gitcode.com/opengauss/oGRAC/pull/245

https://gitcode.com/opengauss/oGRAC/pull/229

2、文档pr：



3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=da20b209d3b4457e8edfb6bbefac8592

4、测试用例：

openGauss_7.0.0LTS->oGRAC->oGRAC-bison框架重构-PART2
