![avatar](https://gitee.com/opengauss/QA/raw/73f2da3e363486e074e4b63614ff4b23995c9b01/images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode。)

修订记录

| 日期       | 修订 版本 | 修改描述           | 作者            |
| ---------- | --------- | ------------------ | --------------- |
| 2023-11-15 | 1.0       | 测试报告初稿完成   | zhanghuan96     |
| 2023-12-11 | 2.0       | 修改缺陷状态及描述 | zou_jialiang050 |

关键词：

A兼容性、PL/SQL编译功能拓展

摘要：

本文档是在A兼容性下PL/SQL编译功能拓展，主要实现两个特性，一是openGauss用户在创建存储过程时可以无视依赖关系的前后顺序或者无法避免的循环依赖导致繁琐的创建操作序列，使用更加简单高效的操作创建存储过程对象；二是可以在修改存储过程对象依赖的type或者表字段类型后，通过支持重编译的语法，保证执行存储过程时结果的正确性。并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1 特性概述

在A兼容模式下通过设置参数behavior_compat_options为plpgsql_dependency，实现两个特性，（1）用户可以在修改存储过程依赖的自定义type或表字段类型后，通过对存在依赖的存储过程进行重编译，保证调用存储过程不报错以及运行结果的准确性；（2）用户在创建存在互相依赖关系的PLSQL对象时，可以忽略依赖关系的约束进行创建。

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 96aca89e | 2023-10-26   | 2023-11-2    |
| openGauss 5.1.1 build 5099d94c | 2023-11-9    | 2023-11-9    |
| openGauss 5.1.1 build 01b191f0 | 2023-12-8    | 1023-12-8    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3 测试结论概述

## 3.1 测试整体结论

openGauss支持忽略PLSQL间的依赖进行创建，共计执行50条用例，主要覆盖了功能测试和资料测试。功能测试覆盖设置behavior_compat_options='plpgsql_dependency'及behavior_compat_options=''下测试该需求。资料测试验证资料描述及示例是否正确。累计发现缺陷单3个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 参数behavior_compat_options不同设置方式测试，测试通过        |
| 功能测试 | 存储过程的入参，指定的是自定义type或表字段的%type，修改自定义type（如增加类型属性等）或表字段类型，调用存储过程，测试通过 |
| 功能测试 | 存在嵌套依赖关系（如存储过程依赖自定义type、type依赖表字段），修改type或者表字段数据类型，未进行手动编译，调用报错，进行手动编译后，调用成功，测试通过 |
| 功能测试 | 创建存储过程时使用未定义对象（如存储过程创建时存在互相依赖对方定义的类型或函数），测试系统表是否记录对象依赖关系并提供告警功能，测试通过 |
| 功能测试 | compile、specification作为非保留关键字测试，测试通过         |
| 功能测试 | pg_object系统表新增valid字段验证，gs_dependencies、gs_dependencies_object系统表测试，测试通过 |
| 资料测试 | 验证资料描述及示例正确，测试通过                             |

## 3.2 约束说明

- openGauss的A兼容模式下使用
- 本需求实现需设置guc参数behavior_compat_options值为'plpgsql_dependency'，默认不设置该参数
- rename类型和rename类型的属性，若存在依赖时，不允许操作

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       | NA       | NA       | NA                 | NA       |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 0    | 3    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I8BV38](https://gitee.com/opengauss/openGauss-server/issues/I8BV38?from=project-issue) | 次要     | 不设置参数behavior_compat_options='plpgsql_dependency'，修改依赖的自定义类型后，调用存储过程传入新增的参数，报错，预期调用成功，不打印新增的参数值 | 已验收   |
| 2    | [I8B6QD](https://gitee.com/opengauss/openGauss-server/issues/I8B6QD?from=project-issue) | 次要     | 包体中定义函数，函数体内使用自定义类型的变量，修改自定义类型后，调用函数没有进行自动编译的功能 | 已验收   |
| 3    | [I8H86E](https://gitee.com/opengauss/openGauss-server/issues/I8H86E?from=project-issue) | 次要     | 存储过程的参数类型是自定义类型，修改自定义类型的所属模式，删除类型后重新创建，报错 | 已验收   |

# 4 测试执行

## 4.1 功能测试

| 测试步骤                                     | 测试结果                         |
| -------------------------------------------- | -------------------------------- |
| 1.不同方式设置guc参数behavior_compat_options | 执行5条用例，未发现bug，执行通过 |

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.设置guc参数behavior_compat_options值为plpgsql_dependency<br>2.创建自定义类型<br>3.创建存储过程，指定入参类型是自定义类型<br>4.修改自定义类型属性<br>5.调用存储过程 | 执行10条用例，发现1个bug，已验收通过 |

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.设置guc参数behavior_compat_options值为plpgsql_dependency<br>2.创建表和自定义类型<br>3.创建存储过程，指定入参类型是表字段%type<br>4.修改表字段类型属性<br>5.调用存储过程 | 执行8条用例，未发现bug，执行通过 |

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.不设置guc参数behavior_compat_options值为plpgsql_dependency<br>2.创建表和自定义类型<br>3.创建存储过程，指定入参类型是自定义类型和表字段%type<br>4.修改自定义类型及表字段类型属性<br>5.调用存储过程 | 执行2条用例，发现1个bug，已验收通过 |

| 测试步骤                               | 测试结果                         |
| -------------------------------------- | -------------------------------- |
| 1.compile及specification作为关键字测试 | 执行2条用例，未发现bug，执行通过 |

| 测试步骤                                                     | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.设置guc参数behavior_compat_options值为plpgsql_dependency<br/>2.创建包、存储过程、自定义函数，入参指定自定义type，分别测试自定义type存在及不存在的情况<br>3.查询系统表，pg_object中新增字段valid值以及新增系统表gs_dependencies、gs_dependencies_obj | 执行27条用例，发现1个bug，已验收通过 |

## 4.2 资料测试

| 测试步骤                                                     | 测试结果            |
| ------------------------------------------------------------ | ------------------- |
| 验证资料中behavior_compat_options参数对新增参数值的描述及示例是否正确 | 测试通过，未发现bug |

## 4.2 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.1 build 96aca89e | 50         | Passed: 48 Failed: 2 | 2            |
| openGauss 5.1.1 build 5099d94c | 50         | Passed: 49 Failed: 1 | 1            |
| openGauss 5.1.1 build 01b191f0 | 1          | Passed：1 Failed：0  | 0            |

*数据项说明：*

- 累计发现缺陷单3个，缺陷已验收通过
- 缺陷密度：3(缺陷个数)/18.158kloc(代码行数)=0.165(个/kloc)

## 4.3 后续测试建议

无

# 5 附件

## 5.1 源代码及文档PR

opengauss支持忽略PLSQL间的依赖进行创建源码修改PR：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4251

opengauss支持忽略PLSQL间的依赖进行创建资料修改PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/5846

## 5.2 示例

```sql
testpoint：存储过程的参数类型依赖自定义类型，修改自定义类型属性
--设置参数
set behavior_compat_options='plpgsql_dependency';
--创建自定义类型
create type type_guc0006 as(
id integer,
name varchar,
addr text
);
--创建存储过程
create or replace procedure proc_guc0006(a type_guc0006)
is
begin
raise info 'call a: %',a;
end;
/
--调用存储过程
openGauss=# call proc_guc0006((1,'测试','shanghai'));
INFO:  call a: (1,测试,shanghai)
 proc_guc0006
--------------

(1 row)
--增加type属性
openGauss=# alter type type_guc0006 add attribute a int;
ALTER TYPE
--再次调用，打印新增加的类型值100
openGauss=# call proc_guc0006((1,'测试','shanghai',100));
INFO:  call a: (1,测试,shanghai,100)
 proc_guc0006
--------------

(1 row)
```