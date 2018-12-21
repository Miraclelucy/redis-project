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
del ztan

8. 修改key的名字
rename ztan lusq
9. 
