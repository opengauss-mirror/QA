![avatar](../../../images/openGauss.png)

版权所有 © 2023  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述           | 作者 |
| ---------- | ----------- | ------------------ | ---- |
| 2023-09-19 | 1.0         | 兼容性测试报告初稿 | 宋晶 |
| 2023-09-21 | 1.1         | 增加升级测试部分   | 李鑫 |
| 2023-09-26 | 1.2         | 补充升级路径       | 李鑫 |
| 2023-09-28 | 1.3         | 增加轻量版升级部分  | 李鑫 |

 关键词： 软件兼容；升级兼容；硬件兼容；灰度升级；就地升级；openGauss 5.1.0；轻量版；Lite

 

摘要：本报告基于对openGauss 5.1.0 版本进行兼容性测试，描述openGauss的兼容性测试情况，报告主要包括四个方面：企业版升级兼容性测试（关注灰度升级、就地升级、升级后回滚、回滚后重新升级提交、升级后特性功能运行正常），轻量版升级兼容性测试（升级、升级回退、升级后特性功能运行正常），硬件兼容（支持在不同服务器上部署、支持在不同硬盘部署），软件兼容（支持容器化部署、支持在OpenEuler及CentOS操作系统上部署），根据测试情况评估openGaus兼容性质量，识别兼容性关键风险。



缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

本报告是面向开源的openGauss 5.1.0版本兼容性测试报告，报告主要包括openGauss 5.1.0 版本兼容性测试的四个方面：
- 企业版升级兼容性测试：重点关注通过就地升级/灰度升级等方式，直接/间接升级至openGauss 5.1.0版本，升级失败或升级未提交时回滚/强制回滚到原版本，再重新升级提交至openGauss 5.1.0版本，升级后特性功能运行正常；
- 轻量版升级兼容性测试：重点关注升级后特性功能运行正常、升级失败或者升级未提交可以回滚到原版本；
- 硬件兼容：支持在X86/鲲鹏服务器上部署，支持在本地盘、云盘上部署，支持在SAS、SATA、SSD部署；
- 软件兼容：支持容器化部署，支持在OpenEuler 20.03 LTS、OpenEuler 22.03 LTS、CentOS 7.6等操作系统上部署。

报告对兼容性测试工作进行总结，结合兼容性问题解决情况，为openGauss 5.1.0 版本开源提供依据。

# 2     特性测试信息

## 2.1   测试对象版本及测试时间

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称                       | 测试起始时间 | 测试结束时间 |测试项 |
| ------------------------------ | ------------ | ------------ |------------ |
| openGauss 5.1.0 build 2ef75654 | 2023-09-06   | 2023-09-08   | 软硬件兼容   |
| openGauss 5.1.0 build 86199cfa | 2023-09-05   | 2023-09-13   |企业版升级   |
| openGauss 5.1.0 build 8cfdb19a | 2023-09-14   | 2023-09-19   |企业版升级   |
| openGauss 5.1.0 build 24ebc36b | 2023-09-20   | 2023-09-22   |企业版升级   |
| openGauss 5.1.0 build 4e532edf | 2023-09-25   | 2023-09-25   |轻量版升级   |
| openGauss 5.1.0 build b5a8d5b0 | 2023-09-28   | 2023-09-28   |轻量版升级   |

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
| OpenEuler release20.03（LTS） | 18.09.0, build 8f92b96    |
| OpenEuler release22.03（LTS） | 18.09.0, build d1134d1    |



# 3     测试结论概述

## 3.1   测试整体结论
兼容性测试整体结论如下，满足版本兼容性要求。

