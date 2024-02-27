### 1.Redis 给缓存数据设置过期时间有啥⽤？ Redis 是如何判断数据是否过期的呢？过期数据是如何被删除的？
> 因为不设置过期时间，缓存数据一直保存，会产生Out of Memory问题,除此之外，也可以帮助业务代码，比如限制token一天过期，不需要我们自己判断过期。
> 
> Redis 通过一个叫做过期字典（可以看作是 hash 表）来保存数据过期的时间。过期字典的键指向 Redis 数据库中的某个 key(键)，过期字典的值是一个 long long 类型的整数，这个整数保存了 key 所指向的数据库键的过期时间（毫秒精度的 UNIX 时间戳）。
>
> 惰性删除：只会在取出 key 的时候才对数据进行过期检查。这样对 CPU 最友好，但是可能会造成太多过期 key 没有被删除。
> 
>定期删除：每隔一段时间抽取一批 key 执行删除过期 key 操作。并且，Redis 底层会通过限制删除操作执行的时长和频率来减少删除操作对 CPU 时间的影响。

### 2.Redis 内存满了怎么办？
> 那么就会使用内存淘汰机制
> 
> Redis 提供 6 种数据淘汰策略：
> 1. volatile-lru（least recently used）：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰。
> 2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰。
> 3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰。
> 4. allkeys-lru（least recently used）：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）。
> 5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰。
> 6. no-eviction：禁止驱逐数据，也就是说当内存不足以容纳新写入数据时，新写入操作会报错。这个应该没人使用吧！
> 
> 4.0 版本后增加以下两种：
> 7. volatile-lfu（least frequently used）：从已设置过期时间的数据集（server.db[i].expires）中挑选最不经常使用的数据淘汰。
> 8. allkeys-lfu（least frequently used）：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的 key。

### 3.Redis 内存淘汰算法除了 LRU 还有哪些？
> 参考上面