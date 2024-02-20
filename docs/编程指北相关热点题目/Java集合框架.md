### 1.说说 List,Set,Map 三者的区别？三者底层的数据结构？

> List存储元素是有序，可重复的
>
> Set存储元素不可重复
>
> Map存储kv键值对，key是无序，不可重复的，value是无序，可重复的
>
> 如果非要说list，set，map的数据结构，要看具体的实现类
>
> 比如List:
>
> ArrayList:数组
>
> LinkedList:链表（1.7之后不是循环链表0
>
> set里面就是Hashset（底层：HashMap），TreeSet（红黑树），LinkedHashSet（LinkedHashMap）
>
> Map:HashMap(数组+链表/红黑树),LinkedHashMap(数组+链表/红黑树，区别就是在每个存放结点上用双向链表连起来),TreeMap(红黑树)

### 2.有哪些集合是线程不安全的？怎么解决呢？

> 比如ArrayList,HashMap等等都是线程不安全的
>
> 解决方法就是两种
>
> 用Collections.synchronizedList去包装这些集合
>
> 或者用并发集合类，比如ConcurrentHashMap、CopyOnWriteArrayList
>
> 最后或者自己手动上锁

### 3.⽐较 HashSet、LinkedHashSet 和 TreeSet 三者的异同

>  HashSet、LinkedHashSet 和 TreeSet都实现了Set接口
>
> 主要区别在于底层数据结构不同
>
> Hashset是哈希表吗，LinkedHashSet 的底层数据结构是链表和哈希表，TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
>
> 底层数据结构不同又导致这三者的应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。

### 4.HashMap 和 Hashtable 的区别？HashMap 和 HashSet 区别？HashMap 和 TreeMap 区别？

> HashMap和Hashtable主要就是线程安全问题，Hashtable加上了同步锁，因此线程安全，并且不允许kv为null。因为锁的缘故，效率是HashMap更高一些。
>
> HashMap 和 HashSet的区别就是，HashSet底层是HashMap，只用上了key
>
> HashMap 和 TreeMap 的区别： HashMap 是无序的，根据键的 hashCode() 来存储数据，性能较好；TreeMap 是有序的，根据键的自然顺序或者 Comparator 接口来存储数据。 2.迭代顺序：HashMap 的迭代顺序是不确定的；而 TreeMap 的迭代顺序是按照键的大小顺序进行遍历。 3. 性能表现：HashMap 的性能在大多数情况下优于 TreeMap，因为 HashMap 使用哈希表来存储数据，查找操作的时间复杂度为 O(1)；而 TreeMap 使用红黑树来存储数据，查找操作的时间复杂度为 O(logN)。

### 5.HashMap 的底层实现

> JDK1.8 之前 `HashMap` 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。HashMap 通过 key 的 `hashcode` 经过hash函数处理过后得到 hash 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

### 6.HashMap 的⻓度为什么是 2 的幂次⽅

> - 使用 2 的幂次方作为容量可以保证生成的哈希值更加分散，降低碰撞的概率，提高 HashMap 的性能。 
> -  2 的幂次方减 1，二进制表示为全是 1，这样可以通过 (n - 1) & hash 来替代取模运算，这样可以提高计算速度。 
> - 在扩容时，HashMap 的新容量是原容量的两倍，这样可以保证新容量是 2 的幂次方，符合前面提到的优化。总的来说，HashMap 使用 2 的幂次方作为容量，既能提高性能，又能简化运算。

### 7.ConcurrentHashMap 和 Hashtable 的区别？

> 我这边主要研究过1.8以后版本的，所以1.7的分段锁我就不讨论了。
>
> ConcurrentHashMap 底层是Node数组+链表/红黑树，跟Hashtable差不多。但是，HashTable为了实现线程安全，对整个集合上锁，比较冗余，效率底下。ConcurrentHashMap实现的是对某个Node结点上锁，使用Synchronized和CAS操作，并不会对所有结点上锁，因此效率较高。

### 8.ConcurrentHashMap 线程安全的具体实现⽅式/底层具体实现

> 跟7差不多