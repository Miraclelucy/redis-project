# Redis是什么
Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets.
redis是开源,BSD许可,高级的key-value存储系统. 
可以用来存储字符串,哈希结构,链表,集合,因此,常用来提供数据结构服务.

redis和memcached相比,的独特之处:
1. redis可以用来做存储(storge), 而memccached是用来做缓存(cache)
  这个特点主要因为其有”持久化”的功能.
2. 存储的数据有”结构”,对于memcached来说,存储的数据,只有1种类型--”字符串”,
  而redis则可以存储字符串,链表,哈希结构,集合,有序集合.

# Redis基本特点
1. 速度快:Redis使用C编写，能支持10万每秒的读写频率。
2. 数据结构丰富:可以将Redis看做“数据结构服务器”。目前Redis支持5种数据结构string,list，set，sorted set，hash table
3. 持久化:支持数据持久化存储。
4. 主-从复制:Redis支持简单而快速的主-从复制。
5. 丰富的特性:支持publish/subscribe, key过期等特性

# Redis使用方式与应用场景
1. Redis两种使用方式:
- 缓存系统
- 存储系统
2. Redis应用场景
- 取最新N个数据的操作；
- 排行榜应用，取TOP N操作；
- 精准设定过期时间的应用；
- 计数器应用；
- 数据排重；
- 实时系统，反垃圾
系统；
- 发布订阅系统；
- 分布式队列系统；
- 缓存系统

# 安装 Redis
1. 官方站点: redis.io 下载最新版或者最新stable版
2. 解压源码并进入目录
3. 不用configure
4. 执行make(说明： “-j 4”表示同时启动四个并发线程编译源代码，你可以改为其他线程数，默认为1)
```bash 
make -j 4  
```
5. 执行make install(说明：也可以通过指定添加参数指定安装目录 make PREFIX=/usr/local/redis  install)
```bash  
sudo make install 
```
6. 安装完后，在安装目录的src目录下可以看到以下几个工具
- redis-benchmark  性能测试工具
- redis-check-aof  日志文件检测工(比如断电造成日志损坏,可以检测并修复)
- redis-check-dump  快照文件检测工具,效果类上
- redis-cli  客户端
- redis-server 服务端
7. 启动redis
在redis安装目录下，使用以下命令启动redis (注意最后有一个“&”符号，表示后台执行)
 ```bash 
 src/redis-server ./redis.conf & 
 ```
8. 验证 redis 是否启动成功
 ```bash  
 ps -ef | grep redis 
 ```
9. 使用src/redis-cli 访问 redis



