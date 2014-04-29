---
  layout: default
  title: Solr and Term
  tags: [solr]
---

Solr计算a query item的score时受两个因素的影响：
1、Lucene的算分模型
2、Boost


Lucene的算分模型: 
1、TF（Term Frequency）关键字频率： 即关键字在文档中（或目标字段中）出现的频率。
计算公式是： Term在文档中出现的次数 / 文档中所有词出现的次数之和
2、IDF（Inverse Document Frequency）逆向文档频率。

计算公式是：（系统中的）总文档数目 / 包含目标关键字的文档数目。
TF-IDF：某一特定文件内的高词语频率，以及该词语在整个文件集合中的低文件频率，可以产生出高权重的TF-IDF。

FieldNorm（Filed Norm）： 由两个因素决定： 字段长度 和 Boost。 字段的长度越长，它的权重越低。
Boost可以在 Index-time or Query-time 指定。

coord（Coordination Factor），当有多个查询关键字时，匹配的关键字个数越多，得分越高。



reference links:
http://lucene.apache.org/core/4_7_1/index.html
https://cwiki.apache.org/confluence/display/solr/Relevance


if(exists(project_id),1,0) asc