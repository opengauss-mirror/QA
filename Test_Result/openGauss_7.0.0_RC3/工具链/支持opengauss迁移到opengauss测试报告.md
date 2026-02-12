![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期        | 修订版本 | 修改描述 | 作者  |
|-----------|------| -------- |-----|
| 2026.2.11 | 1.0  |        | 姚雨驰 |

[TOC]

**Keywords 关键词**：*XX*

**Abstract 摘要**：*给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。*

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告为支持opengauss迁移到opengauss特性测试报告，主要测试opengauss全量迁移至opengauss功能，覆盖传统om集群和资源池化集群之间的迁移，涉及opengauss5.0.5，6.0.3，7.0.0-RC3版本


# 2 测试版本说明


## 2.1 测试版本信息

###   2.1.1 被测版本

*本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段*

| 版本名称      | 软件包名称 | 测试起始时间   | 测试结束时间    | 测试人员 |
|-----------| ---------- |----------|-----------|------|
| 7.0.0-RC3 |openGauss-FullReplicate-7.0.0rc3.tar.gz   | 2026.2.2 | 2026.2.11 | 1    |

###  2.1.2 配套测试的版本

*描述发布版本所涉及到的配套版本信息，包括配套产品版本、配套工具版本、内嵌平台配套版本等。针对版本配套表中唯一版本的，可简略说明，如果不唯一或是市场一线的特别要求，就要注明主要的相关产品配套版本信息（不支持或支持的特殊性等）。建议使用以下表格说明，可自行增减表中字段*

| 版本名称      | 配套版本       | 版本说明 |
|-----------| -------------- | -------- |
| 5.0.5     |openGauss-5.0.5-openEuler-64bit-all.tar.gz   |          |
| 6.0.3     |openGauss-All-6.0.3-openEuler20.03-aarch64.tar.gz                  |          |
| 7.0.0-RC3 |openGauss-All-7.0.0-RC3-openEuler22.03-aarch64.tar.gz   |          |


## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 硬件型号                          | 硬件配置信息                                                 | 备注 |
|-------------------------------| ------------------------------------------------------------ | ---- |
| openEuler release 20.03 (LTS) | CPU：Kunpeng-920 内存：30G 硬盘：1T OS：openEuler aarch64 |  |
| openEuler release 22.03 (LTS) | CPU：Kunpeng-920 内存：30G 硬盘：1T OS：openEuler aarch64 |  |

### 2.2.2 虚拟化平台

### 2.2.3 OS版本


# 3 版本概要测试结论、关键风险和规避措施


## 3.1 测试结论总结

本次测试覆盖opengauss数据库的数据类型迁移测试，表类型迁移测试，索引迁移测试，对象迁移测试，以及迁移工具参数测试，所有被测特性的测试用例的累计执行177个用例，执行覆盖率达到100%，发现问题19个，开发质量一般

## 3.2 约束说明

数据库需要配置白名单

## 3.3 关键风险和规避措施

无
# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价



| 特性                      | 特性价值评估                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况          | 特性质量评估               | 主要风险 |
|-------------------------|------------------------| -------------------------- |------------|-------------------| -------------------------- |------|
| 支持opengauss迁移至opengauss | 提供池化环境和传统om之间进行数据迁移的功能 | 数据库需要配置白名单                         | 无          | 数据类型，表类型，索引及数据库对象 | <font color=green>▮</font> | 无    |


## 4.2 产品功能属性测试结论


| 功能点验证   | 是否覆盖 |
|---------|------|
| 数据类型迁移  | YES  |
| 表类型迁移   | YES  |
| 索引迁移    | YES  |
| 数据库对象迁移 | YES  |
| 迁移工具测试  | YES  |



## 4.3 产品质量属性目标(DFX)测试结论


###  4.3.1 性能测试结论

不涉及

###  4.3.2 可靠性测试结论

不涉及

###  4.3.3 安全&隐私保护测试结论

不涉及
### 4.3.4 可服务性测试结论

不涉及

### 4.3.5 生命周期管理测试结论

不涉及

### 4.3.6 韧性测试结论

不涉及

###  4.3.7 兼容性测试结论

不涉及

###  4.2.8 升级测试结论

不涉及

## 4.3 资料测试结论



| 序号 | 测试章节                                    | 测试结论 |
|----|-----------------------------------------|------|
| 1  | debezium -> migration-tool -> README.md | 质量一般 |

# 5 测试对象质量评估



##  5.1 覆盖率分析


本次测试主要覆盖数据类型迁移测试，表类型迁移测试，索引迁移测试，数据库对象迁移测试，迁移工具测试，池化环境与传统主备环境迁移测试，及不同版本的数据库间的迁移测试

##  5.2 缺陷统计和分析


###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要   | 次要   | 不重要  |
| ------ |------|----|------|------|------|
| 数目   | 19   | 0  | 0    | 19   | 0    |
| 百分比 | 100%  | 0% | 0% | 100% | 0% |

###   5.2.2 缺陷列表

| 问题单号                                              | 问题描述                                           | 问题级别 | 当前状态 |
|---------------------------------------------------|------------------------------------------------|------|------|
| https://gitcode.com/opengauss/debezium/issues/255 | [Bug]: opengauss迁移外键表时报错                       | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/254 | [Bug]: opengauss迁移索引时，将local索引识别为global索引      | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/253 | [Bug]: opengauss迁移point类型的gist索引时报错            | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/252 | [Bug]: opengauss迁移时表中有自定义类型列时，迁移报错             | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/251 | [Bug]: opengauss迁移序列时，large sequence未迁移        | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/250 | [Bug]: opengauss迁移无法识别并迁移物化视图                  | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/249 | [Bug]: opengauss迁移存储过程时报错                      | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/248 | [Bug]: 资料：opengauss迁移不支持部分类型的表，资料需要进行说明        | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/247 | [Bug]: opengauss迁移时，部分表的with参数未迁移              | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/246 | [Bug]: opengauss迁移时， 使用create index命令创建的索引未被迁移 | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/245 | [Bug]: opengauss迁移CLOB、xml类型时数据丢失              | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/244 | [Bug]: opengauss迁移SMALLSERIAL类型报错              | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/243 | [Bug]: opengauss迁移blob类型数据时，会给原数据加上'\x',导致迁移失败 | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/242 | [Bug]: opengauss迁移时对bit类型数据识别错误                | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/241 | [Bug]: opengauss迁移部分类型，精度异常                    | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/240 | [Bug]: opengauss迁移B库迁移失败                       | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/239 | [Bug]: opengauss迁移列存表失败                        | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/238 | [Bug]: openGauss迁移表时，Description列注释没有迁移        | 次要   | 待办   |
| https://gitcode.com/opengauss/debezium/issues/237 | [Bug]: opengauss迁移NUMERIC类型时报错                 | 次要   | 待办   |

# 6 测试过程评估


##  6.1 测试策略回顾

*回顾本阶段的测试策略，建议以表格的方式检查测试策略规定的活动是否都已经落实。*

| 编号 | 特性      | 验证策略                                                           | 是否按照测试策略执行 |
|----|---------|----------------------------------------------------------------|------------|
| 1  | 数据迁移测试  | 数据类型 -> 表 -> 索引 -> 数据库对象                                       | YES        |
| 2  | 迁移工具测试  | 表 -> 主键 -> 外键 -> 索引 -> 序列 -> 存储过程 -> 视图 -> 函数 -> 触发器           | YES        |
| 3  | 不同版本间迁移 | 5.0.5传统OM集群迁移至7.0.0-RC3资源池化集群 -> 6.0.3资源池化集群迁移至7.0.0-RC3传统OM集群 | YES        |

##  6.2 测试设计评估

*提供对本次测试活动的测试设计的评估。可结合测试设计评审建议；评审完成，测试过程中，发现的新增或删减的测试点进行说明。*

| 编号 | 测试点修改说明                           | 修改原因           | 是否影响测试质量 |
|----|-----------------------------------|----------------|----------|
| 1  | 列存表、ustore表等表类型改为在传统集群迁移至传统集群进行测试 | 资源池化不支持这部分类型的表 | 否        |

##  6.3 测试执行评估


###  6.3.1 测试执行统计数据

| 版本名称      | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
|-----------|-----------|-------|-------|-------|-----------|------|
| 7.0.0-RC3 | 8         | 177   | 177   | 19    | 2.008     | 9.46 |

###  6.3.2 测试用例执行结果统计数据


|     | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率  | 执行通过率 |
|-----|---------|----------|--------|--------|---------|-------------|------|-------|
| 第一轮 | 177     | 177      | 142    | 35     | 0       | 0           | 100% | 80%   |



# 7 附件

##  7.1 附件1：遗留问题列表
无

##  7.2 附件2：特性相关PR

代码PR：

https://gitcode.com/opengauss/debezium/pull/339

https://gitcode.com/opengauss/debezium/pull/337

https://gitcode.com/opengauss/debezium/pull/333

https://gitcode.com/opengauss/debezium/pull/332

https://gitcode.com/opengauss/debezium/pull/331

测试用例：

    版本：openGauss_7.0.0RC3
    目录：特性目录/工具链/支持opengauss迁移至opengauss
链接：https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb2100010u4mjc78

测试设计：
    
    版本：基线
    目录：支持opengauss迁移opengauss
链接：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/home
    


