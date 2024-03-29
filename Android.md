## 简历问题回顾

#### 一.launcher相关：

1. back虚拟键消失问题：

   bug本来就是移植时引入的，重点是如何定位这个消失的过程

   Android Studio debug

   condition:   view.getId() == 整数串

   满足这个条件，打印调用栈  

   

#### 可扩展的问题：

1. p Launcher备份失败：

    private static boolean backupVaildDb(String name){
        return name.endsWith(".db") || name.endsWith(".db-shm") || name.endsWith(".db-wal");
    }
   .db-shm和.db-wal作用：

https://stackoverflow.com/questions/13441906/how-handle-backuphelper-and-sqlites-additional-files

wal不是传统的日志文件。 相反，数据库的一些提交会附加到wal文件

## 计算机网络相关

###  1. 浏览器键入地址, 回车后发生了什么?

1. DNS解析

2. TCP连接

3. 发送HTTP请求

4. 服务器请求并返回HTTP报文

5. 浏览器解析渲染页面

6. 连接结束



### 2. get 和post方法的区别

https://baijiahao.baidu.com/s?id=1626599028653203490&wfr=spider&for=pc

![img](images/E190E1331F1C55C8465D6CDA0100E0B3.png)

 GET 用于获取信息，是无副作用的，是幂等的，且可缓存

 POST 用于修改服务器上的数据，有副作用，非幂等，不可缓存

*幂等*操作的特点是其任意多次执行所产生的影响均与一次执行的影响相同

![img](images/0558C1324952C0CE0AEDF4DF030010B2.png)

REST API 又进一步细化

**CRUD       HTTP**
Create      POST
Read         GET
Update     PUT
Delete      DELETE



### 3. 网页劫持?

  **在具体的做法上，一般分为DNS劫持和HTTP劫持。**

​    **DNS劫持：**

​    一般而言，用户上网的DNS服务器都是运营商分配的，所以，在这个节点上，运营商可以为所欲为。

​    例如，访问http://jiankang.qq.com/index.html，正常DNS应该返回腾讯的ip，而DNS劫持后，会返回一个运营商的中间服务器ip。

​    **HTTP劫持：**

​    在运营商的路由器节点上，设置协议检测，一旦发现是HTTP请求，而且是html类型请求，则拦截处理。

后续做法往往分为2种，

1. 类似DNS劫持返回302让用户浏览器跳转到另外的地址，

2. 在服务器返回的HTML数据中插入js或dom节点（广告）。



### 4. http 各个版本差别

<https://www.cnblogs.com/andashu/p/6441271.html>

![img](images/1090298-20170225110614929-1968350403.png)

1. Persistent Connection（keep-alive连接）
   允许HTTP设备在事务处理结束之后将TCP连接保持在打开的状态，以便未来的HTTP请求重用已有的TCP链接(但是对每个请求仍然要单独发 header)，直到客户端或服务器端决定将其关闭为止。

   https://www.jianshu.com/p/ac0ebb1e7d8f

   - keep-alive 只是一种为了达到复用tcp连接的“协商”行为，双方并没有建立正真的连接会话，服务端也可以不认可，也可以随时（在任何一次请求完成后）关闭掉。

   - WebSocket 不同，它本身就规定了是正真的、双工的长连接，两边都必须要维持住连接的状态

     

![img](images/1090298-20170225110733726-609742802.png)

2. chunked编码传输

   编码将实体分块传送并逐块标明长度,直到长度为0块表示传输结束, 这在实体长度未知时特别有用(比如由数据库动态产生的数据)

3. 字节范围请求   	range头域, 断点续传的基础

4. Pipelining（请求流水线）  不等得到响应就发起下一个请求

![img](images/1090298-20170225110825413-1215050354.png)

关于keep alive

https://www.cnblogs.com/havenshen/p/3850167.html>

HTTP协议的Keep-Alive意图在于连接复用，同一个连接上串行方式传递请求-响应数据
TCP的KeepAlive机制意图在于保活、心跳，检测连接错误

**总之，记住HTTP的Keep-Alive和TCP的KeepAlive不是一回事。**

tcp的keepalive是在ESTABLISH状态的时候，双方如何检测连接的可用行。而http的keep-alive说的是如何避免进行重复的TCP三次握手和四次挥手的环节。

![img](images/170853222245220.png)



## Linux 指令

1. find指令

   ```
   find ~ -iname "target*" 不区分文件名大小写
   find ~ 列出~目录下所有文件
   ```

