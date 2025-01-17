![avatar](https://gitee.com/opengauss/QA/raw/master/images/openGauss.png)

版权所有 © 2024 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode)。

修订记录

| 日期       | 修订版本 | 修改描述                        | 作者      |
| ---------- | -------- | ------------------------------- | --------- |
| 2024.12.24 | v1.0     | 测试报告初稿                    | zhanghuan |
| 2025.1.9   | v1.1     | 更新测试报告缺陷密度及issue状态 | zhanghuan |

**Keywords 关键词**：GMS_UTILITY

**Abstract 摘要**：gms_utility是一个基于openGauss的插件。目前支持23个接口，包括：GMS_UTILITY.ANALYZE_DATABASE、GMS_UTILITY.CANONICALIZE、GMS_UTILITY.COMPILE_SCHEMA、GMS_UTILITY.DB_VERSION、GMS_UTILITY.EXPAND_SQL_TEXT、GMS_UTILITY.GET_CPU_TIME、GMS_UTILITY.GET_ENDIANNESS、GMS_UTILITY.GET_SQL_HASH、GMS_UTILITY.IS_BIT_SET、GMS_UTILITY.IS_CLUSTER_DATABASE、GMS_UTILITY.NAME_RESOLVE、GMS_UTILITY.NAME_TOKENIZE、GMS_UTILITY.OLD_CURRENT_SCHEMA、GMS_UTILITY.OLD_CURRENT_USER、GMS_UTILITY.FORMAT_ERROR_BACKTRACE、GMS_UTILITY. FORMAT_ERROR_STACK、GMS_UTILITY.FORMAT_CALL_STACK、GMS_UTILITY.GET_TIME、GMS_UTILITY.COMMA_TO_TABLE、GMS_UTILITY. EXEC_DDL_STATEMENT、GMS_UTILITY.GET_HASH_VALUE、GMS_UTILITY.TABLE_TO_COMMA、GMS_UTILITY.ANALYZE_SCHEMA

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 概述

此特性主要用来支持在A兼容模式下，支持gms_utility高级包，为用户提供了各种实用的程序和函数，目前支持23个接口。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

#### PART 1

| 版本名称                                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员  |
| ---------------------------------------- | ---------- | ------------ | ------------ | --------- |
| openGauss 7.0.0-RC1 build a9d64cfe(B005) | openGauss  | 2024-12-16   | 2024-12-26   | zhanghuan |
| openGauss 7.0.0-RC1 build 7ef91864(B007) | openGauss  | 2024-12-5    | 2024-12-5    | zhanghuan |
| openGauss 7.0.0-RC1 build 96ee5da3(B008) | openGauss  | 2024-12-13   | 2024-12-13   | zhanghuan |
| openGauss 7.0.0-RC1 build f8ba23e6(B009) | openGauss  | 2024-12-23   | 2024-12-23   | zhanghuan |
| openGauss 7.0.0-RC1 build eb367cc7(B010) | openGauss  | 2024-12-26   | 2024-12-26   | zhanghuan |
| openGauss 7.0.0-RC1 build 784af0ca(B011) | openGauss  | 2025-1-9     | 2025-1-9     | zhanghuan |

#### PART 2

| 版本名称                                 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员  |
| ---------------------------------------- | ---------- | ------------ | ------------ | --------- |
| openGauss 7.0.0-RC1 build 96ee5da3(B008) | openGauss  | 2024-12-16   | 2024-12-26   | zhanghuan |
| openGauss 7.0.0-RC1 build f8ba23e6(B009) | openGauss  | 2024-12-23   | 2024-12-23   | zhanghuan |
| openGauss 7.0.0-RC1 build eb367cc7(B010) | openGauss  | 2024-12-26   | 2024-12-26   | zhanghuan |

### 2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| 无       |          |          |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件型号          | 硬件配置信息                                                 | 备注 |
| -------- | ----------------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | aarch64+openEuler | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>MEM: 32GB<br/>DISK: 200GB<br/>OS: openEuler 20.03 (LTS) |      |

# 3 版本概要测试结论、关键风险和规避措施

##  3.1 测试结论总结

openGauss 兼容A的gms_utility特性，共计执行了308个用例（PART1：184；PART2：124），主要覆盖了功能测试、升级测试、兼容性测试、驱动测试、资料测试。功能测试覆盖了插件的创建、删除，插件包的23个接口函数功能，以及接口函数在不同场景下的验证；升级测试在6.0.0版本升级到7.0.0-RC1版本后执行用例验证此功能的正确性；兼容性测试覆盖在不同兼容模式验证插件功能；驱动测试覆盖使用python驱动，测试插件功能是否正常；资料测试验证描述、约束、示例是否完善、有效。测试中发现26个问题，问题均以修复且回归通过，无遗留问题，整体质量良好。

