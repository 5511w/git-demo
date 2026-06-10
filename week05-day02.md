#### 继承
Java中提供了一个关键字extends,用这个关键字，可以让一个类和另一个类建立其父子关系

```
public class B extends A{

}
// A类称为父类，B类称为子类
```

子类能继承父类的非私有成员（成员变量、成员方法）

子类的对象是由子类、父类共同完成的

子类对象其实是由子类和父类多张设计图共同创建出来的对象，所以子类对象是完整的。

继承的好处：1.代码复用 2.减少重复代码

#### 权限修饰符
就是用来限制类中的成员（成员变量、成员方法、构造器）能够被访问的范围。

```java
//1.private 只能当前类中访问
    private void privateMethod(){
        System.out.println("privateMethod");
    }

//2.缺省，只能当前类中访问，同一包的其他类中访问
    void defaultMethod(){
        System.out.println("defaultMethod");
    }

//3.protected，只能当前类中访问，同一包中的其他类中访问，子孙类中访问
    protected void protectedMethod(){
        System.out.println("protectedMethod");
    }

//4.public，所有类中访问
    public void publicMethod(){
        System.out.println("publicMethod");
    }
```

private < 缺省 < protected < public

成员变量大概率私有，方法公开

#### 继承的特点

1. java的类只能是单继承的，不支持多继承，支持多层继承
   
    为什么不支持多继承？

    ![alt test](/image/屏幕截图%202026-06-09%20163522.png)
2. 一个类要么直接继承object(祖宗类)，要么默认继承object，要么间接继承 object


3. 继承后子类的访问特点，就近原则

    在子类方法中访问其他成员（成员变量、成员方法），是依照就近原则

    先子类局部范围找，让后子类成员范围找，然后父类成员范围找，如果父类范围还没有找到则报错。

    如果父类中，出现了重名的成员，会优先使用子类的，如果此时一定要在子类中使用父类的怎么办？

    可以使用super关键字，指定访问父类成员：super.父类成员变量/父类成员方法

#### 方法重写
当子类觉得父类中的某个方法不好用，或者无法满足自己的需求时，子类可以重写一个方法名称、参数列表一样的方法，去覆盖父类的这个方法

```java
class Cat extends Animal{
    //方法重写：方法名称、参数列表必须相同，这个方法就是方法重写
    //重写的规范：声明不变，重新实现
    @Override//方法重写的校验注解（标志）：要求方法名称和参数列表必须与被重写方法一致，否则会报错
    public void cry() {
        System.out.println("猫会喵喵喵~~~");
    }
}
class Animal{
    public void cry(){
        System.out.println("动物会叫~~~");
    }
}
```

其他注意事项：

* 子类重写父类方法时，访问权限必须大于或等于父类该方法的权限（public>protected>缺省）
* 重写的方法返回值类型，必须与被重写返回值类型一样，或者范围更小
* 私有方法、静态方法不能被重写，如果重写会报错

##### 常见的应用场景
子类重写Object类的toString方法，以便返回对象的内容

```java
 Student s = new Student("小王", 18, '男');
System.out.println(s);//com.xixi.extends5override.Student@1b6d3586  所谓的地址
//System.out.println(s.toString());//com.xixi.extends5override.Student@1b6d3586

//注意1：直接输出对象，默认调用object的toString方法（可以省略不写调用toString的代码），返回对象的地址信息

//注意2：输出对象的地址实际上没有什么意义，开发中更希望输出对象时看对象的内容信息，所以子类需要重写object的toString方法，以便以后输出对象时默认就近调用子类重写的toString方法返回对象的内容
```

#### 子类构造器的特点
子类的全部构造器，都会先调用父类的构造器，再执行自己的

子类构造器必须先调用父类构造器，再执行自己的构造器

子类构造器是如果实现调用父类构造器的：

* 默认情况下，子类全部构造器的第一行代码都是super()（写不写都有），它会调用父类的无参构造器
* 如果父类没有无参构造器，则我们必须在子类构造器的第一行手写super(...)，去指定调用父类的有参数构造器

```java
public Teacher(String name, char sex, String skill) {
        //子类构造器调用父类构造器的应用场景
        //可以把子类继承自父类这部分的数据也完成初始化赋值
        super(name,sex);
        this.skill=skill;
    }
```


    子类构造器可以通过调用父类构造器，把对象中包含父类这部分的数据先初始化赋值

    再回来把对象里包含子类这部分的数据也进行初始化赋值

#### this（...）调用兄弟构造器
在任意类的构造器中，是可以通过this（...）调用其他构造器

```java
 public Student(String name, int age, char sex) {
        //this调用兄弟构造器,节省代码
        //注意：super(...) this(...)必须写在构造器的第一行，且两个不能同时出现
        this(name, age, sex, "西安邮电大学");
    }

public Student(String name, int age, char sex, String schoolName) {
        //super();//必须在第一行
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.schoolName = schoolName;

    }
```

#### 多态
多态是在继承/实现情况下的一种现象，表现为：对象多态。行为多态

```java
//1.对象多态,行为多态
Animal a1 = new Wolf();
a1.run();//方法：编译看左边，运行看右边
System.out.println(a1.name);//成员变量：编译看左边，运行也看左边
```

多态是对象、行为的多态，Java中的属性（成员变量）不谈多态

##### 多态的前提
有继承/实现关系；存在父类引用子类对象；存在方法重写

#### 多态的好处
1. 多态的好处1：右边的对象时解耦合的

```java
Animal a1 = new Tortoise();
    a1.run();
```

2. 多态好处2：父类类型的变量作为参数，可以接收一个子类对象

多态下的一个问题:

多态下不能调用子类特有方法

##### 多态下的类型转换问题

* 自动类型转换：父类 变量名 = new 子类（）
    ```java
    Animal a1 = new Tortoise();
    ```

* 强制类型转换：子类 变量名 = （子类）父类变量
    ```java
    Tortoise c1 = (Tortoise) a1;
    ```

强制类型转换注意事项

* 存在继承/实现关系就可以在编译阶段进行强制类型转换，编译阶段不会报错。
* 运行时，如果发现对象的真实类型与转换后的类型不同，就会报类型转换异常（ClassCastException）的错误出来

```java
Animal a1 = new Tortoise();
Wolf w1 = (Wolf) a1;//运行报错
```

强转前，Java建议：

使用instanceof关键字，判断当前对象的真实类型，再进行强转

```java
public static void play(Animal a){
    System.out.println("开始游戏......");
    a.run();
    //a1.eat();//报错，多态下不能调用子类特有方法

    //java建议强制转换前，应该判断对象的真实类型，再进行强制类型转换
    if(a instanceof Wolf){
        Wolf w1 = (Wolf) a;
        w1.attack();
    }else if(a instanceof Tortoise){
        Tortoise t1 = (Tortoise) a;
        t1.eat();
    }
}
```

其他：

```java
//lombok技术可以实现类自动添加getter、setter方法，无参数构造器，toString方法等。
@Data //@Data注解可以自动生成getter、setter方法，无参数构造器，toString方法等。
@NoArgsConstructor //生成无参构造器
@AllArgsConstructor //生成全参构造器
```

使用需打开功能

setting，搜索annotat,勾选，应用后确认