# Docker

## Docker基本组成

镜像（images）：docker通过镜像来创建容器服务，通过镜像可以创建多个容器

容器（container）：Docker利用容器技术，独立运行一个或者一组应用

仓库（repository）：存放镜像的地方，分为私有仓库和共有仓库



# Docker命令

## 镜像操作

#### 查看所有镜像

```bash
# 查看本地主机上所有的镜像
docker images
-a, --all # 列出所有镜像
-q, --quiet # 只显示镜像的id

REPOSITORY   TAG                IMAGE ID       CREATED       SIZE
REPOSITORY # 镜像的仓库源
TAG # 镜像的标签
IMAGE ID # 镜像的id
CREARED # 镜像创建的时间
SIZE # 镜像的大小
```

#### 搜索镜像

```bash
# 搜索镜像
docker search 镜像名
```

#### 拉取镜像

```bash
# 拉取镜像
docker pull 镜像名[:tag]
```

#### 删除镜像

```bash
# 删除镜像
docker rmi -f 镜像id
```

## 容器操作

#### 创建容器并启动

```bash
# 创建容器并启动
docker run [可选参数] image

# 参数
--name= 容器名字
-d 容器在后台运行
-it 进入容器使用命令行
-p 指定容器的端口 # 主机端口:容器端口 将容器端口映射到主机端口
```

#### 查看所有容器

```bash
# 列出所有运行中的容器
docker ps
-a # 显示所有容器，包括未运行的
-q # 只显示容器的编号
```

#### 删除容器

```bash
# 删除容器
docker rm 容器id
```

#### 启动和停止容器

```bash
# 启动容器
docker start 容器id
# 重启容器
docker restart 容器id
# 停止容器
docker stop 容器id
# 强制停止容器
docker kill 容器id
```

#### 查看日志

```bash
# 查看日志
docker logs 容器id
```

#### 查看容器进程信息

```bash
# 查看容器进程信息
docker top 容器id
```

#### 查看容器元数据

```bash
# 查看容器元数据
docker inspect 容器id
```

#### 进入正在运行的容器

```bash
# 进入正在运行的容器
docker exec -it 容器id bashshell # 进入容器，开启一个新的终端

docker attach 容器id # 进入容器正在执行的终端
```

#### 容器拷贝文件

```bash
# 从容器内拷贝文件到容器外
docker cp 容器id:路径 路径
# 从容器外拷贝文件到容器内
docker cp 路径 容器id:路径
```

### commit镜像

```bash
docker commit #提交容器成为一个新的副本
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[R]
```

## 容器数据卷

- 将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全

### 操作数据卷

```bash
docker volume [COMMAND]
create #创建一个volume
inspect # 显示一个或多个volume的信息
ls #列出所有的volume
prune # 删除未使用的volume
rm # 删除一个或多个指定的volume
```

### 挂载数据卷

```bash
# 指定路径挂载
docker run -v 主机目录:容器内目录
#匿名挂载
docker run -v 容器内路径
# 具名挂载
docekr run -v 卷名:r
```



# Dockerfile

## 什么是dockerfile

dockerfile就是一个文本文件，其中包含一个个的指令，用指令来说明要执行什么操作来构建镜像。每一个指令都会形成一层Layer

## Dockerfile保留字

| 指令        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| FROM        | 指定基础镜像                                                 |
| ENV         | 设置环境变量，可在后面指令使用                               |
| COPY        | 拷贝本地文件到镜像的指定目录                                 |
| RUN         | 执行Linux的shell命令，一般是安装过程的命令                   |
| EXPOSE      | 指定容器运行时监听的端口，是给镜像使用者看的                 |
| WORKDIR     | 指定在创建容器后，终端默认登录来进行的工作目录               |
| USER        | 指定该镜像以什么样的用户去执行，默认root                     |
| VOLUME      | 容器数据卷，用于数据存储和持久化工作                         |
| ADD         | 将宿主机目录下的文件拷贝进镜像并且会自动处理URL和解压tar压缩包 |
| CMD         | 指定容器启动后要做的事情，Dockerfile中可以有多个CMD命令，但只有最后一个生效，CMD会被docker run之后的参数替换，CMD是在docker run时运行，RUN是在docker builder时运行 |
| ENTRYPORINT | 用来指定一个容器启动时要运行的命令，不会被docker run后面的命令覆盖，当ENTRYPORINT和CMD一起用时，CMD就会被当成是ENTRYPORINT的参数 |
| MAINTAINER  | 镜像的作者和邮箱                                             |

## Dockerfile 构建镜像

```bash
docker build -t 新镜像名称:TAG .
```

# Docker 网络

Docker 服务默认会创建一个docker0 网桥，该桥接网络的名称为docker0，在内核层面连同了其他的物理或虚拟网卡，这就将所有容器和本地主机都放到同一个物理网络。Docker默认指定了docker0 接口的 IP 地址和子网掩码，让主机和容器之间可以通过网桥相互通信。

## 作用

- 保证容器间的互联和通信以及端口映射
- 容器IP变动可以通过服务名直接网络通信而不受影响

## 网络模式

