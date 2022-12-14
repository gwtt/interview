### Redis有哪些持久化机制

> RDB,AOF
>
> **RDB：是Redis DataBase缩写快照**
>
> - RDB是Redis**默认的持久化方式**。按照一定的时间将内存的数据以快照的形式保存到硬盘中，对应产生的数据文件为dump.rdb。通过配置文件中的save参数来定义快照的周期。
>
> **优点：**
>
> 1. 只有一个文件 dump.rdb，方便持久化。
> 2. 容灾性好，一个文件可以保存到安全的磁盘。
> 3. 性能最大化，fork 子进程来完成写操作，让主进程继续处理命令，所以是 IO 最大化。使用单 独子进程来进行持久化，主进程不会进行任何 IO 操作，保证了 redis 的高性能
> 4. 相对于数据集大时，比 AOF 的启动效率更高。
>
> **缺点：**
>
> 1. RDB 是间隔一段时间进行持久化，如果持久化之间 redis 发生故障，会发生数据丢失。
> 2. 由于RDB是通过fork子进程来协助完成数据持久化，如果当数据集较大时，可能会导致整个服务器间歇性暂停服务
>
> **AOF：Append Only File持久化：**
>
> - AOF持久化(即Append Only File持久化)，则是将Redis执行的每次**写命令记录**到单独的日志文件中，当重启Redis会重新将持久化的日志中文件恢复数据。当两种方式同时开启时，数据恢复Redis会**优先选择AOF恢复**
>
> **优点：**
>
> 1. 数据安全，aof 持久化可以配置 appendfsync 属性，有 always，**每进行一次 命令操作就记录 到 aof 文件中一次**。
> 2. 通过 append 模式写文件，即使中途服务器宕机，可以通过 **redis-check-aof 工具**解决数据一致性问题。
> 3. AOF 机制的 rewrite 模式。AOF 文件没被 rewrite 之前（文件过大时会对命令 进行合并重写），可以删除其中的某些命令（比如误操作的 flushall）然后重新恢复。
>
> **缺点：**
>
> 1. AOF 文件比 RDB 文件大，且恢复速度慢。
> 2. 数据集大的时候，比 rdb 启动效率低。
>
> ```
> always：同步刷盘，可靠性高，但性能影响较大
> everysec：每秒刷盘，性能适中，最多丢失 1 秒的数据
> no：操作系统控制，性能最好，可靠性最差
> ```
>
> **俩种持久化的优缺点是什么？**
>
> - AOF文件比RDB更新频率高，优先使用AOF还原数据。
> - AOF比RDB更安全也更大
> - RDB性能比AOF好
> - 如果两个都配了优先加载AOF

