版权所有 © 2024 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期     | 修订 版本 | 修改描述 | 作者   |
| -------- | --------- | -------- | ------ |
| 2024-3-4 | V1.0      | 初稿     | 姚雨驰 |

关键词：

portal执行流程优化

摘要：

支持用户通过Datakit平台之间调用portal，进而调用MySQL迁移全流程中任意一个或多个阶段



# 1 特性概述

【工具链】portal执行流程优化（一）特性设计文档，主要实现了对MySQL迁移全流程中各阶段的工具进行解耦，并定义工具接口，使其易于扩展，可便于支持oralce迁移全套工具管理

# 2 特性测试信息

##### 功能测试

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | ------------ | ------------ |
| openGauss 6.0.0 build 9bb9a9b1 | 2024-02-07   | 2024-02-27   |
| openGauss 6.0.0 build b8fc2278 | 2024-02-28   | 2024-03-04   |
| MySQL 5.7.36                   | 2024-02-07   | 2024-03-04   |
| Datakit build time：2024-02-06 | 2024-02-07   | 2024-02-19   |
| Datakit build time：2024-02-19 | 2024-02-20   | 2024-03-04   |
| portal build time: 2024-02-16  | 2024-02-18   | 2024-02-19   |
| portal build time: 2024-02-19  | 2024-02-20   | 2024-03-04   |

硬件环境信息：

##### 功能测试

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | Architecture：aarch64 CPU：kunpeng-920 7260 2p 128核 内存：16 * 32GB 硬盘：3 * 2.0TB OS：openEuler release 20.03 (LTS-SP1) |      |

# 3 测试结论概述

## 3.1 测试整体结论

增量迁移工具能力增强，共执行用例21个，主要覆盖了功能测试及资料测试。功能测试主要覆盖Datakit启动portal进行离线、在线迁移，使用Datakit在线、离线、导入安装portal是否能正常安装以及能否卸载portal，命令行启动portal、脚本启动portal进行数据迁移。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单7个，6个缺陷已解决，回归通过，整体质量良好。

## 3.2 约束说明

1、MySQL 5.7及以上版本

2、MySQL参数设置： log_bin=ON,  binlog_format=ROW,  binlog_row_image=FULL,  gtid_mode = ON，若未设置gtid_mode = OFF，则sink端回放退化为串行回放，会降低在线迁移性能

3、增量迁移是直接透传DDL的，对于openGauss侧语法和MySQL侧语法不一样的，迁移失败

