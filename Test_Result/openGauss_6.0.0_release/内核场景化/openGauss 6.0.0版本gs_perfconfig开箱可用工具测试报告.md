![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期     | 修订版本 | 修改描述                                   | 作者    |
| -------- | -------- | ------------------------------------------ | ------- |
| 2024.3.7 | 1.0      | gs_perfrconfig开箱即用工具测试报告初稿完成 | liutong |

 关键词：性能测试、gs_perfconfig、开箱即用工具、tpmC、性能参数



摘要：

 本文对openGauss 6.0.0版本gs_perfrconfig开箱即用工具测试情况进行详细说明，并给出最终测试结论。



缩略语清单：

| 缩略语 | 英文全名                               | 中文解释                                                     |
| ------ | -------------------------------------- | ------------------------------------------------------------ |
| TPC-C  | Trade Promotion Coordination Committee | 在线事务处理（OLTP）的基准程序                               |
| tpmC   | transaction per minute                 | TPC-C的吞吐量，按有效TPC-C配置期间每分钟处理的平均交易次数测试 |

# 1     特性概述

开箱即用工具gs_perfconfig可以根据用户环境自动计算性能最优时的参数，并在用户允许的条件下对这些参数进行修改，降低用户手动调优的时间成本，从而保证用户在使用数据库性能达到很高的水平。

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称             | 测试起始时间 | 测试结束时间 |
| -------------------- | ------------ | ------------ |
| openGauss 5.1.1 RC07 | 2023-12-04   | 2023-12-07   |
| openGauss 5.1.1 RC09 | 2023-12-14   | 2023-12-21   |
| openGauss 5.1.1 RC10 | 2023-12-22   | 2023-12-27   |
| openGauss 6.0.0 RC02 | 2024-1-23    | 2024-1-25    |
| openGauss 6.0.0 RC05 | 2024-2-19    | 2024-2-21    |

硬件环境信息：

| 硬件型号                                                     | 硬件配置信息                                                 | 备注 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| ARM+openEuler 2P<br />TaiShan 200 (Model 2280 7260)          | CPU：Kunpeng-920 2p 128核<br />内存：768G<br />硬盘：NVME 3.7T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| ARM+openEuler 4P<br />TaiShan 200 (Model 2480 7260)          | CPU：Kunpeng-920 4p 256核<br />内存：1T<br />硬盘：NVME 3T * 4<br />OS：openEuler release 20.03 (LTS)<br />文件系统：XFS<br />网卡：4*25GE Hi1822 |      |
| 物理机加压端：<br />ARM+openEuler 2P<br />TaiShan 200 (Model 2280 7260) | CPU：Kunpeng-920 2p 128核<br />内存：256G<br />OS：openEuler release 20.03 (LTS)<br />网卡：4*25GE Hi1822 **(与数据库端在同一个交换机)** |      |
| 虚拟机：<br />centOS7.6 x86_64环境<br />Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz | CPU：Intel 8核<br />内存：32G<br />硬盘：200G<br />OS：centOS7.6.1810<br />文件系统：ext4<br /> |      |
| 虚拟机加压端：<br />centOS7.6 x86_64环境<br />Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz | CPU：Intel 8核<br />内存：32G<br />硬盘：200G<br />OS：centOS7.6.1810<br />文件系统：ext4<br /> |      |

OS版本说明如下：

| 操作系统  | OS版本      | 版本说明                                                     |
| --------- | ----------- | ------------------------------------------------------------ |
| OpenEuler | 20.03 (LTS) | openEuler 20.03 (LTS)，aarch版本ISO:<br />SHA256:419592be9cba55a2b800e761d865550f28133875920e7bb9c2d5cdaad90a9cbf |

# 3     测试结论概述

## 3.1   测试整体结论

性能特性共计执行7个用例，主要覆盖了gs_perfrconfig开箱即用工具的基础功能测试和tpcc性能测试、共发现8个问题单，遗留0个问题，整体质量良好，性能多轮测试稳定。

| 测试活动                                   | 活动评价                                                     |
| ------------------------------------------ | ------------------------------------------------------------ |
| om安装数据库时调用--enable-perf-config参数 | 预安装命令添加--enable-perf-config参数之后会自动调整系统层面的参数，安装命令添加--enable-perf-config参数之后会自动调整数据库层面的参数，并在此过程中生成修改建议文档，用户可以查看哪些参数被修改。 |
| gs_perfconfig工具help功能测试              | gs_perfconfig工具help功能验收，通过python3 gs_perfconfig help相关命令调用gs_perfconfig工具的help功能，提示内容可以覆盖到所有可用命令与命令对应的含义，说明功能可以正常使用。 |
| gs_perfconfig工具单项tune功能测试          | gs_perfconfig工具tune功能验收，通过python3 gs_perfconfig tune相关命令调用gs_perfconfig工具的单项tune功能，通过以上方式可以调用gs_perfconfig的tune功能并生成修改建议文档：os可以给出操作系统、环境配置调整建议，setup可以给出数据库安装调整建议，guc可以给出数据库guc参数调整建议，suggest可以给出其他建议项。<br />调用命令时添加--apply参数代表用户允许工具将提示的对应参数直接应用，将调用命令前后的参数值进行比对，发现这些参数的值成功修改，修改后的值和生成的修改建议文档保持一致，说明功能可以正常使用。 |
| gs_perfconfig工具混合tune功能测试          | gs_perfconfig工具tune功能验收，通过python3 gs_perfconfig tune相关命令调用gs_perfconfig工具的tune功能，通过以上方式可以调用gs_perfconfig的tune功能并生成修改建议文档，同时获取多方面的参数调整建议时，各方面参数不重复，不遗漏。<br />调用命令时添加--apply参数代表用户允许工具将提示的对应参数直接应用，将调用命令前后的参数值进行比对，发现这些参数的值成功修改，修改后的值和生成的修改建议文档保持一致，说明功能可以正常使用。 |
| gs_perfconfig工具混合preset功能测试        | 通过python3 gs_perfconfig preset相关命令调用gs_perfconfig工具的preset功能，其中无参数时默认打印添加预设场景的方法以及当前所有的预设场景名称，在指令后添加场景名时，可以打印场景名对应的预设场景，说明功能可以正常使用。 |
| gs_perfconfig工具混合recover功能测试       | 记录当前系统参数，先执行tune命令并添加--apply参数修改参数，通过python3 gs_perfconfig recover相关命令可以调用gs_perfconfig的recover功能，将调用命令后的参数值与执行tune命令之前的参数记录进行比对，查看os、setup、guc、suggest的相关参数的值和最开始的记录保持一致，说明功能可以正常使用。 |
| gs_perfconfig工具tpcc性能测试              | 通过gs_perfconfig工具tune功能获取tp性能优先场景下的修改建议并执行，执行后进行tpcc性能测试。使用benchmarksql工具，生成1000仓库的测试数据，在不同环境不同并发下进行1H的单节点压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280)的2p单机环境在696并发下进行1H的单节点压力测试，tpmC 154.0w，达到标准150w。<br />基于TaiShan 200 (Model 2280)的2p主备环境在345并发下进行1H的单节点压力测试，tpmC 124.8w，达到标准120w。<br />基于TaiShan 200 (Model 2480)的4p单机环境在812并发下进行1H的单节点压力测试，tpmC 241.8w，达到标准230w。<br />虚拟机在100并发下进行1H的单节点压力测试tpmC 15.6w，手动调整参数测试tpmC 15.7w，相差小于10%达到标准。<br /> |

