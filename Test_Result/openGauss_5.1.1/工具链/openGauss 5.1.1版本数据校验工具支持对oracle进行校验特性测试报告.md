![avatar](../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述         | 作者   |
| ---------- | ----------- | ---------------- | ------ |
| 2023-12-25 | 1.0         | 特性测试报告初稿 | 薛欣怡 |

 关键词： openGauss、oracle、datakit、gs_datachecker

 

摘要：本文档主要介绍oracle数据库内容迁移到opengauss数据库后，对两边数据库的数据进行校验，确保数据一致性。

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

oracle数据库迁移到opengauss数据库后，利用openGauss数据校验工具 （gs_datachecker）对迁移后的数据进行校验，校验方式包含全量数据校验以及增量数据校验。对于校验出现异常的场景，给出异常场景原因以及修复该场景的参考信息。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| ------------------------------------------------------------ | ------------ | ------------ |
| Oracle 19.3.0.0.0<br/>openGauss 5.1.1 build 79509ee2<br/>kafka_2.12-3.6.1<br/>debezium 1.9.5<br/>gs_datacheck 5.1.1 | 2023-12-14   | 2023-12-26   |

描述特性测试的硬件环境信息

| 环境信息      | 配置信息                                                     | 备注 |
| ------------- | ------------------------------------------------------------ | ---- |
| x86+centOS7.9 | Intel(R) Pentium(R) Gold G5400 CPU @ 3.70GHz 2核<br/>内存：4GB<br/>硬盘：60G<br/>OS：CentOS Linux release 7.9.2009 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

gs_datacheck数据校验工具支持对oracle进行检验特性，共计执行26条用例，主要覆盖了功能测试和可靠性测试。功能测试覆盖范围包括oracle数据库中的数据分别进行全量迁移、增量迁移以及反向迁移至opengauss数据库后，对两库间的数据进行全量校验和增量校验。可靠性测试主要场景为迁移后的数据不同，或人为进行数据构造执行校验，验证校验结果的可靠和准确性。累计发现缺陷单4个，4个缺陷皆已解决，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 源端为oracle，目的端为opengauss，开启全量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | 源端为oracle，目的端为opengauss，开启增量校验，查看校验结果是否准确。测试通过。 |
| 功能测试   | application-sink.yml、application-source.yml、application.yml配置文件中数据校验参数配置验证。测试通过。 |
| 可靠性测试 | 构造会校验失败的数据类型，校验后验证修复信息文件中提供参考的内容。测试通过。 |
| 可靠性测试 | 校验过程中注入各种故障如kill相关进程，校验环境恢复后能否重新进行校验。测试通过。 |
| 性能测试   | 插入1w张表进行校验，收集校验性能数据。由于测试环境无配置环境，万张表场景容易出现kafka响应速度慢于校验工具的查询速度，导致校验失败。最终环境缩减表数量验证多表场景通过，校验速率约1s/张。 |
| 性能测试   | 1张大表中插入50w条数据进行校验，收集校验性能数据，校验速率约4762条/s。 |

## 3.2   约束说明

- 校验的表必须包含主键
- 校验内容差距过大（200行及以上），校验后不会出修复报告，此时建议重新迁移或检查文件配置

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 1    | 3    | 0      |
| 百分比 | 100%     | 0%   | 25%  | 75%  | 0%     |

###  3.3.3 问题汇总

| 序号 | issue号 | 级别 | 问题描述                                                     | 状态   |
| ---- | ------- | ---- | ------------------------------------------------------------ | ------ |
| 1    | I8P9CM  | 主要 | 【测试类型：工具功能】【测试版本：5.1.1】oracle和opengauss数据库中存在同表名不同列名，校验失败后，校验进程不退出 | 已验收 |
| 2    | I8Q6LG  | 次要 | 【测试类型：工具功能】【测试版本：5.1.1】oracle端数据存在null值，迁移完成后，校验失败 | 已验收 |
| 3    | I8Q69V  | 次要 | 【测试类型：工具功能】【测试版本：5.1.1】校验工具未适配RAW类型 | 已验收 |
| 4    | I8QK99  | 次要 | 【测试类型：性能】【测试版本：5.1.1】使用datachecker工具对oracle和opengauss数据库分别对1w张表发起数据抽取，观察到opengauss端串行执行列查询语句时间过长 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1.源端（oracle）插入数据模型并构造数据

该模型包含number、char、nchar、long、raw、timestamp、clob、nclob、xml等数据类型，并执行各种迁移。

CREATE TABLE example_table (
    char_column CHAR(50) primary key,
    nchar_column NCHAR(50),
    varchar2_column VARCHAR2(100),
    nvarchar2_column NVARCHAR2(100),
    number_column NUMBER(10, 2),
    int_number_column NUMBER(10, 0),
    float_column FLOAT,
    bin_float_column BINARY_FLOAT,
    bin_double_column BINARY_DOUBLE,
    long_raw_column LONG RAW,
    raw_column RAW(100),
    date_column DATE,
    timestamp_column TIMESTAMP,
    tz_timestamp_column TIMESTAMP WITH TIME ZONE,
    local_tz_timestamp_column TIMESTAMP WITH LOCAL TIME ZONE,
    clob_column CLOB,
    nclob_column NCLOB,
    blob_column BLOB,
    bfile_column BFILE,
    rowid_column ROWID,
    urowid_column UROWID,
    xmltype_column XMLTYPE
);

### 4.1.2.在校验端机器上执行抽取以及校验进程

启动数据抽取：sh extract-endpoints.sh start

启动数据校验：sh check-endpoint.sh start

### 4.1.3.校验完成后，在check_result目录下观察执行结果

| 结果目录下存在的文件    | 文件用途                                                     |
| ----------------------- | ------------------------------------------------------------ |
| exec_sink_history.log   | 源端抽取到的表清单以及每张表抽取完成的时间                   |
| exec_source_history.log | 目标端抽取到的表清单以及每张表抽取完成的时间                 |
| exec_check_history.log  | 记录已完成校验的表清单以及每张表校验完成的时间               |
| success.log             | 校验进程号、schema和表名称，校验时创建的topic，校验总耗时以及校验开始和结束的时间 |
| failed.log              | 除了与校验成功的表一样的记录信息以外，额外增加校验差异行数收集，校验失败消息(insert=0 update=1 delete=0)，目标端缺少主键集合、不一致主键集合以及多余行主键集合 |
| process.pid             | 源端&目标端&检验段开始和结束抽取&校验的时间                  |
| progress.log            | 记录校验进度信息：每张表抽取类型（全/增量），抽取状态（1未开始/2进行中/3已完成），总表数，已经校验完成的表数，完成校验表大小（M），校验表耗时，校验开始/结束/当前 |
| summary.log             | 记录校验汇总信息：除progress.log中记载的内容外，额外增加目标端多出/丢失表数量，缺失校验表数量，以及上述异常的表清单 |
| repair_TEST_tb_0_1.txt  | 一般命名规则为repair+schemaname+表名开头。内容为校验失败的表提供修复数据使用的SQL内容 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称                                                     | 测试用例数 | 用例执行结果 | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------ | ------------ |
| 源端为oracle，目的端为opengauss，开启全量校验                | 9          | success：6   | 2            |
| 源端为oracle，目的端为opengauss，开启增量校验                | 7          | success：5   | 0            |
| application-sink.yml、application-source.yml、application.yml配置文件中数据校验参数配置验证 | 3          | success：3   | 0            |
| 构造会校验失败的数据类型，校验后验证修复信息文件中提供参考的内容 | 2          | success：1   | 1            |
| 校验过程中注入各种故障如kill相关进程，校验环境恢复后能否重新进行校验 | 5          | success：5   | 0            |
| 插入1000张表进行校验                                         | 1          | success：0   | 1            |
| 表中插入50w行数据进行校验                                    | 1          | success：1   | 0            |



## 4.3   后续测试建议

无

# 5     附件

无
