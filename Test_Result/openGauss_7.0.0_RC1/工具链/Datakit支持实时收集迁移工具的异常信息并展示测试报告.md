![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| XX   | XX       | XX       | XX   |

[TOC]

**Keywords 关键词**：迁移告警

**Abstract 摘要**：Datakit迁移任务支持实时收集后端各个异常过程中的异常、告警等信息并展示到前端页面上

**缩略语清单： 无

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告是Datakit支持实时收集迁移工具的异常信息并展示的测试总结，测试对象主要为datakit 7.0.0-RC1，portal 7.0.0-RC1

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称           | 软件包名称 | 测试起始时间     | 测试结束时间   | 测试人员 |
|----------------|---|------------|----------|------|
| 7.0.0-RC1 B010 | Datakit-7.0.0-RC1.tar.gz  | 2024/12/24 | 2025/1/8 | 姚雨驰  |

###  2.1.2 配套测试的版本


| 版本名称 | 配套版本       | 版本说明 |
| -------- | -------------- |------|
| 7.0.0-RC1 B010      |PortalControl-7.0.0rc1-aarch64.tar.gz  | B010 |

## 2.2 测试环境描述



###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号     | 硬件配置信息                                                                                   | 备注 |
|------|----------|------------------------------------------------------------------------------------------| ---- |
| 物理机  | aarch64+openEuler20.03 | CPU：Kunpeng 920 7260 2p 128核<br />内存：1021G<br />硬盘：3.0T<br />OS：openEuler release 20.03 (LTS-SP1)<br /> |      |
|物理机  | aarch64+openEuler20.03 | CPU：Kunpeng 920 7260 2p 128核<br />内存：1021G<br />硬盘：3.0T<br />OS：openEuler release 20.03 (LTS)<br /> |      |
|物理机  | aarch64+openEuler20.03 | CPU：Kunpeng 920 7260 2p 128核<br />内存：1021G<br />硬盘：3.0T<br />OS：openEuler release 20.03 (LTS)<br /> |      |



### 2.2.2 虚拟化平台

不涉及

### 2.2.3 OS版本

# 3 版本概要测试结论、关键风险和规避措施


## 3.1 测试结论总结

7.0.0-RC1 B010版本测试完成了功能测试、兼容性测试、功能测试，需求实现100%。所有被测特性的测试用例的累计执行覆盖率达到了100%，总计发现问题4个，开发质量良好，测试中发现的问题都已修复，回归测试通过。
## 3.2 约束说明

datakit和potal的版本均为7.0.0-RC1版本及以上时才支持该功能

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价


| 特性 | 特性价值评估                                                  | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ---- |---------------------------------------------------------|---------------|------------|----------| -------------------------- |------|
| Datakit支持实时收集迁移工具的异常信息并展示  | 能有效捕捉到迁移过程中报错的情况，让客户在前端能够直观的了解问题原因，同时能在一定程度上对迁移功能进行问题拦截 | 无             | 无          | 100%     | <font color=green>▮</font> | 无    |


## 4.2 产品质量属性目标(DFX)测试结论


###  4.2.1 性能测试结论

*按照总体测试策略中对产品质量属性目标分析和执行策略，完成相应质量属性目标评估；总体测试策略中有要求但验证未执行的需给出说明。（如本测试阶段涉及则填写）下表仅供参考，不作为强制输出方式*

| 测试步骤                                                                     | 测试结果          |
|--------------------------------------------------------------------------|---------------|
| 1.MySQL建表插入数据，10张表，每张表3000000万</br>2.启动离线迁移                              | 全量迁移性能387MB/s |
| 1.MySQL建表</br>2.启动在线迁移</br>3.增量迁移阶段使用sysbench向mysql压入数据，50张表、50线程、压测120秒 | 增量迁移性能>3Wtps  |
| 1.MySQL建表</br>2.启动在线迁移</br>3.反向迁移阶段使用sysbench向mysql压入数据，50张表、50线程、压测120秒 | 反向迁移性能>3Wtps  |

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



| 测试步骤                                              | 测试结果 |
|---------------------------------------------------|------|
| 1.使用6.0.0Datakit和7.0.0-RC1portal进行迁移任务</br> | 迁移正常 |
| 1.使用7.0.0-RC1Datakit和6.0.0portal进行迁移任务</br> | 迁移正常 |

###  4.2.8 升级测试结论



| 测试步骤                                             | 升级路径             | 测试结果 |
|--------------------------------------------------|------------------|------|
| 1、 6.0.0datakit更换7.0.0-RC1.jar包</br>2、 重启datakit | 6.0.0->7.0.0-RC1 | 升级成功 |

## 4.3 资料测试结论



| 序号 | 测试章节                                        | 测试结论   |
|----|---------------------------------------------|--------|
| 1  | workbench仓库plugins/data-migration/README.md | 整体质量良好 |

# 5 测试对象质量评估


##  5.1 覆盖率分析

本次测试包括了datakit迁移的功能测试、性能测试、兼容性测试，功能覆盖在线迁移、离线迁移、全量迁移、增量迁移、反向迁移以及各迁移工具的的异常报错场景的异常捕捉。
##  5.2 缺陷统计和分析



###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ |------|----|----|----|-----|
| 数目   | 4    | 0  | 0  | 4  | 0   |
| 百分比 | 100% | 0  | 0  |  100%  | 0   |

###   5.2.2 缺陷列表

| 问题单号 | 问题链接                                                       | 问题描述 | 问题级别 | 当前状态 |
| -------- |------------------------------------------------------------| -------- |------|------|
|   IBFY69       | https://e.gitee.com/opengaussorg/issues/table?issue=IBFY69 |   【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】迁移异常捕获kafka内存溢出异常失败       | 次要   | 已验收  |
| IBF95Y       |https://e.gitee.com/opengaussorg/issues/table?issue=IBF95Y  | 【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】迁移告警异常显示位置不正确          |次要   | 已验收  |
| IBE8YK    |https://e.gitee.com/opengaussorg/issues/table?issue=IBE8YK       | 【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】datakit部分迁移异常未捕获并展示到前端          |次要  | 已验收  |
| IBF3QU   |https://e.gitee.com/opengaussorg/issues/table?issue=IBF3QU       | 【测试类型：工具功能】【测试版本：datakit 7.0.0-RC1】告警详情界面框易用性不足，需要优化 | 次要   | 已验收 |

# 6 测试过程评估



##  6.1 测试策略回顾

*回顾本阶段的测试策略，建议以表格的方式检查测试策略规定的活动是否都已经落实。*

| 编号 | 特性             | 验证策略                                           | 是否按照测试策略执行 |
|----|----------------|------------------------------------------------| -------------------- |
| 1  | 界面易用性          | UI是否合理，提示信息和报错信息是否合理                    | YES               |
| 2  | datakit升级及前向兼容 | datakit升级功能是否正常、不同版本之间的兼容性是否正常                 |    YES         |
| 3  | 不同迁移阶段异常捕捉     | 根据官网资料给出的迁移约束，构造异常场景，看出异常能否被捕捉                 |    YES            |
| 4  | 不同工具抛出的异常捕捉    | 验证portal、chameleon、debezium、kafka工具内部抛出的异常能否捕捉 |      YES        |
| 5  | 迁移性能           | 各阶段迁移性能是否有劣化                                   |         YES  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

*提供对本次测试活动的测试执行过程的评估结论。描述对测试执行活动的改进建议，以供后续测试执行活动中借鉴参考；*

*测试执行活动评估结论可依据以下提供的“测试执行统计数据”及“测试用例执行结果统计数据”进行分析而给出。*

###  6.3.1 测试执行统计数据

| 版本名称           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
|----------------|-----------|-------|-------|-------|-----------|------|
| 7.0.0-RC1 B010 | 10        | 64    | 64    | 4  | 4.787     | 0.8/KLOC |


###  6.3.2 测试用例执行结果统计数据

*对本次测试用例执行结果统计，其中的字段可根据实际情况进行设计和裁剪。对于Failed，Blocked，Unavailable的测试用例项不为0，须给出说明，并明确相应的后续计划（如：对于物料问题导致未执行用例，则需明确相应的物料计划；对于Blocked用例，需明确问题解决与相应版本的测试时间等）。对于最后一轮测试，要给出相应的规避措施。正文内容写明该需求共计多少用例*

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率  | 执行通过率 |
| ------ |---------|----------|--------|--------|---------|-------------|------|-------|
| 第一轮 | 64      | 64       | 54     | 4      | 0       | 6           | 100% | 84%   |
| 第二轮 | 58      | 58       | 58     | 0      | 0       | 0           | 100% | 100%  |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

代码PR:</br>
datachecker适配异常日志收集框架 </br>
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-tools-datachecker-performance/pulls/216</br>
debezium适配异常信息收集框架</br> https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/268</br>
chameleon适配异常信息收集框架及日志适配</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-tools-chameleon/pulls/264</br>
【feature】Debezium适配错误日志收集框架</br>https://e.gitee.com/opengaussorg/repos/opengauss/debezium/pulls/270</br>
数据迁移异常信息收集需求提交</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/981</br>
portal适配异常信息收集</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-migration-portal/pulls/175</br>
数据迁移展示异常信息前端修改</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/982</br>
Datakit校验适配异常收集需求</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-tools-datachecker-performance/pulls/217</br>
修复残留告警文件未清空的问题</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-migration-portal/pulls/176</br>
</br>资料PR</br>迁移异常信息收集特性说明提交</br>https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-workbench/pulls/987</br>

[测试设计-TestPlan](https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=a3cff91060f945ccb5bc1533e8b2764a&hideDevcloudHead=true)

测试用例-TestPlan![image-20250124160108683](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250124160108683.png)



