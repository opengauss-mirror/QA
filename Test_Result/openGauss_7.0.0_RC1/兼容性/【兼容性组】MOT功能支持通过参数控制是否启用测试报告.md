![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者  |
| --------- | -------- | ------------ | ----- |
| 2025.3.12 | v1.0     | 测试报告初稿 | lixin |

**Keywords 关键词**：mot、mot_server

**Abstract 摘要**：主要测试MOT功能支持通过参数控制是否启用，及通过编译宏控制是否使用原来的rdtscp指令集，整体质量良好。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

本报告主要测试MOT功能支持通过参数控制是否启用，及通过编译宏控制是否使用原来的rdtscp指令集，整体质量良好，执行测试共2轮，发现issue共2个，已修复并回归通过，无遗留问题，整体质量良好。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ----------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1B017 | openGauss  | 2025-3-5     | 2025-3-12    | lixin    |
| openGauss 7.0.0-RC1B020 | openGauss  | 2025-3-12    | 2025-3-12    | lixin    |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Inter(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 4核<br/>内存：8GB<br/>硬盘：220GB<br/>OS：openEuler release 20.03(LTS)<br />架构：aarch64 |      |
| 虚拟机   | Intel(R) Xeon(R) Platinum 8378C CPU @ 2.80GHz<br />内存：16GB<br />硬盘：280GB<br />OS：CentOS Linux release 7.6.1810 (Core) <br />架构：x86_64 |      |

# 3 版本概要测试结论、关键风险和规避措施

本次特性主要测试MOT功能支持通过参数控制是否启用，及通过编译宏控制是否使用原来的rdtscp指令集，输出测试用例44个，执行测试共2轮，发现issue共2个，均已修复并回归通过，无遗留问题，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                      | 特性价值评估                                                 | 应用说明及关键约束假设依赖                             | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ | -------------------- | ---------------- | -------------------------- | -------- |
| 【兼容性】MOT功能支持通过参数控制是否启用 | 该特性来源于内部需求，主要用于MOT内部优化，默认关闭MOT引擎，解决安装过程中因rdtscp指令缺少或者numa分配不均等常见core问题，提升用户的安装使用体验。 | 若已有mot表，设置enable_mot_server=off，数据库启动失败 | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

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

若从含本需求的版本升级后续版本，升级过程中不对该参数做修改；若旧版本无mot表，升级后默认不启用mot引擎；若旧版本（版本号<7.0.0-RC1）含有mot表，新版本（版本号>=7.0.0-RC1(含该特性，>=93_xxx)），将在升级过程中修改guc参数启动mot引擎，保持升级后mot表可用。

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [前提条件](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseAdministrationGuide/%E5%89%8D%E6%8F%90%E6%9D%A1%E4%BB%B6.html) | 测试通过 |
| 2    | [内存表](https://docs.opengauss.org/zh/docs/latest/docs/DatabaseReference/%E5%86%85%E5%AD%98%E8%A1%A8.html) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖启用mot功能guc参数测试，覆盖通过编译宏控制mot功能、rdtscp指令集组合测试，覆盖小型化测试，覆盖mot功能开启，mot数据与gs_dump/ gs_restore/ gs_basebackup /gs_backup/ gs_probackup/ gs_basebackup等工具耦合测试，覆盖mot功能关闭时，以上工具恢复含mot数据/参数等场景测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 1    | 1      |
| 百分比 | 100%     | 0%   | 0%   | 50%  | 50%    |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBS5W8](https://e.gitee.com/opengaussorg/issues/list?issue=IBS5W8) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】关闭enable_mot_server，查询mot内存相关统计函数卡住 | 次要     | 已验收   |
| [IBQS8K](https://e.gitee.com/opengaussorg/issues/list?issue=IBQS8K) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】【测试类型：SQL功能】【测试版本：7.0.0-RC1】社区无enable_mot_server控制mot启用相关文档 | 不重要   | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                                     | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------------------------------------- | ------------------------------------------------------------ | -------------------- |
| 1    | MOT功能支持通过参数控制是否启用          | 启用mot功能guc参数测试，mot数据与gs_dump/ gs_restore/ gs_basebackup /gs_backup/ gs_probackup/ gs_basebackup等工具耦合测试，覆盖mot功能关闭时，以上工具恢复含mot数据/参数等场景测试 | YES                  |
| 2    | 通过编译宏控制是否使用原来的rdtscp指令集 | 通过编译宏控制mot功能、rdtscp指令集组合测试，及小型化测试等  | YES                  |

##  6.2 测试设计评估

已评审并归档Testplan，归档地址详见附件

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC1 B017 | 6                | 44         | 44         | 2          | 1.54   | 0.65     |
| openGauss 7.0.0-RC1 B020 | 1                | 2          | 2          | 0          | 1.54   | 0.65     |

本次测试共发现2个issue，其中1个为资料单，不计入缺陷密度，所有issue已修复并回归通过，缺陷密度为1/1.54k=0.65，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 44           | 44               | 39     | 2      | 0       | 3           | 100%   | 95%        |
| 2            | 2                | 2      | 2      | 0       | 0           | 100%   | 100%       |

本次测试共输出测试用例44个，执行测试共2轮，发现issue共2个，其中1个为资料单，已修复并回归通过，无遗留问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-OM/pulls/1011

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7199

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7221

资料pr：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/7330

测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=bb9a376c4b2b4b7382b755ffc8cef524&hideDevcloudHead=true

测试用例：testplan _ openGauss7.0.0 _ 数据库服务 _ MOT功能支持通过参数控制是否启用

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg



