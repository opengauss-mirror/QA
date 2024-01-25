![avatar](../images/openGauss.png)

版权所有 © 2023  openGauss社区
您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述            | 作者 |
| ---------- | -------- | ------------------- | ---- |
| 2023-12-27 | 1.0      | 初稿                | 2JQ  |
| 2024-01-24 | 1.1      | 补充测试项和bug验收 | 张山 |
|            |          |                     |      |

 关键词： 

 Oracle，openGauss，全量迁移

摘要：

 本报告主要介绍Oracle向openGauss全量迁移的测试结果。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1     特性概述

支持Oracle数据库的常用数据类型、表类型、索引、约束、序列、视图、表空间、同义词等对象全量迁移至openGauss。

支持的Oracle数据类型为：

- 字符型：char、nchar、varchar2、nvarchar2、…
- 数值型：number、float、int、decimal、numeric、real、double、…
- 时间日期类型：timestamp、date

- 大对象型：blob、clob、nclob、long
- raw类型：raw、long raw

支持的Oracle索引类型：Btree索引

支持的Oracle约束类型：主键约束、唯一约束、外键约束、check约束

# 2     特性测试信息

本节描述被测对象的版本信息和测试的时间及测试轮次，包括依赖的硬件。

| 版本名称           | 测试起始时间 | 测试结束时间 |
| ------------------ | ------------ | ------------ |
| Opengauss5.1.1B009 | 2023-12-15   | 2023-12-22   |
| Opengauss5.1.1B009 | 2024-01-17   | 2024-01-24   |
|                    |              |              |

描述特性测试的硬件环境信息

| 环境信息 | 配置信息        | 备注                                                         |
| -------- | --------------- | ------------------------------------------------------------ |
| 硬件     | 服务器架构      | Linux  localhost 3.10.0-1127.el7.x86_64 #1 SMP Tue Mar 31 23:36:51 UTC 2020 x86_64  x86_64 x86_64 GNU/Linux |
| 硬件     | CPU             | 2核                                                          |
| 硬件     | 内存            | 7GB                                                          |
| 硬件     | 磁盘            | SDA  100GB                                                   |
| 硬件     | 网络            | 10000Mb/s                                                    |
| 软件     | Opengauss数据库 | OpenGauss-5.1.1-CentOS-64bit                                 |
| 软件     | Oracle数据库    | Oracle_11g-11.2.0.1.0-64bit                                  |

# 3     测试结论概述

## 3.1   测试整体结论

全量迁移特性，共计执行58个用例，主要覆盖了功能测试和资料测试，功能测试覆盖已支持的oracle常用数据类型、表类型、索引、约束、序列、视图、表空间、同义词等对象测试。资料测试覆盖校验资料的描述及示例的执行结果是否通过。累计发现issue4个，已验收通过，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 带有常用数据类型的表及表类型全量迁移，测试通过               |
| 功能测试   | 需求描述中支持的索引、约束、序列、同义词、视图等对象全量迁移，测试通过 |
| 易用性测试 | 对命令配置及程序启用过程的易用性进行测试，测试通过，配置项说明清晰、错误提示详尽 |
| 可靠性测试 | 对异常（如：kill进程等）场景下的全量迁移过程进行测试，异常恢复后增量迁移过程可以继续进行 |
| 性能测试   | 1张大表边写入10w条数据后进行全量迁移，全量迁移速率约10000条/s |
| 资料测试   | 校验资料的描述及示例的执行结果是否通过，测试通过             |
| 稳定性测试 | 大数据量下，进行全量迁移，测试通过                           |

## 3.2   约束说明

1. 需要配置Oracle和openGauss数据库用户的连接权限、执行sql的权限

   oracle：

   CREATE USER test IDENTIFIED BY 'password123';

   GRANT CONNECT TO test;

   opengauss：

   create user test sysadmin password 'password123';

2. 需要将执行全量迁移的机器IP配置在openGauss数据库的白名单列表中

   gs_guc reload -Z datanode -N all -I all -h "host all all 0.0.0.0/0 sha256"
   gs_guc reload -Z datanode -N all -I all -c "password_encryption_type=1"

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

无

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 4        | 0    | 0    | 4    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

### 问题单汇总

| 序号 | issue号 | 问题级别 |                           问题简述                           | 问题状态 |
| ---- | :-----: | :------: | :----------------------------------------------------------: | :------: |
| 1    | I8PQ5P  |   次要   | 【测试类型：工具功能】【测试版本：5.1.1】 oracle对象和同义词在不同模式下迁移成功，但同义词模式（schema）不正确 |  已验收  |
| 2    | I8PPXK  |   次要   | 【测试类型：工具功能】【测试版本：5.1.1】 A库下，oracle迁移表字段oracle迁移带comment的表迁移失败 |  已验收  |
| 3    | I8PPKB  |   次要   | 【测试类型：工具功能】【测试版本：5.1.1】 oracle迁移分区表，到目标库表实际未分区为普通表 |  已验收  |
| 4    | I8OSL0  |   次要   | 【测试类型：工具功能】【测试版本：5.1.1】 oracle迁移表字段类型为numeric(5,-2)的数据时，报脏数据 |  已验收  |



# 4     测试执行

## 4.1   测试执行统计数据

| 版本名称           | 测试用例数 | 用例执行结果 | 发现问题单数 |
| ------------------ | ---------- | ------------ | ------------ |
| Opengauss5.1.1B009 | 58         | 通过：54     | 4            |
|                    |            |              |              |
|                    |            |              |              |

*数据项说明：*

- 首轮测试用例执行58个，不通过4个，缺陷问题单汇总；第二轮回归问题单执行4个用例，通过4个；
- 本需求缺陷密度为4(缺陷个数)/23.604k(代码行数)=0.17(个/kloc)。

## 4.2   后续测试建议

无

# 5     附件

文档资料：

https://gitee.com/opengauss/ora-migration-tool/blob/master/userGuid.md

https://gitee.com/opengauss/ora-migration-tool/blob/master/迁移说明.md

代码资料：
https://gitee.com/opengauss/ora-migration-tool/pulls/1
https://gitee.com/opengauss/ora-migration-tool/pulls/15
https://gitee.com/opengauss/ora-migration-tool/pulls/16
https://gitee.com/opengauss/ora-migration-tool/pulls/17
https://gitee.com/opengauss/ora-migration-tool/pulls/18



 

 
