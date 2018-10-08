[TOC]
##常见问题
####1 什么是Java虚拟机？为什么Java被称作是“平台无关的编程语言”？
Java虚拟机是一个可以执行Java字节码的虚拟机进程。Java源文件被编译成能被Java虚拟机执行的字节码文件。

Java被设计成允许应用程序可以运行在任意的平台，而不需要程序员为每一个平台单独重写或者是重新编译。Java虚拟机让这个变为可能，因为它知道底层硬件平台的指令长度和其他特性。

####2 JDK和JRE的区别是什么？
Java运行时环境(JRE)是将要执行Java程序的Java虚拟机。它同时也包含了执行applet需要的浏览器插件。Java开发工具包(JDK)是完整的Java软件开发包，包含了JRE，编译器和其他的工具(比如：JavaDoc，Java调试器)，可以让开发者开发、编译、执行Java应用程序。

####3 ”static”关键字是什么意思？Java中是否可以覆盖(override)一个private或者是static的方法？
“static”关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例变量的情况下被访问。

Java中static方法不能被覆盖，因为方法覆盖是基于运行时动态绑定的，而static方法是编译时静态绑定的。static方法跟类的任何实例都不相关，所以概念上不适用。

####4 是否可以在static环境中访问非static变量？
static变量在Java中是属于类的，它在所有的实例中的值是一样的。当类被Java虚拟机载入的时候，会对static变量进行初始化。如果你的代码尝试不用实例来访问非static的变量，编译器会报错，因为这些变量还没有被创建出来，还没有跟任何实例关联上。

####5 Java支持的数据类型有哪些？什么是自动拆装箱？
Java语言支持的8中基本数据类型有：byte、short、int、long、float、double、boolean、char

自动装箱是Java编译器在基本数据类型和对应的对象包装类型之间做的一个转化。比如：把int转化成Integer，double转化成Double，等等。反之就是自动拆箱。

####6 Java中的方法覆盖(Overriding)和方法重载(Overloading)是什么意思？
Java中的方法重载发生在同一个类里面两个或者是多个方法的方法名相同但是参数不同的情况。与此相对，方法覆盖是说子类重新定义了父类的方法。方法覆盖必须有相同的方法名，参数列表和返回类型。覆盖者可能不会限制它所覆盖的方法的访问。

####7 Java中，什么是构造函数？什么是构造函数重载？什么是复制构造函数？
当新对象被创建的时候，构造函数会被调用。每一个类都有构造函数。在程序员没有给类提供构造函数的情况下，Java编译器会为这个类创建一个默认的构造函数。

Java中构造函数重载和方法重载很相似。可以为一个类创建多个构造函数。每一个构造函数必须有它自己唯一的参数列表。

Java不支持像C++中那样的复制构造函数，这个不同点是因为如果你不自己写构造函数的情况下，Java不会创建默认的复制构造函数。

####8 Java支持多继承么？
不支持，Java不支持多继承。每个类都只能继承一个类，但是可以实现多个接口。

####9 接口和抽象类的区别是什么？
Java提供和支持创建抽象类和接口。它们的实现有共同点，不同点在于：
- 接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法。
- 类可以实现很多个接口，但是只能继承一个抽象类
- 类如果要实现一个接口，它必须要实现接口声明的所有方法。但是，类可以不实现抽象类声明的所有方法，当然，在这种情况下，类也必须得声明成是抽象的。
- 抽象类可以在不提供接口方法实现的情况下实现接口。
- Java接口中声明的变量默认都是final的。抽象类可以包含非final的变量。
- Java接口中的成员函数默认是public的。抽象类的成员函数可以是private，protected或者是public。
- 接口是绝对抽象的，不可以被实例化。抽象类也不可以被实例化，但是，如果它包含main方法的话是可以被调用的。
也可以参考JDK8中抽象类和接口的区别

####10 什么是值传递和引用传递？
对象被值传递，意味着传递了对象的一个副本。因此，就算是改变了对象副本，也不会影响源对象的值。

对象被引用传递，意味着传递的并不是实际的对象，而是对象的引用。因此，外部对引用对象所做的改变会反映到所有的对象上。

##线程
####1 进程和线程的区别是什么？
进程是执行着的应用程序，而线程是进程内部的一个执行序列。一个进程可以有多个线程。线程又叫做轻量级进程。

####2 创建线程有几种不同的方式？你喜欢哪一种？为什么？
有三种方式可以用来创建线程：
- 继承Thread类
- 实现Runnable接口
- 应用程序可以使用Executor框架来创建线程池

实现Runnable接口这种方式更受欢迎，因为这不需要继承Thread类。在应用设计中已经继承了别的对象的情况下，这需要多继承（而Java不支持多继承），只能实现接口。同时，线程池也是非常高效的，很容易实现和使用。

####3 概括的解释下线程的几种可用状态
线程在执行过程中，可以处于下面几种状态：
- 就绪(Runnable):线程准备运行，不一定立马就能开始执行。
- 运行中(Running)：进程正在执行线程的代码。
- 等待中(Waiting):线程处于阻塞的状态，等待外部的处理结束。
- 睡眠中(Sleeping)：线程被强制睡眠。I/O阻塞(Blocked on I/O)：等待I/O操作完成。
- 同步阻塞(Blocked on Synchronization)：等待获取锁。
- 死亡(Dead)：线程完成了执行。

####4 同步方法和同步代码块的区别是什么？
在Java语言中，每一个对象有一把锁。线程可以使用synchronized关键字来获取对象上的锁。synchronized关键字可应用在方法级别(粗粒度锁)或者是代码块级别(细粒度锁)。

####5 在监视器(Monitor)内部，是如何做线程同步的？程序应该做哪种级别的同步？
监视器和锁在Java虚拟机中一块使用。监视器监视一块同步代码块，确保一次只有一个线程执行同步代码块。每一个监视器都和一个对象引用相关联。线程在获取锁之前不允许执行同步代码。

####6 什么是死锁(deadlock)？
两个进程都在等待对方执行完毕才能继续往下执行的时候就发生了死锁。结果就是两个进程都陷入了无限的等待中。

####7 如何确保N个线程可以访问N个资源同时又不导致死锁？
使用多线程的时候，一种非常简单的避免死锁的方式就是：指定获取锁的顺序，并强制线程按照指定的顺序获取锁。因此，如果所有的线程都是以同样的顺序加锁和释放锁，就不会出现死锁了。

##集合
####1 Java集合类框架的基本接口有哪些？
集合类接口指定了一组叫做元素的对象。集合类接口的每一种具体的实现类都可以选择以它自己的方式对元素进行保存和排序。有的集合类允许重复的键，有些不允许。

Java集合类提供了一套设计良好的支持对一组对象进行操作的接口和类。Java集合类里面最基本的接口有：
- Collection：代表一组对象，每一个对象都是它的子元素。
- Set：不包含重复元素的Collection。
- List：有顺序的collection，并且可以包含重复元素。
- Map：可以把键(key)映射到值(value)的对象，键不能重复。

####2 为什么集合类没有实现Cloneable和Serializable接口？
克隆(cloning)或者是序列化(serialization)的语义和含义是跟具体的实现相关的。因此，应该由集合类的具体实现来决定如何被克隆或者是序列化。

####3 什么是迭代器(Iterator)？
Iterator接口提供了很多对集合元素进行迭代的方法。每一个集合类都包含了可以返回迭代器实例的迭代方法。迭代器可以在迭代的过程中删除底层集合的元素。

####4 Iterator和ListIterator的区别是什么？
下面列出了他们的区别：

- Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。
- Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。

ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

####5 快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？
Iterator的安全失败是基于对底层集合做拷贝，因此，它不受源集合上修改的影响。java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException异常，而安全失败的迭代器永远不会抛出这样的异常。

####6 Java中的HashMap的工作原理是什么？
Java中的HashMap是以键值对(key-value)的形式存储元素的。HashMap需要一个hash函数，它使用hashCode()和equals()方法来向集合/从集合添加和检索元素。当调用put()方法的时候，HashMap会计算key的hash值，然后把键值对存储在集合中合适的索引上。如果key已经存在了，value会被更新成新值。

HashMap的一些重要的特性是它的容量(capacity)，负载因子(load factor)和扩容极限(threshold resizing)。

####7 hashCode()和equals()方法的重要性体现在什么地方？
Java中的HashMap使用hashCode()和equals()方法来确定键值对的索引，当根据键获取值的时候也会用到这两个方法。如果没有正确的实现这两个方法，两个不同的键可能会有相同的hash值，因此，可能会被集合认为是相等的。而且，这两个方法也用来发现重复元素。所以这两个方法的实现对HashMap的精确性和正确性是至关重要的。

####8 HashMap和Hashtable有什么区别？
HashMap和Hashtable都实现了Map接口，因此很多特性非常相似。但是，他们有以下不同点：

- HashMap允许键和值是null，而Hashtable不允许键或者值是null。
- Hashtable是同步的，而HashMap不是。因此，HashMap更适合于单线程环境，而Hashtable适合于多线程环境。
- HashMap提供了可供应用迭代的键的集合，因此，HashMap是快速失败的。另一方面，Hashtable提供了对键的列举(Enumeration)。
- ** 一般认为Hashtable是一个遗留的类。**

####9 数组(Array)和列表(ArrayList)有什么区别？什么时候应该使用Array而不是ArrayList？
下面列出了Array和ArrayList的不同点：

- Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
- Array大小是固定的，ArrayList的大小是动态变化的。
- ArrayList提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。

对于基本类型数据，集合使用自动装箱来减少编码工作量。但是，当处理固定大小的基本数据类型的时候，这种方式相对比较慢。

####10 ArrayList和LinkedList有什么区别？
ArrayList和LinkedList都实现了List接口，他们有以下的不同点：

- ArrayList是基于索引的数据接口，它的底层是数组。它可以以O(1)时间复杂度对元素进行随机访问。与此对应，LinkedList是以元素列表的形式存储它的数据，每一个元素都和它的前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)。
- 相对于ArrayList，LinkedList的插入，添加，删除操作速度更快，因为当元素被添加到集合任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。
- LinkedList比ArrayList更占内存，因为LinkedList为每一个节点存储了两个引用，一个指向前一个元素，一个指向下一个元素。

也可以参考ArrayList vs. LinkedList。

####11 Comparable和Comparator接口是干什么的？列出它们的区别
Java提供了只包含一个compareTo()方法的Comparable接口。这个方法可以个给两个对象排序。具体来说，它返回负数，0，正数来表明输入对象小于，等于，大于已经存在的对象。

Java提供了包含compare()和equals()两个方法的Comparator接口。compare()方法用来给两个输入参数排序，返回负数，0，正数表明第一个参数是小于，等于，大于第二个参数。equals()方法需要一个对象作为参数，它用来决定输入参数是否和comparator相等。只有当输入参数也是一个comparator并且输入参数和当前comparator的排序结果是相同的时候，这个方法才返回true。

####12 什么是Java优先级队列(Priority Queue)？
PriorityQueue是一个基于优先级堆的无界队列，它的元素是按照自然顺序(natural order)排序的。在创建的时候，我们可以给它提供一个负责给元素排序的比较器。PriorityQueue不允许null值，因为他们没有自然顺序，或者说他们没有任何的相关联的比较器。最后，PriorityQueue不是线程安全的，入队和出队的时间复杂度是O(log(n))。

####13 你了解大O符号(big-O notation)么？你能给出不同数据结构的例子么？
大O符号描述了当数据结构里面的元素增加的时候，算法的规模或者是性能在最坏的场景下有多么好。

大O符号也可用来描述其他的行为，比如：内存消耗。因为集合类实际上是数据结构，我们一般使用大O符号基于时间，内存和性能来选择最好的实现。大O符号可以对大量数据的性能给出一个很好的说明。

####14 如何权衡是使用无序的数组还是有序的数组？
有序数组最大的好处在于查找的时间复杂度是O(log n)，而无序数组是O(n)。有序数组的缺点是插入操作的时间复杂度是O(n)，因为值大的元素需要往后移动来给新元素腾位置。相反，无序数组的插入时间复杂度是常量O(1)。

####15 Java集合类框架的最佳实践有哪些？
根据应用的需要正确选择要使用的集合的类型对性能非常重要，比如：假如元素的大小是固定的，而且能事先知道，我们就应该用Array而不是ArrayList。

有些集合类允许指定初始容量。因此，如果我们能估计出存储的元素的数目，我们可以设置初始容量来避免重新计算hash值或者是扩容。

为了类型安全，可读性和健壮性的原因总是要使用泛型。同时，使用泛型还可以避免运行时的ClassCastException。

使用JDK提供的不变类(immutable class)作为Map的键可以避免为我们自己的类实现hashCode()和equals()方法。

编程的时候接口优于实现。

底层的集合实际上是空的情况下，返回长度是0的集合或者是数组，不要返回null。

####16 Enumeration接口和Iterator接口的区别有哪些？
Enumeration速度是Iterator的2倍，同时占用更少的内存。但是，Iterator远远比Enumeration安全，因为其他线程不能够修改正在被iterator遍历的集合里面的对象。同时，Iterator允许调用者删除底层集合里面的元素，这对Enumeration来说是不可能的。

>Enumeration接口中定义了一些方法，通过这些方法可以枚举（一次获得一个）对象集合中的元素。
这种传统接口已被迭代器取代，虽然Enumeration 还未被遗弃，但在现代代码中已经被很少使用了。尽管如此，它还是使用在诸如Vector和Properties这些传统类所定义的方法中，除此之外，还用在一些API类，并且在应用程序中也广泛被使用。 下表总结了一些Enumeration声明的方法：
- boolean hasMoreElements( ):测试此枚举是否包含更多的元素
- Object nextElement( ):如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素

####17 HashSet和TreeSet有什么区别？
HashSet是由一个hash表来实现的，因此，它的元素是无序的。add()，remove()，contains()方法的时间复杂度是O(1)。

另一方面，TreeSet是由一个树形的结构来实现的，它里面的元素是有序的。因此，add()，remove()，contains()方法的时间复杂度是O(logn)。

##GC 垃圾收集器(Garbage Collectors)
####1 Java中垃圾回收有什么目的？什么时候进行垃圾回收？
垃圾回收的目的是识别并且丢弃应用不再使用的对象来释放和重用资源。

####2 System.gc()和Runtime.gc()会做什么事情？
这两个方法用来提示JVM要进行垃圾回收。但是，立即开始还是延迟进行垃圾回收是取决于JVM的。
java.lang.System.gc()只是java.lang.Runtime.getRuntime().gc()的简写，两者的行为没有任何不同。

####3 finalize()方法什么时候被调用？析构函数(finalization)的目的是什么？
在释放对象占用的内存之前，垃圾收集器会调用对象的finalize()方法。一般建议在该方法中释放对象持有的资源。

####4 如果对象的引用被置为null，垃圾收集器是否会立即释放对象占用的内存？
不会，在下一个垃圾回收周期中，这个对象将是可被回收的。

####5 Java堆的结构是什么样子的？什么是堆中的永久代(Perm Gen space)?
JVM的堆是运行时数据区，所有类的实例和数组都是在堆上分配内存。它在JVM启动的时候被创建。对象所占的堆内存是由自动内存管理系统也就是垃圾收集器回收。

堆内存是由存活和死亡的对象组成的。存活的对象是应用可以访问的，不会被垃圾回收。死亡的对象是应用不可访问尚且还没有被垃圾收集器回收掉的对象。一直到垃圾收集器把这些对象回收掉之前，他们会一直占据堆内存空间。

