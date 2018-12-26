
# 服务端常见命令：
```bash
time # 显示服务器时间 , 时间戳(秒), 微秒数
dbsize # 当前数据库的key的数量
select 2 # 选择数据库
bgrewriteaof  # 后台进程重写AOF
bgsave       # 后台保存rdb快照
save         # 保存rdb快照
lastsave     # 上次保存时间
slaveof master-Host port # 把当前实例设为master的slave
flushall  # 清空所有库所有键 
flushdb  # 清空当前库所有键
showdown [save/nosave] # 关闭服务器
slowlog  # 显示慢查询
info [Replication/CPU/Memory..] # 查看redis服务器的信息
config get # 获取redis.conf中的配置项  
config set # 设置redis.conf中的配置项 值 (特殊的选项,不允许用此命令设置,如slave-of, 需要用单独的slaveof命令来设置)
config get/set slowlog-log-slower-than # 指定多慢才叫慢查询
config get/set slowlog-max-len  # 服务器储存多少条慢查询的记录
slowlog get N  # 获取慢日志
```

# 常见命令说明
#### time和dbsize

```bash
redis 127.0.0.1:6380> time  # 显示服务器时间 , 时间戳(秒), 微秒数
1) "1375270361"
2) "504511"

redis 127.0.0.1:6380> select 2
OK
redis 127.0.0.1:6380> dbsize  # 当前数据库的key的数量
(integer) 2

```
#### 不小心运行了flushall，如何恢复

立即 shutdown nosave，关闭服务器。然后手工编辑aof文件，去掉文件中的 “flushall ”相关行，然后开启服务器,就可以导入回原来数据。 如果flushall之后，系统其他人恰好执行了bgrewriteaof的命令，那么aof就清空了，数据丢失，所以不小心flushall后必须立即马上shutdown nosave

#### slowlog 显示慢查询
> 问:多慢才叫慢? 

答: 由slowlog-log-slower-than 10000 ,来指定,(单位是微秒)

> 问:服务器储存多少条慢查询的记录?

答: 由 slowlog-max-len 128 ,来做限制

#### info [Replication/CPU/Memory..] 查看redis服务器的信息

info查看到的内容：
```bash
1: 内存
# Memory
used_memory:859192 数据结构的空间
used_memory_rss:7634944 实占空间
mem_fragmentation_ratio:8.89 前2者的比例,1.N为佳,如果此值过大,说明redis的内存的碎片化严重,可以导出再导入一次.
2: 主从复制
# Replication
role:slave
master_host:192.168.1.128
master_port:6379
master_link_status:up
3:持久化
# Persistence
rdb_changes_since_last_save:0
rdb_last_save_time:1375224063
4: fork耗时
#Status
latest_fork_usec:936  上次导出rdb快照,持久化花费微秒
注意: 如果某实例有10G内容,导出需要2分钟,
每分钟写入10000次,导致不断的rdb导出,磁盘始处于高IO状态.
```


