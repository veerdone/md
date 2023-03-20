# redis

## string类型

- 存储的数据：单个数据，最简单的数据存储类型，也是最常用的数据存储类型
- 存储数据的格式：一个存储空间保存一个数据
- 存储内容：通用的字符串，如果字符串以整数的形式展示，可以作为数字操作使用

### 基本操作

- 添加/ 修改数据

  ````
  set key value
  ````

- 获取数据

  ```
  get key
  ```

- 删除数据

  ```
  del key
  ```

- 添加/修改多个数据

  ```
  mset key1 value1 key2 value2 ...
  ```

- 获取多个数据

  ```
  mget key1 key2...
  ```

- 获取数据字符个数（字符串长度）

  ```
  strlen key
  ```

- 追加信息到原始信息后部（如果原始信息存在就追加，否则新建）

  ```
  append key value
  ```

### 拓展操作

- 设置数值数据增加指定范围的值

  ```
  incr key
  incr key increment
  incrbyfloat key increment
  ```

- 设置数值数据减少指定范围的值

  ```
  decr key
  decrby key increment
  ```

- 设置数据具有指定的生命周期

  ```
  setex key seconds value
  psetex key milliseconds value
  ```

  

## hash类型

- 对一系列存储的数据进行编辑，方便管理，典型应用存储对象信息
- 一个存储空间可以保存多个键值对数据

### 基本操作

- 添加/修改数据

  ```
  hset key field value
  ```

- 获取数据

  ```
  hget key field
  hgetall key
  ```

- 删除数据

  ```
  hdel key field1 [field2]
  ```

- 添加/修改多个数据

  ```
  hmset key field1 value1 field2 value2...
  ```

- 获取多个数据

  ```
  hmget key field1 field2 ...
  ```

- 获取哈希表中字段的数量

  ```
  hlen key
  ```

- 获取哈希表中是否存在指定的字段

  ```
  hexists key field
  ```

### 拓展操作

- 获取哈希表中所有的字段名或字段值

  ```
  hkeys key
  hvals key
  ```

- 设置指定字段的数值数据增加指定范围的值

  ```
  hincrby key field increment
  hincrbyfloat key field increment
  ```

## list类型
- 数据存储需求：存储多个数据，并对数据进入存储空间的顺序进行区分
- 需要的存储结构：一个储存空间保存多个数据，且通过数据可以体现进入顺序
- list类型：保存多个数据，底层使用双向链表存储结构实现

### 基本操作

- 添加/修改数据

  ```
  lpush key value1 [value2]
  rpush key value1 [value2]
  ```

- 获取数据

  ```
  lrange key start stop
  lindex key index
  llen key
  ```

- 获取并移除数据

  ```
  lpop key
  rpop key
  ```

### 拓展操作

- 规定时间内获取并移除数据

  ```
  blpop key1 [key2] timeout
  brpop key1 [key2] timeout
  ```

- 移除指定数据

  ```
  lrem key count value
  ```


## set类型

- 新的存储需求：存储大量的数据，再查询方面提供更高的效率
- 需要的存储结构：能够保存大量的数据，高效的内部存储机制，便于查询
- set类型：与hash存储结构相同，仅存储键，不存储值，并且值是不允许重复的

### 基本操作

- 添加数据

  ```
  sadd key member1 [member2]
  ```

- 获取全部数据

  ```
  smembers key
  ```

- 删除数据

  ```
  srem key member1 [member2]
  ```

- 获取集合数据总量

  ```
  scard key
  ```

- 判断集合中是否包含指定数据

  ```
  sismember key member
  ```

### 扩展操作

- 随机获取集合中指定数量的数据

  ```
  srandmember key [count]
  ```

- 随机获取集合中的某个数据并将该数据移除集合

  ```
  spop key
  ```

- 求两个集合的交、并、差集

  ```
  sinter key1 [key2]
  sunion key1 [key2]
  sdiff key1 [key2]
  ```