### 

## 3.2   约束说明

（1）本报告中的各场景测试结果中的处理时间，指的是openGauss的处理时间，不包含其他第三方业务的处理时间；

（2）本次性能测试是在实验室网络环境下，搭建测试环境并进行的性能测试，网络条件较稳定，不考虑现网与实验室的网络条件差异；

（3）本报告中，使用benchmarkSQL工具、sysbench工具模拟压力场景，代表的仅是已知业务模型，没有特殊业务场景，不能代表在现网复杂场景下的实际处理能力；

（4）本次测试是在固定的网络、物料、软件版本等配套的前提下进行的，如果实际环境与本次测试存在不同，请自行评估性能差异，本次测试环境具体情况，请参考第二章节。

## 3.3   问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 8        | 0    | 0    | 8    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号 | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------- | -------- | ------------------------------------------------------------ | -------- |
| 1    | I8LF92  | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】 gs_perfconfig的tune功能无法正常使用 | 已验收   |
| 2    | I8MJNM  | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】 gs_perfconfig的tune功能报错，DiskDeviceInfo缺少属性值 | 已验收   |
| 3    | I8MK76  | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】 gs_perfconfig的tune功能报错，虚拟机只有1numa导致获取cpulist出现问题 | 已验收   |
| 4    | I8OH74  | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】 om安装数据库时添加--enable-perf-config参数，没有执行gs_perfconfig | 已验收   |
| 5    | I8OIQG  | 次要     | 【测试类型：工具功能】【测试版本：5.1.1】 4p环境执行gs_perfconfig，根据修改建议对参数进行修改，性能不达标 | 已验收   |
| 6    | I8PBFP  | 次要     | 【测试版本：5.1.1】 2p主备环境执行gs_perfconfig，根据修改建议对参数进行修改，性能不达标 | 已验收   |
| 7    | I8RT76  | 次要     | 【测试版本：5.1.1】 4p环境执行gs_perfconfig出现报错TypeError: object of type 'function' has no len() | 已验收   |
| 8    | I8YBKR  | 次要     | 【测试类型：工具功能】【测试版本：6.0.0】 2p主备环境执行gs_perfconfig，根据修改建议绑定网卡中断号，但是失败 | 已验收   |

