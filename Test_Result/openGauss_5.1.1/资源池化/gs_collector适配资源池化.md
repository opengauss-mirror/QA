![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

|   日期    | 修订版本 |     修改描述     | 作者  |
| :-------: | :------: | :--------------: | :---: |
| 2023-12-20 |   1.0    | 特性测试报告初稿 | chenziyang |

 关键词：

 gs_collector、采集、资源池化、DSS、DMS

摘要：

本文档主要介绍gs_collector采集工具适配资源池化，并介绍适配资源池化相关模块的信息收集，给出测试总结。


缩略语清单：

| 缩略语 |                       英文全名                        | 中文解释                                             |
| :---- | :--------------------------------------------------- | :---------------------------------------------------- |
|  NA  |          |          |

# 1     特性概述

在资源池化模式下，实现gs_collector采集工具收集dss配置信息及日志信息、dms日志信息，同时保证在数据库正常运行以及数据库down等异常情况下能够收集原有非资源池化模式中gs_collector收集到的信息

主要改动点如下：

- **log**：收集\$GAUSSLOG/pg_log/DMS、\$GAUSSLOG/pg_log/DSS、\$DSS_HOME/log中日志文件。
- **DSS**：DSS配置文件，包括$DSS_HOME/cfg目录中dss_inst.ini、dss_vg_conf.ini文件。
- **磁阵信息**：包含 +data 以及 +p0 信息。
- **config**：非资源池化模式下收集的pg_replslot以及pg_control文件目录更新为：+data/pg_replslot以及+data/pg_control。
- **xlog**：pg_xlog目录更新为：+data/pg_xlog0、+data/pg_xlog1。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 45f6613a<br/>openGauss OM 5.1.1 build 1e74d4f2 | 2023-11-30 | 2023-12-5 |
| openGauss 5.1.1 build 01b191f0<br/>openGauss OM 5.1.1 build 6f740bc2 | 2023-12-7   | 2023-12-12 |
| openGauss 5.1.1 build 79509ee2<br/>openGauss OM 5.1.1 build 0cdfd49e | 2023-12-14 | 2023-12-15 |

| 硬件型号 |                         硬件配置信息                         | 备注 |
| :------: | :----------------------------------------------------------: | ---- |
|  x86+centOS  | Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz 56核<br/>内存：755GB<br/>硬盘：800G<br/>OS：CentOS Linux 7 (Core) |      |



# 3     测试结论概述

## 3.1   测试整体结论

gs_collector采集工具，共计执行44条用例，主要覆盖了功能测试和资料测试。功能测试覆盖gs_collector的采集基础功能，以及指定配置文件采集指定信息，资源池化特定采集配置。累计发现缺陷单4个，4个问题单均已修复，整体质量良好。

| 测试活动 |                           活动评价                           |
| :------: | :----------------------------------------------------------: |
| 功能测试 | 对于gs_collector参数的错误值及不适用的数据库版本，会打印合理的报错信息 |
| 功能测试 |              参数配套使用，采集正常              |
| 功能测试 | 收集配置文件json如何正确配置，指定的配置文件收集节点的信息 |
| 功能测试 |       传统模式、资源池化模式采集节点信息        |
| 资料测试 | 资料对于新增加的功能描述准确，示例的执行结果正确，整体质量良好 |

## 3.2   约束说明

- gs_collector工具适配资源池化目前只支持openGauss版本：5.1.1
- 只能数据库用户执行（非root）
- 目前仅能收集用户设定时间范围内存在的备节点日志信息
- 目前仅能收集共享磁盘中已有内容的文件信息

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| :------: | :------: | :------: | :----------------: | :------: |
|    NA    |          |          |                    |          |

### 3.3.2 问题统计

|             | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| :---------: | :------: | :--: | :--: | :--: | :----: |
|  数目 (个)  |    4     |  0   |  0   |  3   |   1    |
| 百分比  (%) |   100    |  0   |  0   | 75%  |  25%   |

### 3.3.3 问题详情

| 序号 |                           issue号                            | 问题级别 |                 问题简述                  | 问题状态 |
| :--: | :----------------------------------------------------------: | :------: | :---------------------------------------: | :------: |
|  1   | [I8KR0H](https://e.gitee.com/opengaussorg/dashboard?issue=I8KR0H) |   次要   |        工具收集信息时开始时间和结束时间问题         |  已取消  |
|  2   | [I8L11B](https://e.gitee.com/opengaussorg/dashboard?issue=I8L11B) |   不重要   | gs_collector工具收集DataNode配置信息的结果目录错误  |  已完成  |
|  3   | [I8L46I](https://e.gitee.com/opengaussorg/dashboard?issue=I8L46I) |   次要   |    gs_collector工具收集core信息未能收集到结果      |  已完成  |
|  4   | [I8MGPN](https://e.gitee.com/opengaussorg/dashboard?issue=I8MGPN) |  次要  |   gs_collector文档收集 CoreDump 描述有问题   |  已完成  |

# 4     测试执行

## 4.1   测试执行统计数据

### 4.1.1   gs_collector参数验证

| 测试步骤：                                                   | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. gs_collector参数验证<br/>2. 错误的参数有明确的的提示信息 | 执行10条用例，发现1个bug，非问题取消 |

###  4.1.2   参数组合采集信息

| 测试步骤：                                                   | 测试结果                                   |
| ------------------------------------------------------------ | ------------------------------------------ |
| 参数配套使用，采集正常 | 执行12条用例，执行通过 |

###  4.1.3  指定json配置，采集信息

| 测试步骤：                                                   | 测试结果                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| 1. 基于某个特定信息配置专属json配置收集 | 执行22条用例，发现2个bug，现已修复且验收通过 |

###  4.1.3   资料测试

| 测试步骤：                                                | 测试结果                                    |
| --------------------------------------------------------- | ------------------------------------------- |
| 1. 资料对于新增加的功能描述准确<br/>2. 示例的执行结果正确 | 发现1个bug，1个bug已修复且验收通过 |

## 4.2  测试执行统计数据

| 版本名称                       | 测试用例数   | 用例执行结果             | 发现问题单数 |
| ------------------------------ | ------------ | ------------------------ | ------------ |
| openGauss 5.1.1 build 45f6613a | 44           | Passed：41<br>Failed：3 | 3            |
| openGauss 5.1.1 build 01b191f0 | 回归issue：3<br/>阻塞：4  | Passed：7<br>Failed：0   | 0            |
| openGauss 5.1.1 build 79509ee2 | 回归issue：3<br/>阻塞：4 | Passed：7<br>Failed：0   | 0            |

*数据项说明：*

* 累计发现缺陷单4个.4个缺陷均已解决且回归通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度: 3(缺陷个数)/0.742kloc(代码行数)=4.04(个/kloc)

## 4.3  后续测试建议

无

# 5     附件

无