- openGauss 5.1.0 企业版支持从openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级和就地升级至openGauss 5.1.0，升级失败或者升级未提交可以回滚到原版本，故障场景环境恢复后升级可以重入并升级成功；openGauss 5.1.0 企业版升级测试共计测试3轮，共143个场景，覆盖直接升级和间接升级、就地升级和灰度升级、回滚和强制回滚、带cm升级和不带cm升级，32个场景不支持，其他测试通过，共发现5个问题，1个非问题取消，其余均已修复并验证回归通过。
- openGauss轻量版本支持从openGauss 3.0.0/openGauss 5.0.0/升级至openGauss 5.1.0，升级失败或者升级未提交可以回滚到原版本，故障场景环境恢复后升级可以重入并升级成功；openGauss 3.1.0暂不支持升级至openGauss 5.0.0/5.1.0，当前遗留，后续修复；openGauss 5.1.0轻量版升级测试共计测试2轮，共12个场景，包含直接升级和间接升级，共发现3个问题，1个遗留，1个非问题取消，1个已修复并验证回归通过。
- 支持在X86/鲲鹏服务器上部署，支持在本地盘、云盘上部署，支持在SAS、SATA、SSD部署；支持容器化部署，支持在openEuler 20.03 LTS、CentOS 7.6操作系统上部署；openGauss 5.1.0企业版软硬件兼容性测试共计测试2轮，在X86+CentOS 7.6，ARM+openEuler 20.03 LTS，X86+openEuler 20.03 LTS，ARM+openEuler 22.03 LTS，X86+openEuler 22.03 LTS环境下共计执行14个用例，测试用例累计执行率100%，测试发现1个问题，已修复并验证回归通过。

#### 3.1.1 企业版升级兼容

针对openGauss 5.1.0 企业版，规划的升级路径如下表所示（不支持场景未列出）。测试情况：

- openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1升级到openGauss 5.1.0.成功，升级失败或者升级未提交，可以成功回滚；
- openGauss 2.0.0/openGauss 3.0.0/openGauss3.1.0升级到中间版本，间接升级到openGauss 5.1.0.成功，升级失败或者升级未提交，可以成功回滚；
- 升级成功后，特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1灰度升级到openGauss 5.1.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1就地升级到openGauss 5.1.0后强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1不带cm灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0/openGauss 2.0.1/openGauss 3.0.0/openGauss 3.1.0/openGauss 3.0.5/openGauss 5.0.0/openGauss 5.0.1带cm灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0灰度升级到openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 2.0.0不带cm灰度升级到openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0带cm，再灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 2.0.0不带cm灰度升级到openGauss 2.0.5/openGauss 3.0.0/openGauss 3.1.0/openGauss 5.0.0不带cm，再灰度升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0灰度升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0不带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.0.0不带cm就地升级到openGauss 3.0.5/openGauss 3.1.0/openGauss 5.0.0不带cm，再就地升级到openGauss 5.1.0带cm | 测试通过 |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0 | 测试通过   |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过   |
| openGauss 3.1.0灰度升级到openGauss 5.0.1，再灰度升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过   |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0 | 测试通过   |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0，再回滚，再升级提交 | 测试通过   |
| openGauss 3.1.0就地升级到openGauss 5.0.1，再就地升级到openGauss 5.1.0，再强制回滚，再升级提交 | 测试通过   |
| openGauss 3.1.0带cm就地升级到openGauss 5.0.1带cm，再就地升级到openGauss 5.1.0带cm | 测试通过   |
| openGauss 3.1.0不带cm就地升级到openGauss 5.0.1带cm，再就地升级到openGauss 5.1.0带cm | 测试通过   |
| openGauss 3.1.0不带cm就地升级到openGauss 5.0.1不带cm，再就地升级到openGauss 5.1.0带cm | 测试通过   |

#### 3.1.2 轻量版升级兼容

针对openGauss 5.1.0 轻量版，规划的升级路径如下表所示。测试情况：

