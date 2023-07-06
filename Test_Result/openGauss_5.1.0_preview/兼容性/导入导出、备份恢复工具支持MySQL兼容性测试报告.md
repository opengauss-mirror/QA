![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期       | 修订 版本 | 修改描述   | 作者        |
| ---------- | --------- | ---------- | ----------- |
| 2023-05-19 | 1.0       | 报告初稿   | zhanghuan96 |
| 2023-06-14 | 1.1       | 补充问题单 | zhanghuan96 |

关键词：兼容B库，dolphin插件，导入导出

摘要：本文档为openGauss在兼容B库情形下实现导入导出、备份恢复工具支持M\*特性。包括逻辑导入导出工具（gs_dump、gs_dumpall、gs_restore）和物理备份恢复工具（gs_basebackup、gs_probackup）支持M*兼容性，兼容需求包括openGauss3.0.0、3.1.0、3.1.1的对象、数据类型、DDL语法的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1 特性概述

openGauss在兼容B库情形下安装dolphin插件，实现逻辑导入导出工具（gs_dump、gs_dumpall、gs_restore）和物理备份恢复工具（gs_basebackup、gs_probackup）支持M*兼容性，包括对象、数据类型、DDL语法。

# 2 特性测试信息

被测对象的版本信息和测试的时间

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build 90dad678 | 2023-04-13   | 2023-04-20   |
| openGauss 5.1.0 build 338028e0 | 2023-05-10   | 2023-05-10   |
| dolphin build b5d5f123         | 2023-05-10   | 2023-05-10   |
| openGauss 5.1.0 build ad27d72a | 2023-6-30    | 2023-6-30    |
| dolphin build 95444853         | 2023-6-30    | 2023-6-30    |

特性测试的环境信息

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU: Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz MEM: 32GB DISK: 200GB OS: CentOS Linux release 7.6.1810 (Core) |      |

# 3 测试结论概述

## 3.1 测试整体结论

导入导出、备份恢复工具支持M\*兼容特性，共计执行70个用例，主要覆盖了功能测试和资料测试。功能测试覆盖了逻辑导入导出工具（gs_dump、gs_dumpall、gs_restore）和物理备份恢复工具（gs_basebackup、gs_probackup）支持M*兼容特性，导入导出数据一致；资料测试验证描述是否准确，示例的执行结果是否正确。测试发现5个问题，5个已解决且回归通过，整体质量较好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | gs_dump导出M*数据类型，对象，DDL语句，使用gs_restore导入，导入前后数据一致，功能正常，通过 |
| 功能测试 | gs_dumpall导出tablespace，用户等对象，导入对象成功，通过     |
| 功能测试 | 兼容b库创建表、存储过程等对象，使用gs_basebackup执行备份，修改data_directory后使用备份目录启动数据库，查询对象存在，通过 |
| 功能测试 | 兼容b库创建表、存储过程等对象，使用gs_probackup执行备份，stop数据库并删除DN下的数据，恢复后启动数据库，查询对象存在，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2 约束说明

* B兼容性数据库+dolphin插件

* show create procedure/function等语法的database_collation和collation_connection与数据库lc_collate相同，由于InitSession会重新初始化lc_collate参数，所以show create procedure/function的database_collation和collation_connection两列值不稳定

* 临时表，预备语句等不支持导入导出

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       | NA       | NA       | NA                 | NA       |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 0    | 0    | 5    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题列表

| 序号 | 问题单号                                                     | 问题描述                                                     | 问题级别 | 当前状态 |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- | -------- |
| 1    | [I6VSZ2](https://gitee.com/opengauss/Plugin/issues/I6VSZ2?from=project-issue) | 兼容b库下，number类型导出前和导入后类型不一样导致数据不一致  | 次要     | 已验收   |
| 2    | [I6WJWG](https://gitee.com/opengauss/Plugin/issues/I6WJWG?from=project-issue) | gs_dump导出兼容M侧时间日期类型，timestamp列导入报错          | 次要     | 已验收   |
| 3    | [I6WOW7](https://gitee.com/opengauss/Plugin/issues/I6WOW7?from=project-issue) | 列名使用反引号含空格，gs_dump导出报错                        | 次要     | 已验收   |
| 4    | [I6YM6D](https://gitee.com/opengauss/openGauss-server/issues/I6YM6D?from=project-issue) | 导入导出、备份恢复工具支持M*兼容性需求社区没有资料，请添加   | 次要     | 已验收   |
| 5    | [I7880I](https://gitee.com/opengauss/Plugin/issues/I7880I?from=project-issue) | year(2)类型使用gs_restore导入后是4位年份，预期应该和导入前一致，保持两位年份 | 次要     | 已验收   |

# 4 测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 导入导出兼容M*数据类型

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.创建兼容b库<br>2.建表覆盖兼容数据类型<br>3.gs_dump导出，覆盖不同的导出文件格式<br>4.gsql及gs_restore导入<br>5.验证导入库数据是否正确 | 执行8条用例，发现3个bug，验收通过 |

#### 4.1.1.2 导入导出兼容M*对象

| 测试步骤                                                     | 测试结果                           |
| ------------------------------------------------------------ | ---------------------------------- |
| 1.创建兼容b库<br/>2.创建对象，含表、视图、函数/存储过程、索引等<br>3.导出上述对象<br>4.删除对象后执行导入<br>5.查看导入后对象是否存在 | 执行40条用例，发现1个bug，验收通过 |

#### 4.1.1.3 导入导出兼容M*DDL语法

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.创建兼容b库<br>2.创建DDL语句<br>3.导出DDL<br>4.导入新库<br>5.连接导入库查询DDL语句是否正确 | 执行22条用例，结果符合预期，测试通过 |

### 4.1.2 资料测试

| 测试步骤           | 测试结果               |
| ------------------ | ---------------------- |
| 资料的描述是否正确 | 发现1个bug，已回归通过 |

## 4.2 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.0.0 build 90dad678 | 70         | Passed: 67 Failed: 4 | 5            |
| openGauss 5.1.0 build 338028e0 | 4          | Passed: 4 Failed: 0  | 0            |
| openGauss 5.1.0 build ad27d72a | 1          | Passed: 1 Failed: 0  | 0            |

数据项说明：

- 累计发现缺陷单5个，5个缺陷均已解决且回归通过，其中资料单1个
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：4(缺陷个数)/0.201kloc(代码行数)=19.9(个/kloc)

## 4.3 后续测试建议

无

# 5 附件

```sql
--创建表，含枚举类型
db=# create table t1(name varchar(20),gender enum('male','female'));
CREATE TABLE
--插入数据
db=# insert t1 values('Tom','male'),('jenny','female'),('lily',1);
INSERT 0 3
--查询
db=# select * from t1;
 name  | gender
-------+--------
 Tom   | male
 jenny | female
 lily  | male
(3 rows)

db=# \d+ t1
                                      Table "public.t1"
 Column |              Type               | Modifiers | Storage  | Stats target | Description
--------+---------------------------------+-----------+----------+--------------+-------------
 name   | character varying(20)           |           | extended |              |
 gender | t1_gender_2200_anonymous_enum_1 |           | plain    |              |
Has OIDs: no
Options: orientation=row, compression=no
--gs_dump导出
[zxh@home script]$ gs_dump -p 30522 db -F t -f /home/zxh/dump0001
gs_dump[port='30522'][db][2023-05-22 10:37:55]: The total objects number is 432.
gs_dump[port='30522'][db][2023-05-22 10:37:55]: [100.00%] 432 objects have been dumped.
gs_dump[port='30522'][db][2023-05-22 10:37:55]: dump database db successfully
gs_dump[port='30522'][db][2023-05-22 10:37:55]: total time: 2021  ms
--gs_restore导入
[zxh@home script]$ gs_restore -p 30522 -d db1 /home/zxh/dump0001
start restore operation ...
table t1 complete data imported !
Finish reading 6 SQL statements!
end restore operation ...
restore operation successful
total time: 17  ms
--连接导入库查询，数据类型和数据一致
openGauss=# \c db1
Non-SSL connection (SSL connection is recommended when requiring high-security)
You are now connected to database "db1" as user "zxh".
db1=# \d+ t1
                                      Table "public.t1"
 Column |              Type               | Modifiers | Storage  | Stats target | Description
--------+---------------------------------+-----------+----------+--------------+-------------
 name   | character varying(20)           |           | extended |              |
 gender | t1_gender_2200_anonymous_enum_1 |           | plain    |              |
Has OIDs: no
Options: orientation=row, compression=no

db1=# select * from t1;
 name  | gender
-------+--------
 Tom   | male
 jenny | female
 lily  | male
(3 rows)
```

