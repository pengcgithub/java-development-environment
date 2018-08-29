# managed-schema（schema.xml）

## 简介

schema文件是在SolrConfig中的架构工厂定义，有两种定义模式：

1、默认的托管模式
solr默认使用的就是托管模式。也就是当在solrconfig.xml文件中没有显式声明<schemaFactory/>时，Solr隐式地使用ManagedIndexSchemaFactory，它是默认的"mutable"并将模式信息保存在一个managed-schema文件中。

2、经典schema.xml
这种模式的配置方式是在solrconfig.xml文件中显式配置一个ClassicIndexSchemaFactory。ClassicIndexSchemaFactory 需要使用schema.xml配置文件，并且不允许在运行时对架构进行任何编程式更改。该schema.xml文件必须手动编辑，仅在加载集合时才加载。
```
<schemaFactory class="ClassicIndexSchemaFactory"/>
```

## schema文件中各种标签的含义

```
<schema name="default-config" version="1.6"></schema>
这是schema文件的根节点，类似于html文件的<html></html>，只是schema文件还有一些属性。

<uniqueKey>id</uniqueKey>
指定某个字段是唯一的字段。

<fieldType ></fieldType>
定义字段类型。

<field  name=" " type=" "/>
定义字段。

<dynamicField name=" " type=" " indexed="true" stored="true"/>
定义动态字段。

<copyField source=" " dest=" " />
复制字段。
```

## 配置示例

```
  <!-- 
     fields各个属性说明:
     name: 必须属性 - 字段名
     type: 必须属性 - <types>中定义的字段类型 
     indexed: 如果字段需要被索引（用于搜索或排序），属性值设置为true
     stored: 如果字段内容需要被返回，值设置为true
     docValues: 如果这个字段应该有文档值（doc values），设置为true。文档值在门
           面搜索，分组，排序和函数查询中会非常有用。虽然不是必须的，而且会导致生成
           索引变大变慢，但这样设置会使索引加载更快，更加NRT友好，更高的内存使用效率。
           然而也有一些使用限制：目前仅支持StrField, UUIDField和所有 Trie*Fields, 
           并且依赖字段类型, 可能要求字段为单值（single-valued）的,必须的或者有默认值。
     multiValued: 如果这个字段在每个文档中可能包含多个值，设置为true
     termVectors: [false] 设置为true后，会保存所给字段的相关向量（vector）
           当使用MoreLikeThis时, 用于相似度判断的字段需要设置为stored来达到最佳性能.
     termPositions: 保存和向量相关的位置信息，会增加存储开销 
     termOffsets: 保存 offset 和向量相关的信息，会增加存储开销
     required: 字段必须有值，否则会抛异常
     default: 在增加文档时，可以根据需要为字段设置一个默认值，防止为空
   -->
```

- 配置参数

```
<field name="id" type="text_ik" indexed="true" stored="true" />
<field name="name" type="string" indexed="true" stored="true" />
<field name="copyFieldName" type="text_ik" indexed="true" stored="true" multiValued="true" />
<copyField source="firstTypeName" dest="copyFieldName" />
<copyField source="brandName" dest="copyFieldName" />
```

- 出参格式

```
{
  "responseHeader":{
    "status":0,
    "QTime":20,
    "params":{
      "mm":"100%",
      "q":"*:*",
      "defType":"edismax",
      "start":"6780",
      "rows":"10",
      "_":"1533567295626"}},
  "response":{"numFound":1,"start":1,"docs":[
      {
        "id":"2017121517108604C96BE17B11E7834E",
        "name":"轮挖EX125W",
        "queryKeywords":[
		  "轮挖EX125W",
          "2017121517108604C96BE17B11E7834E"]
	  }
  ]
}
```

## 参考资料
- [https://blog.csdn.net/weixin_39082031/article/category/7370960](https://blog.csdn.net/weixin_39082031/article/category/7370960)