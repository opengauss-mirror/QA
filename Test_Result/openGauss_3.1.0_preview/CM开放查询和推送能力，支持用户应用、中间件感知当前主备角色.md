

![avatar](D:\QA-master\images\openGauss.png)

版权所有 © 2022  openGauss社区
 您对“本文档”的复制、使用、修改及分发受知识共享(Creative Commons)署名—相同方式共享4.0国际公共许可协议(以下简称“CC BY-SA 4.0”)的约束。为了方便用户理解，您可以通过访问https://creativecommons.org/licenses/by-sa/4.0/ 了解CC BY-SA 4.0的概要 (但不是替代)。CC BY-SA 4.0的完整协议内容您可以访问如下网址获取：https://creativecommons.org/licenses/by-sa/4.0/legalcode。

修订记录

| 日期       | 修订   版本 | 修改描述     | 作者        |
| :--------- | ----------- | ------------ | ----------- |
| 2022-09-22 | 1.0         | 特性测试报告 | houshaolong |

 关键词： 

CMRestAPI

 摘要：

通过运行cmrestapi-xxx.jar组件，实现远程查询集群的状态，在数据库集群发生切主事件时，推送集群最新主备信息。

缩略语清单：

| 缩略语 | 英文全名 | 中文解释 |
| ------ | -------- | -------- |
| N/A    |          |          |

# 1     特性概述

通过运行cmrestapi-xxx.jar组件，CM能够支持：

1. 通过http/https服务远程查询到集群的状态，便于管理人员、运维平台等监控集群状态
2. 在数据库集群发生切主事件时，通过http/https服务及时地将集群最新的主备信息推送到应用端注册的接收地址，便于应用端及时的感知到集群的主备变化，从而能够快速的连接到新的主机和备机

# 2     特性测试信息

被测对象的版本信息：

| 版本名称                       | 测试起始时间 | 测试结束时间 |
| ------------------------------ | :----------- | ------------ |
| openGauss 3.1.0 build 88ca9bcd | 2022-09-17   | 2022-09-21   |
| openGauss 3.1.0 build a9a88787 | 2022-09-22   | 2022-09-22   |

环境信息：

| 环境信息信息 | 配置信息                                                     | 备注 |
| ------------ | ------------------------------------------------------------ | ---- |
| 虚拟机       | cpu :    Intel(R) Xeon(R) Gold 6266c CPU @ 3.00HZ <br />内存：16GB<br />硬盘：320G<br />OS：openEuler release 20.03 (LTS-SP1) |      |

# 3     测试结论概述

## 3.1   测试整体结论

CM开放查询和推送能力，支持用户应用（中间件）感知当前主备角色共计执行用例**条，覆盖启动组件、查询数据库集群和数据库节点信息、信息接收地址和注册功能和更新功能、推送功能、删除主备机信息接收地址功能、安全测试、可靠性测试和资料测试，累计发现两个缺陷，缺陷均已解决，回归通过，无遗留风险，整体质量良好。

| 测试活动   | 活动评价                                                     |
| ---------- | ------------------------------------------------------------ |
| 功能测试   | 启动cmrestapi-xxx.jar组件：使用不同命令行方式和自定义资源方式启动组件，测试通过 |
| 功能测试   | 查询数据库集群和数据库节点信息：数据库集群和数据库节点不同状态下分别查询，查询结果与数据库状态一致，测试通过 |
| 功能测试   | 信息接收地址和注册/更新功能：通过填写url键值对 和app键值对作为入参进行测试，结果符合预期，测试通过 |
| 功能测试   | 推送功能：启动应用端程序，对数据库集群进行切主操作，推送结果及时且符合预期，测试通过 |
| 功能测试   | 删除主备机信息接收地址功能：通过填写app键值对作为入参进行测试，结果符合预期，测试通过 |
| 安全测试   | 通过设置白名单的方式测试http和https服务，通过生成ssl证书测试https服务，结果符合预期，测试通过 |
| 可靠性测试 | 通过注入网络故障进行测试，结果符合预期，测试通过             |
| 资料测试   | 资料描述准确，示例完整，可正确操作，格式正确，测试通过       |

## 3.2   约束说明

1.数据库需要使用3.1.0版本

2.用户使用的应用/中间件需要用户根据自身需求自定义开发

3.CMRestAP默认使用http服务，支持配置访问白名单，若要使用https服务，需要在启动cmrestapi-xxx.jar组件时，指定系统参数serve.ssl相关参数来启动https服务，配置参数示例：

