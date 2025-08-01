![avatar](../../../images/openGauss.png)

版权所有 © 2023 openGauss社区 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问[https://creativecommons.org/licenses/by-sa/4.0/](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2F) 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：[https://creativecommons.org/licenses/by-sa/4.0/legalcode。](https://gitee.com/link?target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-sa%2F4.0%2Flegalcode%E3%80%82)

修订记录

| 日期      | 修订 版本 | 修改描述 | 作者        |
| --------- | --------- | -------- | ----------- |
| 2024-3-15 | V1.0      | 初稿     | yangyixiang |

关键词：

datakit、资源池化双集群

摘要：

本文档主要介绍datakit支持资源池化主备双集群管理，实现异地容灾部署、切换，并给出最终测试结论

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| 无     |          |          |

# 1 特性概述

datakit工具支持资源池化容灾集群部署，支持安装、切换和状态查询操作

# 2 特性测试信息

被测对象的版本信息：

| 版本名称                                         | 测试起始时间 | 测试结束时间 |
| ------------------------------------------------ | ------------ | ------------ |
| datakit 6.0.0<br>openGauss 6.0.0 build b346ce32  | 2024-2-22    | 2024-3-6     |
| datakit 6.0.0<br/>openGauss 6.0.0 build a337a799 | 2024-3-7     | 2024-3-14    |

硬件环境信息：

| 硬件型号                 | 硬件配置信息                                                 | 备注 |
| ------------------------ | ------------------------------------------------------------ | ---- |
| TaiShan 200 (Model 2280) | Architecture：aarch64<br/>CPU：kunpeng-920 7260 2p 128核<br/>内存：32 * 32GB<br/>硬盘：3 * 2.9TB<br/>OS：openEuler release 20.03 (LTS-SP1) |      |

# 3 测试结论概述

## 3.1 测试整体结论

datakit工具支持资源池化双集群部署，共执行用例38个，主要覆盖了功能测试、资料测试。功能测试覆盖新增、修改、删除磁阵信息，安装资源池化容灾双集群，查看容灾双集群信息，集群间进行主备切换、解除容灾关系操作，验证容灾集群单集群启停、主备切换操作，解除容灾关系后卸载、删除集群。资料测试覆盖校验资料的描述是否完整。累计发现缺陷单3个，2个缺陷已解决且回归通过，1个资料缺陷未解决，整体质量良好。

## 3.2 约束说明

用于搭建资源池化容灾集群的单集群，需要满足如下约束：

1) 每个单集群需关联不同的Dorado磁阵，且磁阵之间需要能够相互访问。
2) 每个单集群的xlog卷只能有一个，并且需要建立好多个集群上xlog盘之间的同步复制关系。上述操作均需要在DeviceManager上操作完成。
3) 具有同步关系的xlog盘的容量要相同。

## 3.3 遗留问题分析

### 3.3.1 遗留问题影响以及规避措施

| 问题单号 | 问题描述                                                     | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | ------------------------------------------------------------ | -------- | ------------------ | -------- |
| I99OHI   | 【测试类型：资料】【测试版本：6.0.0】【DataKit】缺少安装资源池化集群及容灾双集群资料 | 次要     | 需后续补充资料     | 待办的   |

### 3.3.2 问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 3        | 0    | 1    | 2    | 0      |
| 百分比 | 100%     | 0%   | 33%  | 67%  | 0%     |

### 3.3.3 问题单汇总

| 序号 | issue号                                                      | 级别 | 问题简述                                                     | 状态   |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------ |
| 1    | [I94CKI](https://gitee.com/opengauss/openGauss-workbench/issues/I94CKI?from=project-issue) | 主要 | 【测试类型：工具功能】【测试版本：6.0.0】【DataKit】安装资源池化双集群卡住，数据库产生core | 已验收 |
| 2    | [I94I0H](https://gitee.com/opengauss/openGauss-workbench/issues/?from=project-issue) | 次要 | 【测试类型：工具功能】【测试版本：6.0.0】【DataKit】磁阵管理添加磁阵，磁阵名称、用户名、密码、pair id输入框未做纯空格校验 | 已验收 |
| 3    | [I99OHI](https://gitee.com/opengauss/openGauss-workbench/issues/I99OHI?from=project-issue) | 次要 | 【测试类型：资料】【测试版本：6.0.0】【DataKit】缺少安装资源池化集群及容灾双集群资料 | 待办的 |

# 4 测试执行

## 4.1 功能测试

### 4.1.1 磁阵管理

| 测试步骤                                                     | 测试结果                 |
| ------------------------------------------------------------ | ------------------------ |
| 1. 创建、修改、删除磁阵信息，验证输入框是否校验格式、空、纯空格<br>2. 删除已绑定集群的磁阵信息查看是否删除失败 | 执行21条用例，发现1个bug |

### 4.1.2 容灾集群安装

| 测试步骤                                                     | 测试结果                 |
| ------------------------------------------------------------ | ------------------------ |
| 1. 集群名称、集群、磁阵均配置正确，查看是否安装成功<br>2. 集群名称、集群、磁阵配置错误，查看是否合理报错<br>3. 主备集群选择同一集群，安装是否合理报错<br>4. 主备集群绑定的磁阵选择同一磁阵，安装是否合理报错<br>5. 容灾集群安装中刷新页面，稍后再查看是否安装成功<br>6. 容灾集群安装中制造异常，安装失败后清理异常，重试安装是否成功 | 执行11条用例，发现1个bug |

### 4.1.3 集群操作

| 测试步骤                                                     | 测试结果               |
| ------------------------------------------------------------ | ---------------------- |
| 1. 容灾集群执行主备集群切换操作，查看是否切换成功<br>2. 容灾集群执行解除容灾关系操作，查看是否成功解除<br>3. 查看容灾集群信息是否正常显示<br>4. 容灾集群单集群执行启停、重启、主备切换操作，查看是否成功执行<br>5. 解除容灾关系后查看能否正常卸载、删除集群<br>6. 容灾关系未解除时查看是否禁止卸载、删除集群 | 执行6条用例，未发现bug |

## 4.2 资料测试

| 测试步骤                | 测试结果 |
| ----------------------- | -------- |
| 1. 文档描述 2. 文档示例 | 暂无资料 |

## 4.3 测试数据统计

根据用例进行测试验证，结果如下：

| 版本名称                                         | 测试用例数 | 用例执行结果         | 发现问题单数 |
| ------------------------------------------------ | ---------- | -------------------- | ------------ |
| datakit 6.0.0<br/>openGauss 6.0.0 build b346ce32 | 29         | Passed：21 Failed：8 | 2            |
| datakit 6.0.0<br/>openGauss 6.0.0 build a337a799 | 38         | Passed：38 Failed：0 | 0            |

*数据项说明：*

- 累计发现缺陷单3个，2个缺陷已解决且回归通过，1个资料缺陷未解决
- 失败用例已在后续问题修复后，回归issue执行通过
- 缺陷密度：2(缺陷个数)/4kloc(代码行数)=0.5(个/kloc)

## 4.4 后续测试建议

查看资料是否完整

# 5 附件

## 5.1 源代码及文档PR

https://gitee.com/opengauss/openGauss-workbench/pulls/474

https://gitee.com/opengauss/openGauss-workbench/pulls/603
