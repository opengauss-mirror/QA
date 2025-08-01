![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |     修改描述     | 作者  |
| :-------: | :------: | :--------------: | :---: |
| 2023-6-25 |   1.0    | 特性测试报告初稿 | xiaqi |

 关键词：

 gs_upgradechk、校验 、导出 、升级

摘要：

本文档主要介绍实现upgrade-checker升级之后元数据完整性的检测工具，在数据库升级之后，对元数据进行检验，检查在升级过程前后，是否出现元数据的损坏，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

upgrade-checker是一个升级之后元数据完整性的检测工具，由python3编写，在数据库升级之后，对元数据进行检验，检查在升级过程前后，是否出现元数据的损坏。目前仅支持检测builtin、initdb的元数据，对于用户所创建的表等，暂时不会进行检测。

主要功能如下：

- **导出**：基于某个数据库，生成一份元数据校验地图。
- **校验**：基于某个元数据校验地图，对数据库进行元数据的校验，并生成报告。

# 2     特性测试信息

|            版本名称            | 测试起始时间 | 测试结束时间 |
| :----------------------------: | :----------: | :----------: |
| openGauss 3.0.3 build 0b0270b1 |   2023-6-9   |  2023-6-15   |

| 硬件型号 |                         硬件配置信息                         | 备注 |
| :------: | :----------------------------------------------------------: | ---- |
|  虚拟机  | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：openEuler release 20.03 (LTS-SP1) |      |



# 3     测试结论概述

## 3.1   测试整体结论

gs_upgradechk升级校验工具，共计执行21条用例，主要覆盖了功能测试和资料测试。功能测试覆盖gs_upgradechk的导出元数据校验地图功能，以及基于元数据校验并生成报告的功能，还在升级过程中测试gs_upgradechk的相关功能。累计发现缺陷单5个，5个问题单均已修复，整体质量良好。

| 测试活动 |                           活动评价                           |
| :------: | :----------------------------------------------------------: |
| 功能测试 | 对于gs_upgradechk参数的错误值及不适用的数据库版本，会打印合理的报错信息 |
| 功能测试 |              基于当前数据库，导出数据的检验地图              |
| 功能测试 | 指定的元数据校验地图或者自动检测目标数据库版本并在网上下载生成好的vmap，生成报告 |
| 功能测试 |       校验报告的模式，当前支持summary跟detail两种模式        |
| 功能测试 | 日志打印信息，默认关闭debug，指定参数开启debug后，打印更多的日志，用于工具问题定位 |
| 资料测试 | 资料对于新增加的功能描述准确，示例的执行结果正确，整体质量良好 |

## 3.2   约束说明

- gs_upgradechk工具目前只支持openGauss版本：3.0.3+，5.0.X，5.1.X。
- 不指定vmap参数时，会自动检测目标数据库版本并在网上下载和使用我们生成好的vmap，需要机器网络正常。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------: | :------: | :------: | :----------------: | :------: |
|    NA    |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :---------: | :------: | :--: | :--: | :--: | :----: |
|  数目 (个)  |    5     |  0   |  0   |  3   |   2    |
| 百分比  (%) |   100    |  0   |  0   | 60%  |  40%   |

### 3.3.3 问题详情

| 序号 |                           issue号                            | 问题级别 |                 问题简述                  | 问题状态 |
| :--: | :----------------------------------------------------------: | :------: | :---------------------------------------: | :------: |
|  1   | [I7CNUM](https://e.gitee.com/opengaussorg/issues/list?issue=I7CNUM) |   次要   |        导出报告模式指定detail报错         |  已完成  |
|  2   | [I7CB64](https://e.gitee.com/opengaussorg/issues/list?issue=I7CB64) |   次要   | gs_upgradechk生成报告中校验了不必要的信息  |  已完成  |
|  3   | [I7CATK](https://e.gitee.com/opengaussorg/issues/list?issue=I7CATK) |   次要   |    文档描述help不区分大小写与实现不符      |  已完成  |
|  4   | [I7DGLS](https://e.gitee.com/opengaussorg/issues/list?issue=I7DGLS) |  不重要  |   gs_upgradechk资料README里部分标点缺失   |  已完成  |
|  5   | [I7HBYW](https://e.gitee.com/opengaussorg/issues/list?issue=I7HBYW) |  不重要  |     官网资料gs_upgradechk部分描述有误     |  已完成 |

# 4     测试执行

## 4.1   测试执行统计数据

### 4.1.1   gs_upgradechk参数验证

| 测试步骤：                                                   | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. gs_upgradechk参数验证<br/>2. 错误的参数有明确的的提示信息 | 执行7条用例，发现1个bug，现已修复且验收通过 |

###  4.1.2   导出校验地图

| 测试步骤：                                                   | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 1. 基于某个数据库，生成一份元数据校验地图<br/>2. gs_upgradechk export -p port | 执行11条用例，发现1个bug，现已修复验收通过 |

###  4.1.3  校验

| 测试步骤：                                                   | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1. 基于某个元数据校验地图，对数据库进行元数据的校验，并生成报告 | 执行3条用例，发现1个bug，现已修复且验收通过 |

###  4.1.4   资料测试

| 测试步骤：                                                | 测试结果                                    |
| --------------------------------------------------------- | ------------------------------------------- |
| 1. 资料对于新增加的功能描述准确<br/>2. 示例的执行结果正确 | 发现2个bug，2个bug已修复且验收通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数   | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ------------ | ------------------------ | ------------ |
| openGauss 3.0.3 build 0b0270b1 | 21           | Passed：16<br>Failed：5 | 5            |
| openGauss 3.0.3 build 0b0270b1 | 回归issue：5  | Passed：5<br>Failed：0   | 0            |
| openGauss 5.0.0 build 584be3be | 回归issue：5 | Passed：5<br>Failed：0   | 0            |

*数据项说明：*

* 累计发现缺陷单5个.5个缺陷均已解决且回归通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度: 3(缺陷个数)/5.027kloc(代码行数)=0.597(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

无