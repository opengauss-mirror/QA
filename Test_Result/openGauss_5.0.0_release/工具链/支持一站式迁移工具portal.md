

![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者     |
| ---------- | ----------- | -------------------- | -------- |
| 2023-03-23 | 1.0         | 特性测试报告初稿完成 | lihongji |

关键词： 全量迁移、增量迁移、反向迁移、数据校验

摘要：portal工具集成现有全量迁移、增量迁移、反向迁移、数据校验工具，实现一键安装、启动、卸载功能，。portal工具可通过默认计划运行迁移校验流程，如默认计划plan3实现全量迁移->全量校验->增量迁移->增量校验->反向迁移。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

 portal可以根据需要下载并安装迁移工具，制定迁移工具操作计划并启停对应迁移工具，通过操作portal对M*全量迁移工具、反向迁移工具、数据校验工具进行管理

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 build 5a69c469 | 2023-03-04   | 2023-03-17   |
| portal master                  | 2023-03-04   | 2023-03-17   |
| MySQL 5.7                      | 2023-03-04   | 2023-03-17   |
| jdk 11.0.16                    | 2023-03-04   | 2023-03-17   |

描述特性测试的硬件环境信息

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br>内存：31GB<br>硬盘：523G<br>OS：CentOS Linux release 7.6.1810 (Core)<br> |      |

# 3     测试结论概述

## 3.1   测试整体结论

支持一站式迁移工具portal共计执行39条用例，主要覆盖了功能测试、可靠性测试、资料测试。功能测试验证portal安装卸载、启停、默认计划；可靠性测试注入磁盘、cpu故障进行工具可靠性测试，成功启动默认计划，并进行数据校验与迁移。资料测试中描述恰当，示例正确无误。共计发现21个问题，其中21个问题已验收。问题均回归通过，无遗留风险，整体质量一般。

| 测试活动   | 活动评价                                       |
| ---------- | ---------------------------------------------- |
| 功能测试   | 验证通过portal安装卸载、启停迁移校验工具，通过 |
| 可靠性测试 | 注入故障，使用工具进行数据校验测试，通过       |
| 资料测试   | 使用方法描述准确，示例正确无误，通过           |
## 3.2   约束说明

1. portal在执行增量迁移、反向迁移、增量校验时需要使用curl工具 
2. 增量校验前需先启动增量迁移
3. 增量迁移和反向迁移不能同时开启 
4. portal使用的workspace.id只能为小写字母与数字的组合 
5. portal在启动多个计划时，需要保证mysql数据库实例各不相同，openGauss端数据库各不相同 
6. MySQL使用5.7版本，openGauss为兼容B库；

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | :------: | :--: | :--: | :--: | :----: |
| 数目   |    21    |  0   |  8   |  13  |   0    |
| 百分比 |   100%   |  0%  | 38%  | 62%  |   0%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I6ERMV](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6ERMV?from=project-issue) | 次要 | gitee资料中迁移命令描述不清晰，官网中未上传资料              | 已验收 |
| 2    | [I6ERYR](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6ERYR?from=project-issue) | 主要 | 增量校验和反向迁移方案未打通                                 | 已验收 |
| 3    | [I6ES1S](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6ES1S?from=project-issue) | 主要 | 使用workspace进行迁移任务，不支持多个迁移任务并行            | 已验收 |
| 4    | [I6F4FP](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6F4FP?from=project-issue) | 主要 | 增量迁移和反向迁移的包在同一个包中，应该分包                 | 已验收 |
| 5    | [I6FQC2](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6FQC2?from=project-issue) | 主要 | 日志未显示错误来源，全量迁移报错会导致卡死                   | 已验收 |
| 6    | [I6H2H9](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6H2H9?from=project-issue) | 次要 | 需求描述支持一站式迁移portal，对接debezium+kafka，集成全量迁移、增量迁移。目前全量迁移使用工具是chameleon全量迁移工具，应使用debezium全量迁移工具 | 已验收 |
| 7    | [I6H5A1](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6H5A1?from=project-issue) | 次要 | 安装、卸载、全量迁移与校验完成后没有自动终止进程，释放资源   | 已验收 |
| 8    | [I6HLT9](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6HLT9?from=project-issue) | 主要 | 使用portal启动全量校验工具，启动失败                         | 已验收 |
| 9    | [I6HYOP](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6HYOP?from=project-issue) | 主要 | config文件夹没有同步更新，执行安装工具命令失败               | 已验收 |
| 10   | [I6IT7T](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6IT7T?from=project-issue) | 主要 | portal安装启动变色龙工具，drop_replica_schema命令多了--progress参数，导致启动全量迁移失败 | 已验收 |
| 11   | [I6J1RK](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6J1RK?from=project-issue) | 次要 | portal启动mysql增量迁移，kafka启动失败的情况下，程序未退出，提示Incremental migration is running增量迁移正在运行，与实际状态不符合 | 已验收 |
| 12   | [I6J22R](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6J22R?from=project-issue) | 次要 | portal的配置文件对confluent、kafka的包名称版本有限制，配置其他版本的包名会或zip格式的压缩包提示错误 | 已验收 |
| 13   | [I6J5CY](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6J5CY?from=project-issue) | 主要 | portal启动MySQL增量迁移服务，config里面的公共配置的数据库配置项加载不到，需通过命令指定参数才可以。显屏日志出现明文密码 | 已验收 |
| 14   | [I6JKS7](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6JKS7?from=project-issue) | 次要 | portal自定义schema-registry.properties端口号后，启动MySQL增量迁移服务报错 | 已验收 |
| 15   | [I6JMZ1](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6JMZ1?from=project-issue) | 次要 | portal启动MySQL反向迁移工具，没执行所依赖的curl指令，无crul.log日志 | 已验收 |
| 16   | [I6JNAQ](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6JNAQ?from=project-issue) | 次要 | portal代码仓config/debezium配置文件mysql-source.properties、opengauss-sink.properties分别少了bigint.unsigned.handing.mode、max_thread_count参数 | 已验收 |
| 17   | [I6L191](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6L191?from=project-issue) | 次要 | portal工具在校验工具、反向迁移、增量迁移未启动的情况下，全量迁移完成后停止了校验工具、反向迁移、增量迁移任务，日志打印 IO exception | 已验收 |
| 18   | [I6LHVO](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6LHVO?from=project-issue) | 次要 | portal启动datachecker校验工具后无法完成校验，连接失败        | 已验收 |
| 19   | [I6LI2K](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6LI2K?from=project-issue) | 次要 | portal启动datachecker校验工具后校验失败无法自行退出且手动停止后夯住 | 已验收 |
| 20   | [I6MA2X](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6MA2X?from=project-issue) | 次要 | 启动增量校验任务后异常，无法通过stop_plan停止计划            | 已验收 |
| 21   | [I6MHMQ](https://gitee.com/opengauss/openGauss-migration-portal/issues/I6MHMQ?from=project-issue) | 次要 | portal中show_status显示的迁移进度不准确                      | 已验收 |

# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                    |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| 1. portal卸载安装迁移与校验工具<br />2. portal启停迁移与校验工具<br />3.portal执行自定义计划与默认计划<br/>4.portal显示迁移进度 | 共执行39条用例，<br />共发现20个bug，20个现已修复且验收通过 |

### 4.1.2 可靠性测试

| 测试步骤：               | 测试结果   |
| ------------------------ | ---------- |
| 1. cpu、内存不足故障<br> | 未发现缺陷 |

### 4.1.3 资料测试

| 测试步骤：                            | 测试结果                            |
| ------------------------------------- | ----------------------------------- |
| 1. 资料一致性验证<br>2.资料示例正确性 | 共发现1个bug，1个现已修复且验收通过 |



## 4.2   测试执行统计数据

|            版本名称            | 测试用例数 |     用例执行结果      | 发现问题单数 |
| :----------------------------: | :--------: | :-------------------: | :----------: |
| openGauss 5.0.0 build 5a69c469 |     39     | Passed：19 Failed：20 |      20      |
| openGauss 5.0.0 build 3cf655f3 |     20     | Passed：20 Failed：0  |      0       |

数据说明

1. 1个issue为资料单，不统计为portal工具缺陷密度
2. 缺陷密度为20(缺陷个数)/4.494k(代码行数)=4.45(个/kloc)

## 4.3   后续测试建议

无

# 5  附件

无