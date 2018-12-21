# String类型的key高阶操作
1. 设置单个key的同时，设置过期秒数和毫秒数。注意ex和px不要同时写，如果同时写以后面的有效期为准

**set key value [ex 秒数]/[px 毫秒数]  [nx] /[xx]**

说明：nx表示不存在时,执行操作；xx表示key存在时,执行操作

2. 设置多个key

**mset key1 v1 key2 v2**

3. 获取多个key

**mget key1 key2 ..keyn**

4. 设置value值的某一部分

**setrange key offset value**
```bash
  redis 127.0.0.1:6379> set greet hello
  OK
  redis 127.0.0.1:6379> setrange greet 2 x
  (integer) 5
  redis 127.0.0.1:6379> get greet
  "hexlo"
```
5. 获取value值的某一部分 使用[start, stop]范围的值（对于字符串的下标,左数从0开始,右数从-1开始）

**getrange key start stop**
```bash
  redis 127.0.0.1:6379> set title 'chinese'
  OK
  redis 127.0.0.1:6379> getrange title 0 3
  "chin"
  redis 127.0.0.1:6379> getrange title 1 -2
  "hines"
```

6. 获取并返回旧值,设置新值

**getset key newvalue**
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

**incr key**
作用: 指定的key的值加1,并返回加1后的值.
注意:
- 不存在的key当成0,再incr操作
- 范围为64有符号 

**incrby key number**
```bash
redis 127.0.0.1:6379> incrby age  90
(integer) 92

incrbyfloat key floatnumber
redis 127.0.0.1:6379> incrbyfloat age 3.5
"95.5"
```
8. 减法
decr key
decrby key number
```bash
redis 127.0.0.1:6379> set age 20
OK
redis 127.0.0.1:6379> decr age
(integer) 19


redis 127.0.0.1:6379> decrby age 3
(integer) 16
```

9. 