- 求两个集合的交、并、差集并存储到指定集合中

  ```
  sinterstore destination key1 [key2]
  sunionstore destination key1 [key2]
  sdiffstore destination key1 [key2]
  ```

- 将指定数据从原始集合中移到目标集合中

  ```
  smove source destination member
  ```

## sorted_set类型

- 新的存储需求：数据排序利于数据的有效展示，需要提供一种可以根据自身特征进行排序的方式
- 需要的存储结构：新的存储结构，可以保存可排序的数据
- sorted_set类型：在set的存储结构基础上添加可排序字段

### 基本操作

- 添加数据

  ```
  zadd key scorel member1 [soure2 menber2]
  ```

- 获取全部数据

  ```
  zrange key start stop [WITHSCORES]
  zrevrange key start stop [WITHSCORES]
  ```

- 删除数据

  ```
  zrem key member [member ...]
  ```

- 按条件获取数据

  ```
  zrangebysource key min max [WITHSOURES] [LIMIT]
  zrevrangebysource key max min [WITHSOURES]
  ```

- 按条件删除数据

  ```
  zremrangebyrank key start stop
  zremrangebyscore key min max
  ```

- 获取集合数据总量

  ```
  zcard key
  zcount key min max
  ```

- 集合交、并操作

  ````
  zinterstore destination numkeys key [key...]
  zunionstore destination numkeys key [key...]
  ````

## key通用操作

### key基本操作

- 删除指定key

  ```
  del key
  ```

- 获取key是否存在

  ```
  exists key
  ```

- 获取key的类型

  ```
  type key
  ```



### 拓展操作

- 为指定key设置有效期

  ```
  expire key seconds
  pexpire key milliseconds
  expireat key timestamp
  pexpireat key milliseconds-timestamp
  ```

- 获取key的有效时间

  ```
  ttl key
  pttl key
  ```

- 切换key从时效性转换为永久性

  ````
  persist key
  ````

- 查看key

  ```
  keys pattern
  查询模式规则
  * 匹配任意数量的任意符号
  ? 匹配一个任意符号
  [] 匹配一个指定符号
  ```

- 为key改名

  ```
  rename key new key
  renamex key newkey
  ```

- 对所有key排序

  ```
  sort
  ```



## 数据库操作

### 基本操作

- 切换数据库

  ```
  select index
  ```

- 其他操作

  ```
  quit
  ping
  echo message
  ```

### 相关操作

- 数据移动

  ```
  move key db
  ```

- 数据清楚

  ```
  dbsize
  flushdb
  flushall
  ```

## redis持久化

### RDB

- 命令

```
save
作用：
	手动执行一次保存操作

bgsave
作用：
	手动启动后台保存操作，但不是立即执行
```

- 服务器运行过程中重启

  ```
  debug reload
  ```

- 关闭服务器时指定保存数据

  ```
  shutdown save
  ```

  

配置文件相关配置

```bash
dbfilename dump.rdb
	设置本地数据库文件名，默认为dump.rdb
dir 
	设置存储.rdb文件的路径
rdbcompression yes
	设置存储至本地数据库时是否压缩数据，默认为yes，采用LZF压缩
rdbchecksum yes
	设置是否进行RDB文件格式校验，该校验过程在写文件和读文件过程均进行
save second changes
	满足限定时间范围内key的变化数量达到指定数量即进行持久化
```

#### 优点

- RDB时一个紧凑型的二进制文件，存储效率高
- RDB内部存储的时redis在某个时间点的数据快照，非常适合于数据备份，全量复制等场景
- RDB恢复数据的速度比AOF快很多
- 应用：服务器每X小时执行bgsave备份，并将RDB文件拷贝到远程机器，用于灾难恢复

#### 缺点

- RDB方式无论是执行指令还是利用配置，无法做到实时持久化，具有较大的可能性丢失数据
- bgsave指令每次运行要执行fork操作创建子进程，要牺牲掉一些性能
- Redis的众多版本中未进行RDB文件格式的版本统一，有可能出现各版本服务器之间数据格式无法兼容现象