2. grep

   ```
   支持管道操作符， 还有sed，awk， less， more， wc，join， sort， split， head， top
   
    grep -o 筛选 eg： grep "part\[true\]" xxx.log | grep -o "engine\[[0-9]*\]"
    grep -v 过滤 eg： ps -ef | grep chrome | grep -v "grep"
   ```

3. awk

   ```
   语法： awk [option] 'cmd' file
   
   a. 一次读取一行文本， 按输入分隔符进行切片， 切成多个组成部分
   b. 将切片保存在内建的变量中，$1, $2...($0 表示行的全部)
   c. 支持对单个切片判断， 默认分隔符为空格
   
   eg：
   awk '{print $1, $4}' nestat.txt
   awk '$1=="tcp" && $2==1 {print $0}' netstat.txt  打印出满足第一列tcp且第二列为1的行
   awk '（$1=="tcp" && $2==1）|| NR==1 {print $0}' netstat.txt 保留表头
   netstat | awk '{print $1,$4}'
   awk -F "," 指定分隔符
   ```

![image-20190603065644962](images/image-20190603065644962.png)

4. sed  流编辑器，擅长利用正则对文本行处理

 eg：adb shell dumpsys activity activities | sed -n '/Running activities/,/Run #0/p'

## JAVA基础

### 1.垃圾回收算法有哪几种

(1).标记-清除算法：

标记清除之后会产生大量的不连续的内存碎片

(2).复制算法：

- 内存分成两块
- 每次只使用一块, 要进行回收时, 将需要保留的对象复制到另一块内存上去,当前块整个清除

缺点: 对象存活率高的情况下就要执行较多的复制操作，效率将会变低, 适合新生代

(3).标记-整理算法：在标记完成之后不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，在移动过程中清理掉可回收的对象，这个过程叫做整理

对象存活率高的情况下使用标记-整理算法效率会大大提高, 适合年老代

(4).分代收集算法： 新生代, 年老代

当新创建对象时一般在新生代中分配内存空间, 当新生代垃圾收集器回收几次之后仍然存活的对象会被移动到年老代内存中



### 2.软引用和弱引用的区别

<https://www.cnblogs.com/dolphin0520/p/3784171.html>

强引用：就是正常的引用，类似于下面： 

-  `Object object = new Object();`,object就是一个强引用，gc是不会清理一个强引用引用的对象的，即使面临内存溢出的情况。

软引用：SoftReference，GC会在**内存不足**的时候清理引用的对象： 这一点可以很好地用来解决OOM的问题，并且这个特性很适合用来实现缓存：比如网页缓存、图片缓存等

- `SoftReference reference = new SoftReference(object); object = null;`

弱引用：GC线程会**直接清理**弱引用对象，不管内存是否够用： 

- `WeakReference reference = new WeakReference(object); object = null;`

虚引用：和弱引用一样，会直接被GC清理，而且通过虚引用的get方法不会得到对象的引用，形同虚设，这里弱引用是可以的：

使用虚引用的目的就是为了得知对象被GC的时机，所以可以利用虚引用来进行销毁前的一些操作，比如说资源释放等。这个虚引用对于对象而言完全是无感知的，有没有完全一样，但是对于虚引用的使用者而言，就像是待观察的对象的把脉线，可以通过它来观察对象是否已经被回收，从而进行相应的处理。

事实上，虚引用有一个很重要的用途就是用来做堆外内存的释放，DirectByteBuffer就是通过虚引用来实现堆外内存的释放的。

### 3.反射

把java类中的各成分映射成一个个java对象， Method， Filed

```
Class rc = Class.forName("");
Robot r = (Robot)rc.newInstance();
Method getHello = rc.getDeclaredMethod("", String.class);
getHello.setAccessible(true);
getHello.invoke(r, "bob")；

Filed name = rc.getDeclaredField("name");
name.setAccessible(true);
name.set(r, "Alice");

```

getDeclaredMethod 所有方法， 不能获取继承和接口的方法

getMethod 只能获取公共方法， 包括继承和接口方法

### 4.类加载

双亲委派机制

1. 自底向上检查类是否已经加载
2. 自顶向下尝试加载类

类加载方式

- 隐式加载： new

- 显式加载： loadClass， forName等   都是先获取类对象，再生成对象实例

  forName 加载，链接，初始化(执行类变量赋值和静态代码块)

  loadClass 不会进行类初始化

### 5.Java内存模型

线程私有： 程序计数器， 虚拟机栈， 本地方法栈

线程共享：MetaSapce， Java堆

## Android相关

### 1.双指缩放如何实现

1. 重写OnTouch方法

   从这个方法中我们就可以获取实现两指缩放功能的全部信息