- openGauss 3.0.0/openGauss 5.0.0升级到openGauss 5.1.0成功，升级失败或者升级未提交，可以成功回滚；
- openGauss 3.0.0升级到中间版本，间接升级到openGauss 5.1.0成功，升级失败或者升级未提交，可以成功回滚；
- openGauss 3.1.0升级到openGauss 5.1.0失败，或升级到中间版本，间接升级到openGauss 5.1.0失败，问题单遗留，后续修复；
- 升级成功后，特性功能运行正常。

| 升级路径                                                     | 测试结论 |
| ------------------------------------------------------------ | -------- |
| openGauss 3.0.0/openGauss 5.0.0直接升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0/openGauss 5.0.0直接升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.0.0升级到openGauss 5.0.0，升级到openGauss 5.1.0 | 测试通过 |
| openGauss 3.0.0升级到openGauss 5.0.0/openGauss 5.0.0，升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0升级到openGauss 5.1.0后回滚，再升级提交 | 测试通过 |
| openGauss 3.1.0升级到openGauss 5.1.0 | 测试未通过，问题单遗留，后续修复 |
| openGauss 3.1.0升级到openGauss 5.0.0，升级到openGauss 5.1.0 | 测试未通过，问题单遗留，后续修复 |
| openGauss 3.1.0升级到openGauss 5.0.0，升级到openGauss 5.1.0，回滚，再升级提交| 测试未通过，问题单遗留，后续修复 |
#### 3.1.3 硬件兼容

| Domain       | 测试活动                                                     | 测试结论 |
| ------------ | ------------------------------------------------------------ | -------- |
| 服务器兼容   | 在X86（Intel(R) Xeon(R) Gold）/鲲鹏（Kunpeng 920）服务器上安装部署openGauss 5.1.0数据库 | 测试通过 |
| 存储设备兼容 | 在本地盘（SAS、SATA和SSD）及云盘上安装部署openGauss 5.1.0数据库 | 测试通过 |

#### 3.1.4 软件兼容

| Domain       | 测试活动                                                     | 测试结论                      |
| ------------ | ------------------------------------------------------------ | ----------------------------- |
| 支持云化部署 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/X86+openEuler 22.03 LTS/ARM+openEuler 22.03 LTS环境下，通过Docker（18.03.1-ce, build 9ee9f40、18.09.0, build 8f92b96、18.09.0, build d1134d1）安装部署openGauss 5.1.0数据库 | 发现1个问题，已修复且回归通过 |
| 操作系统兼容 | 在X86+Centos7.6/X86+openEuler 20.03 LTS/ARM+openEuler 20.03 LTS/ARM下，安装部署openGauss 5.1.0数据库 | 测试通过                      |

## 3.2   约束说明

1. 企业版升级兼容测试的前置条件及约束：
   - 大版本升级推荐使用灰度升级，升级时环境要求符合gs_upgradectl工具注意事项；
   - 从2.0.0和2.0.1版本升级到5.1.0版本，升级未提交阶段，不支持备份；
   - 从2.0.0和2.0.1版本升级到5.1.0版本，升级未提交阶段，不支持强制回滚;
   - 2.x版本不支持cm工具。
