
# Redis与 mysql事务的对比

| 操作   | Mysql	 | Redis |
| :-------- | --------:| :--: |
| 开启	 | start transaction | 	muitl |
| 语句	 | 普通sql	 | 普通命令 |
| 失败   | 	rollback 回滚	 | discard 取消 |
| 成功	 | commit | 	exec |

> 注: rollback与discard 的区别
如果已经成功执行了2条语句, 第3条语句出错.
Rollback后,前2条的语句影响消失.
Discard只是结束本次事务,前2条语句造成的影响仍然还在

> 注:
在mutil后面的语句中, 语句出错可能有2种情况
1: 语法就有问题, 
这种,exec时,报错, 所有语句得不到执行

2: 语法本身没错,但适用对象有问题. 比如 zadd 操作list对象
Exec之后,会执行正确的语句,并跳过有不适当的语句.

(如果zadd操作list这种事怎么避免? 这一点,由程序员负责)

# Redis中的乐观锁
思考: 
> 我正在买票Ticket -1 , money -100
而票只有1张, 如果在我multi之后,和exec之前, 票被别人买了---即ticket变成0了.
我该如何观察这种情景,并不再提交。

【A】一个进程A操作：
```bash
redis 127.0.0.1:6379> set ticket 1
OK
redis 127.0.0.1:6379> set money 200
OK
redis 127.0.0.1:6379> multi
OK
redis 127.0.0.1:6379> decr ticket
QUEUED
redis 127.0.0.1:6379> decrby money 100
QUEUED------------------------------------------->此时B进程抢先一步执行了下面的购票操作
redis 127.0.0.1:6379> exec
1)(integer)-1 ----------------------------------------->显然这种现象是不正常的，票不能为负数。
2)(integer)100
```

【B】此时另一个进程B操作了
```bash
redis 127.0.0.1:6379> decr ticket
OK
redis 127.0.0.1:6379> get ticket
"0"
```

如何观察这种情景,并不再提交呢？？？？？？


**悲观的想法:**
世界充满危险,肯定有人和我抢, 给 ticket上锁, 只有我能操作. [悲观锁]

**乐观的想法:**
没有那么人和我抢,因此,我只需要注意,
--有没有人更改ticket的值就可以了 [乐观锁]

Redis的事务中,启用的是乐观锁,只负责监测key没有被改动.

例:watch命令 
```bash
redis 127.0.0.1:6379> watch ticket
OK
redis 127.0.0.1:6379> multi
OK
redis 127.0.0.1:6379> decr ticket
QUEUED
redis 127.0.0.1:6379> decrby money 100
QUEUED
redis 127.0.0.1:6379> exec
(nil)   // 返回nil,说明监视的ticket已经改变了,事务就取消了.
redis 127.0.0.1:6379> get ticket
"0"
redis 127.0.0.1:6379> get money
"200"
```

语法：
```bash
watch key1 key2  ... keyN #作用:监听key1 key2..keyN有没有变化,如果有变, 则事务取消
unwatch  #作用: 取消所有watch监听
```


# Redis分布式锁实现秒杀（Java）
```java

package com.group2.dataStore.redis;


import java.util.List;
import java.util.Set;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

/*** redis乐观锁实例***/
public class RedisLockTest {
    public static void main(String[] args) throws InterruptedException {
        long starTime = System.currentTimeMillis();
        initPrduct();
        initClient();
        printResult();
        long endTime = System.currentTimeMillis();
        long Time = endTime - starTime;
        System.out.println("程序运行时间： " + Time + "ms");
    }

    /*** 输出结果*/
    public static void printResult() {
        Jedis jedis = JavaRedisClient.get().getResource();
        Set set = jedis.smembers("clientList");
        int i = 1;
        for (Object value : set) {
            System.out.println("第" + i++ + "个抢到商品，" + value.toString() + " ");
        }
        jedis.close();
    }

    /**
     * 初始化顾客开始抢商品
     */
    public static void initClient() {
        ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
        int clientNum = 10000;// 模拟客户数目
        for (int i = 0; i < clientNum; i++) {
            cachedThreadPool.execute(new ClientThread(i));
        }
        cachedThreadPool.shutdown();
        while (true) {
            if (cachedThreadPool.isTerminated()) {
                System.out.println("所有的线程都结束了！");
                break;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    /*** 初始化商品个数*/
    public static void initPrduct() {
        int prdNum = 100;// 商品个数
        String key = "prdNum";
        String clientList = "clientList";// 抢购到商品的顾客列表
        Jedis jedis = JavaRedisClient.get().getResource();
        if (jedis.exists(key)) {
            jedis.del(key);
        }
        if (jedis.exists(clientList)) {
            jedis.del(clientList);
        }
        jedis.set(key, String.valueOf(prdNum));// 初始化
        jedis.close();
    }
}

/*** 顾客线程****/
class ClientThread implements Runnable {
    Jedis jedis = null;
    String key = "prdNum";// 商品主键
    String clientList = "clientList";//// 抢购到商品的顾客列表主键
    String clientName;

    public ClientThread(int num) {
        clientName = "编号=" + num;
    }

    public void run() {
        try {
            Thread.sleep((int) (Math.random() * 5000));// 随机睡眠一下
        } catch (InterruptedException e1) {
        }

        while (true) {
            System.out.println("顾客:" + clientName + "开始抢商品");
             jedis = JavaRedisClient.get().getResource();
            try {
                jedis.watch(key);
                int prdNum = Integer.parseInt(jedis.get(key));// 当前商品个数
                if (prdNum > 0) {
                    Transaction transaction = jedis.multi();
                    transaction.set(key, String.valueOf(prdNum - 1));
                    List result = transaction.exec();
                    if (result == null || result.isEmpty()) {
                        System.out.println("悲剧了，顾客:" + clientName + "没有抢到商品");// 可能是watch-key被外部修改，或者是数据操作被驳回
                    } else {
                        jedis.sadd(clientList, clientName);// 抢到商品记录一下
                        System.out.println("好高兴，顾客:" + clientName + "抢到商品");
                        break;
                    }
                } else {
                    System.out.println("悲剧了，库存为0，顾客:" + clientName + "没有抢到商品");
                    break;
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                jedis.unwatch();
                jedis.close();
            }
        }
    }
}

```