根据 JVM 规范，JVM 内存共分为虚拟机栈、堆、方法区、程序计数器、本地方法栈五个部分，如下图所示：
![](img/JVMMemStructure.jpg)  
1. 虚拟机栈：每个线程有一个私有的栈，随着线程的创建而创建。栈里面存着的是一种叫“栈帧”的东西，每个方法会创建一个栈帧，栈帧中存放了局部变量表（基本数据类型和对象引用）、操作数栈、方法出口等信息。栈的大小可以固定也可以动态扩展。当栈调用深度大于JVM所允许的范围，会抛出StackOverflowError的错误，不过这个深度范围不是一个恒定的值，可以通过如下的测试程序来测试
~~~java
public class MyTest 
{ 
	private static int index = 0; 
	public static void call() { 
		index++; 
		call(); 
	} 
	public static void main(String[] args) { 
		try { 
		call(); 
		} catch (Throwable e) { 
			System.out.println(“Index is ” + index); 
		} 
	} 
} 
~~~
运行结果：
![](img/MemTestResult.jpg)  
2. 本地方法栈：这部分主要与虚拟机用到的 Native 方法相关，一般情况下， Java 应用程序员并不需要关心这部分的内容
3. PC寄存器：PC 寄存器，也叫程序计数器。JVM支持多个线程同时运行，每个线程都有自己的程序计数器。倘若当前执行的是 JVM 的方法，则该寄存器中保存当前执行指令的地址；倘若执行的是native 方法，则PC寄存器中为空
4. 堆：堆内存是 JVM 所有线程共享的部分，在虚拟机启动的时候就已经创建。所有的对象和数组都在堆上进行分配。这部分空间可通过 GC 进行回收。当申请不到空间时会抛出 OutOfMemoryError。如下的测试程序：
![](img/OverStackTestResult.jpg)  
5. 方法区：方法区也是所有线程共享。主要用于存储类的信息、常量池、方法数据、方法代码等。方法区逻辑上属于堆的一部分，但是为了与堆进行区分，通常又叫“非堆”。 关于方法区内存溢出的问题会在下文中详细探讨
>永久代（PermGen） 
绝大部分 Java 程序员应该都见过 “java.lang.OutOfMemoryError: PermGen space “这个异常。这里的 “PermGen space”其实指的就是方法区。不过方法区和“PermGen space”又有着本质的区别。前者是 JVM 的规范，而后者则是 JVM 规范的一种实现，并且只有 HotSpot 才有 “PermGen space”，而对于其他类型的虚拟机，如 JRockit（Oracle）、J9（IBM） 并没有“PermGen space”。由于方法区主要存储类的相关信息，所以对于动态生成类的情况比较容易出现永久代的内存溢出。最典型的场景就是，在 jsp 页面比较多的情况，容易出现永久代内存溢出。我们现在通过动态生成类来模拟 “PermGen space”的内存溢出
~~~java
public class MyTest 
{ 
	public static void main(String[] args) { 
		URL url = null; 
		List classLoaderList = new ArrayList(); 
		try { 
			url = new File(“/tmp”).toURI().toURL(); 
			URL[] urls = {url}; 
			while (true){ 
				ClassLoader loader = new URLClassLoader(urls); 
				classLoaderList.add(loader); 
				loader.loadClass(“com.huawei.MyTest”); 
			} 
		} catch (Exception e) { 
			e.printStackTrace(); 
		} 
	} 
}
~~~
运行结果：  
![](img/PermGenTestResult.jpg)  
>Metaspace（元空间）:其实，移除永久代的工作从JDK1.7就开始了。JDK1.7中，存储在永久代的部分数据就已经转移到了Java Heap或者是 Native Heap。但永久代仍存在于JDK1.7中，并没完全移除，譬如符号引用(Symbols)转移到了native heap；字面量(interned strings)转移到了java heap；类的静态变量(class statics)转移到了java heap。我们可以通过一段程序来比较 JDK 1.6 与 JDK 1.7及 JDK 1.8 的区别，以字符串常量为例：
~~~java
package com.paddx.test.memory; 
import java.util.ArrayList; 
import java.util.List;
public class StringOomMock { 
	static String base = “string”; 
	public static void main(String[] args) { 
		List list = new ArrayList(); 
		for (int i=0;i< Integer.MAX_VALUE;i++){ 
			String str = base + base; 
			base = str; 
			list.add(str.intern()); 
		} 
	} 
} 
~~~
这段程序以2的指数级不断的生成新的字符串，这样可以比较快速的消耗内存。我们通过 JDK 1.6、JDK 1.7 和 JDK 1.8 分别运行：
1.6运行结果：
![](img/jdk1.6TestResult.png)
1.7运行结果：
![](img/jdk1.7TestResult.png)
1.8运行结果：
![](img/jdk1.8TestResult.png)
从上述结果可以看出，JDK 1.6下，会出现“PermGen Space”的内存溢出，而在 JDK 1.7和 JDK 1.8 中，会出现堆内存溢出，并且 JDK 1.8中 PermSize 和 MaxPermGen 已经无效。因此，可以大致验证 JDK 1.7 和 1.8 将字符串常量由永久代转移到堆中，并且 JDK 1.8 中已经不存在永久代的结论。现在我们看看元空间到底是一个什么东西？
元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限制，但可以通过以下参数来指定元空间的大小：
-XX:MetaspaceSize，初始空间大小，达到该值就会触发垃圾收集进行类型卸载，同时GC会对该值进行调整：如果释放了大量的空间，就适当降低该值；如果释放了很少的空间，那么在不超过MaxMetaspaceSize时，适当提高该值。 
-XX:MaxMetaspaceSize，最大空间，默认是没有限制的。
除了上面两个指定大小的选项以外，还有两个与 GC 相关的属性： 
-XX:MinMetaspaceFreeRatio，在GC之后，最小的Metaspace剩余空间容量的百分比，减少为分配空间所导致的垃圾收集 
-XX:MaxMetaspaceFreeRatio，在GC之后，最大的Metaspace剩余空间容量的百分比，减少为释放空间所导致的垃圾收集
现在我们在 JDK 8下重新运行一下代码段 4，不过这次不再指定 PermSize 和 MaxPermSize。而是指定 MetaSpaceSize 和 MaxMetaSpaceSize的大小。输出结果如下：
![](img/MetaSpaceSize.png)  
从输出结果，我们可以看出，这次不再出现永久代溢出，而是出现了元空间的溢出。

####6 串行(serial)收集器和吞吐量(throughput)收集器的区别是什么？
吞吐量收集器使用并行版本的新生代垃圾收集器，它用于中等规模和大规模数据的应用程序。而串行收集器对大多数的小应用(在现代处理器上需要大概100M左右的内存)就足够了

####7 在Java中，对象什么时候可以被垃圾回收？
当对象对当前使用这个对象的应用程序变得不可触及的时候，这个对象就可以被回收了。

####8 JVM的永久代中会发生垃圾回收么？
垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收(Full GC)。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。请参考下Java8：从永久代到元数据区
(译者注：Java8中已经移除了永久代，新加了一个叫做元数据区的native内存区)

##异常处理
####1 Java中的两种异常类型是什么？他们有什么区别？
Java中有两种异常：受检查的(checked)异常和不受检查的(unchecked)异常。不受检查的异常不需要在方法或者是构造函数上声明，就算方法或者是构造函数的执行可能会抛出这样的异常，并且不受检查的异常可以传播到方法或者是构造函数的外面。相反，受检查的异常必须要用throws语句在方法或者是构造函数上声明。这里有Java异常处理的一些小建议。

####2 Java中Exception和Error有什么区别？
Exception和Error都是Throwable的子类。Exception用于用户程序可以捕获的异常情况。Error定义了不期望被用户程序捕获的异常。

####3 throw和throws有什么区别？
throw关键字用来在程序中明确的抛出异常，相反，throws语句用来表明方法不能处理的异常。每一个方法都必须要指定哪些异常不能处理，所以方法的调用者才能够确保处理可能发生的异常，多个异常是用逗号分隔的。

####4 异常处理的时候，finally代码块的重要性是什么？
无论是否抛出异常，finally代码块总是会被执行。就算是没有catch语句同时又抛出异常的情况下，finally代码块仍然会被执行。最后要说的是，finally代码块主要用来释放资源，比如：I/O缓冲区，数据库连接。

####5 异常处理完成以后，Exception对象会发生什么变化？
Exception对象会在下一个垃圾回收过程中被回收掉。

####6 finally代码块和finalize()方法有什么区别？
无论是否抛出异常，finally代码块都会执行，它主要是用来释放应用占用的资源。finalize()方法是Object类的一个protected方法，它是在对象被垃圾回收之前由Java虚拟机来调用的。

##Applet小程序
####1 什么是Applet？
java applet是能够被包含在HTML页面中并且能被启用了java的客户端浏览器执行的程序。Applet主要用来创建动态交互的web应用程序。

####2 解释一下Applet的生命周期
applet可以经历下面的状态：
- Init：每次被载入的时候都会被初始化。
- Start：开始执行applet。
- Stop：结束执行applet。
- Destroy：卸载applet之前，做最后的清理工作。

####3 当applet被载入的时候会发生什么？
首先，创建applet控制类的实例，然后初始化applet，最后开始运行。

####4 Applet和普通的Java应用程序有什么区别？
applet是运行在启用了java的浏览器中，Java应用程序是可以在浏览器之外运行的独立的Java程序。但是，它们都需要有Java虚拟机。

进一步来说，Java应用程序需要一个有特定方法签名的main函数来开始执行。Java applet不需要这样的函数来开始执行。

最后，Java applet一般会使用很严格的安全策略，Java应用一般使用比较宽松的安全策略。

####5 Java applet有哪些限制条件？
主要是由于安全的原因，给applet施加了以下的限制：
- applet不能够载入类库或者定义本地方法。
- applet不能在宿主机上读写文件。
- applet不能读取特定的系统属性。
- applet不能发起网络连接，除非是跟宿主机。
- applet不能够开启宿主机上其他任何的程序。

####6 什么是不受信任的applet？
不受信任的applet是不能访问或是执行本地系统文件的Java applet，默认情况下，所有下载的applet都是不受信任的。

####7 从网络上加载的applet和从本地文件系统加载的applet有什么区别？
当applet是从网络上加载的时候，applet是由applet类加载器载入的，它受applet安全管理器的限制。

当applet是从客户端的本地磁盘载入的时候，applet是由文件系统加载器载入的。

从文件系统载入的applet允许在客户端读文件，写文件，加载类库，并且也允许执行其他程序，但是，却通不过字节码校验。

####8 applet类加载器是什么？它会做哪些工作？
当applet是从网络上加载的时候，它是由applet类加载器载入的。类加载器有自己的java名称空间等级结构。类加载器会保证来自文件系统的类有唯一的名称空间，来自网络资源的类有唯一的名称空间。

当浏览器通过网络载入applet的时候，applet的类被放置于和applet的源相关联的私有的名称空间中。然后，那些被类加载器载入进来的类都是通过了验证器验证的。验证器会检查类文件格式是否遵守Java语言规范，确保不会出现堆栈溢出(stack overflow)或者下溢(underflow)，传递给字节码指令的参数是正确的。

####9 applet安全管理器是什么？它会做哪些工作？
applet安全管理器是给applet施加限制条件的一种机制。浏览器可以只有一个安全管理器。安全管理器在启动的时候被创建，之后不能被替换覆盖或者是扩展。

##Swing
####1 弹出式选择菜单(Choice)和列表(List)有什么区别
Choice是以一种紧凑的形式展示的，需要下拉才能看到所有的选项。Choice中一次只能选中一个选项。List同时可以有多个元素可见，支持选中一个或者多个元素。

####2 什么是布局管理器？
布局管理器用来在容器中组织组件

####3 滚动条(Scrollbar)和滚动面板(JScrollPane)有什么区别？
Scrollbar是一个组件，不是容器。而ScrollPane是容器。ScrollPane自己处理滚动事件。

####4 哪些Swing的方法是线程安全的？
只有3个线程安全的方法： repaint(), revalidate(), and invalidate()。

####5 说出三种支持重绘(painting)的组件。
Canvas, Frame, Panel,和Applet支持重绘。

####6 什么是裁剪(clipping)？
限制在一个给定的区域或者形状的绘图操作就做裁剪

