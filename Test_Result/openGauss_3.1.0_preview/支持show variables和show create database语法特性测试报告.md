![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者       |
| :--------- | ----------- | -------------------- | ---------- |
| 2022-09-23 | 1.0         | 特性测试报告初稿完成 | lizhuo9527 |
| 2022-09-26 | 1.1         | 修改特性测试报告     | lizhuo9527 |

 关键词： 

M*、dolphin插件、show variables、show create databse

摘要：

本文档主要基于dolphin插件兼容M*的两个语法，分别是`show variables`和`show create database`，前者用于查询GUC参数的值，后者展示schema信息，针对上述两个相关语法进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1	特性概述

基于dolphin插件，实现openGauss对M*部分语法的兼容，第一，`show [global | session] variables [like 'pattern' | where expr];`可以显示系统GUC参数的值并使用like或where子句进一步筛选和匹配；第二个是`show create {database | schema} [if not exists] db_names;`展示当前数据库的schema信息。

# 2	特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build ece73c7d | 2022-9-22    | 2022-9-23    |
| dolphin 1.0                    | 2022-9-22    | 2022-9-23    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3	测试结论概述

## 3.1 测试整体结论

基于dolphin插件兼容M*的部分语法测试，含下述两个语法：

* `show [global | session] variables [like 'pattern' | where expr];`
* `show create {database | schema} [if not exists] db_names;`

共计执行26条用例，主要覆盖了功能测试和资料测试。功能测试覆盖以下5点：

1. `show [global | session] variables`分别展示系统变量和当前session变量的值；
2. 包含like子句的情形，包括：
   - %和_通配符及混合使用
   - 通配符结合转义字符使用
   - like子句为空的情形
3. 包含where子句的情形，包括：
   - 等于/不等于 、in/not in条件筛选
   - 结合like子句及通配符、多条件匹配
4. 修改参数值后进行查询
5. show create database，包括：
   - 有效及无效的schema名
   - 指定名称、根据用户创建schema并包含参数的情形

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

未发现缺陷单，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 展示global变量和当前session变量的值，执行结果符合预期，通过  |
| 功能测试 | like子句%和_通配符及混合使用的情形，执行结果符合预期，通过   |
| 功能测试 | like子句通配符结合转义字符使用的情形，执行结果符合预期，通过 |
| 功能测试 | like子句为空的情形，执行结果符合预期，通过                   |
| 功能测试 | where子句等于/不等于 、in/not in条件筛选的情形，执行结果符合预期，通过 |
| 功能测试 | where子句结合like子句及通配符、多条件匹配的情形，执行结果符合预期，通过 |
| 功能测试 | 修改参数值后进行查询，执行结果符合预期，通过                 |
| 功能测试 | `show create database`有效及无效的schema名，执行结果符合预期，通过 |
| 功能测试 | 指定名称、根据用户创建schema并包含参数的情形，执行结果符合预期，通过 |
| 资料测试 | 资料描述正确，示例的执行无误，通过                           |

## 3.2	约束说明

- 兼容M*5.7版本
- openGauss需使用兼容B库且包含dolphin插件
- `show create database`语法实际上展示的是当前数据库下的schema信息

## 3.3    遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%       | 0%   | 0%   | 0%   | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号 | 问题级别 | 问题简述 | 问题状态 |
| ---- | ------- | -------- | -------- | -------- |
| NA   |         |          |          |          |

# 4    测试执行

## 4.1 测试执行步骤

###  4.1.1 展示global变量和当前session变量的值

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.`show global varilables`<br>2.`show session varilables`<br>3.`show variables` | 执行1条用例，未发现bug<br/>执行通过 |

### 4.1.2 like子句%和_通配符及混合使用

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.like子句结合%单个、多个通配符<br/>2.like子句结合_单个、多个通配符<br/>3.like子句结合多个通配符混合使用 | 执行8条用例，未发现bug<br/>执行通过 |

### 4.1.3 like子句结合转义字符及子句为空的情形

| 测试步骤                                      | 测试结果                            |
| --------------------------------------------- | ----------------------------------- |
| 1.通配符结合转义字符<br/>2.like子句表达式为空 | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.4 where子句条件筛选的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.where variable_name =/!=<br/>2.where value =/!=<br/>3.where variable_name in/not in<br/>4.where value in/not in | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.5 where子句结合like子句及多条件匹配的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.where子句结合like子句及通配符<br/>2.where子句结合like子句and/or多条件嵌套 | 执行3条用例，未发现bug<br/>执行通过 |

### 4.1.6 修改参数值进行查询

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.set 设置session级别参数后与global值对比<br>2.alter database/alter user<br>3.gs_guc set/reload | 执行2条用例，未发现bug<br/>执行通过 |

### 4.1.7 有效及无效的schema名

| 测试步骤                                          | 测试结果                            |
| ------------------------------------------------- | ----------------------------------- |
| 1.展示已存在的schema名<br/>2.展示不存在的schema名 | 执行1条用例，未发现bug<br/>执行通过 |

### 4.1.8 指定名称或根据用户创建schema并包含参数的情形

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.指定schema名创建schema并包含参数<br/>2.根据用户名创建schema并包含参数 | 执行7条用例，未发现bug<br/>执行通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.0.0 build ece73c7d | 26         | Passed：26<br>Failed：0 | 0            |

数据项说明：

* 缺陷密度：0(缺陷个数)/0.780kloc(代码行数)=0(个/kloc)

## 4.3  后续测试建议

-  针对`show variables`语法覆盖尽可能多的参数进行校验

# 5     附件

##  5.1 `show variables`

```sql
--step1: show variables expect: 查询成功
show variables where variable_name like 'e%n_' and value != 'off';
           Variable_name           | Value
-----------------------------------+-------
 enable_incremental_checkpoint     | on
 enable_instance_metric_persistent | on
 enable_user_metric_persistent     | on
 enable_valuepartition_pruning     | on
 enable_vector_engine              | on
 enable_xlog_prune                 | on
 escape_string_warning             | on
(7 rows)

--step2: show session variables expect: 查询成功
show session variables where variable_name like 'e%n_' and value != 'off';
           Variable_name           | Value
-----------------------------------+-------
 enable_incremental_checkpoint     | on
 enable_instance_metric_persistent | on
 enable_user_metric_persistent     | on
 enable_valuepartition_pruning     | on
 enable_vector_engine              | on
 enable_xlog_prune                 | on
 escape_string_warning             | on
(7 rows)

--step3: show global variables expect: 查询成功
show global variables where variable_name like 'e%n_' and value != 'on';
       Variable_name        | Value
----------------------------+-------
 enable_cbm_tracking        | off
 enable_codegen_print       | off
 enable_consider_usecount   | off
 enable_force_vector_engine | off
 enable_hadoop_env          | off
(5 rows)

```

## 5.2 `show create database`

```sql
--step1: 创建用户 expect: 创建成功
drop user if exists u_variable_0022;
NOTICE:  role "u_variable_0022" does not exist, skipping
DROP ROLE
create user u_variable_0022 password 'password@123';
CREATE ROLE

--step2: 创建schema expect: 创建成功
drop schema if exists sch_variable_0022;
NOTICE:  schema "sch_variable_0022" does not exist, skipping
DROP SCHEMA
create schema sch_variable_0022 authorization u_variable_0022 with blockchain;
CREATE SCHEMA

--step3: 查询schema expect: 查询成功
show create database sch_variable_0022;
     Database      |                                Create Database
-------------------+-------------------------------------------------------------------------------
 sch_variable_0022 | CREATE SCHEMA sch_variable_0022 AUTHORIZATION u_variable_0022 WITH BLOCKCHAIN
(1 row)

show create schema sch_variable_0022;
     Database      |                                Create Database
-------------------+-------------------------------------------------------------------------------
 sch_variable_0022 | CREATE SCHEMA sch_variable_0022 AUTHORIZATION u_variable_0022 WITH BLOCKCHAIN
(1 row)

--step4: 查询schema expect: 查询成功
show create database if not exists sch_variable_0022;
     Database      |                                       Create Database
-------------------+---------------------------------------------------------------------------------------------
 sch_variable_0022 | CREATE SCHEMA IF NOT EXISTS sch_variable_0022 AUTHORIZATION u_variable_0022 WITH BLOCKCHAIN
(1 row)

show create schema if not exists sch_variable_0022;
     Database      |                                       Create Database
-------------------+---------------------------------------------------------------------------------------------
 sch_variable_0022 | CREATE SCHEMA IF NOT EXISTS sch_variable_0022 AUTHORIZATION u_variable_0022 WITH BLOCKCHAIN
(1 row)

--step5: 清理环境 expect: 成功
drop schema if exists sch_variable_0022;
DROP SCHEMA
drop user if exists u_variable_0022;
DROP ROLE
```
