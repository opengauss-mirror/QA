![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者  |
| ---------- | -------- | ------------ | ----- |
| 2024.11.30 | v1.0     | 测试报告初稿 | xiaqi |
| 2024.12.5  | v1.1     | 完善执行记录 | xiaqi |

**Keywords 关键词**：GMS_COMPRESS.LZ_COMPRESS、GMS_COMPRESS.LZ_COMPRESS_ADD、GMS_COMPRESS.LZ_COMPRESS_ADD、GMS_COMPRESS.LZ_COMPRESS_CLOSE、GMS_COMPRESS.LZ_COMPRESS_OPEN、GMS_COMPRESS.LZ_UNCOMPRESS、GMS_COMPRESS.LZ_UNCOMPRESS_CLOSE、GMS_COMPRESS.LZ_UNCOMPRESS_EXTRACT、GMS_COMPRESS.LZ_UNCOMPRESS_OPEN

**Abstract 摘要**：主要测试gms_compress插件嵌套使用交替解压压缩及添加，整体质量良好。

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |


# 1 概述

本报告主要测试插件gms_compress嵌套交替使用，包含功能测试和资料测试，输出测试用例57个，执行测试共2轮，发现issue共3个，均已修复并回归通过，无遗留问题，整体质量良好。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | ---------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build f03109c2 | openGauss  | 2024-11-21   | 2024-11-28   | xiaqi    |
| openGauss 7.0.0-RC1 build 7ef91864 | openGauss  | 2024-11-29   | 2024-12-4    | xiaqi    |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Inter(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 4核<br/>内存：8GB<br/>硬盘：220GB<br/>OS：openEuler release 20.03(LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

本报告主要测试插件gms_compress嵌套交替使用，包含功能测试和资料测试，输出测试用例57个，执行测试共2轮，发现issue共3个，均已修复并回归通过，无遗留问题，整体质量良好。

##  3.2 约束说明

1.该功能在openGauss的A兼容模式下使用。

2.仅支持create extension命令方式加载插件。

3.仅支持最大使用5个压缩句柄。

4.仅支持压缩row和blob类型。

5.每个句柄存储的数据不能超过1GB。 

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性                             | 特性价值评估                     | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险 |
| -------------------------------- | -------------------------------- | -------------------------- | -------------------- | ---------------- | -------------------------- | -------- |
| 【兼容性】支持GMS_COMPRESS高级包 | 支持GMS_COMPRESS插件嵌套交替执行 | 无                         | 无                   | 100%             | <font color=green>▮</font> | 无       |

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

从无该特性版本升级至该特性所在版本，升级成功；升级后数据库，执行该特性插件及表达式成功，测试通过。

## 4.3 资料测试结论

社区已增加该特性函数及表达式文档，与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [gms_compress](https://docs.opengauss.org/zh/docs/latest/docs/ExtensionReference/gms_compress%E6%A6%82%E8%BF%B0.htmll) | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本特性主要从功能、特性交互等方面测试，主要通过边界值以及等价类的测试设计方法对这些特性验证功能与需求相符,已覆盖插件gms_compress表达式功能实现、与其他特性交互场景及测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 0    | 3    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述                                                     | 问题级别 | 当前状态 |
| -------- | ------------------------------------------------------------ | -------- | -------- |
| IB6U06   | 【测试类型：资料】【测试版本：7.0.0-RC1】gms_compress工具约束不完整 | 次要     | 已验收   |
| IB6TL6   | 【测试类型：功能测试】【测试版本：7.0.0-RC1】A兼容性，LZ_UNCOMPRESS_OPEN打开句柄，LZ_COMPRESS_CLOSE关闭句柄执行报错 | 次要     | 已验收   |
| IB6R4U   | 【测试类型：功能测试】【测试版本：7.0.0-RC1】A兼容性，连续执行LZ_UNCOMPRESS_EXTRACT两次成功 | 次要     | 已验收   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 特性                                                         | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| 1    | 支持GMS_COMPRESS.LZ_COMPRESS<br/>GMS_COMPRESS.LZ_COMPRESS_ADD<br/>GMS_COMPRESS.LZ_COMPRESS_OPEN<br/>GMS_COMPRESS.LZ_COMPRESS_CLOSE<br/>语法 | 测试compress压缩等级/输入空和不合理的字符/并发执行/句柄的边界值/反复打开压缩/添加数据边界值 /返回值是否正确 / 对应的上下文关系/ 连续执行打开和关闭报错/ 入参为null/ 反复嵌套执行等场景测试 | YES                  |
| 2    | 支持GMS_COMPRESS.LZ_UNCOMPRESS<br/>GMS_COMPRESS.LZ_UNCOMPRESS_OPEN<br/>GMS_COMPRESS.LZ_UNCOMPRESS_CLOSE<br/>GMS_COMPRESS.LZ_UNCOMPRESS_EXTRACT<br/>GMS_COMPRESS.LZ_UNCOMPRESS_CLOSE<br/>语法 | 测试uncompress解压等级/输入空和不合理的字符/并发执行/句柄的边界值/反复打开压缩/添加数据边界值 /返回值是否正确 / 对应的上下文关系/ 连续执行打开和关闭报错/ 入参为合理格式/入参为null/ 反复嵌套执行等场景测试 | YES                  |
| 3    | 支持GMS_COMPRESS.ISOPEN语法                                  | 测试解压压缩上下文的句柄打开返关闭回返回值及并发执行场景测试 | YES                  |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量   | 缺陷密度 |
| ------------------------ | ---------------- | ---------- | ---------- | ---------- | -------- | -------- |
| openGauss 7.0.0-RC1 B004 | 7                | 57         | 57         | 3          | 2.3/kloc | 1.3/kloc |
| openGauss 7.0.0-RC1 B007 | 0.5              | 3          | 3          | 0          | 0        | 0        |

本次测试共发现3个issue，包含1个资料单，均已修复并回归通过，缺陷密度为1.3/kloc，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 57           | 57               | 57     | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

特性代码pr：https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6569

资料pr：https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6948

测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=5dc5e6b2a2bd4be4a0cad62fbe5251c5&hideDevcloudHead=true