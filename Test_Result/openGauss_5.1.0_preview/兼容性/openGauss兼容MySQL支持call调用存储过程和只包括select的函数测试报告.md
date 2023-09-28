版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2023-9-25 | 1.0 | 测试报告初稿完成 | danny20001210 |


关键词：

dolphin插件，Call调用，存储过程，函数，兼容B库

摘要：

本文档主要介绍openGauss在兼容B库且有dolphin插件的情形下，支持存储过程中含有查询语句的调用并返回结果集，当有多个查询语句在一个过程中时，也会按照执行的顺序返回多个结果集。

# 1     特性概述

openGauss在兼容B库情形下，安装dolphin插件，支持存储过程中含有查询语句的调用并返回结果集，当有多个查询语句在一个过程中时，也会按照执行的顺序返回多个结果集。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| dolphin 2.0 | 2023-09-20 | 2023-09-22 |
| mysql 5.7                      | 2023-09-20   | 2023-09-22   |
| openGauss 5.1.0 build 24d9203f | 2023-09-20 | 2023-09-22 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 虚拟机 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.00GHz 20核 OS：CentOS Linux 7 (Core) |  |


# 3     测试结论概述

## 3.1   测试整体结论
openGauss兼容mysql，可以使用call关键字调用存储过程和只包含select语句的函数，测试用例35条，主要覆盖了功能测试。功能测试覆盖：存储过程中进行各种表类型的查询和dml操作，存储过程中对表同义词、视图、物化视图、视图同义词查询，存储过程中使用聚合函数、窗口函数、类型转换函数、日期处理函数、字符处理函数，存储过程中label与loop、while、repeat结合查询，自定义用户测试，在join、union、子查询、group by、connect by中使用@变量，存储过程中的多结果集中有部分报错，返回结果0条和多条结合，不支持多结果集，嵌套调用，校验输出结果。累计发现问题单1个，特性质量良好。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 存储过程中进行不同表类型（行存表、列存表、range分区表、list、interval、hash、二级分区表、unlogged表、临时表、压缩表）的查询，执行结果符合预期，通过 |
| 功能测试 | 存储过程中对表同义词、视图、物化视图、增量物化视图、视图同义词查询，执行结果符合预期，通过 |
| 功能测试 | 存储过程中使用聚合函数、窗口函数、类型转换函数、日期处理函数、字符处理函数，执行结果符合预期，通过 |
| 功能测试 | 自定义用户测试，执行结果符合预期，通过 |
| 功能测试 | 存储过程中label与loop、while、repeat结合查询，触发器函数，在join、union、子查询、group by、connect by中使用@变量，执行结果符合预期，通过 |
| 功能测试 | 存储过程中的多结果集中有部分报错，返回结果0条和多条结合，设置参数不支持多结果集，执行结果符合预期，通过 |
| 功能测试 | 存储过程中dml后查询，嵌套使用，analyze、vacuum、explain、exexute immediate，执行结果符合预期，通过 |

## 3.2   约束说明
1.openGauss需使用兼容B库且有dolphin插件
2.mysql使用5.7版本

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| N/A |  |  |  |  |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 1 | 0 | 1 | 0 | 0 |
| 百分比 | 100% | 0% | 100% | 0% | 0% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I83G0F](https://gitee.com/opengauss/openGauss-server/issues/I83G0F) | 主要 | 存储过程嵌套调用，查询报错 | 已解决 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件 <br />3. 创建兼容B库并安装dolphin插件 <br />4. 执行用例 |编译库及插件成功，共执行35条用例，共发现1个bug。|
#### 4.1.1.1 存储过程中进行不同表类型的查询等测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行10条用例，共发现0个bug。|
#### 4.1.1.2 存储过程中对表同义词、视图、物化视图、增量物化视图、视图同义词查询等测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行5条用例，共发现0个bug。|
#### 4.1.1.3 存储过程中使用聚合函数、窗口函数、类型转换函数、日期处理函数、字符处理函数等测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行5条用例，共发现0个bug|
#### 4.1.1.4 自定义用户测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行3条用例，共发现0个bug|
#### 4.1.1.5 存储过程中label与loop、while、repeat结合查询，触发器函数，在join等中使用@变量等测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行5条用例，共发现0个bug|
#### 4.1.1.6 存储过程中的多结果集中有部分报错，返回结果0条和多条结合，设置参数不支持多结果集
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 | 编译库及插件成功，共执行3条用例，共发现0个bug |

#### 4.1.1.7 存储过程中dml后查询，嵌套使用，analyze、vacuum、explain、exexute immediate

| **测试步骤**                                                | **测试结果**                                  |
| ----------------------------------------------------------- | --------------------------------------------- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 | 编译库及插件成功，共执行4条用例，共发现1个bug |

## 

## 4.2   测试执行统计数据

| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| openGauss 5.1.0 build 24d9203f | 35 | Passed：34 Failed：1 | 1 |

数据说明

1. 累计发现问题单1个
## 4.3   后续测试建议
无
# 5     附件
## 5.1 存储过程中进行不同表类型的查询的示例
create table tab_1143771(id int,pid int,a1 char(8))
partition by list(id)
(
partition p1 values(1),
partition p2 values(2),
partition p3 values(3),
partition p4 values(4));
create table a_1143771(id int,a1 char(8))
partition by list(id)
(
partition p1 values(1),
partition p2 values(2));
create table b_1143771(id int,a1 char(8))
partition by list(id)
(
partition p1 values(2),
partition p2 values(3));
--index;
create index on tab_1143771(id);
create index on tab_1143771 using btree(pid);
--insert;
insert into tab_1143771 values(1,2,'s'),(2,3,'b'),(3,4,'c'),(4,5,'d');
insert into a_1143771 values(1,'s'),(2,'b');
insert into b_1143771 values(2,'s'),(3,'b');
--创建存储过程;
create or replace procedure pro_1143771()
as
begin
select * from a_1143771 union select * from b_1143771 order by id;
with temp_1143771(a1,a2) as (select id,a1 from tab_1143771 where id > 1) select * from temp_1143771;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a,b_1143771 b where a.id>1 and b.id > 1 order by a.id,b.id;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a full join b_1143771 b on a.id> 1 and b.id > 1 order by a.id,b.id;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a inner join b_1143771 b on a.id> 1 and b.id > 1 order by a.id,b.id;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a left join b_1143771 b on a.id> 1 and b.id > 1 order by a.id,b.id;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a right join b_1143771 b on a.id> 1 and b.id > 1 order by a.id,b.id;
select a.id,a.a1,b.id,b.a1 from tab_1143771 a cross join b_1143771 b where a.id> 1 and b.id > 1 order by a.id,b.id;
select * from tab_1143771 start with pid = 4 connect by prior id = pid order by a1;
select * from tab_1143771 where id = 1 connect by pid = id order by a1;
select * from tab_1143771 start with id = 1 connect by nocycle prior pid=id;
select a.id,a.a1,b.id,b.a1 from a_1143771 a,b_1143771 b where a.id+1 =b.id start with a.id=1 connect by b.id=prior a.id and prior b.id=a.id;
select avg(id),a1 from a_1143771 group by a1 having avg(id) > 1;
select id,a1 from a_1143771 group by cube(id,a1) order by id;
select id,a1 from a_1143771 group by grouping sets((id,a1),a1) order by id;
end;
/

call pro_1143771();

