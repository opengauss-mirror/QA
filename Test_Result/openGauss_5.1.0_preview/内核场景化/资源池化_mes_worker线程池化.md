![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |               修改描述               |      作者       |
| :------- | :------ | :---------------------------------- | :------------- |
| 2024-1-30 |   1.0    |           特性测试报告初稿           | liumin30023837 |

 关键词： 资源池化 mes_worker 线程池化

摘要：

本文对资源池化mes_worker 线程池化相关特性测试，并给出最终测试结论。

缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |  |          |

# 1     特性概述

openGauss实现对资源池化消息的处理线程mes_worker进行池化管理，从而做到随着业务压力的增加，增加对应的消息处理线程，业务压力减少，减少对应的消息处理线程。

# 2     特性测试信息

| 版本名称                                                     | 测试起始时间 | 测试结束时间 |
| :----------------------------------------------------------- | :----------- | :----------- |
| openGauss 5.1.1 build 582426cb<br />openGauss 6.0.0 build d10e4849<br />openGauss 6.0.0 build d28f3889 | 2024-1-22    | 2024-1-31    |

| 环境信息 | 配置信息                                                     | 备注 |
| :------- | :----------------------------------------------------------- | :---- |
| 物理机 | CPU：Intel(R) Xeon(R) CPU E5-2697 v3 @ 2.60GHz<br />内存：251GB<br />硬盘：Dorado 磁阵<br />OS： openEuler 20.03 (LTS) |      |

# 3     测试结论概述

## 3.1   测试整体结论

实现资源池化mes_worker 线程池化相关特性测试特性测试中，主要涉及功能验证、特性耦合测试，故障场景，性能场景测试共发现10个问题，回归通过，无遗留风险，质量一般。</u>

| 测试活动 | 活动评价                                                     |
| :------ | :----------------------------------------------------------- |
| 功能测试 | 验证新增ss_work_thread_pool_attr接口，以及功能，通过。 |
| 特性耦合 | 验证升级，和按需回放+写转发交互，通过。 |
| 故障 | 验证switchover failover 集群启停 kill进程，以及叠加故障，通过。 |
| 性能场景 | 性能验收场景，通过。 |
| 资料测试 | 新增参数资料验证，通过。 |

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
|  数目 (个)  |    10    |  1  |  7  |  2  |   0    |
| 百分比  (%) |   100   |  10  |  70  |  20  |   0    |

### 3.3.3 问题单汇总

| 序号 |                           issue号                            | 问题级别 |                           问题简述                           | 问题状态 |
| :-- | :---------------------------------------------------------- | :------ | :---------------------------------------------------------- | :------ |
| 1 | https://e.gitee.com/opengaussorg/issues/table?issue=I8YAP1 | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】设置GUC参数ss_work_thread_pool_attr=16 拉起业务 线程正常添加  停止业务后  线程未正常释放 | 已验收 |
| 2 | https://e.gitee.com/opengaussorg/issues/table?issue=I8YATL | 严重 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】升级后，设置GUC参数ss_work_thread_pool_attr=128 重启失败 生成core | 已验收 |
| 3 | https://e.gitee.com/opengaussorg/issues/table?issue=I8YHEV | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】节点多次启停 节点core | 已验收 |
| 4 | https://e.gitee.com/opengaussorg/issues/table?issue=I8YLMN | 次要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】kill 备机 switchover 后  主机偶现core | 待办的 |
| 5 | https://e.gitee.com/opengaussorg/issues/table?issue=I8YMGR | 次要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】 switchover +kill 进程 主机unknown | 已完成 |
| 6 | https://e.gitee.com/opengaussorg/issues/table?issue=I8ZQKG | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】debug包  设置GUC参数ss_work_thread_pool_attr=16 stop集群生成core | 已验收 |
| 7 | https://e.gitee.com/opengaussorg/issues/table?issue=I8ZY06 | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】switchover 后生成core | 已验收 |
| 8 | https://e.gitee.com/opengaussorg/issues/table?issue=I909HQ | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】debug版本 主备切换+业务跑大于12小时 查询query_all_drc_info视图  卡住 | 已完成 |
| 9 | https://e.gitee.com/opengaussorg/issues/table?issue=I90GKU | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】主备切换后，在新主和备机长跑业务  几个小时后业务断 | 已验收 |
| 10 | https://e.gitee.com/opengaussorg/issues/table?issue=I91G5E | 主要 | 【测试类型：功能测试】【测试版本：6.0.0】【资源池化】【需求名称：MES worker线程池化】kill进程后  偶现备机状态异常显示卡在升主上 | 待办的 |

# 4     测试执行

## 4.1   功能测试

| 测试步骤                                                     | 测试结果    |
| :----------------------------------------------------------- | ----------- |
| 1.针对资源池化mes_worker 线程池化，验证新增GUC 参数ss_work_thread_pool_attr接口测试，包括默认值、边界值、异常值等。验证 ss_work_thread_pool_attr不同取值跑业务场景以及一致性检查。<br />2.验证升级测试，包括升级不提交，升级提交，升级回滚场景下新增GUC参数ss_work_thread_pool_attr是否可以设置，已经设置成功后功能使用是否正常。<br />3.跑tpcc并对主机备机做故障，包括kill备节点进程，switchover过程中机器故障，启停过程中集群故障等。<br />4.开启按需回放+写转发，同时设置参数ss_work_thread_pool_attr跑tpcc业务做交互测试。<br />5.验收性能场景：固定MES worker线程数量和开启worker线程池下对比cpu占用率；开启128后，CPU利用率下降10%以上。 | 发现10个bug |

## 4.2   资料测试

| 测试步骤                           | 测试结果   |
| :--------------------------------- | :--------- |
| 1.验证语法资料描述及示例是否正确。 | 发现0个bug |

## 4.3   测试执行统计数据

缺陷单共10个，

PR:

```
https://e.gitee.com/opengaussorg/repos/opengauss/CBB/pulls/124
https://e.gitee.com/opengaussorg/repos/opengauss/DMS/pulls/356
https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4728
https://e.gitee.com/opengaussorg/repos/opengauss/CM/pulls/175
https://e.gitee.com/opengaussorg/repos/opengauss/DSS/pulls/293
```

代码数量：1534

 缺陷密度：6.52

## 4.4   后续测试建议

无。

# 5     附件

```sql
gs_guc set -Z datanode -N all -I all -c "ss_work_thread_pool_attr ='' "
gs_guc set -Z datanode -N all -I all -c "ss_work_thread_pool_attr ='16' "
```