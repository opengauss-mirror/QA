![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期        | 修订版本 | 修改描述     | 作者 |
|-----------| -------- | ------------ |--|
| 2025.2.28 | v1.0     | 测试报告初稿 | peilinqian |
| 2025.3.13 | V2.0 | 优化更新报告 | peilinqian |

**Keywords 关键词**：DATAVEC PQ索引 HNSWPQ索引 IVFPQ索引

**Abstract 摘要**：本文档主要介绍 openGauss 向量数据库PQ索引相关功能的验证及测试结果；

**缩略语清单：**

| 缩略语 | 英文全名                                                     | 中文解释                     |
| ------ | ------------------------------------------------------------ | ---------------------------- |
| IVFPQ  | Inverted File with Product Quantization                      | 基于倒排与量化的向量索引算法 |
| HNSWPQ | Hierarchical Navigable Small World combined with Product Quantization | 基于图的量化向量索引算法     |


# 1 概述

本报告主要测试 openGauss 数据库 DataVec 向量引擎IVFPQ及HNSWPQ的两种索引创建、修改、重建、检索等功能，包括功能测试、性能测试、可靠性测试、升级测试、资料测试。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员   |
| ---------------------------------- | ---------- | ------------ | ------------ | ---------- |
| openGauss 7.0.0-RC1 build a44948f6 | openGauss  | 2025-12-23   | 2025-12-25   | peilinqian |
| openGauss 7.0.0-RC1 build 7e79432c | openGauss  | 2025-2-20    | 2025-2-25    | peilinqian |
| openGauss 7.0.0-RC1 build 6309f4cb | openGauss  | 2024-2-26    | 2025-2-28    | peilinqian |
| openGauss 7.0.0-RC1 build d47a8b78 | openGauss  | 2025-3-8     | 2025-3-8     | peilinqian |
| Boostkit 24.0.T15.B011             | Boostkit   | 2025-2-20    | 2025-2-25    | peilinqian |
| BoostKit 25.0.RC1                  | Boostkit   | 2025-2-20    | 2025-3-8     | peilinqian |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

**功能测试环境信息**

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 20.03(LTS) |      |

**性能测试环境信息**

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng 920 7280Z操作系统；openeuler 22.03LTS SP3，GCC 10.3，python3.11 | 整机 |

## 3.1 测试结论总结

本报告主要测试DataVec向量数据库HNSWPQ、IVFPQ索引创建、修改、重建及检索模块，覆盖所有索引选项参数及查询检索参数，测试详情如下：

- 功能测试：输出测试用例166个，累计执行测试4轮，发现issue7个（其中2个issue为非PQ需求测试发现，PQ场景同样存在问题），待解决issue3个；
- 可靠性测试：输出用例51个，执行1轮，发现issue1个，待解决issue1个；
- 性能测试：执行Boostkit POC版本、商用版本共计2轮，HNSWPQ与IVFPQ性能提升达到30%以上，达到验收标准；
- 升级测试：验证pq功能具备场景下升级提交及回滚功能正常；
- 资料测试：覆盖企业版、轻量版，发现issue3个，已全部解决。

需求覆盖100%，用例执行100%，总计发现issue共11个，4个问题待解决；开发质量一般。

## 3.2 约束说明

> 安装完数据库后，需要获取PQ加速安装包，详见[链接](https://support.huawei.com/enterprise/zh/kunpeng-computing/kunpeng-boostkit-pid-253662285/software/263761464?idAbsPath=fixnode01|23710424|251364417|9856629|253662285)获取libkvecturbo.so安装包。解压安装包获得rpm文件并安装，默认路径为/usr/local/sra_recall。配置环境变量export DATAVEC_PQ_LIB_PATH=<libkvecturbo.so所在路径>
>
> PQ特性暂时只支持ARM架构环境。
>
> PQ特性暂时只支持HNSW和IVF索引。
>
> PQ特性暂时只支持vector数据类型，在其他向量数据类型构建HNSWPQ以及IVFPQ索引会导致执行失败。
>
> 在创建PQ索引前需要先插入数据，无数据情况下会创建失败。
>



# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                       | 特性价值评估                                                 | 应用说明及关键约束假设依赖         | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估                                          | 主要风险 |
| ------------------------------------------ | ------------------------------------------------------------ | ---------------------------------- | -------------------- | ---------------- | ----------------------------------------------------- | -------- |
| IVF-PQ索引算法支持，融合PQ距离计算加速算法 | 乘积量化(Product Quantization, PQ)是一种基于高效压缩高维向量的方法，适用于大规模数据集的相似度搜索。通过将高维向量分割成为多个低维子向量，并对每个子向量进行独立聚类，将原始向量表示为一系列质心，从而显著减少内存使用和提升检索速度。本需求从功能、可靠性、性能层面进行测试，达到验收标准。 | 安装完数据库后需要加载boostkit插件 | 待解决issue4个       | 100%             | <font color=yellow><font color=yellow>▲</font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 功能测试结论

HNSWPQ、IVFPQ索引创建、修改、重建、与dml交互测试、索引检索及索引检索Guc参数交互测试功能正常。目前ustore和极致RTO场景存在问题，待解决issue。

###  4.2.2 性能测试结论

（1）基于920B完成HNSWPQ索引性能测试，openGauss向量数据库HNSW索引整体吞吐量相比EMR8558P上的PGVector/milvus高于30%以上；达到性能验收标准；

（2）基于920B完成IVFPQ索引性能测试，IVFPQ索引性能较IVFFLAT索引性能吞吐量提升30%；达到性能验收标准。

**HNSWPQ性能测试比对EMR环境信息**

EMR8558P 操作系统：openEuler 22.03LTS SP3，编译器：GCC10.3，python:3.11

**HNSWPQ性能测试结果：**

| 压测工具       | 数据集        | 920B/openGaussPQ（QPS） | EMR  4numa/PGVector（QPS） | EMR  4numa/Milvus（QPS） | 性能提升 |
| -------------- | ------------- | ----------------------- | -------------------------- | ------------------------ | -------- |
| ann-benchmarks | DEEP          | 10556.46                | 5691.05                    | /                        | 85.49%   |
| ann-benchmarks | Fashion-MNIST | 33802.52                | 13792.12                   | /                        | 145.09%  |
| ann-benchmarks | GIST          | 8219.37                 | 2682.22                    | /                        | 206.44%  |
| ann-benchmarks | GloVe         | 5038.19                 | 3473.95                    | /                        | 45.03%   |
| ann-benchmarks | SIFT          | 11766.28                | 7838.97                    | /                        | 50.10%   |
| VectorDBBench  | Cohere        | 6728.26                 | /                          | 3675.20                  | 83.07%   |

**IVFFLAT与IVFPQ性能比对测试结果：**

| 数据集           | IVFFLAT(QPS) | IVFPQ(QPS) | 性能提升 |
| ---------------- | ------------ | ---------- | -------- |
| Fashion-MNIST    | 409.59       | 460.30     | 12.38%   |
| GIST             | 8.51         | 7.43       | -12.74%  |
| GloVe            | 4.35         | 17.49      | 302.11%  |
| SIFT             | 47.14        | 118.01     | 150.36%  |
| **总体性能提升** |              |            | 100.58%  |

###  4.2.3 可靠性测试结论

整体验证结论，数据库状态功能正常，但是存在偶现问题，注入故障恢复后，使用索引查询主备结果不一致，[问题单IBRL10](https://e.gitee.com/opengaussorg/issues/table?issue=IBRL10)

| 测试场景                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 构建索引、重建索引过程主机注入kill-9、kill-19、kill-2、重启switchover等故障，验证合理报错，恢复数据库再次执行成功，主备索引查询结果一致； | 部分通过 |
| 构建索引、重建索引过程备机注入kill-9、kill-19、重启switchover等故障，验证主机SQL执行正常，恢复数据库后，主备索引查询结果一致； | 部分通过 |
| 存在索引场景下循环进行增删改数据过程注入kill-9、kill-19、kill-2、数据库重启，验证合理报错，恢复数据库后再次执行功能正常，主备索引查询结果一致； | 部分通过 |
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

不涉及

###  4.2.9 升级测试结论

| 测试场景                                         | 升级路径                                            | 测试结果 |
| ------------------------------------------------ | --------------------------------------------------- | -------- |
| 验证存在PQ功能的数据库集群升级提交、回滚功能正常 | openGauss 7.0.0-RC1 B017-->openGuass 7.0.0-RC1 B018 | 通过     |

## 4.3 资料测试结论

| 序号 | 测试章节                                                   | 测试结论     |
| ---- | ---------------------------------------------------------- | ------------ |
| 1    | 文档地图>关于openGauss>特性描述>企业级特性>DataVec向量引擎 | 整体质量良好 |
| 2    | 文档地图>数据库参考>GUC参数说明>DataVec向量引擎参数        | 整体质量良好 |
| 3    | 文档地图>简易教程>高级特性>PQ                              | 整体质量良好 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 9        | 0    | 6    | 3    | 0      |
| 百分比 | 100%     | 0%   | 67%  | 33%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBPAF3](https://gitee.com/opengaussorg/dashboard?issue_id=IBPAF3) | 【测试类型：资料】【测试版本：7.0.0】 DATAVEC PQ相关资料链接失效、页面缩进格式需要调整、部分说明需要增加 | 次要     | 已验收   |
| [IBOUP5](https://gitee.com/opengaussorg/dashboard?issue_id=IBOUP5) | 【测试类型：SQL功能】【测试版本：7.0.0】 ivfpq索引选项by_residual为on的情况下，update、insert数据产生lwlock deadlock后FATAL | 主要     | 已验收   |
| [IBNVZ0](https://gitee.com/opengaussorg/dashboard?issue_id=IBNVZ0) | 【测试类型：资料】【测试版本：7.0.0】 IVFPQ索引选项lists为200，查询参数ivfflat_probes为200的情况下，查询召回率为0或者极低 | 主要     | 已验收   |
| [IBMRPT](https://gitee.com/opengaussorg/dashboard?issue_id=IBMRPT) | 【测试类型：资料】【测试版本：7.0.0】 PQ索引已支持IVFPQ，但是“DataVec向量引擎”页面资料未更新资料。PQ索引创建数据量小于pq_ksub值会NOTICE提示召回率低，建议资料增加说明约束 | 次要     | 已验收   |
| [IBMR9S](https://gitee.com/opengaussorg/dashboard?issue_id=IBMR9S) | 【测试类型：SQL功能】【测试版本：7.0.0】 使用HNSWPQ，内积距离索引查询召回率为0 | 主要     | 已验收   |
| [IBLNZ1](https://gitee.com/opengaussorg/dashboard?issue_id=IBLNZ1) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnswpq索引，在高维度场景下，创建索引选项pq_m值较大会出现failed，执行失败 | 主要     | 已验收   |
| [IBGJGG](https://gitee.com/opengaussorg/dashboard?issue_id=IBGJGG) | 【测试类型：SQL功能】【测试版本：7.0.0】 enable_hnswpq设置为on创建pq索引，再重新设置enable_hnswpq为off重启集群后，使用pq索引查询表，数据库异常core | 主要     | 已验收   |
| [IBRL10](https://e.gitee.com/opengaussorg/issues/table?issue=IBRL10) | 【测试类型：故障注入】【测试版本：7.0.0】【概率问题】 向量数据插入、创建索引过程进行备机kill-19注入故障后恢复，对主备使用索引查询结果不一致，停一段时间后再查询后数据一致。需要定位数据不一致及后续又一致的原因 | 次要     | 待办的   |
| [IBSTZL](https://e.gitee.com/opengaussorg/issues/table?issue=IBSTZL) | opengauss长稳测试出现内存泄漏问题                            | 主要     | 待办的   |
| [IBSORG](https://e.gitee.com/opengaussorg/issues/table?issue=IBSORG) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnswpq、hnsw索引存在的Ustore表进行增删改，会异常抛错‘DETAIL:  Could not open file "pg_csnlog/000058064800": ’<br />缺陷统计在【内核加速组】DataVec向量数据库能力增强测试报告.md | 严重     | 待办的   |
| [IBSLKT](https://e.gitee.com/opengaussorg/issues/table?issue=IBSLKT) | 【测试类型：SQL功能】【测试版本：7.0.0】 极致RTO备机读场景下，向量索引（ivfflat、hnsw、ivfpq、hnswpq）创建后，主机进行增删改后，备机进行查询core<br />缺陷统计在【内核加速组】DataVec向量数据库能力增强测试报告.md | 严重     | 待办的   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | PQ索引开关 | 通过设置GUC参数进行验证，设置值与PQ加速插件具备情况组合测试，验证功能正常、异常情况合理报错； | YES                  |
| 2    | PQ索引检索 | 创建HNSWPQ、IVFPQ索引选项参数、不同索引距离覆盖验证；验证不同索引选项设置不同值后存储空间、执行时长、查询结果召回率准确度趋势验证，功能正常；不同SQL执行场景下可靠性场景验证正常；<br />验证索引检索GUC参数功能验证、设置不同值对查询结果召回率准确性影响趋势正常；索引检索场景下可靠性场景验证正常；<br />极致RTO备机读、ustore场景使用索引功能正常； | YES                  |
| 4    | PQ索引性能 | HNSWPQ索引性能通过annbenchmark和VectorDBBench工具，验证不同pq_m，ef_construction、ef_search、sub_dim、earlystop_threshold，在召回率大于99%的情况下，选择qps最高的结果，与EMR  4numa/PGVector、Milvus测试结果进行比对，性能；<br />IVFPQ索引性能，通过annbenchmark工具，验证不同ivfflat_probes在召回率大于99%情况下，选择qps最高结果，比对相同环境下IVFFLAT索引，性能提升30%以上； | YES                  |
| 5    | PQ升级     | 验证存在PQ功能的场景下升级、回滚功能正常；                   | YES                  |
| 6    | PQ资料     | 验证涉及资料正确，可正常指导用户进行PQ相关功能加载、使用过程参数选项配置等。 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数                                          | 代码量（KLOC） | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | --------------------------------------------------- | -------------- | -------- |
| openGauss 7.0.0-RC1 B010 | 3                | 4          | 4          | 1                                                   | 3.493          | 0.286    |
| openGauss 7.0.0-RC1 B016 | 5                | 110        | 110        | 3                                                   | 3.493          | 0.859    |
| openGauss 7.0.0-RC1 B017 | 5                | 72         | 72         | 3                                                   | 3.493          | 0.859    |
| openGauss 7.0.0-RC1 B018 | 2                | 91         | 74         | 4(其中2个是非PQ同样存在问题，计算缺陷密度按2个计算) | 3.493          | 0.572    |

- 总体缺陷密度：9/3.493kloc=2.577

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 4              | 4                | 3      | 1      | 0       | 0           | 100%   | 75%        |
| 第二轮 | 110            | 110              | 50     | 60     | 0       | 0           | 100%   | 45%        |
| 第三轮 | 72             | 72               | 60     | 12     | 0       | 0           | 100%   | 83%        |
| 第四轮 | 91             | 74               | 67     | 7      | 17      | 0           | 100%   | 90%        |



| 异常用例情况    | 影响分析                                                     | 规避措施 | 后续计划 |
| --------------- | ------------------------------------------------------------ | -------- | -------- |
| Failed用例5个   | 可靠性场景，备机注入不同故障恢复后，主备使用索引查询的结果一段时间存在不一致 | 暂无     | 待解决   |
| Failed用例2个   | 极致RTO、ustore场景bug；                                     | 暂无     | 待解决   |
| Blocked用例17个 | 极致RTO、ustore场景存在bug，导致相关场景无法执行             | 暂无     | 待解决   |

# 7 附件

##  7.1 附件1：遗留问题列表

| 序号 | 问题单号                                                     | 问题描述                                                     | 分类   | 问题级别 | 问题分析与影响                                     | 规避措施                                       |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ | -------- | -------------------------------------------------- | ---------------------------------------------- |
| 1    | [IBRL10](https://e.gitee.com/opengaussorg/issues/table?issue=IBRL10) | 【测试类型：故障注入】【测试版本：7.0.0】【概率问题】 向量数据插入、创建索引过程进行备机kill-19注入故障后恢复，对主备使用索引查询结果不一致，停一段时间后再查询后数据一致。需要定位数据不一致及后续又一致的原因 | 可靠性 | 次要     | 故障注入后恢复，备机概率出现索引查询结果不一致问题 | 目前无规避措施，概率等待一段时间后查询结果一致 |
| 2    | [IBSORG](https://e.gitee.com/opengaussorg/issues/table?issue=IBSORG) | 【测试类型：SQL功能】【测试版本：7.0.0】 hnswpq、hnsw索引存在的Ustore表进行增删改，会异常抛错‘DETAIL:  Could not open file "pg_csnlog/000058064800": ’ | 功能   | 主要     | ustore表相关dml无法执行                            | 待优化                                         |
| 3    | [IBSLKT](https://e.gitee.com/opengaussorg/issues/table?issue=IBSLKT) | 【测试类型：SQL功能】【测试版本：7.0.0】 极致RTO备机读场景下，向量索引（ivfflat、hnsw、ivfpq、hnswpq）创建后，主机进行增删改后，备机进行查询core | 功能   | 严重     | 极致RTO场景备机使用索引查询会core                  | 待优化                                         |
| 4    | [IBSTZL](https://e.gitee.com/opengaussorg/issues/table?issue=IBSTZL) | opengauss长稳测试出现内存泄漏问题                            | 长稳   | 主要     | 内存占用无法释放                                   | 待优化                                         |

##  7.2 附件2：特性相关PR

代码PR：

https://gitee.com/opengauss/openGauss-server/pulls/6786
https://gitee.com/opengauss/openGauss-server/pulls/7119

资料PR：

https://gitee.com/opengauss/docs/pulls/6997
https://gitee.com/opengauss/docs/pulls/7184

测试设计归档：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=cfebf800dba84ca794bc05d9967712c6&hideDevcloudHead=true

测试用例归档：

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg

