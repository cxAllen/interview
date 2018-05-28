# interview
JVM面试相关
1.内存模型以及分区，需要详细到每个分区存放什么？
jvm内存模型是线程工作内存与主内存相当于计算机的缓存和主内存。每个线程都有自己的工作内存，工作内存存放着数据副本，
当他们需要读取数据时，就要从主存中先read到工作内存中，再load到工作内存副本中，在通过use指令将变量传递给执行引擎。
写数据时，要先将数据assign到工作内存变量副本，再将数据从工作内存store到主存中，再利用指令write将store的变量写入到主存中的变量。
JVM分区：
a.程序计数器：（线程私有）当前线程执行字节码指令的行号信息。
b.虚拟机栈：（线程私有）一个方法对应一个栈帧，一个方法的执行就对应一个栈帧的入栈和出栈。
一个栈帧包含：（线程私有）1.局部变量表（记录局部变量信息int/reference）2.操作技术栈（变量操作的指令写入或读取）3.动态链接（字符引用转换成直接引用）4.方法出口（正常或异常返回地址）.
c.本地方法栈：与虚拟机栈类似，主要用于本地方法的执行对应入栈与出栈。
d.堆区：（线程共享）对象创建存放。
e.元数据区：类信息，静态变量，代码信息。

2.对象创建方法，对象的内存分配和对象访问定位？
对象创建方法就是new关键字或者是反射的newInstance。
对象内存分配：先确定该对象所属的类是否已经被加载到虚拟机中，在元数据区中能否找到该类的类信息。如果有的话，就直接在堆区创建一块内存。
对象访问定位：利用内存地址信息找到具体内存。

3.GC中2种判定方法？
a.引用计数法：每有一个地方引用该对象计数加1，引用失效计数减1，当计数为0时，就标记位可回收对象。缺点：2个对象相互引用，计数永远不为0无法回收对象。
b.可达性算法：当某个对象与所有的root引用无法形成关联时，则该对象可回收。root引用对象可为虚拟机栈中的引用对象，静态/常量引用对象，本地方法栈中的引用对象.
