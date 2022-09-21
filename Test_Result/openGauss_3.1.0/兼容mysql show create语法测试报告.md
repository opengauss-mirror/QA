![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2022-9-20 |   1.0    |           特性测试报告初稿           | zou_jialiang050 |

 关键词： show create table、show create view、show create procedure、show create function、show create trigger

摘要：

本文对openGauss实现对MYSQ中 show create table、show create view、show create procedure、show create function、show create trigger语法兼容特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  无  |          |          |

# 1     特性概述

openGauss在兼容b库下，可以实现对 show create table、show create view、show create procedure、show create function、show create trigger语法兼容，其执行结果与MYSQL侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :----------- |
| MYSQL 5.7                     | 2022-9-14    | 2022-9-22    |
| openGauss3.1.0 build 4c9602a1 | 2022-9-14    | 2022-9-22    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6161 8核<br />内存：32GB<br />硬盘：100G<br />OS：openEluer release 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss实现对MYSQL部分语法的支持特性测试中，共计执行用例59个，主要涉及 show create table、show create view、show create procedure、show create function、show create trigger语法，未发现问题，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 兼容show create table语法测试通过 |
| 功能测试 | 兼容show create view语法测试通过 |
| 功能测试 | 兼容show create procedure语法测试通过 |
| 功能测试 | 兼容show create function语法测试通过 |
| 功能测试 | 兼容show create trigger语法测试通过 |
| 资料测试 | 资料叙述准确无误，示例正确，通过                             |

## 3.2   约束说明

- 兼容MYSQL 5.7
- openGauss需在兼容b库下执行

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    0    |  0  |  0   |  0  |   0    |
| 百分比  (%) |   100    |  0  |  0   |  0  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
|  无  |    |      |  |    |

# 4     测试执行

## 4.1   功能测试

### 4.1.1   兼容show create table 语法功能验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.结合多种表类型及字段类型对show create table语法进行功能验证 | 共执行测试用例22条<br />未发现bug。 |

###  4.1.2   兼容show create view语法功能验证

| 测试步骤                                                   | 测试结果                           |
| :--------------------------------------------------------- | :--------------------------------- |
| 1.结合视图、表及视图名称对show create view语法进行功能验证 | 共执行测试用例7条<br />未发现bug。 |

###  4.1.3   兼容show create procedure语法功能验证

| 测试步骤                                                     | 测试结果                            |
| :------------------------------------------------------------ | :----------------------------------- |
| 1.结合in模式、out模式及包含循环等存储过程对show create procedure语法进行功能验证 | 共执行测试用例9条<br />未发现bug。 |

###  4.1.4   兼容show create function语法功能验证

| 测试步骤                                                     | 测试结果                            |
| :------------------------------------------------------------ | :----------------------------------- |
| 1.结合多种类型函数对show create function语法进行功能验证 | 共执行测试用例10条<br />未发现bug。 |

###  4.1.5   兼容show create trigger语法功能验证

| 测试步骤                                                     | 测试结果                           |
| :------------------------------------------------------------ | :---------------------------------- |
| 1.结合不同触发事件、触发条件及触发频率的触发器对show create trigger语法进行功能验证 | 共执行测试用例11条<br />未发现bug。 |

## 4.2   资料测试

| 测试步骤                                   |                                                     |
| :------------------------------------------ | :--------------------------------------------------- |
| 1.确认资料描述是否准确无误，示例是否正确。 | 未发现bug。 |

## 4.3   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果            | 发现问题单数 |
| :----------------------------- | :---------- | :----------------------- | :------------ |
| openGauss3.1.0 build 4c9602a1 | 59  | Passed:59<br />Failed:0 | 0          |

未发现缺陷

缺陷单共0个，修改代码量为2.32kloc， 缺陷密度为0个/kloc

## 4.4   后续测试建议

1.无

# 5     附件

```
test=# DROP TABLE IF EXISTS test;
NOTICE:  table "test" does not exist, skipping
DROP TABLE
test=# CREATE TABLE test(c1 int);
CREATE TABLE
test=# SHOW CREATE TABLE test;
 Table |              Create Table
-------+-----------------------------------------
 test  | SET search_path = public;              +
       | CREATE TABLE test (                    +
       |     c1 integer                         +
       | )                                      +
       | WITH (orientation=row, compression=no);
(1 row)

test=# DROP TABLE IF EXISTS test;
DROP TABLE
test=# CREATE TABLE test(c1 int);
CREATE TABLE
test=# CREATE VIEW v_test AS SELECT * FROM test;
CREATE VIEW
test=# SHOW CREATE VIEW v_test;
  View  |               Create View               | character_set_client | collation_connection
--------+-----------------------------------------+----------------------+----------------------
 v_test | CREATE OR REPLACE VIEW public.v_test AS+| UTF8                 | C
        | SELECT  * FROM test;                    |                      |
(1 row)

test=# CREATE FUNCTION test(integer, integer) RETURNS integer
test-# AS 'SELECT $1 + $2;'
test-# LANGUAGE sql
test-# IMMUTABLE
test-# RETURNS Null ON Null INPUT;
CREATE FUNCTION
test=# SHOW CREATE FUNCTION test;
 Function |                       Create Function                       |                      sql_mode                       | character_set_client | collation_conn
ection | Database Collation
----------+-------------------------------------------------------------+-----------------------------------------------------+----------------------+---------------
-------+--------------------
 test     | CREATE DEFINER = zjl FUNCTION public.test(integer, integer)+| sql_mode_strict,sql_mode_full_group,pipes_as_concat | UTF8                 | C
       | C
          |  RETURNS integer                                           +|                                                     |                      |
       |
          |  LANGUAGE sql                                              +|                                                     |                      |
       |
          |  IMMUTABLE STRICT NOT FENCED NOT SHIPPABLE                 +|                                                     |                      |
       |
          | AS $function$select $1 + $2;$function$;                    +|                                                     |                      |
       |
          |                                                             |                                                     |                      |
       |
(1 row)

test=# CREATE OR REPLACE PROCEDURE test(
test(# c1 IN number,
test(# c2 OUT varchar2,
test(# c3 OUT varchar2
test(# ) IS
test$# BEGIN
test$# SELECT * FROM test;
test$# END;
test$# /
CREATE PROCEDURE
test=# SHOW CREATE PROCEDURE test;
 Procedure |              Create Procedure               |                      sql_mode                       | character_set_client | collation_connection | Databa
se Collation
-----------+---------------------------------------------+-----------------------------------------------------+----------------------+----------------------+-------
-------------
 test      | CREATE DEFINER = zjl PROCEDURE public.test(+| sql_mode_strict,sql_mode_full_group,pipes_as_concat | UTF8                 | C                    | C
           | c1 IN number,                              +|                                                     |                      |                      |
           | c2 OUT varchar2,                           +|                                                     |                      |                      |
           | c3 OUR varchar2                            +|                                                     |                      |                      |
           | )                                          +|                                                     |                      |                      |
           | AS  DECLARE                                +|                                                     |                      |                      |
           | BEGIN                                      +|                                                     |                      |                      |
           | SELECT * FROM test;                        +|                                                     |                      |                      |
           | END;                                       +|                                                     |                      |                      |
           | /                                          +|                                                     |                      |                      |
           |                                             |                                                     |                      |                      |
(1 row)

test=# CREATE TABLE test(c1 int);
CREATE TABLE
test=# CREATE OR REPLACE FUNCTION test() RETURNS trigger
test-# AS $$
test$# DECLARE
test$# BEGIN
test$# DELETE FROM test;
test$# RETURN new;
test$# END
test$# $$ LANGUAGE plpgsql;
CREATE FUNCTION
test=# CREATE TRIGGER test
test-# BEFORE DELETE ON test
test-# FOR EACH ROW
test-# EXECUTE PROCEDURE test();
CREATE TRIGGER
test=# SHOW CREATE TRIGGER test;
 Trigger |                      sql_mode                       |                             SQL Original Statement                              | character_set_clie
nt | collation_connection | Database Collation
---------+-----------------------------------------------------+---------------------------------------------------------------------------------+-------------------
---+----------------------+--------------------
 test    | sql_mode_strict,sql_mode_full_group,pipes_as_concat | CREATE TRIGGER test BEFORE DELETE ON test FOR EACH ROW EXECUTE PROCEDURE test() | UTF8
   | C                    | C
(1 row)
```