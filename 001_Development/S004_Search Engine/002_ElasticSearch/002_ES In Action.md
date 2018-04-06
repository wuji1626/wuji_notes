#ES实战
[TOC]

##1 ES数据类型

index定义字段的分析类型以及检索方式：
- 如果是no，则无法通过检索查询到该字段；
- 如果设置为not_analyzed则会将整个字段存储为关键词，常用于汉字短语、邮箱等复杂的字符串；
- 如果设置为analyzed则将会通过默认的standard分析器进行分析

store定义了字段是否存储，ES中原始的文本会存储在_source里面（除非你关闭了它）。默认情况下其他提取出来的字段都不是独立存储的，是从_source里面提取出来的。当然你也可以独立的存储某个字段，只要设置store:true即可。

###1 String
字符串类型  
比较重要的参数：

index分析：
- analyzed(默认)
- not_analyzed
- no
store存储：
- true 独立存储
- false（默认）不存储，从_source中解析

###2 Numeric
数值类型，注意numeric并不是一个类型，它包括多种类型，比如：long,integer,short,byte,double,float，每种的存储空间都是不一样的，一般默认推荐integer和float  

index分析
- not_analyzed(默认) ，设置为该值可以保证该字段能通过检索查询到
- no
store存储
- true 独立存储
- false（默认）不存储，从_source中解析

###3 date
日期类型，该类型可以接受一些常见的日期表达方式  
重要的参数：

index分析
- not_analyzed(默认) ，设置为该值可以保证该字段能通过检索查询到
- no

store存储
- true 独立存储
- false（默认）不存储，从_source中解析

format格式化
strict_date_optional_time||epoch_millis（默认）
你也可以自定义格式化内容，比如
~~~
"date": {
  "type":   "date",
  "format": "yyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
}
~~~

###4 IP
这个类型可以用来标识IPV4的地址
常用参数：

index分析
- not_analyzed(默认) ，设置为该值可以保证该字段能通过检索查询到
- no

store存储
- true 独立存储
- false（默认）不存储，从_source中解析

###5 boolean
布尔类型，所有的类型都可以标识布尔类型
- False: 表示该值的有:false, "false", "off", "no", "0", "" (empty string), 0, 0.0
- True: 所有非False的都是true

重要的参数：

index分析
- not_analyzed(默认) ，设置为该值可以保证该字段能通过检索查询到
- no

store存储
- true 独立存储
- false（默认）不存储，从_source中解析

##2创建索引
###1 简单创建
~~~
curl -XPUT [address]/blog/_mapping/article?pretty -d '{

         "properties":{

                   "id":{"type":"long"},

                   "name":{"type":"string"},

                   "published":{"type":"date"}

         }

}'
~~~
###2 带参数的创建语句
~~~
curl -XPUT [address]/blog/_mapping/article?pretty -d '{

         "dynamic":"false",  //关闭自动添加字段，关闭后索引数据中如果有多余字段不会修改mapping,默认true

         "_id":{"index":"not_analyzed","store":"no"},        //设置文档标识符可以被索引，默认不能被索引。可以设置为"_id":{"path":"book_id"},这样将使用字段book_id作为标识符

         "_all":{"enabled":"false"},       //禁用_all字段，_all字段包含了索引中所有其他字段的所有数据，便于搜索。默认启用

         "_source":{"enabled":"false"},        //禁用_source字段，_source字段在生成索引过程中存储发送到elasticsearch的原始json文档。elasticsearch部分功能依赖此字段(如局部更新功能),因此建议开启。默认启用

         "_index":{"enabled":"true"},  //启用_index字段，index字段返回文档所在的索引名称。默认关闭。

         "_timestamp":{"enabled":"true","index":"not_analyzed","store":"true","format":"YYYY-mm-dd"},                  //启用时间戳并设置。时间戳记录文档索引时间，使用局部文档更新功能时，时间戳也会被更新。默认未经分析编入索引但不保存。

         "_ttl":{"enabled":"true","default":"30d"},     //定义文档的生命周期,周期结束后文档会自动删除。

         "_routing":{"required":"true","path":"name"}      //指定将name字段作为路由，且每个文档必须指定name字段。

        

         "properties":{

                   "id":{

                            "type":"long",

                           

                            //公共属性

                            "store":"yes",

                           

                            //数值特有属性

                            "precision_step":"0"       //指定为该字段生成的词条数，值越低，产生的词条数越多，查询会更快，但索引会更大。默认4

                           

                   },

                   "name":{

                            "type":"string",

                           

                            //公共属性

                            "store":"yes",

                            "index":"not_analyzed", //analyzed:编入索引供搜索、no:不编入索引、not_analyzed(string专有):不经分析编入索引

                            "boost":"1",    //文档中该字段的重要性，值越大表示越重要，默认1

                            "null_value":"jim",  //当索引文档的此字段为空时填充的默认值，默认忽略该字段

                            "include_in_all":"xxx"      //此属性是否包含在_all字段中,默认为包含

                           

                            //字符串特有属性

                            "analyzer":"xxx",     //定义用于索引和搜索的分析器名称，默认为全局定义的分析器名称。可以开箱即用的分析器:standard,simple,whitespace,stop,keyword,pattern,language,snowball

                            "index_analyzer":"xxx",           //定义用于建立索引的分析器名称

                            "search_analyzer":"xxx",        //定义用于搜索时分析该字段的分析器名称

                            "ignore_above":"xxx"      //定义字段中字符的最大值，字段的长度高于指定值时，分析器会将其忽略

                           

                           

                   },

                   "published":{

                            "type":"date",

                           

                            //公共属性

                            "store":"yes",

                           

                            //日期特有属性

                            "precision_step":"0",      //指定为该字段生成的词条数，值越低，产生的词条数越多，查询会更快，但索引会更大。默认4

                            "format":"YYYY-mm-dd"          //指定日期格式，默认为dateOptionalTime

                   }

         }

}'
~~~
