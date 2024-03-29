![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-09-14 | 1.0 | 报告初稿完成 | xue.xiao |
| 2022-09-22 | 1.1 | 根据评审意见修改完成 | xue.xiao | 
| 2022-09-30 | 1.2 | 根据评审意见修改完成 | xue.xiao |

关键词：兼容B库，dolphin插件，make_set

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL数据库下的make_set函数的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|   NA   |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL的make_set函数。make_set函数首先会将bits位的值转换为二进制，然后将二进制翻转，利用翻转后的每一位二进制与其相对应的str值按位与，如得到的值为零或null的情况下会跳过，其他情况会输出。

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-08-02 | 2022-09-14 |
| openGauss 3.1.0 build 1cb614d5 | 2022-08-02 | 2022-09-01 |
| openGauss 3.1.0 build ece73c7d | 2022-09-01 | 2022-09-14 |
| dolphin 1.0 | 2022-08-02 | 2022-09-14 |

特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 |系统版本:openEuler 20.03 (LTS)<br />CPU:aarch64 64-bit Little Endian 8 Kunpeng-920<br />MEM:30Gi ||

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容make_set函数，共计执行21个用例，主要覆盖了功能测试、极限测试、约束测试、资料测试以及备份恢复测试。功能测试覆盖了在表中利用make_set返回列或是直接根据bits位输出对应的str值以及在不同场景中执行make_set函数的测试；极限测试验证了bits位的有效值范围以及str位可取到的最大范围；约束测试验证了make_set功能的测试用例在非兼容“B”库下执行数据库可稳定运行；备份恢复测试验证是否可以逻辑备份逻辑恢复、物理备份以及物理恢复的功能。测试中发现4个问题，2个已解决且回归通过，2个已取消。

|  测试活动 |  活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | bits位在不输入参数或输入合法以及非法参数下的测试，执行结果符合预期，通过 |
| 功能测试 | str位在不输入参数或输入合法以及非法参数下的测试，执行结果符合预期，通过 |
| 功能测试 | 在单表或多表中利用make_set返回选择的列的情况下，bits位在不输入参数或输入合法以及非法参数下的测试，执行结果符合预期，通过 |
| 功能测试 | 在单表或多表中利用make_set返回选择的列的情况下，str位在不输入参数或输入合法以及非法参数下的测试，执行结果符合预期，通过 |
| 功能测试 | 在事物、存储函数、存储过程、视图以及查询下的测试，执行结果符合预期，通过 |
| 功能测试 | 在bits位输入合法范围内的数据以及非法范围内的数据，验证不同情况下str位可取到的最大值,执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |
| 备份恢复测试 | 可进行逻辑备份逻辑恢复、物理备份以及物理恢复，符合预期，通过 |

## 3.2   约束说明

1. make_set函数需在openGauss兼容B库下安装dolphin插件验证。
2. MySQL5.7版本

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA         |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4     | 0      |
| 百分比 | 100%     | 0    | 0    | 100%  | 0      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I5QRWR](https://gitee.com/opengauss/Plugin/issues/I5QRWR?from=project-issue) | 在make_set里边对除数为0的处理 | 次要 | 已取消 |
| [I5QRZD](https://gitee.com/opengauss/Plugin/issues/I5QRZD?from=project-issue) | 在make_set里边结合其他函数的返回结果与MySQL不一致 | 次要 | 已验收 |
| [I5RENG](https://gitee.com/opengauss/Plugin/issues/I5RENG?from=project-issue) | 在make_set里边默认取的小数点后位数与MySQL中不一致 | 次要 | 已取消 |
| [I5S7QQ](https://gitee.com/opengauss/Plugin/issues/I5S7QQ?from=project-issue) | make_set对FALSE参数当做空指针处理 | 次要 | 已验收 |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 入参测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在str位不输入参数或输入合法以及非法参数执行测试<br />2.在bits位不输入参数或输入合法以及非法参数执行测试<br />3.在单表或多表中测试bits位和str位在合法以及非法参数情况下的输出<br />4.在事物、存储过程、存储函数、视图以及查询中执行测试<br /> | 执行19条用例，发现4个问题，2个已修复且验收通过，2个已取消，其他测试通过 |

### 4.1.2 极限测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 验证最大有效数为9223372036854775807<br />2. 验证最小有效数为-9223372036854775808<br />3. 验证在bits位输入正数的情况下str位可取到63位<br />4. 验证在bits位输入负数的情况下str位可取到63位<br /> | 执行2条用例，结果符合预期，测试通过 |

### 4.1.3 约束测试

|  测试步骤 |  测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行21条用例，执行结果符合预期，测试通过 |

### 4.1.4 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br />2. 文档示例<br /> | 文档描述准确，示例正确无误 |

### 4.1.5 备份恢复测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 逻辑备份逻辑恢复<br />2. 物理备份物理恢复<br /> | 执行结果符合预期，测试通过 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 3.1.0 build 1cb614d5 | 21 | Passed: 17<br />Failed: 4 | 4 | 19% |
| openGauss 3.1.0 build ece73c7d | 21 | Passed: 21<br />Failed: 0 | 0 | 0 |

## 4.3   后续测试建议

1.已取消问题[I5QRWR](https://gitee.com/opengauss/Plugin/issues/I5QRWR?from=project-issue):此问题属于opengauss与MySQL机制不同导致，在opengauss中make_set里边出现除数为0则会出现错误，应避免使用除数为0的操作；已取消问题[I5RENG](https://gitee.com/opengauss/Plugin/issues/I5RENG?from=project-issue):此问题属于opengauss与MySQL机制不同导致，在opengauss中make_set里边出现除不尽的数，则会出现较多的小数点后位数，不出现错误可正常使用。
# 5     附件

无
 
