![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期       | 修订 版本 | 修改描述 | 作者        |
| ---------- | --------- | -------- | ----------- |
| 2023.11.15 | V1.0      | 初稿     | yangyixiang |

关键词：

pg_stat_activity、statement_history

摘要：

本文档主要介绍查询系统表pg_stat_activity、statement_history定位存储过程正在执行的sql语句及慢sql，并给出最终测试结论

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

1. 存储过程执行中定位当前执行语句，在一个存储过程执行过程中，可以通过查询pg_stat_activity系统表，来查看正在执行的语句是哪一个。只支持查询sql语句，不支持实时查询存储计划
2. 存储过程结束后通过statement_history查询历史慢sql，存储过程执行过程中，会根据guc参数来判断是否保存plsql的信息，存入到statement_history中，通过设置时间阈值来实现查询历史慢sql的目的

# 2 特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 5099d94c | 2023-11-8    | 2023-11-14   |
| openGauss 5.1.1 build de769317 | 2023-11-14   | 2023-11-16   |

硬件环境信息：

| 硬件型号 | 硬件配置信息                                                 | 备注            |
| -------- | ------------------------------------------------------------ | --------------- |
| 虚拟机   | Architecture：x86_64<br/>CPU：Inter(R) Xeon(R) Gold 6278C 8核<br/>内存： 31GB<br/>硬盘：99G<br/>OS：CentOS Linux release 7.7.1908 (Core) | openGauss 5.1.1 |

# 3 测试结论概述

## 3.1 测试整体结论

支持复杂存储过程中SQL语句的分析，共执行用例47个，主要覆盖了功能测试、资料测试。功能测试覆盖查询系统表pg_stat_activity定位存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数正在执行的sql语句。配置track_stmt_stat_level参数，查询系统表statement_history记录的存储过程的慢sql。配置参数log_min_duration_statement，查询系统表statement_history记录的存储过程的慢sql。执行存储过程、函数、包体中的存储过程以及嵌套的存储过程、函数，查询系统表statement_history记录的慢sql。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单1个，1个缺陷均已解决且回归通过，整体质量良好。

## 3.2 约束说明

需设置参数

track_activities = on

enable_resource_track = on

instr_unique_sql_track_type = all

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 1    | 0    | 0      |
| 百分比 | 100%     | 0%   | 100% | 0%   | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I8F6P7](https://gitee.com/opengauss/openGauss-server/issues/I8F6P7?from=project-issue) | 主要 | 设置参数track_stmt_stat_level为'OFF,OFF'，参数instr_unique_sql_track_type为all，无法创建存储过程，数据库状态异常 | 已验收 |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 查询系统表pg_stat_activity定位存储过程执行的sql语句

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建存储过程，覆盖语句包含普通plsql、动态sql、open游标语句、perform语句，调用存储过程并在另一会话中查询系统表pg_stat_activity定位当前正在执行的sql<br>2. 创建嵌套存储过程，覆盖语句包含普通plsql、动态sql、open游标语句、perform语句，调用存储过程并在另一会话中查询系统表pg_stat_activity定位当前正在执行的sql | 执行8条用例，未发现bug |

### 4.1.2 查询系统表pg_stat_activity定位函数执行的sql语句

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建函数，覆盖嵌套函数与非嵌套函数，调用函数并在另一会话中查询系统表pg_stat_activity定位当前正在执行的sql | 执行2条用例，未发现bug |

### 4.1.3 查询系统表pg_stat_activity定位包体中存储过程执行的sql语句

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建包体，包体中包含存储过程，覆盖普通plsql、动态sql、open游标语句、perform语句，调用包体中存储过程并在另一会话中查询系统表pg_stat_activity定位当前正在执行的sql | 执行4条用例，未发现bug |

### 4.1.4 配置参数后查询系统表statement_history记录的慢sql

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. 配置参数track_stmt_stat_level，在不同参数值的情况下调用存储过程后查询系统表statement_history记录的慢sql，查看不同参数值情况下系统表statement_history的字段级别是否变更<br>2. 配置参数log_min_duration_statement，在不同参数值的情况下调用存储过程后查询系统表statement_history记录的慢sql，查看不同参数值情况下系统表statement_history的is_slow_sql字段的判定时长是否变更 | 执行16条用例，发现1个bug，现已修复且回归通过 |

### 4.1.5 查询系统表statement_history记录的存储过程执行的慢sql

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建存储过程，覆盖语句包含普通plsql、动态sql、open游标语句、perform语句、for in后跟select语句、return query、动态fors、for用来执行游标的循环语句，调用存储过程后查询系统表statement_history记录的存储过程执行的慢sql<br/>2. 创建嵌套存储过程，覆盖语句包含普通plsql、动态sql、open游标语句、perform语句、for in后跟select语句、return query、动态fors、for用来执行游标的循环语句，调用存储过程后查询系统表statement_history记录的存储过程执行的慢sql | 执行7条用例，未发现bug |

### 4.1.6 查询系统表statement_history记录的包体中存储过程执行的慢sql

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建包体，包体中包含存储过程，覆盖语句包含普通plsql、动态sql、open游标语句、perform语句、for in后跟select语句、return query、动态fors、for用来执行游标的循环语句，调用包体中存储过程后查询系统表statement_history记录的存储过程执行的慢sql | 执行8条用例，未发现bug |

### 4.1.7 查询系统表statement_history记录的函数执行的慢sql

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 创建函数，覆盖嵌套函数与非嵌套函数，调用函数后查询系统表statement_history记录的函数执行的慢sql | 执行2条用例，未发现bug |

## 4.2 资料测试

| 测试步骤                | 测试结果                              |
| ----------------------- | ------------------------------------- |
| 1. 文档描述 2. 文档示例 | 文档描述准确，示例正确无误，未发现bug |

## 4.5 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.1 build 5099d94c | 47         | Passed：47 Failed：1 | 1            |
| openGauss 5.1.1 build de769317 | 47         | Passed：47 Failed：0 | 0            |

*数据项说明：*

- 累计发现缺陷单1个，1个缺陷均已解决且回归通过
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：1(缺陷个数)/3.511kloc(代码行数)=0.28(个/kloc)

## 4.6 后续测试建议

无

# 5 附件

### 5.1 查询系统表pg_stat_activity定位存储过程执行的sql语句

```
建表并插数据
openGauss=# create table big_table(
openGauss(# column1 int,
openGauss(# column2 varchar(100)
openGauss(# );
CREATE TABLE
openGauss=# insert into big_table (column1,column2)
openGauss-# select
openGauss-# generate_series(1,10000000),
openGauss-# 'data'||generate_series(1,10000000);
INSERT 0 10000000

创建存储过程
openGauss=# create or replace procedure test_exec_plsql()
openGauss-# is
openGauss$# begin
openGauss$# insert into big_table (column1,column2) select generate_series(1,10000000),'data'||generate_series(1,10000000);
openGauss$# end;
openGauss$# /
CREATE PROCEDURE

调用存储过程
openGauss=# call test_exec_plsql();
 test_exec_plsql
-----------------

(1 row)

查询系统表pg_stat_activity定位存储过程执行的sql语句
openGauss=# select * from pg_stat_activity where query like '%call test_exec_plsql();%';
-[ RECORD 1 ]----+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
datid            | 15800
datname          | postgres
pid              | 281456624310224
sessionid        | 281456624310224
usesysid         | 10
usename          | houshaolong
application_name | gsql
client_addr      |
client_hostname  |
client_port      | -1
backend_start    | 2023-11-17 16:15:32.63216+08
xact_start       |
query_start      | 2023-11-17 16:16:31.957535+08
state_change     | 2023-11-17 16:17:03.47447+08
waiting          | f
enqueue          |
state            | idle
resource_pool    | default_pool
query_id         | 0
query            | query string: call test_exec_plsql(); current level: test_exec_plsql(); execute stmt: insert into big_table (column1,column2) select generate_series(1,10000000),'data'||generate_series(1,10000000)
connection_info  | {"driver_name":"libpq","driver_version":"(openGauss 5.1.1 build 5099d94c) compiled at 2023-11-08 14:52:29 commit 0 last mr  "}
unique_sql_id    | 0
trace_id         |


```

