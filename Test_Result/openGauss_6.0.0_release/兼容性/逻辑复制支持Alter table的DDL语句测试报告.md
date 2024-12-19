![](../../images/openGauss.png#id=y7buq&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)  
版版权所有 © 2024  openGauss社区  
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://creativecommons.org/licenses/by-sa/4.0/legalcode。)  
修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2024.09.05 | v1.0 | 初稿 | 戴榛俞 |


Keywords 关键词：逻辑复制，DDL，功能测试  
Abstract 摘要：本文档是对openGauss中逻辑复制使用发布订阅支持同步DDL的测试总结，针对ALTER TABLE、CREATE/DROP VIEW、CREATE/DROP FUNCTION、CREATE/DROP TYPE、CREATE/DROP TRIGGER、TRUNCATE TABLE、RENAME TABLE等发布端操作正常同步到订阅端，逻辑复制功能可正常使用。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| --- | --- | --- |
| N/A | N/A | N/A |


---

# 1 概述
本特性从6.0.0版本开始逻辑复制使用发布订阅时支持同步DDL，增强openGauss的逻辑复制的功能，实现支持ALTER TABLE、CREATE/DROP VIEW、CREATE/DROP FUNCTION、CREATE/DROP TYPE、CREATE/DROP TRIGGER、TRUNCATE TABLE、RENAME TABLE等DDL操作的逻辑复制功能。

# 2 测试版本说明
## 2.1 测试版本信息
### 2.1.1 被测版本
本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段

| 版本名称 | 软件包名称 | 测试起始时间 | 测试结束时间 | 测试人员 |
| --- | --- | --- | --- | --- |
| openGauss 6.0.0 a04eaf40 | openGauss_-All-6.0.0-CentOS7-x86_64.tar.gz | 2024.8.16 | 2024.8.30 | 戴榛俞 |


## 2.2 测试环境描述
### 2.2.1 环境硬件信息
| 环境信息 | 硬件型号 | 硬件配置信息 | 备注 |
| --- | --- | --- | --- |
| 物理机 | x86_CentOS | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz   内存：376G   硬盘：7.7T   OS：CentOS Linux 7 (Core)   文件系统：XFS   网卡：1000Mb/s**** |  |


# 3 版本概要测试结论、关键风险和规避措施
## 3.1 测试结论总结
openGauss逻辑复制支持Alter table等DDL语句同步的功能，共计执行61个用例，主要覆盖ALTER TABLE、CREATE/DROP VIEW、CREATE/DROP FUNCTION、CREATE/DROP TYPE、CREATE/DROP TRIGGER、TRUNCATE TABLE、RENAME TABLE等语句的同步场景，通过手工执行用例，测试过程中发现问题6个，其中单机问题3个，逻辑复制DDL功能3个，修复后待出包，当前整体质量一般。

## 3.2 约束说明
1. 使用该功能时，发布端与订阅端的兼容性模式需保持一致，若不一致创建订阅时会报错。
2. ALTER TABLE相关语法不支持同步如下操作：ALTER TABLE ADD FULLTEXT SPATIAL INDEX、ALTER TABLE ALGORITHM、ALTER TABLE ALTER CHECK、ALTER TABLE PASSWORD、ALTER TABLE ROW_FORMAT。
3. CREATE/DROP DATABASE仅支持B模式下使用，且参数dolphin.b_compatibility需要为on，即相当于使用CREATE/DROP SCHEMA。
4. VIEW相关DDL仅支持普通视图，不支持物化视图相关DDL同步。

## 3.3 关键风险和规避措施
无

# 4 版本详细测试结论
## 4.1 特性测试结论
### 4.1.1 新需求质量评价
| 特性 | 特性价值评估 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估 | 主要风险 |
| --- | --- | --- | --- | --- | --- | --- |
| 逻辑复制支持Alter table的DDL语句 | 用户可通过逻辑复制发布订阅，进行同步DDL语句，提高了逻辑复制的实用性、便利性 | 详见3.2章节描述 | 当前遗留缺陷6个 | 功能性测试 | ▲ | 遗留缺陷中有2个挂库缺陷 |


_特性质量评估说明_：  
●： _表示特性不稳定，风险高_  
▲： _表示特性基本可用，遗留少量问题_  
▮： _表示特性质量良好_

## 4.2 产品质量属性目标(DFX)测试结论
### 4.2.1 性能测试结论
无

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
无

## 4.3 资料测试结论
无

# 5 测试对象质量评估
## 5.1 覆盖率分析
本次覆盖的逻辑复制同步的DDL包括create/drop/alter/truncate/rename table、create/drop view、create/drop function、create/drop type、create/drop trigger等，通过对这些语句按测试场景覆盖的方式操作来测试在逻辑复制发布订阅端的同步情况，检查正常同步情况及异常情况复制槽没有inactive。

## 5.2 缺陷统计和分析
### 5.2.1 缺陷统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 6 | 0 | 2 | 4 | 0 |
| 百分比 | N/A | 0% | 33.33% | 66.67% | 0% |


### 5.2.2 缺陷列表
| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| --- | --- | --- | --- |
| IANPRD | 变长字段varchar(n)创建唯一索引并作为replica identity，该列有多个null值时，更新挂库 | 主要 | 待回归 |
| IANPN2 | B模式event定时任务sql执行失败，pg_job系统表没有更新相应失败信息 | 次要 | 已验收 |
| IANQVT | 创建执行失败的任务后，再次create event失败 | 次要 | 已完成 |
| IAN5HW | 创建发布后，删除不存在的type时报错异常，提示cache lookup failed for type with OID 0 | 次要 | 待办的 |
| IAN6HK | 逻辑复制环境，创建发布订阅后，发布端创建列存物化视图再创建ustore物化视图挂库 | 主要 | 待办的 |
| IAOVSR | 逻辑复制环境，create schema指定属主同步失败 | 次要 | 待办的 |


# 6 测试过程评估
## 6.1 测试策略回顾
| 编号 | 特性 | 验证策略 | 是否按照测试策略执行 |
| --- | --- | --- | --- |
| 1 | 功能测试 | 逻辑复制支持DDL同步按需求描述实现 | YES |
|  |  |  |  |


## 6.2 测试设计评估
| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| --- | --- | --- | --- |
| N/A |  |  |  |


## 6.3 测试执行评估
### 6.3.1 测试执行统计数据
| 版本名称 | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（KLOC） | 缺陷密度 |
| --- | --- | --- | --- | --- | --- | --- |
| openGauss 6.0.0 a04eaf40 | 14 | 61 | 61 | 6（其中单机3个） | 8K | 0.0375 |


### 6.3.2 测试用例执行结果统计数据
| 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 61 | 61 | 54 | 7 | 0 | 0 | 100% | 88.52% |


| 异常用例情况 | 影响分析 | 规避措施 | 后续计划 |
| --- | --- | --- | --- |
| N/A |  |  |  |


# 7 附件
## 7.1 附件1：遗留问题列表
| 序号 | 问题单号 | 问题描述 | 分类 | 问题级别 | 问题分析与影响 | 规避措施 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | IANPRD | 变长字段varchar(n)创建唯一索引并作为replica identity，该列有多个null值时，更新挂库 | 单机 | 主要 | 逻辑复制解码列为变长字段时，更新拥有多个null值的表时挂库 | <font style="color:rgb(64, 72, 91);">恢复手段：重启可以恢复正常</font> |
| 2 | IANPN2 | B模式event定时任务sql执行失败，pg_job系统表没有更新相应失败信息 | 单机 | 次要 | B模式event定时任务sql执行失败，pg_job系统表没有更新job_status | 无 |
| 3 | IANQVT | 创建执行失败的任务后，再次create event失败 | 单机 | 次要 | 创建执行失败的任务后，再次create event失败 | 无 |
| 4 | IAN5HW | 创建发布后，删除不存在的type时报错异常，提示cache lookup failed for type with OID 0 | 逻辑复制 | 次要 | 创建发布后，删除不存在的type时报错异常 | 订阅端手动删除type |
| 5 | IAN6HK | 逻辑复制环境，创建发布订阅后，发布端创建列存物化视图再创建ustore物化视图挂库 | 逻辑复制 | 主要 | 逻辑复制环境，创建发布订阅后，发布端创建列存物化视图再创建ustore物化视图挂库 | <font style="color:rgb(64, 72, 91);">恢复手段：重启可以恢复正常</font> |
| 6 | IAOVSR | 逻辑复制环境，create schema指定属主同步失败 | 逻辑复制 | 次要 | 逻辑复制环境，create schema指定属主，订阅端同步失败 | 订阅端手动创建 |


## 7.2 附件2：特性相关PR
1 源代码PR：  
[https://gitee.com/opengauss/openGauss-server/pulls/5711](https://gitee.com/opengauss/openGauss-server/pulls/5711)  
2 文档PR：  
[https://gitee.com/opengauss/docs/pulls/](https://gitee.com/opengauss/docs/pulls/)

