![avatar](https://gitee.com/opengauss/QA/raw/c20a64f26771233d8d2a92c91138cbec5c4967ec/images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期      | 修订 版本 | 修改描述         | 作者        |
| --------- | --------- | ---------------- | ----------- |
| 2023-3-8  | V1.0      | 初稿             | zhanghuan96 |
| 2023-3-16 | V1.1      | 根据评审意见修改 | zhanghuan96 |
| 2023-3-24 | V1.2      | 修改检视意见-测试信息章节 | zhanghuan96 |

关键词：

增量迁移、Debezium+kafka、性能、3w tps

摘要：

本文档主要基于Debezium+kafka开源组件，构建集数据全量迁移(chameleon工具)、实现增量迁移功能，同时增量迁移性能满足3w tps。

缩略语清单：

| 缩略语 | 英文全名                 | 中文解释   |
| ------ | ------------------------ | ---------- |
| `tps`  | `Transaction Per Second` | 每秒事务数 |

# 1 特性概述

本文档主要基于Debezium+kafka开源组件，实现MySQL增量迁移到openGauss，同时增量迁移性能满足3w tps。

# 2 特性测试信息

##### 功能测试

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build c323237f | 2023-2-23    | 2023-3-8     |
| openGauss 5.0.0 build 5a69c469 | 2023-3-8     | 2023-3-9     |
| openGauss 5.0.0 build e94d3b43 | 2023-3-9     | 2023-3-10    |
| openGauss 5.0.0 build 78f2bb65 | 2023-3-16    | 2023-3-16    |
| openGauss 5.0.0 build eacf1d20 | 2023-3-19    | 2023-3-19    |
| dolphin build 2df314c0         | 2023-2-23    | 2023-3-8     |
| dolphin build 4288a62f         | 2023-3-8     | 2023-3-9     |
| dolphin build 1a5da341         | 2023-3-9     | 2023-3-10    |
| dolphin build 0bfcd89f         | 2023-3-16    | 2023-3-16    |
| dolphin build 3aa83d2d         | 2023-3-19    | 2023-3-19    |
| MySQL 5.7.35                   | 2023-2-23    | 2023-3-19    |
| chameleon 3.1.1                | 2023-2-23    | 2023-3-19    |
| Debezium v1.8.1.Final          | 2023-2-23    | 2023-3-19    |

##### 性能测试

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build c3e3237f | 2023-3-4     | 2023-3-13    |
| MySQL 5.7.27                   | 2023-3-4     | 2023-3-13    |
| chameleon 3.1.1                | 2023-3-4     | 2023-3-13    |
| Debezium v1.8.1.Final          | 2023-3-4     | 2023-3-13    |
| sysbench 1.0.20                | 2023-3-4     | 2023-3-13    |

硬件环境信息：

##### 功能测试

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.6.1810 (Core) |      |

##### 性能测试

| 硬件型号                 | 硬件配置信息                                                 | 备注                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64 CPU：kunpeng-920 7260 2p 128核 内存：32 * 32GB 硬盘：3 * 2.9TB OS：openEuler release 20.03 (LTS-SP1) | openGauss 5.0.0 build c3e3237f<br>debezium-connector-MySQL-1.8.1.Final-plugin.tar.gz |
| TaiShan 200 (Model 2280) | Architecture：aarch64 CPU：kunpeng-920 7260 2p 128核 内存：24 * 32GB 硬盘：3 * 2.9TB OS：openEuler release 20.03 (LTS-SP1) | MySQL 5.7.27<br>chameleon 3.1.1                              |
| TaiShan 200 (Model 2280) | Architecture：aarch64 CPU：kunpeng-920 7260 2p 128核 内存：24 * 32GB 硬盘：3 * 2.9TB OS：openEuler release 20.03 (LTS-SP1) | sysbench 1.0.20                                              |

# 3 测试结论概述

## 3.1 测试整体结论

MySQL数据迁移完整方案打通_增量迁移，共执行用例72个，主要覆盖了功能测试、性能测试、安全测试、资料测试。功能测试覆盖开启增量迁移，在MySQL侧执行DDL及DML，使用增量迁移工具迁移到openGauss，保证迁移前后数据的一致性、有序性和完整性。性能测试，在高性能机器下，开启增量迁移，通过sysbench工具给MySQL压测数据，待数据回放完成，计算迁移效率。安全测试关注迁移日志中密码是否加密。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单27个，27个缺陷已解决，回归通过，整体质量良好。

## 3.2 约束说明

1、MySQL 5.7及以上版本

2、MySQL参数设置： log_bin=ON， binlog_format=ROW,，binlog_row_image=FULL,，gtid_mode = ON，若未设置gtid_mode = OFF，则sink端回放退化为串行回放，会降低在线迁移性能

3、增量迁移是直接透传DDL的，对于openGauss侧语法和MySQL侧语法不一样的，迁移失败

4、在线迁移支持的数据类型详见[chameleon使用指南.md#5-默认的类型转换规则](https://gitee.com/opengauss/openGauss-tools-chameleon/blob/master/chameleon%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.md#5-%E9%BB%98%E8%AE%A4%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E8%A7%84%E5%88%99)

5、增量迁移二进制类型时openGauss侧dolphin.b_compatibility_mode参数需设置为on，DML可正常迁移

6、启动增量迁移，可同时启动source端和sink端，也可以分开启动，先启动source端，再启动sink端，confluent/confluent-5.5.1/etc/schema-registry/connect-avro-standalone.properties文件rest.port需配置不同端口

7、为保证事务的顺序性，kafka_2.13-3.2.3/config/server.properties文件num.partitions参数必须设置为1

8、Kafka中以AVRO格式存储数据，AVRO字段名称详见[命名规则](https://gitee.com/link?target=https%3A%2F%2Favro.apache.org%2Fdocs%2F1.11.1%2Fspecification%2F%23names)，不满足命名规范，在线迁移会报错

8、性能测试对硬件系统有较高要求，仅insert和混合场景迁移性能达到3w tps且必须进行性能调优

* mysql高性能配置，binlog位置、安装目录、数据目录分别部署在3个不同的NVME盘
* openGauss侧openGauss高性能配置，pg_xlog、安装目录、数据目录分别部署在3个不同的NVME盘

* mysql-source.properties配置文件parallel.parse.event=true

* 在java11环境上运行在线迁移工具
* 为减少日志刷屏，可设置confluent/confluent-5.5.1/etc/kafka/connect-log4j.properties文件日志级别为WARN

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 27       | 0    | 3    | 24   | 0      |
| 百分比 | 100%     | 0%   | 11%  | 89%  | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   | 备注                                                         |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| 1    | [I6HJDS](https://gitee.com/opengauss/debezium/issues/I6HJDS?from=project-issue) | 次要 | Debezium+kafka增量迁移，M侧表的列名为大小写混合（M侧列名不区分大小写），表结构迁移成功，列名迁移后为小写，但insert数据报错 | 已验收 | dolphin插件适配，工具不做修改                                |
| 2    | [I6G1Z0](https://gitee.com/opengauss/debezium/issues/I6G1Z0?from=project-issue) | 次要 | Debezium+kafka增量迁移，M侧gtid_mode参数设置为off，kafka已采集到，但opgs侧不回放 | 已验收 |                                                              |
| 3    | [I6FXEE](https://gitee.com/opengauss/debezium/issues/I6FXEE?from=project-issue) | 次要 | Debezium+kafka增量迁移，若DDL语句迁移报错，后续执行DML不再同步，但迁移没有停止且无提示阻塞 | 已验收 |                                                              |
| 4    | [I6GJFB](https://gitee.com/opengauss/debezium/issues/I6GJFB?from=project-issue) | 次要 | Debezium+kafka增量迁移数值类型，insert正常，update和delete不同步 | 已验收 |                                                              |
| 5    | [I6IU5G](https://gitee.com/opengauss/debezium/issues/I6IU5G?from=project-issue) | 次要 | Debezium+kafka增量迁移大数据，迁移工具日志刷屏，修改日志级别 | 已验收 |                                                              |
| 6    | [I6HHNU](https://gitee.com/opengauss/debezium/issues/I6HHNU?from=project-issue) | 主要 | Debezium+kafka增量迁移表结构成功，insert语句失败，迁移工具异常报错 | 已验收 | AVRO解析限制                                                 |
| 7    | [I6GN26](https://gitee.com/opengauss/debezium/issues/I6GN26?from=project-issue) | 次要 | Debezium+kafka增量迁移，bigint列含unsigned属性,insert报错    | 已验收 | dolphin插件新增unsigned属性属性，迁移工具没有适配            |
| 8    | [I6GKPX](https://gitee.com/opengauss/debezium/issues/I6GKPX?from=project-issue) | 主要 | Debezium+kafka增量迁移，启动增量迁移后，迁移工具回显信息密码明文显示 | 已验收 |                                                              |
| 9    | [I6G1QK](https://gitee.com/opengauss/debezium/issues/I6G1QK?from=project-issue) | 次要 | Debezium+kafka增量迁移key分区，迁移失败，报错为ddl无法解析   | 已验收 | Debezium原生bug                                              |
| 10   | [I6G1HJ](https://gitee.com/opengauss/debezium/issues/I6G1HJ?from=project-issue) | 次要 | Debezium+kafka增量迁移二进制类型，insert语句迁移失败         | 已验收 | 和变色龙类型映射类型不一样                                   |
| 11   | [I6FZO6](https://gitee.com/opengauss/debezium/issues/I6FZO6?from=project-issue) | 次要 | 使用chameleon全量迁移表成功，再使用Debezium+kafka开启增量迁移，对全量迁移的表数据进行update或delete，不会同步 | 已验收 |                                                              |
| 12   | [I6FVIN](https://gitee.com/opengauss/debezium/issues/I6FVIN?from=project-issue) | 次要 | Debezium+kafka增量迁移，drop多个表，迁移工具报错，表不存在   | 已验收 |                                                              |
| 13   | [I6FNT3](https://gitee.com/opengauss/debezium/issues/I6FNT3?from=project-issue) | 次要 | Debezium+kafka增量迁移，列名带单引号，迁移失败               | 已验收 | dolphin插件新增反引号属性，迁移工具没有适配                  |
| 14   | [I6FCS9](https://gitee.com/opengauss/debezium/issues/I6FCS9?from=project-issue) | 次要 | Debezium+kafka增量迁移，列名有空格，迁移报错                 | 已验收 | AVRO解析限制                                                 |
| 15   | [I6FBFM](https://gitee.com/opengauss/debezium/issues/I6FBFM?from=project-issue) | 次要 | Debezium+kafka增量迁移创建及增加外键，迁移失败               | 已验收 |                                                              |
| 16   | [I6F2MY](https://gitee.com/opengauss/debezium/issues/I6F2MY?from=project-issue) | 次要 | Debezium+kafka增量迁移，使用create index迁移部分类型索引（普通索引、唯一索引），迁移报错，alter table语句添加索引迁移成功 | 已验收 |                                                              |
| 17   | [I6ETOA](https://gitee.com/opengauss/debezium/issues/I6ETOA?from=project-issue) | 次要 | Debezium+kafka增量迁移bit类型，insert报错，update和delete受insert影响，暂无法验证 | 已验收 |                                                              |
| 18   | [I6ESRZ](https://gitee.com/opengauss/debezium/issues/I6ESRZ?from=project-issue) | 次要 | Debezium+kafka增量迁移json类型，insert正常，执行update和delete操作，数据不同步迁移报错 | 已验收 |                                                              |
| 19   | [I6EFZ6](https://gitee.com/opengauss/debezium/issues/I6EFZ6?from=project-issue) | 次要 | Debezium+kafka，开启增量迁移，M*侧执行修改表名操作，DDL语句透传时在表名前加了schema,导致语法错误，迁移失败 | 已验收 | I6F2MY、I6F2MY、I6EF09、I6EDSQ问题根因一致，均为schema.mapping映射问题 |
| 20   | [I6EFL3](https://gitee.com/opengauss/debezium/issues/I6EFL3?from=project-issue) | 次要 | 开启增量迁移，若openGauss侧不支持的语法，迁移页面报错，建议增加具体报错提示 | 已验收 |                                                              |
| 21   | [I6EF09](https://gitee.com/opengauss/debezium/issues/I6EF09?from=project-issue) | 次要 | debezium+kafka增量迁移表结构成功，对增量迁移的表执行insert操作，insert不同步 | 已验收 |                                                              |
| 22   | [I6EDSQ](https://gitee.com/opengauss/debezium/issues/I6EDSQ?from=project-issue) | 次要 | debezium+kafka增量迁移，drop table语句解析报错，迁移失败     | 已验收 |                                                              |
| 23   | [I6KIWQ](https://gitee.com/opengauss/debezium/issues/I6KIWQ?from=project-issue) | 次要 | Debezium+kafka增量迁移，timestamp等时间戳类型，迁移后时间有误 | 已验收 | 和变色龙类型映射类型不一样                                   |
| 24   | [I6K877](https://gitee.com/opengauss/debezium/issues/I6K877?from=project-issue) | 次要 | Debezium+kafka增量迁移，列属性含zerofill属性，insert报错     | 已验收 | Debezium原生bug                                              |
| 25   | [I6JYPN](https://gitee.com/opengauss/debezium/issues/I6JYPN?from=project-issue) | 次要 | Debezium+kafka增量迁移，二进制类型insert成功，update、delete失败 | 已验收 | dolphin插件问题                                              |
| 26   | [I6L4F3](https://gitee.com/opengauss/debezium/issues/I6L4F3?from=project-issue) | 次要 | Debezium+kafka增量迁移，补充相关资料说明                     | 已验收 |                                                              |
| 27   | [I6LTCP](https://gitee.com/opengauss/debezium/issues/I6LTCP?from=project-issue) | 主要 | Debezium+kafka增量迁移混合场景时，迁移性能达不到3W+ tps      | 已验收 |                                                              |

# 4 测试执行

## 4.1.1 功能测试实现步骤

1、MySQL数据库创建表，插入数据

2、使用chameleon工具进行数据的离线迁移

```shell
chameleon create_replica_schema --config default --debug
chameleon add_source --config default --source MySQL --debug
chameleon init_replica --config default --source MySQL --debug
```

3、开启增量迁移

(1)启动zookeeper

```shell
cd kafka_2.13-3.2.3
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
```

(2)启动kafka

```shell
cd kafka_2.13-3.2.3
./bin/kafka-server-start.sh ./config/server.properties
```

(3)注册schema

```shell
cd confluent-5.5.1
./bin/schema-registry-start etc/schema-registry/schema-registry.properties
```

(4)启动kafka-connect

```shell
cd confluent-5.5.1
./bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/kafka/MySQL-source.properties etc/kafka/MySQL-sink.properties
```

4、MySQL数据库执行DDL或DML操作

5、openGauss数据库查看迁移情况

### 4.1.1.1 功能测试-迁移数据类型

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启增量迁移，在MySQL侧建表并执行DML语句，覆盖MySQL侧数据类型<br> 2. opengauss端查看数据一致性，有序性，准确性 | 执行13条用例，执行结果符合预期，测试通过，发现8个bug，8个问题已解决回归通过 |

### 4.1.1.2 功能测试-迁移DDL语句

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启增量迁移，在MySQL侧建表并执行DDL，如在线rename表名、truncate table、drop table、alter table、表的comment、列的comment、create index等<br> 2. opengauss端查看数据一致性，有序性，准确性 | 执行17条用例，执行结果符合预期，测试通过，发现5个bug，已解决并回归通过 |

### 4.1.1.3 功能测试-迁移其他对象

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启增量迁移，在MySQL侧创建视图、存储过程、触发器、自定义函数、分区表 | 执行10条用例，执行结果符合预期，测试通过，发现1个bug，已解决并回归通过 |

### 4.1.1.4 功能测试-配置文件参数测试

| 测试步骤                                      | 测试结果                                            |
| --------------------------------------------- | --------------------------------------------------- |
| 1. 修改source和sink端的配置文件，开启增量迁移 | 执行14条用例，执行结果符合预期，测试通过，未发现bug |

### 4.1.1.5 功能测试-其他测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 修改MYSQL侧的gtid_mode为off；MYSQL生成多个gtid；DDL和DML并发执行 | 执行14条用例，执行结果符合预期，测试通过，发现10个bug，已解决并回归通过 |

## 4.1.2 性能测试实现步骤

1、sysbench执行prepare命令，为mysql端准备数据

2、通过chameleon离线迁移数据至openGauss端

3、 开启在线复制依次启动zookeeper，kafka，注册schema，绑核启动source端，绑核启动sink端

4、 sysbench执行run命令，给mysql压入数据

5、 统计迁移工具日志，得到迁移效率(事务数/时间)

### 4.1.2.1 性能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 开启增量迁移，sysbench向MySQL压测数据<br> 2. 查看迁移日志，计算迁移性能 | 执行4条用例，执行结果符合预期，测试通过，发现1个bug，已解决并回归通过 |

## 4.1.3 安全测试

| 测试步骤                                                    | 测试结果                     |
| ----------------------------------------------------------- | ---------------------------- |
| 1. 开启增量迁移<br> 2. 查看迁移日志、迁移进程中密码是否加密 | 发现1个bug，已解决并回归通过 |

## 4.1.4 资料测试

| 测试步骤                   | 测试结果                                                 |
| -------------------------- | -------------------------------------------------------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误，发现1个bug，已解决并回归通过 |

## 4.2 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 5.0.0 build c323237f | 50         | Passed:35<br/>Failed:15 | 17           |
| openGauss 5.0.0 build 5a69c469 | 30         | Passed:20<br/>Failed: 10    | 10           |
| openGauss 5.0.0 build e94d3b43 | 3          | Passed:3<br/>Failed:0   | 0            |
| openGauss 5.0.0 build eacf1d20 | 3          | Passed:3<br/>Failed:0   | 0            |
| openGauss 5.0.0 build 78f2bb65 | 3          | Passed:3<br/>Failed:0   | 0            |
| openGauss 5.0.0 build eacf1d20 | 1          | Passed:1<br/>Failed:0   | 0            |

*数据项说明：*

- 累计发现缺陷单27个，27个缺陷已解决且回归通过，其中属于迁移工具问题的缺陷为21个，其中1个为资料单
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：20(缺陷个数)/4.465kloc(代码行数)=4.47(个/kloc)

### 4.2.1 sysbench使用不同参数配置，所测得的平均迁移速度

| lua模型                                                     | num-threads | oltp-tables-count | oltp-table-size | max-time | sysbench速度(w tps) | 平均迁移速度(w/tps) |
| ----------------------------------------------------------- | ----------- | ----------------- | --------------- | -------- | ------------------- | ------------------- |
| Insert.lua                                                  | 30          | 10                | 1000            | 50       | 6.515               | 3.126               |
| Update_index.lua                                            | 30          | 10                | 10000           | 50       | 5.049               | 2.137               |
| Update_non_index.lua                                        | 30          | 10                | 10000           | 50       | 5.374               | 2.326               |
| Delete.lua                                                  | 30          | 10                | 100000          | 5        | 3.167               | 1.942               |
| 混合场景(insert-30线程、update-index-10线程、delete-10线程) | 50          | 10                | 100000          | 50       |                     | 3.048               |

混合场景占比：

| 事务总数 | sysbench速率(w tps) | Insert事务数 | Insert速率 | Insert占比 | Update事务数 | Update速率 | Update占比 | Delete事务数 | Delete速率 | Delete占比 |
| -------- | ------------------- | ------------ | ---------- | ---------- | ------------ | ---------- | ---------- | ------------ | ---------- | ---------- |
| 2859520  | 5.719               | 2257800      | 4.516      | 78.95%     | 302257       | 4.240      | 10.57%     | 299463       | 5.036      | 10.47%     |

根据测试数据，分析可得：insert和混合场景增量迁移性能测试结果达到3w tps以上。

## 4.3 后续测试建议

1、基于不同硬件配置的物理机进行增量迁移性能测试

2、基于sysbench自定义lua脚本生成数据，如修改数据类型、数据长度等

# 5 附件

### 5.1 30个线程，10张表，1000行数据，使用insert模型测试迁移效率

```shell
1.sysbench执行prepare命令，为mysql端准备数据
numactl -C 121-127 sysbench --db-driver=MySQL --debug=off --test=xxx/insert.lua  --oltp-test-mode=complex --MySQL-host=xx.xx.xx.xx --MySQL-port=3396 --MySQL-db=source_testdb --MySQL-password=xxx --max-time=50  --max-requests=0  --MySQL-user=xxx --MySQL-table-engine=innodb --oltp-table-size=1000  --oltp-tables-count=10 --num-threads=30 --oltp-read-write --report-interval=1 prepare
2.通过chameleon离线迁移数据至openGauss端
chameleon init_replica --config default_zh --source MySQL --debug
3.开启增量迁移
--绑核启动source端
numactl -C 32-63 -m 0 ./bin/connect-standalone etc/schema-registry/connect-avro-standalone.properties etc/kafka/MySQL-source.properties
--绑核启动sink端
numactl -C 64-95 -m 0 ./bin/connect-standalone etc/schema-registry/connect-avro-standalone-zh.properties etc/kafka/MySQL-sink.properties
4.sysbench执行run命令，给mysql压入数据
numactl -C 121-127 sysbench --db-driver=MySQL --debug=off --test=xxx/insert.lua  --oltp-test-mode=complex --MySQL-host=xx.xx.xx.xx --MySQL-port=3396 --MySQL-db=source_testdb --MySQL-password=xxx --max-time=50  --max-requests=0  --MySQL-user=usr_replica --MySQL-table-engine=innodb --oltp-table-size=1000  --oltp-tables-count=10 --num-threads=30 --oltp-read-write --report-interval=1 run
Initializing worker threads...

Threads started!

[ 10s ] thds: 30 tps: 65469.95 qps: 65469.95 (r/w/o: 0.00/65469.95/0.00) lat (ms,95%): 0.58 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 30 tps: 65111.81 qps: 65111.91 (r/w/o: 0.00/65111.91/0.00) lat (ms,95%): 0.58 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 30 tps: 65074.50 qps: 65074.40 (r/w/o: 0.00/65074.40/0.00) lat (ms,95%): 0.58 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 30 tps: 65141.10 qps: 65141.10 (r/w/o: 0.00/65141.10/0.00) lat (ms,95%): 0.58 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 30 tps: 65008.05 qps: 65008.05 (r/w/o: 0.00/65008.05/0.00) lat (ms,95%): 0.58 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           3258220
        other:                           0
        total:                           3258220
    transactions:                        3258220 (65156.27 per sec.)
    queries:                             3258220 (65156.27 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          50.0047s
    total number of events:              3258220

Latency (ms):
         min:                                    0.21
         avg:                                    0.46
         max:                                   10.36
         95th percentile:                        0.58
         sum:                              1497577.31

Threads fairness:
    events (avg/stddev):           108607.3333/1738.55
    execution time (avg/stddev):   49.9192/0.00
5.source端解析的事务：
WARNING: have parsed 2996675 GtidEvent, and current time is 2023-03-13 15:48:13.052, and current speed is 41213
Mar 13, 2023 3:48:14 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3037830 GtidEvent, and current time is 2023-03-13 15:48:14.052, and current speed is 41142
Mar 13, 2023 3:48:15 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3078144 GtidEvent, and current time is 2023-03-13 15:48:15.052, and current speed is 40304
Mar 13, 2023 3:48:16 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3118365 GtidEvent, and current time is 2023-03-13 15:48:16.053, and current speed is 40221
Mar 13, 2023 3:48:17 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3158699 GtidEvent, and current time is 2023-03-13 15:48:17.053, and current speed is 40323
Mar 13, 2023 3:48:18 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3198898 GtidEvent, and current time is 2023-03-13 15:48:18.053, and current speed is 40193
Mar 13, 2023 3:48:19 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3239191 GtidEvent, and current time is 2023-03-13 15:48:19.053, and current speed is 40282
Mar 13, 2023 3:48:20 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3258220 GtidEvent, and current time is 2023-03-13 15:48:20.053, and current speed is 19020
Mar 13, 2023 3:48:21 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3258220 GtidEvent, and current time is 2023-03-13 15:48:21.053, and current speed is 0
Mar 13, 2023 3:48:22 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3258220 GtidEvent, and current time is 2023-03-13 15:48:22.053, and current speed is 0
Mar 13, 2023 3:48:23 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3258220 GtidEvent, and current time is 2023-03-13 15:48:23.053, and current speed is 0
Mar 13, 2023 3:48:24 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
WARNING: have parsed 3258220 GtidEvent, and current time is 2023-03-13 15:48:24.053, and current speed is 0
Mar 13, 2023 3:48:25 PM com.github.shyiko.mysql.binlog.BinaryLogClient$8 run
6.sink端回放的事务：
[2023-03-13 15:48:31,095] WARN have replayed 3028494 transaction, and current time is 2023-03-13 15:48:31.094, and current speed is 35371 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:31,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:31.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:32,094] WARN have replayed 3061640 transaction, and current time is 2023-03-13 15:48:32.094, and current speed is 33131 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:32,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:32.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:33,094] WARN have replayed 3096601 transaction, and current time is 2023-03-13 15:48:33.094, and current speed is 34938 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:33,096] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:33.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:34,095] WARN have replayed 3131636 transaction, and current time is 2023-03-13 15:48:34.094, and current speed is 35017 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:34,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:34.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:35,094] WARN have replayed 3160393 transaction, and current time is 2023-03-13 15:48:35.094, and current speed is 28748 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:35,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:35.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:36,094] WARN have replayed 3194925 transaction, and current time is 2023-03-13 15:48:36.094, and current speed is 34532 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:36,096] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:36.096, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:37,095] WARN have replayed 3230116 transaction, and current time is 2023-03-13 15:48:37.094, and current speed is 35190 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:37,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:37.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:38,094] WARN have replayed 3258220 transaction, and current time is 2023-03-13 15:48:38.094, and current speed is 28099 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:38,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:38.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:39,094] WARN have replayed 3258220 transaction, and current time is 2023-03-13 15:48:39.094, and current speed is 0 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:39,096] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:39.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:40,094] WARN have replayed 3258220 transaction, and current time is 2023-03-13 15:48:40.094, and current speed is 0 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:40,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:40.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:41,094] WARN have replayed 3258220 transaction, and current time is 2023-03-13 15:48:41.094, and current speed is 0 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
[2023-03-13 15:48:41,095] WARN have constructed 3258220 transaction, and current time is 2023-03-13 15:48:41.095, and current speed is 0 (io.debezium.connector.mysql.sink.replay.JdbcDbWriter:420)
[2023-03-13 15:48:42,094] WARN have replayed 3258220 transaction, and current time is 2023-03-13 15:48:42.094, and current speed is 0 (io.debezium.connector.mysql.sink.replay.TransactionDispatcher:144)
7.计算迁移效率：
(3061640 -60960)/(1min+36s)=3.126w tps
```