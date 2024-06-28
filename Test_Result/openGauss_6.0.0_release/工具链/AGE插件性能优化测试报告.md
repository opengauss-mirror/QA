![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述           | 作者     |
| :--------- | ----------- | ------------------ | -------- |
| 2024-06-27 | 1.0         | 特性测试报告初稿完 | chenziyang |

 关键词： 

openGauss、Apache-AGE、PostgreSQL

摘要：

本文档主要介绍apache-age插件适配openGauss第二阶段性能优化测试报告

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

   运行图数据库测试套件，评估openGauss上AGE插件的性能，识别待改进点，突破相应算法和关键技术，目标是性能超越基于主流PostgreSQL版本（PG11和PG16）的AGE。

# 2 特性测试信息
   本节描述被测对象的版本信息、环境信息
| 版本信息 | 测试起始时间 | 测试结束时间 |
| :------: | :------: | :------: |
| openGauss 6.0.0 build 229b15f3   | 2024-06-20 | 2024-06-26 |
| (PostgreSQL) 16.3 + age 1.5.0    | 2024-06-20 | 2024-06-26 |

环境信息：
| 硬件型号 | 硬件配置信息 | 备注 |
| ------ | -------- | -------- |
| x86_64+centos | CPU：Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz<br/> 内存：755GB<br/> 硬盘：3.5T<br/> OS: CentOS Linux release 7.9.2009 (Core) | |
| arrch64+openEuler | CPU：Kunpeng-920 128核<br/> 内存：765GB<br/> 硬盘：3T<br/> OS: openEuler release 20.03 (LTS) | |

# 3 测试概述

## 3.1 测试模型

LDBC-SNB图库测试模型。LDBC是一个致力于发展图数据管理的产业联盟组织，它开发了一套标准的benchmarks，用于系统地衡量不同图数据库产品的功能和性能；SNB是基于社交网络场景开发的一组benchmarks，由交互式场景和商业智能场景组成。
官方提供了7个短查询（SQ）和14个长查询（CQ）

## 3.2 性能测试

openGauss + openGauss插件AGE 与 PostgreSQL(16) + AGE(1.5) 各个场景下的性能对比

## 3.3 约束

参数：使用各数据库的默认配置参数
不支持语法：
| 语法 | openGauss | PostgreSQL |
| :------: | :--------: | :--------: |
| CQ7 | 支持 | PG-AGE语法不支持 |
| CQ10 | 支持 | PG-AGE无相关函数 |
| CQ13 | AGE不支持最短路径 | AGE不支持最短路径 |
| CQ14 | AGE不支持最短路径 | AGE不支持最短路径 |

## 3.4 数据集

参考 https://github.com/ldbc/ldbc_snb_datagen_spark 官方提供了数据生成工具，SNB定义了比例因子（Scale Factor，即所谓的SF）用来控制最终生成的数据量大小。本次测试生成数据集为SF0.1、SF1、SF10。
各数据集在openGauss、PostgreSQL中的数据大小为
| 数据集 | 点 | 边 |
| :------: | :--------: | :--------: |
| SF0.1 | 342838 | 1609155 |
| SF1 | 2841619 | 16028263 |
| SF10 | 25986682 | 159810165 |

# 4 测试执行

## 4.1 数据导入测试

测试流程：
数据生成-->导入数据-->创建索引-->Vacuum

- 注：openGuass侧在SF1、SF10数据导入后创建索引时需调大guc参数：max_process_memory = 64G、max_process_memory = 128G

x86机器执行三次，取平均值，结果如下:
| 数据库 | 数据集 | 导入时间(s) | 创建索引时间(s) | Vacuum时间(s) | 整体时间(s) |
| :------ | :------: | ------: | ------: | ------: | ------: |
| PostgreSQL | SF0.1 | 9.729 | 29.531 | 1.443 | 48.796 |
| openGauss | SF0.1 | 5.299 | 32.498 | 1.401 | 47.238 |
| PostgreSQL | SF1 | 130.325 | 248.691 | 1.915 | 382.235 |
| openGauss | SF1 | 89.612 | 273.758 | 73.4 | 438.421 |
| PostgreSQL | SF10 | 1366.373 | 2515.484 | 6.965 | 3890.776 |
| openGauss | SF10 | 991.725 | 7014.878 | 921.264 | 8936.239 |

- openGauss数据库相比于PostgreSQL数据库，在数据导入时速度更快，有25%~45%的性能提升。
- openGauss数据库相比于PostgreSQL数据库，创建索引时有明显的劣化，数据集越大，性能越差，SF0.1和SF1数据集中表现为约慢10%，SF10数据集中表现为劣化1.7倍。
- openGauss数据库相比于PostgreSQL数据库，Vacuum时间存在较差性能，数据集越大，性能越差，SF0.1数据集劣化92%、SF1数据集劣化2.84倍、SF10数据集劣化22.38倍。

arm机器执行三次，取平均值，结果如下：
| 数据库 | 数据集 | 导入时间(s) | 创建索引时间(s) | Vacuum时间(s) | 整体时间(s) |
| :------ | :------: | ------: | ------: | ------: | ------: |
| PostgreSQL | SF0.1 | 2.303 | 16.277 | 0.915 | 20.103 |
| openGauss | SF0.1 | 1.5371 | 21.737 | 1.76 | 25.584 |
| PostgreSQL | SF1 | 24.783 | 160.071 | 2.342 | 187.338 |
| openGauss | SF1 | 18.531 | 250.037 | 8.989 | 277.923 |
| PostgreSQL | SF10 | 251.875 | 1784.624 | 12.933 | 2049.64 |
| openGauss | SF10 | 176.437 | 2652.594 | 302.336 | 3131.861 |

- openGauss数据库相比于PostgreSQL数据库，在数据导入时速度更快，有25%~34%的性能提升。
- openGauss数据库相比于PostgreSQL数据库，创建索引有明显的劣化，约慢33%~56%
- openGauss数据库相比于PostgreSQL数据库，Vacuum时间存在较差性能，数据集越大，性能越差，SF0.1数据集劣化92%、SF1数据集劣化2.84倍、SF10数据集劣化22.38倍。

> 不同机器架构下，openGauss数据库相比于PostgreSQL数据库，导入数据性能有明显的提升；创建索引时CentOS-x86架构下，数据集越大性能劣化越明显；Vacuum劣化不受机器架构影响

> - 优势
>  数据导入方面，数据集预处理成本、数据导入速度方面openGauss+AGE优于postgreSQL+AGE
> - 劣势
>  建立索引的速度，openGauss+AGE劣于postgreSQL+AGE，原因在于建立索引调用函数，postgreSQL函数有可并行的参数

## 4.2 数据查询测试

查询性能测试原理，使用python驱动psycopg连接数据库，执行对应的查询语句，在执行SQL前记录一个时间，执行完成后记录一个时间，计算时间差值。
部分查询，第一次执行时，需要load图数据到内存，因此每个查询语句执行两次，并分别比较，第二次查询测试结果是指，连接不中断，连续执行两次查询，记录第一次查询和第二次查询的结果

x86机器执行三次，取平均值：

<center>第一次查询比较</center>

![第一次查询](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%B8%80%E6%AC%A1%E6%9F%A5%E8%AF%A2.png)


<center>第二次查询比较</center>

![第二次查询](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%BA%8C%E6%AC%A1%E6%9F%A5%E8%AF%A2.png)

- 注：带有斜线的柱状图代表不支持语法

<center>第一次查询语法比较</center>

![第一次查询语法比较](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%B8%80%E6%AC%A1%E6%9F%A5%E8%AF%A2%E8%AF%AD%E5%8F%A5%E6%AF%94%E8%BE%83.png)

- CentOS X86架构下，第一次查询结果分析</br>
> - SF0.1数据集opengauss整体查询时间表现优于pg性能，约60.49%
> - SF1数据集opengauss整体查询时间表现优于pg性能，约83.36%
> - SF10数据集opengauss整体查询时间表现优于pg性能，约90.24%

opengauss+AGE的性能表现较postgreSQL+AGE的性能优秀，随着数据量的增大，差距越明显

由绘制的柱状图可以看出，不同数据集大小下，CQ1、CQ2、CQ3、CQ6、CQ9、CQ11、CQ12、SQ1、SQ2、SQ3、SQ4、SQ6等类型的查询结果在openGauss数据库中的表现优于PostgreSQL数据库
| 数据集\查询类型 | CQ1 | CQ2 | CQ3 | CQ6 | CQ9 | CQ11 | CQ12 | SQ1 | SQ2 | SQ3 | SQ4 | SQ6 |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----:|
|SF0.1|96.64%|79.08%|94.53%|70.10%|63.28%|94.61%|69.30%|28.26%|99.75%|27.52%|44.44%|70.10%|
|SF1|98.88%|95.04%|97.13%|96.49%|94.09%|94.95%|92.17%|30.47%|99.98%|17.74%|46.21%|97.19%|
|SF10|99.47%|94.45%|98.57%|98.32%|97.30%|92.98%|97.39%|26.57%|99.99%|72.81%|31.99%|98.15%|

>CQ4类型查询,SF0.1、SF10数据集中openGauss性能明显优于PostgreSQL性能；SF1数据集中两者性能基本持平</br>
CQ5类型查询类型随着数据集的增大，openGauss数据库性能会逐渐优于PostgreSQL数据库性能</br>
CQ8类型查询随着数据集的增大，openGauss数据库性能会逐渐劣化，SF0.1数据集openGauss性能优于PostgreSQL，约30%；SF1数据集openGauss性能劣化7.5倍；SF10数据集openGauss性能劣化2.8倍</br>
SQ5类型查询随着数据集的增大，openGauss数据库性能会逐渐劣化，SF0.1数据集openGauss性能优于PostgreSQL，约30%；SF1数据集openGauss和PostgreSQL相比基本持平；SF10数据集openGauss性能劣化4.2倍</br>
SQ7类型查询随着数据集增大，openGauss数据库的表现越差，不同数据集中的表现为SF0.1下openGauss和PostgreSQL相比基本持平；SF1数据集openGauss性能劣化2倍；SF10数据集openGauss性能劣化3.1倍</br>


<center>第二次查询语法比较</center>

![第一次查询语法比较](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%BA%8C%E6%AC%A1%E6%9F%A5%E8%AF%A2%E8%AF%AD%E5%8F%A5%E6%AF%94%E8%BE%83.png)


- CentOS X86架构下，第二次查询结果分析</br>
> - SF0.1数据集opengauss整体查询时间表现优于pg性能，约1.34%
> - SF1数据集opengauss整体查询时间表现优于pg性能，约48.71%
> - SF10数据集opengauss整体查询时间表现优于pg性能，约75.61%

随着数据量的增大，opengauss+AGE的性能优势越明显

由绘制的柱状图可以看出，不同数据集大小下，CQ1、CQ2、CQ3、CQ6、CQ9、CQ11、CQ12、SQ2、SQ6等类型的查询结果在openGauss数据库中的表现优于PostgreSQL数据库
| 数据集\查询类型 | CQ1 | CQ2 | CQ3 | CQ6 | CQ9 | CQ11 | CQ12 | SQ2 | SQ6 |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | 
|SF0.1|33.62%|80.23%|72.47%|2.87%|9.16%|74.52%|43.96%|99.62%|96.47%|
|SF1|66.81%|94.88%|81.09%|83.20%|73.23%|79.49%|36.02%|99.98%|66.54%|
|SF10|73.77%|94.70%|94.55%|93.80%|90.31%|80.24%|91.45%|99.99%|71.23%|

>CQ4类型查询SF0.1、SF10数据集中openGauss性能明显优于PostgreSQL性能；SF1性能openGauss性能略差约8%</br>
CQ5类型查询随着数据集的增大，openGauss的性能越来越好；SF0.1数据集openGauss性能劣化1.2倍，SF1数据集openGauss性能劣化26%，SF10数据集openGauss性能略优约8%</br>
CQ8类型查询SF0.1数据集openGauss性能优于PostgreSQL性能，约23%；SF1数据集openGauss性能劣化约10倍；SF10数据集openGauss性能劣化约2倍</br>
SQ1类型查询SF0.1数据集openGauss性能劣化约79%；SF1数据集openGauss性能劣化约1.5倍；SF10数据集openGauss性能劣化约50%</br>
SQ3类型查询SF0.1数据集openGauss性能劣化约67%；SF1数据集openGauss性能劣化约87%；SF10数据集openGauss性能优于PostgreSQL性能约78%</br>
SQ4类型查询SF0.1数据集openGauss性能劣化约82%；SF1数据集openGauss性能劣化约64%；SF10数据集openGauss性能劣化约81%</br>
SQ5类型查询SF0.1数据集openGauss性能劣化约74%；SF1数据集openGauss性能劣化约4%；SF10数据集openGauss性能劣化约7.5倍</br>
SQ7类型查询SF0.1数据集openGauss性能劣化约38%；SF1数据集openGauss性能劣化约1.7倍；SF10数据集openGauss性能劣化约3倍</br>


arm机器执行三次，取平均值：

<center>第一次查询比较</center>

![第一次查询](images/AGE_arm%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%B8%80%E6%AC%A1%E6%9F%A5%E8%AF%A2.png)

<center>第二次查询比较</center>

![第二次查询](images/AGE_arm%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%BA%8C%E6%AC%A1%E6%9F%A5%E8%AF%A2.png)

<center>第一次查询语法比较</center>

![第一次查询语法比较](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%B8%80%E6%AC%A1%E6%9F%A5%E8%AF%A2%E8%AF%AD%E5%8F%A5%E6%AF%94%E8%BE%83.png)

- openEuler aarch64架构下，第一次查询结果分析</br>
> - SF0.1数据集opengauss整体查询时间表现优于pg性能，约56.54%
> - SF1数据集opengauss整体查询时间表现优于pg性能，约82.86%
> - SF10数据集opengauss整体查询时间表现优于pg性能，约89.74%

opengauss+AGE的性能表现较postgreSQL+AGE的性能优秀，随着数据量的增大，差距越明显

由绘制的柱状图可以看出，不同数据集大小下，CQ1、CQ2、CQ3、CQ6、CQ9、CQ11、CQ12、SQ1、SQ2、SQ3、SQ4、SQ6等类型的查询结果在openGauss数据库中的表现优于PostgreSQL数据库
opengauss+AGE与postgreSQL+AGE性能比较

| 数据集\查询类型 | CQ1 | CQ2 | CQ3 | CQ6 | CQ9 | CQ11 | CQ12 | SQ1 | SQ2 | SQ3 | SQ4 | SQ6 |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| SF0.1 | 96.78% | 76.68% | 91.86% | 65.28% | 60.62% | 93.98% | 68.65% | 12.28% | 99.74% | 12.98%|17.51% | 65.28%|
| SF1 | 99.19% | 93.98% | 96.25% | 95.77% | 93.74% | 92.60% | 90.90% | 34.26% | 99.98% |2.24%|13.98%| 95.77%|
| SF10 | 99.44% | 91.64% | 98.20% | 98.17% | 97.13% | 89.34% | 96.68% | 16.86% | 99.99% | 73.78% | 2.11% | 98.17% |


>CQ4类型查询,SF0.1、SF10数据集中openGauss性能明显优于PostgreSQL性能；SF1数据集中两者性能基本持平</br>
CQ5类型查询类型随着数据集的增大，openGauss数据库性能会逐渐优于PostgreSQL数据库性能</br>
CQ8类型查询随着数据集的增大，openGauss数据库性能会逐渐劣化，SF0.1数据集openGauss性能优于PostgreSQL，约21%；SF1数据集openGauss性能劣化约17倍；SF10数据集openGauss性能劣化约9.7倍</br>
SQ5类型查询随着数据集的增大，openGauss数据库性能会逐渐劣化，SF0.1数据集openGauss性能优于PostgreSQL，约15%；SF1数据集openGauss和PostgreSQL相比基本持平；SF10数据集openGauss性能劣化9.8倍</br>
SQ7类型查询随着数据集增大，openGauss数据库的表现越差，不同数据集中的表现为SF0.1下openGauss和PostgreSQL相比基本持平；SF1数据集openGauss性能劣化约2.3倍；SF10数据集openGauss性能劣化约3.2倍</br>

<center>第二次查询语法比较</center>

![第一次查询语法比较](images/AGE_x86%E6%9E%B6%E6%9E%84%E7%AC%AC%E4%BA%8C%E6%AC%A1%E6%9F%A5%E8%AF%A2%E8%AF%AD%E5%8F%A5%E6%AF%94%E8%BE%83.png)

- openEuler aarch64架构下，第二次查询结果分析</br>
> - SF0.1数据集opengauss整体查询时间表现优于pg性能，约5.3%
> - SF1数据集opengauss整体查询时间表现优于pg性能，约99.96%
> - SF10数据集opengauss整体查询时间表现优于pg性能，约77.39%

 opengauss+AGE的性能表现较postgreSQL+AGE的性能优秀，随着数据量的增大，差距越明显

由绘制的柱状图可以看出，不同数据集大小下，CQ1、CQ2、CQ3、CQ6、CQ9、CQ11、CQ12、SQ2、SQ6等类型的查询结果在openGauss数据库中的表现优于PostgreSQL数据库 (og/pg)

| 数据集\查询类型 | CQ1 | CQ2 | CQ3 | CQ6 | CQ9 | CQ11 | CQ12 | SQ2 | SQ6 |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| SF0.1 | 48.33% | 76.34% | 68.22% | 2.49% | 11.49% | 78.87% | 48.3% | 99.52% | 96.44% |
| SF1 | 79.32% | 94.24% | 80.79% | 84.44% | 74.94% | 76.95% | 36.29% | 99.98% | 71.13% |
| SF10 | 76.53% | 92.01% | 94.71% | 94.86% | 92.04% | 77.88% | 90.95% | 99.998% | 61.66% |


>CQ4类型查询SF0.1、SF10数据集中openGauss性能明显优于PostgreSQL性能；SF1性能openGauss性能略差约26%</br>
CQ5类型查询随着数据集的增大，openGauss的性能越来越好；SF0.1数据集openGauss性能劣化1倍，SF1数据集openGauss性能劣化2%，SF10数据集openGauss性能略优约6%</br>
CQ8类型查询SF0.1数据集openGauss性能优于PostgreSQL性能，约11%；SF1数据集openGauss性能劣化约21.5倍；SF10数据集openGauss性能劣化约9.6倍</br>
SQ1类型查询SF0.1数据集openGauss性能劣化约92%；SF1数据集openGauss性能劣化约96%倍；SF10数据集openGauss性能劣化约91%</br>
SQ3类型查询SF0.1数据集openGauss性能劣化约91%；SF1数据集openGauss性能劣化约85%；SF10数据集openGauss性能优于PostgreSQL性能约80%</br>
SQ4类型查询SF0.1数据集openGauss性能劣化约1倍；SF1数据集openGauss性能劣化约70%；SF10数据集openGauss性能劣化约1.2倍</br>
SQ5类型查询SF0.1数据集openGauss性能劣化约1倍；SF1数据集openGauss性能劣化约47%；SF10数据集openGauss性能劣化约13.4倍</br>
SQ7类型查询SF0.1数据集openGauss性能劣化约29%；SF1数据集openGauss性能劣化约1.9倍；SF10数据集openGauss性能劣化约3.1倍</br>

### 4.2.1 性能劣化分析
> - 优势
>4.2章节中可以看出，openGauss+AGE在使用变长路径查询时性能优于postgreSQL+AGE
>查询性能测试中有70.59%的查询，openGauss+AGE查询效率大于postgreSQL+AGE

- 执行计划列举
![查询比较](images/%E5%A4%8D%E6%9D%82%E6%9F%A5%E8%AF%A25%E5%88%86%E6%9E%90.png)
执行计划完全相同时，openGauss+AGE可能会劣于postgreSQL+AGE，主要原因应该还是并行参数，基础算子执行效率差异
![DDL](images/pg%E7%9A%84DDL%E8%AF%AD%E5%8F%A5.png)
AGE创建函数时在PG侧多了个参数，openGauss不支持

- 其他佐证
![火焰图](images/%E6%80%A7%E8%83%BD%E7%81%AB%E7%84%B0%E5%9B%BE.png)
从图中可以看出AGE函数都在openGauss内核函数ExecScan和ExecMakeTableFunctionResult之内，底层的扫描过程使用的还是openGauss的存储引擎，性能瓶颈在存储层。