```
-Dserver.port=服务监听端口 -Dserver.ssl.key-store=秘钥文件路径 -Dserver.ssl.key-store-password=秘钥文件密码 -Dserver.ssl.key-store-type=秘钥类型  
如：java -jar -Dserver.port=8443 -Dserver.ssl.key-store=/home/omm/keystore.p12 -Dserver.ssl.key-store-password=Abcdef@123 -Dserver.ssl.key-store-type=PKCS12 cmrestapi-xxx.jar -e envFile
```

4.由于本程序使用了springboot框架，默认启动会占用较大内存（约1G左右），若并发量不大不希望该程序占用较大内存，则可以在启动时指定一些系统参数减小内存占用，启动示例：

```
-XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=56m -Xms128m -Xmx128m -Xmn32m -Xss328k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC  
如：java -jar -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=56m -Xms128m -Xmx128m -Xmn32m -Xss328k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC cmrestapi-xxx.jar -e envFile
```

5.本程序需要依赖cm相关进程和指令，所以必须与cm同时运行，需配置自定义资源配置文件，配置方法详见自定义资源监控特性相关内容。

## 3.3   遗留问题分析

### 3.3.1  遗留问题影响以及规避措施

| 问题单号 | 问题描述 | 问题级别 | 问题影响和规避措施 | 当前状态 |
| -------- | -------- | -------- | ------------------ | -------- |
| N/A      |          |          |                    |          |

### 3.3.2  问题统计

|        | 问题总数 | 严重 | 主要 | 次要 | 不重要 |
| ------ | -------- | ---- | ---- | ---- | ------ |
| 数目   | 2        | 0    | 0    | 2    | 0      |
| 百分比 | 100%     | 0    | 0    | 100% | 0      |

### 3.3.3  问题单汇总

