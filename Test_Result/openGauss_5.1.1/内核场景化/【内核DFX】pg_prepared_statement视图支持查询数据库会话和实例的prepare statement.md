![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者     |
| ---------- | ----------- | -------- | -------- |
| 2023-12-18 | 1.0         | 初稿完成 | ningyali |
| 2023-12-28 | 2.0         | bug验收  | ningyali |

关键词： pg_prepared_statement函数，当前会话的prepare statement，指定会话ID的prepare statement，整个数据库实例的prepare statement

 

摘要：本文档主要验证新增pg_prepared_statement(sessionid)函数在传入不同入参时的表现，并给出测试结论。

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

本需求主要涉及一部分改造：

（1）重载函数pg_prepared_statement(）

原pg_prepared_statement()支持查询当前会话的prepare statement，重载pg_prepared_statement(sessionid)函数实现以下几个功能点：

​	1）支持查询指定会话ID会话的prepare statement；

​	2）如用户会话id为0时，查询整个数据库实例的prepare statement；

​	3）新增sessionid字段和username字段，打印会话id和会话所属用户；

​	4）控制只有超级用户和monitor admin才可访问;

​	5) 支持升级和回滚。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 45f6613a | 2023-11-30   | 2023-12-7    |
| openGauss 5.1.1 build 01b191f0 | 2023-12-8    | 2023-12-14   |

| 环境信息 | 配置信息                                                     | 备注     |
| -------- | ------------------------------------------------------------ | -------- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br />内存：15GB<br />硬盘：300GB<br />OS：CentoOS Linux release 7.6.1908 (Core) | 单机部署 |

# 3     测试结论概述

## 3.1   测试整体结论

共计执行54个用例，主要覆盖了功能测试、兼容性测试、性能测试、资料测试，发现issue5个，已验收通过，整体质量良好；

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | (1)验证原函数pg_prepared_statement()功能正常;<br />(2)验证重载pg_prepared_statement(sessionid)函数传入0、session_id、无效值等场景；<br />(3)验证多session创建预备语句，函数查询返回正确<br />执行用例47个；发现2个主要issue，1个次要issue。<br />测试结论：通过 |
| 兼容性测试 | (1)验证5.0.0/5.0.1升级到5.1.1，升级前原函数功能正常，升级后原函数和重载函数功能正常<br />(2)验证5.0.0升级到5.0.1，再升级到5.1.1，升级到5.1.1前原函数功能正常，升级到5.1.1后原函数和重载函数功能正常<br />(3)验证5.0.0/5.0.1升级到5.1.1，升级回退再升级，升级前原函数功能正常，升级后原函数和重载函数功能正常，升级回退后原函数功能正常，再升级后原函数和重载函数功能正常；<br />执行用例5个，发现1个次要issue。<br />测试结论：通过 |
| 性能测试   | (1)验证查询全局会话响应性能<br />(2)验证间隔1s不间断查询函数，性能无明显劣化；<br />执行用例2个，发现0个issue。<br />测试结论：通过 |
| 资料测试   | 验证pg_prepared_statement函数资料描述正确；发现不重要issue1个。 |

## 3.2   约束说明

（1）只有超级用户和monitor admin才可访问函数pg_prepared_statement(sessionid)。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 0    | 2    | 2    | 1      |
| 百分比 | 100%     | 0%   | 40%  | 40%  | 20%    |

### 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I8L3VX](https://e.gitee.com/opengauss/openGauss-server/issues/I8L3VX) | 主要     | 【测试类型：SQL功能】【测试版本：5.1.1】 初始用户/sysadmin用户连接数据库使用pg_pared_statement查询产生core文件，数据库宕 | 已验收   |
| 2    | [I8LKHX](https://e.gitee.com/opengauss/openGauss-server/issues/I8LKHX) | 主要     | 【测试类型：SQL功能】【测试版本：5.1.1】 monitor admin用户查询视图pg_prepared_statement报错 | 已验收   |
| 3    | [I8LWN7](https://e.gitee.com/opengauss/openGauss-server/issues/I8LWN7) | 次要     | 【测试类型：SQL功能】【测试版本：5.1.1】兼容B库使用pg_prepared_statement查询报错 | 已验收   |
| 4    | [I8M86J](https://e.gitee.com/opengauss/openGauss-server/issues/I8M86J) | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】5.0.0版本升级到5.1.1，回滚失败，报错ERROR：syntax error at or near "DROP FUNCTION" | 已验收   |
| 5    | [I8O5LC](https://e.gitee.com/opengauss/docs/issues/I8O5LC)   | 不重要   | 【测试类型：资料】【测试版本：5.1.1】 文档地图>SQL参考>函数和操作符>系统管理函数>其他函数-pg_prepared_statement函数描述有歧义，且入参取值范围有误 | 已完成   |

# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果          | 发现问题单数 |
| ------------------------------ | ---------- | --------------------- | ------------ |
| openGauss 5.1.1 build 45f6613a | 54         | Passed：46  Failed：8 | 4            |
| openGauss 5.1.1 build 01b191f0 | 8          | Passed：8  Failed：0  | 0            |

*数据项说明：*

- 首轮测试用例执行54个，不通过8个，缺陷问题单汇总；第二轮回归问题单执行8个用例，通过8个；
- 本需求缺陷密度为5(缺陷个数)/0.493k(代码行数)=10.14(个/kloc)。

## 4.2   后续测试建议

无

# 5     附件

修改资料（pg_prepared_statement）：

https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/SQLReference/%E5%85%B6%E5%AE%83%E5%87%BD%E6%95%B0.html

源代码及文档PR
- 【内核DFX】pg_prepared_statement视图支持查询数据库会话和实例的prepare statement
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4493
- 【内核DFX】pg_prepared_statement视图支持查询数据库会话和实例的prepare statement（core问题修复）
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4524
- 【内核DFX】pg_prepared_statement视图支持查询数据库会话和实例的prepare statement（需求相关文档修改）
https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/5991
- 关于pg_prepared_statement()函数修改文档，表格中描述有误
https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6028



 

 
