# 基础操作

1. 增加元素
```bash
sadd key  value1 value2
```

2. 删除元素
```bash
srem value1 value2
```

3. 删除集合中key中1个随机元素并返回
```bash
spop key
```

4. 随机返回一个元素
```bash
srandmember key
```

5. 判断value是否在key集合中
```bash
sismember key  value
```

6. 返回集合中所有的元素
```bash
smembers key
```

7. 返回集合个数
```bash
scard key
```

8. 把source中的value删除,并添加到dest集合中
```bash
smove source dest value
```

9. 求交集
```bash
sinter  key1 key2 key3
```

10. 求交集并存储
```bash
sinterstore dest key1 key2 key3
```
求出key1 key2 key3 三个集合中的交集,并赋给dest

11. 求并集
```bash
suion key1 key2.. Keyn
```

12. 求差集
```bash
sdiff key1 key2 key3 
```
即key1-key2-key3 
