# 两种持久化方式

持久化的方式
1. 主从：通过从服务器保存和持久化，如mongDB的replication sets配置。
2. 日志：写数据同时也产生二进制日志，挂掉后可以通过日志来恢复数据，如传统关系型数据库mysql、oracle（本身是往磁盘上写数据的）。

Redis中的持久化有2种方式  
- 1. 快照rdb  
- 2. 日志aof
> 快照rdb：把内存里的数据（内存镜像，一个大的二进制块）整体直接搬到磁盘上每隔N分钟或者N次写操作后，从内存dump数据压缩形成rdb文件，存入磁盘目录。
> AOF(append only file)持久化：以独立日志的方式记录每次写命令，重启时再重新执行AOF文件中命令达到恢复数据的目的。AOF的主要作用是解决了数据持久化的实时性。

# rdb配置方式
### 配置rdb
Rdb快照的配置选项redis-3.2.8/redis.conf
```bash
save 900 1      # 900秒内,有1条写入,则产生快照 
save 300 1000   # 如果300秒内有1000次写入,则产生快照
save 60 10000  # 如果60秒内有10000次写入,则产生快照(这3个选项都屏蔽,则rdb禁用)
stop-writes-on-bgsave-error  yes  # 后台备份进程出错时,主进程停不停止写入
rdbcompression  yes    # 导出的rdb文件是否压缩
rdbchecksum   yes  # 导入rbd恢复时数据时,要不要检验rdb的完整性(有没有损坏过，版本是否兼容等)
dbfilename dump.rdb  # 导出来的rdb文件名
dir ./  # rdb的放置路径
```
### rdb的缺陷
在2个保存点之间断电，将会丢失1-N分钟的数据，没办法做到实时持久化/秒级持久化


# aof配置方式
开启AOF功能需要设置配置：appendonly yes,默认不开启
```bash
aof 的配置 redis-3.2.8 /redis.conf
appendonly  yes # 是否打开 aof日志功能
appendfilename  /appendonly.aof  存放aof日志的文件名
appendfsync always   # 每1个命令,都立即同步到aof. 安全,速度慢
appendfsync everysec # 折衷方案,每秒写1次
appendfsync no      # 写入工作交给操作系统,由操作系统判断缓冲区大小,统一写入到aof. 同步频率低,速度快,
no-appendfsync-on-rewrite  yes: # 正在导出rdb快照的过程中,要不要停止同步aof
auto-aof-rewrite-percentage 100 #aof文件大小比起上次重写时的大小,增长率100%时,重写
auto-aof-rewrite-min-size 64mb #aof文件,至少超过64M时,重写
```

#  重启加载
AOF和RDB文件都可以用于服务器重启时的数据恢复：
- AOF持久化开启且存在AOF文件时，优先加载AOF文件
- AOF关闭或者AOF文件不存在时，加载RDB文件
- 开启AOF功能需要设置配置：appendonly yes,默认不开启


# 注意事项
> 问: 在dump rdb过程中,aof如果停止同步,会不会丢失?
答: 不会,所有的操作缓存在内存的队列里, dump完成后,统一操作.

> 问: aof重写是指什么?
答: aof重写是指把内存中的数据,逆化成命令,写入到.aof日志里.以解决 aof日志过大的问题.

> 问: 如果rdb文件,和aof文件都存在,优先用谁来恢复数据?
答: aof

> 问: 2种是否可以同时用?
答: 可以,而且推荐这么做

> 问: 恢复时rdb和aof哪个恢复的快
答: rdb快,因为其是数据的内存映射,直接载入到内存,而aof是命令,需要逐条执行