# 4     测试执行

## 4.1   测试结果

### 4.1.1 使用om安装工具添加--enable-perf-config参数

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、通过om安装数据库，添加--enable-perf-config参数<br/>调用gs_preinstall命令：<br/>gs_preinstall -U USER -G GROUP -X XMLFILE --enable-perf-config<br/>调用gs_install命令：<br/>gs_install -X XMLFILE --enable-perf-config<br/> | 预安装命令添加--enable-perf-config参数之后会自动调整系统层面的参数，并且生成修改建议文档，安装命令添加--enable-perf-config参数之后会自动调整数据库层面的参数，并在此过程中生成修改建议文档。 |

### 4.1.2 调用gs_perfconfig工具的help功能

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 通过python3 gs_perfconfig 命令调用gs_perfconfig工具的help功能<br/>调用help功能的方式：<br/>python3 gs_perfconfig help<br/>python3 gs_perfconfig -h<br/>python3 gs_perfconfig --help<br/>python3 gs_perfconfig -? | 通过以上方式均可以调用gs_perfconfig的help功能，提示内容可以覆盖到所有可用命令与命令对应的含义，说明功能可以正常使用。 |

### 4.1.3 调用gs_perfconfig工具的tune功能获得单方面的修改建议

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能<br/>调用的tune功能以及参数：<br/>python3 gs_perfconfig tune -t os<br/>python3 gs_perfconfig tune -t setup<br/>python3 gs_perfconfig tune -t guc<br/>python3 gs_perfconfig tune -t suggest<br/><br/>2、记录step1中提到的相关参数并查询参数的值，通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能，并查询相关参数的值<br/>调用的tune功能以及参数：<br/>python3 gs_perfconfig tune -t os --apply<br/>python3 gs_perfconfig tune -t setup --apply<br/>python3 gs_perfconfig tune -t guc --apply<br/>python3 gs_perfconfig tune -t suggest --apply | 1、通过以上方式均可以调用gs_perfconfig的tune功能，os可以给出操作系统、环境配置调整建议；setup可以给出数据库安装调整建议，guc可以给出数据库guc参数调整建议，suggest可以给出其他建议项。<br/><br/>2、添加--apply参数代表用户允许工具将提示的对应参数直接应用，将调用命令前后的参数值进行比对，发现这些参数的值成功修改，修改后的值和提示保持一致，说明功能可以正常使用。 |

