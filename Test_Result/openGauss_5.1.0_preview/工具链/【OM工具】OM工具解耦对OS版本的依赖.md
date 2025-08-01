![avatar](https://gitee.com/opengauss/QA/raw/c20a64f26771233d8d2a92c91138cbec5c4967ec/images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期      | 修订 版本 | 修改描述 | 作者        |
| --------- | --------- | -------- | ----------- |
| 2023-9-23 | V1.0      | 初稿     | houshaolong |

关键词：

OM工具、解耦、OS版本

摘要：

提供在OM工具中进行文件配置，只要用户的发行版基于OM工具支持的版本，且进行了相关文件配置，则可以顺利安装。

缩略语清单：无



# 1 特性概述

基于Centos和openEuler发行的版本越来越多，很多厂商对Centos和openEuler做了相关适配，在OM工具的严格校验下，导致OM工具checkos校验不通过，在此时，编辑一个配置文件，让用户根据自己的开源OS版本，填写相关内容，使用户可以顺利安装，提升用户体验。

# 2 特性测试信息

##### 功能测试

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss-5.1.0 build fb44126a | 2023-8-23    | 2023-9-11    |

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

OM工具解除对OS版本的依赖，共执行用例10个，功能测试主要覆盖了Centos和openEuler环境以及暂不支持的系统比如bcLinux上使用OM方式安装数据库，测试资料的描述是否完整。累计发现缺陷单0个，整体质量良好。

## 3.2 约束说明

无

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%       | 0%   | 0%   | 0%   | 0%     |

### 3.3.3 问题单汇总

无



# 4 测试执行

## 4.1 功能测试-配置osid.conf文件中对应的系统，直接安装数据库

| 测试步骤                                     | 测试结果                                           |
| -------------------------------------------- | -------------------------------------------------- |
| 1. 配置osid.conf文件中对应的系统，安装数据库 | 执行4条用例，执行结果符合预期，测试通过，未发现bug |

## 4.2 功能测试-不配置osid.conf文件中对应的系统，直接安装数据库

| 测试步骤                                          | 测试结果                                           |
| ------------------------------------------------- | -------------------------------------------------- |
| 1.不配置osid.conf文件中对应的系统，直接安装数据库 | 执行4条用例，执行结果符合预期，测试通过，未发现bug |

## 4.3 功能测试-osid.conf文件中配置多个OS信息，安装数据库

| 测试步骤                                    | 测试结果                                           |
| ------------------------------------------- | -------------------------------------------------- |
| 1.osid.conf文件中配置多个OS信息，安装数据库 | 执行1条用例，执行结果符合预期，测试通过，未发现bug |

## 4.4 功能测试-osid.conf文件中配置不支持的系统比如bcLinux，直接安装数据库

| 测试步骤                                                     | 测试结果                                           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 1.osid.conf文件中配置不支持的系统比如bcLinux，直接安装数据库 | 执行1条用例，执行结果符合预期，测试通过，未发现bug |

## 4.5 资料测试

| 测试步骤                    | 测试结果                              |
| --------------------------- | ------------------------------------- |
| 1. 查看资料的一致性和可读性 | 执行结果符合预期，测试通过，未发现bug |

## 4.6 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果           | 发现问题单数 |
| ------------------------------ | ---------- | ---------------------- | ------------ |
| openGauss-5.1.0 build fb44126a | 10         | Passed:10<br/>Failed:0 | 0            |

*数据项说明：*

- 累计发现缺陷单0个
- 缺陷密度：0(缺陷个数)0.423/kloc(代码行数)=0(个/kloc)



## 4.7 后续测试建议

尝试在其他Linux系统上安装openGauss数据库

