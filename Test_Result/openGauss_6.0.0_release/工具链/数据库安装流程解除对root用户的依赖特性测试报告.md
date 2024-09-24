![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期 | 修订版本 | 修改描述 | 作者 |
| ---- | -------- | -------- | ---- |
| 2024.9.13 |v1.0 | 测试报告初稿 | nanyang |

**Keywords 关键词**：安装部署，普通用户安装部署，解除对root用户依赖

**Abstract 摘要**：本文档是对openGauss数据库安装流程解除对root依赖功能进行的测试。主要测试普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，包含功能、兼容性和资料测试等。

1.已实现去除ssh映射关系对root的依赖，普通用户预安装能够建立互信，可进行安装部署、升级、回滚、扩容缩容、卸载等操作。

2.文件句柄数、系统信号量满足的情况，可以实现普通用户的（带CM、不带CM）安装部署、升级、回滚、扩容缩容、卸载等操作。

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|  NA    |          |          |

***


# 1 概述

该测试需求的目的是对om工具进行优化，最大限度解除数据库安装流程对root的依赖。在预安装的流程中，去除ssh映射对root的依赖，并加入前置check操作，判断各资源（句柄数、信号量）的情况，实现普通用户就可以执行安装部署、升级、回滚、扩容、缩容、卸载等操作。
# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

*本节描述每轮被测对象的版本信息（若使用了补丁，补丁版本号不能遗漏)；描述测试的时间、地点和测试人员。建议使用以下表格说明，可自行增减表中字段*

| 版本名称              | 软件包名称                                  | 测试起始时间   | 测试结束时间  | 测试人员  |
| ---------------------| -------------------------------------------| ------------ | ------------ | --------- |
| openGauss 6.0.0 B027 | openGauss-All-6.0.0-CentOS7-x86_64.tar.gz | 2024.8.16    | 2024.8.22    | nanyang |
|  | openGauss-All-6.0.0-openEuler20.03-aarch64.tar.gz |  |  |  |
|  | openGauss-All-6.0.0-openEuler20.03-x86_64.tar.gz |  |  |  |
|  | openGauss-All-6.0.0-openEuler22.03-aarch64.tar.gz |  |  |  |
|  | openGauss-All-6.0.0-openEuler22.03-x86_64.tar.gz |  |  |  |
| openGauss 6.0.0 B028 | openGauss-All-6.0.0-CentOS7-x86_64.tar.gz | 2024.8.22    | 2024.9.2     | nanyang |
|  | openGauss-All-6.0.0-openEuler20.03-aarch64.tar.gz |              |              |  |
|  | openGauss-All-6.0.0-openEuler20.03-x86_64.tar.gz |  |  |  |
|  | openGauss-All-6.0.0-openEuler22.03-aarch64.tar.gz |  |  |  |
|  | openGauss-All-6.0.0-openEuler22.03-x86_64.tar.gz |  |  |  |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号 | 硬件配置信息                                                 | 备注 |
| -------- | --------- | ------------------------------------------------------------ | ---- |
| 虚拟机    | x86_CentOS| CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：16G<br />硬盘：40G<br />OS：CentOS Linux 7 (Core)<br /> |      |
| 虚拟机 | x86_openEuler20.03 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：16G<br />硬盘：40G<br />OS：openEuler release 20.03 (LTS-SP4)<br /> | |
| 虚拟机 | x86_openEuler22.03 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：16G<br />硬盘：40G<br />OS：openEuler release 22.03 (LTS-SP4)<br /> | |
| 虚拟机 | aarch64_openEuler20.03 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：32G<br />硬盘：100G<br />OS：openEuler release 20.03 (LTS-SP4)<br /> | |
| 虚拟机 | aarch64_openEuler22.03 | CPU：Intel(R) Xeon(R) Gold 6138 CPU @ 2.20GHz<br />内存：32G<br />硬盘：100G<br />OS：openEuler release 22.03 (LTS-SP4)<br /> | |


# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

openGauss数据库安装流程解除对root用户依赖功能特性，共计执行28个用例，主要覆盖arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7操作系统下普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，覆盖原有的root用户下安装流程，包含功能、兼容性和资料测试等。发现问题8个，回归通过6个，2个问题正在解决，整体质量良好。

