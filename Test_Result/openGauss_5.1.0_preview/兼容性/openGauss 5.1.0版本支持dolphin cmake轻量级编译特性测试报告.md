![avatar](../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期 | 修订   版本 | 修改描述 | 作者 |
| ---- | ----------- | -------- | ---- |
| 2023-09-20 | 1.0 | 报告初稿 | xue.xiao |

 关键词： dolphin插件，cmake，编译安装

摘要：本文档为openGauss利用build/script/cmake_package_mini.sh脚本构建openGauss轻量版安装包, 并在脚本中实现dolphin插件基于cmake的轻量版安装包编译的测试报告。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA |          |          |

# 1     特性概述

opengauss使用build在/script/cmake_package_mini.sh脚本构建openGauss轻量版安装包, 并在脚本中实现dolphin插件基于cmake的轻量版安装包编译。

# 2     特性测试信息

| 版本名称 | 测试起始时间 | 测试结束时间 |
| -------- | ------------ | ------------ |
| openGauss 5.1.0 build 90e6ef7e | 2023-09-19 | 2022-09-20 |

描述特性测试的硬件环境信息

| 环境信息 | 配置信息 | 备注 |
| -------- | -------- | ---- |
| 虚拟机 |PRETTY_NAME：CentOS Linux 7 (Core)<br />Architecture:x86_64  CPU op-mode(s):32-bit, 64-bit<br />MEM:30GB ||

# 3     测试结论概述

## 3.1   测试整体结论

opengauss实现dolphin插件基于cmake的轻量版安装包编译，经过测试能生成dolphin.so且能正常出包，原来的测试用例（和轻量级版本无关的用例）能正常跑过,回归测试共执行361个测试用例均通过。主要覆盖了功能测试以及回归测试。功能测试包括：1. 在build/script目录，执行sh cmake_package_mini.sh -3rd ~/openGauss-third_party_binarylibs -m release 执行成功后查看是否生成dolphin.so和生成包 2.设置以下环境变量到对应位置CODE_BASE,CODE_BASE_SRC,PREFIX_HOME,GAUSSHOME,DEBUG_TYPE,THIRD_BIN_PATH 3.在tmp_build中执行make plugin_check。回归测试中利用cmake编译dolphin插件后执行数据库可稳定运行。测试中发现0个问题。

| 测试活动 | 活动评价 |
| -------- | -------- |
| 功能测试 |  在build/script目录，执行sh cmake_package_mini.sh -3rd ~/openGauss-third_party_binarylibs -m release 执行成功后查看生成dolphin.so和生成包，执行结果符合预期，通过        |
| 功能测试 |  设置以下环境变量到对应位置CODE_BASE,CODE_BASE_SRC,PREFIX_HOME,GAUSSHOME,DEBUG_TYPE,THIRD_BIN_PATH，执行结果符合预期，通过        |
| 功能测试 |  在tmp_build中执行make plugin_check，执行结果符合预期，通过        |
| 回归测试 |  利用cmake编译dolphin插件后执行数据库可稳定运行，执行结果符合预期，通过       |

## 3.2   约束说明

dolphin插件由cmake的轻量版安装包编译

## 3.3   遗留问题分

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
|          |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 0        | 0    | 0    | 0    | 0      |
| 百分比 | 0%     | 0%    | 0%  | 0%  | 0%      |

# 4     测试执行

## 4.1 功能测试

### 4.1.1 安装测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 在build/script目录，执行sh cmake_package_mini.sh -3rd ~/openGauss-third_party_binarylibs -m release 执行成功后查看生成dolphin.so和生成包<br />2. 设置以下环境变量到对应位置CODE_BASE,CODE_BASE_SRC,PREFIX_HOME,GAUSSHOME,DEBUG_TYPE,THIRD_BIN_PATH<br />3. 在tmp_build中执行make plugin_check | 执行结果符合预期，测试通过 |

### 4.1.1 回归测试

| 测试步骤 | 测试结果 |
| -------- | -------- |
| 1. 编译安装好dolphin插件<br />2. 利用编译好的插件执行dolphin中的测试用例 | 执行结果符合预期，测试通过 |

## 4.2   测试执行统计数据

*本节内容根据测试用例及实际执行情况进行特性整体测试的统计，可根据第二章的测试轮次分开进行统计说明。*

| 版本名称 | 测试用例数 | 用例执行结果 | 发现问题单数 | 问题占据百分比 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| openGauss 5.1.0 build 90e6ef7e  | 361 | Passed: 361<br />Failed: 0 | 0 | 0% |

## 4.2   后续测试建议

无

# 5     附件

无
 



 

 
