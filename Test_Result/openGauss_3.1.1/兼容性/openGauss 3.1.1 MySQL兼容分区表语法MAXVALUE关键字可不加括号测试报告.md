![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2022-12-13 | 1.0   | 报告初稿 | zhanbiao.chen |
| 2022-12-20 | 1.1   | 完善结论 | zhanbiao.chen |


关键词：兼容B库，dolphin插件，MAXVALUE

 
摘要：本文档为openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表RANGE分区中MAXVALUE关键字可不加括号功能的测试报告。


缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

openGauss在兼容B库情形下安装dolphin插件，实现兼容MySQL分区表RANGE分区MAXVALUE关键字可不加括号的功能，在一级、二级分区表中不管是否对MAXVALUE关键字加括号，均可执行成功。

- MySQL语法：
```sql
-- CREATE TABLE
CREATE TABLE employees (
  id INT NOT NULL,
  fname VARCHAR(50) NOT NULL,
  lname VARCHAR(50) NOT NULL,
  hired DATE NOT NULL
)
PARTITION BY RANGE( YEAR(hired) ) (
  PARTITION p1 VALUES LESS THAN (1991),
  PARTITION p2 VALUES LESS THAN (1996),
  PARTITION p3 VALUES LESS THAN (2001),
  PARTITION p4 VALUES LESS THAN  MAXVALUE
);

-- ALTER TABLE
ALTER TABLE employees ADD PARTITION (
    PARTITION p5 VALUES LESS THAN (2010),
    PARTITION p6 VALUES LESS THAN MAXVALUE
);
```

# 2     特性测试信息

被测对象的版本信息和测试的时间

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| MySQL 5.7 | 2022-12-01 | 2022-12-20  |
| openGauss 3.1.0 build 152c968e | 2022-12-01 | 2022-12-18 |
| openGauss 3.1.0 build c950de5f | 2022-12-19 | 2022-12-20 |
| dolphin 1.0 | 2022-12-01 | 2022-12-20  |

特性测试的环境信息

| 环境信息 |  配置信息    | 备注 |
| -------- | ------------ | ---- |
| 虚拟机 | CPU: Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz <br /> MEM: 32GB <br /> DISK: 200GB <br /> OS: CentOS Linux release 7.6.1810 (Core) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss dolphin插件MySQL兼容分区表语法MAXVALUE关键字可不加括号功能，共计执行了116个用例，主要覆盖了功能测试、约束测试、备份恢复测试和资料测试。功能测试覆盖了合理报错场景测试，一级分区表（range）和二级分区（range-range、range-list、range-hash、list-range、hash-range）的语法测试；约束测试验证了功能测试用例在非兼容"B"库下执行数据库可稳定运行；备份恢复测试验证了数据恢复后数据的完整性、正确性；资料测试手册是否完善、有效。测试中发现2个问题,已解决且回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 | 编译安装数据库和插件，执行结果符合预期，通过 |
| 功能测试 | 间隔分区表MAXVALUE关键字测试，执行结果符合预期，通过 |
| 功能测试 | range分区表MAXVALUE关键字测试，执行结果符合预期，通过 |
| 功能测试 | range-range、range-list、range-hash、list-range、hash-range二级分区表MAXVALUE关键字测试，执行结果符合预期，通过 |
| 约束测试 | 所有测试用例在非兼容"B"库下运行，不通过用例正常返回错误，数据库稳定性不受影响，执行结果符合预期，通过 |
| 备份恢复测试 | 逻辑备份逻辑恢复、物理备份以及物理恢复，符合预期，通过 |
| 资料测试 | 资料描述准确，示例的执行结果正确，整体质量良好，符合预期，通过 |

## 3.2   约束说明

1. 兼容MySQL分区重组织语法需在openGauss兼容B库下安装dolphin插件验证。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA         |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 1    | 0    |  0   | 1      |
| 百分比 | 100%     | 50%  | 0%   |  0%  | 50%    |

### 3.3.3 问题列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
| [I66239](https://gitee.com/opengauss/Plugin/issues/I66239?from=project-issue) | MySQL兼容分区语法MAXVALUE关键字，range分区使用 start end 语法时数据库异常宕机 | 严重 | 已验收 |
| [I65V4X](https://gitee.com/opengauss/Plugin/issues/I65V4X?from=project-issue) | MySQL兼容分区语法MAXVALUE关键字，建表时不加括号，pg_get_tabledef获取的结果含有括号，确认是否需要修改 | 不重要 | 已取消 |


# 4     测试执行

## 4.1 测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 插件安装

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装openGauss数据库<br />2. 编译dolphin插件<br />3. 创建兼容B库并安装dolphin插件 | 执行结果符合预期，测试通过 |

#### 4.1.1.2 间隔分区表

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.间隔分区表验证MAXVALUE关键字加或不加括号语法，部分非法场景合理报错 | 执行8条用例，结果符合预期，测试通过 |

#### 4.1.1.3 RANGE分区表

| 测试步骤 | 测试结果 |
| -------- | -------- |
|1.RANGE分区表MAXVALUE关键字加或不加括号语法基本功能验证，部分非法场景合理报错<br />2.RANGE分区表MAXVALUE关键字加或不加括号语法ALTER(ADD、DROP、MOVE、EXCHANGE、SPLIT、TRUNCATE、RENAME、REORGANIZE)场景验证，部分非法场景合理报错 | 执行72条用例，结果符合预期，测试通过 |

#### 4.1.1.4 二级分区表

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.二级分区表(range-range、range-list、range-hash、list-range、hash-range)MAXVALUE关键字加或不加括号语法基本功能验证，部分非法场景合理报错<br />2.二级分区表(range-range、range-list、range-hash、list-range、hash-range)MAXVALUE关键字加或不加括号语法ALTER(ADD、DROP、SPLIT、REORGANIZE)场景验证，部分非法场景合理报错 | 执行36条用例，结果符合预期，测试通过 | 

### 4.1.2 约束测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在兼容"A"、"C"、"PG"类型数据库下执行所有测试用例<br />2. 用例全部执行，正常返回错误，执行完成后数据库稳定运行 | 执行116条用例，执行结果符合预期，测试通过 |

### 4.1.3 备份恢复测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1.对表做增删分区操作，备份数据，删除数据<br />2.恢复数据后验证数据完整性、正确性 | 结果符合预期，测试通过 | 

### 4.1.4 资料测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 文档描述<br>2. 文档示例 | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 |
| -------- | ---------- | ------------ | ------------ |
| openGauss 3.1.0 build 152c968e | 116 | Passed: 110<br />Failed: 6 | 2 |
| openGauss 3.1.0 build c950de5f | 116 | Passed: 116<br />Failed: 0 | 0 |

## 4.3   后续测试建议

无

# 5     附件

无

