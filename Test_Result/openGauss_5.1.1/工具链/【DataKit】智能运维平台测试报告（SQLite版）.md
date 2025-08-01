![avatar](../../../images/openGauss.png)

版权所有 © 2022 openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA
4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (
但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。
修订记录

| 日期         | 修订   版本 | 修改描述        | 作者           |
|------------|---------|-------------|--------------|
| 2024.02.05 | V1.0    | 新建          | Feiling Peng |


关键词：

摘要：

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |

# 1     特性概述

版本包括的特性有：SQL诊断、慢SQL监控、日志检索、实例监控、告警通知、历史数据诊断分析。

1.通过监控硬件、OS和数据库指标以及TOP SQL相关信息 ，为用户观测数据库提供首要入口。

2.支持监控慢SQL和创建诊断任务 ，通过对应用、数据库、OS、硬件的全栈跟踪，实现基于专家模型的故障诊断。

3.支持数据库、OS日志在Web端的统一检索。

<br>

1230版本新增/修改的功能有：

（1）实例监控：增加新监控指标、存储时长配置、采集间隔配置、页面优化、部署架构优化支持外部部署Agent

（2）智能诊断：历史、SQL诊断增加新诊断点、支持不安装Agent能正常运行

（3）日志检索：支持lucene语法搜索、CM日志采集

（4）告警监控：增加内置告警规则、增加告警收敛、页面优化

（5）安装部署：各组件安装路径支持修改

（6）集群监控：增加集群统计、切换记录、拓扑图显示

（7）数据库切换：实例监控、SQL诊断


# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称  | 测试起始时间     | 测试结束时间     |
|-------|------------|------------|
| 第一轮测试 | 2023.11.24 | 2023.12.25 |
| 第二轮测试 | 2023.12.26 | 2024.01.24 |
| 第三轮测试 | 2024.01.29 | 2024.02.05 |

第一轮测试环境：

| 环境信息      | 配置信息                                                                                                                              | 备注 |
| ------------- |-----------------------------------------------------------------------------------------------------------------------------------| ---- |
| 虚拟机 | Intel Xeon Processor (Cascadelake) CPU @ 2.00GHz 8核<br/>内存：16GB<br/>硬盘：120G<br/>OS：CentOS Linux 7.6 <br>                          |
|软件| Java：openjdk 11.0.17 2022-10-18<br>python: 2.7.18<br>ElasticSearch: 8.3.3<br>Filebeat: 8.3.3<br>prometheus-2.42.0.linux-amd64<br> |      |
|数据库| openGauss 轻量版3.0.0<br>openGauss 企业版3.0.0<br>SQLite 3.32.3                                                                         |

第二轮/第三轮测试环境：

| 环境信息      | 配置信息                                                                                                                             | 备注 |
| ------------- |----------------------------------------------------------------------------------------------------------------------------------| ---- |
| 虚拟机 | Intel Xeon Processor (Cascadelake) CPU @ 2.00GHz 8核<br/>内存：16GB<br/>硬盘：150G<br/>OS：CentOS Linux 7.6 <br>                         |
|软件| Java：openjdk 11.0.17 2022-10-18<br>python: 2.7.5<br>ElasticSearch: 8.3.3<br>Filebeat: 8.3.3<br>prometheus-2.42.0.linux-amd64<br> |      |
|数据库| openGauss 轻量版3.0.0<br>openGauss 企业版3.0.0<br>SQLite 3.7.17                                                                        |




# 3     测试结论概述

## 3.1   测试整体结论

进行了1轮测试，覆盖了SQL诊断、实例监控、历史数据诊断、告警通知、集群监控特性，共设计了843个测试用例，整个测试过程覆盖了功能测试。
总共发现82个问题，4个功能建议，目前遗留了14个问题，整体质量一般。

| 测试活动 | 活动评价                                                                            |
|------|---------------------------------------------------------------------------------|
| 功能测试 | 测试增加新监控指标、存储时长配置、采集间隔配置、页面优化、安装部署优化、智能诊断、日志检索支持lucene语法搜索和CM日志采集、集群监控，测试结果符合预期。 |
| 功能测试 | 测试使用的数据库切换回opengauss/sqlite时，平台功能与插件功能可用、插件安装卸载，测试结果符合预期。                       |
| 接口测试 | 测试 实例监控、智能诊断、集群监控、告警监控目前已有接口，存在一个接口的响应不符合预期                                     |  
| 性能测试 | 测试 接口的性能与资源占用情况。部分插件存在一定的资源消耗。                                                  |

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

（2）集群数据库使用内存监控时需要设置对应参数：max_process_memory=12G, shared_buffers=64M, cstore_buffers=16M,enable_memory_limit=on

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

（4）服务器需配置：`echo vm.max_map_count = 262144 >> /etc/sysctl.conf &&sysctl -p`

（5）SQL诊断使用explain需配置参数：`track_stmt_parameter=on`

（6）慢sql使用时，数据库需配置：
```
track_stmt_stat_level='L1,L1'
enable_stmt_track=on
track_stmt_parameter=on
log_min_duration_statement=10s
```
如果track_stmt_stat_level参数通过guc参数修改不生效，则执行`ALTER DATABASE postgres SET track_stmt_stat_level TO 'L1,L1'`

**自动部署**
（1）需要先在服务器上安装yum命令

（2）安装后如若无数据显示，可做如下操作：卸载Agent、重装Agent、停用插件、启动插件

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号      | 问题描述                    | 问题级别 | 问题影响和规避措施                                                                               | 当前状态   |
|-----------|-------------------------|------|-----------------------------------------------------------------------------------------|--------|
| 710093154 |日志上下文检索_时间、关键字搜索不可用| 重要   | 上下文检索时，不可通过时间、关键字过滤。不影响其他功能使用                                                           | 遗留        |
| 710076986 |【遗留到330】日志检索_查询往年的系统日志问题| 一般   | 目前处理逻辑只能处理到去年日志，处理方式需优化。                                                                | 遗留        |
| 710067025 |【遗留到330】日志检索_分布图显示不正确| 一般   | 查询时间分段时，如果无法整除会溢出，往前加一段时间处理，导致页面上显示的时间超出查询时间。                                           | 遗留        |
| 710059125 |【遗留到330】elastic在不同环境使用同一个路径不同端口安装问题| 一般   | 需要人为控制不要在不同环境用同一个路径，如果误使用，重装即可。                                                         | 遗留        |
| 710059092 |【遗留到330】elastic卸载残留| 一般   | 个别插件卸载时进程未关闭，目录未删除。需要手动处理进程。                                                            | 遗留        |
| 710058526 |【遗留到330】【优化】切换物理机后用户联动处理| 一般   | 切换物理机后，用户未清空，需要人为检查重选。不影响功能使用。                                                          | 遗留        |
| 710054435 |【遗留到330】sql诊断_新建任务按钮被遮挡| 一般   | 展开部署列表窗口时，按钮被遮挡一半。不影响功能使用。                                                              | 已修复、未合入社区         |
| 710038989 |wdr报告存放路径优化| 一般   | wdr报告未单独建立一个目录存放，服务器查看文件目录时不美观                                                          | 遗留        |
| 710030422 |日志检索_lucene语法检查结果错误| 一般   | 范围查询等个别搜索结果与预期不符                                                                        | 遗留        |
| 710029543 |告警_数据库状态告警_未告警| 重要   | 规则and逻辑问题，暂不可使用and                                                                      | 遗留        |
| 710027555 |在线安装elastic_报找不到包| 一般   | 卸载有残留信息记录，安装卸载逻辑在下一个版本优化。                                                               | 遗留        |
| 710020240 |sql诊断_执行create语句错误| 一般   | 执行sql时，会在sql前面带入explain analyze字符用于获取实际的执行计划，会导致create语句执行失败。目前只支持查询语句诊断，后续版本确认相关功能是否需要支持。 | 遗留        |
|710109354|[SQLite]切换记录抛出异常需排查| 一般   | 定时任务读取集群日志然后写入库中，设置的3秒，有时会出现超时，现调整为10s                                                  | 已修复、未合入社区                                                                                  |
|710105605|有节点不可用_节点角色未显示| 一般   | 节点不可用时，不能实时获取到角色信息，返回的空数据。不影响功能使用。                                                      | 已修复、未合入社区                                                                                  |
|710110273|集群监控_request error| 重要   | 外网ip导致                                                                                  | 进行中 |
|710097865|离线安装上传安装包接口无法加载相应数据| 一般   | 上传成功，但是接口未返回信息，不影响功能使用。下一个版本功能变更，将去掉离线上传功能。                                             | 遗留                                                                                         |
|710059213|日志检索filebeat安装流程中点击返回后页面显示异常| 一般   | 页面样式显示问题，文本框未适应宽度导致页面留白多，不影响功能使用。                                                       | 遗留                                                                                         |

### 3.3.2 问题统计

| | 问题总数 | 关键  | 重要  | 一般  | 提示  |
| ------ |------|-----|-----|-----|-----|
| 数目 | 82   | 1   | 42  | 36  | 3   |
| 百分比 | 100% | 1%  | 51% | 44% | 4%  |

# 4     测试执行

## 4.1   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，根据第二章的测试轮次分开进行统计说明。*

| 版本名称  | 测试用例数 | 用例执行结果       | 发现问题单数 |
|-------|-------|--------------|--------|
| 第一轮测试 | 846   | 执行172，通过率80% | 45     |
| 第二轮测试 | 846   | 执行438，通过率94% | 29     |
| 第三轮测试 | 846   | 执行388，通过率96% | 9      |

进行问题回归后，测试用例数共846条，通过率98%。

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*发现问题单数－－本测试活动总共发现的问题单数。*

## 4.2 测试执行步骤

### 4.2.1 实例监控

| 测试步骤                                     | 测试结果                                 |
|------------------------------------------|--------------------------------------|
| 1、 新指标验证、存储时长配置、采集间隔配置、系统配置等页面优化（1230版本） | 执行64条用例，发现13个bug，现已修复且验证通过           |
| 2、 wdr报告、ASP、指标验证                        | 执行85条用例，发现6个bug，遗留1个bug，其他问题已修复且验证通过 |

### 4.2.2 集群监控

| 测试步骤                 | 测试结果                                     |
|----------------------|------------------------------------------|
| 1、 集群统计、切换记录、拓扑图显示（1230版本） | 执行10条用例，发现7个bug，2个bug已修复未合入社区，其他已修复且验证通过 |
| 2、 集群详情、节点详情         | 执行9条用例，发现1个bug，遗留1个偶发的bug。               |

### 4.2.3 安装部署

| 测试步骤                            | 测试结果                                |
|---------------------------------|-------------------------------------|
| 1、 部署架构优化：支持自定义路径、支持一对多（1230版本） | 执行22条用例，发现17个bug，遗留6个bug，其他已修复且验证通过 |

### 4.2.4 智能诊断

| 测试步骤                      | 测试结果                                    |
|---------------------------|-----------------------------------------|
| 1、 新诊断经验 （1230版本）         | 执行6条用例，发现3个bug，遗留1个问题，其他已修复且验证通过 |
| 2、 sql诊断查询/删除、查看原诊断点、慢sql | 执行66条用例，发现9个bug，遗留1个问题，其他已修复且验证通过       |
| 3、 历史数据分析                 | 执行22条用例，发现1个bug，现已修复且验证通过               |


### 4.2.5 日志检索

| 测试步骤                          | 测试结果                               |
|-------------------------------|------------------------------------|
| 1、 支持lucene语法搜索、CM日志采集（1230版本） | 执行12条用例，发现4个bug，遗留1个bug，其他已修复且验证通过 |
| 2、 日志按条件搜索、上下文检索              | 执行53条用例，发现6个bug，遗留3个bug，其他已修复且验证通过 |

### 4.2.6 告警监控

| 测试步骤                                         | 测试结果                       |
|----------------------------------------------|----------------------------|
| 1、 告警规则优化、告警收敛、配置解除、页面优化（1230版本）             | 执行10条用例，发现10个bug，遗留1个bug，现已修复且验证通过 |
| 2、 告警规则/模板的新增/查看/编辑/删除，通知方式的新增/查看/编辑/删除，告警配置 | 执行109条用例，发现5个bug，现已修复且验证通过 |

### 4.3 测试执行数据

**1230版本测试执行数据**

| 功能   | 测试用例数 | 用例最终执行结果     | 发现问题单数 |点灯| 备注                                                       |
|------|-------|--------------|--------|---|----------------------------------------------------------|
| 实例监控 | 198  | 执行149，通过率99% | 16     |绿||
| 告警通知 | 240 | 执行119，通过率99% | 18     |黄| and规则不可用                                                 |
| 集群监控 | 24 | 执行19，通过率99%  | 6      |黄| 存在一个request error问题待处理  |
| 智能诊断 | 163  | 执行94，通过率100% | 18     |绿|                                                          |
| 安装部署 |  45  | 执行22，通过率99%  | 13     |黄| 插件卸载安装存在冲突时，需手动处理进程。遗留至下一个版本优化。prometheus离线上传接口无法加载响应数据。 |
| 日志检索 | 96    | 执行65，通过率95%  | 10     |绿|                                |

**注：包括旧功能回归用例*

## 4.4 问题分析

**本版本（1230版本）的问题主要集中在告警通知、sql诊断、实例监控、安装部署。主要是设计与实现方面的问题。
如新功能前端处理未刷新，指标实现不正确，诊断点实现逻辑有误，资源未释放，代码误改动有误导致旧功能出现问题。

## 4.5 性能测试

本次性能测试共执行1个性能测试脚本，测试采集配置查询接口的性能。并发用户数设置100，每秒增加20个用户，执行120秒。
执行性能测试脚本期间同时执行资源占用采集脚本，以测试该期间的资源占用情况。

接口性能预期RPS在10以上，平均请求响应时间不超过1.5s。
目前SQL诊断不支持并发，执行较慢，执行诊断任务期间会影响SQL诊断与慢SQL功能的使用。

实例监控性能脚本执行中出现了数据库连接问题（socket未关闭），集群节点接口、top进程接口响应较慢。集群监控存在集群节点信息接口request error导致请求均为失败状态，需要解决问题并优化相应接口。

告警监控性能脚本执行中出现了JDBC连接数据库超时、数据库不可用的情况，占比4%，需要优化。

采集配置接口性能脚本执行中出现SQLite数据库死锁，需要优化。

**SQL诊断**

| Method     | Name                                                       |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|------------------------------------------------------------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|GET|/historyDiagnosis/api/v1/pageQuery|89|0|12912|403|34667|592461|0.7|0.0|11000|11000|17000|25000|31000|34000|35000|35000|
|DELETE|/historyDiagnosis/api/v1/tasks/{taskId}|67|0|21881|13512|34058|28|0.6|0.0|21000|23000|25000|27000|31000|32000|34000|34000|
|POST|/historyDiagnosis/api/v2/tasks|107|0|32373|628|55263|40|0.9|0.0|37000|41000|45000|47000|48000|49000|51000|55000|
|GET|/historyDiagnosis/api/v2/tasks/{taskId}/points/{pointName}|87|0|40195|5588|50534|52|0.7|0.0|42000|44000|45000|47000|48000|49000|51000|51000|
|GET|/sqlDiagnosis/api/v1/clusters|126|0|74|18|293|2457|1.1|0.0|31|41|100|140|190|220|270|290|
| |Aggregated|476|0|20137|18|55263|111448|4.0|0.0|18000|25000|33000|40000|46000|48000|50000|55000|

**安装部署**

| Method     | Name |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|-----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|GET|/plugins/observability-instance/observability/environment/api/v1/exporters|7596|0|553|46|2206|2197|63.2|0.0|510|570|650|740|890|1100|1400|2200|
|GET|/plugins/observability-instance/observability/v1/environment/prometheus|7842|0|80|7|864|582|65.3|0.0|39|75|94|130|190|260|400|860|
|GET|/plugins/observability-log-search/observability/v1/environment/elasticsearch|7930|0|81|7|979|520|66.0|0.0|43|75|95|130|200|260|410|980|
|GET|/plugins/observability-log-search/observability/v1/environment/filebeat|7726|0|409|34|1688|748|64.3|0.0|370|420|480|560|680|790|1100|1700|
|GET|/plugins/observability-sql-diagnosis/observability/v1/environment/agent|7617|0|408|32|1777|753|63.4|0.0|370|420|480|560|690|810|1100|1800|
| |Aggregated|38711|0|303|7|2206|952|322.1|0.0|260|330|410|510|660|800|1100|2200|

**日志检索**

| Method     | Name |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|-----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|GET|/logSearch/api/v1/logDistributionMap|1546|0|2334|388|5060|1616|13.0|0.0|2300|2400|2600|2800|3000|3200|3700|5100|
|GET|/logSearch/api/v1/logs|3019|0|2623|575|5554|11501|25.3|0.0|2600|2700|2900|3100|3400|3600|4100|5600|
|GET|/sqlDiagnosis/api/v1/slowSqls|1599|0|35|3|825|60|13.4|0.0|8|9|11|19|160|190|330|830|
| |Aggregated|6164|0|1879|3|5554|6054|51.6|0.0|2200|2400|2600|2900|3200|3400|3900|5600|

**实例监控与集群监控**

| Method     | Name |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|-----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|GET|/api/v1/asp/analysis|125|5|8262|2068|13171|131|1.3|0.1|8700|9400|10000|10000|11000|12000|13000|13000|
|GET|/api/v1/asp/count|97|0|5258|1250|9302|71|1.0|0.0|5400|5800|6600|7100|8000|8400|9300|9300|
|GET|/api/v1/clusters/allClusterState|96|0|288|14|1926|221|1.0|0.0|19|22|34|570|1500|1800|1900|1900|
|GET|/api/v1/clusters/list|213|0|24|14|160|202|2.3|0.0|19|19|21|23|32|72|120|160|
|GET|/api/v1/cpu|129|0|47|23|231|6623|1.4|0.0|37|42|45|50|86|120|230|230|
|GET|/api/v1/io|100|0|62|34|273|2261|1.1|0.0|52|56|63|75|100|140|270|270|
|GET|/api/v1/mainMetrics|103|0|1211|45|4820|1224|1.1|0.0|680|1100|1500|2300|3300|4100|4800|4800|
|GET|/api/v1/memory|111|0|2691|48|7972|4510|1.2|0.0|2700|3100|4000|5400|6000|7200|7900|8000|
|GET|/api/v1/network|122|0|58|36|152|1011|1.3|0.0|51|56|60|69|83|110|140|150|
|GET|/api/v1/topSQLNow|98|0|6394|75|18248|22070|1.1|0.0|4300|7600|8900|14000|16000|18000|18000|18000|
|GET|/clusters/{clusterId}/nodes|107|107|2982|36|8717|23|1.2|1.2|2600|3900|4900|5600|6100|7400|7800|8700|
|GET|/observability/v1/param/paramInfo|94|0|15|6|137|39834|1.0|0.0|11|12|13|15|18|47|140|140|
|GET|/plugins/observability-instance/instanceMonitoring/api/v1/clusters/nodes|104|0|1407|27|4879|827|1.1|0.0|990|1600|1800|2900|3700|4000|4800|4900|
|GET|/plugins/observability-instance/instanceMonitoring/api/v1/topOSProcessAndDBThread|1|0|88180|88180|88180|3604|0.0|0.0|88000|88000|88000|88000|88000|88000|88000|88000|
|GET|/plugins/observability-instance/wdr/findSnapshot|124|0|2193|13|7940|72|1.3|0.0|890|1500|3100|5200|5900|7400|7800|7900|
|GET|/plugins/observability-instance/wdr/list|84|0|18|7|141|142|0.9|0.0|12|12|13|16|25|60|140|140|
| |Aggregated|1708|112|2034|6|88180|4647|18.4|1.2|80|400|1800|4200|7200|9100|14000|88000|

**告警监控**

| Method     | Name                                               |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|----------------------------------------------------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| GET        | /plugins/alert-monitor/api/v1/alertClusterNodeConf |93|0|9806|46|30117|633|0.8|0.0|5600|9200|12000|22000|26000|29000|30000|30000|
| GET        | /plugins/alert-monitor/api/v1/alertRule            |95|5|18625|111|57109|67172|0.8|0.0|15000|19000|25000|39000|47000|49000|57000|57000|
| GET        | /plugins/alert-monitor/api/v1/alertRule/ruleList   |127|5|20457|27|52633|68089|1.1|0.0|18000|22000|30000|42000|48000|50000|52000|53000|
| DELETE     | /plugins/alert-monitor/api/v1/alertTemplate        |22|3|30902|12507|44766|52|0.2|0.0|30000|34000|38000|40000|43000|44000|45000|45000|
| GET        | /plugins/alert-monitor/api/v1/alertTemplate        |105|4|10271|16|30105|15285|0.9|0.0|8000|11000|16000|24000|25000|27000|30000|30000|
| POST       | /plugins/alert-monitor/api/v1/alertTemplate        |103|8|20582|1370|46560|193|0.9|0.1|22000|26000|30000|31000|35000|36000|45000|47000|
| | Aggregated                                         |545|25|16803|16|57109|30667|4.7|0.2|14000|19000|25000|30000|39000|47000|52000|57000|

**采集配置**

| Method     | Name                                               |# Requests|# Fails|Average (ms)|Min (ms)|Max (ms)|Average size (bytes)|RPS|Failures/s|50%ile (ms)|60%ile (ms)|70%ile (ms)|80%ile (ms)|90%ile (ms)|95%ile (ms)|99%ile (ms)|100%ile (ms)|
|------------|----------------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|POST|/collectConfig/api/v1/templates/action|2288|2211|1764|9|19589|711|19.2|18.6|710|1300|1900|2900|4500|7300|15000|20000|
|GET|/collectConfig/api/v1/templates/getDetailByNodeId/{nodeId}|2220|0|3308|38|22218|6799|18.6|0.0|2800|3300|4100|4900|6600|8000|15000|22000|
| |Aggregated|4508|2211|2525|9|22218|3709|37.8|18.6|1600|2400|3200|4200|5800|7700|15000|22000|

**资源占用情况**

在执行性能脚本120s时，进行120次的采样。对于主平台进程，内存占用会增加0.3~0.5G左右。
日志检索filebeat内存占用较高。

|                | 主平台进程                                                      |
|----------------|------------------------------------------------------------|
| 空闲时            | 内存占用2.6~2.8G，%CPU为0.7~0.1                                  |
| 实例监控与集群性能脚本运行时 | 内存占用3.2G，%CPU为1~9.7                                        |
| 日志检索性能脚本运行时    | 内存占用3.5G，%CPU为1~95（大部分在4以下，约1%的样本达到95）                     |
| 安装部署性能脚本运行时    | 内存占用3.1~3.3G，%CPU为0.7~0.1                                  |
| SQL诊断性能脚本运行时   | 内存占用2.9~3.2G，%CPU为1~96（大部分在50以下，约2%的样本达到70%，约2%的样本达到90以上）  |
| 采集配置性能脚本运行时    | 内存占用3.2~3.3G ，%CPU为1~96（大部分在50以下，约2%的样本达到70%，约1%的样本达到90以上） |
| 告警监控性能脚本运行时    | 内存占用3.2G ，%CPU为1~2.9                                       |


|插件| 	安装包大小 | 	内存占用      | 	cpu占用   | 	DataKit插件功能   | 	端口占用是否检测 |说明|
|---|-------|------------|----------|----------------|-----------|---|
|filebeat| 	48M	| 118~926M   | 	0.7~1%	 | 日志检索           | 	无端口设置    ||
|elastic| 	515M | 1.7G~1.9G	 | 0.3~1%		 | 日志检索   | 	是        |占用较高，建议不要与数据库安装在同一个服务器|
|promethues| 	87M  	| 48M~89M	   | 	0.3%	   | 实例监控/告警监控/集群监控 | 	是        |
|instance-exporter| 	25M	| 	689M	     | 	 7.3%   | 实例监控/告警监控/集群监控 | 	是        |
|agent| 4K	   | 	374M	     | 0.3%     | sql诊断  | 	是        |  |


## 4.6 接口测试

目前接口为内部接口，不对外开放。安装部署功能中，离线上传安装包的接口无法加载响应数据。

| 接口数 | 用例数 | 接口通过率 |
|-----|-----|-------|
| 125 | 229 | 99%   |

## 4.7   后续测试建议

后续商用还需要进行UI自动化、接口测试、稳定性测试、兼容性测试、渗透测试、接口性能自动化、根据实际场景进行历史数据诊断测试，测试多服务器兼容性。

# 5     附件

PR链接:

告警监控插件1230特性代码
https://gitee.com/opengauss/openGauss-workbench/pulls/507

智能运维实例监控插件1230特性代码合入 
https://gitee.com/opengauss/openGauss-workbench/pulls/485

智能运维日志检索插件1230特性代码合入
https://gitee.com/opengauss/openGauss-workbench/pulls/496

智能运维智能诊断插件1230特性代码合入
https://gitee.com/opengauss/openGauss-workbench/pulls/499

智能运维智能诊断插件修改版本号
https://gitee.com/opengauss/openGauss-workbench/pulls/521

智能运维日志检索插件1230特性bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/526

1230告警监控bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/524

实例监控1230bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/533

DataKit智能运维插件使用IntarkDB替代SQLite
https://gitee.com/opengauss/openGauss-workbench/pulls/538

实例监控插件，增加SSH连接池，解决线程堆积问题
https://gitee.com/opengauss/openGauss-workbench/pulls/537

日志检索插件更新操作手册、修复缺陷
https://gitee.com/opengauss/openGauss-workbench/pulls/547

【国创】修复使用Intarkdb时的平台bug
https://gitee.com/opengauss/openGauss-workbench/pulls/551

告警监控1230特性bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/557

实例监控1230特性bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/556

【国创】更新Intarkdb依赖，支持arch64
https://gitee.com/opengauss/openGauss-workbench/pulls/561

日志检索1230特性BUG修复
https://gitee.com/opengauss/openGauss-workbench/pulls/560

【国创】数据迁移插件适配Intarkdb 
https://gitee.com/opengauss/openGauss-workbench/pulls/559

告警监控1230bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/562

实例监控1230bug修复——添加集群同步状态
https://gitee.com/opengauss/openGauss-workbench/pulls/563

告警监控1230bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/566

【国创】业务开发、实例监控、sql诊断插件根据平台数据库类型，自动切换自身数据库（OpenGauss--SQLite，Intarkdb--Intarkdb）
https://gitee.com/opengauss/openGauss-workbench/pulls/567

告警监控1230bug修复——数据库为离线或者异常状态下，没有进行告警
https://gitee.com/opengauss/openGauss-workbench/pulls/570

告警监控1230bug修复——修复promQL为 A or B，并且A和B都满足条件的情况下，发送两条相同的告警记录的问题
https://gitee.com/opengauss/openGauss-workbench/pulls/572

【国创】修复SQL诊断bug，实例监控、sql诊断、业务开发插件在初始化时创建数据库父目录
https://gitee.com/opengauss/openGauss-workbench/pulls/569

告警监控1230bug修复
https://gitee.com/opengauss/openGauss-workbench/pulls/576

智能诊断插件1230特性BUG修复
https://gitee.com/opengauss/openGauss-workbench/pulls/574

实例监控1230bug修复——修复实例监控_top10表容量/表索引数据的问题
https://gitee.com/opengauss/openGauss-workbench/pulls/580

告警监控1230bug修复——修复告警规则和通知方式在某种特殊情况下无法删除的问题
https://gitee.com/opengauss/openGauss-workbench/pulls/579

告警监控1230bug修复——删除通知方式报错
https://gitee.com/opengauss/openGauss-workbench/pulls/583


智能诊断插件1230特性BUG修复
https://gitee.com/opengauss/openGauss-workbench/pulls/587

智能运维实例监控插件，采集配置功能性能优化 
https://gitee.com/opengauss/openGauss-workbench/pulls/586

【国创】更新Intarkdb依赖，修复集群监控bug
https://gitee.com/opengauss/openGauss-workbench/pulls/588
