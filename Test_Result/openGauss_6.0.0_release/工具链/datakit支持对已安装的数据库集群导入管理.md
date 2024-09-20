![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者     |
| --------- | -------- | ------------ | -------- |
| 2024.9.19 | v1.0     | 测试报告初稿 | lihongji |

**Keywords 关键词**：datakit、已安装企业版、轻量版、极简版数据库单机、主备批量导入

**Abstract 摘要**：datakit通过文件模板批量导入已安装好的数据库，包含功能测试、资料测试，整体质量良好。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

测试通过datakit对已安装企业版、轻量版、极简版数据单机、主备通过模板文件批量导入。om、cm集群导入

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 数据库版本

| 版本名称                        | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 6.0.0 build f0b05404  | openGauss  | 2024-7-15    | 2024-7-31    | lihongji |
| openGauss 6.0.0 build  6d91e117 | openGauss  | 2024-8-01    | 2024-8-15    | lihongji |

###  2.1.2 datakit版本

| 版本名称 | 配套版本      | 版本说明 |
| -------- | ------------- | -------- |
| datakit  | datakit 6.0.0 |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 硬件型号          | 硬件配置信息                                                 | 备注                |
| ----------------- | ------------------------------------------------------------ | ------------------- |
| aarch64+openEuler | CPU：Kunpeng-920 2600MHz 内存：765GB 硬盘：3TB OS：openEuler release 20.03 (LTS-SP1) | 部署openGauss数据库 |
| aarch64+openEuler | CPU：Kunpeng-920 2600MHz 内存：765GB 硬盘：3TB OS：openEuler release 20.03 (LTS-SP1) | 部署datakit         |

# 3 版本概要测试结论、关键风险和规避措施

通过datakit对已安装的企业版、轻量版、极简版数据单机、主备通过模板文件批量导入。om、cm集群导入。包含功能测试、资料测试，输出测试用例18个，执行测试共2轮，发现issue共6个，均已修复并回归通过，无遗留问题，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                              | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【工具链】Datakit支持对已安装的数据库集群导入管理 | datakit支持按文件导入方式，批量添加集群以及集群导入信息，可以有效提高已有集群导入效率 | 无                         | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

无

### 4.2.2 可靠性测试结论

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

资料与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [datakit批量导入已有集群](https://gitee.com/opengauss/openGauss-workbench/blob/1b290127687bc795ae48edbd6bf130bc3b8b35c2/plugins/base-ops/doc/DataKit%20Product%20Manual%20-%20Base%20OPS.md) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖cm、om集群导入，单机、主备导入，企业版、轻量版、极简版数据库导入。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6        | 0    | 1    | 4    | 1      |
| 百分比 | 100%     | 0%   | 16%  | 68%  | 16%    |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IACWUV](https://gitee.com/opengauss/openGauss-workbench/issues/IACWUV?from=project-issue) | 【测试类型：工具功能】【测试版本：6.0.0】批量导入cm集群，错误报告信息提示check node status error!实际集群各节点状态正常 | 不重要   | 已验收   |
| [IAD42L](https://gitee.com/opengauss/openGauss-workbench/issues/IAD42L?from=project-issue) | 【测试类型：工具功能】【测试版本：6.0.0】导入失败后再次导入，ops_cluster_node表未回滚错误数据且导入失败的提示信息不准确 | 次要     | 已验收   |
| [IAD8WA](https://gitee.com/opengauss/openGauss-workbench/issues/IAD8WA?from=project-issue) | 【测试类型：工具功能】【测试版本：6.0.0】极简版、轻量版通过批量导入集群信息失败 | 次要     | 已验收   |
| [IAE0BL](https://gitee.com/opengauss/openGauss-workbench/issues/IAE0BL?from=project-issue) | 【测试类型：工具功能】【测试版本：6.0.0】备节点ip不在服务器管理页面应导入失败，提示把备节点ip在服务器管理页面添加上 | 次要     | 已验收   |
| [IAE8YX](https://gitee.com/opengauss/openGauss-workbench/issues/IAE8YX?from=project-issue) | 【测试类型：工具功能】【测试版本：6.0.0】安装用户名错误、企业版环境变量路径错误，上传导入解析失败无法下载错误报告。极简版、轻量版环境变量路径错误导入集群成功 | 次要     | 已验收   |
| [IAEK9I](https://gitee.com/opengauss/openGauss-workbench/issues/IAEK9I?from=project-issue) | 【测试类型：安全】【测试版本：6.0.0】在datakit导入数据库时，日志visualtool-main.out打印数据库明文密码 | 主要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性     | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | -------- | ------------------------------------------------------------ | -------------------- |
| 1    | 功能测试 | cm、om集群导入，单机、主备导入，企业版、轻量版、极简版数据库导入 | 是                   |
| 2    | 资料测试 | 资料介绍与实现功能一致                                       | 是                   |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称             | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| -------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 6.0.0 B022 | 8                | 35         | 35         | 7          | 1K     | 7        |
| openGauss 6.0.0 B025 | 4                | 7          | 7          | 0          | 3.5K   | 0        |

本次测试共发现6个issue，均已修复并回归通过，缺陷密度为6/1.3k=4.6，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 18           | 18               | 12     | 6      | 0       | 0           | 100%   | 67%        |
| 18           | 6                | 6      | 0      | 0       | 0           | 100%   | 100%       |

本次测试共输出测试用例18个，执行测试共2轮，发现issue共6个，均已修复并回归通过，无遗留问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://gitee.com/opengauss/openGauss-workbench/pulls/719

https://gitee.com/opengauss/openGauss-workbench/pulls/718

https://gitee.com/opengauss/openGauss-workbench/pulls/711



资料pr：

https://gitee.com/opengauss/openGauss-workbench/pulls/799



测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=6d06c590efdd41b6980debf7ef1eda02&hideDevcloudHead=true
