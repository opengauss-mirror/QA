![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| XX   | XX       | XX       | XX   |

[TOC]

**Keywords 关键词**：datakit、pg迁移

**Abstract 摘要**：本次测试主要覆盖pg迁移的全流程功能测试，以及前端易用性。开发质量良好

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

本报告为datakit支持PostgreSQL到openGauss的迁移能力特性测试报告，测试对象为Datakit-7.0.0RC2及pg迁移portal工具

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 软件包名称                   | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------- | ------------ | ------------ | -------- |
| Datakit-All-7.0.0-RC2.tar.gz | 2025/7/30    | 2025/8/7     | 1        |

###  2.1.2 配套测试的版本

无

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 硬件型号                      | 硬件配置信息                                                 | 备注 |
| ----------------------------- | ------------------------------------------------------------ | ---- |
| openEuler release 20.03 (LTS) | CPU：Intel(R) Xeon(R) Platinum 8378C CPU @ 2.80GHz<br />内存：30G<br />硬盘：100G<br />OS：openEuler x86_64 |      |

### 2.2.2 虚拟化平台

无

### 2.2.3 OS版本

无

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本阶段完成了datakit集成postgresql迁移的功能测试、易用性测试、资料测试，需求特性100%实现。本次测试发现问题4个，开发质量良好

## 3.2 约束说明

1.pg库和opengauss库的连接用户需要有创建逻辑复制槽的权限

2.wal_level=logical

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                   | 特性价值评估                                                 | 应用说明及关键约束假设依赖                                   | 关键遗留事项如缺陷等 | 测试整体覆盖情况               | 特性质量评估               | 主要风险 |
| ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- | ------------------------------ | -------------------------- | -------- |
| datakit支持PostgreSQL到openGauss的迁移能力特性测试报告 | 实现Datakit支持postgresql到opengauss的数据迁移功能，完成创建、启动和管理postgresql到opengauss的数据迁移任务的适配，并支持实时查看迁移任务进度等详情信息 | 1.pg库和opengauss库的连接用户需要有创建逻辑复制槽的权限<br />2.wal_level=logical | 无                   | 功能测试，易用性测试，资料测试 | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

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

无

###  4.2.8 升级测试结论

无

## 4.3 资料测试结论

*按照总体测试策略中制定的兼资料测试策略，给出验证结论。（如本测试阶段涉及则填写）*

*如下表格所示。下表仅供参考，不作为强制输出方式*

| 序号 | 测试章节                                                     | 测试结论     |
| ---- | ------------------------------------------------------------ | ------------ |
|      | [项目文件预览 - openGauss-migration-portal - GitCode](https://gitcode.com/opengauss/openGauss-migration-portal/blob/master/multidb-portal/README.md) | 整体质量良好 |
|      |                                                              |              |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试覆盖pg数据库实例的导入管理，pg迁移任务的创建管理，迁移详情界面信息展示，异常场景的和相应的报错提示，以及迁移工具portal的安装管理

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4    | 0      |
| 百分比 | 100%     | 0    | 0    | 100% | 0      |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1057 | [Bug]: datakit进行pg离线迁移，迁移任务完成后，主任务与子任务的任务状态不一致 | 次要     | 待回归   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1070 | [Bug]: portal缺少pg迁移前置校验项，wal_level=logical         | 次要     | 待办     |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1083 | [Bug]: pg迁移时kill -9 kafka进程后占用端口号，portal会进入死循环不停重启kafka | 次要     | 已完成   |
| https://gitcode.com/opengauss/openGauss-workbench/issues/1076 | [Bug]: datakit进行pg迁移时，在任务中心界面点击停止增量，会提示增量迁移存在异常，实际迁移正常，后台无报错 | 次要     | 已完成   |

# 6 测试过程评估

##  6.1 测试策略回顾



| 编号 | 特性       | 验证策略                                               | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------ | -------------------- |
| 1    | 功能测试   | pg数据库实例的导入管理->迁移任务的创建和管理->迁移流程 | yes                  |
| 2    | 易用性测试 | ui设置是否合理->提示信息是否合理                       | yes                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                     | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| ---------------------------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| Datakit-All-7.0.0-RC2.tar.gz | 7                | 42         | 42         | 4          | 32.541         | 0.12     |
|                              |                  |            |            |            |                |          |

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 42             | 42               | 38     | 4      | 0       | 0           | 100%   | 90%        |
| 第二轮 |                |                  |        |        |         |             |        |            |
| ...    |                |                  |        |        |         |             |        |            |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

*提供本需求相关的代码pr及资料pr*

代码及资料pr：

https://gitcode.com/opengauss/debezium/pull/326

https://gitcode.com/opengauss/openGauss-workbench/pull/1128

https://gitcode.com/opengauss/openGauss-migration-portal/pull/203

https://gitcode.com/opengauss/openGauss-workbench/pull/1153

https://gitcode.com/opengauss/openGauss-workbench/pull/1155

https://gitcode.com/opengauss/openGauss-workbench/pull/1156

https://gitcode.com/opengauss/openGauss-workbench/pull/1157

https://gitcode.com/opengauss/openGauss-workbench/pull/1162

https://gitcode.com/opengauss/openGauss-workbench/pull/1170

https://gitcode.com/opengauss/openGauss-workbench/pull/1225

https://gitcode.com/opengauss/openGauss-workbench/pull/1231

https://gitcode.com/opengauss/openGauss-workbench/pull/1232

https://gitcode.com/opengauss/openGauss-workbench/pull/1233

https://gitcode.com/opengauss/community/pull/425

测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=58df0b0096f94951a1ea614c5c8ab11f&hideDevcloudHead=true

![image-20250807195053461](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250807195053461.png)

测试用例：

![image-20250807195724886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250807195724886.png)