## 3.2 约束说明

1.文件句柄数不满足最小值64w的情况下，带CM的数据库无法安装，只能报错提示用户，仍需要依赖root权限设置文件句柄数。

2.系统可用信号量不足时，数据库无法安装成功，仍依赖root进行信号量参数设置。

3.gs_postuninstall仍需要root用户权限执行。

4.自定义定时任务保活ssh进程未实现，如果服务器发生重启等情况，互信会丢失，需要手动执行Python3 /（数据库安装目录）/tool/script/local/CheckSshAgent.py脚本重新建立互信。

5.普通用户安装的数据库，gs_guc不支持-N，-I 参数

## 3.3 关键风险和规避措施

无

# 4 版本详细测试结论


## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性 | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等 | 测试整体覆盖情况 | 特性质量评估               | 主要风险               |
| ---- | ------------------------------------------------------------ | -------------------------- | -------------------- | ---------------- | -------------------------- | ---------------------- |
| 数据库安装流程解除对root用户的依赖 |用户可以通过普通用户进行数据库安装部署，减少对root依赖，便于现场维护人员在没有root权限下安装部署、处理问题等 |详见3.2章节描述                        | 无                  | 功能性测试               | <font color=green>▮</font> |  |

*特性质量评估说明*：

<font color=red><font color=red>●</font></font>： *表示特性不稳定，风险高*

<font color=yellow><font color=yellow>▲</font></font>： *表示特性基本可用，遗留少量问题*

<font color=green>▮</font>： *表示特性质量良好*

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

无

###  4.2.2 可靠性测试结论

无
###  4.2.3 安全&隐私保护测试结论

无

### 4.2.4 可服务性测试结论

无

### 4.2.5 生命周期管理测试结论

无

### 4.2.6 韧性测试结论

无

###  4.2.7 兼容性测试结论

分别对arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7操作系统下普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作进行测试，验收通过。

| 操作系统           | 测试内容                                                     | 测试结果 |
| ------------------ | ------------------------------------------------------------ | -------- |
| arm openEuler20.03 | 普通用户下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载 | 测试通过 |
| arm openEuler22.03 | 普通用户下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载 | 测试通过 |
| x86 openEuler20.03 | 普通用户下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载 | 测试通过 |
| x86 openEuler22.03 | 普通用户下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载 | 测试通过 |
| x86 CentOS7        | 普通用户下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载 | 测试通过 |

###  4.2.8 升级测试结论

验证5.0.3版本升级至6.0.0版本，测试通过。

| 测试步骤                                                     | 升级路径                 | 测试结果 |
| ------------------------------------------------------------ | ------------------------ | -------- |
| 使用root用户安装5.0.3版本数据库，然后使用普通用户升级至6.0.0版本 | 5.0.3版本升级至6.0.0版本 | 测试通过 |

## 4.3 资料测试结论

社区已修改普通用户安装部署相关流程，对应数据库安装流程解除对root依赖页面中增加特性的描述，与设计方案描述一致，测试通过。

