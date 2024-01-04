![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期       | 修订 版本 | 修改描述 | 作者        |
| ---------- | --------- | -------- | ----------- |
| 2023.11.15 | V1.0      | 初稿     | yangyixiang |

关键词：

MySQL、dolphin、chameleon

摘要：

本文档主要介绍基于内核及dolphin插件已兼容的MySQL特性，对迁移工具进行适配修改，并给出最终测试结论

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

该特性主要基于内核及dolphin插件已兼容的MySQL特性，对迁移工具进行适配修改，主要包括工具在数据类型、DDL、字段、表名、表数据、大小写等方面进行适配

# 2 特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 5099d94c | 2023-11-8    | 2023-11-14   |
| openGauss 5.1.1 build de769317 | 2023-11-14   | 2023-11-16   |

硬件环境信息：

| 硬件型号 | 硬件配置信息                                                 | 备注                            |
| -------- | ------------------------------------------------------------ | ------------------------------- |
| 虚拟机   | Architecture：x86_64<br/>CPU：Inter(R) Xeon(R) Gold 6278C 8核<br/>内存： 31GB<br/>硬盘：99G<br/>OS：CentOS Linux release 7.7.1908 (Core) | openGauss 5.1.1                 |
| 虚拟机   | Architecture：x86_64<br/>CPU：Inter(R) Xeon(R) Gold 6278C 8核<br/>内存： 31GB<br/>硬盘：724G<br/>OS：CentOS Linux release 7.6.1908 (Core) | MySQL 5.7.35<br>chameleon 5.1.1 |

# 3 测试结论概述

## 3.1 测试整体结论

MySQL迁移工具适配内核兼容性，共执行用例39个，主要覆盖了功能测试、资料测试。功能测试覆盖迁移普通表，表字段类型覆盖整数类型、日期类型、二进制类型、浮点类型，适配最新数据类型映射。迁移DDL语句覆盖建表、函数、触发器、视图、存储过程的内核及dolphin插件已兼容的MySQL语法。覆盖配置参数lower_case_table_names迁移大小写名称的对象。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单2个，2个缺陷均已解决且回归通过，整体质量良好。

## 3.2 约束说明

1. MySQL5.7及以上版本
2. MySQL参数设置：log_bin=on、binlog_format=row、binlong_row_image=full、gtid_mode=on
3. 迁移工具目标端openGauss的数据库应设置为使用dolphin插件的b库，b库下建议打开的参数：dolphin.b_compatibility_mode=on、enable_set_variable_b_format=on
4. 为保证大小写一致性，迁移时需保证MySQL的大小写参数lower_case_table_names和openGauss的大小写参数dolphin.lower_case_table_names保持一致
5. sql_mode设置openGauss与MySQL保持一致
6. 对象的在线DDL操作，DDL语句是直接透传到openGauss端的，对于MySQL与openGauss不兼容的语法将报错

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I8DNV7](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I8DNV7?from=project-issue) | 次要 | 全量迁移mysql对象，部分未适配内核兼容语法                    | 已验收 |
| 2    | [I8I0GY](https://gitee.com/opengauss/openGauss-tools-chameleon/issues/I8I0GY?from=project-issue) | 次要 | MySQL创建表并指定表的字符集为ascii，全量迁移至openGauss侧，表未适配字符集SQL_ASCII | 已验收 |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 迁移普通表适配新数据类型映射

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. MySQL侧创建普通表并插入数据，表字段类型覆盖整数类型、日期类型、二进制类型、浮点类型，迁移表及数据至openGauss侧，查看openGauss侧表结构及数据是否符合新的数据类型映射 | 执行4条用例，未发现bug |

### 4.1.2 迁移表适配建表DDL兼容语句

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. MySQL侧创建表，建表语句覆盖分区表函数作为分区表条件、指定分区键值less than (maxvalue)语法，创表时创建comment注释，创建前缀索引，创建timestamp on update约束、autoincrement约束，指定表、列的字符集和字符序。迁移表至openGauss，查看表结构是否适配已兼容的MySQL语法 | 执行5条用例，发现1个bug |

### 4.1.3 迁移对象适配对象创建DDL兼容语句

| 测试步骤                                                     | 测试结果                 |
| ------------------------------------------------------------ | ------------------------ |
| 1. MySQL创建对象，覆盖创建函数添加definer属性、comment注释，指定库名创建触发器并添加definer属性、comment注释，创建视图添加definer属性、comment注释、指定algorithm，创建存储过程，覆盖语法指定definer属性、while循环带标签、case when condition控制语法、if then控制语法、cursor相关语法、declare condition语法、do statement语法、repeat子句、create procedure select语法、return子句、declare同时声明多个变量、replace语法、language sql语句、deterministic语法、sql security语句、contains sql、no sql、reads sql data、modifies sql data语法、loop循环语法、leave label、iterate label、comment注释，不同语法之间互相结合创建存储过程。创建对象使用'user'@'host'语法。迁移对象至openGauss，查看对象DDL是否适配已兼容的MySQL语法 | 执行20条用例，发现1个bug |

### 4.1.4 迁移表和对象区分大小名称适配

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 配置两侧数据库lower_case_table_names参数为0，MySQL侧创建区分大小的表名和对象名，迁移至openGauss侧，查看两侧名称是否一致<br>2. 配置两侧数据库lower_case_table_names参数为1，MySQL侧创建不区分大小的表名和对象名，迁移至openGauss侧，查看两侧名称是否一致<br>3. 两侧数据库lower_case_table_names参数不一致，MySQL侧创建区分大小的表名和对象名，迁移至openGauss侧，查看重复的名称是否合理报错 | 执行10条用例，未发现bug |

## 4.2 资料测试

| 测试步骤                | 测试结果                              |
| ----------------------- | ------------------------------------- |
| 1. 文档描述 2. 文档示例 | 文档描述准确，示例正确无误，未发现bug |

## 4.5 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.1 build 5099d94c | 36         | Passed：36 Failed：6 | 2            |
| openGauss 5.1.1 build de769317 | 39         | Passed：39 Failed：0 | 0            |

*数据项说明：*

- 累计发现缺陷单2个，2个缺陷均已解决且回归通过
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：2(缺陷个数)/1.95kloc(代码行数)=1.03(个/kloc)

## 4.6 后续测试建议

无

# 5 附件

无
