![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者            |
| :--------- | -------- | -------------------- | --------------- |
| 2023-04-20 | 1.0      | 特性测试报告初稿完成 | zou_jialiang050 |
| 2023-04-24 | 1.1      | 修改特性测试报告     | zou_jialiang050 |

关键词： 

M*、dolphin插件、协议、`JDBC`

摘要：

本文档主要基于dolphin插件兼容M*协议的语法特性进行测试并给出最终测试结论。即分别使用M\*客户端和`JDBC`连接openGauss的兼容B库，使得B库兼容语法可以通过M\*客户端和`JDBC`连接的两种方式执行并获取返回结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1	特性概述

基于dolphin插件，实现openGauss对M*协议的语法特性兼容。

- 第一，使用M\*客户端连接openGauss兼容B库，并执行相关SQL语句；


- 第二，使用M\*`JDBC`连接openGauss兼容B库，并执行相关SQL语句；


# 2	特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build 90dad678 | 2023-4-13    | 2023-04-20   |
| openGauss 5.0.0 build 1c922789 | 2023-04-23   | 2023-04-24   |
| openGauss 5.1.0 build 68d1772f | 2023-06-28   | 2023-06-29   |
| dolphin build 246feca1         | 2023-04-13   | 2023-04-20   |
| dolphin build 2896e501         | 2023-04-23   | 2023-04-24   |
| dolphin build 08775bc0         | 2023-06-28   | 2023-06-29   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3	测试结论概述

## 3.1 测试整体结论

基于dolphin插件兼容M*协议的语法特性测试，共计执行50条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下7点：

1. M\*客户端`TCP/IP`连接测试及创建用户连接测试；
2. `CREATE/DROP DATABASE/SCHEMA`测试；
3. `CREATE TABLE`结合约束、存储引擎、`COMMENT`及表类型和分区表测试；
4. `ALTER TABLE`结合列操作及分区操作测试；
5. `DROP TABLE`包含参数的功能验证；
6. `DESC/DESCRIBE/ANALYZE TABLE`功能测试；
7. `PG_CLASS`、`PG_INDEX`等系统表查询验证；
8. 多种数据类型建表测试；
9. 创建`BTREE/UBTREE/HASH`等多种类型索引测试；
10. `CREATE/ALTER TABLE`同时`CREATE INDEX`语法测试；
11. `VISBILE/INVISIBLE INDEX`功能测试及`SHOW INDEX FROM`测试；
12. `SHOW DATABASES/SHOW TABLE STATUS/SHOW PROCESSLIST`等多种`SHOW`语法测试；
13. `CREATE FUNCTION/PROCEDURE`及`SHOW FUNCTION/PROCEDURE STATUS`语法测试；
14. `CREATE TRIGGER`及`SHOW TRIGGERS`语法测试；
15. `SELECT/INSERT/UPDATE/DELETE`及`DISTINCT`支持`ORDER BY`表达式语法测试；
16. `EXPLAIN`及`PREPARE/EXECUTE`语句功能测试；
17. `CREATE USER`及`GRANT/REVOKE PRIVILEGE`权限测试；
18. `START TRANSACTION/BEGIN`两种方式开启事务语法测试；
19. `CREATE VIEW`及`SELECT INTO`语法测试；
20. 兼容数学函数、条件类型函数及日期时间处理函数测试；
21. 兼容`!`操作符及多数据类型四则运算语法测试；

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

累计发现缺陷单5个，其中1个缺陷单已取消，剩余所有缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | M\*客户端`TCP/IP`连接测试及创建用户连接测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE/DROP DATABASE/SCHEMA`测试，执行结果符合预期，通过    |
| 功能测试 | `CREATE TABLE`结合约束、存储引擎、`COMMENT`及表类型和分区表测试，<br/>执行结果符合预期，通过 |
| 功能测试 | `ALTER TABLE`结合列操作及分区操作测试，执行结果符合预期，通过 |
| 功能测试 | `DROP TABLE`包含参数的功能验证，执行结果符合预期，通过       |
| 功能测试 | `DESC/DESCRIBE/ANALYZE TABLE`功能测试，执行结果符合预期，通过 |
| 功能测试 | `PG_CLASS`、`PG_INDEX`等系统表查询验证，执行结果符合预期，通过 |
| 功能测试 | 多种数据类型建表测试，执行结果符合预期，通过                 |
| 功能测试 | 创建`BTREE/UBTREE/HASH`等多种类型索引测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE/ALTER TABLE`同时`CREATE INDEX`语法测试，执行结果符合预期，通过 |
| 功能测试 | `VISBILE/INVISIBLE INDEX`功能测试及`SHOW INDEX FROM`测试，<br/>执行结果符合预期，通过 |
| 功能测试 | `SHOW DATABASES/SHOW TABLE STATUS/SHOW PROCESSLIST`等<br/>多种`SHOW`语法测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE FUNCTION/PROCEDURE`及`SHOW FUNCTION/PROCEDURE STATUS`语法<br/>测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE TRIGGER`及`SHOW TRIGGERS`语法测试，执行结果符合预期，通过 |
| 功能测试 | `SELECT/INSERT/UPDATE/DELETE`及`DISTINCT`支持`ORDER BY`表达式<br/>语法测试，执行结果符合预期，通过 |
| 功能测试 | `EXPLAIN`及`PREPARE/EXECUTE`语句功能测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE/ALTER USER`及`GRANT/REVOKE PRIVILEGE`权限测试，执行结果符合预期，通过 |
| 功能测试 | `START TRANSACTION/BEGIN`两种方式开启事务语法测试，执行结果符合预期，通过 |
| 功能测试 | `CREATE VIEW`及`SELECT INTO`语法测试，执行结果符合预期，通过 |
| 功能测试 | 兼容数学函数、条件类型函数及日期时间处理函数测试，执行结果符合预期，通过 |
| 功能测试 | 兼容`!`操作符及多数据类型四则运算语法测试，执行结果符合预期，通过 |
| 资料测试 | 资料描述正确，示例的执行无误，通过                           |

## 3.2	约束说明

- M*客户端需使用5.7.38及以上版本，低于5.7.38的小版本兼容待后续迭代优化；
- openGauss需使用兼容B库；
- M\*`JDBC`需使用5.1.47版本；
- 因M\*的`DATABASE`与openGauss的`SCHEMA`相同，故M*客户端连接B库时无法执行`CREATE DATABASE`带参数(如`DBCOMPATIBILITY`)的语句；
- 使用M\*客户端连接B库前需先使用`gsql`客户端连接一次B库，才能加载`DOLPHIN`动态库，拉起指定端口，否则会连接失败；

## 3.3    遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 0    | 0    | 5    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                       | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ---------------------------------------------- | -------- |
| 1    | [I6WITO](https://gitee.com/opengauss/Plugin/issues/I6WITO?from=project-issue) | 次要     | 使用mysql客户端连接B库后无法查看执行计划       | 已验收   |
| 2    | [I6WLVL](https://gitee.com/opengauss/Plugin/issues/I6WLVL?from=project-issue) | 次要     | 使用mysql客户端连接B库，触发器创建失败         | 已验收   |
| 3    | [I6WNRF](https://gitee.com/opengauss/Plugin/issues/I6WNRF?from=project-issue) | 次要     | 使用mysql客户端连接B库，执行execute语句失败    | 已验收   |
| 4    | [I6WZLR](https://gitee.com/opengauss/Plugin/issues/I6WZLR?from=project-issue) | 次要     | 使用mysql的jdbc连接B库，创建函数及存储过程报错 | 已取消   |
| 5    | [I6WZVX](https://gitee.com/opengauss/Plugin/issues/I6WZVX?from=project-issue) | 次要     | 使用mysql的jdbc连接B库，开启事务报错           | 已验收   |

# 4    测试执行

## 4.1 测试执行步骤

###  4.1.1 M\*客户端`TCP/IP`连接测试及创建用户连接测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.指定正确的`-h -P`参数测试连接<br/>2.指定错误的`-h`参数测试连接<br/>3.指定错误的`-P`参数测试连接<br/>4.新建用户并指定新用户连接B库 | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.2 `CREATE/DROP DATABASE/SCHEMA`测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE DATABASE`<br/>2.`CREATE SCHEMA`<br/>3.`DROP DATABASE`<br/>4.`DROP SCHEMA` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.3 `CREATE TABLE`结合约束、存储引擎、`COMMENT`及表类型和分区表测试

| 测试步骤                                                     | 测试结果                                |
| ------------------------------------------------------------ | --------------------------------------- |
| 1.`CREATE TABLE`结合`PRIMARY KEY/FOREIGN KEY/UNIQUE/NOT NULL`约束<br/>2.`CREATE TABLE`结合`COMMENT`<br/>3.`CREATE TABLE`结合存储引擎<br/>4.`CREATE TABLE`结合字符集和排序规则<br/>5.创建`USTORE/UNLOGGED/SEGMENT`表<br/>6.创建一级分区表和二级分区表 | 执行10条用例<br/>未发现bug<br/>执行通过 |

### 4.1.4 `ALTER TABLE`结合列操作及分区操作测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`ALTER TABLE ADD/DROP/MODIFY`修改列<br/>2.`ALTER TABLE REBUILD/REMOVE/CHECK/REPAIR/OPTIMIZE`分区操作 | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.5 `DROP TABLE`包含参数的功能验证

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`DROP TABLE CASCADE`<br/>2.`DROP TABLE CASCADE CONSTRAINTS` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.6 `DESC/DESCRIBE/ANALYZE TABLE`功能测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`SHOW CREATE TABLE TAB_NAME`<br/>2.`SHOW TABLES`<br/>3.`DESC TAB_NAME`<br/>4.`DESCRIBE TAB_NMAE`<br/>5.`ANALYZE TABLE` | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.7 `PG_CLASS`、`PG_INDEX`等系统表查询验证

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE TABLE`并查询`PG_CLASS`系统表<br/>2.`CREATE INDEX`并查询`PG_INDEX`系统表 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.8 多种数据类型建表测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE TABLE`包含`BIGINT/FLOAT8/NUMERIC/BIT/VARBINARY/`<br/>`LONGBLOB/ENUM/SET/TEXT/JSON`等28种数据类型建表 | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.9 创建`BTREE/UBTREE/HASH`等多种类型索引测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE INDEX USING BTREE`<br/>2.`CREATE INDEX USING UBTREE`<br/>3.`CREATE INDEX USING HASH`<br/>4.`CREATE INDEX USING Psort`<br/>5.`CREATE GLOBAL/LOCAL INDEX` | 执行4条用例<br/>未发现bug<br/>执行通过 |

### 4.1.10 `CREATE/ALTER TABLE`同时`CREATE INDEX`语法测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE TABLE`同时`CREATE INDEX`<br/>2.`ALTER TABLE`同时`CREATE INDEX` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.11 `VISBILE/INVISIBLE INDEX`及`SHOW INDEX FROM`测试

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1.设置`INVISIBLE INDEX`并执行查询计划<br/>2.设置`VISIBLE INDEX`并执行查询计划<br/>3.`SHOW INDEX FROM TAB_NAME` | 执行2条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.12 `SHOW DATABASES/SHOW TABLE STATUS/SHOW PROCESSLIST`等多种`SHOW`语法测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`SHOW DATABASES`<br/>2.`OPTIMIZE`<br/>3.`SET CHARSET`<br/>4.`SHOW MASTER STATUS`<br/>5.`SHOW SALVE HOSTS`<br/>6.`SHOW GRANTS FOR 'USERNAME'`<br/>7.`SHOW TABLE STATUS LIKE 'TAB_NAME'`<br/>8.`SHOW VARIABLES`<br/>9.`FLUSH BINARY LOGS`<br/>10.`SHOW GUC参数名` | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.13 `CREATE FUNCTION/PROCEDURE`及`SHOW FUNCTION/PROCEDURE STATUS`语法测试

| 测试步骤                                                     | 测试结果                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| 1.`CREATE FUNCTION`<br/>2.`CREATE PROCEDURE`<br/>3.`SHOW FUNCTION STATUS`<br/>4.`SHOW PROCEDURE STATUS` | 执行2条用例<br/>共发现1条bug<br/>该issue以非问题取消 |

### 4.1.14 `CREATE TRIGGER`及`SHOW TRIGGERS`语法测试

| 测试步骤                                 | 测试结果                                            |
| ---------------------------------------- | --------------------------------------------------- |
| 1.`CREATE TRIGGER`<br/>2.`SHOW TRIGGERS` | 执行1条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.15 `SELECT/INSERT/UPDATE/DELETE`及`DISTINCT`支持`ORDER BY`表达式语法测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.验证`SELECT`嵌套查询<br/>2.验证`SELECT`联合查询<br/>3.验证`INSERT/UPDATE/DELETE`语句<br/>4.验证`DISTINCT`支持`ORDER BY`表达式 | 执行4条用例<br/>未发现bug<br/>执行通过 |

### 4.1.16 `EXPLAIN`及`PREPARE/EXECUTE`语句功能测试

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1.执行`EXPLAIN`查看SQL执行计划<br/>2.`PREPARE`语句测试<br/>3.`EXECUTE`语句测试 | 执行2条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.17 `CREATE USER`及`GRANT/REVOKE PRIVILEGE`权限测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.`CREATE USER`并授予`SELECT INSERT`权限<br/>2.使用新用户连接并测试`SELECT INSERT`<br/>3.`REVOKE`新用户`INSERT`权限<br/>4.使用新用户连接并测试`SELECT INSERT` | 执行1条用例<br/>未发现bug<br/>执行通过 |

### 4.1.18 `START TRANSACTION/BEGIN`两种方式开启事务语法测试

| 测试步骤                                                  | 测试结果                                            |
| --------------------------------------------------------- | --------------------------------------------------- |
| 1.`START TRANSACTION;...COMMIT;`<br/>2.`BEGIN;...COMMIT;` | 执行1条用例<br/>共发现1条bug<br/>现已解决且回归通过 |

### 4.1.19 `CREATE VIEW`及`SELECT INTO`语法测试

| 测试步骤                                             | 测试结果                               |
| ---------------------------------------------------- | -------------------------------------- |
| 1. `CREATE VIEW`并`SELECT VIEW`<br/>2. `SELECT INTO` | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.20 兼容数学函数、条件类型函数及日期时间处理函数测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1. 三种兼容数学函数运算`PI()/CONV()/CRC32()`<br/>2. 兼容条件类型函数运算<br/>3. 兼容日期时间处理函数 | 执行6条用例<br/>未发现bug<br/>执行通过 |

### 4.1.21 兼容`!`操作符及多数据类型四则运算语法测试

| 测试步骤                                                   | 测试结果                               |
| ---------------------------------------------------------- | -------------------------------------- |
| 1. 兼容`!`操作符测试<br/>2. 兼容28种数据类型的四则运算测试 | 执行2条用例<br/>未发现bug<br/>执行通过 |

### 4.1.22 资料测试

| 测试步骤                             | 测试结果               |
| ------------------------------------ | ---------------------- |
| 1.资料描述是否准确、示例能否正确执行 | 未发现bug<br/>执行通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 5.0.0 build 90dad678 | 50         | Passed：41<br>Failed：9 | 5            |
| openGauss 5.0.0 build 1c922789 | 50         | Passed：48<br>Failed：2 | 0            |
| openGauss 5.0.0 build 68d1772f | 50         | Passed：50<br>Failed：0 | 0            |

数据项说明：

* 缺陷密度：4(缺陷个数，其中1个已取消)/1.578kloc(代码行数)=2.53(个/kloc)
* 问题单[I6WZLR](https://gitee.com/opengauss/Plugin/issues/I6WZLR?from=project-issue)以非问题取消，因`DELIMITER`为M\*客户端专用，`JDBC`不需要设置也不支持设置

## 4.3  后续测试建议

无

# 5   附件

## 5.1 M\*客户端连接B库

```sql
--step1:create table结合非空约束;expect:创建成功
mysql> drop table if exists tab_protocol_0007;
Query OK, 0 rows affected (0.01 sec)
mysql> create table tab_protocol_0007
    -> (id tinyint
    -> ,name varchar(100) not null);
Query OK, 0 rows affected (0.00 sec)

--step2:insert数据;expect:insert成功
mysql> insert into tab_protocol_0007(id,name)
    -> values(1,'aa'),(2,'bb'),(3,'cc');
Query OK, 3 rows affected (0.01 sec)

--step3:查询数据;expect:查询成功
mysql> select * from tab_protocol_0007 order by id;
+------+------+
| id   | name |
+------+------+
|    1 | aa   |
|    2 | bb   |
|    3 | cc   |
+------+------+
3 rows in set (0.00 sec)

--step4:清理环境;expect:清理成功
mysql> drop table if exists tab_protocol_0007 cascade constraints;
Query OK, 0 rows affected (0.01 sec)
```
