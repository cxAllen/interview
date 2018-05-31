    多线程

    什么是线程？<br/>
    线程是操作系统可调度的最小执行单元。<br/>

    什么是线程安全和线程不安全？<br/>
    线程安全就是多个线程对共享变量进行操作，不会引起程序执行结果的正确性。<br/>

    什么是自旋锁？(锁优化)<br/>
    如果物理机器有一个以上的处理器，能让两个或以上的线程同时并行执行，我们就可以让后面请求锁的那个线程“稍等一下”，但不放弃处理器的执行时间，<br/>
    看看持有锁的线程是否很快就会释放锁。为了让线程等待，我们只需让线程执行一个忙循环（自旋），这项技术就是所谓的自旋锁。<br/>
    在JDK1.6中引入了自适应的自旋锁。自适应意味着自旋的时间不再固定了，而是由一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。如果在同一个锁对象上，自旋等<br/>
    待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也很有可能再次成功，进而它将允许自旋等待持续相对更长的时间，比如100个循环。<br/>
    另外，如果对于某个锁，自旋很少成功获得过，那在以后要获取这个锁时将可能省略掉自旋过程，以避免浪费处理器资源。<br/>
    
    ![image](https://github.com/cxAllen/interview/blob/master/markword.png)
    ![image](https://github.com/cxAllen/interview/blob/master/%E9%94%81%E4%BC%98%E5%8C%96%E7%AE%80%E5%8C%96%E6%B5%81%E7%A8%8B.png)
    
    synchronized和ReentrantLock的区别？<br/>
    synchronized关键字经过编译之后，会在同步块的前后分别形成monitorenter和monitorexit这两个字节码指令，<br/>
    这两个字节码都需要一个reference类型的参数来指明要锁定和解锁的对象。如果Java程序中的 synchronized明确指定了对象参数，那就是这个对象的reference；如果没有明确指定，<br/>
    那就根据synchronized修饰的是实例方法还是类方法，去取对应的对象实例或Class对象来作为锁对象。根据虚拟机规范的要求，在执行monitorenter指令时，首先要尝试获取对象的锁。<br/>
    如果这个对象没被锁定，或者当前线程已经拥有了那个对象的锁，把锁的计数器加1，相应的，在执行monitorexit指令时会将锁计数器减1，当计数器为0时，锁就被释放。<br/>
    如果获取对象锁失败，那当前线程就要阻塞等待，直到对象锁被另外一个线程释放为止。<br/>
    
    ReentrantLock是利用AQS同步队列器的独占资源模式，控制state状态来实现锁机制。<br/>

    什么是Java内存模型？<br/>
    每个线程都有自己的工作内存，工作内存中有共享变量的副本及其是否失效的标志位。工作内存对共享变量的操作需要与主内存的交互。<br/>

    什么是CAS？<br/>
    compare and swap 比较并替换 三个参数V、A、B；V内存地址，A期望值，B更新值；<br/>
    JUC包中原子类调用unsafe.compareAndSwapInt(this,valueOffset,expect,update)来保证数据操作的原子性。<br/>
    CAS会出现的问题：1.ABA 解决：加版本号 2.循环开销大 解决：JVM添加暂停指令。<br/>

    什么是乐观锁和悲观锁？<br/>
    悲观锁是悲观地认为每次进行数据操作都必须要以独占的方式即加锁来执行，才能保证数据正确性。<br/>
    乐观锁是相对于悲观锁而言的，乐观锁认为每次数据操作一般不会发生冲突，只在数据更新时进行冲突检测，如果有冲突就返回信息结束操作。<br/>

    什么是AQS？<br/>
    AQS定义了一套多线程访问共享资源的同步器框架，许多同步类实现都依赖于它，如常用的ReentrantLock/Semaphore/CountDownLatch.。<br/>
    它维护了一个volatile int state（代表共享资源）和一个FIFO线程等待队列（多线程争用资源被阻塞时会进入此队列）。<br/>
    AQS定义两种资源共享方式：Exclusive(独占，只有一个线程能执行，如ReentrantLock)和Share(共享，多个线程可同时执行，如Semaphore/CountDownLatch)。<br/>
    不同的自定义同步器争用共享资源的方式也不同。自定义同步器在实现时只需要实现共享资源state的获取与释放方式来控制资源是独占还是共享。<br/>

    什么是原子操作？在Java Concurrency API中有哪些原子类(atomic classes)？<br/>
    原子操作就是一个操作在执行过程中不会受到其他操作的影响，不会被打断。AtomicInteger....<br/>

    什么是Executors框架？<br/>
    便于创建线程池的框架。<br/>

    什么是阻塞队列？如何使用阻塞队列来实现生产者-消费者模型？<br/>
    阻塞队列是添加2种附加操作的队列。<br/>
    2个附加操作：1.阻塞插入：插入时当队列满时会一直阻塞至队列不满为止；2.阻塞移除：移动时当队列为空时，会一直阻塞至队列不为空；<br/>
    生产者向队列添加产品，消费者向队列消费产品；<br/>

    什么是Callable和Future?<br/>
    可以异步获取线程执行结果的工具。<br/>
    什么是FutureTask?<br/>
    FutureTask是实现了runnable和future接口的类，也是可以异步获取线程执行结果；<br/>

    什么是同步容器和并发容器的实现？<br/>
    同步容器实现大多是用syncharonized关键字修饰；<br/>
    并发容器实现大多是用AQS和CAS实现；<br/>

    什么是多线程？优缺点？<br/>
    多线程是进程中启动多个线程执行任务。<br/>
    优点：利用计算机多核优势；将耗费长时间的任务交给后台线程执行；<br/>
    缺点：代码复杂；线程安全；<br/>

    什么是多线程的上下文切换？<br/>
    多线程上下文切换时指CPU给每个线程分配时间片，在时间片时间到后CPU需要记录每个线程执行位置，便于下次执行。<br/>

    ThreadLocal的设计理念与作用？<br/>
    Java中的ThreadLocal类允许我们创建只能被同一个线程读写的变量。<br/>

    ThreadPool（线程池）用法与优势？<br/>
    减少线程创建和销毁次数，复用线程，控制最大线程数。<br/>

    Concurrent包里的其他东西：ArrayBlockingQueue、CountDownLatch等等。<br/>
    
    Semaphore有什么作用？<br/>
    它的作用是限制某段代码块的并发数。<br/>
    信号量可以用来做限流，创建n个许可证，表示只允许n个线程并发执行。<br/>

    Java Concurrency API中的Lock接口(Lock interface)是什么？对比同步它有什么优势？<br/>
    可以使线程在等待锁的时候响应中断，可以设置成公平锁。<br/>
    
    Hashtable的size()方法中明明只有一条语句”return count”，为什么还要做同步？<br/>

    ConcurrentHashMap的并发度是什么？<br/>
    同时可以有多少线程并发执行。<br/>

    ReentrantReadWriteLock读写锁的使用？<br/>
    读读不互斥，读写互斥，写写互斥<br/>

    CyclicBarrier和CountDownLatch的用法及区别？<br/>
    都是用来控制多个线程达到同一状态，再统一执行一段逻辑。<br/>
    区别是CyclicBarrier是可重用的；一个是加法，一个减法；<br/>

    LockSupport工具？<br/>
    LockSupport是JDK中比较底层的类，用来创建锁和其他同步工具类的基本线程阻塞。<br/>
    
    Condition接口及其实现原理？<br/>
    相当于wait、notify用于线程间的通信。实现原理是线程1.lock()加入AQS等待队列，调用await方法后从AQS等待队列中移除，并加入condition等待队列，<br/>
    线程2获取到锁加入AQS等待队列，线程2调用signal方法准备唤醒线程1从condition队列取出并加入AQS队列，但要在线程2执行unlock后才会从AQS队列中唤醒。<br/>

    Fork/Join框架的理解?<br/>
    拆分一个大的任务为多个小任务来执行逻辑。工作窃取算法<br/>

    wait()和sleep()的区别?<br/>
    wait()会释放锁然后会一直等待被唤醒，sleep()不释放锁到时间后就重新进入可运行状态。<br/>

    线程的五个状态（五种状态，创建、就绪、运行、阻塞和死亡）?<br/>
    start()方法和run()方法的区别？<br/>
    Runnable接口和Callable接口的区别？<br/>

    volatile关键字的作用？<br/>
    内存可见性：某个线程更新共享变量到主内存时，会将其他线程的该变量在工作内存中副本失效标志位置为失效，下次其他线程读取该变量时就会去主内存重新load。<br/>
    禁止指令重排序：字节码指令中会有标志位，使得后面的字节码指令无法被优化到标志位之前执行。<br/>

    Java中如何获取到线程dump文件？<br/>
    jps获取线程pid jstack pid 打印堆栈信息<br/>

    线程和进程有什么区别？<br/>
    进程是系统进行资源分配的基本单位，有独立的内存地址空间<br/>
    线程是CPU独立运行和独立调度的基本单位，没有单独地址空间，有独立的栈，局部变量，寄存器， 程序计数器等。<br/>

    线程实现的方式有几种（四种）？<br/>
    new Thread() 继承Thread、实现runnable接口、线程池、callable和future<br/>

    高并发、任务执行时间短的业务怎样使用线程池？并发不高、任务执行时间长的业务怎样使用线程池？并发高、业务执行时间长的业务怎样使用线程池？<br/>
    高并发、任务时间短使用CPU核心数的倍数线程池；<br/>
    并发不高、任务时间长可分为IO密集型、计算密集型；若是IO密集型则线程数可设置稍大一些，因为CPU的计算能力都在，时间主要用在了IO阻塞上。<br/>
    若是计算密集型则可将线程数设置为系统核心数，减少线程上下文切换。<br/>
    并发高且业务时间长的可考虑拆分业务。<br/>

    如果你提交任务时，线程池队列已满，这时会发生什么？<br/>
    队列为有界队列的情况<br/>
    如果当前线程数达到线程核心数且小于最大线程数，则会创建线程执行任务。<br/>
    如果线程数已经是最大线程数，则会启动拒绝策略；抛出异常策略：丢弃任务并抛出异常、丢弃策略：直接丢弃任务不执行、<br/>
    丢弃最老任务策略：丢弃最老的一个任务再去执行、强行执行策略：从当前线程池中取出一个线程执行当前任务。<br/>

    锁的等级：方法锁、对象锁、类锁?<br/>
    方法锁也是对象锁，锁定的是当前对象。<br/>
    类锁以及类方法锁，锁定的是类对象。<br/>
    
    如果同步块内的线程抛出异常会发生什么？<br/>
    释放锁，结束运行。<br/>

    并发编程（concurrency）并行编程（parallellism）有什么区别？<br/>
    并发多个线程切换执行，并行多个线程同时执行。<br/>

    如何保证多线程下 i++ 结果正确？<br/>
    加锁或原子类<br/>

    一个线程如果出现了运行时异常会怎么样?<br/>
    未被try catch住的线程直接结束运行。<br/>
    
    如何在两个线程之间共享数据?<br/>

    生产者消费者模型的作用是什么?<br/>
    通过平衡生产者的生产能力和消费者的消费能力来提高系统效率。<br/>

    怎么唤醒一个阻塞的线程?<br/>
    notify<br/>

    Java中用到的线程调度算法是什么<br/>
    抢占式，优先级高的可能执行概率高<br/>

    单例模式的线程安全性?<br/>
    DCL双重检测锁 枚举 <br/>
    
    线程类的构造方法、静态块是被哪个线程调用的?<br/>
    主线程<br/>

    同步方法和同步块，哪个是更好的选择?<br/>
    同步快<br/>

    如何检测死锁？怎么预防死锁？<br/>
    检测死锁：用2个容器，一个存储已经获取到的锁，另一个存储等待获取的锁；<br/>
    检测请求锁的线程，其请求的锁被其他线程获取，将这些线程记录下来，然后再检测这些线程是否有持有锁是其他线程所请求的，如果是的话则出现了死锁。<br/>
    预防死锁：良好的程序执行顺序；锁获取超时机制；<br/>