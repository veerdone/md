# RocketMQ

## 命令行使用

### 创建 Topic 并指定消息类型

Type

- TRANSACTION
- DELAY
- NORMAL
- FIFO

未指定类型时默认是 NORMAL

```sh
mqadmin updatetopic -n {NameSrvAddr} -t {YourTopic} -c {YourCluster}  -a +message.type={Type}
```

