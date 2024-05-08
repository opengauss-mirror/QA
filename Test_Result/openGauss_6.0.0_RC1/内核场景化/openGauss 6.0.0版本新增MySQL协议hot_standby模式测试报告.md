![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期      | 修订版本 | 修改描述                                                     | 作者    |
| --------- | -------- | ------------------------------------------------------------ | ------- |
| 2024.2.22 | 1.0      | openGauss 6.0.0版本新增MySQL协议hot_standby模式测试报告初稿完成 | liutong |

 关键词：MySQL协议hot_standby模式、dolphin_hot_standby、hot_standby、dolphin_server_port



摘要：

 本文对openGauss 6.0.0版本新增MySQL协议hot_standby模式测试情况进行详细说明，并给出最终测试结论。



缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |

# 1     特性概述

新增dolphin_hot_standby参数，该参数可以控制MySQL JDBC以及MySQL客户端是否可以通过MySQL协议端口连接备机。

使用该参数之前需要搭建openGauss主备数据库，创建B类型数据库，创建用户并设置MySQL native密码、开启MySQL协议兼容（设置enable_dolphin_proto=on、设置MySQL端口号dolphin_server_port，要和openGauss本身监听的端口号不同）。

本报告中将用MySQL端口指代dolphin_server_port参数设置的端口，openGauss端口指代openGauss数据库本身的端口（即port参数设置的端口）。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称             | 测试起始时间 | 测试结束时间 |
| -------------------- | ------------ | ------------ |
| openGauss 6.0.0 RC05 | 2024-2-17    | 2024-2-20    |

硬件环境信息：

| 硬件型号                                                     | 硬件配置信息                                                 | 备注 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280 7260)          | CPU：Kunpeng-920 2p 128核<br />内存：768G<br />硬盘：NVME 3.7T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| 加压端：<br />ARM+openEuler 2P<br />TaiShan 200 (Model 2280 7260) | CPU：Kunpeng-920 2p 128核<br />内存：256G<br />OS：openEuler release 20.03 (LTS)<br />网卡：4*25GE Hi1822 **(与数据库端在同一个交换机)** |      |

OS版本说明如下：

| 操作系统  | OS版本      | 版本说明                                                     |
| --------- | ----------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS) | openEuler 20.03 (LTS)，aarch版本ISO:<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |

# 3     测试结论概述

## 3.1   测试整体结论

性能特性共计执行9个用例，主要覆盖了dolphin_hot_standby参数是否可以正常设置，该参数的基础功能测试和主备切换情况下是否可用，共发现0个问题单，整体质量良好。

| 测试活动                                                     | 活动评价                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 通过设置guc参数的三种方式设置dolphin_hot_standby参数并观察设置是否生效 | 通过alter system set dolphin_hot_standby、修改postgresql.conf、gs_guc set dolphin_hot_standby三种方式设置dolphin_hot_standby参数均生效，设置值为非bool类型时可以正常报错，默认值和资料保持一致，说明参数设置正常。 |
| 分别设置hot_standby参数为on或off，dolphin_hot_standby参数为on或off，然后通过JDBC和客户端连接备机的openGauss端口和MySQL端口，观察连接状况是否符合预期 | hot_standby参数为off时，不论dolphin_hot_standby参数如何设置，备机的openGauss端口和MySQL端口均无法连接。<br />hot_standby参数为on，dolphin_hot_standby参数为off时，备机的openGauss端口可以连接，MySQL端口无法连接。<br />hot_standby参数为on，dolphin_hot_standby参数为on时，备机的openGauss端口和MySQL端口均可以连接。<br />连接状况符合预期，说明参数功能可用。 |
| 主备切换时设置hot_standby参数为on，分别设置dolphin_hot_standby参数为on或off，观察连接状况 | hot_standby参数为on，dolphin_hot_standby参数为off时，备机的openGauss端口可以连接，MySQL端口无法连接。主备切换后，建立到原主的连接断开，新主可建立新连接。<br />hot_standby参数为on，dolphin_hot_standby参数为on时，备机的openGauss端口和MySQL端口均可以连接。主备切换后，只有建立到原主的连接断开，新主和新备都可建立新连接。<br />连接状况符合预期，说明参数功能可用。 |

### 

## 3.2   约束说明

- MySQL JDBC driver应为5.1.47版本，MySQL命令行客户端版本号应在5.7.26~5.7.42之间

## 3.3   问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%       | 0%   | 0%   | 0%   | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号 | 问题级别 | 问题简述 | 问题状态 |
| ---- | ------- | -------- | -------- | -------- |
|      |         |          |          |          |

# 4     测试执行

## 4.1   测试结果

### 4.1.1 使用alter system set方式设置参数

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.执行alter system set dolphin_hot_standby = on，重启数据库后查看参数<br/>show dolphin_hot_standby;<br/>2.执行alter system set dolphin_hot_standby = off，重启数据库后查看参数<br/>show dolphin_hot_standby; | 1、参数dolphin_hot_standby的值为on<br/><br/>2、参数dolphin_hot_standby的值为off<br/>以上结果说明参数的值可以正常设置 |

### 4.1.2 使用修改postgresql.conf方式设置参数

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、修改postgresql.conf，添加dolphin_hot_standby = on，重启数据库后查看参数<br/>show dolphin_hot_standby;<br/><br/>2、修改postgresql.conf，添加dolphin_hot_standby = off，重启数据库后查看参数<br/>show dolphin_hot_standby;<br/><br/>3、修改postgresql.conf，添加dolphin_hot_standby = 12345，重启数据库后查看参数<br/>show dolphin_hot_standby;<br/><br/>4、修改postgresql.conf，使用#注释掉dolphin_hot_standby = 12345，重启数据库后查看参数<br/>show dolphin_hot_standby; | 1、参数dolphin_hot_standby的值为on<br/><br/>2、参数dolphin_hot_standby的值为off<br/><br/>3、重启数据库报错，参数dolphin_hot_standby的值不正确<br/><br/>4、参数dolphin_hot_standby的值变为默认值on<br/>以上结果说明参数的值可以正常设置 |

### 4.1.3 使用gs_guc set方式设置参数

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、查看dolphin_hot_standby默认值是否为on<br/>修改postgresql.conf注释掉数据库参数dolphin_hot_standby，重启数据库后查看参数show dolphin_hot_standby;<br/><br/>2、使用设置gs_guc set设置dolphin_hot_standby为其他数据类型<br/>gs_guc set -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby=12345"<br/>启动数据库后查看参数show dolphin_hot_standby;<br/>gs_guc set -N all -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby=12345"<br/>重启数据库后查看参数show dolphin_hot_standby;<br/><br/>3、使用设置gs_guc set设置dolphin_hot_standby为off<br/>gs_guc set  -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby=off"<br/>重启数据库后查看参数show dolphin_hot_standby;<br/>gs_guc set -N all -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby=off"<br/>重启数据库后查看参数show dolphin_hot_standby;<br/><br/>4、使用设置gs_guc set设置dolphin_hot_standby为默认值<br/>gs_guc set -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby"<br/>重启数据库后查看参数show dolphin_hot_standby;<br/>gs_guc set -N all -D /opt/opengauss810/cluster/dn1 -c "dolphin_hot_standby"<br/>重启数据库后查看参数show dolphin_hot_standby; | 1、显示参数dolphin_hot_standby的默认值为on<br/><br/>2、报错，参数修改失败<br/><br/>3、不论哪种方式设置，参数dolphin_hot_standby的值为off<br/><br/>4、不论哪种方式设置，参数dolphin_hot_standby的值为默认值on<br/>以上结果说明参数的值可以正常设置 |

### 4.1.4 dolphin_hot_standby=on、hot_standby=on状态测试备机连接

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=on、hot_standby=on并启动数据库<br/><br/>2、通过openGauss客户端连接备机的openGauss端口<br/><br/>3、通过MySQL客户端连接备机的MySQL端口(即dolphin_server_port对应的端口)<br/><br/>4、通过MySQL JDBC driver连接备机的MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机 | 1、guc参数成功被修改<br/><br/>2、备机的openGauss端口正常连接<br/><br/>3、备机的MySQL端口正常连接<br/><br/>4、备机的MySQL端口正常连接<br/>以上结果说明参数功能可用 |