### AOF

- always（每次）

  每次写入操作均同步到AOF文件中，**数据零误差**，**性能较低**

- everysec（每秒）

  每秒将缓冲区的指令同步到AOF文件中，**数据准确性高**，**性能较高**，也是默认配置

  在系统突然宕机的情况下丢失1秒内的数据

- no（系统控制）

  由操作系统控制每次同步到AOF文件的周期，整体过程**不可控**

#### 配置文件配置

```
appendonly yes|no
	是否开启AOF持久化功能，默认不开启
appendfsync always|everysec|no
	AOF写数据策略
```

#### AOF重写

随着命令不断写入AOF，文件会越来越大，为了解决这个问题，Redis引入了AOF重写机制压缩文件体积，AOF文件重写是将Redis进程内的数据转化为写命令同步到新AOF文件的过程。简单来说就是将同一个数据的若干条命令执行结果转化为最终结果数据对应的指令进行记录

#### AOF重写规则

- 进程内已超时的数据不再写入文件
- 忽略无效指令，重写时使用进程内数据直接生成，这样新的AOF文件只会保留最终数据的写入命令
- 对同一数据的多条写命令合并为一条命令

#### AOF重写方式

- 手动重写

  ```
  bgrewritreaof
  ```

- 自动重写

  ```
  auto-aof-rewrite-min-size size
  auto-aof-rewrite-percentage percent
  ```

- 自动重写触发比对参数

  ```
  aof_current_size
  aof_base_size
  ```

- 自动重写触发条件

  ```
  aof_current_size>auto-aof-rewrite-min-size
  aof_current_size-aof_base_seze
  ————————————————————————————————>=  auto-aof-rewrite-perctage
  		aof_base_size
  ```

  

### RDB于AOF区别

|  持久化方式  |        RDB         |        AOF         |
| :----------: | :----------------: | :----------------: |
| 占用存储空间 | 小（数据级：压缩） | 大（指令级：重写） |
|   存储速度   |         慢         |         快         |
|   恢复速度   |         块         |         慢         |
|  数据安全性  |     会丢失数据     |    依据策略决定    |
|   资源消耗   |     高/重量级      |     低/轻量级      |
|  启动优先级  |         低         |         高         |

### RDB和AOF的选择

- 对数据非常敏感，使用默认的AOF持久化方案
  - AOF持久化策略使用everysecond，每秒fsync一次。该策略redis仍可以保持很好的处理性能
  - 由于AOF文件存储体积较大，恢复速度较慢
- 数据呈现阶级有效性，建议使用RDB持久化方案
  - 数据可以良好的做到阶段内无丢失，且恢复速度较快，阶段点数据恢复通常采用RDB方案
  - 利用RDB实现紧凑的数据持久化会是redis降的很低

## 事务

### 事务的基本操作

- 开启事务

  ```
  multi
  	设定事务的开启位置，此指令执行后，后续的所有指令均加入到事务中
  ```

- 执行事务

  ````
  exec
  	设定事务的结束位置，同时执行事务，于muti成对出现
  ````

- 取消事务

  ```
  discard
  	终止当前事务的定义，发生在multi之后，exec之前
  ```



- 语法错误，指令书写格式有误，如果定义的事务中所包含的命令存在语法错误，整体事务中所有命令均不会执行，包括正确的命令
- 运行错误，指令格式正确，但是无法正确的执行。能够正确运行的命令执行，运行错误的命令不会执行
- 已经执行完毕的命令对应的数据不会自动回滚，需要自己在代码中实现回滚



### 锁

- 对key添加监视锁，在执行exec之前如果key发生了变化，终止事务执行

  ```
  watch key1 [key2...]
  ```

- 取消所有key的监视

  ```
  unwatch
  ```

### 分布式锁

