![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期     | 修订   版本 | 修改描述           | 作者        |
| :------- | ----------- | ------------------ | ----------- |
| 2023-7-4 | 1.0         | 特性测试报告初稿完 | yangyixiang |

 关键词： 

datacheck、全量校验、增量校验

摘要：

本文档主要介绍数据校验工具源端是openGauss、目的端是MySQL以及源端和目的端是openGauss的全量校验和增量校验测试结果，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

针对数据校验过程中源端是openGauss的情形，检查源端和宿端的增量数据和全量数据是否一致；对于出现异常的数据，输出异常结果信息，并给出修复参考信息。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| M* 5.7.35<br/>openGauss 5.0.0 build b82d5699<br/>chameleon 5.0.0<br>debezium 5.0.0<br>gs_datacheck 5.0.0 | 2023-6-19  | 2023-6-29 |
| M* 5.7.35<br/>openGauss 5.0.0 build b82d5699<br/>openGauss 5.1.0 build ad27d72a<br>chameleon 5.0.0<br/>debezium 5.0.0<br/>gs_datacheck 5.0.0 | 2023-6-30   | 2023-6-30 |

| 硬件型号   | 硬件配置信息                                                 | 备注 |
| ---------- | ------------------------------------------------------------ | ---- |
| x86+centOS | Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz 8核<br/>内存：31GB<br/>硬盘：724G<br/>OS：CentOS Linux release 7.6.1908 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

gs_datacheck数据校验工具支持源端和目的端都是openGauss的校验特性，共计执行66条用例，主要覆盖了功能测试和资料测试。功能测试覆盖源端为openGauss、目的端为M*以及源端和目的端均为openGauss的全量校验和增量校验，覆盖数据校验工具配置文件校验参数验证。可靠性测试覆盖校验中注入故障、kill校验相关进程。资料测试覆盖校验资料的描述与实际的功能是否一致，是否可按照示例正确执行校验。累计发现缺陷单6个，6个缺陷已解决且回归通过，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | :----------------------------------------------------------- |
| 功能测试   | 源端为openGauss，目的端为M*，开启全量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | 源端和目的端均为openGauss，开启全量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | 源端为openGauss，目的端为M*，开启增量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | 源端和目的端均为openGauss，开启增量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | application-sink.yml、application-source.yml、application.yml配置文件中数据校验参数配置验证。测试通过。 |
| 可靠性测试 | 校验过程中注入故障、kill相关进程，校验是否正常执行或反馈相关异常。测试通过。 |
| 资料测试   | 资料对于新增加的功能描述准确，与实际功能保持一致，可按照示例正常执行。测试通过。 |

## 3.2   约束说明

- 不支持无主键表的数据校验
- jdk8及以上版本
- openGauss3.0.0以上版本
- debezium+kafka反向迁移源端环境
- openGauss库为B库

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |  |          |  |  |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6    | 0    | 3  | 3 | 0      |
| 百分比 | 100%         | 0%   | 50% | 50% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I7EK1E](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7EK1E?from=project-issue) | 次要     | 支持源端和目的端都是openGauss的校验，需同时支持全量校验和增量校验，资料未更新 | 已验收   |
| 2    | [I7EUAH](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7EUAH?from=project-issue) | 次要     | 全量校验，校验结束后，查看check.log日志delete topic报错      | 已验收   |
| 3    | [I7EZ28](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7EZ28?from=project-issue) | 次要     | 全量校验源端和目的端均为openGauss，字段类型包含clob类型和xml类型，实际数据一致，校验不通过，修复语句不正确 | 已验收   |
| 4    | [I7F0AJ](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7F0AJ?from=project-issue) | 次要     | 全量校验源端和目的端均为openGauss，字段类型包含bytea类型，修复语句不正确 | 已验收   |
| 5    | [I7F5NW](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7F5NW?from=project-issue) | 次要     | 全量校验源端和目的端均为openGauss，字段类型包含文本搜索类型，修复语句不正确 | 已验收   |
| 6    | [I6R0HN](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I6R0HN?from=project-issue) | 次要     | 全量数据校验工具，校验结束偶现不产生修复报告文件 2.bit类型修复语句不准确 | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 源端为openGauss、目的端为M*的全量校验

| 测试步骤：                                                   | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1. M*预制数据，使用chameleon全量迁移至openGauss，开启全量校验。数据类型覆盖字符、日期时间、浮点与整型、unsigned、二进制、枚举、集合、布尔、json、位，覆盖验证校验success、校验failed(insert、update、delete、表缺失)场景，覆盖特殊场景同名大小写表、空格、空字符、null。 | 执行17条用例，发现2个bug，2个未修复 |

### 4.1.2 源端、目的端均为openGauss的全量校验

