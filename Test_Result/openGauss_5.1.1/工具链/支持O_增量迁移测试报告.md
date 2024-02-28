![avatar](../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者 |
| ---------- | -------- | -------------------- | ---- |
| 2023-12-27 | 1.0      | 初稿                 | 2JQ  |
| 2024-01-24 | 1.1      | 补充测试项和bug验收  | 张山 |
| 2024-02-22 | 1.2      | 根据检视意见修改描述 | 张山 |

 关键词： 

O*，openGauss，增量迁移 

摘要：

 本报告主要介绍O*向openGauss增量迁移的测试结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

支持O*数据库在全量迁移过后，对openGauss数据库进行数据的增量迁移操作。

其中，支持的增量迁移操作包括：

dml操作：insert、update、delete操作

ddl操作：按事务回放的create、alter、drop等操作

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称           | 测试起始时间 | 测试结束时间 |
| ------------------ | ------------ | ------------ |
| openGauss5.1.1B009 | 2023-12-15   | 2023-12-22   |
| openGauss5.1.1B009 | 2024-01-17   | 2024-01-24   |
|                    |              |              |

描述特性测试的硬件环境信息

| 环境信息 | 配置信息        | 备注                                                         |
| -------- | --------------- | ------------------------------------------------------------ |
| 硬件     | 服务器架构      | Linux  localhost 3.10.0-1127.el7.x86_64 #1 SMP Tue Mar 31 23:36:51 UTC 2020 x86_64  x86_64 x86_64 GNU/Linux |
| 硬件     | CPU             | 2核                                                          |
| 硬件     | 内存            | 7GB                                                          |
| 硬件     | 磁盘            | SDA  100GB                                                   |
| 硬件     | 网络            | 10000Mb/s                                                    |
| 软件     | openGauss数据库 | openGauss-5.1.1-CentOS-64bit                                 |
| 软件     | O*数据库        | O*_11g-11.2.0.1.0-64bit                                      |
| 软件     | Kafka中间数据库 | Kafka_2.13-3.6.0                                             |
| 软件     | Confluent中间件 | Confluent-5.5.1                                              |
| 软件     | Postman         | Postman-10.21.0.0                                            |

# 3     测试结论概述

## 3.1   测试整体结论

增量迁移特性，共计执行48个用例，主要覆盖了功能测试、易用性测试、可靠性测试、性能测试、稳定性测试和资料测试，功能测试覆盖已支持的O*常用数据类型、表类型等测试，以及不同的dml、ddl语法对于增量迁移的影响。校验资料的描述及示例的执行结果是否通过。累计发现issue3个，已验收通过，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 带有常用数据类型的表及表类型增量迁移，测试通过               |
| 功能测试   | 使用常用的dml、ddl语法进行增量迁移，测试通过                 |
| 易用性测试 | 对命令配置及程序启用过程的易用性进行测试，测试通过，配置项说明清晰、错误提示详尽 |
| 可靠性测试 | 对异常（如：kill进程等）场景下的增量迁移过程进行测试，异常恢复后增量迁移过程可以继续进行 |
| 性能测试   | 1张大表边写入10w条数据边进行增量迁移，增量迁移速率约588条/s  |
| 资料测试   | 校验资料的描述及示例的执行结果是否通过，测试通过             |
| 稳定性测试 | 运行7*24后，进行增量迁移，测试通过                           |

## 3.2   约束说明

1. 本次测试对O*到openGauss的增量迁移进行测试

2. 需要为O*开启归档参数archive log，并赋予相关权限，操作步骤如下：

   1）查看是否处于归档模式：SELECT log_mode FROM v$database;如果现实数据库为归档模式（ARCHIVELOG），则后续步骤不用做。

   2）开启归档模式：

   --设置归档目录位置（注：此目录需要在执行sql前手动创建）

   SQL>ALTER SYSTEM SET db_recovery_file_dest='/opt/O/oradata/recovery_area' scope=spfile;

   --重启数据库

   SQL> SHUTDOWN IMMEDIATE;

   SQL> STARTUP MOUNT;

   --开启归档日志

   SQL> ALTER DATABASE ARCHIVELOG;

   --开启数据库

   SQL> ALTER DATABASE OPEN;

   --如果为pdb库，执行

   SQL> ALTER PLUGGABLE DATABASE 【库名】 OPEN;

   3）为待同步的用户授权，此处用test进行说明，如用其他用户请进行替换

   SQL>  grant connect to test;

   SQL> grant execute on dbms_logmnr to test;

   SQL> grant select any transaction to test;

   SQL> grant create table to test;

   SQL> grant select on v_$logmnr_contents to test;

   SQL> grant select on v_$log to test;

   SQL> grant select on v_$archived_log to test;

   SQL> grant select on v_$database to test;

   SQL> grant select on v_$logfile to test;

   SQL> grant select on gv_$log to test;

   SQL> grant select on gv_$archived_log to test;

   SQL> grant select on gv_$logfile to test;

   4）为数据库和特定的表启用补充日志记录

   alter databse SID add supplemental log data;

   alter table schema_name.table_name add supplemental log data (all) columns;

3. 在启动增量迁移任务之前，需要启动zookeeper、kafka等工具

   cd kafka_2.13-3.2.3
   ./bin/zookeeper-server-start.sh ./config/zookeeper.properties

   ./bin/kafka-server-start.sh ./config/server.properties

3. dml中仅对数据的insert、update、delete操作进行增量迁移，dml操作可以不在事务中；ddl必须在事务中才可以进行增量同步。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 0    | 3    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 问题单汇总

| 序号 | issue号 | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------- | -------- | ------------------------------------------------------------ | -------- |
| 1    | I8ONWC  | 次要     | Debezium+Kafka增量迁移O*表名字段名为小写的表失败             | 已验收   |
| 2    | I8PDVK  | 次要     | Debezium+Kafka增量迁移O*的blob、long、raw、long raw类型的数据失败 | 已验收   |
| 3    | I8PWJ4  | 次要     | Debezium+Kafka增量迁移O*的alter部分语法形式同步失败          | 已验收   |



# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称           | 测试用例数 | 用例执行结果 | 发现问题单数 |
| ------------------ | ---------- | ------------ | ------------ |
| openGauss5.1.1B009 | 48         | 通过：45     | 3            |
|                    |            |              |              |
|                    |            |              |              |

*数据项说明：*

- 首轮测试用例执行48个，不通过3个，缺陷问题单汇总；第二轮回归问题单执行3个用例，通过3个；
- 本需求缺陷密度为3(缺陷个数)/5.681k(代码行数)=0.53(个/kloc)。

## 4.2   后续测试建议

无

# 5     附件

文档资料：

https://gitee.com/openGauss/debezium/blob/master/README.md

代码资料：

https://gitee.com/openGauss/debezium/pulls/176

https://gitee.com/openGauss/debezium/pulls/209
https://gitee.com/openGauss/debezium/pulls/208
https://gitee.com/openGauss/debezium/pulls/211 



 

 
