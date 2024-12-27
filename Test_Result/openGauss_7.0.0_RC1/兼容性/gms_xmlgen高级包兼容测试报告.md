![avatar](https://gitee.com/opengauss/QA/raw/master/images/openGauss.png)

版权所有 © 2024 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode)。

修订记录

| 日期       | 修订版本 | 修改描述     | 作者      |
| ---------- | -------- | ------------ | --------- |
| 2024.12.24 | v1.0     | 测试报告初稿 | zhanghuan |

**Keywords 关键词**：GMS_XMLGEN

**Abstract 摘要**：gms_xmlgen是一个基于openGauss的插件，用于将sql查询结果转换成规范的xml格式。支持sql query字符串或者游标作为输入，并将结果以clob类型或者xmltype类型返回。目前支持的接口有：gms_xmlgen.closecontext、gms_xmlgen.getxmltype、gms_xmlgen.newcontextfromhierarchy、gms_xmlgen.convert、gms_xmlgen.getnumrowsprocessed、gms_xmlgen.getxml、gms_xmlgen.newcontext、gms_xmlgen.restartquery、gms_xmlgen.setconvertspecialchars、gms_xmlgen.setmaxrows、gms_xmlgen.setnullhandling、gms_xmlgen.setrowsettag、gms_xmlgen.setrowtag、gms_xmlgen.setskiprows、gms_xmlgen.useitemtagsforcoll、gms_xmlgen.usenullattributeindicator

**缩略语清单：**

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 概述

此特性主要用来支持在A兼容模式下，支持gms_xmlgen高级包，将SQL查询的结果转换为规范的XML格式。该包以任意SQL查询作为输入，将其转换为XML格式，并将结果作为CLOB返回。

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称                           | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员  |
| ---------------------------------- | ---------- | ------------ | ------------ | --------- |
| openGauss 7.0.0-RC1 build 08280ec0 | openGauss  | 2024-11-18   | 2024-11-28   | zhanghuan |
| openGauss 7.0.0-RC1 build 30d8cc0f | openGauss  | 2024-11-29   | 2024-11-29   | zhanghuan |
| openGauss 7.0.0-RC1 build 96ee5da3 | openGauss  | 2024-12-12   | 2024-12-12   | zhanghuan |

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

openGauss 兼容A的gms_xmlgen特性，共计执行了125个用例，主要覆盖了功能测试、升级测试、兼容性测试、资料测试。功能测试覆盖了插件的创建、删除，插件包的16个接口函数功能，以及接口函数在不同场景下的验证；升级测试在6.0.0版本升级到7.0.0-RC1.B004版本后执行用例验证此功能的正确性；资料测试验证描述、约束、示例是否完善、有效。测试中发现4个问题，问题均以修复且回归通过，无遗留问题，整体质量良好。

## 3.2 约束说明

- 仅支持在A模式数据库创建。
- 仅支持`create extension`命令方式加载插件。
- gms_xmlgen插件依赖libxml，而轻量版openGuass不支持libxml，故轻量版openGauss不支持该插件。
- gms_xmlgen插件依赖xmltype类型，该类型于7.0.0-RC1版本引入，对数据库进行升级或回滚操作时，须先删除该插件，否则回滚升级脚本中存在依赖，升级将失败报错。
- 涉及xmltype的接口需要设置GUC参数behavior_compat_options值为bind_procedure_searchpath。

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性                             | 特性价值评估               | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况                                 | 特性质量评估 | 主要风险 |
| -------------------------------- | -------------------------- | -------------------------- | -------------------- | ------------------------------------------------ | ------------ | -------- |
| 【兼容性组】支持GMS_XMLGEN高级包 | 提升兼容能力，方便用户迁移 | 详见3.2章节描述            | 无                   | 主要覆盖功能测试、兼容性测试、升级测试和资料测试 | ▮            | 无       |

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

| 测试步骤                                              | 测试结果                                |
| ----------------------------------------------------- | --------------------------------------- |
| 1. 在兼容"B"、"C"、"PG"类型数据库下创建gms_xmlgen插件 | 执行3条用例，执行结果符合预期，测试通过 |

