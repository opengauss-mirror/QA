![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者     |
| --------- | -------- | ------------ | -------- |
| 2025.2.28 | v1.0     | 测试报告初稿 | ningyali |

**Keywords 关键词**：HTAP行列融合

**Abstract 摘要**：本文档主要介绍openGauss行列融合功能的验证测试结果；

**缩略语清单：**

| 缩略语 | 英文全名                                   | 中文解释 |
| ------ | ------------------------------------------ | -------- |
| HTAP   | Hybrid Transactional/Analytical Processing | 行列融合 |


# 1 概述

本报告主要测试单机和主备集群场景下，HTAP行列融合的功能测试、可靠性测试、性能测试、兼容性测试和资料测试。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build 3dafabf6 | openGauss  | 2024-11-7    | 2024-11-28   | ningyali |
| openGauss 7.0.0-RC1 build 5baeba20 | openGauss  | 2024-11-28   | 2025-1-9     | ningyali |
| openGauss 7.0.0-RC1 build 8c7a615a | openGauss  | 2025-1-9     | 2025-2-7     | ningyali |
| openGauss 7.0.0-RC1 build 445855a8 | openGauss  | 2025-2-7     | 2025-2-27    | ningyali |
| openGauss 7.0.0-RC1 build eb295a12 | openGauss  | 2025-2-27    | 2025-3-7     | ningyali |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng-920<br/>内存：721GB<br/>硬盘：1TB<br/>OS：openEuler release 22.03 (LTS-SP4) |      |
| 虚拟机   | 内存：15GB<br/>硬盘：80GB<br/>OS：openEuler release 20.03 (LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试单机&主备集群行列融合功能，主要包含功能场景测试、可靠性场景测试、兼容性场景测试、性能场景测试，输出测试用例402个，执行测试2轮，发现issue共25个，所有问题已修复并回归通过，整体质量良好。

## 3.1 特性约束

表约束：仅支持普通表的行列转换，临时表、系统表、Toast表、Unlogged表、列存表、外表暂不支持，主备场景下不支持段页式表的行列转换。

数据类型约束：参考列存表支持的数据类型

[详见资料页](https://docs.opengauss.org/zh/docs/latest/docs/AboutopenGauss/%E8%A1%8C%E5%88%97%E8%9E%8D%E5%90%88.html)



# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ---------------------------------------------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【内核加速组】传统主备支持行列转换，备机支持列存查询 | 通过简单的指令设置，有效利用备节点可用内存空间进行行存数据的列缓存转换及存储（In-Memory-Column-Store）。考虑列存的查询优势，在数据量庞大，表结构复杂，而用户仅关注部分列数据的查询的场景下，行列转换后的列缓存可有效提升企业执行大型复杂OLAP数据分析的整体查询效率。 | 见3.1约束说明              | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

#### 4.2.1.1 TPCH

参数配置：

​       单机：max_process_memory=350GB、shared_buffers=150GB、max_imcs_cache=150GB、work_mem=5GB、query_dop=4

​       主备：shared_buffers=150GB、max_imcs_cache=150GB、work_mem=5GB、query_dop=4

测试结论：

​       22条TPC-H语句100G数据进行性能测试，通过单节点、主备机分别执行22条TPC-H SQL语句，对比行列转换前行存查询耗时和行列转换后行列混合查询耗时，测试结果为单机场景下性能提升5.09倍，主备场景下性能提升5.31倍，满足性能提升5倍的基准要求。

#### 4.2.1.2 TPCC测试结论

参数配置：

​       单机：max_process_memory=400GB、shared_buffers=50GB、max_imcs_cache=150GB、update_lockwait_timeout=60min、lockwait_timeout=60min

​       主备：max_process_memory=400GB、shared_buffers=50GB、max_imcs_cache=150GB、update_lockwait_timeout=60min、lockwait_timeout=60min

测试结论：

​       进行TPC-C性能测试，<u>经验证2p主机读写备机只读场景数据一致性校验通过，单机场景下性能差异X，主备场景下性能差异，满足性能差异小于10%的基准要求。</u>

### 4.2.3 可靠性测试结论

针对单节点和主备集群，分别进行故障场景验证和故障恢复功能验证，验收通过。

| 测试步骤                                                     | 测试结果                                               |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| 单机场景，行列转换过程中节点异常                             | 故障不会导致数据库宕机，故障恢复后可正常转换，数据一致 |
| 主备场景，行列转换过程中节点异常，主备切换，备机重建，备升主 | 故障不会导致集群宕机，故障恢复后可正常转换，数据一致   |
| 行列转换生成的临时文件异常                                   | 故障不会导致数据库宕机，故障恢复后可正常转换，数据一致 |
| 磁盘满时进行行列转换                                         | 故障不会导致数据库宕机，故障恢复后可正常转换，数据一致 |
| 主备网络异常时进行行列转换或查询列存数据                     | 故障不会导致数据库宕机，故障恢复后可正常转换，数据一致 |
| 一主两备缩容成一主一备、单节点，进行行列转换                 | 故障不影响行列转换功能，已经转换的数据可取消重新转换   |
| 单节点扩容成一主一备、一主两备，进行行列转换                 | 故障不影响行列转换功能，已经转换的数据可取消重新转换   |

###  4.2.4 安全&隐私保护测试结论

无

### 4.2.5 可服务性测试结论

无

### 4.2.6 生命周期管理测试结论

无

### 4.2.7 韧性测试结论

无

###  4.2.8 兼容性测试结论

| 测试步骤                                                     | 测试结果 |
| ------------------------------------------------------------ | -------- |
| 6.0.0升级到7.0.0，验证新增GUC参数无误，行列转换功能正常      | 验收通过 |
| 6.0.0升级到7.0.0，升级回滚再升级，验证7.0.0版本新增GUC参数无误，行列转换功能正常，升级回滚后6.0.0版本无新增GUC参数和行列转换功能 | 验收通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [HTAP行列融合](https://docs.opengauss.org/zh/docs/latest/docs/AboutopenGauss/%E8%A1%8C%E5%88%97%E8%9E%8D%E5%90%88.html) | 测试通过 |
| 2    | [行列融合参数](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseReference/%E8%A1%8C%E5%88%97%E8%9E%8D%E5%90%88%E5%8F%82%E6%95%B0.html) | 测试通过 |
| 3    | [HTAP行列融合系统函数](https://docs.opengauss.org/zh/docs/latest/docs/SQLReference/HTAP%E8%A1%8C%E5%88%97%E8%9E%8D%E5%90%88%E7%B3%BB%E7%BB%9F%E5%87%BD%E6%95%B0.html) | 测试通过 |
| 4    | [GS_TOTAL_MEMORY_DETAIL](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseReference/GS_TOTAL_MEMORY_DETAIL.html) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

覆盖单机和主备场景下，表类型、数据类型、存储过程、函数、触发器、多表连接查询等，结合行列转换功能验证测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 25       | 0    | 2    | 19   | 4      |
| 百分比 | 100%     | 0%   | 8%   | 77%  | 15%    |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBJJ2Q](https://e.gitee.com/opengaussorg/issues/table?issue=IBJJ2Q) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】空表行列转换后插入大量数据，备机列存查询hang住 | 主要     | 已验收   |
| [IB89EK](https://e.gitee.com/opengaussorg/issues/table?issue=IB89EK) | 【测试类型：SQL功能】【测试版本：7.0.0】 【HTAP行列融合】配置参数support_extended_features后，行列转换后修改压缩策略后，备机查询hang住 | 主要     | 已验收   |
| [IBNTMR](https://e.gitee.com/opengaussorg/issues/table?issue=IBNTMR) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】 【HTAP行列融合】行列转换后，增删大量数据，gs_total_memory_detail和query_imcstore_views视图中已使用内存字段无变化 | 次要     | 待办的   |
| [IBLLLP](https://e.gitee.com/opengaussorg/issues/table?issue=IBLLLP) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】【HTAP行列融合】 兼容B库下进行指定分区转换后select count(*)查询计划不走imcs scan | 次要     | 已验收   |
| [IBKU42](https://e.gitee.com/opengaussorg/issues/table?issue=IBKU42) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】【HTAP行列融合】 主备场景下999个分区进行指定分区转换耗时过久 | 次要     | 已完成   |
| [IBJ8P9](https://e.gitee.com/opengaussorg/issues/table?issue=IBJ8P9) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】 数据库下存在行列转换数据时删除数据库报错 | 次要     | 已验收   |
| [IBCPQI](https://e.gitee.com/opengaussorg/issues/table?issue=IBCPQI) | 【测试类型：SQL功能】【测试版本：7.0.0】 【HTAP行列融合】配置了enable_imcsscan为on，select count(*)查询计划未走imcs scan | 次要     | 已验收   |
| [IBADTT](https://e.gitee.com/opengaussorg/issues/table?issue=IBADTT) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】变长类型如nvachar/nvarchar2指定较小长度时转换报错 | 次要     | 已验收   |
| [IBA72A](https://e.gitee.com/opengaussorg/issues/table?issue=IBA72A) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】A库中转换关系清除后切换B库进行行列转换报错 | 次要     | 已验收   |
| [IB9K79](https://e.gitee.com/opengaussorg/issues/table?issue=IB9K79) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】分区表删除后系统视图query_imcstore_views中相关数据未清除 | 次要     | 已验收   |
| [IB6B45](https://e.gitee.com/opengaussorg/issues/table?issue=IB6B45) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】 表中含hll/cidr/inet类型进行全表转换报错 | 次要     | 已验收   |
| [IB2EIV](https://e.gitee.com/opengaussorg/issues/table?issue=IB2EIV) | 【测试类型：SQL功能】【测试版本：7.0.0】 【HTAP行列融合】高概率偶现主备场景全表转换时报错 | 次要     | 已验收   |
| [IB47TY](https://e.gitee.com/opengaussorg/issues/table?issue=IB47TY) | 【测试类型：SQL功能】【测试版本：7.0.0】【HTAP行列融合】对toast表进行行列转换应合理报错 | 次要     | 已验收   |
| [IBQDOJ](https://e.gitee.com/opengaussorg/issues/table?issue=IBQDOJ) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】 【HTAP行列融合】数据库不存在行列转换数据时删除不应有相关提示 | 不重要   | 待办的   |
| [IBR3QU](https://e.gitee.com/opengaussorg/issues/table?issue=IBR3QU) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】【HTAP行列融合】单机场景，恢复移走的imcu文件，查询hang | 次要     | 待回归   |
| [IBP6KD](https://e.gitee.com/opengaussorg/issues/table?issue=IBP6KD) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】 【HTAP行列融合】主备场景，磁盘满行列转换报错，清理磁盘后取消转换再转换报错 | 次要     | 修复中   |
| [IBOBAV](https://e.gitee.com/opengaussorg/issues/table?issue=IBOBAV) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】【HTAP行列融合】主备场景，删除行列转换生成的临时文件后，取消转换再转换偶现hang住 | 次要     | 修复中   |
| [IBN4PI](https://e.gitee.com/opengaussorg/issues/table?issue=IBN4PI) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】【HTAP行列融合】 主备切换后，执行行列转换，再主备切换还原主备，取消转换再次转换报错 | 次要     | 修复中   |
| [IBMY8K](https://e.gitee.com/opengaussorg/issues/table?issue=IBMY8K) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】 【HTAP行列融合】行列转换过程中备2转变成备1的级联备，转换失败后，取消转换再次转换报错 | 次要     | 已验收   |
| [IBMXZ0](https://e.gitee.com/opengaussorg/issues/table?issue=IBMXZ0) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】【HTAP行列融合】 一主两备集群，备1异常时行列转换报错 | 次要     | 已验收   |
| [IBM3W5](https://e.gitee.com/opengaussorg/issues/table?issue=IBM3W5) | 【测试类型：可靠性】【测试版本：7.0.0-RC1】【HTAP行列融合】主备场景，行列转换过程中备节点shutdown，主节点无响应 | 次要     | 已验收   |
| [IBQLIO](https://e.gitee.com/opengaussorg/issues/table?issue=IBQLIO) | 【测试类型：性能】【测试版本：7.0.0-RC1】【HTAP行列融合】benchmark连接主节点执行过程中，guc设置enable_imcsscan备机core | 次要     | 待办的   |
| [IB2CLH](https://e.gitee.com/opengaussorg/issues/table?issue=IB2CLH) | 【测试类型：资料】【测试版本：7.0.0】 【HTAP行列融合】资料未说明远程连接时pg_hba.conf中需将主备节点认证方式配置为trust | 不重要   | 已验收   |
| [IBADDH](https://e.gitee.com/opengaussorg/issues/table?issue=IBADDH) | 【测试类型：资料】【测试版本：7.0.0】【HTAP行列融合】左侧导航栏无HTAP行列融合 | 不重要   | 已验收   |
| [IBCPVR](https://e.gitee.com/opengaussorg/issues/table?issue=IBCPVR) | 【测试类型：资料】【测试版本：7.0.0】【HTAP行列融合】资料中约束未说明不支持vacuum full操作，支持其他vacuum操作 | 不重要   | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | 单机&主备场景，验证新增GUC参数，所有支持表/数据类型及相关业务 | YES                  |
| 2    | 可靠性测试 | 单机&主备场景，验证故障过程中数据库不会宕机，故障恢复数据库可恢复功能 | YES                  |
| 3    | 性能测试   | 单机&主备场景，TPC-C测试验证数据一致性且性能差异小于10%，TPC-H测试验证性能提升5倍 | YES                  |
| 4    | 兼容性测试 | 升级，升级回退，升级回退再升级，验证新版本行列转换功能正常，旧版本无相关参数和功能 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 |       | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ----- | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC1 B002 | 10.31 | 18               | 80         | 80         | 4          | 7.672K |          |
| openGauss 7.0.0-RC1 B006 | 11.28 | 18               | 80         | 80         | 7          | 7.672K |          |
| openGauss 7.0.0-RC1 B011 | 1.9   | 15               | 80         | 80         | 2          | 7.672K |          |
| openGauss 7.0.0-RC1 B014 | 2.7   | 18               | 80         | 80         | 9          | 7.672K |          |
| openGauss 7.0.0-RC1 B017 | 2.27  | 7                | 82         | 82         | 4          | 7.672K |          |

本次测试共发现25个issue，部分已修复并回归通过，缺陷密度为25/7.672k=3.25，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 402          | 80               | 76     | 4      | 0       | 0           | 100%   | 95%        |
| 402          | 80               | 73     | 7      | 0       | 0           | 100%   | 91%        |
| 402          | 80               | 78     | 2      | 0       | 0           | 100%   | 97.5%      |
| 402          | 80               | 71     | 9      | 0       | 0           | 100%   | 89%        |
| 402          | 82               | 78     | 4      | 0       | 0           | 100%   | 97%        |

本次测试共输出测试用例402个，执行测试共2轮，发现issue共25个，所有问题已修复并回归通过，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitee.com/opengauss/openGauss-server/pulls/6565

https://gitee.com/opengauss/openGauss-server/pulls/6834

资料pr：

https://gitee.com/opengauss/docs/pulls/6910

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=a3dc7428798c48f98b4d3f215e2ec46b&hideDevcloudHead=true

测试用例：

testplan _ openGauss_7.0.0 _ 数据库服务 _ HTAP行列融合

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg

