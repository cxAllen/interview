# interview
JVM相关

1.内存模型以及分区，需要详细到每个分区存放什么？<br/>
jvm内存模型是线程工作内存与主内存相当于计算机的缓存和主内存。每个线程都有自己的工作内存，工作内存存放着数据副本，
当他们需要读取数据时，就要从主存中先read到工作内存中，再load到工作内存副本中，在通过use指令将变量传递给执行引擎。
写数据时，要先将数据assign到工作内存变量副本，再将数据从工作内存store到主存中，再利用指令write将store的变量写入到主存中的变量。<br/>
JVM分区：<br/>
a.程序计数器：（线程私有）当前线程执行字节码指令的行号信息。<br/>
b.虚拟机栈：（线程私有）一个方法对应一个栈帧，一个方法的执行就对应一个栈帧的入栈和出栈。<br/>
一个栈帧包含：（线程私有）1.局部变量表（记录局部变量信息int/reference）2.操作技术栈（变量操作的指令写入或读取）3.动态链接（字符引用转换成直接引用）4.方法出口（正常或异常返回地址）.<br/>
c.本地方法栈：与虚拟机栈类似，主要用于本地方法的执行对应入栈与出栈。<br/>
d.堆区：（线程共享）对象创建存放。<br/>
e.元数据区：类信息，静态变量，代码信息。<br/>

2.对象创建方法，对象的内存分配和对象访问定位？<br/>
对象创建方法就是new关键字或者是反射的newInstance。<br/>
对象内存分配：先确定该对象所属的类是否已经被加载到虚拟机中，在元数据区中能否找到该类的类信息。如果有的话，就直接在堆区创建一块内存。<br/>
对象访问定位：利用内存地址信息找到具体内存。<br/>

3.GC中2种判定方法？<br/>
a.引用计数法：每有一个地方引用该对象计数加1，引用失效计数减1，当计数为0时，就标记位可回收对象。缺点：2个对象相互引用，计数永远不为0无法回收对象<br/>
b.可达性算法：当某个对象与所有的root引用无法形成关联时，则该对象可回收。root引用对象可为虚拟机栈中的引用对象，静态/常量引用对象，本地方法栈的引用对象.<br/>

4.GC三种收集算法？<br/>
a.标记清除算法：初始标记回收对象，标记完成后直接回收所有标记过的对象。缺点：效率不高，容易造成内存空间碎片。<br/>
b.标记整理算法：初始标记回收对象，标记完成后不直接回收对象而是将所有存活的对象移动到一端，移动完成后再将端边界以外的所有区域全部回收。<br/>
c.复制算法：先将内存分为2个相等大小的区块S0，S1，只在S0内存中分配内存。当S0内存不足时，先将分配内存的中存活对象移动到另S1内存块中，然后清空S0内存块。<br/>

5.GC收集器？<br/>
a.Serial收集器 年轻代 单线程收集器 复制算法 会停止用户线程 <br/>
b.ParNew收集器 年轻代 并行多线程收集器 复制算法 stop-the-world <br/>
c.Parallel Scavenge收集器 年轻代 并行多线程收集器 复制算法  stop-the-world <br/>
Parallel Scavenge收集器的特点是它的关注点与其他收集器不同，CMS等收集器的关注点是尽可能地缩短垃圾收集时用户线程的停顿时间，而Parallel Scavenge 收集器的目标则是达到一个可控制的吞吐量（Throughput）。所谓吞吐量就是CPU用于运行用户代码的时间与CPU总消耗时间的比值，即吞吐量=运行用户代码时间/（运行用户代码时间+垃圾收集时间），虚拟机总共运行了100分钟，其中垃圾收集花掉1分钟，那吞吐量就是99%. <br/>
d.Serial Old收集器 老年代 单线程收集器 标记整理算法 会停止用户线程 <br/>
e.Parallel Old收集器 老年代 多线程收集器 标记整理算法 停止用户线程 <br/>
f.CMS(concurrent mark sweep）并发标记清除收集器 老年代 多线程 标记清除算法 用户线程停止时间短 初始标记--并发标记--重新标记--并发清除
CMS会产生内存空间碎片，空间碎片过多会导致fullGC，所以可以设置JVM参数使得CMS清除对象时进行空间整理，但会让用户停顿时间变长一点。 <br/>
g.G1(garbage-first) 内存分为很多块大小相同的region分区 年轻代/老年代 并发多线程 复制算法 标记整理算法 初始标记--并发标记--最终标记--筛选回收 <br/>

6.MinorGC和FullGC发生在什么时候？<br/>
MinorGC发生在年轻代空间不足，无法分配内存给对象时。<br/>
发生Minor GC之前，虚拟机会先检查老年代最大可用的连续空间是否大于新生代所有对象总空间。<br/>
FullGC发生在老年代空间不足，如创建大对象时直接要分配在老年代；年轻代对象到达存活阀值需要进入老年代；minorGC后S1内存无法存放下存活对象需要空间担保分配至老年代，当老年代的最大可用连续空间小于存活对象且HandlePromotionFailure设置不允许冒险，那这时也要改为进行一次FullGC。<br/>

7.类加载过程？<br/>
加载---验证---准备---解析---初始化---使用---卸载<br/>
加载：根据类全限定名将class文件载入虚拟机并转化为运行时数据区，再在堆区中创建class对象。<br/>
验证：进行class文件验证是否有魔数；文件格式验证；是否符合语法；字节码验证是否会对jvm造成危害；<br/>
准备：对类中的静态变量赋默认值，或者变量时由final修饰则赋变量值；<br/>
解析：将常量池中的符号引用解析为直接引用；常量池中主要存放两大类常量：字面量（Literal）和符号引用（Symbolic References）。
![image](https://github.com/cxAllen/interview/blob/master/%E5%B8%B8%E9%87%8F%E6%B1%A0.png)<br/>
初始化：将类变量以及静态代码块执行；<br/>

8.双亲委派模式？<br/>
BootstrapClassLoad (虚拟机启动类加载器JAVA_HOME/lib) <---ExtensionClassLoad（扩展类加载器JAVA_HOME/lib/ext) <---ApplicationClassLoad(应用类加载器classPath) <---自定义类加载器<br/>
一个类再被加载的时候，当前的类加载器会先将类交由父类加载器加载。<br/>

9.分派：静态分派和动态分派？</br>
/**</br>
*方法静态分派演示</br>
*@author allen</br>
*/</br>
public class StaticDispatch {</br>
static abstract class Human{</br>
}</br>
static class Man extends Human{</br>
}</br>
static class Woman extends Human{</br>
}</br>
public void sayHello（Human guy）{</br>
System.out.println（"hello,guy！"）；</br>
}</br>
public void sayHello（Man guy）{</br>
System.out.println（"hello,gentleman！"）；</br>
}</br>
public void sayHello（Woman guy）{</br>
System.out.println（"hello,lady！"）；</br>
}</br>
public static void main（String[]args）{</br>
Human man=new Man（）；</br>
Human woman=new Woman（）；</br>
StaticDispatch sr=new StaticDispatch（）；</br>
sr.sayHello（man）；</br>
sr.sayHello（woman）；</br>
}</br>
}</br>
</br>
/**</br>
*方法动态分派演示</br>
*@author allen</br>
*/</br>
public class DynamicDispatch{</br>
static abstract class Human{</br>
protected abstract void sayHello（）；</br>
}</br>
static class Man extends Human{</br>
@Override</br>
protected void sayHello（）{</br>
System.out.println（"man say hello"）；</br>
}</br>
}</br>
static class Woman extends Human{</br>
@Override</br>
protected void sayHello（）{</br>
System.out.println（"woman say hello"）；</br>
}</br>
}</br>
public static void main（String[]args）{</br>
Human man=new Man（）；</br>
Human woman=new Woman（）；</br>
man.sayHello（）；</br>
woman.sayHello（）；</br>
man=new Woman（）；</br>
man.sayHello（）；</br>
}</br>
}</br>

JVM内存优化？</br>
jmap: Java内存映射工具(Java Memory Map)，主要用于打印指定Java进程、核心文件或远程调试服务器的共享对象内存映射或堆内存细节。 可以导出heap dump文件，然后利用MAT内存分析工具进行分析问题所在。</br>
jstack: Java堆栈跟踪工具，主要用于打印指定Java进程、核心文件或远程调试服务器的Java线程的堆栈跟踪信息。<br/>
JVM工具详细：https://blog.csdn.net/zhongjh1/article/details/52909720

