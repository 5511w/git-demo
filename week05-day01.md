### 面向对象编程
面向对象的三大特征：封装、继承、多态
#### 对象
对象是一种特殊的数据结构(可以理解成一张表)，可以用来记住一个事物的数据，从而代表该事物

class也就是类，也称为对象的设计图（或者对象的模板）

![alt test](/image/屏幕截图%202026-06-05%20161055.png)
![alt test](/image/屏幕截图%202026-06-05%20160803.png)

万物皆对象，谁的数据谁存储

#### 步骤
1. 先设计对象的模板，也就是对象的设计图：类
2. 通过new关键字，每new一次类就得到一个新对象
 
![alt test](/image/屏幕截图%202026-06-05%20154753.png)

#### 类的基本语法
##### 构造器
```java
public class Studebt{
    /** 构造器 **/
    public Student(){
                ...
    }
}
```

1. 无参构造器
```java
public Student(){
        System.out.println("=======无参数构造器=====");
    }
```

2. 有参构造器
```java
public Student(String n){
        System.out.println("=======有参数构造器=====");
    }
    //重载
    public Student(String name,int age,char sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
```

特点：

创建对象时，对象会自动去调用构造器

###### 常见应用场景

创建对象时，可以指定对象去调用哪个构造器执行

创建对象时，同时完成对对成员变量（属性）的初始化赋值

```java 
//Studebt.java
public Student(String name,int age,char sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
```

```java
//Test.java
 Student s4 = new Student("小王",18,'男');
System.out.println(s4.name);
System.out.println(s4.age);
System.out.println(s4.sex);
```

###### 注意事项
* 类默认就自带了一个无参构造器
* 如果为类定义了有参构造器，类默认的无参构造器就没有了。如果还想用无参构造器，就必须自己手写一个无参构造器出来。

##### this关键字
this就是一个变量，可以用在方法中，来拿到当前对象

那个对象调用这个方法，this就拿到那个对象

###### 应用场景
主要用来解决对象的成员变量与方法内部变量的名称一样时，导致访问冲突问题

![alt test](/image/屏幕截图%202026-06-05%20181451.png)

##### 封装
用类设计对象去处理某一个实物的数据时，应该把要处理的数据，以及处理这些数据的方法，设计到一个对象中去

类就是一种封装

###### 设计要求
合理隐藏、合理暴露

1. 如何隐藏：使用private关键字（私有，隐藏）修饰成员变量，就只能在本类中访问，其他地方不能直接访问

```java
public class Student {
    private int age;
    ...
}
```

2. 如何暴露（合理暴露）：使用public修饰（公开）的get和set方法合理暴露

    成员变量的取值或赋值

```java
public void setAge(int age) {//为年龄赋值
        if (age > 0 && age < 200) {
            this.age = age;
        }else{
            System.out.println("年龄输入有误");
        }
    }

    public int getAge() {//获取年龄
        return age;
    }
```

* 即使不进行校验也要进行封装

    右击generate...
```java
public String getName() {
        return name;
    }

public void setName(String name) {
        this.name = name;
    }
```

代码层面如何控制对象成员公开或隐藏？

公开成员，可以使用public（公开）进行修饰

隐藏成员，使用private（私有、隐藏）进行修饰

##### 实体类
是一种特殊类，类中要求满足如下需求

1. 类中的成员变量全部私有，并提供public修饰的getter/setter方法
2. 类中需要提供一个无参数构造器，有参数构造器可选

基本作用：

创建它的对象，存取数据（封装数据）
```java
Student s1 = new Student();
s1.setName("小王");
System.out.println(s1.getName());
s1.setChinese(80);
System.out.println(s1.getChinese());
s1.setMath(90);
System.out.println(s1.getMath());

Student s2 = new Student("小王", 80, 90);
System.out.println(s2.getName());
System.out.println(s2.getChinese());
System.out.println(s2.getMath());
```

###### 应用场景
实体类的对象只负责数据的存取，而对数据的业务处理交给其他类的对象来完成，以实现数据和数据业务的分离

```java
//Test.java
//创建一个学生的操作对象专门负责对学生对象的数据进行业务处理
StundentOperator operator = new StundentOperator(s2);
operator.printTotal();
operator.printAverage();
```

```java
//StudentOperator.java
public class StundentOperator {
    //必须拿到学生对象
    private Student s;
    public StundentOperator(Student s){
        this.s = s;
    }

    //提供方法：打印学生对象总成绩
    public void printTotal(){
        System.out.println("总成绩是：" + (s.getChinese() + s.getMath()));
    }

    //提供方法：打印学生对象平均分
    public void printAverage(){
        System.out.println("平均分是：" + (s.getChinese() + s.getMath()) / 2);
    }
}
```

##### static 关键字
叫静态，可以修饰成员变量、成员方法

###### static静态变量

按照有无static修饰分为：静态变量（类变量），实例变量（对象的变量）

![alt test](/image//屏幕截图%202026-06-06%20182046.png)

```java
//1.类名.静态变量（推荐）
Student.name = "小王";
System.out.println(Student.name);

//2.对象名.静态变量（不推荐）
Student s1 = new Student();
s1.name = "马冬梅";

Student s2 = new Student();
s2.name = "秋雅";

System.out.println(s1.name);//秋雅
System.out.println(Student.name);//秋雅

//3.对象名.实例变量
s1.age=23 ;
s2.age=18;
System.out.println(s1.age);//23
//System.out.println(Student.age);报错
```

应用场景

如果某个数据只需要一份，且希望=能够被共享（访问、修改），则改数据可以定义成静态变量来记住。

例：系统启动后，要求用户类可以记住自己创建了多少个用户对象。
```java
//User.java
public User(){
        //User.count++;
        //注意：同一个类中访问静态成员可以省略类名不写
        count++;
    }
```

```java
//Test2.java
new User();
new User();

System.out.println(User.count);
```

###### static静态方法

静态方法：有static修饰的成员方法，属于类
![alt test](/image/屏幕截图%202026-06-06%20184010.png)

实例方法：无static修饰的成员方法，属于对象
![alt test](/image/屏幕截图%202026-06-06%20184220.png)

使用静态方法还是实例方法？

如果这个方法只是为了做一个功能且不需要直接访问对象的数据，这个方法直接定义成静态方法

如果这个方法是对象的行为，需要访问对象的数据，这个方法必须定义成实例方法

静态方法的常见应用场景

做工具类
* 工具类中的方法都是一些静态方法，每个方法用来完成一个功能，以便供给开发人员直接使用
![alt test](/image/屏幕截图%202026-06-06%20185312.png)

提高代码的复用；调用方便

不用实例方法做工具类：

实例方法需要创建对象来调用，此时对象只是为了调用方法，对象占用内存，这样会浪费内存

```java
//工具类没有创建对象的必要性，所以建议私有构造器
private VerifyCodeUtil(){
    }
```

静态方法，实例方法访问相关的几点注意事项

* 静态方法中可以直接访问静态成员，不可以直接访问实例成员
```java
public static void printHelloWorld(){
        System.out.println(count);
        printHelloWorld2();
        //System.out.println(name);//错误
        //run();//错误
        //System.out.println(this);//错误
        
    }
```
* 实例方法中即可以直接访问静态成员，也可以直接访问实例成员
* 实例方法中可以出现this关键字，但是静态方法中不可以出现this关键字
```java
public void go(){
        System.out.println(count);
        printHelloWorld2();
        System.out.println(name);
        run();
        System.out.println(this);
    }
```