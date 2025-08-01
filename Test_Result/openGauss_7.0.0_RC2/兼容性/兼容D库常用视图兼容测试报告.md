![avatar](https://gitee.com/opengauss/QA/raw/master/images/openGauss.png)

版权所有 © 2024 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者    |
| --------- | -------- | ------------ | ------- |
| 2025.4.25 | v1.0     | 测试报告初稿 | xiaqi14 |

**Keywords 关键词**：兼容D库常用视图

**Abstract 摘要**：兼容D数据库兼容性能力提升，包括以下常用系统视图场景：

sysobjects：数据库中创建的每个对象（例如约束、默认值、存储过程）都对应一行。

syscolumns：为每个表和视图中的每列返回一行，并为数据库中的存储过程的每个参数返回一行。

sysindexes：当前数据库中的每个索引和表各对应一行。

sysindexkeys：包含有关数据库的索引中的键或列的信息。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 概述

此特性主要用来支持在D兼容模式下，提升兼容D数据库兼容性能力，返回数据库每一行创建的对象，视图，以及索引和主键等信息。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build c20928a5 | openGauss  | 2025-4-15    | 2024-4-29    | xiaqi14  |

### 2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号      | 硬件配置信息                                                 | 备注 |
| -------- | ------------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | x86+openEuler | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>MEM: 32GB<br/>DISK: 200GB<br/>OS: openEuler 20.03 (LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

##  3.1 测试结论总结

openGauss D库兼容性常用语法兼容 ，共计执行了38个用例，主要覆盖了功能测试、兼容性测试、资料测试。功能测试覆盖了3个子特性的基本功能，第一，视图返回值的入参边界值组合测试。第二，创建数据类型和函数返回回显列信息测试验证。第三，针对多主键的表和存储过程及触发器测试，查看返回值信息，执行结果符合预期。测试中发现1个问题，问题已修复，整体质量良好.

## 3.2 约束说明

- 仅允许在D兼容性下有shark插件使用
- 7.0.0-RC1版本(`带D库和shark`插件)升级到7.0.0-RC2 B001版本，该视图功能不支持

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性                            | 特性价值评估               | 应用说明及关键约束假设依赖                                   | 关键遗留事项如缺陷等 | 测试整体覆盖情况                       | 特性质量评估 | 主要风险 |
| ------------------------------- | -------------------------- | ------------------------------------------------------------ | -------------------- | -------------------------------------- | ------------ | -------- |
| 【兼容性组】D兼容性常用语法兼容 | 提升兼容能力，方便用户迁移 | https://docs.opengauss.org/zh/docs/latest/docs/ExtensionReference/shark-%E7%B3%BB%E7%BB%9F%E8%A7%86%E5%9B%BE.html | 无                   | 主要覆盖功能测试、兼容性测试和资料测试 | ▮            | 无       |

*特性质量评估说明*：

●： *表示特性不稳定，风险高*

▲： *表示特性基本可用，遗留少量问题*

▮： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.3 安全&隐私保护测试结论

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

### 4.2.7 兼容性测试结论

| 测试步骤                                                     | 测试结果                                |
| ------------------------------------------------------------ | --------------------------------------- |
| 1. 创建兼容"A"、B"、"C"、"PG"类型数据库<br/> 2.分别执行<br />select * from sys.sysobjects;<br />select * from sys.syscolumns;<br />select * from sys.sysindexes;<br />select * from sys.sysindexkeys; | 执行2条用例，执行结果符合预期，测试通过 |

### 4.2.8 升级测试结论

| 测试步骤                                                     | 升级路径            | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------- | ------------------------------------------------------------ |
| 带D库和shark插件<br/>1. 7.0.0-RC1版本升级到7.0.0-RC2 B01版本未提交<br/>查询系统视图<br/>2. 7.0.0-RC1版本升级到7.0.0-RC2 B01版本并提交<br/>查询系统视图<br/>不带D库和shark插件<br/>1.7.0.0-RC1版本升级到7.0.0-RC2 B01版本并提交<br/>2.查询系统视图 | 7.0.0-RC1-7.0.0-RC2 | 执行1条用例，执行结果符合预期。带D库和shark插件升级未提交与提交，该视图功能不支持。不带D库和shark插件，升级并提交后创建D库和shark插件，使用查询系统视图功能正常。 |

## 4.3 资料测试结论

| 序号 | 测试章节                  | 测试结论 |
| ---- | ------------------------- | -------- |
| 1    | 文档地图-》shark-系统视图 | 测试通过 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试主要针对D库shark插件中的4个系统视图基本功能进行测试.

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 5.2.2 缺陷列表

| 问题单号                                                     | 问题描述              | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | --------------------- | -------- | -------- |
| [IBZIPH](https://gitee.com/opengauss/openGauss-server/issues/IBPF8F?from=project-issue) | D库视图列名都改为小写 | 次要     | 已完成   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | 通过执行创建多种数据类型查看sysobjects视图返回和信息是否合理 | YES                  |
| 2    | 功能测试   | 通过执行创建多种数据类型查看syscolumns视图返回和信息是否合理 | YES                  |
| 3    | 功能测试   | 创建各类主键组合嵌套测试查看sysindexkeys视图返回和信息是否合理 | YES                  |
| 4    | 功能测试   | 创建索引和各类主键组合嵌套测试查看sysindexes视图返回和信息是否合理 | YES                  |
| 5    | 兼容性测试 | 不同数据库兼容性下测试视图功能，执行结果符合预期             | YES                  |
| 6    | 升级验证   | 7.0.0RC1升级到7.0.0RC2创建D库和shark插件，测试函数基本功能   | YES                  |
| 7    | 资料验证   | 企业版及lite版验证资料描述及示例                             | YES                  |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称                            | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量(k) | 缺陷密度 |
| ----------------------------------- | ---------------- | ---------- | ---------- | ---------- | --------- | -------- |
| openGauss 7.0.0-RC1 build  c20928a5 | 8                | 38         | 38         | 1          | 1         | 1/kloc   |

### 6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 38           | 38               | 37     | 1      | 0       | 0           | 100%   | 100%       |

# 7 附件

## 7.1 附件1：遗留问题列表

| 编号 | 问题单号 | 问题描述 | 问题级别 | 当前状态无 |
| ---- | -------- | -------- | -------- | ---------- |
| 无   |          |          |          |            |

## 7.2 附件2：特性相关PR

1、源代码PR：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7289

2、文档pr：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/7498

3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=7b844809f34349848353668a238c894b&hideDevcloudHead=true

4、测试用例：

测试用例-》openGauss_7.0.0RC2-》兼容性-》D库常用语法兼容-常用视图