4、在java11环境上运行Datakit和portal迁移工具

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      | N/A      | N/A      | N/A                | N/A      |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 7        | 0    | 0    | 7    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   | 备注 |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ | ---- |
| 1    | [I954E6](https://e.gitee.com/opengaussorg/dashboard?issue=I954E6) | 次要 | 【迁移工具质量加固】【Datakit】导入安装portal失败，清理环境失败 | 已验收 |      |
| 2    | [I953NF](https://e.gitee.com/opengaussorg/dashboard?issue=I953NF) | 次要 | 【迁移工具质量加固】【Datakit】 Datakit离线安装portal，安装失败，重复安装3次后成功安装 | 已验收 |      |
| 3    | [I92FPI](https://e.gitee.com/opengaussorg/dashboard?issue=I92FPI) | 次要 | 【迁移工具质量加固】【portal】离线模式，全量迁移完成后，全量校验总量为0 | 修复中 |      |
| 4    | [I92TA6](https://e.gitee.com/opengaussorg/dashboard?issue=I92TA6) | 次要 | 【迁移工具质量加固】【portal】使用脚本启动增量迁移报错       | 已验收 |      |
| 5    | [I926T6](https://e.gitee.com/opengaussorg/dashboard?issue=I926T6) | 次要 | 【迁移工具质量加固】【portal】全量校验完成后，datachecker进程未正常退出 | 已验收 |      |
| 6    | [I960B2](https://e.gitee.com/opengaussorg/issues/list?issue=I960B2) | 次要 | 【迁移工具质量加固】【portal】portal启动增量迁移后，无法停止，无法开启增量校验 | 已验收 |      |
| 7    | [I96Y4P](https://e.gitee.com/opengaussorg/issues/list?issue=I96Y4P) | 次要 | 【迁移工具质量加固】【portal】脚本启动反向迁移失败           | 已验收 |      |

# 4 测试执行

## 4.1 功能测试实现步骤

1、MySQL数据库源数据库，创建表，插入数据

2、openGauss数据库创建创建datakit数据库和目标数据库

3、安装部署Datakit

4、使用Datakit在线、离线、导入安装portal工具

5、Datakit平台进行全量迁移、增量迁移、反向迁移及其校验

6、MySQL数据库执行DDL或DML操作

7、openGauss数据库查看迁移情况

### 4.1.1 功能测试-Datakit平台安装portal测试

| 测试步骤          | 测试结果                                                     |
| ----------------- | ------------------------------------------------------------ |
| 1. 在线安装portal | 执行2条用例，执行结果符合预期，测试通过，发现个bug1个，1个问题已解决回归通过 |
| 2. 离线安装portal | 执行2条用例，执行结果符合预期，测试通过，发现bug1个          |
| 3. 导入安装portal | 执行2条用例，执行结果符合预期，测试不通过，发现BUG1个        |

### 4.1.2 功能测试-Datakit平台迁移功能测试

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.离线模式，启动全量迁移及全量校验。<br />2.在线模式，启动全量迁移及校验。<br />3.在线模式，启动增量迁移。<br />4.在线模式，启动反向迁移 | 执行4条用例，执行结果符合预期，测试通过，发现1个bug，1个问题已解决回归通过 |

### 4.1.3 功能测试-命令行启动portal功能测试

测试步骤

1.  检测离线/在线模式前置条件

   ```shell
   cd portal
   sh gs_rep_portal.sh verify_pre_migration
   ```

2. 启动全量迁移/校验

   ```shell
   sh gs_rep_portal.sh start_mysql_full_migration
   sh gs_rep_portal.sh start_mysql_full_migration_datacheck
   ```

3. 启动增量迁移/校验

   ```shell
   sh gs_rep_portal.sh start_mysql_incremental_migration
   sh gs_rep_portal.sh start_mysql_incremental_migration_datacheck
   ```

4. 停止增量迁移并进行反向迁移

   ```shell
   sh gs_rep_portal.sh stop_incremental_migration
   sh gs_rep_portal.sh start_mysql_reverse_migration
   sh gs_rep_portal.sh stop_mysql_reverse_migration
   ```

5. 使用默认迁移计划/自定义迁移计划进行迁移

   ```shell
   sh gs_rep_portal.sh start_plan1 
   sh gs_rep_portal.sh start_plan2
   sh gs_rep_portal.sh start_plan3
   sh gs_rep_portal.sh start_current_plan 
   停止计划
   sh gs_rep_portal.sh stop_plan
   ```

6. 查看portal状态等相关信息

   ```shell
   sh gs_rep_portal.sh show_plans ——显示默认计划
   
   sh gs_rep_portal.sh show_information ——显示数据库相关信息
   
   sh gs_rep_portal.sh load_tools_config ——加载工具配置文件
   
   sh gs_rep_poratl.sh show_portal_status ——展示portal状态
   
   sh gs_rep_portal.sh show_task_status ——显示portal每个任务状态
   ```

   

7. 测试结果

   执行8条用例，执行结果符合预期，测试通过，发现1个bug，待修复

### 4.1.4 功能测试-脚本启动portal进行迁移

| 步骤             | 命令                                                  | 测试结果                                                     |
| ---------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| 脚本启动全量迁移 | sh gs_mysync.sh start workspace.id                    | 执行1条用例，执行结果符合预期，测试通过                      |
| 脚本启动全量校验 | sh gs_datacheck.sh start workspace.id                 | 执行1条用例，执行结果符合预期，测试通过                      |
| 脚本启动增量迁移 | sh gs_replicate.sh start mysql-opengauss workspace.id | 执行1条用例，执行结果符合预期，测试通过，发现1个bug，1个问题已解决回归通过 |
| 脚本启动增量校验 | sh gs_datacheck.sh start incremental workspace.id     | 执行1条用例，执行结果符合预期，测试通过                      |
| 脚本启动反向迁移 | sh gs_replicate.sh start opengauss-mysql workspace.id | 执行1条用例，执行结果不符合预期，测试未通过，发现1个bug，    |



## 4.2 资料测试

| 测试步骤                                        | 测试结果                                   |
| ----------------------------------------------- | ------------------------------------------ |
| 1. 官网文档<br>2. 文档示例<br />3. help界面文本 | 官网文档有两处错误，help界面文本有一处错误 |

## 4.3 测试数据统计

根据用例进行测试验证，结果如下：

| 测试版本                                                     | 测试用例数 | 用例执行结果                          | 发现问题单数 |
| ------------------------------------------------------------ | ---------- | ------------------------------------- | ------------ |
| portal build time：2024-02-19<br />Datakit build time：2024-02-19 | 21         | Passed:14<br/>Failed:4<br />Blocked:3 | 5            |
| portal build time：2024-03-04<br />Datakit build time：2024-03-04 | 21         | Passed:19<br/>Failed:2<br />Blocked:0 | 2            |

*数据项说明：*

- 累计发现缺陷单7个，4个缺陷已解决，3个已回归通过，1个待回归，3个待办
- 失败用例及阻塞用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：7(缺陷个数)11.577kloc(代码行数)=0.605(个/kloc)



## 4.5 后续测试建议

1、测试下工具性能和稳定性

# 5 附件

##  5.1 源代码及文档PR 

【迁移工具】portal 流程优化PR

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-migration-portal/pulls/115

