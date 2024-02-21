### 1.什么是线程和进程?线程与进程的关系,区别及优缺点？

> 进程就是一个程序执行的一个过程。而线程是一个比进程更小的执行单位，是能够进行运算调度的最小单位。
>
> 一个进程在执行过程中会产生多个线程。线程会共享同一进程下的堆和方法区，但每个线程都会有自己的程序计数器、虚拟机栈和本地方法栈。
>
> 区别上来讲的话
>
> 进程拥有系统资源，线程基本不拥有
>
> 进程切换开销大，线程切换开销小
>
> 线程可以通过共享内存来通信，进程需要额外的通信机制
>
> 进程的优点是相互独立，安全性高，缺点是创建开销大，资源消耗多
>
> 线程的优点是创建简单，开销小，相互通信比较容易，缺点会造成死锁问题

### 2.为什么要使⽤多线程呢?

> 从单核上讲来说，多线程提高了进程利用IO和CPU的效率。比如现在一个Java进程运行，当我们请求IO，如果只有一个线程，然后被IO阻塞，CPU和IO只有一个设备在运行，所以效率只有50%，因此开启多线程可以乘IO阻塞的时候可以继续使用CPU。
>
> 现在普遍是多核，与单核的利用率不同的是，现在多线程主要为了提高作业运行效率，将一个复杂的任务拆分，结果合并，可以大大提高任务执行的效率。

### 3.什么是线程上下⽂切换?

> 线程上下文切换指的就是保存当前线程的运行条件和状态(上下文)，留待线程下次占用CPU时可以恢复现场，并且加载下一个占用CPU的线程上下文。

### 4.什么是线程死锁?如何避免死锁?

> 线程死锁是指两个或多个线程互相等待对方释放资源，导致它们都无法继续执行的情况.
>
> - 避免循环等待：当线程需要获取多个资源时，应该按照相同的顺序获取资源，避免循环依赖。 
> - 使用资源分配顺序：对共享资源进行编号或者采用某种规则，使得所有线程都按照相同的顺序获取资源，从而避免死锁。
> -  使用超时机制：在获取资源时设定一个超时时间，在超时之后尝试释放已获取的资源，避免长时间等待。

### 5.乐观锁和悲观锁了解么？如何实现乐观锁？

> 悲观锁让某一资源同一时间只被一个线程所拥有
>
> 乐观锁则相反，允许共享资源在同一时间被多个线程所拥有，只需要在提交时去验证对应的资源。
>
> 实现的话一般通过CAS和版本号机制来实现。总的来说，就是要提交的时候比对一些版本，如果版本不匹配，则驳回。	

### 6.说说 sleep() ⽅法和 wait() ⽅法区别和共同点?