### 4.1.5 dolphin_hot_standby=on、hot_standby=off状态测试备机连接

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=on、hot_standby=off并启动数据库<br/><br/>2、通过openGauss客户端连接备机的openGauss端口<br/><br/>3、通过MySQL客户端连接备机的MySQL端口(即dolphin_server_port对应的端口)<br/><br/>4、通过MySQL JDBC driver连接MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机 | 1、guc参数成功被修改<br/><br/>2、备机的openGauss端口无法连接<br/><br/>3、备机的MySQL端口无法连接<br/><br/>4、只有备机的MySQL端口无法连接<br/>以上结果说明参数功能可用 |

### 4.1.6 dolphin_hot_standby=off、hot_standby=on状态测试备机连接

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=off、hot_standby=on并启动数据库<br/><br/>2、通过openGauss客户端连接备机的openGauss端口<br/><br/>3、通过MySQL客户端连接备机的MySQL端口(即dolphin_server_port对应的端口)<br/><br/>4、通过MySQL JDBC driver连接MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机 | 1、guc参数成功被修改<br/><br/>2、备机的openGauss端口正常连接<br/><br/>3、备机的MySQL端口无法连接<br/><br/>4、只有备机的MySQL端口无法连接<br/>以上结果说明参数功能可用 |

### 4.1.7 dolphin_hot_standby=off、hot_standby=off状态测试备机连接

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=off、hot_standby=off并启动数据库<br/><br/>2、通过openGauss客户端连接备机的openGauss端口<br/><br/>3、通过MySQL客户端连接备机的MySQL端口(即dolphin_server_port对应的端口)<br/><br/>4、通过MySQL JDBC driver连接MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机 | 1、guc参数成功被修改<br/><br/>2、备机的openGauss端口无法连接<br/><br/>3、备机的MySQL端口无法连接<br/><br/>4、只有备机的MySQL端口无法连接<br/>以上结果说明参数功能可用 |

### 4.1.8 备机MySQL端口不可用状态进行主备切换功能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=off、hot_standby=on并启动数据库<br/><br/>2、通过MySQL JDBC Driver连接MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机<br/><br/>3、预置数据，使用连接查询表中语句验证连接均可用，之后暂停并进行主备切换，再次新增连接 | 1、guc参数成功被修改<br/><br/>2、原主正常连接，原备不可连接<br/><br/>3、主备切换后，原主连接断开，新主可建立新连接<br/>以上结果说明参数功能可用 |

### 4.1.9 备机MySQL端口可用状态进行主备切换功能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、设置guc参数dolphin_hot_standby=on、hot_standby=on并启动数据库<br/><br/>2、通过MySQL JDBC Driver连接MySQL端口(即dolphin_server_port对应的端口)，连接时设置多个连接和ip地址同时连接主机和备机<br/>3、预置数据，使用连接查询表中语句验证连接均可用，之后暂停并进行主备切换，再次新增连接 | 1、guc参数成功被修改<br/><br/>2、原主正常连接，原备可以连接<br/><br/>3、主备切换后，只有原主连接断开，新主和新备均可建立新连接<br/>以上结果说明参数功能可用 |

###  4.1.10 资料测试

对资料进行测试，文档描述参数说明和取值范围等均准确，资料链接：https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/DatabaseReference/%E8%BF%9E%E6%8E%A5%E8%AE%BE%E7%BD%AE.html

## 4.2   测试执行统计数据

| 版本名称             | 测试用例数 | 用例执行结果           | 发现问题单数 |
| -------------------- | ---------- | ---------------------- | ------------ |
| openGauss 6.0.0 RC05 | 9          | Passed:9<br />Failed:0 | 0            |

###### 数据项说明：

首轮测试用例执行9个，通过9个。

本需求缺陷密度为0(缺陷个数)/0.013k(代码行数)=0(个/kloc)。

## 4.3   后续测试建议

无

# 5     附件

*无*

 



 

 
