### 1.什么是 Sentinel？ 有什么⽤？
> Sentinel是一种Redis运行模式，不提供读写服务，帮助在主从复制集群下，监控Redis的运行状态并自动实现故障转移
### 2.Sentinel 如何检测节点是否下线？主观下线与客观下线的区别?

### 3.Sentinel 是如何实现故障转移的？

### 4.Sentinel 如何选择出新的 master（选举机制）?

### 5.如何从 Sentinel 集群中选择出 Leader ？

### 6.Sentinel 可以防⽌脑裂吗？

### 7.为什么需要 Redis Cluster？解决了什么问题？有什么优势？

### 8.Redis Cluster 是如何分⽚的？

### 9.为什么 Redis Cluster 的哈希槽是 16384 个?

### 10.如何确定给定 key 的应该分布到哪个哈希槽中？

### 11.Redis Cluster ⽀持重新分配哈希槽吗？

### 12.Redis Cluster 扩容缩容期间可以提供服务吗？

### 13.Redis Cluster 中的节点是怎么进⾏通信的？