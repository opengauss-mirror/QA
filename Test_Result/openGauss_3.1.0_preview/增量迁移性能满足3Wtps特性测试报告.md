![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者    |
| ---------- | ----------- | -------- | ------- |
| 2022.09.22 | V1.0        | 初稿     | qiuying |
|            |             |          |         |

 关键词： 

online在线迁移、增量迁移、性能、3w tps

 摘要：

本文档内容为验证在特定场景下借助online在线迁移工具把Mysql的DML操作迁移到openGauss数据库，迁移性能满足3w tps。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| tps    | Transaction Per Second         |  每秒事务数        |

# 1     特性概述

在特定场景下借助online在线迁移工具把Mysql的DML操作迁移到openGauss数据库，insert和混合迁移性能满足3w tps。

特定场景如下：使用sysbench向Mysql压测数据，当oltp-table-size = 1000、oltp-tables-count =10、oltp-tables-count=30、使用insert模型时，MySQL数据在线迁移性能满足3w tps。

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                                | 测试起始时间 | 测试结束时间 |
| --------------------------------------- | :----------- | ------------ |
| openGauss 3.1.0 build 4c9602a1          | 2022-09-07   | 2022-9-22    |
| MySQL 5.7.27                            | 2022-09-07   | 2022-9-22    |
| chameleon 3.0.0                         | 2022-09-07   | 2022-9-22    |
| sysbench 1.0.20                         | 2022-09-07   | 2022-9-22    |
| online-migration-mysql-3.1.0-SNAPSHOT.jar | 2022-09-07   | 2022-9-22    |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：32 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | openGauss 3.1.0 build 4c9602a1<br />online-migration-mysql-3.1.0-SNAPSHOT.jar |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | MySQL 5.7.27<br/>chameleon 3.0.0                             |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | sysbench 1.0.20                                              |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss 数据增量迁移性能满足3w tps特性，共执行用例12个，无问题单，无遗留风险，整体质量良好。

根据测试数据，得出结论：当oltp-table-size = 1000、oltp-tables-count =10、oltp-tables-count=30、使用insert模型时，MySQL数据在线迁移性能满足3w tps。

## 3.2   约束说明

1、MySQL 5.7以上版本，数据库除初始用户外的数据库用户均有复制权；配置文件添加以下参数：

MySQL参数设置： log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON

2、openGauss使用高性能配置部署，绑核启动，openGauss侧的目标库为兼容B库，除初始用户外的数据库用户均有复制权

3、chameleon工具已安装并完成配置

4、sysbench已安装

5、online在线迁移工具已编译jar包

6、update和delete场景下，迁移性能无法满足3w tps

7、仅支持从MySQL迁移至openGauss，支持DML迁移，不支持DDL迁移

# 4     测试执行

## 4.1.1   测试实现步骤

1、借助sysbench工具在MySQL的目标库中生成数据（lua脚本中create_insert功能模块中条件为db_driver == "mysql"时，将char修改为varchar，然后使用sysbench生成数据。sysbench生成的数据总量 = data_pieces * table_num）

``` shell
numactl -C cpu_num1-cpu_num2 sysbench --db-driver=mysql --debug=off --test=[path_to_lua]  --oltp-test-mode=complex --mysql-host=[ip] --mysql-port=[port] --mysql-db=[db_name] --mysql-password=[password]  --mysql-user=[user] --max-time=50  --max-requests=0 --mysql-table-engine=innodb --oltp-table-size=1000  --oltp-tables-count=10 --num-threads=30 --oltp-read-write --report-interval=10 prepare
```

2、使用chameleon工具进行数据的离线迁移

```shell
chameleon create_replica_schema --config default --debug
chameleon add_source --config default --source mysql --debug
chameleon init_replica --config default --source mysql --debug
```

3、开启online在线迁移

```
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/online-migration-mysql-0809-3.1.0-SNAPSHOT.jar
```

4、sysbench执行run命令，给mysql压测数据(insert模型，30个线程，10张表,1000行数据)

```
numactl -C cpu_num1-cpu_num2 sysbench --db-driver=mysql --debug=off --test=[path_to_lua]  --oltp-test-mode=complex --mysql-host=[ip] --mysql-port=[port] --mysql-db=[db_name] --mysql-password=[password]  --mysql-user=[user] --max-time=50  --max-requests=0 --mysql-table-engine=innodb --oltp-table-size=1000  --oltp-tables-count=10 --num-threads=30 --oltp-read-write --report-interval=10 run
```

5、统计工具日志，得到迁移效率

```
迁移速度 = 总迁移事务 / 迁移耗时
```

### 4.1.2 性能测试

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1. 开启在线迁移，sysbench向mysql压测数据<br />2. 查看online日志，计算迁移性能 | 执行10条用例，执行结果符合预期，测试通过，未发现bug |

### 4.1.3 功能测试

| 测试步骤                                                     | 测试结果                                           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 1. 开启在线迁移，在mysql执行DML语句<br/>2. opengauss端查看数据一致性，有序性，准确性 | 执行2条用例，执行结果符合预期，测试通过，未发现bug |

### 4.1.4 资料测试

| 测试步骤                   | 测试结果                              |
| -------------------------- | ------------------------------------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误，未发现bug |

## 4.2   测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build 4c9602a1 | 12         | Passed: 12<br />Failed: 0 | 0            |

*数据项说明：*

* 累计发现缺陷单0个
* 全部用例执行成功
* 缺陷密度：0

### 4.2.1   sysbench使用不同参数配置，online所测得的平均迁移速度

| lua模型                                         | num-threads | oltp-tables-count | oltp-table-size | max-time | 平均迁移速度(w/tps) |
| ----------------------------------------------- | ----------- | ----------------- | --------------- | -------- | --------------- |
| insert.lua带主键                                | 30          | 10                | 1000            | 50       | 3.447           |
| insert.lua带主键                                | 50          | 10                | 1000            | 50       | 3.934           |
| insert.lua带主键                                | 30          | 50                | 1000            | 50       | 3.216           |
| insert.lua带主键                                | 30          | 10                | 10000           | 50       | 3.496           |
| insert.lua带主键                                | 30          | 10                | 1000            | 100      | 3.294           |
| insert.lua不带主键                              | 30          | 10                | 1000            | 50       | 3.805           |
| update_non_index.lua                            | 30          | 10                | 10000           | 50       | 2.531           |
| update_index.lua                                | 30          | 10                | 10000           | 50       | 2.591           |
| delete.lua                                      | 30          | 10                | 100000          | 5        | 1.8847          |
| 混合(insert-28线程、update-1线程、delete-1线程) | 30          | 10                | 1000            | 50       | 3.380           |

## 4.3    测试结果分析

根据测试数据，分析可得：

除update和delete由于内核的限制，性能测试结果达不到3w tps外，其他性能测试场景下，数据增量迁移性能满足3w tps。

## 4.4   后续测试建议

1、基于不同硬件配置的物理机进行online迁移性能测试

2、基于sysbench自定义lua脚本生成数据，如修改数据类型、数据长度等

# 5     附件

### 5.1 30个线程，10张表，1000行数据，使用insert模型时所测得的迁移速度

sysbench压测mysql对应的性能为49260.88/s

```
numactl -C 121-127 [sysbench_path] --db-driver=mysql --debug=off --test=[lua_path] --oltp-test-mode=complex --mysql-host=[ip] --mysql-port=[port] --mysql-db=[db_name] --mysql-password=[password] --max-time=50  --max-requests=0  --mysql-user=[user] --mysql-table-engine=innodb --oltp-table-size=1000  --oltp-tables-count=10 --num-threads=30 --oltp-read-write --report-interval=10 run

WARNING: the --test option is deprecated. You can pass a script name or path on the command line without any options.
WARNING: --num-threads is deprecated, use --threads instead
WARNING: --max-time is deprecated, use --time instead
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 30
Report intermediate results every 10 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 10s ] thds: 30 tps: 49506.24 qps: 49506.24 (r/w/o: 0.00/49506.24/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 30 tps: 48812.66 qps: 48812.66 (r/w/o: 0.00/48812.66/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 30 tps: 49502.21 qps: 49502.21 (r/w/o: 0.00/49502.21/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 30 tps: 49177.87 qps: 49177.87 (r/w/o: 0.00/49177.87/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 30 tps: 49339.21 qps: 49339.21 (r/w/o: 0.00/49339.21/0.00) lat (ms,95%): 0.83 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           2463522
        other:                           0
        total:                           2463522
    transactions:                        2463522 (49260.88 per sec.)
    queries:                             2463522 (49260.88 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          50.0084s
    total number of events:              2463522

Latency (ms):
         min:                                    0.24
         avg:                                    0.61
         max:                                   19.80
         95th percentile:                        0.83
         sum:                              1498219.30

Threads fairness:
    events (avg/stddev):           82117.4000/518.67
    execution time (avg/stddev):   49.9406/0.00
```

online日志统计迁移效率：(2484286- 2300) /(2022-09-19 14:17:43 - 2022-09-19 14:16:28) = 3.447W tps

```
numactl -C 0-31 -m 0 java -Xms15G -Xmx25G -jar ./target/online-migration-mysql-1.0-SNAPSHOT.jar
Sep 19, 2022 2:16:20 PM com.github.shyiko.mysql.binlog.BinaryLogClient connect
INFO: Connected to 20.20.20.82:3346 at mysql-bin.000035/517723374 (sid:65535, cid:2190)
have replayed 0 transaction, and current time is 2022-09-19 14:16:21.794, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:22.795, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:23.795, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:24.795, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:25.795, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:26.796, and current speed is 0
have replayed 0 transaction, and current time is 2022-09-19 14:16:27.796, and current speed is 0
have replayed 2300 transaction, and current time is 2022-09-19 14:16:28.796, and current speed is 2300
have replayed 25190 transaction, and current time is 2022-09-19 14:16:29.796, and current speed is 22890
have replayed 61796 transaction, and current time is 2022-09-19 14:16:30.797, and current speed is 36606
have replayed 86759 transaction, and current time is 2022-09-19 14:16:31.797, and current speed is 24963
have replayed 112248 transaction, and current time is 2022-09-19 14:16:32.797, and current speed is 25489
have replayed 148889 transaction, and current time is 2022-09-19 14:16:33.797, and current speed is 36638
have replayed 185501 transaction, and current time is 2022-09-19 14:16:34.798, and current speed is 36615
have replayed 220613 transaction, and current time is 2022-09-19 14:16:35.798, and current speed is 35085
have replayed 255987 transaction, and current time is 2022-09-19 14:16:36.798, and current speed is 35401
have replayed 283004 transaction, and current time is 2022-09-19 14:16:37.799, and current speed is 27002
have replayed 320212 transaction, and current time is 2022-09-19 14:16:38.799, and current speed is 37213
have replayed 354667 transaction, and current time is 2022-09-19 14:16:39.799, and current speed is 34464
have replayed 391392 transaction, and current time is 2022-09-19 14:16:40.799, and current speed is 36725
......
have replayed 2119568 transaction, and current time is 2022-09-19 14:17:29.819, and current speed is 37573
have replayed 2157003 transaction, and current time is 2022-09-19 14:17:30.819, and current speed is 37423
have replayed 2194298 transaction, and current time is 2022-09-19 14:17:31.820, and current speed is 37302
have replayed 2231892 transaction, and current time is 2022-09-19 14:17:32.820, and current speed is 37599
have replayed 2261775 transaction, and current time is 2022-09-19 14:17:33.820, and current speed is 29883
have replayed 2295251 transaction, and current time is 2022-09-19 14:17:34.821, and current speed is 33471
have replayed 2333167 transaction, and current time is 2022-09-19 14:17:35.821, and current speed is 37921
have replayed 2367445 transaction, and current time is 2022-09-19 14:17:36.821, and current speed is 34278
have replayed 2404887 transaction, and current time is 2022-09-19 14:17:37.821, and current speed is 37440
have replayed 2442292 transaction, and current time is 2022-09-19 14:17:38.822, and current speed is 37407
have replayed 2480158 transaction, and current time is 2022-09-19 14:17:39.822, and current speed is 37866
have replayed 2484286 transaction, and current time is 2022-09-19 14:17:40.822, and current speed is 4128
have replayed 2484286 transaction, and current time is 2022-09-19 14:17:41.822, and current speed is 0
have replayed 2484286 transaction, and current time is 2022-09-19 14:17:42.823, and current speed is 0
have replayed 2484286 transaction, and current time is 2022-09-19 14:17:43.823, and current speed is 0
```

### 5.2 opengauss和mysql高可用配置示例

5.2.1 MySQL高性能参数配置示例

binlog位置、安装目录、数据目录分别部署在3个不同NVME盘

```
[client]
port=[port]
socket=[mysql.sock_path]

[mysqld]
port=[port]
basedir=[mysql_path]
datadir=[data_path]
pid-file=[mysql.pid_path]
socket=[mysql.sock_path]
log_error=[error.log_path]
server-id=1
max_binlog_size=1G
#ENFORCE_GTID_CONSISTENCY = ON
enforce_gtid_consistency=on
gtid_mode=on
binlog_format= ROW
log_bin = [mysql-bin_path]
binlog_row_image=FULL
bind_address = [ip]
max_allowed_packet=32M


#innodb_page_size=4k

max_connections=2000
back_log=4000
performance_schema=OFF
max_prepared_stmt_count=128000
#transaction_isolation=READ-COMMITTED

#file
innodb_file_per_table
innodb_log_file_size=2048M
innodb_log_files_in_group=32
innodb_open_files=10000
table_open_cache_instances=64

#buffers
innodb_buffer_pool_size=230G
innodb_buffer_pool_instances=16
innodb_log_buffer_size=2048M

#tune
#cpu_affinity=ON
#innodb_numa_interleave=OFF
#innodb_numa_aware=ON
sync_binlog=1
innodb_flush_log_at_trx_commit=1
innodb_use_native_aio=1
innodb_spin_wait_delay=180
innodb_sync_spin_loops=25
innodb_flush_method=O_DIRECT
innodb_io_capacity=30000
innodb_io_capacity_max=40000
innodb_lru_scan_depth=9000
innodb_page_cleaners=16
#innodb_spin_wait_pause_multiplier=25

#perf special
innodb_flush_neighbors=0
innodb_write_io_threads=24
innodb_read_io_threads=16
innodb_purge_threads=32

sql_mode=STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_AUTO_VALUE_ON_ZERO,STRICT_ALL_TABLES

#skip_log_bin
#log_bin=/usr3/qiuying/mysql/mysql/mysql-bin
ssl=0
table_open_cache=30000
max_connect_errors=2000
innodb_adaptive_hash_index=0
skip-name-resolve
```

5.2.2 opengauss高可用配置示例，pg_xlog分盘，postgres.conf添加以下参数

```
max_connections = 4096
allow_concurrent_tuple_update = true
audit_enabled = off
cstore_buffers = 16MB
enable_alarm = off
enable_codegen = false
enable_data_replicate = off
full_page_writes = off
max_files_per_process = 100000
max_prepared_transactions = 2048
shared_buffers = 250GB
use_workload_manager = off
wal_buffers = 1GB
work_mem = 1MB
transaction_isolation = 'read committed'
default_transaction_isolation = 'read committed'
synchronous_commit = on
fsync = on
maintenance_work_mem = 2GB
vacuum_cost_limit = 10000
autovacuum = on
autovacuum_mode = vacuum
autovacuum_max_workers = 20
autovacuum_naptime = 5s
autovacuum_vacuum_cost_delay = 10
update_lockwait_timeout = 20min
enable_mergejoin = off
enable_nestloop = off
enable_hashjoin = off
enable_material = off
wal_log_hints = off
log_duration = off
checkpoint_timeout = 15min
autovacuum_vacuum_scale_factor = 0.1
autovacuum_analyze_scale_factor = 0.02
enable_save_datachanged_timestamp = false
enable_double_write = on
enable_incremental_checkpoint = on
enable_opfusion = on
advance_xlog_file_num = 100
numa_distribute_mode = 'all'
track_activities = on
enable_instr_track_wait = on
enable_instr_rt_percentile = off
track_counts = off
track_sql_count = off
enable_instr_cpu_timer = on
plog_merge_age = 0
session_timeout = 0
enable_instance_metric_persistent = off
enable_logical_io_statistics = off
enable_page_lsn_check = off
enable_user_metric_persistent = off
enable_xlog_prune = off
enable_resource_track = off
instr_unique_sql_count=0
remote_read_mode=non_authentication
wal_level = archive
hot_standby = off
hot_standby_feedback = off
client_min_messages = ERROR
log_min_messages = LOG
enable_asp = off
enable_bbox_dump = off
bgwriter_flush_after = 32
wal_keep_segments = 1025
enable_bitmapscan = off
enable_seqscan = off
enable_thread_pool = on
enable_stmt_track=false
bgwriter_delay = 5s
walwriter_sleep_threshold = 50000
xloginsert_locks=16
pagewriter_sleep = 10ms
incremental_checkpoint_timeout=120s
wal_file_init_num = 30
checkpoint_segments = 100000
gs_clean_timeout =0
track_counts=on
undo_zone_count=0
pagewriter_thread_num = 2
max_redo_log_size=400GB
max_io_capacity=1GB
walwriter_cpu_bind = 2
local_syscache_threshold = 32MB
enable_wdr_snapshot = on
```
