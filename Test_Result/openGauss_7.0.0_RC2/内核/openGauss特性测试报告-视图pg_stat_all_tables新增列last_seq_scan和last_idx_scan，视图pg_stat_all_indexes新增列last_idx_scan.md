![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2025.09.08   | V1.0       | 测试报告初稿       | xiaozhengli   |

[TOC]

**Keywords 关键词**：*pg_stat_all_tables、pg_stat_all_indexes、last_seq_scan、last_idx_scan*

**Abstract 摘要**：*本文档是对openGauss ABPD兼容性下视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan的测试。*

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|   无     |          |          |

***


# 1 概述

该测试需求的目的是增强openGauss的ABPD兼容性，实现视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan，支持查询到表或索引最近一次被扫描的时间。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------- | ---------- | ------------ | ------------ | -------- |
|       openGauss 7.0.0-RC2.B023/B024 |    openGauss-All-7.0.0-RC2-CentOS7-x86_64.tar.gz        |     2025.09.01         |       2025.09.04       |    xiaozhengli


## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | -------- | ------------------------------------------------------------ | ---- |
| 物理机       | x86_CentOS       | CPU：Intel Xeon Gold 6138 CPU @ 2.00GHz<br />内存：376G<br />硬盘：PERC H730P Mini 7.7T<br />OS：CentOS 7.9.2009<br />文件系统：XFS<br />网卡：1000Mb/s |      |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

openGauss的ABPD兼容模式下，实现视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan，支持查询到表或索引最近一次被扫描的时间。

共计执行33个用例，主要覆盖：

1、不同类型表（普通表\临时表\分区表）在不同扫描场景（seq_scan\index_scan\index_only_scan\bitmap_scan）下的被扫描时间记录；

2、不同存储格式+索引类型组合在不同扫描场景（seq_scan\index_scan\index_only_scan\bitmap_scan）下的被扫描时间记录；

3、字段数据更新情况；

4、调用新增内置函数pg_stat_force_next_flush()和pg_stat_get_lastscan()，测试正常调用及非法入参；

总结：发现问题1个，特性质量良好，遗留0个问题。

## 3.2 约束说明

1.该功能在openGauss的ABPD兼容模式下使用。

## 3.3 关键风险和规避措施
无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价


| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan  | 支持查询表或索引最近一次被顺序扫描或索引扫描的时间 | 详见3.2章节描述                         | 无                   | 功能性测试               | <font color=green>▮</font> | 无 |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

无

###  4.2.1 性能测试结论

无

###  4.2.2 可靠性测试结论

无

###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

无

###  4.2.7 兼容性测试结论

ABPD模式均支持。

###  4.2.8 升级测试结论

从无该特性版本（openGauss 7.0.0-RC2.B021）升级至openGauss 7.0.0-RC2.B023版本，升级后验证该特性功能使用正常。

## 4.3 资料测试结论

社区增加该特性文档，与设计方案描述一致，测试pr通过，https://gitcode.com/opengauss/docs/pull/7662/commit 。

# 5 测试对象质量评估

##  5.1 覆盖率分析

本特性主要从功能方面测试，验证功能与需求相符,已覆盖视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan、视图pg_stat_all_indexes新增列last_idx_scan的功能实现，覆盖新增内置函数pg_stat_force_next_flush()、pg_stat_get_lastscan()调用正常且异常入参合理报错。

##  5.2 缺陷统计和分析

*给出各特性或模块缺陷的分布或分类统计以及缺陷走势分析。建议通过表格说明，下表仅供参考，不作为强制输出方式*

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   |      1    |  0    |  0    |   1   |    0    |
| 百分比 |      100%    |  0    |   0   |  100    |    0    |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
|    7533      |   视图pg_stat_all_tables与pg_stat_all_indexes，对象未被扫描过时，查询last_seq_scan和last_idx_scan偶现初始值不为空，且拿到的时间是对象创建前的时间       |      次要    |     待回归    |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性 | 验证策略                                         | 是否按照测试策略执行 |
| ---- | ---- | ------------------------------------------------ | -------------------- |
| 1    | 视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan  | <br>1、视图定义，查看新增字段存在</br><br>2、普通表/临时表/分区表，在各扫描场景(seq_scan\index_scan\index_only_scan\bitmap_scan)下，last_seq_scan和last_idx_scan记录正确时间</br><br>3、不同存储格式（astore/sgment/cstore/ustore）+不同索引类型(btree/hash/gin)，在各扫描场景下，last_seq_scan和last_idx_scan记录正确时间（不支持情况除外）</br><br>4、pg_stat_get_lastscan、pg_stat_force_next_flush函数调用及错误入参</br> <br>5、字段数据更新--未扫描查询为null，无再次扫描不更新、事务内回滚不更新、单table多索引时表被扫描字段更新</br><br>6、权限：无权限控制</br>| YES             |
|   2   |    升级测试  |           无特性版本升级到合入特性版本后，特性功能使用正常                                       |   YES
3|兼容性测试|ABCD四种模式下验证：特性功能使用均正常|YES
4|资料测试  |           社区文档相关内容验证，文档新增内容正确                                      |   YES
##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 缺陷密度 |
| -------- | ---------------- | ---------- | ---------- | ---------- | -------- |
|     openGauss 7.0.0-RC2.B023/B024     |         5        |       33     |     33       |       1     |   0.7

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
|     33         |        33          |     32   |     1   |    0     |         0    |   100%     |       0.97     |

*其中：*

*Passed 用例执行成功*

*Failed 用例全部或部分执行结果未通过或者测试执行过程中遇到其他与本用例无关的失效事件*

*Blocked由于产品本身的问题导致用例不可执行*

*Unavailable由于软件本身以外，如环境、工具、物料、时间、人力资源等因素导致用例未执行*

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1 源代码PR： https://gitcode.com/opengauss/openGauss-server/pull/7719/

2 文档PR： https://gitcode.com/opengauss/docs/pull/7662

3 测试设计：openGauss/测试/测试设计/openGauss_7.0.0RC2/视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan

4 测试用例：openGauss/测试/测试报告/openGauss_7.0.0RC2/视图pg_stat_all_tables新增列last_seq_scan和last_idx_scan，视图pg_stat_all_indexes新增列last_idx_scan