## 3.2 约束说明

- 仅支持`CREATE EXTENSION`命令方式加载，使用`DROP EXTENSION`命令方式卸载。
- 仅支持在A库环境下使用，非A库环境不建议使用该插件
- 部分函数表现和A库不完全兼容

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性                              | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                                 | 特性质量评估 | 主要风险 |
| --------------------------------- | -------------------------- | -------------------------- | -------------------- | ------------------------------------------------ | ------------ | -------- |
| 【兼容性组】支持GMS_UTILITY高级包 | 提升兼容能力，方便用户迁移 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试、兼容性测试、升级测试和资料测试 | ▮            | 无       |

*特性质量评估说明*：

●： *表示特性不稳定，风险高*

▲： *表示特性基本可用，遗留少量问题*

▮： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.3 安全&隐私保护测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.4 可服务性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.5 生命周期管理测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.6 韧性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.7 兼容性测试结论

| 测试步骤                                               | 测试结果                                |
| ------------------------------------------------------ | --------------------------------------- |
| 1. 在兼容"B"、"C"、"PG"类型数据库下创建gms_utility插件 | 执行3条用例，执行结果符合预期，测试通过 |

### 4.2.8 升级测试结论

| 测试步骤                        | 升级路径    | 测试结果                                |
| ------------------------------- | ----------- | --------------------------------------- |
| 1. 6.0.0版本升级到7.0.0-RC1版本 | 6.0.0-7.0.0 | 执行2条用例，执行结果符合预期，测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                       | 测试结论    |
| ---- | ---------------------------------------------- | ----------- |
| 1    | 插件参考>gms_utility Extension>gms_utility概述 | 未发现问题  |
| 2    | 插件参考>gms_utility Extension>gms_utility限制 | 未发现问题  |
| 3    | 插件参考>gms_utility Extension>gms_utility安装 | 未发现问题  |
| 4    | 插件参考>gms_utility Extension>gms_utility使用 | 发现1个问题 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试主要针对gms_utility高级包功能覆盖了23个接口函数的功能验证、复杂使用场景下的功能是否正确，异常场景合理报错

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 26       | 1    | 0    | 25   | 0      |
| 百分比 | 100%     | 4%   | 0%   | 96%  | 0%     |

### 5.2.2 缺陷列表

