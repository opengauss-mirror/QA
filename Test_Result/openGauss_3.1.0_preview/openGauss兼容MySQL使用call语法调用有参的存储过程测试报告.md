![](../../images/openGauss.png#crop=0&crop=0&crop=1&crop=1&id=d4u0Z&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=#crop=0&crop=0&crop=1&crop=1&id=S95Ra&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2022-09-23 | 1.0 | 测试报告初稿完成 | wangrururu |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |


关键词：

dolphin插件，call，in、out、inout参数，兼容B库

摘要：

本文档主要介绍openGauss在兼容B库且有dolphin插件的情形下，实现兼容mysql使用call语法调用有参的存储过程。针对有out和inout类型参数的存储过程，允许在使用call语法调用的时候，对out和inout参数的位置，传入变量调用。

# 1     特性概述

openGauss在兼容B库情形下，安装dolphin插件，可以实现实现兼容mysql使用call语法调用有参的存储过程。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| opengauss 3.1.0 build a100917c | 2022-09-20 | 2022-09-21 |
| dolphin 1.0 | 2022-09-20 | 2022-09-21 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 虚拟机 | CPU：Intel(R) Xeon(R) Gold 6130 CPU @ 2.10GHz 8核
内存：32GB
硬盘：100G
OS：CentOS Linux release 7.6.1810 (Core) |  |


# 3     测试结论概述

## 3.1   测试整体结论
openGauss兼容mysql，支持使用call语法调用有参的存储过程，测试用例23条，主要覆盖了功能测试及资料测试。功能测试覆盖：只有in参数、只有out参数、只有inout参数、三种参数混合使用、参数数量最大值，匿名块调用、函数调用以及直接调用，校验输出结果。资料测试覆盖校验资料的描述及示例的执行结果。累计发现问题单3个，3个非本需求问题取消。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | in参数的存储过程在函数、匿名块、存储过程中调用，执行结果符合预期，通过 |
| 功能测试 | out参数的存储过程在函数、匿名块、存储过程中调用，执行结果符合预期，通过 |
| 功能测试 | inout参数的存储过程在函数、匿名块、存储过程中调用，执行结果符合预期，通过 |
| 功能测试 | 异常调用存储过程，执行结果符合预期，通过 |
| 功能测试 | 函数、匿名块、存储过程中调用存储过程，加call，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，通过 |


## 3.2   约束说明
1.openGauss需使用兼容B库且有dolphin插件

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| N/A |  |  |  |  |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 3 | 
 | 0 | 3 | 0 |
| 百分比 | 100% | 
 |  | 100% |  |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I5S4LH](https://gitee.com/opengauss/Plugin/issues/I5S4LH?from=project-issue) | 次要 | out类型参数，变量名带引号，报错 | 已取消 |
| 2 | [I5S402](https://gitee.com/opengauss/Plugin/issues/I5S402?from=project-issue) | 次要 | 对于in、inout类型参数，部分数据类型不支持空字符串输入 | 已取消 |
| 3 | [I5S313](https://gitee.com/opengauss/Plugin/issues/I5S313?from=project-issue) | 次要 | out、inout类型参数，函数或存储过程调用时，输入为null，报错 | 已取消 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 编译安装openGauss数据库
2. 编译dolphin插件
3. 创建兼容B库并安装dolphin插件
4. 执行用例 | 编译库及插件成功，共执行23条用例，
共发现3个bug，3条非问题已取消 |



## 4.2   测试执行统计数据
| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| opengauss 3.1.0 build a100917c | 23 | Passed：23
Failed：0 | 0 |

数据说明

1. 累计发现问题单3个，3个非问题取消
## 4.3   后续测试建议
无
# 5     附件
无
