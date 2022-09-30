![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者    |
| ---------- | ----------- | -------- | ------- |
| 2022.09.22 | V1.0        | 初稿     | qiuying |
|    2022.9.29        |    V1.1         |   根据评审意见修改       |    qiuying     |

 关键词： 

反向迁移、性能、3W tps

 摘要：

本文档内容为验证mysql在线迁移到openGauss之后，支持反向迁移，把数据从openGauss侧迁移到MySQL侧，insert反向迁移性能达到3w tps。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| `tps`       | `Transaction Per Second` | 每秒事务数                                       |

# 1     特性概述

MySQL在线迁移到openGauss之后，支持反向迁移，把数据从openGauss侧迁移到MySQL侧，insert反向迁移性能达到3w tps。

insert场景如下：使用sysbench向mysql压测数据，当oltp-table-size = 10000、oltp-tables-count =100、num-threads=100、使用insert模型时，反向迁移性能满足3w tps。

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                                 | 测试起始时间 | 测试结束时间 |
| ---------------------------------------- | :----------- | ------------ |
| openGauss 3.1.0 build 4c9602a1           | 2022-09-11   | 2022-9-22    |
| MySQL 5.7.27                             | 2022-09-11   | 2022-9-22    |
| chameleon 3.0.0                          | 2022-09-11   | 2022-9-22    |
| sysbench 1.0.20                          | 2022-09-11   | 2022-9-22    |
| reverse-migration-mysql-1.0-SNAPSHOT.jar | 2022-09-11   | 2022-9-22    |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：32 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | openGauss 3.1.0 build 4c9602a1<br />reverse-migration-mysql-1.0-SNAPSHOT.jar |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | MySQL 5.7.27<br/>chameleon 3.0.0                             |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 7260 2p 128核<br />内存：24 * 32GB<br />硬盘：3 * 2.9TB<br />OS：openEuler release 20.03 (LTS-SP1) | sysbench 1.0.20                                              |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss侧的增量数据反向迁移到MySQL，且性能满足3w tps特性，共执行用例21个，主要覆盖了功能测试、性能测试和资料测试。功能测试覆盖在opengauss数据库分别执行增删改操作，使用反向迁移工具迁移上述DML操作到MySQL，保证迁移前后数据的一致性、有序性和完整性。资料测试覆盖校验资料的描述及示例的执行结果是否成功。累计发现缺陷单2个，1个缺陷已解决，回归通过，剩余1个缺陷暂未解决，整体质量良好。

## 3.2   约束说明

1、MySQL 5.7.27版本，数据库除初始用户外的数据库用户均有复制权；配置文件添加以下参数：

MySQL参数设置： log_bin=ON, binlog_format=ROW, binlog_row_image=FULL, gtid_mode = ON

2、openGauss使用高性能配置部署，绑核启动，openGauss侧的目标库为兼容B库，除初始用户外的数据库用户均有复制权。

迁移前需要开启数据库的逻辑复制相关配置：wal_level = logical，无主键的表需要执行：alter table tablename replica identity full

3、chameleon工具已安装并完成配置

4、sysbench已安装

5、反向迁移工具已编译jar包

6、仅insert迁移性能达到3w tps

7、仅支持从openGauss迁移至MySQL，支持DML迁移，不支持DDL迁移

8、仅支持在线迁移，不支持离线全量迁移

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2       | 0    | 0    | 2    | 0      |
| 百分比 |  100%        | 0%   | 0%   | 100%  | 0%    |

### 3.3.3 问题单汇总

|序号| issue号                                                      | 级别 | 问题简述                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------- | ------ |
| 1    | [I5SDIF](https://gitee.com/opengauss/CM/issues/I5SDIF?from=project-issue) | 次要 | 启动反向迁移工具时，指定错误参数时提示不合理 | 已验收 |
| 2 | [I5SBS4](https://gitee.com/opengauss/CM/issues/I5SBS4?from=project-issue) | 次要 | chameleon drop_replic_schema无法停掉迁移进程 | 待办的 |

# 4     测试执行

## 4.1.1   功能测试实现步骤

1、mysql数据库创建表，插入数据

2、使用chameleon工具进行数据的离线迁移

```shell
chameleon create_replica_schema --config default --debug
chameleon add_source --config default --source mysql --debug
chameleon init_replica --config default --source mysql --debug
```

3、开启反向迁移

```
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar drop
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar create
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar start
```

4、opengauss数据库执行DML操作

5、mysql查看表数据

## 4.1.1   性能测试实现步骤

1、借助sysbench工具在MySQL和opengauss的目标库中生成数据

``` shell
numactl -C 121-127 [path_to_sysbench] --db-driver=mysql --debug=off --test=[path_to_lua] --oltp-test-mode=complex --mysql-host=[ip] --mysql-port=[port] --mysql-db=[db_name] --mysql-password=[password] --max-time=50  --max-requests=0  --mysql-user=[user] --mysql-table-engine=MyISAM/innodb --oltp-table-size=10000  --oltp-tables-count=100  --num-threads=100 --oltp-read-write --report-interval=10 prepare
numactl -C 121-127 [path_to_sysbench] --db-driver=pgsql [path_to_lua] --pgsql-host=[ip] --pgsql-port=[port] --pgsql-db=[db_name] --pgsql-password=[password]  --pgsql-user=[user] --oltp-table-size=10000  --oltp-tables-count=100 --num-threads=100 --max-time=50 --report-interval=10 prepare
```

2、开启反向迁移

```
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar drop
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar create
numactl -C 0-31 -m 0 java -Xms10G -Xmx15G -jar ./target/reverse-migration-mysql-1.0-SNAPSHOT.jar start
```

3、sysbench执行run命令，给opengauss压测数据(insert模型，100个线程，100张表,10000行数据)

```
numactl -C 121-127 [path_to_sysbench] --db-driver=pgsql [path_to_lua] --pgsql-host=[ip] --pgsql-port=[port] --pgsql-db=[db_name] --pgsql-password=[password]  --pgsql-user=[user] --oltp-table-size=10000  --oltp-tables-count=100 --num-threads=100 --max-time=50 --report-interval=10 run
```

4、计算迁移效率：每秒查看一次mysql的执行总数据量，计算每秒的数据差值

```
select sum(table_rows) from information_schema.tables where table_schema='onlintest' ;	
```

### 4.1.2 功能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启反向迁移，在opengauss执行DML语句<br/>2. opengauss端查看数据一致性，有序性，准确性 | 执行17条用例，执行结果符合预期，测试通过，发现2个bug，1个问题已解决回归通过，1个暂未解决 |

### 4.1.3 性能测试

| 测试步骤                                                     | 测试结果                                           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 1. 开启反向迁移，sysbench向opengauss压测数据<br />2. 查看mysql表的总行数，计算迁移性能 | 执行4条用例，执行结果符合预期，测试通过，未发现bug |

### 4.1.4 资料测试

| 测试步骤                   | 测试结果                              |
| -------------------------- | ------------------------------------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误，未发现bug |

## 4.2   测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build 4c9602a1 | 21         | Passed: 19<br />Failed: 2 | 2            |

*数据项说明：*

* 累计发现缺陷单2个，1个缺陷已解决且回归通过，剩余1个未解决
* 剩余1个失败用例待缺陷解决后回归，其余失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：2(缺陷个数)/0.444kloc(代码行数)=4.50(个/kloc)

### 4.2.1   sysbench使用不同参数配置，所测得的平均迁移速度

| lua模型    | num-threads | oltp-tables-count | oltp-table-size | max-time | 平均迁移速度(w/tps) |
| ---------- | ----------- | ----------------- | --------------- | -------- | --------------- |
| insert.lua | 100         | 100               | 10000           | 50       | 3.551043        |
| insert.lua | 150         | 100               | 10000           | 50       | 3.529478        |
| insert.lua | 100         | 150               | 10000           | 50       | 3.457414        |
| delete.lua | 100         | 100               | 10000           | 50       | 1.8847          |

根据测试数据，分析可得：insert反向迁移性能测试结果达到3w tps以上。

## 4.4   后续测试建议

1、基于不同硬件配置的物理机进行反向迁移性能测试

2、基于sysbench自定义lua脚本生成数据，如修改数据类型、数据长度等

# 5     附件

### 5.1 100个线程，100张表，10000行数据，使用insert模型时所测得的迁移速度

sysbench压测opengauss对应的性能为36256.73/s

```
numactl -C 121-127 [path_to_sysbench] --db-driver=pgsql [path_to_lua] --pgsql-host=[ip] --pgsql-port=[port] --pgsql-db=[db_name] --pgsql-password=[password]  --pgsql-user=[user] --oltp-table-size=10000  --oltp-tables-count=100 --num-threads=100 --max-time=50 --report-interval=10 run
WARNING: --num-threads is deprecated, use --threads instead
WARNING: --max-time is deprecated, use --time instead
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 100
Report intermediate results every 10 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 10s ] thds: 100 tps: 36190.55 qps: 36190.55 (r/w/o: 0.00/36190.55/0.00) lat (ms,95%): 3.75 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 100 tps: 36522.81 qps: 36522.91 (r/w/o: 0.00/36522.91/0.00) lat (ms,95%): 3.82 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 100 tps: 36313.61 qps: 36313.51 (r/w/o: 0.00/36313.51/0.00) lat (ms,95%): 4.03 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 100 tps: 36018.91 qps: 36018.91 (r/w/o: 0.00/36018.91/0.00) lat (ms,95%): 4.10 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 100 tps: 36292.07 qps: 36292.07 (r/w/o: 0.00/36292.07/0.00) lat (ms,95%): 4.10 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           1813550
        other:                           0
        total:                           1813550
    transactions:                        1813550 (36256.73 per sec.)
    queries:                             1813550 (36256.73 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          50.0180s
    total number of events:              1813550

Latency (ms):
         min:                                    1.57
         avg:                                    2.76
         max:                                   68.72
         95th percentile:                        4.03
         sum:                              4998614.41

Threads fairness:
    events (avg/stddev):           18135.5000/53.33
    execution time (avg/stddev):   49.9861/0.00
```

反向迁移效率：每秒查询一次表的总行数，计算前后差的平均值(3.55w tps)

```
2022/9/22 19:28:47.2847.	1414851
2022/9/22 19:28:48.2848.	1414851
2022/9/22 19:28:49.2849.	1414851
2022/9/22 19:28:50.2850.	1414851
2022/9/22 19:28:51.2851.	1414851
2022/9/22 19:28:52.2852.	1414851
2022/9/22 19:28:53.2853.	1422260
2022/9/22 19:28:54.2854.	1458691
2022/9/22 19:28:55.2855.	1496482
2022/9/22 19:28:56.2856.	1527283
2022/9/22 19:28:57.2857.	1564100
2022/9/22 19:28:58.2858.	1601948
2022/9/22 19:28:59.2859.	1638495
2022/9/22 19:29:00.290.		1676524
2022/9/22 19:29:01.291.		1712956
2022/9/22 19:29:02.292.		1749271
2022/9/22 19:29:03.293.		1786886
2022/9/22 19:29:04.294.		1823696
2022/9/22 19:29:05.295.		1860972
2022/9/22 19:29:06.296.		1898063
2022/9/22 19:29:07.297.		1935475
2022/9/22 19:29:08.298.		1972638
2022/9/22 19:29:09.299.		2009550
2022/9/22 19:29:10.2910.	2046795
2022/9/22 19:29:11.2911.	2082961
2022/9/22 19:29:12.2912.	2119792
2022/9/22 19:29:13.2913.	2154309
2022/9/22 19:29:14.2914.	2191338
2022/9/22 19:29:15.2915.	2226925
2022/9/22 19:29:16.2916.	2264331
2022/9/22 19:29:17.2917.	2300172
2022/9/22 19:29:18.2918.	2336282
2022/9/22 19:29:19.2919.	2374023
2022/9/22 19:29:20.2920.	2409441
2022/9/22 19:29:21.2921.	2448450
2022/9/22 19:29:22.2922.	2484956
2022/9/22 19:29:23.2923.	2521496
2022/9/22 19:29:24.2924.	2557007
2022/9/22 19:29:25.2925.	2592537
2022/9/22 19:29:26.2926.	2630213
2022/9/22 19:29:27.2927.	2666933
2022/9/22 19:29:28.2928.	2702005
2022/9/22 19:29:29.2929.	2738802
2022/9/22 19:29:30.2930.	2775147
2022/9/22 19:29:31.2931.	2811629
2022/9/22 19:29:32.2932.	2847985
2022/9/22 19:29:33.2933.	2884718
2022/9/22 19:29:34.2934.	2921454
2022/9/22 19:29:35.2935.	2957862
2022/9/22 19:29:36.2936.	2994823
2022/9/22 19:29:37.2937.	3030969
2022/9/22 19:29:38.2938.	3068095
2022/9/22 19:29:39.2939.	3103659
2022/9/22 19:29:40.2940.	3138804
2022/9/22 19:29:41.2941.	3176358
2022/9/22 19:29:42.2942.	3212573

```
