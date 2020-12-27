# Concurrent



### Synchronized

1. code synchronized
2. Class monitor enter monitor exit
3. Lock upgrade new 偏向锁 自旋锁 重量级锁
4. lock compare and exchange



### Volitale

new Object() 过程  有半初始化状态



汇编码

申请内存 负初始值

invoke special  调用构造方法负值

Astore 建立对象内存与对象关联



双检锁 volitale

使用了半初始化状态的对象



Volitale 实现方法

 ***** jvm内存屏障 

Load load 

store store 

load store 

store load



StoreStoreBarrier

Volatile write

StoreLoadBarrier



LoadloadBarrier 

Volatile read

LoadStoreBarrier



Hotspot 实现 lock addl  锁住总线



### Thread Local



对象被GC后 调用 finalize()

4种引用 强软弱虚

* 强  常住内存 没有引用 被GC

```
M m = new M();
```

* 软  主要用于缓存 在heap装不下时 被GC

```java
SoftReference<byte[]> m = new SoftReference<>(new byte[1024]);
```

   m  ------> 内存中的sr ~~~>  array

* 弱  每次GC都会被GC    

  ThreadLocal add -----> 当前线程 ThreadLocal.ThreadLocalMap 添加记录 key 为  ThreadLocal 对象

  Key value 对  为一个Entry extends WeakReference<TheadLocal<?>>

  解决垃圾回收 避免内存泄露 thread不存在了  threadLocal会被回收， 因为线程内的弱引用

  thread local map 中的key 变为空， 但是因为在map里 不会被回收 需要手动回收

```java
WeakReference<M> m = new WeakReference<>(new M());
```

* 虚  管理堆外内存  NIO ---->   buffer （DirectByteBuffer）指向堆外内存  ---> OS  不需要从JVM 内部 （堆）中拷贝 （0拷贝）

  DirectByteBuffer  被回收   -------->  放入queue中 ------>GC 线程监听queue ------> 回收堆外内存

```java
PhantomReference<M> m = new PhantomReference<>(new M(),QUEUE);
m.get();  拿不到 m  
```





@Transactional  应用





ThreadLocal 实现

没个线程内部有ThreadLocal.ThreadLocalMap 的成员变量

ThreadLocal 

 

