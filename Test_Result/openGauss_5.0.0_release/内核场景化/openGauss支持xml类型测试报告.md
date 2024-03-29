![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者     |
| ---------- | -------- | -------------------- | -------- |
| 2023-03-17 | 1.0      | 特性测试报告初稿完成 | songjing |
| 2023-03-23 | 1.1      | 根据检视意见修改 | songjing |

 关键词： xml类型

摘要：

本文档主要介绍对openGauss默认支持xml类型进行的测试，并给出测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| N/A    |          |          |

# 1 特性概述

该特性主要是openGauss源码包通过make或者cmake编译安装和安装包方式安装，启动数据库，都可以默认支持xml数据类型。

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build c3e3237f | 2023-02-23   | 2023-03-01   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：32GB<br />硬盘：100GB<br />OS：CentoOS Linux release 7.6.1810 (Core) |      |

# 3  测试结论概述

## 3.1 测试整体结论

openGauss支持xml类型特性，共计执行用例58条，主要覆盖了功能测试、升级测试和资料测试。功能测试主要包括xml取值校验，验证xml类型作为表字段数据类型进行增删改查、创建索引、约束等操作，验证数据类型隐/显式转换，xml类型结合函数、存储过程、自定义类型进行测试，测试导入导出xml类型数据，使用JDBC/ODBC连接数据库对xml类型字段增删改查操作；升级测试主要验证升级后支持xml类型且回滚后不支持；资料测试覆盖资料描述是否准确及示例是否正确执行。累计发现有效问题1个，已修复且回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 验证xml类型在创建/修改表时作为字段类型，通过                 |
| 功能测试 | xml类型字段添加约束、创建索引、视图，DML操作，xml取值校验，通过 |
| 功能测试 | 数据类型间隐式、显示转换，通过                               |
| 功能测试 | xml类型结合函数、存储过程，自定义类型测试，通过              |
| 功能测试 | 导入导出包含xml类型数据的表，通过                            |
| 功能测试 | 测试JDBC/ODBC驱动支持xml类型数据操作                         |
| 升级测试 | 升级后支持该特性，回滚后不支持，通过                         |
| 资料测试 | 资料描述是否准确、约束覆盖是否全面以及示例是否正确，通过     |

## 3.2 约束说明

无

## 3.3  遗留问题分析

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

| 序号 | issue号                                                      | 问题级别 | 问题简述                                 | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ---------------------------------------- | -------- |
| 1    | [I6JAKE](https://gitee.com/opengauss/openGauss-server/issues/I6JAKE?from=project-issue) | 不重要   | xml类型资料描述有误，xml类型函数无超链接 | 已验收   |

# 4 测试执行

## 4.1 功能测试

#### 4.1.1 验证xml类型在创建/修改表时作为字段类型

| 测试步骤                                                     | 测试结果                   |
| ------------------------------------------------------------ | -------------------------- |
| 1.创建不同类型的表，字段类型使用xml数据类型<br/>2.修改表添加/删除xml类型字段，修改字段类型 | 执行12条用例，结果符合预期 |

#### 4.1.2 xml类型字段添加约束、创建索引、视图，DML操作，xml取值校验

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1.创建表，xml类型字段添加约束<br/>2.修改表xml类型字段添加表约束，xml类型字段上创建索引<br/>3.创建视图包含表中xml类型字段<br/>4.对表xml类型字段进行DML操作，校验xml取值 | 执行23条用例，结果符合预期 |

#### 4.1.3 数据类型间隐式、显示转换

| 测试步骤：                                                   | 测试结果                  |
| ------------------------------------------------------------ | ------------------------- |
| 1.测试xml类型与其他类型隐式转换<br/>2.测试xml类型与其他类型显式转换 | 执行6条用例，结果符合预期 |

#### 4.1.4 xml类型结合函数、存储过程，自定义类型测试

| 测试步骤：                                                   | 测试结果                   |
| ------------------------------------------------------------ | -------------------------- |
| 1.xml类型结合自定义函数、存储过程，自定义类型测试<br/>2.xml类型结合窗口函数、字符处理函数等系统函数测试 | 执行10条用例，结果符合预期 |

#### 4.1.5 导入导出xml类型数据

| 测试步骤：                                                   | 测试结果                  |
| ------------------------------------------------------------ | ------------------------- |
| 1.创建表，包含xml数据类型字段，插入数据<br/>2.使用copy ，gs_basebackup，gs_dump导出表<br>3.清空表数据，使用gsql，gs_restore恢复表数据 | 执行4条用例，结果符合预期 |

#### 4.1.6 测试JDBC/ODBC驱动支持xml类型数据操作

| 测试步骤：                                                   | 测试结果                  |
| ------------------------------------------------------------ | ------------------------- |
| 1.分别使用JDBC和ODBC连接数据库，对xml类型表insert、update、delete、select操作 | 执行2条用例，结果符合预期 |

## 4.2  升级测试

| 测试步骤：                                                   | 测试结果                             |
| ------------------------------------------------------------ | ------------------------------------ |
| 1.安装3.0.0版本数据库（无本特性）<br/>2.升级数据库到包含本特性的数据库版本<br/>3.验证是否支持xml类型<br/>4.回滚后验证是否支持xml类型 | 执行1条用例，未发现bug，结果符合预期 |

## 4.3  资料测试

| 测试步骤：                                           | 测试结果                      |
| ---------------------------------------------------- | ----------------------------- |
| 1.资料描述是否准确、约束覆盖是否全面以及示例是否正确 | 发现1个问题，已修复，验收通过 |

## 4.4 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 5.0.0 build c3e3237f | 58         | Passed：58<br/>Failed：0 | 1            |

数据说明

1.  测试过程中共发现1个缺陷，为资料缺陷，已修复且验收通过
2.  以上资料问题不计入代码缺陷密度，缺陷密度：0个(缺陷个数)/0.09kloc(代码行数)=0(个/kloc)

## 4.5 后续测试建议

无

# 5 附件 

## 5.1 示例

```sql
--创建表，字段数据类型为xml，插入数据并查询
openGauss=# create table testtb(c_xml xml);
CREATE TABLE
openGauss=# insert into testtb values('test xml');
INSERT 0 1                                    ^
openGauss=# insert into testtb values(xmlelement(name foo, xmlattributes(current_date as bar), 'cont', 'ent'));
INSERT 0 1
openGauss=# select * from testtb;
                c_xml
-------------------------------------
 test xml
 <foo bar="2023-03-23">content</foo>
(2 rows)

--更新字段值，在非文档格式的值后串接123
openGauss=# update testtb set c_xml = xmlconcat(c_xml, '123') where c_xml is not document;
UPDATE 1
openGauss=# select * from testtb;
                c_xml
-------------------------------------
 <foo bar="2023-03-21">content</foo>
 test xml123
(2 rows)

openGauss=# delete from testtb ;
DELETE 2

--xml解析方式为content
openGauss=# set xmloption to content;
SET
openGauss=# select '<root1>a</root1><root2>b</root2>'::xml;
               xml
----------------------------------
 <root1>a</root1><root2>b</root2>
(1 row)

--xml解析方式为document，xml值为非文档格式时会报错
openGauss=# set xmloption to document;
SET
openGauss=# select '<root1>a</root1><root2>b</root2>'::xml;
ERROR:  invalid XML document
LINE 1: select '<root1>a</root1><root2>b</root2>'::xml;
               ^
DETAIL:  line 1: Extra content at the end of the document
<root1>a</root1><root2>b</root2>
                ^
CONTEXT:  referenced column: xml
openGauss=# select '<root><child>abc</child><child2 /></root>'::xml;
                    xml
-------------------------------------------
 <root><child>abc</child><child2 /></root>
(1 row)

```