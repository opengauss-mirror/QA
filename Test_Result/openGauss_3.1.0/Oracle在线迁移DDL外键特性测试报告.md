

![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述                 | 作者     |
| ---------- | ----------- | ------------------------ | -------- |
| 2022-04-26 | 1.0         | 特性测试报告初稿完成     | liyazhou |
| 2022-04-28 | 1.1         | 根据评审意见修改测试报告 | liyazhou |

关键词： onlineMigration工具；在线DDL外键

摘要：本文档主要介绍oracle-openGauss在线迁移工具由oracle(生产端)，debezium，kafka， onlineMigration-openGauss(消费端)这几部分组成的在线DDL外键测试特性报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

oracle-openGauss在线迁移工具由oracle(生产端)，debezium，kafka， OnlineMigration-openGauss(消费端)这几部分组成。debezium监控oracle中的变化并把改变的记录捕捉到记录到kafak中的topic中，然后通过onlineMigration把topic中的记录迁移到openGauss数据库中，这样就组成从oracle到openGauss的在线迁移。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 02c14696 | 2022-04-20   | 2022-04-26   |
| Oracle version 19.3.0.0.0      | 2022-04-20   | 2022-04-26   |
| kafka v2.13-2.8.1              | 2022-04-20   | 2022-04-26   |
| debezium v1.8.1                | 2022-04-20   | 2022-04-26   |
| onlineMigration v1.0           | 2022-04-20   | 2022-04-26   |

描述特性测试的硬件环境信息

| 硬件型号       | 硬件配置信息                                                 | 备注 |
| -------------- | ------------------------------------------------------------ | ---- |
| OpenStack Nova | CPU:  Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br>内存：16GB<br>硬盘：ssd<br>OS：Centos Linux 7<br>文件系统：XFS |      |

# 3     测试结论概述

## 3.1   测试整体结论

oracle在线DDL迁移外键共计执行87条用例，主要覆盖了功能测试。功能测试覆盖表级外键，列级外键，包括外键的drop，alter；发现问题已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                  |
| -------- | --------------------------------------------------------- |
| 功能测试 | create外键，alter table add外键，alter table drop外键测试 |
| 功能测试 | 验证DML和DDL的执行顺序测试                                |
（当插入的数据量较小时（插入10000条数据进行测试），执行顺序为DDL-DML-DDL,当数据量较大时，（插入1000000条数据进行测试），执行顺序为当DML执行到中途时会执行DDL,执行完DDL后再执行剩余的DML操作，当DML中途时修改openGauss侧的表名，DML操作会被中断，online里面报错，表不存在。

## 3.2   约束说明

1. Oracle及openGauss两端数据库需有同名schema，如“C##ROMA_LOGMINER”；
2. 开启zookeeper、kafka、kafka_connector、onlineMigration工具并保证正常运行；
3. 各对象名（表名、字段名、约束名等）需加上双引号，可保证两端数据库迁移结果一致；
4. 关闭迁移工具后需更改kafka_connector连接名或清空/tmp下kafka日志；

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 | 备注 |
| ------ | -------- | ---- | ---- | ---- | ------ | ---- |
| 数目   | 6        | 0    | 2    | 4    | 0      |      |
| 百分比 | 100%     | 0%   | 33%  | 66%  | 0%     |      |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I53JHC](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I53JHC?from=project-issue) | 主要 | or在线迁移外键父表添加约束指定多列迁移不成功                 | 已验收 |
| 2    | [I53HJK](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I53HJK?from=project-issue) | 主要 | 在线迁移外键时不支持列级外键                                 | 已验收 |
| 3    | [I5545Z](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I5545Z?from=project-issue) | 次要 | alter table drop在外键约束存在的情况下强制删除主键迁移不成功 | 已验收 |
| 4    | [I553TN](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I553TN?from=project-issue) | 次要 | alter table add在两表均添加联合主键，迁移不成功              | 已验收 |
| 5    | [I553FV](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I553FV?from=project-issue) | 次要 | 当父表中包含联合主键时，迁移不成功                           | 已验收 |
| 6    | [I53NN1](https://gitee.com/openGauss/openGauss-tools-onlineMigration/issues/I53NN1?from=project-issue) | 次要 | or在线迁移外键先迁移带主键的父表和不带外键的子表，再给子表添加外键约束，迁移失败 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. 开启两端数据库并设置同名schema<br />2. 开启kafka、onlineMigration等工具 | 共执行87条用例，<br />共发现6个bug，6条现已修复且验收通过 |

#### 4.1.1.1 create外键测试

| 测试步骤：               | 测试结果                                                |
| ------------------------ | ------------------------------------------------------- |
| 1. create table 包含外键 | 共执行29条用例<br />共发现2个bug，2条现已修复且验收通过 |

#### 4.1.1.2 alter table add外键测试

| 测试步骤：                   | 测试结果                                                |
| ---------------------------- | ------------------------------------------------------- |
| 1. create table add 添加外键 | 共执行29条用例<br />共发现3个bug，3条现已修复且验收通过 |

#### 4.1.1.3 alter table drop外键测试

| 测试步骤：                    | 测试结果                                                |
| ----------------------------- | ------------------------------------------------------- |
| 1. create table drop 删除外键 | 共执行29条用例<br />共发现1个bug，1条现已修复且验收通过 |

## 4.2   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果          | 发现问题单数 |
| ----------------------------- | ---------- | --------------------- | ------------ |
| openGauss 3.0.0build 02c14696 | 87         | Passed：87  Failed：0 | 6            |

数据说明

1. 目前onlineMigration不支持分区表迁移，涉及用例数目为11个

## 4.3   后续测试建议

1. 约束实现后需对继承特性进行验证
2. 在oracle侧建表不指定schema，会默认迁移到和oracle用户同名的schema下，这样就导致alter外键和DML的时候会迁移失败，因为在oracle侧如果不指定schema会默认是到openGauss侧的public下找表

# 5     附件

### 5.1 Oracle-openGauss在线迁移DDL外键语句执行过程示例

```sql
--step1:oracle侧创建父表并添加主键约束 expect:创建父表并添加约束成功
create table "C##ROMA_LOGMINER"."t_fkey_0002_01"(
	p_id int primary key,
	p_name varchar(20) not null
);

--step2:oracle侧创建子表并添加外键约束 expect:创建子表并添加外键约束
create table "C##ROMA_LOGMINER"."t_fkey_0002_02"(
	s_id int references "C##ROMA_LOGMINER"."t_fkey_0002_01"(p_id),
	s_name varchar(20) not null
);

--step3:在opengauss侧查看表 expect:迁移成功
\d+ "C##ROMA_LOGMINER"."t_fkey_0002_01"
Column |         Type          | Modifiers | Storage  | Stats target | Description
--------+-----------------------+-----------+----------+--------------+-------------
 p_id   | numeric(38,0)         | not null  | main     |              |
 p_name | character varying(20) | not null  | extended |              |
Indexes:
    "t_fkey_0002_01_pkey" PRIMARY KEY, btree (p_id) TABLESPACE pg_default
Has OIDs: no
Options: orientation=row, compression=no

\d+ "C##ROMA_LOGMINER"."t_fkey_0002_02"
                      Table "C##ROMA_LOGMINER.t_fkey_0002_02"
 Column |         Type          | Modifiers | Storage  | Stats target | Description
--------+-----------------------+-----------+----------+--------------+-------------
 s_id   | integer               |           | plain    |              |
 s_name | character varying(20) | not null  | extended |              |
Foreign-key constraints:
    "t_fkey_0002_02_s_id_fkey" FOREIGN KEY (s_id) REFERENCES "C##ROMA_LOGMINER".t_fkey_0002_01(p_id)
Has OIDs: no
Options: orientation=row, compression=no
```