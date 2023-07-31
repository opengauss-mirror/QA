![avatar](./images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。
修订记录

| 日期         | 修订   版本 | 修改描述            | 作者  |
|------------|---------|-----------------|-----|
| 2023.07.13 | V1.0    | 新建              | 江鹏 |
| 2023.07.14 | V2.0    | 修改              | 江鹏 |
| 2023.07.18 | V3.0    | 根据评审意见修改        | 江鹏 |
| 2023.07.21 | V4.0    | 修改接口测试描述和性能测试描述 | 彭斐灵 |

 关键词：



摘要：



缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |

# 1     特性概述

版本包括的特性有：SQL诊断、慢SQL监控、日志检索、实例监控。

1.通过监控硬件、OS和数据库指标以及TOP SQL相关信息 ，为用户观测数据库提供首要入口。

2.支持监控慢SQL和创建诊断任务 ，通过对应用、数据库、OS、硬件的全栈跟踪，实现基于专家模型的故障诊断。

3.支持数据库、OS日志在Web端的统一检索。  

<br>

630版本新增的功能有：实例监控、历史数据诊断、告警监控。

1.通过监控实例OS资源（CPU、内存、磁盘IO、网络）和实例指标（TPS、QPS、连接数、慢SQL数、活跃会话数量、实时TOPSQL、阻塞会话、长事务）以及TOP SQL（DB_TIME、CPU_TIME、EXEC_TIME）等相关信息 ，为用户观测数据库提供可视化入口。

2.支持创建历史数据诊断任务 ，实现对IO（磁盘利用率）和CPU（CPU占用率、数据库进程CPU占用率、其他进程CPU占用率、活跃会话数量、线程池使用率、SQL执行时长、慢SQL数量、等待事件数量）的诊断分析。

3.支持邮件通知的方式对实例OS资源（磁盘读速率过高、磁盘写速率过高、磁盘使用量过高、内存使用率过高、CPU使用率过高）的告警监控。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称 | 测试起始时间     | 测试结束时间     |
|------|------------|------------|
| 第一轮测试 | 2023.05.29 | 2023.06.16  |
| 第二轮测试 | 2023.06.19 | 2023.07.13  |
|           |            |             |


| 环境信息      | 配置信息                                                 | 备注 |
| ------------- | ------------------------------------------------------------ | ---- |
| 虚拟机 | Intel Xeon Processor (Cascadelake) CPU @ 2.00GHz 8核<br/>内存：16GB<br/>硬盘：150G<br/>OS：CentOS Linux 7.6 <br> ||
|软件|ElasticSearch: 8.3.3<br>Filebeat: 8.3.3<br>Java：11<br>python: 3.79<br>|      |
|  |  |   |

# 3     测试结论概述

## 3.1   测试整体结论

进行了2轮测试，覆盖了实例监控、历史数据诊断、告警监控的特性，设计了526个测试用例，用例经过社区的评审，整个测试过程覆盖了功能测试、接口测试、性能测试。
总共发现139个问题，目前遗留了3个问题，整体质量良好。

| 测试活动 | 活动评价                                                                                |
|------|-------------------------------------------------------------------------------------|
| 功能测试 | 测试实例监控（首页-资源使用率图表、首页-活跃会话数量、首页-实时TOPSQL、首页-阻塞会话、首页-长事务、资源监控-CPU、资源监控-内存、资源监控-磁盘IO、资源监控-网络、实例指标-TPS、实例指标-QPS、实例指标-连接数、实例指标-慢SQL数、TOPSQL-DB_TIME、TOPSQL-CPU_TIME、TOPSQL-EXEC_TIME）、历史数据诊断（磁盘利用率、CPU占用率、数据库进程CPU占用率、其他进程CPU占用率、活跃会话数量、线程池使用率、线程池使用率、SQL执行时长、慢SQL数量、等待事件数量）、告警监控（告警记录、告警配置、告警模板、通知模板、通知方式），测试达到预期结果 |
| 接口测试 | 测试slowsql、sql诊断、安装部署列表查询、告警、历史诊断、日志检索、实例监控的http接口，测试达到预期结果   |
| 压力测试 | 测试slowsql、sql诊断、安装部署列表查询、告警、历史诊断、日志检索、实例监控的常用接口，测试达到预期结果   |
|         |                       |

## 3.2   约束说明

**日志检索**
（1）FileBeat版本由8.3.3进行二次开发新增openGauss Module
（2）需要在openGauss实例所在服务器安装FileBeat，以及FileBeat配置文件要求
（3）ES版本与FileBeat版本保持一致
（4）用户需要自行搭建ElasticSearch环境；ElasticSearch服务器性能要求、ElasticSearch配置文件要求
（5）Java 11及以上版本
（6）需要根据约定规则设计ES索引名称与插入具体字段值
（7）ElasticSearch需要集中存储多实例的日志，对存储容量要求较高，实际根据数据库实例数量、实例日志数量、ElasticSeach部署方式决定；

**实例监控**
（1）使用平台插件安装部署exporter和Prometheus

**SQL诊断-性能分析**
（1）仅支持Linux版本在4.1或以上版本
（2）BCC版本需与内核版本匹配
（3）Python3或以上版本
（4）java 11或以上版本
（5）agent工具使用Java语言编写，但eBPF需与Linux内核交互，故只支持Linux平台
（6）需与Linux内核交互对性能有较高的要求，因此对于openGauss数据库的硬件系统有较高的要求。

**SQL诊断与慢SQL**
（1）需要在openGauss实例所在服务器安装bcc，以及安装诊断数据采集服务
（2）Java 11或以上版本
（3）个人开发者最低配置2核4G，推荐配置4核8G。预留50%以上的磁盘剩余空间用于数据存储。

**自动部署**
（1）需要先在服务器上安装yum命令

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态  |
| -------- | -------- |------|-----------|-------|
|708932215|	【实例监控】性能指标低于0.7的接口需优化| 一般   |   因为opengauss本身查询慢，暂无法处理。      |    新建    |
|708935067|	【SQL诊断】slowsql性能指标低于0.7的接口需要优化| 一般   |   因为opengauss本身查询慢，暂无法处理。       |    新建    |
|708935971|	【SQL诊断】慢SQL查询数据后，点击最后几页，页面数据刷新很慢。| 一般 |sql本身查询慢，暂无法处理。  |  新建  |
|         |        |        |        |      |

> 社区openGauss已建issue
> 
> (1) 创建WDR时报错：https://gitee.com/opengauss/openGauss-server/issues/I7MZPT?from=project-issue
> 
> (2) statement_history表查询过慢：https://gitee.com/opengauss/openGauss-server/issues/I7OEE2?from=project-issue

 
### 3.3.2 问题统计

|        | 问题总数 | 关键  | 重要  | 一般  | 提示 |
| ------ |------|-----|-----|-----|-----|
| 数目   | 139   | 7  | 44  | 76  | 12   |
| 百分比 | 100%  | 5% | 32% | 54% | 9%   |
|        |      |    |      |     |      |

# 4     测试执行

## 4.1   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果      | 发现问题单数 |
|------|-------|----------------|--------|
| 第一轮测试  | 526  | 执行305，通过率72%  |    102    |
| 第二轮测试  | 526  | 执行526，通过率90%  |    37     |
|            |      |                    |           |

进行问题回归后，测试用例数共526条，通过率99%。

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*发现问题单数－－本测试活动总共发现的问题单数。*

## 4.2 测试执行步骤

###  4.2.1 实例监控

| 测试步骤                            | 测试结果                       |
|---------------------------------|----------------------------|
| 1、实例监控首页OS资源、活跃会话数量、实时TOPSQL、阻塞会话、长事务数据的图表显示  | 执行39条用例，发现13个bug，现遗留1个问题，其他问题已修复且验证通过 |
| 2、资源监控CPU、内存、磁盘IO、网络的数据图表显示  | 执行45条用例，发现19个bug，现已修复且验证通过   |
| 3、实例指标TPS、QPS、连接数、慢SQL数的数据图表显示| 执行8条用例，发现1个bug，现已修复且验证通过|
| 4、TOPSQL DB_TIME、CPU_TIME、EXEC_TIME的数据图表显示| 执行31条用例，发现6个bug，现已修复且验证通过|
|                   |                           |

### 4.2.2 历史数据诊断

| 测试步骤                 | 测试结果           |
|----------------------|----------------|
| 1、选择实例和诊断时间，勾选锁分析，进行IO和CPU的诊断阈值配置，进行诊断，查看分析诊断报告 | 执行26条用例，发现20个bug，现遗留1个问题，其他问题已修复且验证通过 |
|                       |                |


### 4.2.3 告警监控
| 测试步骤                    | 测试结果                      |
|-------------------------|---------------------------|
| 1、告警记录：告警记录数据统计图表显示、告警详情显示 | 执行23条用例，发现18个bug，现已修复且验证通过 |
| 2、告警配置：对实例配置告警模板和告警规则 | 执行18条用例，发现5个bug，现已修复且验证通过 |
| 3、告警模板：新增、编辑、删除告警模板，配置告警规则 | 执行21条用例，发现9个bug，现已修复且验证通过 |
| 4、告警规则：查看告警规则 | 执行30条用例，发现5个bug，现已修复且验证通过 |
| 5、通知模板：新增、编辑、修改通知模板 | 执行53条用例，发现4个bug，现已修复且验证通过 |
| 6、通知方式：新增、编辑、修改通知方式，配置和测试通知渠道 | 执行46条用例，发现3个bug，现已修复且验证通过 |
|                     |                        |

### 4.2.4 旧版本特性回归测试
| 测试步骤                    | 测试结果                      |
|-------------------------|---------------------------|
| 1、SQL诊断特性回归测试 | 执行64条用例，发现18个bug，现遗留2个问题，其他问题已修复且验证通过 |
| 2、日志检索特性回归测试 | 执行90条用例，发现13个bug，现已修复且验证通过 |
| 3、WDR报告特性回归测试| 执行32条用例，发现8个bug，其他问题已修复且验证通过|
|                       |                        |

### 4.3 测试执行数据

**630版本新增功能的测试执行数据**

| 功能         | 测试用例数 | 用例最终执行结果     | 发现问题单数      |
|------------|-------|--------------|-------------|
| 实例监控    | 123    |执行123，通过率100%|   39          |
| 历史数据诊断| 26     |执行26，通过率100%|    20          |
| 告警监控    | 191    |执行191，通过率100%|   44           |
|      |      |          |           |

**旧版本功能回归的测试执行数据**

| 功能         | 测试用例数 | 用例最终执行结果     | 发现问题单数      |
|------------|-------|--------------|-------------|
| SQL诊断    | 64   |执行64，通过率98%|   13          |
| 日志检索    | 90   |执行90，通过率100%|    15          |
| WDR报告    | 32    |执行32，通过率97%|   8          |
|            |      |          |           |

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*用例最终执行结果－－在问题修复后，最终用例执行情况。*

*发现问题单数－－本测试活动总共发现的问题单数。

## 4.3 问题分析

**本版本（630版本）的问题主要集中在实例监控、历史数据诊断、告警监控和日志检索。前期主要是实例监控功能出现问题，采集的数据有误或与需求存在差异；告警监控的问题主要是集中在告警记录、告警规则、告警模板、告警配置功能上的问题；其次是历史数据诊断的诊断详情、样式显示、诊断任务新建与执行、图表显示、指标数据显示问题。其他问题主要包括页面跳转、组件显示、图表显示等问题。**

本版本出现的问题主要是在编码时未考虑到数据采集、业务关联性、边界值、特殊字符编码、实时刷新、异常情况的处理，导致出现了较多问题。


以下为截至目前为止，630版本问题的分布图：

<div>
<div style="float:left;">
<img src="images/智能运维问题分布图-按特性.png" width = "500" height = "400" alt="问题分布图-特性" />
</div>
<div>
<img src="images/智能运维问题分布图-按重要级别.png" width = "500" height = "400" alt="问题分布图-重要级别" />
</div>
</div>
<br>
  

**实例监控**出现的主要问题如下：

| 序号    |        问题         |                    处理              |
|-------|---------------------|--------------------------------------|
| 1     | 慢SQL数量不正确       |   慢SQL的采集有误，更换正确的统计方式  |
| 2     | TOPSQL，点击SQLID进入到SQL诊断页面，创建诊断任务，SQL不能修改参数。| 程序把SQL中变量代替为常量 |  
| 3     | 会话监控，刷新页面重新进入会话监控，折线图显示异常。 |修复前端代码，增加延时0.1秒再resize一下echarts。|  
| 4     | 网络_TCP socket数量统计和UDP socket数量统计图标显示数据有误。|  计算公式错误，与业务不符，修改计算公式。 |  
| 5     | 会话详情--有些会话的等待事件记录不显示 |  等待事件记录统计SQL有误，修改为正确的SQL。|  
| 6     | 实例监控、实例指标，图表不会自动刷新，点击刷新也没反应。|  修复前端代码  |  
| 7     | 会话详情-阻塞树数据不正确 |  阻塞树数据统计SQL有误，修改为正确的SQL。  |  
| 8     | 资源监控，内存显示数据不对     |  计算公式错误，修改计算公式。 |  
| 9     | 时间插件显示不正确。    |   修复前端代码 |
| 10    | 实时TOPSQL，点击会话ID，内容显示为空。    |   修改前端，会话关闭时增加相应提示 | 
| 11    | 首页，活跃会话数量鼠标点击选中图表，指标数据显示不全。    |  修改前端，固定高度，鼠标可以进入，进行上下滚动 |  
| 12    | WDR报告，连续多次点击查询按钮后，列表的操作按钮无法点击。    |  修复前端代码 |  
| 13    | wdr生成报告_选择快照_下拉列表不显示全部已有的快照    |  修改前端分页参数，获取9999条记录 |


**历史数据诊断**出现的主要问题如下：

| 序号 |        问题         |                    处理              |  
|------|---------------------|--------------------------------------|
|   1  |  分析报告中其他进程CPU当前使用率较高，CPU使用率有误。  |  问题原因：CPU使用率显示未除以CPU核数、国际化文字描述错误。解决方式：根据prometheus获取CPU核数相除、修改国际化文字描述。                             |  
|   2  |  诊断阈值，输入小数报错。|  类型转换错误，修复类型转换。                          |  
|   3  |  分析报告详情，数据库进程当前使用CPU过高，优化显示样式。|   增加状态处理：不满足条件未执行的诊断点显示、执行异常的诊断点显示、进行了诊断且诊断结果符合阈值显示、进行了诊断但诊断结果不符合阈值显示                            |  
|   4  |  勾选锁分析，报错Request Error。  |  当时服务器处于不稳定状态，依赖的工具未安装或连不上服务器，修改错误提示。  |  
|   5  |  诊断结果时间不完整。 |      修复前端代码  |  
|   6  |  topsql项数据显示很慢 |   问题原因：表查询很慢，对sql进行SQL诊断发现死亡元组比例99.66%,查询时估算扫描130多万行，真实数据只有5万多行。解决方式：可对表进行vacuum清理。                            |  
|   7  | 业务连接数显示为空 |     修改前后端代码  |  
|   8  |  系统CPU当前使用率、数据库进程CPU平均使用率、等待事件分析-当前等待事件的诊断结果显示异常。 |从Prometheus取数时的step过长，修改时间间隔为10000秒内，step为15，时间间隔大于10000秒以上（包含），step=时间间隔/120，修改诊断结果描述为等待事件总数 |  
|   9  |【性能】指标低于0.7的接口需优化 |未用多线程，解决方案：添加多线程执行诊断，因需等待线程执行完成，故添加了1s等待时间| 
|   10  | 数据库进程CPU平均使用率不高，但是出来了诊断点 |修改拓扑图展示方式，当子节点为展示节点时，父节点超过阈值为建议点时，父节点和子节点都展示，当不为建议点时，父节点和子节点都隐藏| 
|   11  | 当前数据库CPU高的诊断点，满足条件但是没有显示 |问题原因：阈值判断有问题，解决方案：增加了数据库进程CPU使用率阈值| 
|   12  | 勾选锁分析进行诊断，诊断结果可选项没勾选锁分析。 |前后端联调问题，解决方案：修改前后端联调，前端可不传，后端自行判断；修改数据返回格式| 
|       |          |                   | 

**告警监控**出现的主要问题如下：

| 序号 |        问题         |                    处理              |  
|------|---------------------|--------------------------------------|
|   1  | 选择告警规则_点击行操作菜单“编辑”_提示要求先勾选 |  去掉该限制条件  |  
|   2  | 下拉选择实例/集群、告警时间范围后，告警列表记录没有刷新。  |  实例/集群传的是数组，平台在处理get请求时，无法将string(yyyy-MM-dd HH:mm:ss)参数转化为日期参数，导致查询出错，已修复。      |  
|   3  | 统计图中没有显示数量。      |     增加统计图的数量    |  
|   4  | 列表记录告警等级和告警状态，没有显示对应的图标。  |  增加对应的状态图标   |  
|   5  | 搜索输入框输入内容，规则搜索输入框会同步显示。 |  修复输入框错误   |  
|   6  | 编辑模板，勾选告警规则点击保存后，规则没保存成功。 |   页面刷新后，数据重新排序造成的问题，修复：修改为按照id的倒序排数据，即最新创建的数据会排在前面，修改数据后，数据的位置不变 |  
|   7  | 删除在告警配置被引用的告警模板，却可以删除成功。 | 告警配置被引用的告警模板不再允许删除，删除时，会提示“删除失败,模板正被使用，无法删除，需要到告警配置中解除该模板，才能删除”|  
|   8  | 下拉规则类型为日志时，报错alertRule.tableDataFail。|   当查询的结果为空时，未直接返回空数据，已修复。  |  
|   9  | 告警模板页数选择20条/页，模板列表记录显示不全 |   更改前端样式  |  
|   10  | 告警规则名称搜索框未支持模糊搜索。 |    当查询的结果为空时，未直接返回空数据，已修复 |  
|   11  | 告警插件过一段时间会自动停用 |    代码问题，已修复。   |  
|   12  | 告警记录分页_切换单页条数_查询异常 |   修复翻页功能。   |  
|       |          |                   | 

## 4.4 压测

对slowsql、sql诊断、安装部署列表查询、告警、历史诊断、日志检索、实例监控特性进行压测，在10秒钟内开启100个线程组，循环10次，进行了1000次采样。

slowsql、sql诊断、安装部署列表查询、告警的接口请求通过率为100%，整体性能指数在0.797 到1之前（预期0.7以上），整体使用满意度良好。

历史诊断的接口请求通过率为100%，整体性能指数0.880  （预期0.7以上），整体使用满意度良好。/historyDiagnosis/api/v1/tasks接口性能指数为0.521，该接口进行各项诊断，目前可支持使用。

日志检索的接口请求通过率为100%，整体性能指数0.626 （预期0.7以上），整体使用满意度一般。且logs_default接口的性能指数为0.590，logs接口的性能指数为0.625，logDistributionMap接口的性能指数为0.625，需进一步优化。

实例监控的接口请求通过率为100%，整体性能指数0.785 （预期0.7以上），整体使用满意度良好。但/instanceMonitoring/api/v1/network接口的性能指数为0.606，/instanceMonitoring/api/v1/io接口的性能指数为0.611，/instanceMonitoring/api/v1/mainMetrics接口的性能指数为0.662，需进一步优化。

slowsql、安装部署列表查询、日志检索、实例监控特性涉及接口的每秒吞吐量在17~56之间（预期在10以上），可供日常DBA管理使用。
sql诊断涉及接口的每秒吞吐量大多在4至9之间（预期在10以上），还需继续优化。
告警涉及接口的每秒吞吐量大多在8左右（预期在10以上），还需继续优化。
历史诊断涉及接口的每秒吞吐量大多在0.1左右（预期在10以上），还需继续优化。

**性能指数**：

slowsql性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|0.797  |500 ms	|1 sec 500 ms	|Total|
|0.797  |500 ms	|1 sec 500 ms	|slowSqls|
|1.000  |500 ms	|1 sec 500 ms	|BeanShell Sampler|
|1.000  |500 ms	|1 sec 500 ms	|login|
|      |      |      |      |

sql诊断性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|0.963  |500 ms	|1 sec 500 ms	|Total|
|0.911  |500 ms	|1 sec 500 ms	|/sqlDiagnosis/api/v1/diagnosisTasks|
|1.000  |500 ms	|1 sec 500 ms	|delete_diagnosisTask|
|1.000  |500 ms	|1 sec 500 ms	|BeanShell Sampler|
|1.000  |500 ms	|1 sec 500 ms	|/sqlDiagnosis/api/v1/clusters|
|1.000  |500 ms	|1 sec 500 ms	|/suggestions/TaskInfo|
|1.000  |500 ms	|1 sec 500 ms	|login|
|      |      |      |      |

安装部署列表查询性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|0.960  |500 ms	|1 sec 500 ms	|Total|
|0.927  |500 ms	|1 sec 500 ms	|exporter|
|0.933  |500 ms	|1 sec 500 ms	|filebeat|
|0.942  |500 ms	|1 sec 500 ms	|agent|
|1.000  |500 ms	|1 sec 500 ms	|BeanShell Sampler|
|1.000  |500 ms	|1 sec 500 ms	|elasticsearch|
|1.000  |500 ms	|1 sec 500 ms	|prometheus|
|1.000  |500 ms	|1 sec 500 ms	|login|
|      |      |      |      |

告警性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|1.000  |500 ms	|1 sec 500 ms	|Total|
|1.000  |500 ms	|1 sec 500 ms	|BeanShell Sampler|
|1.000  |500 ms	|1 sec 500 ms	|/plugins/alert-monitor/api/v1/alertTemplate|
|1.000  |500 ms	|1 sec 500 ms	|/plugins/alert-monitor/api/v1/alertRecord|
|1.000  |500 ms	|1 sec 500 ms	|【创建】/plugins/alert-monitor/api/v1/alertTemplate|
|1.000  |500 ms	|1 sec 500 ms	|login|
|1.000  |500 ms	|1 sec 500 ms	|/plugins/alert-monitor/api/v1/alertClusterNodeConf|
|1.000  |500 ms	|1 sec 500 ms	|/plugins/alert-monitor/api/v1/alertRule|
|1.000  |500 ms	|1 sec 500 ms	|【编辑】/plugins/alert-monitor/api/v1/alertTemplate|
|1.000  |500 ms	|1 sec 500 ms	|【删除】/plugins/alert-monitor/api/v1/alertTemplate|
|      |      |      |      |

历史诊断性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
| 0.880 | 500 ms | 1 sec 500 ms | Total |
| 0.521 | 500 ms | 1 sec 500 ms | /historyDiagnosis/api/v1/tasks |
| 1.000 | 500 ms | 1 sec 500 ms | BeanShell Sampler |
| 1.000 | 500 ms | 1 sec 500 ms | /historyDiagnosis/api/v1/thresholds |
| 1.000 | 500 ms | 1 sec 500 ms | /suggestPoints/all=false |
| 1.000 | 500 ms | 1 sec 500 ms | /historyDiagnosis/api/v1/options |
| 1.000 | 500 ms | 1 sec 500 ms | login |

日志检索性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|0.626  |500 ms	|1 sec 500 ms	|Total|
|0.590  |500 ms	|1 sec 500 ms	|logs_default|
|0.625  |500 ms	|1 sec 500 ms	|logs|
|0.625  |500 ms	|1 sec 500 ms	|logDistributionMap|
|1.000  |500 ms	|1 sec 500 ms	|	logDistributionMap|
|1.000  |500 ms	|1 sec 500 ms	|login|
|      |      |      |      |

实例监控性能指数：

| APDEX | T(Toleration threshold) |  F(Frustration threshold) | label |
|------|---|----------------|--------|
|0.785  |500 ms	|1 sec 500 ms	|Total|
|0.606  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/network|
|0.611  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/io|
|0.662  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/mainMetrics|
|0.708  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/topSQLNow|
|0.712  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/topOSProcessAndDBThread|
|0.869  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/memory|
|0.898  |500 ms	|1 sec 500 ms	|/instanceMonitoring/api/v1/cpu|
|1.000  |500 ms	|1 sec 500 ms	|BeanShell Sampler|
|1.000  |500 ms	|1 sec 500 ms	|/observability/v1/param/paramInfo|
|1.000  |500 ms	|1 sec 500 ms	|/wdr/list|
|1.000  |500 ms	|1 sec 500 ms	|login|
|      |      |      |      |

*数据项说明*：
* Apdex指数：Apdex指数从0到1不等，其中0为最差的分数（100％的响应时间感到沮丧），而1为最佳的分数（100％的响应时间得到满意）
* T(Toleration threshold)：可接受（容忍或满意）阈值，即用户可接受的响应时间。
* F(Frustration threshold)：不可接受（失败）阈值，即1.5s以上的响应时间为用户不可接受的。
* Lable：采样器名称
* 计算： <=0.5s的样本归类为满意的样本，>0.5且<=1.5的样本归类为容许样本，>1.5的样本归类为失败的样本。
Apdex =（满意的样本+ 0.5x容许样本+ 0x失败的样本）/总样本


<br></br>

**事务统计**：

slowsql事务统计：

| Label | #Samples| Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total|  1002  |	0|	0.00%|	2586.23| 1  | 15473 |	14014.40 | 32.09 |
|BeanShell Sampler|  1  |	0|	0.00%|	1.00 | 1  | 1  |	1.00  | 0.00 |
|login|   1 |	0|	0.00%|	172.00| 172  | 172 |	172.00 | 3.03 |
|slowSqls|  1000  |	0|	0.00%|	2591.22| 23  | 15473 | 14030.80 | 17.00 |
|      |      |      |      |      |      |      |      |      |

sql诊断事务统计：

| Label | #Samples | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total|  5002  |	0|	0.00%|	226.10 | 1  | 7068 |	386.00 | 21.68 |
|/sqlDiagnosis/api/v1/clusters|  1000  |	0|	0.00%|	29.63 | 10  | 259  |	65.90  | 4.60 |
|/sqlDiagnosis/api/v1/diagnosisTasks|  2000  |	0 |0.00%| 486.91| 50 | 6516 |	558.60	 | 8.68 |
|/suggestions/TaskInfo|  1000  |	0|	0.00%|	10.91 | 4  | 166 | 16.00 | 4.35 |
|BeanShell Sampler|  1  |	0|	0.00%| 1.00 | 1   | 1 |	1.00 | 1000.00|
|delete_diagnosisTask|  1000  |	0|	0.00%|	116.42 | 60  | 7068 | 142.90| 4.35 |
|login|  1  |	0 |	0.00%| 161.00| 161  | 161 | 161.00 | 6.21 |
|      |      |      |      |      |      |      |      |      |

安装部署列表查询事务统计：

| Label | #Samples  | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total|  5002  |	0|	0.00%|	203.80 | 1  | 1520 |	462.00 | 269.93 |
|agent| 1000   |	0|	0.00%|	294.25 | 16  | 1059 |	523.00  | 55.38 |
|BeanShell Sampler| 1   |	0 |0.00%| 1.00| 1 | 1|	1.00	 | 1000.00 |
|elasticsearch|  1000  |	0|	0.00%|	57.50 | 7  | 549 | 135.90| 55.36 |
|exporter| 1000  |	0|	0.00%| 	306.32 | 19   | 1520 |	556.90 | 55.17 |
|filebeat|  1000  |	0|	0.00%|	297.72 | 18  | 1122 | 532.70 | 	55.23 |
|login| 1   |	0 |	0.00%| 240.00 | 240  | 240  | 240.00 | 	4.17 |
|prometheus|  1000  |	0 |	0.00%| 	63.37 | 7  | 471 | 149.00 | 55.16 |
|      |      |      |      |      |      |      |      |      |

告警事务统计：

| Label | #Samples  | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total|  	7002  |	0|	0.00%|	24.98 | 1  | 339 |	45.00 | 57.12 |
|/plugins/alert-monitor/api/v1/alertClusterNodeConf| 1000   |	0|	0.00%|	13.41 | 10  | 139 |	15.90  | 8.67 |
|/plugins/alert-monitor/api/v1/alertRecord| 1000   |	0 |0.00%| 10.14| 7 | 82|	13.00 | 8.67|
|/plugins/alert-monitor/api/v1/alertRule| 1000   |	0|	0.00%|	10.36 | 7  | 	10.36 | 15.00 | 8.66 |
|/plugins/alert-monitor/api/v1/alertTemplate|  1000  |	0|	0.00%|20.05 | 13   | 258 |	31.00 | 31.00 |
|BeanShell Sampler|  1  |	0|	0.00%|	1.00 | 1  | 1 | 1.00| 1000 |
|login|	1| 0 |	0.00%| 162.00| 162  | 162  | 162.00 | 6.17 |
|【创建】/plugins/alert-monitor/api/v1/alertTemplate|  1000  |	0 |	0.00%| 	46.89 | 27  | 321 | 62.00 |8.19 |
|【创建】/plugins/alert-monitor/api/v1/alertTemplate|  1000  |	0 |	0.00%| 32.41| 11  | 339  | 46.00| 	8.21 |
|【编辑】/plugins/alert-monitor/api/v1/alertTemplate|  1000  |	0 |	0.00%| 	41.51 |  28  | 273 | 58.00 | 8.21 |
|      |      |      |      |      |      |      |      |      |

历史诊断事务统计：

| Label | #Samples  | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)| Transcation/s |
|------|------|---|----------------|--------|---|---|---|---------------|
| Total | 4002 | 0 | 0.00% | 213.28 | 1 | 1716 | 695.70 | 4.72          | 10.63 | 3.26 |
| /historyDiagnosis/api/v1/options | 1000 | 0 | 0.00% | 3.47 | 2 | 93 |  6.00 | 1.30          | 0.67 | 0.56 |
| /historyDiagnosis/api/v1/tasks | 1000 | 0 | 0.00% | 696.52 | 461 | 1716 |  940.40  | 1.18          | 0.53 | 1.69 |
| /historyDiagnosis/api/v1/thresholds | 1000 | 0 | 0.00% | 5.79 | 3 | 88 |  8.00 | 1.30          | 5.92 | 0.57 |
| /suggestPoints/all=false | 1000 | 0 | 0.00% | 147.58 | 127 | 240 | 154.90 | 1.18          | 4.13 | 0.55 |
| BeanShell Sampler | 1 | 0 | 0.00% | 1.00 | 1 | 1 | 1.00 | 1000.00       | 0.00 | 0.00 |
| login | 1 | 0 | 0.00% | 179.00 | 179 | 179 | 179.00 | 5.59          | 3.51 | 2.91 |

日志检索事务统计：

| Label | #Samples  | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total| 3002   |	0|	0.00%|	730.08 | 2  | 1902 |	1083.70 | 100.31 |
|BeanShell Sampler|  1  |	0|	0.00%|	2.00 | 2 | 1982 |	2.00  | 500.00 |
|logDistributionMap| 1000   |	0 |0.00%| 656.94	| 15 | 1315|	1016.90 | 	0.10|
|login|  1  |	0|	0.00%|	162.00  | 162 | 162 | 162.00 | 6.17 |
|logs|  1000   |	0|	0.00%|744.03| 57   | 1339 |	1114.80 | 36.18 |
|logs_default|   1000  |	0|	0.00%|	790.57 | 79  | 1902 | 1156.90| 33.74 |
|      |      |      |      |      |      |      |      |      |

实例监控事务统计：

| Label | #Samples  | Fail  |Error| Average |Min(ms)|Max(ms)|90%(ms)|Transcation/s|
|------|------|---|----------------|--------|---|---|---|---|
|Total| 9002|	0|	0.00%|	 444.52 | 3  | 2268 |	915.00 | 185.65 |
|/instanceMonitoring/api/v1/cpu|  1000  |	0|	0.00%|	358.97 | 23 | 1493 | 605.90 | 21.31 |
|/instanceMonitoring/api/v1/io|  1000  |	0 |0.00%| 715.65	| 33	 | 1926 |	1127.80 | 	21.41 |
|/instanceMonitoring/api/v1/mainMetrics|  1000  |	0|	0.00%|	658.03  | 	61 | 	2090 | 	1070.70 | 21.12 |
|/instanceMonitoring/api/v1/memory|   1000 |	0|	0.00%| 401.57 | 54   | 1626 |	650.90 | 21.31 |
|/instanceMonitoring/api/v1/network|  1000  |	0|	0.00%|	720.41 | 33  | 2268 | 1115.90| 21.43 |
|/instanceMonitoring/api/v1/topOSProcessAndDBThread|  1000  |	0|	0.00%|	526.70  | 378 | 1016  | 639.90| 21.24 |
|/instanceMonitoring/api/v1/topSQLNow|   1000 |	0|	0.00%|1710| 549.00   | 1072.00 |	1252.90	 | 21.22 |
|/observability/v1/param/paramInfo|  1000  |	0|	0.00%|	10.52 | 5 | 102 | 	15.00 | 21.11 |
|/wdr/list|   1000 |	0|	0.00%|	13.42  | 5 | 134 | 20.00 | 21.06 |
|BeanShell Sampler|  1  |	0|	0.00%|3.00| 3   |3 |	3.00 | 333.33 |
|login|  1  |	0|	0.00%|	188.00 |188  | 188 | 188.00| 	5.32 |
|      |      |      |      |      |      |      |      |      |

*数据项说明*：
* Lable：采样器名称
* Fail：失败数
* Error: 错误率
* Average: 平均响应时间
* Min: 最小响应时间
* Max: 最大响应时间
* 90%：90%在该响应时间以下
* Transcation：吞吐量


## 4.5 接口测试
| 接口数 | 用例数 | 通过率 |
|-------|--------|----------------|
|   90  |   224  |       99%      |
|       |        |                |


本次接口测试共覆盖90个接口，执行了224个接口测试用例，其中成功223个，失败1个，失败用例是因为数据库生成的WDR报告本身大小为0，是空文件，导致下载不了WDR报告。


接口覆盖情况：

|序号| 类或模块名称 | 接口名称| 接口请求方法 | 接口地址 | 是否覆盖 | 是否通过 | 备注                                                                                                        |
|---|------------|--------|-----------|--------|------|---|-----------------------------------------------------------------------------------------------------------|
| 1  | public-controller |  listCluster |	GET |	/sqlDiagnosis/api/v1/clusters |	是 | 是 | 		                                                                                                        |
| 2  | public-controller | DATABASE |	GET |	/sqlDiagnosis/api/v1/clusters/{clusterId}/instances |	是 | 是 | 	                                                                                                         |	
| 3  | public-controller | configList |	GET |	/sqlDiagnosis/api/v1/node-settings |	是 | 是 | 	                                                                                                         |		
| 4  | public-controller | configSet |	PUT |	/sqlDiagnosis/api/v1/node-settings/{nodeid}	 |	是 | 是 | 	                                                                                                         |	
| 5  | cluster-controller|	实例信息 |	GET |	/logSearch/api/v1/clusters |	是 | 是 | 		                                                                                                        |
| 6  | log-search-controller | Log Distribution Map |	GET |	/logSearch/api/v1/logDistributionMap |	是 | 是 | 	                                                                                                         |	
| 7  | log-search-controller | LogLevel Info |	GET |	/logSearch/api/v1/logLevels |	是 | 是 | 	                                                                                                         |	
| 8  | log-search-controller | LogType Info |	GET |	/logSearch/api/v1/logTypes |	是 | 是 | 	                                                                                                         |
| 9  | log-search-controller | Log Info |	GET |	/logSearch/api/v1/logs |	是 | 是 | 		                                                                                                        |	
| 10 | log-search-controller | logContextSearch |	GET |	/logSearch/api/v1/logContextSearch |	是 | 是 | 	                                                                                                         |	
| 11 | Slow SQL	List |  Slow SQLs |	GET |	/sqlDiagnosis/api/v1/slowSqls |	是 | 是 | 		                                                                                                        |	
| 12 | open-controller |	diagnosisResult	| POST |	/sqlDiagnosis/api/open/v1/diagnosisTasks/{id}/result |	是 | 是 | 	                                                                                                         |	
| 13 | sql-diagnosis-controller |	page |	GET	 |/sqlDiagnosis/api/v1/diagnosisTasks |	是 | 是 | 		                                                                                                        |	
| 14 | sql-diagnosis-controller |	start |	POST |	/sqlDiagnosis/api/v1/diagnosisTasks	 |	是 | 是 | 		                                                                                                        |
| 15 | sql-diagnosis-controller	 |plan |	GET |	/sqlDiagnosis/api/v1/diagnosisTasks/plan/{nodeId}/{sqlId} |	是 | 是 | 	                                                                                                         |			
| 16 | sql-diagnosis-controller	 |res |	GET |	/sqlDiagnosis/api/v1/diagnosisTasks/res/{id}.{type} |	是 | 是 | 	                                                                                                         |		
| 17 | sql-diagnosis-controller	 |delete |	DELETE |	/sqlDiagnosis/api/v1/diagnosisTasks/{id} |	是 | 是 | 	                                                                                                         |
| 18 | sql-diagnosis-controller	 |detail |	GET |	/sqlDiagnosis/api/v1/diagnosisTasks/{id}/suggestPoints |	是 | 是 | 	                                                                                                         |		
| 19 | sql-diagnosis-controller	 |result |	GET |	/sqlDiagnosis/api/v1/diagnosisTasks/{id}/suggestions/{type}	 |	是 | 是 | 	                                                                                                         |		
| 20 | instance-controller |	paramInfo |	GET |	/observability/v1/param/paramInfo |	是 | 是 | 			                                                                                                       |
| 21 | wdr  |	wdr报告列表 |		GET |	/wdr/list |	是 | 是 | 	                                                                                                         |
| 22 | wdr  |	生成wdr报告 |		POST |	/wdr/generate |	是 | 是 | 	                                                                                                         |
| 23 | wdr  |	快照列表 |		GET	 |/wdr/listSnapshot |	是 | 是 | 	                                                                                                         |
| 24 | wdr  |	创建快照 |		GET |	/wdr/createSnapshot	 |	是 | 是 | 	                                                                                                         |
| 25 | wdr  |	删除wdr报告 |		GET	 |/wdr/del/{id}	 |	是 | 是 | 	                                                                                                         |
| 26 | wdr  |	下载wdr报告 |		GET |	/wdr/downloadWdr	 |	是 | 是 | 接口请求通过，有接口用例执行失败，数据库内部的报错“Instance reset time is different”，旧的快照不能用来生成WDR，WDR报告本身大小为0，故而断言失败。新的快照生成WDR成功。 |
| 27 | 安装部署	 | 实例详情-pkg |		GET	 |/observability/v1/environment/pkg	 |	是 | 是 | 	                                                                                                         |
| 28 | 安装部署	 | sql诊断-agent列表 |		GET |	/plugins/observability-sql-diagnosis/observability/v1/environment/agent |	是 | 是 | 	                                                                                                         |
| 29 | 安装部署	 | 实例详情-exporter列表 |		GET |	/plugins/observability-instance/observability/v1/environment/exporter	 |	是 | 是 | 	                                                                                                         |
| 30 | 安装部署	 | 实例详情-prometheus列表 |		GET |	/plugins/observability-instance/observability/v1/environment/prometheus	 |	是 | 是 | 	                                                                                                         |
| 31 | 安装部署	 | 日志检索-es列表 |		GET |	/plugins/observability-log-search/observability/v1/environment/elasticsearch	 |	是 | 是 | 	                                                                                                         |
| 32 | 安装部署	 | 日志检索-filebeat列表 |		GET |	/plugins/observability-log-search/observability/v1/environment/filebeat |	是 | 是 | 	                                                                                                         |	
| 33 | 安装部署	 | 环境-sql诊断-主机列表 |		GET	 |/plugins/observability-sql-diagnosis/observability/v1/environment/hosts	 |	是 | 是 | 	                                                                                                         |
| 34 | 安装部署	 | 环境-实例详情-主机列表 |		GET |	/plugins/observability-instance/observability/v1/environment/hosts	 |	是 | 是 | 	                                                                                                         |
| 35 | 安装部署	 | 环境-日志检索-主机列表 |		GET |	/plugins/observability-log-search/observability/v1/environment/hosts |	是 | 是 | 	                                                                                                         |
| 36 | history-diagnosis  | 	发起诊断 |		POST	 |/historyDiagnosis/api/v1/tasks |	是 | 是 | 		                                                                                                        |
| 37 | history-diagnosis  |	查看单个节点数据 |		GET |	/historyDiagnosis/api/v1/tasks/{id}/points/{pointName}	 |	是 | 是 | 	                                                                                                         |
| 38 | history-diagnosis  |	查询拓扑图 |		GET |	/historyDiagnosis/api/v1/tasks/6/suggestPoints/all=false |	是 | 是 | 		                                                                                                        |
| 39 | history-diagnosis  |	查询全部阈值 |		GET |	/historyDiagnosis/api/v1/thresholds |	是 | 是 | 	                                                                                                         |	
| 40 | history-diagnosis  |	获取可选项 |		GET |	/historyDiagnosis/api/v1/options |	是 | 是 | 	                                                                                                         |		
| 41 | 实例监控_首页  |	首页-指标 |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/mainMetrics |	是 | 是 | 	                                                                                                         |
| 42 | 实例监控_首页  |	TOPSQL |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/topSQLNow |	是 | 是 | 	                                                                                                         |
| 43 | 实例监控_首页  |	会话监控 会话详情 |	GET |	/plugins/observability-instance/instanceMonitoring/api/v1/session/detail	 |	是 | 是 | 	                                                                                                         |
| 44 | 实例监控_资源监控  |	CPU |		GET	 |/plugins/observability-instance/instanceMonitoring/api/v1/cpu |	是 | 是 | 	                                                                                                         |
| 45 | 实例监控_资源监控  |	内存 |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/memory	 |	是 | 是 | 	                                                                                                         |
| 46 | 实例监控_资源监控  |	TOP命令 |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/topOSProcessAndDBThread	 |	是 | 是 | 	                                                                                                         |
| 47 | 实例监控_资源监控  |	IO |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/io	 |	是 | 是 | 	                                                                                                         |
| 48 | 实例监控_资源监控  |	网络 |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/network |	是 | 是 | 	                                                                                                         |
| 49 | 实例监控_实例指标  |	实例指标 |		GET |	/plugins/observability-instance/instanceMonitoring/api/v1/instance |	是 | 是 | 		                                                                                                        |
| 50 | alert-monitor  |	规则分页查询 |		GET |	/plugins/alert-monitor/api/v1/alertRule	 |	是 | 是 | 	                                                                                                         |
| 51 | alert-monitor  |	获取所有规则 |		GET |	/plugins/alert-monitor/api/v1/alertRule/ruleList	 |	是 | 是 | 	                                                                                                         |
| 52 | alert-monitor  |	规则详细 |		GET |	/plugins/alert-monitor/api/v1/alertRule/{ruleId}	 |	是 | 是 | 	                                                                                                         |
| 53 | alert-monitor  |	规则项相关信息 |		GET |	/plugins/alert-monitor/api/v1/alertRule/ruleItem/properties	 |	是 | 是 | 	                                                                                                         |
| 54 | alert-monitor  |	告警模板分页 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate	 |	是 | 是 | 	                                                                                                         |
| 55 | alert-monitor  |	获取所有告警数据 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate/list	 |	是 | 是 | 	                                                                                                         |
| 56 | alert-monitor  |	告警模板详细 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate/{id}	 |	是 | 是 | 	                                                                                                         |
| 57 | alert-monitor  |	模板下的规则分页接口 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate/{id}/rule	 |	是 | 是 | 	                                                                                                         |
| 58 | alert-monitor  |	获取模板下的所有规则 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate/{id}/rule/list	 |	是 | 是 | 	                                                                                                         |
| 59 | alert-monitor  |	模板规则详细 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate/ruleList/{templateRuleId}	 |	是 | 是 | 	                                                                                                         |
| 60 | alert-monitor  |	新增/修改告警模板 |		POST |	/plugins/alert-monitor/api/v1/alertTemplate	 |	是 | 是 | 		                                                                                                        |
| 61 | alert-monitor  |	新增/修改告警模板规则 |		POST |	/plugins/alert-monitor/api/v1/alertTemplate/templateRule	 |	是 | 是 | 	                                                                                                         |
| 62 | alert-monitor  |	删除告警模板 |		GET |	/plugins/alert-monitor/api/v1/alertTemplate	 |	是 | 是 | 	                                                                                                         |
| 63 | alert-monitor  |	告警配置列表 |		GET |	/plugins/alert-monitor/api/v1/alertClusterNodeConf	 |	是 | 是 | 		                                                                                                        |
| 64 | alert-monitor  |	告警配置详细 |		GET	 |/plugins/alert-monitor/api/v1/alertClusterNodeConf/clusterNode/{clusterNodeId}	 |	是 | 是 | 	                                                                                                         |
| 65 | alert-monitor  |	实例告警配置 |		POST |	/plugins/alert-monitor/api/v1/alertClusterNodeConf	 |	是 | 是 | 	                                                                                                         |
| 66 | alert-monitor  |	实例直接配置规则 |		POST |	/plugins/alert-monitor/api/v1/alertClusterNodeConf/alertTemplate	 |	是 | 是 | 		                                                                                                        |
| 67 | alert-monitor  |	告警记录列表 |		GET	 |/plugins/alert-monitor/api/v1/alertRecord	 |	是 | 是 | 		                                                                                                        |
| 68 | alert-monitor  |	告警记录统计 |		GET	 |/plugins/alert-monitor/api/v1/alertRecord/statistics	 |	是 | 是 | 	                                                                                                         |	
| 69 | alert-monitor  |	告警记录详情 |		GET	 |/plugins/alert-monitor/api/v1/alertRecord/{id}	 |	是 | 是 | 	                                                                                                         |
| 70 | alert-monitor  |	告警关联视图接口 |		GET |	/plugins/alert-monitor/api/v1/alertRecord/{id}/relation	 |	是 | 是 | 	                                                                                                         |
| 71 | alert-monitor  |	告警信息推送接口 |		GET |	/plugins/alert-monitor/api/v1/alerts	 |	是 | 是 | 	                                                                                                         |
| 72 | alert-monitor  |	集群/实例下拉框接口 |		GET |	/plugins/alert-monitor/api/v1/environment/cluster	 |	是 | 是 | 	                                                                                                         |
| 73 | alert-monitor  |	获取告警接口信息 |		GET |	/plugins/alert-monitor/api/v1/alertConf |	是 | 是 | 	                                                                                                         |
| 74 | alert-monitor  |	设置告警接口信息 |		POST |	/plugins/alert-monitor/api/v1/alertConf |	是 | 是 | 	                                                                                                         |
| 75 | alert-monitor  |	告警模板规则-通知 |		POST |	/plugins/alert-monitor/api/v1/environment/alertContentParam	 |	是 | 是 | 		                                                                                                        |
| 76 | alert-monitor  |	标记为未读 |		POST |	/plugins/alert-monitor/api/v1/alertRecord/markAsUnread	 |	是 | 是 | 		                                                                                                        |
| 77 | alert-monitor  |	标记为已读 |		POST |	/plugins/alert-monitor/api/v1/alertRecord/markAsRead	 |	是 | 是 | 	                                                                                                         |
| 78 | notify-configuration |	通知模板分页 |		GET	 | /plugins/alert-monitor/api/v1/notifyTemplate |	是 | 是 | 	                                                                                                         |
| 79 | notify-configuration |	通知模板详细 |		GET	 | /plugins/alert-monitor/api/v1/notifyTemplate/{id}	 |	是 | 是 | 	                                                                                                         |
| 80 | notify-configuration |	新增/修改通知模板 |		POST |	/plugins/alert-monitor/api/v1/notifyTemplate	 |	是 | 是 | 	                                                                                                         |
| 81 | notify-configuration |	删除通知模板 |		DELETE |	/plugins/alert-monitor/api/v1/notifyTemplate/{id}	 |	是 | 是 | 		                                                                                                        |
| 82 | notify-configuration |	通知方式分页 |		GET |	/plugins/alert-monitor/api/v1/notifyWay	 |	是 | 是 | 	                                                                                                         |
| 83 | notify-configuration |	获取所有通知方式 |		GET |	/plugins/alert-monitor/api/v1/notifyWay/list	 |	是 | 是 | 	                                                                                                         |	
| 84 | notify-configuration |	通知方式详细 |		GET |	/plugins/alert-monitor/api/v1/notifyWay/{id} |	是 | 是 | 		                                                                                                        |			
| 85 | notify-configuration |	根据类型获取通知模板 |		GET |	/plugins/alert-monitor/api/v1/notifyTemplate/list	 |	是 | 是 | 	                                                                                                         |	
| 86 | notify-configuration |	新增/修改通知方式 |		POST |	/plugins/alert-monitor/api/v1/notifyWay	 |	是 | 是 | 	                                                                                                         |			
| 87 | notify-configuration |	删除通知方式 |		DELETE |	/plugins/alert-monitor/api/v1/notifyWay/{id}	 |	是 | 是 | 	                                                                                                         |			
| 88 | notify-configuration |	获取通知渠道 |		GET |	/plugins/alert-monitor/api/v1/notifyConfig/list	 |	是 | 是 | 		                                                                                                        |
| 89 | notify-configuration |	更新通知渠道 |		POST |	/plugins/alert-monitor/api/v1/notifyConfig/list	 |	是 | 是 | 		                                                                                                        |		
| 90 | notify-configuration |	测试通知渠道 |		POST |	/plugins/alert-monitor/api/v1/notifyConfig/testConfig |	是 | 是 | 	                                                                                                         |		
|     |     |     |     |     |     |     |                                                                                                           |


## 4.6   后续测试建议
后续商用还需要进行UI自动化、接口测试、稳定性测试、兼容性测试、渗透测试、接口性能自动化、根据实际场景进行历史数据诊断测试。


# 5     附件
无
