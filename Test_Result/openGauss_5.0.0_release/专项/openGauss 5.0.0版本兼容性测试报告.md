![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述           | 作者   |
| ---------- | ----------- | ------------------ | ------ |
| 2023-03-23 | 1.0         | 兼容性测试报告初稿 | 李鑫   |
| 2023-03-23 | 1.1         | 升级测试报告初稿   | 白小丽 |
| 2023-03-28 | 2.0         | 根据评审意见修改报告   | 白小丽 |

 关键词： 软件兼容；升级兼容；硬件兼容；灰度升级；openGauss 5.0.0

 

摘要：本报告基于对openGauss 5.0.0 版本进行兼容性测试，描述openGauss的兼容性测试情况，报告主要包括三个方面：升级兼容性测试（关注灰度升级、升级回退、升级故障后恢复重入、升级后特性功能运行正常），硬件兼容（支持在不同服务器上部署、支持在不同硬盘部署），软件兼容（支持容器化部署、支持在OpenEuler及CentOS操作系统上部署），根据测试情况评估openGaus兼容性质量，识别兼容性关键风险。



缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

本报告是面向开源的openGauss 5.0.0版本兼容性测试报告，报告主要包括openGauss 5.0.0 版本兼容性测试的三个方面：升级兼容性测试：重点关注升级后特性功能运行正常、升级失败或者升级未提交可以回滚到原版本，支持任意版本升级至openGauss 5.0.0版本；硬件兼容：支持在X86/鲲鹏服务器上部署，支持在本地盘、云盘上部署，支持在SAS、SATA、SSD部署；软件兼容：支持容器化部署，支持在OpenEuler 20.03 LTS、OpenEuler 22.03 LTS、CentOS 7.6等操作系统上部署。报告对兼容性测试工作进行总结，结合兼容性问题解决情况，为openGauss 5.0.0 版本开源提供依据。

# 2     特性测试信息

## 2.1   测试对象版本及测试时间

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.0.0 Release RC5    | 2023-2-15    | 2023-2-21    |
| openGauss 5.0.0 Release RC9    | 2022-3-15    | 2022-3-21    |
| openGauss 5.0.0 build 8c9b369a | 2022-3-2     | 2022-3-8     |
| openGauss 5.0.0 build df91ad5d | 2022-3-9     | 2022-3-14    |
| openGauss 5.0.0 build 78f2bb65 | 2022-3-15    | 2022-3-20    |
| openGauss 5.0.0 build cec10a6b | 2022-3-22    | 2022-3-22    |

## 2.2   测试环境的硬件信息

### 2.2.1.升级兼容

软硬件环境信息：

| 环境       | cpu  | 内存 | 磁盘 | 操作系统                            | 组网方式 |
| ---------- | ---- | ---- | ---- | ----------------------------------- | -------- |
| X86-CentOS | 4    | 15G  | 210G | CentOS Linux release 7.6.1810(Core) | 1主2备   |

### 2.2.2 硬件兼容

| 硬件型号                 | 硬件配置信息                                                 | 备注 |
| ------------------------ | ------------------------------------------------------------ | ---- |
| RH2288H V3               | CPU：Intel(R) Xeon(R) Gold E5-2698 64核<br />内存：384GB<br />硬盘：SSD 2.9T <br />OS：CentOS Linux release 7.6.1810（Core）<br />文件系统：EXT4<br />网卡：4 * 10GE |      |
| TaiShan 200 (Model 2280) | CPU：Kunpeng-920 5220 2p 64核<br />内存：512G<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4 * 25GE Hi1822 |      |

### 2.2.3 软件兼容

虚拟化平台：

| 虚拟化平台 | 版本说明                                     |
| ---------- | -------------------------------------------- |
| KVM虚拟化  | KVM+GuestOS (Centos7.6/OpenEuler20.03 (LTS)) |

Docker版本信息：

| OS系统                        | Docker版本                |
| ----------------------------- | ------------------------- |
| CentOS-7.6                    | 18.03.1-ce, build 9ee9f40 |
| OpenEuler release20.03（LTS） | 18.09.0, build 4f81b51    |
| OpenEuler release22.03（LTS） | 18.09.0, build d1134d1    |



# 3     测试结论概述

## 3.1   测试整体结论

openGauss 5.0.0版本支持从openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0灰度升级和就地升级至openGauss 5.0.0，升级失败或者升级未提交可以回滚到原版本，故障场景环境恢复后升级可以重入并升级成功；支持在X86/鲲鹏服务器上部署，支持在本地盘、云盘上部署，支持在SAS、SATA、SSD部署；支持容器化部署，支持在openEuler 20.03 LTS、CentOS 7.6操作系统上部署。满足版本兼容性要求。

openGauss 5.0.0版本升级测试共计测试4轮，执行测试用例33个，覆盖直接升级和间接升级、就地升级和灰度升级、回滚和强制回滚、带cm升级和不带cm升级，测试用例累计执行率100%，测试发现3个问题，均已个回归通过。

openGauss 5.0.0版本兼容性测试共计测试2轮，在X86+CentOS 7.6，ARM+openEuler 20.03 LTS，X86+openEuler 20.03 LTS，ARM+openEuler 22.03 LTS，X86+openEuler 22.03 LTS环境下共计执行14个用例，测试用例累计执行率100%，测试发现1个问题。

#### 5.0.0 升级兼容

针对openGauss 5.0.0 版本，规划的升级路径如下表所示。测试情况：

1. openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0升级到openGauss 5.0.0.成功，升级失败或者升级未提交，可以成功回滚；
2. 升级成功后，特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0灰度升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0就地升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0灰度升级到openGauss5.0.0后回滚 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0就地升级到openGauss5.0.0后回滚 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0灰度升级到openGauss5.0.0后强制回滚 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0就地升级到openGauss5.0.0后强制回滚 | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0不带cm灰度升级到openGauss5.0.0带cm | 测试通过 |
| openGauss2.0.0/openGauss2.0.1/openGauss3.0.0不带cm灰度升级到openGauss5.0.0带cm后回滚 | 测试通过 |
| openGauss3.0.0带cm灰度升级到openGauss5.0.0带cm               | 测试通过 |
| openGauss3.0.0带cm灰度升级到openGauss5.0.0带cm后回滚         | 测试通过 |
| openGauss2.0.0灰度升级到openGauss2.0.1再灰度升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0灰度升级到openGauss2.0.1再灰度升级到openGauss5.0.0后回滚到openGauss2.0.1 | 测试通过 |
| openGauss2.0.0灰度升级到openGauss2.0.1再灰度升级到openGauss5.0.0后强制回滚到openGauss2.0.1 | 测试通过 |
| openGauss2.0.0就地到openGauss2.0.1再就地升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0就地到openGauss2.0.1再就地升级到openGauss5.0.0后回滚到openGauss2.0.1 | 测试通过 |
| openGauss2.0.0就地升级到openGauss2.0.1再就地升级到openGauss5.0.0后强制回滚到openGauss2.0.1 | 测试通过 |
| openGauss2.0.0不带cm灰度升级到openGauss2.0.1不带cm再灰度升级到openGauss5.0.0带cm | 测试通过 |
| openGauss2.0.0灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0后回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.0灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0后强制回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.0就地到openGauss3.0.0再就地升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.0就地到openGauss3.0.0再就地升级到openGauss5.0.0后回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.0就地升级到openGauss3.0.0再就地升级到openGauss5.0.0后强制回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.0不带cm灰度升级到openGauss3.0.0不带cm再灰度升级到openGauss5.0.0带cm | 测试通过 |
| openGauss2.0.0不带cm灰度升级到openGauss3.0.0带cm再灰度升级到openGauss5.0.0带cm | 测试通过 |
| openGauss2.0.1灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.1灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0后回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.1灰度升级到openGauss3.0.0再灰度升级到openGauss5.0.0后强制回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.1就地到openGauss3.0.0再就地升级到openGauss5.0.0 | 测试通过 |
| openGauss2.0.1就地到openGauss3.0.0再就地升级到openGauss5.0.0后回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.1就地升级到openGauss3.0.0再就地升级到openGauss5.0.0后强制回滚到openGauss3.0.0 | 测试通过 |
| openGauss2.0.1不带cm灰度升级到openGauss3.0.0不带cm再灰度升级到openGauss5.0.0带cm | 测试通过 |
| openGauss2.0.1不带cm灰度升级到openGauss3.0.0带cm再灰度升级到openGauss5.0.0带cm | 测试通过 |

#### 3.1.2 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 5.0.0数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 5.0.0数据库 | 测试通过 |

#### 3.1.3 软件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 支持云化部署 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS环境下，通过Docker（18.03.1-ce, build 9ee9f40、18.09.0, build 4f81b51、18.09.0, build d1134d1）安装部署openGauss 5.0.0数据库 | 测试通过 |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/ARM+Kylin V10环境下，安装部署openGauss 5.0.0数据库 | 测试通过 |

## 3.2   约束说明

1. 升级兼容测试的前置条件及约束：
   1. 大版本升级推荐使用灰度升级，升级时环境要求符合gs_upgradectl工具注意事项；
   2. 从2.0.0和2.0.1版本升级到5.0.0版本，升级未提交阶段，不支持备份；
   3. 从2.0.0和2.0.1版本升级到5.0.0版本，升级未提交阶段，不支持强制回滚;
   4. 2.0.0和2.0.1版本不支持cm工具。
2. 硬件兼容测试的前置条件及约束：Linux环境运行正常，磁盘空闲空间1G。
3. 软件兼容测试的前置条件及约束：Linux环境运行正常，磁盘空闲空间1G。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号                                                     | 问题描述                                        | 问题级别 | 问题影响和规避措施                                 | 当前状态 |
| ------------------------------------------------------------ | ----------------------------------------------- | -------- | -------------------------------------------------- | -------- |
| 无 |  |      |  |    |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 4    | 0    | 0      |
| 百分比 | 100%     | 0%   | 100% | 0%   | 0%     |

### 3.3.3问题单汇总

| 问题单号                                                     | 问题级别 | 问题简述                                                     | 问题单状态 |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ---------- |
| [I6J9JM](https://gitee.com/opengauss/openGauss-server/issues/I6J9JM?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.0.0】 docker安装opengauss拉起镜像时报错FATAL:Listen_address can not set to empty | 已验收     |
| [I6L6RP](https://gitee.com/opengauss/openGauss-server/issues/I6L6RP) | 主要     | 3.0.0版本带cm灰度升级到5.0.0带cm失败报错ERROR: cannot assign TransactionIds during recovery | 已验收     |
| [I6LHBM](https://gitee.com/opengauss/openGauss-server/issues/I6LHBM) | 主要     | 2.0.0灰度升级到5.0.0后回滚，再次灰度升级报错                 | 已验收     |
| [I6O0K6](https://gitee.com/opengauss/CM/issues/I6O0K6)       | 主要     | 2.0.1、2.0.0和2.0.5版本不带cm工具灰度升级到5.0.0带cm工具后提交失败 | 已验收     |

# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果        | 发现问题单数 |
| ------------------------------ | ---------- | ------------------- | ------------ |
| openGauss 5.0.0 Release RC5    | 14         | Passed:12  Failed:2 | 1            |
| openGauss 5.0.0 Release RC9    | 2          | Passed:2  Failed:0  | 0            |
| openGauss 5.0.0 build 8c9b369a | 33         | Passed:31  Failed:2 | 1            |
| openGauss 5.0.0 build df91ad5d | 33         | Passed:31  Failed:2 | 1            |
| openGauss 5.0.0 build 78f2bb65 | 11         | Passed:8  Failed:2  | 1            |
| openGauss 5.0.0 build cec10a6b | 5          | Passed:5  Failed:0  | 0            |

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*发现问题单数－－本测试活动总共发现的问题单数。*

1.openGauss 5.0.0 Release RC5 版本发现1个问题，回归通过。

2.openGauss 5.0.0 build 8c9b369a版本发现1个问题，回归通过。

3.openGauss 5.0.0 build df91ad5d版本发现1个问题，回归通过。

4.openGauss 5.0.0 build 78f2bb65版本发现1个问题，回归通过。

## 4.2   后续测试建议

无

# 5     附件

无

 



 

 
