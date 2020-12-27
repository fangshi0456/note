# Class and Interface

### 区分一个组件设计好坏的唯一重要因素在于，它对外部组件而言，是否隐藏了其内部数据和其他实现细节。



把实现和API清晰的隔离开

一个模块不需要知道其他模块的内部工作情况 information hiding or encapsulation



### benefit

* 通过API连接

* 解除服务间的耦合

* 让每个组件可独立的开发、测试、优化、使用、理解和修改

* 因为可以并行开发，加快了系统的开发速度

* 更容易理解，减少了系统的维护负担
* 虽然不会带来更好的性能，但是可以有效的调节性能
  * 快速排查出现问题的组件
  * 进一步优化问题组件并不影响其他组件的正确性
* 因为解耦，增加了可重用性



### 隐藏信息 使类和成员的可访问性最小化

* Access control 
  * public 
  * protected
  * private



### Public Class 不能直接暴露可变成员

```java
public class cat{
  // 作为可变量 不能暴露 防止破坏封装性
  private String color;
  // 做为不可变量 可以直接暴露
  public final String name;
}
```



### 近最大能力让类接近不可变

###### 如何实现

* 不设置set()方法，通过构造函数设值
* 保证该类不会被扩展
  * 私有构造方法，通过静态工厂创建
  * 标记final
* 属性加private final



*** Why ?  

* 不可变对象本质上时线程安全的，他们不需要同步，可以自由的被共享（lock）

* 保证了原子性，不存在临时不一致的情况



*** How ?

* 不要为每一个get 配套set 方法， 除非有很好的理由要求类是可变的

* 如果类不能被做成不可变的，仍然应该尽可能的限制他的可变性

* 每个对象的属性都应该是private final的 除特殊情况

* 构造器应该创建完全初始化的对象，并建立所有约束关系





### 复合（装饰者模式）优先于继承

继承在一定程度上破坏了封装性

子类依赖父类中的特定功能实现细节，当父类发生变化时，子类会被破坏。

子类只能选择随着父类的改变而改变



扩展HashSet 提供计数功能 记录共添加过多少元素

```java
public class InstrumentedHashSet<E>  extends HashSet<E> {

    private int count;

    public InstrumentedHashSet() {
        this.count = 0;
    }

    @Override
    public boolean add(E e) {
        count++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        count += c.size();
        return super.addAll(c);
    }

    public int getCount() {
        return count;
    }
}
```

```java
Dog dog0 = new Dog();
Dog dog1 = new Dog();
Dog dog2 = new Dog();

List<Dog> dogs = Arrays.asList(dog0, dog1, dog2);

InstrumentedHashSet<Dog> dogSet = new InstrumentedHashSet<>();
dogSet.addAll(dogs);
// count 应该为3 但是实际为6 因为HashSet 内部实现中  addAll 会调用 add
dogSet.getCount();

```



*****  如果子类依赖父类实现扩展 要明白父类的运行机制



为了满足功能重写HashSet addAll 方法，避免重复计算

```java
public class InstrumentedHashSet<E>  extends HashSet<E> {

    private int count;

    public InstrumentedHashSet() {
        this.count = 0;
    }

    @Override
    public boolean add(E e) {
        count++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        boolean modified = false;
        for (E e : c)
            if (add(e))
                modified = true;
        return modified;
    }

    public int getCount() {
        return count;
    }
}
```

缺点

* HashSet 的addAll 方法不会再调用到

* 当HashSet addAll 内部发生变化时，子类并不知情，不能达成同步
* 因为不能访问父类的私有域，不是所有情况都可以复写的
* 当HashSet发布新的添加方法时，如果子类不跟随调整，就会破坏封装



***** 装饰者

```java
public class ForwardHashSet<E> implements Set<E> {

    private final Set<E> wrapper;

    public ForwardHashSet(Set<E> wrapper) {
        this.wrapper = wrapper;
    }

    @Override
    public int size() {
        return wrapper.size();
    }

    @Override
    public boolean isEmpty() {
        return wrapper.isEmpty();
    }

    @Override
    public boolean contains(Object o) {
        return wrapper.contains(o);
    }

    @Override
    public Iterator<E> iterator() {
        return wrapper.iterator();
    }

    @Override
    public Object[] toArray() {
        return wrapper.toArray();
    }

    @Override
    public <T> T[] toArray(T[] a) {
        return wrapper.toArray(a);
    }

    @Override
    public boolean add(E e) {
        return wrapper.add(e);
    }

    @Override
    public boolean remove(Object o) {
        return wrapper.remove(o);
    }

    @Override
    public boolean containsAll(Collection<?> c) {
        return wrapper.contains(c);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        return wrapper.addAll(c);
    }

    @Override
    public boolean retainAll(Collection<?> c) {
        return wrapper.retainAll(c);
    }

    @Override
    public boolean removeAll(Collection<?> c) {
        return wrapper.removeAll(c);
    }

    @Override
    public void clear() {
        wrapper.clear();
    }

    @Override
    public Spliterator<E> spliterator() {
        return wrapper.spliterator();
    }
}
```

```java
public class InstrumentedHashSet<E>  extends ForwardHashSet<E> {

    private int count;

    public InstrumentedHashSet(Set<E> wrapper) {
        super(wrapper);
    }

    @Override
    public boolean add(E e) {
        count++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        count+=c.size();
        return super.addAll(c);
    }

    public int getCount() {
        return count;
    }
}
```



总结

当父类和子类确实存在子类型关系时可以使用继承（因为子类和父类的可以共享安全逻辑）

当类没有作可继承的设计时，继承他会导致子类的脆弱性，应使用复合代理继承。





### 要么设计继承并提供文档说明，要么禁止继承

***** 好的API文档应该描述一个给定方法做了什么工作，而不是描述他是如何做到的

要标识方法是可继承的 还是自用的





### 接口和抽象类有本质上的区别 

抽象类可以有default 的实现，实现代码的复用，是对象

接口用来描述对象的行为

没有银弹，可以混用



接口可以default 做默认实现 不会影响实现类编译





### 静态成员类优于非静态成员类

嵌套类 nested class 是一个类的内部类，存在的目的在于给外围类提供服务，如果嵌套类被用于其他地方，那他应是一个顶层类。

* 静态成员类 static member class
* 非静态成员类 nonstatic member class
* 匿名类 anonymous class
* 局部类 local class

Nested classes are divided into two categories: static and non-static. Nested classes that are declared static are called ***static nested classes***. Non-static nested classes are called ***inner classes***.



