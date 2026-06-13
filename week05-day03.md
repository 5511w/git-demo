#### final关键字
final关键字是最终的意思，可以修饰：类、方法、变量

* 修饰类：该类被称为最终类，特点是不能被继承了。
* 修饰方法：该方法被称为最终类，特点是不能被重写了。
* 修饰变量：该变量有且仅能被赋值一次。

变量哪些呢？

            a.成员变量
                静态变量(static)
                实例变量
            b.局部变量

final修饰静态成员变量，这个变量今后被称为常量，可以记住一个固定值，并且程序中不能修改，通常这个值作为系统的配置信息

常量的名称，建议全部大写，多个单词用下划线链接

```java
public static final String SCHOOL_NAME = "西安西西学院";
```

final修饰实例变量（一般没有意义）
```java
private  final String name = "小西";
```

```java
FinalDemo f = new FinalDemo();
System.out.println(f.name);
//f.name = "小花";//第二次赋值，报错
```

fianl修饰基本类型的变量，变量存储的数据不能被改变。

final修饰引用类型的变量，变量存储的地址不能被改变，但地址所指向对象的内容是可以被改变的。

##### 常量
使用了static final 修饰的成员变量就被称为常量

作用：常用于记录系统的配置信息。

```java
public class Constant {
    public static final String SYSTEM_NAME = "系统";
}
```

程序编译后，常量会被”宏替换“：保证使用常量和直接用字面量的性能是一样的。

#### 单例类（设计模式）

什么是设计模式？

一个问题通常有n种解法，其中肯定有一种解法是最优的，这个最优解法被人总结出来了，称之为设计模式

设计模式有20多种，对应20多种软件开发中会遇到的问题

单例设计模式

作用：确保某个类只能创建一个对象

饿汉式单例

写法，实现步骤：

```java
public class A {
    //2.定义一个静态变量，用于基本本类的一个唯一对象
    //public static final A a = new A();
    private static A a = new A();

    //1.私有化构造器，确保单例类对外不能创建太多对象，单例才有可能性
    private A(){
    }

    //3.提供一个公开的静态方法，返回这个类的唯一对象
    public static A getInstance(){
        return a;
    }
}
```

懒汉式单例类

作用：用对象是才开始创建对象

```java
public class B {
    //2.创建一个静态的成员变量
    private static B b;

    //1.私有化构造器
    private B(){

    }
    //3.提供静态方法返回对象  真正需要对象的时候才开始创建对象
    public static B getInstance(){
        if(b == null){
            b = new B();
        }
        return b;
    }
}
```

#### 枚举类
一种特殊类

```java
public enum A {
    //枚举类的第一行，只能罗列枚举对象的名称，这些名称本质是常量，每个常量都记住了一个枚举类的对象
    X, Y, Z;
    ...
}
```

![alt test](/image/屏幕截图%202026-06-13%20144750.png)

枚举类常见应用场景

做信息分支和标志

```java
//第一种是常量做信息标志和分类;但参数值不受约束
move(Constant.UP);
//第二种枚举类做信息标志和分类;参数值受枚举类约束
mov2(Direction.UP);
```

#### 抽象类
Java中的一个关键字：abstract，它就是抽象的意思，可以用它修饰类、成员方法。

abstract修饰类，这个类就是抽象类

abstract修饰方法，这个方法就是抽象方法

```java
public abstract class A {

    //抽象方法：必须用abstract修饰,没有方法体，只有方法声明
    public abstract void show();

}
```

* 抽象类中不一定要有抽象方法，有抽象方法的类必须是抽象类。
* 类有的成员：成员变量、方法、构造器，抽象类都可以有
* 抽象类最主要的特点：抽象类不能创建对象，仅作为一种特殊的父类，让子类继承并实现。！
* 一个类继承抽象类，必须重写完抽象类的全部抽象方法，否则这个类也必须定义成抽象类。
* 抽象类的使命就是被子类继承

使用抽象类的好处

父类知到每个子类都要做某个行为，但每个子类要做的情况不一样，父类就定义成抽象方法，交给子类去重写实现，为了更好的支持多态。

##### 模板方法设计模式
提供一个方法作为完成某类功能的模板，模板方法封装了每个实现步骤，但允许子类提供特定步骤的实现，

写法：

1. 定义一个抽象类
2. 在里面定义两个方法
    一个是模板方法：把共同的实现步骤放里面去

    一个是抽象方法：不确定的实现步骤，交给具体的子类来实现

```java
public abstract class People {
    //1，模板方法设计模式
    public final void write(){
        System.out.println("\t\t\t《我的爸爸》");
        System.out.println("\t我的爸爸是......");
        //2.模板方法知到子类一定要写这个正文，但每个子类写的信息是不同的，父类定义一个抽象方法，
        // 具体的实现交给子类来重写
        writeBody();
        System.out.println("\t我的爸爸喜欢吃......");
    }
    public abstract void writeBody();
}
```