- 使用setnx设置一个公共锁

  ```
  setnx lock-key value
  ```

  利用setnx命令的返回值特征，有值则返回设置失败，无值则返回设置成功

  - 利用返回设置成功的，拥有控制权，进行下一步的具体业务操作
  - 对于返回设置失败的，不具有控制权，排队或等待

  操作完毕通过del删除锁

### 分布式锁改良

- 使用 expire为锁key添加时间限定，到时不释放，放弃锁

  ```
  expire lock-key second
  pexpire lock-key milliseconds
  ```

## 过期数据

### 数据删除策略

1. 定时删除

   - 创建一个定时器，当key设置有过期时间，且过期时间到达时，由定时器任务立即执行对键的删除操作
   - 优点：节约内存，到时就删除，快速释放掉不必要的内存占用
   - 缺点：CPU压力大，无论CPU此时负载量多高，均占用CPU，会影响redis服务器相应时间和指令吞吐量

2. 惰性删除

   - 数据到达过期时间，不做处理，等下次访问该数据时
     - 如果未过期，返回数据
     - 发现已过期，删除，返回不存在
   - 优点：节约CPU性能，发现必须删除的时候才删除
   - 缺点：内存压力大，出现长期占用内存的数据

3. 定期删除

   - 周期性的轮询redis库中的时效性数据，采用随机抽取的策略，利用过期数据占比的方式控制删除频度
   - 特点1：CPU性能占用设置有峰值，检测频度可自定义设置
   - 特点2：内存压力不是很大，长期占用内存的冷数据会被持续清理

   

## 逐出算法

- redis使用内存存储数据，在执行每一个命令前，会调用freeMemorylfNeeded()检测内存是否充足，如果内存不满足新加入数据的最低存储要求，redis要临时删除一些数据为当前指令清理存储空间，清理数据的策略成为逐出算法

- 最大可使用内存

  ````
  maxmemory
  占用物理内存的比例，默认值为0，表示不限制
  ````

- 每次选取带删除数据的个数

  ```
  maxmemory-samples
  选取数据时不会全库扫描，会导致严重的性能消耗，降低读写性能，因此采用随机获取数据的方式
  ```

- 删除策略

  ```
  maxmemory-policy
  到达最大内存后，对被挑出的数据进行删除的策略
  - 影响数据逐出的相关配置
  - 检测易失数据（可能会过期的数据集）
    - volatile-lru：挑选最近最少使用的数据淘汰
    - volatile-lfu：挑选最近使用次数最少的数据淘汰
    - volatile-ttl：挑选将要过期的数据淘汰
    - volatile-random：任意选择数据淘汰
  - 检测全库数据
    - allkeys-lru：挑选最近最少使用的数据淘汰
    - allkeys-lfu：挑选最近使用次数最少的数据淘汰
    - allkeys-random：任意选择数据淘汰
  - 放弃数据驱逐
    - no-enviction：禁止驱逐数据（redis4.0默认策略），会引发错误(Out of Memory)
  ```



## 服务器基础配置

### 服务器端设定

- 设置服务器是否已守护进程的方式运行

  ```
  daemonize yes | no
  ```

- 绑定主机地址

  ```
  bind 127.0.0.1
  ```

- 设置服务器端口

  ```
  port 6379
  ```

- 设置数据库数量

  ```
  databases 16
  ```

### 日志配置

- 设置服务器以指定日志记录级别

  ```
  loglevel debug | verbose | notice | waring
  ```

- 日志记录文件名

  ```
  logfile 端口号.log
  ```

  

### 客户端配置

- 设置同一时间最大客户端连接数，默认无限制，当客户端连接到达上限，redis会关闭新的连接

  ```
  maxclients 0
  ```

- 客户端闲置等待最大时长，到达最大值后关闭连接，如需关闭功能，设置为0

  ```
  timeout 300
  ```

  

### 多服务器快捷配置

- 导入并加载指定配置文件信息，用于快速创建redis公共配置较多的redis实例配置我呢见，便于维护

  ```
  include /path/server-端口号.conf
  ```


