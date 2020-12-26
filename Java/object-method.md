# Methods

### Equals and Hashcode

Default equals 比较的是基础对象值 引用对象地址

Default hashcode native 方法生成



重写equals必须重写hashCode 

```java
public class Cat{
    private final String name;
    private final String color;

    public Cat(String name, String color) {
        this.name = name;
        this.color = color;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Cat cat = (Cat) o;
        return name.equals(cat.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, color);
    }
}
```



```
Cat blackFF = new Cat("ff", "black");
Cat pinkFF = new Cat("ff", "pink");
                      
// it's true
boolean equals = blackFF.equals(pinkFF);

// should be one cat in the set but two
Set<Cat> cats = new HashSet<>();
cats.add(blackFF);
cats.add(pinkFF);
```



### toString()

为所有对象复写toString()，使使用更加舒适。



### to implements Comparable



使用Long.compare(a,b),Integer.compare(a,b). 不使用<> 容易出错



使用comparator 组合比较

```java
private static final Comparator<Cat> comparator = comparing((Cat::getName)).thenComparing(Cat::getColor);
```