| 序号 | issue号                                                      | 问题级别 | 问题简述                                                     | 问题状态 |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | -------- |
| 1    | [I5RWEM](https://gitee.com/opengauss/CM-RestAPI/issues/I5RWEM?from=project-issue) | 次要     | 自定义资源zookeeper，查询集群状态时，不显示zookeeper相关信息 | 已验收   |
| 2    | [I5S52S](https://gitee.com/opengauss/docs/issues/I5S52S?from=project-issue) | 次要     | 数据库统一管理工具中，支持集群信息查询和推送部分，密码书写不规范 | 已验收   |

# 4     测试执行

## 4.1.1   启动cmrestapi-xxx.jar组件

| 测试步骤                                   | 测试结果                         |
| ------------------------------------------ | -------------------------------- |
| 使用不同命令行方式和自定义资源方式启动组件 | 执行9条用例，未发现bug，验收通过 |

## 4.1.2   查询数据库集群和数据库节点信息

| 测试步骤                               | 测试结果                           |
| -------------------------------------- | ---------------------------------- |
| 查看不同情况下数据库状态和单个节点信息 | 执行17条用例，发现bug1个，验收通过 |

## 4.1.3    信息接收地址和注册/更新功能

| 测试步骤                                 | 测试结果                          |
| ---------------------------------------- | --------------------------------- |
| 填写url键值对和app键值对作为入参进行测试 | 执行8条用例，发现bug2个，验收通过 |

## 4.1.4   推送功能

| 测试步骤                                 | 测试结果                         |
| ---------------------------------------- | -------------------------------- |
| 启动应用端程序，对数据库集群进行切主操作 | 执行5条用例，未发现bug，验收通过 |

## 4.1.5  删除主备机信息接收地址功能

| 测试步骤                          | 测试结果                         |
| --------------------------------- | -------------------------------- |
| 通过填写app键值对作为入参进行测试 | 执行5条用例，未发现bug，验收通过 |

## 4.1.6  安全测试

| 测试步骤                        | 测试结果                         |
| ------------------------------- | -------------------------------- |
| 通过设置白名单，http和https服务 | 执行1条用例，未发现bug，验收通过 |

## 4.1.7  可靠性测试

| 测试步骤                                                     | 测试结果                          |
| ------------------------------------------------------------ | --------------------------------- |
| 通过设置白名单的方式测试http和https服务，通过生成ssl证书测试https服务 | 执行10条用例，未发现bug，验收通过 |

## 4.1.8  资料测试

| 测试步骤                                               | 测试结果                          |
| ------------------------------------------------------ | --------------------------------- |
| 资料描述准确，示例完整，可正确操作，格式正确，测试通过 | 执行1条用例，发现bug1给，验收通过 |

## 4.1.9  后续测试建议

1.通过用户自主开发应用组件，测试应用组件和cmrestapi-xxx.jar组件的兼容性

## 4.2   测试执行统计数据

| 版本名称                       | 测试用例数 | 用例执行结果                | 发现问题单数 |
| ------------------------------ | ---------- | --------------------------- | ------------ |
| openGauss 3.1.0 build 88ca9bcd | 56         | Passed：56 <br />Failed：2  | 2            |
| openGauss 3.1.0 build a9a88787 | 56         | Passed：56  <br />Failed：0 | 0            |

*数据项说明：*

* 累计发现缺陷单2个，其中2个缺陷均已解决且回归通过
* 失败用例已在后续问题修复后，回归issue执行通过
* 缺陷密度：2(缺陷个数)/27.939kloc(代码行数)=0.47(个/kloc)

# 5     附件

## 5.1   cmrestapi.sh 资源脚本示例 

```
#!/bin/bash
#set -ex   #取消该行注释可帮助调试脚本
#资源名称
resName=CMrestAPI
#资源binpath
cmrestapiPath=/data/housaolong/Install/cluser/app/bin/cmrestapi-3.1.0-RELEASE.jar
#用于过滤资源实例的命令关键词
cmdKey=cmresterapi-3.1.0-RELEASE.jar
#用于保存首次检测到资源僵死时间的文件
phony_dead_time_file=.cmrestapi_phony_dead_time
#最长僵死时间，单位为s
envFile=/home/shaolong_0923/env 
appwhitelistFile=/home/shaolong_0923/whitelist
PHONY_MAX_TIME=20

function exec_start
{
  #资源启动命令
  nohup java -jar -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=56m -Xms128m -Xmx128m -Xmn32m -Xss328k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC -Dserver.port=8090 $cmrestapiPath -e $envFile -w $appwhitelistFile>>$GAUSSLOG/cm/cmrestapi/cmrestapi.log 2>&1 $; 
  exit $?
}

function exec_stop
{
  #资源停止命令
  ps x | grep "$cmdkey" grep | awk '{print $1}' | xargs kill -9 ; exit $?
}

function exec_check
{
  #查询资源实例pid
  pid=`ps x | grep "$cmdKey" | grep -v grep | awk '{print $1}'`
  if [ "${pid}" == "" ]; then
    echo "$resName is not running."
    exit 1
  fi
  #查询资源实例进程状态
  state=`cat /proc/$pid/status | grep "State" | awk '{print $2}'`
  if [ "$state" == "T" ]; then
    #僵死检查和处理
    if [ ! -f $phony_dead_time_file ]; then
      touch ./${phony_dead_time_file}
      echo "export firstphonytime=''" > ./${phony_dead_time_file}
    fi
    source ./$phony_dead_time_file;
    curtime=$(date +%s);
    if [ "$firstphonytime" == "" ]; then
      #首次检测到资源僵死，将首次检测到僵死的时间写入僵死时间存储文件
      #firstphonytime为用于保存当前资源实例僵死时间的变量名称，
      #若当前节点存在多个自定义资源实例，该名称需要指定为不同的名称
      echo "export firstphonytime=$curtime" > ./$phony_dead_time_file;
      exit 0;
    fi
    dead_time=$(( $curtime - $firstphonytime ));
    #若僵死时间大于等于用户设定的最大僵死时间，则立即杀死资源实例，否则不做处理正常退出
    if [ $dead_time -ge $PHONY_MAX_TIME ]; then
      echo "$resName is detected in a state of phony dead(T) and will be forcibly killed!"
      kill -9 $pid
      rm ./${phony_dead_time_file} -f
      sh ${shardingPath}/start.sh; exit $?
    else
      exit 0
    fi
  elif [ "$state" == "S" ]; then
    #未处于僵死状态清理环境后正常退出
    rm ./${phony_dead_time_file} -f
    exit 0
  fi
}

#以下为固定接口无需更改，对应以上三个函数，且必须实现
if [ $1 == '-start' ]; then
  exec_start $2
elif [ $1 == '-stop' ]; then
    exec_stop $2
elif [ $1 == '-check' ]; then
  exec_check $2
fi
```

## 5.2   自定义资源配置文件cm_resource.json 示例

```
{
  "resources": [
    {
      "name": "CM-RestAPI",
      "instances": [
        {
          "node_id": 1,
          "res_instance_id": 1
        },
        {
          "node_id": 2,
          "res_instance_id": 2
        }
      ],
      "script": "/data/housaolong/Install/cluser/cm/cm_agent/cmrestapi.sh",
      "check_interval": 1,
      "time_out": 5,
      "restart_delay":3,
      "restart_period":5,
      "restart_times":10
    }
}
```