| 测试步骤：                                                   | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1. openGauss预制数据，使用gs_dumpall导出数据后恢复至另一openGauss，开启全量校验。数据类型覆盖数值类型、货币类型、布尔类型、字符类型、二进制类型、日期/时间类型、几何类型、网络地址类型、位串类型、文本搜索类型、UUID类型、JSON/JSONB类型、HLL数据类型、范围类型、对象标识符类型、XML类型、账本数据库HASH类型、SET类型，表类型覆盖分区表、列存表、外表，覆盖验证校验success、校验failed(insert、update、delete、表缺失)场景，覆盖特殊场景同名大小写表、空格、空字符、null。 | 执行21条用例，发现3个bug，2个未修复 |

### 4.1.3 源端为openGauss、目的端为M*的增量校验

| 测试步骤：                                                   | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. M*侧和openGauss侧创建表，开启反向迁移，开启增量校验，openGauss侧插入数据，查看增量校验结果，数据类型覆盖字符类型、日期时间类型、浮点与整型、unsigned类型、二进制类型、枚举类型、集合类型、布尔类型、json类型、位类型，覆盖验证校验success、校验failed(insert、update、delete、表缺失)场景。 | 执行10条用例，未发现bug |

### 4.1.4 源端、目的端均为openGauss的全量校验

| 测试步骤：                                                   | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 开启openGauss到M*的反向迁移，再开启MySQL到另一openGauss的增量迁移，在三个库中创建表，开启增量校验，在source 端的openGauss插入数据，查看校验结果。数据类型覆盖字符类型、日期时间类型、浮点与整型、unsigned类型、二进制类型、枚举类型、集合类型、布尔类型、json类型、位类型，覆盖验证校验success、校验failed(insert、update、delete、表缺失)场景。 | 执行10条用例，未发现bug |

### 4.1.5 参数配置验证

| 测试步骤：                                                   | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1.验证源端和目的端的参数配置，覆盖databaseType、schema参数、url、用户名、密码。 | 执行4条用例，未发现bug |

### 4.1.6 可靠性测试

| 测试步骤：                                                   | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1.在校验过程中注入故障，覆盖cpu占80%故障、磁盘80%故障、网络延时故障，查看迁移是否正常进行。<br>2.在校验过程中kill进程，覆盖kill校验工具进程、kill kafka进程、kill数据库进程、kill迁移工具进程，查看校验日志和校验结果是否有对应报错提示。 | 执行4条用例，未发现bug |

### 4.1.7 资料测试

| 测试步骤：                                                   | 测试结果                       |
| ------------------------------------------------------------ | ------------------------------ |
| 1.资料对于新增加的功能描述准确，与实际功能保持一致，可按照示例正常执行。 | 发现1个bug，现已修复且验收通过 |

## 4.2  测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ----------------------- | ------------ |
| M* 5.7.35<br/>openGauss 5.0.0 build b82d5699<br/>chameleon 5.0.0<br/>debezium 5.0.0<br/>gs_datacheck 5.0.0 | 66         | Passed：58<br>Failed：8 | 6            |
| M* 5.7.35<br/>openGauss 5.0.0 build b82d5699<br/>openGauss 5.1.0 build ad27d72a<br/>chameleon 5.0.0<br/>debezium 5.0.0<br/>gs_datacheck 5.0.0 | 8          | Passed：8<br>Failed：0  | 0            |

*数据项说明：*

* 累计发现缺陷单6个，1个为资料单，6个缺陷均已解决且回归通过
* 缺陷密度为5个(缺陷个数)/12.83kloc(代码行数)=0.39(个/kloc)

## 4.3   后续测试建议

无

# 5     附件

##  5.1 源端及目的端配置为openGauss

```sql
server:
  port: 9002
logging:
  config: config/log4j2sink.xml
spring:
  check:
    server-uri: http://127.0.0.1:9000
    core-pool-size: 1
    maximum-pool-size: 5
    maximum-topic-size: 5
    maximum-table-slice-size: 100000
  extract:
    schema: test
    databaseType: OG
    query-dop: 8 # jdbc Parallel Query  config
    debezium-enable: false # no need config,but not delete
    debezium-topic:  # no need config,but not delete
    debezium-groupId: # no need config,but not delete
    debezium-serializer: AvroSerializer # StringSerializer or AvroSerializer
    debezium-avro-registry: http://localhost:8081 # avro schema registry
    debezium-time-period: 1 # no need config,but not delete
    debezium-num-period: 1000 # no need config,but not delete
  kafka:
    bootstrap-servers: localhost:9092
  datasource:
    druid:
      dataSourceOne:
        driver-class-name: org.opengauss.Driver
        url: jdbc:opengauss://127.0.0.1:5432/postgres?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
        username: test
        password: 'xxxx'  # The password text may contain special characters, which need to be enclosed in quotation marks
        # Configure initialization connection pool size, minimum number of connections, and maximum number of connections
        # Users can make appropriate adjustments according to the number of current database tables
        initialSize: 5 # initialization connection pool size
        minIdle: 10 # minimum number of connections
        maxActive: 20 # maximum number of connections
```

 



 

 
