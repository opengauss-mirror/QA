![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期       | 修订 版本 | 修改描述 | 作者          |
| ---------- | --------- | -------- | ------------- |
| 2023.01.16 | V1.0      | 初稿     | wangzhengyuan |

关键词：

备份、恢复、构建、升级、进度条

摘要：

本文档主要介绍了内核备份、恢复、构建、升级工具的进度条打印功能，并给出测试结论。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

该特性给内核的备份、恢复工具，升级、构建工具加了显示进度条的功能，使得工具在执行备份、恢复、构建、升级的时候，在一些比较耗时的阶段能够以进度条的形式展示操作执行的进度。

# 2 特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 5.1.1 build 6034bc01 | 2023-1-4     | 2023-1-6     |
| openGauss 6.0.0 build d2533e77 | 2023-1-10    | 2023-1-15    |

硬件环境信息：

| 硬件型号 | 硬件配置信息                                                 | 备注   |
| -------- | ------------------------------------------------------------ | ------ |
| 虚拟机   | Architecture：aarch64<br/>CPU：Kunpeng-920 32核<br/>内存： 126GB<br/>硬盘：1T<br/>OS：Kylin Linux Advanced Server release V10 (Sword) | 主节点 |
| 虚拟机   | Architecture：aarch64<br/>CPU：Kunpeng-920 32核<br/>内存： 126GB<br/>硬盘：1T<br/>OS：Kylin Linux Advanced Server release V10 (Sword) | 备节点 |



# 3 测试结论概述

## 3.1 测试整体结论

内核工具打印进度条特性，共执行用例18个，主要覆盖了功能测试、资料测试。功能测试覆盖逻辑备份、恢复，物理备份、恢复，数据库主机对备机的构建以及数据库om升级操作，场景覆盖单机、主备。资料测试覆盖工具相关资料是否准确、完整。累计发现缺陷单1个，已回归通过，整体质量良好。

## 3.2 约束说明

1. GUC参数、操作约束集成内核备份、恢复、构建、升级工具，无新增约束。

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 1    | 0    | 0      |
| 百分比 | 100%     | 0%   | 100% | 0%   | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I8TMEG](https://gitee.com/opengauss/openGauss-server/issues/I8TMEG) | 主要 | gs_dump执行逻辑备份，无进度条展示                    | 已验收 |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 逻辑备份与恢复

| 测试步骤                                                     | 测试结果                |
| ------------------------------------------------------------ | ----------------------- |
| 1. 数据库预制数据执行执行逻辑备份，备份格式为文本、目录、tar及自定义格式，查看备份过程中全局扫描阶段和数据转存阶段的进度条展示情况；<br>2.根据逻辑备份的各种文件，执行备份恢复，将指定数据导入到指定的数据库中，查看数据恢复阶段的进度条展示情况。 | 执行9条用例，发现1个bug |

### 4.1.2 物理备份与恢复

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 数据库预制数据进行全量备份与增量备份，查看数据备份阶段、同步备份文件阶段以及数据验证阶段的进度条展示；<br>2.设置GUC参数enable_cbm_tracking=on执行备份恢复，查看数据验证阶段、数据恢复阶段及同步恢复文件阶段的进度条展示。 | 执行4条用例，未发现bug |

### 4.1.3 数据库主机对备机的构建

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 数据库预制数据，执行对备机的全量构建，查看主机向备机传输文件阶段的进度条展示；<br>2. 执行业务，执行主机对备机的增量构建，查看主机向备机传输文件阶段的进度条展示。 | 执行2条用例，未发现bug |

### 4.1.4 OM升级操作

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 数据库3.0.5版本预制数据，停业务，就地升级到6.0.0，查看对应阶段进度；<br>2. 数据库3.0.5版本，执行业务，灰度升级至6.0.0，查看对应阶段进度<br>3.数据库3.0.5版本，执行业务，升级至6.0.0，先指定主节点升级，再升级其余节点，查看对应阶段进度 | 执行3条用例，未发现bug |

## 4.2 资料测试

| 测试步骤    | 测试结果             |
| ----------- | -------------------- |
| 1. 文档描述 | 文档描述准确、无遗漏 |

## 4.3 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                       | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------ | ---------- | -------------------- | ------------ |
| openGauss 5.1.1 build 6034bc01 | 18         | Passed：9 Failed：9  | 1            |
| openGauss 6.0.0 build d2533e77 | 18         | Passed：18 Failed：0 | 0            |

*数据项说明：*

- 累计发现缺陷单1个，已解决且回归通过
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：1(缺陷个数)/0.994kloc(代码行数)=(1.01个/kloc)

## 4.4 后续测试建议

无

# 5 附件
## 5.1 源代码及文档PR

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4475

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4479

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4576

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-OM/pulls/621

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6009