
# 设计技巧
1: 把表名转换为key前缀 如, tag:
2: 第2段放置用于区分区key的字段--对应mysql中的主键的列名,如userid
3: 第3段放置主键值,如2,3,4...., a , b ,c
4: 第4段,写要存储的列名

用户表 user  , 转换为key-value存储
| userid	| username	| passworde	| email | 
| :-------- | --------:| :--: |
| 9 | 	Lisi	| 1111111	| lisi@163.com | 
```mysql
set  user:userid:9:username lisi
set  user:userid:9:password 111111
set  user:userid:9:email   lisi@163.com
```
keys user:userid:9*


# 注意
在关系型数据中,除主键外,还有可能其他列也步骤查询,如上表中, username 也是极频繁查询的,往往这种列也是加了索引的.转换到k-v数据中,则也要相应的生成一条按照该列为主的key-value
```mysql
Set  user:username:lisi:uid  9  
```
这样,我们可以根据username:lisi:uid ,查出userid=9, 再查user:9:password/email ...


# 好处：
这样的设计在分布式存储时，可以更好的设计Key到底分配到哪一台服务器。
在做hash分配服务器时，最对前面的进行hash，即[表名：索引健：索引值]

