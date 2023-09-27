![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者       |
| :--------- | -------- | -------------------- | ---------- |
| 2023-09-23 | 1.0      | 特性测试报告初稿完成 | guoweibiao |

关键词： 

O*、whale extension

摘要：

whale 插件基于openGauss的A兼容（dbcompatibility='A'）扩展，从关键字、数据类型、常量与宏、函数和操作符、表达式、类型转换、DDL/DML/DCL语法、存储过程/自定义函数、系统视图等方面兼容O*数据库。本文档主要基于内核语法有新增、修改的内容进行测试并给出最终测试结论。即创建兼容A库，在A库中测试whale extension的新增语法。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1	特性概述

基于whale插件，实现openGauss对O*协议的语法特性兼容。

使用gsql连接openGauss兼容A库，并执行相关SQL语句；


# 2	特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.0 build db2a3a72 | 2023-9-13    | 2023-09-20   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3	测试结论概述

## 3.1 测试整体结论

整体测试结论:

| 需求名称                                | 测试结论                                                     |
| --------------------------------------- | ------------------------------------------------------------ |
| SQL引擎解耦化，实现O兼容性基础插件whale | 共执行107条用例，主要覆盖函数功能测试、whale插件安装测试、guc参数测试及资料测试。函数功能测试包括：gsql连接openGauss兼容A库，连接A库创建插件并执行相关SQL语句结果与postgres库orafce插件执行相关SQL语句结果是否一致。whale插件安装测试包括：OM方式安装的数据库是否可以正常创建插件，编译方式安装后的数据库，whale源码拷贝到openGauss-server源码的contrib目录下make install 后，连接数据库是否可以创建whale插件create extension whale。GUC参数测试包括：参数开启关闭后，插件语法功能是否有差异。资料测试：资料描述是否准确、示例能否正确执行。 |

累计发现缺陷单5个，剩余所有缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 测试点                                       | 第一轮测试结果 | 第二轮测试结果 |
| -------- | -------------------------------------------- | -------------- | -------------- |
| 功能测试 | whale插件函数和操作符的功能                  | 发现缺陷单4个  | 测试通过       |
| 安装测试 | 插件安装测试                                 | 测试通过       | 测试通过       |
| 资料测试 | 资料描述准确，示例完整，可正确操作，格式正确 | 发现缺陷单1个  | 测试通过       |
| 功能测试 | guc参数                                      | 测试通过       | 测试通过       |



下面是具体的测试项：

1.函数功能测试：

| 插件名 | 函数和操作符名称              |
| ------ | ----------------------------- |
| whale  | 1.GMS_ALERT包函数             |
| whale  | 2.GMS_ASSERT包函数            |
| whale  | 3.GMS_OUTPUT包函数            |
| whale  | 4.GMS_RANDOM包函数            |
| whale  | 5.GMS_UTILITY包函数           |
| whale  | 6.GMS_PIPE包函数              |
| whale  | 7.UTL_FILE包函数              |
| whale  | 8.字符处理函数和操作符        |
| whale  | 9.数字操作函数和操作符        |
| whale  | 10.时间和日期处理函数和操作符 |
| whale  | 11.条件表达式函数             |
| whale  | 12.聚合函数                   |

2.安装测试：



| whale安装 | 安装步骤                                                     |
| --------- | ------------------------------------------------------------ |
| 编译安装  | 1. 编译安装好openGauss<br/>2. 将whale源码拷贝到openGauss-server源码的contrib目录下<br/>3. 进入whale目录执行make install<br/>4. 创建A库并使用初始用户连接A库，创建whale插件create extension whale; |
| OM安装    | 1.创建A兼容性数据库<br/>create database dbtest dbcompatibility='A';<br/>2.在A库中创建whale插件<br/>create extension whale; |

3.GUC参数测试：

| guc参数                         | 测试步骤                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| whale.a_compatibility_mode      | set whale.a_compatibility_mode = on;<br/>show whale.a_compatibility_mode; |
| whale.timezone                  | set whale.timezone = GMT;<br/>show whale.timezone;           |
| whale.nls_date_format           | set whale.nls_date_format = 'YYYY-MM-DD HH24:MI:SS';<br/>show whale.nls_date_format; |
| whale.varchar2_null_safe_concat | set whale.varchar2_null_safe_concat = on;<br/>show whale.varchar2_null_safe_concat; |

4.资料测试：

资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。

## 3.2	约束说明

- 不支持小型化版本；
- 不支持删除whale插件；
- whale插件只能在A兼容性数据库下创建；
- whale插件需要在pg_catalog等schema下创建数据类型、函数等，所以加载whale插件需要初始用户权限。whale插件考虑到前向兼容性，不会自动加载，需要使用初始用户执行create extension whale语句在A库进行创建后才能使用。如果一个A兼容性数据库从来没有被初始用户或拥有初始用户权限的用户连接并调用create extension whale创建whale插件，那么它也不会加载whale插件；
- whale中所有新增/修改的语法不支持在gsql客户端通过```\h```查看帮助说明，不支持在gsql客户端自动补齐；
- whale插件的创建会删除数据库存在的插件所需的同名函数和类型以及之前存在的与之依赖的对象；

## 3.3    遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 5        | 0    | 0    | 5    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I81UNQ](https://gitee.com/opengauss/Plugin/issues/I81UNQ?from=project-issue) | 次要     | 设置guc参数后执行函数报错，dump函数表现与pg库不一致          | 已验收   |
| 2    | [I80R1U](https://gitee.com/opengauss/Plugin/issues/I80R1U?from=project-issue) | 次要     | whale插件文档关于gms_pipe.reset_buffer描述有重复             | 已验收   |
| 3    | [I8145K](https://gitee.com/opengauss/Plugin/issues/I8145K?from=project-issue) | 次要     | whale插件执行gms_alert.waitone函数，执行结果与orafce不一致，以及相关资料问题 | 已验收   |
| 4    | [I81VNN](https://gitee.com/opengauss/Plugin/issues/I81VNN?from=project-issue) | 次要     | whale插件函数gms_random.normal()和gms_random.random()不能生成随机数 | 已验收   |
| 5    | [I81OU1](https://gitee.com/opengauss/Plugin/issues/I81OU1?from=project-issue) | 次要     | whale插件函数gms_random.value和select gms_random.string表现与预期不符 | 已验收   |

# 4    测试执行

## 4.1 测试执行步骤

###  4.1.1 GMS_ALERT包函数测试

| 测试函数                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.gms_alert.register<br/>2.gms_alert.remove<br/>3.gms_alert.removeall()<br/>4.gms_alert.signal<br/>5.gms_alert.waitany<br/>6.gms_alert.waitone<br/>7.gms_alert.set_defaults | 执行7条用例，执行结果符合预期，测试通过，发现1个bug，1个问题已解决回归通过 |

### 4.1.2 GMS_ASSERT包函数测试

| 测试函数                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.gms_assert.enquote_literal<br/>2.gms_assert.enquote_name<br/>3.gms_assert.noop<br/>4.gms_assert.schema_name<br/>5.gms_assert.object_name<br/>6.gms_assert.simple_sql_name<br/>7.gms_assert.qualified_sql_name | 执行7条用例，执行结果符合预期，测试通过，无bug |

### 4.1.3 GMS_OUTPUT包函数测试

| 测试函数                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.gms_output.enable<br/>2.ggms_output.disable<br/>3.gms_output.put<br/>4.gms_output.put_line<br/>5.gms_output.serveroutput<br/>6.gms_output.get_line<br/>7.gms_output.new_line<br/>8.gms_output.get_lines | 执行8条用例，执行结果符合预期，测试通过，无bug |

### 4.1.4 GMS_RANDOM包函数测试

| 测试函数                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.gms_random.initialize<br/>2.gms_random.normal<br/>3.gms_random.seed<br/>4.gms_random.string<br/>5.gms_random.terminate<br/>6.gms_random.value<br/>7.gms_random.random | 执行7条用例，执行结果符合预期，测试通过，发现2个bug，2个问题已解决回归通过 |

### 4.1.5 GMS_UTILITY包函数测试

| 测试函数                                                   | 测试结果                                       |
| ---------------------------------------------------------- | ---------------------------------------------- |
| 1.gms_utility.format_call_stack<br/>2.gms_utility.get_time | 执行2条用例，执行结果符合预期，测试通过，无bug |

### 4.1.6 GMS_PIPE包函数测试

| 测试函数                                                     | 测试结果                                        |
| ------------------------------------------------------------ | ----------------------------------------------- |
| 1.gms_pipe.pack_message<br/>2.gms_pipe.send_message<br/>3.gms_pipe.receive_message<br/>4.gms_pipe.unpack_message_text<br/>5.gms_pipe.unpack_message_date<br/>6.gms_pipe.unpack_message_timestamp<br/>7.gms_pipe.unpack_message_number<br/>8.gms_pipe.unpack_message_bytea<br/>9.gms_pipe.unpack_message_record<br/>10.gms_pipe.unique_session_name<br/>11.gms_pipe.__list_pipes<br/>12.gms_pipe.next_item_type<br/>13.gms_pipe.create_pipe<br/>14.gms_pipe.reset_buffer<br/>15.gms_pipe.purge<br/>16.gms_pipe.remove_pipe | 执行16条用例，执行结果符合预期，测试通过，无bug |

### 4.1.7 UTL_FILE包函数测试

| 测试函数                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.utl_file.fopen<br/>2.utl_file.is_open<br/>3.utl_file.put<br/>4.utl_file.new_line<br/>5.utl_file.put_line<br/>6.utl_file.putf<br/>7.utl_file.fflush<br/>8.utl_file.fclose<br/>9.utl_file.fclose_all<br/>10.utl_file.fremove<br/>11.utl_file.frename<br/>12.utl_file.fcopy<br/>13.utl_file.fgetattr<br/>14.utl_file.tmpdir | 执行14条用例，执行结果符合预期，测试通过，发现1个bug，1个问题已解决回归通过 |

### 4.1.8 数字操作函数和操作符测试

| 测试函数                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.函数sinh<br/>2.函数cosh<br/>3.函数tanh<br/>4.函数nanvl(f1 float4, f2 float4)<br/>5.函数nanvl(f1 float8, f2 float8)<br/>6.函数nanvl(n1 numeric, n2 numeric) | 执行6条用例，执行结果符合预期，测试通过，无bug |



### 4.1.9 字符处理函数和操作符测试

| 测试函数                                                     | 测试结果                                        |
| ------------------------------------------------------------ | ----------------------------------------------- |
| 1.to_multi_byte<br/>2.to_single_byte<br/>3.dump(any)<br/>4.dump(any, integer)<br/>5.dump(text)<br/>6.dump(src text, fmt integer)<br/>7.orafce.substr<br/>8.strposb<br/>9.orafce.lpad<br/>10.orafce.rpad<br/>11.orafce.ltrim<br/>12.orafce.rtrim<br/>13.orafce.btrim<br/>14.orafce.length<br/>15.orafce.regexp_like<br/>16.orafce.regexp_count<br/>17.orafce.regexp_instr<br/>18.orafce.regexp_substr | 执行18条用例，执行结果符合预期，测试通过，无bug |

### 4.1.10 时间和日期处理函数和操作符测试

| 测试函数                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.months_between<br/>2.round<br/>3.orafce.months_between<br/>4.orafce.to_char<br/>5.orafce.sessiontimezone<br/>6.orafce.dbtimezone | 执行6条用例，执行结果符合预期，测试通过，无bug |

### 4.1.11 条件表达式函数测试

| 测试函数                                                     | 测试结果                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| 1.nvl(expr1, expr2)<br/>2.nvl2(expr1, expr2, expr3)<br/>3.public.decode(expr1, expr2, result1 ,...) | 执行3条用例，执行结果符合预期，测试通过，无bug |

### 4.1.12 聚合函数测试

| 测试函数      | 测试结果                                       |
| ------------- | ---------------------------------------------- |
| 1.median(col) | 执行1条用例，执行结果符合预期，测试通过，无bug |

### 4.1.13 whale插件安装测试

| 安装测试 | 测试步骤                                                     | 测试结果                                       |
| -------- | ------------------------------------------------------------ | ---------------------------------------------- |
| OM安装   | 1.om安装方式安装数据库<br/>2.创建A兼容性数据库<br/>create database dbtest dbcompatibility='A';<br/>3.在A库中创建whale插件<br/>create extension whale; | 执行2条用例，执行结果符合预期，测试通过，无bug |
| 编译安装 | 1. 编译安装好openGauss<br/>2. 将whale源码拷贝到openGauss-server源码的<br/>contrib目录下<br/>3. 进入whale目录执行make install<br/>4. 创建A库并使用初始用户连接A库，创建whale插件<br/>create extension whale; | 执行1条用例，执行结果符合预期，测试通过，无bug |

### 4.1.14 guc参数测试

| GUC参数                         | 测试步骤                                                     | 测试结果                                       |
| ------------------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| whale.a_compatibility_mode      | set whale.a_compatibility_mode = on;<br/>show whale.a_compatibility_mode; | 执行1条用例，执行结果符合预期，测试通过，无bug |
| whale.timezone                  | set whale.timezone = GMT;<br/>show whale.timezone;           | 执行1条用例，执行结果符合预期，测试通过，无bug |
| whale.nls_date_format           | set whale.nls_date_format = 'YYYY-MM-DD HH24:MI:SS';<br/>show whale.nls_date_format; | 执行1条用例，执行结果符合预期，测试通过，无bug |
| whale.varchar2_null_safe_concat | set whale.varchar2_null_safe_concat = on;<br/>show whale.varchar2_null_safe_concat; | 执行1条用例，执行结果符合预期，测试通过，无bug |



### 4.1.15 资料测试

| 测试步骤                             | 测试结果                  |
| ------------------------------------ | ------------------------- |
| 1.资料描述是否准确、示例能否正确执行 | 发现1个bug,1个问题已解决回归通过 |



## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 5.1.0 build db2a3a72 | 107        | Passed：102<br>Failed：5 | 5            |
| openGauss 5.1.0 build 4e532edf | 107        | Passed：107<br>Failed：0 | 0            |

数据项说明：

* 缺陷密度：4/1.578kloc(代码行数)=2.53(个/kloc)（5个问题单，其中1个资料单）


## 4.3  后续测试建议

无

# 5   附件

## 5.1 创建及基本用法示例

```sql
openGauss=# create database dbtest1 dbcompatibility='A';
CREATE DATABASE
openGauss=# \c dbtest1
Non-SSL connection (SSL connection is recommended when requiring high-security)
You are now connected to database "dbtest1" as user "gwb0731".
dbtest1=# create extension whale;
CREATE EXTENSION
dbtest1=# select 'test' as src, gms_assert.enquote_literal('test');
 src  | enquote_literal
------+-----------------
 test | 'test'
(1 row)
```