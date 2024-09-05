![avatar](../../../images/openGauss.png)

版权所有 © 2024  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述 | 作者     |
| ---------- | ----------- | -------- | -------- |
| 2024-07-15 | 1.0         | 初稿完成 | goupanlong |


关键词： primary key，unique，check，enable/disable，validate/novalidate

 

摘要：本文档主要验证新增功能primary key,unique,check约束的启用与禁用，验证其在openGauss数据库使用时，约束是否生效，并给出测试结论。

 

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| NA     |          |          |

# 1     特性概述

本需求主要涉及openGauss数据库支持tables约束功能：

（1）主要涉及以下功能:

​	1）create table约束语法测试；

​	2）alter table约束语法测试;

​    3）约束类型测试
​	primary key enable validate；
​	primary key enable novalidate；
​	primary key disable validate；
​	primary key disable novalidate；
​	check enable validate；
​	check enable novalidate；
​	check disable validate；
​	check disable novalidate；
​	unique enable validate；
​	unique enable novalidate；
​	unique disable validate；
​	unique disable novalidate；	

# 2     特性测试信息

| 版本名称                           | 测试起始时间 | 测试结束时间 |
| ---------------------------------- | ------------ | ------------ |
| openGauss 6.0.0-RC1 build 229b15f3 | 2024-06-3    | 2024-07-03   |
|                                    |              |              |


| 环境信息 | 配置信息                                                     | 备注     |
| -------- | ------------------------------------------------------------ | -------- |
| 虚拟机   | CPU：Intel(R) Xeon(R) CPU E5-2640 v2 @ 2.00GHz<br />内存：8GB<br />硬盘：200GB<br />OS：CentOS Linux 7 (Core) | 单机部署 |

# 3     测试结论概述

## 3.1   测试整体结论

共计执行46个用例，主要覆盖了功能测试、性能测试、安全测试、资料测试，稳定性测试；发现issue2个，整体质量良好；

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   |(1)验证建表约束功能正常；<br />(2)验证更改表约束功能正常；<br />(3)验证表操作功能正常；<br />(4)验证系统表约束信息准确；<br />(5)验证异常场景，报错合理；<br />(6)验证导入导出功能是否正常；<br />(7)验证接口驱动功能是否正常<br />执行用例46个；发现2个issue。<br />测试结论：通过 |
| 安全测试 | (1)验证日志信息，查看资源统计，sql日志是否正确            |
| 稳定性测试 | (1)持续24h在虚拟机上进行表约束增删改查操作，数据库无异常。 |
| 性能测试   | (1)增加表约束后，操作表对性能无明显影响；<br />测试结论：测试通过。 |
| 资料测试   | (1)验证表约束相关资料描述是否正确。 |

## 3.2   约束说明

（1）enable/disable novalidate对primary key，unique不生效。

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2      | 0    | 0   | 2   | 0      |
| 百分比 | 100% | 0%   | 0%  | 100% | 0%    |

### 问题单汇总


# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果          | 发现问题单数 |
| ------------------------------ | ---------- | --------------------- | ------------ |
| openGauss 6.0.0 build 229b15f3 | 46      | Passed：46  Failed：0 | 2                   |

*数据项说明：*

- 累计发现缺陷单2个。

- 缺陷密度：2(缺陷个数)/0.7kloc(代码行数)=2.86(个/kloc)

  


## 4.2   后续测试建议

无

# 5     附件



源代码及文档PR
- 支持启用禁用table约束需求
  https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/5011

  



 

 
