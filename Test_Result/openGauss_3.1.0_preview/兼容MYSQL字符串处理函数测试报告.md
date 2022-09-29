![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|    日期    | 修订版本 |       修改描述       |    作者    |
| :--------: | :------: | :------------------: | :--------: |
| 2022-09-14 |   1.0    | 特性测试报告初稿完成 | yangruolan |
| 2022-09-14 |   1.1    | 根据评审意见修改 | yangruolan |

 关键词： mysql、字符串处理函数、dolphin插件、兼容B库

 摘要：

本文档主要基于插件dolphin兼容字符串处理函数进行测试，包括兼容mysql侧bin(n)、chara、char_length、character_length、convert、elt、field、find_in_set、soundex、space(n)、between and、not between and字符串处理函数，并给出最终测试结论。 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| :----: | :------: | -------- |
|   无   |          |          |

# 1     特性概述

基于插件dolphin，实现openGauss在兼容B库里对字符串处理函数的兼容，其函数功能及执行结果与mysql侧一致。由于是在openGauss中存在通用问题，如非法字符串转数字失败，二进制不支持0b+不加引号等问题，部分类型的返回结果等和mysql侧不完全一致。

# 2     特性测试信息

| 软件名称                      | 测试起始时间 | 测试结束时间 |
| ----------------------------- | ------------ | ------------ |
| mysql 5.7                     | 2022-08-20   | 2022-09-17   |
| openGauss3.0.0 build ae11cad2 | 2022-08-20   | 2022-09-17   |
| dolphin 1.0                   | 2022-08-20   | 2022-09-17   |

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| :------: | :----------------------------------------------------------- | ---- |
|  虚拟机  | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：31GB<br />硬盘：100GB<br />OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss兼容mysql条件类型函数共计执行用例291条，主要覆盖了功能测试和资料测试，功能测试覆盖有效及无效的入参情形，结合部分字符处理函数、数字操作函数、日期函数、聚集函数及常见运算符，常见的数据库表及自定义函数、存储过程等进行运算并校验输出结果，与mysql执行结果进行比对。资料测试覆盖校验资料的描述是否正确，示例的执行结果是否成功。累计发现问题单19个，3个非问题取消，16个问题已解决并回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | :----------------------------------------------------------- |
| 功能测试 | om安装库、安装插件，执行结果符合预期，通过                   |
| 功能测试 | 对字符串处理函数参数个数、参数的类型、有效入参无效入参的验证，通过 |
| 功能测试 | 函数在常用类型表(列存表、临时表、分区表、segment/unlogged/ustore表)中的输出结果测试，通过 |
| 功能测试 | 与其他常用的字符处理函数、数字操作函数及常见运算符结合测试，通过 |
| 功能测试 | 在视图自定义存储过程、自定义函数中的输出结果测试，通过       |
| 功能测试 | 与聚合函数结合测试，通过                                     |
| 功能测试 | 结合日期函数测试，通过                                       |
| 功能测试 | 对不同编码格式下convert函数的编码输出测试，通过              |
| 功能测试 | 对不同编码格式下char_length函数和character_length函数的返回长度测试，通过 |
| 功能测试 | chara函数根据ascii码对多个数字转换多少对应字符测试，通过     |
| 功能测试 | 分别和select、update、delete、insert结合使用对函数出现的位置测试，通过 |
| 功能测试 | 对函数输入参数中包含select查询及子查询返回结果测试，通过     |
| 功能测试 | 资料描述完整准确，示例执行结果正确，整体质量良好             |

## 3.2   约束说明

1. mysql使用5.7版本
2. openGauss需使用兼容B库且有dolphin插件
3. 多个字符串中间加空格，@符号不添加引号，二进制数据类型的0b+不加引号的方式，十六进制的0x+不加引号的方式，字符串转数字报错如上这些场景属于通用问题，plugin sig会议开发专家认为可作非问题处理，openGauss通过sql_mode统一控制，不区分select和insert/update/delete场景。
4. openGauss默认client端的编码格式为utf8，server端的编码格式默认是sql_ascii，函数 character_length和char_length在入参为汉字的用例之前需要将两端的编码格式修改为一致的，否则客户端和服务器端的编码格式不一致的情况下，返回的结果与mysql不一致。
5. openGauss默认client端的编码格式为utf8，server端的编码格式默认是sql_ascii，chara函数在入参大于127时，测试之前需要将两端的编码格式修改为一致的；否则客户端和服务器端的编码格式不一致的情况下，openGauss返回结果会报错。
6. chara函数控制字符在客户端显示的问题为十六进制，8月23日plugin sig会议开发专家认为在jdbc环境下输出结果与mysql一致即可。
7. 对于between...and函数对于时间晚上24点即为下一天00点，查询的时候数据库识别不出24点的信息，得换成下一天00点才查询出正确结果。
8. 对于between...and函数datetime和timestamp类型的字段进行between and查询时是不包括右边界的，date类型的字段是没有时分秒的，所以date类型是包括右边界的。
9. 字符串处理函数中text类型对int类型取余(例如：select bin(123)%3)，目前openGauss不支持。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 |  严重  | 主要 |  次要  | 不重要 |
| :---------: | :------: | :----: | :--: | :----: | :----: |
|  数目 (个)  |    19    |   2    |  0   |   17   |   0    |
| 百分比  (%) |   100%   | 10.53% |  0   | 89.47% |   0    |

### 3.3.3 问题详情

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| :--: | ------------------------------------------------------------ | :------: | ------------------------------------------------------------ | :------: |
|  1   | [I5N00Y](https://e.gitee.com/opengaussorg/issues/list?issue=I5N00Y) |   严重   | 当参数里含有分数或参数为日期类型时，会导致数据库断开连接，产生core文件 |  已验收  |
|  2   | [I5N046](https://e.gitee.com/opengaussorg/issues/list?issue=I5NO46) |   次要   | chara函数针对ascill控制字符返回结果有误                      |  已取消  |
|  3   | [I5NRWA](https://e.gitee.com/opengaussorg/issues/list?issue=I5NRWA) |   次要   | 对于bin()和(not)tbween and函数，如下这些用例与mysql返回结果不一致 |  已验收  |
|  4   | [I5NYYT](https://e.gitee.com/opengaussorg/issues/list?issue=I5NYYT) |   次要   | char_length/character_length函数在相同编码格式下参数为分数和汉字时，返回结果与mysql不一致 |  已验收  |
|  5   | [I5OBQA](https://e.gitee.com/opengaussorg/issues/list?issue=I5OBQA) |   次要   | 字符串函数与比较运算符结合时，字符串里出现非数字类型的参数，返回结果与mysql不一致 |  已取消  |
|  6   | [I5ONX6](https://e.gitee.com/opengaussorg/issues/list?issue=I5ONX6) |   次要   | elt\field\find_in_set函数入参为特殊值：‘’\‘   ’\null\@时，返回结果与mysql不一致 |  已验收  |
|  7   | [I5OO16](https://e.gitee.com/opengaussorg/issues/list?issue=I5OO16) |   次要   | field函数参数时布尔类型时，返回与mysql不一致                 |  已验收  |
|  8   | [I5OO8Q](https://e.gitee.com/opengaussorg/issues/list?issue=I5OO8Q) |   次要   | 函数field(str，str1，str2，str3，...)的参数str为字符串类型时，返回结果与mysql不一致 |  已验收  |
|  9   | [I5OTTU](https://e.gitee.com/opengaussorg/issues/list?issue=I5OTTU) |   次要   | space函数与聚合函数count结合使用时，同时执行多个select语句与一次执行一个select语句返回结果不一致 |  已验收  |
|  10  | [I5OU7G](https://e.gitee.com/opengaussorg/issues/list?issue=I5OU7G) |   次要   | space函数向数据表中插入以数字开头的字符串时，返回结果与mysql不一致 |  已取消  |
|  11  | [I5PDIV](https://e.gitee.com/opengaussorg/issues/list?issue=I5PDIV) |   次要   | between...and范围查询，无返回结果                            |  已验收  |
|  12  | [I5PFI2](https://e.gitee.com/opengaussorg/issues/list?issue=I5PFI2) |   严重   | between...and函数在数据表中入参为日期类型时，查询操作会挂库  |  已验收  |
|  13  | [I5PK98](https://e.gitee.com/opengaussorg/issues/list?issue=I5PK98) |   次要   | soundex函数入参是布尔型和汉字类型时，返回结果与mysql不一致   |  已验收  |
|  14  | [I5PTFX](https://e.gitee.com/opengaussorg/issues/list?issue=I5PTFX) |   次要   | chara函数在入参大于127时，返回结果与mysql不一致              |  已验收  |
|  15  | [I5PTGY](https://e.gitee.com/opengaussorg/issues/list?issue=I5PTGY) |   次要   | bin()函数入参为二进制时，返回结果与mysql不一致               |  已验收  |
|  16  | [I5RBDC](https://e.gitee.com/opengaussorg/issues/list?issue=I5RBDC) |   次要   | space()函数入参为二进制时，报错                              |  已验收  |
|  17  | [I5RA3I](https://e.gitee.com/opengaussorg/issues/list?issue=I5RA3I) |   次要   | chara函数和space函数在X86环境下和ARM环境下，部分用例的返回结果不一致 |  已验收  |
|  18  | [I5SBZU](https://e.gitee.com/opengaussorg/issues/list?issue=I5SBZU) |   次要   | YAT使用JDBC方式链接兼容B库soundex函数入参为其它语言时，返回错误 |  已验收  |
|  19  | [I5S68L](https://e.gitee.com/opengaussorg/issues/list?issue=I5S68L) |   次要   | convert函数，转换时utf8不带引号报错 |  已验收  |

# 4     测试执行

## 4.1   测试执行统计数据

### 4.1.1   编译库及插件执行测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 <br />4. 执行用例 | 编译库及插件成功，共执行291条用例，<br />共发现17个bug，其中14个现已修复且验收通过，3个作为非问题单已取消 |

#### 4.1.1.1 函数参数个数、参数的类型、有效入参无效入参测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 在兼容B库并安装dolphin插件的环境 <br />2. 函数参数个数、参数的类型、有效入参无效入参测试函数执行结果 | 执行77条用例，发现12个bug<br />11条现已修复且验收通过，1条经plugin sig组评审作为非问题取消，<br />1.chara函数针对ascill控制字符返回结果与预期不符(issue单号:I5N046)，经评审开发专家讨论认为在jdbc环境下输出结果一致即可，经测试以上用例在jdbc环境下与mysql输出一致,故取消问题单 |

#### 4.1.1.2函数在常用表中结合聚合函数日期函数视图自定义存储过程输出结果测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 在兼容B库并安装dolphin插件的环境 <br />2. 在常用类型表(列存表、临时表、分区表、segment/unlogged/ustore表)中结合聚合函数日期函数测试函数输出结果 | 执行178条用例，发现5个bug<br />4条现已修复且验收通过，1条经plugin sig组评审作为非问题取消，<br />1.space函数向数据表中插入以数字开头的字符串时，返回结果与mysql不一致(issue单号:I5OU7G)，经评审非法字符串转数字问题属于通用问题不予修改，故取消问题单 |

#### 4.1.1.3与其他常用的字符处理函数、数字操作函数及常见运算符结合输出结果测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 在兼容B库并安装dolphin插件的环境 <br />2.与其他常用的字符处理函数、数字操作函数及常见运算符结合输出结果测试 | 执行36条用例，发现2个bug<br />1条现已修复且验收通过，1条经plugin sig组评审作为非问题取消，<br />1.字符串函数与比较运算符结合时，字符串里出现非数字类型的参数，返回结果与mysql不一致(issue单号:I5OBQA),经评审非法字符串转数字问题属于通用问题不予修改，故取消问题单 |

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果                  | 发现问题 |
| ------------------------------ | ---------- | ----------------------------- | -------- |
| openGauss 3.0.0 build 4c4dc36c | 291        | Passed：221  <br />Failed：70 | 10       |
| openGauss 3.0.0 build 2c221dae | 291        | Passed：264 <br />Failed：27  | 5        |
| openGauss 3.1.0 build 0b50eccc | 291        | Passed：289  <br />Failed：2  | 2        |
| openGauss 3.1.0 build 70e18011 | 291        | Passed：289  <br />Failed：2  | 2        |
| openGauss 3.1.0 build 2c0ccaf9 | 291        | Passed：291  <br />Failed：0  | 0        |

数据项说明：

- 累计发现缺陷单19个，其中16个缺陷均已解决且回归通过，3个经评审属于通用问题，已取消
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：19个(缺陷个数)/1.591kloc(代码行数)=11.94(个/kloc)

## 4.3 后续测试建议

1.后续对openGauss的多个字符串中间加空格，@符号不到引号，二进制数据类型的0b+不加引号的方式，十六进制的0x+不加引号的方式，字符串转数字等通用问题进行进一步验证。

2.字符串处理函数中text类型对int类型取余，目前openGauss不支持，建议后续支持后进行测试

# 5     附件

## 5.1  bin()函数执行结果示例

```sql
test_yat=# select bin('156465nhjhio99999');
        bin
--------------------
 100110001100110001
(1 row)

test_yat=# select bin('!@#%$');
 bin
-----
 0
(1 row)

test_yat=# select bin((select bin(100)));
          bin
-----------------------
 100001100100101000100
(1 row)
```

