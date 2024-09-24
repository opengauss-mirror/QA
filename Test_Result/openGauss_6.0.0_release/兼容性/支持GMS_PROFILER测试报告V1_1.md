![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者     |
| ---------- | ----------- | -------- | -------- |
| 2024-07-15 | 1.0         | 初稿完成 | goupanlong |


关键词： GMS_PROFILER

 

摘要：本文档主要验证插件GMS_PROFILER的功能，测试其函数接口在存储过程中的使用及信息收集的准确性，确保其在openGauss数据库的正确使用，并给出测试结论。

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

本需求主要涉及openGauss数据库支持GMS_PROFILER高级包功能：

（1）主要涉及以下功能:

​	1）支持START_PROFILER，STOP_PROFILER，PAUSE_PROFILER，RESUME_PROFILER，FLUSH_DATA函数；

​	2）系统表信息准确，plsql_profiler_runs，plsql_profiler_units，plsql_profiler_data；

​	3）用于在存储过程中的信息收集；

# 2     特性测试信息

| 版本名称                           | 测试起始时间 | 测试结束时间 |
| ---------------------------------- | ------------ | ------------ |
| openGauss 6.0.0-RC1 build 229b15f3 | 2024-06-03   | 2024-07-10   |
|                                    |              |              |


| 环境信息 | 配置信息                                                     | 备注     |
| -------- | ------------------------------------------------------------ | -------- |
| 虚拟机   | CPU：Intel(R) Xeon(R) CPU E5-2640 v2 @ 2.00GHz<br />内存：7GB<br />硬盘：300GB<br />OS：CentOS Linux 7 (Core) | 单机部署 |

# 3     测试结论概述

## 3.1   测试整体结论

共计执行32个用例，主要覆盖了功能测试、资料测试，发现issue1个，整体质量良好；

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | (1)验证函数入参是否准确;<br />(2)验证函数功能是否正常；<br />(3)验证在存储过程中收集信息是否正确；<br />(4)验证函数组合使用功能正常；<br />(5)验证在其他高级包中的使用是否正常；<br />(6)验证异常值，错误入参等异常场景，报错准确<br />(7)验证在动态语句/begin等场景使用是否正常<br />(8)验证其他耦合场景使用是否正常<br />执行用例32个；发现1个issue。<br />测试结论：通过 |
| 资料测试 | 验证gms_profiler资料描述正确。                               |

## 3.2   约束说明

- 仅支持Create extension命令方式加载插件。
- 不支持存储过程中存在异常处理的场景，会导致收集信息不准确。
- 如果测试过程调用了flush_data接口，不支持其后调用ROOLBACK操作，会报错。如需使用ROOLBACK, 建议统一通过stop_profiler接口完成收集信息写表。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1      | 0    | 0   | 1   | 0      |
| 百分比 | 100% | 0%   | 0%  | 100% | 0%    |

### 问题单汇总


# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果          | 发现问题单数 |
| ------------------------------ | ---------- | --------------------- | ------------ |
| openGauss 6.0.0 build 229b15f3 | 32       | Passed：32 Failed：0 | 1                   |

*数据项说明：*

- 累计发现缺陷单1个。

- 缺陷密度：1(缺陷个数)/1.5kloc(代码行数)=0.67(个/kloc)


## 4.2   后续测试建议

无

# 5     附件



源代码及文档PR
- 支持gms_profiler包
  https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/4993
- gms_profiler extension文档
  https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ExtensionReference/gms_profiler-Extension.html



  



 

 
