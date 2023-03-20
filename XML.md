# XML

## 概念：

Extensible Markup Language 可拓展标记语言

可拓展：标签都是自定义的

功能：存储数据

1. 配置文件
2. 在网络中传输

## 基本语法：

1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明
3. xml有且仅有一个跟标签
4. 属性值必须使用引号（单双都可）引起来
5. 标签必须正确关闭
6. xml标签名称区分大小写

## 组成部分：

1. 文档声明

    ```xml
    <?xml 属性列表 ?>
    属性列表
    version：版本号，必须的属性
    encoding：编码方式，告知解析引擎当前文档使用的字符串。默认：ISO-8869-1
    standalone：是否独立。取值：yes 不依赖其他文件 no 依赖其他文件
    ```

2. 指令：结合css

3. 标签：标签名称自定义

    1. 规则：
        1. 名称可以包含字母、数字以及其他字符
        2. 名称不能以数字或标点符号开始
        3. 名称不能以字母xml（或者XML、Xml等）开始
            1. 名称不能包含空格

4. 属性

    1. id属性值唯一

5. 文本

    1. CDATE区：该区域中的数据会被原样展示

        1. 格式

            ```xml
            <![CDATE[ 数据 ]]>
            ```


## 约束：规定xml的编写规则

	1. 能够在xml中引入约束文档
	2. 能够简单的读懂约束文档

分类：

1. DTD：一种简单的约束技术

    1. 引入DTD文档到xml文档中
        1. 内部dtd：将约束规则定义在xml文档中
        2. 外部dtd：将约束规则定义在外部的dtd文档中
            1. 本地：<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
            2. 网络：<!DOCTYPE 跟标签名 PUBLIC “dtd文件名" “dtd文件位置URL">

2. Schema：一种复杂的约束技术

    引入：

    1. 填写xml文档的根元素
    2. 引入xsi前缀 `xmlns:xsi=""`
    3. 引入xsd文件命名空间 `xsi:schemaLocation=""`
    4. 为每一个xsd约束声明一个前缀，作为标识 `xmlns:前缀=""`

## 解析

操作xml文档，将文档中的数据读取到内存中

操作xml文档

1. 解析（读取）：将文档中的数据读取到内存中
2. 写入：将内存中的数据保存到xml文档中。持久化的存储

解析xml的方式：

1. DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树
    1. 优点：操作方便，可以对文档进行CRUD的所有操作
    2. 缺点：占内存
2. SAX：逐行读取，基于事件驱动
    1. 优点：不占内存
    2. 缺点：只能读取，不能增删改

xml常见的解析器：

1. JAXP：sum公司提供的解析器，支持dom和sax两种思想（了解）
2. DOM4J：一款非常优秀的解析器
3. Jsoup：html解析器，可直接解析某个URL地址，html文本内容
4. PULL：Android操作系统内置的解析器，sax方式

### Jsoup

1. Jsoup：工具类，可以解析html或xml文档，返回Document
2. Document：文档对象，代表内存中的dom树
3. Elements：元素Element对象的集合。可以当作ArrayList<Element> 来使用
4. ELement：元素对象
5. Node：节点对象


