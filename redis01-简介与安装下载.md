# Redis是什么
Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets.
redis是开源,BSD许可,高级的key-value存储系统. 
可以用来存储字符串,哈希结构,链表,集合,因此,常用来提供数据结构服务.

redis和memcached相比,的独特之处:
1:redis可以用来做存储(storge), 而memccached是用来做缓存(cache)
  这个特点主要因为其有”持久化”的功能.
2: 存储的数据有”结构”,对于memcached来说,存储的数据,只有1种类型--”字符串”,
  而redis则可以存储字符串,链表,哈希结构,集合,有序集合.

# Redis基本特点
o 速度快
Redis使用C编写，能支持10万每秒的读写频率。
o 数据结构丰富
可以将Redis看做“数据结构服务器”。目前，Redis支持5种
数据结构。
o 持久化
支持数据持久化存储。
o 主-从复制
Redis支持简单而快速的主-从复制。
o 丰富的特性
支持publish/subscribe, key过期等特性

# Redis使用方式与应用场景
o Redis两种使用方式
缓存系统
存储系统
o Redis应用场景
取最新N个数据的操作；
排行榜应用，取TOP N操作；
精准设定过期时间的应用；
计数器应用；
数据排重；
o Redis两种使用方式
缓存系统
存储系统
o Redis应用场景
取最新N个数据的操作；
排行榜应用，取TOP N操作；
精准设定过期时间的应用；
计数器应用；
数据排重；
