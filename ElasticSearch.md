# ElasticSearch

## 数据格式

Elasticsearch是面向文档型数据库，一条数据在这里就是一个文档



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

