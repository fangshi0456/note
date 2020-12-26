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