####7 MenuItem和CheckboxMenuItem的区别是什么？
CheckboxMenuItem类继承自MenuItem类，支持菜单选项可以选中或者不选中。

####8 边缘布局(BorderLayout)里面的元素是如何布局的？
BorderLayout里面的元素是按照容器的东西南北中进行布局的。

####9 网格包布局(GridBagLayout)里面的元素是如何布局的？
GridBagLayout里面的元素是按照网格进行布局的。不同大小的元素可能会占据网格的多于1行或一列。因此，行数和列数可以有不同的大小。

####10 Window和Frame有什么区别？
Frame类继承了Window类，它定义了一个可以有菜单栏的主应用窗口。

####11 裁剪(clipping)和重绘(repainting)有什么联系？
当窗口被AWT重绘线程进行重绘的时候，它会把裁剪区域设置成需要重绘的窗口的区域。

####12 事件监听器接口(event-listener interface)和事件适配器(event-adapter)有什么关系？
事件监听器接口定义了对特定的事件，事件处理器必须要实现的方法。事件适配器给事件监听器接口提供了默认的实现。

####13 GUI组件如何来处理它自己的事件？
GUI组件可以处理它自己的事件，只要它实现相对应的事件监听器接口，并且把自己作为事件监听器。

####14 Java的布局管理器比传统的窗口系统有哪些优势？
Java使用布局管理器以一种一致的方式在所有的窗口平台上摆放组件。因为布局管理器不会和组件的绝对大小和位置相绑定，所以他们能够适应跨窗口系统的特定平台的不同。

####15 Java的Swing组件使用了哪种设计模式？
Java中的Swing组件使用了MVC(视图-模型-控制器)设计模式。

##JDBC
####1 什么是JDBC？
JDBC是允许用户在不同数据库之间做选择的一个抽象层。JDBC允许开发者用JAVA写数据库应用程序，而不需要关心底层特定数据库的细节。

####2 解释下驱动(Driver)在JDBC中的角色。
JDBC驱动提供了特定厂商对JDBC API接口类的实现，驱动必须要提供java.sql包下面这些类的实现：Connection, Statement, PreparedStatement,CallableStatement, ResultSet和Driver。

####3 Class.forName()方法有什么作用？
这个方法用来载入跟数据库建立连接的驱动。

####4 PreparedStatement比Statement有什么优势？
PreparedStatements是预编译的，因此，性能会更好。同时，不同的查询参数值，PreparedStatement可以重用。

####5 什么时候使用CallableStatement？用来准备CallableStatement的方法是什么？
CallableStatement用来执行存储过程。存储过程是由数据库存储提供的。存储过程可以接受输入参数，也可以有返回结果。非常鼓励使用存储过程，因为它提供了安全性和模块化。准备一个CallableStatement的方法是：
CallableStament.prepareCall();

####6 数据库连接池是什么意思？
像打开关闭数据库连接这种和数据库的交互可能是很费时的，尤其是当客户端数量增加的时候，会消耗大量的资源，成本是非常高的。可以在应用服务器启动的时候建立很多个数据库连接并维护在一个池中。连接请求由池中的连接提供。在连接使用完毕以后，把连接归还到池中，以用于满足将来更多的请求。

##远程方法调用(RMI)
####1 什么是RMI？
Java远程方法调用(Java RMI)是Java API对远程过程调用(RPC)提供的面向对象的等价形式，支持直接传输序列化的Java对象和分布式垃圾回收。远程方法调用可以看做是激活远程正在运行的对象上的方法的步骤。RMI对调用者是位置透明的，因为调用者感觉方法是执行在本地运行的对象上的。看下RMI的一些注意事项。

####2 RMI体系结构的基本原则是什么？
RMI体系结构是基于一个非常重要的行为定义和行为实现相分离的原则。RMI允许定义行为的代码和实现行为的代码相分离，并且运行在不同的JVM上。

####3 RMI体系结构分哪几层？
RMI体系结构分以下几层：

- 存根和骨架层(Stub and Skeleton layer)：这一层对程序员是透明的，它主要负责拦截客户端发出的方法调用请求，然后把请求重定向给远程的RMI服务。
- 远程引用层(Remote Reference Layer)：RMI体系结构的第二层用来解析客户端对服务端远程对象的引用。这一层解析并管理客户端对服务端远程对象的引用。连接是点到点的。
- 传输层(Transport layer)：这一层负责连接参与服务的两个JVM。这一层是建立在网络上机器间的TCP/IP连接之上的。它提供了基本的连接服务，还有一些防火墙穿透策略。

####4 RMI中的远程接口(Remote Interface)扮演了什么样的角色？
远程接口用来标识哪些方法是可以被非本地虚拟机调用的接口。远程对象必须要直接或者是间接实现远程接口。实现了远程接口的类应该声明被实现的远程接口，给每一个远程对象定义构造函数，给所有远程接口的方法提供实现。

####5 java.rmi.Naming类扮演了什么样的角色？
java.rmi.Naming类用来存储和获取在远程对象注册表里面的远程对象的引用。Naming类的每一个方法接收一个URL格式的String对象作为它的参数。

####6 RMI的绑定(Binding)是什么意思？
绑定是为了查询找远程对象而给远程对象关联或者是注册以后会用到的名称的过程。远程对象可以使用Naming类的bind()或者rebind()方法跟名称相关联。

####7 Naming类的bind()和rebind()方法有什么区别？
bind()方法负责把指定名称绑定给远程对象，rebind()方法负责把指定名称重新绑定到一个新的远程对象。如果那个名称已经绑定过了，先前的绑定会被替换掉。

####8 让RMI程序能正确运行有哪些步骤？
为了让RMI程序能正确运行必须要包含以下几个步骤：