> **共同点**：两者都可以暂停线程的执行。
>
> **区别**：
>
> - **`sleep()` 方法没有释放锁，而 `wait()` 方法释放了锁** 。
> - `wait()` 通常被用于线程间交互/通信，`sleep()`通常被用于暂停执行。
> - `wait()` 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify()`或者 `notifyAll()` 方法。`sleep()`方法执行完成后，线程会自动苏醒，或者也可以使用 `wait(long timeout)` 超时后线程会自动苏醒。
> - `sleep()` 是 `Thread` 类的静态本地方法，`wait()` 则是 `Object` 类的本地方法。

### 7.讲⼀下 JMM(Java 内存模型)。volatile 关键字解决了什么问题？说说 synchronized 关键字和 volatile 关键字的区别。

> JMM就是Java内存模型，本身是不存在的描述的是一组规则或者规范。JMM内存模型规范中规定所有的变量都存储在主内存中，而主内存中的变量是所有的线程都可以共享的，而对主内存中的变量进行操作时，必须在线程的工作内存进行操作，首先将主内存的变量copy到工作内存，进行操作后，再将变量刷回到主内存中。所有线程只有通过主内存来进行通信。
>
> volatile关键字解决了可见性的问题。在多线程环境下，一个线程对共享变量的修改可能不会立即被其他线程可见，因为数据可能被缓存在线程的本地内存中。volatile关键字可以保证被修饰的变量对所有线程可见，任何一个线程对变量的修改都会立刻被其他线程读取到。
>
> synchronized关键字和volatile关键字的区别主要体现在以下几点： 
>
> 1. 作用范围：synchronized关键字可以用来修饰方法或代码块，对一段代码进行同步控制，实现临界区互斥访问；而volatile关键字只能修饰成员变量，用来保证访问该变量的线程之间的可见性。 
> 2. 原子性：synchronized关键字可以保证代码块的原子性，即在一个线程执行synchronized代码块时，其他线程不能执行该代码块；而volatile关键字只能保证变量对所有线程的可见性，不能保证操作的原子性。
> 3. 重排序：synchronized关键字保证了原子性同时也保证了有序性，能够防止指令重排序；而volatile关键字只能保证可见性，不能保证有序性。

### 8.Java 内存区域和 JMM 有何区别？

> Java内存区域是Java虚拟机在运行时内存分配的结构，主要包括了堆内存、方法区、虚拟机栈、本地方法栈和程序计数器等 
>
> Java内存模型（JMM）是描述Java程序中多线程之间如何进行内存交互的规范，包括可见性、有序性和原子性等概念。

### 9.happens-before 原则

> 指的就是多线程中操作的理想顺序化关系，比如同一个线程中，前面的操作的先发生。在锁规则中，上锁肯定在解锁之前。

### 10.synchronized 关键字的作⽤

> 主要就是保证修饰的方法或者代码块在同一时刻只能有一个线程执行。本质上就是上锁。

### 11.synchronized 和 ReentrantLock 的区别

> 1.synchronized是关键字,ReentrantLock是Java API提供的类
>
> 2.ReentrantLock可实现公平锁和可中断，synchronized 不易扩展和定制
>
> 3.性能方面的话，因为synchronized 是JVM层面的锁，然后现在迭代优化了，两者性能差不多。

### 12.synchronized 和 volatile 的区别。

> `synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！
>
> - `volatile` 关键字是线程同步的轻量级实现，所以 `volatile`性能肯定比`synchronized`关键字要好 。但是 `volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块 。
> - `volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。
> - `volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性

### 13.synchronized 关键字的底层原理(难度高)

> 每个 Java 对象在内存中都会有一个对象头，对象头中包含了对象的一些元数据信息，其中就包括指向该对象的监视器（Monitor）的指针，用于实现对象锁。当一个线程获取了某个对象的监视器（锁）时，其他线程想要获取这个对象的监视器就会被阻塞，直到持有锁的线程释放锁。
>
> 在 Java 中，synchronized 关键字是通过进入和退出对象的 Monitor 对象来实现对对象的加锁和解锁。当一个线程进入 synchronized 代码块时，它会尝试获取对象的 Monitor 对象的锁，如果获取成功就可以执行代码，如果获取失败就会被阻塞。当线程执行完 synchronized 代码块时，会释放对象的 Monitor 对象的锁。

### 14.ThreadLocal 关键字的作⽤，内存泄露问题

>  **`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，可以将`ThreadLocal`类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。**
>
> `ThreadLocalMap` 中使用的 key 为 `ThreadLocal` 的弱引用，而 value 是强引用。所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。
>
> 这样一来，`ThreadLocalMap` 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。`ThreadLocalMap` 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 `ThreadLocal`方法后最好手动调用`remove()`方法。

### 15.线程池有什么⽤？为什么不推荐使⽤内置线程池？

> 为了线程复用，减少线程创建和销毁的开销，提高程序性能。
>
> **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
>
> **提高响应速度**。当任务到达时，任务可以不需要等到线程创建就能立即执行。
>
> **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。
>
> 最关键的是，如果不使用线程池，有可能会造成系统创建大量同类线程而导致消耗完内存或者“过度切换”的问题。
>
> 因为内置线程池的任务队列一般不作限制，会堆积大量的请求，导致OOM；

### 16.Java 线程池有哪些参数？阻塞队列有⼏种？拒绝策略有⼏种？

