# 基础操作

1. 赋值
```bash
hset key field value
hmset key field1 value1 [field2 value2 field3 value3 ......fieldn valuen]
```

2. 查询

```bash
hget key field #返回key中field域的值
hmget key field1 field2 fieldN #返回key中field1 field2 fieldN域的值
```

3. 查询所有
```bash
hgetall key #返回key中,所有field与其value
hkeys key #返回key中所有的field
kvals key #返回key中所有的value
```

4. 删除

```bash
hdel key field
```

5. 求长度
```bash
hlen key #返回key中元素的数量
```

6. 判断key中有没有field域
```bash
hexists key field
```

7. 加法
```bash
hinrby key field value # key中的field域的值+value（整型）
hinrby float  key field value #  key中的field域的值+value（浮点型）
```



