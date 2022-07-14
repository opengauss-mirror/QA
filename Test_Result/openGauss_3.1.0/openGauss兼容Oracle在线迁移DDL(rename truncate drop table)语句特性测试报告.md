![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述                 | 作者       |
| ---------- | -------- | ------------------------ | ---------- |
| 2022-07-04 | 1.0      | 特性测试报告初稿完成     | lizhuo9527 |
| 2022-07-12 | 1.1      | 根据评审意见修改测试报告 | lizhuo9527 |
|            |          |                          |            |

关键词： 

kafka；onlineMigration工具；在线迁移DDL语句

摘要：

在线迁移工具由Oracle(生产端)，debezium，kafka， onlineMigration-openGauss(消费端)这几部分组成，本次测试针对迁移工具支持DDL语句的rename/truncate/drop table语法，本文档主要介绍Oracle-openGauss在线迁移工具支持DDL语句rename/truncate/drop table语法特性测试结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容A库情形下，依次开启kafka、kafka_connector、onlineMigration工具，由debezium监控Oracle中DDL及DML语句的变化并把记录捕捉到kafak的topic中，然后通过onlineMigration消费topic中的记录并迁移到openGauss数据库中，这样就完成从Oracle到openGauss的在线迁移。

本次特性支持DDL语句的rename/truncate/drop table语法，支持DML及DDL语句迁移顺序的实现（即在DML语句迁移过程中同时执行DDL语句，则会将DDL语句写入缓存，待DML语句执行完毕再执行）。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 02c14696 | 2022-06-20   | 2022-07-04   |
| Oracle version 19.3.0.0.0      | 2022-06-20   | 2022-07-04   |
| kafka v2.13-2.8.1              | 2022-06-20   | 2022-07-04   |
| debezium v1.8.1                | 2022-06-20   | 2022-07-04   |
| onlineMigration v1.0           | 2022-06-20   | 2022-07-04   |

描述特性测试的硬件环境信息

| 硬件型号       | 硬件配置信息                                                 | 备注 |
| -------------- | ------------------------------------------------------------ | ---- |
| OpenStack Nova | CPU:  Intel(R) Xeon(R) Platinum 8378A CPU @ 3.00GHz<br>内存：32GB<br>硬盘：1000G<br>OS：Linux version 3.10.0-1160.53.1.el7.x86_64 (Red Hat 4.8.5-44)<br>文件系统：XFS |      |

# 3     测试结论概述

## 3.1   测试整体结论

Oracle在线迁移支持DDL语句rename/truncate/drop table语法共计执行31条用例，主要覆盖了功能测试及资料测试。功能测试首先覆盖rename、truncate table及cascade参数（truncate table drop/reuse [all]  storage参数openGauss不支持）、drop table及cascade constraints、purge参数，其次覆盖了对前期版本所支持的DML语句、create table包含约束以及DDL（add/drop/modify）约束迁移的验证，最后覆盖了对DML及DDL迁移顺序的验证。资料测试主要验证资料的描述的准确性。测试中共计发现问题11条，包括5条主要问题单，6条次要问题单，其中9条已处理完成且回归通过，2条非问题取消。无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                             |
| -------- | ---------------------------------------------------- |
| 功能测试 | rename语句验证，通过                                 |
| 功能测试 | truncate table及cascade参数验证，通过                |
| 功能测试 | drop table及cascade constraints、purge参数验证，通过 |
| 功能测试 | DML语句迁移验证，通过                                |
| 功能测试 | create table包含约束及add/drop/modify约束验证，通过  |
| 功能测试 | DML及DDL迁移顺序验证，通过                           |
| 功能测试 | 大小写转换开关测试，通过                             |
| 资料测试 | 资料描述准确，整体质量良好                           |

## 3.2   约束说明

1. Oracle及openGauss两端数据库需有同名schema，如“C##ROMA_LOGMINER”；
2. 开启zookeeper、kafka、kafka_connector、onlineMigration工具并保证正常运行；
3. 启动onlineMigration时，参数--smartConversionOfObjectNames表示大小写转换开关，启动命令包含该参数则大小写转换开关开启，否则开关关闭。目前无须开启该参数；
4. 各对象名（表名、字段名、约束名等）需加上双引号，可保证两端数据库迁移结果一致；
5. 关闭迁移工具后需更改kafka_connector连接名或清空/tmp下kafka日志；
6. 约束（create table及add/drop/modify约束）暂不支持大小写转换开关；
7. truncate语句不支持truncate table的drop/reuse [all] storage参数；

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 11       | 0    | 5    | 6    | 0      |
| 百分比 | 100%     | 0%   | 45%  | 55%  | 0%     |

### 3.3.3  问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I5EOS0]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5EOS0?from=project-issue)) | 次要 | online修改合入后，编译失败报错                               | 已验收 |
| 2    | [I5EN5P](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5EN5P?from=project-issue) | 次要 | 使用debezium社区版本后，create table迁移失败                 | 已验收 |
| 3    | [I5DRMW](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DRMW?from=project-issue) | 主要 | 开启大小写转换开关后，inset及drop语句迁移失败                | 已验收 |
| 4    | [I5DJZI]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DJZI?from=project-issue) ) | 次要 | truncate table不支持cascade参数                              | 已验收 |
| 5    | [I5DA1Y]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DA1Y?from=project-issue) ) | 次要 | 使用了 ByLogicalTableRouter 配置后对主键表进行update及delete操作online报错 | 已验收 |
| 6    | [I5C4Q8](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5C4Q8?from=project-issue) | 主要 | 在线迁移insert和rename失败                                   | 已验收 |
| 7    | [I5BJFM](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5BJFM?from=project-issue) | 主要 | DDL需求合入后online启动失败                                  | 已验收 |
| 8    | [I5D304](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5D304?from=project-issue) | 主要 | 在线迁移建表后添加约束失败                                   | 已验收 |
| 9    | [I5BKCF](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5BKCF?from=project-issue) | 主要 | 在线迁移添加约束失败                                         | 已验收 |
| 10   | [I5DDZ0]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DDZ0?from=project-issue) ) | 次要 | 在线迁移drop table cascade constraint，kafka连接关闭         | 已取消 |
| 11   | [I5DDGK]((https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5DDGK?from=project-issue) ) | 次要 | 在线迁移工具不支持rename to语句                              | 已取消 |



# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启两端数据库并设置同名schema<br />2. 开启kafka、onlineMigration等工具 | 共执行31条用例，<br />共发现11个bug，9条现已修复且验收通过，2条非问题取消 |

#### 4.1.1.1 truncate测试

| 测试步骤：                                                   | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. truncate table <br />2. truncate table cascade <br />3. truncate table drop/reuse [all] storage | 共执行31条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.2 rename测试

| 测试步骤：               | 测试结果                                                     |
| ------------------------ | ------------------------------------------------------------ |
| 1. alter table rename to | 共执行31条用例，<br />共发现2个bug，1条现已修复且验收通过，1条非问题已取消 |

#### 4.1.1.3 drop测试

| 测试步骤：                                                   | 测试结果                                          |
| ------------------------------------------------------------ | ------------------------------------------------- |
| 1. drop table <br />2. drop table cascade constraints<br />3. drop table purge | 执行31条用例，<br />共发现1个bug，1条非问题已取消 |

#### 4.1.1.4 DML语句测试

| 测试步骤：                               | 测试结果                                                |
| ---------------------------------------- | ------------------------------------------------------- |
| 1. insert <br />2. update<br />3. delete | 执行13条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.5 create table包含约束及add/drop/modify约束

| 测试步骤：                                            | 测试结果                                                  |
| ----------------------------------------------------- | --------------------------------------------------------- |
| 1. create table 包含约束<br />2. add/drop/modify 约束 | 共执行13条用例，<br />共发现5个bug，5条现已修复且验收通过 |

#### 4.1.1.6 大小写转换开关测试

| 测试步骤：                                                   | 测试结果                                                 |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| 1. 启动online时增加参数<br />--smartConversionOfObjectNames <br />2. 执行迁移操作 | 共执行1条用例，<br />共发现1个bug，1条现已修复且验收通过 |

#### 4.1.1.7 DML及DDL迁移顺序验证

| 测试步骤：                                                   | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1. insert <br />2. alter table rename/alter table add constraint | 执行3条用例，未发现bug，<br />执行结果符合预期 |

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果                                    | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------------------------------- | ------------ |
| openGauss 3.0.0 build 02c14696 | 31         | Passed：31  <br />Failed：0<br />Unavailable：0 | 11           |

数据说明

1. 共发现11个问题，9条已修改并验证通过，2条非问题取消
2. 1条issue为在环境部署过程中发现（[I5BJFM](https://gitee.com/opengauss/openGauss-tools-onlineMigration/issues/I5BJFM?from=project-issue)）onlineMigration修改后启动失败
3. 失败用例已在后续issue回归中执行通过

## 4.3   后续测试建议

1. 关注各对象名大小写混合使用情况下，工具的迁移结果

# 5     附件

### 5.1 Oracle-openGauss在线迁移DDL语句执行过程示例

```sql
--step1:Oracle创建表 expect:迁移成功
create table "C##ROMA_LOGMINER"."t_ddl_0001"(
	"ID" int,
	"NAME" varchar(50),
	"ADDRESS" varchar(50)
);
Table created.

--step2:insert插入数据 expect:迁移成功
insert into "C##ROMA_LOGMINER"."t_ddl_0001" values(1,'lier','Beijing');
1 row created.
insert into "C##ROMA_LOGMINER"."t_ddl_0001" values(2,'lier','Beijing');
1 row created.
insert into "C##ROMA_LOGMINER"."t_ddl_0001" values(3,'lier','Beijing');
1 row created.
insert into "C##ROMA_LOGMINER"."t_ddl_0001" values(4,'lier','Beijing');
1 row created.
commit;
Commit complete.

--step3:opgs侧查看数据 expect:成功
select * from "C##ROMA_LOGMINER"."t_ddl_0001";
 ID | NAME | ADDRESS
----+------+---------
  1 | lier | Beijing
  2 | lier | Beijing
  3 | lier | Beijing
  4 | lier | Beijing
(4 rows)

--step4:rename表 expect:迁移成功
alter table "t_ddl_0001" rename to "tab_ddl_0001";
Table altered.

--step5:opgs侧查看数据 expect:成功
select * from "C##ROMA_LOGMINER"."tab_ddl_0001";
 ID | NAME | ADDRESS
----+------+---------
  1 | lier | Beijing
  2 | lier | Beijing
  3 | lier | Beijing
  4 | lier | Beijing
(4 rows)

--step6:truncate表 expect:迁移成功
truncate table "C##ROMA_LOGMINER"."tab_ddl_0001";
Table truncated.

--step7:opgs侧查看数据 expect:成功
select * from "C##ROMA_LOGMINER"."tab_ddl_0001";
 ID | NAME | ADDRESS
----+------+---------
(0 rows)

--step8:drop表 expect:迁移成功
drop table "C##ROMA_LOGMINER"."tab_ddl_0001";
Table dropped.

--step9:opgs侧查看表 expect:表不存在
\d
```

 



 

 
