![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述           | 作者     |
| :--------- | ----------- | ------------------ | -------- |
| 2024-03-29 | 1.0         | 特性测试报告初稿完 | chenziyang |

 关键词： 

openGauss、apache-age

摘要：

本文档主要介绍apache-age插件适配openGauss第一阶段测试报告

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

   Apache AGE 是一个开源图数据库引擎，它为所有关系型数据库提供图形数据处理和分析功能，可以访问现有关系数据库中的图形查询建模。本次需求实现openGauss 6.0.0-RC1版本适配Apache AGE插件。

## 1.1 适配openGauss 6.0.0-RC1内核中缺失的AGE所依赖的扩展点
   根据开发交付件识别出结构体、函数、算子扩展点如下：
| 编号 | 需求全名 | 描述 |
| ------ | -------- | -------- |
| 1     |   识别openGuass内核中缺失的AGE所依赖的结构体       |          |
| 2     |   openGauss内核中缺失的AGE所依赖的函数       |  大部分核心函数签名不一致        |
| 3     |   openGauss内核中缺失的AGE所依赖的算子       |  AGE依赖的算子在openGauss内核中无缺失        |
| 4     |   openGauss内核中缺失的AGE所依赖的数据结构       |          |
| 5     |   openGauss内核中在编译AGE时缺少的头文件       |          |
### 1.1.1 openGuass内核中缺失的AGE所依赖的结构体

| 序号 | 结构体                        | 说明                                                         |      解决方案                                                        |
| ---- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | ExtensibleNode                        | An extensible node is a new type of node defined by an extension. The type is always T_ExtensibleNode,while the extnodename identifies the specific type of node. extnodename can be looked up to find the ExtensibleNodeMethods for this node type. | 可扩展点是由扩展定义的新型节点。类型始终为T_ExtensibleNode，而extnodename标识了特定类型的节点，可查找extnodename以找到此节点类型的可扩展节点方法，将这个结构体定义在了插件侧                   |
| 2    | CustomExecMethods                 | Execution-time methods for a CustomScanState. this is more complex than what we need for a custom path or scan. | 使用openGauss内核中的ExtensibleExecMethods                       |
| 3    | CustomScanState        | 对应CustomScan                                                   | 使用openGauss内核中的ExtensiblePlanState                                                      |
| 4    | CustomScan                   | CustomScan nodes are used to execute custom code within executor.   | 使用openGauss内核中的ExtensiblePlan                               |
| 5    | RangeTblFunction                  | RangeTblEntry subsidiary data for noe function in a FUNCTION RTE. | 修改AGE源码对这个结构体不做解析，因为openGauss语法解析不会解析出这个结构体                         |



### 1.1.2 openGauss内核中缺失的AGE所依赖的数据结构

| 序号 | 文件                                      | 数据结构                                                        |    说明及解决方案                                                          |
| ---- | ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | src/include/nodes/parsenodes_common.h                                   | 详见源码                         | Allow omitting one or both boundaries in an array slice specifier. 参考PG6efbded6e4672c597a6f0dc0f09263e7db7369ff 2015.12.23                                          |
| 2    | src/gausskernel/optimizer/commands/dropcmds.cpp                       | 详见源码             | openGauss drop extension 命令只支持部分插件，修改openGauss的源码，使得drop extension age命令能够通过                                       |
| 3    | src/common/backend/catalog/pg_aggregate.cpp                | 详见源码 | age中聚合函数的支持     |
| 4    | src/common/backend/parser/parse_collate.cpp                          | 详见源码 | AGE解析出的查询树此结构可能为空，导致查询崩溃，postgres11源码中也有这样的保护代码，并且增加该逻辑后，openGauss回归测试全部通过 |
| 5    | src/common/backend/parser/parse_clause.cpp              | 详见源码    | cypher的vle查询，lateral逻辑不被当前的lateral逻辑支持。因此采用了postgres中的修改，强化lateral。并且增加该逻辑后，openGauss回归测试全部通过                       |
| 6    | src/common/backend/parser/parse_func.cpp                  | 详见源码 | 并且增加该逻辑后，openGauss回归测试全部       |

### 1.1.3 openGauss内核中在编译AGE时缺少的头文件

1.编译问题
   编译时，因为AGE引用的部分头文件在openGuass安装时，没有将源码中的全部头文件安装到相应的目录，因此AGE编译时会报出头文件找不到问题
2.解决办法
   参考postgis的安装src/include/MakeFile 增加age需要的头文件 安装代码

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 6.0.0 build 7a4ae87f | 2024-02-28   | 2024-03-29   |

| 硬件型号          | 硬件配置信息                                                 | 备注 |
| ----------------- | ------------------------------------------------------------ | ---- |
| x86_64+centOS     | Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz<br/>内存：755GB<br/>硬盘：1T<br/>OS：CentOS Linux release 7.9.2009 (Core) |      |
| aarch64+openEuler | CPU：kunpeng-920 2600MHz<br/>内存：765GB<br/>硬盘：3.0TB<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3     测试结论概述

## 3.1   测试整体结论

​       Apache AGE适配openGauss第一阶段累计发现缺陷6个，其中已验收6个，总体质量良好。

## 3.2   约束说明

- 用户不存在schema ag_catalog。插件会默认创建schema ag_catalog，并在schema中存储相关元数据

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 6       | 0    | 0    | 6    | 0      |
| 百分比 | 100%     | 0%   | 0%  | 100%  | 0%    |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I94XYW](https://e.gitee.com/opengaussorg/dashboard?issue=I94XYW) | 次要     | AGE图数据引擎cypher数据类型，Integer下边界值和Age官网不一致                                      | 已验收   |
| 2    | [I94QFH](https://e.gitee.com/opengaussorg/dashboard?issue=I94QFH) | 次要     | AGE插件按照指导文档编译失败，需要修改指导文档 | 已验收   |
| 3    | [I95Z8W](https://e.gitee.com/opengaussorg/dashboard?issue=I95Z8W) | 次要     | cypher列表函数toBooleanList(list)执行报错                        | 已验收   |
| 4    | [I99BTR](https://e.gitee.com/opengaussorg/dashboard?issue=I99BTR) | 次要     | Age图数据库，对应各种语言的驱动缺少使用指南，无法正确使用                          | 已验收   |
| 5    | [I961B8](https://e.gitee.com/opengaussorg/dashboard?issue=I961B8) | 次要     | 使用jdbc创建图，然后使用jdbc执行清除图的语句，清理后有残留数据 | 已验收   |
| 6    | [I95Z1O](https://e.gitee.com/opengaussorg/dashboard?issue=I95Z1O) | 次要     | age图库创建图表后，使用delete删除对应的图名称后，导致数据清理有残留，使用清理图表的函数执行失败                   | 已验收   |

# 5     测试执行
 
## 5.1 测试执行步骤

### 5.1.1 功能测试

#### 5.1.1.1 安装测试、基础创建能力

| 测试点                                                       | 执行结果             |
| 编译安装、自动化脚本安装、创建age插件、图表操作               | 执行6条，发现问题1个            |

#### 5.1.1.2 数据类型、实体测试

| 测试点                                                       | 执行结果             |
| 简单数据类型（null、integer、float、numeric、bool、string）<br/>复合类型（List、Map）<br/> 实体操作               | 执行8条，发现问题1个            |

#### 5.1.1.3 DML语句，各种子句测试

| 测试点                                                       | 执行结果             |
| cypher增删改查<br/> 节点查询<br/>关系查询<br/>with子句<br/>unwind子句<br/>merge用法等              | 执行15条，未发现问题            |

#### 5.1.1.4 函数测试

| 测试点                                                       | 执行结果             |
| 谓词函数、列表函数、标量函数、聚合函数等                                                       | 执行8条，发现问题1个            |

#### 5.1.1.5 清理图表测试

| 测试点                                                       | 执行结果             |
| 清理图表                                                       | 执行3条，发现问题1个            |

#### 5.1.1.6 JDBC驱动测试

| 测试点                                                       | 执行结果             |
| cypher增删改查、包含预编译场景<br/>图表创建与清理                                                     | 执行10条，发现问题2个            |

### 5.1.2 AGE 1.0.0 官方用例执行

AGE官方提供了18个回归测试用例，全部通过

| 用例名称              | 测试点                                                       | 执行结果             |
| --------------------- | ------------------------------------------------------------ | -------------------- |
| age_load.sql      | CSV数据导入 | 通过                 |
| agtype.sql | agtype数据类型支持                                     | 通过                 |
| catalog.sql              | 图谱和点边标签的创建删除修改操作 | 通过 |
| cypher_create.sql           | cypher的create语法 | 通过                 |
| cypher_delete.sql           | cypher的delete语法 | 通过                 |
| cypher_match.sql           | cypher的match语法 | 通过                 |
| cypher_merge.sql           | cypher的merge语法 | 通过                 |
| cypher_remove.sql           | cypher的remove语法 | 通过                 |
| cypher.sql           | cypher的基本语法（只包含return） | 通过                 |
| cypher_union.sql           | cypher的union语法 | 通过                 |
| cypher_unwind.sql           | cypher的unwind语法 | 通过                 |
| cypher_vle.sql           | cypher的vle语法 | 通过                 |
| cypher_with.sql           | cypher的with语法 | 通过                 |
| drop.sql           | AGE扩展的创建和删除测试 | 通过                 |
| expr.sql           | cypher的语法解析中的expr测试 | 通过                 |
| graphid.sql           | agtype中graphid的特别测试 | 通过                 |
| scan.sql           | cypher的此法解析测试 | 通过                 |



### 5.1.4资料测试

| 测试步骤：                               | 测试结果               |
| ---------------------------------------- | ---------------------- |
| 1.校验资料的描述及示例的执行结果是否成功 | 提交issue2个，均已验收 |



## 5.2  测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果              | 发现问题单数 |
| ------------------------------ | ---------- | ------------------------- | ------------ |
| openGauss 6.0.0 build 7a4ae87f | 50        | Passed：44<br/>Failed：6 | 6           |
| openGauss 6.0.0-RC1 build 93cd11e9 | 6         | Passed：6<br/>Failed：0  | 0            |

*数据项说明：*

- 第一阶段累计发现缺陷单6个，已验收6个
- 缺陷密度为6个(缺陷个数)/226.498kloc(代码行数)=0.026(个/kloc)

## 5.3   后续测试建议

第二阶段代码转测后，需要对代码进行回归测试，对代码进行性能测试，对代码进行稳定性测试。

# 6     附件

### 需求pr
https://e.gitee.com/opengaussorg/repos/opengauss/Plugin/pulls/1209
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4429
https://e.gitee.com/opengaussorg/repos/opengauss/Plugin/pulls/1366
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4862
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4893
https://e.gitee.com/opengaussorg/repos/opengauss/Plugin/pulls/1387

### 使用示例
```sql 
-- 创建插件
openGauss=# create extension age; 
CREATE EXTENSION

-- 加载插件
openGauss=# load 'age';
LOAD

-- 设置查询空间
openGauss=# SET search_path TO ag_catalog;
SET

-- 创建图空间
openGauss=# SELECT create_graph('test');
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "_ag_label_vertex_pkey" for table "_ag_label_vertex"
CONTEXT:  referenced column: create_graph
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "_ag_label_edge_pkey" for table "_ag_label_edge"
CONTEXT:  referenced column: create_graph
NOTICE:  graph "test" has been created
CONTEXT:  referenced column: create_graph
 create_graph
--------------

(1 row)

-- 执行cypher语句
openGauss=# SELECT * FROM cypher('test', $$CREATE (:v {i: 0})$$) AS (a agtype);
a
---
(0 rows)

openGauss=# SELECT * FROM cypher('test', $$MATCH (n:v) RETURN n$$) AS (n agtype);
n
-----------------------------------------------------------------------
{"id": 844424930131969, "label": "v", "properties": {"i": 0}}::vertex
(1 row)
```