| 编号 | 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| 1    | [IBAAXU](https://gitee.com/opengauss/openGauss-server/issues/IBAAXU?from=project-issue&search_text=gms_utility) | 当视图存在级联依赖且有效，gms_utility.compile_schema函数，参数2为true时，执行报错 | 次要     | 已验收   |
| 2    | [IBC1YO](https://gitee.com/opengauss/openGauss-server/issues/IBC1YO?from=project-issue&search_text=gms_utility) | gms_utility.table_to_comma函数，数组元素的索引表现和A库不一样 | 次要     | 已验收   |
| 3    | [IBBXSV](https://gitee.com/opengauss/openGauss-server/issues/IBBXSV?from=project-issue&search_text=gms_utility) | gms_utility.table_to_comma函数，传入的数组为null或者含null，返回结果和A库不一样 | 次要     | 已验收   |
| 4    | [IBBA36](https://gitee.com/opengauss/openGauss-server/issues/IBBA36?from=project-issue&search_text=gms_utility) | gms_utility.comma_to_table函数，参数1的字符串双引号内有逗号，返回结果和A库不一样 | 次要     | 已验收   |
| 5    | [IBB9CC](https://gitee.com/opengauss/openGauss-server/issues/IBB9CC?from=project-issue&search_text=gms_utility) | gms_utility.comma_to_table函数，参数1的字符串为保留关键字，未使用双引号，预期报错，实际成功 | 次要     | 已验收   |
| 6    | [IBB4FF](https://gitee.com/opengauss/openGauss-server/issues/IBB4FF?from=project-issue&search_text=gms_utility) | GMS_UTILITY.FORMAT_ERROR_BACKTRACE、gms_UTILITY.FORMAT_ERROR_STACK函数执行，数据库挂掉并产生core | 主要     | 已验收   |
| 7    | [IBA1R1](https://gitee.com/opengauss/openGauss-server/issues/IBA1R1?from=project-issue&search_text=gms_utility) | gms_utility.name_resolve函数，对象是包及包的同义词，解析报错，预期成功 | 次要     | 已验收   |
| 8    | [IBA08I](https://gitee.com/opengauss/openGauss-server/issues/IBA08I?from=project-issue&search_text=gms_utility) | gms_utility.name_resolve函数参数1和参数2的值不匹配，返回结果和A库不一样 | 次要     | 已验收   |
| 9    | [IB9XYL](https://gitee.com/opengauss/openGauss-server/issues/IB9XYL?from=project-issue&search_text=gms_utility) | gms_utility.name_resolve函数，参数1为标识符，返回结果和A库不一样 | 次要     | 已验收   |
| 10   | [IB9BHT](https://gitee.com/opengauss/openGauss-server/issues/IB9BHT?from=project-issue&search_text=gms_utility) | gms_utility.name_resolve函数，参数2取值为null或小数，表现和A库不一样 | 次要     | 已验收   |
| 11   | [IB9ARQ](https://gitee.com/opengauss/openGauss-server/issues/IB9ARQ?from=project-issue&search_text=gms_utility) | gms_utility.name_resolve函数，参数1的字符串带@dblink，加双引号、有空格的情况下，返回结果和A库不一样 | 次要     | 已验收   |
| 12   | [IB8Y7J](https://gitee.com/opengauss/openGauss-server/issues/IB8Y7J?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数函数，字符串为a[.b.c]@dblink时，字符串有引号且已@开头的，解析结果有误 | 次要     | 已验收   |
| 13   | [IB8VVL](https://gitee.com/opengauss/openGauss-server/issues/IB8VVL?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数，字符串为纯逗号分隔或含有逗号分隔，结果和A库不一样 | 次要     | 已验收   |
| 14   | [IB8ST5](https://gitee.com/opengauss/openGauss-server/issues/IB8ST5?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数，字符串有@符号且@和其余字符都在双引号内，执行报错，预期成功 | 次要     | 已验收   |
| 15   | [IB8QSD](https://gitee.com/opengauss/openGauss-server/issues/IB8QSD?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数，字符串为单引号，结果和A库不一样 | 次要     | 已验收   |
| 16   | [IB8MQF](https://gitee.com/opengauss/openGauss-server/issues/IB8MQF?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数，字符串加双引号已数字开头或为纯数字，预期成功，实际报错 | 次要     | 已验收   |
| 17   | [IB8LL7](https://gitee.com/opengauss/openGauss-server/issues/IB8LL7?from=project-issue&search_text=gms_utility) | gms_utility.name_tokenize函数字符串中间有空格无引号，输出结果和A库不一样 | 次要     | 已验收   |
| 18   | [IB8HUZ](https://gitee.com/opengauss/openGauss-server/issues/IB8HUZ?from=project-issue&search_text=gms_utility) | gms_utility.canonicalize函数，指定的字符串字节超过63，报错提示是最大64，应该是63 | 次要     | 已验收   |
| 19   | [IB8FMU](https://gitee.com/opengauss/openGauss-server/issues/IB8FMU?from=project-issue&search_text=gms_utility) | gms_utility.canonicalize函数，指定的字符串有非点号分隔符(,)，预期报错，实际成功 | 次要     | 已验收   |
| 20   | [IBDSY1](https://gitee.com/opengauss/docs/issues/IBDSY1?from=project-issue) | gms_utility插件章节若干描述错误                              | 次要     | 已验收   |
| 21   | [IB8AVW](https://gitee.com/opengauss/openGauss-server/issues/IB8AVW?from=project-issue&search_text=gms_utility) | gms_utility.canonicalize函数，指定的字符串为数字，预期成功，实际报错 | 次要     | 已验收   |
| 22   | [IB89HO](https://gitee.com/opengauss/openGauss-server/issues/IB89HO?from=project-issue&search_text=gms_utility) | gms_utility.canonicalize函数，指定的字符串中间有空格且无双引号包裹，预期报错，实际成功 | 次要     | 已验收   |
| 23   | [IB6QRO](https://e.gitee.com/opengaussorg/issues/table?issue=IB6QRO) | 补充gms_utility高级包社区文档                                | 次要     | 已验收   |
| 24   | [IBDY5D](https://gitee.com/opengauss/openGauss-server/issues/IBDY5D?from=project-issue) | gms_UTILITY.CANONICALIZE函数，字符串为点号分隔的，返回结果末尾多个点号 | 次要     | 已验收   |
| 25   | [IB6U14](https://gitee.com/opengauss/openGauss-server/issues/IB6U14?from=project-issue&search_text=gms_utility) | gms_utility.expand_sql_text函数，用户无查询对象权限，执行函数未报错 | 次要     | 已验收   |
| 26   | [IB6R20](https://gitee.com/opengauss/openGauss-server/issues/IB6R20?from=project-issue&search_text=gms_utility) | gms_utility.is_bit_set函数，参数2是null时，返回结果和A库不一样 | 次要     | 已验收   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性             | 验证策略                                                | 是否按照测试策略执行 |
| ---- | ---------------- | ------------------------------------------------------- | -------------------- |
| 1    | 插件创建和删除   | 在兼容A库下创建、删除插件                               | YES                  |
| 2    | 接口函数功能验证 | 接口函数基础功能及在复杂场景下的验证                    | YES                  |
| 3    | 兼容验证         | 在非兼容A库下执行用例，验证功能是否支持，数据库正常运行 | YES                  |
| 4    | 升级验证         | 从6.0.0升级到7.0.0之后执行用例验证功能是否正确          | YES                  |
| 5    | 驱动测试         | 使用python驱动验证插件相关功能                          | YES                  |
| 6    | 资料验证         | 资料描述与需求相符，示例是否正确无误                    | YES                  |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

#### PART 1

| 版本名称                                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度  | 备注                                                         |
| ---------------------------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | --------- | ------------------------------------------------------------ |
| openGauss 7.0.0-RC1 build a9d64cfe(B005) | 10               | 184        | 184        | 19         | 8.504  | 1.99/KLOC | `IB9BHT IB9ARQ IB8Y7J IB8VVL IB8ST5 IB8QSD IB8MQF IB8LL7 IB6U14 IB8HUZ IB8FMU IB8AVW IB89HO IB6QRO IB6R20 IB7FWC IBAAXU IBA1R1 IBA08I` |
| openGauss 7.0.0-RC1 build 7ef91864(B007) | 0.5              | 5          | 5          | 0          | 0      | 0         |                                                              |
| openGauss 7.0.0-RC1 build 96ee5da3(B008) | 0.5              | 10         | 10         | 0          | 0      | 0         |                                                              |
| openGauss 7.0.0-RC1 build f8ba23e6(B009) | 0.125            | 1          | 1          | 0          | 0      | 0         |                                                              |
| openGauss 7.0.0-RC1 build eb367cc7(B010) | 0.5              | 4          | 4          | 1          | 0      | 0         | 引入新问题`IBDY5D`                                           |
| openGauss 7.0.0-RC1 build 784af0ca(B011) | 0.5              | 2          | 2          | 0          | 0      | 0         |                                                              |

说明：`IB8Y7J、IB9ARQ、IB8ST5`此三个issue为同一根因，计算为1个；`IB6QRO`为资料issue，因此缺陷数为17个，代码量为8.504，缺陷密度为17/8.504=1.99KLOC

#### PART 2

| 版本名称                                 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度   | 备注                                     |
| ---------------------------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | ---------- | ---------------------------------------- |
| openGauss 7.0.0-RC1 build 96ee5da3(B008) | 9                | 124        | 124        | 5          | 1.953  | 2.048/KLOC | `IBB4FF、IBB9CC、IBBA36、IBBXSV、IBC1YO` |
| openGauss 7.0.0-RC1 build f8ba23e6(B009) | 0.5              | 4          | 4          | 0          | 0      | 0          |                                          |
| openGauss 7.0.0-RC1 build eb367cc7(B010) | 0.5              | 4          | 4          | 1          | 0      | 0          | 资料问题`IBDSY1`                         |

说明：`IBB4FF`非该需求引入，`IBDSY1`为资料issue，因此缺陷密度为4/1.953=2.048/KLOC

该特性总共缺陷密度为21/10.457=2.008/KLOC

### 6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 308          | 308              | 308    | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

## 7.1 附件1：遗留问题列表

无

## 7.2 附件2：特性相关PR

1、源代码PR：

https://gitee.com/opengauss/openGauss-server/pulls/6575

https://gitee.com/opengauss/openGauss-server/pulls/6752

2、文档pr：

https://gitee.com/opengauss/docs/pulls/6977

3、测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=8d0b14dd13124ad58b6bd7c364670bb0&hideDevcloudHead=true

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=2271128fe6df4b5a80f826e88fe716e7&hideDevcloudHead=true

4、测试用例：

测试用例->openGauss_7.0.0->数据库兼容性->兼容A库特性->支持gms_utility高级包