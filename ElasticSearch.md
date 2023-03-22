# ElasticSearch

## 数据格式

Elasticsearch是面向文档型数据库，一条数据在这里就是一个文档



## 索引

索引在ES中所表述的含义和MSQL中的索引完全不同，在MySQL中索引指的是加速数据查询的一种特殊的数据结构，如normal index。
而在ES中，索引表述的含义等价于MySQL中的表（仅针对ES7.x以后版本），只是类比去理解，索引并不等于表。

### 索引的组成部分

- alias：索引别名
- settings：索引设置，常见设置如分片和副本数量等
- mapping：映射，定义了索引中包含哪些字段，以及字段的类型、长度、分词器等

### 索引的含义

在 ES 中，索引在不同的特定条件下可以表示三种不同的意思

- 表示源文件数据：当做数据的载体，即类比为数据表，通常称作idex。例如：通常说集群中有product索引，即表述当前ES的服务中存储了product这样一张“表”。
- 表示索引文件：以加速查询检索为目的而设计和创建的数据文件，通常承载于某些特定的数据结构，如哈希、FST等。例如：通常所说的正排索引和倒排索引（也叫正向索引和反向索引)。就是当前这个表述，索引文件和源数据是完全独立的，索引文件存在的目的仅仅是为了加快数据的检索，不会对源数据造成任何影响，
- 表示创建数据的动作：通常说创建或添加一条数据，在ES的表述为索引一条数据或索引一条文档，或者index一个doc进去。此时索引一条文档的含义为向索引中添加数据。



## 类型：Type（ES 7.x 之后版本已经删除此概念）

### 类型的基本概念

从 Elasticsearch 的第一个版本开始，每个文档都存储在一个索引中并分配一个映射类型。映射类型用于表示被索引的文档或实体的类型，例如 product 索引可能具有 user 类型和 order 类型，每个映射类型都可以有自己的字段，因此该user类型可能有一个user_name字段、一个title字段和一个 email 字段，而该order类型可以有一 content 字段、一个 title 字段，并且和 user 类型一样，也有一个 user_name 字段
每个文档都有一个`_type`包含类型名称的元数据字段，通过在URL中指定类型名称，可以将索引限制为种或多种类型

### 为什么删除 type 的概念

- 逻辑不合理：然而这是错误的类比，官方后来也意识到了这是个错误。在SQL数据库中，表是相互独立的。一个表中的列与另一个表中的同名列无关。对于映射类型中的字段，情况并非如此。
- 数据结构混乱：在Elasticsearch索引中，不同映射类型中具有相同名称的字段在内部由相同的Lucene字段支持。换句话说，使用上面的示例，类型中的 user_name 字段与 user 和 order 类型中的字段存储在完全相同的 user_name 字段中，并且两个 user_name 字段在两种类型中必须具有相同的映射（定义）
- 影响性能：最重要的是。在同一索引中存储具有很少或没有共同字段的不同实体会导致数据稀疏并干扰Lucene有效压缩文档的能力。

## 文档

### 元数据：meta data

所有的源字段均已下划线开头，为系统字段

- _index：索引名称
- _id：文档id
- _version：版本号
- _seq_no：索引级别的版本号，索引中所有文档共享一个版本号
- _primary_term：是一个整数，每当 Primary Shard 发生重新分配时，比如节点重启， Primary 选举或重新分配等，都会递增1，主要作用是用来回复数据时处理当多个文档的 _seq_no 一样时的冲突，避免 Primary Shard 上的数据写入被覆盖

### 源数据：source data

最终写入的用户数据

## API

### Search API

```http
GET /{index}/_search
```

可选参数

- size：单次查询多少条文档，默认为10
- from：起始文档偏移量，需要为非负数，默认为0
- timeout：指定等待每个分片响应的时间段，如果在超时前未收到响应，则请求失败并返回错误。默认为无超时

### Index API

#### 基本API

```http
PUT /{index}
```

#### 创建时指定 settings

```http
PUT /{index}
{
	"settings": {
		"number_of_shards": 1, // 主分片数量为1
		"number_of_replicas": 1 // 每个主分片分配一个副本
	}
}
```

#### 修改 settings

使用 _settings 只能修改允许动态修改的配置项

```http
PUT /{index}/_settings
```

#### 静态索引设置

只能在创建索引时或者关闭状态的索引上设置

**重要的静态配置**

- index.number_of_shards

  索引的主分片的个数，默认为1，此设置只能在创建索引时设置

  每个索引的分片的数量上限为1024，这是一个安全限制，以防止意外创建索引，这些索引可能因资分配而破坏集群的稳定性。`export ES_JAVA_OPTS="-Des.index.max_number_of_shards=128"`可以通过在属集群的每个节点上指定系统属性来修改限制

#### 动态索引设置

既可以使用 _settings API 在实时修改的配置项

**重要的动态设置**

- `index.number_of_replicas:` 每个主分片的副本数。默认为1，允许配置为0
- `index.refresh_interval:` 执行别新操作的频率，默认为ls.可以设置-1为禁用剧新
- `index.max_result_window:` from+size搜索此索引的最大值。默认为10000。搜索
  请求占用堆内存和时间 from+size ，这限制了内存

#### 删除索引

```http
DELETE /{index}
```

#### 索引的不可变性

ES 索引创建成功之后，以下属性将不可修改

- 索引名称
- 主分片数量
- 字段类型

### Document API

#### 文档的操作类型

ES 对文档操作类型的定义

- index：索引（动词），可以是创建，也可以是全量替换
- create：不存在则创建，存在则报错

> 注意
>
> - 以上操作均为写操作
> - ES 中的数据写入均发生在 Primary Shard
> - 当操作对象为数据流时，op_type 必须为 create

