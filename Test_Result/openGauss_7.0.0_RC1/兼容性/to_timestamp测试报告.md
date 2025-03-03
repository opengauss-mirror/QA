![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2024/12/13   | V1.0       | 测试报告初稿       | 王俊   |

[TOC]

**Keywords 关键词**：*to_timestamp，函数，功能测试*

**Abstract 摘要**：*本次测试是对to_timestamp函数的default功能的测试。主要测试常用场景为基本功能测试，性能测试，升级回滚测试，资料测试等，共发现3个issue，2个新引入的问题，1个遗留问题，。*

# 1 概述

*本次测试主要测试to_timestamp函数中的default功能（TO_TIMESTAMP(char [ DEFAULT return_value ON CONVERSION ERROR ] [, fmt ])），包括功能测试，性能测试，升级回滚测试，资料测试，jdbc/odbc的使用情况以及部分不涉及到核心代码功能的部分*

# 2 测试版本说明

## 2.1 测试版本信息

### 2.1.1 被测版本

| 版本名称 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| -------- | ---------- | ------------ | ------------ | -------- |
| openGauss7.0.0-RC1.B0010 | openGauss-All-7.0.0-RC1-openEuler20.03-aarch64.tar.gz |2024.12.19| 2024.12.26 | wangjun |
| openGauss7.0.0-RC1.B011 | openGauss-All-7.0.0-RC1-openEuler20.03-aarch64.tar.gz |2024.1.10| 2024.1.17 | wangjun |

## 2.2 测试环境描述

### 2.2.1 环境硬件信息

| 环境信息 | 硬件配置信息                                                 |
| -------- | ------------------------------------------------------------ |
| 云服务器|     CPU：4核 <br />内存：16GB<br />硬盘：SSD 40GB<br />OS：openEuler 20.03 (LTS)<br />

### 2.2.2 OS版本

| 操作系统 | OS版本 |
| -------- | ------ |
| Linux    | openEuler 20.03 (LTS) |

# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

主要测试内容为测试兼容A库的to_timestamp()函数，测试包括功能测试，升级回滚测试，性能测试，资料测试，共发现5个issue，2个主要问题，3个次要问题，其中有4个新增问题，1个遗留问题。开发质量一般。

## 3.2 约束说明

无

## 3.3 关键风险和规避措施

| 风险类型     | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| ------------ | -------- | -------- | -------------- | ------ | -------- |
|质量风险|default后的参数为列时数据库会崩溃|导致数据库崩溃|不在default后填入列参数|李浩|待办的|
|质量风险|升级回滚导致元数据不一致|函数在升级和直装两种情况下表现不一致|不升级|李浩|已合入|
|质量风险|当第一个入参为null并且使用default功能时，会报错，而不是输出null|输出结果不符合预期|使用default功能时第一个参数不为null|李浩|待办的|
|质量风险|to_timestamp的fmt不支持字母的大小写|opengauss的sql无法兼容A库|保证fmt处的值全大写或全小写|李浩|待办的|
|质量风险|只判断第一个入参，若没问题直接执行|利用sql向A库迁移数据时会出现sql不兼容的问题|保证default正确输入|李浩|待办的|

# 4 版本详细测试结论

## 4.1 特性测试结论

### 4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 新增to_timestamp函数对DEFAULT子句的支持  | 支持to_timestamp函数的default功能 | 无| 5个| 100% |<font color=red>●</font>| 在有fmt的情况下触发default会导致数据库崩溃 |


## 4.2 产品质量属性目标(DFX)测试结论

### 4.2.1 性能测试结论

*主要对照A库做了参照性的性能测试，测试方案如下： 
1.首先，建表，建表时考虑到两种情况，1）表中的数据重复度很高的情况；2）表中的重复度很低 
2.将to_timestamp()函数在select和where处使用。3.最终观察相同数据量下与A库的性能对照   
PS：数据量挡位为100W，300W，1000W*

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.首先，建表，建表时考虑到两种情况：1）表中的数据重复度很高的情况；2）表中的重复度很低；  2.将to_timestamp()函数在select和where处使用。3.最终观察相同数据量下与A库的性能对照 观察在三种数据量挡位下的性能表现|  同样的sql，oracle所用的时间为比opengauss多约0.84%，性能无劣化|

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
| 1.首先安装6.0.0（LTS） | 6.0.0->7.0.0 | 成功 |
| 2.回滚到6.0.0（LTS） | 7.0.0->6.0.0 | 成功 |
| 3.6.0.0（LTS）再次升级到7.0.0 | 6.0.0->7.0.0 | 成功 |
| 4.升级提交 | 7.0.0->7.0.0 | 成功 |
经检验均无异常

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论                                          |
| ---- | ------------------------------------------------------------ | ------------------------------------------------- |
|     1. | *content/docs-lite/zh/docs/BriefTutorial/函数.md>类型转换相关函数>to_timestamp* | *整体质量良好* |
|     2. | *content/zh/docs/BriefTutorial/函数.md>类型转换相关函数>to_timestamp* |   *整体质量良好* |
|     3. | *content/docs-lite/zh/docs/SQLReference/类型转换函数.md>类型转换相关函数>to_timestamp* | *整体质量良好* |
|     4. | *content/zh/docs/SQLReference/类型转换函数.md>类型转换相关函数>to_timestamp* | *整体质量良好* |
|     5. | *content/docs-lite/en/docs/BriefTutorial/functions.md>Functions Related to Type Conversion>to_timestamp* | *整体质量良好* |
|     6. | *content/docs-lite/en/docs/BriefTutorial/type-conversion-functions.md>Functions Related to Type Conversion>to_timestamp* | *整体质量良好* |
|     7. | *content/docs-lite/en/docs/SQLReference/type-conversion-functions.md>Functions Related to Type Conversion>to_timestamp* | *整体质量良好* |
|     8. | *content/en/docs/BriefTutorial/functions.md>Functions Related to Type Conversion>to_timestamp* | *整体质量良好* |

资料测试整体质量良好

# 5 测试对象质量评估

## 5.1 覆盖率分析

## 5.2 缺陷统计和分析

### 5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   |   5  |   0   |    2  |   3   |     0   |
| 百分比 | 100% |  0%  |  40%  |  60%   |    0%  |

### 5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| IB3TLI  | 对比直接安装7.0.0-RC1，6.0.0升级至7.0.0-RC1后，元数据不一致 | 主要 | 待办的|
| IBNCMC  | to_timestamp函数default后的参数若为列，则数据库core |主要 | 待办的  |
| IBN7X2  | to_timestamp(expr default return_value on conversion error ,fmt) 若expr为null，输出既不为null又不将return_vales输出，但若不使用default，则可以正常输出null| 次要|待办的|
| IBMY1I  |SELECT TO_TIMESTAMP ('11-01-02 10:10:10.123000','Dd-MM-RR HH24:MI:SS.FF') ;执行上述sql时若描述day的部分是大小写不统一的则会报错 | 次要|待办的|
| IBNJXP  |to_timestamp(expr default return_value on conversion error,'YYYY-MM-DD HH24:MI:SS')，当expr正确，return_value错误时不报错，而是正确输出 | 次要|待办的|

# 6 测试过程评估

## 6.1 测试策略回顾


| 编号 | 测试项目 | 验证策略                                         | 是否按照测试策略执行 |
| ---- | ---- | ------------------------------------------------ | -------------------- |
| 1    | 功能测试  |1.主要测试了不使用default功能的情况，观察是否对原有功能产生了影响 2.测试了本次新增特性 3.对边界值进行了测试 4.测试了可能会受到影响的其他情况的场景（select to_timestamp(' 2022-09-21 14:13:29' default '01-09-02 14:10:10' on conversion error,'yyyy-mm-dd hh24:mi:ss')+30/60/24;）5.测试了被多重嵌套和多重嵌套其他函数的复杂场景  | YES |
| 2    | 性能测试  | *主要对照A库做了参照性的测试，测试方案如下： 1.首先，建表，建表时考虑到两种情况，1）表中的数据重复度很高的情况；2）表中的重复度很低 2.将to_timestamp()函数在select和where处使用。3.最终观察相同数据量下与A库的性能对照   PS：数据量挡位为100W，300W，1000W*| YES |
| 3   | 资料测试  |1.依照特性说明文档与官方说明文档对比测试结果 | YES |
| 4   | 驱动测试  |使用jdbc和odbc连接数据库，检验新增功能，观察是否有异常情况 | YES |
| 5    | 升级回滚测试  |1.安装6.0.0（LTS）2.6.0.0升级至7.0.0  3.7.0.0回滚到6.0.0  4.6.0.0再次升级至7.0.0 5.7.0.0升级提交，观察每次升级和回滚后的执行结果是否符合预期| YES |

## 6.2 测试设计评估

无

## 6.3 测试执行评估

### 6.3.1 测试执行统计数据

| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| -------- | ---------------- | ---------- | ---------- | ---------- | -------------- | -------- |
| openGauss7.0.0-RC1.B008 | 0.5 | 483 | 483 | 5 | 452 |4/0.5k=8



### 6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 |     483       |       483        |    449  |    34   |    0     |     0    |   100%  |    92.43%    | 

共计测试用例483，34个未通过，共发现5个issue，1个遗留问题，4个新增问题
# 7 附件

## 7.1 附件1：遗留问题列表

| 序号 | 问题单号 | 问题描述 |  问题级别 | 问题分析与影响 | 规避措施                          |
| ---- | -------- | -------- | ---- | -------- | -------------- | 
| 1    | IB3TLI  | 对比直接安装7.0.0-RC1，6.0.0升级至7.0.0-RC1后，元数据不一致     | 主要       | 导致升级后to_timestamp函数表现与直装不一致 |无|
| 2    | IBNCMC  | to_timestamp函数default后的参数若为列，则数据库core  | 主要  | to_number()相关用例会导致服务挂掉  |确保default后的参数不为列|
| 3    | IBN7X2   | to_timestamp(expr default return_value on conversion error ,fmt) 若expr为null，输出既不为null又不将return_vales输出，但若不使用default，则可以正常输出null | 次要 | 出现预期之外的报错 |不使用default功能，或者使用时保证第一个入参不为null|
| 4   | IBMY1I   | SELECT TO_TIMESTAMP ('11-01-02 10:10:10.123000','Dd-MM-RR HH24:MI:SS.FF') ;执行上述sql时若描述day的部分是大小写不统一的则会报错 | 次要  | 不兼容A库适用sql |保证fmt中的关键词为全大写或者全小写|
| 5   | IBNJXP  | to_timestamp(expr default return_value on conversion error,'YYYY-MM-DD HH24:MI:SS')，当expr正确，return_value错误时不报错，而是正确输出| 次要  | 利用sql迁移A库数据时会出现不严容的问题 |不使用default功能后的参数正确，或者不适用default功能|



## 7.2 附件2：特性相关PR

*提供本需求相关的代码pr及资料pr*

设计文档及checkin文档地址

https://gitee.com/opengauss-compatible/openGauss-design/tree/master/to_timestamp%E5%87%BD%E6%95%B0%E7%89%B9%E6%80%A7

pr链接

https://gitee.com/opengauss/openGauss-server/pulls/6495/files

测试设计

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=48ea12a526694512b1a7e9ad1fe38244&hideDevcloudHead=true

测试用例  

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vjo9uumo