### 4.2.8 升级测试结论

| 测试步骤                             | 升级路径    | 测试结果                                |
| ------------------------------------ | ----------- | --------------------------------------- |
| 1. 6.0.0版本升级到7.0.0-RC1.B004版本 | 6.0.0-7.0.0 | 执行2条用例，执行结果符合预期，测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                     | 测试结论   |
| ---- | -------------------------------------------- | ---------- |
| 1    | 插件参考>gms_xmlgen Extension>gms_xmlgen概述 | 未发现问题 |
| 2    | 插件参考>gms_xmlgen Extension>gms_xmlgen限制 | 未发现问题 |
| 3    | 插件参考>gms_xmlgen Extension>gms_xmlgen安装 | 未发现问题 |
| 4    | 插件参考>gms_xmlgen Extension>gms_xmlgen使用 | 未发现问题 |

# 5 测试对象质量评估

## 5.1 覆盖率分析

本次测试主要针对gms_xmlgen高级包功能覆盖了16个接口函数的功能验证、复杂使用场景下的功能是否正确，异常场景合理报错

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 5.2.2 缺陷列表

| 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| [IB4NFN](https://gitee.com/opengauss/openGauss-server/issues/IB4NFN?from=project-issue&search_text=gms_xmlgen) | gms_xmlgen.setmaxrows，gms_xmlgen.setskiprows等接口，参数2传入小数，和A库不兼容，应该取整 | 次要     | 已验收   |
| [IB5SGB](https://gitee.com/opengauss/openGauss-server/issues/IB5SGB?from=project-issue&search_text=gms_xmlgen) | gms_xmlgen.setmaxrows和gms_xmlgen.getxml结合使用，多次使用getxml函数，输出xml结果和A库不一样 | 次要     | 已验收   |
| [IB5WWZ](https://gitee.com/opengauss/openGauss-server/issues/IB5WWZ?from=project-issue&search_text=gms_xmlgen) | gms_xmlgen.setconvertspecialchars函数，参数2为null，特殊字符转义，和A库不一致，A库不转义 | 次要     | 已验收   |
| [IB7PO4](https://gitee.com/opengauss/openGauss-server/issues/IB7PO4?from=project-issue&search_text=gms_xmlgen) | 兼容B库创建gms_xmlgen插件，报错提示有误                      | 次要     | 已验收   |

# 6 测试过程评估

## 6.1 测试策略回顾

| 编号 | 特性             | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------------- | ------------------------------------------------------------ | -------------------- |
| 1    | 插件创建和删除   | 在兼容A库下创建、删除插件                                    | YES                  |
| 2    | 接口函数功能验证 | 接口函数基础功能及在复杂场景下的验证                         | YES                  |
| 3    | 兼容验证         | 在非兼容A库下执行用例，验证是否有合理报错提示，数据库正常运行 | YES                  |
| 4    | 升级验证         | 从6.0.0升级到7.0.0之后执行用例验证功能是否正确               | YES                  |
| 5    | 资料验证         | 资料描述与需求相符，示例是否正确无误                         | YES                  |

## 6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称                           | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度   |
| ---------------------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | ---------- |
| openGauss 7.0.0-RC1 build 08280ec0 | 10               | 125        | 125        | 4          | 6.166  | 0.648/kloc |
| openGauss 7.0.0-RC1 build 30d8cc0f | 0.5              | 3          | 3          | 0          | 0      | 0          |
| openGauss 7.0.0-RC1 build 96ee5da3 | 0.5              | 3          | 3          | 0          | 0      | 0          |

### 6.3.2 测试用例执行结果统计数据

| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 125          | 125              | 125    | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

## 7.1 附件1：遗留问题列表

无

## 7.2 附件2：特性相关PR

1、源代码PR：https://gitee.com/opengauss/openGauss-server/pulls/6566/files

2、文档pr：https://gitee.com/opengauss/docs/pulls/6944

3、测试设计：https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=6506402939864cda8a8a7676965be0df&hideDevcloudHead=true