- 编译所有的源文件。
- 使用rmic生成stub。
- 启动rmiregistry。
- 启动RMI服务器。
- 运行客户端程序。

####9 RMI的stub扮演了什么样的角色？
远程对象的stub扮演了远程对象的代表或者代理的角色。调用者在本地stub上调用方法，它负责在远程对象上执行方法。当stub的方法被调用的时候，会经历以下几个步骤：

- 初始化到包含了远程对象的JVM的连接。
- 序列化参数到远程的JVM。
- 等待方法调用和执行的结果。
- 反序列化返回的值或者是方法没有执行成功情况下的异常。
- 把值返回给调用者。

####10 什么是分布式垃圾回收(DGC)？它是如何工作的？
DGC叫做分布式垃圾回收。RMI使用DGC来做自动垃圾回收。因为RMI包含了跨虚拟机的远程对象的引用，垃圾回收是很困难的。DGC使用引用计数算法来给远程对象提供自动内存管理。

####11 RMI中使用RMI安全管理器(RMISecurityManager)的目的是什么？
RMISecurityManager使用下载好的代码提供可被RMI应用程序使用的安全管理器。如果没有设置安全管理器，RMI的类加载器就不会从远程下载任何的类。

####12 解释下Marshalling和demarshalling。
当应用程序希望把内存对象跨网络传递到另一台主机或者是持久化到存储的时候，就必须要把对象在内存里面的表示转化成合适的格式。这个过程就叫做Marshalling，反之就是demarshalling。

####13 解释下Serialization和Deserialization。
Java提供了一种叫做对象序列化的机制，他把对象表示成一连串的字节，里面包含了对象的数据，对象的类型信息，对象内部的数据的类型信息等等。因此，序列化可以看成是为了把对象存储在磁盘上或者是从磁盘上读出来并重建对象而把对象扁平化的一种方式。反序列化是把对象从扁平状态转化成活动对象的相反的步骤

####14 什么是Servlet？
Servlet是用来处理客户端请求并产生动态网页内容的Java类。Servlet主要是用来处理或者是存储HTML表单提交的数据，产生动态内容，在无状态的HTTP协议下管理状态信息。

####15 说一下Servlet的体系结构。
所有的Servlet都必须要实现的核心的接口是javax.servlet.Servlet。每一个Servlet都必须要直接或者是间接实现这个接口，或者是继承javax.servlet.GenericServlet或者javax.servlet.http.HTTPServlet。最后，Servlet使用多线程可以并行的为多个请求服务。

####16 Applet和Servlet有什么区别？
Applet是运行在客户端主机的浏览器上的客户端Java程序。而Servlet是运行在web服务器上的服务端的组件。Applet可以使用用户界面类，而Servlet没有用户界面，相反，Servlet是等待客户端的HTTP请求，然后为请求产生响应。

####17 GenericServlet和HttpServlet有什么区别？
GenericServlet是一个通用的协议无关的Servlet，它实现了Servlet和ServletConfig接口。继承自GenericServlet的Servlet应该要覆盖service()方法。最后，为了开发一个能用在网页上服务于使用HTTP协议请求的Servlet，你的Servlet必须要继承自HttpServlet。

####18 解释下Servlet的生命周期。
对每一个客户端的请求，Servlet引擎载入Servlet，调用它的init()方法，完成Servlet的初始化。然后，Servlet对象通过为每一个请求单独调用service()方法来处理所有随后来自客户端的请求，最后，调用Servlet(译者注：这里应该是Servlet而不是server)的destroy()方法把Servlet删除掉。

####19 doGet()方法和doPost()方法有什么区别？
**doGet**：GET方法会把名值对追加在请求的URL后面。因为URL对字符数目有限制，进而限制了用在客户端请求的参数值的数目。并且请求中的参数值是可见的，因此，敏感信息不能用这种方式传递。

**doPOST**：POST方法通过把请求参数值放在请求体中来克服GET方法的限制，因此，可以发送的参数的数目是没有限制的。最后，通过POST请求传递的敏感信息对外部客户端是不可见的。

####20 什么是Web应用程序？
Web应用程序是对Web或者是应用服务器的动态扩展。有两种类型的Web应用：面向表现的和面向服务的。面向表现的Web应用程序会产生包含了很多种标记语言和动态内容的交互的web页面作为对请求的响应。而面向服务的Web应用实现了Web服务的端点(endpoint)。一般来说，一个Web应用可以看成是一组安装在服务器URL名称空间的特定子集下面的Servlet的集合。

####21 什么是服务端包含(Server Side Include)？
服务端包含(SSI)是一种简单的解释型服务端脚本语言，大多数时候仅用在Web上，用servlet标签嵌入进来。SSI最常用的场景把一个或多个文件包含到Web服务器的一个Web页面中。当浏览器访问Web页面的时候，Web服务器会用对应的servlet产生的文本来替换Web页面中的servlet标签。

####22 什么是Servlet链(Servlet Chaining)？
Servlet链是把一个Servlet的输出发送给另一个Servlet的方法。第二个Servlet的输出可以发送给第三个Servlet，依次类推。链条上最后一个Servlet负责把响应发送给客户端。

####23 如何知道是哪一个客户端的机器正在请求你的Servlet？
ServletRequest类可以找出客户端机器的IP地址或者是主机名。getRemoteAddr()方法获取客户端主机的IP地址，getRemoteHost()可以获取主机名。

####24 HTTP响应的结构是怎么样的？
HTTP响应由三个部分组成：
**状态码(Status Code)**：描述了响应的状态。可以用来检查是否成功的完成了请求。请求失败的情况下，状态码可用来找出失败的原因。如果Servlet没有返回状态码，默认会返回成功的状态码HttpServletResponse.SC_OK。

