# 鲲鹏超节点加速资源池化clog,csnlog特性测试报告

版权所有 © 2026 openGauss 社区

## 修订记录

| 日期         | 修订版本 | 修改描述                    | 作者      |
| ---------- | ---- | ----------------------- | ------- |
| 2026-06-26 | 1.0  | 鲲鹏超节点加速资源池化clog,csnlog测试报告 | l1azzzy |

## Keywords 关键词

openGauss, 资源池化, 鲲鹏超节点, UB, 事务元数据, CLOG, CSNLOG, Snapshot, OldestXmin, enable_ub, ubs_mem_path

## Abstract 摘要

本文档总结 openGauss 资源池化场景下鲲鹏超节点加速事务元数据访问特性的测试活动。测试覆盖 GUC 参数、共享内存初始化、事务一致性、Snapshot/OldestXmin、主备切换、节点启停、在线 failover、资料与性能等场景。功能测试有效范围 37 条，通过 28 条，发现问题 2 条，阻塞 7 条；性能测试在 300+300 与 500+100 两个场景下总性能分别提升 14.24% 和 20.43%，但因主机性能劣化较为严重，性能测试结论为未通过。

## 缩略语清单

| 缩略语      | 英文全称                               | 中文解释                  |
| -------- | ---------------------------------- | --------------------- |
| UB       | Unified Buffer / Unified Borrowing | 本报告中特指鲲鹏超节点事务元数据共享内存访问能力 |
| DSS      | Distributed Storage Service        | 资源池化共享存储服务            |
| DMS      | Distributed Memory Service         | 资源池化分布式内存服务           |
| CLOG     | Commit Log                         | 事务提交状态日志              |
| CSNLOG   | Commit Sequence Number Log         | 事务 CSN 日志             |
| SHM      | Shared Memory                      | 共享内存                  |
| tpmC     | transactions per minute C          | TPC-C 主指标             |
| tpmTOTAL | total transactions per minute      | 总事务吞吐指标               |

# 1 概述

本报告为 openGauss 特性的测试总结。被测特性通过 `enable_ub` 开关启用，在资源池化一写多读部署中，将部分事务元数据访问由 TCP/DMS 路径优化为共享内存 SHM 路径，以降低备机查询事务状态、Snapshot、OldestXmin 等场景的访问开销。

测试重点包括：

- `enable_ub`、`ubs_mem_path`、`ss_init_clog_size`、`ss_enable_ondemand_recovery` 等参数行为。
- CLOG、CSNLOG、Snapshot、OldestXmin 基础功能一致性。
- 主备切换、节点启停、在线 failover 等 reform 场景。
- UB 共享内存对象可见性。
- 性能提升。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                 | 软件包名称                                                 | 测试起始时间     | 测试结束时间     | 测试人员    |
| -------------------- | ----------------------------------------------------- | ---------- | ---------- | ------- |
| openGauss 7.0.0 B007 | openGauss-All-7.0.0-RC3-openEuler24.03-aarch64.tar.gz | 2026-06-08 | 2026-06-17 | l1azzzy |


### 2.1.2 配套测试的版本

| 版本名称    | 配套版本                 | 版本说明             |
| ------- | -------------------- | ---------------- |
| CM      | openGauss 7.0.0 B007 | 资源池化集群管理组件       |
| DSS/DMS | openGauss 7.0.0 B007 | 资源池化共享存储与分布式内存组件 |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号                  | 硬件配置信息                                                                                                                         | 备注         |
| :--- | :-------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :--------- |
| 主节点  | HiSilicon aarch64 服务器 | CPU：308 vCPU，2 Socket，77 Core/Socket，2 Thread/Core，最高 2300MHz；内存：约 1.0TiB；磁盘：系统盘与数据盘分离部署；OS：openEuler 24.03 (LTS-SP3)；文件系统：LVM | 资源池化数据库主节点 |
| 备节点  | HiSilicon aarch64 服务器 | CPU：308 vCPU，2 Socket，77 Core/Socket，2 Thread/Core，最高 2300MHz；内存：约 658GiB；磁盘：系统盘与数据盘分离部署；OS：openEuler 24.03 (LTS-SP3)；文件系统：LVM | 资源池化数据库备节点 |
| 压测节点 | \                     | 部署 BenchmarkSQL / TPC-C 压测工具；通过测试网络访问数据库集群                                                                                     | 性能测试客户端    |


