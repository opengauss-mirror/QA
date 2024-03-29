

![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述             | 作者      |
| ---------- | -------- | -------------------- | --------- |
| 2022-09-23 | 1.0      | 特性测试报告初稿完成 | baixiaoli |
| 2022-09-26 | 1.1      | 根据评审意见修改报告 | baixiaoli |
| 2022-09-29 | 1.2      | 添加回归测试版本     | baixiaoli |

关键词：一主多备 同步提交 增量build 多数派 

摘要：本文档为基于openGauss一主多备同步提交模式的架构下，支持主机异常场景发生后，将备机升为主机，原主机执行增量build时校验主备多数派一致性，build时源节点没有该xlog位置，拒绝此次增量build的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

在一主多备且存在活跃的同步备机场景下，主机执行事务操作时，在等待与同步备机达成多数派一致性位置时，将多数派一致性位置持久化到磁盘上，记录到主机pg_replslot目录下对应同步备机的复制槽状态文件confirmed LSN字段。当主机发生故障后，备机升为主机，原主机作为备机发起增量build时，检测源节点是否存在相同的confirmed LSN，如果不存在，拒绝此次增量build，避免原主机数据被build覆盖。

# 2     特性测试信息

本节描述被测对象的版本信息，测试的时间

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build afdf9b98 | 2022-09-17   | 2022-09-21   |
| openGauss 3.1.0 build 20c4bf07 | 2022-09-27   | 2022-09-27   |

描述特性测试的硬件环境信息

| 硬件型号                  | 硬件配置信息                                                 | 备注 |
| ------------------------- | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280) | CPU:  Kunpeng-920<br>内存：254GB<br>硬盘：ssd<br>OS：openEuler release 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

openGauss支持增量build时校验多数派一致性共计执行23条用例，主要覆盖了功能测试，可靠性测试、性能测试。功能测试覆盖参数enable_save_confirmed_lsn的取值和设置方式测试，主机正常/异常停止，事务同步/未同步到备机时，原主机发起build等场景测试；可靠性测试覆盖主机pg_replslot目录下文件损坏或磁盘满测试；性能测试覆盖一主一同步备TPCC性能测试。共计发现1个问题，回归通过，无遗留风险，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 参数enable_save_confirmed_lsn的取值和设置方式测试，通过      |
| 功能测试   | 主机正常/异常停止，事务同步/未同步到备机时，原主机发起build等场景测试，通过 |
| 可靠性测试 | 主机pg_replslot目录下文件损坏或磁盘满测试，通过              |
| 性能测试   | 一主一同步备TPCC性能测试，通过                               |
## 3.2   约束说明

1. 支持一主多备，同步提交模式，关闭最大可用模式，且存在活跃的同步备机；
2. 异步备或逻辑复制槽不记录confirmed LSN信息；
3. 只影响增量build或不指定build模式的自动build，不影响强制指定全量build模式。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

|  序号  | 问题单号                                                     | 问题描述                                                              | 问题级别  | 问题影响和规避措施      | 当前状态    |
| ------ | ------------------------------------------------------------ | ---------------------------------------------------------------------- | --------- | ----------------------- | ----------- |
| NA    ||  |     |                 |    |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 | 备注 |
| ------ | -------- | ---- | ---- | ---- | ------ | ---- |
| 数目   | 1        | 0    | 0    | 1    | 0      |      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |      |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I5RM5K](https://gitee.com/opengauss/openGauss-server/issues/I5RM5K?from=project-issue) | 次要 | gs_guc set方式修改POSTMASTER类型参数enable_save_confirmed_lsn失败报错 | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

#### 4.1.1.1 参数enable_save_confirmed_lsn的取值和设置方式测试

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.设置参数enable_save_confirmed_lsn=on/off<br />2.设置参数enable_save_confirmed_lsn为on/off之外的值 | 共执行2条用例，共发现1个bug，已验收 |

#### 4.1.1.2 主机正常/异常停止，事务同步/未同步到备机时，原主机发起build等场景测试

| 测试步骤                                                     | 测试结果                         |
| ------------------------------------------------------------ | -------------------------------- |
| 1.配置同步提交模式，关闭最大可用模式，<br />开启enable_save_confirmed_lsn参数<br />2.主机执行事务<br />3.查询state文件confirmed LSN信息记录情况<br />4.主机正常/停止，将备机升为主机，原主机发起build | 共执行18条用例，测试结果符合预期 |

### 4.1.2可靠性测试

#### 4.1.2.1主机pg_replslot目录下文件损坏或磁盘满测试

| 测试步骤                                                     | 测试结果                        |
| ------------------------------------------------------------ | ------------------------------- |
| 1.配置同步提交模式，关闭最大可用模式，<br />开启enable_save_confirmed_lsn参数<br />2.主机pg_replslot目录下文件损坏或注入故障<br />3.主机执行事务<br />4.查询state文件<br />5.恢复主机pg_replslot目录下文件或清除故障 | 共执行2条用例，测试结果符合预期 |

### 4.1.3性能测试

#### 4.1.3.1 一主一同步备TPCC性能测试

| 测试步骤                                                     | 测试结果                            |
| ------------------------------------------------------------ | ----------------------------------- |
| 1.搭建一主一同步备环境，创造数据<br />2.不开启enable_save_confirmed_lsn参数，<br />TPCC 1000仓696并发跑30分钟<br />3.开启enable_save_confirmed_lsn参数，<br />TPCC 1000仓696并发跑30分钟<br />4.对比性能tpmC值 | 共执行1条用例<br />测试结果符合预期 |

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果          | 发现问题单数 |
| ------------------------------ | ---------- | --------------------- | ------------ |
| openGauss 3.1.0 build afdf9b98 | 23         | Passed：22  Failed：1 | 1            |
| openGauss 3.1.0 build 20c4bf07 | 1          | Passed：1   Failed：0 | 0            |

说明：

1.openGauss 3.1.0 build afdf9b98版本测试时发现1个问题。

2.缺陷密度为 1(缺陷个数)/0.5k(代码行数)=2(个/kloc)。

## 4.3   后续测试建议

无

# 5     附件

 无