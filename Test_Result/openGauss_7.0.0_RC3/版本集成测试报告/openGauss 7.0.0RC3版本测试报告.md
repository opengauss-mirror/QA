![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改章节 | 修改描述 | 作者      |
| --------- | -------- | -------- | -------- | --------- |
| 2026.3.14 | 1.0      | 初稿撰写 |          | sungang14 |
|           |          |          |          |           |
|           |          |          |          |           |

目 录

1 概述

2 测试版本说明

> 2.1 测试版本信息

> 2.2 测试环境
>
> 2.3 需求清单

3 版本概要测试结论

4 版本详细测试结论

> 4.1 特性测试结论

> 4.2 专项测试结论
>
> 4.3 DBV伙伴测试结论

5 问题单统计

> 5.1 问题单统计

> 5.2 问题单分析
>
> 5.3 执行用例与问题单数量对应关系
>
> 5.4 继承测试迭代版本基线

6 附件

7 致谢

**Keywords 关键词**：openGauss 7.0.0 RC3

**Abstract 摘要**：主要是描述了openGauss 7.0.0 RC3版本的整体测试情况，给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。

> 缩略语清单： 

| 缩略语 | 英文全名                           | 中文解释                               |
| ------ | ---------------------------------- | -------------------------------------- |
| SQL    | Structured Query Language          | 结构化查询语言                         |
| LTS    | Long Time support                  | 长时间维护                             |
| DML    | Data Manipulation Language         | 数据操纵语言                           |
| DDL    | Data Definition Language           | 数据定义语言                           |
| DCL    | Data Control Language              | 数据控制语言                           |
| CM     | Cluster Management                 | 集群管理工具                           |
| DMS    | Distributed Memory Service         | 分布式内存服务                         |
| DSS    | Distributed Storage Service        | 分布式存储服务                         |
| RAG    | Retrieval-Augmented Generation     | 检索增强生成                           |
| HNSW   | Hierarchical Navigable Small World | 基于分层导航小世界（HNSW）算法的图索引 |
| OGAI   | openGauss AI                       | openGauss智能向量化框架                |



***


# 1、概述

openGauss是一款全面友好开放，携手伙伴共同打造的企业级开源关系型数据库。openGauss提供面向多核架构的极致性能、全链路的业务、数据安全、基于AI的调优和高效运维的能力。openGauss具有高性能、高可靠、高安全和易运维等特性，深度融合华为在数据库领域多年的研发经验，结合企业级场景需求，持续构建竞争力特性。

openGauss 7.0.0 RC3版本新增内核场景化、工具链、资源池化和兼容性等特性，并对若干关键缺陷进行了修改。本文主要描述了openGauss 7.0.0 RC3版本整体测试情况，重点从特性质量、专项测试和问题单统计等维度展开叙述。综合来看，openGauss 7.0.0 RC3版本整体质量良好。

# 2、测试版本说明

## 2.1 版本测试信息

| 版本名称                 | 测试起始时间 | 测试结束时间 |
| ------------------------ | ------------ | ------------ |
| openGauss 7.0.0 RC3 B001 | 2025/10/1    | 2025/10/22   |
| openGauss 7.0.0 RC3 B002 | 2025/10/23   | 2025/10/29   |
| openGauss 7.0.0 RC3 B003 | 2025/10/30   | 2025/11/5    |
| openGauss 7.0.0 RC3 B004 | 2025/11/6    | 2025/11/12   |
| openGauss 7.0.0 RC3 B005 | 2025/11/13   | 2025/11/19   |
| openGauss 7.0.0 RC3 B006 | 2025/11/20   | 2025/11/26   |
| openGauss 7.0.0 RC3 B007 | 2025/11/27   | 2025/12/3    |
| openGauss 7.0.0 RC3 B008 | 2025/12/4    | 2025/12/10   |
| openGauss 7.0.0 RC3 B009 | 2025/12/11   | 2025/12/17   |
| openGauss 7.0.0 RC3 B010 | 2025/12/18   | 2025/12/24   |
| openGauss 7.0.0 RC3 B011 | 2025/12/25   | 2025/12/28   |
| openGauss 7.0.0 RC3 B012 | 2025/12/29   | 2025/12/31   |
| openGauss 7.0.0 RC3 B013 | 2026/1/1     | 2026/1/14    |
| openGauss 7.0.0 RC3 B014 | 2026/1/15    | 2026/1/21    |
| openGauss 7.0.0 RC3 B015 | 2026/1/22    | 2026/1/28    |
| openGauss 7.0.0 RC3 B016 | 2026/1/29    | 2026/2/4     |
| openGauss 7.0.0 RC3 B017 | 2026/2/5     | 2026/2/11    |
| openGauss 7.0.0 RC3 B018 | 2026/2/12    | 2026/2/25    |
| openGauss 7.0.0 RC3 B019 | 2026/2/26    | 2026/3/4     |
| openGauss 7.0.0 RC3 B020 | 2026/3/5     | 2026/3/11    |
| openGauss 7.0.0 RC3 B021 | 2026/3/12    | 2026/3/16    |
| openGauss 7.0.0 RC3 B022 | 2026/3/17    | 2026/3/19    |
| openGauss 7.0.0 RC3 B023 | 2026/3/20    | 2026/3/23    |
| openGauss 7.0.0 RC3 B024 | 2026/3/24    | 2026/3/30    |

## 2.2 测试环境

| 硬件型号                              | 硬件配置信息                                                 | 备注 |
| ------------------------------------- | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 22.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 7260 2p 128核<br />内存：768G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 24.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| TaiShan 200 (Model 2480)              | CPU：Kunpeng-920 7260 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| TaiShan 200 (Model 2280)              | CPU：Kunpeng-920 V200 7270z 2p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*10GE |      |
| RH2288H V3                            | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4*10GE |      |
| 服务端<br />Taishan 200（Model 2280） | CPU：Kunpeng 920<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 鲲鹏920 7280Z                         | CPU：Kunpeng 920 7280Z<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 鲲鹏950                               | CPU：--<br />内存：256GB<br />数据盘：2\*3.2TB NVME SSD<br />文件系统：EXT4<br />网卡：2\*25GE |      |
| 存储侧<br />OceanStore                | Dorado 18500 V6                                              |      |
| DCS/ECS虚拟机                         | 8核30G                                                       |      |
| 公有云虚拟机                          | 8核16G/8核32G/16核32G                                        |      |

虚拟化平台

| 虚拟化平台 | 版本说明                                                     |
| ---------- | ------------------------------------------------------------ |
| KVM虚拟化  | KVM+GuestOS（Centos7.6/OpenEuler20.03(LTS)/openEuler22.03(LTS)/openEuler24.03(LTS)） |

OS版本说明如下：

| 操作系统  | OS版本           | 版本说明                                                     |
| --------- | ---------------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，aarch版本ISO<br />SHA256:3e7cb72d746c5385b02b7a4bf18360925145d13f06bbd41c1a137e545b651d40 |
| OpenEuler | 20.03 (LTS)      | openEuler 20.03 (LTS)，x86-64版本ISO<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，aarch版本ISO<br />SHA256:8ee6e6ea6fe3af075846efb28196aac6edd50c99b663b0fc4651fa71195a68e6 |
| OpenEuler | 22.03（LTS）     | openEuler 22.03 (LTS)，x86-64版本ISO<br />SHA256:a07952feb2f9f0239143daf6cc061a396e09bbb3e26d8fbf38eeb21d0251bde0 |
| OpenEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，aarch版本ISO<br />SHA256:9967f955f5b7637cf77bc4ecb69cb6ac3816d591357c374a53c0dbbfd5402ac9 |
| OpenEuler | 24.03（LTS）     | openEuler 24.03 (LTS)，x86-64版本ISO<br />SHA256:786b9683659512e71c978c34aea806a97ed6cacf04e1b0a22017a50eec582cbe |
| CentOS    | 7.6.1810（Core） | CentOS Linux release 7.6.1810（Core），x86-64版本ISO<br />SHA256:6d44331cc4f6c506c7bbe9feb8468fad6c51a88ca1393ca6b8b486ea04bec3c1 |

## 2.3 需求清单

openGauss 7.0.0 RC3版本是openGauss社区继续联合多方力量推出的版本，本次发布的需求列表、分工如下：

| 序号 | 需求                                                         | 开发主体      | 测试主体 | 验证策略                                                     |
| ---- | ------------------------------------------------------------ | ------------- | -------- | ------------------------------------------------------------ |
| 1    | 多写驱动能力增强                                             | oGRAC         | QA       | 1、基本功能验证：SQLAllocEnv、SQLFreeEnv、SQLSetEnvAttr、SQLAllocConnect、SQLFreeConnect、SQLSetConnectAttr、SQLConnect、SQLDisconnect、SQLAllocStmt、SQLFreeStmt、SQLSetStmtAttr、SQLExecDirect、SQLPrepare、SQLBindParameter、SQLExecute、SQLPutData、SQLBindCol、SQLFetch、SQLGetData、SQLRowCount、SQLError接口基础功能、入参边界值和入参排列组合以及重复性执行的相关用例执行，观测执行结果是否符合预期即用例是否执行通过<br/>2、内存稳定性验证：多次连接执行DDL语句，观察内存变化<br/>3、长期稳定性验证：建立ODBC连接以后，定期执行简单语句，保持24小时以上<br/>4、并发稳定性验证：创建100个会话，同时执行查询更新操作保持12小时以上<br/>5、网络稳定性验证：占用端口模拟网络中断并在30s后恢复重新连接<br/>6、资料测试：检查官网资料对特性描述是否完整准确 |
| 2    | 参数化路径功能增强                                           | SQL           | QA       | 功能验证：验证自动化参数功能支持select特性、GPC特性和兼容B库<br/>性能验证：性能测试验证支持自动参数化缓存的DML和DQL在开启参数化缓存功能和不开启参数化功能场景执行时间无劣化<br/>升级验证：验证升级后功能新增功能可用和视图查询函数字段值变更<br/>资料验证：自动参数化和GPC功能相关资料验证 |
| 3    | 支持oGAI                                                     | AI            | QA       | 功能测试：新增schema验证权限控制；ogai.model_sources验证表结构以及权限控制；ogai_embedding验证多种大模型支持以及参数异常情况；ogai_embedding验证异常向量维度；ogai_generate验证模型异常情况，返回大模型结果是否正常；根据问题进行文档相关性评分，参数异常测试，长文本测试；onnx模型加载，卸载，调用测试；自动向量化测试，rag检索 权重，guc参数测试<br/>可靠性测试：存在向量化任务时故障重启，主节点故障备机升主等故障，功能可恢复；单节点扩容，可以自动加载已注册的onnx模型，并且不影响当前任务 |
| 4    | 黑匣子特性增强-支持主备参数同步                              | StorageEngine | QA       | 功能测试：覆盖参数设置、参数生效返回等流程场景<br/>可靠性测试：覆盖进程异常、网络故障以及其他场景 |
| 5    | 黑匣子特性增强-支持秘钥同步                                  | StorageEngine | QA       | 功能测试：基本安装流程，通信流程；证书的生成流程，是否进行同步；更改grcmd参数是否有影响<br/>安全测试：证书的加密验证，权限验证<br/>可靠性测试：错误的秘钥信息和证书信息是否能通信；不同文件夹目录是否能正常通信 |
| 6    | 黑匣子特性增强-支持升级                                      | StorageEngine | QA       | 功能测试：覆盖在线升级、离线升级等流程场景<br/>可靠性测试：覆盖进程异常、网络故障以及其他场景 |
| 7    | 黑匣子特性增强-API优化                                       | StorageEngine | QA       | 功能测试：覆盖流程、正常&异常接口场景测试<br/>可靠性测试：覆盖进程异常、网络故障以及其他场景 |
| 8    | openGauss SQL引擎性能优化                                    | SQL           | QA       | 性能测试：覆盖2P单机和4P单机场景普通标tpcc性能优化是否满足需求指标 |
| 9    | 支持在线DDL+二次增强转测                                     | StorageEngine | QA       | 功能测试：验证常见ddl操作同步进行dml操作，包括增删改查及插入不符合数据<br/>可靠性测试：覆盖进程异常场景 |
| 10   | 灵衢接口变更适配                                             | StorageEngine | QA       | 功能测试：验证基本接口名字是否正确修改；验证SMB工具基本功能是否实现；验证failover是否能实现主备切换<br/>可靠性测试：SMB工具在异常情况是否能执行<br/>性能测试：failover -f新增参数是否能正确快速响应 |
| 11   | D兼容性-系统视图兼容                                         | SQL           | QA       | 功能测试：DDL功能验证；DML功能验证；与继承系统表联合查询；CMAKE/MAKE在多操作系统下测试<br/>升级测试：验证7.0.0-RC1,7.0.0-RC2,6.0.X升级7.0.0-RC3<br/>驱动测试：使用jdbc驱动和psycopg2驱动验证sql |
| 12   | D兼容性-系统函数兼容                                         | SQL           | QA       | 函数基本功能验证：函数基本功能及在复杂场景下的验证<br/>兼容验证：在非兼容D库下执行用例，验证功能是否支持，数据库正常运行<br/>升级验证：从7.0.0RC1、7.0.0RC2升级到7.0.0RC3之后执行用例验证功能是否正确<br/>驱动测试：使用jdbc驱动验证插件相关功能 |
| 13   | B兼容性提升                                                  | Plugin        | QA       | 功能测试：基本语法功能；在主流驱动，框架下的测试验证<br/>兼容测试：在AB库进行系统视图验证<br/>升级测试：从6.0.2、5.0.5升级到7.0.0RC3之后执行用例验证功能是否正确<br/>驱动测试：使用jdbc驱动和psycopg2驱动验证插件相关功能<br/>框架测试：使用MYSQL_JDBC Meta HIBENATE MYBATIS DJANGO等主流框架链接数据库进行测试 |
| 14   | 支持热补丁机制                                               | Tools         | QA       | 补丁安装制作测试：补丁制作安装流程测试，包括符号表制作、文件补丁制作、补丁类型差异、补丁制作命令参数验证、补丁安装参数验证、补丁卸载参数验证、黑白名单配置、重启补丁失效 |
| 15   | 多写SQL引擎能力增强-支持启动代价处理                         | oGRAC         | QA       | 基本语法功能验证：基本语法功能及在复杂场景下的验证           |
| 16   | 多写SQL引擎能力增强-索引扫描增强（一）                       | oGRAC         | QA       | 功能测试：index fast full scan<br/>功能测试：index skip scan<br/>性能测试：特性合入前后索引扫描cost变化 |
| 17   | 多写SQL引擎能力增强-索引扫描增强（二）                       | oGRAC         | QA       | 功能测试：[not] between操作符验证；[not] in操作符验证；[!] = true/false操作符验证；[not] like验证；is [not] null验证；大数据量（1w+）下的预估查询 |
| 18   | 多写SQL引擎能力增强-支持动态采样                             | oGRAC         | QA       | 参数设置：不同设置参数方式进行设置，进行合理范围、非法参数等验证<br />基本功能验证：少量单表采样测试、大量单表采样测试、少量多表采样测试、大量多表采样测试、分区表采样测试、索引采样测试、非约束条件内表能否动态采样测试、表数据缺失场景采样测试<br />稳定性测试：执行完成基本功能用例后，对内存进行24h监控 |
| 19   | 多写SQL引擎能力增强-支持rowid scan                           | oGRAC         | QA       | 功能验证:覆盖单表条件扫描、多表条件扫描、生成索引扫描计划、非nestedloop的场景，其中包括单条件和多条件，单条件包括条件运算符：基础比较运算符、范围运算符、模糊匹配运算符、逻辑运算符、高级条件运算符；数值处理函数：取绝对值、四舍五入、向上取整、向下取整、取余数、取平方根、截断小数。单表条件扫描、多表条件扫描和生成索引扫描计划时支持使用rowid sacn，非nested loop场景下对原本功能无影响则符合预期 |
| 20   | 支持1bit量化索引                                             | StorageEngine | QA       | 功能测试：参数设置->rabitq索引构建、检索与增删改->支持的模式库->支持的表类型和数据类型<br/>性能测试：以容器限核的形式运行，比较openGauss与Milvus索引性能<br/>资料测试：官网资料走读 |
| 21   | 支持direct IO、异步IO                                        | Tools         | QA       | 基本功能验证：基本功能在基础使用场景下是否生效<br/>性能验证：该特性能否满足需求提出的性能要求<br/>升级验证：7.0.0RC1->7.0.0RC3 & 6.0.0->7.0.0RC3 |
| 22   | 通过向量数据库完成推理加速                                   | AI            | QA       | 功能测试：针对CacheBlend框架搭建AI文档小工具，底座数据库为openGauss搭建好服务验证基本功能正常；验证使用过程中数据入库情况正常。 |
| 23   | ES/milvus迁移工具对接datakit                                 | Tools         | QA       | 功能测试：实例添加->迁移工具安装->迁移任务创建->迁移任务执行->数据类型迁移<br/>资料测试：向量数据库迁移脚本资料->portal资料 |
| 24   | 支持branch                                                   | StorageEngine | QA       | neon branching功能：neon branching 分支、租户、计算节点管理功能验证，数据隔离性验证<br/>openGauss数据库功能验证：覆盖数据库常用模块的Yat用例测试<br/>可靠性：重复启停计算节点，启动计算节点后数据查询正常 |
| 25   | 分布式场景支持灵活扩缩容，支持数据自动重平衡                 | StorageEngine | QA       | 功能测试：重分布函数入参测试、分布式集群扩缩容后调用重分布函数<br/>资料测试：函数使用方法及约束资料是否正确 |
| 26   | openGauss性能基线                                            | SQL           | QA       | 性能测试：分别测试openGauss+Intel 192核服务器TPCC、openGauss+HG 7490服务器TPCC和openGauss+鲲鹏新型号7280Z服务器TPCC值 |
| 27   | 鲲鹏950DPA适配                                               | StorageEngine | QA       | 功能验证：测试VecHashAgg算子支持走DPA加速流程结果、key列和agg列支持与不支持类型及聚集函数验证<br/>性能验证：开启硬件加速与不开启，3条sql10次执行时间求平均值，时间优化下降是否大于等于20%<br/>升级验证：升级到高版本DPA加速功能是否可用、回归到低版本是否无uadk_path和enable_dpa_hashagg参数<br/>资料验证：验证资料描述及示例正确 |
| 28   | 【HW】【兼容性组】D库语法兼容-DDL语法兼容2-part1             | SQL           | QA       | 基本语法功能验证：基本语法功能及在复杂场景下的验证<br/>兼容验证：在非兼容D库下执行用例，验证功能是否支持，数据库正常运行<br/>升级验证：从7.0.0RC1、7.0.0RC2升级到7.0.0RC3之后执行用例验证功能是否正确<br/>导入导出验证：在兼容D库使用本特性语法导入导出功能是否正常<br/>驱动测试：使用jdbc驱动验证插件相关功能 |
| 29   | 【HW】【兼容性组】D库语法兼容-DDL语法兼容2-part2             | SQL           | QA       | 基本语法功能验证：基本语法功能及在复杂场景下的验证<br/>兼容验证：在非兼容D库下执行用例，验证功能是否支持，数据库正常运行<br/>升级验证：从7.0.0RC1、7.0.0RC2升级到7.0.0RC3之后执行用例验证功能是否正确<br/>导入导出验证：在兼容D库使用本特性语法导入导出功能是否正常<br/>驱动测试：使用jdbc驱动验证插件相关功能 |
| 30   | 向量数据库适配LSG算法                                        | StorageEngine | QA       | 性能：HNSW_LSG索引性能通过annbenchmark工具，验证SIFT和GIST数据集，在召回率大于99%的情况下，与原生HNSW对比，全并发HNSW_LSG的QPS均为HNSW的2x+<br/>升级：验证存在LSG功能的场景下升级、回滚功能正常<br/>资料：验证涉及资料正确，可正常指导用户进行LSG相关功能加载、使用过程参数选项配置等 |
| 31   | python驱动支持鲲鹏RAG多模融合检索接口                        | AI            | QA       | 基本功能验证：接口功能在基础使用场景下是否生效<br/>资料验证：文档资料是否完整 |
| 32   | MCP服务器工具增强                                            | AI            | QA       | 部署使用：底座数据库&MCP服务在同节点、跨节点使用<br/>文档检索：在LLM下发需搜索信息，mcp工具使用、返回结果验证<br/>操作数据库：LLM调用不同的MCP工具，落库数据正确、查询正确<br/>AI记忆系统：LLM记忆偏好信息落入正确表中，增删改查正确 |
| 33   | RAG Cache-Blend KVCache融合推理加速                          | AI            | QA       | 功能测试：KV Cache向量化写入测试；Cache元数据测试 全计算/动态check layer/cake kv+overwrite；动态Check Layer测试；CAKE KV + OVERWRITE测试<br/>性能测试：长序列LLM问答（6500tokens）查询时间2s；执行相似查询，缓存命中，日志刷新且查询时间降低,10%；重启大模型，清理缓存后，执行相似查询，测试数据库缓存对性能提升<br/>可靠性测试：40并发下进行查询，查询结果不受影响 |
| 34   | 传统主备性能优化-支持代码段大页安装部署                      | Tools         | QA       | 基本功能验证：基本功能在基础使用场景下是否生效<br/>性能验证：该特性能否满足需求提出的性能要求<br/>升级验证：7.0.0RC2->7.0.0RC3 & 6.0.0->7.0.0RC3<br/>CI验证：SQL & TOOLS测试套选取部分进行连跑测试 |
| 35   | 支持openGauss迁移openGauss                                   | Tools         | QA       | 数据迁移测试：数据类型 -> 表 -> 索引 -> 数据库对象<br/>迁移工具测试：表 -> 主键 -> 外键 -> 索引 -> 序列 -> 存储过程 -> 视图 -> 函数 -> 触发器<br/>不同版本间迁移：5.0.5传统OM集群迁移至7.0.0-RC3资源池化集群 -> 6.0.3资源池化集群迁移至7.0.0-RC3传统OM集群 |
| 36   | openGauss完成鲲鹏对位友商性能基线及竞争力分析                | SQL           | QA       | 性能调优：鲲鹏920 V200和海光服务器性能调优，测试性能分数及跑分过程对应CPU/IO/网络负载变化是否满足需求目标 |
| 37   | 【海量数据】段页式支持空间回收                               | StorageEngine | QA       | gs_table_shrink函数：1.基本功能测试；2.入参测试；3.权限验证；4.索引测试；5.分区表测试；6.主备表现<br/>gs_space_shrink_compact函数：1.基本功能测试；2.入参测试；3.权限验证；4.主备表现<br/>高可用测试：1.结束进程；2.重启集群；3.主备切换<br/>高并发测试：1.业务并发时分别执行2个函数；2.业务并发时并发执行2个函数；3.业务并发时并发执行2个函数和vacuum；4.业务并发时<br/>性能测试：1.插入1亿条数据后多次删除数据；2.分别执行2个函数<br/>升级测试：升级后执行函数查看功能 |
| 38   | 【海量数据】支持全局临时表和本地临时表的使用（#table_name，##table_name） | SQL           | QA       | 支持#符号或##符号开头的临时表功能：支持本地临时表（#开头）和全局临时表（##开头）按需求描述实现，包括创建、删除、修改、DML操作、索引、约束、视图、存储过程、权限、导入导出等场景<br/>资料测试：社区文档相关内容验证<br/>升级测试：升级版本需求可用，功能正常 |
| 39   | 【海量数据】支持pg_matviews视图                              | SQL           | QA       | pg_matviews视图功能测试：验证视图各字段的准确性和与物化视图操作的同步性。<br/>pg_matviews视图兼容性测试：验证在不同数据库兼容模式下的行为。<br/>pg_matviews视图权限与安全测试：验证视图的只读保护与用户创建权限控制。<br/>pg_matviews视图升级测试：验证版本升级与回滚后视图的可用性。 |
| 40   | 【海量数据】postgreSQL兼容性-支持WITH ORDINALITY 语法        | SQL           | QA       | with ordinality功能测试：1、新增语法验证2. with ordinality带不带别名验证3. with ordinality与各种返回多行函数一起使用4. with ordinality出现在ddl、dml及dql中5. with ordinality出现在不同位置6. 不支持场景验证<br/>兼容性测试：分别在A/B/C/PG库中执行<br/>升级测试：升级后再次执行如上用例<br/>资料测试：资料描述准确无误 |
| 41   | 【海量数据】postgreSQL兼容性—支持alter sequence rename       | SQL           | QA       | 序列重命名功能：使用序列重命名，验证结果<br/>调用场景：各种场景使用序列重命名<br/>升级验证：升级之后执行用例验证功能是否正确<br/>资料验证：资料描述与需求相符，示例正确无误 |
| 42   | 【南大通用】支持Xmltable                                     | SQL           | QA       | 功能测试：针对passing xmldata的多种形式，CLOUNMS的不同情况，path的正确形式和异常情况以及基础的解析还有扩展情况<br/>性能测试：表中插入大量数据后，进行解析<br/>兼容性测试：在B库和PG兼容库使用xmltable函数<br/>升级测试：从不支持xmltable函数的版本升级到支持xmltable函数的版本，升级成功且函数功能正常 |
| 43   | 【南大通用】支持GMS_XMLPARSER高级包                          | SQL           | QA       | 功能测试：通过执行SQL查看PARSE返回和信息是否合理；通过执行SQL查看PARSEBUFFER返回和信息是否合理；通过执行SQL查看NEWPARSER返回和信息是否合理；通过执行SQL查看PARSECLOB返回和信息是否合理；通过执行SQL查看FREEPARSER返回和信息是否合理；通过执行SQL查看GETDOCUMENT返回和信息是否合理<br/>升级验证：7.0.0RC3.B017升级到7.0.0RC3.B020创建gms_xmlparser插件，测试函数基本功能<br/>资料验证：企业版及lite版验证资料描述及示例 |
| 44   | 【南大通用】支持sqlserver exec执行语法                       | SQL           | QA       | 基本功能测试：通过exec执行各类函数和存储过程（标量、非标量、带DDL/DML/游标/循环/动态SQL/事务控制等），验证执行结果正确性<br/>参数入参测试：以整型、浮点、布尔、字符串、日期、空值等不同类型参数执行存储过程，验证数据插入和返回值正确性<br/>异常场景测试：验证参数个数不匹配、类型不匹配、对象不存在等情况下exec返回错误信息是否合理<br/>特殊场景测试：在集群重启、升级后、事务中执行exec，验证功能逻辑一致性<br/>兼容性测试：在A/B/C/PG兼容模式下执行exec，验证语法是否被拒绝（仅D模式支持）<br/>资料验证：验证产品文档中exec语法的描述和示例是否正确、可执行 |
| 45   | 【南大通用】支持sqlserver set XACT_ABORT ON 语法             | SQL           | QA       | 功能测试：1、事务中所有sql全部执行成功2、事务中所有sql全部执行失败3、存储过程的使用4、try...catch中的使用5、嵌套事务6、死锁场景7、并发场景多个会话中启动多个事务<br/>升级验证：7.0.0RC3.B018升级到7.0.0RC3.B020，升级带D库，升级后参数正常可用<br/>资料验证：企业版及lite版验证资料描述及示例 |
| 46   | 【神舟通用】支持 long 和 long raw 类型(A库)                  | SQL           | QA       | 功能测试：作为表字段的类型：行存普通表、分区表、local临时表、global临时表；作为表字段的类型：列存普通表、分区表、local临时表、global临时表；修改表字段类型：行存普通表、分区表、临时表其他类型修改为long；修改表字段类型：列存普通表、分区表、临时表其他类型修改为long；修改表字段类型：行存普通表、分区表、临时表long修改为其他类型；修改表字段类型：列存普通表、分区表、临时表long修改为其他类型；删除表字段：行存普通表、分区表、临时表增加列删除列；删除表字段：列存普通表、分区表、临时表增加列删除列；数据合法性：正常字符、中文、混合字符；数据合法性：特殊符号；数据合法性：number类型，0、正数、负数；数据合法性：插入空值；插入合理值，结合条件查询：select like、between；隐式转换：to_date、char、varchar、number；表中多给字段使用类型；基于表，创建视图，查询视图；PL中使用类型；查询系统表、系统视图，如表定义、列定义等<br/>兼容性测试：本需求兼容A、PG模式<br/>特性耦合：驱动JDBC验证，Yat工具执行自动化测试用例；逻辑备份、逻辑恢复<br/>资料验证：资料描述清晰，无错字，示例正确可执行<br/>升级测试：完美升级，升级版本，功能可用；完美回退，回退版本，功能不可用 |
| 47   | 【神舟通用】支持jsonb_concat函数和\|\|操作符连接jsonb功能    | SQL           | QA       | 功能测试：键值连接键值；键值连接数组；键值连接标量；数组连接数组，连接相同维度、不同维度数组；数组连接标量；标量连接标量；jsonb_concat不拼接，返回单个数值；执行方式：select调用、入参为表的列、PL中调用；数据含重复值（键值、数组、标量）；数据含空值（NULL，{}，[]）；函数嵌套连续拼接jsonb_concat(jsonb_concat(),[])；函数无入参jsonb_concat()；异常：函数入参非jsonb类型<br/>兼容性测试：本需求兼容PG模式<br/>特性耦合：驱动JDBC验证，Yat工具执行自动化测试用例<br/>资料验证：资料描述清晰，无错字，示例正确可执行<br/>升级测试：完美升级，升级版本，功能可用；完美回退，回退版本，功能不可用 |
| 48   | 【神舟通用】支持 jsonb_path_exists ， jsonb_path_query_first 函数(A库) | SQL           | QA       | 功能测试：基础语法验证；验证jsonpath运算符；验证jsonpath过滤条件is unknown，like_regex，starts_with，exists；验证jsonpath递归通配符；验证函数规则为严格/不严格模式；验证算数操作符；验证函数表达式；验证类型转换函数；异常验证；验证函数可选参数-vars；验证函数可选参数-silent；验证边界/空值场景；验证在PL中调用函数<br/>兼容性测试：本需求兼容PG模式<br/>特性耦合：驱动JDBC验证，Yat工具执行自动化测试用例<br/>升级测试：完美升级，升级版本，功能可用；完美回退，回退版本，功能不可用<br/>资料验证：资料描述清晰，无错字，示例正确可执行 |
| 49   | 【神舟通用】支持json以及jsonb类型转换为 bool类型、 数值类、字符类、时间类、数组类 | SQL           | QA       | 功能测试：select cast() 函数，直接传入参数执行转换；select cast() 函数，传入表的列执行转换；select cast() 函数，转换后作为查询条件；在PL中执行转换；异常：数据超过目的类型的精度；异常：转换成不支持的目的类型，如blob\xml等；异常：目的键名与源键名不匹配；键名大小写敏感；键名含有特殊符号；边界：数据为数值类型、日期类型边界；数据：带有双引号和不带有双引号；数据：空值、空串<br/>兼容性测试：本需求兼容A、PG模式<br/>特性耦合：驱动JDBC验证，Yat工具执行自动化测试用例<br/>资料验证：资料描述清晰，无错字，示例正确可执行<br/>升级测试：完美升级，升级版本，功能可用；完美回退，回退版本，功能不可用 |
| 50   | 【神舟通用】支持ALTER TABLE ADD COLUMN [IF NOT EXISTS]语法   | SQL           | QA       | 功能测试：带IF NOT EXISTS，新增不存在的字段；带IF NOT EXISTS，新增存在的字段；带IF NOT EXISTS，新增多个字段，col1、col2均不存在；带IF NOT EXISTS，新增多个字段，col1、col2均存在；带IF NOT EXISTS，新增多个字段，col1存在、col2不存在；字段名特殊字符；字段名大小写敏感；字段名为关键字；不带IF NOT EXISTS，原有功能正常执行<br/>安全：对无表修改权限的用户执行该语法<br/>兼容性测试：本需求兼容PG模式<br/>特性耦合：驱动JDBC验证，Yat工具执行自动化测试用例<br/>资料验证：资料描述清晰，无错字，示例正确可执行<br/>升级测试：完美升级，升级版本，功能可用；完美回退，回退版本，功能不可用 |
| 51   | 【邮储】支持updatexml与EXTRACTVALUE函数                      | Plugin        | QA       | 功能测试：功能测试updatexml和extractvalue函数mysql官方支持案例测试<br/>性能测试：连续不间断调用updatexml和extractvalue函数累计两百万次<br/>升级测试：从7.0.0RC2升级到7.0.0RC3之后执行用例验证功能是否正确 |

# 3、版本概要测试结论

openGauss 7.0.0 RC3版本整体测试按照release-manager团队的计划，版本测试规划采取18+3+3的测试方式，即18个B版本进行系统测试+3个B版本进行集成验证（1个B版本并行需求测试）+2个3版本进行回归测试的策略，实际完成了19个B版本的系统测试+3个B版本的集成验证+并行6个B版本的回归测试：

版本迭代1：对应openGauss 7.0.0 RC3 B001版本至openGauss 7.0.0 RC3 B003版本。主要进行D库兼容性需求验收及问题单验收，开展一轮全量CI测试和分析优化。

版本迭代2至版本的迭代4：对应openGauss 7.0.0 RC3 B004版本至openGauss 7.0.0 RC3 B017版本。该阶段主要进行新需求测试设计、验收和issue回归；开展多轮全量功能CI连跑、分析和优化；持续进行专项能力测试，重点覆盖性能、长稳、可靠性、升级和安全测试，并对已修复的问题单进行回归和对已验收完成特性测试报告进行评审。B016至B017版本，联合伙伴部分内核及兼容性需求转测，伙伴负责测试设计和验证。

openGauss 7.0.0 RC3 B018版本至openGauss 7.0.0 RC3 B019版本主要转测联合伙伴需求和少量内部需求，该阶段进行历史需求收尾和新需求验收，同时评审和优化已完成需求测试报告。

集成测试阶段：对应openGauss 7.0.0 RC3 B019版本至openGauss 7.0.0 RC3 B021版本开展全量集成测试，覆盖工具链、兼容性、内核加速、资源池化等继承能力测试，涉及功能、可靠性、安全、性能、长稳、升级、资料领域，累计执行测试用例7.5w+个，并覆盖两轮7*24H长稳测试，保证社区版本基础功能正常、稳定性良好，期间对每个特性的测试报告进行评审优化。openGauss 7.0.0 RC3 B019版本至openGauss 7.0.0 RC3 B024版本集中验收问题单，例行展开自动化测试，对关键指标、功能进行看护，防止修改引入新的问题。

openGauss 7.0.0 RC3版本按照测试策略完成了全量功能验证和专项测试（性能、可靠性、稳定性、兼容性、安全和资料等），所有测试任务均按计划完成。本版本计划交付需求51个，实际交付51个，交付率100%，所有发布需求均验证通过。

openGauss 7.0.0 RC3版本共发现有效问题715个。修复问题回归测试结果正常，版本整体质量良好。遗留1个问题，详见附件1遗留问题列表。

# 4、版本详细测试结论

openGauss 7.0.0 RC3版本详细测试内容包括：

1、通过自动化和手工用例测试看护，从数据库服务、数据库运维管理、数据库备份恢复、数据库兼容性、dataKit功能看护、资源池化存储能力、黑匣子安全能力、内核加速数据库优势特性、向量数据库、系统性能、系统可靠性11个维度进行openGauss继承特性测试，继承功能无丢失。

2、在内核场景化的竞争力构建上，向量数据库支持RabitQ索引、适配LSG算法和推理加速，支持oGAI、MCP及neno branchind能力，在AI适配、向量数据库检索性能更优上持续发力；在数据库高可用方面，如提供支持在线DDL和多写SQL引擎能力增强特性；持续提升openGauss数据库高可靠性高安全，提供黑匣子支持主备参数同步和密钥同步能力。测试覆盖上述需求，关注功能的实现和规格的达成。但后续仍需从产品可靠性、竞争力的角度出发，持续进行加固测试和关键指标验证。

3、在兼容性和数据库服务能力提升上，社区共交付16个需求涵盖D兼容性、B兼容性和PG兼容性，提升openGauss在语法、函数、视图等方法兼容能力；当然各兼容库兼容性需要持续提升。在工具链方面，向量数据库迁移工具对接DataKit、openGauss迁移openGauss和openGauss支持热补丁机制等能力，后续仍需对工具的易用性和稳定性提高要求。

4、针对系统的稳定性，进行长稳测试，包括事务并发测试、benchmarksql+sysbench加压测试等，数据库满足7*24H正常运行，测试较为充分，产品稳定性好；

5、专项测试包括性能专项、安全专项、兼容性测试、可靠性测试和资料测试。

## 4.1 特性测试结论

### 4.11 继承特性评价

对产品所有继承特性进行评价，包含9大项51小项，根据集成测试结果和问题遗留，进行质量评估

| Domain           | Feature                 | 质量评估                   | 备注                                                         |
| ---------------- | ----------------------- | -------------------------- | ------------------------------------------------------------ |
| DataKit          | 插件管理                | <font color=green>▮</font> | 测试插件管理模块，关注该模块下各子特性功能正常及界面显示正常 |
|                  | 安全中心                | <font color=green>▮</font> | 测试安全中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 日志中心                | <font color=green>▮</font> | 测试日志中心模块，关注各子特性功能正常及界面显示正常         |
|                  | 资源管理                | <font color=green>▮</font> | 测试资源管理模块，验证资源管理功能，界面显示正常             |
|                  | 安装部署                | <font color=green>▮</font> | 测试安装部署模块，验证安装部署流程正常，界面显示正常         |
|                  | 业务建模                | <font color=green>▮</font> | 测试业务建模模块，关注各子特性功能正常及界面显示正常         |
|                  | 基础运维                | <font color=green>▮</font> | 测试基础运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 智能运维                | <font color=green>▮</font> | 测试智能运维模块，关注各子特性功能正常及界面显示正常         |
|                  | 告警监控                | <font color=green>▮</font> | 测试告警监控模块，关注各子特性功能正常及界面显示正常         |
|                  | 数据迁移                | <font color=green>▮</font> | 测试迁移功能，迁移前后数据一致性校验                         |
|                  | 监控插件工具            | <font color=green>▮</font> | 测试监控插件功能，界面显示正常                               |
|                  | 业务开发                | <font color=green>▮</font> | 测试业务开发模块，关注各子特性功能正常及界面显示正常         |
|                  | 兼容性评估              | <font color=green>▮</font> | 测试兼容性评估模块，关注各子特性功能正常及界面显示正常       |
|                  | 录制回放                | <font color=green>▮</font> | 测试录制回放模块，关注各子特性功能正常及界面显示正常         |
|                  | datakit_安装部署        | <font color=green>▮</font> | 测试安装部署、启停、兼容性                                   |
|                  | datakit_系统可靠性      | <font color=green>▮</font> | 测试系统级长稳、性能、可靠性、内存                           |
|                  | datakit_升级            | <font color=green>▮</font> | 测试平台与含插件升级                                         |
| 数据库服务       | SQL语法                 | <font color=green>▮</font> | 继承已有测试能力，支持DDL/DML/DCL/DQL语句，不同特性组合下用户、权限的验证(含资源池化、兼容性)(表/视图/索引等基础对象，fdw、postgis、物化视图) (含资源池化、兼容性) |
|                  | 功能SQL                 | <font color=green>▮</font> | 继承已有测试能力，vacuum、analyze、explain、事务(含自治事务)、审计、安全&加密、AI特性、密态等值查询、账本数据库、逻辑复制 |
|                  | 主备管理                | <font color=green>▮</font> | 继承已有测试能力，极致RTO、switchover、failover等            |
|                  | guc参数控制             | <font color=green>▮</font> | 继承已有测试能力，不同参数影响sql的执行效果，应该放到各个sql模块云设计；这里仅验证参数生效和组合场景 |
|                  | 内核工具链              | <font color=green>▮</font> | 继承已有测试能力，gs_ctl/gstrace/perctrl/pg_config/pagehack/pg_recvlogic/pg_controldata/pg_xlogdump/pg_resetxlog/gs_restore等 |
|                  | 资源负载管理            | <font color=green>▮</font> | 继承已有测试能力，gs_cgroup验证                              |
| 数据库备份恢复   | 物理备份/恢复           | <font color=green>▮</font> | 继承已有测试能力，支持物理全量/增量备份能力，还原能力，恢复能力，基于时间点恢复能力 |
|                  | 逻辑备份/恢复           | <font color=green>▮</font> | 继承已有测试能力，逻辑备份/还原支持对指定库、指定表、指定一组对象（某个模式所属对象）进行备份及还原 |
|                  | PITR、日志归档          | <font color=green>▮</font> | 继承已有测试能力，全量PITR物理恢复                           |
|                  | 延时备份                | <font color=green>▮</font> | 继承已有测试能力，支持延迟备份                               |
| 数据库管理与运维 | 安装卸载                | <font color=green>▮</font> | 继承已有测试能力，测试数据库安装、卸载全流程                 |
|                  | 升级                    | <font color=green>▮</font> | 测试带业务操作下多升级路径覆盖，升级成功后，特性功能运行正常 |
|                  | 实例管理                | <font color=green>▮</font> | 测试主备高可用(switchover/failover)、重启、启停              |
|                  | 运维视图                | <font color=green>▮</font> | 测试系统表与系统视图、系统schema(如dbe_perf、information_schema、WDR、pldebugger、db4ai等) |
| 数据库兼容性     | 环境兼容                | <font color=green>▮</font> | 测试硬件兼容、操作系统兼容、依赖软件版本                     |
|                  | 驱动兼容                | <font color=green>▮</font> | 测试jdbc/odbc/libpq/psycopg2等 mysql兼容(协议兼容、类型兼容)继承已有测试能力，支持JDBC、ODBC、PDBC、GDBC驱动 |
|                  | 生态兼容                | <font color=green>▮</font> | 测试ORM(mybatis)，连接池(druid)                              |
|                  | mysql兼容性             | <font color=green>▮</font> | 测试SQL语法（单双引号、反引号、关键字、类型、函数、操作符等）、通信协议 |
|                  | A库兼容性               | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | pg兼容性                | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | D库兼容性               | <font color=green>▮</font> | 测试SQL语法、函数、视图、事务等、通信协议                    |
|                  | 资源池化兼容性          | <font color=green>▮</font> | 测试资源池化特性，双集群极致RTO、网络复制双集群、refrom、主备实时一致性 |
| 系统性能         | 系统性能                | <font color=green>▮</font> | 测试2P/4P性能、主备、RTO(含兼容性、资源池化)                 |
| 系统可靠性       | 系统可靠性/长时间稳定性 | <font color=green>▮</font> | 故障注入测试，包含MOT、兼容性、资源池化等内存专项、长稳测试  |
| 资源池化         | reform                  | <font color=green>▮</font> | 测试集群对外提供服务高可用能力                               |
|                  | 极致RTO\按需回访        | <font color=green>▮</font> | 测试主机故障后集群不可用情况快速恢复提供服务的能力           |
|                  | 双集群                  | <font color=green>▮</font> | 测试资源池化双集群容灾能力测试，保障主备集群切换功能正常     |
|                  | 非池化场景能力继承      | <font color=green>▮</font> | 测试包含语法兼容性、工具等用例，保证资源池化场景下非池化能力不丢失 |
|                  | 黑匣子                  | <font color=green>▮</font> | 测试API单接口，API组合场景提供快速恢复数据零丢失能力；覆盖可靠性测试：性能测试 |
| 内核加速         | ustore                  | <font color=green>▮</font> | 测试ustore相关存储、索引、闪回等特性用例，保障ustore功能正常 |
|                  | HTAP                    | <font color=green>▮</font> | 测试行列转换功能及相关GUC参数生效                            |
|                  | 向量索引                | <font color=green>▮</font> | 测试DataVec向量数据库HNSW、IVFFLAT、PQ、DiskANN、RabitQ索引创建、修改、重建及检索模块，覆盖所有索引选项参数及查询检索参数 |
|                  | 全文检索索引            | <font color=green>▮</font> | 测试BM25索引和GIN索引创建、修改、重建及检索模块，覆盖所有索引选项参数及查询检索参数 |
|                  | OGAI                    | <font color=green>▮</font> | 测试在数据库内部直接调用AI模型进行文本向量化、文本生成、文档重排序等操作 |

<font color=red><font color=red>●</font></font>： 表示特性不稳定，风险高

<font color=yellow><font color=yellow>▲</font></font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

### 4.12 新需求评价

建议以表格的形式汇总新特性测试执行情况及遗留问题单情况的评估，给出特性质量评估结论。

| 特性名称                                                     | 测试情况说明                                                 | 约束                                                         | 质量点灯                    | 遗留问题  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | --------- |
| 多写驱动能力增强                                             | 本次测试主要针对特性新增的17个接口进行句柄参数设置测试、句柄功能测试、内存稳定性测试、长期稳定性测试和并发运行稳定性测试，共计22个测试用例，覆盖完全，未发现问题，整体质量良好。 | 1、不涉及性能有关测试                                        | <font color=green>▮</font>  | 无        |
| 参数化路径功能增强                                           | 参数化路径功能增强特性共计执行38个用例，主要覆盖功能测试、升级测试、性能测试和资料测试。功能测试验证自动化参数功能支持select特性、GPC特性和兼容B库，升级测试覆盖升级后功能新增功能可用和视图查询函数字段值变更，性能测试验证支持自动参数化缓存的DML和DQL在开启参数化缓存功能和不开启参数化功能场景执行时间无劣化，资料测试覆盖该需求相关资料验证。用例执行率100%，发现2个有效问题（包含1个资料问题）；整体质量良好。 | 仅支持Insert, Update, Delete(DML)以及含有WHERE的SELECT(DQL)。 仅支持Simple Query Protocol， 不支持Extended Query Protocol。 SQL语句的长度必须小于等于512。 不支持RETURNING关键词。 不支持WITH CLAUSE。 不支持UPSERT CLAUSE。 不支持HINT。 不支持子查询。 不支持ORDER BY关键词。 不支持LIMIT关键词。 不支持函数。 不支持COLLATE关键词。 不支持值的类型转换。 不支持使用NULL关键词。 不支持IGNORE关键词。 不支持DEFAULT VALUES与DEFAULT关键词。 不支持USING关键词。 不支持ON DUPLICATE KEY UPDATE语法。 不支持PARTITION关键词。 不支持包含数组(Array)的语句。 不支持包含隐式类型转换的语句。 当enable_ignore_case_in_quotes = on时，参数化不生效。 当td_compatible_truncation = on时，参数化不生效。 操作分区表，临时表，系统表，压缩表，视图，无日志表的语句不会被参数化。 | <font color=green>▮</font>  | 无        |
| 支持oGAI                                                     | 本报告主要测试新增系统视图的功能及驱动，框架兼容性，共计执行测试用例60个，主要覆盖了功能测试、可靠性测试、升级测试、资料测试等。升级测试在5.0.x,6.0.x版本升级到7.0.0-RC3版本后执行用例验证此功能的正确性。测试输出用例60个，整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 黑匣子特性增强-支持主备参数同步                              | 本报告主要测试基于oGRecorder集群架构中，参数设置及其设置方式、参数生效方式，主要包含oGRecorder提供的参数设置获取接口、grcmd命令、修改conf配置文件，相关参数均覆盖以上3种方式验证参数生效场景，参数同步场景一并对可靠稳定性进行了验证，输出测试用例53，执行测试1轮，发现异常问题5个。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 黑匣子特性增强-支持秘钥同步                                  | 黑匣子特性增强-秘钥同步特定，共执行了13个用例，主要覆盖了功能测试、安全测试、可靠性测试。功能测试主要覆盖黑匣子安装部署，使用grcmd命令生成CA证书，使用grcmd命令生成服务端证书，使用grcmd命令生成客户端证书，进行SSL认证流程，更改grcmd迭代参数是否能进行通信，检查所有节点是否公用一个证书（worm是否同步）。安全测试主要覆盖检查证书文件权限，检查秘钥文件是否全部加密保存。可靠性测试主要覆盖错误的秘钥信息是否能通信，不同根证书生成的客户端服务端证书能否通信，不同文件目录下的客户端服务端证书是否能通信。测试过程中发现1个非本需求引入问题，整体质量良好。 | 需要提供根证书+根证书私钥                                    | <font color=green>▮</font>  | 无        |
| 黑匣子特性增强-支持升级                                      | 本报告主要测试基于oGRecorder服务提供的工具gr_om upgrade验证黑匣子在线升级，包含指定参数校验、拦截、多节点顺序组合场景验证，离线升级包含只升级单一服务（CM/oGRecorder）、全部服务场景验证，可靠性场景测试，输出测试用例50，执行测试1轮，发现异常问题4个，资料问题1个。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 黑匣子特性增强-API优化                                       | 本报告主要测试基于restAPI功能，验证服务端和客户端HTTPS接口的测试，主要包含基本功能、可靠性测试，输出测试用例30个，执行测试1轮，发现6个异常问题，总体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 支持在线DDL+二次增强转测                                     | openGauss在线ddl同步执行dml操作，共执行了54个用例，主要覆盖了功能测试、可靠性测试。需求100%实现。功能测试主要覆盖在线ddl同步执行dml操作，包含传统主备中Astore、段页式的普通表和分区表在线执行修改列数据类型、修改压缩属性、增加表约束(范围约束、非空约束)、分区表分裂合并、VACUUM FULL及CLUSTER语句测试。可靠性测试主要覆盖大量数据测试和数据库意外中断测试。测试过程中发现4个问题，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 灵衢接口变更适配                                             | 本测试完成功能测试、性能测试、可靠性测试，测试详情如下：功能测试：测试变更接口名字，测试SMB工具和failover是否能正确执行。性能测试：failover -f是否能瞬间执行。用例共10个，共计1轮。整体质量良好无漏洞。 | SMB工具需要重命名，令其找环境中的.so文件<br/>主机需要手动kill备机才能执行failover，否则会出现意料之外的情况 | <font color=green>▮</font>  | 无        |
| D兼容性-系统视图兼容                                         | 本报告主要测试新增系统视图的功能及驱动，框架兼容性，共计执行测试用例90个，主要覆盖了功能测试、驱动测试、升级测试、资料测试等。升级测试在7.0.0RC1,7.0.0RC2，6.0.x版本升级到7.0.0-RC3版本后执行用例验证此功能的正确性；兼容性测试覆盖在不同兼容模式验证该功能；驱动测试覆盖使用jdbc驱动和psycopg2驱动。测试输出用例90个，共发现问题5个，其中本需求引入问题3个，非本需求引入问题2个，资料问题3个。整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |
| D兼容性-系统函数兼容                                         | D库系统函数特性测试，共计执行了102个用例，主要覆盖了功能测试、升级测试、兼容性测试、耦合功能测试、资料测试。测试中发现8个问题，7个功能问题，1个资料问题，整体质量良好。 | 在D库中加载shark插件使用 <br />部分函数入参只支持可以隐式转换成入参支持的数据类型 | <font color=green>▮</font>  | 无        |
| B兼容性提升                                                  | 本报告主要测试新增系统视图的功能及驱动，框架兼容性，共计执行测试用例60个，主要覆盖了功能测试、兼容性测试、驱动测试、升级测试、资料测试等。升级测试在5.0.x,6.0.x版本升级到7.0.0-RC3版本后执行用例验证此功能的正确性；兼容性测试覆盖在不同兼容模式验证该功能；驱动测试覆盖使用jdbc驱动和psycopg2驱动。测试输出用例60个，整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 支持热补丁机制                                               | 支持对openGauss中含_thread类型的变量函数进行打补丁操作，制作热补丁过程中支持跳过特定的源文件。功能验证补丁制作安装、opengauss版本、命令语句格式、黑白名单配置。执行23个测试用例，发现1个问题，遗留问题0，整体质量良好。 | 1.支持对openGauss中含__thread类型的变量函数进行打补丁操作<br/>2.制作热补丁过程中，支持跳过特定的源文件<br/>3.仅支持通过upatch-build进行热补丁制作和使用。不支持通过syscare-build进行热补丁制作 | <font color=green>▮</font>  | 无        |
| 多写SQL引擎能力增强-支持启动代价处理                         | oGRAC支持启动代价处理特性，共计执行了19个用例，主要覆盖了功能测试。 功能测试主要覆盖了多种表类型的单表全表扫描、索引扫描、join方式nestloop和hash join、单limit使用、带offset的limit、sort group、多列group、结合聚合函数group的启动代价估算。测试过程未发现问题，整体质量良好。 | 1、不涉及有关distinct的启动代价计算<br/>2、group只支持sort group和hash group<br/>3、依赖有列统计信息和执行计划行数正确信息，依赖已实现的几种join | <font color=green>▮</font>  | 无        |
| 多写SQL引擎能力增强-索引扫描增强（一）                       | 索引增强扫描，index fast full scan和index skip scan适配，验证索引扫描的功能和性能够。主要覆盖了使用索引扫描和不使用或使用其他索引扫描的情况，验证特性合入前后索引扫描cost变化。共执行10个用例，未发现该特性有效问题，整体质量良好。 | 需要提供统计信息，开启cbo模式（默认模式）                    | <font color=green>▮</font>  | 无        |
| 多写SQL引擎能力增强-索引扫描增强（二）                       | 本报告主要测试[not]between,[not] in [!]=true or false [not]like， is [not] null操作符在oGRAC中对计划层的预估rows是否准确，共计执行测试用例31个，主要覆盖了功能测试，整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 多写SQL引擎能力增强-支持动态采样                             | 针对oGRAC支持动态采样特性，执行22个用例，涵盖基本参数设置验证、基本功能验证、内存稳定性验证3个方面，测试过程未发现该特性有效问题，整体质量良好。 | 仅支持普通表、分区表的动态采样；不支持虚拟表、系统表、临时表和双表 | <font color=green>▮</font>  | 无        |
| 多写SQL引擎能力增强-支持rowid scan-存储二组                  | oGRAC支持rowid scan特性，共计执行了7个用例，主要覆盖了功能测试。 功能测试主要包括两方面生成表条件扫描计划和生成索引扫描计划。测试中未发现问题，整体质量良好。 | 仅支持nested loop                                            | <font color=green>▮</font>  | 无        |
| 支持1bit量化索引                                             | 本次测试完成的功能测试、资料测试和性能测试，需求100%实现，所有被测特性的测试用例执行覆盖率达到100%，累计测试用例通过率100%，测试发现问题2个，均已解决验收。 | 1.需要结合HNSW或IVF索引使用<br/>2.精排只支持fp32和sq8类型    | <font color=green>▮</font>  | 无        |
| 支持direct IO、异步IO                                        | openGauss异步直接IO特性, 共计执行了43个用例, 主要覆盖了功能测试, 升级测试, 性能测试, 文档测试。测试中发现4个问题, 总DI为5.1, 代码量1.7K, 缺陷密度3，整体质量良好。 | enable_adio_function = on                                    | <font color=green>▮</font>  | 无        |
| 通过向量数据库完成推理加速                                   | 本次测试通过搭建服务，发现资料问题2处，通过规避手段，成功搭建完整服务，验证功能正常；Dify侧落库数据查看正常，KV Cache层数据落库成功。 | 无                                                           | <font color=green>▮</font>  | 无        |
| ES/milvus迁移工具对接datakit                                 | 本次测试覆盖了datakit适配Elasticsearch和Milvus迁移从添加实例到数据迁移的全流程功能测试、datakit升级测试和资料测试，需求100%实现。本次测试执行测试用例90个，发现一个资料问题，开发质量良好。 | 1.python版本>=3.8<br/>2.Elasticsearch版本>=7.3<br/>3.Milvus版本>=2.3<br/>4.openGauss版本>=7.0.0-RC1<br/>5.openGauss用户具有sysadmin权限 | <font color=green>▮</font>  | 无        |
| 支持branch                                                   | 主要测试neon branching特性功能，测试详情如下：neon branching功能测试：输出测试用例20个;数据库功能测试：执行测试用例5728条需求覆盖100%，用例执行100%，总计发现issue共8个；开发质量一般。 | 不支持Ustore。<br/>当前仅支持单机。                          | <font color=yellow>▲</font> | 详见附件1 |
| 分布式场景支持灵活扩缩容，支持数据自动重平衡                 | 本阶段完成了重分布函数的功能测试、资料测试，需求特性100%实现。本次测试执行用例17个，发现问题1个，开发质量良好。 | 1.CN主节点修改postgresql.conf<br/>password_encryption_type = 1<br/>shared_preload_libraries = 'spq'<br/>enable_pbe_optimization = OFF<br/>2.DN节点修改postgresql.conf<br/>password_encryption_type = 1<br/>shared_preload_libraries = 'spq'<br/>3.DN节点修改pg_hba.conf<br/>host	all	all	CN节点IP/32	trust<br/>4.所有节点都需要执行sql：<br/>create extension spq; | <font color=green>▮</font>  | 无        |
| openGauss存储引擎性能优化                                    | 本需求测试覆盖2P单机A库普通表tpcc优化后提升4w+，4P单机B库普通表TPCC优化后提升4w+。覆盖场景2个，优化后满足性能指标，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| openGauss性能基线                                            | 本需求测试结果：openGauss+Intel 192核服务器TPCC 247w、openGauss+HG 7490服务器TPCC 176w、openGauss+鲲鹏9207280Z服务器tpcc298w，覆盖三个场景，满足需求指标，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 鲲鹏950DPA适配                                               | 鲲鹏950 DPA适配特性共计执行22个用例，主要覆盖功能测试、性能测试、升级测试和资料测试。功能测试验证key列和agg列支持DPA加速功能，性能测试验证vecHashAgg算子sql执行时间开启DPA加速较软算提升20%性能，升级测试覆盖升级后功能可用/回退后无相关guc参数，资料测试覆盖该需求相关资料验证。用例执行率100%，发现3个有效问题（包含1个资料问题）；整体质量良好。 | 1、依赖DPA硬件算子支持，仅适用于部署DPA的环境<br/>2、数据类型约束：key列varchar支持长度最大30B，char类型长度最大32B；仅key列支持char和varchar类型，且数量不超过5列<br/>3、列数量约束：key列最多9列，agg列最多9列 | <font color=green>▮</font>  | 无        |
| 【HW】【兼容性组】D库语法兼容-DDL语法兼容2-part1             | D兼容性-DDL语法兼容2 PART1特性，共计执行了47个用例，主要覆盖了功能测试、升级测试、兼容性测试、耦合功能测试、资料测试。测试中发现2个问题，1个功能问题，1个资料问题，整体质量良好。 | 1、在D库中加载shark插件使用 <br />2、当ENABLE_MULTIPLE_NODES开启时，不能使用创建constraint支持ASC/DESC选项。仅当查询计划走index scan时表现与D原生库一致 <br />3、设置参数d_format_behavior_compat_options = 'enable_sbr_identifier'，数据类型可用[]包裹 | <font color=green>▮</font>  | 无        |
| 【HW】【兼容性组】D库语法兼容-DDL语法兼容2-part2             | D兼容性-DDL语法兼容2 PART2特性，共计执行了55个用例，主要覆盖了功能测试、升级测试、兼容性测试、耦合功能测试、资料测试。 | 在D库中加载shark插件使用                                     | <font color=green>▮</font>  | 无        |
| 向量数据库适配LSG算法                                        | 本报告主要测试DataVec向量数据库HNSW_LSG索引，测试详情如下：功能测试：影响原生HNSW功能发现issue1个；性能测试：百万数据集，HNSW_LSG相比原生HNSW性能提升达到2倍以上，达到验收标准；升级测试：验证LSG功能具备场景下升级提交及回滚功能正常；资料测试：覆盖企业版、轻量版，无问题。用例10个，共计1轮，总计发现issue共1个；开发质量良好。 | 1、LSG特性暂时只支持HNSW索引。<br/>2、LSG特性暂时只支持vector数据类型。<br/>3、在创建LSG索引前需要先插入数据，无数据情况下会创建失败。<br/>4、不支持和PQ以及RabitQ等量化方法一起使用，否则会有报错提示。 | <font color=green>▮</font>  | 无        |
| python驱动支持鲲鹏RAG多模融合检索接口                        | 本报告主要测试Python支持多模态融合检索特性功能，共计执行了用例174个，主要覆盖了功能测试。 功能测试主要覆盖了统一多模数据治理接口，其中包括表相关接口：create_table、drop_table、list_tables、describe_table；索引相关接口：create_index、drop_index、list_indexes； DML接口insert、update、delete、query；多模态融合检索接口：vector_search、fulltext_search、hybrid_search；AI 模型服务接口：embed、rerank、chat.测试中发现了1个问题，代码量3K，缺陷密度0.33，相关问题单正在修改中，整体质量良好. | 无                                                           | <font color=green>▮</font>  | 无        |
| MCP服务器工具增强                                            | 本次测试分别搭建openGauss底座数据库、openGauss MCP服务。客户端选用vscode插件cline使用deepseek模型协议使用openGauss MCP服务。覆盖了跨节点交互使用、向量检索、混合查询等场景，验证了MCP服务在openGauss底座数据库上使用。所有接口单元测试通过，个别交互场景存在异常，提单2个缺陷，整体测试通过，质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| RAG Cache-Blend KVCache融合推理加速                          | 本报告主要测试KVCache融合推理加速对性能的提升，共计执行测试用例12个，主要覆盖了功能测试，性能测试，资料测试,可靠性测试。主要验证在长序列LLM问答下的性能提升，相似问答下 KV复用查询时间减少。测试输出用例12个，整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 传统主备性能优化-支持代码段大页安装部署                      | openGauss支持二进制代码段大页特性, 共计执行了14个用例, 主要覆盖了功能测试, 性能测试, 升降级测试, CI测试, 资料测试。测试中发现2两个问题, 总DI为0.2, 代码量0.5k, 缺陷密度0.4, 整体质量良好。 | 1、xml文件中添加`<PARAM name="enableHugeBin" value="on"/>` 2、root用户下执行预安装 | <font color=green>▮</font>  | 无        |
| 支持openGauss迁移openGauss                                   | 本次测试覆盖opengauss数据库的数据类型迁移测试，表类型迁移测试，索引迁移测试，对象迁移测试，以及迁移工具参数测试，所有被测特性的测试用例的累计执行177个用例，执行覆盖率达到100%，发现问题19个，开发质量一般。 | 数据库需要配置白名单                                         | <font color=yellow>▲</font> | 无        |
| openGauss完成鲲鹏对位友商性能基线及竞争力分析                | 本次测试覆盖鲲鹏920新型号之间性能对比、鲲鹏920新型号和海光服务器性能对比，测试性能分数、跑分过程对应CPU\IO\网络负载变化，性能提升满足需求指标。覆盖7组场景对比，满足特性要求，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【海量数据】段页式支持空间回收                               | openGauss段页式相关函数测试，共计执行用例31个，包含gs_table_shrink、gs_space_shrink_compact2个函数。主要覆盖了功能测试、参数校验、性能测试，高可用测试、高并发测试、升级测试，发现4个缺陷问题，无遗留风险；整体质量良好。 | 1.gs_table_shrink仅支持段页式表，非段页式表报错不支持。<br/>2.gs_table_shrink仅支持普通表和分区表，二级分区报错不支持。<br/>3.系统恢复期间、资源池化备机、备集群等场景不允许执行本操作。<br/>4.gs_table_shrink、gs_space_shrink_compact系统函数需对表上最高级别排他锁，会影响此时表上的相关业务，业务场景中需寻找合适时机执行本操作。<br/>5.gs_space_shrink_compact可能会移动末尾非空extent，从而导致相关索引可能失效，需谨慎使用，建议段页式索引与业务表放在不同表空间管理。 | <font color=green>▮</font>  | 无        |
| 【海量数据】支持全局临时表和本地临时表的使用（#table_name，##table_name） | openGauss的D兼容模式下，实现支持D语法中以#符号（本地临时表）或##符号（全局临时表）开头的临时表功能。共计执行36个用例，主要覆盖创建表（CREATE TABLE、SELECT INTO）、修改表（ALTER TABLE）、删除表（DROP TABLE）等语法，对临时表进行DML操作（增删改查），测试了索引（唯一、btree、hash、全文、部分、表达式、gin、gist）、列字段约束（主键、唯一、btree、check、null、not null，不支持外键）、视图（普通视图，不支持全量/增量物化视图）、触发器（不支持）、存储过程（创建单个/多个临时表、匿名块、动态修改临时表）、系统表、用户权限、帮助信息（\h create table）、导入导出（gs_dump/restore/gsql）等关联场景。结合常用测试场景，通过手工执行用例，发现次要问题5个，问题单主要分布在存储过程相关场景，整体质量良好。 | 1.该功能在openGauss的D兼容模式下使用，要有shark插件。<br/>2.不支持全量/增量物化视图。<br/>3.不支持触发器。 | <font color=green>▮</font>  | 无        |
| 【海量数据】支持pg_matviews视图                              | openGauss的pg兼容模式下，实现支持pg_matviews视图功能，共计执行26个用例，主要覆盖了物化视图与增量物化视图的视图创建、索引状态跟踪、表空间信息、ALTER操作同步、删除清理、权限控制、模式兼容性、升级回退等全部设计场景，通过手工执行用例，测试过程中未发现缺陷，所有功能行为符合设计预期。整体质量良好。 | ALTER MATERIALIZED VIEW ... SET TABLESPACE操作当前版本不支持。执行该语句不会修改物化视图的物理存储位置，`pg_matviews`中的 `tablespace`字段保持不变。此为已知设计限制。ALTER MATERIALIZED VIEW ... SET SCHEMA操作当前版本不支持。无法通过DDL语句更改物化视图所属的模式，`pg_matviews`中的 `schemaname`字段保持不变。此为已知设计限制。 | <font color=green>▮</font>  | 无        |
| 【海量数据】postgreSQL兼容性-支持WITH ORDINALITY 语法        | 共计执行18个用例，主要覆盖了功能测试、语法测试、兼容性测试、升级测试和资料测试，未发现问题，无遗留风险，整体质量良好。 | （1）使用时有如下约束：对于with ordinality与rows from一起使用不支持，与返回record类型的函数一起使用不支持，与unnest参数为多个array一起使用不支持。 | <font color=green>▮</font>  | 无        |
| 【海量数据】postgreSQL兼容性—支持alter sequence rename       | 本次特性测试主要测试 PG兼容性-支持序列重命名 ,主要覆盖了功能测试、升级测试、兼容性测试、资料测试;输出测试用例8个,发现issue共0个，整体质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【南大通用】支持Xmltable                                     | opengauss兼容A库下支持xmltable函数，共计执行35条用例，主要覆盖功能测试，性能测试和不同数据库的兼容性测试以及升级测试。在功能测试中发现了两个问题，主要涉及嵌套标签的解析以及多行相同标签数据的解析，问题已提单。整体质量良好。 | 只兼容A库                                                    | <font color=green>▮</font>  | 无        |
| 【南大通用】支持GMS_XMLPARSER高级包                          | openGauss 数据库常用插件gms_xmlparser ，共计执行了53个用例，主要覆盖了功能测试、资料测试。功能测试覆盖了插件函数的基本功能，第一，PARSE函数将XML字符串解析为XML数据类型，并关联到之前创建的解析上下文的测试验证。第二，NEWPARSER函数返回一个新的解析器实例的测试验证。第三，FREEPARSER函数释放解析器对象的测试验证。第四，PARSEBUFFER函数解析给定BUFFER的XML文件的测试验证。第五，PARSECLOB函数解析给定的CLOB的XML文件的测试验证。第六，GETDOCUMENT函数获取DOM文件的测试验证。测试中发现1个问题，整体质量良好。 | 1、仅支持在创建gms_xmlparser插件后使用<br/>2、parse,newparser,freebuffer,parserclob,parserbuffer,getdocument相关函数接口 | <font color=green>▮</font>  | 无        |
| 【南大通用】支持sqlserver exec执行语法                       | 支持sqlserver exec执行语法特性，共计执行37个用例，主要覆盖了基本功能语法测试和特殊场景测试（升级、重启集群、其他兼容模式测试、在事务中执行、在存储过程中嵌套调用），共提交bug单1个，无遗留风险，整体质量良好。 | 仅支持在创建shark插件后使用                                  | <font color=green>▮</font>  | 无        |
| 【南大通用】支持sqlserver set XACT_ABORT ON 语法             | 支持sqlserver set XACT_ABORT ON 语法特性，共计执行19个用例，主要覆盖了兼容模式，并发场景，死锁场景，嵌套事务，事务场景，存储过程，升级场景等，无遗留风险，整体质量良好。 | 在opengauss数据库中，需要创建shark插件。                     | <font color=green>▮</font>  | 无        |
| 【神舟通用】支持 long 和 long raw 类型(A库)                  | 本需求在测试阶段完成了功能测试、特性耦合分析、兼容性测试、资料验证、升级测试。共执行70个自动化用例，需求100%实现，所有被测特性的测试用例的累计执行覆盖率达到100%，无遗留问题。测试质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【神舟通用】支持jsonb_concat函数和操作符连接jsonb功能        | 本需求在测试阶段完成了功能测试、特性耦合分析、兼容性测试、资料验证、可服务性测试、升级测试。共执行20个测试用例（20个自动化测试用例），需求100%实现。所有被测特性的测试用例的累计执行覆盖率达到100%，无遗留问题。测试质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【神舟通用】支持 jsonb_path_exists ， jsonb_path_query_first 函数(A库) | 本需求在测试阶段完成了功能测试、特性耦合分析、兼容性测试、升级测试、资料测试。共执行12个自动化测试用例，需求100%实现，所有被测特性的测试用例的累计执行覆盖率达到100%，无遗留问题。测试质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【神舟通用】支持json以及jsonb类型转换为 bool类型、 数值类、字符类、时间类、数组类 | 本需求在测试阶段完成了功能测试、特性耦合分析、兼容性测试、资料验证、升级测试。共执行12个自动化测试用例，需求100%实现，所有被测特性的测试用例的累计执行覆盖率达到100%，无遗留问题。测试质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【神舟通用】支持ALTER TABLE ADD COLUMN [IF NOT EXISTS]语法   | 本需求在测试阶段完成了功能测试、特性耦合分析、兼容性测试、资料验证、安全权限验证、可服务性测试、升级测试。共执行12个测试用例（11个自动化用例，1个手动用例），需求100%实现。所有被测特性的测试用例的累计执行覆盖率达到100%，无遗留问题。测试质量良好。 | 无                                                           | <font color=green>▮</font>  | 无        |
| 【邮储】支持updatexml与EXTRACTVALUE函数                      | 本报告主要测试updatexml和extractvalue函数，共计执行测试用例330个，主要覆盖了功能测试、性能测试、升级测试、资料测试等。升级测试在7.0.0-RC2版本升级到7.0.0-RC3版本后执行用例验证此功能的正确性；测试输出用例330个，整体测试质量优秀。 | 无                                                           | <font color=green>▮</font>  | 无        |

<font color=red>●</font>： 表示特性不稳定，风险高

<font color=yellow>▲</font>： 表示特性基本可用，遗留少量问题

<font color=green>▮</font>： 表示特性质量良好

## 4.2 专项测试结论

### 4.21 安全测试

openGauss 7.0.0 RC3版本安全测试覆盖：

1、通过工具进行端口扫描/主机漏洞扫描/开源软件漏洞扫描/安全编译/安全配置/密码和信息泄漏/网络安全红线/安全资料/病毒扫描/敏感信息扫描。

2、数据库安全用例CI连跑；针对地址消毒，结合相关测试工具，对memcheck版本执行全量测试用例。

3、从数据库权限管理、数据库连接配置、数据库文件目录安全、数据库安全认证配置、数据库账号口令管理、数据库审计、数据库日志配置、数据库运行环境配置和数据库其他配置进行安全测试。

4、完成安全编码规范扫描，PR满足合入要求；开发白盒安全检视，问题修复后测试回归。

5、使用ICSL官方策略对数据库、工具链进行BAS扫描并分析整改，问题修复后测试回归。

6、使用SQLSmith对转测版本进行安全测试。

7、通过openGauss白盒扫描规则集扫描sql注入、仿冒对象、越权、敏感信息、命令注入等安全问题，并分析整改。

openGauss 7.0.0 RC3版本所有适用的安全扫描和安全测试均已执行，整体质量良好，风险可控。

### 4.22 可靠性\稳定性测试

openGauss 7.0.0 RC3版本可靠性\稳定性测试覆盖：硬件故障/操作系统故障/数据库系统故障/人为因素故障/RTO/工具等6个故障注入类测试及6个长时间负载测试。

1、故障注入类测试：在x86+Centos，x86openEuler环境下共计执行879个用例12轮次，测试用例累计执行率100%，测试发现问题已全部优化闭环，整体质量良好。

| Domain         | 测试内容                                                     | 测试结论 |
| -------------- | ------------------------------------------------------------ | -------- |
| 硬件故障       | 注入CPU、内存、网络故障时无可靠性问题。磁盘满异常时，有有效提示，并且消除故障后数据库可恢复正常。 | 测试通过 |
| 操作系统故障   | 修改系统时间（夏令时，闰年）无可靠性问题。端口、文件句柄、信号量故障时，有有效提示，且故障消除后数据库可恢复正常。 | 测试通过 |
| 数据库系统故障 | 双机故障、事务管理、数据库进程故障消除后，数据库可恢复正常，且有有效日志记录。大量执行SQL、TPCC高并发、数据库参数调整后对数据库无影响。 | 测试通过 |
| 人为因素故障   | 人为破坏系统表、业务执行过程中启停数据库，会有对应日志记录，并且消除故障后数据库可恢复正常。 | 测试通过 |
| RTO            | 注入磁盘满故障、数据库进程异常时，有有效提示，且消除故障后RTO模式下数据库运行正常。主备频繁切换，无可靠性问题。 | 测试通过 |
| 工具           | 对于时间跳转、频繁使用、主备切换后工具可正常使用，无可靠性问题。 | 测试通过 |

2、长时间负载测试：执行6个用例9个轮次，测试用例累计执行率100%，测试发现问题已解决并回归验证通过，整体质量良好。

| Domain | 测试内容                                                     | 测试结论                                                     |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 场景1  | astore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景2  | ustore场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core，系统可以长时间正常处理tpcc业务、DML增删改查并发事务、DDL事务；cpu、内存、io等系统资源使用均正常；一致性检查通过，平稳运行7*24H |
| 场景3  | 分区表场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core；cpu、内存、io等系统资源使用均正常 |
| 场景4  | 压缩表场景，TPCC+sysbench连跑insert/update/delete事务，200仓+100并发，7*24H测试 | 业务正常运行，主备机无core；cpu、内存、io等系统资源使用均正常 |
| 场景5  | 资源池化，一主一备主机读写备机只读场景，TPCC连跑insert/update/delete事务，主机800仓+500并发，备机800仓+200并发，7*24H测试 | 业务正常运行，主备机无core                                   |
| 场景6  | 资源池化，7*24H内，每12H一次全量备份，每6H一次增量备份；开启xlog归档长稳运行 | 备份功能正常；xlog归档运行正常                               |

### 4.23 性能测试

对关键性能指标进行摸底和测试，主要覆盖了A库、B库和资源池化场景下，2P单机/主备、4P单机的TPC-C测试及RTO测试，性能多轮测试稳定。

| **指标大项**      | **指标小项**                   | **指标值** | **说明**          | 测试结论                                                     |
| ----------------- | ------------------------------ | ---------- | ----------------- | ------------------------------------------------------------ |
| TPCC              | A库astore 2P单节点 1H 普通表   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***151.1***W+                  |
|                   | A库astore 2P单节点 1H 分区表   | 150万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***150.7***W+                  |
|                   | A库astore 2P一主一备 1H 普通表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***135.7***W+                |
|                   | A库astore 2P一主一备 1H 分区表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***137.6***W+                |
|                   | A库astore 4P单节点 1H 分区表   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***235.5***W+                    |
|                   | B库astore 2P单节点 1H 普通表   | 145万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***147.3***W+                  |
|                   | B库astore 2P单节点 1H 分区表   | 145万      | 此即为release基线 | 单节点1H在极限场景配置下tpmC为***147.8***W+                  |
|                   | B库astore 2P一主一备 1H 普通表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***135.4***W+                |
|                   | B库astore 2P一主一备 1H 分区表 | 130万      | 此即为release基线 | 一主一备1H在极限场景配置下tpmC为***135.5***W+                |
|                   | B库astore 4P单节点 1H 分区表   | 230万      | 此即为release基线 | 单节点在极限场景配置下tpmC为***231.6***W+                    |
|                   |                                |            |                   |                                                              |
| RTO高可用         | 一主一备failover               | 小于10s    | 此即为release基线 | **4**s                                                       |
| 资源池化sysbench  | 2P主机读写备机空载             | 3.5万      |                   | 主机读写备机空载场景下，主机使用oltp_read_write，性能为***4.1w tps***。 |
|                   | 2P主机读写备机只读             | 6万        |                   | 主机读写备机只读场景下，主机使用oltp_read_write，备机使用oltp_read_only，总计***6.1w tps***。 |
| 资源池化TPCC      | 2P主机读写备机空载 1H          | 120万      | 此即为release基线 | 主机读写备机空载 1H在极限场景配置下tpmC为***125.5***W+       |
|                   | 2P主机读写备机只读 1H          | 220万      | 此即为release基线 | 主机读写备机只读 1H在极限场景配置下tpmC为***248.4***W+       |
| 资源池化RTO高可用 | 单集群                         | 小于10s    |                   | 极限场景下业务恢复时间验收,使用benchmarksql工具，生成1000仓库的测试数据，模拟资源池化数据库主备组网环境出现故障并恢复的测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br/>故障前50wtpmC，85并发场景下，RTO恢复时间为***6.2s***，达到标准<10s。 |

### 4.24 兼容性测试

#### 4.241 升级兼容

针对openGauss 7.0.0 RC3版本，规划的升级路径如下表所示。测试整体情况：

1. openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss6.0.3(包含容器化场景)/openGauss 7.0.0 RC2灰度升级和指定节点升级到openGauss 7.0.0 RC3版本成功，升级失败或者升级未提交，可以成功回滚；
1. 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss6.0.3/openGauss 7.0.0 RC2灰度升级到openGauss 7.0.0 RC3版本成功，升级失败或者升级未提交，可以成功回滚；
3. 升级成功后，特性功能运行正常；
3. 升级成功后，元数据校验通过

| 升级路径                                                     | 测试结论                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2 不带CM灰度升级到openGauss 7.0.0 RC3不带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2 不带CM灰度升级到openGauss 7.0.0 RC3不带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2不带CM指定节点升级到openGauss 7.0.0 RC3不带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2 不带CM指定节点升级到openGauss 7.0.0 RC3不带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2不带CM灰度升级到openGauss 7.0.0 RC3带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2不带CM灰度升级到openGauss 7.0.0 RC3带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2不带CM指定节点升级到openGauss 7.0.0 RC3带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2不带CM指定节点升级到openGauss 7.0.0 RC3带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2带CM灰度升级到openGauss 7.0.0 RC3带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.3/openGauss 7.0.0 RC2带CM灰度升级到openGauss 7.0.0 RC3带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 7.0.0 RC1 带CM指定节点升级到openGauss 7.0.0 RC3带CM版本 | 测试通过                                                     |
| openGauss 5.0.0/openGauss 5.0.2/openGauss 5.0.5/openGauss 6.0.0/openGauss 6.0.2/openGauss 7.0.0 RC1 带CM指定节点升级到openGauss 7.0.0 RC3带CM版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 6.0.3容器化升级到openGauss 7.0.0 RC3版本           | 测试通过                                                     |
| openGauss 6.0.3容器化升级到openGauss 7.0.0 RC3版本，再回滚，再升级提交 | 测试通过                                                     |
| openGauss 5.0.0带CM灰度升级到5.0.2，继续升级到5.0.5，再灰度升级到openGauss 7.0.0 RC3带CM版本 | [8046](https://gitcode.com/opengauss/openGauss-server/issues/8046) |
| openGauss 5.0.0带CM灰度升级到5.0.2，继续升级到6.0.0到6.0.3带CM版本，再灰度升级到openGauss 7.0.0 RC3带CM版本 | 测试通过                                                     |
| openGauss 5.0.0带CM灰度升级到5.0.2，继续升级到6.0.0到6.0.3带CM版本，再灰度升级到openGauss 7.0.0 RC3带CM版本，回滚openGauss 6.0.3版本，再升级 | 测试通过                                                     |
| 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.2/openGauss 7.0.0 RC2 灰度升级到openGauss 7.0.0 RC3版本 | 测试通过                                                     |
| 资源池化openGauss 6.0.0/openGauss 6.0.2/openGauss 6.0.2/openGauss 7.0.0 RC2 灰度升级到openGauss 7.0.0 RC3版本，再回滚，再升级提交 | 测试通过                                                     |
| 资源池化openGauss 6.0.0灰度升级openGauss 6.0.3提交，再灰度升级到openGauss 7.0.0 RC3版本，再回滚openGauss 6.0.3，再升级提交 | 测试通过                                                     |

#### 4.242 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏920/鲲鹏920新型号/鲲鹏950服务器/虚拟机上安装部署openGauss 7.0.0 RC3数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）、云盘上和磁阵上安装部署openGauss 7.0.0 RC3数据库 | 测试通过 |

#### 4.243 软件兼容

| Domain       | 测试活动                                                     | 测试结论    |
| ------------ | ------------------------------------------------------------ | ----------- |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS/X86+openEuler 24.03 LTS/ARM+openEuler 24.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 7.0.0 RC3数据库 | 测试   通过 |

### 4.25 资料测试

| 序号 | **手册名称**                   | **测试结论** |
| ---- | ------------------------------ | ------------ |
| 1    | 《法律声明》                   | PASS         |
| 2    | 《关于openGauss》              | PASS         |
| 3    | 《新手入门》                   | PASS         |
| 4    | 《安装部署》                   | PASS         |
| 5    | 《使用与运维》- SQL教程        | PASS         |
| 6    | 《使用与运维》- 数据库管理指南 |              |
| 7    | 《使用与运维》- 数据库运维指南 | PASS         |
| 8    | 《使用与运维》- 性能调优指南   | PASS         |
| 9    | 《使用与运维》- 数据迁移指南   | PASS         |
| 10   | 《重要特性》 - 向量数据库      | PASS         |
| 11   | 《重要特性》 - 资源池化        | PASS         |
| 12   | 《开发指南》- 应用开发指南     | PASS         |
| 13   | 《开发指南》- 编译指南         | PASS         |
| 14   | 《参考指南》- 插件参考         | PASS         |
| 15   | 《参考指南》- 数据库参考       | PASS         |
| 16   | 《参考指南》- 工具和命令参考   | PASS         |
| 17   | 《参考指南》- 特性参考         | PASS         |
| 18   | 《参考指南》- 附录             | PASS         |

## 4.3 DBV伙伴测试结论

| 伙伴名称 | 主导新需求数量 | 问题单数量 | 测试设计篇数 | 用例数量                          | 测试报告 | 测试内容                                                     | 遗留问题个数 | 版本测试结论 |
| -------- | -------------- | ---------- | ------------ | --------------------------------- | -------- | ------------------------------------------------------------ | ------------ | ------------ |
| 海量     | 5              | 21         | 5            | 文本用例：120+<br />自动化用例80+ | 5        | 1、需求测试<br />2、继承特性验证：数据库服务（SQL语法、主备管理、GUC参数控制、内核工具链）、数据库管理与运维（实例管理）、数据库兼容性（资源池化兼容性）和可靠性测试 | 0            | 测试通过     |
| 南大     | 4              | 8          | 4            | 文本用例：148；自动化：90+        | 4        | 1、需求测试<br />2、继承特性验证：上个版本负责需求自动化实现和测试<br />3、兼容性测试：升级兼容<br /> | 0            | 测试通过     |
| 神通     | 5              | 6          | 5            | 文本用例：126；自动化用例125      | 5        | 1、需求测试<br />2、安全测试：神通自研语句模糊测试工具RandomTest模糊测试<br />3、资料测试：随新需求验证，验证通过 | 0            | 测试通过     |
| 邮储     | 1              | 7          | 1            | /                                 | 1        | 1、需求测试<br />2、问题回归                                 | 0            | 测试通过     |
| 国创     | 0              | 3          | 0            | /                                 | 0        | 1、datakit测试：主要集中智能运维页面测试和问题解决回归       | 0            | 测试通过     |



# 5、问题单统计分析

## 5.1 问题单统计

openGauss 7.0.0 RC3版本截止目前转测24个B版本，共发现问题848个，有效问题715个，无效问题133个。修复问题回归测试结果正常，版本整体质量良好。详细分布见下表: 

| 版本名称                 | 测试起始时间 | 测试结束时间 | 有效问题数 | 无效问题数 |
| ------------------------ | ------------ | ------------ | ---------- | ---------- |
| openGauss 7.0.0 RC3 B001 | 2025/10/1    | 2025/10/22   | 89         | 21         |
| openGauss 7.0.0 RC3 B002 | 2025/10/23   | 2025/10/29   | 16         | 6          |
| openGauss 7.0.0 RC3 B003 | 2025/10/30   | 2025/11/5    | 11         | 4          |
| openGauss 7.0.0 RC3 B004 | 2025/11/6    | 2025/11/12   | 11         | 1          |
| openGauss 7.0.0 RC3 B005 | 2025/11/13   | 2025/11/19   | 13         | 4          |
| openGauss 7.0.0 RC3 B006 | 2025/11/20   | 2025/11/26   | 14         | 5          |
| openGauss 7.0.0 RC3 B007 | 2025/11/27   | 2025/12/3    | 22         | 4          |
| openGauss 7.0.0 RC3 B008 | 2025/12/4    | 2025/12/10   | 24         | 0          |
| openGauss 7.0.0 RC3 B009 | 2025/12/11   | 2025/12/17   | 22         | 2          |
| openGauss 7.0.0 RC3 B010 | 2025/12/18   | 2025/12/24   | 54         | 4          |
| openGauss 7.0.0 RC3 B011 | 2025/12/25   | 2025/12/28   | 7          | 1          |
| openGauss 7.0.0 RC3 B012 | 2025/12/29   | 2025/12/31   | 18         | 1          |
| openGauss 7.0.0 RC3 B013 | 2026/1/1     | 2026/1/14    | 67         | 18         |
| openGauss 7.0.0 RC3 B014 | 2026/1/15    | 2026/1/21    | 35         | 7          |
| openGauss 7.0.0 RC3 B015 | 2026/1/22    | 2026/1/28    | 37         | 2          |
| openGauss 7.0.0 RC3 B016 | 2026/1/29    | 2026/2/4     | 43         | 4          |
| openGauss 7.0.0 RC3 B017 | 2026/2/5     | 2026/2/11    | 43         | 10         |
| openGauss 7.0.0 RC3 B018 | 2026/2/12    | 2026/2/25    | 25         | 7          |
| openGauss 7.0.0 RC3 B019 | 2026/2/26    | 2026/3/4     | 64         | 11         |
| openGauss 7.0.0 RC3 B020 | 2026/3/5     | 2026/3/11    | 61         | 18         |
| openGauss 7.0.0 RC3 B021 | 2026/3/12    | 2026/3/16    | 24         | 2          |
| openGauss 7.0.0 RC3 B022 | 2026/3/17    | 2026/3/19    | 10         | 0          |
| openGauss 7.0.0 RC3 B023 | 2026/3/20    | 2026/3/31    | 3          | 1          |
| openGauss 7.0.0 RC3 B024 | 2026/3/24    | 2026/3/30    | 1          | 0          |

## 5.2 问题单分析

本次测试共23轮，从openGauss 7.0.0 RC3 B001持续到openGauss 7.0.0 RC3 B024。问题单提单数量同需求转测转测周期对应，集成测试和需求测试少量并行；同时问题单较少的B版本均为月底增加转测版本，测试周期一般在3天左右。

2026.10.9启动openGauss 7.0.0 RC3版本迭代1测试，直至2025.12.3 openGauss 7.0.0 RC3迭代2测试结束，对应B001到B007版本，需求转测数量较少，该阶段问题单占比问题单总量1/5；问题单主要来源于兼容性、工具链和向量数据库相关验证，根据转测需求数量不同提单数量有所差异，问题单整体变化趋势较为平缓；因兼容性和工具链需求占比多，对应问题也最多，占比80%左右，资源池化由于转测需求少问题数量最少。

2025.12.4正式进入openGauss 7.0.0 RC3版本迭代3测试，直至2026.2.11转测B018版本，该阶段版本需求集中交付，工具链、向量数据库、资源池化和内核需求都有序转测，提单也主要来源于对应需求，该周期问题单占比总量约52%。迭代4有联合伙伴部分需求测试。该阶段全量CI和专项测试周版本验证，自动化和长稳、升级、可靠性和性能问题占比该阶段提单10%左右。迭代3和迭代4需求转测、测试验证和问题发现整体能相互对应，版本平稳。

openGauss 7.0.0 RC3 B019版本，开始进入版本集成测试阶段。截止目前主要测试活动版本为B019到B022版本：B019版本有少量内部和伙伴未完成需求继续测试活动；B019到B021三个版本SIT测试涉及全量用例和专项测试，包含CI连跑、手工用例执行、长稳、可靠性、性能、安全、资料等场景测试，SIT测试发现有效缺陷85个，伙伴需求测试和开发自提单发现有效缺陷20+，工具链和资源池化问题较为突出；过程中同步进行问题回归。B022到B024主要进行问题单回归和集成测试工作收尾，新增问题单较少。通过B版本和逐日分析提单数量，提单数量呈收敛趋势；

## 5.3 执行用例与问题单数对应关系

| 领域               | 执行用例数量 | 有效问题单数量（CI+需求+集成测试+开发自提） |
| ------------------ | ------------ | ------------------------------------------- |
| 数据库服务         | 50000+       | 137                                         |
| 工具链             | 5500+        | 233                                         |
| 资源池化           | 4000+        | 156                                         |
| 兼容性             | 11000+       | 47                                          |
| 内核加速           | 2000+        | 95                                          |
| 可靠性\稳定性\性能 | 1100+        | 28                                          |
| 资料               | 50+          | 26                                          |

## 5.4 集成测试迭代版本基线

| 迭代版本 | 转测试B版本                                       | 测试项                                                       |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| 迭代1    | openGauss 7.0.0-RC2.B001~openGauss 7.0.0-RC2.B003 | D库兼容性；低频率全量CI、性能/可靠性/长稳测试                |
| 迭代2    | openGauss 7.0.0-RC2.B004~openGauss 7.0.0-RC2.B007 | 兼容性、工具链和向量数据库相关特性测试；低频率全量CI、性能/可靠性/长稳测试 |
| 迭代3    | openGauss 7.0.0-RC2.B008~openGauss 7.0.0-RC2.B011 | 向量数据库、黑匣子和工具链相关需求测试；周力度全量CI、性能/可靠性/常稳/升级/安全测试 |
| 迭代4    | openGauss 7.0.0-RC2.B012~openGauss 7.0.0-RC2.B017 | 兼容性、工具链和内核加速相关需求测试；周粒度全量CI、性能/可靠性/常稳/升级/安全测试 |
| 版本发布 | openGauss 7.0.0-RC2.B018~openGauss 7.0.0-RC2.B024 | 全量CI连跑、专项测试、集成测试、问题单回归                   |



# 6、附件

openGauss 7.0.0 RC3版本评审遗留问题1个，详情见下表

## 附件1：遗留问题列表

| 序号 | issue号                                                      | 问题简述                                                     | 分类 | 问题级别 | 问题分析与影响                                               | 规避措施                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | [7840](https://gitcode.com/opengauss/openGauss-server/issues/7840) | [Bug]: [测试类型 功能测试] neon branching 使用创建main计算节点跑TPCC测试失败，多线程导入数据超时 | 缺陷 | 次要     | 问题分析：TPCC多次压测以后wal proposer进程异常断连导致wal日志流式发送失败，数据回放不完整，TPCC压测报错<br />问题影响：TPCC压测完之后重启计算节点，wal redo进程回放数据失败导致数据丢失，数据持久化能力不可用； | 基于safekeeper日志查看故障前的LSN号，创建分支恢复故障前的数据 |

 优先级和DI对应关系说明：

严重/主要：DI 3分

次要：DI 1分

不重要/无级别：DI 0.1分

# 致谢

感谢参与撰写本文、或在过程中给出宝贵指导意见的各位社区开发者（排名不分先后）

+ [@sungang14](https://gitcode.com/sungang14)                                               

+ [@czywj](https://gitcode.com/czywj)

+ [@u012280419](https://gitcode.com/u012280419)

+ [@l1azzzy](https://gitcode.com/l1azzzy)

+ [@szoscar55](https://gitcode.com/szoscar55)

+ [@cloudsbreak](https://gitcode.com/cloudsbreak)

+ [@xiaobai-181](https://gitcode.com/xiaobai-181)

+ [@yc-jianda](https://gitcode.com/yc-jianda)

  

