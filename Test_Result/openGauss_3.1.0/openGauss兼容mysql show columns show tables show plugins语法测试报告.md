![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订版本 | 修改描述         | 作者            |
| :-------- | :------- | :--------------- | :-------------- |
| 2022-8-20 | 1.0      | 特性测试报告初稿 | zou_jialiang050 |
| 2022.8.31 | 1.1      | 增加缺陷密度     | zou_jialiang050 |

 关键词： show columns、show tables、show plugins

摘要：

本文对openGauss兼容mysql show columns、show tables、show plugins语法特性进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| :----- | :------- | :-------- |
| 无     |          |          |

# 1     特性概述

openGauss在兼容b库下，可以实现对mysql show columns、show tables、show plugins三个语法的兼容，执行结果同MySQL侧一致。

# 2     特性测试信息

| 版本名称                      | 测试起始时间 | 测试结束时间 |
| :---------------------------- | :----------- | :------------ |
| openGauss3.0.0 build 3d018a7e | 2022-8-15    | 2022-8-20    |
| MYSQL 5.7                     | 2022-8-15    | 2022-8-20    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6161 8核<br />内存：1*32GB<br />硬盘：100G<br />OS：openEluer release 20.03(LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss兼容mysql show columns tables plugins语法特性，共计执行用例63个，主要涉及show columns、show tables、show plugins语法测试，未发现问题，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                         |
| :------- | :------------------------------- |
| 功能测试 | show cloumns语法功能测试通过     |
| 功能测试 | show tables语法功能测试通过      |
| 功能测试 | show pugins语法功能测试通过      |
| 资料测试 | 资料叙述准确无误，示例正确，通过 |

## 3.2   约束说明

- 兼容MYSQL 5.7版本
- openGauss需在兼容b库下执行

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|    NA    |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    0     |  0   |  0   |  0   |   0    |
| 百分比  (%) |    0     |  0   |  0   |  0   |   0    |

### 3.3.3 问题单汇总

| 序号 | issue号 | 问题级别 | 问题简述 | 问题状态 |
| :-- | :----- | :------ | :------ | :------ |
|  1   |   NA    |          |          |          |

# 4   测试执行

## 4.1   功能测试

### 4.1.1   show columns功能测试

| 测试步骤                                                     | 测试结果                      |
| :------------------------------------------------------------ | :----------------------------- |
| 1.show [full] {columns \| fileds} {from \| in} tbl_name [{from \| in} db_name] [like 'pattern' \| where expr]语法参数及功能验证，例如模糊匹配、条件筛选、多种数据类型字段显示、有无select权限操作、多种约束显示正确性等验证。 | 共执行用例37条<br />未发现bug |

###  4.1.2   show tables功能测试

| 测试步骤                                                     | 测试结果                        |
| :------------------------------------------------------------ | :------------------------------- |
| 1.show [full] tables [{from \| in} db_name] [like 'pattern' \| where expr]语法参数及功能验证，例如模糊匹配、条件筛选、多种格式表显示正确性等验证。 | 共执行条用例23条<br />未发现bug |

###  4.1.3   show plugins功能测试

| 测试步骤                                                     | 测试结果                       |
| :------------------------------------------------------------ | :------------------------------ |
| 1.show plugins语法功能验证、例如插件显示正确性、插件状态变更显示正确性等验证。 | 共执行条用例2条<br />未发现bug |

## 4.2   资料测试

###  4.2.1   show columns、show tables、show plugins资料验证

| 测试步骤                                                     | 测试结果                       |
| :------------------------------------------------------------ | :------------------------------ |
| 1.验证show columns、show tables、show plugins 文档描述是否准确、示例是否正确等。 | 共执行条用例1条<br />未发现bug |

## 4.3   测试执行统计数据

| 版本名称                      | 测试用例数 | 用例执行结果              | 发现问题单数 |
| :----------------------------- | :---------- | :------------------------- | :------------ |
| openGauss3.0.0 build 3d018a7e | 63         | Passed：63<br />Failed：0 | 0            |

缺陷单共0个，修改代码量为1.5kloc， 缺陷密度为0个/kloc

## 4.4   后续测试建议

1.多种不同权限用户执行show plugins查看插件状态

# 5     附件

```
test=# show full tables;
 Tables_in_tst_schema | Table_type 
----------------------+------------
 tst_t1               | BASE TABLE
 tst_v1               | VIEW
 test                 | BASE TABLE
(3 rows)

test=# show columns from test;
 Field |  Type   | Null | Key | Default | Extra 
-------+---------+------+-----+---------+-------
 c1    | integer | YES  |     | NULL    |
(1 rows)

test=# show plugins;
      Name       |  Status  | Type | Library | License |                     Comment
-----------------+----------+------+---------+---------+--------------------------------
 hstore          | ACTIVE   |      | NULL    |         | data type for storing sets of (key, value) pairs
 security_plugin | ACTIVE   |      | NULL    |         | provides security functionality
 plpgsql         | ACTIVE   |      | NULL    |         | PL/pgSQL procedural language
 log_fdw         | ACTIVE   |      | NULL    |         | Foreign-Data Wrapper for accessing logging data
 file_fdw        | ACTIVE   |      | NULL    |         | foreign-data wrapper for flat file access
 postgres_fdw    | DISABLED |      | NULL    |         | foreign-data wrapper for remote PostgreSQL servers
 mot_fdw         | ACTIVE   |      | NULL    |         | foreign-data wrapper for MOT access
 dolphin         | ACTIVE   |      | NULL    |         | sql engine
 dist_fdw        | ACTIVE   |      | NULL    |         | foreign-data wrapper for distfs access
(10 rows)
```