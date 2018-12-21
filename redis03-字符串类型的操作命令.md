# String类型的key高阶操作
1.设置value的同时，设置过期秒数和毫秒数。注意ex和px不要同时写，如果同时写以后面的有效期为准
```bash
set key value [ex 秒数]/[px 毫秒数]  [nx] /[xx]
```
说明：