| 序号 | 测试章节                                                     | 测试结论 |
| ---- | ------------------------------------------------------------ | -------- |
| 1    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[关于openGauss](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/AboutopenGauss/关于openGauss.html)[特性描述](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/AboutopenGauss/CharacteristicDescription.html)[工具链](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/AboutopenGauss/工具链.html)数据库安装流程解除对root用户的依赖 | 测试通过 |
| 2    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)前提操作    | 测试通过 |
| 3    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[安装指南](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/InstallationGuide/InstallationGuide.html)[企业版安装](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/InstallationGuide/企业版安装.html)[安装openGauss](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/InstallationGuide/安装openGauss.html)执行安装 | 测试通过 |
| 4    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[工具与命令参考](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/ToolandCommandReference.html)[服务端工具](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/服务端工具.html)gs_checkos | 测试通过 |
| 5    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[工具与命令参考](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/ToolandCommandReference.html)[系统内部命令](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/系统内部命令.html)gs_expansion | 测试通过 |
| 6    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[工具与命令参考](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/ToolandCommandReference.html)[服务端工具](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/服务端工具.html)gs_guc | 测试通过 |
| 7    | [文档地图](https://docs-opengauss.osinfra.cn/zh/)[工具与命令参考](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/ToolandCommandReference.html)[系统内部命令](https://docs-opengauss.osinfra.cn/zh/docs/latest/docs/ToolandCommandReference/系统内部命令.html)gs_postuninstall | 测试通过 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

本次覆盖arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7操作系统下普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，重点关注普通用户建立互信情况，以及不同句柄数、信号量资源情况下的安装部署现象，并覆盖原有的root用户下安装流程，包含功能、兼容性和资料测试等。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 8        | 0    | 3    | 5    | 0      |
| 百分比 | 100%     | 0%   | 37%  | 63%  | 0%     |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述 | 问题级别 | 当前状态 |
| -------- | -------- | -------- | -------- |
|   IAKFRG | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户（非root）安装数据库时报错Installation environment dose not meet the desired result. | 次要 | 已验收 |
| IAL2DZ | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户（非root）安装数据库后，执行gs_om -t start报错，无法启动数据库 | 主要 | 已验收 |
| IAL6GX | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户（非root）安装数据库后，执行扩容操作报错，无法扩容数据库 | 次要 | 已验收 |
| IAL8PX | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户（非root）安装一主两备带CM数据库，install报错，安装失败 | 主要 | 已验收 |
| IALAXQ | 【测试类型：功能测试】【测试版本：6.0.0】配置文件句柄不满足最小值64w时，使用普通用户（非root）安装带CM数据库，未在预安装时报错退出并给出提示信息，安装时直接失败 | 次要 | 已验收 |
| IALQS1 | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户（非root）安装、卸载数据库，然后切换root用户执行gs_postuninstall清理环境，执行失败core dump | 次要 | 已完成 |
| IANGY4 | 【测试类型：功能测试】【测试版本：6.0.0】系统剩余信号量10000，使用普通用户安装一主两备数据库，安装失败，查看postgres.conf中max_connection=5000，计算的连接数也存在问题 | 主要 | 已验收 |
| IARTKV | 【测试类型：功能测试】【测试版本：6.0.0】使用普通用户gs_preinstall报错，Error: /bin/sh:行1: pssh：未找到命令 | 次要 | 已完成 |

# 6 测试过程评估

##  6.1 测试策略回顾


| 编号 | 特性      | 验证策略                                         | 是否按照测试策略执行 |
| ---- | ----     | ------------------------------------------------ | -------------------- |
| 1    | 功能测试  | 普通用户（非root）下进行数据库的预安装、安装、升级、回滚、扩容、缩容、卸载等操作，覆盖原有的root用户下安装流程，关注普通用户建立互信情况，以及不同句柄数、信号量资源情况下的安装部署现象 | YES             |
| 2 | 兼容性测试 | 覆盖arm openEuler20.03、arm openEuler22.03、x86 openEuler20.03、x86 openEuler22.03、x86 CentOS7等操作系统下普通用户安装流程 | YES |
| 3 | 资料测试 | 覆盖官网latest关于普通用户安装部署流程相关资料验证 | YES |

##  6.2 测试设计评估

无

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称             | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量 | 缺陷密度 |
| -------------------- | ---------------- | ---------- | ---------- | ---------- | ------ | -------- |
| openGauss 6.0.0 B027 | 5                | 28         | 28         | 5          | 1k     | 5        |
| openGauss 6.0.0 B028 | 7                | 28         | 28         | 3          | 1k     | 3        |

本次测试共发现8个issue，6个已修复并回归通过，2个修复中，缺陷密度为8/1k=8，整体质量良好。

###  6.3.2 测试用例执行结果统计数据

|        | 总测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | ------------ | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 28           | 28               | 21     | 7      | 0       | 0           | 100%   | 75%        |
| 第二轮 | 28           | 28               | 27     | 1      | 0       | 0           | 100%   | 96%        |

# 7 附件

##  7.1 附件1：遗留问题列表

无

##  7.2 附件2：特性相关PR

1 特性代码PR：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-OM/pulls/805

2 文档PR：

https://e.gitee.com/opengaussorg/repos/opengauss/docs/pulls/6723

3 测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=aed3951dba5149d6bc462ec824c67a00&hideDevcloudHead=true
