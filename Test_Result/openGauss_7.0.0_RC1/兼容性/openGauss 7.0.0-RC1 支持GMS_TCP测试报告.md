![avatar](../../images/openGauss.png)

版权所有 © 2024  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[*https://creativecommons.org/licenses/by-sa/4.0/*](https://creativecommons.org/licenses/by-sa/4.0/) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[*https://creativecommons.org/licenses/by-sa/4.0/legalcode*](https://creativecommons.org/licenses/by-sa/4.0/legalcode)。

修订记录

| 日期       | 修订版本 | 修改描述 | 作者     |
| ---------- | -------- | -------- | -------- |
| 2025/03/04 | 1.0      | 初稿     | xuexinyi |



**Keywords 关键词**：gms_tcp

**Abstract 摘要**：文档主要对gms_tcp高级包对TCP/IP的连接管理、数据读取和数据写入等功能接口进行验证，本次测试整体质量良好

**缩略语清单： **

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
|        |          |          |


# 1 概述

​       本特性是基于openGauss的网络通信插件，提供TCP/IP级别的网络编程功能，允许数据库直接进行网络通信操作。提供了TCP/IP的连接管理、数据读取和数据写入等功能接口。

# 2 测试版本说明

## 2.1 测试版本信息

###   2.1.1 被测版本

| 版本名称                           | 软件包名称                                    | 测试起始时间 | 测试结束时间 | 测试人员 |
| ---------------------------------- | --------------------------------------------- | ------------ | ------------ | -------- |
| openGauss 7.0.0-RC1 build f2632d45 | openGauss-All-7.0.0-RC1-CentOS7-x86_64.tar.gz | 2025/2/11    | 2025/2/20    | xuexinyi |

###  2.1.2 配套测试的版本

| 版本名称 | 配套版本 | 版本说明 |
| -------- | -------- | -------- |
| NA       |          |          |

## 2.2 测试环境描述

###  2.2.1 环境硬件信息

| 环境信息 | 硬件型号                             | 硬件配置信息                                                 | 备注     |
| -------- | ------------------------------------ | ------------------------------------------------------------ | -------- |
| 虚拟机   | CentOS Linux release 7.9.2009 (Core) | CPU：Intel(R) Xeon(R) CPU E5-2640 v2 @ 2.00GHz<br />内存：4GB<br />硬盘：50GB<br />OS：CentOS Linux 7 (Core) | 单机部署 |



# 3 版本概要测试结论、关键风险和规避措施

## 3.1 测试结论总结

支持gms_tcp高级包特性，共计执行53个用例，主要覆盖了功能测试、语法测试、可靠性测试、兼容性测试和资料测试，发现1个问题，无遗留风险，整体质量良好；

## 3.2 约束说明

（1）使用create extension gms_tcp方式加载

## 3.3 关键风险和规避措施

| 风险类型 | 风险描述 | 风险影响 | 规避措施和计划 | 责任人 | 当前进展 |
| -------- | -------- | -------- | -------------- | ------ | -------- |
| NA       |          |          |                |        |          |

# 4 版本详细测试结论

## 4.1 特性测试结论

###   4.1.1 新需求质量评价

| 特性              | 特性价值评估                                                 | 应用说明及关键约束假设依赖 | 关键遗留事项如缺陷等        | 测试整体覆盖情况                                     | 特性质量评估               | 主要风险 |
| ----------------- | ------------------------------------------------------------ | -------------------------- | --------------------------- | ---------------------------------------------------- | -------------------------- | -------- |
| 支持GMS_TCP高级包 | GMS_TCP包的价值在于它为PL/SQL提供了强大的网络通信能力，使数据库能够直接与外部TCP/IP服务器进行交互。通过其连接管理、数据读取和写入功能接口，GMS_TCP极大地扩展了数据库的应用场景，支持实时数据传输和外部系统集成，提升了数据库的灵活性和扩展性。 | 详见章节3.2                | 1个次要缺陷，预计本期不遗留 | 功能测试、语法测试、可靠性测试、兼容性测试和资料测试 | <font color=green>▮</font> | 无       |

## 4.2 产品质量属性目标(DFX)测试结论

###  4.2.1 性能测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.2 可靠性测试结论

| 测试步骤                           | 测试结果                               |
| ---------------------------------- | -------------------------------------- |
| 网络断连状态下执行上面各项接口     | 执行14条用例，执行均符合预期，测试通过 |
| 接收主机暂时不可达的情况下重试发送 | 执行6条用例，执行均符合预期，测试通过  |
| 高并发场景下发数据                 | 执行3条用例，执行均符合预期，测试通过  |
| 打开连接后，数据库重启再关闭连接   | 执行1条用例，执行均符合预期，测试通过  |
| 入参顺序异常                       | 执行14条用例，执行均符合预期，测试通过 |

###  4.2.3 安全&隐私保护测试结论

​	gms_tcp提供了发送和接收数据的功能，但不会对数据进行任何加密或解密操作。若需要传输密码等敏感数据，建议优先使用 HTTPS、SSL/TLS 等加密协议进行通信，故本次特性不涉及安全隐私测试。

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.4 可服务性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.5 生命周期管理测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

### 4.2.6 韧性测试结论

| 测试步骤 | 测试结果 |
| -------- | -------- |
| NA       |          |

###  4.2.7 兼容性测试结论

| 测试步骤                                                     | 测试结果                               |
| ------------------------------------------------------------ | -------------------------------------- |
| 1.分别在兼容B、C、PG库下执行测试用例场景，功能正常，不影响数据库正常使用 | 执行14条用例，执行均符合预期，测试通过 |

###  4.2.8 升级测试结论	

| 测试步骤                        | 升级路径 | 测试结果                            |
| ------------------------------- | -------- | ----------------------------------- |
| 升级前后检查gms_tcp扩展插件可用 | B02→B07  | 执行1条用例，执行符合预期，测试通过 |

## 4.3 资料测试结论

| 序号 | 测试章节                                                     | 测试结论                 |
| ---- | ------------------------------------------------------------ | ------------------------ |
| 1    | 插件参考>gms_tcp Extension>gms_tcp使用<br />https://gitee.com/opengauss/docs/pulls/7128/files | 整体质量良好，未发现问题 |
| 2    | 插件参考>gms_tcp Extension>gms_tcp安装                       | 整体质量良好，未发现问题 |
| 3    | 插件参考>gms_tcp Extension>gms_tcp概述                       | 整体质量良好，未发现问题 |
| 4    | 插件参考>gms_tcp Extension>gms_tcp限制                       | 整体质量良好，未发现问题 |

# 5 测试对象质量评估

##  5.1 覆盖率分析

​	本次测试主要覆盖gms_stats包所提供的连接管理、数据读取和数据写入等功能接口，针对接口功能实现、语法使用、异常场景的合理报错和超时等待等场景进行测试。

##  5.2 缺陷统计和分析

###   5.2.1 缺陷统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 1        | 0    | 0    | 1    | 0      |
| 百分比 | 100%     | 0%   | 0%   | 100% | 0%     |

###   5.2.2 缺陷列表

| 问题单号 | 问题描述                                                     | 问题级别 | 当前状态 |
| -------- | ------------------------------------------------------------ | -------- | -------- |
| IBN305   | 【测试类型：SQL功能】【测试版本：7.0.0-RC1】 gms_tcp.available设置超时参数为null，无法识别，未进行等待<br/>https://e.gitee.com/opengaussorg/issues/table?issue=IBN305 | 次要     | 待办的   |

# 6 测试过程评估

##  6.1 测试策略回顾

| 编号 | 接口功能                                                     | 接口语法                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 用以创建到其他服务的TCP/IP连接,并返回一个connection对象      | gms_tcp.open_connection(<br/>remote_host     in varchar2,<br/>remote_port     in integer,<br/>local_host       in varchar2 default NULL,<br/>local_port       in integer default NULL,<br/>in_buffer_size   in integer default NULL,<br/>out_buffer_size  in integer default NULL,<br/>charset         in varchar2 default NULL, <br/>newline        in varchar2 default CRLF,<br/>tx_timeout      in varchar2 default NULL<br/>)<br/>return connection; |
| 2    | 关闭打开的TCP/IP连接.                                        | gms_tcp.close_connections(<br/>c in connection<br/>);        |
| 3    | 关闭所有打开的TCP/IP连接.这个接口可以在PL/SQL过程结束前调用,防止残留TCP/IP连接. | gms_tcp.close_all_connections();                             |
| 4    | 将文本行传输到打开连接上的服务.换行符序列将在传输之前附到消息中. | gms_tcp.write_line(<br/>c    in out connection,<br/>data  in varchar2<br/>)<br/>return integer; |
| 5    | 将二进制消息传输到开放连接上的服务,返回实际传输的数据字节数. | gms_tcp.write_raw(<br/>c    in out connection,<br/>data  in raw,<br/>len   in integer default NULL<br/>)<br/>return integer; |
| 6    | 此功能在开放连接上将文本消息传输到服务,并返回写入数据的字节数. | gms_tcp.write_text(<br/>c    in out connection,<br/>data  in varchar2,<br/>len   in integer default NULL)<br/>return integer; |
| 7    | 此过程将输出缓冲区中的所有数据(如果使用缓冲区)立即传输到服务器. | gms_tcp.flush(<br/>c in connection<br/>);                    |
| 8    | 确定可用于从TCP/IP连接读取的字节数. 它是可以立即读取而不会阻塞的字节数. | gms_tcp.available(<br/>c      in connection, <br/>timeout in integer default 0<br/>) <br/>return integer; |
| 9    | 返回以 CHAR 格式读取的数据行.                                | gms_tcp.get_line(<br/>c         in connection,<br/>remove_crlf in boolean default false,<br/>peek       in boolean default false<br/>)<br/>return text; |
| 10   | 返回读取的数据而不是读取的数据量.                            | gms_tcp.get_raw(<br/>c   in connection,<br/>len  in integer default 1,<br/>peek in boolean default false<br/>)<br/>return raw; |
| 11   | 返回读取的文本数据.                                          | gms_tcp.get_text(<br/>c   in connection,<br/>len  in integer default 1,<br/>peek in boolean default false<br/>)<br/>return text; |
| 12   | 此函数在打开的连接上接收来自服务的文本行.一行数据以换行、回车或回车后跟换行结束,返回实际接收到的数据字符数. | gms_tcp.read_line(<br/>c    in out connection,<br/>data  out varchar2,<br/>len   out integer,<br/>remove_crlf in boolean default false,<br/>peek  in boolean default false<br/>)<br/>return; |
| 13   | 此函数在打开的连接上从服务接收二进制数据,返回实际接收到的数据字节数. | gms_tcp.read_raw(<br/>c       in out connection,<br/>data     in out raw,<br/>data_len  out integer,<br/>len      in integer default 1,<br/>peek     in boolean default false<br/>)<br/>return; |
| 14   | 此函数在打开的连接上从服务接收文本数据,返回接受到的文本字节数. | gms_tcp.read_text(<br/>c    in out connection,<br/>data  in out varchar2,<br/>data_len  out integer,<br/>len   in integer default 1,<br/>peek  in boolean default false<br/>)<br/>return; |

| 编号 | 特性       | 验证策略                                                     | 是否按照测试策略执行 |
| ---- | ---------- | ------------------------------------------------------------ | -------------------- |
| 1    | 语法测试   | 验证入参异常、缺省等语法场景                                 | YES                  |
| 2    | 功能测试   | 各接口功能测试                                               | YES                  |
| 3    | 故障测试   | 接口开启连接后，各种数据库类故障如重启数据库等，验证是否可正常关闭连接，无内存泄漏 | YES                  |
| 4    | 兼容性测试 | A、BC、PG库下分别验证接口可正常使用                          | YES                  |
| 5    | 并发测试   | 高并发场景下发数据，验证不会出现数据乱码，紊乱               | YES                  |
| 6    | 资料测试   | 资料描述与需求相符，示例正确无误                             | YES                  |

##  6.2 测试设计评估

| 编号 | 测试点修改说明 | 修改原因 | 是否影响测试质量 |
| ---- | -------------- | -------- | ---------------- |
| NA   |                |          |                  |

##  6.3 测试执行评估

###  6.3.1 测试执行统计数据

| 版本名称            | 工作量投入(人天) | 测试用例数 | 用例执行数 | 发现缺陷数 | 代码量（K） | 缺陷密度KLOC |
| ------------------- | ---------------- | ---------- | ---------- | ---------- | ----------- | ------------ |
| openGauss 6.0.0 B07 | 10               | 53         | 53         | 1          | 3.727       | 0.268        |

###  6.3.2 测试用例执行结果统计数据

|        | 计划测试用例数 | 实际测试的用例数 | Passed | Failed | Blocked | Unavailable | 执行率 | 执行通过率 |
| ------ | -------------- | ---------------- | ------ | ------ | ------- | ----------- | ------ | ---------- |
| 第一轮 | 53             | 53               | 53     | 0      | 0       | 0           | 100%   | 100%       |

# 7 附件

##  7.1 附件1：遗留问题列表

| 序号 | 问题单号 | 问题描述 | 分类 | 问题级别 | 问题分析与影响 | 规避措施 |
| ---- | -------- | -------- | ---- | -------- | -------------- | -------- |
| NA   |          |          |      |          |                |          |

##  7.2 附件2：特性相关PR

1.需求链接：
https://e.gitee.com/opengaussorg/issues/table?issue=I9FZFD

2.源代码pr：

https://e.gitee.com/opengaussorg/repos/opengauss/openGauss-server/pulls/6490

3.测试设计：

https://devcloud.cn-east-3.huaweicloud.com/testmind/project/03669bfd256c444bbfda6d7fb8b83bb2/testmind/mindmap?mindId=1b0bf4caafb34f1ab34da610da712d88&hideDevcloudHead=true

4.测试用例：

https://devcloud.cn-east-3.huaweicloud.com/cloudtestportal/project/03669bfd256c444bbfda6d7fb8b83bb2/testcase?branch_id=vb100000vjo9uumo

南大_TC →I9FZFD-[南大通用]支持gms_tcp高级包

