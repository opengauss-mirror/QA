![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者     |
| ---------- | ----------- | -------- | -------- |
| 2022.12.29 | V1.0        | 初稿     | lihongji |
|            |             |          |          |

 关键词： 

M*、chameleon、openGauss、datachecker、kafka、全量校验性能、70MB/s

 摘要：

本文档内容为验证在使用chameleon工具从M*数据库向openGauss数据库进行全量迁移后，使用datacheck校验工具进行全量数据校验，校验速率满足70MB/s。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

使用chameleon工具从M*数据库向openGauss数据库进行全量数据迁移，迁移后使用datachecker校验工具进行数据校验，校验速率满足70MB/s。

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | :----------- | ------------ |
| openGauss 3.1.0 build 4e931f9a | 2022-11-28   | 2022-12-29   |
| M* 5.7.27                   | 2022-11-28   | 2022-12-29   |
| chameleon 3.0.0                | 2022-11-28   | 2022-12-29   |
| jdk 11.0.16                    | 2022-11-28   | 2022-12-29   |
| kafka 2.13-2.8.11              | 2022-11-28   | 2022-12-29   |
| datachecker master             | 2022-11-28   | 2022-12-29   |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注                     |
| ------------------------ | ------------------------------------------------------------ | ------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 2600MHz <br />内存：1021GB<br />硬盘：7.3TB<br />OS：openEuler release 20.03 (LTS) | openGauss 及宿端抽取服务 |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920  2600MHz<br />内存：765GB<br />硬盘：3.0TB<br />OS：openEuler release 20.03 (LTS-SP1) | MySQL 及源端抽取服务     |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 2600MHz<br />内存：765GB<br />硬盘：7.3TB<br />OS：openEuler release 20.03 (LTS-SP1) | kafka及数据校验服务      |

# 3     测试结论概述

## 3.1   测试整体结论

datachecker全量校验MySQL与openGauss数据的一致性满足校验速度70MB/s特性，共执行用例16个，合计提6个问题单，大数据量（表个数1000以上或数据条数1亿以上）存在瓶颈，整体质量一般。

## 3.2   约束说明

1. M* 5.7.27数据存储分盘
2. openGauss使用默认安装配置，openGauss侧的目标库为兼容B库，除初始用户外的数据库用户均有复制权
3. 表须含主键
4. 仅对数据的差异性及表字段名称的差异性进行数据校验，不对表结构及字段类型的差异性做校验


## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 | 转需求 |
| ------ | :------: | :--: | :--: | :--: | :----: | :----: |
| 数目   |    6     |  0   |  6   |  0   |   0    |   0    |
| 百分比 |   100%   |  0%  | 100% |  0%  |   0%   |   0%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I64UMD](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I64UMD?from=project-issue) | 主要 | 性能基线数据测试，表个数1张，数据条数500w,数据总量约1GB,查看校验结果，数据校验结果不准确 | 已验收 |
| 2    | [I64FX4](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I64FX4?from=project-issue) | 主要 | 表个数511个，每张表数据条数10w个，数据总量14.16G，校验服务卡停 | 已验收 |
| 3    | [I66ODF](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I66ODF?from=project-issue) | 主要 | 数据基线测试，存在部分场景数据校验速度达不到70M/S            | 已验收 |
| 4    | [I67TBG](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I67TBG?from=project-issue) | 主要 | M*侧存在3个以上的库，每个库表个数大于1000个，元数据加载会报错。openGauss抽取服务偶现存在元数据加载报错 | 已完成 |
| 5    | [I67SOG](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I67SOG?from=project-issue) | 主要 | 瓶颈测试。场景一：3张表，每张表1亿数据，生成了1张表的校验结果。场景二：3000张表，每张表10w数据，生成校验结果不完整（应生成3000个结果文件，实际生成1765个）             | 已确认 |
| 6    | [I646UY](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I646UY?from=project-issue) | 主要 | 全量校验满足70M/S没有对应的资料文件，需要说明什么场景下能达到指标，包括硬件、操作系统、数据库配置等 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

1. sysbench批量创造数据至M*，表结构含15个字段

