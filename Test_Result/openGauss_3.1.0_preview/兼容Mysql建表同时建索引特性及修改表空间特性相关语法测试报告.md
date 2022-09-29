![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者        |
| :--------- | ----------- | -------------------- | ----------- |
| 2022-09-20 | 1.0         | 特性测试报告初稿完成 | zhanghuan96 |
| 2022-09-29 | 1.1         | 修改评审意见         | zhanghuan96 |

 关键词： 

M*部分语法、dolphin插件、CREATE/ALTER TABLE、TABLESPACE

摘要：

本文档主要基于插件dolphin兼容M*的部分语法，包含两个特性，第一，支持CREATE TABLE同时创建索引，支持ALTER TABLE添加索引；第二，兼容DDL语法（含ALTER TABLESPACE语法支持WAIT和ENGINE选项；DROP TABLESPACE语法支持ENGINE选项；CREATE TABLE LIKE语句不加括号复制源表），对上述特性进行测试，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

基于插件dolphin，实现openGauss对M*部分语法的兼容，包括支持CREATE TABLE同时创建索引，支持ALTER TABLE添加索引；兼容DDL语法（含ALTER TABLESPACE语法支持WAIT和ENGINE选项；DROP TABLESPACE语法支持ENGINE选项；CREATE TABLE LIKE语句不加括号复制源表）进行测试。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build b4215eeb | 2022-9-8     | 2022-9-12    |
| openGauss 3.1.0 build 0b50eccc | 2022-9-15    | 2022-9-19    |
| dolphin 1.0                    | 2022-8-22    | 2022-8-26    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300GB<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于插件dolphin兼容M*的部分语法测试，含如下两个特性

* 支持CREATE/ALTER TABL时创建索引
* 多个M* DDL语法兼容

其中支持CREATE/ALTER TABLE时创建索引语法进行测试，共计执行41条用例，主要覆盖了功能测试和资料测试。功能测试包括创建表同时创建索引功能可用；使用ALTER TABLE语法添加索引功能可用。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

多个M* DDL语法兼容语法测试，共计执行21条用例，主要覆盖了功能测试和资料测试。功能测试覆盖修改表空间添加WAIT或ENGINE可选项是否可用；删除表空间添加ENGINE可选项是否可用；复制表结构语法是否可用。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | om方式安装数据库，创建兼容B库，自动加载dolphin插件测试，执行结果符合预期，通过 |
| 功能测试 | CREATE TABLE同时创建btree/hash/gin/gist/psort/ubtree索引，通过 |
| 功能测试 | CREATE TABLE同时创建复合索引，通过                           |
| 功能测试 | ALTER TABLE tbl_name add index语句添加索引 ，通过            |
| 功能测试 | ALTER TABLESPACE语句添加WAIT和ENGINE选项，通过               |
| 功能测试 | DROP TABLESPACE语句添加ENGINE选项，通过                      |
| 功能测试 | CREATE TABLE tbl_name LIKE source_tblname，通过              |
| 资料测试 | 资料描述完整准确，示例执行结果正确，整体质量良好             |

## 3.2   约束说明

#####       特性1 支持CREATE/ALTER TABL时创建索引

- 兼容M*5.7版本

- openGauss需使用兼容B库且含dolphin插件

- 同一schema下索引名不能重复

- 建表同时创建索引，索引名可以省略

- 建表同时创建索引，using语句可以省略，如行存表默认创建btree索引

- 创建分区表同时创建索引，默认索引为local索引，无法指定local或global关键字

- key关键字在dolphin插件下不能作为列名使用，支持其他对象名；index关键字在dolphin插件下只能作为函数名，不支持其他对象名

- 建表同时创建索引支持index_options选项，其中comment和index_type数量和顺序任意

  ##### 特性2 多个M* DDL语法兼容

- ALTER TABLESPACE语句添加WAIT和ENGINE选项，选项可重复或乱序

- DROP TABLESPACE语句添加ENGINE选项，ENGINE选项不能重复

- CREATE TABLE tbl_name LIKE source_tblname语法，若源表是全局临时表，则目标表可以是普通表/全局临时表/本地临时表

- CREATE TABLE tbl_name LIKE source_tblname语法源表是本地临时表，则目标表必须是本地临时表

- CREATE TABLE tbl_name LIKE source_name [like_option...]，支持添加like_option选项

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5RUJH](https://gitee.com/opengauss/docs/issues/I5RUJH?from=project-issue) | 次要     | 修改并补充兼容CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name LIKE old_tbl_name语法描述内容 | 已验收   |
| 2    | [I5QV39](https://gitee.com/opengauss/Plugin/issues/I5QV39?from=project-issue) | 次要     | 创建表同时创建索引，添加comment可选项，创建成功，但comment功能未生效 | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 CREATE TABLE语句创建索引

| 测试步骤                            | 测试结果                           |
| ----------------------------------- | ---------------------------------- |
| 1.建表并创建索引<br/>2.查询索引信息 | 执行27条用例，发现1个bug，验收通过 |

### 4.1.2 ALTER TABLE语句创建索引

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 1.建表<br/>2.执行ALTER TABLE tbl_name ADD INDEX/KEY 添加索引<br/>3.查询索引信息 | 执行14条用例，未发现bug，执行通过 |

###  4.1.3 ALTER TABLESPACE语法测试

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1.创建表空间<br>2.修改表空间，添加wait和engine选项<br>3.删除表空间，添加engine选项 | 执行9条用例，未发现bug |

### 4.1.4 CREATE TABLE tbl_name LIKE source_name [like_option...]语法测试

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.创建源表，源表含约束，索引，with选项等<br/>2.复制源表，创建新表 | 执行12条用例，发现1个bug，已验收通过 |

## 4.2  测试执行统计数据

#### 特性1 支持CREATE/ALTER TABL时创建索引

| 版本名称                       | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ------------------------------ | ---------- | ----------------------- | ------------ |
| openGauss 3.1.0 build b4215eeb | 41         | Passed：40<br>Failed：1 | 1            |
| openGauss 3.1.0 build 075791e0 | 4          | Passed：4<br/>Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单1个，已验收通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：1(缺陷个数)/1.379kloc(代码行数)=0.72(个/kloc)

#### 特性2 多个M* DDL语法兼

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 3.1.0 build 0b50eccc | 21         | Passed：21<br/>Failed：0 | 1            |

*数据项说明：*

* 累计发现缺陷单1个，为资料问题，已验收通过
* 缺陷密度：1(缺陷个数)/0.422kloc(代码行数)=2.37(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

##  5.1 CREATE TABLE创建索引语法使用

```sql
--建表同时创建btree索引
zh_db=# create table if not exists t_index0001(id int unsigned not null,uid int not null, index 索引名称 using btree(id))with(orientation=row);
CREATE TABLE
--查询
zh_db=# \d+ t_index0001
                     Table "public.t_index0001"
 Column |  Type   | Modifiers | Storage | Stats target | Description
--------+---------+-----------+---------+--------------+-------------
 id     | uint4   | not null  | plain   |              |
 uid    | integer | not null  | plain   |              |
Indexes:
    "索引名称" btree (id) TABLESPACE pg_default
Has OIDs: no
Options: orientation=row, compression=no
```

## 5.2 CREATE TABLE tbl_name LIKE source_name [like_option...]语法使用

```sql
--创建源表
zh_db=# create table t_grammar0001(
zh_db(# id int primary key,
zh_db(# name varchar2(20) unique,
zh_db(# sex text not null,
zh_db(# age int default 18,
zh_db(# salary numeric check(salary >5000),
zh_db(# key using btree(age)) with(storage_type=astore,fillfactor=50);
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_grammar0001_pkey" for table "t_grammar0001"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_grammar0001_name_key" for table "t_grammar0001"
CREATE TABLE
--字段添加注释
zh_db=# comment on column t_grammar0001.id is '序号';
COMMENT
--使用CREATE TABLE tbl_name LIKE source_name [like_option...]语法创建目标表
zh_db=# create table t_grammar0001_01 like t_grammar0001 including defaults including constraints including storage including indexes including comments including reloptions;
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "t_grammar0001_01_pkey" for table "t_grammar0001_01"
NOTICE:  CREATE TABLE / UNIQUE will create implicit index "t_grammar0001_01_name_key" for table "t_grammar0001_01"
CREATE TABLE
--查询目标表，源表的默认约束，check约束，字段注释，with选项等都被复制到目标表
zh_db=# \d+ t_grammar0001_01
                           Table "public.t_grammar0001_01"
 Column |         Type          | Modifiers  | Storage  | Stats target | Description
--------+-----------------------+------------+----------+--------------+-------------
 id     | integer               | not null   | plain    |              | 序号
 name   | character varying(20) |            | extended |              |
 sex    | text                  | not null   | extended |              |
 age    | integer               | default 18 | plain    |              |
 salary | numeric(10,0)         |            | main     |              |
Indexes:
    "t_grammar0001_01_pkey" PRIMARY KEY, btree (id) TABLESPACE pg_default
    "t_grammar0001_01_name_key" UNIQUE CONSTRAINT, btree (name) TABLESPACE pg_default
    "t_grammar0001_01_age_idx" btree (age) TABLESPACE pg_default
Check constraints:
    "t_grammar0001_salary_check" CHECK (salary > 5000::numeric)
Has OIDs: no
Options: orientation=row, storage_type=astore, fillfactor=50, compression=no, toast.storage_type=astore

```