* 建议使用 final 关键字修饰模板方法
* 一旦子类重写模板方法，模板方法就失效了

#### 接口
interface关键字定义出接口

* 注意：接口不能创建对象
* 接口是用来被实现（implements）的，实现接口的类称为实现类，一个类可以同时实现多个接口

JDK8前的传统接口

```java
public interface A {
    //JDK8之前，接口只能定义常量和抽象方法
    //1.常量：接口中定义的变量，通常可以省略public static final不写，默认是加上的
    String name = "xixi";
    //public static final String name = "xixi";

    //2.抽象方法：接口中定义的方法，通常可以省略public abstract不写，默认是加上的
    //public abstract void run();
    void run();
    String show();
}
```
```java
//实现类实现多个接口，必须重写全部接口的抽象方法，否则这个类必须被定义成抽象类
class C implements A,B{

    @Override
    public void run() {
            System.out.println("C类正在跑");
    }

    @Override
    public String show() {
            return "逃跑";
    }

    @Override
    public void play() {
            System.out.println("C类正在玩");
    }
}
```

##### 接口的好处

* 接口弥补了类单继承的不足，可以让类拥有更多角色，类的功能更强大
* 接口可以实现面向接口编程，更利于解耦合

##### JDK8后接口新增的三种方法

```java
public interface A {
    //1.默认方法（普通实例方法），必须加default关键字
    //默认加public修饰
    //如何调用？使用接口的实现类的对象来调用
    default void go() {
        System.out.println("go go go");
        run();
    }

    //2.私有方法（JDK 9 开始支持）
    //私有的实例方法
    //如何调用？使用接口的其他实例方法来调用
    private void run() {
        System.out.println("run run run");
    }

    //3.静态方法
    //默认加public修饰
    //如何调用？只能使用当前接口名来调用
    static void show() {
        System.out.println("show show show");
    }
}
```

##### 接口的几点注意事项

```java
//1.接口与接口可以多继承，一个接口可以同时继承多个接口[重视]
//类与类：单继承，一个类只能继承一个直接父类
//类与接口：多继承，一个类可以同时实现多个接口
//接口与接口，一个接口可以同时继承多个接口
interface A{
        void show();
    }
interface B{
        void show2();
    }
interface C extends A,B{
        void show3();
    }
class D implements C{
    @Override
    public void show() {
    }

    @Override
    public void show2() {
    }

    @Override
    public void show3() {
    }
}
```

```java
//2，一个接口继承多个接口，如果多个接口中存在方法签名冲突，则此时不支持多继承，也不支持多实现[了解]
interface A1{
        void show();
    }
interface B1{
        String show();
    }
//    interface C1 extends A1,B1{
//    }
//    class D1 implements C1{
//    }
```
```java
//3.一个类继承了父类，又同时实现了接口，如果父类中和接口中有同名的方法，实现类会优先用父类的
interface A2{
    default void show(){
        System.out.println("A2 show");
    };
}
class B2{
    public void show(){
        System.out.println("B2 show");
    };
}
class dog extends B2 implements A2{
    public void go(){
        show();//B2 show
        super.show();//B2 show
        A2.super.show();//A2 show
    }
}
```
```java
//4.一个类实现了多个接口，如果多个接口中存在同名的默认方法，可以不冲突，这个类重写该方法即可
interface A3{
    default void show(){
        System.out.println("A3 show");
    }
}
interface B3{
    default void show(){
        System.out.println("B3 show");
    }
}
class C3 implements A3,B3{
    @Override
    public void show(){
        A3.super.show();//A3 show
        B3.super.show();//B3 show
    }
}
```

##### 抽象类和接口的区别

相同点：

     1、都是抽象形式，都可以有抽象方法，都不能创建对象

     2、都是派生子类形式：抽象类是子类继承使用，接口是被实现类实现

     3、一个类继承抽象类，或者实现接口，都必须重写完他们的抽象方法，否则自己要成为抽象类或报错
     
     4、都支持多态，都能够实现解耦合

不同点：

     1、抽象类中可以定义类的全部普通成员，接口只能定义常量、抽象方法（JDK 8新增的三种方法）

     2、抽象类只能被类单继承，接口可以被类多实现
     3、一个类继承抽象类就不能再继承其他类，一个类实现了接口（还可以继承其他类或者实现其他接口）

     4、抽象类体现模板思想：跟利于做父类，实现代码的复用性   最佳实践

     5、接口更适合做功能的解耦合：解耦合性更强更灵活  最佳实践

