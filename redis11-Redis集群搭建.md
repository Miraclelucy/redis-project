
# 集群的作用
1. 主从备份，防止主机宕机
2. 读写分离，分担master任务
3. 任务分离，有一些求交集求并集很耗内存，碰到大量的运算任务交给更合适的计算机来进行

常见的集群拓扑图：
1. 星型
2. 直线型

# master和slave如何达同步的
1. slave启动后会自动连接到主服务器；
2. 首先主服务器dump一次性将内存中的镜像同步到从服务器
3. 然后主服务器将aof缓冲在队列中的命令同步到从服务器
4. 最后用一个单独的进程replicationFeedSlaves保持主从服务器之间的同步

# master和slave配置：redis.conf
### master配置：
1. 关闭rdb快照（备份交给slave）
2. 可以开启aof

### slave配置：
 1. 声明slaveof 。只需要打开slaveof ip:port 
 2. 配置密码（如果master有密码）--内网搭建集群一般不用密码，作为一个整体
 3. [某1个]slave打开rdb快照功能
 4. 配置是否只读[slave-read-only]

### 主服务器设置密码
访问主服务器执行操作命令之前，需要先授权auth
redis 127.0.0.1:6379> get title
(error)ERR operation not permitted 
redis 127.0.0.1:6379> auth passwd
OK
redis 127.0.0.1:6379> get title
“sunshine”
同时从服务器的配置文件里面也需要加上密码

# 主从复制的缺陷
如果slave断开了和master的连接，重启slave同步的过程都要重新执行一遍，所以记住多台slave不要同时全部启动，否则主服务器的IO剧增。
总之，从服务器要一台一台的启动。
