@[toc](目录)
# 基本知识
## Switch能否用string做参数？
1. switch在jdk7之后可以用String作为参数；
2. switch支持byte,short,char,int,enum,String六种数据类型，其对应的封装类在作为参数的时候会自动拆箱与装箱，因此实际上是不支持封装类的，可参考[switch case支持的6种数据类型](https://www.cnblogs.com/HDK2016/p/6854671.html)
## 八种基本数据类型的大小以及封装类
|类型|数据|表示范围|包装类
|--|--|--|--|
| byte(字节型)  | 1 |-128~127|Byte|
| short(短整型) |2|-32768～32767|Short|
| int(整型)|4|-2147483648～2147483647|Integer|
| long(长整型)|8|-9223372036854775808 ~ 9223372036854775807|Long|
| float(浮点型)|4|-3.4E38～3.4E38|Float|
| double(双精度型)|8|-1.7E308～1.7E308|Double|
| char(字符型)|2|从字符型对应的整型数来划分，其表示范围是0～65535|Charater|
|booealn(布尔型)|没明确定义|true或false|Boolean|
## Object有哪些公用方法？
1）公有方法：getClass,equals,toString,wait,notify,notifyall,hashCode
2）受保护方法：clone,finalize
3）私有方法：registerNatives
## Thread有哪些常用方法
1. activeCount：返回当前线程的线程组中活动线程的数目。
2. currentThread：返回对当前正在执行的线程对象的引用。
3. getId：获取线程id
4. getName：获取线程名称
5. getPriority：获取线程优先级别
6. isDaemon：是否为守护线程
7. interrupt：中断线程
8. isInterrupted：线程是否中断
9. join：等待线程
10. sleep：睡眠一段时间
11. start：启动线程
12. yield：暂停正在执行的线程，让其他线程执行； 
## Java的四中引用
1）强引用：通过new关键字创建的对象就是强引用，拥有强引用的对象，JVM不会回收该对象，即使内存用完了，JVM宁可报OOM（out of memory）也不会回收该对象；
2）软引用：当一个对象只拥有软引用，在内存足够的时候是永远不会回收该对象，只有当内存不足时，才会回收该对象；
3）弱引用：当一个对象只拥有弱引用时，当垃圾回收器检测到了就会进行回收。但是垃圾回收器是一个优先级很低的线程，并不一定立马就会回收掉；
4）虚引用：当一个对象只拥有虚引用的时候，他就跟没有引用一样，随时都会被回收掉
上面的四种引用对应的是new关键字以及java.lang.ref包中的SoftReference，WeakReference, PhantomReference，参考[java四种引用类型](https://www.toutiao.com/a6688637014524297732/)
## JDK7与JDK8中HashMap的实现？
jdk7中的HashMap是数组+链表，而在jdk8中增加了红黑树，用来提高HashMap的效率
## 多并发情况下HashMap是否还会产生死循环？
1）在jdk1.7之前会因为resize的时候调用get方法造成死循环，但是jdk1.8之后虽然修复了原来造成死循环的原因会因为其他原因造成死循环，例如：**多线程下操作同一对象时，对象内部属性的不一致性**
2）[死循环原因](https://blog.csdn.net/zhuqiuhui/article/details/51849692)，[死循环源代码参考](https://blog.csdn.net/u010412719/article/details/52049347)，[丢失数据原因](https://www.cnblogs.com/dongguacai/p/5599100.html)
3）主要是因为resize操作之后元素指针相互指向，再get时会出现死循环（cpu100%）
## Excption与Error包结构，OOM你遇到过哪些情况，SOF你遇到过哪些情况？
1）结构图![error跟exception结构图](https://img-blog.csdnimg.cn/20190509171228473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMwMzI1ODMz,size_16,color_FFFFFF,t_70)
2）[oom](https://www.cnblogs.com/baizhanshi/p/6704731.html)（out of memory）：主要是3种
	a：java堆溢出，java.lang.OutofMemoryError:Java heap space
	b：java栈溢出，主要是栈空间不足，会抛出OutofMemoryError，但是如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常
	c：java方法区（包括常量池）溢出，java.lang.OutOfMemoryError:PermGen space
3）sof（StackOverflow）：主要是递归调用或者死循环引起的请求栈深度大于虚拟机允许的最大深度；
## Java(OOP)面向对象的三个特征与含义？
1）封装：将某事物的属性和行为包装到对象中，这个对象只对外公布需要公开的属性和行为
2）继承：子对象可以继承父对象的属性和行为
3）多态：不同类对象对同一消息做出不同的响应，重载：静态多态，重写：动态多态
## foreach与正常for循环效率对比
1）for循环根据下标来循环，因此用来遍历数组类集合更快
2）foreach实际上是调用了iterator迭代器来遍历，当遍历链表类集合，一定不要用for来遍历，效率特别低
3) 参考[for效率](https://www.cnblogs.com/suneryong/p/6520442.html)
## java中IO跟NIO
[IO操作分为两个阶段](https://blog.csdn.net/pzysoft/article/details/58677185)：
	a、查看数据是否准备就绪 b、进行数据拷贝（内核将数据拷贝到用户线程）
[IO分为5大类](https://www.cnblogs.com/euphie/p/6376508.html)
1）同步阻塞：用户线程查看数据是否准备就绪，没就绪就一直等待（阻塞），当数据准备就绪之后，**用户线程**将内核数据拷贝到用户线程空间中；
2）同步非阻塞：用户线程查看数据是否准备就绪，没就绪返回一个错误标志，然后轮询查看是否就绪（**所以事实上，在非阻塞IO模型中，用户线程需要不断地询问内核数据是否就绪，也就说非阻塞IO不会交出CPU，而会一直占用CPU，非阻塞IO就有一个非常严重的问题，在while循环中需要不断地去询问内核数据是否就绪，这样会导致CPU占用率非常高，因此一般情况下很少使用while循环这种方式来读取数据**），当数据准备就绪之后，**用户线程**将内核数据拷贝到用户线程空间中；
3）多路复用IO：在多路复用IO模型中，会有一个线程不断轮询多个socket的状态，只有真正准备就绪的socket才真正调用IO的读写事件（**如果没有事件，则一直阻塞在那里，因此这种方式会导致用户线程的阻塞**）；在java NIO中通过Selector.select方法判断注册了选择键的通道是否有事件到达；处理流程：用户线程开启通道在选择器上注册选择键，然后用户线程可以去做其他事情，当选择器判断有事件到达时（**如果之前注册的是read选择键，那么这里就是需要读取的数据准备就绪**），**用户线程**将内核数据拷贝到用户线程空间中；
4）信号驱动IO：在信号驱动IO模型中，当用户线程发起一个IO请求时，会给对应的socket注册一个信号函数，然后用户线程可以去做其他事情，当请求资源准备就绪之后，内核线程会返回一个信号给用户线程，然后**用户线程**将内核数据拷贝到用户线程空间中；
5）异步IO：用户线程发一个IO请求之后去做其他事情，**内核线程**接收到IO请求之后等待请求数据准备完毕，然后**内核线程**将数据拷贝到用户线程，之后在返回一个标志给用户线程，自始至终用户线程都没有参与任何IO操作；
**前面四种IO模型实际上都属于同步IO，只有最后一种是真正的异步IO**，因为无论是多路复用IO还是信号驱动模型，IO操作的第2个阶段都会引起用户线程阻塞，也就是内核进行数据拷贝的过程都会让用户线程阻塞。
## java反射的作用与原理
1）作用：在JAVA中，只要给定类的名字，就可以通过反射机制来获取类的所有信息，可以动态的创建对象和编译
2）原理：主要依靠4个类
	1. Class：包装类本身的类
	2. Field：包装类中属性字段的类
	3. Constructor：包装类中构造方法的类
	4. Method：包装类中方法的类
## 泛型常用特点
1）泛型：参数化类型，定义时将类型参数化，根据实际使用具体赋值某个具体类型；
2）泛型检查：在编译之前对所有的泛型进行检测；
3）实现方式类型擦除：在生成class字节码的的信息中是不包含泛型信息的，在使用泛型的时候加的参数类型，在编译时都会去掉，这个过程称之为类型擦除；
4）由于最终JVM中的class是没有泛型信息的，因此多个不同泛型类行其实共享同一个class
1. 泛型类并没有自己独有的Class类对象。比如并不存在List\<String\>.class或是List\<Integer>.class，而只有List.class。

2. 静态变量是被泛型类的所有实例所共享的。对于声明为MyClass\<T>的类，访问其中的静态变量的方法仍然是 MyClass.myStaticVar。不管是通过new MyClass\<String>还是new MyClass\<Integer>创建的对象，都是共享一个静态变量。

3. 泛型的类型参数不能用在Java异常处理的catch语句中。因为异常处理是由JVM在运行时刻来进行的。由于类型信息被擦除，JVM是无法区分两个异常类型MyException\<String>和MyException\<Integer>的。对于JVM来说，它们都是 MyException类型的。也就无法执行与异常对应的catch语句。
## 设计模式
总体来说设计模式分为三大类：

创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

[23中设计模式巧记](https://blog.csdn.net/qq_30325833/article/details/90484981)
1. 工厂方法模式：[java之工厂模式](https://blog.csdn.net/qq_30325833/article/details/90240613)
2. 单例模式：[java之五种单例模式](https://blog.csdn.net/qq_30325833/article/details/90238018)
3. 建造者模式：[java之建造者模式](https://blog.csdn.net/qq_30325833/article/details/90260184)
4. 原型模式：[java之原型模式](https://blog.csdn.net/qq_30325833/article/details/90262923)
5. 适配器模式：[java之适配器模式](https://blog.csdn.net/qq_30325833/article/details/90270701)
6. 装饰器模式：[java之装饰器模式](https://blog.csdn.net/qq_30325833/article/details/90288832)
7. 代理模式：[java之代理模式](https://blog.csdn.net/qq_30325833/article/details/90312951)
8. 外观模式：[java之外观模式](https://blog.csdn.net/qq_30325833/article/details/90314755)
9. 桥接模式：[java之桥接模式](https://blog.csdn.net/qq_30325833/article/details/90315405)
10. 组合模式：[java之组合模式](https://blog.csdn.net/qq_30325833/article/details/90316422) 
11. 享元模式：[java之享元模式](https://blog.csdn.net/qq_30325833/article/details/90370538)
12. 策略模式：[java之策略模式](https://blog.csdn.net/qq_30325833/article/details/90372759)
13. 模板方法模式：[java之模板方法模式](https://blog.csdn.net/qq_30325833/article/details/90400791)
14. 观察者模式：[java之观察者模式](https://blog.csdn.net/qq_30325833/article/details/90401831)
15. 迭代子模式：[java之迭代子模式](https://blog.csdn.net/qq_30325833/article/details/90403802)
16. 责任链模式：[java之责任链模式](https://blog.csdn.net/qq_30325833/article/details/90408648)
17. 命令模式：[java之命令模式](https://blog.csdn.net/qq_30325833/article/details/90438427)
18. 备忘录模式：[java之备忘录模式](https://blog.csdn.net/qq_30325833/article/details/90440102)
19. 状态模式：[java之状态模式](https://blog.csdn.net/qq_30325833/article/details/90449277)
20. 访问者模式：[java之访问者模式](https://blog.csdn.net/qq_30325833/article/details/90477669)
21. 中介者模式：[java之中介者模式](https://blog.csdn.net/qq_30325833/article/details/90478787)
22. 解释器模式：[java之解释器模式](http://www.cnblogs.com/java-my-life/archive/2012/06/19/2552617.html)
## volatile跟synchronized的区别
1. 关键字volatile是线程同步的轻量级实现，性能比synchronized要好，并且volatile只能修于变量，而synchronized可以修饰方法，代码块等。  
2. 多线程访问volatile不会发生阻塞，而synchronized会发生阻塞。  
3. volatile可以保证数据的可见性，但不可以保证原子性，而synchronized可以保证原子性，也可以间接保证可见性，因为他会将私有内存和公共内存中的数据做同步。  
4. volatile解决 的是变量在多个线程之间的可见性，而synchronized解决的是多个线程之间访问资源的同步性。
## 如何监控和诊断JVM堆内和堆外内存使用
1. 使用JConsole，jstat,jstack等等；

## synchronized修饰类方法和普通方法的锁区别,获取类锁之后还能获取对象锁么
synchronized是对类的当前实例进行加锁，防止其他线程同时访问该类的该实例的所有synchronized块，注意这里是“类的当前实例”，类的两个不同实例就没有这种约束了。

那么static synchronized恰好就是要控制类的所有实例的访问了，static synchronized是限制线程同时访问jvm中该类的所有实例同时访问对应的代码块。

实际上，如果在类中某方法或某代码块中有 synchronized，那么在生成一个该类实例后，该类也就有一个监视块，监视线程并发访问改实例synchronized保护块。而static synchronized则是该类的所有实例公用一个监视块，这便是两个的区别了。

也就是synchronized相当于 this.synchronized，而static synchronized相当于Something.synchronized.

结论：
A: synchronized static是某个类的范围，synchronized static cSync{}防止多个线程同时访问这个类中的synchronized static 方法。它可以对类的所有对象实例起作用。
B: synchronized 是某实例的范围，synchronized isSync(){}防止多个线程同时访问这个实例中的synchronized 方法。

## 如何确保密钥安全，是否非对称加密？
https在数据传输还是对称加密，但是在传输密钥的时候用的是非对称加密以及证书机制
参考：[非对称加密与HTTPS](https://zhuanlan.zhihu.com/p/37738632)

## 为何没有像Iterator.add()这样的方法，向集合中添加元素
iterator需要保证遍历集合的大小才能进行遍历，而且如果是set这种无序列表，add元素没有意义，所以对于list有单独的listIterator来使用，其中就有add方法；

## 为何迭代器没有一个方法可以直接获取下一个元素，而不需要移动游标
他可以在迭代器的最顶层接口实现，但是实际上用的并不多，如果有这么一个接口的话，那么子类都得实现，这没有意义；

## Map接口提供了哪些不同的集合视图？

（1）Set keyset()：返回map中包含的所有key的一个Set视图。集合是受map支持的，map的变化会在集合中反映出来，反之亦然。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为未定义。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。

（2）Collection values()：返回一个map中包含的所有value的一个Collection视图。这个collection受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个collection时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为未定义。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。

（3）Set<Map.Entry<K,V>> entrySet()：返回一个map钟包含的所有映射的一个集合视图。这个集合受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作，以及对迭代器返回的entry进行setValue外），迭代器的结果会变为未定义。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。

## java类加载
参考：https://blog.csdn.net/boyupeng/article/details/47951037

## 串行(serial)收集器和吞吐量(throughput)收集器
参考：https://www.nowcoder.com/questionTerminal/46b6030921164ab0a3cb3dbd6d64f01a?orderByHotValue=1&pos=384
### Serial 收集器：
Serial 收集器是历史悠久，最基本的收集器。它是一个单线程的收集器（说明：这里的单线程不仅仅是指收集器工作时使用一个CPU或者一条收集线程去收集，并且Serial工作时，必须暂停其他所有的工作线程，也就是“stop the world”，直到垃圾收集完成。）Serial是JVM运行在Client模式默认的新生代收集器
### throughput收集器
也叫做Parallel Scavenge 收集器，它的目标是达到一个可控制的吞吐量(throughput)，（说明：吞吐量就是CPU用于执行代码的时间和CPU总共消耗时间的比值，即：吞吐量 = 运行代码时间 / (运行代码时间 + 垃圾收集器工作时间)），JVM提供了两个参数以精确的控制吞吐量，-XX:MaxGCPauseMillis 最大收集停顿时间；-XX:GCTimeRatio 垃圾收集时间占总时间的比例。

吞吐量就是CPU用于运行用户代码的时间与CPU总消耗时间的比值，即吞吐量 = 运行用户代码时间 /（运行用户代码时间 + 垃圾收集时间）。
虚拟机总共运行了100分钟，其中垃圾收集花掉1分钟，那吞吐量就是99%。

## Mysql索引原理
参考1：https://blog.csdn.net/waeceo/article/details/78702584
参考2：[深入理解Mysql](https://blog.csdn.net/tongdanping/article/details/79878302#%E4%B8%89%E3%80%81%E7%B4%A2%E5%BC%95%E7%9A%84%E5%88%86%E7%B1%BB)

## java中编译期常量
参考：[编译期常量](https://blog.csdn.net/sha18751330807/article/details/85345446)
指的是java中8种基本类型变量跟直接赋值的String类型变量在经过final修饰之后即为编译期常量，一般会直接`public static final`（静态常量）来声明；

这种常量在字节码准备的过程中会直接赋值，而不是等到初始化才赋值；

因此，也就必须指出编译期常量的使用风险：A类定义了一个编译期常量，B类中使用了这个常量，两者都进行了编译。然后修改了A中常量的值，重新进行编译时，系统只会重新编译改动的A类，而旧代码B没有重新编译。导致了B中常量值与A中常量值的不一致。

对应到实际业务中，可能是我们的程序中使用了一个第三方库中公有的编译期常量时，如果对方更新了该常量的值，而我们随后也只更新依赖的jar包，那么我们的程序中该常量就是老值，就会产生隐患。为了避免这种情况，在更新依赖的jar文件时，应该重新编译我们的程序。
## java虚拟机中栈帧结构
参考：[栈帧结构](https://blog.csdn.net/quinnnorris/article/details/76467948)
### 局部变量表
一般包括方法的参数列表，方法内部的变量，非static方法的this指针，trycatch的异常变量等等；
### 操作数栈
一个后进先出的栈结构，用来处理变量之间的运算
### 动态链接
栈帧内部包含一个当前所在类型的运行时常量池的引用，以便对当前方法实现动态链接；在一个方法中如果想要调用另外一个方法或者另外一个类的变量需要通过符号引用来表示，而动态链接就是在运行的时候将符号引用解析成直接引用
### 返回地址
方法执行完有两种[方式返回](https://www.jianshu.com/p/c1d4469887dd)：1.正常返回 2. 异常退出
不管哪种方式，最终方法都是需要返回到被调用的位置，程序才能正常执行，方法退出，实际上就是当前栈帧出栈，恢复上层方法的局部变量表跟操作数栈，将返回值（如果有）压入调用者栈帧的操作数栈中，然后调整pc计数器，以指向方法调用的后一条指令；

## 为什么 Java 中的 String 是不可变的（Immutable）
需要从三个角度考虑：1. 设计 2. 效率 3. 安全性 可参考 ：[为什么string不可变](https://blog.csdn.net/zongrongna/article/details/51578920) 
### 设计方面
常量池是方法区中一块特殊的区域，当创建一个字符串时，会判断该字符串是否在常量池中存在，存在的话直接返回该字符串引用，如果一个字符串允许改变，那么其他引用改字符串的对象都会出错；
### 效率
字符串的hashcode在集合中频繁的被使用到
字符串的不变形保证了hash一致性，因此可以放心的缓存，也不用每次都计算新的hashcode；
### 安全性
许多String字符串都是作为一些常用类的参数，例如网络连接url，文件路径或者反射全类名等等，这些字符串都要求不可变，如果能够变则会出现问题；
### 为什么是不可变的
String的源码中String是有个私有的不可变对象value数组，char类型数组保存整个字符串的，由于该对象是私有的，并且value并没有相应的get跟set方法，因此可以保证value的不变，但是value在用反射的情况下还是可以直接改变引用的数据，只是通常不会这么做；
## String ,StringBudder,StringBuffer的区别
总性能上来考虑StringBudder>StringBuffer>String
### String
字符串常量类，封装的对象都存在常量池并且String是不可变对象，因此如果要进行String的连接操作实际上是新建StringBudder对象来append然后在转换成String，这样不断新建对象然后频繁gc因此拉慢效率
### StringBudder
线程不安全的字符串变量类，由于没有加锁操作，因此执行效率最高
### StringBuffer
线程安全的字符串变量类，由于有加锁操作，因此执行效率比StringBudder慢；
## 快速失败(fail-fast)和安全失败(fail-safe)的区别
### fail-fast
快速失败：是用迭代器遍历时直接对原集合进行操作，每次遍历的时候都会将modcount跟expectedCount比较，如果其他地方改变了集合内容，modcount会改变，导致跟expectedCount比较不一致就会报出ConcurrentModificationException；但是如果是ABA现象则这里也是不能发现的；
java.util下的集合类都是快速失败的
### fail-safe
安全失败：遍历的时候会将原集合拷贝一份然后进行相关操作，因此不会触发ConcurrentModificationException异常，但是由于在遍历的时候做了拷贝，因此如果有其他线程在拷贝之后做了修改，当前线程是不知道了，操作的还是原来的数据；
java.util.concurrent包下的集合类都是安全失败的；

## 怎么用java程序判断jvm是32位还是64位？
使用：System.getProperty("sun.arch.data.model")；
## jit指的什么
jit（just in time compile）：即时编译，动态编译，将经常知心的java字节码转换成本地代码，例如主要的热点代码会被转换成本地代码，这样会大幅提高java应用性能；
## 怎么获取 Java 程序使用的内存
可以使用RunTime的freeMemory，totalMemory，maxMemory等方法查看，也可以是使用jstat -gc等查看；
## java中为什么不允许多继承
使用多继承，java中就不知道真正的成员变量或者方法是属于哪个父类，导致整个结构混乱；
在Java中：接口+组合=多继承；
## java中继承内存分配
参考：https://blog.csdn.net/qq_34188112/article/details/54969801

实际上是构造子类对象前，调用父类构造器在子类空间中专门分配一块区域用来保存父类数据，但是并没有实例化父类对象，实例化父类对象需要三步：

1. 分配堆空间，2. 初始化 3. 使变量赋值父类对象引用

一般只有new关键字或者反射等才会实例化对象，所以继承实际上是在子类对象内存空间中有一块父类内存数据空间，使用子类变量或者方法时按照“从下往上”的方式寻找，子类找不到的方法变量往父类空间中寻找，而且实际上父类所有的数据都会被继承到子类中父空间中，根据权限修饰符判断是否对子类可见；

所以如果是父类对象指向子类对象，实际上堆空间还是子类对象，也没有新建父类对象，调用父类变量也是使用继承初始化的父类数据，即使子类有相同的变量，也不会使用子类的，即变量没有重写之说；但是方法不一样，方法会从下往上的方式寻找，子类中如果有相同名称参数列表的方法，则会调用子类中的方法，这个属于方法的重写；

## 向上转型跟向下转型
参考：https://www.cnblogs.com/buptldf/p/4959480.html

向下转型需要考虑安全性，如果父类引用的对象是父类本身，那么在向下转型的过程中是不安全的，编译不会出错，但是运行时会出现java.lang.ClassCastException错误。它可以使用instanceof来避免出错此类错误即能否向下转型，**只有先经过向上转型的对象才能继续向下转型**。

## b = a + b 跟 b += a有什么区别
1. 首先要明白的一点是，在java中，byte、short、char类型的变量在做运算时会自动向上转型为int，因此如下运算编译报错：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618093930326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMwMzI1ODMz,size_16,color_FFFFFF,t_70)
2. 上图中a跟b都是byte类型，做+运算时，a跟b自动转成int类型相加然后赋值给byte类型的b导致类型转换错误；
3. 但是如果是用 += 则不会报错：
```
	public static void main(String[] args) {
		byte a = 126;
		
		byte b = 1;
		b += a;
		System.out.println(b);
	}
```
4. 原因是 +=是一个运算符不是两个运算符，在做运算的时候会自动转换类型，实际上是先转成int相加然后 在重新转成byte赋值；

## hashmap原理，扩容机制跟为什么扩容是2次幂
原理图示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190618142746394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMwMzI1ODMz,size_16,color_FFFFFF,t_70)
扩容机制：如果桶满了（容量 * 加载因子），就需要resize。
扩容两倍（2次幂）的好处：
可参考：https://www.cnblogs.com/kaleidoscope/p/9794775.html
好处1：
```
static int indexFor(int h, int length) {
    // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
    return h & (length-1);
}
```
如上所示，indexFor方法是用来返回元素在数组中的位置的，最终返回到数组的位置是用key的hashcode跟（长度-1）做与（&）运算，假设长度是15，不为2次幂，则15-1=14的二进制表示是1110，最后一位是0，那么这样做与运算，不管hashcode是什么数，与运算的最后一位都是0，因此最后一位为1的位置都不会被占用，因此位置利用率降低，分布不均匀，并且会增大hash冲突；
好处2：
由于hashcode是跟（长度-1）与运算，因此，如果长度翻倍，对于长度的二进制数来说也就是高位增加1来做与运算，对于原位置上的元素来说，如果原来高位是0，则该元素位置保持不变。若为1，则位置变成原位置+原容量大小（例如原位置为5，容量16，那么经过这次之后变成5+16=21）；
## io流相关
相关面试题参考：https://blog.csdn.net/weixin_41768263/article/details/80453196

## concurrentHashMap在1.7跟1.8的区别
## CAP理论
参考：https://blog.csdn.net/dongying1751/article/details/77142314
1. C:Consistency（强一致性）

2. A:Availability（可用性）

3. P:Partition tolerance（分区容错性）

## 有了二叉树跟平衡树为什么还要红黑树
参考：https://blog.csdn.net/u013486414/article/details/92759838

## 几种常见树结构了解
参考：https://blog.csdn.net/wanderlustLee/article/details/81297253

## poll() 方法和 remove() 方法的区别
poll在取元素失败的时候会返回null但是remove会返回异常；
## Java 中 LinkedHashMap 和 PriorityQueue 的区别是什么
priorityQueue保证最高优先级的元素排在队首，但是linkedHashMap保证元素插入顺序，遍历时，priorityQueue是无序的，但是linkedHashMap是元素插入顺序；

## hibernate中的1级和2级缓存的使用方式以及区别原理
1. 一级缓存是session级别缓存，二级缓存是sessionFactory级别缓存；

## 反射的原理，反射创建类实例的三种方式是什么
原理：根据类在加载时对应的class获取类相关的所有信息；
反射创建实例的三种方式：
1. 通过class.newInstance()获取
2. 通过class.getConstructor().newInstance()获取
## 反射class获取的三种方式
1. 类名打点调用：String.class
2. 实例变量利用getClass：a.getClass();
3. 通过Class.forName()获取
## 反射中，Class.forName 和 ClassLoader 区别
参考：https://my.oschina.net/gpzhang/blog/486743
class.forName：会根据类名获取实例并初始化；
classLoader.loacClass：只会加载类到内存但是不会初始化；

## jvm中gc以及参数
参考：https://wangkang007.gitbooks.io/jvm/content/jvmcan_shu_xiang_jie.html
| 选项 | 参数详解 | 默认值 |
|--|--|--|
| -Xms  | 初始堆大小 | - - |
| -Xmx | 最大堆大小 | - - |
| -Xss | 每个线程的堆栈大小| - - |
| -Xmn | 年轻代大小，整个jvm内存大小=年轻代大小+老年代大小+永久代大小，持久代一般固定大小为64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun官方推荐配置为整个堆的3/8，-Xmn:对-XX:newSize、-XX:MaxnewSize两个参数同时进行配置（注意：JDK1.4之后才有该参数）| - - |
| -XX:newSize | 表示新生代初始内存的大小，应该小于 -Xms的值 | - - |
| -XX:MaxNewSize | 新生代最大内存大小，该值需要小于-Xmx的值| - - |
| -XX:NewRatio | 设置为3则表示新生代跟老年代的比为1:3 | - - |
| -XX:PermSize | 永久代的初始大小 | - - |
| -XX:MaxPermSize | 永久代的最大大小 | - - |
| -Xss | 每个线程的堆栈大小 | - - |
| -XX:MaxTenuringThreshold | 每个垃圾对象存活时间，如果设置为0的话，则新生代对象不经过Survivor区，直接进入老年代。对于老年代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则新生代对象会在Survivor区进行多次复制，这样可以增加对象在新生代的存活时间，增加在新生代即被回收的概论 | - - |
| -XX:PretenureSizeThreshold | 大于这个设置值的对象直接在老年代分配 | 3m |

## Minor GC与Full GC分别在什么时候发生
如果 Eden 空间占满了, 会触发 minor GC。 Minor GC 后仍然存活的对 象会被复制到 S0 中去。这样 Eden 就被清空可以分配给新的对象。 又触发了一次 Minor GC , S0 和 Eden 中存活的对象被复制到 S1 中, 并且 S0 和 Eden 被清空。 在同一时刻, 只有 Eden 和一个 Survivor Space 同时被操作。 当每次对象从 Eden 复制到 Survivor Space 或者从 Survivor Space 中的一个复制 到另外一个,有一个计数器会自动增加值。 默认情况下如果复制发生超过 16次, JVM 会停止复制并把他们移到老年代中去。 如果一个对象不能在 Eden 中被创建,它会直接被创建在老年代中。 如果 老年代的空间被占满会触发老年代的 GC,也被称为 Full GC。Full GC 是一个 压缩处理过程,所以它比 Minor GC 要慢很多

内存分配规则: 
 
1. 对象优先分配在 Eden 区,如果 Eden 区没有足够的空间时,虚拟机执行一次 Minor GC。
2. 大对象直接进入老年代(大对象是指需要大量连续内存空间的对象)。这样做的目的是 避免在 Eden 区和两个 Survivor 区之间发生大量的内存拷贝(新生代采用复制算法收集内存)。
3. 长期存活的对象进入老年代。虚拟机为每个对象定义了一个年龄计数器,如果对象经过 了 1 次 Minor GC 那么对象会进入 Survivor 区,之后每经过一次 Minor GC 那么对象 的年龄加 1,直到达到阀值,对象进入老年区。 
4. 动态判断对象的年龄。如果 Survivor 区中相同年龄的所有对象大小的总和大于 Survivor 空间的一半,年龄大于或等于该年龄的对象可以直接进入老年代。 
5. 空间分配担保。每次进行 Minor GC 时,JVM 会计算 Survivor 区移至老年区的对象的 平均大小,如果这个值大于老年区的剩余值大小则进行一次 Full GC,如果小于检查 HandlePromotionFailure 设置,如果 true 则只进行 Monitor GC,如果 false 则进 行 Full GC。

## 怎么优化复制算法
优化复制算法:由于每次执行复制算法的时候,所有存活的对象都要被复 制,这样效率很低。由于程序中创建的大部分对象的生命周期都很短,只有一 部分对象有较长的生命周期,因此可以针对这个特点对复制算法进行优化,采 用分代垃圾回收算法。

## 什么是内存泄露和内存溢出
### 内存泄露 memory leak
是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光。

memory leak会最终会导致out of memory！

内存泄露的解决方案(重要!!): 
1、避免在循环中创建对象。 
2、尽早释放无用对象的引用。(最基本的建议) 
3、尽量少用静态变量,因为静态变量存放在永久代(方法区),永久代基本不参与垃圾回收。 
4、使用字符串处理,避免使用 String,应大量使用 StringBuffer,每一个 String对象都得独立占用内存一块区域。

在实际场景中,你怎么查找内存泄露?
可以使用 Jconsole。
### 内存溢出  out of memory
是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个integer,但给它存了long才能存下的数，那就是内存溢出。

1. 虚拟机栈和本地方法栈溢出 
     如果线程请求的栈深度大于虚拟机所允许的最大深度,将抛出StackOverflowError 异常。 
     如果虚拟机在扩展栈时无法申请到足够的内存空间,则抛出OutOfMemoryError 异常。 
2. 堆 溢出 
     一般的异常信息:java.lang.OutOfMemoryError:Java heap spaces。 
      出现这种异常,一般手段是先通过内存映像分析工具(如 Eclipse Memory Analyzer)对 dump 出来的堆转存快照进行分析,重点是确认内存中的对象是否 是必要的,先分清是因为内存泄漏(Memory Leak)还是内存溢出(MemoryOverflow)。 
     如果是内存泄漏,可进一步通过工具查看泄漏对象到 GC Roots 的引用链。 于是就能找到泄漏对象是通过怎样的路径与 GC Roots 相关联并导致垃圾收集器 无法自动回收。
3. 方法区溢出 跟运行时常量池溢出 
     异常信息:java.lang.OutOfMemoryError:PermGen space。 

导致内存溢出的原因: 

1. 内存中加载的数据量过于庞大,如一次从数据库取出过多数据; 
2. 集合类中有对对象的引用,使用完后未清空,使得 JVM 不能回收; 
3. 代码中存在死循环或循环产生过多重复的对象实体; 
4. 启动参数内存值设定的过小。

2，3两点可以说是存在内存泄漏

内存溢出的解决方法: 

1. 修改 JVM 启动参数,直接增加内存。(-Xms,-Xmx 参数一定不要忘记加。 
一般要将-Xms 和-Xmx 选项设置为相同,以避免在每次 GC 后调整堆的大小;建议堆的最大值设置为可用内存的最大值的 80%)。 
2. 检查错误日志,查看“OutOfMemory”错误前是否有其它异常或错误。 
3. 对代码进行走查和分析,找出可能发生内存溢出的位置。 
4. 使用内存查看工具动态查看内存使用情况(Jconsole)。

基本上如果抛出 OutOfMemory 有两种原因: 
5. 内存泄露。 
6. 应用程序本身就是需要这么多的内存。 

## 如何减少GC出现的次数(Java内存管理)。(重点!!!!)
1. 增大-Xmx的值
2. 对象不用时显示的置为null
3. 尽量使用int等基本类型
4. 尽量少用System.gc，会加重gc次数；
5. 尽量少使用静态变量
6. 尽量使用stringBuffer代替String
7. 尽量少使用finalize函数
8. 尽量分散对象创建跟销毁时间

## 数组多大放在JVM老年代?永久代对象如何GC?如果想不被GC怎么办?如果想在 GC 中生存 1 次怎么办?
1. -XX:PretenureSizeThreshold控制对象大于该值直接放进老年代；
2. 永久代只会在满了之后执行full gc
3. 想不被gc可以加上引用或者放入永久代
4. 想生存一次可以重写finalize方法

## JVM 常见的启动参数。
-Xms: 设置堆的最小值。
-Xmx: 设置堆的最大值。
-Xmn: 设置新生代的大小。
-Xss: 设置每个线程的栈大小。
-XX:NewSize: 设置新生代的初始值。
 -XX:MaxNewSize: 设置新生代的最大值。
-XX:PermSize: 设置永久代的初始值。
 -XX:MaxPermSize: 设置永久代的最大值。
 -XX:SurvivorRatio: 年轻代中 Eden 区与 Survivor 区的大小比值。
 -XX:PretenureSizeThreshold: 令大于这个设置值的对象直接在老年代分配。

## 说下几种常用的内存调试工具: jps、 jmap、 jhat、 jstack、 jconsole,jstat。
Java 内存泄露的问题调查定位: jmap,jstack 的使用等等。
1.  jps: 查看虚拟机进程的状况,如进程 ID。
2.  jmap: 用于生成堆转储快照文件(某一时刻的)。
3. jhat: 对生成的堆转储快照文件进行分析。
4.  jstack: 用来生成线程快照(某一时刻的)。生成线程快照的主要目的是定位线程长时停顿的原因(如死锁,死循环,等待 I/O 等), 通过查看各个线程的调用堆栈,就可以知道没有响应的线程在后台做了什么或者等待什么资源。 
5.  jstat: 虚拟机统计信息监视工具。如显示垃圾收集的情况,内存使用的情况。
6. Jconsole: 主要是内存监控和线程监控。内存监控:可以显示内存的使用情况。线程监控:遇到线程停顿时,可以使用这个功能。
## 什么是线程局部变量
1. 线程局部变量是线程私有的变量，通常使用threadlocal来支持，是线程安全的一种实现方式；

## 消费者与生产者代码
```
public class Producer implements Runnable {

	private Vector<Integer> sharedQueue ;
	
	private int SIZE ;
	
	public Producer(Vector<Integer> sharedQueue,int size) {
		this.sharedQueue = sharedQueue;
		this.SIZE = size;
	}
	
	public void run() {
		for(int i =0;i<7;i++){
			System.out.println("Producted: "+i);
			try {
				produce(i);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}

	private void produce(int i) throws InterruptedException {
		while(sharedQueue.size() == SIZE){
			synchronized(sharedQueue){
				System.out.println("Queue is full " + Thread.currentThread().getName()  
                        + " is waiting , size: " + sharedQueue.size());
				sharedQueue.wait();
			}
		}
		
		synchronized(sharedQueue){
			sharedQueue.add(i);
			sharedQueue.notifyAll();
		}
	}

}
```
```
public class Consumer implements Runnable {

	private Vector<Integer> sharedQueue ;
	
	private int SIZE ;
	
	public Consumer(Vector<Integer> sharedQueue,int size) {
		this.sharedQueue = sharedQueue;
		this.SIZE = size;
	}
	
	@Override
	public void run() {
		while(true){
			try {
				System.out.println("Consumer: "+consume());
				Thread.sleep(50);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}

	private int consume() throws InterruptedException {
		
		while(sharedQueue.isEmpty()){
			synchronized (sharedQueue) {
				System.out.println("Queue is empty " + Thread.currentThread().getName()  
                        + " is waiting , size: " + sharedQueue.size());
				sharedQueue.wait();
			}
		}
		
		synchronized (sharedQueue) {
			sharedQueue.notifyAll();
			return sharedQueue.remove(0);
		}
	}

}
```

```
public class ProducerConsumerSolution {

	public static void main(String[] args) {
		Vector sharedQueue = new Vector();  
        int size = 4;  
        Thread prodThread = new Thread(new Producer(sharedQueue, size), "Producer");  
        Thread consThread = new Thread(new Consumer(sharedQueue, size), "Consumer");  
        prodThread.start();  
        consThread.start();
	}
}
```

## 判断是否含有某个键 
在 HashMap 中,null 可以作为键,这样的键只有一个;可以有一个或多个键所对 应的值为 null。当 get()方法返回 null 值时,既可以表示 HashMap 中没有该键,也可 以表示该键所对应的值为 null。因此,在 HashMap 中不能用 get()方法来判断 HashM ap 中是否存在某个键,而应该用 containsKey()方法来判断。Hashtable 的键值都不能 为 null,所以可以用 get()方法来判断是否含有某个键


## Integer 的缓存策略。
在类加载时就将-128 到 127 的 Integer 对象创建了,并保存在 cache 数 组中(Integer cache[]),一旦程序调用 Integer.valueOf( i ) 方法,如果 i 的 值是在 -128 到 127 之间就直接在 cache 缓存数组中去取 Integer 对象,不 在的话,就创建新的包装类对象

Short(-128 — 127 缓存) 
Long(-128 — 127 缓存) 
Float(没有缓存) 
Doulbe(没有缓存)

## 运算符的优先级
`(++,--)> (*,/,%) > (+,-) > (<<,>>) > (&) > ( | ) >&& > ||`
```
		byte a = 5;
		int b = 5;
		int c = a >> 2 + b ;
		
		System.out.println(c);//结果为0，先执行相加再位运算
```

## java重载与重写
参考：https://blog.csdn.net/qq_30325833/article/details/90265970

## 内部类
静态内部类和非静态内部类主要的不同: 
1. 静态内部类不依赖于外部类实例而被实例化,而非静态内部类需要在 外部类实例化后才可以被实例化。 
2. 静态内部类不需要持有外部类的引用。但非静态内部类需要持有对外 部类的引用。 
3. 静态内部类不能访问外部类的非静态成员和非静态方法。它只能访问 外部类的静态成员和静态方法。非静态内部类能够访问外部类的静态和非静态 成员和方法。 

扩展:内部类都有哪些? 
有四种:静态内部类,非静态内部类,局部内部类,匿名内部类。 
     1.静态内部类和 2.非静态内部类的讲解见上面的部分。 
     3.局部内部类:在外部类的方法中定义的类。其作用的范围是所在的方法 
内。它不能被 public,private,protected 来修饰。它只能访问方法中定义为 final 类型的局部变量
```
class outerClass{
    public void f(){
        class innerClass{//局部内部类
        }
    }
}
```
需要注意的是: 
     4.1 匿名内部类一定是在 new 的后面,这个匿名内部类必须继承一个父类 或者实现一个接口。 
     4.2 匿名内部类不能有构造函数。 
     4.3 只能创建匿名内部类的一个实例。 
     4.4 在 Java 8 之前,如果匿名内部类需要访问外部类的局部变量,则必须 使用 final 来修饰外部类的局部变量。在现在的 Java 8 已经去取消了这个限制。 

