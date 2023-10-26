版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| --- | --- | --- | --- |
| 2023-9-25 | 1.0 | 测试报告初稿完成 | danny20001210 |


关键词：

dolphin插件，DIAGNOSTICS，兼容B库

摘要：

本文档主要介绍openGauss在兼容B库且有dolphin插件的情形下，支持diagnostic用法。

# 1     特性概述

openGauss在兼容B库情形下，安装dolphin插件，支持diagnostic用法。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间 | 测试结束时间 |
| --- | --- | --- |
| dolphin 2.0 | 2023-09-20 | 2023-09-22 |
| mysql 5.7                   | 2023-09-20   | 2023-09-22   |
| openGauss 5.1.0 build 24d9203f | 2023-09-20 | 2023-09-22 |


描述特性测试的环境信息

| 环境信息 | 配置信息 | 备注 |
| --- | --- | --- |
| 虚拟机 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.00GHz 20核 OS：CentOS Linux 7 (Core) |  |


# 3     测试结论概述

## 3.1   测试整体结论
openGauss兼容mysql，支持diagnostic用法，测试用例20条，主要覆盖了功能测试。功能测试覆盖：直接调用测试，存储过程测试，自定义函数测试，导入导出测试，其他兼容模式下测试，自定义用户测试，大小写敏感场景，校验输出结果。累计发现问题单1个，特性质量良好。

| **测试活动** | **活动评价** |
| --- | --- |
| 功能测试 | 直接调用测试，双引号、NULL、不同数据类型等，执行结果符合预期，通过 |
| 功能测试 | 存储过程测试，约束、冲突、游标异常处理等，执行结果符合预期，通过 |
| 功能测试 | 触发器测试，执行结果符合预期，通过 |
| 功能测试 | 自定义函数测试，执行结果符合预期，通过 |
| 功能测试 | 其他兼容模式，执行结果符合预期，通过 |
| 功能测试 | 导入导出测试，执行结果符合预期，通过 |
| 功能测试 | 大小写敏感场景，执行结果符合预期，通过 |

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
| 数目 | 1 | 0 | 0 | 1 | 0 |
| 百分比 | 100% | 0% | 0% | 100% | 0% |


### 3.3.3  问题单汇总
| **序号** | **issue号** | **问题级别** | **问题简述** | **问题状态** |
| --- | --- | --- | --- | --- |
| 1 | [I83BW9](https://gitee.com/opengauss/openGauss-server/issues/I83BW9) | 主要 | 违反非空约束（@variable），调用结果不符合预期 | 已解决 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件 <br />3. 创建兼容B库并安装dolphin插件 <br />4. 执行用例 |编译库及插件成功，共执行20条用例，共发现1个bug。|
#### 4.1.1.1 直接调用测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行3条用例，共发现0个bug。|
#### 4.1.1.2 存储过程测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行8条用例，共发现1个bug。|
#### 4.1.1.3 触发器测试

| **测试步骤**                                     | **测试结果**                                    |
| ------------------------------------------------ | ----------------------------------------------- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 | 编译库及插件成功，共执行1条用例，共发现0个bug。 |

#### 4.1.1.4 自定义函数测试

| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行2条用例，共发现0个bug|
#### 4.1.1.5 自定义用户测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建兼容B库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行1条用例，共发现0个bug|
#### 4.1.1.6 其他兼容模式测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 |编译库及插件成功，共执行1条用例，共发现0个bug|
#### 4.1.1.7 导入导出测试
| **测试步骤** | **测试结果** |
| --- | --- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 | 编译库及插件成功，共执行1条用例，共发现0个bug |

#### 4.1.1.8 大小写敏感测试

| **测试步骤**                                                | **测试结果**                                  |
| ----------------------------------------------------------- | --------------------------------------------- |
| 1. 创建其他兼容模式数据库并安装dolphin插件<br />2. 执行用例 | 编译库及插件成功，共执行3条用例，共发现0个bug |

## 

## 4.2   测试执行统计数据

| **版本名称** | **测试用例数** | **用例执行结果** | **发现问题单数** |
| --- | --- | --- | --- |
| openGauss 5.1.0 build 24d9203f | 20 | Passed：19 Failed：1 | 1 |

数据说明

1. 累计发现问题单1个
## 4.3   后续测试建议
无
# 5     附件
## 5.1 存储过程测试
CREATE DATABASE db_mysql DBCOMPATIBILITY 'B';

\c db_mysql;

SET enable_set_variable_b_format=on;

DROP PROCEDURE proc_1145167;

drop table if exists t_diagnostics_1145167;
create table t_diagnostics_1145167(id int primary key,name varchar(40),object varchar(40),score int);
insert into t_diagnostics_1145167 values(1,'lili','math',96);
insert into t_diagnostics_1145167 values(2,'kiko','math',90);
insert into t_diagnostics_1145167 values(3,'amy','math',87);
insert into t_diagnostics_1145167 values(4,'bob','math',79);

set @num='',@rowcount='';

set @class_origin='',@subclass_origin='',@returned_sqlstate='',@message_text= '',@mysql_errno='',@constraint_catalog='',@constraint_schema='',@constraint_name='',@catalog_name='',@schema_name='',@table_name='',@column_name='',@cursor_name='';
create or replace procedure proc_1145167() as
declare id1 int;
cursor cur_id is select id from t_diagnostics_1145167 ;
BEGIN
declare continue handler for sqlstate'23502'
begin

GET DIAGNOSTICS @num=NUMBER,@rowcount=ROW_COUNT;

RAISE NOTICE 'NUMBER = %,ROW_COUNT = %',@num,@rowcount; 
GET STACKED DIAGNOSTICS CONDITION 1 @class_origin=CLASS_ORIGIN,@subclass_origin=SUBCLASS_ORIGIN,@returned_sqlstate=RETURNED_SQLSTATE,@message_text= MESSAGE_TEXT,@mysql_errno=MYSQL_ERRNO,@constraint_catalog=CONSTRAINT_CATALOG,@constraint_schema=CONSTRAINT_SCHEMA,@constraint_name=CONSTRAINT_NAME,@catalog_name=CATALOG_NAME,@schema_name=SCHEMA_NAME,@table_name=TABLE_NAME,@column_name=COLUMN_NAME,@cursor_name=CURSOR_NAME;
RAISE NOTICE 'CLASS_ORIGIN = %,SUBCLASS_ORIGIN = %,RETURNED_SQLSTATE = %,MESSAGE_TEXT = %,MYSQL_ERRNO = %,CONSTRAINT_CATALOG = %,CONSTRAINT_SCHEMA = %,CONSTRAINT_NAME = %,CATALOG_NAME = %,SCHEMA_NAME = %,TABLE_NAME = %,COLUMN_NAME = %,CURSOR_NAME= %',@class_origin,@subclass_origin,@returned_sqlstate,@message_text,@mysql_errno,@constraint_catalog,@constraint_schema,@constraint_name,@catalog_name,@schema_name,@table_name,@column_name,@cursor_name;
END;
open cur_id;
loop
fetch cur_id into id1;
if id1<=3 then
update t_diagnostics_1145167 set score=score-5 where id=id1;
iterate;
else
update t_diagnostics_1145167 set id=null where id=id1;
LEAVE;
END IF;
END LOOP;
close cur_id;
raise info 'close';
END;
/
call proc_1145167();
select * from t_diagnostics_1145167 order by id;

