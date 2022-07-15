![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述                       | 作者        |
| :-------- | ----------- | ------------------------------ | ----------- |
| 2022-7-12 | 1.0         | 特性测试报告初稿完成           | zhanghuan96 |
| 2022-7-14 | 1.1         | 修改测试整体结论及遗留问题章节 | zhanghuan96 |

 关键词： 

pg_chameleon、全量迁移、mysql对象

摘要：

本文档主要介绍pg_chameleon迁移工具支持全量迁移mysql数据库对象，包括视图（VIEW）、触发器（TRIGGER）、存储过程(PROCEDURE)、自定义函数(FUNCTION）、列的comment，使用chameleon将这些创建的数据库对象和数据迁移到openGauss中，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

pg_chameleon迁移工具支持全量迁移mysql数据库的视图、触发器、存储过程、自定义函数、列的comment至openGauss侧，保证迁移成功且迁移后的对象在openGauss侧可以正常使用。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| mysql 5.7                      | 2022-5-24    | 2022-6-10    |
| openGauss 3.0.0 build b2c6e04e | 2022-5-24    | 2022-6-10    |
| chameleon-3.0.0                | 2022-5-24    | 2022-6-10    |
| Druid 1.2.8                    | 2022-5-24    | 2022-6-10    |
| dolphin 1.0                    | 2022-5-24    | 2022-6-10    |

| 硬件型号   | 硬件配置信息                                                 | 备注 |
| ---------- | ------------------------------------------------------------ | ---- |
| x86+centOS | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

pg_chameleon支持全量迁移mysql数据库对象至openGauss，共计执行63条用例，主要覆盖了功能测试和资料测试。功能测试覆盖在mysql数据库分别创建视图、触发器、存储过程、自定义函数、创建表时给列添加comment，使用chameleon工具迁移上述对象到openGauss，保证迁移后的对象在openGauss可以正常使用。因mysql和openGauss的语法存在差异，对于openGauss侧不支持的语法以及Druid 无法解析的参数，若迁移失败，日志会有合理提示（语法差异详见[openGauss-tools-sql-translator)](https://gitee.com/opengauss/openGauss-tools-sql-translator)）。资料测试覆盖校验资料的描述及示例的执行结果是否成功。累计发现缺陷单18个，需求单1个，其中2个缺陷单经评审转为需求，其余16个缺陷均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | mysql数据库创建视图，chameleon工具执行chameleon start_view_replica --config default --source mysql --debug迁移至openGauss，迁移成功，查询视图定义及数据成功 |
| 功能测试 | mysql数据库创建触发器，chameleon工具执行chameleon start_trigger_replica --config default --source mysql --debug迁移至openGauss，迁移成功且openGauss侧使用触发器，触发器生效 |
| 功能测试 | mysql数据库创建存储过程，chameleon工具执行chameleon start_proc_replica --config default --source mysql --debug迁移至openGauss，迁移成功且openGauss侧调用存储过程，存储过程生效 |
| 功能测试 | mysql数据库创建自定义函数，chameleon工具执行chameleon start_func_replica --config default --source mysql --debug迁移至openGauss，迁移成功且openGauss侧调用函数，函数生效 |
| 功能测试 | mysql数据库创建表时给列添加comment，迁移至openGauss，表迁移成功，列的comment迁移成功 |
| 功能测试 | 对于openGauss侧不支持的语法或参数，迁移过程若报错，会打印合理且明确的日志信息 |
| 资料测试 | 资料对于新增加的功能描述准确，示例的执行结果正确，整体质量良好 |

## 3.2   约束说明

- mysql使用5.7版本
- openGauss需使用兼容B库
- pg_chameleon中的第三方库Druid目前的版本为1.2.8，本身不支持解析mysql侧某些参数，暂时无法屏蔽，会导致迁移失败。如源库创建存储过程含openGauss侧不支持的参数，迁移存储过程报错；存储过程体含开启事务等操作，迁移报错
- 若想迁移到openGauss的表名和视图名的大小写与MySQL一致，MySQL的系统变量lower_case_table_names的值应设置为0。存在大小写的触发器名、自定义函数名、存储过程名迁移前后一致。当在openGauss访问这些字段时，需要使用双引号才能识别该字段
- 查询sch_chameleon.t_replica_object表（注意en_object_type的值是为大写）即可获取。
-  mysql与openGauss的兼容性语法说明详见[openGauss-tools-sql-translator](https://gitee.com/opengauss/openGauss-tools-sql-translator)

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 19       | 0    | 8    | 11   | 0      |
| 百分比 |          | 0%   | 42%  | 58%  | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I59JS1](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I59JS1?from=project-issue) | 主要     | 源库创建存储过程，过程体中含变量赋值，迁移存储过程至openGauss侧报错 | 已验收   |
| 2    | [I59DJO](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I59DJO?from=project-issue) | 主要     | 变色龙迁移触发器报错                                         | 已验收   |
| 3    | [I598ZA](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I598ZA?from=project-issue) | 主要     | 表的列名为反引号加空格的形式，表可迁移成功，视图迁移报错     | 已验收   |
| 4    | [I59UV3](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I59UV3?from=project-issue) | 次要     | 源库创建自定义函数，返回值为sql语句或表达式，迁移报错        | 已验收   |
| 5    | [I5AXUD](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5AXUD?from=project-issue) | 主要     | 存储过程中存储体为set transaction设置事务隔离级别，迁移报语法错误，多解析一个set | 已验收   |
| 6    | [I5AVCB](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5AVCB?from=project-issue) | 主要     | 存储过程体中，创建用户，迁移至openGauss侧调用报错            | 已验收   |
| 7    | [I5ATEU](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5ATEU?from=project-issue) | 主要     | 存储过程中，存储体为创建数据库，迁移至openGauss侧后，调用存储过程，创建的是schema不是database | 已验收   |
| 8    | [I5B92U](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5B92U?from=project-issue) | 次要     | 存储体有set语句，迁移报错，set autocommit=1在openGauss侧是支持的 | 已验收   |
| 9    | [I5B462](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5B462?from=project-issue) | 次要     | 迁移自定义函数，函数体有if表达式，迁移成功后在openGauss侧调用报错 | 已验收   |
| 10   | [I5ARSR](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5ARSR?from=project-issue) | 次要     | 存储过程中建表，迁移存储过程至openGauss侧后调用，列的注释未创建 | 已验收   |
| 11   | [I5AKMG](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5AKMG?from=project-issue) | 次要     | 迁移存储过程，存储过程体含loop循环以及while循环、repeat，迁移报错 | 已验收   |
| 12   | [I5A31I](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5A31I?from=project-issue) | 次要     | 触发器中含2个触发语句，迁移报错                              | 已验收   |
| 13   | [I5EDAR](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5EDAR?from=project-issue) | 次要     | 迁移自定义函数，函数体有return，迁移报错                     | 已验收   |
| 14   | [I5BGTD](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5BGTD?from=project-issue) | 次要     | 存储过程中，建表中指定索引信息，openGauss侧调用存储过程后，索引信息未创建 | 已验收   |
| 15   | [I5AAZC](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5AAZC?from=project-issue) | 次要     | 触发器迁移成功，openGauss使用触发器语句，报错                | 已验收   |
| 16   | [I5B4PA](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5B4PA?from=project-issue) | 次要     | 在线迁移中，列的comment未迁移                                | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 迁移视图

| 测试步骤：               | 测试结果                                     |
| ------------------------ | -------------------------------------------- |
| 1. mysql侧创建视图并迁移 | 执行15条用例，发现1个bug，现已修复且验收通过 |

### 4.1.2 迁移触发器

| 测试步骤：                 | 测试结果                                    |
| -------------------------- | ------------------------------------------- |
| 1. mysql侧创建触发器并迁移 | 执行5条用例，发现3个bug，现已修复且验收通过 |

### 4.1.3 迁移存储过程

| 测试步骤：                   | 测试结果                                                     |
| ---------------------------- | ------------------------------------------------------------ |
| 1. mysql侧创建存储过程并迁移 | 执行30条用例，发现10个bug，2个转为需求，其余8个现已修复且验收通过 |

### 4.1.4 迁移自定义函数

| 测试步骤：                     | 测试结果                                     |
| ------------------------------ | -------------------------------------------- |
| 1. mysql侧创建自定义函数并迁移 | 执行10条用例，发现3个bug，现已修复且验收通过 |

### 4.1.5 迁移列的comment

| 测试步骤：                     | 测试结果                                    |
| ------------------------------ | ------------------------------------------- |
| 1.mysql侧建表并指定列的comment | 执行2条用例，发现1个bug，现已修复且验收通过 |

### 4.1.2 全量迁移成功后，测试在线迁移

| 测试步骤：                                                   | 测试结果              |
| ------------------------------------------------------------ | --------------------- |
| 1. 开启在线迁移<br>2.mysql侧创建表<br>3.openGauss侧查询<br>4.mysql侧执行DML操作 | 执行1条用例，测试通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build b2c6e04e | 63         | Passed：63<br>Failed：0 | 19           |

*数据项说明：*

* 累计发现缺陷单18个，需求单1个，其中2个缺陷经评审转为需求，其余16个缺陷均已解决且回归通过
* 失败用例已在后续问题修复后，回归issue执行通过

## 4.3   后续测试建议

-  全量迁移对象成功后，需测试chameleon工具的在线迁移功能是否正常
-  mysql侧创建存储过程或者自定义函数时，可参考[openGauss-tools-sql-translator](https://gitee.com/opengauss/openGauss-tools-sql-translator)语法差异，测试目前openGauss兼容mysql的语法情况
-  迁移无论成功或失败，可以观察迁移日志及sch_chameleon.t_replica_object 对象迁移信息表查看具体信息

# 5     附件

##  5.1 迁移mysql侧的自定义函数至openGauss执行过程示例

```sql
--mysql侧创建自定义函数
mysql> DELIMITER //
mysql> create function func_object0043(a int) returns varchar(20)
    -> begin
    ->     if a > 0 and a<20 then
    ->         return "大于10小于20";
    ->     elseif a<10 or a>20 then
    ->         return "小于10或者大于20";
    ->     end if;
    -> END//
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
--迁移至openGauss
chameleon start_func_replica --config default --source mysql --debug
--openGauss侧查询
\df
     Schema       |      Name       | Result data type  | Argument data types |  Type  | fencedmode | propackage | prokind
--------------------+-----------------+-------------------+---------------------+--------+------------+------------+---------
 loxodonta_africana | func_object0043 | character varying | a integer           | normal | f          | f          | f
(1 row)

--openGauss侧调用
tpccdb=> call func_object0043(10);
 func_object0043
-----------------
 大于10小于20
(1 row)
--openGauss侧删除自定义函数
drop function func_object0043;
```

 



 

 
