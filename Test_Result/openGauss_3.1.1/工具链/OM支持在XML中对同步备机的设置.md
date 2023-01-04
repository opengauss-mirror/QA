

![avatar](../../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述             | 作者     |
| ---------- | ----------- | -------------------- | -------- |
| 2022-12-08 | 1.0         | 特性测试报告初稿完成 | lihongji |

关键词：OM支持在XML中对同步备机的设置 、FIRST、ANY、 syncNode_hostname 

摘要：在安装前可以通过XML文件指定各节点机的同步备， syncNode_hostname中的hostname根据主机名自行替换。参数中指定同步备主机名存在且正确，同步备数量不能超过备选同步备主机个数。参数中FIRST与ANY不可以同时存在，ANY支持组合配置，FIRST不支持组合配置 。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

OM支持在XML中对同步备机设置，在安装前用户可以指定数据库节点对应的同步备机名称，安装后指定的同步备机生效

# 2     特性测试信息

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 3.1.0 build 0d771b43 | 2022-11-09   | 2022-11-19   |

描述特性测试的硬件环境信息

| 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz<br>内存：30GB<br>硬盘：261GB<br>OS：openEuler  release 20.03 (LTS)<br> |  一主三备    |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>内存：15GB<br/>硬盘：20GB<br/>OS：openEuler  release 20.03 (LTS-SP1)<br/> |      |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz<br/>内存：15GB<br/>硬盘：17GB<br/>OS：openEuler  release 20.03 (LTS-SP1)<br/> |      |
| 虚拟机   | CPU:  Intel(R) Xeon(R) Gold 6287C CPU @ 2.60GHz<br/>内存：7GB<br/>硬盘：30GB<br/>OS：openEuler  release 20.03 (LTS)<br/> |      |

# 3     测试结论概述

## 3.1   测试整体结论

OM支持在XML中对同步备机的设置共计执行19条用例，主要覆盖了功能测试、资料测试。功能测试验证了ANY、FRIST方式指定同步备，通过该方式指定同步备后进行数据安装，同步备生效且准确。资料测试中描述恰当，示例正确无误。共计发现6个问题，其中6个问题已解决。问题均回归通过，无遗留风险，整体质量良好。

| 测试活动 | 活动评价                                                   |
| -------- | ---------------------------------------------------------- |
| 功能测试 | FIRST、ANY方式是设置同步备，数据库安装成功后设置生效，通过 |
| 资料测试 | 使用方法描述准确，示例正确无误，通过                       |
## 3.2   约束说明

1. FIRST不支持组合配置

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | :------: | :--: | :--: | :--: | :----: |
| 数目   |    6     |  0   |  0   |  6   |   0    |
| 百分比 |   100%   |  0%  |  0%  | 100% |   0%   |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级      别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | :--------- | ------------------------------------------------------------ | ------ |
| 1    | [I62VZY](https://gitee.com/opengauss/openGauss-OM/issues/I62VZY?from=project-issue) | 次要       | xml文件中设置同步备，实际设置的节点名与配置的个数不符，在gs_guc校验前没有合理报错，给出提示 | 已验收 |
| 2    | [I62BSS](https://gitee.com/opengauss/openGauss-OM/issues/I62BSS?from=project-issue) | 次要       | xml文件中删除某个节点的name="syncNode_{hostname}"及主机名为空，没有合理报错 | 已验收 |
| 3    | [I61YJX](https://gitee.com/opengauss/openGauss-OM/issues/I61YJX?from=project-issue) | 次要       | xml文件中FIRST与ANY 都存在，进行gs_install安装时，未合理报错给出提示 | 已验收 |
| 4    | [I61XCT](https://gitee.com/opengauss/openGauss-OM/issues/I61XCT?from=project-issue) | 次要       | 备机节点配置同步备，备节点主机名与同步备主机名相同，没有合理报错 | 已验收 |
| 5    | [I60XKH](https://gitee.com/opengauss/openGauss-OM/issues/I60XKH?from=project-issue) | 次要       | 使用om安装方式，在xml文件配置中，各节点通过ANY的方式指定同步备，预安装成功。gs_install安装失败 | 已验收 |
| 6    | [I64G9Q](https://gitee.com/opengauss/docs/issues/I64G9Q?from=project-issue) | 次要       | xml文件中指定同步备FIRST存在组合配置                         | 已验收 |



# 4     测试执行

## 4.1   测试执行步骤

### 4.1.1 功能测试

| 测试步骤：                                                   | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1.  在XML文件中通过ANY、FIRST方式设定同步备<br />2 . 编译openGauss-OM仓库，生成openGauss-3.1.0-openEuler-64bit-om.sha256、openGauss-3.1.0-openEuler-64bit-om.tar.gz<br />3. ./gs_preinstall -U omm -G dbgrp -X /opt/software/openGauss/cluster_config.xml预安装<br />4. gs_install -X /opt/software/openGauss/clusterconfig.xml安装<br />5. 查看指定的同步备生效 | 共执行19条用例，<br />共发现5个bug，其中3个转为资料单，均已修复且验收通过 |

### 4.1.2 资料测试

| 测试步骤                     | 测试结果                            |
| ------------------------------ | ----------------------------------- |
| 1. 使用方法描述<br>2. 示例测试 | 共发现1个bug，现已修复且验收通过 |

## 4.2   测试执行统计数据

|            版本名称            | 测试用例数 |     用例执行结果     | 发现问题单数 |
| :----------------------------: | :--------: | :------------------: | :----------: |
| openGauss 3.1.0 build 0d771b43 |     19     | Passed：13 Failed：6 |      6       |
| openGauss 3.1.0 build ef6a5318 |     19     | Passed：19 Failed：0 |      0       |

数据说明
1. datacheck全量校验满足70M/S测试共执行19条用例，共计发现6个bug
2. 以上缺陷4个为资料单，不计入代码缺陷密度。缺陷密度为2(缺陷个数)/0.212k(代码行数)=9.43(个/kloc)

## 4.3   后续测试建议

无

# 5. 附件

## 5.1 一主三备配置文件（支持自定义利用FIRST、ANY设置同步备机）

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<ROOT> 
    <!-- openGauss整体信息 --> 
    <CLUSTER> 
        <PARAM name="clusterName" value="Cluster_template" /> 
        <PARAM name="nodeNames" value="node1_hostname,node2_hostname,node3_hostname,node4_hostname" /> 
    
        <PARAM name="gaussdbAppPath" value="/opt/huawei/install/app" /> 
        <PARAM name="gaussdbLogPath" value="/var/log/omm" /> 
        <PARAM name="tmpMppdbPath" value="/opt/huawei/tmp"/> 
        <PARAM name="gaussdbToolPath" value="/opt/huawei/install/om" /> 
        <PARAM name="corePath" value="/opt/huawei/corefile"/> 
        <PARAM name="backIp1s" value="192.168.0.1,192.168.0.2,192.168.0.3,192.168.0.4"/> 
         
    </CLUSTER> 
    <!-- 每台服务器上的节点部署信息 --> 
    <DEVICELIST> 
        <!-- node1上的节点部署信息 --> 
        <DEVICE sn="node1_hostname"> 
            <PARAM name="name" value="node1_hostname"/> 
            <PARAM name="azName" value="AZ1"/> 
            <PARAM name="azPriority" value="1"/> 
            <!-- 如果服务器只有一个网卡可用，将backIP1和sshIP1配置成同一个IP --> 
            <PARAM name="backIp1" value="192.168.0.1"/> 
            <PARAM name="sshIp1" value="192.168.0.1"/> 
            <!--CM节点部署信息--> 
            <PARAM name="cmsNum" value="1"/> 
            <PARAM name="cmServerPortBase" value="15000"/> 
            <PARAM name="cmServerListenIp1" value="192.168.0.1,192.168.0.2,192.168.0.3,192.168.0.4"/> 
            <PARAM name="cmServerHaIp1" value="192.168.0.1,192.168.0.2,192.168.0.3,192.168.0.4"/> 
            <PARAM name="cmServerlevel" value="1"/> 
            <PARAM name="cmServerRelation" value="node1_hostname,node2_hostname,node3_hostname,node4_hostname"/> 
            <PARAM name="cmDir" value="/opt/huawei/data/cmserver"/> 
	    <!--dn--> 
            <PARAM name="dataNum" value="1"/> 
	    <PARAM name="dataPortBase" value="15400"/> 
	    <PARAM name="dataNode1" value="/opt/huawei/install/data/dn,node2_hostname,/opt/huawei/install/data/dn,node3_hostname,/opt/huawei/install/data/dn,node4_hostname,/opt/huawei/install/data/dn ,/opt/huawei/install/data/dn"/> 
            <!-- syncNode_hostname中的hostname替换为自己的主机名称 -->
            <PARAM name="syncNode_node1_hostname" value="ANY 2(node2_hostname, node4_hostname), ANY 1(node3_hostname, node2_hostname)"/> 
        </DEVICE> 
 
        <!-- node2上的节点部署信息，其中“name”的值配置为主机名称 --> 
        <DEVICE sn="node2_hostname"> 
            <PARAM name="name" value="node2_hostname"/> 
            <PARAM name="azName" value="AZ1"/> 
            <PARAM name="azPriority" value="1"/> 
            <!-- 如果服务器只有一个网卡可用，将backIP1和sshIP1配置成同一个IP --> 
            <PARAM name="backIp1" value="192.168.0.2"/> 
            <PARAM name="sshIp1" value="192.168.0.2"/> 
            <!-- syncNode_hostname中的hostname替换为自己的主机名称 -->
            <PARAM name="syncNode_node2_hostname" value="ANY 2(node1_hostname, node3_hostname), ANY 1(node1_hostname, node4_hostname)"/>
            <!-- cm --> 
            <PARAM name="cmServerPortStandby" value="15000"/> 
            <PARAM name="cmDir" value="/opt/huawei/data/cmserver"/> 
	</DEVICE> 
 
        <!-- node3上的节点部署信息，其中“name”的值配置为主机名称 --> 
        <DEVICE sn="node3_hostname"> 
            <PARAM name="name" value="node3_hostname"/> 
            <PARAM name="azName" value="AZ1"/> 
            <PARAM name="azPriority" value="1"/> 
            <!-- 如果服务器只有一个网卡可用，将backIP1和sshIP1配置成同一个IP --> 
            <PARAM name="backIp1" value="192.168.0.3"/> 
            <PARAM name="sshIp1" value="192.168.0.3"/> 
            <!-- syncNode_hostname中的hostname替换为自己的主机名称 -->
            <PARAM name="syncNode_node3_hostname" value="FIRST 3(node2_hostname, node1_hostname, node4_hostname)"/>
            <!-- cm --> 
            <PARAM name="cmServerPortStandby" value="15000"/> 
            <PARAM name="cmDir" value="/opt/huawei/data/cmserver"/> 
	</DEVICE> 
 
        <!-- node4上的节点部署信息，其中“name”的值配置为主机名称 --> 
        <DEVICE sn="node4_hostname"> 
            <PARAM name="name" value="node4_hostname"/> 
            <PARAM name="azName" value="AZ1"/> 
            <PARAM name="azPriority" value="1"/> 
            <!-- 如果服务器只有一个网卡可用，将backIP1和sshIP1配置成同一个IP --> 
            <PARAM name="backIp1" value="192.168.0.4"/> 
            <PARAM name="sshIp1" value="192.168.0.4"/> 
            <!-- syncNode_hostname中的hostname替换为自己的主机名称 -->
            <PARAM name="syncNode_node4_hostname" value="ANY 2(node2_hostname, node1_hostname, node1_hostname, node3_hostname)"/>
            <!-- cm --> 
            <PARAM name="cmServerPortStandby" value="15000"/> 
            <PARAM name="cmDir" value="/opt/huawei/data/cmserver"/> 
	</DEVICE> 
    </DEVICELIST> 
</ROOT>

```



```sql


```