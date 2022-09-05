![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-07-11 | 1.0 | 报告初稿 | zhanbiao.chen |
| 2022-07-29 | 1.1 | 完善测结论和测试执行 | zhanbiao.chen |
| 2022-08-02 | 1.2 | 根据检视意见修改报告 | zhanbiao.chen |
| 2022-08-15 | 1.3 | 遗留问题说明和规避措施 | zhanbiao.chen | 

关键词：兼容B库，dolphin插件，sql_mode

摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，通过会话级参数"sql_mode"实现兼容MySQL数据库STRICT和ONLY_FULL_GROUP_BY模式的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL的sql_mode功能。在严格模式(默认模式)即sql_mode='sql_mode_strict'>情形下，数据插入和更新保持现状，在非严格模式即sql_mode=''情形下，对超过此列限定大小的、超过此列限定长度的做截断插入，不同数据类型的取默认值0或NULL值等插入；在默认模式即 sql_mode='sql_mode_full_group' 下，如果select列表中的列没有使用聚合函数，也没有出现在group by子句，那么会报错，不在此模式即sql_mode=''下，则会执行成功，并在所有符合条件的元组中选取第一个元组。

# 2     特性测试信息

测试软件环境

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-06-27 | 2022-07-29 |
| openGauss 3.0.0 build 004f671b | 2022-06-27 | 2022-07-29 |
| dolphin 1.0 | 2022-06-27 | 2022-07-29 |

测试硬件环境

| 硬件型号 | 硬件配置信息 | 备注 |
| -------- | ------------ | ---- |
| X86 CentOS 7.6 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容sql_mode特性，共计执行395个用例，主要覆盖了功能测试、性能测试、可靠性测试和资料测试。功能测试覆盖了参数设置，直接执行及在程序块中执行严格和非严格模式下各数据类型的数据插入、修改，以及full group模式的查询兼容特性；性能测试验证了非严格模式下数据截断的性能不受影响；可靠性测试验证了sql_mode功能的测试用例在非兼容"B"库下执行数据库可稳定运行。测试中发现1个问题,已解决且回归通过，已有功能的回归发现两个问题通过SIG会议评审暂不解决，问题已挂起，提供约束说明和规避措施。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 合法与非法参数值测试，执行结果符合预期，通过 |
| 功能测试 | 在sql_mode_strict模式下执行insert into table values插入，执行结果符合预期，通过 |
| 功能测试 | 在非sql_mode_strict模式下执行insert into table values插入，执行结果符合预期，通过 |
| 功能测试 | 在sql_mode_strict模式下执行insert into table select插入，执行结果符合预期，通过 |
| 功能测试 | 在非sql_mode_strict模式下执行insert into table select插入，执行结果符合预期，通过 |
| 功能测试 | 在sql_mode_strict模式下执行update数据修改,执行结果符合预期，通过 |
| 功能测试 | 在非sql_mode_strict模式下执行update数据修改,执行结果符合预期，通过 |
| 功能测试 | sql_mode_strict模式和非sql_mode_strict模式测试覆盖tinyint、smallint、int、bigint、float4、double precision、numeric、char、varchar、money、boolean、bytea、timestamp、point、inet、bit、数组等数据类型，执行结果符合预期，通过 |
| 功能测试 | sql_mode_strict模式和非sql_mode_strict模式测试覆盖列定义无默认属性、默认非空等情形下的数据插入、修改，执行结果符合预期，通过 |
| 功能测试 | sql_mode_strict模式和非sql_mode_strict模式测试覆盖直接执行insert、update语句，和在程序块(存储函数、存储过程)中执行，执行结果符合预期，通过 |
| 功能测试 | 在sql_mode_full_group模式下，group by列表包含select列表下的列，执行数据查询，执行结果符合预期，通过 |
| 功能测试 | 在sql_mode_full_group模式下，group by列表不包含select列表下的列，执行数据查询，执行结果符合预期，通过 |
| 功能测试 | 在非sql_mode_full_group模式下，group by列表包含select列表下的列，执行数据查询，执行结果符合预期，通过 |
| 功能测试 | 在非sql_mode_full_group模式下，group by列表不包含select列表下的列，执行数据查询，执行结果符合预期，通过 |
| 可靠性测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 用户锁功能需在openGauss兼容B库下安装dolphin插件验证。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| [I5GHDP](https://gitee.com/opengauss/Plugin/issues/I5GHDP?from=project-issue) | xor操作符返回结果有误，当设置sql_mode为空时，返回结果正常 | 次要 | 当输入值为非法数据，sql_mode为默认参数时会导致执行报错，需要设置sql_mode为非严格模式 | 挂起  |
| [I5FHEG](https://gitee.com/opengauss/Plugin/issues/I5FHEG?from=project-issue) | 函数log\log2\log10\sqrt参数含有字符串时，必须取消严格模式，否则返回结果错误 | 次要 | 当输入值为非法数据，sql_mode为默认参数时会导致执行报错，需要设置sql_mode为非严格模式 | 挂起 |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 1    | 2    | 0      |
| 百分比 | 100%     | 0    | 33%  | 67%  | 0      |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I5EJ9L](https://gitee.com/opengauss/Plugin/issues/I5EJ9L?from=project-issue) | 截断插入int类型时正数变为负数，负数变为正数；bigint正数变为负数 | 主要 | 已验收 |
| [I5GHDP](https://gitee.com/opengauss/Plugin/issues/I5GHDP?from=project-issue) | xor操作符返回结果有误，当设置sql_mode为空时，返回结果正常 | 次要 | 挂起  |
| [I5FHEG](https://gitee.com/opengauss/Plugin/issues/I5FHEG?from=project-issue) | 函数log\log2\log10\sqrt参数含有字符串时，必须取消严格模式，否则返回结果错误 | 次要 | 挂起 |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 参数值测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 设置合法和非法的参数值 | 执行5条用例，结果符合预期，测试通过 |

#### 4.1.1.3 SQL_MODE_STRICT INSERT测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在sql_mode_strict模式下执行insert into table values插入<br />2. 在非sql_mode_strict模式下执行insert into table values插入<br />3. 在sql_mode_strict模式下执行insert into table select插入<br />4. 在非sql_mode_strict模式下执行insert into table select插入<br /> | 执行300条用例，发现1个问题，已修复且验收通过，测试通过 |

#### 4.1.1.3 SQL_MODE_STRICT UPDATE测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在sql_mode_strict模式下执行update数据修改<br />2. 在非sql_mode_strict模式下执行update数据修改 | 执行40条用例，执行结果符合预期，测试通过 |

#### 4.1.1.4 SQL_MODE_STRICT PROCEDURE测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. sql_mode_strict模式下在PROCEDURE中执行INSERT<br />2. 非sql_mode_strict模式下在PROCEDURE中执行INSERT<br />3. sql_mode_strict模式下在PROCEDURE中执行UPDATE<br />4. 非sql_mode_strict模式下在PROCEDURE中执行UPDATE | 执行20条用例，执行结果符合预期，测试通过 |

#### 4.1.1.5 SQL_MODE_STRICT FUNCTION测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. sql_mode_strict模式下在FUNCTION中执行INSERT<br />2. 非sql_mode_strict模式下在FUNCTION中执行INSERT<br />3. sql_mode_strict模式下在FUNCTION中执行UPDATE<br />4. 非sql_mode_strict模式下在FUNCTION中执行UPDATE | 执行20条用例，执行结果符合预期，测试通过 |

#### 4.1.1.6 SQL_MODE_FULL_GROUP测试
| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在sql_mode_full_group模式下，group by列表包含select列表下的列，执行数据查询<br />2. 在sql_mode_full_group模式下， group by列表不包含select列表下的列，执行数据查询<br />3. 在非sql_mode_full_group模式下，group by列表包含select列表下的列，执行数据查询<br />4. 在非sql_mode_full_group模式下，group by列表不包含select列表下的列，执行数据查询  | 执行10条用例<br />执行结果符合预期，测试通过 |

### 4.1.2 性能测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 针对sql_mode_strict模式进行性能测试<br />2. 测试正常情况下100W行数据插入和修改的耗时<br />3. 测试100W行数据在截断插入和修改情况下的耗时<br />4. 对比性能结果，测试SQL和数据附文末 | 无明显性能损耗，执行结果符合预期，测试通过 |

### 4.1.3 可靠性测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行395条用例，执行结果符合预期，测试通过 |

### 4.1.4 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
| openGauss 3.0.0 build 004f671b | 395 | Passed: 395<br />Failed: 0 | 3 |

## 4.2   后续测试建议

- sql_mode参数对其它MySQL兼容功能是否有影响

# 5     附件

## 5.1 性能测试相关

### 5.1.1 性能测试环境

| 硬件型号 | 硬件配置信息 | 备注 |
| -------- | ------------ | ---- |
| X86 CentOS 7.6 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

### 5.1.2 性能测试结果

SQL_MODE转换INSERT和UPDATE 100W行数据性能测试(耗时单位秒S，循环执行5次)
| 测试项 | 1 | 2 | 3 | 4 | 5 | AVG |
| ------ | - | - | - | - | - | --- |
|多列数据插入不转换 | 4.56 | 6.45 | 4.90 | 5.50 | 6.18 | 5.52 |
|多列数据插入数值截断 | 4.10 | 4.09 | 4.14 | 4.24 | 4.36 | 4.19 |
|多列数据插入字符截断 | 5.98 | 4.60 | 4.51 | 4.82 | 6.18 | 5.21 |
|update数值不转换 | 12.47 | 11.79 | 12.45 | 10.59 | 10.65 | 11.59 |
|update数值截断后面字符 | 12.54 | 12.44 | 12.38 | 11.30 | 10.54 | 11.84 |
|update数值截断前面字符 | 12.30 | 12.48 | 11.40 | 10.58 | 10.60 | 11.47 |
|update字符不转换 | 12.51 | 11.99 | 10.67 | 10.62 | 11.91 | 11.54 |
|update字符截断 | 12.11 | 11.45 | 10.61 | 12.33 | 12.52 | 11.80 |
|程序块内循环insert数值不转换 | 19.94 | 19.68 | 19.77 | 19.88 | 20.26 | 19.91 |
|程序块内循环insert数值超过最大值截断 | 20.07 | 19.61 | 19.89 | 19.73 | 20.40 | 19.94 |
|程序块内循环insert数值截断后面字符 | 20.02 | 20.30 | 20.48 | 20.13 | 20.00 | 20.18 |
|程序块内循环insert数值截断前面字符 | 20.48 | 20.38 | 19.92 | 19.94 | 20.14 | 20.17 |
|程序块内循环insert字符不转换 | 17.40 | 16.79 | 17.42 | 17.33 | 17.21 | 17.23 |
|程序块内循环insert字符截断 | 17.06 | 17.36 | 17.19 | 17.51 | 17.29 | 17.28 |
 
### 5.1.3 性能测试SQL

```sql
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--创建数据表
drop table if exists tt;
create table tt(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into tt select a,a+1,a+2,a*0.12,a*0.13,now(),'a','aaa','test' from generate_series(1,1000000) as a;
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--整表插入，第一次插入作为缓存，结果忽略
\timing on
set sql_mode = '';
show sql_mode;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
---------------------------------------------
insert into t1 select * from tt;
---------------------------------------------
drop table t1;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
---------------------------------------------
insert into t1 select * from tt;
---------------------------------------------
drop table t1;

create table t2(c1 smallint,c2 smallint,c3 smallint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
---------------------------------------------
insert into t2 select * from tt;
---------------------------------------------
drop table t2;

create table t3(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(1),c9 text);
---------------------------------------------
insert into t3 select * from tt;
---------------------------------------------
drop table t3;
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--全表update
\timing on
set sql_mode = '';
show sql_mode;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into t1 select * from tt;
---------------------------------------------
update t1 set c1 = 100;
---------------------------------------------
drop table t1;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into t1 select * from tt;
---------------------------------------------
update t1 set c1 = '100a';
---------------------------------------------
drop table t1;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into t1 select * from tt;
---------------------------------------------
update t1 set c1 = 'a100';
---------------------------------------------
drop table t1;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into t1 select * from tt;
---------------------------------------------
update t1 set c7 = 'b';
---------------------------------------------
drop table t1;

create table t1(c1 int,c2 bigint,c3 bigint,c4 numeric(20,2),c5 float8,c6 timestamp,c7 char(1),c8 varchar(5),c9 text);
insert into t1 select * from tt;
---------------------------------------------
update t1 set c7 = 'bb';
---------------------------------------------
drop table t1;
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--function内循环insert数值
\timing on
set sql_mode = '';
show sql_mode;

create table t4(c1 int,c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 2147483648,'test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;

create table t4(c1 int,c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 2147483649,'test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;

create table t4(c1 int,c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 2147483648 || 'a','test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;

create table t4(c1 int,c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 'a' || 2147483648,'test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--function内循环insert字符
\timing on
set sql_mode = '';
show sql_mode;

create table t4(c1 char(1),c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 'a','test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;

create table t4(c1 char(1),c2 text);
create function f1() returns void
as $$ begin
for i in 1..1000000 loop
insert into t4 select 'aa','test'; 
end loop;
end; $$ language plpgsql;
---------------------------------------------
select f1();
---------------------------------------------
drop function f1();
drop table t4;
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
```
 
