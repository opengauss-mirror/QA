![avatar](../../images/openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订版本 | 修改描述         | 作者  |
| ---------- | -------- | ---------------- | ----- |
| 2022/08/12 | 1.0      | 特性报告初稿完成 | lixin |
| 2022/08/30 | 1.1      | 根据检视意见修改 | lixin |
| 2022/08/31 | 1.2      | 根据检视意见修改 | lixin |

关键词： `assessment`、`dolphin`、`whale`、`MySQL`、`O*`、兼容性、兼容性评估工具

摘要：使用兼容性评估工具，可串联起数据库和插件、用户输入，最终生成评估报告。本文档主要介绍了测试包含整个流程的测试、评估工具的测试及对评估报告的测试，并给出最终测试结论。

缩略语清单：

| 缩略语       | 英文全名     | 中文解释            |
| ------------ | ------------ | ------------------- |
| `assessment` | `assessment` | 评估兼容性插件/工具 |
| `dolphin`    | `dolphin`    | B兼容数据库插件     |
| `whale`      | `whale`      | A兼容数据库插件     |

# 1     特性概述

本工具支持利用已有的openGauss节点评估数据SQL文本在openGauss的兼容性。

兼容性评估主要分为三个部分：正常运行的有足够权限的openGauss数据库、评估插件提供数据库结构、评估工具。评估工具作用是串联起数据库和插件、用户输入，最终生成评估报告。

# 2     特性测试信息

| 版本名称                    | 测试起始时间 | 测试结束时间 |
| -------------------------- | ------------ | ------------ |
| `assessment 1.0(2fffb4d)`  | 2022-06-01   | 2022-06-08   |
| `assessment 1.0(fb65f0c)`  | 2022-06-19   | 2022-06-25   |
| `assessment 1.0(035d41b)`  | 2022-06-29   | 2022-06-30   |
| `assessment 1.0(a0095ac)`  | 2022-07-20   | 2022-07-20   |
| `assessment 1.0(6556cec)`  | 2022-08-25   | 2022-08-25   |

| 环境信息 | 配置信息                                                     | 备注 |
| -------- | ------------------------------------------------------------ | ---- |
| 虚拟机   | CPU：Intel(R) Xeon(R) Gold 6278C CPU @ 2.60GHz<br />内存：32GB<br />硬盘：1040GB<br />OS：CentOS Linux release 7.6.1810 (core)<br /> |      |

| 软件名称                      | 软件版本  | 备注 |
| ---------------------------- | -------- | ---- |
| `openGauss build c89a93b `   | 3.0.0    |      |
| `dolphin`                    | 1.0      |      |
| `whale`                      | 1.0      |      |

# 3     测试结论概述

## 3.1   测试整体结论

共设计73个用例，主要覆盖了遍历评估结果场景、异常场景、MySQL测试输入、O*测试输入、评估工具使用及对评估报告的测试；资料测试中描述恰当，示例正确无误。共计提交需求单1个、缺陷单14个，其中8个缺陷均已解决且回归通过，5个已取消，1个转需求，本轮测试无遗留问题，整体质量良好。

| 测试活动 | 活动评价                                                     |
| -------- | ------------------------------------------------------------ |
| 功能测试 | 遍历评估结果场景，包括完全兼容、语法兼容、不兼容、暂不支持评估、忽略语句，<br />测试评估结果与预期一致 |
| 功能测试 | 遍历异常场景，如脚本中无分号、多个分号、一个SQL语句中含多个错误等场景，<br />测试评估结果与预期一致 |
| 功能测试 | MySQL常见DDL、DML、DQL语句作为测试输入，测试评估结果与预期一致 |
| 功能测试 | O*常见DDL、DML、DQL语句作为测试输入，测试评估结果与预期一致 |
| 功能测试 | 测试评估工具的使用，如必选参数非空，多个参数组合等，<br />测试评估工具正确方式使用时能评估成功，不正确方式使用时回显合理 |
| 功能测试 | 测试评估报告UI显示及各字段值显示正确，与预期一致             |
| 资料测试 | 资料对该工具描述准确，示例的执行结果正确，整体质量良好       |

## 3.2   约束说明

- 仅支持SQL文本文件输入，且SQL之间以`;`分割。

- 不使用`dolphin`、`whale`等兼容性插件场景，不兼容语句的报错信息可能不准确。

- 暂不支持`#`注释，请将文本内的`#`注释替换为`--`注释或直接删除。

- 存储过程、函数语句仅支持：创建体的合法性校验和函数体的语法兼容校验。

- 对于评估结果的准确率：
  - 完全兼容：openGauss完全支持该语法。兼容结果可能依赖于传入SQL语句的前置执行结果，因此实际在openGauss内执行时不一定完全兼容。
  - 语法兼容：openGauss支持该语法，但是实际使用过程中可能包含字段类型不支持、函数不存在等问题。
  - 不兼容：openGauss不支持该语法。
  - 暂不支持评估：未考虑的语句。
  - 忽略语句：包含`/* */`或`--`的语句。

对于A兼容性数据库，在做SQL语句导出时，最好提前做如下设置：

```sql
EXECUTE DBMS_METADATA.SET_TRANSFORM_PARAM(DBMS_METADATA.SESSION_TRANSFORM,'SEGMENT_ATTRIBUTES',false);
EXECUTE DBMS_METADATA.SET_TRANSFORM_PARAM(DBMS_METADATA.SESSION_TRANSFORM,'SQLTERMINATOR',true);
EXECUTE DBMS_METADATA.SET_TRANSFORM_PARAM(DBMS_METADATA.SESSION_TRANSFORM,'STORAGE',false);
EXECUTE DBMS_METADATA.SET_TRANSFORM_PARAM(DBMS_METADATA.SESSION_TRANSFORM,'TABLESPACE',false);
```

## 3.3   遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| NA       |          |          |                    |          |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 15       | 2    | 2    | 10   | 1      |
| 百分比 |          | 13%  | 13%  | 67%  | 7%     |

### 3.3.3 问题单汇总

|序号| issue号                                                      | 级别 | 问题简述                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | -------------------------------------------- | ------ |
| 1    | [I5D6GP](https://gitee.com/opengauss/Plugin/issues/I5D6GP) | 严重 | 编译插件whale报错error: ‘uint64 CopyFrom(CopyState)’<br /> was declared ‘extern’ and later ‘static’ [-fpermissive] | 已验收 |
| 2 | [I5BMK1](https://gitee.com/opengauss/Plugin/issues/I5BMK1?from=project-issue) | 严重 | assessment评估远程已创建的数据库（未使用dolphin）产生core | 已验收 |
| 3 | [I5B6RZ](https://gitee.com/opengauss/Plugin/issues/I5B6RZ?from=project-issue) | 主要 | assessment评估时，无法识别同一行的多个语句 | 已验收 |
| 4 | [I5B3YY](https://gitee.com/opengauss/Plugin/issues/I5B3YY?from=project-issue) | 主要 | 输入的sql文本文件中包含#注释时，被作为普通语句处理，<br />与其后兼容的sql语句合并成一条处理，<br />导致兼容的SQL评估结果为不兼容 | 已验收 |
| 5 | [I5E483](https://gitee.com/opengauss/Plugin/issues/I5E483?from=project-issue) | 次要 | assessment评估执行语句同时指定-d、-c,不应评估成功 | 已验收 |
| 6 | [I5E46Z](https://gitee.com/opengauss/Plugin/issues/I5E46Z) | 次要 | assessment评估执行语句指定-p为空或端口号错误时，<br />报错信息不正确 | 已验收 |
| 7 | [I5B44E](https://gitee.com/opengauss/Plugin/issues/I5B44E?from=project-issue) | 次要 | assessment评估报告显示语言不统一，部分中文，部分英文 | 已验收 |
| 8 | [I5NSN9](https://gitee.com/opengauss/Plugin/issues/I5NSN9?from=project-issue) | 不重要 | 资料应按照使用场景，说明下需要集成其他插件的约束接口 | 已验收 |
| 8 | [I5E40B](https://gitee.com/opengauss/Plugin/issues/I5E40B?from=project-issue) | 次要 | assessment评估包含o*执行存储过程的excute语句块<br />时，被拆分成两个断句分别评估 | 已取消 |
| 10 | [I5E3X6](https://gitee.com/opengauss/Plugin/issues/I5E3X6?from=project-issue) | 次要 | assessment评估语句中包含from dual时，评估结果<br />不应按照表是否存在来判断，评估结果应为完全兼容 | 已取消 |
| 11 | [I5E3W0](https://gitee.com/opengauss/Plugin/issues/I5E3W0?from=project-issue) | 次要 | assessment评估语句中包含O*自定义函数，缺少/时，<br />未能识别到语句，评估结果为空 | 已取消 |
| 12 | [I5DFH7](https://gitee.com/opengauss/Plugin/issues/I5DFH7) | 次要 | assessment插件的，远程连接另一个没有插件的数据库<br />提示assessment_database: "assessment" extension is needed | 已取消 |
| 13 | [I5DFG3](https://gitee.com/opengauss/Plugin/issues/I5DFG3?from=project-issue) | 次要 | assessment工具远程连接数据库，没配置白名单，<br />执行评估命令，出现重复提示 | 已取消 |
| 14 | [I5KC6F](https://gitee.com/opengauss/Plugin/issues/I5KC6F?from=project-issue) | 次要 | 可在opengauss中执行，但评估结果为暂不支持评估的语法，<br />应在后续迭代中支持 | 需求单 |
| 15 | [I5E41F](https://gitee.com/opengauss/Plugin/issues/I5E41F?from=project-issue) | 次要 | assessment评估包含mysql存储过程的语句，结果为不兼容 | 转需求 |


# 4     测试执行

## 4.1 测试执行步骤

###  4.1.1 遍历评估结果场景

| 测试步骤                                                     | 测试结果                                                  |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| 1. 准备SQL文件，文件中SQL语句间使用`;`分隔，文件中包含的语句分别为预估评估结果为完全兼容、语法兼容、不兼容、暂不支持评估、忽略语句的语句<br />2. cd到../contrib/assessment目录下，执行命令：<br />`assessment_database -p xxx -x A -U xxx -W xxx -f xx/xx.sql -o xx/xx.html`<br />3. 查看评估结果是否与预期一致<br />4. 清理环境 | 执行16条用例，新增需求单1个发现2个bug，现已修复且验收通过 |

### 4.1.2 遍历异常场景

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. 准备SQL文件，文件中SQL语句间使用`;`分隔，文件中包含脚本中无分号、多个分号、一个SQL语句中含多个错误等场景<br />2. cd到../contrib/assessment目录下，执行命令：<br />`assessment_database -p xxx -x A -U xxx -W xxx -f xx/xx.sql -o xx/xx.html`<br />3. 查看评估结果是否与预期一致<br />4. 清理环境 | 执行12条用例，发现2个bug，现已修复且验收通过 |

### 4.1.3常见DML/ DDL/ DQL作为测试输入

| 测试步骤                                                     | 测试结果                                                |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| 1. 准备SQL文件，文件中SQL语句间使用`;`分隔，文件中包含常见DML/ DDL/ DQL语句<br />2. cd到../contrib/assessment目录下，执行命令：<br />`assessment_database -p xxx -x A -U xxx -W xxx -f xx/xx.sql -o xx/xx.html`<br />3. 查看评估结果是否与预期一致<br />4. 清理环境 | 执行6条用例，发现4个bug，3个判定非问题已取消，1个转需求 |

### 4.1.4 测试评估工具的使用

| 测试步骤                                                     | 测试结果                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1. 准备SQL文件，正确的SQL语法，SQL语句间使用`;`分隔<br />2. cd到../contrib/assessment目录下，校验必选参数、参数组合等方式执行命令<br />3. 测试评估工具正确方式使用时能评估成功，不正确方式使用时回显合理<br />4. 清理环境 | 执行29条用例，发现5个bug，<br />3个已修复且验收通过，2个判定非问题已取消 |

### 4.1.5 测试评估报告

| 测试步骤                                                     | 测试结果                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| 1. 准备SQL文件，文件中SQL语句间使用`;`分隔<br />2. cd到../contrib/assessment目录下，执行命令：<br />`assessment_database -p xxx -x A -U xxx -W xxx -f xx/xx.sql -o xx/xx.html`<br />3. 查看评估结果报告，测试评估报告UI显示及各字段值显示正确，与预期是否一致<br />4. 清理环境 | 执行10条用例，发现1个bug，现已修复且验收通过 |

评估报告样式如下图：
![report](../../images/assessment评估报告.png)

## 4.2  测试执行统计数据

| 版本名称         | 测试用例数 | 用例执行结果            | 发现问题单数 |
| ---------------- | ---------- | ----------------------- | ------------ |
| `assessment 1.0` | 73         | Passed：73<br>Failed：0 | 15           |

*数据项说明：*

* 累计发现需求单1个、缺陷单14个，其中8个缺陷均已解决且回归通过，5个已取消，1个转需求。
* 失败用例已在后续问题修复后，回归issue执行通过。
* 缺陷密度为 8(缺陷个数)/1.869k(代码行数)=4.28(个/kloc)。

## 4.3   后续测试建议

当前版本为assessment兼容性评估工具最初版本，覆盖SQL语句并不全面，已有需求单两个，后续测试时可继续尝试不同SQL语句作为输入，测试评估工具是否支持，以促进该工具的迭代完善。

# **5     附件**

无


****