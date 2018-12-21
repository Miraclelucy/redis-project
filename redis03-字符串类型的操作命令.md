# 基础操作
1. 设置单个key的同时，设置过期秒数和毫秒数。注意ex和px不要同时写，如果同时写以后面的有效期为准
```bash
 set key value [ex 秒数]/[px 毫秒数]  [nx] /[xx]
```
  说明：nx表示不存在时,执行操作；xx表示key存在时,执行操作

2. 设置多个key
```bash
mset key1 v1 key2 v2
```
3. 获取多个key
```bash
mget key1 key2 ..keyn
```
4. 设置value值的某一部分
```bash
setrange key offset value
```
exp:
```bash
  redis 127.0.0.1:6379> set greet hello
  OK
  redis 127.0.0.1:6379> setrange greet 2 x
  (integer) 5
  redis 127.0.0.1:6379> get greet
  "hexlo"
```
5. 获取value值的某一部分 使用[start, stop]范围的值（对于字符串的下标,左数从0开始,右数从-1开始）
```bash
getrange key start stop
```
exp:
```bash
  redis 127.0.0.1:6379> set title 'chinese'
  OK
  redis 127.0.0.1:6379> getrange title 0 3
  "chin"
  redis 127.0.0.1:6379> getrange title 1 -2
  "hines"
```

6. 获取并返回旧值,设置新值
```bash
getset key newvalue
```
exp:
```bash
  127.0.0.1:6379> set ztan ttt
  OK
  127.0.0.1:6379> get ztan
  "ttt"
  127.0.0.1:6379> getset ztan sss
  "ttt"
  127.0.0.1:6379> get ztan
  "sss"
``` 

7. 加法
```bash
incr key
```
作用: 指定的key的值加1,并返回加1后的值.
注意:
- 不存在的key当成0,再incr操作
- 范围为64有符号 
```bash
incrby key number
incrbyfloat key floatnumber
```
exp:
```bash
redis 127.0.0.1:6379> incrby age  90
(integer) 92
redis 127.0.0.1:6379> incrbyfloat age 3.5
"95.5"
```
8. 减法
```bash
decr key
decrby key number
```
exp:
```bash
redis 127.0.0.1:6379> set age 20
OK
redis 127.0.0.1:6379> decr age
(integer) 19
redis 127.0.0.1:6379> decrby age 3
(integer) 16
```

# 位操作
1. 获取二进制位上的值,对应位上的值(从左,从0编号，A对应的ascii码是65，表示为二进制即是0100 0001)
```bash
getbit key offset
```
exp:
```bash
redis 127.0.0.1:6379> set char A
OK
redis 127.0.0.1:6379> getbit char 1
(integer) 1
redis 127.0.0.1:6379> getbit char 2
(integer) 0
redis 127.0.0.1:6379> getbit char 7
(integer) 1
```

2. 设置二进制位上的值,返回: 该位上的旧值
```bash
setbit  key offset value
```
注意: <a href="https://redis.io/commands/setbit">https://redis.io/commands/setbit</a>
- 如果offset过大,则会在中间填充0,
- offset最大大到多少
- offset最大2^32-1,可推出最大的的字符串为512M

3. 多个key按二进制进行运算（比如，用于大小写的转换）
```bash
bitop operation destkey key1 [key2 ...]
```
对key1,key2..keyN作operation,并将结果保存到 destkey 上。
operation 可以是 AND 、 OR 、 NOT 、 XOR
```bash
redis 127.0.0.1:6379> setbit lower 2 1
(integer) 0
redis 127.0.0.1:6379> get lower
" "
redis 127.0.0.1:6379> set char Q
OK
redis 127.0.0.1:6379> get char
"Q"
redis 127.0.0.1:6379> bitop or char char lower
(integer) 1
redis 127.0.0.1:6379> get char
"q"
```