| 网络模式  | 简介                                                         |
| --------- | ------------------------------------------------------------ |
| bridge    | 为每一个容器分配、设置IP等，并将容器连接到一个docker0，虚拟网桥，默认为该模式 |
| host      | 容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口 |
| none      | 容器有独立的Network namespace，但并没有对其进行 任何网络设置，如分配veth pair和网桥连接，IP等 |
| container | 新创建的容器不会创建自己的网卡和配置自己的IP，而是和一个指定的容器共享IP、端口范围等 |

# DockerCompose

## 什么是DockerCompose

- Docker Compose可以基于Compose文件帮我们快速的部署分布式应用，而无需手动一个个创建和运行容器
- Compose文件是一个文本文件，通过指令定义集群中的每个容器如何运行，实现对容器集群的编排

##  Compose常用命令

| 命令                       | 解释                                         |
| -------------------------- | -------------------------------------------- |
| docker-compose up          | 启动所有docker-compose服务                   |
| docker-compose up -d       | 启动所有docker-compose服务并在后台运行       |
| docker-compose down        | 停止并删除所有容器、网络、卷、镜像           |
| docker-compose exec 服务id | 进入容器实例内部                             |
| docker-compose ps          | 展示当前docker-compose编排过的运行的所有容器 |
| docker-compose top         | 展示当前docker-compose编排过的容器过程       |
| docker-compose logs        | 查看容器输出日志                             |
| docker-compose config      | 检查配置                                     |
| docker-compose config -q   | 检查配置，有问题才输出                       |
| docker-compose restart     | 重启服务                                     |
| docker-compose start       | 启动服务                                     |
| docker-compose stop        | 停止服务                                     |

# Kubernetes

大规模容器编排系统

## 特性

- 服务发现和负载均衡
- 存储编排
- 自动部署和回滚
- 自动完成装箱计算
- 自我修复

## k8s资源创建方式

- 命令行
- yaml

## Namespace

> 名称空间用来隔离资源。默认只隔离资源，不隔离网络

使用命令行创建名称空间

```bash
kubectl get ns # 获取全部 namespace
kubectl create ns xxx # 创建 namespace
kubectl delete ns xxx # 删除 namespace
```

使用配置文件创建命名空间

```yaml
apiVersion: v1
kind: Namespace
metadata:
	name: xxx
```

```shell
kubectl apply -f xxx.yaml # 以配置文件创建namespace
kubectl delete -f xxx.yaml # 删除该配置文件创建的namespace
```

> 删除 namespace 会将其包含的所有资源一并删除

## Pod

运行中的一组容器，Pod是 kubernetes 中应用的最小的单位

```bash
kubectl describe pod xxx # 查看 Pod 的描述信息
kubectl logs xxx # 查看 Pod 的运行日志
kubectl get pod xxx -owide # 每个 Pod k8s 都会分配一个IP
kubectl exec -it xxx -- /bin/bash # 进入 Pod 内部
```



使用命令行创建pod

```shell
kubectl run xxx --image=xxx # 创建 Pod
kubectl delete pod xxx # 删除 Pod
```

使用yaml文件创建pod

```yaml
apiVersion: v1
kind: Pod
metadata:
	labels:
		run: xxx
	name: xxx
spec:
	containers:
		- image: xxx
		  name: xxx
```

```shell
kubectl apply -f xxx.yaml
```



## Deployment

控制 Pod，使 Pod 拥有多副本，自愈，扩缩容等能力

### 基础操作

```bash
kubectl create deployment xxx --image=xxx # 创建
kubectl delete deployment xxx # 删除
```

### 多副本

命令行操作

```bash
kubectl create deployment xxx --image=xxx --replicas=x # 多副本
```

配置文件

```yaml
apiVersion: v1
kind: Deployment
metadata:
	labels:
		app: xxx
	name: xxx
spce:
	replicas: x
	matchLabels:
		app: xxx
	template:
		metadata:
			labels:
				app: xxx
		spec:
			containers:
				- image: xxx
				  name: xxx
```

```bash
kubectl apply -t xxx.yaml
```

### 扩缩容

```bash
kubectl scale deployment xxx --replicas=x 
```

### 滚动更新

```bash
kubectl set image xxx xxx=xxx --record
```

### 版本回退

```bash
kubectl rollout history xxx # 历史记录
kubectl rollout history xxx --revision=x # 查看某个历史详情
kubectl rollout undo xxx # 回滚（回滚到上一次）
kubectl rollout undo xxx --to-revision=x # 回滚到指定版本
```

## 其他工作负载

| 名称        | 主要作用                                   |
| ----------- | ------------------------------------------ |
| Deployment  | 无状态应用部署，提供多副本等功能           |
| StatefulSet | 有状态应用部署，提供稳定的存储、网络等功能 |
| DaemonSet   | 守护型应用部署，每个机器都运行一份         |
| Job/CronJob | 定时任务部署，在指定时间运行               |

## Service

将一组 Pods 公开为网络服务的抽象方法 

Pod的服务发现和负载均衡

```bash
kubectl expose deployment xxx --port=xxx --target-port=xxx # 默认仅在集群内部可以访问
kubectl expose deployment xxx --port=xxx --target-port=xxx --type=NodePort # 集群外部可以访问
```

## Ingress

service 的统一网关入口
