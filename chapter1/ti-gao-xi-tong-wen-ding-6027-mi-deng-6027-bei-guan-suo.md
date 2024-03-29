## 前言

继上次乐观锁方案之后, 接下来讨论幂等性的第二个方案

## 悲观锁方案

鉴于前两个系列文章的问题和乐观锁的缺点, 我们又提出了悲观锁方案保证接口的幂等性.

回忆一下, 没有悲观锁的方式是这样的:

![](/assets/2021013100.png)

有了悲观锁的时候:

![](/assets/database.png)

### 有几点讨论说明一下:

1. 是不是可以先去查, 查完之后再等修改的时候再锁, 其实查询完加锁是可以防止并发修改的, 只能一个一个的修改, 但是不能保证查询到的是正确的信息, 如果注册记录没有乐观锁的情况下, 还是会插入多条.
2. 阻塞锁和非阻塞锁的区别: 非阻塞锁如果获取不到就返回失败, 阻塞锁获取不到会尝试一定的时间或者尝试一定懂得次数, 到了阈值才会获取失败.
3. 可重入锁和非可重入锁的区别: 当注册用户一再次申请同一个 email 的锁, 那么如果是不可重入锁, 就会造成死锁, 如果是重入锁, 可以继续获取这个锁做事情

### 悲观锁的优缺点

1. 对于用户和系统更友好, 如果冲突概率很大的时候, 不会造成数据库过大的压力和系统报警
2. 悲观锁让代码编写逻辑变得简单了一些, 可以在代码编写中假设自己在单线程内编程, 在一些不重要的业务场景下甚至可以减少思考乐观锁的设计
3. 缺点是如果冲突的可能性不高, 会造成性能的缺失
4. 如果纯依赖悲观锁, 如果集群网络不好等极端的情况下可能会导致两个客户端获取锁

## 分布式悲观锁实现常见方案

1. 数据库锁\(基于乐观锁实现悲观锁\)
2. Redis 等缓存锁\([https://github.com/redisson/redisson\](https://github.com/redisson/redisson\)\)
3. Zookeeper 锁\([https://curator.apache.org/\](https://curator.apache.org/\)\)
4. 如果有必要的话, 还可以构建多级锁来提高性能并兼顾稳定性



