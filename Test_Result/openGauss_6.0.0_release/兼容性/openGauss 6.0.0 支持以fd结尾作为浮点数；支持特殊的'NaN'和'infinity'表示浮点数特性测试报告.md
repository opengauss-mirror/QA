![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- |----|
| 2024.9.13 |v1.0 | 测试报告初稿 | 曹戎 |


**Keywords 关键词**：兼容A，以f，d结尾，特殊的'NaN'和'infinity'

**Abstract 摘要**：本文档主要验证实现以‘f/F’或‘d/D’结尾的浮点数以及支持特殊的‘NaN’和‘infinity’表示浮点数的兼容性。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|  NA    |          |          |

***


# 1 概述

本特性设计旨在介绍如何实现以‘f/F’或‘d/D’结尾的浮点数的兼容性支持，以及如何支持特殊的‘NaN’和‘infinity’表示浮点数。这些功能的实现将使系统能够处理更广泛的浮点数表示方式，提高灵活性。
# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

*本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段*

| 版本名称                 | 软件包名称                                  | 测试起始时间    | 测试结束时间    | 测试人员 |
|----------------------| -------------------------------------------|-----------|-----------|------|
| openGauss 6.0.0 B025 |  | 2024.7.18 | 2024.7.25 | 曹戎   |
| openGauss 6.0.0 B026 |  | 2024.7.25 | 2024.7.31 | 曹戎   |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | --------- | ------------------------------------------------------------ | ---- |
| 虚拟机 | aarch64_openEuler20.03 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：32G<br />硬盘：100G<br />OS：openEuler release 20.03 (LTS-SP4)<br /> | |



# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

通过支持以‘f/F’或‘d/D’结尾的浮点数以及支持特殊的‘NaN’和‘infinity’表示浮点数功能特性，提升openGaussA库兼容性，共计执行67个用例，主要在函数的创建，调用方面进行测试，通过手工执行‘f/F’或‘d/D’结尾的浮点数和特殊的‘NaN’和‘infinity’的基础识别、与函数结合使用、自定义函数、表达式等的结合场景的用例，发现问题2个，无遗留风险，整体质量良好。

## 3.2 约束说明

- 需要开启GUC参数float_suffix_acceptance来允许接收f 或 d 结尾的浮点数。
- 需要开启GUC参数enable_binary_special_o_format使浮点数特殊值正确显示。
- 支持以f/d结尾作为浮点数功能需要在A兼容库下进行。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 支持以f/d结尾作为浮点数；支持特殊的'NaN'和'infinity'表示浮点数 |如何实现以‘f/F’或‘d/D’结尾的浮点数的兼容性支持，以及如何支持特殊的‘NaN’和‘infinity’表示浮点数。这些功能的实现将使系统能够处理更广泛的浮点数表示方式，提高灵活性。 |详见3.2章节描述                        | 无                  | 功能性测试               | <font color=green>▮</font> |  |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

性能测试以对比opengauss与oracle在查询通过索引的表的where过滤特殊值BINARY_DOUBLE_INFINITY的查询速度。在数据量为百万级的情况下，oracle因为有结果集缓存这一机制，多次查询的速度明显快于opengauss。单次查询的速度oracle大约为1.90秒，opengauss的单次查询速度约为1.60秒，由于受到环境波动以及环境配置的影响，且查询速度差距小于20%。测试结论为单次查询速度opengauss与oralce几乎无差别，opengauss稍快一些，多次查询oralce速度明显快于opengauss。

| 测试步骤                                                   | 测试结果                                        |
|--------------------------------------------------------|---------------------------------------------|
| 创建带有主键的表，创建过滤索引，插入百万级数据，查询值为BINARY_DOUBLE_INFINITY的数量。 | 单次查询性能未见明显差距，多次查询opengauss速度明显慢于oracle，符合预期 |
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

针对数据库支持的多种兼容性，在A、B、PG兼容性下进行相应的功能测试

| 测试步骤                                    | 测试结果 |
|-----------------------------------------| -------- |
| 开启相关配置，在A、B、PG库中执行相应测试用例，测试用例是否只在A库执行成功 | 符合预期 |

###  4.2.8 升级测试结论

验证5.0.3版本升级至6.0.0版本，测试通过。

| 测试步骤                                     | 升级路径                 | 测试结果 |
|------------------------------------------| ------------------------ | -------- |
| 使用root用户安装5.0.3版本数据库，然后升级至6.0.0版本，执行部分用例 | 5.0.3版本升级至6.0.0版本 | 测试通过 |

## 4.3 资料测试结论

社区已修改支持特殊浮点数相关流程，对支持特殊浮点数增加特性的描述，与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                                                                                                                                                    | 测试结论 |
| ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| -------- |
| 1    | 文档地图->关于openGauss->特性描述->企业级特性->支持特殊浮点数<br/>(https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/AboutopenGauss/%E6%94%AF%E6%8C%81%E7%89%B9%E6%AE%8A%E6%B5%AE%E7%82%B9%E6%95%B0.html) | 测试通过 |
| 2    | 文档地图->数据库参考->GUC参数说明->开发人员选项<br/>(https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/DatabaseReference/%E5%BC%80%E5%8F%91%E4%BA%BA%E5%91%98%E9%80%89%E9%A1%B9.html)                                                                                                                                                     | 测试通过 |
--链接

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次测试通过支持以‘f/F’或‘d/D’结尾的浮点数以及支持特殊的‘NaN’和‘infinity’表示浮点数功能特性，提升openGaussA库兼容性，主要在函数的创建，调用方面进行测试，通过手工执行‘f/F’或‘d/D’结尾的浮点数和特殊的‘NaN’和‘infinity’的基础识别、与函数结合使用、自定义函数、表达式等的结合场景的用例，验证功能与需求相符，异常情况有合理报错。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要   | 不重要 |
| ------ |------| ---- |----|------| ------ |
| 数目   | 2    | 0    | 0  | 2    | 0      |
| 百分比 | 100% | 0%   | 0% | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号   | 问题描述                                                                                                         | 问题级别 | 当前状态 |
|--------|--------------------------------------------------------------------------------------------------------------|------| -------- |
| IAEPH6 | 【测试类型：SQL功能】【测试版本：6.0.0】对binary_double_nan、binary_double_infinity的查询结果与期望不符(https://e.gitee.com/opengaussorg/issues/table?issue=IAEPH6)                                | 次要   | 已验收 |
| IAGGDM | 【测试类型：SQL功能】【测试版本：6.0.0】default值设置为以f/d结尾的浮点数时报错(https://e.gitee.com/opengaussorg/issues/table?issue=IAGGDM) | 次要   | 已验收 |
--链接

# 6 测试过程评估

##  6.1 测试策略回顾


| 编号 | 特性   | 验证策略                                                                      | 是否按照测试策略执行 |
| ---- |------|---------------------------------------------------------------------------| -------------------- |
| 1    | 功能测试 | 通过手工执行‘f/F’或‘d/D’结尾的浮点数和特殊的‘NaN’和‘infinity’的基础识别、与函数结合使用、自定义函数、表达式等的结合场景的用例 | YES             |
| 2 | 性能测试 | 对比opengauss与oracle在查询通过索引的表的where过滤特殊值BINARY_DOUBLE_INFINITY的查询速度                                                  | YES |
| 3 | 资料测试 | 覆盖官网latest关于支持特殊浮点数相关流程，对支持特殊浮点数增加特性的描述相关资料验证                             | YES |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 |
|----------------------|-----------|-------|-------|-------|
| openGauss 6.0.0 B025 | 5         | 67    | 67    | 1     |
| openGauss 6.0.0 B026 | 5         | 67    | 67    | 1     |
代码总量为0.196k行 ，缺陷密度为2/0.196k=10.204
 
本次测试共发现2个issue，2个已修复并回归通过，整体质量良好。


###  6.3.2 测试用例执行结果统计数据

|     | 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
|-----|--------|----------|--------|--------|---------| ----------- | ------ |-------|
| 第一轮 | 35     | 35       | 31     | 4      | 0       | 0           | 100%   | 89%   |
| 第二轮 | 32     | 32       | 32     | 0      | 0       | 0           | 100%   | 100%  |
| 第三轮 | 4      | 4        | 4      | 0      | 0       | 0           | 100%   | 100%  |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1 特性代码PR：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5730?tab=files
https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6510

2 文档PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6658
https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6539

3 测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=1d817533a2bc4d15b7a55da8fbbacbdb&hideDevcloudHead=true
