![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者     |
| ---------- | ----------- | -------- | -------- |
| 2023.06.08 | V1.0        | 初稿     | lihongji |


 关键词： 

M*、chameleon、openGauss、gs_datachecker、kafka、全量校验性能、150MB/s、20w条/s

 摘要：

本文档内容为验证在使用chameleon工具从M\*数据库向openGauss数据库进行全量迁移后，数据量满足50张表，每张表1000万条数据，在特定表结构(详见测试步骤)下使用gs_datachecker校验工具进行全量数据校验，校验速率满足150MB/s、20w条/s。


缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

gs_datachecker校验工具全量数据校验，校验速率满足150MB/s、20w条/s。

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | :----------- | ------------ |
| openGauss 5.1.0 build 68d1772f | 2023-05-26   | 2023-08-20   |
| M* 5.7.27                      | 2022-05-26   | 2023-08-20   |
| chameleon 5.0.0                | 2022-05-26   | 2023-08-20   |
| jdk 11.0.16                    | 2022-05-26   | 2023-08-20   |
| kafka 2.13-2.8.11              | 2022-05-26   | 2023-08-20   |
| gs_datachecker master          | 2022-05-26   | 2023-08-20   |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注                     |
| ------------------------ | ------------------------------------------------------------ | ------------------------ |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 2600MHz <br />内存：1021GB<br />硬盘：7.3TB<br />OS：openEuler release 20.03 (LTS) | openGauss 及宿端抽取服务 |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920  2600MHz<br />内存：765GB<br />硬盘：3.0TB<br />OS：openEuler release 20.03 (LTS-SP1) | M* 及源端抽取服务        |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br />CPU：kunpeng-920 2600MHz<br />内存：765GB<br />硬盘：7.3TB<br />OS：openEuler release 20.03 (LTS-SP1) | kafka及数据校验服务      |

# 3     测试结论概述

## 3.1   测试整体结论

​基于sysbench自定义表结构数据预置。通过更改sysbench的lua脚本，使用自定义表结构对M*数据库预置50张表，每张表1000万条数据。使用chameleon全量迁移工具将数据从M\*迁移至openGauss,利用gs_datachecker工具对M\*与openGauss数据库的数据一致性校验速率进行测试。计算数据库大小与数据总条数，校验完成后性指标满足：总size/time≥150MB/s、总条数/time≥20w条/s且数据校验结果准确。共计执行用例50个，累计发现缺陷3个，3个缺陷已解决，回归通过，整体质量一般

## 3.2   约束说明

1. M* 5.7.27数据存储分盘
2. openGauss侧的目标库为兼容B库，除初始用户外的数据库用户均有复制权。
3. 表须含主键
4. 仅对数据的差异性及表字段名称的差异性进行数据校验，不对表结构及字段类型的差异性做校验


## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 |  严重  | 主要 | 次要  | 不重要 |
| ------ | :------: | :----: | :--: | :---: | :----: |
| 数目   |    3     |   0    |  2   |   1   |   0    |
| 百分比 |   100%   | 0%     | 66.67%| 33.3% |   0%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I79TWK](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I79TWK?from=project-issue) | 次要 | 全量数据校验速率满足150M/S：验证60张表，单表总行数10w，当宿端openGauss多表的时候，校验结果文件failed.log未生成，没有相应提示 | 已验收|
| 2    | [I7ABZ0](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I7ABZ0?from=project-issue) | 主要 | 全量数据校验速率满足150M/S：10张表，每张表1亿数据。源端M*、宿端openGauss抽取服务报错 | 已验收 |
| 3    | [I79DCH](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I79DCH?from=project-issue) | 主要 | 全量数据校验速率满足150M/S：10张表，每张表50万数据。校验速率达不到150M/S | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

1. sysbench批量创造数据，表结构如下：

