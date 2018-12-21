# 基本操作
1. 查询所有的keys

```bash
keys *
```
返回的结果如下
>1) "website"
>2) "app::users::click"
>3) "province"
>4) "testkey"
>5) "ztan"
>6) "ztan_key"
>7) "redis-test"
>8) "ztan tan"
2. 精确查询key
```bash
keys website
```
3. 模糊查询key，使用正则表达式(有3个通配符 * ? [])
```bash
keys ztan*
keys ztan[1|2]
```
4. 随机返回一个key
```bash
randomkey
```
5. 查询key的类型
```bash
type key
```
6. 判断key是否存在(判断key是否存在,返回1/0)
```bash
exists ztan
```
7. 删除key
```bash
del ztan
```
8. 修改key的名字,如果新的key名存在则覆盖
```bash
rename ztan lusq
```
修改key的名字,仅仅当新的key名不存在时，
```bash
renamex ztan lusq
```
9. 获取value值
```bash
get ztan
```
10. 设置value值
```bash
set ztan sss
```
11. redis默认处于0号database，默认共16个database,编号为【0-15】
```bash
select 1
```
12. 将某个key移动到1号database
```bash
move ztan 1
```

# 有效期设置
1. 查询key的生命周期
返回: 秒数（对于不存在的key,返回-2；永久有效的key,都返回-1；）
```bash
ttl ztan
```
2. 设置key的生命周期,如下，将生命周期设为10秒
```bash
expire ztan 10
```
3. 同理: 
```bash
pexpire key 毫秒数, 设置生命周期
pttl  key, 以毫秒返回生命周期
```
4. 将key设置为永久有效
```bash
persist ztan
```
