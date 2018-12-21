# 基础操作

1. 增加元素
```bash
zadd key score1 value1 score2 value2 
```

2. 返回名次在[start,stop]之间的元素,默认是升续排列

```bash
zrange key start stop [WITHSCORES]# Withscores 是把score也打印出来
```

3. 返回名次在[start,stop]之间的元素,采用降续排列
```bash
zrevrange key start stop
```

4. 集合(升续)排序后,取score在[min,max]内的元素,并跳过 offset个, 取出N个

```bash
zrangebyscore  key min max [withscores] limit offset N
```

5. 查询member的排名(升续 0名开始)
```bash
zrank key member
```

6. 查询member的排名(降续 0名开始)
```bash
zrevrank key memeber
```

7. 删除集合中的元素
```bash
zrem key value1 value2 .
```

8. 按照socre来删除元素,删除score在[min,max]之间的
```bash
zremrangebyscore key min max
```

9. 按排名删除元素,删除名次在[start,end]之间的
```bash
zremrangebyrank key start end
```

10. 返回[min,max] 区间内元素的数量
```bash
zcount key min max
```

11. 求交集
```bash
zinterstore destination numkeys key1 [key2 ...] 
[weights weight1 [weight2 ...]] 
[aggregate sum|min|max]
```
- 求key1,key2的交集,key1,key2的权重分别是 weight1,weight2
- 聚合方法用: sum |min|max
- 聚合的结果,保存在destination集合内