**HTTP头部(HTTP Header)**：它们包含了更多关于响应的信息。比如：头部可以指定认为响应过期的过期日期，或者是指定用来给用户安全的传输实体内容的编码格式。如何在Serlet中检索HTTP的头部看这里。

**主体(Body)**：它包含了响应的内容。它可以包含HTML代码，图片，等等。主体是由传输在HTTP消息中紧跟在头部后面的数据字节组成的。

####25 什么是cookie？session和cookie有什么区别？
cookie是Web服务器发送给浏览器的一块信息。浏览器会在本地文件中给每一个Web服务器存储cookie。以后浏览器在给特定的Web服务器发请求的时候，同时会发送所有为该服务器存储的cookie。下面列出了session和cookie的区别：

无论客户端浏览器做怎么样的设置，session都应该能正常工作。客户端可以选择禁用cookie，但是，session仍然是能够工作的，因为客户端无法禁用服务端的session。

在存储的数据量方面session和cookies也是不一样的。session能够存储任意的Java对象，cookie只能存储String类型的对象。

####26 浏览器和Servlet通信使用的是什么协议？
浏览器和Servlet通信使用的是HTTP协议。

####27 什么是HTTP隧道？
HTTP隧道是一种利用HTTP或者是HTTPS把多种网络协议封装起来进行通信的技术。因此，HTTP协议扮演了一个打通用于通信的网络协议的管道的包装器的角色。把其他协议的请求掩盖成HTTP的请求就是HTTP隧道

####28 sendRedirect()和forward()方法有什么区别？
sendRedirect()方法会创建一个新的请求，而forward()方法只是把请求转发到一个新的目标上。重定向(redirect)以后，之前请求作用域范围以内的对象就失效了，因为会产生一个新的请求，而转发(forwarding)以后，之前请求作用域范围以内的对象还是能访问的。一般认为sendRedirect()比forward()要慢。

####29 什么是URL编码和URL解码？
URL编码是负责把URL里面的空格和其他的特殊字符替换成对应的十六进制表示，反之就是解码。

####30 什么是JSP页面？
JSP页面是一种包含了静态数据和JSP元素两种类型的文本的文本文档。静态数据可以用任何基于文本的格式来表示，比如：HTML或者XML。JSP是一种混合了静态内容和动态产生的内容的技术。这里看下JSP的例子。

####31 JSP请求是如何被处理的？
浏览器首先要请求一个以.jsp扩展名结尾的页面，发起JSP请求，然后，Web服务器读取这个请求，使用JSP编译器把JSP页面转化成一个Servlet类。需要注意的是，只有当第一次请求页面或者是JSP文件发生改变的时候JSP文件才会被编译，然后服务器调用servlet类，处理浏览器的请求。一旦请求执行结束，servlet会把响应发送给客户端。这里看下如何在JSP中获取请求参数

####32 JSP有什么优点？
下面列出了使用JSP的优点：
- JSP页面是被动态编译成Servlet的，因此，开发者可以很容易的更新展现代码。
- JSP页面可以被预编译。
- JSP页面可以很容易的和静态模板结合，包括：HTML或者XML，也可以很容易的和产生动态内容的代码结合起来。
- 开发者可以提供让页面设计者以类XML格式来访问的自定义的JSP标签库。
- 开发者可以在组件层做逻辑上的改变，而不需要编辑单独使用了应用层逻辑的页面。

####33 什么是JSP指令(Directive)？JSP中有哪些不同类型的指令？

Directive是当JSP页面被编译成Servlet的时候，JSP引擎要处理的指令。Directive用来设置页面级别的指令，从外部文件插入数据，指定自定义的标签库。Directive是定义在 <%@ 和 %>之间的。下面列出了不同类型的Directive：

- 包含指令(Include directive)：用来包含文件和合并文件内容到当前的页面。
- 页面指令(Page directive)：用来定义JSP页面中特定的属性，比如错误页面和缓冲区。
- Taglib指令： 用来声明页面中使用的自定义的标签库。

####34 什么是JSP动作(JSP action)？

JSP动作以XML语法的结构来控制Servlet引擎的行为。当JSP页面被请求的时候，JSP动作会被执行。它们可以被动态的插入到文件中，重用JavaBean组件，转发用户到其他的页面，或者是给Java插件产生HTML代码。下面列出了可用的动作：

- jsp:include-当JSP页面被请求的时候包含一个文件。
- jsp:useBean-找出或者是初始化Javabean。
- jsp:setProperty-设置JavaBean的属性。
- jsp:getProperty-获取JavaBean的属性。
- jsp:forward-把请求转发到新的页面。
- jsp:plugin-产生特定浏览器的代码。

####35 什么是Scriptlets？
JSP技术中，scriptlet是嵌入在JSP页面中的一段Java代码。scriptlet是位于标签内部的所有的东西，在标签与标签之间，用户可以添加任意有效的scriplet。

####36 声明(Decalaration)在哪里？
声明跟Java中的变量声明很相似，它用来声明随后要被表达式或者scriptlet使用的变量。添加的声明必须要用开始和结束标签包起来。

####37 什么是表达式(Expression)？
JSP表达式是Web服务器把脚本语言表达式的值转化成一个String对象，插入到返回给客户端的数据流中。表达式是在<%=和%>这两个标签之间定义的。

####38 隐含对象是什么意思？有哪些隐含对象？
JSP隐含对象是页面中的一些Java对象，JSP容器让这些Java对象可以为开发者所使用。开发者不用明确的声明就可以直接使用他们。JSP隐含对象也叫做预定义变量。下面列出了JSP页面中的隐含对象：

- application
- page
- request
- response
- session
- exception
- out
- config
- pageContext