## Bitmaps

### 基础操作

- 获取指定key对应偏移量上的bit值

  ```
  getbit key offset
  ```

- 设置指定key对应偏移量上的bit值，value只能是1或0

  ```
  setbit key offset value
  ```

### 拓展操作

- 对指定key按位进行交、并、非、异或操作，并将结果保存到destKey中

  ```
  bitop op destKey key1 [key2...]
  ```

  - and：交
  - or：并
  - not：非
  - xor：异或

- 统计指定key中1的数量

  ```
  bitcount key [start end]
  ```

  

## HyperLogLog

### 基本操作

- 添加数据

  ```
  pfadd key element [element...]
  ```

- 统计数据

  ```
  pfcount key [key...]
  ```

- 合并数据

  ```
  pfmerge destkey sourcekey [sourcekey...]
  ```

  

## GEO

### 基本操作

- 根据坐标求范围内的数据

  ```
  georadius key longitude latitude radius m|km|ft|mi [withcoord] [withdist] [withhash] [count count]
  ```

- 根据点求范围内数据

  ```
  georadiusbymember key member radius m|km|ft|mi [withcoord] [withdist] [withhash] [count count]
  ```

- 获取指定点对应的坐标hash值

  ```
  geohash key member [member...]
  ```

  

## 主从复制

### 基本介绍

主从复制即将master中的数据即使、有效的复制到slave中

特征：一个master可以拥有多个slave，一个slave只对应一个master

职责：

- master
  - 写数据
  - 执行写操作，将出现变化的数据自动同步到slave
  - 读数据（可忽略）
- slave
  - 读数据
  - 写数据（禁止）

### 主从复制的作用

- 读写分离：master写，slvae读，提高服务器的读写负载能力
- 负载均衡：基于主从结构，配合读写分离，有slvae分担master负载，并根据需求的变化，改变slave的数量，通过多个从节点分担数据读取负载，大大提高Redis服务器并发量与数据吞吐量
- 故障恢复：当master出现问题时，有slvae提供服务，实现快速的故障恢复
- 数据冗余：实现数据热备份，是持久化之外的一种数据冗余方式
- 高可用基石：基于主从复制，构建哨兵模式与集群，实现Redis的高可用方案

### 主从连接（slave连接master）

- 客户端发送命令

  ```
  slaveof <masterip> <masterport>
  ```

- 启动服务器参数

  ```
  redis-server -slaveof <masterip> <masterport>
  ```

- 服务器配置

  ```
  slaveof <masterip> <masterport>
  ```

- slave系统信息

  - master_link_down_since_seconds
  - masterhost
  - masterport

- master系统信息

  - slave_listening_port（多个）

### 主从断开连接

- 客户端发送命令

  ```
  slaveof no one
  ```

### 授权访问

- master配置文件设置密码

  ```
  requirepass <password>
  ```

- master客户端发送命令设置密码

  ```
  config set requirepass <password>
  config set requirepass
  ```

- slave客户端发送命令设置密码

  ```
  auth <password>
  ```

- slave配置文件设置密码

  ```
  masterauth <password>
  ```

- 启动客户端设置密码

  ```
  redis-cli -a <password>
  ```



### 数据同步

master

1. 如果master数据量巨大，数据同步阶段应避开流量高峰期，避免造成master阻塞，影响业务正常执行

2. 复制缓冲区大小设定不合理，会导致数据溢出，如进行全量复制周期太长，进行部分复制时发现数据已经存在丢失的情况，必须进行第二次全量复制，致使slave陷入死循环状态

   ```
   repl-backlog-size 1mb
   ```

3. master单机内存占用主机内存的比例不应过大，建议使用50%-70%，留下剩下的用于执行bgsave命令和创建复制缓冲区

slave

1. 为避免slave进行全量复制、部分复制时服务器响应阻塞或数据不同步，建议关闭此期间的对外服务

   ```
   slave-serve-status-data yes|no
   ```