#### Create：创建

如果在PUT数据的时候当前数据已经存在，则数据会被覆盖，如果在PUT的时候指定操作类型 create ,此时如果数据已存在则会返回失败，因为已经强制指定了操作类型为 create ,ES 就不会再去执行 update 操作。
比如：下面一行代码创建 id 为1的文档，指的就是在索引 goods 中强制创建 id 为1的文档，如果id为1的文档已存在，则返回失败。指定创建文档id

```http
PUT /{index}/_doc/{id}?op_type=create

// 可简化为
PUT /{index}/_create/{id}
```

#### Index：索引（创建或全量更新文档）

在 ES 中，写入操作被称为 Index，这里 Index 为动词，及索引数据为将数据创建在 ES 中的索引，写入数据亦可称为“索引数据”。可以是创建，也可以是全量替换

```http
PUT /{index}/_doc/{id}?op_type=index

// 可简化为
PUT /{index}/_doc/{id}
```

#### 文档的 CRUD

基本语法

```http
PUT /{index}/_doc/{id}

PUT /{index}/_create/{id}

POST /{index}/_create/{id}
```

##### 创建文档

```http
PUT /{index}/_doc/{id}
{
...
}
```

##### 查询文档

查询指定 id 的文档

```http
GET /{index}/_doc/{id}
```

判断指定 id 的文档是否存在

```
HEAD /{index}/_doc/{id}
```

_source

使用 _source 可以打开或者关闭源数据字段，true 为打开， false 为关闭，默认为 true

```http
GET /{index}/_doc/{id}?_source=false
```

当添加了 `_source=false` 之后，返回结果仅返回元数据字段

也可以之查看 _source 字段，而不查看任何 meta data

```http
GET /{index}/_source/{id}
```



##### 删除文档

```http
// 删除指定 id 的文档，Document DELETE API 必须指定 id
DELETE /{index}/_doc/{id}
```

> 7.x 及以前版本的语法 `POST /{index}/_doc/{id}/_update` 已不再支持

##### 修改文档

修改部分字段

```http
POST /{index}/_update/{id}
{
	"doc": {
		...
	}
}
```



## Rest 接口

### 索引

#### 添加索引

```http
PUT URL/{index}
```

#### 查询索引信息

```http
GET URL/{index}
```

#### 查询所有索引

```http
GET URL/_cat/indices?v
```

#### 删除索引

```http
DELETE URL/{index}
```

### 文档

#### 添加文档

```http
POST URL/{index}/_doc
```

#### 添加文档并自定义id

```http
POST URL/{index}/_doc/{id}
PUT URL/{index}/_doc/{id}
PUT URL/{index}/_create/{id}
```

#### 根据id查询文档

```http
GET URL/{index}/_doc/{id}
```

#### 查询全部文档

```http
GET URL/{index}/_search
```

#### 全量数据更新

```http
PUT URL/{index}/_doc/{id}
```

#### 局部数据更新

```http
PUT URL/{index}/_doc/{id}

{
	"doc": data
}
```

#### 删除文档

```http
DELETE URL/{index}/_doc/{id}
```

#### 条件查询

```http
GET URL/{index}/_search?q=k:v
```

```http
GET URL/{index}/_search

{
	"query": {
		"match": {
			field: val
		}
	}
}
```

####  查询全部

```http
GET URL/{index}/_search

{
	"query": {
		"match_all": {}
	}
}
```

#### 分页查询

```http
GET URL/{index}/_search

{
	"from": v,
	"size": v
}
```

#### 查询指定字段

```http
GET URL/{index}/_search

{
	"_source": [field...]
}
```

#### 排序

```http
GET URL/{index}/_search

{
	"sort": {
		field: {
			"order": "desc" | "asc"
		}
	}
}
```

#### 多条件查询

```http
GET URL/{index}/_search

{
	"query": {
		"bool": {
		// must 等同于 sql and
			"must": [
				{
					"match": {field: val}
				},
				{
					"match": {field: val}
				}
			],
			// 等同于 sql or
			"should": [],
			// 范围查询
			"filter": {
				"range": {
					field: {
						cond: val
					}
				}
			}
		}
	}
}
```

#### 完全匹配

```http
GET URL/{index}/_search

{
	"query": {
		"match_phrase": {
			field: val
		}
	}
}
```

#### 高亮显示匹配结果

```http
GET URL/{index}/_search

{
	"highlight": {
		"fields": {
			field: {}
		}
	}
}
```

#### 聚合操作

```
GET URL/{index}/_search

{
	"aggs": {
		// name 统计结果名称
		name: {
			// 分组
			"terms": {
				"field": val // 分组字段
			},
			// 平均值
			"avg": {
				"field": val
			}
		}
	}
}
```

### 映射

#### 添加映射

```http
POST URL/{index}/_mapping

{
	"properties": {
		field: {}
	}
}
```

#### 查询映射

```http
GET URL/{index}/_mapping
```



## 集群

### 角色

#### 常用角色

- 主节点（active master）：一般指活跃的主节点，一个集群中只能有 一个，主要作用是对集群的管理
- 候选节点（master-eligible）：当主节点发生故障时，参与选举，也就是主节点的替代节点
- 数据节点（data node）：数据节点保存包含已编入索引的文档的分片。数据节点处理数据相关操作，如CRUD、搜索和聚合。这些操作是I/O密集型、内存密集型和CPU密集型的。监控这些资源并在它们过载时添加更多数据节点非常重要
- 预处理节点（ingest node）：预处理节点有点类似于logstash的消息通道，所以也叫ingest pipeline，常用于一些数据写入之前的预处理操作