2. 配置chameleon（详见[chameleon使用指南.md](https://gitee.com/opengauss/openGauss-tools-chameleon/blob/master/chameleon%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.md)）

3. 使用chameleon工具将MySQL数据迁移至openGauss

   ```shell
   chameleon drop_replica_schema --config default --debug
   chameleon create_replica_schema --config default --debug
   chameleon add_source --config default --source M* --debug
   chameleon init_replica --config default --source M* --debug
   ```

4. 查看M*待校验的数据库大小

5. 启动M*、openGauss抽取服务及数据校验服务

6. 迁移速度 = 总Size / 时间，其中总Size是M*待校验的数据库大小，时间取自校验服务的cost time

7. 查看生成的结果文件准确性

## 4.2   性能场景

### 4.2.1  基准场景

<center> 表1  基准场景平均校验速率（表含15个字段）
</center>

| 场景 | 场景描述                                       | 平均校验速度(MB/S) | 校验结果 |
| :--: | :--------------------------------------------- | :----------------: | :------: |
|  1   | 1张表，数据条数500万，数据总量约2.19GB         |       97.43        |   准确   |
|  2   | 2张表，每张表数据条数250万，数据总量约2.32GB   |       94.92        |   准确   |
|  3   | 10张表，每张表数据条数50万，数据总量约2.32GB   |       91.44        |   准确   |
|  4   | 40张表，每张表数据条数12.5万，数据总量约2.33GB |       113.48       |   准确   |
|  5   | 200张表，每张表数据条数2.5万，数据总量约2.44GB |       131.74       |   准确   |
|  6   | 250张表，每张表数据条数2万，数据总量约2.32GB   |       108.13       |   准确   |
|  7   | 500张表，每张表数据条数1万，数据总量约2.69GB   |       74.54        |   准确   |

### 4.2.2  瓶颈测试

<center>表2  瓶颈测试平均校验速度
</center>

| 场景 |                  场景描述                  | 平均校验速度(MB/S) |       校验结果        |
| :--: | :----------------------------------------: | :----------------: | :-------------------: |
|  1   | 3张表，每张表1亿数据，数据总量约130G|      校验服务卡停       | 校验结果输出不完整 |
|  2   | 场景二：3000张表，每张表10w数据，数据总量约130G   |   校验服务卡停   |      校验结果输出不完整       |

<center>
</center>

### 4.2.3  资料测试
| 测试步骤                     | 测试结果                            |
| ------------------------------ | ----------------------------------- |
| 1. 使用方法描述<br>2. 示例测试 | 共发现2个bug，现已修复且验收通过 ||

## 4.3   测试结果

### 4.3.1 测试结果分析

本次测试中，以下场景下会出现全量校验性能不满足70MB/s的情况（详见下表）：

| 约束条件                    |      校验速度      |
| :-------------------------- | :----------------: |
| 3张表，表数据行记录>=1亿    |      校验服务卡停       |
| 3000张表，每张表数据条数10w | 校验服务卡停 |

根据测试数据，分析可得：

除上述场景外，本次测试中的基准场景，datachecker数据全量校验性能满足70MB/s。

### 4.3.2 测试数据统计

| 版本名称                       | 测试用例数 | 用例执行结果           | 发现问题单数 |
| ------------------------------ | :--------- | :--------------------- | ------------ |
| openGauss 3.1.0 build 4e931f9a | 16         | Passed:10<br/>Failed:6 | 6            |
| openGauss 3.1.0 build ef6a5318 | 8         | Passed:8<br/>Failed:0 | 0            |

数据说明：

1. datacheck全量校验满足70M/S测试共计16条用例，执行通过14条，共计发现6个bug
2. 以上缺陷2个为资料单，不计入代码缺陷密度。缺陷密度为：4(缺陷个数)/2.969k(代码行数)=1.34(个/kloc)

## 4.4   后续测试建议

1. 待瓶颈修复后构造大数据量场景测试，构造上增加每张表的数据条数，如数据条数大于50w

# 5     附件

## 5.1 校验速率计算

校验速率 =  源端(M*)数据库大小 / 校验时间

源端数据库大小：select concat(round(sum(data_length/1024/1024),2),'MB') as data from information_schema.tables where table_schema='databasename';

校验时间：在校验服务日志check.log日志中打印cost time作为时间统计





