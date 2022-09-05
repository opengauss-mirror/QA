![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者       |
| ---------- | -------- | -------------------- | ---------- |
| 2022-09-01 | 1.0      | 特性测试报告初稿完成 | lizhuo9527 |
| 2022-09-02 | 1.1      | 修改特性测试报告     | lizhuo9527 |
| 2022-09-03 | 1.2      | 修改特性测试报告     | lizhuo9527 |

关键词： 

kafka；onlineMigration；debezium；在线迁移；DDL语句；create/drop primary key；create/drop index

摘要：

在线迁移工具由Oracle（生产端），debezium，kafka， onlineMigration，openGauss（消费端）这几部分组成，本文档针对在线迁移工具支持DDL语句的create/drop primary key及create/drop index语法进行测试，并给出特性测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容A库情形下，依次开启kafka、kafka_connector、onlineMigration工具，由debezium监控Oracle中DDL及DML语句的变化并把记录捕捉到kafak的topic中，然后通过onlineMigration消费topic中的记录并迁移到openGauss数据库中，这样就完成从Oracle到openGauss的在线迁移。

本次特性支持在线迁移create primary key、drop primary key、create index、drop index语法。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 02c14696 | 2022-08-24   | 2022-09-01   |
| Oracle version 19.3.0.0.0      | 2022-08-24   | 2022-09-01   |
| kafka v2.13-2.8.1              | 2022-08-24   | 2022-09-01   |
| debezium v1.8.1                | 2022-08-24   | 2022-09-01   |
| onlineMigration v1.0           | 2022-08-24   | 2022-09-01   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Platinum 8378A CPU @ 3.00GHz<br>内存：32GB<br>硬盘：1000G<br>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

Oracle在线迁移支持DDL语句create/drop primary key及create/drop index语法共计执行52条用例，主要覆盖了功能测试及资料测试。功能测试主要覆盖以下9个方面：

1. create [unique] index结合排序参数asc/desc，指定列表达式expression(Function-based index)的场景

2. create table语句中，column及constraint结合create index的场景

3. alter table语句中，column及constraint结合create index及using index的场景

4. drop index的场景

5. create/drop primary key（指定、不指定constraint_name）的场景

6. 特殊场景测试

   - add column多列结合create index
   - 字段上存在索引时drop column cascade constraints
   - openGauss端存在同名表时create/drop primary key及create/drop index

7. 前序已支持的功能测试

   - create/alter foreign key验证
   - rename/truncate/drop table验证
   - DML语句(insert/update/delete)验证
   - DDL及DML迁移顺序验证

8. onlineMigration工具三个参数的验证

   - --frombegining断点续传功能

   - --smartConversionOfObjectNames大小写转换开关功能

   - --indexPrefix索引添加前缀功能

9. Oracle多session执行多表DDL及DML场景

资料测试主要验证资料的描述的准确性以及示例的正确性。

测试中共计发现问题单4个，所有问题单均已处理完成且回归通过。无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | create [unique] index结合排序参数asc/desc，指定列表达式expression<br>(Function-based index)的场景验证，通过 |
| 功能测试 | create table语句中，column及constraint结合create index的场景验证，通过 |
| 功能测试 | alter table语句中，column及constraint结合create index及using index的场景验证，通过 |
| 功能测试 | drop index的场景验证，通过                                   |
| 功能测试 | create/drop primary key（指定、不指定constraint_name）的场景验证，通过 |
| 功能测试 | 特殊场景验证，通过                                           |
| 功能测试 | 前序已支持的功能验证，通过                                   |
| 功能测试 | onlineMigration工具三个参数的验证，通过                      |
| 功能测试 | Oracle多session执行多表DDL及DML场景验证，通过                |
| 资料测试 | 资料描述准确，示例执行无误，整体质量良好                     |

## 3.2   约束说明

1. openGauss数据库为兼容A库；
2. Oracle数据库版本为19.3.0.0.0；
3. Oracle及openGauss两端数据库需有同名schema，如“C##ROMA_LOGMINER”；
4. 开启zookeeper、kafka、kafka_connector、onlineMigration工具并保证正常运行；
5. 最后一次执行的DDL语句会置于onlineMigration的缓存中，需要DML语句驱动其迁移；
6. Oracle中primary key及unique约束可以关联普通索引(B-Tree index)，但openGauss中primary key及unique约束必须关联唯一索引(unique index)；
7. Oracle的Bitmap index、Domain index类型索引openGauss不支持，因此迁移至openGauss端转换为普通B_Tree索引；
8. 在线迁移工具暂不支持分区表迁移，因此基于分区表的索引Partitioned index不支持在线迁移；
9. 关闭迁移工具后需更改kafka_connector连接名或清空/tmp下kafka日志

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3  问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                  | 状态   |
| ---- | ------------------------------------------------------------ | ---- | --------------------------------------------------------- | ------ |
| 1    | [I5O2CM](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5O2CM?from=project-issue) | 次要 | 在线迁移工具不支持create table中嵌套create index语句的sql | 已验收 |
| 2    | [I5ORQ3](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5ORQ3?from=project-issue) | 次要 | 在线迁移工具创建index列名无法识别                         | 已验收 |
| 3    | [I5OT4S](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OT4S?from=project-issue) | 次要 | 在线迁移工具drop主键迁移失败                              | 已验收 |
| 4    | [I5OW4L](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OW4L?from=project-issue) | 次要 | 在线迁移工具单表多字段unique约束迁移online报错            | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. 开启两端数据库并设置同名schema<br />2. 开启kafka、onlineMigration等工具 | 共执行52条用例，<br />共发现4个bug，4条现已修复且验收通过 |

#### 4.1.1.1 create index测试

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. create index普通索引<br />2. create unique index唯一索引<br />3. 索引字段添加排序参数asc/desc<br />4. 不同类型索引(Bitmap index、Function-based index) | 共执行14条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.2 create table结合create index测试

| 测试步骤                                                     | 测试结果                                                 |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| 1. create table t_name col_name datatype<br> constraint con_name using index<br>(create unique index ...);<br>2. create table t_name col_name datatype,<br/> constraint con_name unique(col_name)<br/>using index( create unique index ...); | 共执行7条用例，<br />共发现2个bug，2条现已修复且验收通过 |

#### 4.1.1.3 alter table结合create index及using index测试

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1. alter table t_name add col_name datatype<br/> constraint con_name using index<br/>(create unique index ...);<br/>2. alter table t_name add col_name datatype<br/> constraint con_name using index idx_name;<br/>3. alter table t_name add constraint con_name(col_name) <br/>using index(create unique index ...);<br/>4. alter table t_name add constraint con_name(col_name) <br/>using index idx_name; | 执行13条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.4 drop index测试

| 测试步骤      | 测试结果                                   |
| ------------- | ------------------------------------------ |
| 1. drop index | 执行1条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.5 create/drop primary key测试

| 测试步骤                                             | 测试结果                                                 |
| ---------------------------------------------------- | -------------------------------------------------------- |
| 1. 指定constraint_name<br />2. 不指定constraint_name | 共执行1条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.6 特殊场景测试

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. add column多列结合create index<br>2. 字段上存在索引时drop column cascade constraints<br>3. openGauss端存在同名表时create/drop primary key及create/drop index | 共执行3条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.7 前序已支持的功能测试

| 测试步骤                                                     | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 1. create/alter foreign key<br>2. rename/truncate/drop table<br>3. DML语句(insert/update/delete)<br>4. DDL及DML迁移顺序验证 | 执行9条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.8 onlineMigration工具三个参数测试

| 测试步骤                                                     | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 1. --frombegining断点续传<br>2. --smartConversionOfObjectNames大小写转换开关<br>3. --indexPrefix索引添加前缀 | 执行3条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.9 Oracle多session执行多表DDL及DML场景测试

| 测试步骤                                    | 测试结果                                   |
| ------------------------------------------- | ------------------------------------------ |
| 1. Oracle多开session<br>2. 三表DDL及DML迁移 | 执行1条用例，<br />未发现bug，结果符合预期 |

## 4.2   测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.0.0 build 02c14696<br>onlineMigration v1.0 dbbcb9a | 52         | Passed：47<br />Failed：5 | 4            |
| openGauss 3.0.0 build 02c14696<br>onlineMigration v1.0 fb0500b | 52         | Passed：52<br />Failed：0 | 0            |

数据项说明

1. 共发现4个问题，4条已修改并验证通过
2. 失败用例已在后续问题修复后，回归issue执行通过
3. 缺陷密度：4个(缺陷个数)/3.75kloc(代码行数)=1.1(个/kloc)

## 4.3   后续测试建议

1. 关注onlineMigration工具控制大小写开关的参数`--smartConversionOfObjectNames`，验证添加该参数后的执行结果

# 5     附件

### 5.1 Oracle-openGauss在线迁移DDL语句执行过程示例

```sql
--step1:Oracle创建表 expect:迁移成功
create table c##roma_logminer.t_index_0001(
	col1 int,
	col2 varchar(50),
	col3 int,
	col4 varchar(50),
	col5 number
);
Table created.

--step2:创建单列索引 expect:需DML驱动
create index c##roma_logminer.i_index_0001_01 on c##roma_logminer.t_index_0001(col1);
Index created.

--step3:DML驱动 expect:迁移成功
insert into c##roma_logminer.t_index_0001 values(1,'aa',2,'bb',3);
1 row created.
commit;
Commit complete.

--step4:openGauss端查看索引 expect:索引存在
\di
                              List of relations
      Schema      |      Name       | Type  | Owner |    Table     | Storage
------------------+-----------------+-------+-------+--------------+---------
 C##ROMA_LOGMINER | I_INDEX_0001_01 | index | lz    | T_INDEX_0001 |
(1 row)

--step5:创建多列索引 expect:需DML驱动
create index c##roma_logminer.i_index_0001_02 on c##roma_logminer.t_index_0001(col3,col4,col5);
Index created.

--step6:DML驱动 expect:迁移成功
insert into c##roma_logminer.t_index_0001 values(4,'cc',5,'dd',6);
1 row created.
commit;
Commit complete.

--step7:openGauss端查看索引 expect:索引存在
\di
                              List of relations
      Schema      |      Name       | Type  | Owner |    Table     | Storage
------------------+-----------------+-------+-------+--------------+---------
 C##ROMA_LOGMINER | I_INDEX_0001_01 | index | lz    | T_INDEX_0001 |
 C##ROMA_LOGMINER | I_INDEX_0001_02 | index | lz    | T_INDEX_0001 |
(2 rows)

--step8:drop索引 expect:需DML驱动
drop index c##roma_logminer.i_index_0001_02;
Index dropped.
drop index c##roma_logminer.i_index_0001_01;
Index dropped.

--step9:DML驱动 expect:迁移成功
insert into c##roma_logminer.t_index_0001 values(7,'cc',8,'dd',9);
1 row created.
commit;
Commit complete.

--step10:openGauss端查看索引 expect:索引不存在
\di
No relations found.

--step11:drop table expect:迁移成功
drop table c##roma_logminer.t_index_0001 purge;
Table dropped.
```

