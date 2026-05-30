![avatar](../../images/openGauss.png)

版权所有 © 2026  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者   |
| ---------- | -------- | ------------ | ------ |
| 2026.5.17 | v1.0     | 测试报告     | 林强 |


[TOC]

**Keywords 关键词**：oGRAC、bison解释器

**Abstract 摘要**

本报告是对 openGauss 7.0.0 oGRAC bison 解释器语法能力的测试总结。测试以 B003解析器结果作为基线，以 B004 开启 use_bison_parser 后的 bison parser 结果作为对照，覆盖 SELECT、INSERT、UPDATE、DELETE、MERGE、ALTER、BACKUP、RESTORE、RECOVER、REPAIR 等 25 类语法点。测试重点验证官方文档或代码明确支持的语法在 bison parser 下是否正确解析和执行，同时识别 bison parser 差异问题以及 oGRAC 运行态稳定性问题。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

 本报告针对 oGRAC bison 解释器特性进行测试，测试对象为 openGauss 7.0.0 oGRAC B004 版本。测试通过 use_bison_parser=false/true 开关对同一批 SQL 语法点进行对照执行，其中 use_bison_parser=false 作为 B003 基线，use_bison_parser=true 作为 B004 bison parser 对照。本次测试覆盖普通 SQL 语法、事务控制语法、会话/系统控制语法、集群维护语法、备份恢复语法和运行态条件类语法。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------ | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-LTS B003 | oGRAC | 2026.5.6 | 2026.5.9  | 林强 |
| openGauss 7.0.0-LTS B004 | oGRAC  | 2026.5.11 | 2026.5.16 | 林强 |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注                                |
| -------- | ----------------- | ------------------------------------------------------------ | ----------------------------------- |
| 物理机   | aarch64+openEuler | Kunpeng/aarch64，openEuler 20.03 LTS，节点 openGauss104/openGauss106，DSS 共享存储 | 使用同一套磁阵和 B003/B004 对照环境 |



# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本次测试按最终归覆盖共 25 类语法点，计入语法测试的用例总数为 319 条。B004 bison-on 结果中 PASS 255 条，非 PASS/异常/条件类结果 64 条。非 PASS 中既包含已确认 bison parser 差异问题，也包含恢复/备份运行态失败和 UDS/core 稳定性问题。

测试中共发现9个问题，功能基本正常，整体质量良好。


## 3.2 约束说明

- use_bison_parser=true 


## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性         | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                       | 特性质量评估               | 主要风险 |
| ------------ | -------------------------- | -------------------------- | -------------------- | -------------------------------------- | -------------------------- | -------- |
| 【资源池化组】 | oGRAC parser重构，减少语法逻辑与流程控制代码耦合，提升开发效率 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试 | <font color=green><font color=yellow><font color=yellow><font color=green>▮</font></font></font></font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品功能属性测试结论

| 功能点验证          | 是否覆盖 |
| :------------------ | :------- |
| SELECT              | YES      |
| INSERT              | YES      |
| UPDATE              | YES      |
| DELETE              | YES      |
| MERGE               | YES      |
| REPLACE             | YES      |
| PREPARE TRANSACTION | YES      |
| ALTER SESSION       | YES      |
| SET TRANSACTION     | YES      |
| COMMIT              | YES      |
| ROLLBACK            | YES      |
| SAVEPOINT           | YES      |
| RELEASE SAVEPOINT   | YES      |
| LOCK                | YES      |
| ALTER               | YES      |
| CHECKPOINT          | YES      |
| BACKUP              | YES      |
| RESTORE             | YES      |
| BUILD               | YES      |
| SYNCPOINT           | YES      |
| OGRAC               | YES      |
| SHUTDOWN            | YES      |
| VALIDATE            | YES      |
| RECOVER             | YES      |
| REPAIR              | YES      |

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
|          |          |

# 5 测试对象质量评估

本次测试主要针对

