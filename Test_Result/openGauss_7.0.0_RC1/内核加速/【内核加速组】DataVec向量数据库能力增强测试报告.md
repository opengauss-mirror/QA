![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期        | 修订版本 | 修改描述     | 作者 |
|-----------| -------- | ------------ |--|
| 2025.3.3 | v1.0     | 测试报告初稿 | peilinqian |

**Keywords 关键词**：DATAVEC向量数据类型 向量操作符 向量函数 向量索引 HNSW索引 IVFFALT索引

**Abstract 摘要**：本文档主要介绍 openGauss 向量数据库相关功能的验证及测试结果；

**缩略语清单：**

| 缩略语  | 英文全名                           | 中文解释               |
| ------- | ---------------------------------- | ---------------------- |
| IVFFLAT | Inverted File with Flat storage    | 基于倒排的向量索引算法 |
| HNSWPQ  | Hierarchical Navigable Small World | 基于图的向量索引算法   |
| ANN     | Approximate Nearest Neighbors      | 近似最临近             |


# 1 概述

本报告主要测试 openGauss 数据库3种向量数据类型（vector、sparsevec、bit）、向量函数、向量距离操作符、IVFFLAT及HNSW向量索引、向量索引检索等功能，包括功能测试、可靠性测试、升级测试、资料测试。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员   |
| ---------------------------------- | ---------- | ------------ | ------------ | ---------- |
| openGauss 7.0.0-RC1 build f03109c2 | openGauss  | 2024/11/22   | 2025/11/27   | peilinqian |
| openGauss 7.0.0-RC1 build 96ee5da3 | openGauss  | 2024/12/1    | 2025/12/25   | peilinqian |
| openGauss 7.0.0-RC1 build a44948f6 | openGauss  | 2024/12/26   | 2025/1/24    | peilinqian |
| openGauss 7.0.0-RC1 build d98993bb | openGauss  | 2025/2/6     | 2025/2/27    | peilinqian |
| openGauss 7.0.0-RC1 build 6309f4cb | openGauss  | 2025/3/5     | 2025/3/13    | peilinqian |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

**功能测试环境信息**

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |

## 3.1 测试结论总结

本报告主要测试向量数据类型、向量函数、向量距离操作符、向量IVFFLAT及HNSW的两种索引、向量检索等功能，覆盖所有索引选项参数及查询检索参数，测试详情如下：

- 功能测试：输出测试用例166个，累计执行测试6轮，发现issue14个，待办4个；
- 可靠性测试：输出用例51个，执行1轮，功能正常；
- 升级测试：测试升级路径2个，功能正常；
- 兼容性测试：输出测试用例3个，发现issue2个，已全部解决；
- 资料测试：覆盖企业版、轻量版，发现issue9个，已全部解决。

需求覆盖100%，用例执行100%，总计发现issue共25个，4个问题待解决；整体质量一般。

## 3.2 约束说明

数据类型、索引构建约束细节参考：

- [向量数据类型](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量数据类型.html)
- [向量函数和操作符](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量函数和操作符.html)
- [向量索引](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量索引.html)

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                  | 特性价值评估                                                 | 应用说明及关键约束假设依赖                                   | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                    | 主要风险                                      |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- | ---------------- | ----------------------------------------------- | --------------------------------------------- |
| openGauss支持向量存储 | 新增向量数据类型Vector、sparsevec，数据类型建表存储数据功能正常；类型转换功能正常。 | 见[向量数据类型](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量数据类型.html) | 无                   | 100%             | <font color=green>▮</font>                      | 无                                            |
| openGauss支持向量计算 | 向量简单运算、向量距离计算操作符、向量相关计算函数功能正常；不同数据类型支持对应距离函数功能正常。 | 见[向量函数和操作符](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量函数和操作符.html) | 无                   | 100%             | <font color=green>▮</font>                      | 无                                            |
| openGauss支持向量检索 | 向量支持IVFFLAT索引、HNSW图索引、暴力扫描，索引创建、修改、重建功能正常，根据不同索引选项，召回率趋势正常。存在索引场景下，ustore、极致RTO场景功能存在问题。create Index，reindex并行构建场景下概率出现core。 | 见[向量索引](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/向量索引.html) | 无                   | 100%             | <font color=red><font color=red>●</font></font> | ustore、极致RTO场景存在问题；构建索引概率core |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 功能测试结论

| 测试场景                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 验证向量类型与其他数据类型转换功能是否正常、验证数据存储空间大小是否正确、验证向量数据类型数据增删改查功能正常； | 通过     |
| 验证基础运算符、距离计算操作符功能是否正常，数据类型聚合函数、距离计算函数功能正常； | 通过     |
| 创建HNSW、IVF索引选项参数、不同索引距离覆盖验证，功能正常<br />验证不同索引选项设置不同值后存储空间、执行时长、查询结果召回率准确度趋势验证，功能正常<br />并行构建索引功能验证。hnsw索引执行概率出现core，测试不通过<br />不同SQL执行场景下可靠性场景验证，功能正常； | 部分通过 |
| 极致RTO场景下，向量数据类型存在向量索引场景下，增删改查功能正常。主备数据一致，主备使用索引查询功能存在问题，备机使用索引查询，备机core；测试不通过 | 不通过   |
| ustore场景下，向量数据类型存在向量索引场景下，增删改查功能异常。主备数据一致性功能，主备使用索引查询功能验证阻塞；测试不通过 | 不通过   |
| 验证索引检索GUC参数功能验证、设置不同值对查询结果召回率准确性影响趋势正常；<br />索引检索场景下可靠性场景验证正常； | 通过     |

###  4.2.2 性能测试结论

不涉及

###  4.2.3 可靠性测试结论

| 测试场景                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 构建索引、重建索引过程主机注入kill-9、kill-19、kill-2、重启switchover等故障，验证合理报错，恢复数据库再次执行成功，主备索引查询结果一致； | 通过     |
| 构建索引、重建索引过程备机注入kill-9、kill-19、重启switchover等故障，验证主机SQL执行正常，恢复数据库后，主备索引查询结果一致； | 通过     |
| 存在索引场景下循环进行增删改数据过程注入kill-9、kill-19、kill-2、数据库重启，验证合理报错，恢复数据库后再次执行功能正常，主备索引查询结果一致； | 通过     |
| 存在索引场景下使用索引查询过程同时进行reindex、analyze、vacuum，并行增删改操作，数据库功能正常 | 通过     |

###  4.2.4 安全&隐私保护测试结论

不涉及

### 4.2.5 可服务性测试结论

不涉及

### 4.2.6 生命周期管理测试结论

不涉及

### 4.2.7 韧性测试结论

不涉及

###  4.2.8 兼容性测试结论

| 测试场景                                                | 测试结果 |
| ------------------------------------------------------- | -------- |
| 验证不同向量类型及相关SQL功能在兼容B、C、PG库下功能正常 | 通过     |

###  4.2.9 升级测试结论

| 测试场景                                                     | 升级路径                                          | 测试结果 |
| ------------------------------------------------------------ | ------------------------------------------------- | -------- |
| 不带datavec功能版本灰度升级至带datavec功能版本后提交及回滚功能正常 | openGauss 6.0.0-->openGuass 7.0.0-RC1 B018        | 通过     |
| 带datavec功能版本（已有datavec测试数据）灰度升级至带datavec功能版本后进行提交及回滚功能正常 | openGauss 7.0.0-RC1 B017>openGuass 7.0.0-RC1 B020 | 通过     |

## 4.3 资料测试结论

| 序号 | 测试章节                                                   | 测试结论 |
| ---- | ---------------------------------------------------------- | -------- |
| 1    | 文档地图>关于openGauss>特性描述>企业级特性>DataVec向量引擎 | 通过     |
| 2    | 文档地图>数据库参考>GUC参数说明>DataVec向量引擎参数        | 通过     |
| 3    | 文档地图>SQL参考>数据类型>向量数据类型                     | 通过     |
| 4    | 文档地图>SQL参考>DataVec向量引擎>向量索引                  | 通过     |

# 5 测试对象质量评估

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 24       | 3    | 5    | 16   | 0      |
| 百分比 | 100%     | 13%  | 21%  | 67%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBLCW0](https://gitee.com/opengaussorg/dashboard?issue_id=IBLCW0) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnsw索引，查询结果存在一条数据重复返回的情况。且limit数据未按距离操作符计算排序 | 主要     | 已验收   |
| [IBKLCW](https://gitee.com/opengaussorg/dashboard?issue_id=IBKLCW) | 【测试类型：资料】【测试版本：7.0.0】 向量索引资料存在说明不详细的问题 | 次要     | 已验收   |
| [IBKEYD](https://gitee.com/opengaussorg/dashboard?issue_id=IBKEYD) | 【测试类型：资料】【测试版本：7.0.0】 hnsw向量索引，vector、sparsevec字段内积距离索引类型场景，使用索引查询数量不正确；hnsw索引在向量维度较高时，使用索引查询数据缺失 | 主要     | 已验收   |
| [IBJNQP](https://gitee.com/opengaussorg/dashboard?issue_id=IBJNQP) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnsw向量索引查询，如果数据存在重复，查询结果数量不正确 | 主要     | 已验收   |
| [IBIIZH](https://gitee.com/opengaussorg/dashboard?issue_id=IBIIZH) | 【测试类型：SQL功能】【测试版本：7.0.0】 兼容B库向量内积运算<#>存在问题 | 次要     | 已验收   |
| [IBII4V](https://gitee.com/opengaussorg/dashboard?issue_id=IBII4V) | 【测试类型：资料】【测试版本：7.0.0】 向量索引查询sql语句限制及索引创建等资料约束部分缺失，需要优化 | 次要     | 已验收   |
| [IBFZLK](https://gitee.com/opengaussorg/dashboard?issue_id=IBFZLK) | 【测试类型：SQL功能】【测试版本：7.0.0】vector类型不支持转换成text[]等字符数组，需要进行支持 | 次要     | 已验收   |
| [IBF804](https://gitee.com/opengaussorg/dashboard?issue_id=IBF804) | 【测试类型：SQL功能】【测试版本：7.0.0】 主机执行对含vector字段的表进行truncate，备机core | 严重     | 已验收   |
| [IBEQBF](https://gitee.com/opengaussorg/dashboard?issue_id=IBEQBF) | 【测试类型：存储功能】【测试版本：7.0.0】 字符串转vector部分场景存储占用空间不正确 | 次要     | 已验收   |
| [IBEBEY](https://gitee.com/opengaussorg/dashboard?issue_id=IBEBEY) | 【测试类型：SQL功能】【测试版本：7.0.0】 sparsevec数据类型非零元素临界值达到16000的时候原代码会默认以非0元素结尾，0元素结尾报错 | 次要     | 已验收   |
| [IBE355](https://gitee.com/opengaussorg/dashboard?issue_id=IBE355) | 【测试类型：资料】【测试版本：7.0.0】 datavec向量相关功能轻量化也支持，但是没有相关资料；PQ相关资料需要更新 | 次要     | 已验收   |
| [IBCQE1](https://gitee.com/opengaussorg/dashboard?issue_id=IBCQE1) | 【测试类型：资料】【测试版本：7.0.0】 vector转numeric[]、dec[]不带精度场景之前不支持，资料存在约束，但是现在版本已经功能性支持，需要删除之前的资料约束；向量数据类型需要增加维度相关资料 | 次要     | 已验收   |
| [IBC9XG](https://gitee.com/opengaussorg/dashboard?issue_id=IBC9XG) | 【测试类型：SQL功能】【测试版本：7.0.0】 sparsevec类型数据，非零最大维度数不超过16000，类型转换失败 | 次要     | 已验收   |
| [IBBFXE](https://gitee.com/opengaussorg/dashboard?issue_id=IBBFXE) | 【测试类型：资料】【测试版本：7.0.0】 datavec 融合查询使用指南、vector类型函数等相关资料说明存在问题 | 次要     | 已验收   |
| [IB8ABE](https://gitee.com/opengaussorg/dashboard?issue_id=IB8ABE) | 【测试类型：资料】【测试版本：7.0.0】 向量操作符比较符</<=/>/>=无实际比较意义，需删除相关资料;官网资料SQL参考页面，向量相关链接存在缺失 | 次要     | 已验收   |
| [IB786O](https://gitee.com/opengaussorg/dashboard?issue_id=IB786O) | 【测试类型：资料】【测试版本：7.0.0】 datavec资料部分说明存在问题 | 次要     | 已验收   |
| [IB5HAA](https://gitee.com/opengaussorg/dashboard?issue_id=IB5HAA) | 【测试类型：SQL功能】【测试版本：7.0.0】 subvector(vector, integer, integer) 函数的参数2,3代表起始位置及截取的维度数，应该存在范围限制，实际未限制 | 次要     | 已验收   |
| [IB5H3T](https://gitee.com/opengaussorg/dashboard?issue_id=IB5H3T) | 【测试类型：SQL功能】【测试版本：7.0.0】 datavec相关的数据量类型vector转成array仅支持转成real[],应该支持其他数值数组类型转换 | 次要     | 已验收   |
| [IB4GT6](https://gitee.com/opengaussorg/dashboard?issue_id=IB4GT6) | 【测试类型：SQL功能】【测试版本：7.0.0】 兼容B库不支持指定维度datavec类型的表创建，提示信息需要优化 | 次要     | 已验收   |
| [IB3CJ5](https://gitee.com/opengaussorg/dashboard?issue_id=IB3CJ5) | 【测试类型：资料】【测试版本：7.0.0】 datavec向量相关资料描述非最新现状，且相关数据类型、操作符、距离函数、索引使用等需单独在指定模块进行页面新增及详细说明，可指导用户操作 | 主要     | 已验收   |
| [IBNP8Q](https://gitee.com/opengaussorg/dashboard?issue_id=IBNP8Q) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnsw向量索引查询极低概率会出现“ERROR: could not open file "base/15018/59217.1" (target block 327686): No such file or directory” | 次要     | 待办的   |
| [IBSELO](https://e.gitee.com/opengaussorg/issues/table?issue=IBSELO) | 【测试类型：故障注入】【测试版本：7.0.0】【概率问题】 hnsw索引在进行reindex时，备机stop，主机产生core | 严重     | 待办的   |
| [IBSLKT](https://e.gitee.com/opengaussorg/issues/table?issue=IBSLKT) | 【测试类型：SQL功能】【测试版本：7.0.0】 极致RTO备机读场景下，向量索引（ivfflat、hnsw、ivfpq、hnswpq）创建后，主机进行增删改后，备机进行查询core 缺陷统计在【内核加速组】DataVec向量数据库能力增强测试报告.md严重待办的 | 严重     | 待办的   |
| [IBSORG](https://e.gitee.com/opengaussorg/issues/table?issue=IBSORG) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnswpq、hnsw索引存在的Ustore表进行增删改，会异常抛错‘DETAIL: Could not open file "pg_csnlog/000058064800": ’ | 主要     | 待办的   |
| [IBSTZL](https://e.gitee.com/opengaussorg/issues/table?issue=IBSTZL) | opengauss长稳测试出现内存泄漏问题<br /> 说明：PQ长稳发现问题。缺陷统计在【内核加速组】向量数据库支持IVF-PQ索引算法，融合PQ距离计算加速算法测试报告 | 主要     | 待办的   |



# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性         | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ------------ | ------------------------------------------------------------ | -------------------- |
| 1    | 支持向量存储 | 验证向量类型与其他数据类型转换功能是否正常、验证数据存储空间大小是否正确、验证向量数据类型数据增删改查功能正常； | YES                  |
| 2    | 向量计算     | 验证基础运算符、距离计算操作符功能是否正常，数据类型聚合函数、距离计算函数功能是否正常； | YES                  |
| 3    | 向量检索     | 创建HNSW、IVF索引选项参数、不同索引距离覆盖验证；验证不同索引选项设置不同值后存储空间、执行时长、查询结果召回率准确度趋势验证，功能正常；不同SQL执行场景下可靠性场景验证正常； 验证索引检索GUC参数功能验证、设置不同值对查询结果召回率准确性影响趋势正常；索引检索场景下可靠性场景验证正常； | YES                  |
| 4    | 升级         | 验证6.0.0升级至需求合入的版本、回滚、提交功能正常；验证需求合入低版本升级至高版本存在向量数据场景，提交回滚功能正常； | YES                  |
| 5    | 资料         | 验证涉及资料正确，可正常指导用户进行相关功能使用、使用过程参数选项配置等 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss 7.0.0-RC1 B004 | 5                | 15         | 15         | 4          | 16.88          | 0.237    |
| openGauss 7.0.0-RC1 B008 | 15               | 40         | 40         | 5          | 16.88          | 0.296    |
| openGauss 7.0.0-RC1 B010 | 10               | 55         | 55         | 5          | 16.88          | 0.296    |
| openGauss 7.0.0-RC1 B014 | 15               | 156        | 156        | 6          | 16.88          | 0.355    |
| openGauss 7.0.0-RC1 B018 | 7                | 51         | 51         | 4          | 16.88          | 0.237    |

总体缺陷密度：24/16.88kloc = 1.42

###  6.3.2 测试用例执行结果统计数据

| 执行轮次 | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------- | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮   | 15             | 15               | 12     | 3      | 0       | 0           | 100%   | 80%        |
| 第二轮   | 40             | 40               | 39     | 1      | 0       | 0           | 100%   | 98%        |
| 第三轮   | 55             | 55               | 52     | 3      | 0       | 0           | 100%   | 95%        |
| 第四轮   | 156            | 156              | 139    | 17     | 0       | 0           | 100%   | 89%        |
| 第五轮   | 72             | 70               | 67     | 3      | 12      | 0           | 100%   | 96%        |

| 异常用例情况  | 影响分析                                                     | 规避措施 | 后续计划 |
| ------------- | ------------------------------------------------------------ | -------- | -------- |
| 失败用例3个   | 并行构建索引概率core，ustore、极致RTO备机读场景异常          |          |          |
| block用例12个 | 并行构建索引、极致RTO、ustore场景存在bug，导致相关场景无法执行暂无待解决 |          |          |

# 7 附件

##  7.1 附件1：遗留问题列表

| 序号 | 问题单号                                                     | 问题描述                                                     | 分类   | 问题级别 | 问题分析与影响          | 规避措施 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ | -------- | ----------------------- | -------- |
| 1    | [IBSELO](https://e.gitee.com/opengaussorg/issues/table?issue=IBSELO) | 【测试类型：故障注入】【测试版本：7.0.0】【概率问题】 hnsw索引在进行reindex时，备机stop，主机产生core | 可靠性 | 主要     | 并行构建索引概率core    |          |
| 2    | [IBSORG](https://e.gitee.com/opengaussorg/issues/table?issue=IBSORG) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnswpq、hnsw索引存在的Ustore表进行增删改，会异常抛错‘DETAIL:  Could not open file "pg_csnlog/000058064800": ’ | 功能   | 主要     | ustore表相关dml无法执行 | 待优化   |
| 3    | [IBSLKT](https://e.gitee.com/opengaussorg/issues/table?issue=IBSLKT) | 【测试类型：SQL功能】【测试版本：7.0.0】 极致RTO备机读场景下，向量索引（ivfflat、hnsw、ivfpq、hnswpq）创建后，主机进行增删改后，备机进行查询core | 功能   | 严重     | 极致RTO场景备机会core   | 待优化   |
| 4    | [IBSTZL](https://e.gitee.com/opengaussorg/issues/table?issue=IBSTZL) | opengauss长稳测试出现内存泄漏问题<br/>（PQ长稳发现问题。缺陷统计在【内核加速组】向量数据库支持IVF-PQ索引算法，融合PQ距离计算加速算法测试报告） | 长稳   | 主要     | 内存占用无法释放        | 待优化   |



##  7.2 附件2：特性相关PR

代码PR：

https://gitee.com/opengauss/openGauss-server/pulls/6616

资料PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6952

测试设计归档：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=cfebf800dba84ca794bc05d9967712c6&hideDevcloudHead=true

测试用例归档：

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg

