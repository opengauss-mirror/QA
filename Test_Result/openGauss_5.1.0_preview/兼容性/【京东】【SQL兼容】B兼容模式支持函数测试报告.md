![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode。)

修订记录

| 日期      | 修订 版本 | 修改描述             | 作者        |
| --------- | --------- | -------------------- | ----------- |
| 2023-6-26 | 1.0       | 特性测试报告初稿完成 | zhanghuan96 |
| 2023-7-7  | 1.1       | 更新问题单状态       | zhanghuan96 |
| 2023-8-30 | 1.2       | 刷新问题单           | zhanghuan96 |

关键词：

B兼容模式、内置函数

摘要：

本文档是在B兼容模式下支持M\*侧内置函数，包括atan(y,x)函数、name_const函数、compress函数、uncompress函数、uncompressed_length函数、weight_string函数。对上述函数进行测试，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1 特性概述

在B兼容模式下支持M*侧内置函数，并保证这些函数特性同M\*侧一致。

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.0 build a74b7333 | 2023-06-20   | 2023-06-26   |
| dolphin build 88dc066d         | 2023-06-20   | 2023-06-26   |
| openGauss 5.1.0 build ad27d72a | 2023-06-30   | 2023-06-30   |
| dolphin build 95444853         | 2023-06-30   | 2023-06-30   |
| openGauss 5.1.0 build 2819e518 | 2023-7-7     | 2023-7-7     |
| dolphin build 2c720fb2         | 2023-7-7     | 2023-7-7     |
| openGauss 5.1.0 build 5762b67e | 2023-8-30    | 2023-8-30    |
| dolphin build d4795ef6         | 2023-8-30    | 2023-8-30    |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3 测试结论概述

## 3.1 测试整体结论

B兼容模式支持M*内置函数，共计执行75条用例，主要覆盖了功能测试和资料测试。功能测试覆盖函数入参类型校验、函数入参取值测试、函数嵌套使用、函数结合对象使用等。资料测试验证资料描述及示例是否正确。累计发现缺陷单7个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | atan(y,x)函数，y/x的反正切。入参有效取值及无效取值测试，入参个数测试，功能正常，通过 |
| 功能测试 | name_const函数，返回指定列名和列值组成的结果集。测试入参类型，结合表达式测试，变量测试，功能正常，通过 |
| 功能测试 | compress函数，压缩指定字符串。测试入参类型，结合表达式测试，变量测试，功能正常，通过 |
| 功能测试 | uncompress函数 ，解压缩压缩过的二进制数据，并返回原始数据。测试入参类型，结合表达式测试，变量测试，功能正常，通过 |
| 功能测试 | uncompressed_length函数，返回经过压缩的数据在解压缩后的长度。测试入参类型，结合表达式测试，变量测试，功能正常，通过 |
| 功能测试 | weight_string函数，测试和调试字符集排序规则。测试入参类型，结合表达式测试，变量测试，功能正常，通过 |
| 资料测试 | 验证资料描述及示例正确，测试通过                             |

## 3.2 约束说明

- 兼容M*5.7版本
- openGauss需使用兼容B库且含dolphin插件
- atan(y,x)函数返回结果为double precison，因和M\*侧精度处理不同，返回结果不一样
- M\*侧和openGauss侧对0x00的处理显示不相同，因此weight_string函数建议加hex函数输出结果

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       | NA       | NA       | NA                 | NA       |

### 3.3.2 问题统计

|        | 问题总数 | 严重  | 主要 | 次要  | 不重要 |
| ------ | -------- | ----- | ---- | ----- | ------ |
| 数目   | 1        | 1     | 0    | 6     | 0      |
| 百分比 | 100%     | 14.3% | 0%   | 85.7% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------- | -------- |
| 1    | [I7FS13](https://gitee.com/opengauss/Plugin/issues/I7FS13?from=project-issue) | 次要     | atan(y,x)函数，入参是布尔类型，报错，预期支持           | 已验收   |
| 2    | [I7EW5F](https://gitee.com/opengauss/Plugin/issues/I7EW5F?from=project-issue) | 次要     | name_const函数加as后，别名不生效                        | 已验收   |
| 3    | [I7FT7B](https://gitee.com/opengauss/Plugin/issues/I7FT7B?from=project-issue) | 次要     | weight_string函数入参是数值型，返回结果和M侧处理不一样  | 已验收   |
| 4    | [I7FSRU](https://gitee.com/opengauss/Plugin/issues/I7FSRU?from=project-issue) | 次要     | weight_string函数，入参为时间日期类型，返回结果有误     | 已验收   |
| 5    | [I7FQL7](https://gitee.com/opengauss/Plugin/issues/I7FQL7?from=project-issue) | 次要     | weight_string函数，参数中的N为0，预期报错，实际成功     | 已验收   |
| 6    | [I7F5UH](https://gitee.com/opengauss/Plugin/issues/I7F5UH?from=project-issue) | 次要     | weight_string函数入参为超长字符串，数据库挂掉并产生core | 已验收   |
| 7    | [I7JUE9](https://gitee.com/opengauss/Plugin/issues/I7JUE9?from=project-issue) | 次要     | weight_string函数入参是bit类型，返回结果有误            | 已验收   |

# 4 测试执行

## 4.1 功能测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.atan(y,x)，name_const，compress，uncompress，uncompressed_length，weight_string函数的入参类型，入参个数，和其他函数嵌套使用，结合对象使用等方面对函数进行测试 | 执行75条用例，发现7个bug，现已验收通过 |

## 4.2 资料测试

| 测试步骤                       | 测试结果            |
| ------------------------------ | ------------------- |
| 验证函数资料描述及示例是否正确 | 测试通过，未发现bug |

## 4.2 测试执行统计数据
| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.0 build a74b7333 | 75         | Passed: 69 Failed: 7 | 7            |
| openGauss 5.1.0 build ad27d72a | 2          | Passed: 2 Failed: 0  | 0            |
| openGauss 5.1.0 build 2819e518 | 4          | Passed: 4 Failed: 0  | 0            |
| openGauss 5.1.0 build 5762b67e | 1          | Passed: 0 Failed: 0  | 0            |

*数据项说明：*

- 累计发现缺陷单7个，其中一个为内核函数转换问题，缺陷均已验收通过
- 缺陷密度：6(缺陷个数)/3.754kloc(代码行数)=1.59(个/kloc)

## 4.3 后续测试建议

无

# 5 附件

## 5.1 atan(y,x)函数

```sql
mysqldb=# select atan(5,2);
       atan
------------------
 1.19028994968253
(1 row)
```

## 5.2 name_const函数

```sql
mysqldb=# select name_const('young',18);
 young
-------
    18
(1 row)
```

## 5.3 compress函数

```sql
mysqldb=# select hex(compress(123));
              hex
--------------------------------
 03000000789c3334320600012d0097
(1 row)
```

## 5.4 weight_string函数

```sql
mysqldb=# select hex(weight_string('abc' as char(3) level 2,5));
     hex
--------------
 004100420043
(1 row)
```