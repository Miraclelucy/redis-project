# 基础操作
1. 插入
```bash
lpush key value  #把值插入到链接头部
rpush key value  #把值插入到链接尾部
```

2. 删除
```bash
lpop key value  #把链接头部元素删除
rpop key value  #把链接尾部元素删除
```

3. 查询
```bash
lrange key start  stop #左数从0开始,右数从-1开始
```
返回链表中[start ,stop]中的元素

4. 删除部分
```bash
lrem key count value #删除count的绝对值个value后结束
```
- Count>0 从表头删除
- Count<0 从表尾删除


5. 剪切
```bash
ltrim key start stop
```
剪切key对应的链接,切[start,stop]一段,并把该段重新赋给key

6. 根据索引查询
```bash
lindex key index #返回index索引上的值
```


7.返回长度
```bash
llen key
```

8.在指定元素前插入
```bash
linsert  key after|before search value
```
在key链表中寻找’search’,并在search值之前|之后,.插入value。
注: 一旦找到一个search后,命令就结束了,因此不会插入多个value


9.source的尾部拿出,放在dest的头部
```bash
rpoplpush source dest #并返回 该单元值
```
>场景: task + bak 双链表完成安全队列
>Task列表                             bak列表

>业务逻辑:
- i.Rpoplpush task bak
- ii.接收返回值,并做业务处理
- iii.如果成功,rpop bak 清除任务. 如不成功,下次从bak表里取任务


10.等待弹出key的尾/头元素
```bash
brpop ,blpop  key timeout
```
Timeout为等待超时时间。如果timeout为0,则一直等待。
>场景: 长轮询Ajax,在线聊天时,能够用到


