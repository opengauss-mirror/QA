修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2023-09-26 | 1.0 | 测试报告初稿完成 | cloudsbreak |


关键词：

dolphin插件，sql security语法，兼容B库

摘要：

本文档主要介绍openGauss在兼容B库且有dolphin插件的情形下，实现支持sql security语法。语法描述：CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] [DEFINER = user] [SQL SECURITY { DEFINER | INVOKER }] VIEW view_name [(column_list)] AS select_statement [WITH [CASCADED | LOCAL] CHECK OPTION]

# 1     特性概述

openGauss在兼容B库情形下，安装dolphin插件，可以支持sql security语法。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| dolphin 2.0 | 2023-09-20 | 2023-09-22 |
| mysql 5.7 | 2023-09-20 | 2023-09-22 |
| openGauss 5.1.0 build 24d9203f | 2023-09-20 | 2023-09-22 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 虚拟机 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.00GHz 20核 OS：CentOS Linux  7 (Core) |  |


# 3     测试结论概述

## 3.1   测试整体结论
openGauss兼容mysql，支持sql security语法，测试用例54条，主要聚焦于功能测试。功能测试覆盖：视图、函数体和存储过程使用中支持sql security语法，不同权限的用户创建，然后通过赋权交叉调用查看是否支持sql security语法，校验输出结果。累计发现问题单1个，特性质量良好。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 创建视图支持sql security语法-不同权限用户创建-赋权交叉调用，执行结果符合预期，通过 |
| 功能测试 | 创建视图支持sql security语法-备份恢复，执行结果符合预期，通过 |
| 功能测试 | 创建视图支持sql security语法-alter view，执行结果符合预期，通过 |

