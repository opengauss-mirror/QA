![avatar](https://gitee.com/opengauss/QA/raw/c20a64f26771233d8d2a92c91138cbec5c4967ec/images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期      | 修订 版本 | 修改描述 | 作者        |
| --------- | --------- | -------- | ----------- |
| 2023-9-26 | V1.0      | 初稿     | houshaolong |

关键词：

chameleon、变色龙

摘要：

内网场景下，pip3 install 功能无法正常使用，导致原有方案portal无法进行chameleon的离线安装，使用将chameleon所在虚拟环境一起打包放入portal安装包，解决内网场景下chameleon无法正常安装的问题。

缩略语清单：无



# 1 特性概述

​		内网场景下，pip3 install 功能无法正常使用，导致原有方案portal无法进行chameleon的离线安装，使用将chameleon所在虚拟环境一起打包放入portal安装包，解决内网场景下chameleon无法正常安装的问题。

# 2 特性测试信息

##### 功能测试

| 版本名称        | 测试起始时间 | 测试结束时间 |
| --------------- | ------------ | ------------ |
| chameleon 5.0.0 | 2023-8-2     | 2023-9-11    |

硬件环境信息：

##### 功能测试

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 4核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.6.1810 (Core) |      |
| 虚拟机   | Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz 8核 内存：32GB 硬盘：300GB OS：CentOS Linux release 7.7.1908 (Core) |      |
| 虚拟机   | kunpeng-920 7260 2p 128核 内存：32 * 32GB 硬盘：3 * 2.9TB OS：openEuler release 20.03 (LTS-SP1) arrch64 |      |
| 虚拟机   | Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz 8核 内存：32GB 硬盘：128GB OS：openEuler release 20.03（LTS-SP1）arrch64 |      |
| 虚拟机   | Intel(R) Xeon(R) Platinum 8365HC CPU @ 3.00GHz 4核 内存：32GB 硬盘：128GB OS：openEuler release 22.03（LTS-SP1）X86 |      |
| 虚拟机   | Intel(R) Xeon(R) Platinum 8365HC CPU @ 3.00GHz 4核 内存：32GB 硬盘：128GB OS：openEuler release 22.03（LTS-SP1）arrch64 |      |

# 3 测试结论概述

## 3.1 测试整体结论

chameleon 解除跨平台依赖，共执行用例19个，主要覆盖了Centos7、openEuler20.03_x86、openEuler20.03_aarch64、openEuler22.03_x86、openEuler22.03_aarch64环境的安装和使用。资料测试覆盖chameleon 和portal资料的描述是否完整。累计发现缺陷单1个，1个缺陷已解决，回归通过，整体质量良好。

## 3.2 约束说明

1、环境需要安装jdk11及以上版本

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   | 备注 |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ | ---- |
| 1    | [I7UUXE](https://e.gitee.com/opengaussorg/issues/table?issue=I7UUXE) | 次要 | 【测试类型：工具】【测试版本：5.1.0】变色龙和portal没有对应的openEuler22.03的离线安装包和安装包下载链接 | 已验收 |      |

# 4 测试执行

## 4.1 功能测试-分别在不同操作系统离线安装chameleon 和poratl

| 测试步骤                                                     | 测试结果                                            |
| ------------------------------------------------------------ | --------------------------------------------------- |
| 1. 分别在Centos7、openEuler20.03_x86、openEuler20.03_aarch64、openEuler22.03_x86、openEuler22.03_aarch64环境离线安装变色龙和portal | 执行13条用例，执行结果符合预期，测试通过，未发现bug |

## 4.2 功能测试-通过DataKit分别在不同操作系统离线安装chameleon 和poratl

| 测试步骤                                                     | 测试结果                                           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 1.通过DataKit平台在Centos、openEuler环境离线安装变色龙和portal | 执行5条用例，执行结果符合预期，测试通过，未发现bug |

## 4.3 资料测试

| 测试步骤                                  | 测试结果                                                     |
| ----------------------------------------- | ------------------------------------------------------------ |
| 1. 查看文档离线安装包下载地址是否完整可用 | 执行1条用例，执行结果符合预期，测试通过，发现1个bug，1个bug已解决并回归通过 |

## 4.2 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称        | 测试用例数 | 用例执行结果            | 发现问题单数 |
| --------------- | ---------- | ----------------------- | ------------ |
| chameleon 5.0.0 | 19         | Passed:16<br/>Failed:3  | 1            |
| chameleon 5.0.0 | 19         | Passed:19<br/>Failed: 0 | 0            |

*数据项说明：*

- 累计发现缺陷单1个，为资料单
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：0(缺陷个数)0.893/kloc(代码行数)=0(个/kloc)



## 4.3 后续测试建议

无