> ```java
>     /**
>      * 用给定的初始参数创建一个新的ThreadPoolExecutor。
>      */
>     public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
>                               int maximumPoolSize,//线程池的最大线程数
>                               long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
>                               TimeUnit unit,//时间单位
>                               BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
>                               ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
>                               RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
>                                ) {
> 
> ```
>
> 常见的阻塞队列有以下几种：
>
> 1. ArrayBlockingQueue：基于数组的有界阻塞队列，容量固定，不支持扩容。
> 2. LinkedBlockingQueue：基于链表的无界阻塞队列，容量无限，可能会导致内存溢出。
> 3. SynchronousQueue：不存储元素的阻塞队列，每个插入操作必须等待另一个线程的删除操作。
> 4. PriorityBlockingQueue：基于优先级的阻塞队列，可以根据元素的优先级进行排序。
>
> 常见的拒绝策略有以下几种：
>
> 1. AbortPolicy（默认策略）：直接抛出RejectedExecutionException异常。
>
> 2. CallerRunsPolicy：由提交任务的线程执行该任务，如果线程池未停止，则会继续执行后续任务。
>
> 3. DiscardPolicy：默默的抛弃掉无法处理的任务，不做任何处理。
>
> 4. DiscardOldestPolicy：抛弃队列中等待时间最长的任务，然后尝试重新提交当前任务

### 17.线程池处理任务的流程了解吗？

> 如果当前运行的线程数小于核心线程数，那么就会新建一个线程来执行任务。
>
> 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，那么就把该任务放入到任务队列里等待执行。
>
> 如果向任务队列投放任务失败（任务队列已经满了），但是当前运行的线程数是小于最大线程数的，就新建一个线程来执行任务。
>
> 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，饱和策略会调用`RejectedExecutionHandler.rejectedExecution()`方法

### 18.实现 Runnable 接⼝和 Callable 接⼝的区别。

> 1. Runnable 接口是Java中用于表示可运行任务的接口，它只有一个run()方法，没有返回值。通过实现Runnable接口，可以将任务提交给线程池执行。
> 2. Callable 接口也是Java中用于表示可运行任务的接口，但它有一个call()方法，并且可以返回执行结果。通过实现Callable接口，可以创建一个带有返回值的任务，并提交给线程池执行。
>
> 主要区别：
>
> -  Runnable接口的run方法没有返回值，不会抛出受检异常；而Callable接口的call方法具有返回值，可以抛出受检异常。 
> -  Callable接口的call方法允许抛出异常，可以获取任务执行过程中的异常信息；而Runnable接口的run方法不能抛出异常，只能通过try-catch捕获异常。 
> -  Callable接口的call方法有返回值，可以通过Future类来获取任务执行的结果；而Runnable接口没有返回值，无法直接获取执行结果。

### 19.如何给线程池命名？为什么建议给线程池命名？ 

> 初始化线程池的时候需要显示命名（设置线程池名称前缀），有利于定位问题。
>
> 默认情况下创建的线程名字类似 `pool-1-thread-n` 这样的，没有业务含义，不利于我们定位问题。

### 20.如何动态修改线程池参数？(较难)

