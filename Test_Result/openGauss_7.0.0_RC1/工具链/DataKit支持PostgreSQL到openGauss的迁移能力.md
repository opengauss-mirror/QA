![avatar](../../../images/openGauss.png)

版权所有 © 2025 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode)。

修订记录

| 日期       | 修订版本 | 修改描述 | 作者     |
| ---------- | -------- | -------- | -------- |
| 2025/03/11 | v1.0     | 创建     | lihongji |

**Keywords 关键词**：PostgreSQL、openGauss、debezium、正向全量/增量迁移、反向全量/增量迁移

**Abstract 摘要**：PostgreSQL数据迁移至openGauss

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 概述

​        该特性用于PostgreSQL数据库迁移，通过逻辑复制解码PostgreSQL数据库日志，捕获数据库发生的更改事件，将每个更改事件解析为可执行的sql语句，在目标端openGauss数据库进行回放，实现在不停业务的前提下将PostgreSQL数据库迁移至openGauss数据库，支持正向迁移及反向迁移。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                 | 软件包名称                                            | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------ | ----------------------------------------------------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 B015 | openGauss-All-7.0.0-RC1-openEuler22.03-aarch64.tar.gz | 2025.2.15    | 2025.3.6     | lihongji |
| openGauss 7.0.0-RC1 B019 | openGauss-All-7.0.0-RC1-openEuler22.03-aarch64.tar.gz | 2025.3.7     | 2025.3.11    | lihongji |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

硬件环境信息

| 环境信息 | 硬件型号                                           | 硬件配置信息                                                 | 备注 |
| -------- | -------------------------------------------------- | ------------------------------------------------------------ | ---- |
| 物理机   | ARM+openEuler 4P<br/>TaiShan 200 (Model 2280 7260) | CPU：Kunpeng-920 4p 128核<br/>内存：768G<br/>硬盘：NVME 3.7T * 4<br/>OS：openEuler release 20.03 (LTS)<br/>文件系统：XFS<br/>网卡：4*25GE Hi1822 |      |
| 物理机   | ARM+openEuler 4P<br/>TaiShan 200 (Model 2280 7260) | CPU：Kunpeng-920 4p 128核<br/>内存：768G<br/>硬盘：NVME 3.7T * 4<br/>OS：openEuler release 20.03 (LTS)<br/>文件系统：XFS<br/>网卡：4*25GE Hi1822 |      |
| 物理机   | ARM+openEuler 4P<br/>TaiShan 200 (Model 2280 7260) | CPU：Kunpeng-920 4p 128核<br/>内存：768G<br/>硬盘：NVME 3.7T * 4<br/>OS：openEuler release 20.03 (LTS)<br/>文件系统：XFS<br/>网卡：4*25GE Hi1822 |      |

# 3 版本概要测试结论、关键风险和规避措施

Datakit支持postgreSQL到openGauss的迁移能力，主要包括功能测试，性能测试。功能验证数据类型边界值、对象（函数、触发器、存储过程、视图）、多模式、表及模式黑白名单配置、csv文件夹阈值、数据一致性测试,反向增量迁移断点续传验证。性能测试，基于sybench预置数据，20张表(无主键表)，每张表500w条数据，全量迁移速率达300M/s。正向增量使用pgoutput插件，基于sysbench, 50张表，50线程（insert30、update10、delete10）,性能可达3wtps，使用wal2json插件,基于sysbench, 50张表，50线程（insert30、update10、delete10）,性能可达2wtps。执行146个测试用例，发现15个问题（其中内核不兼容3个），遗留问题0，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                     | 特性价值评估                                                 | 应用说明及关键约束假设依赖                                   | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| -------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- | ---------------- | -------------------------- | -------- |
| 【工具链】**datakit支持PostgreSQL到openGauss的迁移能力** | 当前迁移工具支持的有M*、O*数据库，增加postgresql数据，完善迁移能力 | 1.正向全量迁移不支持唯一约束、非空约束、外键约束、check约束<br/>2.正向全量迁移不支持自定义类型的迁移、其他对象的迁移（序列、数据库用户、权限）<br/>3.正向全量迁移不支持表结构comment信息、schema级、表级、列级字符集和字符序等信息的迁移<br/>4.正向增量迁移不支持断点续传<br/>5.正向增量迁移中目前最多支持二维数组迁移且需要使用pgoutput插件<br/>6.内核不兼容的类型暂不支持迁移<br/>7.反向增量迁移不支持列存和Ustore存储引擎<br/>8.反向增量迁移不支持分区表、分区索引、隐藏索引、重建索引<br/>9.反向增量迁移alter view/type/schema仅支持rename<br/>10.type类型仅支持复合类型与枚举类型 | 无                   | 100%             | <font color=green>▮</font> | 无       |



