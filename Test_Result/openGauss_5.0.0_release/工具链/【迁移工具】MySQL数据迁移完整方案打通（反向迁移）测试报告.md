![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者    |
| ---------- | ----------- | -------- | ------- |
| 2023.03.22 | V1.0        | 初稿     | qiuying |
|            |             |          |         |

 关键词： 

反向迁移、Debezium+kafka、性能、3w tps

 摘要：

本文档内容为反向迁移收编到Dezebium+kafka，完成性能1w tps和数据类型转换目标。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| `tps`       | `Transaction Per Second` | 每秒事务数                                       |

# 1     特性概述

本文档主要基于Debezium+kafka开源组件，实现openGauss反向迁移到MySQL，同时反向迁移性能满足1w tps。

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | :----------- | ------------ |
| openGauss 5.0.0 build c323237f | 2023-2-23    | 2023-3-8     |
| openGauss 5.0.0 build 5a69c469 | 2023-3-8     | 2023-3-9     |
| openGauss 5.0.0 build e94d3b43 | 2023-3-9     | 2023-3-10    |
| openGauss 5.0.0 build 78f2bb65 | 2023-3-16    | 2023-3-16    |
| openGauss 5.0.0 build eacf1d20 | 2023-3-19    | 2023-3-19    |
| MySQL 5.7.27                   | 2023-2-23    | 2023-3-19    |
| chameleon 3.1.1                | 2023-2-23    | 2023-3-19    |
| sysbench 1.0.20                | 2023-2-23    | 2023-3-19    |
| Debezium v1.8.1.Final          | 2023-2-23    | 2023-3-19    |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：32 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | openGauss 5.0.0<br />                                        |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | MySQL 5.7.27<br/>chameleon 3.1.0                             |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | sysbench 1.0.20                                              |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：32 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | kafka_2.13-3.2.3<br/>confluent-community-5.5.1-2.12<br/>debezium-connector-opengauss-1.8.1.Final-plugin.tar.gz |

# 3     测试结论概述

## 3.1   测试整体结论

MySQL数据迁移完整方案打通_反向迁移，且性能满足1w tps特性，共执行用例100个，主要覆盖了功能测试、性能测试、安全测试、资料测试。功能测试覆盖开启反向迁移，在openGauss数据库分别执行增删改操作，反向迁移工具迁移将上述DML操作迁移到MySQL，保证迁移前后数据的一致性、有序性和完整性。性能测试，在高性能机器下，开启反向迁移，通过sysbench工具给openGauss压测数据，待数据回放完成，计算迁移效率。安全测试关注迁移日志中密码是否加密。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单11个，11个缺陷均已解决且回归通过，整体质量良好。



## 3.2   约束说明

1、MySQL 5.7.27版本，数据库除初始用户外的数据库用户均有复制权；配置文件添加以下参数：

MySQL参数设置： log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON

2、openGauss侧的目标库为兼容B库，仅限初始用户和拥有REPLICATION权限的用户进行操作。

3、三权分立关闭时数据库管理员可以进行逻辑复制操作，三权分立开启时不允许数据库管理员进行逻辑复制操作。

4、迁移前需要开启数据库的逻辑复制相关配置：wal_level = logical，ssl=on，

5、迁移无主键的表时，openGauss侧需要执行：alter table tablename replica identity full

6、反向迁移sink端按表分发数据，不支持按事务分发，日志中记录的回放条数为实际成功执行的sql语句条数

7、反向迁移connector端配置连接数据库的用户需要有对应数据库下所有schema以及所有表的操作权限

8、仅支持从openGauss迁移至MySQL，支持DML迁移，不支持DDL迁移

9、仅支持在线迁移，不支持离线全量迁移

10、性能测试对硬件系统有较高要求，必须进行性能调优

- mysql高性能配置，binlog位置、安装目录、数据目录分别部署在3个不同的NVME盘
- openGauss侧openGauss高性能配置
- 反向迁移进程绑核启动

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 11     | 0    | 4  | 7 | 0      |
| 百分比 |  100%        | 0%   | 36% | 64% | 0%    |

### 3.3.3 问题单汇总

|序号| issue号                                                      | 级别 | 问题简述                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------- | ------ |
| 1    | [I6BLU0](https://gitee.com/opengauss/debezium/issues/I6BLU0?from=project-issue) | 主要 | 【测试类型：工具功能】【测试版本：5.0.0】 需求已转测但代码未全部合入 | 已验收 |
| 2    | [I6DRS8](https://gitee.com/opengauss/debezium/issues/I6DRS8?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】 openGauss的库名和MySQL的库名不一致时，开启反向迁移kafka-connect报错 | 已验收 |
| 3    | [I6EFP2](https://gitee.com/opengauss/debezium/issues/I6EFP2?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】 debezium+kafka反向迁移成功，回显信息应提示replayed多少条事务 | 已验收 |
| 4 | [I6EFV4](https://gitee.com/opengauss/debezium/issues/I6EFV4?from=project-issue) | 主要 | 【测试类型：安全】【测试版本：5.0.0】 debezium+kafka反向迁移开启后，回显和日志中存在明文密码 | 已验收 |
| 5 | [I6FUJC](https://gitee.com/opengauss/debezium/issues/I6FUJC?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】：Debezium+kafka反向迁移blob类型，insert,update和delete操作后迁移结果不一致 | 已验收 |
| 6 | [I6FUYL](https://gitee.com/opengauss/debezium/issues/I6FUYL?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】：Debezium+kafka反向迁移bit/set/几何类型，insert,update和delete操作迁移失败 | 修复中 |
| 7 | [I6FV5D](https://gitee.com/opengauss/debezium/issues/I6FV5D?from=project-issue) | 主要 | 【测试类型：资料】【测试版本：5.0.0】 缺少反向迁移资料，补充无主键时的约束说明 | 已验收 |
| 8 | [I6G1YY](https://gitee.com/opengauss/debezium/issues/I6G1YY?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】：Debezium+kafka反向迁移json类型，update和delete后，迁移结果显示已回放，实际M*库数据未更新 | 已验收 |
| 9 | [I6H1WT](https://gitee.com/opengauss/debezium/issues/I6H1WT?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】：Debezium+kafka反向迁移，用户没有指定数据库下所有schema的权限，修改连接器名称name，启动迁移报错 | 已完成 |
| 10 | [I6HAMZ](https://gitee.com/opengauss/debezium/issues/I6HAMZ?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：5.0.0】：Debezium+kafka反向迁移，opengauss数据库开启线程池参数后，迁移进程启动失败 | 已完成 |
| 11 | [I6HRP3](https://gitee.com/opengauss/debezium/issues/I6HRP3?from=project-issue) | 主要 | 【测试类型：性能】【测试版本：5.0.0】 Debezium+kafka反向迁移insert场景时，迁移性能达不到3W+ tps | 修复中 |

# 4     测试执行

## 4.1.1 功能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启反向迁移，在opengauss执行DML语句<br/>2. opengauss端查看数据一致性，有序性，准确性 | 执行17条用例，执行结果符合预期，测试通过，发现2个bug，1个问题已解决回归通过，1个暂未解决 |

### 4.1.1   功能测试实现步骤

1、mysql数据库创建表，插入数据

2、使用chameleon工具进行数据的离线迁移

```shell
chameleon create_replica_schema --config default --debug
chameleon add_source --config default --source mysql --debug
chameleon init_replica --config default --source mysql --debug
```

3、开启反向迁移

（1）启动zookeeper

```
cd kafka_2.13-3.2.3
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
```

（2）启动kafka

```
cd kafka_2.13-3.2.3
./bin/kafka-server-start.sh ./config/server.properties
```

（3）注册schema

```
cd confluent-5.5.1
./bin/schema-registry-start etc/schema-registry/schema-registry.properties
```

（4）启动kafka-connect source端

```
cd confluent-5.5.1
./bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/kafka/opengauss-source.properties
```

（5）启动kafka-connect sink端

```
cd confluent-5.5.1
./bin/connect-standalone etc/schema-registry/connect-avro-standalone-1.properties etc/kafka/opengauss-sink.properties
```

4、opengauss数据库执行DML操作

5、mysql查看表数据

## 4.1.2   性能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启反向迁移，sysbench向opengauss压测数据<br />2. 查看迁移日志，计算迁移性能 | 执行5条用例，执行结果符合预期，测试通过，发现1个bug，已解决并回归通过 |

### 4.1.2.1   性能测试实现步骤

1、借助sysbench工具在MySQL和openGauss的目标库中生成数据

2、 开启在线复制依次启动zookeeper，kafka，注册schema，绑核启动source端，绑核启动sink端

3、 sysbench执行run命令，给openGauss压入数据

4、 统计迁移工具日志，得到迁移效率(事务数/时间)

## 4.1.3 安全测试

| 测试步骤                                                     | 测试结果                     |
| ------------------------------------------------------------ | ---------------------------- |
| 1. 开启反向迁移  <br/>2. 查看迁移日志、迁移进程中密码是否加密 | 发现1个bug，已解决并回归通过 |

## 4.1.4 资料测试

| 测试步骤                     | 测试结果                                                 |
| ---------------------------- | -------------------------------------------------------- |
| 1. 文档描述 <br/>2. 文档示例 | 文档描述准确，示例正确无误，发现1个bug，已解决并回归通过 |

## 4.2   测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.0.0 build c323237f | 50         | Passed:35 Failed:15  | 11           |
| openGauss 5.0.0 build 5a69c469 | 30         | Passed:20 Failed: 10 | 0            |
| openGauss 5.0.0 build e94d3b43 | 3          | Passed:3 Failed:0    | 0            |
| openGauss 5.0.0 build eacf1d20 | 3          | Passed:3 Failed:0    | 0            |
|                                |            |                      |              |

*数据项说明：*

* 累计发现缺陷单11个，11个缺陷已解决且回归通过，其中属于迁移工具问题的缺陷为7个，其中3个为资料单
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：7(缺陷个数)/18.766kloc(代码行数)=0.37(个/kloc)

### 4.2.1   sysbench使用不同参数配置，所测得的平均迁移速度

| lua模型    | num-threads | oltp-tables-count | oltp-table-size | max-time | sysbench速度(w tps) | 平均迁移速度(w/tps) |
| ---------- | ----------- | ----------------- | --------------- | -------- | --------------- | ---------- |
| Insert.lua                                                  | 30          | 30                | 1000            | 50       | 6.6968              | 2.1520              |
| Update_index.lua                                            | 30          | 30                | 100000          | 50       | 6.1271              | 2.0374              |
| Update_non_index.lua                                        | 30          | 30                | 100000          | 50       | 6.1804              | 2.3226              |
| Delete.lua                                                  | 30          | 30                | 100000          | 10       | 2.5841              | 2.6212              |
| 混合场景(insert-30线程、update-index-10线程、delete-10线程) | 50          | 50                | 100000          | 50       | 2.6910              | 2.1381              |

混合场景占比：

| 事务总数 | sysbench速率(w tps) | Insert事务数 | Insert速率 | Insert占比 | Update事务数 | Update速率 | Update占比 | Delete事务数 | Delete速率 | Delete占比 |
| -------- | ------------------- | ------------ | ---------- | ---------- | ------------ | ---------- | ---------- | ------------ | ---------- | ---------- |
| 4022888  | 2.6910              | 3199213      | 6.3975     | 79.53%     | 399145       | 0.7982     | 9.92%      | 424530       | 0.8774     | 10.55%     |

根据测试数据，分析可得：反向迁移性能测试结果达到1w tps以上。

## 4.3   后续测试建议

1、基于不同硬件配置的物理机进行反向迁移性能测试

2、基于sysbench自定义lua脚本生成数据，如修改数据类型、数据长度等

# 5     附件

### 5.1 30个线程，30张表，1000行数据，使用insert模型时所测得的迁移速度

sysbench压测opengauss对应的性能为68703.28/s

```
numactl -C 121-127 [path_to_sysbench] --db-driver=pgsql [path_to_lua] --pgsql-host=[ip] --pgsql-port=[port] --pgsql-db=[db_name] --pgsql-password=[password]  --pgsql-user=[user] --oltp-table-size=1000  --oltp-tables-count=30 --num-threads=30 --max-time=50 --report-interval=10 run
WARNING: --num-threads is deprecated, use --threads instead
WARNING: --max-time is deprecated, use --time instead
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 30
Report intermediate results every 10 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 10s ] thds: 30 tps: 71374.85 qps: 71374.85 (r/w/o: 0.00/71374.85/0.00) lat (ms,95%): 0.54 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 30 tps: 69133.88 qps: 69133.98 (r/w/o: 0.00/69133.98/0.00) lat (ms,95%): 0.67 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 30 tps: 68188.57 qps: 68188.47 (r/w/o: 0.00/68188.47/0.00) lat (ms,95%): 0.75 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 30 tps: 67324.78 qps: 67324.78 (r/w/o: 0.00/67324.78/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 30 tps: 67524.92 qps: 67524.92 (r/w/o: 0.00/67524.92/0.00) lat (ms,95%): 0.81 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           3435623
        other:                           0
        total:                           3435623
    transactions:                        3435623 (68703.28 per sec.)
    queries:                             3435623 (68703.28 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          50.0050s
    total number of events:              3435623

Latency (ms):
         min:                                    0.25
         avg:                                    0.44
         max:                                   46.08
         95th percentile:                        0.68
         sum:                              1497926.42

Threads fairness:
    events (avg/stddev):           114520.7667/3984.45
    execution time (avg/stddev):   49.9309/0.00
```

反向迁移sink端回放的事务：

```
[2023-03-20 15:00:20,261] INFO have replayed 48196 data, and current time is 2023-03-20 15:00:20.261, and current speed is 34179 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:21,262] INFO have replayed 69636 data, and current time is 2023-03-20 15:00:21.262, and current speed is 21441 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:22,263] INFO have replayed 92164 data, and current time is 2023-03-20 15:00:22.262, and current speed is 22528 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:23,263] INFO have replayed 114656 data, and current time is 2023-03-20 15:00:23.263, and current speed is 22486 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:24,264] INFO have replayed 135172 data, and current time is 2023-03-20 15:00:24.264, and current speed is 20521 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:25,265] INFO have replayed 155652 data, and current time is 2023-03-20 15:00:25.264, and current speed is 20480 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:26,265] INFO have replayed 176132 data, and current time is 2023-03-20 15:00:26.265, and current speed is 20480 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:27,266] INFO have replayed 198660 data, and current time is 2023-03-20 15:00:27.266, and current speed is 22528 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:28,267] INFO have replayed 219140 data, and current time is 2023-03-20 15:00:28.266, and current speed is 20480 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:29,267] INFO have replayed 242956 data, and current time is 2023-03-20 15:00:29.267, and current speed is 23793 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:00:30,268] INFO have replayed 264196 data, and current time is 2023-03-20 15:00:30.268, and current speed is 21263 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
......
[2023-03-20 15:02:45,355] INFO have replayed 3276804 data, and current time is 2023-03-20 15:02:45.355, and current speed is 22977 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:46,356] INFO have replayed 3299332 data, and current time is 2023-03-20 15:02:46.356, and current speed is 22528 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:47,357] INFO have replayed 3321779 data, and current time is 2023-03-20 15:02:47.356, and current speed is 22438 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:48,357] INFO have replayed 3342340 data, and current time is 2023-03-20 15:02:48.357, and current speed is 20570 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:49,358] INFO have replayed 3366915 data, and current time is 2023-03-20 15:02:49.358, and current speed is 24574 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:50,358] INFO have replayed 3389444 data, and current time is 2023-03-20 15:02:50.358, and current speed is 22530 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:51,045] INFO WorkerSinkTask{id=connect-opengauss-sink-0} Committing offsets asynchronously using sequence number 3: {dml_topic_0336-0=OffsetAndMetadata{offset=3403780, leaderEpoch=null, metadata=''}} (org.apache.kafka.connect.runtime.WorkerSinkTask:348)
[2023-03-20 15:02:51,359] INFO have replayed 3409925 data, and current time is 2023-03-20 15:02:51.359, and current speed is 20480 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
[2023-03-20 15:02:52,360] INFO have replayed 3433174 data, and current time is 2023-03-20 15:02:52.359, and current speed is 23234 (io.debezium.connector.opengauss.sink.replay.JdbcDbWriter:195)
```

计算迁移效率(事务数之差/时间之差)：

```
(3433174 - 48196)/(15:02:52 - 15:00:20)=2.2269w tps
```

