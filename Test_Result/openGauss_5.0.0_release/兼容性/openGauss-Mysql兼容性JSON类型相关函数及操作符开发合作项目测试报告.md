![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|    日期    | 修订版本 |       修改描述       |    作者     |
| :--------: | :------: | :------------------: | :---------: |
| 2023-03-24 |   1.0    | 特性测试报告初稿完成 | yangyixiang |

 关键词： 插件dolphin、兼容b库、JSON函数、操作符

 摘要：

本文档主要基于插件dolphin实现兼容MySQL5.7的29个函数及操作符进行测试，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| :----- | :------- | -------- |
| NA     |          |          |

# 1     特性概述

基于插件dolphin，openGauss在兼容B模式下兼容MySQL5.7版本的29个JSON函数及操作符

# 2     特性测试信息

| 版本名称                                                    | 测试起始时间 | 测试结束时间 |
| ----------------------------------------------------------- | ------------ | ------------ |
| MySQL5.7<br/>openGauss 5.0.0 build f7453c1b<br/>dolphin 1.0 | 2023-02-11   | 2023-02-24   |
| MySQL5.7<br/>openGauss 5.0.0 build 35192ba5<br/>dolphin 1.0 | 2023-03-22   | 2023-03-23   |

| 环境信息 | 配置信息                                                     | 备注 |
| :------: | :----------------------------------------------------------- | ---- |
|  虚拟机  | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br />内存：32GB<br />硬盘：100GB<br />OS：CentOS Linux release 7.6.1908 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss-Mysql兼容性JSON类型相关函数及操作符开发合作项目共计执行用例64条，主要覆盖了功能测试和资料测试，功能测试覆盖验证29个JSON函数及操作符基础功能，在行存表、列存表、分区表、临时表、unlogged表、segment表相关语法中验证JSON函数及操作符，在视图、索引、存储过程、自定义函数相关语法中验证JSON函数及操作符，验证json函数是否可以作为表名、视图名、列名、自定义函数名、存储过程名、数据库名、序列名、索引名、用户名、角色名、组名。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。发现问题单13个，均已解决且回归通过，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 验证29个JSON函数及操作符基础功能，通过                       |
| 功能测试 | 在行存表、列存表、分区表、临时表、unlogged表、segment表相关语法中验证JSON函数及操作符，通过 |
| 功能测试 | 在视图、索引、存储过程、自定义函数相关语法中验证JSON函数及操作符，通过 |
| 功能测试 | 验证json函数是否可以作为表名、视图名、列名、自定义函数名、存储过程名、数据库名、序列名、索引名、用户名、角色名、组名，通过 |
| 资料测试 | 资料描述完整准确，整体质量良好                               |

## 3.2   约束说明

- 在B模式下且有dolphin插件，功能正常
- 在B模式下无dolphin插件，无此些函数功能
- 在其他兼容模式下无此类功能

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :---------: | :------: | :--: | :--: | :--: | :----: |
|  数目 (个)  |    13    |  0   |  3   |  9   |   1    |
| 百分比  (%) |   100%   |  0%  | 23%  | 69%  |   8%   |

### 3.3.3 问题详情

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| :--: | ------------------------------------------------------------ | :------: | ------------------------------------------------------------ | :------: |
|  1   | [I6FBG3](https://gitee.com/opengauss/Plugin/issues/I6FBG3?from=project-issue) |   次要   | json_array()、json_object()、json_append()函数在部分场景下opgs和mysql侧结果不一致 |  已验收  |
|  2   | [I6GF6Q](https://gitee.com/opengauss/Plugin/issues/I6GF6Q?from=project-issue) |   主要   | json_array_insert()函数的path入参是不存在的索引，返回结果与M*不一致 |  已验收  |
|  3   | [I6GFBM](https://gitee.com/opengauss/Plugin/issues/I6GFBM?from=project-issue) |   次要   | json_contains()函数的candidate参数无效，opgs侧返回结果与M*不一致 |  已验收  |
|  4   | [I6GFQ5](https://gitee.com/opengauss/Plugin/issues/I6GFQ5?from=project-issue) |   次要   | json_extract()函数部分场景下返回结果与M*侧不一致             |  已验收  |
|  5   | [I6GFWH](https://gitee.com/opengauss/Plugin/issues/I6GFWH?from=project-issue) |   主要   | 部分json函数的json_doc入参无效，opgs侧返回结果与M*不一致     |  已验收  |
|  6   | [I6GI1B](https://gitee.com/opengauss/Plugin/issues/I6GI1B?from=project-issue) |   主要   | 部分json函数的入参含浮点数，opgs侧返回结果精度不正确         |  已验收  |
|  7   | [I6GI6Y](https://gitee.com/opengauss/Plugin/issues/I6GI6Y?from=project-issue) |   次要   | json_set()函数返回的json数组格式与M*侧不一致，分隔符后缺少空格 |  已验收  |
|  8   | [I6GIMS](https://gitee.com/opengauss/Plugin/issues/I6GIMS?from=project-issue) |   次要   | json_search()函数部分场景下返回结果与M*侧不一致              |  已验收  |
|  9   | [I6H0PF](https://gitee.com/opengauss/Plugin/issues/I6H0PF?from=project-issue) |   次要   | json_quote()函数部分场景下返回结果与M*侧不一致               |  已验收  |
|  10  | [I6H0UX](https://gitee.com/opengauss/Plugin/issues/I6H0UX?from=project-issue) |   次要   | json_unquote()、json_valid()函数部分场景下返回结果与M*侧不一致 |  已验收  |
|  11  | [I6H15C](https://gitee.com/opengauss/Plugin/issues/I6H15C?from=project-issue) |  不重要  | json_type()函数返回的数据类型大小写不一致                    |  已验收  |
|  12  | [I6I4A6](https://gitee.com/opengauss/Plugin/issues/I6I4A6?from=project-issue) |   次要   | 自定义json_array函数或存储过程后，原json_array([val[, val] ...])函数执行报错 |  已验收  |
|  13  | [I6LSOL](https://gitee.com/opengauss/docs/issues/I6LSOL?from=project-issue) |   次要   | dolphin语法介绍>SQL参考>函数和操作符章节，json函数和操作符资料部分内容有误 |  已验收  |

# 4     测试执行

## 4.1   测试执行统计数据

### 4.1.1  验证JSON函数及操作符基础功能

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1. 验证函数json_array、json_object、json_append、json_array_append、json_array_insert、json_contains、json_contains_path、json_depth、json_extract、json_insert、json_keys、json_length、json_merge、json_merge_patch、json_merge_preserve、json_pretty、json_remove、json_replace、json_set、json_search、json_storage_size、json_quote、json_unquote、json_type、json_vaild、json_arrayagg、json_objectagg基础功能<br />2.验证json函数操作符->、->>基础功能 | 执行29条用例，发现12个bug，均已验收 |

### 4.1.2  验证不同类型的表中JSON函数及操作符的使用

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 在行存表、列存表、分区表、临时表、unlogged表、segment表相关语法中验证JSON函数及操作符 | 执行20条用例，未发现bug |

### 4.1.3  验证视图、索引、存储过程、自定义函数中JSON函数及操作符的使用

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1.在视图、索引、存储过程、自定义函数相关语法中验证JSON函数及操作符 | 执行4条用例，未发现bug |

### 4.1.4  验证JSON函数作为名称的使用

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1.验证json函数是否可以作为表名、视图名、列名、自定义函数名、存储过程名、数据库名、序列名、索引名、用户名、角色名、组名 | 执行11条用例，未发现bug |

## 4.2   资料测试

| 测试步骤           | 测试结果           |
| ------------------ | ------------------ |
| 资料的描述是否正确 | 发现1个bug，修复中 |

## 4.3   测试执行统计数据

| 版本名称                                                    | 测试用例数 | 用例执行结果               | 发现问题 |
| ----------------------------------------------------------- | ---------- | -------------------------- | -------- |
| MySQL5.7<br/>openGauss 5.0.0 build f7453c1b<br/>dolphin 1.0 | 64         | Passed：46<br />Failed：18 | 13       |
| MySQL5.7<br/>openGauss 5.0.0 build 35192ba5<br/>dolphin 1.0 | 64         | Passed：64<br />Failed：0  | 0        |

数据项说明：

- 累计发现缺陷单13个，1个为资料缺陷，均已解决且回归通过
- 缺陷密度为12个(缺陷个数)/5kloc(代码行数)=2.4(个/kloc)

## 4.4 后续测试建议

1个问题单[I6HGON](https://gitee.com/opengauss/Plugin/issues/I6HGON?from=project-issue)经ccb评审后转为需求，后续关注测试。

# 5     附件

## 5.1  执行结果示例

```sql
dbtest=# --step1:设置dolphin.b_compatibility_mode参数
dbtest=# set dolphin.b_compatibility_mode = 1;
SET
dbtest=# --step2:标量（元素个数为偶数）
dbtest=# select json_object(true,null);
 json_object
-------------
 {"1": null}
(1 row)

dbtest=# select json_object(FALSE,'null');
  json_object
---------------
 {"0": "null"}
(1 row)

dbtest=# select json_object('id',87,'name','carrot');
         json_object
------------------------------
 {"id": 87, "name": "carrot"}
(1 row)

dbtest=# select json_object(1,'哈哈哈','select',null,'true','on',now(),curtime());
                                   json_object
----------------------------------------------------------------------------------
 {"1": "哈哈哈", "true": "on", "select": null, "2023-03-24 14:39:06": "14:39:06"}
(1 row)

dbtest=# select json_object(1,'哈哈哈','select',null,'true','on'::bool,now(),curtime());
                                   json_object
----------------------------------------------------------------------------------
 {"1": "哈哈哈", "true": true, "select": null, "2023-03-24 14:39:25": "14:39:25"}
(1 row)

dbtest=# --step3:数组（元素个数为偶数）
dbtest=# select json_object('["a",["b","c"],"d"]',null);
              json_object
---------------------------------------
 {"[\"a\",[\"b\",\"c\"],\"d\"]": null}
(1 row)

dbtest=# --step4:对象（元素个数为偶数）
dbtest=# select json_object('{"a":1}',null,'{"b":[1,2]}','{"c":{"c1":[11,null]}}');
                            json_object
--------------------------------------------------------------------
 {"{\"a\":1}": null, "{\"b\":[1,2]}": "{\"c\":{\"c1\":[11,null]}}"}
(1 row)

dbtest=# --step5:为空
dbtest=# select json_object();
 json_object
-------------
 {}
(1 row)

dbtest=# --step6:清理环境
dbtest=# reset dolphin.b_compatibility_mode;
RESET
```