2. 数据同步阶段，master发送给slave信息可以理解master是slave的一个客户端，主动向slave发送命令

3. 多个slave同时对master请求数据同步，master发送的RDB文件增多，会对宽带造成巨大冲击，如果master带宽不足，数据同步需要根据业务需求，适量错峰

### 服务器运行ID（runid)

- 概念：服务器运行id时每一台服务器每次运行的身份识别码，一台服务器多次运行可以生成多个运行id
- 运行id由40位字符组成，是随机的十六进制字符
- 运行id被用于在服务器间进行传输，识别身份，如果想两次操作均对同一台服务器进行，必须每次操作携带对应的运行id，用于对方识别
- 运行id在每台服务器启动时自动生成，master在首次连接slave时，会将自己的运行id发送给slave，slvae保存此id

### 复制缓冲区

- 复制缓冲区，又名复制积压缓冲区，是一个先进先出的队列，用于存储服务器执行过的命令，每次传播命令，master都会将传播的命令记录下来，并存储在复制缓冲区
  - 复制缓冲区默认数据存储空间大小是1M，由于存储空间大小是固定的，当入队元素的数量大于队列长度时，最先入队的元素会被弹出，而新元素会被放入队列
  - 每台服务器启动时，如果开启AOF或被连接成为master节点，即创建复制缓冲区
  - 作用：用来保存master收到的所有指令（仅影响数据变更的指令）
  - 数据来源：当master接收到著客户端的指令时，除了将指令执行，会将该指令存储到缓冲区中

### 复制偏移量

- 一个数字，描述复制缓冲区的至零字节位置
- 分类：
  - master复制偏移量：记录发送给所有slave的指令字节对应的位置
  - slave复制偏移量：记录slave接受master发送过来的指令字节对应的位置
- 数据来源：
  - master端：发送一次记录一次
  - slave端：接受一次记录一次
- 作用：同步信息，比对master与slave的差异，当slave断线后，恢复数据使用



### 心跳机制

- 进入命令传播阶段，master与slave间需要进行信息交换，使用心跳机制进行维护，实现双方连接保持在线
- master心跳：
  - 指令：PING
  - 周期：由repl-ping-slave-period决定，默认10秒
  - 作用：判断slave是否在线
  - 查询：INFO replication 获取slave最后一次连接时间间隔，
- slave心跳任务
  - 指令：REPLCONF ACK
  - 周期：1秒
  - 作用1：汇报slave自己的复制偏移量，获取最新的数据变更指令
  - 作用2：判断master是否在线

注意事项：

- 当slave多数掉线，或延迟过高时，master为保障数据稳定性，将拒绝所有信息同步操作

  ````
  min-slaves-to-write 2
  min-slaves-max-lag 8
  ````

  slave数量少于2个，或者所有slave的延迟都大于等于10秒，强制关闭master写功能，停止数据同步

- slave数量由slave发送REPLCONF ACK命令做确认

- slave延迟由slave发送REPLCONF ACK命令做确认



## 哨兵

哨兵是一个分布式系统，用于对主从结构中的每台服务器进行监控，当出现故障时通过投票机制选择新的master并将所有slave连接到新的master

### 哨兵的作用

- 监控
  - 不断的检查master和slave是否正常运行
  - master存活检测、master与slave运行情况检测
- 通知（提醒）
  - 当被监控的服务器出现问题时，向其他（哨兵间、客户端）发送通知
- 自动故障转移
  - 断开master与slave连接，选取一个slave作为master，将其他slave连接到新的master，并告知客户端新的服务器地址
- 注意
  - 哨兵也是一台redis服务器，只是不提供数据服务
  - 通常哨兵配置数量为单数

### 配置哨兵

- 启动哨兵

  ```
  redis-sentinel 配置文件
  ```



## 集群

### 集群作用

- 分散单台服务器的访问压力，实现负载均衡
- 分散单台服务器的存储压力，实现可扩展性
- 降低单台服务器宕机带来的业务灾难
