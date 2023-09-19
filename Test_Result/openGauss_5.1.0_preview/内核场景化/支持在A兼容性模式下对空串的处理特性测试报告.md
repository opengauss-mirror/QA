![avatar](C:\images\openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode。)

修订记录

| 日期      | 修订 版本 | 修改描述         | 作者        |
| --------- | --------- | ---------------- | ----------- |
| 2023-8-30 | 1.0       | 测试报告初稿完成 | zhanghuan96 |

关键词：

A兼容性、behavior_compat_options

摘要：

本文档是在A兼容模式下通过给参数behavior_compat_options增加新的参数值控制空字符串是否当NULL处理，并给出最终测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1 特性概述

在A兼容模式下通过给参数behavior_compat_options增加新的参数值控制空字符串是否当NULL处理

# 2 特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.0 build 68d1772f | 2023-07-28   | 2023-08-2    |
| openGauss 5.1.0 build 46d51516 | 2023-08-16   | 2023-08-16   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.6.1810 (Core) |      |

# 3 测试结论概述

## 3.1 测试整体结论

在A兼容模式下支持对空串的处理，共计执行55条用例，主要覆盖了功能测试和资料测试。功能测试覆盖设置behavior_compat_options='accept_empty_str'及behavior_compat_options=''下，测试空串和NULL的处理。资料测试验证资料描述及示例是否正确。累计发现缺陷单1个，缺陷已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，不同类型插入空串，测试空串的处理是否正常 |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，空字符串作为常量测试 |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，测试空字符串类型转换 |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，空字符串作为变量测试 |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，函数入参及返回值验证 |
| 功能测试 | behavior_compat_options='accept_empty_str'或behavior_compat_options=''时，copy to，copy from语句测试 |
| 资料测试 | 验证资料描述及示例正确，测试通过                             |

## 3.2 约束说明

- 兼容A模式

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       | NA       | NA       | NA                 | NA       |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 1    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I7P9P9](https://gitee.com/opengauss/openGauss-server/issues/I7P9P9?from=project-issue) | 次要     | replace(string,substring)函数，2个参数一样，报错，预期返回null | 已验收   |

# 4 测试执行

## 4.1 功能测试

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.不同方式设置参数<br>2.空字符串原样显示及空字符串作为null处理下，结合表，作为常量、变量、函数入参及返回值验证、通过jdbc连接测试，copy to及copy from语句测试 | 执行55条用例，发现1个bug，现已验收通过 |

## 4.2 资料测试

| 测试步骤                                                     | 测试结果            |
| ------------------------------------------------------------ | ------------------- |
| 验证资料中behavior_compat_options参数对新增参数值的描述及示例是否正确 | 测试通过，未发现bug |

## 4.2 测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.0 build 68d1772f | 55         | Passed: 54 Failed: 1 | 1            |
| openGauss 5.1.0 build 46d51516 | 1          | Passed: 1 Failed: 0  | 0            |

*数据项说明：*

- 累计发现缺陷单1个，非该需求引入，缺陷已验收通过
- 缺陷密度：0(缺陷个数)/7.069kloc(代码行数)=0(个/kloc)

## 4.3 后续测试建议

无

# 5 附件

## 5.1 示例

```sql
--设置behavior_compat_options为'accept_empty_str'，空串原样处理，设置behavior_compat_options为''，空串当NULL处理
openGauss=# set behavior_compat_options to 'accept_empty_str';
SET
openGauss=# select '' is null;
 ?column?
----------
 f
(1 row)

openGauss=# set behavior_compat_options to '';
SET
openGauss=# select '' is null;
 ?column?
----------
 t
(1 row)
```