## 3.2   约束说明
1.openGauss需使用兼容B库且有dolphin插件
2.mysql使用8.0版本

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| [I83D1P](https://gitee.com/opengauss/openGauss-server/issues/I83D1P) | 创建视图支持sql security语法-存储过程、函数中创建，修改视图——权限问题 | 主要 | 规避修改视图场景 | 未解决 |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 1 | 0 | 1 | 0 | 0 |
| 百分比 | 100% | 0% | 100% | 0% | 0% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I83D1P](https://gitee.com/opengauss/openGauss-server/issues/I83D1P) | 主要 | 创建视图支持sql security语法-存储过程、函数中创建，修改视图——权限问题 | 未解决 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 编译安装openGauss数据库<br />2.编译dolphin插件<br />3.创建兼容B库并安装dolphin插件<br />4.执行用例 |编译库及插件成功，共执行54条用例，共发现1个bug。|
#### 4.1.1.1 创建视图支持sql security语法-不同权限用户创建-赋权交叉调用测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行37条用例，共发现1个bug。|
#### 4.1.1.2 创建视图支持sql security语法-备份恢复测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行2条用例，共发现0个bug。|
#### 4.1.1.3 创建视图支持sql security语法-alter view语句测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行15条用例，共发现0个bug。|
## 4.2   测试执行统计数据
| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| openGauss 5.1.0 build 24d9203f | 54 | Passed：43 Failed：11 | 1 |

数据说明

1. 累计发现问题单1个，失败用例根因一致
## 4.3   后续测试建议
无
# 5     附件
## 5.1 创建视图支持sql security语法的示例
--**创建视图支持sql security语法-超户创建视图缺省definer,sql security 是definer和invoker，超户可以正常查询**

--创建MySQL兼容数据库
create database db_a1144421 dbcompatibility 'B';
--进入数据库
\c db_a1144421;
--建表
create table sql_security_1144421(id int,cal int);
insert into sql_security_1144421 value(1,1);
insert into sql_security_1144421 value(2,2);
insert into sql_security_1144421 value(3,3);
--缺省
create sql security definer view v_1144421_1 as select * from sql_security_1144421 where id <=2;
create sql security invoker view v_1144421_11 as select * from sql_security_1144421 where id <=2;
create definer=current_user() sql security definer view v_1144421 as select * from sql_security_1144421;
create definer=current_user sql security definer view v_1144421_2 as select * from sql_security_1144421;
--超户调用
select * from v_1144421 order by 1,2;
select * from v_1144421_1 order by 1,2;
select * from v_1144421_11 order by 1,2;
select * from v_1144421_2 order by 1,2;
create or replace sql security definer view v_1144421_1 as select * from sql_security_1144421 ;
select * from v_1144421_1 order by 1,2;

--**创建视图支持sql security语法-备份恢复测试**

--创建MySQL兼容数据库
create database db_a1145004 dbcompatibility 'B';
create database db_a1145004_bak dbcompatibility 'B';
--进入数据库
\c db_a1145004;
--创建普通用户
create user use_a_1145004 identified by 'A@123456';
create user use_b_1145004 identified by 'A@123456';
grant all on schema public to use_a_1145004;
grant all on schema public to use_b_1145004;
--超户建表和视图
create table sql_security_1145004(id int,cal int);
insert into sql_security_1145004 value(1,1);
insert into sql_security_1145004 value(2,2);
insert into sql_security_1145004 value(3,3);
create table sql_security_1145004_1(id int,cal int);
insert into sql_security_1145004_1 value(3,1);
insert into sql_security_1145004_1 value(4,2);
insert into sql_security_1145004_1 value(5,3);
create definer=use_a_1145004 sql security invoker view public.v_1145004 as select * from sql_security_1145004 with local check option;
create definer=use_a_1145004 sql security definer view public.v_1145004_1 as select * from sql_security_1145004_1 with local check option;
create sql security definer view v_1145004_11 as select * from sql_security_1145004_1;
create sql security invoker view public.v_1145004_12 as select * from sql_security_1145004;
create definer=use_b_1145004 sql security definer view v_1145004_111 as select * from sql_security_1145004;
create definer=use_b_1145004 sql security invoker view public.v_1145004_121 as select * from sql_security_1145004_1;
--超户调用
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
--用户a 调用
set role use_a_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role;
--用户b调用
set role use_b_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role;
--3.导出
gs_dump -f $PGDATA/er_me1145004.sql db_a1145004
--4.导入到新数据库
gsql -d db_a1145004_bak -f $PGDATA/er_me1145004.sql
--5.在数据库中调用
\c db_1142042_bak;
--超户调用
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
--用户a 调用
set role use_a_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role;
--用户b调用
set role use_b_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role;
--超户调用
grant select on sql_security_1145004_1 to use_a_1145004;
grant select on sql_security_1145004 to use_b_1145004;
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
--用户a 调用
grant select on sql_security_1145004 to use_a_1145004;
grant select on v_1145004_11 to use_a_1145004;
grant select on v_1145004_12 to use_a_1145004;
grant select on v_1145004_111 to use_a_1145004;
grant select on v_1145004_121 to use_a_1145004;
set role use_a_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role;
--用户b调用
grant select on sql_security_1145004_1 to use_b_1145004;
grant select on v_1145004_11 to use_b_1145004;
grant select on v_1145004_12 to use_b_1145004;
grant select on v_1145004 to use_b_1145004;
grant select on v_1145004_1 to use_b_1145004;
set role use_b_1145004 password 'A@123456';
select * from v_1145004 order by 1,2;
select * from v_1145004_1 order by 1,2;
select * from v_1145004_11 order by 1,2;
select * from v_1145004_12 order by 1,2;
select * from v_1145004_111 order by 1,2;
select * from v_1145004_121 order by 1,2;
reset role; 

--**创建视图支持sql security语法-alter view-普通用户创建视图和表，普通用户alter view当前用户**

--创建MySQL兼容数据库
create database db_a1144484 dbcompatibility 'B';
--进入数据库
\c db_a1144484;
--创建普通用户
create user use_a_1144484 identified by 'A@123456';
create user use_b_1144484 identified by 'A@123456';
create user use_c_1144484 identified by 'A@123456';
grant all on schema public to use_a_1144484;
set role use_a_1144484 password 'A@123456';
--普通用户建表和视图
create table sql_security_1144484(id int,cal int);
insert into sql_security_1144484 value(1,1);
insert into sql_security_1144484 value(2,2);
insert into sql_security_1144484 value(3,3);
create table sql_security_1144484_1(id int,cal int);
insert into sql_security_1144484_1 value(3,1);
insert into sql_security_1144484_1 value(4,2);
insert into sql_security_1144484_1 value(5,3);
create definer=use_a_1144484 sql security invoker view public.v_1144484 as select * from sql_security_1144484 with local check option;
create definer=use_a_1144484 sql security definer view public.v_1144484_1 as select * from sql_security_1144484_1 with local check option;
reset role;
--超户调用
select * from v_1144484 order by 1,2;
select * from v_1144484_1 order by 1,2;
set role use_a_1144484 password 'A@123456';
select * from public.v_1144484 order by 1,2;
select * from public.v_1144484_1 order by 1,2;
reset role;
--其他用户调用
grant select on public.v_1144484 to use_b_1144484;
grant select on public.v_1144484_1 to use_b_1144484;
grant select on use_a_1144484.sql_security_1144484 to use_b_1144484;
grant all on schema use_a_1144484 to use_b_1144484;
set role use_b_1144484 password 'A@123456';
select * from public.v_1144484 order by 1,2;
select * from public.v_1144484_1 order by 1,2;
reset role;
--alter view
set role use_a_1144484 password 'A@123456';
alter definer=current_user sql security invoker view public.v_1144484 as select * from sql_security_1144484;
alter definer=current_user sql security invoker view public.v_1144484_1 as select * from sql_security_1144484;
alter definer=use_a_1144484 sql security invoker view public.v_1144484 as select * from sql_security_1144484;
alter definer=use_a_1144484 sql security invoker view public.v_1144484_1 as select * from sql_security_1144484;
reset role;
--超户调用
select * from v_1144484 order by 1,2;
select * from v_1144484_1 order by 1,2;
--视图所有者调用
set role use_a_1144484 password 'A@123456';
select * from public.v_1144484 order by 1,2;
select * from public.v_1144484_1 order by 1,2;
reset role;
--其他用户正常调用
set role use_b_1144484 password 'A@123456';
select * from public.v_1144484 order by 1,2;
select * from public.v_1144484_1 order by 1,2;
reset role;
