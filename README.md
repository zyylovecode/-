# -
Java 集合对⽐
⼀、 HashMap与HashTable的区别
Hashtable是个过时的集合类，存在于Java API中很久了。在Java 4中被重写了，实现了Map接
⼝，所以⾃此以后也成了Java集合框架中的⼀部分。 Hashtable和HashMap在Java⾯试中相当容
易被问到，甚⾄成为了集合框架⾯试题中最常被考的问题，所以在参加任何Java⾯试之前，都不
要忘了准备这⼀题。
HashMap和Hashtable都实现了Map接⼝，但决定⽤哪⼀个之前先要弄清楚它们之间的分别。主
要的区别有：
HashMap HashTable
⾮线程安全（⾮线程同步） 线程安全（线程同步）
更适合于单线程 更适合于多线程
允许null值 不允许null值
迭代器Iterator是fail-fast迭代器 迭代器enumerator不是fail-fast的
初始容量为16 初始容量为11
两者最主要的区别在于Hashtable是线程安全，⽽HashMap则⾮线程安全
HashMap是⾮synchronized，⽽Hashtable是synchronized，这意味着Hashtable是线程安全的，
多个线程可以共享⼀个Hashtable；⽽如果没有正确的同步的话，多个线程是不能共享HashMap
的
由于Hashtable的实现⽅法⾥⾯都添加了synchronized关键字来确保线程同步，所以在单线程环
境下它⽐HashMap要慢。如果你不需要同步，只需要单⼀线程，那么使⽤HashMap性能要好过Hashtable。仅在你需要完全的线程安全的时候使⽤Hashtable，
⽽如果你使⽤Java 5或以上的话，请使⽤ConcurrentHashMap吧。
线程安全的实现原理： jvm有⼀个main memory，⽽每个线程有⾃⼰的working memory，⼀个线
程对⼀个变量进⾏操作时，都要在⾃⼰的working memory⾥⾯建⽴⼀个copy，操作完之后再写
⼊main memory。多个线程同时操作同⼀个变量，就可能会出现不可预知的结果。⽤
synchronized的关键是建⽴⼀个镜像，这个镜像可以是要修改的变量也可以其他你认为合适的对
象⽐如⽅法和类，然后通过给这个镜像加锁来实现线程安全，每个线程在获得这个锁之后，要执
⾏完才会释放它得到的锁。这样就实现了所谓的线程安全。 sychronized意味着在⼀次仅有⼀个
线程能够更改Hashtable。就是说任何线程要更新Hashtable时要⾸先获得同步锁，其它线程要等
到同步锁被释放之后才能再次获得同步锁更新Hashtable。
我们平时使⽤时若⽆特殊需求建议使⽤HashMap，在多线程环境下若使⽤HashMap需要使⽤
Collections.synchronizedMap()⽅法来获取⼀个线程安全的集合
（Collections.synchronizedMap()实现原理是Collections定义了⼀个SynchronizedMap的内部
类，这个类实现了Map接⼝，在调⽤⽅法时使⽤synchronized来保证线程同步,当然了实际上操作
的还是我们传⼊的HashMap实例，简单的说就是Collections.synchronizedMap()⽅法帮我们在操
作HashMap时⾃动添加了synchronized来实现线程同步，类似的其它
Collections.synchronizedXX⽅法也是类似原理）
HashMap的迭代器Iterator是fail-fast迭代器，⽽Hashtable的enumerator迭代器不是fail-fast
的。
当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出
ConcurrentModificationException，但迭代器本身的remove()⽅法移除元素则不会抛出
ConcurrentModificationException异常。但这并不是⼀个⼀定发⽣的⾏为，要看JVM。这条同样
也是Enumeration和Iterator的区别。
Fail-safe和iterator迭代器相关。如果某个集合对象创建了Iterator或者ListIterator，然后其它的线
程试图“结构上”更改集合对象，将会抛出ConcurrentModificationException异常。但其它线程可
以通过set()⽅法更改集合对象是允许的，因为这并没有从“结构上”更改集合。但是假如已经从结
构上进⾏了更改，再调⽤set()⽅法，将会抛出IllegalArgumentException异常。
结构上的更改指的是删除或者插⼊⼀个元素，这样会影响到map的结构。
HashMap可以使⽤null作为key，⽽Hashtable则不允许null作为key
虽说HashMap⽀持null值作为key，不过建议还是尽量避免这样使⽤，因为⼀旦不⼩⼼使⽤了，
若因此引发⼀些问题，排查起来很是费事。 HashMap以null作为key时，总是存储在table数组的
第⼀个节点上
HashMap是对Map接⼝的实现， HashTable实现了Map接⼝和Dictionary抽象类
HashMap的初始容量为16， Hashtable初始容量为11，两者的填充因⼦默认都是0.75
HashMap扩容时是当前容量翻倍即:capacity ∗ 2， Hashtable扩容时是容量翻倍+1即:两者计算hash的⽅法不同
Hashtable计算hash是直接使⽤key的hashcode对table数组的⻓度直接进⾏取模
int	hash	=	key.hashCode();
int index	=	(hash	&	0x7FFFFFFF)	%	tab.length;
HashMap计算hash对key的hashcode进⾏了⼆次hash，以获得更好的散列值，然后对table数组
⻓度取摸
static final int	hash(Object key)	{
int	h;
return	(key	==	null)	?	0	:	(h	=	key.hashCode())	^	(h	>>>	16);
}
static int	indexFor(int	h,	int	n)	{
return	h	&	(n-1);
