![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| XX   | XX       | XX       | XX   |

[TOC]

**Keywords 关键词**：*XX*

**Abstract 摘要**：*给出本阶段的测试范围、结果、分析及质量评价，同时对测试活动进行回顾总结。*

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告为datakit适配Elasticsearch和Milvus迁移特性测试报告，测试版本为datakit 7.0.0-RC3 B010，本次测试执行用例90个，发现问题一个，测试质量良好。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本


| 版本名称           | 软件包名称 | 测试起始时间     | 测试结束时间     | 测试人员 |
|----------------| ---------- |------------|------------|------|
| 7.0.0-RC3 B010 |  openGauss-Datakit-All-7.0.0-RC3.tar.gz          | 2025/12/24 | 2025/12/30 | 1    |

###  2.1.2 配套测试的版本


无

## 2.2 测试环境描述


###  2.2.1 环境硬件信息

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| openEuler release 20.03 (LTS) | CPU：Kunpeng-920 内存：30G 硬盘：1T OS：openEuler aarch64 |  |

### 2.2.2 虚拟化平台

无

### 2.2.3 OS版本

无

# 3 版本概要测试结论、关键风险和规避措施


## 3.1 测试结论总结
本次测试覆盖了datakit适配Elasticsearch和Milvus迁移从添加实例到数据迁移的全流程功能测试、datakit升级测试和资料测试，需求100%实现。本次测试执行测试用例90个，发现一个资料问题，开发质量良好。

## 3.2 约束说明

1.python版本>=3.8 

2.Elasticsearch版本>=7.3 

3.Milvus版本>=2.3 

4.openGauss版本>=7.0.0-RC1 

5.openGauss用户具有sysadmin权限

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

*建议以表格的形式汇总本阶段新特性的商用化建议（此章节除了考虑可靠性、性能、开源等风险评估之外，还需考虑基础特性安全与隐私风险），如下表格所示。*

*下表仅供参考，不作为强制输出方式。*

| 特性 | 特性价值评估                                                                                                        | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况  | 特性质量评估               | 主要风险 |
| ---- |---------------------------------------------------------------------------------------------------------------| -------------------------- |------------|-----------| -------------------------- |------|
| datakit适配ElasticSearch和Milvus迁移  | 特性提供一个高效、易用的数据迁移通道，用户可以将ElasticSearch/Milvus中的向量数据迁移到openGauss数据库中。以方便用户在不丢弃历史业务数据的情况下，无缝衔接使用openGauss的向量检索能力 | python版本>=3.8，Elasticsearch版本>=7.3 ，Milvus版本>=2.3 ，openGauss版本>=7.0.0-RC1 ，openGauss用户具有sysadmin权限                        | 无          | 功能测试、升级测试 | <font color=green>▮</font> | 无    |


## 4.2 产品功能属性测试结论


| 功能点验证                    | 是否覆盖 |
|--------------------------| ------- |
| 添加ElasticSearch/Milvus实例 | YES  |
| 安装迁移工具                   | YES   |
| 创建迁移任务                   | YES  |
| 启动迁移任务                   | YES  |
| 迁移向量数据                   | YES  |

## 4.3 产品质量属性目标(DFX)测试结论


###  4.3.1 性能测试结论

无

###  4.3.2 可靠性测试结论

无

###  4.3.3 安全&隐私保护测试结论

无

### 4.3.4 可服务性测试结论

无

### 4.3.5 生命周期管理测试结论

无

### 4.3.6 韧性测试结论

无

###  4.3.7 兼容性测试结论

无

###  4.2.8 升级测试结论


| 测试步骤                             | 升级路径                   | 测试结果 |
|----------------------------------|------------------------|------|
| 更换datakit的jar包和插件的jar包后重启datakit | 7.0.0-RC2 -> 7.0.0-RC3 | 成功   |

## 4.3 资料测试结论


| 序号 | 测试章节                                   | 测试结论                 |
| ---- |----------------------------------------|----------------------|
|      | 文档中心>向量数据库>从Milvus迁移至openGauss | 整体质量良好，发现问题均已解决 |
|      | 文档中心>向量数据库>从ElasticSearch迁移至openGauss  | 整体质量良好 |
|      | multidb-portal/README.md  | 整体质量良好 |

# 5 测试对象质量评估



##  5.1 覆盖率分析
本次测试覆盖ElasticSearch/Milvus实例添加，迁移工具安装，迁移任务的创建、执行及支持迁移的向量数据类型。开发设计文档中涉及的功能点均已覆盖

##  5.2 缺陷统计和分析


###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ |------|----|----|----|-----|
| 数目   | 1    | 0  | 0  | 1  | 0   |
| 百分比 | 100% |  0%  |  0%  |  100%  |  0%   |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- |------|------|
| https://gitcode.com/opengauss/docs/issues/7067         |    ElasticSearch迁移资料需要更新      | 次要   | 待办   |

# 6 测试过程评估


##  6.1 测试策略回顾

*回顾本阶段的测试策略，建议以表格的方式检查测试策略规定的活动是否都已经落实。*

| 编号 | 特性   | 验证策略                                 | 是否按照测试策略执行 |
|----|------|--------------------------------------| -------------------- |
| 1  | 功能测试 | 实例添加->迁移工具安装->迁移任务创建->迁移任务执行->数据类型迁移 | YES               |
| 2  | 资料测试 | 向量数据库迁移脚本资料->portal资料                |         YES              |

##  6.2 测试设计评估

无

##  6.3 测试执行评估



###  6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------- |-----------|-------|-------|-------|-----------|------|
|    openGauss-Datakit-All-7.0.0-RC3.tar.gz      | 5         | 90    | 90    | 0     | 20        | 0    |
|          |           |       |       |       |           |      |

###  6.3.2 测试用例执行结果统计数据

*对本次测试用例执行结果统计，其中的字段可根据实际情况进行设计和裁剪。对于Failed，Blocked，Unavailable的测试用例项不为0，须给出说明，并明确相应的后续计划（如：对于物料问题导致未执行用例，则需明确相应的物料计划；对于Blocked用例，需明确问题解决与相应版本的测试时间等）。对于最后一轮测试，要给出相应的规避措施。正文内容写明该需求共计多少用例*

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率  | 执行通过率 |
| ------ |---------|----------|--------|--------|---------|-------------|------|-------|
| 第一轮 | 90      | 90       | 90     | 0      | 0       | 0           | 100% | 100%  |


# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关
用例：
https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb2100010u4mjc78


特性目录/工具链/datakit适配ElasticSearch和Milvus迁移

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=e46aab47c6eb42c2823176a553c1f5aa


需求pr：

https://gitcode.com/opengauss/openGauss-migration-portal/pull/232

https://gitcode.com/opengauss/openGauss-workbench/pull/1376

https://gitcode.com/opengauss/openGauss-workbench/pull/1378

https://gitcode.com/opengauss/openGauss-workbench/pull/1379

https://gitcode.com/opengauss/community/pull/462

https://gitcode.com/opengauss/community/pull/463

https://gitcode.com/opengauss/openGauss-workbench/pull/1384

https://gitcode.com/opengauss/openGauss-workbench/pull/1386



