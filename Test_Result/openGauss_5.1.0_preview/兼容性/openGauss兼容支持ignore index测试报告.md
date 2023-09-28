修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2023-09-26 | 1.0 | 测试报告初稿完成 | cloudsbreak |


关键词：

dolphin插件，sql security语法，兼容B库

摘要：

本文档主要介绍openGauss在兼容B库且有dolphin插件的情形下，实现支持ignore index语法。

# 1     特性概述

openGauss在兼容B库情形下，安装dolphin插件，可以支持ignore index语法。

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
openGauss兼容mysql，支持ignore index语法，测试用例30条，主要聚焦于功能测试。功能测试覆盖：各种索引类型中支持ignore index语法，DML操作中支持ignore index语法，各种分区表中支持ignore index语法，校验输出结果。累计发现问题单0个，特性质量良好。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 各种索引类型中支持ignore index语法，执行结果符合预期，通过 |
| 功能测试 | DML操作中支持ignore index语法，执行结果符合预期，通过 |
| 功能测试 | 各种分区表中支持ignore index语法，执行结果符合预期，通过 |

## 3.2   约束说明
1.openGauss需使用兼容B库且有dolphin插件
2.mysql使用8.0版本

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施
| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| --- | --- | --- | --- | --- |
| N/A |  |  |  |  |


### 3.3.2  问题统计
|  | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| --- | --- | --- | --- | --- | --- |
| 数目 | 0 | 0 | 0 | 0 | 0 |
| 百分比 | 0% | 0% | 0% | 0% | 0% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| N/A | / | / | / | / |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 编译安装openGauss数据库<br />2.编译dolphin插件<br />3.创建兼容B库并安装dolphin插件<br />4.执行用例 |编译库及插件成功，共执行30条用例，共发现0个bug。|
#### 4.1.1.1 各种索引类型中支持ignore index语法
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行5条用例，共发现0个bug。|
#### 4.1.1.2 DML操作中支持ignore index语法
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行20条用例，共发现0个bug。|
#### 4.1.1.3 各种分区表中支持ignore index语法
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行5条用例，共发现0个bug。|
## 4.2   测试执行统计数据
| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| openGauss 5.1.0 build 24d9203f | 30 | Passed：30 Failed：0 | 0 |

数据说明

1. 累计发现问题单0个
## 4.3   后续测试建议
无
# 5     附件
## 5.1 **支持IGNORE INDEX**语法的示例
--**ignore index 使用多个索引**

CREATE DATABASE db_1130452 DBCOMPATIBILITY 'B';
\c db_1130452; 
create table db_1130452_tb (col1 int ,col2 int,col3 int,col4 varchar(10));
insert into db_1130452_tb values(1,1,1,'a');
insert into db_1130452_tb values(1,2,2,'a');
insert into db_1130452_tb values(2,2,2,'a');
insert into db_1130452_tb values(2,2,3,'b');
insert into db_1130452_tb values(2,3,3,'b');
insert into db_1130452_tb values(3,3,4,'b');
insert into db_1130452_tb values(3,3,4,'a');
insert into db_1130452_tb values(3,4,5,'c');
insert into db_1130452_tb values(4,4,5,'c');
insert into db_1130452_tb values(4,null,1,'c');
create index index_1130452_1 on db_1130452_tb (col1);
create index index_1130452_2 on db_1130452_tb (col2);
create index index_1130452_3 on db_1130452_tb (col3);
create index index_1130452_4 on db_1130452_tb (col4);
analyze db_1130452_tb;
select * from db_1130452_tb ignore index (index_1130452_1) ignore index (index_1130452_2) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_2) ignore index (index_1130452_1) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_2) ignore index (index_1130452_2) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_1) ignore index (index_1130452_2)ignore index (index_1130452_3) ignore index (index_1130452_4) where col2= 3 and col1 = 2 and col3 = 3 and col4='b' order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_1, index_1130452_2) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_2, index_1130452_1) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_2, index_1130452_2) where col2= 3 order by 1,2,3;
select * from db_1130452_tb ignore index (index_1130452_1, index_1130452_2, index_1130452_3, index_1130452_4) where col2= 3 and col1 = 2 and col3 = 3 and col4='b' order by 1,2,3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_1) ignore index (index_1130452_2) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_2) ignore index (index_1130452_1) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_2) ignore index (index_1130452_2) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_1) ignore index (index_1130452_2)ignore index (index_1130452_3) ignore index (index_1130452_4) where col2= 3 and col1 = 2 and col3 = 3 and col4='b';
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_1 ,index_1130452_2) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_2 ,index_1130452_1) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_2, index_1130452_2) where col2= 3;
explain (costs off )select * from db_1130452_tb ignore index (index_1130452_1, index_1130452_2, index_1130452_3, index_1130452_4) where col2= 3 and col1 = 2 and col3 = 3 and col4='b';

--**insert中使用ignore index**