### 2.2.2 测试环境组网与软件配置

| 配置项   | 说明                                      |
| ----- | --------------------------------------- |
| 集群形态  | 资源池化一主一备部署，启用 DSS/DMS/CM                |
| 数据库版本 | openGauss 7.0.0 B007                    |
| 操作系统  | openEuler 24.03 (LTS-SP3) aarch64       |
| 测试工具  | BenchmarkSQL、gsql、gs_guc、cm_ctl、ubsectl |
| UB 依赖 | UBS Memory 服务与 UB SDK 动态库               |
| 测试库   | 使用功能测试库和业务压测库进行验证                       |

### 2.2.3 OS 版本

| 操作系统 | OS 版本 | 版本说明 |
| -------- | ------- | -------- |
| openEuler | 24.03 (LTS-SP3) aarch64 | openGauss 资源池化测试环境 |


# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本轮功能测试有效范围 37 条，执行覆盖率 100%。其中通过 28 条，发现问题 2 条，阻塞 7 条。

性能测试在 300+300 和 500+100 场景中，SHM / `enable_ub=on` 相比 TCP / `enable_ub=off` 的主机 tpmC 下降，但备机 tpmTOTAL 显著提升，总性能分别提升 14.24% 与 20.43%。由于主机性能下降明显，性能测试结论为未通过。

综合结论：该特性基础功能可用，但 reform/节点启停场景存在风险，性能验收未通过。

## 3.2 约束说明

- `ub_debug_log`、`ub_sigbus_handler` 不纳入本轮测试。

## 3.3 关键风险和规避措施

| 风险类型  | 风险描述                                                                                      | 风险影响               | 规避措施和计划               | 责任人 | 当前进展 |
| ----- | ----------------------------------------------------------------------------------------- | ------------------ | --------------------- | --- | ---- |
| 可靠性风险 | `enable_ub=on` 后使用 `cm_ctl stop/start -n 2 -D` 启停备机，DMS 变 `Deleted`，DN `Manually stopped` | 节点恢复失败，集群降级，需要人工恢复 | 等待120s之后重新启动集群        | 开发  | 待办的  |
| 恢复风险  | 在线 failover 后旧主恢复概率性触发 DSS `Deleted`                                                      | 影响高可用恢复能力          | 等待120s之后重新启动集群        | 开发  | 待办的  |
| 性能风险  | SHM 优化后主机 tpmC 下降                                                                         | 未达到验收指标            | 分析主机路径开销和负载转移影响，优化后重测 | 开发  | 待办的  |

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性            | 特性价值评估                                                              | 应用说明及关键约束假设依据                                              | 关键遗留事项                       | 测试整体覆盖情况                                | 特性质量评价           | 主要风险           |
| ------------- | ------------------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------- | --------------------------------------- | ---------------- | -------------- |
| 资源池化鲲鹏超节点事务元数据加速 | 在资源池化一写多读场景下，通过 SHM 优化备机访问事务元数据路径，提升备机事务状态/Snapshot/OldestXmin 查询效率 | 依赖 `enable_ub=on`、`ubs_mem_path` 正确配置、DSS/DMS 正常，且不与按需回放冲突 | 节点启停和恢复场景存在 DSS/DMS 异常；性能未达标 | 有效范围 37 条，执行 37 条，通过 28 条，问题 2 条，阻塞 7 条 | <font color=yellow><font color=yellow>▲</font></font>：基本可用但存在可靠性和性能风险 | reform/节点恢复、性能 |

## 4.2 产品功能属性测试结论

| 功能点验证                                          | 是否覆盖 | 结论                              |
| ---------------------------------------------- | ---- | ------------------------------- |
| `enable_ub` 默认关闭、开启后重启生效、在线修改不立即生效             | YES  | 通过                              |
| `enable_ub` 非法值配置拦截                            | YES  | 通过                              |
| `enable_ub` 与 `ss_enable_ondemand_recovery` 冲突 | YES  | 通过，冲突配置无法启动为 Normal             |
| `ss_init_clog_size` 默认值、合法边界、非法值、reload 行为     | YES  | 通过                              |
| CLOG/CSNLOG 基础事务状态一致性                          | YES  | 通过                              |
| Snapshot 正常获取与内容一致性                            | YES  | 通过                              |
| OldestXmin / vacuum / delete 场景一致性             | YES  | 通过                              |
| switchover                                     | YES  | 通过，备节点 可升主，后续已切回 主节点            |
| 在线 failover                                    | YES  | 通过，备节点 可升主并写入；旧主恢复存在风险          |
| 节点重启/残留共享内存                                    | YES  | 不通过，`cm_ctl start -n 2 -D` 恢复失败 |


