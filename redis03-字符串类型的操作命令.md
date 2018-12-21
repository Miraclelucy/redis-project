# String类型的key高阶操作
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
> redis 127.0.0.1:6379> set greet hello
> OK
> redis 127.0.0.1:6379> setrange greet 2 x
> (integer) 5
> redis 127.0.0.1:6379> get greet
> "hexlo"

5. 获取value值的某一部分 使用[start, stop]范围的值（对于字符串的下标,左数从0开始,右数从-1开始）
```bash
getrange key start stop
```
> exp:
> redis 127.0.0.1:6379> set title 'chinese'
> OK
> redis 127.0.0.1:6379> getrange title 0 3
> "chin"
> redis 127.0.0.1:6379> getrange title 1 -2
> "hines"

6. 获取并返回旧值,设置新值
```bash
getset key newvalue
```
> exp:
> 127.0.0.1:6379> set ztan ttt
> OK
> 127.0.0.1:6379> get ztan
> "ttt"
> 127.0.0.1:6379> getset ztan sss
> "ttt"
> 127.0.0.1:6379> get ztan
> "sss"
  
7. 
