![avatar](D:/images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者         |
| ---------- | ----------- | -------------------- | ------------ |
| 2022-09-29 | 1.0         | 特性测试报告初稿完成 | shuai_lei666 |
|            |             |                      |              |

关键词： 兼容B库，grant proxy，revoke proxy，checksum table

摘要：本文档介绍openGauss在兼容B库中，兼容mysql的grant proxy、revoke proxy和checksum table语法，并给出最总测试结论。

# 1     特性概述

openGauss在兼容B库中，且dolphin插件生效的情况下，使用grant proxy on user1 to user2，可以将user1的权限授予users2。使用revoke proxy on user1 from user2，可以将user1授予user2的权限回收，使用checksum table table_name[,table_name..]，可以返回单表或多表的checksum值，可以用来校验表中数据是否一致。

# 2     特性测试信息

本节描述被测对象的版本信息，测试的时间，依赖的环境

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build 075791e0 | 2022-09-22   | 2022-09-27   |
| dolphin 1.0                    | 2022-09-22   | 2022-09-27   |

描述特性测试的环境信息

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br>内存：32GB<br>硬盘：100G<br>OS：Centos Linux 7 |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss在创建和修改试图支持definer语法，主要覆盖了功能测试，资料测试，性能测试。功能测试主要覆盖不同用户授权及回收权限的返回结果是否正确，对参数的有效和无效值进行验证，checksum不同的表；资料测试主要确保描述合理，约束点覆盖全面以及示例正确；性能测试主要覆盖checksum单表和多表，且表中数据较多时，执行chekcsum所用的时间。发现问题9个，2个和开发确认非问题取消，4个已解决并验收通过，3个未解决。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 针对不同用户授予或回收不同用户proxy权限，通过                |
| 功能测试 | 对参数的有效和无效值进行验证，返回结果是否正确，通过         |
| 功能测试 | 针对checksum不同的表，返回结果是否正确，通过                 |
| 性能测试 | checksum单表和多表，且表中数据较多时，执行chekcsum所用的时间，通过 |
| 资料测试 | 资料叙述准确无误，示例正确，通过                             |

## 3.2   约束说明

1. mysql使用版本5.7
2.需要在兼容B库中执行
3. grant proxy和revoke proxy部分结果与mysql不一致，此部分参照grant role to role，grant role from role，此部分与mysql不做兼容，由opengauss角色管理和权限管理决定
4. 不支持checksum视图，如果checksum视图，返回为null

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2 问题统计

openGauss支持create view和alter view通过definer指定属主：

|        | 问题总数 | 严重 | 主要  | 次要  | 不重要 | 备注 |
| ------ | -------- | ---- | ----- | ----- | ------ | ---- |
| 数目   | 8        | 0    | 1     | 7     | 0      |      |
| 百分比 | 100%     | 0%   | 12.5% | 87.5% | 0%     |      |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                           | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------------- | ------ |
| 1    | [I5T4T4](https://gitee.com/opengauss/Plugin/issues/I5T4T4?from=project-issue) | 次要 | 执行checksum table时没有表的select权限报错         | 已取消 |
| 2    | [I5T7J6](https://gitee.com/opengauss/Plugin/issues/I5MIVQ?from=project-issue) | 次要 | 系统管理员授予普通用户权限，普通用户没有public权限 | 已取消 |
| 3    | [I5T6EM](https://gitee.com/opengauss/Plugin/issues/I5T6EM)   | 次要 | checksum table表中列精度不一致，返回结果一致       | 待办的 |
| 4    | [I5TD7V](https://gitee.com/opengauss/Plugin/issues/I5TD7V)   | 次要 | checksum不存在的表，返回错误                       | 已验收 |
| 5    | [I5TDAX](https://gitee.com/opengauss/Plugin/issues/I5TDAX)   | 次要 | checksum空表，返回错误                             | 已验收 |
| 6    | [I5TDGX ](https://gitee.com/opengauss/Plugin/issues/I5TDGX)  | 次要 | checksum数据相同的表，返回结果不一致               | 待办的 |
| 7    | [I5TGI6](https://gitee.com/opengauss/Plugin/issues/I5TGI6)   | 次要 | checksum ustore表，数据库stop，并产生core          | 已验收 |
| 8    | [I5TGJK ](https://gitee.com/opengauss/Plugin/issues/I5TGJK)  | 次要 | checksum分区表，返回错误                           | 已验收 |
| 9    | [I5TNB1 ](https://gitee.com/opengauss/Plugin/issues/I5TNB1)  | 次要 | checksum表中约束不同，预期返回不一致，实际返回一致 | 待办的 |

# 4     测试执行

## 4.1   测试执行步骤

#### 4.1.1 针对不同用户授予或回收不同用户proxy权限的测试

| 测试步骤                                   | 测试结果                                  |
| ------------------------------------------ | ----------------------------------------- |
| 1. 针对不同用户授予或回收不同用户proxy权限 | 共执行26条用例，发现1个bug，1个非问题取消 |

#### 4.1.2 针对参数的有效和无效值进行验证的测试

| 测试步骤                                   | 测试结果                         |
| ------------------------------------------ | -------------------------------- |
| 1. 针对不同用户授予或回收不同用户proxy权限 | 共执行26条用例，测试结果符合预期 |

#### 4.1.3 针对checksum不同的表的测试

| 测试步骤                | 测试结果                                  |
| ----------------------- | ----------------------------------------- |
| 1. 针对checksum不同的表 | 共执行29条用例，发现7个bug，1个非问题取消，4个已验收，2个待办的 |

#### 4.1.1.4 性能测试

| 测试步骤                                                     | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1. checksum单表和多表，且表中数据较多时，执行chekcsum所用的时间 | 共执行3条用例，测试结果符合预期 |

#### 4.1.5 资料测试

| 测试步骤                       | 测试结果                       |
| ------------------------------ | ------------------------------ |
| 1. 使用方法描述<br>2. 示例测试 | 使用方法描述准确，示例正确无误 |

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果        | 发现问题单数 |
| ------------------------------ | ---------- | ------------------- | ------------ |
| openGauss 3.1.0 build 075791e0 | 83         | Passed：70  Failed：13 | 9            |
| openGauss 3.1.0 build 1068fe6d | 83         | Passed：81  Failed：3  |              |

说明：

1.openGauss 3.0.0 build 075791e0版本测试时发现9个问题，2个和开发确认作为非问题取消，4个已解决并验收通过，3个待办的

3.缺陷密度：7(缺陷个数)/0.887k(代码行数)=7.9(个/kloc)

## 4.3   后续测试建议

1. grant proxy和revoke proxy按opengauss角色管理和权限管理决定，部分返回结果与mysql不一致，是否影响用户权限

2. checksum table，表中有约束，或表所在的schema不同时，返回结果与mysql不一致，是否影响迁移或校验等功能

3. checksum table语法正常计算走系统自带的checksum计算，性能无提升，是否影响迁移速率或其他功能

# 5     附件

## 5.1 checksum table用例执行结果

```
test=# create table test(c1 int);
CREATE TABLE
test=# insert into test values(1);
INSERT 0 1
test=# checksum table test;
    Table   | Checksum
------------+------------
public.test | 178907681
```

