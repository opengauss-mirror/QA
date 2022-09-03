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

kafka；onlineMigration；debezium；在线迁移；DDL语句；add/drop/modify语法

摘要：

在线迁移工具由Oracle（生产端），debezium，kafka， onlineMigration，openGauss（消费端）这几部分组成，本文档针对在线迁移工具支持DDL语句的alter table add/drop/modify语法进行测试，并给出特性测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容A库情形下，依次开启kafka、kafka_connector、onlineMigration工具，由debezium监控Oracle中DDL及DML语句的变化并把记录捕捉到kafak的topic中，然后通过onlineMigration消费topic中的记录并迁移到openGauss数据库中，这样就完成从Oracle到openGauss的在线迁移。

本次特性支持在线迁移add column/constraint、drop column/constraint以及modify修改列数据类型以及列约束语法。

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

Oracle在线迁移支持DDL语句add/drop/modify语法共计执行33条用例，主要覆盖了功能测试及资料测试。功能测试主要覆盖以下7个方面：

1. alter table add添加单列和多列、添加列同时添加约束（约束分别验证单一约束和复合约束的场景）
2. alter table drop删除单列和多列
3. alter table modify修改列的数据类型、修改列的not null及default约束
4. alter table add/drop/modify添加、删除、修改约束（分别验证单一约束和复合约束的场景）
5. 字段上存在和不存在约束时drop column cascade constraints强制删除
6. openGauss端存在同名表的场景时add/drop/modify修改表结构
7. 多表多列多约束结合modify两个复杂场景的验证

资料测试主要验证资料的描述的准确性以及示例的正确性。

测试中共计发现问题单5个，所有问题单均已处理完成且回归通过。无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | alter table add添加单列和多列、添加列同时添加约束场景验证，通过 |
| 功能测试 | alter table drop删除单列和多列场景验证，通过                 |
| 功能测试 | alter table modify修改列的数据类型、修改列的not null及default约束场景验证，通过 |
| 功能测试 | alter table add/drop/modify添加、删除、修改约束场景验证，通过 |
| 功能测试 | 字段上存在和不存在约束时drop column cascade constraints强制删除场景验证，通过 |
| 功能测试 | openGauss端存在同名表的场景时add/drop/modify修改表结构场景验证，通过 |
| 功能测试 | 多表多列多约束结合modify两个复杂场景的验证，通过             |
| 资料测试 | 资料描述准确，示例执行无误，整体质量良好                     |

## 3.2   约束说明

1. openGauss数据库为兼容A库；
2. Oracle数据库版本为19.3.0.0.0；
3. Oracle及openGauss两端数据库需有同名schema，如“C##ROMA_LOGMINER”；
4. 开启zookeeper、kafka、kafka_connector、onlineMigration工具并保证正常运行；
5. 最后一次执行的DDL语句会置于onlineMigration的缓存中，需要DML语句驱动其迁移；
6. 关闭迁移工具后需更改kafka_connector连接名或清空/tmp下kafka日志

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 0    | 0    | 5    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3  问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                | 状态   |
| ---- | ------------------------------------------------------------ | ---- | --------------------------------------- | ------ |
| 1    | [I5OO0F](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OO0F?from=project-issue) | 次要 | 在线迁移工具强制删除主键列失败          | 已验收 |
| 2    | [I5OHG0](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OHG0?from=project-issue) | 次要 | 在线迁移工具drop column后insert迁移失败 | 已验收 |
| 3    | [I5OIUB](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OIUB?from=project-issue) | 次要 | 在线迁移工具drop constraint失败         | 已验收 |
| 4    | [I5OKRK](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5OKRK?from=project-issue) | 次要 | 在线迁移工具drop check约束失败          | 已验收 |
| 5    | [I5ONKR](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5ONKR?from=project-issue) | 次要 | 在线迁移工具外键约束名大小写识别不一致  | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. 开启两端数据库并设置同名schema<br />2. 开启kafka、onlineMigration等工具 | 共执行33条用例，<br />共发现5个bug，5条现已修复且验收通过 |

#### 4.1.1.1 add添加列测试

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. alter table add单列<br />2. alter table add多列 <br />3. alter table add包含约束 | 共执行13条用例，<br />共发现3个bug，3条现已修复且验收通过 |

#### 4.1.1.2 drop删除列测试

| 测试步骤                                             | 测试结果                                     |
| ---------------------------------------------------- | -------------------------------------------- |
| 1. alter table drop单列<br />2. alter table drop多列 | 共执行2条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.3 modify修改列测试

| 测试步骤                                                     | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 1. alter table modify修改数据类型 <br />2. alter table modify修改列约束（not null/default） | 执行4条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.4 add/drop/modify添加、删除、修改约束测试

| 测试步骤                                                     | 测试结果                                               |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| 1. add/drop/modify primary key<br />2. add/drop/modify foreign key<br />3. add/drop/modify unique<br />4. add/drop/modify check | 执行4条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.5 drop column cascade constraints强制删除字段测试

| 测试步骤                       | 测试结果                                                 |
| ------------------------------ | -------------------------------------------------------- |
| 1. 包含约束<br />2. 不包含约束 | 共执行7条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.6 openGauss端存在同名表的场景测试

