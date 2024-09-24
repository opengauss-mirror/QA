

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期          | 修订版本 | 修改描述                                                     | 作者 |
| ------------- | -------- | ------------------------------------------------------------ | ---- |
| 2024年8月12日 | V1.0     | 新建                                                         | 宋菲 |
| 2024年9月5日  | V2.0     | 验收问题，修改测试报告结果                                   | 宋菲 |
| 2024年9月9日  | V3.0     | 根据评审意见修改文档，增加gs_restore导入测试验证；重新计算缺陷密度；删除反体提示信息 | 宋菲 |
| 2024年9月11日 | V4.0     | 回归剩余一个缺陷，更新缺陷状态                               | 宋菲 |

[TOC]

**Keywords 关键词**：游标、rowtype

**Abstract 摘要**：

​        需求内容为[支持用ROWTYPE给游标赋值](https://e.gitee.com/opengaussorg/dashboard?issue=I8WWP9)，本次从功能、兼容性、可靠性、特性耦合分析、资料等方面进行测试，测试质量良好。

***


# 1 概述

​        本报告描述[支持用ROWTYPE给游标赋值](https://e.gitee.com/opengaussorg/dashboard?issue=I8WWP9)特性测试活动，本需求功能随 openGauss 6.0.0 build 982a7bec（B017）版本发布，并在此版本基础上进行测试验证。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                       | 软件包名称          | 测试起始时间  | 测试结束时间  | 测试人员 |
| ------------------------------ | ------------------- | ------------- | ------------- | -------- |
| openGauss 6.0.0 build 982a7bec | openGauss6.0.0.B017 | 2024年5月27日 | 2024年6月11日 | 宋菲     |
| openGauss 6.0.0 build 0d75f549 | openGauss6.0.0.B031 | 2024年9月11日 | 2024年9月11日 | 宋菲     |

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

- %ROWTYPE不支持引用复合类型或RECORD类型变量的类型、跨PACKAGE cursor的类型。

- PL对象受预编译参数控制，例如 **set** behavior_compat_options='allow_procedure_compile_check';

  - 开启预编译开关时可以检测游标所涉及的表是否存在、表中是否存在对应列，能够在创建函数、存储过程、包时抛出错误。
  - 关闭预编译开关时无论游标所涉及的表、或者表中的列是否存在，都可以成功创建函数、存储过程、包。

- 目前未考虑在PL执行过程中修改表的情况，将在后续提出其他需求进一步完善。

- 嵌套游标不支持使用%ROWTYPE。

  

## 3.3 关键风险和规避措施

| 风险类型 | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| -------- | -------- | -------- | -------------- | ------ | -------- |
| 无       | 无       | 无       | 无             | 无     | 无       |

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                         | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| [支持用ROWTYPE给游标赋值](https://e.gitee.com/opengaussorg/dashboard?issue=I8WWP9) | 游标的ROWTYPE可以读取整行数据，当查询为整行数据的时候效率比每一个字段对应快，方便用户查询。 | 无                         | 无                   | 良好             | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

本需求不涉及。

###  4.2.2 可靠性测试结论

| 测试步骤                                               | 测试结果                             |
| ------------------------------------------------------ | ------------------------------------ |
| 单session，执行257个不同的PL对象，内存是否泄露         | 测试通过，内存无泄漏，数据库状态健康 |
| 并发1000个sessoin，执行257个不同的PL对象，内存是否泄露 | 测试通过，内存无泄漏，数据库状态健康 |

###  4.2.3 安全&隐私保护测试结论

本需求不涉及。

### 4.2.4 可服务性测试结论

| 测试步骤                                 | 测试结果 |
| ---------------------------------------- | -------- |
| 验证异常情况时，可以给予告警或报错。     | 测试通过 |
| 告警或报错信息，语句清晰，内容有效易懂。 | 测试通过 |

### 4.2.5 生命周期管理测试结论

本需求不涉及。

### 4.2.6 韧性测试结论

本需求不涉及。

###  4.2.7 兼容性测试结论

| 测试步骤                                     | 测试结果 |
| -------------------------------------------- | -------- |
| 用例在兼容A库模式下执行，验证功能是否正确。  | 测试通过 |
| 用例在兼容B库模式下执行，验证功能是否正确。  | 测试通过 |
| 用例在兼容C库模式下执行，验证功能是否正确。  | 测试通过 |
| 用例在兼容PG库模式下执行，验证功能是否正确。 | 测试通过 |

###  4.2.8 升级测试结论

| 测试步骤                                                     | 升级路径                                               | 测试结果 |
| ------------------------------------------------------------ | ------------------------------------------------------ | -------- |
| 升级版本，新增本需功能，且测试用例执行通过                   | 从openGauss6.0.0.B016版本升级到openGauss6.0.0.B017版本 | 测试通过 |
| 回退版本，不支持本需求功能，即不支持游标参数设置默认值，报语法错误 | 从openGauss6.0.0.B017版本回退到openGauss6.0.0.B016版本 | 测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论               |
| ---- | ------------------------------------------------------------ | ---------------------- |
| 1    | [SQL参考>存储过程>基本语句>定义变量](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/SQLReference/定义变量.html) | 测试通过，整体质量良好 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

全部为问题单，无资料单。

|        | 问题总数 | 严重  | 主要 | 次要  | 不重要 |
| ------ | -------- | ----- | ---- | ----- | ------ |
| 数目   | 8        | 3     | 0    | 5     | 0      |
| 百分比 |          | 37.5% | 0    | 62.5% | 0      |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IAD5JX]([测试类型：SQL功能][支持用ROWTYPE给游标赋值]给游标rowtype赋空值，挂库<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5JX) | 给游标rowtype类型整体赋NULL值，挂库                          | 严重     | 已验收   |
| [IAD5LG](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】只有一列时，rowtype的值插入表，表现错误<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5LG) | 表只有一列时，游标rowtype的值插入表，表现错误，多输出出一对英文括号() | 次要     | 已验收   |
| [IAD5M1](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】给游标rowtype变量赋默认值时，未检查varchar类型长度约束<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5M1) | 给游标rowtype变量赋默认值时，varchar类型超出定义时长度也可以赋值成功，未检查该类型长度约束 | 次要     | 已验收   |
| [IAD5MG](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】raise notice打印不存在的列，未报错<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5MG) | raise notice打印游标依赖的表的不存在的列，未报错             | 次要     | 已验收   |
| [IAIXA7](【测试类型：SQL功能】【测试版本：6.0.0】 【支持用ROWTYPE给游标赋值】嵌套游标的rowtype，输出值有误<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAIXA7) | 嵌套游标使用rowtype，输出值有误，期望报错                    | 次要     | 已验收   |
| [IAFBUR](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】创建使用游标rowtype的触发器函数，挂库<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAFBUR) | 创建使用游标rowtype的触发器函数，挂库                        | 严重     | 已验收   |
| [IAINT6](【测试类型：SQL功能】【测试版本：6.0.0】【支持用ROWTYPE给游标赋值】包中proc调用其他包中参数为cursor%rowtype的proc，挂库，产生core<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAINT6) | 包中proc1调用本包中参数为cursor%rowtype的proc2，挂库         | 严重     | 已验收   |
| [IANJZD](【测试类型：SQL功能】【测试版本：6.0.0】 【支持用ROWTYPE给游标赋值】游标rowtype类型为numeric，赋值时未检查精度<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IANJZD) | 游标rowtype类型为numeric，赋值时未检查精度                   | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | 定义游标rowtype时，游标SQL为单表查询                         | YES                  |
| 2    |            | 定义游标rowtype时，游标SQL为多表查询                         | YES                  |
| 3    |            | 定义游标rowtype时，游标SQL查询所有基础类型                   | YES                  |
| 4    |            | 定义游标rowtype时，定义默认值，默认值类型为所有基础类型      | YES                  |
| 5    |            | 重复定义游标rowtype                                          | YES                  |
| 6    |            | 包中，分别定义相同名称的公有游标rowtype和私有游标rowtype     | YES                  |
| 7    |            | 开启预编译参数，创建PL对象，对象中使用游标rowtype，PL对象包括函数、存储过程、触发器、包； | YES                  |
| 8    |            | 关闭预编译参数，创建PL对象，对象中使用游标rowtype，PL对象包括函数、存储过程、触发器、包；触发器兼容B库写法、兼容PG库事件触发器 | YES                  |
| 9    |            | 使用游标rowtype默认值，raise notice打印、向表插入数据        | YES                  |
| 10   |            | fetch游标1的值到游标2的rowtype类型变量中                     | YES                  |
| 11   |            | 与record类型变量相互赋值                                     | YES                  |
| 12   |            | 执行过程中，依赖表或者视图的结构变化，drop、alter表字段个数、表类型 | YES                  |
| 13   |            | 执行过程中，依赖表或者视图的数据变化，DML                    | YES                  |
| 14   |            | 游标rowtype变量作为（包中的）函数的入参、出参、返回值        | YES                  |
| 15   |            | 包中嵌套调用使用游标rowtype变量作为入参的存储过程            | YES                  |
| 16   |            | 游标rowtype变量赋空值                                        | YES                  |
| 17   |            | 并发更新，表更新数据时，使用游标rowtype变量查询数据          | YES                  |
| 18   |            | 与执行游标语句的相互影响，open游标、fetch游标、给游标rowtype变量赋值、引用游标rowtype变量，乱序执行 | YES                  |
| 19   |            | 嵌套游标的rowtype的使用                                      | YES                  |
| 20   | 兼容性测试 | 兼容A、B、C、PG，B模式中不包含触发器函数，PG模式中含事件触发器 | YES                  |
| 21   | 可靠性测试 | 单session，执行257个不同的PL对象，内存是否泄露               | YES                  |
| 22   |            | 并发1000个sessoin，执行257个不同的PL对象，内存是否泄露       | YES                  |
| 23   | 资料测试   | 资料验证                                                     | YES                  |
| 24   | 升级测试   | 升级验证                                                     | YES                  |
| 25   | 特性耦合   | 驱动JDBC验证，Yat工具执行自动化测试用例                      | YES                  |
| 26   |            | gs_dump导出、gs_restore导入、gsql导入                        | YES                  |
| 27   |            | 主备场景                                                     | YES                  |

##  6.2 测试设计评估



| 编号 | 测试点修改说明          | 修改原因                                       | 是否影响测试质量 |
| ---- | ----------------------- | ---------------------------------------------- | ---------------- |
| 1    | 增加嵌套游标使用rowtype | 通过其他评审会发现游标可以嵌套使用，故增加用例 | 是               |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                               | 工作量投入(人天) | 测试用例数 | 用例执行数                            | 发现缺陷数 | 代码行数 | 缺陷密度(个/KLOC) |
| -------------------------------------- | ---------------- | ---------- | ------------------------------------- | ---------- | -------- | ----------------- |
| openGauss 6.0.0 build 982a7bec（B017） | 18               | 48         | 48（46个自动化用例，2个人工执行用例） | 8          | 2955     | 8/2955=0.003      |
| openGauss 6.0.0 build 0d75f549（B031） | 0.5              | 49         | 49（47个自动化用例，2个人工执行用例） |            |          |                   |

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 48           | 48               | 40     | 8      | 0       | 0           | 100%   | 83.3%      |
| 49           | 49               | 49     | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

##  7.1 附件1：遗留问题列表

本需求无遗留缺陷。

##  7.2 附件2：特性相关PR

- 需求代码PR

  【神舟通用】支持用ROWTYPE给游标赋值https://e.gitee.com/opengaussorg/projects/633438/requirements/table?issue=I8WWP9

  https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4897

- 问题修复相关PR

  | 问题单号                                                     | PR                                                           | 问题级别 |
  | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- |
  | [IAD5JX]([测试类型：SQL功能][支持用ROWTYPE给游标赋值]给游标rowtype赋空值，挂库<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5JX) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5788 | 严重     |
  | [IAD5LG](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】只有一列时，rowtype的值插入表，表现错误<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5LG) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5789<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6016<br>https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6581 | 次要     |
  | [IAD5M1](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】给游标rowtype变量赋默认值时，未检查varchar类型长度约束<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5M1) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5790<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6008 | 次要     |
  | [IAD5MG](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】raise notice打印不存在的列，未报错<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAD5MG) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5787<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6009 | 次要     |
  | [IAIXA7](【测试类型：SQL功能】【测试版本：6.0.0】 【支持用ROWTYPE给游标赋值】嵌套游标的rowtype，输出值有误<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAIXA7) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6003<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6004<br>https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6576 | 次要     |
  | [IAFBUR](【测试类型：SQL功能】【支持用ROWTYPE给游标赋值】创建使用游标rowtype的触发器函数，挂库<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAFBUR) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5883<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6010 | 严重     |
  | [IAINT6](【测试类型：SQL功能】【测试版本：6.0.0】【支持用ROWTYPE给游标赋值】包中proc调用其他包中参数为cursor%rowtype的proc，挂库，产生core<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IAINT6) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5976<br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6006 | 严重     |
  | [IANJZD](【测试类型：SQL功能】【测试版本：6.0.0】 【支持用ROWTYPE给游标赋值】游标rowtype类型为numeric，赋值时未检查精度<br/>https://e.gitee.com/opengaussorg/projects/633438/bugs/table?issue=IANJZD) | https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6166 | 次要     |

- 资料PR

  https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6392

- 测试设计

  https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=5dd9e33ad5b44d3a9573683610c9396b&hideDevcloudHead=true

  



