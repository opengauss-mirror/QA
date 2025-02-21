![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2024/12/13   | V1.0       | 测试报告初稿       | 王俊   |

[TOC]

**Keywords 关键词**：*to_number，函数，功能测试*

**Abstract 摘要**：*本文档是对to_number函数的default功能的测试。主要测试常用场景，性能测试等。*

# 1 概述

*本次测试主要测试to_number函数中的default功能（TO_NUMBER(expr [ DEFAULT return_value ON CONVERSION ERROR ] [, fmt ])），但是因为相关函数（to_binary_float）有部分功能进行了重写，故还包括除default以外的功能*

# 2 测试版本说明

本次主要测试TO_NUMBER(expr [ DEFAULT return_value ON CONVERSION ERROR ] [, fmt ])函数功能
cpu架构：aarch64架构
操作系统：openEuler 20.03 (LTS)

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------- | ---------- | ------------ | ------------ | -------- |
| openGauss7.0.0-RC1.B009 | openGauss-All-7.0.0-RC1-openEuler20.03-aarch64.tar.gz |2024.12.16| 2024.12.28 | wangjun |
| openGauss7.0.0-RC1.B010 | openGauss-All-7.0.0-RC1-openEuler20.03-aarch64.tar.gz |2024.1.2| 2024.1.10 | wangjun |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 |
| -------- | ------------------------------------------------------------ |
| 云服务器|     CPU：4核 <br />内存：16GB<br />硬盘：SSD 40GB<br />OS：openEuler 20.03 (LTS)<br />

### 2.2.2 OS版本

*选填，如果2.2.1中填写多个硬件配置相同的信息，但操作系统版本不同，可在此章节内容详细列举使用的操作系统*

| 操作系统 | OS版本 |
| -------- | ------ |
| Linux    | openEuler 20.03 (LTS) |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

本报告主要测试兼容A库的to_number()函数，测试包括功能测试，升级回滚测试，性能测试，资料测试，共发现6个issue，其中有4个新增问题，两个遗留问题。开发质量较差。

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

*本章节描述版本进入下一阶段或进行版本发布使用的风险及关键规避措施。*

*建议通过文字和如下表格方式描述*（选填）：

| 风险类型     | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| ------------ | -------- | -------- | -------------- | ------ | -------- |
|质量风险|某些语句会导致数据库崩溃|导致数据库崩溃|暂不使用default功能|李浩|已修复，待合入|
|质量风险|升级回滚导致元数据不一致|函数在升级和直装两种情况下表现不一致|不升级|李浩|已合入|
|质量风险|default后默认值的参数边界问题|在999.1这样的边界值表现异常|不采用极端值作为默认值|李浩|修复中|
|质量风险|expr正确，default错误仍然正确输出|opengauss的sql无法兼容A库|保证default处的值输入正确|李浩|修复中|
|质量风险|to_number('aasda123');输出123|出现不可预料的结果|正确输入|李浩|待办的|
|质量风险|to_number('123.123','9999999999');输出123123|出现不可预料的结果|正确输入|李浩|修复中|

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 新增TO_NUMBER函数对支持子句和 DEFAULT NULL ON CONVERSION ERROR, fmt子句支持  | 支持to_number函数的default功能 | 无| 6个| 100% |<font color=red>●</font>| 在有fmt的情况下触发default会导致数据库崩溃 |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论


### 4.2.1 性能测试结论

*主要对照A库做了参照性的性能测试，测试方案如下： 
1.首先，建表，建表时考虑到两种情况，1）表中的数据重复度很高的情况；2）表中的重复度很低 
2.将to_number()函数在select和where处使用。3.最终观察相同数据量下与A库的性能对照   
PS：数据量挡位为100W，300W，1000W*

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.首先，建表，建表时考虑到两种情况，1）表中的数据重复度很高的情况；2）表中的重复度很低； 2.将to_number()函数在select和where处使用。3.最终观察相同数据量下与A库的性能对照 观察在三种数据量挡位下的性能表现|  同样的sql，opengauss的所用时长是oracle的39.42%，性能提升约253.68%|

### 4.2.2 可靠性测试结论

无

### 4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

无

### 4.2.7 兼容性测试结论

无

### 4.2.8 升级测试结论

升级回滚验证无异常

| 测试步骤 | 升级路径 | 测试结果 |
| -------- | -------- | -------- |
| 1.首先安装5.0.3 | 5.0.3->7.0.0 | 成功 |
| 2.回滚到5.0.3 | 7.0.0->5.0.3 | 成功 |
| 3.5.0.3再次升级到7.0.0 | 5.0.3->7.0.0 | 成功 |
| 4.升级提交 | 7.0.0->7.0.0 | 成功 |
经检验均无异常

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论                                          |
| ---- | ------------------------------------------------------------ | ------------------------------------------------- |
|     1. | *content/docs-lite/zh/docs/BriefTutorial/函数.md>类型转换相关函数>to_number* | *整体质量良好* |
|     2. | *content/zh/docs/BriefTutorial/函数.md>类型转换相关函数>to_number* |   *整体质量良好* |
|     3. | *content/docs-lite/zh/docs/SQLReference/类型转换函数.md>类型转换相关函数>to_number* | *整体质量良好* |
|     4. | *content/zh/docs/SQLReference/类型转换函数.md>类型转换相关函数>to_number* | *整体质量良好* |
|     5. | *content/docs-lite/en/docs/BriefTutorial/functions.md>Functions Related to Type Conversion>to_number* | *整体质量良好* |
|     6. | *content/docs-lite/en/docs/BriefTutorial/type-conversion-functions.md>Functions Related to Type Conversion>to_number* | *整体质量良好* |
|     7. | *content/docs-lite/en/docs/SQLReference/type-conversion-functions.md>Functions Related to Type Conversion>to_number* | *整体质量良好* |
|     8. | *content/en/docs/BriefTutorial/functions.md>Functions Related to Type Conversion>to_number* | *整体质量良好* |

目前资料质量良好，若将问题单解决则需要修改描述资料

# 5 测试对象质量评估

## 5.1 覆盖率分析

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   |   6  |   0   |    2  |   4   |     0   |
| 百分比 | 100% |  0%  |  33%  |  66%   |    0%  |

### 5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| IB3T42  | 对比直接安装7.0.0-RC1，6.0.0升级至7.0.0-RC1后，元数据不一致 | 主要 | 已修复，已合入|
| IBB1QC  | to_number()相关用例会导致服务挂掉 |主要 | 已修复，待合入  |
| IBGNEW  |数据库执行此sql结果异常select to_number('1234.123' DEFAULT 999.1 ON CONVERSION ERROR,'999.99');预期结果为999.1，实际结果为报错| 次要|待办的|
| IBKMHF  |fmt会直接将小数点抹掉，并且不通知用户 | 次要|待办的|
| IBLRSM  |expr正确，default错误仍然正确输出 | 次要|待办的|
| IBM27H |to_number()中fmt对输入值的处理异常 | 次要|待办的|

# 6 测试过程评估

## 6.1 测试策略回顾

*回顾本阶段的测试策略，建议以表格的方式检查测试策略规定的活动是否都已经落实。*

| 编号 | 特性 | 验证策略                                         | 是否按照测试策略执行 |
| ---- | ---- | ------------------------------------------------ | -------------------- |
| 1.    |支持DEFAULT default_val_expr ON CONVERSION ERROR子句<br/>支持DEFAULT default_val_expr ON CONVERSION ERROR, fmt子句| *测试原有功能/测试defatult功能/测试to_binary_float/A库支持的边界值/og库的边界值/表数据的使用情况/对不同的输入值的表现（输出或者报错信息）/性能测试* | YES |
| 2.    |支持DEFAULT default_val_expr ON CONVERSION ERROR子句<br/>支持DEFAULT default_val_expr ON CONVERSION ERROR, fmt子句| *测试升级回滚表现* |YES|
| 3.    |支持DEFAULT default_val_expr ON CONVERSION ERROR子句<br/>支持DEFAULT default_val_expr ON CONVERSION ERROR, fmt子句|*测试在select部分使用/测试在where部分使用/测试在多重被嵌套和嵌套其他函数/JDBC，ODBC*|YES|

## 6.2 测试设计评估

无

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss7.0.0-RC1.B008 | 0.5 | 425 | 425 | 6 | 314 |4/0.5k=8


### 6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 |     441       |       441        |    421   |    20    |    0     |     0    |   100%  |    95.46%    | 

共计测试用例441，20个未通过，共发现6个issue
# 7 附件

## 7.1 附件1：遗留问题列表

| 序号 | 问题单号 | 问题描述 |  问题级别 | 问题分析与影响 | 规避措施    |                       |
| ---- | -------- | -------- | ---- | -------- | -------------- | ---------------------------------- |
| 1    | IB3T42    | 对比直接安装7.0.0-RC1，6.0.0升级至7.0.0-RC1后，元数据不一致     | 主要       | 导致升级后to_number函数表现与直装不一致 |无
| 2    | IBB1QC  | to_number()相关用例会导致服务挂掉     | 主要  | to_number()相关用例会导致服务挂掉  |不适用default功能
| 3    | IBGNEW  | 数据库执行此sql结果异常select to_number('1234.123' DEFAULT 999.1 ON CONVERSION ERROR,'999.99');预期结果为999.1，实际结果为报错  | 次要 | 数值边界处理异常 |不在default处使用999这样的边界值
| 4   | IBKMHF   | fmt会直接将小数点抹掉，并且不通知用户 select to_number(123.123,'999999999999999');输出为123123 | 次要  | fmt对小数点的处理有误 |使fmt的范围将输入值完全覆盖
| 5   | IBLRSM  | expr正确，default错误仍然正确输出，select to_number('1231321' default 'asdasd' on conversion error,'99999999999999999'); 输出为1231321| 次要  | 无法兼容A库sql |不使用default功能或者保证正确输入
| 6   | IBM27H  | to_number()中fmt对输入值的处理异常 ，select to_number('a123n123','999999999999'); 应为报错而不是输出123123 | 次要  | 无法兼容A库sql,出现不可预料的结果 |保证正确输入

## 7.2 附件2：特性相关PR

*提供本需求相关的代码pr及资料pr*

设计文档及checkin文档地址

<https://gitee.com/opengauss-compatible/openGauss-design/tree/master/to_number%E5%87%BD%E6%95%B0%E7%89%B9%E6%80%A7>

pr链接

<https://gitee.com/opengauss/openGauss-server/pulls/6277>

测试设计

<https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=933919cf52c242cdb1163647084461ca&hideDevcloudHead=true>

测试用例  

<https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vjo9uumo>