| 测试步骤                               | 测试结果                                     |
| -------------------------------------- | -------------------------------------------- |
| 1. 开启迁移工具前手动在openGauss端建表 | 共执行1条用例，<br />未发现bug，结果符合预期 |

#### 4.1.1.7 复杂场景测试

| 测试步骤                                                     | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 1. 多表多约束 <br />2. add/drop/modify语句在一个SQL中混合使用 | 执行2条用例，<br />未发现bug，结果符合预期 |

## 4.2   测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.0.0 build 02c14696<br/>onlineMigration v1.0 dbbcb9a | 33         | Passed：27<br />Failed：6 | 5            |
| openGauss 3.0.0 build 02c14696<br/>onlineMigration v1.0 fb0500b | 33         | Passed：33<br />Failed：0 | 0            |

数据项说明

1. 共发现5个问题，5条已修改并验证通过
2. 失败用例已在后续问题修复后，回归issue执行通过
3. 缺陷密度：5个(缺陷个数)/1.53kloc(代码行数)=3.3(个/kloc)

## 4.3   后续测试建议

1. 关注onlineMigration工具控制大小写开关的参数`--smartConversionOfObjectNames`，验证添加该参数后的执行结果

# 5     附件

### 5.1 Oracle-openGauss在线迁移DDL语句执行过程示例

```sql
--step1:Oracle创建表 expect:迁移成功
create table c##roma_logminer.t_ddl_0014(
	col1 int
);
Table created.

--step2:add column时添加列及约束 expect:online日志打印，但未迁移成功，需DML驱动
alter table c##roma_logminer.t_ddl_0014 add 
	(col2 int constraint pk_1 primary key
	,col3 varchar(50) constraint uq_1 unique
	,col4 number constraint ck_1 check (col4 < 60)
	,col5 int references c##roma_logminer.t_ddl_0014(col2)
	,col6 varchar(100) not null
	,col7 varchar(50) default 'aabbcc'
	,col8 int constraint uq_2 unique
	,col9 int constraint ck_2 check (col9 != 5)
	,col10 number not null);
Table altered.

--step3:DML驱动DDL expect:迁移成功
insert into c##roma_logminer.t_ddl_0014 values(1,2,'aa',10,2,'33','abc',9,4,1);
1 row created.
commit;
Commit complete.

--step4:openGauss侧查看表结构 expect:约束存在
db_lz=# \d+ "T_DDL_0014"
                                      Table "C##ROMA_LOGMINER.T_DDL_0014"
 Column |          Type          |              Modifiers              | Storage  | Stats target | Description
--------+------------------------+-------------------------------------+----------+--------------+-------------
 COL1   | numeric(38,0)          |                                     | main     |              |
 COL2   | numeric(38,0)          | not null                            | main     |              |
 COL3   | character varying(50)  |                                     | extended |              |
 COL4   | numeric(38,0)          |                                     | main     |              |
 COL5   | numeric(38,0)          |                                     | main     |              |
 COL6   | character varying(100) | not null                            | extended |              |
 COL7   | character varying(50)  | default 'aabbcc'::character varying | extended |              |
 COL8   | numeric(38,0)          |                                     | main     |              |
 COL9   | numeric(38,0)          |                                     | main     |              |
 COL10  | numeric(38,0)          | not null                            | main     |              |
Indexes:
    "PK_1" PRIMARY KEY, btree ("COL2") TABLESPACE pg_default
    "UQ_1" UNIQUE CONSTRAINT, btree ("COL3") TABLESPACE pg_default
    "UQ_2" UNIQUE CONSTRAINT, btree ("COL8") TABLESPACE pg_default
Check constraints:
    "CK_1" CHECK ("COL4" < 60::numeric)
    "CK_2" CHECK ("COL9" <> 5::numeric)
Foreign-key constraints:
    "SYS_FK_C__ROMA_LOGMINER_T_DDL_0014_COL5_COL2" FOREIGN KEY ("COL5") REFERENCES "T_DDL_0014"("COL2")
Referenced by:
    TABLE ""T_DDL_0014"" CONSTRAINT "SYS_FK_C__ROMA_LOGMINER_T_DDL_0014_COL5_COL2" FOREIGN KEY ("COL5") REFERENCES "T_DDL_0014"("COL2")
Has OIDs: no
Options: orientation=row, compression=no

--step5:drop column expect:online日志打印，但未迁移成功，需DML驱动
alter table c##roma_logminer.t_ddl_0014 drop 
	(col1,col3,col4,col5,col6,col7,col8,col9,col10);
Table altered.

--step6:DML驱动DDL expect:迁移成功
insert into c##roma_logminer.t_ddl_0014 values(3);
1 row created.
commit;
Commit complete.

--step7:openGauss侧查看表结构 expect:迁移成功
db_lz=# \d+ "T_DDL_0014"
                    Table "C##ROMA_LOGMINER.T_DDL_0014"
 Column |     Type      | Modifiers | Storage | Stats target | Description
--------+---------------+-----------+---------+--------------+-------------
 COL2   | numeric(38,0) | not null  | main    |              |
Indexes:
    "PK_1" PRIMARY KEY, btree ("COL2") TABLESPACE pg_default
Has OIDs: no
Options: orientation=row, compression=no

--step8:drop table expect:迁移成功
drop table c##roma_logminer.t_ddl_0014;
Table dropped.
```

