![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述                 | 作者     |
| ---------- | -------- | ------------------------ | -------- |
| 2022-12-13 | 1.0      | 特性测试报告初稿完成     | songjing |
| 2023-01-11 | 1.1      | 根据评审意见修改测试报告 | songjing |

 关键词： desc，describe，explain

摘要：

本文档主要介绍openGauss在兼容B模式下支持explain table查看表结构，支持desc/describe sql查看执行计划，支持explain extended sql，支持explain format=json/traditional选项语法进行测试并得出测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

openGauss在兼容B模式下支持explain和desc保持一致，互为同义词，即支持explain table查看表结构，支持desc/describe sql查看执行计划，支持explain extended sql，支持explain format=json/traditional选项语法。

# 2 特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build e3ed21de | 2022-11-25   | 2022-11-30   |
| openGauss 3.1.0 build ef6a5318 | 2022-12-26   | 2022-12-26   |

环境信息：

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：32GB<br />硬盘：100GB<br />OS：CentoOS Linux release 7.6.1810 (Core) |      |

# 3 测试结论概述

## 3.1 测试整体结论

支持explain和desc保持一致，互为同义词共计执行用例75条，覆盖功能测试、升级测试和资料测试。功能主要覆盖explain查看表结构，使用desc查看执行计划，兼容extended选项、format=json/traditional的语法格式，升级测试主要验证升级后支持该特性且功能正常，资料测试验证描述是否准确及示例是否正确执行，累计发现问题1个，已验收通过，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 使用explain查看表结构信息，结果与desc的结果保持一致，测试通过 |
| 功能测试 | 使用desc/describe查看执行计划，结果与explain查询的保持一致，测试通过 |
| 功能测试 | explain/desc查看执行计划，测试使用extended选项和format=json/traditonal格式，测试通过 |
| 升级测试 | 升级后支持该特性且功能正常，测试通过                         |
| 资料测试 | 资料叙述准确无误，示例正确，约束全面，测试通过               |

## 3.2 约束说明

1. openGauss需使用兼容B库且含dolphin插件

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 0    | 1      |
| 百分比 | 100%     | 0    | 0    | 0    | 100%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                    | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ----------------------------------------------------------- | -------- |
| 1    | [I67WBU](https://e.gitee.com/opengaussorg/dashboard?tab=commits&issue=I67WBU) | 不重要   | explain章节示例执行结果与实际不符，describe章节资料描述有误 | 已验收   |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 explain查看表结构信息

| 测试步骤                                                     | 测试结果                   |
| ------------------------------------------------------------ | -------------------------- |
| 1. 非兼容B库使用explain/desc查看表结构和执行计划<br/>2.兼容B库创建/修改表， explain查看表结构 | 执行用例26条，结果符合预期 |

### 4.1.2 desc/describe查看执行计划

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. desc/describe结合原有explain选项查看计划<br/>2. desc/describe结合索引查看执行计划<br/>3. desc/describe查看不同查询方式的计划 | 执行用例39条，发现bug1个（模块交互，其他模块问题），已修复且验收通过 |

### 4.1.3 extended选项和format=json/traditonal格式

| 测试步骤                                                     | 测试结果                  |
| ------------------------------------------------------------ | ------------------------- |
| 1. 查看执行计划，使用extended选项<br />2. 查看执行计划，使用format=json/traditional格式 | 执行用例9条，结果符合预期 |

## 4.2 升级测试

| 测试步骤：                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1. 安装3.0.0版本数据库（无本特性）<br/>2. 升级数据库到包含本特性的数据库版本，提交升级<br/>3. 在兼容B库建表，explain查看表结构，desc查看执行计划 | 执行1条用例，未发现bug，结果符合预期 |

## 4.3 资料测试

| 测试步骤                           | 测试结果 |
| ---------------------------------- | -------- |
| 资料叙述准确，示例正确，约束全面。 | 测试通过 |

## 4.4 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 3.1.0 build e3ed21de | 75         | Passed：74<br />Failed：1 | 1            |
| openGauss 3.1.0 build ef6a5318 | 11         | Passed：11<br />Failed：0 | 0            |

数据说明：

1. 累计发现2个缺陷，1个为资料缺陷，1个为模块交互replace into模块缺陷，均已修复并回归通过。
2. 以上缺陷为资料缺陷和非本特性引入，不计入代码缺陷密度，代码缺陷密度为 0(缺陷个数)/0.56kloc(代码行数)=0(个/kloc)

## 4.5 后续测试建议

无

# 5     附件

## 5.1 示例

```sql
--在兼容B库创建表
testdb=# create table t_test(
testdb(# c_int int primary key,
testdb(# c_char char(10) not null,
testdb(# c_varchar varchar(20) DEFAULT 'beijing');
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_test_pkey" for table "t_test"
CREATE TABLE
       
--explain查看表结构信息，可用模式名修饰表名
testdb=# explain t_test;
   Field   |         Type          | Null | Key |           Default            | Extra
-----------+-----------------------+------+-----+------------------------------+-------
 c_int     | integer               | NO   | PRI | NULL                         |
 c_char    | character(10)         | NO   |     | NULL                         |
 c_varchar | character varying(20) | YES  |     | 'beijing'::character varying |
(3 rows)

testdb=# explain public.t_test;
   Field   |         Type          | Null | Key |           Default            | Extra
-----------+-----------------------+------+-----+------------------------------+-------
 c_int     | integer               | NO   | PRI | NULL                         |
 c_char    | character(10)         | NO   |     | NULL                         |
 c_varchar | character varying(20) | YES  |     | 'beijing'::character varying |
(3 rows)

--desc/describe查看执行计划
testdb=# describe select * from t_test;
                        QUERY PLAN
-----------------------------------------------------------
 Seq Scan on t_test  (cost=0.00..21.34 rows=1134 width=42)
(1 row)

testdb=# desc extended select c_int from t_test;
                        QUERY PLAN
----------------------------------------------------------
 Seq Scan on t_test  (cost=0.00..21.34 rows=1134 width=4)
(1 row)

testdb=# describe analyse delete from t_test;
                                                QUERY PLAN
----------------------------------------------------------------------------------------------------------
 Delete on t_test  (cost=0.00..21.34 rows=1134 width=6) (actual time=0.003..0.003 rows=0 loops=1)
   ->  Seq Scan on t_test  (cost=0.00..21.34 rows=1134 width=6) (actual time=0.001..0.001 rows=0 loops=1)
 Total runtime: 0.079 ms
(3 rows)

--设置explain_perf_mode为normal
testdb=# set explain_perf_mode to normal;
SET
       
--以json格式显示执行计划
testdb=# desc format = json select * from t_test where c_int = 100;
             QUERY PLAN
-------------------------------------
 [                                  +
   {                                +
     "Plan": {                      +
       "Node Type": "Index Scan",   +
       "Scan Direction": "Forward", +
       "Index Name": "t_test_pkey", +
       "Relation Name": "t_test",   +
       "Alias": "t_test",           +
       "Startup Cost": 0.00,        +
       "Total Cost": 8.27,          +
       "Plan Rows": 1,              +
       "Plan Width": 42,            +
       "Index Cond": "(c_int = 100)"+
     }                              +
   }                                +
 ]
(1 row)
```

