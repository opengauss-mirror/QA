![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2024-1-29 |   1.0    |           特性测试报告初稿           | liumin30023837 |

 关键词： 资源池化 在线reform wait_events query_all_drc_info query_node_reform_info_from_dms

摘要：

本文对资源池化支持在线reform，等待事件相关视图 相关特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |  |          |

# 1     特性概述

openGauss实现对资源池化支持在线reform，即对于一个集群，加入备机，踢出备机，重启备机，对于这三种场景，集群中的主机和其他备机的业务，不会中断，在集群恢复正常后，对应业务可以继续跑下去。

openGauss实现对资源池化 新增等待事件视图 DRC信息视图 Reform相关视图 有利于查看以及定位问题。

# 2     特性测试信息

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| :----------------------------------------------------------- | :----------- | :----------- |
| openGauss 5.1.1 build cc4bfd82 <br />openGauss 5.1.1 build 582426cb | 2023-12-7    | 2023-12-30   |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 物理机 | CPU：Intel(R) Xeon(R) CPU E5-2697 v3 @ 2.60GHz<br />内存：251GB<br />硬盘：Dorado 磁阵<br />OS： openEuler 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

实现资源池化支持在线reform，等待事件相关视图 相关特性测试特性测试中，主要涉及功能验证、一致性长跑测试，故障测试共发现12个问题，回归通过，无遗留风险，质量一般。</u>

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 验证备机reform场景，通过。验证新增wait_events query_all_drc_info query_node_reform_info_from_dms视图功能可用，通过。 |
| 一致性长跑 | 跑tpcc 100仓业务，验证一致性，通过，长跑通过。 |
| 故障 | 验证switchover failover 集群启停 kill进程，以及叠加故障，通过 |
| 资料测试 | 新增参数资料验证，通过 |

## 3.2   约束说明

- 无

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------ | :------ | :------ | :---------------- | :------ |
|        |          |  |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :--------- | :------ | :-- | :-- | :-- | :---- |
|  数目 (个)  |    14    |  0  |  11  |  3  |   0    |
| 百分比  (%) |   100   |  0  |  78.5  |  21.5  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
| 1 | https://e.gitee.com/opengaussorg/issues/table?issue=I8US16 | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：资源池化】debug版本 kill 备机1进程  备机2偶现core | 待办的 |
| 2 | https://e.gitee.com/opengaussorg/issues/table?issue=I8TC6O | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：新增DMS、CBB等待事件统计】debug版本 节点启停+查询query_all_drc_info视图 备机生成core | 已验收 |
| 3 | https://e.gitee.com/opengaussorg/issues/table?issue=I8T0L2 | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：新增DMS、CBB等待事件统计】debug版本 主机stop  新主查询query_all_drc_info(1) 结果为空（偶现） | 已验收 |
| 4 | https://e.gitee.com/opengaussorg/issues/table?issue=I8ST2I | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：新增DMS、CBB等待事件统计】debug版本 主备切换+业务  偶现查询query_all_drc_info视图 24小时未出结果 堆栈pool_index  异常 | 测试中 |
| 5 | https://e.gitee.com/opengaussorg/issues/table?issue=I8RCIO | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：支持在线reform】debug版本 业务加集群启停 数据库core | 已验收 |
| 6 | https://e.gitee.com/opengaussorg/issues/table?issue=I8R74H | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：支持在线reform】debug版本 switchover+kill 进程 备机偶现core | 已验收 |
| 7 | https://e.gitee.com/opengaussorg/issues/table?issue=I8QN2K | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：新增DMS、CBB等待事件统计】【需求编号：I852ED】查询query_all_drc_info(1)，RESOURCE_ID 为重复值 | 已验收 |
| 8 | https://e.gitee.com/opengaussorg/issues/table?issue=I8QA2G | 次要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：新增DMS、CBB等待事件统计】【需求编号：I852ED】备机查询query_all_drc_info，查询为空 | 已验收 |
| 9 | https://e.gitee.com/opengaussorg/issues/table?issue=I8PAS2 | 次要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：支持在线reform】开启写转发，普通用户，在备机执行写操作，报错standbywhite could not concect | 已验收 |
| 10 | https://e.gitee.com/opengaussorg/issues/table?issue=I8NQQT | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：支持在线reform】debug版本 主机拉起业务 备机启停 主机业务断掉 | 回归中 |
| 11 | https://e.gitee.com/opengaussorg/issues/table?issue=I8MMET | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：支持在线reform】debug版本 switchover+kill 进程 数据库core | 已验收 |
| 12 | https://e.gitee.com/opengaussorg/issues/table?issue=I8K48U | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：资源池化】debug版本 集群重启，备机出现core | 已验收 |
| 13 | https://e.gitee.com/opengaussorg/issues/table?issue=I8GHD4 | 主要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：资源池化】full clean 场景 节点 加入 一直失败，然后踢出 | 已验收 |
| 14 | https://e.gitee.com/opengaussorg/issues/table?issue=I8C13W | 次要 | 【测试类型：功能测试】【测试版本：5.1.1】【需求名称：资源池化】共享存储 单节点启停  机器状态异常（偶现） | 已验收   |

# 4     测试执行

## 4.1   功能测试

| 测试步骤                                                     | 测试结果    |
| :----------------------------------------------------------- | ----------- |
| 1.针对资源池化支持在线reform，验证备机reform场景包括备机节点启停，备机节点kill，备机节点剔除。验证新增视图wait_events query_all_drc_info query_node_reform_info_from_dms字段结果正确性。<br />2.跑tpcc 100仓100并发 主机读写+备机读+手动备机查询，检查业务数据一致性<br />3.跑tpcc并对主机备机做故障，包括kill备节点进程，switchover过程中机器故障，启停过程中集群故障等。验证在故障场景和新增视图的交互。<br />4.开启按需回放+写转发，和支持在线reform，新增视图做交互。 | 发现12个bug |

## 4.2   资料测试

| 测试步骤                           | 测试结果   |
| :--------------------------------- | :--------- |
| 1.验证语法资料描述及示例是否正确。 | 发现0个bug |

## 4.3   测试执行统计数据

1. 支持在线reform

​		代码数量：222

​		缺陷密度: 5

2. 新增DMS、CBB等待事件统计

   代码数量：800

   缺陷密度: 5

PR:

```
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4509
https://e.gitee.com/opengaussorg/repos/opengauss/DMS/pulls/320
```

```
https://gitee.com/opengauss/openGauss-server/pulls/4469
https://gitee.com/opengauss/DMS/pulls/312
https://codehub-y.huawei.com/Gauss/DDES/DMS/merge_requests/904
```



## 4.4   后续测试建议

无。

# 5     附件

```sql
select * from dbe_perf.wait_events where event='req ask owner for page';
select * from dbe_perf.wait_events where event='req claim owner';
select * from pg_catalog.query_all_drc_info(0);
select * from pg_catalog.query_all_drc_info(1);
select * from pg_catalog.query_node_reform_info_from_dms(0);
select * from pg_catalog.query_node_reform_info_from_dms(1);
```