> [如何设置线程池参数？美团给出了一个让面试官虎躯一震的回答。 (qq.com)](https://mp.weixin.qq.com/s?__biz=Mzg3NjU3NTkwMQ==&mid=2247505103&idx=1&sn=a041dbec689cec4f1bbc99220baa7219&source=41#wechat_redirect)

### 21.AQS 原理了解么？AQS 组件有哪些？(较难)

### 22.Semaphore 有什么⽤？原理是什么？

> 控制同时访问特定资源的线程数量。
>
> `Semaphore` 是共享锁的一种实现，它默认构造 AQS 的 `state` 值为 `permits`，你可以将 `permits` 的值理解为许可证的数量，只有拿到许可证的线程才能执行。
>
> 调用`semaphore.acquire()` ，线程尝试获取许可证，如果 `state >= 0` 的话，则表示可以获取成功。如果获取成功的话，使用 CAS 操作去修改 `state` 的值 `state=state-1`。如果 `state<0` 的话，则表示许可证数量不足。此时会创建一个 Node 节点加入阻塞队列，挂起当前线程。
>
> ```java
> /**
>  *  获取1个许可证
>  */
> public void acquire() throws InterruptedException {
>     sync.acquireSharedInterruptibly(1);
> }
> /**
>  * 共享模式下获取许可证，获取成功则返回，失败则加入阻塞队列，挂起线程
>  */
> public final void acquireSharedInterruptibly(int arg)
>     throws InterruptedException {
>     if (Thread.interrupted())
>       throw new InterruptedException();
>         // 尝试获取许可证，arg为获取许可证个数，当可用许可证数减当前获取的许可证数结果小于0,则创建一个节点加入阻塞队列，挂起当前线程。
>     if (tryAcquireShared(arg) < 0)
>       doAcquireSharedInterruptibly(arg);
> }
> 
> ```
>
> 调用`semaphore.release();` ，线程尝试释放许可证，并使用 CAS 操作去修改 `state` 的值 `state=state+1`。释放许可证成功之后，同时会唤醒同步队列中的一个线程。被唤醒的线程会重新尝试去修改 `state` 的值 `state=state-1` ，如果 `state>=0` 则获取令牌成功，否则重新进入阻塞队列，挂起线程。
>
> ```java
> // 释放一个许可证
> public void release() {
>     sync.releaseShared(1);
> }
> 
> // 释放共享锁，同时会唤醒同步队列中的一个线程。
> public final boolean releaseShared(int arg) {
>     //释放共享锁
>     if (tryReleaseShared(arg)) {
>       //唤醒同步队列中的一个线程
>       doReleaseShared();
>       return true;
>     }
>     return false;
> }
> 
> ```

### 23.CountDownLatch 有什么⽤？原理是什么？

> `CountDownLatch` 允许 `count` 个线程阻塞在一个地方，直至所有线程的任务都执行完毕。
>
> `CountDownLatch` 是一次性的，计数器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当 `CountDownLatch` 使用完毕后，它不能再次被使用。
>
> `CountDownLatch` 是共享锁的一种实现,它默认构造 AQS 的 `state` 值为 `count`。当线程使用 `countDown()` 方法时,其实使用了`tryReleaseShared`方法以 CAS 的操作来减少 `state`,直至 `state` 为 0 。当调用 `await()` 方法的时候，如果 `state` 不为 0，那就证明任务还没有执行完毕，`await()` 方法就会一直阻塞，也就是说 `await()` 方法之后的语句不会被执行。直到`count` 个线程调用了`countDown()`使 state 值被减为 0，或者调用`await()`的线程被中断，该线程才会从阻塞中被唤醒，`await()` 方法之后的语句得到执行。

### 24.CyclicBarrier 有什么⽤？原理是什么？

> `CyclicBarrier` 和 `CountDownLatch` 非常类似，它也可以实现线程间的技术等待，但是它的功能比 `CountDownLatch` 更加复杂和强大。主要应用场景和 `CountDownLatch` 类似。
>
> `CountDownLatch` 的实现是基于 AQS 的，而 `CycliBarrier` 是基于 `ReentrantLock`(`ReentrantLock` 也属于 AQS 同步器)和 `Condition` 的。
>
> `CyclicBarrier` 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是：让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。
>
> `CyclicBarrier` 内部通过一个 `count` 变量作为计数器，`count` 的初始值为 `parties` 属性的初始化值，每当一个线程到了栅栏这里了，那么就将计数器减 1。如果 count 值为 0 了，表示这是这一代最后一个线程到达栅栏，就尝试执行我们构造方法中输入的任务。
>
> ```java
> //每次拦截的线程数
> private final int parties;
> //计数器
> private int count;
> ```
>
> 下面我们结合源码来简单看看。
>
> 1、`CyclicBarrier` 默认的构造方法是 `CyclicBarrier(int parties)`，其参数表示屏障拦截的线程数量，每个线程调用 `await()` 方法告诉 `CyclicBarrier` 我已经到达了屏障，然后当前线程被阻塞。
>
> ```java
> public CyclicBarrier(int parties) {
>     this(parties, null);
> }
> 
> public CyclicBarrier(int parties, Runnable barrierAction) {
>     if (parties <= 0) throw new IllegalArgumentException();
>     this.parties = parties;
>     this.count = parties;
>     this.barrierCommand = barrierAction;
> }
> ```
>
> 其中，parties 就代表了有拦截的线程的数量，当拦截的线程数量达到这个值的时候就打开栅栏，让所有线程通过。
>
> 2、当调用 `CyclicBarrier` 对象调用 `await()` 方法时，实际上调用的是 `dowait(false, 0L)`方法。 `await()` 方法就像树立起一个栅栏的行为一样，将线程挡住了，当拦住的线程数量达到 `parties` 的值时，栅栏才会打开，线程才得以通过执行。
>
> ```java
> public int await() throws InterruptedException, BrokenBarrierException {
>   try {
>       return dowait(false, 0L);
>   } catch (TimeoutException toe) {
>       throw new Error(toe); // cannot happen
>   }
> }
> ```

### 25.多个任务的编排可以怎么做？项⽬⽤到了 CompletableFuture 吗？

> 使用CompletableFuture，没用过 