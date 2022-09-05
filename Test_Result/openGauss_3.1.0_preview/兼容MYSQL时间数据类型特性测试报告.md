![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者        |
| :--------- | ----------- | -------------------- | ----------- |
| 2022-08-01 | 1.0         | 特性测试报告初稿完成 | zhanghuan96 |
| 2022-08-25 | 1.1         | 修改测试报告附件内容 | zhanghuan96 |

 关键词： 

MYSQL、dolphin插件、时间日期类型

摘要：

本文档主要基于插件dolphin兼容MYSQL时间日期数据类型进行测试，包括兼容MYSQL侧date、timestamp[(n)]、datetime[(n)]、time[(n)]、year、year(2)、year(4)数据类型，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

基于插件dolphin，实现openGauss对MYSQL时间数据类型的兼容，包括这些数据类型的取值范围、对无效值的处理方式、数据显示方式、缺省精度、默认值、允许的输入格式、时区处理方式、类型间转换等。由于是在openGauss中实现兼容MYSQL时间日期类型，处理逻辑会发生一些改变，因此部分类型的输入格式、取值范围等和MYSQL侧不完全一致。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.0.0 build b2c6e04e | 2022-6-16    | 2022-6-27    |
| openGauss 3.0.0 build c611b010 | 2022-6-28    | 2022-7-10    |
| openGauss 3.0.0 build dc6a024d | 2022-8-21    | 2022-8-24    |
| dolphin 1.0                    | 2022-6-16    | 2022-8-24    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核<br/>内存：32GB<br/>硬盘：300G<br/>OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

基于插件dolphin兼容MYSQL时间数据类型，共计执行183条用例，主要覆盖了功能测试和资料测试。功能测试覆盖数据类型的取值范围、缺省精度、允许输入的格式、时区处理方式、类型交互使用等。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现缺陷单20个，经评审已取消缺陷单2个，其余缺陷均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | 编译数据库，编译dolphin插件，执行结果符合预期，通过          |
| 功能测试 | om方式安装数据库，拷贝插件依赖文件，测试兼容时间日期数据类型，执行结果符合预期，通过 |
| 功能测试 | 取值范围有效值及无效值测试，通过                             |
| 功能测试 | 精度取值测试，通过                                           |
| 功能测试 | 不同输入格式测试，通过                                       |
| 功能测试 | 不同类型表中插入时间日期类型，通过                           |
| 功能测试 | 类型转换含隐式转换和显示转换，通过                           |
| 功能测试 | 结合时间日期函数测试，通过                                   |
| 资料测试 | 资料描述完整准确，示例执行结果正确，整体质量良好             |

## 3.2   约束说明

- 兼容MYSQL5.7版本
- openGauss需使用兼容B库且含dolphin插件
- date、datetime、timestamp类型取值范围和MYSQL不完全一致；time、year类型和MYSQL取值范围完全兼容，详见（[时间数据类型.md](https://gitee.com/opengauss/Plugin/blob/master/contrib/dolphin/doc/时间数据类型.md)）
- date类型前面或者尾部含有无效字符，只要日期部分合法，会自动去掉无效字符
- year类型目前支持range/list/hash分区表作为分区键，其他分区表暂不支持
- date类型，若有效日期前补0，插入报错
- date、datetime、timestamp类型年份允许输入'0000'年，但不支持月份或天数为零值
- datetime类型输入格式为'YYYYMMDD.hhmmss.[fsec]'或者'YYMMDD.hhmmss.[fsec]'，此种格式不兼容
- datetime、time类型默认不含时区；timestamp类型默认含时区
- date/time/datetime/timestamp/year类型，均支持隐式转换

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 |
| -------- | -------- |
| NA       |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要  | 不重要 |
| ------ | -------- | ---- | ---- | ----- | ------ |
| 数目   | 20       | 0    | 2    | 18    | 0      |
| 百分比 | 100%     | 0%   | 9.5% | 90.5% | 0%     |

###  3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5CMIH](https://gitee.com/opengauss/Plugin/issues/I5CMIH?from=project-issue) | 次要     | date类型，插入的日期格式为'YYYY-MM-DD'或'YY-MM-DD'，月份前有0，插入报错 | 已验收   |
| 2    | [I5CN2R](https://gitee.com/opengauss/Plugin/issues/I5CN2R?from=project-issue) | 次要     | date类型，输入格式为'YYYYMMDD'，年份前面有0时，显示结果和MYSQL不一样 | 已验收   |
| 3    | [I5CSXB](https://gitee.com/opengauss/Plugin/issues/I5CSXB?from=project-issue) | 主要     | dolphin插件下，建表，类型为timestamp without time zone，报错 | 已验收   |
| 4    | [I5CTDS](https://gitee.com/opengauss/Plugin/issues/I5CTDS?from=project-issue) | 次要     | date类型，在dolphin插件仓下，插入日期中含特殊字符不报错      | 已取消   |
| 5    | [I5D4HF](https://gitee.com/opengauss/Plugin/issues/I5D4HF?from=project-issue) | 次要     | dolphin插件下，time类型插入空字符串报错，预期成功            | 已验收   |
| 6    | [I5D520](https://gitee.com/opengauss/Plugin/issues/I5D520?from=project-issue) | 次要     | dolphin插件下，time类型，插入时间，分钟为负数，预期报错，实际成功 | 已取消   |
| 7    | [I5D57W](https://gitee.com/opengauss/Plugin/issues/I5D57W?from=project-issue) | 次要     | 当time格式为'aa:bb.fsec时，解析结果和MYSQL不一样             | 已验收   |
| 8    | [I5D6XH](https://gitee.com/opengauss/Plugin/issues/I5D6XH?from=project-issue) | 主要     | time类型，输入格式为hhmmss[.frac]，时间一样，使用::或者 cast转换的结果不一样 | 已验收   |
| 9    | [I5DADO](https://gitee.com/opengauss/Plugin/issues/I5DADO?from=project-issue) | 次要     | time类型，插入D hh格式报错，预期成功                         | 已验收   |
| 10   | [I5DCF2](https://gitee.com/opengauss/Plugin/issues/I5DCF2?from=project-issue) | 次要     | time类型和interval 进行运算，结果有误                        | 已验收   |
| 11   | [I5DEFY](https://gitee.com/opengauss/Plugin/issues/I5DEFY?from=project-issue) | 次要     | datetime类型，插入格式为'YYYYMMDDhhmmss'，字符串长度为5报错  | 已验收   |
| 12   | [I5DI73](https://gitee.com/opengauss/Plugin/issues/I5DI73?from=project-issue) | 次要     | datetime类型插入now()及current_timestamp函数报错             | 已验收   |
| 13   | [I5DJLN](https://gitee.com/opengauss/Plugin/issues/I5DJLN?from=project-issue) | 次要     | year类型 ,输入为整数，未使用::或者cast转换插入成功           | 已验收   |
| 14   | [I5DKBC](https://gitee.com/opengauss/Plugin/issues/I5DKBC?from=project-issue) | 次要     | year类型与integer类型进行运算，返回结果有误                  | 已验收   |
| 15   | [I5DZC8](https://gitee.com/opengauss/Plugin/issues/I5DZC8?from=project-issue) | 次要     | year类型，插入的数据前面有无效字符，插入成功，预期失败       | 已验收   |
| 16   | [I5ECFK](https://gitee.com/opengauss/Plugin/issues/I5ECFK?from=project-issue) | 次要     | 兼容date类型取值范围表示和openGauss侧原有的一致，但在dolphin插件下取值范围不一样 | 已验收   |
| 17   | [I5FB1E](https://gitee.com/opengauss/Plugin/issues/I5FB1E?from=project-issue) | 次要     | dolphin插件下，datetime类型插入'0000-01-01 00:00:00'成功，预期报错 | 已验收   |
| 18   | [I5FDMD](https://gitee.com/opengauss/Plugin/issues/I5FDMD?from=project-issue) | 次要     | datetime类型 'YYYYMMDD.hhmmss. [fsec]'或者YYYYMMDDhhmmss.frac格式插入报错 | 已验收   |
| 19   | [I5LX0D](https://gitee.com/opengauss/Plugin/issues/I5LX0D?from=project-issue) | 次要     | dolphin插件下，time类型插入数值型报错，预期可以隐式转换      | 已验收   |
| 20   | [I5LSQB](https://gitee.com/opengauss/Plugin/issues/I5LSQB?from=project-issue) | 次要     | microsecond函数返回结果有误                                  | 已验收   |

# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 date类型取值范围及输入格式测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.建表，字段数据类型为date类型<br>2.插入date类型的有效值和无效值验证<br>3.date类型不同输入格式测试<br>4.修改date类型 | 执行39条用例，发现5个bug，4个现已修复且验收通过，1个经评审不予兼容，已取消 |

### 4.1.2 time[(n)]类型取值范围、输入格式及精度测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.建表，字段数据类型为time类型<br/>2.插入time类型的有效值和无效值验证<br>3.time类型不同输入格式测试<br>4.time[(n)]精度测试<br>5.修改time类型 | 执行47条用例，发现7个bug，6个现已修复且验收通过，1个经评审不予兼容，已取消 |

### 4.1.3 datetime[(n)]类型取值范围、输入格式及精度测试

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.建表，字段数据类型为datetime类型<br/>2.插入datetime类型的有效值和无效值验证<br/>3.datetime类型不同输入格式测试<br/>4.datetime精度测试<br>5.修改datetime类型 | 执行29条用例，发现4个bug，现已修复且验收通过 |

### 4.1.4 year/year(2)/year(4)类型取值范围及输入格式测试

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.建表，字段数据类型为year类型<br/>2.插入year类型的有效值和无效值验证<br/>3.year类型不同输入格式测试<br>4.修改year类型 | 执行35条用例，发现3个bug，现已修复且验收通过 |

### 4.1.5 timestamp[(n)]类型取值范围、输入格式及精度测试

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1.建表，字段数据类型为timestamp类型<br/>2.插入timestamp类型的有效值和无效值验证<br/>3.timestamp类型不同输入格式测试<br/>4.timestamp精度测试<br>5.修改timestamp类型 | 执行33条用例，发现1个bug，现已修复且验收通过 |

## 4.2  测试执行统计数据

| 版本名称                                                     | 测试用例数 | 用例执行结果             | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------------------ | ------------ |
| openGauss 3.0.0 build b2c6e04e<br>openGauss 3.0.0 build c611b010<br>openGauss 3.0.0 build dc6a024d | 183        | Passed：183<br>Failed：0 | 20           |

*数据项说明：*

* 累计发现缺陷单20个，其中18个缺陷均已解决且回归通过，2个不予兼容，已取消
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：18(缺陷个数)/3.093kloc(代码行数)=5.82(个/kloc)

## 4.3  后续测试建议

-  除过类型的显示转换，可以测试下类型的隐式转换功能是否正常
-  结合日期函数及表达式进行测试

# 5     附件

##  5.1 year类型使用示例

```sql
--建表，字段使用year类型
db1=# create table t_year0001(c1 year,c2 year(2),c3 year(4));
CREATE TABLE
--插入最小值
db1=# insert into t_year0001 values(1901,1901,1901);
INSERT 0 1
--查询
db1=# select * from t_year0001;
  c1  | c2 |  c3
------+----+------
 1901 | 01 | 1901
(1 row)
--插入最大值
db1=# insert into t_year0001 values(2155,2155,2155);
INSERT 0 1
--查询
db1=# select * from t_year0001;
  c1  | c2 |  c3
------+----+------
 1901 | 01 | 1901
 2155 | 55 | 2155
(2 rows)
--删表
db1=# drop table t_year0001;
DROP TABLE
```

