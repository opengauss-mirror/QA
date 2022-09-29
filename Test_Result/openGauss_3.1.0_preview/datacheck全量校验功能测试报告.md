

![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者     |
| ---------- | ----------- | -------------------- | -------- |
| 2022-09-23 | 1.0         | 特性测试报告初稿 | lihongji |

关键词： kafka、datachecker-check-0.0.1.jar、datachecker-extract-0.0.1.jar、源端(MySQL)、宿端(openGauss)、数据校验

摘要：数据校验工具由MySQL（源端），datachecker，kafka，openGauss（宿端）这几部分组成，本次测试针对该工具支持源端与宿端数据一致性校验，本文档主要介绍datachecker全量校验工具支持源端与宿端表数据一致性的测试结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

datachecker校验工具由MySQL(源端)，kafka，datachecker、openGauss(宿端)这几部分组成。datachecker工具先将源端与宿端的数据抽取，抽取完成后在使用校验服务进行两端的数据对比，两端数据对比完成后生成结果文件，完成数据校验。

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build b30defda | 2022-09-01   | 2022-09-11   |
| chameleon 3.0.0                | 2022-09-01   | 2022-09-11   |
| MySQL 5.7                      | 2022-09-01   | 2022-09-11   |
| kafka 2.13-2.8.11              | 2022-09-01   | 2022-09-11   |
| jdk 11.0.16                    | 2022-09-01   | 2022-09-11   |
| datachecker 1.0                | 2022-09-01   | 2022-09-11   |

描述特性测试的硬件环境信息

| 硬件型号       | 硬件配置信息                                                 | 备注 |
| -------------- | ------------------------------------------------------------ | ---- |
| 虚拟机 | CPU:  Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br>内存：31GB<br>硬盘：523G<br>OS：CentOS Linux release 7.6.1810 (Core)<br> |      |

# 3     测试结论概述

## 3.1   测试整体结论

datachecker数据校验工具（全量）共计执行43条用例，主要覆盖了功能测试、可靠性测试、资料测试。功能测试验证数据的校验结果的准确性；可靠性测试注入了网络延时、磁盘满、cpu满故障进行工具稳定性测试。资料测试中描述恰当，示例正确无误。共计发现11个问题，其中7个问题已解决，1个问题转需求，3个待修复。

| 测试活动   | 活动评价                                 |
| ---------- | ---------------------------------------- |
| 功能测试   | 源端、宿端数据类型校验测试，通过         |
| 可靠性测试 | 注入故障，使用工具进行数据校验测试，通过 |
| 资料测试   | 使用方法描述准确，示例正确无误，通过     |
## 3.2   约束说明

1. 仅对表的数据做一致性校验
2. 不对表对象做一致性校验，如主外键约束、表字段类型等
3. 开启zookeeper、kafka、MySQL、openGauss、datachecker工具并保证正常运行；
4. MySQL使用5.7版本，openGauss为兼容B库；

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 | 转需求 |
| ------ | :------: | :--: | :--: | :--: | :----: | :----: |
| 数目   |    11    |  0   |  3   |  7   |   0    |   1    |
| 百分比 |   100%   |  0%  | 27.3%  | 63.7%  |   0%   |  9%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I5PP11](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5PP11?from=project-issue) | 主要<b> | 源、宿端表数量存在差异，校验结果进入轮询状态，无结果文件输出<b> | 已验收<bbb> |
| 2    | [I5PVER](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5PVER?from=project-issue) | 主要 | 源、宿端存在同名表Ａ与ａ，只输出了Ａ表的校验结果文件         | 已完成 |
| 3    | [I5Q5B4](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5Q5B4?from=project-issue) | 主要 | 校验服务进入轮询状态，无法生成第10张表的结果文件             | 已验收 |
| 4    | [I5P71L](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5P71L?from=project-issue) | 次要 | 源、宿端表仅含一条数据时，无法生成校验结果                   | 已验收 |
| 5    | [I5PCAO](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5PCAO?from=project-issue) | 次要 | 修复语句语法replace into，openGauss暂不支持。源、宿端时间类型未到毫秒级，校验工具进行了毫秒级校验，导致不通过 | 已验收 |
| 6    | [I5PNZL](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5PNZL?from=project-issue) | 次要 | char类型字符串两侧数据一致，校验结果为失败                   | 已验收 |
| 7    | [I5PSUV](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5PSUV?from=project-issue) | 次要 | 二进制类型数据，源、宿端数据无差异，校验结果提示失败         | 进行中 |
| 8    | [I5Q3B7](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5Q3B7?from=project-issue) | 次要 | 无法校验表结构一致性                                         | 转需求 |
| 9    | [I5Q3PG](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5Q3PG?from=project-issue) | 次要 | 源端与宿端同名表，数据条数有差异，校验结果提示通过           | 已验收 |
| 10   | [I5RNFM](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5RNFM?from=project-issue) | 次要 | 源端或宿端表缺失后，无缺失表对应的结果文件                   | 已验收 |
| 11   | [I5TQHE](https://gitee.com/opengauss/openGauss-tools-datachecker-performance/issues/I5TQHE?from=project-issue) | 次要 | bit类型校验结果不准确                   | 待  办 |


# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 源端、宿端数据库正常启动<br />2. kafka、datachecker-extract-0.0.1.jar、<br>datachecker-check-0.0.1.jar服务正常启动<br />3.通过全量校验接口发起校验请求：<br>curl -X 'POST' 'http://localhost:9000/start/check?checkMode=FULL' -H 'accept: */*' -d '' | 共执行43条用例，<br />共发现11个bug，7个现已修复且验收通过，1个转需求，2个待修复，1个偶现 |

#### 4.1.1.1 数据测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 验证int、blob、json等16种数据类型<br />2.验证表结构为分区表。数据包含空值、默认值等场景<br/>3.构造源端、宿端两侧表数据异常场景                    <br/>4.构造源端、宿端两侧表个数异常场景<bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb> | 共执行25条用例<br />共发现11个bug，7个已修复且验收通过，1个转需求,1个偶现验收未通过，2个待修复 |

#### 4.1.1.2 配置测试

| 测试步骤：                                                   | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. 源端抽取服务配置文件application-source.yml<br>参数的测试，包含username、password、<br>databaseType等参数<br/>2. 宿端抽取服务配置文件application-sink.yml<br>参数的测试，包含schema、url等参数<br>3. 校验服务配置文件application.yml<br>参数的测试，包含data-depth、<br>black-white-mode等参数 | 共执行14条用例<br />未发现bug<bbbbbbbbbbbbbbbbbbbbbbbbbb> |

#### 4.1.1.3 资料测试

| 测试步骤：                                                   | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 使用方法描述测试                                                                                                                   <br>2. 一键使用脚本测试 | 1.使用方法描述准确，示例正确无误<br/>2.一键使用脚本可正确启动、停止、查看服务。可设置启动时所需堆内存大小范围等 |

### 4.1.2 可靠性测试

| 测试步骤：                                                   | 测试结果                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 1.在datacheck工具所在的虚拟机注入<br>磁盘满、cpu满、网络延时、网络闪断<br>等故障，查看数据校验结果的准确性 | 共执行4条用例<br />未发现bug<bbbbbbbbbbbbbbbbbb> |

## 4.2   测试执行统计数据

|            版本名称            | 测试用例数 |     用例执行结果     | 发现问题单数 |
| :----------------------------: | :--------: | :------------------: | :----------: |
| openGauss 3.1.0 build b30defda |     43     | Passed：43 Failed：3 |      11      |

数据说明

1. 1个issue转为需求
2. 缺陷密度为10(缺陷个数)/6.51k(代码行数)=1.54(个/kloc)

## 4.3   后续测试建议

1. 表结构一致性校验实现后需对外键约束等进行校验

# 5     附件

### 5.1 数据校验工具执行过程示例

```shell

--前置条件：源、宿端存在待校验的表且源、宿端数据库运行正常
--step1:启动源、宿端数据抽取服务与校验服务 expect:服务启动成功

--启动源端抽取服务
java -jar datachecker-extract-0.0.1.jar -Dspring.config.additional-location=./config/application-source.yml --spring.profiles.active=source
--启动宿端抽取服务
java -jar datachecker-extract-0.0.1.jar -Dspring.config.additional-location=./config/application-sink.yml --spring.profiles.active=sink
或者一键启动源端、宿端抽取服务
sh extract-endpoints.sh start

--启动数据校验服务
java -jar datachecker-check-0.0.1.jar -Dspring.config.additional-location=./config/application.yml
或者一键启动校验服务
sh check-endpoint.sh start

--step2:发起全量校验请求 expect:请求成功
curl -X 'POST' 'http://localhost:9000/start/check?checkMode=FULL' -H 'accept: */*' -d ''

--请求成功返回信息
{"code":200,"message":"SUCCESS","data":"the execution time of BNO0FKJU0AO process is 2022-09-24 10:30:22.535","success":true}

--step3:查看校验结果文件 expect:成功
{
        "schema":"schema_dbtest",
        "table":"t_data_checker_0059",
        "topic":"BNO0FKJU0AO_1_t_data_checker_0059_000000000000",
        "partitions":0,
        "result":"success",
        "message":"schema_dbtest.t_data_checker_0059_[0] check success",
        "createTime":"2022-09-24 10:30:28",
        "keyInsertSet":[],
        "keyUpdateSet":[],
        "keyDeleteSet":[],
        "repairInsert":[],
        "repairUpdate":[],
        "repairDelete":[]
}

```