```
CREATE TABLE `t_datacheck_templete` (
	`id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
	`c_int` INT(11) NULL DEFAULT NULL,
	`c_bigint` BIGINT(20) NULL DEFAULT NULL,
	`c_bigint2` BIGINT(20) NULL DEFAULT NULL,
	`c_char` CHAR(120) NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	`c_char2` CHAR(120) NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	`c_char3` CHAR(120) NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	`c_varchar` VARCHAR(300) NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	`c_varchar2` VARCHAR(300) NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	` c_text` TEXT NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	` c_text2` TEXT NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	`c_date` DATE NULL DEFAULT NULL,
	`c_time` TIME NULL DEFAULT NULL,
	` c_timestamp` TIMESTAMP NULL DEFAULT NULL,
	`c_float` FLOAT NULL DEFAULT NULL,
	`c_double` DOUBLE NULL DEFAULT NULL,
	`c_decimal` DECIMAL(20,6) NULL DEFAULT NULL,
	`c_decimal2` DECIMAL(20,6) NULL DEFAULT NULL,
	`c_enum` ENUM('Y','N') NULL DEFAULT NULL COLLATE 'utf8mb4_general_ci',
	PRIMARY KEY (`id`) USING BTREE
)
COLLATE='utf8mb4_general_ci'
ENGINE=InnoDB
;
```
2. 表的每条数据量如下：

```
|  1 | 794207 | 69885247 |  59010375 | uld do one of two thingstrying to test an application that c | services then we could do one of two thingstrying to test an | ingstrying to test an application that communicates with oth | s then we could do one of two thingstrying to t | mmunicates with other  | ther services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we | we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application that communicates with other services then we could do one of two thingstrying to test an application tha | 2023-08-30 | 09:38:35 | 2.76939 | 3540.9786768702 | 4723.637633 | 67900.487187 | C      | ices then we could do one of  |
```

3. 使用chameleon工具进行全量数据迁移

   ```shell
   chameleon drop_replica_schema --config default --debug
   chameleon create_replica_schema --config default --debug
   chameleon add_source --config default --source m* --debug
   chameleon init_replica --config default --source m* --debug
   ```

4. 查看M*待校验的表数据总量

```
select concat(round(sum(data_length/1024/1024),2),'MB') as data from information_schema.tables where table_schema='databasename';
```

5. 启动M*、openGauss抽取服务及数据校验服务

```
sh extract-endpoints.sh start
sh check-endpoints.sh start
```

6. 迁移速度 = 总Size / 校验耗时，其中总Size是M*待校验的所有表Size之和，校验耗时取自结果文件summary.log的cost字段的值，单位为秒

7. 查看生成的结果文件准确

## 4.2   性能场景

| 场景 | 场景描述                                       | 平均校验速度 | 校验结果 |
| :--: | :--------------------------------------------- | :----------------: | :------: |
|  1   | 50张表，每张表数据条数1000万，数据总量约884G         | 442MB/s、24w条/s |   准确   |



## 4.3 资料测试

| 测试步骤：                     | 测试结果  |
| ------------------------------ | --------- |
| 1. 使用方法描述<br>2. 示例测试 | 未发现bug |

## 4.4   测试结果

### 4.4.1 测试结果分析

​       本次测试中，基于sysbench特定表结构模型，gs_datachecker数据全量校验性能满足150MB/s、20w条/s。

### 4.4.2 测试数据统计

| 版本名称                       | 测试用例数 | 用例执行结果           | 发现问题单数    |
| ------------------------------ | :--------- | :--------------------- | --------------- |
| openGauss 5.1.0 build 68d1772f | 7         | Passed:4<br/>Failed:3 | 3  |
| openGauss 5.1.0 build 24d9203f | 3         | Passed:3<br/>Failed:0 | 0  |
| openGauss 5.1.0 build b5a8d5b0 | 1         | Passed:1<br/>Failed:0 | 0  |


数据说明：

1. gs_datachecker全量校验满足150MB/S、20w条/s，共计发现3个bug
2. 缺陷密度为3(缺陷个数)/0.138k(代码行数)=21.73(个/kloc)

## 4.5   后续测试建议

暂无

# 5     附件

## 5.1 校验速率计算

校验速率 =  源端(MySQL)数据库大小 / 校验时间

源端数据库大小：select concat(round(sum(data_length/1024/1024),2),'MB') as data from information_schema.tables where table_schema='databasename';

校验时间：结果文件summary.log的cost字段的值，单位为秒
