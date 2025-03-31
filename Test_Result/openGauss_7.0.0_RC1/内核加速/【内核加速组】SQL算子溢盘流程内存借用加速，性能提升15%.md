![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期      | 修订版本 | 修改描述     | 作者  |
| --------- | -------- | ------------ | ----- |
| 2025.2.28 | v1.0     | 测试报告初稿 | lixin |

**Keywords 关键词**：htap、内存借用、算子、算子内存借用

**Abstract 摘要**：主要测试算子内存借用，支持sort、Agge、Hash join、vector sonic hash join、vector sonic hash aggregate、vector hash join、vector hash aggregate等算子使用时借用周边节点内存，包含功能测试、可靠性测试及性能测试，整体质量良好。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

本报告主要测试算子内存借用，包含功能测试、可靠性测试及性能测试，输出测试用例48个，执行测试共1轮，发现issue共1个，已修复并回归通过，无遗留问题，整体质量良好。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ----------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1B014 | openGauss  | 2025-2-20    | 2025-2-28    | lixin    |

###  2.1.2 配套测试的版本

| 版本名称   | 配套版本 | 版本说明 |
| ---------- | -------- | -------- |
| RackServer | 1.0.0    |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 物理机   | Kunpeng 920 320核<br/>内存：2TB<br/>硬盘：5.8TB<br/>OS：EulerOS release 2.0 (SP3) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试算子内存借用，支持sort、Agge、Hash join、vector sonic hash join、vector sonic hash aggregate、vector hash join、vector hash aggregate等算子使用时借用周边节点内存，包含功能测试、可靠性测试及性能测试，输出测试用例48个，执行测试共1轮，发现issue共1个，已修复并回归通过，无遗留问题，整体质量良好。

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                                                   | 特性价值评估                                                 | 应用说明及关键约束假设依赖       | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| ------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【内核加速组】SQL算子溢盘流程内存借用加速，性能提交15% | RackServer场景下，可以通过临时借用临近节点内存的方式临时提升算子可用内存总量，提升整体AO查询速度。 | 借用内存上限受到内存借用模块限制 | 无                   | 100%             | <font color=green>▮</font> | 无       |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

该特性对性能要求为：SQL算子溢盘流程内存借用加速，性能提交15%。测试结果如下：

| 测试步骤                                                     | 测试结果           |
| ------------------------------------------------------------ | ------------------ |
| 1.依赖RackServer环境，支持远端内存借用特性，开启行列融合特性（编译时--enable-htap）<br />2.tpch 1T数据，对比测试无借用及借用远端内存性能 | 1.成功<br />2.成功 |
各query详细数据如下:

| 测试项               | HTAP+算子不借用                                              | HTAP+算子借用                                                | 加速比 |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ |
| 查询 \ 参数配置 | Imemfabric_client_path正确路径<br />max_imcs_cache=400GB<br />work_mem=20GB<br />shared_buffers=50GB<br />comm_max_stream=60000<br />enable_borrow_memory=on<br />max_borrow_memory=96GB<br />htap_borrow_mem_percent=0<br />max_connections=20000<br />enable_imcsscan=on<br />query_dop=32<br />borrow_work_mem=0<br />max_process_memory=1300GB | memfabric_client_path正确路径<br />max_imcs_cache=400GB<br />work_mem=20GB<br />shared_buffers=50GB<br />comm_max_stream=60000<br />enable_borrow_memory=on<br />max_borrow_memory=96GB<br />htap_borrow_mem_percent=0<br />max_connections=20000<br />enable_imcsscan=on<br />query_dop=32<br />borrow_work_mem=32GB<br />max_process_memory=1300GB | /      |
| query1          | 275.18                                                       | 288.49                                                       | 0.95   |
| query2          | 51.92                                                        | 50.86                                                        | 1.02   |
| query3          | 150.69                                                       | 181.73                                                       | 0.83   |
| query4          | 147.87                                                       | 139.91                                                       | 1.06   |
| query5          | 189.55                                                       | 218.26                                                       | 0.87   |
| query6          | 86.73                                                        | 83.44                                                        | 1.04   |
| query7          | 359.21                                                       | 126.78                                                       | 2.83   |
| query8          | 75.46                                                        | 51.20                                                        | 1.47   |
| query9          | 1283.35                                                      | 247.80                                                       | 5.18   |
| query10         | 164.88                                                       | 217.04                                                       | 0.76   |
| query11         | 69.40                                                        | 65.20                                                        | 1.06   |
| query12         | 266.93                                                       | 186.83                                                       | 1.43   |
| query13         | 74.58                                                        | 62.6                                                         | 1.19   |
| query14         | 168.66                                                       | 132.79                                                       | 1.27   |
| query15         | 242.43                                                       | 264.36                                                       | 0.92   |
| query16         | 20.96                                                        | 47.77                                                        | 0.44   |
| query17         | 654.90                                                       | 636.58                                                       | 1.03   |
| query18         | 539.88                                                       | 469.44                                                       | 1.15   |
| query19         | 275.31                                                       | 258.81                                                       | 1.06   |
| query20         | 119.14                                                       | 126.15                                                       | 0.94   |
| query21         | 307.30                                                       | 291.86                                                       | 1.05   |
| query22         | 163.79                                                       | 155.21                                                       | 1.06   |
| 汇总            | 5688.13                                                      | 4303.11                                                      | 1.32   |
| 平均值          | 258.55                                                       | 195.60                                                       | 1.32   |



| 测试项               | 行存+算子不借用                                              | 行存+算子借用                                                | 加速比 |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ |
| 查询 \ 参数配置 | Imemfabric_client_path正确路径<br />max_imcs_cache=400GB<br />work_mem=20GB<br />shared_buffers=50GB<br />comm_max_stream=60000<br />enable_borrow_memory=on<br />max_borrow_memory=96GB<br />htap_borrow_mem_percent=0<br />max_connections=20000<br />enable_imcsscan=on<br />query_dop=32<br />borrow_work_mem=0<br />max_process_memory=1300GB | memfabric_client_path正确路径<br />max_imcs_cache=400GB<br />work_mem=20GB<br />shared_buffers=50GB<br />comm_max_stream=60000<br />enable_borrow_memory=on<br />max_borrow_memory=96GB<br />htap_borrow_mem_percent=0<br />max_connections=20000<br />enable_imcsscan=on<br />query_dop=32<br />borrow_work_mem=32GB<br />max_process_memory=1300GB | /      |
| query1          | 550.98                                                       | 556.56                                                       | 0.99   |
| query2          | 375.29                                                       | 349.36                                                       | 1.07   |
| query3          | 451.56                                                       | 403.63                                                       | 1.12   |
| query4          | 419.17                                                       | 330.95                                                       | 1.27   |
| query5          | 784.04                                                       | 791.90                                                       | 1.00   |
| query6          | 127.76                                                       | 136.56                                                       | 0.94   |
| query7          | 467.49                                                       | 468.55                                                       | 1.00   |
| query8          | 606.97                                                       | 285.26                                                       | 2.13   |
| query9          | 1627.66                                                      | 1378.29                                                      | 1.18   |
| query10         | 307.64                                                       | 299.37                                                       | 1.03   |
| query11         | 449.97                                                       | 430.80                                                       | 1.04   |
| query12         | 340.48                                                       | 353.05                                                       | 0.96   |
| query13         | 214.45                                                       | 204.53                                                       | 1.05   |
| query14         | 171.92                                                       | 175.53                                                       | 0.98   |
| query15         | 2306.25                                                      | 2323.56                                                      | 0.99   |
| query16         | 99.53                                                        | 75.68                                                        | 1.32   |
| query17         | 1098.71                                                      | 1057.99                                                      | 1.04   |
| query18         | 1007.20                                                      | 738.43                                                       | 1.36   |
| query19         | 170.79                                                       | 124.88                                                       | 1.37   |
| query20         | 255.70                                                       | 222.90                                                       | 1.15   |
| query21         | 1288.54                                                      | 1105.98                                                      | 1.17   |
| query22         | 327.50                                                       | 289.31                                                       | 1.13   |
| 汇总            | 13449.59                                                     | 12093.06                                                     | 1.11   |
| 平均值          | 611.35                                                       | 549.68                                                       | 1.11   |

###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

主要观测算子借用场景，行存表、行转列、列存表在RackServer服务断连等情况下执行计划。

###  4.2.7 兼容性测试结论

无

###  4.2.8 升级测试结论

从无该特性版本升级至该特性所在版本，升级成功；升级后数据库，执行该特性函数及表达式成功，测试通过。

## 4.3 资料测试结论

社区不增加该特性相关资料

# 5 测试对象质量评估

##  5.1 覆盖率分析

已覆盖算子借用内存相关guc参数测试，分别覆盖算子借用场景，算子不借用场景下，行存表、行转列、列存表、分区表，sort、Agge、Hash join、vector sonic hash join、vector sonic hash aggregate、vector hash join、vector hash aggregate等算子功能测试；覆盖RackServer服务断连等可靠性测试；覆盖性能测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IBP9WC](https://e.gitee.com/opengaussorg/issues/list?issue=IBP9WC) | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】borrow_work_mem参数设置不满足约束时仍可设置成功，重启也无任何提示 | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                                     | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------------------------------------- | ------------------------------------------------------------ | -------------------- |
| 1    | 新增算子借用内存相关参数                 | 测试borrow_work_mem参数设置，参数边界值等                    | YES                  |
| 2    | SQL算子溢盘流程内存借用加速，性能提交15% | 分别覆盖算子借用场景，算子不借用场景下，行存表、行转列、列存表、分区表，sort、Agge、Hash join、vector sonic hash join、vector sonic hash aggregate、vector hash join、vector hash aggregate等算子功能测试；覆盖RackServer服务断连等可靠性测试；覆盖性能测试 | YES                  |

##  6.2 测试设计评估

已评审并归档Testplan，归档地址详见附件

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 7.0.0-RC1 B014 | 7                | 48         | 48         | 1          | 1.6    | 0.63     |

本次测试共发现1个issue，已修复并回归通过，缺陷密度为1/1.6k=0.63，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 48           | 48               | 47     | 1      | 0       | 0           | 100%   | 98%        |

本次测试共输出测试用例48个，执行测试共1轮，发现issue共1个，已修复并回归通过，无遗留问题，整体质量良好。

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/7050

资料pr：此特性暂无社区资料

测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=4f2d4d960e254512bc41e37803ef4612&hideDevcloudHead=true

测试用例：testplan _ openGauss7.0.0 _ 数据库服务 _ SQL算子溢盘流程内存借用加速-性能提升

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vvqelovg