### 4.2.1 性能测试结论

| 场景 | 参数 | 主机 tpmC | 备机 tpmTOTAL | 总性能 = 主机 tpmC + 备机 tpmTOTAL | 相比基线 |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 300 + 300 | 基线 TCP / enable_ub=off | 1,112,072.59 | 2,538,856.75 | 3,650,929.34 | - |
| 300 + 300 | 优化 SHM / enable_ub=on | 763,643.14 | 3,407,160.65 | 4,170,803.79 | +14.24% |
| 500 + 100 | 基线 TCP / enable_ub=off | 1,209,659.41 | 2,617,371.89 | 3,827,031.30 | - |
| 500 + 100 | 优化 SHM / enable_ub=on | 989,224.69 | 3,619,876.21 | 4,609,100.90 | +20.43% |

300 + 300 场景：

| 指标 | 基线 | 优化后 | 变化 |
| :-- | :-- | :-- | :-- |
| 主机 tpmC | 1,112,072.59 | 763,643.14 | -31.33% |
| 主机 tpmTOTAL | 2,471,888.91 | 1,697,762.16 | -31.32% |
| 备机 tpmTOTAL | 2,538,856.75 | 3,407,160.65 | +34.20% |
| 合计 tpmTOTAL | 5,010,745.66 | 5,104,922.81 | +1.88% |
| 合计事务数 | 100,217,861 | 102,100,836 | +1.88% |

500 + 100 场景：

| 指标 | 基线 | 优化后 | 变化 |
| :-- | :-- | :-- | :-- |
| 主机 tpmC | 1,209,659.41 | 989,224.69 | -18.22% |
| 主机 tpmTOTAL | 2,688,435.85 | 2,198,644.26 | -18.22% |
| 备机 tpmTOTAL | 2,617,371.89 | 3,619,876.21 | +38.30% |
| 合计 tpmTOTAL | 5,305,807.74 | 5,818,520.47 | +9.66% |
| 合计事务数 | 106,120,558 | 116,374,649 | +9.66% |


## 4.3 产品质量属性目标（DFX）测试结论

### 4.3.1 可靠性测试结论

| 测试步骤                               | 测试结果                                         |
| ---------------------------------- | -------------------------------------------- |
| `enable_ub=on` 后全量 stop/start      | 通过，集群可恢复 Normal                              |
| switchover 至 备节点                   | 通过，备节点 成为 Primary，集群 Normal                  |
| 在线 failover，停止 主节点 主实例             | 通过，备节点 成为 Primary 并可写入                       |
| 在线 failover 后恢复 主节点                | 存在问题，主节点 DSS 进入 `Deleted`，需要人工恢复             |
| `cm_ctl stop/start -n 2 -D <数据目录>` | 存在问题，备节点 DMS `Deleted`，DN `Manually stopped` |

### 4.3.2 安全与隐私保护测试结论

本特性不涉及。

### 4.3.3 可服务性测试结论

| 测试步骤                                                    | 测试结果                                   |
| ------------------------------------------------------- | -------------------------------------- |
| `ubsectl display memory -t borrow_detail` 查询 UB 共享内存    | 通过，root 下可见 `测试用户_txn_cache`，状态 `done` |
| 参数可查询性：`enable_ub`、`ubs_mem_path`、`ss_init_clog_size` 等 | 通过                                     |


### 4.3.4 兼容性测试结论

| 测试步骤                                                 | 测试结果                   |
| ---------------------------------------------------- | ---------------------- |
| `enable_ub=off` TCP/DMS 原路径                          | 通过                     |
| `enable_ub=on` SHM 路径                                | 基础功能通过，reform/恢复场景存在问题 |
| `ss_enable_ondemand_recovery=on` 与 `enable_ub=on` 组合 | 通过，冲突场景未能启动为 Normal    |

## 4.4 资料测试结论

| 序号  | 测试章节            | 测试结论                                                                                        |
| --- | --------------- | ------------------------------------------------------------------------------------------- |
| 1   | UB 事务快照访问加速特性说明 | 参数 `enable_ub`、`ubs_mem_path`、`ss_enable_ondemand_recovery`、`ss_init_clog_size` 与测试环境可查询项一致 |
| 2   | 运维命令说明          | `ubsectl display memory -t borrow_detail` 可用于观测 UB 共享内存对象                                   |


# 5 测试对象质量评估

## 5.1 覆盖率分析

| 项目        | 数量                                    |
| --------- | ------------------------------------- |
| 设计用例总数    | 39                                    |
| 有效范围      | 37                                    |
| 通过        | 28                                    |
| 有问题       | 2                                     |
| 阻塞        | 7                                     |
| 未执行       | 0                                     |
| 有效范围执行覆盖率 | 100%                                  |

阻塞用例清单：

```text
Opengauss_UB_TxnCache_Function_Case015
Opengauss_UB_TxnCache_Function_Case016
Opengauss_UB_TxnCache_Function_Case019
Opengauss_UB_TxnCache_Function_Case021
Opengauss_UB_TxnCache_Reform_Case029
Opengauss_UB_TxnCache_Exception_Case036
Opengauss_UB_TxnCache_Exception_Case037
```

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

| 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| -------- | ---- | ---- | ---- | ------ |
| 2 | 0 | 1 | 1 | 0 |

### 5.2.2 缺陷列表

| 问题单号                                                                                | 问题描述                                                                                                                             | 问题级别 | 当前状态 |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ---- | ---- |
| [openGauss-server#8217](https://gitcode.com/opengauss/openGauss-server/issues/8217) | `enable_ub=on` 后实例级启停备机：`cm_ctl stop/start -n 2 -D <数据目录>`，备节点 DMS 变 `Deleted`，DSS `OffLine`，DN `Manually stopped`，集群 `Degraded` | 主要   | 待办的  |
| [openGauss-server#8209](https://gitcode.com/opengauss/openGauss-server/issues/8209) | 开启 UB 事务加速访问特性后，主机 tpmC 和主机 tpmTOTAL 下降，性能测试未达到验收指标                                                                              | 主要   | 待办的  |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性 | 验证策略 | 是否按照测试策略执行 |
| --- | --- | --- | --- |
| 1 | GUC 参数与启动控制 | 通过 `enable_ub`、`ubs_mem_path`、`ss_init_clog_size`、`ss_enable_ondemand_recovery` 等参数配置、reload、restart 和异常配置验证参数行为及冲突拦截 | YES |
| 2 | CLOG/CSNLOG 共享内存初始化 | 验证数据库启动、重启、节点启停后 UB 共享内存对象创建、复用、清理及 `ubsectl display memory -t borrow_detail` 可观测结果 | YES |
| 3 | 事务状态访问一致性 | 通过建库建表、事务提交/回滚、并发 DML、主备读写一致性验证 CLOG/CSNLOG 访问结果正确 | YES |
| 4 | Snapshot 与 OldestXmin | 通过长事务、并发事务、VACUUM、主备查询验证 Snapshot/OldestXmin 获取路径和事务可见性符合预期 | YES |
| 5 | reform、切换与节点恢复 | 通过主备切换、在线 failover、节点 stop/start、异常恢复验证 UB 访问路径在资源池化恢复流程中的可靠性，并保留故障现场日志 | YES |
| 6 | 性能 | 通过 300+300、500+100 压测场景对比 TCP 基线和 SHM 优化后的主机、备机及总性能表现 | YES |

## 6.2测试执行评估

| 版本名称                  | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| --------------------- | --------- | ----- | ----- | ----- | --------- | ---- |
| openGauss 7.0.0- B007 | 7         | 39    | 39    | 2     | 3.22k     | 0.62 |


# 7 附件

##  7.1 附件1：遗留问题列表



##  7.2 附件2：特性相关PR

测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=d4e75620517d433ba6430f7fffe7a893
测试用例：7.0.0-LTS-鲲鹏超节点-鲲鹏超节点加速资源池化clog,csnlog
PR：
https://gitcode.com/opengauss/openGauss-server/pull/9229
https://gitcode.com/opengauss/openGauss-server/pull/9244
https://gitcode.com/opengauss/openGauss-server/pull/9246




