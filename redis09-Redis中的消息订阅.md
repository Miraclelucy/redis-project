# 基本操作
> 订阅端: subscribe 频道名称
订阅端: psubscribe  [pattern] 模糊匹配订阅频道
取消订阅: unsubscribe 频道名称
取消订阅: punsubscribe [pattern] 模糊匹配取消订阅频道
发布端: publish 频道名称 发布内容

# 例子
服务端:
```bash
redis 127.0.0.1:6379> publish news  'good good study'
(integer) 1
redis 127.0.0.1:6379> publish newstop  'day day up'
(integer) 1
```
客户端A:
```bash
redis 127.0.0.1:6379> psubscribe news*
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "news"
3) (integer) 1
1) "message"
2) "news"
3) "good good study"
1) "message"
2) "news"
3) "day day up"
```
客户端B:
```bash
redis 127.0.0.1:6379> subscribe news
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "news"
3) (integer) 1
1) "message"
2) "news"
3) "good good study"
```