CREATE DATABASE db_1130459 DBCOMPATIBILITY 'B';
\c db_1130459;
create table db_1130459_tb (col1 int ,col2 int,col3 int,col4 varchar(10));
insert into db_1130459_tb values(1,1,1,'a');
insert into db_1130459_tb values(1,2,2,'a');
insert into db_1130459_tb values(2,2,2,'a');
insert into db_1130459_tb values(2,2,3,'b');
insert into db_1130459_tb values(2,3,3,'b');
insert into db_1130459_tb values(3,3,4,'b');
insert into db_1130459_tb values(3,3,4,'a');
insert into db_1130459_tb values(3,4,5,'c');
insert into db_1130459_tb values(4,4,5,'c');
insert into db_1130459_tb values(4,null,1,'c');
create index index_1130459_1 on db_1130459_tb (col1);
create index index_1130459_2 on db_1130459_tb (col2);
create index index_1130459_3 on db_1130459_tb (col3);
create index index_1130459_4 on db_1130459_tb (col4);
analyze db_1130459_tb;
create table db_1130459_tb_1 (col1 int ,col2 int,col3 int,col4 varchar(10));
create index index_1130459_1_1 on db_1130459_tb_1 (col1);
create index index_1130459_1_2 on db_1130459_tb_1 (col2);
create index index_1130459_1_3 on db_1130459_tb_1 (col3);
create index index_1130459_1_4 on db_1130459_tb_1 (col4);
analyze db_1130459_tb_1;
insert into db_1130459_tb_1 select max(col2),col2 from db_1130459_tb ignore index (index_1130459_1) where col2= 3 and col1 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;
insert into db_1130459_tb_1(col2,col3) select max(t1.col3),t2.col2 from db_1130459_tb as t1 ignore index (index_1130459_1) inner join db_1130459_tb_1 as t2 ignore index (index_1130459_1_2) on t1.col1= t2.col1 where t2.col2= 3 and t1.col1 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;
insert into db_1130459_tb_1(col4,col2) select max(t1.col4),t2.col1 from db_1130459_tb as t1 ignore index (index_1130459_1) left join db_1130459_tb_1 as t2 ignore index (index_1130459_1_2) on t1.col1= t2.col1 where t2.col2= 3 and t1.col1 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;
insert into db_1130459_tb_1 select max(t1.col1),t2.col2 from db_1130459_tb as t1 ignore index (index_1130459_1) right join db_1130459_tb_1 as t2 ignore index (index_1130459_1_2) on t1.col1= t2.col1 where t2.col2= 3 and t1.col1 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;
insert into db_1130459_tb_1 select max(t1.col2),t2.col1 from db_1130459_tb as t1 ignore index (index_1130459_1) full join db_1130459_tb_1 as t2 ignore index (index_1130459_1_2) on t1.col1= t2.col1 where t2.col2= 3 and t1.col1 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;
insert into db_1130459_tb_1 select max(t1.col2),t2.col2 from db_1130459_tb as t1 ignore index (index_1130459_1) ,db_1130459_tb_1 as t2 ignore index (index_1130459_1_2),db_1130459_tb_1 as t3 ignore index (index_1130459_1_3) where t1.col1= t2.col1 and t2.col2= 3 and t1.col1 = 3 and t3.col3 = 3 group by 2 order by 1,2;
select * from db_1130459_tb_1 order by 1,2,3,4;

--**hash分区表中使用ignore index**

CREATE DATABASE db_1130474 DBCOMPATIBILITY 'B';
\c db_1130474;
CREATE TABLE db_1130474_tb
(
col1 INTEGER NOT NULL,
col2 INTEGER NOT NULL,
col3 INTEGER ,
CA_STREET_NAME VARCHAR(60) ,
CA_STREET_TYPE CHAR(15) ,
CA_SUITE_NUMBER CHAR(10) ,
CA_CITY VARCHAR(60) ,
CA_COUNTY VARCHAR(30) ,
CA_STATE CHAR(2) ,
CA_ZIP CHAR(10) ,
CA_COUNTRY VARCHAR(20) ,
CA_GMT_OFFSET DECIMAL(5,2) ,
CA_LOCATION_TYPE CHAR(20)
)
PARTITION BY hash(col1)
(
partition p1,
partition p2,
partition p3);
CREATE INDEX ds_db_1130474_tb_index2 ON db_1130474_tb(col1) LOCAL
(
PARTITION CA_ADDRESS_SK_index1,
PARTITION CA_ADDRESS_SK_index2 ,
PARTITION CA_ADDRESS_SK_index3
)
;
insert into db_1130474_tb select v,v,v from generate_series(1,1200) as v;
create index "Index_1130474%%_1" on db_1130474_tb (col1) local;
create index INDEX_1130474_2 on db_1130474_tb (col2);
analyze db_1130474_tb;
select max(col2)+1 from db_1130474_tb ignore index ("Index_1130474%%_1") where col1>= 3 ;
select max(col2)+1 from db_1130474_tb ignore index ("Index_1130474%%_1") ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb ignore index ("Index_1130474%%_1") where col1>= 3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb ignore index ("Index_1130474%%_1") ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;
select max(col2)+1 from db_1130474_tb partition (p1) ignore index ("Index_1130474%%_1") where col1>= 3 ;
select max(col2)+1 from db_1130474_tb partition (p1) ignore index ("Index_1130474%%_1") ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb partition (p1) ignore index ("Index_1130474%%_1") where col1>= 3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb partition (p1) ignore index ("Index_1130474%%_1") ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;
select max(col2)+1 from db_1130474_tb partition (p1) ignore index (ds_db_1130474_tb_index2) where col1>= 3 ;
select max(col2)+1 from db_1130474_tb partition (p1) ignore index (ds_db_1130474_tb_index2) ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb partition (p1) ignore index (ds_db_1130474_tb_index2) where col1>= 3 ;
explain (costs off) select max(col2)+1 from db_1130474_tb partition (p1) ignore index (ds_db_1130474_tb_index2) ignore index (INDEX_1130474_2) where col2>= 3 and col1 >=3 ;

