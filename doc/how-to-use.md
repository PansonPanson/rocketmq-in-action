# RocketMQ 的单机模式部署与测试

## 1 下载与安装（MacOS）
从官网上上下载最新的版本：rocketmq-all-4.7.1-bin-release.zip

解压放在指定目录。

## 2 修改配置
在 bin 目录下存放着许多执行脚本，我们重点关注：`runbroker.sh` 和 `runserver.sh`，注释上述文件中的 3 行配置：

```shell
[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=$HOME/jdk/java
[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=/usr/java
[ ! -e "$JAVA_HOME/bin/java" ] && error_exit "Please set the JAVA_HOME variable in your environment, We need java(x64)!"
```

替换成自己的 jdk 路径，我的 mac 环境是 `/Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home`。

```shell
[ ! -e "$JAVA_HOME/bin/java" ] && JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home
```

## 3 启动 Name Sever 和 Broker

启动 NameServer：

```shell
nohup sh bin/mqnamesrv &
```

查看日志：

```shell
tail -100f ~/Logs/rocketmqLogs/namesrv.log
```

启动一个 Broker:

```shell
nohup sh bin/mqbroker -n localhost:9876 &
```

查看日志：

```shell
tail -f ~/Logs/rocketmqLogs/broker.log
```

成功之后会看到 `success` 信息：

```shell
INFO main - The Name Server boot success. serializeType=JSON
```

## 4 测试生产者和消费者发送、消费消息

配置环境：

```shell
export NAMESRV_ADDR=localhost:9876
```

启动生产者与消费者：

```shell
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

## 5 关闭 Name Sever 与 Broker

```shell
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv
```



