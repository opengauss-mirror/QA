![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2023-05-18 | 1.0 | 报告初稿 | xue.xiao |

关键词：兼容B库，dolphin插件，show storage engines+open tables+slave status

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现show storage engines+open tables+slave status语法的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL  show storage engines+open tables+slave status语法。其中show [storage] engines结果为系统对执行语句的用户所支持的存储引擎；show open tables [{from | in} schema_name] [like 'pattern' | where expr]，列出所有table缓存中打开的表；show {slave | replica} status [for channel channel_name]查看wal（xlog）日志同步状态信息，例如发起端发送日志位置，接收端接收日志位置等。

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| openGauss 5.1.0 build af4bd962 | 2023-05-12 | 2023-05-18 |
| openGauss 5.1.0 build 0812a0d7 | 2023-06-01 | 2023-06-01 |
| dolphin 1.0 | 2023-05-12 | 2023-06-01 |

特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 |系统版本:openEuler 20.03 (LTS)<br />CPU:aarch64 64-bit Little Endian 8 Kunpeng-920<br />MEM:30GB<br />DISK:200GB ||

# 3     测试结论概述

## 3.1   测试整体结论

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL  show storage engines+open tables+slave status语法，共计执行96个用例，主要覆盖了功能测试、约束测试、资料测试。功能测试覆盖了show open tables [{from|in} schema_name] [like 'pattern' | where expr]语法；show [storage] engines语法；show {slave | replica} status [for channel channel_name]语法。应用测试主要包括show open tables，show slave status，show  engines在不同应用场景中的测试。约束测试验证了show相关语法的测试用例在非兼容“B”库下执行数据库可稳定运行。测试中发现1个问题，1个已解决且回归通过。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 创建触发器语法测试，执行结果符合预期，通过 |
| 功能测试 | show open tables [{from|in} schema_name] [like 'pattern' | where expr]语法，执行结果符合预期，通过 |
| 功能测试 | show {slave | replica} status [for channel channel_name]语法，执行结果符合预期，通过 |
| 功能测试 | show [storage] engines语法，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 需在openGauss兼容B库下安装dolphin插件验证。
2. 兼容MySQL5.7版本  兼容MySQL8.0版本

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%    | 0%  | 100%  | 0%      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I79SB8](https://gitee.com/opengauss/Plugin/issues/I79SB8?from=project-issue) | 在 mysql5.7上show open tables无法查看临时表的信息，此处opengauss可以查询出临时表以及全局临时表的信息与mysql5.7不相符 | 次要 | 已验收 |

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
| 1. show open tables [{from|in} schema_name] [like 'pattern' | where expr]语法 <br />2. show {slave | replica} status [for channel channel_name]语法 <br />3. show相关语法的多场景测试 <br />4. show [storage] engines语法 <br /> | 执行96条用例，发现1个问题，1个已修复且验收通过，其他测试通过 |

### 4.1.2 约束测试

|  测试步骤 |  测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行96条用例，执行结果符合预期，测试通过 |

### 4.1.3 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br />2. 文档示例<br /> | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 5.1.0 build af4bd962 | 96 | Passed: 94<br />Failed: 2 | 1 | 1% |
| openGauss 5.1.0 build 0812a0d7 | 96 | Passed: 96<br />Failed: 0 | 0 | 0% |

## 4.3   后续测试建议

无
# 5     附件

无
 