2. MotionEvent.ACTION_POINTER_UP:   MotionEvent.ACTION_POINTER_DOWN:  

判断点的个数

   3.然后在MotionEvent.ACTION_MOVE事件中，如果大于等于2，就计算两点间的距离，如果距离增大就把字体放大，距离减少就把字体缩小

### 2.binder 通信到底拷贝几次， 为啥使用binder,相较于其它进程间通信方式优点

1.  Client-server的通信方式, 只有socket支持, 其他默认点对点, 但socket通信传输效率低,开销大.
2.  传输性能: 共享内存无需拷贝, binder 数据拷贝一次, 其他都需要拷贝两次(发送方缓冲区->内核缓冲区->接收方缓冲区)
3.  安全性: 可靠的身份标记只有由IPC机制在内核中添加, 传统的IP只能在数据包中传入UID(进程身份,简单理解为PID), 且接入点是开放的.

扩展:

<http://gityuan.com/2015/10/31/binder-prepare/>

<https://blog.csdn.net/happylishang/article/details/62234127>

独特之处: Binder对象是一个可以跨进程引用的对象, 它的实体位于一个进程中, 引用却遍布系统的各个进程

如何做到一次拷贝: 

正常情况下, 需要先用copy_from_user 拷贝到内核空间, 再用copy_to_user 拷贝到另一个用户空间,  为了实现用户空间到用户空间的直接拷贝,  **mmap把Binder在内核空间的数据直接通过指针地址映射到用户空间, 所以调用copy_from_user 将数据拷贝进内核空间也就相当于拷贝进了接收方的用户空间**.

![Binder.jpg](images/1460468-1f61b4f411c35094.jpg)



#### Binder 通信模型:

- Server : 创建binder实体

- Client
- Binder驱动

- ServiceManager: 将字符形式的Binder名字转换成Client对该Binder的引用

```
public static final String VIBRATOR_SERVICE = "vibrator";
```

![binder_overview](images/0_1298798582y7c5.png)



### 3.AsyncTask原理

异步任务类，默认使用SerialExecutor使用，维护了一个静态线程池，默认是顺序执行异步任务，适合短时异步并发的操作，长时的耗时操作不宜放入使用，doInBackground()方法是在子线程中执行，而其他方法是在UI主线程中执行。除此之外，**AsyncTask的实例化以及实例的execute()方法也都需要在UI线程中执行**

### 4.Handler

Looper、MessageQueue、Handler

线程通过Looper建立自己的消息循环，MessageQueue是消息队列，Looper负责从MessageQueue中取出消息，指定分发给目标Handler对象。关键做法：Looper.prepare()、Looper.loop()建立消息循环

postDelayed 实际是先将Runnable包装为message, 然后执行sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);

MessageQueue 其实是Message的管理类, Message是一个链表结构,  MessageQueue 的enqueMessage实际上就是操作链表。

### 5.android事件分发机制

**责任链模式**，事件传递以及事件回传的机制，如果在传递下去的过程中被某个ViewGroup拦截，则走入该ViewGroup的onTouchEvent处理中；如果在下层View的onTouchEvent处理的话就算是被消费，此时就不会再往上继续回传了 

<https://blog.csdn.net/carson_ho/article/details/54136311>



![944365-aea821bbb613c195](images/944365-aea821bbb613c195.png)

### 6.onSaveInstanceState()

【推荐】 Activity#onSaveInstanceState()方法不是 Activity 生命周期方法，也不保证
一定会被调用。它是用来在 Activity 被意外销毁时保存 UI 状态的，只能用于保存临
时性数据，例如 UI 控件的属性等，不能跟数据的持久化存储混为一谈。持久化存储
应该在 Activity#onPause()/onStop()中实行 



## 算法题



### 1. 100万取前一百?



先取出前100个数，维护一个100个数的最小堆，遍历一遍剩余的元素，在此过程中维护堆就可以了。具体步骤如下：

step1：取前m个元素（例如m=100），建立一个小顶堆。保持一个小顶堆得性质的步骤，运行时间为O（lgm);建立一个小顶堆运行时间为m*O（lgm）=O(m lgm);

step2:顺序读取后续元素，直到结束。每次读取一个元素，如果该元素比堆顶元素小，直接丢弃

如果大于堆顶元素，则用该元素替换堆顶元素，然后保持最小堆性质。最坏情况是每次都需要替换掉堆顶的最小元素，因此需要维护堆的代价为(N-m)*O(lgm);

最后这个堆中的元素就是前最大的10W个。时间复杂度为O(N lgm）。



### 2.parseInt 的写法