*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

正向全量迁移性能测试，基于sybench预置数据，20张表(无主键表)，每张表500w条数据，全量迁移速率达300M/s(问题单已修复待回归)。

正向增量使用pgoutput插件，基于sysbench, 50张表，50线程（insert30、update10、delete10）,性能均可达3wtps，使用wal2json插件,基于sysbench, 50张表，50线程（insert30、update10、delete10）,性能可达2wtps。测试通过

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

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [postgresql迁移资料](https://gitee.com/opengauss/debezium/pulls/301/files、https://gitee.com/opengauss/debezium/pulls/293/files) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本特性主要从功能、性能方面进行测试。功能正向全量迁移验证数据类型边界值、对象（函数、触发器、存储过程、视图）、多模式、表及模式黑白名单配置、csv文件夹阈值。正向增量迁移分别验证pgoutput、wal2json插件下的数据类型边界值迁移。反向全量迁移验证数据类型边界值。反向增量迁移验证数据类型边界值、对象（函数、触发器、存储过程、视图）。正向全量迁移性能验证基线数据20张表(无主键)，每张表500w数据迁移速率满足300m/s,正向增量迁移满足分别验证pgoutput插件基线数据 50张表，50线程（insert30、update10、delete10）,性能可达3wtps，使用wal2json插件,基于sysbench, 50张表，50线程（insert30、update10、delete10）,性能可达2wtps

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 15       | 0    | 2    | 12   | 1      |
| 百分比 | 100%     | 0%   | 13%  | 80%  | 7%     |

###   5.2.2 缺陷列表

| 序号 | issue号                                                      | 级别   | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ | ------ |
| 1    | [IBROES](https://e.gitee.com/opengaussorg/issues/table?issue=IBROES) | 次要   | 【测试类型：功能测试】【测试版本：7.0.0rc1】pg反向迁移money、时间日期、枚举、数组、标识符类型、表字段默认值DEFAULT CURRENT_TIMESTAMP\CURRENT_DATE迁移不成功 | 已验收 |
| 2    | [IBRD9R](https://e.gitee.com/opengaussorg/issues/table?issue=IBRD9R) | 主要   | 【测试类型：性能测试】【测试版本：7.0.0rc1】pg全量迁移，20张表，每张表500w数据，无主键表全量迁移速率未达到300m/s | 已验收 |
| 3    | [IBRAL7](https://e.gitee.com/opengaussorg/issues/table?issue=IBRAL7) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg反向全量迁移不支持特殊格式迁移 | 已验收 |
| 4    | [IBQXAY](https://e.gitee.com/opengaussorg/issues/table?issue=IBQXAY) | 主要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg全量正向全量迁移3000张表，每张表10万条数据迁移不成功 | 已验收 |
| 5    | [IBQODG](https://e.gitee.com/opengaussorg/issues/table?issue=IBQODG) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg全量正向迁移range一级分区未迁移到目标端 | 已验收 |
| 6    | [IBQN45](https://e.gitee.com/opengaussorg/issues/table?issue=IBQN45) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】wal2json枚举中文，timestamptz(2)数据类型迁移不成功 | 已验收 |
| 7    | [IBQDQX](https://e.gitee.com/opengaussorg/issues/table?issue=IBQDQX) | 不重要 | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg全量迁移超出csv文件夹大小阈值，频繁循环打印提示日志 | 已验收 |
| 8    | [IBPJ1N](https://e.gitee.com/opengaussorg/issues/table?issue=IBPJ1N) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库全量迁移至openGauss数据库，触发器迁移至openGauss报错 | 已验收 |
| 9    | [IBO1AM](https://e.gitee.com/opengaussorg/issues/table?issue=IBO1AM) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库增量迁移至openGauss数据库，pg_lsn、标识符类型迁移失败 | 已验收 |
| 10   | [IBNOQU](https://e.gitee.com/opengaussorg/issues/table?issue=IBNOQU) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库增量迁移至openGauss数据库，枚举、数组类型迁移失败 | 已验收 |
| 11   | [IBN9TE](https://e.gitee.com/opengaussorg/issues/table?issue=IBN9TE) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库增量迁移至openGauss数据库，'infinity'、'NaN'值迁移失败。bytea类型转移迁移失败 | 已验收 |
| 12   | [IBN2AC](https://e.gitee.com/opengaussorg/issues/table?issue=IBN2AC) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库全量迁移至openGauss数据库，xml数据类型数据迁移数据不正确 | 已验收 |
| 13   | [IBN02W](https://e.gitee.com/opengaussorg/issues/table?issue=IBN02W) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0-RC1】pg全量迁移至openGauss数据库特殊时间格式类型迁移失败 | 已验收 |
| 14   | [IBMS0G](https://e.gitee.com/opengaussorg/issues/table?issue=IBMS0G) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库增量迁移至openGauss数据库，money数据类型的数据迁移失败 | 已验收 |
| 15   | [IBMOIM](https://e.gitee.com/opengaussorg/issues/table?issue=IBMOIM) | 次要   | 【测试类型：工具功能】【测试版本：7.0.0rc1】pg数据库全量迁移至openGauss数据库，money数据类型数据迁移失败 | 已验收 |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                  | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | --------------------- | ------------------------------------------------------------ | -------------------- |
| 1    | 正向全量/增量迁移测试 | 正向全量迁移验证数据类型边界值、对象（函数、触发器、存储过程、视图）、多模式、表及模式黑白名单配置、csv文件夹阈值。正向增量迁移分别验证pgoutput、wal2json插件下的数据类型边界值迁移。测试通过 | YES                  |
| 2    | 反向全量/增量迁移     | 反向全量迁移验证数据类型边界值。反向增量迁移验证数据类型边界值、对象（函数、触发器、存储过程、视图）、反向增量断点续传。测试通过 | YES                  |
| 3    | 正向全量/增量性能测试 | 验证增量/反向迁移任务状态接口、启动增量/反向迁移任务接口、数据校验汇总信息查询接口、全量校验实时进度查询接口、全量校验修复文件下载接口正常、异常入参场景。测试通过 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度   |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | ---------- |
| openGauss 7.0.0-RC1 B015 | 18               | 146        | 146        | 15         | 3.514k | 0.857/kloc |
| openGauss 7.0.0-RC1 B019 | 3                | 15         | 15         | 0          | 0      | 0          |

本次测试共发现15个issue（包含3个内核pg不兼容单不计入工具缺陷），缺陷密度为0.857/kloc,整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 146          | 146              | 109    | 37     | 0       | 0           | 100%   | 100%       |

本次测试共输出测试用例146个，执行测试共2轮，发现issue共15个，均已修复并回归通过，无遗留问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/281

https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/285

https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/286

资料pr：

https://gitee.com/opengauss/debezium/pulls/301/files

https://gitee.com/opengauss/debezium/pulls/293/files

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=d82725fac24c4d1cbbc1574b50d1e310&hideDevcloudHead=true
