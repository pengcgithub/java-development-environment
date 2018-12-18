# solr客户端使用说明

通过`http://127.0.0.1:32080/solr/index.html`地址直接solr客户端界面，如果当前还没有安装则可以参考[solr安装](./solr安装.md)。

以下为solr客户端的界面图，图中已经标识出常用的几类菜单。在访问下列菜单之前，首先需要选择一个索引库。

![](https://i.imgur.com/nuYiZXQ.png)

## Query

![](https://i.imgur.com/byCLsWC.png)

- q：查询的关键字，此参数最为重要。例如q=id:1。如果为多个q=id:(1 OR 2) AND name:张三
- fq:（filter query）过虑查询，提供一个可选的筛选器查询。返回在q查询符合结果中同时符合的fq条件的查询结果，例如：q=id:1&fq=sort:[1 TO 5]，找关键字id为1 的，并且sort是1到5之间的。
- sort：排序方式，例如id desc 表示按照 “id” 降序
- start, rows：分页
- fl：指定返回哪些字段，用逗号或空格分隔，注意：字段区分大小写，例如，fl= id,title,sort
- df：默认的查询字段，一般默认指定。
- Raw Query Parameters：
- wt：输出的数据类型
- indent off：
- debugQuery：开启debug查询
- dismax
	- q.alt
	- qf
	- mm
- edismax
	- q.alt
	- qf
	- mm
- stopwords
- lowercaseOperators
- h1：是否高亮，hl=true，表示采用高亮
	- hl.fl：设定高亮显示的字段，用空格或逗号隔开的字段列表。要启用某个字段的highlight功能，就得保证该字段在schema中是stored。如果该参数未被给出，那么就会高亮默认字段 standard handler会用df参数，dismax字段用qf参数。你可以使用星号去方便的高亮所有字段。如果你使用了通配符，那么要考虑启用hl.requiredFieldMatch选项。
	- hl.simple.pre
	- hl.simple.post
	- hl.requireFieldMatch：如果置为true，除非用hl.fl指定了该字段，查询结果才会被高亮。它的默认值是false。
	- hl.usePhraseHighlighter：如果一个查询中含有短语（引号框起来的）那么会保证一定要完全匹配短语的才会被高亮。
	- hl.highlightMultiTerm：如果使用通配符和模糊搜索，那么会确保与通配符匹配的term会高亮。默认为false，同时hl.usePhraseHighlighter要为true。
- facet：是否分组
	- facet.query：
	- facet.field：分组的字段
	- facet.prefix：表示Facet字段前缀
- spatial
- spellcheck

## Analysis

## Dataimport

## 参考资料

- [https://blog.csdn.net/yelllowcong/article/details/78709435](https://blog.csdn.net/yelllowcong/article/details/78709435)