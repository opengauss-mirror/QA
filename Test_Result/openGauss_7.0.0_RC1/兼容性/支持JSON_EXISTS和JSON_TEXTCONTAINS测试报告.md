版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期          | 修订版本 | 修改描述 | 作者 |
| ------------- | -------- | -------- | ---- |
| 2025年2月26日 | V1.0     | 新建     | 宋菲 |

[TOC]

**Keywords 关键词**：json_exists、json_textcontains

**Abstract 摘要**：

​        需求内容为[支持JSON_EXISTS和JSON_TEXTCONTAINS](https://e.gitee.com/opengaussorg/projects/676554/requirements/table?issue=IAVDNY)，本次从功能、兼容性、可靠性、特性耦合分析、资料等方面进行测试，测试质量良好。

***


# 1 概述

​        本报告描述[支持JSON_EXISTS和JSON_TEXTCONTAINS](https://e.gitee.com/opengaussorg/projects/676554/requirements/table?issue=IAVDNY)特性测试活动，随openGauss 7.0.0-RC1 build f59ce09e（B002）版本发布，在 openGauss 7.0.0-RC1 build a44948f6（B010）版本基础上进行测试验证。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称              | 测试起始时间  | 测试结束时间  | 测试人员 |
| ---------------------------------- | ----------------------- | ------------- | ------------- | -------- |
| openGauss 7.0.0-RC1 build a44948f6 | openGauss7.0.0-RC1.B010 | 2025年1月06日 | 2025年1月8日  | 宋菲     |
| openGauss 7.0.0-RC1 build 29a1d791 | openGauss7.0.0-RC1.B013 | 2025年1月18日 | 2025年2月5日  | 宋菲     |
| openGauss 7.0.0-RC1 build 7e79432c | openGauss7.0.0-RC1.B016 | 2025年2月26日 | 2025年2月26日 | 宋菲     |
| openGauss 7.0.0-RC1 build 70252c08 | openGauss7.0.0-RC1.B019 | 2025年3月9日  | 2025年3月9日  | 宋菲     |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       | 无       | 无       |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号   | 硬件配置信息                                                 | 备注 |
| -------- | ---------- | ------------------------------------------------------------ | ---- |
| 测试环境 | Kunpeng920 | CPU：8核<br />内存：16G<br />硬盘：128G<br />OS：Kylin Linux Advanced Server V10 SP2<br />文件系统：XFS<br />网卡：千兆 | 无   |

### 2.2.2 虚拟化平台

| 虚拟化平台 | 版本说明      |
| ---------- | ------------- |
| KVM        | version 4.1.0 |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

​        本需求在测试阶段完成了功能测试、可靠性测试、可服务性测试、资料测试、特性耦合分析测试。需求100%实现，所有被测特性的测试用例的累计执行覆盖率达到100%，上述测试发现的缺陷均已得到修正，回归测试结果正常，无遗留问题。

## 3.2 约束说明

- JSON_EXISTS的语法中，FORMAT JSON、PASSING子句和JSON_exists_on_empty_clause子句暂未支持。
- 当前JsonPath语法中仅支持了通过键名取值和通过数组下标取值的语法。

## 3.3 关键风险和规避措施

| 风险类型 | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| -------- | -------- | -------- | -------------- | ------ | -------- |
| 无       | 无       | 无       | 无             | 无     | 无       |

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                   | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ---------------------------------------------- | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| [支持JSON_EXISTS和JSON_TEXTCONTAINS](https://e.gitee.com/opengaussorg/projects/676554/requirements/table?issue=IAVDNY) | 增强对JSON函数的兼容性，扩展了数据库适用场景。 | 无                         | 无                   | 良好             | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

​        100万行数据，查询速度达到秒级。

*注意：json_textcontains在设计时，未实现兼容A的特殊json索引，所以性能与A差异较大。但仍可达到秒级。*

| 函数名称          | 数据量  | 吞吐量 | 查询时间（OG） | 查询时间（异构A） |
| ----------------- | ------- | ------ | -------------- | ----------------- |
| json_exists       | 100万行 | 单用户 | 5.4s           | 6s                |
| json_textcontains | 100万行 | 单用户 | 5.6s           | 0s                |

###  4.2.2 可靠性测试结论

​        本需求不涉及。

###  4.2.3 安全&隐私保护测试结论

​       本需求不涉及。

### 4.2.4 可服务性测试结论

| 测试步骤                                 | 测试结果 |
| ---------------------------------------- | -------- |
| 验证异常情况时，可以给予告警或报错。     | 测试通过 |
| 告警或报错信息，语句清晰，内容有效易懂。 | 测试通过 |

### 4.2.5 生命周期管理测试结论

​       本需求不涉及。

### 4.2.6 韧性测试结论

​       本需求不涉及。

###  4.2.7 兼容性测试结论

​       本需求只在A模式下兼容。

###  4.2.8 升级测试结论

| 测试步骤                                   | 升级路径                                                     | 测试结果 |
| ------------------------------------------ | ------------------------------------------------------------ | -------- |
| 升级版本，新增本需功能，且测试用例执行通过 | 从openGauss7.0.0-RC1.B002版本升级到openGauss7.0.0-RC1.B003版本 | 测试通过 |
| 回退版本，不支持本需求功能，报函数不存在   | 从openGauss7.0.0-RC1.B003版本回退到openGauss7.0.0-RC1.B002版本 | 测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论               |
| ---- | ------------------------------------------------------------ | ---------------------- |
| 1    | [SQL参考>函数和操作符>JSON/JSONB函数和操作符](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/JSON-JSONB%E5%87%BD%E6%95%B0%E5%92%8C%E6%93%8D%E4%BD%9C%E7%AC%A6.html) | 测试通过，整体质量良好 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

全部为问题单，无资料单。

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 6        | 0    | 1     | 5     | 0      |
| 百分比 |          | 0    | 16.7% | 83.3% | 0      |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBIX5D](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX5D) | JsonTextcontains函数，指向不存在的json数据节点，挂库         | 主要     | 已验收   |
| [IBK62W](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBK62W) | JsonTextcontains函数，在PL中需要强转为cstring类型，不易用    | 次要     | 已验收   |
| [IBJ0Y5](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBJ0Y5) | JSON_TEXTCONTAINS函数，string为数字字符查询时，结果有误      | 次要     | 已验收   |
| [IBIXG2](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIXG2) | JsonExists函数中的json_path未兼容只有一层数据时使用数组下标0的情况 | 次要     | 已验收   |
| [IBIX99](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX99) | JsonTextcontains函数，报语法错误                             | 次要     | 已验收   |
| [IBIX8B](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX8B) | JsonExists函数，当expr为null，json_path为空格时，表现与A不兼容 | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                          | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ----------------------------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试（json_exists）       | 入参为非json格式数据，验证json_exists_on_error_clause子句    | YES                  |
| 2    |                               | 入参为json格式数据，验证json_exists_on_error_clause子句      | YES                  |
| 3    |                               | json_path入参支持键名和数组下标，键名或数组下标合法、非法    | YES                  |
| 4    |                               | 入参为空值空串                                               | YES                  |
| 5    |                               | 入参为其他数据类型                                           | YES                  |
| 6    |                               | 入参为表的列、常量、PL中的变量                               | YES                  |
| 7    |                               | 未支持FORMAT JSON、PASSING子句和JSON_exists_on_empty_clause子句，需提供报错 | YES                  |
| 8    |                               | select方式执行、PL中执行                                     | YES                  |
| 9    |                               | 与其他json函数联合使用                                       | YES                  |
| 10   | 功能测试（json_textcontains） | json_path入参支持键名和数组下标，键名或数组下标合法、非法    | YES                  |
| 11   |                               | 入参的string参数，以逗号、空格分隔，大小写不敏感、中文字     | YES                  |
| 12   |                               | 入参为空值空串                                               | YES                  |
| 13   |                               | 入参为其他数据类型                                           | YES                  |
| 14   |                               | 入参为表的列、常量、PL中的变量                               | YES                  |
| 15   |                               | select方式执行、PL中执行                                     | YES                  |
| 16   |                               | 与其他json函数联合使用                                       | YES                  |
| 17   | 安全性测试                    | 入参为没有查询权限的表的列，报错                             | YES                  |
| 18   | 性能测试                      | 单用户，100万行数据，查询性能与异构库A对比                   | YES                  |
| 19   | 兼容性测试                    | 只兼容A模式                                                  | YES                  |
| 20   | 资料测试                      | 资料验证                                                     | YES                  |
| 24   | 升级测试                      | 升级验证                                                     | YES                  |
| 25   | 特性耦合                      | 驱动JDBC验证，Yat工具执行自动化测试用例                      | YES                  |

##  6.2 测试设计评估



| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| 1    | 无             | 无       | 无               |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                                   | 工作量投入(人天) | 测试用例数 | 用例执行数                            | 发现缺陷数 | 代码行数（k） | 缺陷密度(个/KLOC) |
| ------------------------------------------ | ---------------- | ---------- | ------------------------------------- | ---------- | ------------- | ----------------- |
| openGauss 7.0.0-RC1 build 29a1d791（B013） | 5                | 38         | 38（36个自动化用例，2个人工执行用例） | 6          | 2.136         | 6/2136=2.809      |
| openGauss 7.0.0-RC1 build 7e79432c（B016） | 0.5              | 38         | 38（36个自动化用例，2个人工执行用例） |            |               |                   |
| openGauss 7.0.0-RC1 build 70252c08（B019） | 0.5              | 38         | 38（36个自动化用例，2个人工执行用例） |            |               |                   |



###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 36           | 36               | 30     | 6      | 0       | 0           | 100%   | 83.3%      |
| 38           | 38               | 38     | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

##  7.1 附件1：遗留问题列表

本需求无遗留缺陷。

##  7.2 附件2：特性相关PR

- 需求代码PR

  https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6534
  
- 问题修复相关PR

  | 问题单号                                                     | PR                                                           | 问题级别 |
  | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- |
  | [IBIX5D](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX5D) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7080 | 主要     |
  | [IBK62W](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBK62W) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7142 | 次要     |
  | [IBJ0Y5](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBJ0Y5) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7082<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7123<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7142 | 次要     |
  | [IBIXG2](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIXG2) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7123 | 次要     |
  | [IBIX99](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX99) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7078 | 次要     |
  | [IBIX8B](https://e.gitee.com/opengaussorg/projects/676554/bugs/table?issue=IBIX8B) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7079 | 次要     |
  
- 资料PR

  https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6929

- 测试用例目录

  测试用例/DBV伙伴\_TC/神通_TC/IAVDNY-支持JSON_EXISTS表达式、支持JSON_TEXTCONTAINS表达式

- 测试设计

  [测试设计/基线/openGauss_7.0.0RC1/#IAVDNY【神舟通用】-支持JSON_EXISTS表达式、支持JSON_TEXTCONTAINS表达式](https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=fe84d913f80f4b5d99ec76c2da6148db&hideDevcloudHead=true)

  

