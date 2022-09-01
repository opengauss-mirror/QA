![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订   版本 | 修改描述                 | 作者      |
| :-------- | ----------- | ------------------------ | --------- |
| 2022-8-23 | 1.0         | 特性测试报告初稿完成     | zhanghang |
| 2022-8-24 | 1.1         | 根据评审意见修改测试报告 | zhanghang |

 关键词： 

chameleon、在线迁移、DDL

摘要：

本文档主要介绍MySQL在线迁移工具支持在线迁移DDL语句，即使用chameleon工具将DDL语句创建的数据库对象在线迁移到openGauss中。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

MySQL在线迁移工具支持在线迁移DDL语句至openGauss侧，包括在线创建分区表、在线增加分区、在线删除分区、在线截断分区、在线合并分区、在线交换分区、在线重组分区、在线创建索引、在线删除索引、在线创建外键、在线删除外键、在线创建唯一约束、在线删除唯一约束。

# 2     特性测试信息

| 版本名称                                                 | 测试起始时间 | 测试结束时间 |
| -------------------------------------------------------- | ------------ | ------------ |
| mysql 5.7.37                                             | 2022-05-25   | 2022-08-23   |
| openGauss 3.0.0 build 475170ca                           | 2022-05-25   | 2022-07-03   |
| chameleon 3.0.0 cdd98c10b3e270c72c6c7c14874e7892f3279f99 | 2022-05-25   | 2022-07-03   |
| openGauss 3.0.0 build 8d13b3e3                           | 2022-07-04   | 2022-08-07   |
| dolphin 1.0 b1914084d32b7e57794a5cdaba4e84634227cbb6     | 2022-07-04   | 2022-08-07   |
| chameleon 3.0.0 aef5b993e6fded438f94f7e36e1d4dfca2f1a07a | 2022-07-04   | 2022-08-07   |
| openGauss 3.0.0 build 038d63d6                           | 2022-08-08   | 2022-08-23   |
| dolphin 1.0 cda68f0efd2b665583a02b0b593ec955c2adcbf7     | 2022-08-08   | 2022-08-23   |
| chameleon 3.0.0                                          | 2022-08-08   | 2022-08-23   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br />内存：16GB<br />硬盘：197GB<br />OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss-MySQL在线迁移工具支持在线迁移DDL语句，共计执行61条用例，主要覆盖了功能测试和资料测试。功能测试覆盖在MySQL数据库分别在线创建分区表、在线增加分区、在线删除分区、在线截断分区、在线合并分区、在线交换分区、在线重组分区、在线创建索引、在线删除索引、在线创建外键、在线删除外键、在线创建唯一约束、在线删除唯一约束，使用chameleon工具在线迁移上述对象到openGauss，保证迁移后的对象在openGauss可以正常使用。对于openGauss目前不支持的功能（详见[  chameleon使用指南.md](https://gitee.com/openGauss/openGauss-tools-chameleon/blob/master/chameleon%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.md)）会迁移失败，日志会有合理提示。资料测试覆盖校验资料描述完整准确，示例执行结果正确。累计发现缺陷单38个，37个缺陷已解决，回归通过，1个确认非问题已取消，整体质量一般。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库创建分区表，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库增加分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库删除分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库截断分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库合并分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库交换分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库重组分区，openGauss数据库查询分区表，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库创建索引，openGauss数据库查询表和索引，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库删除索引，openGauss数据库查询表和索引，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库创建外键，openGauss数据库查询表和外键，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库删除外键，openGauss数据库查询表和外键，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库创建唯一约束，openGauss数据库查询表和约束，执行结果符合预期，通过 |
| 功能测试 | chameleon工具开启在线迁移，MySQL数据库删除唯一约束，openGauss数据库查询表和约束，执行结果符合预期，通过 |
| 资料测试 | 资料描述完整准确，操作示例可执行，结果正确（详见[chameleon使用指南.md](https://gitee.com/opengauss/openGauss-tools-chameleon/blob/master/chameleon%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.md)） |

## 3.2   约束说明

- MySQL使用5.7版本。

- openGauss需使用兼容B库。

- 对于索引或约束中的表达式，如索引前缀表达式id(5)的写法目前暂会迁移为col_name。

- 迁移后的索引或者约束如index_name会改写为tbl_name_index_name的带有表名前缀的格式。

- openGauss支持的分区表类型详见以下表格，其中不支持的分区表将暂不迁移。

  |          | Range分区            | List分区             | Hash/Key分区         | 二级分区             |
  | -------- | -------------------- | -------------------- | -------------------- | -------------------- |
  | 增加分区 | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss × | MySQL ✓，openGauss ✓ |
  | 删除分区 | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss × | MySQL ✓，openGauss ✓ |
  | 截断分区 | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss × | MySQL ✓，openGauss ✓ |
  | 合并分区 | MySQL ×，openGauss × | MySQL ×，openGauss × | MySQL ✓，openGauss × | MySQL ×，openGauss × |
  | 交换分区 | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss × | MySQL ✓，openGauss × |
  | 重组分区 | MySQL ✓，openGauss ✓ | MySQL ✓，openGauss × | MySQL ✓，openGauss × | MySQL ✓，openGauss × |
  | 切割分区 | MySQL ×，openGauss ✓ | MySQL ×，openGauss × | MySQL ×，openGauss × | MySQL ×，openGauss × |
  
  备注：
  
  1.由于openGauss内核中Hash分区表不支持添加和删除分区，故Hash分区表增加分区和删除分区功能不支持迁移。
  
  2.由于openGauss内核中Hash分区数据与MySQL存放分区数据不一致，故Hash分区表截断分区或交换分区功能支持迁移，但数据与MySQL侧有差异。
  
  3.MySQL中合并分区只能用于Hash/Key分区表，由于openGauss内核中Hash分区表不支持切割和合成分区，故Hash分区表合并分区功能不支持迁移。
  
  4.由于openGauss内核中二级分区表暂不支持交换分区，故二级分区表交换分区功能不支持迁移。
  
  5.由于openGauss内核中List分区表和Hash分区表不支持切割和合成分区，故List分区表和Hash分区表重组分区功能不支持迁移。openGauss内核中二级分区表不支持切割分区功能，故二级分区表重组分区功能也不支持迁移。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 备注 |
| -------- | -------- | ---- |
| NA       |          |      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 38 | 0    | 27 | 11 | 0 |
| 百分比 | 100% | 0%   | 71% | 29% | 0%  |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5BQHC](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5BQHC?from=project-issue) | 主要     | 在线创建索引，索引迁移失败后drop table，openGauss侧的表未删除 | 已验收   |
| 2    | [I5BNI6](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5BNI6?from=project-issue) | 主要     | 在线迁移一级分区表，带自增选项时，迁移成功，但是额外多出一张表 | 已验收   |
| 3    | [I5BMXW](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5BMXW?from=project-issue) | 主要     | 在线迁移一级分区表，包含表级唯一键或表级check约束时，迁移失败 | 已验收   |
| 4    | [I5B812](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5B812?from=project-issue) | 主要     | 开启在线迁移，源库创建普通表，字段含auto_increment，在线迁移失败 | 已验收   |
| 5    | [I5AHKM](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5AHKM?from=project-issue) | 主要     | 开启在线迁移，分区表迁移失败，报错                           | 已验收   |
| 6    | [I59WCH](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I59WCH?from=project-issue) | 主要     | 开启在线迁移，迁移失败，在线进程退出                         | 已验收   |
| 7    | [I5JAWY](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5JAWY?from=project-issue) | 次要     | 在线迁移普通表，在线迁移失败                                 | 已验收   |
| 8    | [I5I0JI](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5I0JI?from=project-issue) | 次要     | 在线迁移MySQL侧进行alter操作时，openGauss侧不同步            | 已验收   |
| 9    | [I5GX53](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5GX53?from=project-issue) | 次要     | MySQL在线create index(索引添加描述)，openGauss侧索引的Description约束未迁移 | 已验收   |
| 10   | [I5GFIV](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5GFIV?from=project-issue) | 次要     | 在线迁移过程中，MySQL重命名表，未迁移至openGauss             | 已验收   |
| 11   | [I5G353](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5G353?from=project-issue) | 次要     | MySQL在线创建从表时创建外键(限制删除)，迁移至openGauss后外键信息显示不完整 | 已验收   |
| 12   | [I5EV7B](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5EV7B?from=project-issue) | 次要     | 开启在线迁移，MySQL在线exchange partition(交换表表名和分区名相同），交换后数据不正确 | 已验收   |
| 13   | [I5ETAF](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5ETAF?from=project-issue) | 次要     | 开启在线迁移，给list分区表增加分区，在线迁移报错             | 已验收   |
| 14   | [I5DFIM](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5DFIM?from=project-issue) | 次要     | 清理环境时在线drop table，openGauss侧有未删除的表遗留，无报错日志 | 已验收   |
| 15   | [I5EB0B](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5EB0B?from=project-issue) | 主要     | MySQL在线创建普通表后删除外键，openGauss侧外键未删除，chameleon在线迁移进程中断 | 已验收   |
| 16   | [I5DZTC](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5DZTC?from=project-issue) | 主要     | MySQL在线建表时创建外键，openGauss侧迁移失败，chameleon在线迁移进程中断 | 已验收   |
| 17   | [I5I596](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5I596?from=project-issue) | 主要     | MySQL在线创建从表后创建外键(添加约束名)，然后删除外键，chameleon在线迁移进程中断，openGauss侧外键未删除 | 已验收   |
| 18   | [I5HY11](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5HY11?from=project-issue) | 主要     | MySQL在线创建普通表后add index(索引列添加约束)，chameleon在线迁移进程中断，openGauss侧索引未迁移 | 已验收   |
| 19   | [I5G3M6](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5G3M6?from=project-issue) | 主要     | MySQL在线建表并创建索引后，执行alter table tb_name drop index语句，chameleon在线迁移进程中断，索引未删除 | 已验收   |
| 20   | [I5G1JT](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5G1JT?from=project-issue) | 主要     | MySQL在线建表并创建索引后，执行alter table tb_name drop index语句，chameleon在线迁移进程中断，索引未删除 | 已验收   |
| 21   | [I5G0LB](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5G0LB?from=project-issue) | 主要     | MySQL在线创建表后，执行alter table tb_name add index语句，chameleon在线迁移进程中断 | 已验收   |
| 22   | [I5FZD1](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5FZD1?from=project-issue) | 主要     | MySQL在线创建表并创建唯一索引，openGauss侧索引未迁移；执行drop index语句删除索引，chameleon在线迁移进程中断 | 已验收   |
| 23   | [I5FGCL](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5FGCL?from=project-issue) | 主要     | MySQL在线创建range分区表后add partition，openGauss侧分区数量未增加 | 已验收   |
| 24   | [I5FG2P](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5FG2P?from=project-issue) | 主要     | MySQL在线创建分区表(带非空约束)，openGauss侧非空约束未迁移   | 已验收   |
| 25   | [I5FFMY](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5FFMY?from=project-issue) | 主要     | MySQL在线创建分区表(默认值选项)，openGauss侧默认值约束未迁移 | 已验收   |
| 26   | [I5E8QP](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5E8QP?from=project-issue) | 主要     | MySQL在线创建分区表((带表和列注释信息)，openGauss侧注释信息未迁移 | 已验收   |
| 27   | [I5E8EF](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5E8EF?from=project-issue) | 主要     | MySQL在线创建分区表(带列级为唯一键或check约束)，openGauss侧约束未迁移 | 已验收   |
| 28   | [I5D6T2](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5D6T2?from=project-issue) | 主要     | MySQL在线对一级分区表create index(主键约束)，openGauss侧表和索引迁移失败时，日志中无报错信息；表迁移成功时，索引未迁移，日志中有报错信息 | 已验收   |
| 29   | [I5D5P6](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5D5P6?from=project-issue) | 主要     | MySQL在线对一级分区表create index(带自增选项)，openGauss侧表和索引迁移失败时，日志中无报错信息；表迁移成功时，索引未迁移，日志中有报错信息 | 已验收   |
| 30   | [I5D31V](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5D31V?from=project-issue) | 主要     | MySQL在线对list分区表create index(指定索引类型hash)，openGauss侧索引迁移失败 | 已验收   |
| 31   | [I5CTK8](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5CTK8?from=project-issue) | 主要     | MySQL在线对表的指定列前n个字符create index，openGauss侧迁移失败 | 已验收   |
| 32   | [I5CLOM](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5CLOM?from=project-issue) | 主要     | 在线迁移key分区表失败，日志中没有报错，chameleon进程未停止   | 已验收   |
| 33   | [I5CEA3](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5CEA3?from=project-issue) | 主要     | 在线迁移list-hash复合分区失败，无报错日志                    | 已验收   |
| 34   | [I5DAN0](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5DAN0?from=project-issue) | 主要     | MySQL在线对具有任意形式唯一约束的复合分区表create index，openGauss侧迁移失败 | 已验收   |
| 35   | [I5FWAG](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5FWAG?from=project-issue) | 次要     | MySQL在线创建表(添加无符号选项)，openGauss侧无符号约束未迁移 | 已取消   |
| 36   | [I5EWGN](https://gitee.com/openGauss/openGauss-tools-chameleon/issues/I5EWGN?from=project-issue) | 次要     | 开启在线迁移，40min左右，MySQL侧执行业务，在线迁移报错       | 已验收   |
| 37   | [I5OWH0](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5OWH0?from=project-issue) | 主要     | 二级分区ADD PARTITION报错                                    | 已验收   |
| 38   | [I5P0JK](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I5P0JK?from=project-issue) | 次要     | 【测试类型：资料】chameleon使用指南中第四节部分内容表述不准确 | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 在线创建分区表

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧创建分区表<br />3.openGauss侧查询表信息 | 执行8条用例，发现12个bug，现已修复且验收通过 |

### 4.1.2 在线ALTER分区表ADD PARTITION

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表ADD PARTITION<br />3.openGauss侧查询表信息 | 执行1条用例，发现2个bug，现已修复且验收通过 |

### 4.1.3 在线ALTER分区表DROP PARTITION

| 测试步骤                                                     | 测试结果              |
| ------------------------------------------------------------ | --------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表DROP PARTITION<br />3.openGauss侧查询表信息 | 执行1条用例，验收通过 |

### 4.1.4 在线ALTER分区表TRUNCATE PARTITION

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表TRUNCATE PARTITION<br />3.openGauss侧查询表信息 | 执行2条用例，发现1个bug，现已修复且验收通过 |

### 4.1.5 在线ALTER分区表COALESCE PARTITION

| 测试步骤                                                     | 测试结果              |
| ------------------------------------------------------------ | --------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表COALESCE PARTITION<br />3.openGauss侧查询表信息 | 执行1条用例，验收通过 |

### 4.1.6 在线ALTER分区表EXCHANGE PARTITION

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表EXCHANGE PARTITION<br />3.openGauss侧查询表信息 | 执行8条用例，发现3个bug，现已修复且验收通过 |

### 4.1.7 在线ALTER分区表REORGANIZE PARTITION

| 测试步骤                                                     | 测试结果              |
| ------------------------------------------------------------ | --------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧ALTER分区表REORGANIZE PARTITION<br />3.openGauss侧查询表信息 | 执行6条用例，验收通过 |

### 4.1.8 在线创建索引

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧创建索引<br />3.openGauss侧查询表信息 | 执行10条用例，发现11个bug，10个现已修复且验收通过，1个非问题已取消 |

### 4.1.9 在线删除索引

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧删除索引<br />3.openGauss侧查询表信息 | 执行9条用例，发现1个bug，现已修复且验收通过 |

### 4.1.10 在线创建外键

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧创建外键<br />3.openGauss侧查询表信息 | 执行2条用例，发现2个bug，现已修复且验收通过 |

### 4.1.11 在线删除外键

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧删除外键<br />3.openGauss侧查询表信息 | 执行2条用例，发现2个bug，现已修复且验收通过 |

### 4.1.12 在线创建唯一约束

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧创建唯一索引<br />3.openGauss侧查询表信息 | 执行9条用例，发现1个bug，现已修复且验收通过 |

### 4.1.13 在线删除唯一约束

| 测试步骤                                                     | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1.chameleon开启在线迁移<br />chameleon start_replica --config default --source mysql<br />2.MySQL侧创删除唯一索引<br />3.openGauss侧查询表信息 | 执行2条用例，发现1个bug，现已修复且验收通过 |

### 4.1.14 资料测试

| 测试步骤                                         | 测试结果                       |
| ------------------------------------------------ | ------------------------------ |
| 1.校验资料描述是否完整准确，示例执行结果是否正确 | 发现1个bug，现已修复且验收通过 |



## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.0.0 build 475170ca | 61         | Passed：42<br>Failed：19  | 19           |
| openGauss 3.0.0 build 8d13b3e3 | 61         | Passed：44<br/>Failed：17 | 17           |
| openGauss 3.0.0 build 038d63d6 | 61         | Passed：61<br/>Failed：0  | 0            |

* 累计发现缺陷单38个，37个已解决且回归通过，1个非问题已取消。
* 缺陷密度：37(缺陷个数) / 1.42(代码行数) = 26.1(个/kloc)。

## 4.4   后续测试建议

-  待openGauss支持哈希分区表切割分区、合成分区、添加和删除分区，列表分区表支持切割分区、合成分区后，验证分区表添加分区、删除分区、切割分区、合成分区功能是否迁移成功。
-  待openGauss支持二级分区表中的合并分区、重组分区、交换分区功能，验证二级分区表合并分区、重组分区、交换分区功能是否迁移成功。

# 5     附件

## 5.1   MySQL在线迁移分区表语句执行过程示例

```sql
--开启在线迁移
chameleon start_replica --config default --source mysql
--mysql侧建表
mysql> create table t_chameleon_ddl_0301 (
    ->     id int ,
    ->     first_name varchar(30),
    ->     last_name varchar(30)
    -> )
    -> partition by hash(id)
    -> partitions 4;
--openGauss侧查询
\d+ t_chameleon_ddl_0301
                           Table "public.t_chameleon_ddl_0301"
+------------+-----------------------+-----------+----------+--------------+-------------+
|   Column   |         Type          | Modifiers | Storage  | Stats target | Description |
+------------+-----------------------+-----------+----------+--------------+-------------+
| id         | integer               |           | plain    |              |             |
| first_name | character varying(30) |           | extended |              |             |
| last_name  | character varying(30) |           | extended |              |             |
+------------+-----------------------+-----------+----------+--------------+-------------+
Partition By HASH(id)
Number of partitions: 4 (View pg_partition to check each partition range.)
Has OIDs: no
Options: orientation=row, compression=no
```

