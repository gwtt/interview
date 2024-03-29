### 讲一下SpringCloud的负载均衡机制

> 主要是通过Loadbalancer实现负载均衡器的实现（然后不要细讲，这边不太懂）

### 负载均衡的实现方式

> SpringCloud中是轮询和随机数（默认轮询）
>
> 一般来说方式有:
>
> **轮询（Round Robin）**：顺序循环将请求一次顺序循环地连接每个服务器。当其中某个服务器发生第二到第 7 层的故障，BIG-IP 就把其从顺序循环队列中拿出，不参加下一次的轮询，直到其恢复正常。
>
> 以轮询的方式依次请求调度不同的服务器；实现时，一般为服务器带上权重；这样有两个好处：针对服务器的性能差异可分配不同的负载；当需要将某个结点剔除时，只需要将其权重设置为0即可；
>
> - 优点：实现简单、高效；易水平扩展
> - 缺点：请求到目的结点的不确定，造成其无法适用于有写的场景（缓存，数据库写）
> - 应用场景：数据库或应用服务层中只有读的场景
>
> **随机方式**：请求随机分布到各个结点；在数据足够大的场景能达到一个均衡分布；
>
> - 优点：实现简单、易水平扩展
> - 缺点：同 Round Robin，无法用于有写的场景
> - 应用场景：数据库负载均衡，也是只有读的场景
>
> **哈希方式**：根据 key 来计算需要落在的结点上，可以保证一个同一个键一定落在相同的服务器上；
>
> - 优点：相同 key 一定落在同一个结点上，这样就可用于有写有读的缓存场景
> - 缺点：在某个结点故障后，会导致哈希键重新分布，造成命中率大幅度下降
> - 解决：一致性哈希 or 使用 keepalived 保证任何一个结点的高可用性，故障后会有其它结点顶上来
> - 应用场景：缓存，有读有写
>
> **一致性哈希**：在服务器一个结点出现故障时，受影响的只有这个结点上的 key，最大程度的保证命中率；如 twemproxy 中的 ketama方案；生产实现中还可以规划指定子 key 哈希，从而保证局部相似特征的键能分布在同一个服务器上；
>
> - 优点：结点故障后命中率下降有限
> - 应用场景：缓存
>
> **根据键的范围来负载**：根据键的范围来负载，前 1 亿个键都存放到第一个服务器，1~2 亿在第二个结点。
>
> - 优点：水平扩展容易，存储不够用时，加服务器存放后续新增数据
> - 缺点：负载不均；数据库的分布不均衡；
> - （数据有冷热区分，一般最近注册的用户更加活跃，这样造成后续的服务器非常繁忙，而前期的结点空闲很多）
> - 适用场景：数据库分片负载均衡
>
> **根据键对服务器结点数取模来负载**：根据键对服务器结点数取模来负载；比如有 4 台服务器，key 取模为 0 的落在第一个结点，1 落在第二个结点上
>
> - 优点：数据冷热分布均衡，数据库结点负载均衡分布；
> - 缺点：水平扩展较难；
> - 适用场景：数据库分片负载均衡

