# 索引

## 索引分类

- 单值索引：一个索引只包含单个列，一个表可以有多个单列索引
- 唯一索引：索引的值必须唯一，但允许有空值

- 复合索引：一个索引包含多个列

## 创建索引

- create [unique] index index_name on table_name (column_name[length])
- alter [unique] table table_name add index index_name (column_name[length])

- 使用unique表示创建唯一索引
- 如果是char，varchar类型，length可以小于字段实际长度，如果是blob、text类型，必须执行length

## 删除索引

- drop index [index_name] on table_name;

## 查看索引

- show index from table_name;

## 索引结构

- BTree索引
- Hash索引

- full-text全文索引
- R-Tree索引

## 适合创建索引的场景

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该创建索引

- 查询中与其他表关联的字段，外键关系建立索引
- 查询中排序的字段

- 查询中统计或者分组的字段

## 不适合创建索引的场景

- 频繁更新的字段不适合创建索引
- where条件里用不到的字段不创建索引

- 表记录太少
- 数据重复且分布平均的表字段

# 分析Sql语句

## explain

### 使用

explain + Sql语句 分析sql的执行计划

### 作用

- 表的读取顺序
- 数据读取操作的操作类型

- 哪些索引可以使用
- 哪些索引被实际使用

- 表之间的引用
- 每张表有多少行被优化器查询

### 执行计划包含的信息

| id   | select_type | table | type | possible_keys | key  | key_len | ref  | rows | Extra |
| ---- | ----------- | ----- | ---- | ------------- | ---- | ------- | ---- | ---- | ----- |

#### id

- id相同，执行顺序由上至下
-  id不同，id越大的优先级越高

#### select_type

- simple：简单的select查询，查询中不包含任何子查询或者union
- primary：查询中包含包含任何复杂的子查询，最外层查询则被标记为primary
- subquery：在select或where列表中包含了子查询
- derived：在from列表中包含的子查询被标记为derived，mysql会递归执行这些子查询，把结果放在临时表里
- union：若第二个select出现在union之后，则被标记为union；若union包含在from子句的子查询，外层select将被标记为 derived
- union result：从union表获取结果的select

#### type

显示查询使用了哪种类型

从最好到最差：system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > all

一般来说，要保证查询能达到range级别，最好能达到ref

- system：表只有一行记录，是const类型的特例
- const：表示通过索引一次就能找到
- eq_ref：唯一性索引扫描，对于每个索引键，表中只有一行记录与之匹配，常见于主键或唯一索引
- ref：非唯一性索引扫描，返回匹配某个单独值的所有行，本质上是一种索引访问，返回所有匹配某个单独值的行，可能会找到多个符合条件的行，属于查找和扫描的混合体
- range：只检索给定范围的行，使用一个索引来选择行，key列显示使用了哪个索引，一般就是在where语句汇中出现了between、< 、 >等查询
- index：full index scan，index与all区别为index类型只遍历索引树，通过比all快
- all：full table scan，将遍历全表以找到匹配的行

#### possible_keys

显示可能在这张表中用到的索引，一个或多个，但不一定被实际查询所使用

#### key

实际使用的索引，如果为null，则没有使用索引，查询中若使用了覆盖索引，则该索引仅出现在key列表中

#### key_len

表示索引中使用的字节数，可通过该列计算查询中使用的索引长度。在不损失精度的情况下，长度越短越好。