2. 轻量版升级兼容性测试的前置条件及约束：轻量版升级过程会中断业务运行。
2. 硬件兼容测试的前置条件及约束：Linux环境运行正常，磁盘空闲空间1G。
3. 软件兼容测试的前置条件及约束：Linux环境运行正常，磁盘空闲空间1G。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| [I84A4S](https://gitee.com/opengauss/openGauss-server/issues/I84A4S?from=project-issue)       | 轻量版3.1.0升级到5.1.0版本报错，Exec sql on postgres failed.Error: upgrade post sql failed         | 次要         |  影响：openGauss轻量版3.1.0升级到5.0.0/5.1.0失败；规避措施：升级失败后再次执行升级命令，或回滚后重新升级可升级成功|  待办的        |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 7        | 0    | 4    | 3    | 0      |
| 百分比 | 100%     | 0%   | 57%  | 43%  | 0%     |

### 3.3.3问题单汇总

| 问题单号                                                     | 问题级别 | 问题简述                                                     | 问题单状态 |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ---------- |
| [I82MZR](https://gitee.com/opengauss/openGauss-server/issues/I82MZR?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.1.0】【升级】2.0.0/3.0.0-3.1.0-5.1.0灰度升级，升级到5.1.0失败，报错Key (oid)=(560) already exists | 已验收     |
| [I81P6C](https://gitee.com/opengauss/openGauss-server/issues/I81P6C?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.1.0】【升级】2.0.0-3.1.0-5.1.0就地升级，升级到5.1.0失败，报错could not open file ""base/15103/2690"" | 已验收     |
| [I7ZKJJ](https://gitee.com/opengauss/openGauss-server/issues/I7ZKJJ?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.1.0】【升级】 5.0.1就地升级至5.1.0失败，报错ERROR: extension "dolphin" has no update path from version "1.1" to version "2.0" | 已验收     |
| [I7ZFSX](https://gitee.com/opengauss/openGauss-server/issues/I7ZFSX?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.1.0】【升级】 3.0.5升级至5.1.0失败，报错ERROR: duplicate key value violates unique constraint "pg_proc_oid_index" | 已验收     |
| [I7ZYTB](https://gitee.com/opengauss/openGauss-server/issues/I7ZYTB?from=project-issue) | 主要     | 【测试类型：工具功能】【测试版本：5.1.0】【升级】2.0.0-3.0.0-5.1.0就地升级，升级到3.0.0失败，报错无法识别comm_quota_size等参数 | 已取消     |
| [I7ZKSO](https://gitee.com/opengauss/openGauss-container/issues/I7ZKSO?from=project-issue) | 次要     | 在x86环境中通过buildDockerImage.sh创建镜像，启动容器后查询部署集群状态查询失败 | 已验收     |
| [I845ZN](https://gitee.com/opengauss/openGauss-server/issues/I845ZN?from=project-issue) | 次要     | 轻量版3.0.0升级到5.1.0版本报错，无法连接兼容B库，ERROR:  type "anyset" is only a shell | 已验收     |
| [I84A4S](https://gitee.com/opengauss/openGauss-server/issues/I84A4S?from=project-issue) | 次要     | openGauss轻量版低版本升级到5.1.0，回滚升级后查询插件显示有1.0版本dolphin插件，实际验证没有 | 已取消     |
| [I7ZKSO](https://gitee.com/opengauss/openGauss-container/issues/I7ZKSO?from=project-issue) | 次要     | 轻量版3.1.0升级到5.1.0版本报错，Exec sql on postgres failed.Error: upgrade post sql failed | 待办的     |

# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称                       | 测试用例数 | 测试项 |用例执行结果       | 发现问题单数 |
| ------------------------------ | ---------- | ---------- |------------------ | ------------ |
| openGauss 5.1.0 build 2ef75654 | 14         |软硬件兼容        | Passed:12  Failed:2 | 1            |
| openGauss 5.1.0 build 2ef75654 | 2          |软硬件兼容          |Passed:2  Failed:0 | 0            |
| openGauss 5.1.0 build 86199cfa | / | 企业版升级 | /         | 2            |
| openGauss 5.1.0 build 8cfdb19a | / |企业版升级 |/         | 2            |
| openGauss 5.1.0 build 24ebc36b | / |企业版升级 | /         | 0            |
| openGauss 5.1.0 build 4e532edf | / |轻量版升级 |/         | 2            |
| openGauss 5.1.0 build b5a8d5b0 | / |轻量版升级 | /         | 0            |

*数据项说明：*

*测试用例数－－到本测试活动结束时，所有可用测试用例数；*

*发现问题单数－－本测试活动总共发现的问题单数。*

1.除遗留问题外，以上其余问题单均已修复，已验证回归通过。

## 4.2   后续测试建议

无

# 5     附件

无

 



 

 