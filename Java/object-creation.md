# Object



### Object Creation



###### 用静态工厂方法代替构造器

1. 静态方法可以有名称，可以对方法行为进行描述。

   ```java
       public static T singletonInstance () {...}
   ```

2. 不必在每次被调用时创建新的对象。重复利用已有对象，减少创建和回收的开销，提高系统性能。

3. 可以返回类型的任何子类对象。

4. 可以通过传入参数来控制返回对象。静态方法内部逻辑，在用户端被屏蔽。

   ```java
   public class Cat extends Animal{
   }
   
   public class Dog extends Animal{
   }
   
   public abstract class Animal {
       public static Animal animal(int score) {
           if (score<50){
               return new Cat();
           }
           return new Dog();
       }
   }
   ```

5. 命名

   5.1 from 

   ```java
   Date d = Date.from(instance);
   ```

   5.2 of 聚合方法，多个参数

   ```java
   Set<Rank> faceCards = EnumSet.of(JACK,QUEEN,KING);
   ```

   5.3 valueOf

   ```java
   BigInterger prime = BigInteger.valueOf(Integer.MAX_VALUE);
   ```

   5.4 instance 可以复用

   ```java
   Animal animal = Animal.instance(SRORE);
   ```

   5.5 create 或 newInstance 每次返回新对象

   5.6 getType  type为需要返回的对象类型

   ```java
   Cat cat = Animal.getCat();
   Dog dog = Animal.getDog();
   ```

   

   5.7 newType 同getType 返回的是新对象

   

###### 在遇到构造器参数过多是使用建造者模式

1. 重叠构造函数难以使用，多个类型相同的连续参数很难保证顺序。

   ```java
   public class Animal {
   
       private final String id;
       private final String name;
   
       private String color;
       private int age;
       private String gender;
       private String owner;
   
       public Animal(String id,String name){
           this.id = id;
           this.name = name;
       }
   
       public Animal(String id,String name,String color){
           this(id,name);
           this.color = color;
       }
   
       public Animal(String id,String name,String color,String gender){
           this(id,name,color);
           this.gender = gender;
       }
   }
   ```

2. JavaBeans模式， 不能保证安全性，一致性。当被创建出来后已经破坏了内部规则。

```java
public class Animal {
  
    // required parameters
    private String id;
    private String name;
    private String color;
    private int age;
    private String gender;
    private String owner;

    public Animal() {
    }

    public void setId(String id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void setOwner(String owner) {
        this.owner = owner;
    }
}
```



3. 建造者模式，不破坏一致性，参数可在Builder中校验不侵入业务，流式API便于阅读，便于。

```java
public class Animal {

    private final String id;
    private final String name;

    private final String color;
    private final int age;
    private final String gender;
    private final String owner;

    private Animal(Builder builder){
        this.id= builder.id;
        this.name= builder.name;
        this.color= builder.color;
        this.age= builder.age;
        this.gender= builder.gender;
        this.owner= builder.owner;
    }

    public static class Builder {
        // required parameters
        private final String id;
        private final String name;

        // optional parameters to initialized to default values
        private String color = "red";
        private int age = 1;
        private String gender = "male";
        private String owner = "Brian";

        Builder(String id, String name) {
            this.id = id;
            this.name = name;
        }

        public Builder color(String color){
            this.color = color;
            return this;
        }

        public Builder age(int age){
            this.age = age;
            return this;
        }

        public Builder gender(String gender){
            this.gender = gender;
            return this;
        }


        public Builder owner(String owner){
            this.owner = owner;
            return this;
        }

        public Animal build(){
            return new Animal(this);
        }
    }
}
```

```
Animal animal = new Animal.Builder("1", "FF").age(3).color("grey").gender("female").owner("Brian").build();
```



###### 私有构造器强化Singleton

###### 私有构造器强化不可实例化的能力 (抽象类不能被实例化)

java.util.collecgtions  (utility class)

```java
public class Utility{
  private Utility(){
    throw new AssertionError();
  }
}
```



###### 避免自动装箱

int Interger 

long Long 

..

在计算总互相转换
