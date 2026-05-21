# redis数据结构

## redis支持的数据类型

- String
- Hash
- List
- Set
- Zset



# redis常用命令



# redis事务



## 什么是事务？

> `redis` 中的事务是一组命令的集合，是redis最小的执行单元
>
> 特征：
>
> - 批量操作在发送 `EXEC` 命令前被放入缓存
> - 执行中任意命名执行失败，其余命名依然会执行
> - 在事务执行过程中，其他客户端的命令不会插入到事务的执行命令序列中
>
> `redis` 事务的原理是：将属于一个事务的命令发送给 `redis`，然后依次执行这些命令



## WATCH 命令的实现原理和使用场景？



> 乐观锁代码：

```python
import threading
import time
import redis

r = redis.Redis(host='localhost', port=6379, db=6, decode_responses=True)
r.flushdb()
r.set('stock', 10)  # 初始库存

def decr_stock(client, client_name):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            # WATCH 必须在 pipeline 创建之前调用
            client.watch('stock')
            current = int(client.get('stock'))
            if current <= 0:
                client.unwatch()
                print(f"{client_name}: 库存不足")
                return
            # 开启事务管道（注意：pipeline 会隐式调用 MULTI）
            pipe = client.pipeline()
            pipe.decrby('stock', 1)
            # EXEC 时会检查 WATCH 的键是否被修改
            pipe.execute()
            print(f"{client_name}: 扣减成功，剩余库存 {int(client.get('stock'))}")
            return
        except redis.WatchError:
            print(f"{client_name}: 第{attempt+1}次尝试冲突，重试...")
            time.sleep(0.05)  # 简单退避
    print(f"{client_name}: 重试耗尽，操作失败")

# 模拟 12 个并发请求抢购
threads = []
for i in range(12):
    t = threading.Thread(target=decr_stock, args=(redis.Redis(), f"用户-{i+1}"))
    threads.append(t)
    t.start()
for t in threads:
    t.join()

print("最终库存:", r.get('stock'))  # 0 （不会超卖）
```



## Redis 事务相关的命令有哪些？



## Redis 事务能保证原子性吗？为什么说没有回滚？



##  Redis 事务与 Pipeline 的区别？



## 在什么场景下会使用 Redis 事务？



## Redis 事务支持隔离性吗？



## **为什么redis事务不具备原子性**



##  为什么 Redis 事务不支持回滚？



- Redis 追求简单、快速，不需要回滚机制来拖累性能。
- 运行时错误往往是编程 bug，应该在测试阶段解决，不应该依赖回滚。
- 这种设计使 Redis 内部实现更简洁。