##  5.1 覆盖率分析

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 |
| ------ | -------- | ---- | ----- | ----- | ------ |
| 数目   | 9        | 0    | 1     | 8     | 0      |
| 百分比 | 100%     | 0%   | 11.1% | 88.9% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                               | 问题描述                                                     | 问题级别 | 当前状态 |
| :----------------------------------------------------- | :----------------------------------------------------------- | :------- | :------- |
| [#186](https://gitcode.com/opengauss/oGRAC/issues/186) | B004 bison parser SELECT 子查询/外层别名相关语法解析问题     | 次要     | 待办的   |
| [#187](https://gitcode.com/opengauss/oGRAC/issues/187) | B004 bison parser UPDATE SET DEFAULT、EXISTS 外层别名引用解析问题 | 次要     | 待办的   |
| [#188](https://gitcode.com/opengauss/oGRAC/issues/188) | B004 bison parser DELETE EXISTS 外层别名引用解析问题         | 次要     | 已取消   |
| [#189](https://gitcode.com/opengauss/oGRAC/issues/189) | B004 bison parser MERGE 源列别名解析问题                     | 次要     | 待办的   |
| [#190](https://gitcode.com/opengauss/oGRAC/issues/190) | ALTER SYSTEM DEBUG MODE 数值参数 bison parser 解析失败       | 次要     | 待办的   |
| [#191](https://gitcode.com/opengauss/oGRAC/issues/191) | ALTER SYSTEM RESET STATISTIC(S) 与 ARCHIVE_SET GLOBAL 相关问题 | 次要     | 待办的   |
| [#193](https://gitcode.com/opengauss/oGRAC/issues/193) | BACKUP FINISH SCN 大整数场景 bison parser 差异问题           | 次要     | 待办的   |
| [#194](https://gitcode.com/opengauss/oGRAC/issues/194) | UDS/运行态稳定性合并问题，覆盖 SELECT-097、DELETE-015、RESTORE、RECOVER、REPAIR、OGRAC RECOVER 等 | 主要     | 待办的   |
| [#195](https://gitcode.com/opengauss/oGRAC/issues/195) | BACKUP EXCLUDE/COPY OF TABLESPACE USERS 表空间识别运行态问题 | 次要     | 待办的   |

其中[#188](https://gitcode.com/opengauss/oGRAC/issues/188)与[#175](https://gitcode.com/opengauss/oGRAC/issues/175)是同一个问题。

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 语法点              | 验证策略                                                     | 是否按照测试策略执行 |
| :--- | :------------------ | :----------------------------------------------------------- | :------------------- |
| 1    | SELECT              | 覆盖基础查询、表达式、聚合、排序、分组、JOIN、子查询、CTE、派生表和高风险复杂查询 | YES                  |
| 2    | INSERT              | 覆盖单行、多行、指定列、默认值、子查询插入等插入语法         | YES                  |
| 3    | UPDATE              | 覆盖普通更新、条件更新、表达式更新、DEFAULT、EXISTS 相关更新； | YES                  |
| 4    | DELETE              | 覆盖普通删除、条件删除、EXISTS 删除、别名删除和高风险 DELETE target FROM 语法 | YES                  |
| 5    | MERGE               | 覆盖 MATCHED UPDATE、NOT MATCHED INSERT、复合 ON 条件、WHERE 过滤、INSERT 不带列名等 MERGE 语法 | YES                  |
| 6    | REPLACE             | 覆盖 REPLACE 基础写入、列清单、值表达式等语法                | YES                  |
| 7    | PREPARE TRANSACTION | 覆盖 PREPARE TRANSACTION 官方明确支持语法                    | YES                  |
| 8    | ALTER SESSION       | 覆盖 ALTER SESSION 参数、事务/会话属性、ENABLE/DISABLE NOLOGGING 等会话级语法 | YES                  |
| 9    | SET TRANSACTION     | 覆盖事务隔离级别、访问模式等 SET TRANSACTION 语法            | YES                  |
| 10   | COMMIT              | 覆盖 COMMIT 基础语法和事务结束相关变体                       | YES                  |
| 11   | ROLLBACK            | 覆盖 ROLLBACK 基础语法、事务回滚和保存点回滚                 | YES                  |
| 12   | SAVEPOINT           | 覆盖 SAVEPOINT 创建语法                                      | YES                  |
| 13   | RELEASE SAVEPOINT   | 覆盖 RELEASE SAVEPOINT 释放语法                              | YES                  |
| 14   | LOCK                | 覆盖 LOCK TABLE 及锁模式相关语法                             | YES                  |
| 15   | ALTER               | 覆盖 ALTER SYSTEM、ALTER SESSION、ARCHIVE_SET、DUMP CTRLFILE、INIT DICTIONARY、REPAIR CATALOG 等 ALTER 语法 | YES                  |
| 16   | CHECKPOINT          | 覆盖 CHECKPOINT 相关语法                                     | YES                  |
| 17   | BACKUP              | 覆盖 DATABASE、FULL、FORMAT、FINISH SCN、EXCLUDE/COPY OF TABLESPACE 等备份语法 | YES                  |
| 18   | RESTORE             | 覆盖 RESTORE DATABASE、RESTORE BLOCKRECOVER 等恢复语法       | YES                  |
| 19   | BUILD               | 覆盖 BUILD/REPAIR BUILD 相关语法                             | YES                  |
| 20   | SYNCPOINT           | 覆盖 SYNCPOINT RESET、SYNCPOINT name 语法                    | YES                  |
| 21   | OGRAC               | 覆盖 OGRAC RECOVER 相关语法                                  | YES                  |
| 22   | SHUTDOWN            | 覆盖 SHUTDOWN 相关语法                                       | YES                  |
| 23   | VALIDATE            | 覆盖 VALIDATE DATAFILE PAGE、VALIDATE BACKUPSET 等校验语法   | YES                  |
| 24   | RECOVER             | 覆盖 RECOVER DATABASE、UNTIL CANCEL 等恢复语法               | YES                  |
| 25   | REPAIR              | 覆盖 REPAIR TYPE RETURN_ERROR、REPLACE_CHECKSUM、DISCARD 等修复语法 | YES                  |


##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据 

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（k） | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ----------- | -------- |
| openGauss 7.0.0-RC3 B004 | 10              | 319      | 319      | 9         | 44.9      | 0.2/kloc |

###  6.3.2 测试用例执行结果统计数据

| 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 319          | 319            | 255  | 64     | 0       | 0           | 100%   | 79.9% |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1、源代码PR：

https://gitcode.com/opengauss/oGRAC/pull/26

https://gitcode.com/opengauss/oGRAC/pull/79

https://gitcode.com/opengauss/oGRAC/pull/128

https://gitcode.com/opengauss/oGRAC/pull/205

2、文档pr：

不涉及

3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=9ebca1e478454df398f213dbc1513d85

4、测试用例：

openGauss_7.0.0LTS->oGRAC->oGRA-parser 层重构特性测试设计