### 4.1.4 调用gs_perfconfig工具的tune功能获得多方面的修改建议

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能<br/>调用的tune功能以及参数：<br/>python3 gs_perfconfig tune -t os,guc<br/>python3 gs_perfconfig tune -t os,guc,sysbench<br/>python3 gs_perfconfig tune -t os,guc,sysbench,suggest<br/>python3 gs_perfconfig tune -t all<br/><br/>2、记录step1中提到的相关参数并查询参数的值，通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能，并查询相关参数的值<br/>调用的tune功能以及参数：<br/>python3 gs_perfconfig tune -t os,guc --apply<br/>python3 gs_perfconfig tune -t os,guc,sysbench --apply<br/>python3 gs_perfconfig tune -t os,guc,sysbench,suggest --apply<br/>python3 gs_perfconfig tune -t all --apply | 1、通过以上方式均可以调用gs_perfconfig的tune功能，同时获取多方面的参数调整建议时，各方面参数不重复，不遗漏。<br/>2、添加--apply参数代表用户允许工具将提示的对应参数直接应用，将调用命令前后的参数值进行比对，发现这些参数的值成功修改，修改后的值和提示保持一致，说明功能可以正常使用。 |

### 4.1.5 调用gs_perfconfig工具的preset功能

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 通过python3 gs_perfconfig 命令调用gs_perfconfig工具的preset功能，包括：<br/>python3 gs_perfconfig preset<br/>python3 gs_perfconfig preset --help<br/>python3 gs_perfconfig preset name | 通过python3 gs_perfconfig 命令调用gs_perfconfig工具的preset功能，其中无参数时默认打印添加预设场景的方法以及当前所有的预设场景名称；添加--help参数时打印相关说明，包括应用指定场景，添加预设场景等；在指令后添加场景名时，可以打印场景名对应的预设场景，说明功能可以正常使用。 |

### 4.1.6 调用gs_perfconfig工具的recover功能

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、记录当前系统参数，通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能修改参数<br/>python3 gs_perfconfig tune -t all --env envfile --apply -y<br/><br/>2、通过python3 gs_perfconfig 命令调用gs_perfconfig工具的recover功能<br/>python3 gs_perfconfig recover -y | 将调用命令后的参数值与step1中执行tune命令之前的参数记录进行比对，查看os、setup、guc、suggest的相关参数的值，和最开始的记录保持一致，说明功能可以正常使用。 |

### 4.1.7 通过gs_perfconfig工具进行参数调优并测试性能

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1、通过python3 gs_perfconfig 命令调用gs_perfconfig工具的tune功能，输入tpcc测试相关的参数，包括<br/>python3 gs_perfconfig tune all --apply -y<br/>2、使用benchmarkSQL工具测试数据库的tpcc性能 | 通过gs_perfconfig工具tune功能获取tp性能优先场景下的修改建议并执行，执行后进行tpcc性能测试。使用benchmarksql工具，生成1000仓库的测试数据，在不同环境不同并发下进行1H的单节点压力测试，测试过程符合指标规定的要求，多轮测试下测试结果稳定。<br />基于TaiShan 200 (Model 2280)的2p单机环境在696并发下进行1H的单节点压力测试，tpmC 154.0w，达到标准150w。<br />基于TaiShan 200 (Model 2280)的2p主备环境在345并发下进行1H的单节点压力测试，tpmC 124.8w，达到标准120w。<br />基于TaiShan 200 (Model 2480)的4p单机环境在812并发下进行1H的单节点压力测试，tpmC 241.8w，达到标准230w。<br />基于Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz的虚拟机环境在100仓100并发下进行30min的单节点压力测试，tpmC 15.6w，人工调优参数tpmC 15.7w，相差小于10%，达到标准。 |

###  4.1.8 资料测试

| 测试步骤                | 测试结果                   |
| ----------------------- | -------------------------- |
| 1. 文档描述 2. 文档示例 | 文档描述准确，示例正确无误 |

## 4.2   测试执行统计数据

| 版本名称             | 测试用例数 | 用例执行结果           | 发现问题单数 |
| -------------------- | ---------- | ---------------------- | ------------ |
| openGauss 5.1.1 RC07 | 7          | Passed:2<br />Failed:5 | 3            |
| openGauss 5.1.1 RC09 | 7          | Passed:2<br />Failed:5 | 3            |
| openGauss 5.1.1 RC10 | 7          | Passed:4<br />Failed:3 | 1            |
| openGauss 6.0.0 RC02 | 7          | Passed:6<br />Failed:1 | 1            |
| openGauss 6.0.0 RC05 | 7          | Passed:7<br />Failed:0 | 0            |

本需求缺陷密度为8(缺陷个数)/6.900k(代码行数)=1.159(个/kloc)。

## 4.3   后续测试建议

无

# 5     附件

*无*

 



 

 
