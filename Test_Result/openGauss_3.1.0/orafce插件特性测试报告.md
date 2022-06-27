![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者      |
| ---------- | ----------- | -------------------- | --------- |
| 2022-05-19 | 1.0         | 特性测试报告初稿完成 | kangyao59 |
| 2022-06-22 | 1.1         | 修改报告描述         | kangyao59 |

 关键词：orafce

摘要： 本文档主要介绍了orafce插件对openGauss数据库支持情况。

缩略语清单：

| 缩略语 | 英文全名                                      | 中文解释                   |
| ------ | --------------------------------------------- | -------------------------- |
| orafce | Oracle’s compatibility functions and packages | Oracle的兼容性函数和软件包 |

# 1     特性概述
对orafce插件进行编译、改造，支持在openGauss上使用，支持将Oracle的高级包、函数、部分系统视图等以插件形式兼容到openGauss。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build 4b6643fb | 2022-05-07   | 2022-05-17   |
| orafce 3.17                    | 2022-05-07   | 2022-05-17   |

描述特性测试的硬件环境信息

| 硬件型号         | 硬件配置信息                                                 | 备注 |
| ---------------- | ------------------------------------------------------------ | ---- |
| OpenStack   Nova | CPU:  Intel(R) Xeon(R)   Gold 6266C CPU @ 3.00GHz <br>内存：16GB<br/>硬盘：ssd<br/>OS：Centos Linux 7<br/>文件系统：ext3 |      |

# 3     测试结论概述

## 3.1   测试整体结论

orafce for openGauss 共计执行150条用例，主要覆盖功能测试。功能包含兼容Oracle的包、函数、系统视图等特性。资料测试覆盖校验资料的描述及示例的执行结果。发现问题均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动   | 活动评价                                       |
| ---------- | ---------------------------------------------- |
| 功能测试   | 编译库、编译插件，执行结果符合预期，通过       |
| 功能测试   | om安装库、安装插件，执行结果符合预期，通过     |
| 功能测试   | 开启线程池场景下，执行用例结果符合预期，通过   |
| 功能测试   | 兼容包测试结果符合预期，通过                   |
| 功能测试   | 兼容函数测试结果符合预期，通过                 |
| 功能测试   | 兼容系统视图测试结果符合预期，通过             |
| 功能测试   | 操作符测试结果符合预期，通过                   |
| 可靠性测试 | 可靠性测试结果符合预期，通过                   |
| 资料测试   | 资料描述准确，示例的执行结果正确，整体质量良好 |

## 3.2   约束说明

1、插件的删除需要设置参数：support_extended_features=true（需要重启数据库生效）

2、以下函数需要在数据库字符集为utf8的场景下才能通过

	to_multi_byte(text)
	to_single_byte(text)
	oracle.unistr

3、orafce插件兼容的nlssort函数与openGauss自带底层实现有差异，以openGauss为主

4、兼容A库，不考虑其他库的兼容性

5、openGauss自带的listagg功能（语法关键字非函数）必须在pg_catalog模式才能符合orafce官方用例期望

6、数据库内核变动，可能需要重新编译orafce插件

7、开启线程池需要修改 enable_thread_pool = on  和  thread_pool_attr = '0, 0, (allbind)'  然后重启数据库

8、使用包功能需要加schema名称，schema名称与包名同名，涉及如下包：

~~~
dbms_output
utl_file
dbms_utility
dbms_random
dbms_alert
dbms_assert
dbms_pipe
~~~

9、以下函数使用的时候需要加模式名

| 模式       | 函数                       |
| ---------- | -------------------------- |
| oracle     | months_between             |
| plvdate    | bizdays_between            |
| plvstr     | betwn                      |
| plvlex     | tokens                     |
| plvsubst   | string                     |
| plunit     | assert_true                |
| plunit     | assert_false               |
| plunit     | assert_null                |
| plunit     | assert_not_null            |
| plunit     | assert_not_equals          |
| plunit     | fail()                     |
| oracle     | add_days_to_timestamp      |
| oracle     | replace_empty_strings      |
| oracle     | replace_null_strings       |
| oracle     | unistr                     |
| oracle     | translate_oracle_modifiers |
| oracle     | regexp_count               |
| oracle     | regexp_instr               |
| oracle     | regexp_replace             |
| oracle     | regexp_like                |
| oracle     | regexp_substr              |
| oracle     | subtract                   |
| oracle     | add_months                 |
| oracle     | last_day                   |
| oracle     | lpad                       |
| oracle     | rpad                       |
| oracle     | ltrim                      |
| oracle     | rtrim                      |
| oracle     | btrim                      |
| pg_catalog | listagg                    |

10、以下兼容视图使用时需要加模式名

| 模式   | 视图                      |
| ------ | ------------------------- |
| oracle | get_major_version         |
| oracle | get_major_version_num     |
| oracle | get_full_version_num      |
| oracle | get_platform              |
| oracle | get_status                |
| oracle | user_tab_columns          |
| oracle | user_tables               |
| oracle | user_cons_columns         |
| oracle | user_constraints          |
| oracle | product_component_version |
| oracle | user_objects              |
| oracle | user_procedures           |
| oracle | user_source               |
| oracle | user_views                |
| oracle | user_ind_columns          |
| oracle | dba_segments              |

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       | NA       | NA       | NA                 | NA       |

### 3.3.2 问题统计

|        | 问题总数 | 严重   | 主要  | 次要 | 不重要 |
| ------ | -------- | ------ | ----- | ---- | ------ |
| 数目   | 9        | 1      | 8     | 0    | 0      |
| 百分比 | 100%     | 11.11% | 88.89% |  0%    | 0%       |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I596FK](https://gitee.com/opengauss/Plugin/issues/I596FK?from=project-issue) | 主要     | orafce插件代码删减                                           | 已验收   |
| 2    | [I5913Q](https://gitee.com/opengauss/Plugin/issues/I5913Q?from=project-issue) | 主要     | orafce插件安装和删除脚本                                     | 挂起     |
| 3    | [I57K5B](https://gitee.com/opengauss/Plugin/issues/I57K5B?from=project-issue) | 严重     | 删除插件的时候报错                                           | 已验收   |
| 4    | [I5901A](https://gitee.com/opengauss/Plugin/issues/I5901A?from=project-issue) | 主要     | orafce官方nlssort用例报错                                    | 已验收   |
| 5    | [I58ZKH](https://gitee.com/opengauss/Plugin/issues/I58ZKH?from=project-issue) | 主要     | 官方的listagg用例报错                                        | 已验收   |
| 6    | [I57AFC](https://gitee.com/opengauss/Plugin/issues/I57AFC?from=project-issue) | 主要     | to_single_byte和to_multi_byte功能与预期不符                  | 已验收   |
| 7    | [I574MW](https://gitee.com/opengauss/Plugin/issues/I574MW?from=project-issue) | 主要     | wm_concat函数报错                                            | 已验收   |
| 8    | [I56JJB](https://gitee.com/opengauss/Plugin/issues/I56JJB?from=project-issue) | 主要     | to_date(text)函数报错                                        | 已验收   |
| 9    | [I59087](https://gitee.com/opengauss/Plugin/issues/I59087?from=project-issue) | 主要     | dump(text, integer)针对integer为1008，1010，1016，1017会报错 | 已取消   |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 编译库及插件执行测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 编译安装openGauss数据库<br/>2. 编译orafce插件 <br/>3. 安装插件 <br/>4. 执行用例 <br/>5. 删除orafce插件 | 执行1条用例，提交2条问题<br/>1个问题已修复且验证通过，执行结果符合预期<br/>另外1个问题经sig讨论后挂起 |

### 4.1.2 开启线程池下测试执行
| 测试步骤：                                                   | 测试结果                      |
| ------------------------------------------------------------ | ----------------------------- |
| 1. 开启线程池<br/>2. 安装orafce插件<br/>3. 执行用例<br/>4. 删除orafce插件 | 执行1条用例，执行结果符合预期 |

### 4.1.3 兼容包测试执行
| 测试步骤：                                                   | 测试结果                       |
| ------------------------------------------------------------ | ------------------------------ |
| 1. 安装orafce插件<br/>2. 执行程序包相关用例<br/>3. 环境清理<br/>4. 删除orafce插件 | 执行52条用例，执行结果符合预期 |

### 4.1.4 兼容函数测试执行

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 安装orafce插件<br/>2. 执行兼容函数相关用例<br/>3. 环境清理<br/>4. 删除orafce插件 | 执行90条用例，发现7个问题<br/>6条已修复且验收通过，1条非问题已取消，执行结果符合预期 |

### 4.1.5 兼容系统视图测试执行

| 测试步骤：                                                   | 测试结果                      |
| ------------------------------------------------------------ | ----------------------------- |
| 1. 安装orafce插件<br/>2. 执行兼容系统视图相关用例<br/>3. 环境清理<br/>4. 删除orafce插件 | 执行3条用例，执行结果符合预期 |

### 4.1.6 操作符测试执行

| 测试步骤：                                                   | 测试结果                      |
| ------------------------------------------------------------ | ----------------------------- |
| 1. 安装orafce插件<br/>2. 执行操作符相关用例<br/>3. 环境清理<br/>4. 删除orafce插件 | 执行1条用例，执行结果符合预期 |

### 4.1.7 可靠性测试执行

| 测试步骤：                                                   | 测试结果                      |
| ------------------------------------------------------------ | ----------------------------- |
| 1. 安装orafce插件<br/>2. 执行可靠性测试用例<br/>3. 删除orafce插件 | 执行2条用例，执行结果符合预期 |

### 4.1.8 om安装库及插件执行测试

| 测试步骤：                                                   | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. om安装openGauss数据库<br/>2. 拷贝编译好的插件到对应目录下<br/>3. 安装orafce插件<br/>4. 执行用例 <br/>5. 删除orafce插件 | 执行149条用例，执行结果符合预期 |

##  4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果           | 发现问题单数 |
| ------------------------------ | ---------- | ---------------------- | ------------ |
| openGauss 3.0.0 build 4b6643fb | 150        | Passed：150  Failed：0 | 9            |

说明：发现9个问题单，7条已修改并验证通过，1条非问题已取消，1条经sig讨论挂起。

## 4.3   后续测试建议

1、orafce兼容的dump(text, integer)函数的integer参数没有支持1008、1010、1016、1017，这是orafce本身的缺陷，需要评估后期是否需要支持

	https://www.w3cschool.cn/oraclejc/oraclejc-7pw82r71.html

# 5     附件

无
