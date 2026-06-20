#### 代码块
代码块是类的五大成分之一（成员变量、构造器、方法、代码块、内部类）

分为两种：

    静态代码块：
    格式：static{}
    特点：类加载时自动执行，由于类只会加载一次，所以静态代码块也只会执行一次。
    作用：完成类的初始化，例如：对静态变量的初始化

```java
public static String SchoolName ;
public static String[] cards = new String[54];
static {
    SchoolName = "xixi";
    cards[2] = "2";
    //...
}
```

    实例代码块（构造代码块）:
    格式：{}
    特点：每次创建对象时，照执行实例代码块，并在构造器前执行。
    作用：和构造器一样，都是用来完成对象的初始化，例如：对实例变量进行初始化赋值。

```java
private String name;
private String[] diection = new String[4];
{
        System.out.println("实例代码块执行");
        name = "xixi";
        diection[0] = "N";
        diection[1] = "S";
        diection[2] = "E";
        diection[3] = "W";
    }
public static void main(String[] args) {
    new CodeDemo2();
}
```    

#### 内部类
如果一个类定义在另一个类的内部，这个类就是内部类。

场景：当一个类的内部，包含了一个完整的事物，且这个事物没有必要单独设计时，就可以把这个事物设计成内部类。

##### 成员内部类
就是类中的一个普通成员，类似前面我们学过的普通成员变量、成员方法。

```java
public class Outer {
    //成员内部类
    public class Inner{
    }
}
```

创建对象的格式：

外部类名称.内部类名称 对象名 = new 外部类名称.内部类名称();

Outer.Inner oi = new Outer().new Inner();

![alt test](/image/屏幕截图%202026-06-19%20152429.png)

成员内部类访问外部类成员的特点（拓展）

1.成员内部类可以直接访问外部类的静态变量,也可以直接访问外部类的实例成员

2.成员内部类的实例方法中，可以直接拿到当前寄生的外部类对象：外部类名称.this

#### 静态内部类 
有static修饰的内部类，属于外部类自己持有

```java
public class Outer {
    //静态内部类，属于外部类本身持有
    public static class Inner{
    }
}
```

格式：

外部类名称.静态内部类名称 对象名 = new 外部类名称.静态内部类名称();

Outer.Inner oi = new Outer.Inner();

静态内部中是否可以直接访问外部类的静态成员？  可以

静态内部类是否可以直接访问外部类的实例成员？  不可以

##### 匿名内部类
是一种特殊的局部内部类；

所谓匿名：指的是程序员不需要为这个类声明名字，默认有个隐藏的名字

匿名内部类实际上是有名字，外部类名$编号.class

    new 类或接口（参考值...）{
        类体（一般是方法重写）;
    } ;

特点：匿名内部类本质就是一个子类，并会立即创建出一个子类对象。

作用：用于更方便的创建一个子类对象。

```java
Animal animal = new Animal() {
    @Override
    public void cry() {
        System.out.println("喵喵喵");
    }
};
```

###### 匿名内部类的常见形式
通常可以做为一个对象参数传输给方法使用

```java
public class Test2 {
    public static void main(String[] args) {
        //目标：搞清楚匿名内部类的使用形式（语法），通常可以做为一个对象参数传输给方法使用
        //需求：学生、老师都要参加游泳比赛
        Swim s = new Swim() {
            @Override
            public void swimming() {
                System.out.println("学生开始游泳");
            }
        };
        play(s);


        System.out.println("------------------");


        play(new Swim() {
            @Override
            public void swimming() {
                System.out.println("老师开始游泳");
            }
        });
    }


        //设计一个方法，可以接收老师、学生开始比赛
        public static void play(Swim s){
            System.out.println("开始比赛");
            s.swimming();
            System.out.println("比赛结束");
        }
    }

interface Swim{
    void swimming();//游泳方法
}
```

涉及对象回调：

    将一个对象（包含特定方法）作为参数传递给另一个对象，当后者在某个特定时刻（如任务完成、事件发生）需要时，会“反向”调用前者的这个方法。

###### 应用场景
调用别人提供的方法实现需求时，这个方法正好可以让我们传输一个匿名内部类对象给其使用。

```java
java要求必须给这个按钮添加一个点击事件监听对象，这样就可以监听用户的点击操作，就可以作出反应
开发中不是我们主动去写匿名内部类，而是用别人的功能的时候，别人可以让我们写一个匿名内部类，我们才会写
btn.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("用户点击了登录按钮");
    }
});
```

使用comparator接口的匿名内部类实现对数组进行排序

按照年龄升序排序，可以调用sun公司写好的API直接进行排序

public static void sort(T[] a,Comparator<T> c)

参数一：需要排序的数组

参数二：需要给sort声明一个Comparator比较器对象（指定排序的规则）

sort方法会调用匿名内部类对象的compare方法，对数组中的学生对象进行两两比较，从而实现排序

```java
Arrays.sort(students, new Comparator<Student>(){
    @Override
    public int compare(Student o1, Student o2) {
        //指定排序规则
        //如果你认为左边对象 大于 右边对象，则返回正整数
        //如果如果你认为左边对象 小于 右边对象，则返回负整数
        //如果如果你认为左边对象 等于 右边对象，则返回0
//                if(o1.getAge() > o2.getAge()){
//                    return 1;
//                }else if(o1.getAge() < o2.getAge()){
//                    return -1;
//                }
//                return 0 ;
//                return o1.getAge() - o2.getAge();//按照年龄升序排序
        return o2.getAge() - o1.getAge();//按照年龄降序排序
    }
});
```

#### 函数式编程
此“函数”类似于数学中的函数（强调做什么），只要输入的数据一致返回的结果也是一致的

解决了社么问题？

使用Lambda函数代替某些匿名内部类对象，从而让程序代码更简洁，可读性更好

##### Lambda表达式
JDK 8 开始新增的一种语法形式，它表示函数

可以用于代替某些匿名内部类对象，从而让程序更简洁，可读性更好

    (被重写方法的形参列表) -> {
        被重写方法的方法体代码
    }

* 注意：Lambda表达式只能替代函数式接口的匿名内部类！！
* 函数式接口：有且仅有一个抽象方法的接口。
  
    @FunctionalInterface 注解，用于约束当前接口必须是函数式接口

###### 使用Lambda化简comparator接口的匿名内部类

![alt test](/image//屏幕截图%202026-06-19%20180211.png)

Lambda表达式的省略规则

作用：进一步简化Lambda表达式的写法。

具体规则：
* 参数类型全部可以不写
* 如果只有一个参数，参数类型省略的同时“()”也可以省略，但多个参数不能省略“()”
* 如果Lambda表达式中只有一行代码，大括号可以不写，同时要省略分号“;”如果这行代码是return语句，也必须去掉return。

##### 方法引用
###### 静态方法引用
格式：类名::静态方法

使用场景：

如果某个Lambda表达式里只是调用一个静态方法，并且“->”前后参数的形式一致，就可以使用静态方法引用

```java
//学生类中创建静态方法封装
...
public static int compareByAge(Student o1, Student o2){
        return o1.getAge() -o2.getAge();
    }
...
```
```java
...
//需求：按照年龄升序排序，可以调用sun公司写好的API直接进行排序
//        Arrays.sort(students, (o1, o2) -> o2.getAge() - o1.getAge());

//        Arrays.sort(students, (o1, o2) -> Student.compareByAge(o1, o2));
//静态方法引用：引用类名::方法名
    Arrays.sort(students, Student::compareByAge);
...
```

###### 实例方法引用
格式：对象名::实例方法

使用场景：

    如果某个Lambda表达式里只是通过对象名称调用一个实例方法，并且“->”前后参数的形式一致，就可以使用实例方法引用

```java
//学生类中创建实例方法封装
public int compareByHeight(Student o1, Student o2){
        //按照身高比较
        return Double.compare(o1.getHeight(), o2.getHeight());
    }
...
```
```java
...
Student t = new Student();
Arrays.sort(students, (o1, o2) -> t.compareByHeight(o1, o2));

//实例方法引用：引用对象名::方法名\
Arrays.sort(students, t::compareByHeight);
...
```

###### 特定类型的方法引用
格式：特定类的名称::方法

使用场景：

    如果某个Lambda表达式里只是调用一个特定类型的实例方法，并且前面参数列表中的第一个参数式作为方法的主调，后面的所有参数都是作为该实例方法的入参的，则此时就可以使用特定类型的方法引用

```java
public static void main(String[] args) {
        //目标：特定类型的方法引用
        //需求：有一个字符串数组，里面有一些人名，英文名称，请按照名字的首字母升序排序
    String[] names = {"Tim","Andy","Jerry","Boi","Mike","angela","caocao","喜喜"};

        //把这个数组进行排序：Arrays.sort(names,Comparator)
        //Arrays.sort(names);//默认是按照首字母升序排序
        //要求：忽略首字母的大小写进行升序排序（Java官方是搞不定的，需要我们自己指定比较规则）
//        Arrays.sort(names, new Comparator<String>() {
//            @Override
//            public int compare(String o1, String o2) {
//                //o1 angela
//                //o2 Andy
//                return o1.compareToIgnoreCase(o2);//Java以及为我们提供了字符串忽略大小写按照首字母比较的方法 compareToIgnoreCase
//            }
//        });

//        Arrays.sort(names, (o1, o2) -> o1.compareToIgnoreCase(o2));

    //特定类型方法引用：类型名称::方法名
    //compareToIgnoreCase String类型方法
    Arrays.sort(names, String::compareToIgnoreCase);

    System.out.println(Arrays.toString(names));
}
```

###### 构造器引用
格式：类名::new

使用场景：
    如果某个Lambda表达式里只是在创建对象，并且”->“前后参数情况一致，就可以使用构造器引用

```java
public static void main(String[] args) {
    //目标：理解构造器引用
//        CarFactory cf = new CarFactory() {
//            @Override
//            public Car getCar(String name) {
//                return new Car(name);
//            }
//        };

    //CarFactory cf = name -> new Car(name);

    //构造器引用: 类名::new
    CarFactory cf = Car::new;
    Car car = cf.getCar("法拉利");
    System.out.println(car);
}

@FunctionalInterface
interface CarFactory{
    Car getCar(String name);
}
```

#### 常用API
##### String
String代表字符串，它的对象可以封装字符串数据，并提供了很多方法完成对字符串的处理。

1. 创建字符串对象，封装字符串数据

    方法一(推荐)：Java中的所有字符串文字（例如"abc"）都为此类对象。

     String s1 = "hello world";

    方法二：调用String类的构造器初始化字符串对象。

    ![alt test](/image//屏幕截图%202026-06-20%20121542.png)

    ```java
    String s2 = new String();
    System.out.println(s2);//空字符串

    String s3 = new String("hello world");
    System.out.println(s3);

    char[] chs = {'h', 'e', 'l', 'l', 'o'};
    String s4 = new String(chs);
    System.out.println(s4);

    byte[] bytes = {97, 98, 99, 100};
    String s5 = new String(bytes);
    System.out.println(s5);
    ```

String创建对象的区别
* 只要是以“....”方式写出的字符串对象，会存储到字符串常量池，且相同内容的字符串只存储一份；
* 通过new创建的字符串对象，会存储在堆中，每new一次都会产生一个新的对象放在堆内存中。

额外的：字符串对象的内容比较，千万不要用 ==，==默认比较地址，字符串对象的内容一样是地址不一定一样

判断你字符串的内容，建议用equals方法，只关心内容一样，就返回true，不关心地址

```java
if (username.equals(name)){
            System.out.println("登录成功！");
        }else {
            System.out.println("登录失败！");
        }
```
2. 调用String提供的操作字符串数据的方法

#### 集合
集合是一种容器，用来装数据，类似于数组

有数组，为什么还学集合？
* 数组定义完成并启动后，长度就固定了

##### ArrayList集合

1. 创建ArrayList对象，代表一个集合容器
2. 调用ArrayList提供的方法，对容器中的数据进行增删改查操作

![alt test](/image//屏幕截图%202026-06-20%20144214.png)

```java
public static void main(String[] args) {
    //目标：掌握ArrayList集合的基本使用
    //添加数据
    //创建ArrayList对象，代表一个集合容器
    ArrayList<String> list = new ArrayList<>();//泛型定义集合
    list.add("hello");
    list.add("world2");
    list.add("java");
    list.add("java4");
    System.out.println(list);
    System.out.println("==============");
    //查看数据
    System.out.println(list.get(0));
    System.out.println(list.get(1));
    System.out.println(list.get(2));
    System.out.println("==============");
        //遍历集合
    for (int i = 0; i < list.size(); i++) {
        String s =list.get(i);
        System.out.println(s);
        }
    System.out.println("==============");
    //删除数据
    System.out.println(list.remove(0));//根据索引删除
    System.out.println(list);
    System.out.println(list.remove("java"));//根据元素删除
    System.out.println(list);
    //修改数据
    list.set(0,"hello world");
    System.out.println(list);
}
```

#### GUI编程
图形用户界面

Java的GUI编程包
* AWT
  
    提供一组原生的GUI组件，依赖于操作系统的本地窗口系统
* Swing
  
    基于AWT，提供了更丰富的GUI组件，不依赖于本地窗口系统

常用的Swing组件
* JFram:窗口
* JPanel:用于组织其他组件的容器
* JButton:按钮组件
* JTextField:输入框
* JTable:表格
  
```java
//1.创建一个窗口：有一个登录按钮
        JFrame win = new JFrame("登录窗口");

        JPanel panel = new JPanel();//创建面板
        win.add(panel);//添加面板

        win.setSize(400, 300); // 设置窗口大小
        win.setLocationRelativeTo(null); // 设置窗口居中
        win.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);// 设置关闭窗口的默认操作：关闭窗口退出程序

        JButton btn = new JButton("登录");//创建登录按钮
        panel.add(btn);//添加按钮

        win.setVisible(true);//显示窗口
```

##### 常见布局管理器
它们可以决定组件在容器中的布局方式，避免了手动设置每个组件的位置和大小，从而简化了GUI设计过程

###### FlowLayout

- **简介**: FlowLayout 是最简单的布局管理器，它按水平方向从左到右排列组件，当一行排满时，自动换到下一行。

- **特点**

  - 默认居中对齐，可以设置为左对齐或右对齐。
  - 适用于需要简单排列的场景。

- **代码示例**

  ```java
  import java.awt.*;
  
  public class FlowLayoutExample {
      public static void main(String[] args) {
          JFrame frame = new JFrame("FlowLayout Example");
          frame.setSize(400, 300);
          frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
  
          frame.setLayout(new FlowLayout());
  
          frame.add(new JButton("Button 1"));
          frame.add(new JButton("Button 2"));
          frame.add(new JButton("Button 3"));
          frame.add(new JButton("Button 4"));
          frame.add(new JButton("Button 5"));
  
          frame.setVisible(true);
      }
  }
  ```

###### BorderLayout

- **简介**: BorderLayout 将容器划分为五个区域：东、南、西、北和中（East, South, West, North, Center）。每个区域只能添加一个组件，未添加组件的区域保持空白。

- **特点**

  - 适用于需要在特定区域布局组件的场景。
  - 中间区域会占据所有剩余的空间。

- **代码示例**

  ```java
  import java.awt.*;
  
  public class BorderLayoutExample {
      public static void main(String[] args) {
          JFrame frame = new JFrame("BorderLayout Example");
          frame.setSize(400, 300);
          frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
  
          frame.setLayout(new BorderLayout());
  
          frame.add(new JButton("North"), BorderLayout.NORTH);
          frame.add(new JButton("South"), BorderLayout.SOUTH);
          frame.add(new JButton("East"), BorderLayout.EAST);
          frame.add(new JButton("West"), BorderLayout.WEST);
          frame.add(new JButton("Center"), BorderLayout.CENTER);
  
          frame.setVisible(true);
      }
  }
  ```

###### GridLayout

- **简介**: GridLayout 将容器划分为等大小的网格，每个网格中可以添加一个组件，所有组件大小相同。

- **特点**

  - 适用于需要均匀排列组件的场景。
  - 行和列的数量可以指定。

- **代码示例**

  ```java
  java复制代码import javax.swing.*;
  import java.awt.*;
  
  public class GridLayoutExample {
      public static void main(String[] args) {
          JFrame frame = new JFrame("GridLayout Example");
          frame.setSize(400, 300);
          frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
  
          frame.setLayout(new GridLayout(2, 3)); // 2行3列的网格
  
          frame.add(new JButton("Button 1"));
          frame.add(new JButton("Button 2"));
          frame.add(new JButton("Button 3"));
          frame.add(new JButton("Button 4"));
          frame.add(new JButton("Button 5"));
          frame.add(new JButton("Button 6"));
  
          frame.setVisible(true);
      }
  }
  ```

###### BoxLayout

- **简介**: BoxLayout 能够沿着单一轴线（X轴或Y轴）排列组件。可以创建水平（X轴）或垂直（Y轴）排列的布局。

- **特点**

  - 适用于需要沿单一方向排列组件的场景。
  - 可以通过添加垂直或水平间隔（Glue、Strut）来调整组件间距。

- **代码示例**

  ```java
  import java.awt.*;
  
  public class BoxLayoutExample {
      public static void main(String[] args) {
          JFrame frame = new JFrame("BoxLayout Example");
          frame.setSize(400, 300);
          frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
  
          JPanel panel = new JPanel();
          panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS)); // 垂直排列
  
          panel.add(new JButton("Button 1"));
          panel.add(Box.createVerticalStrut(10)); // 添加垂直间隔
          panel.add(new JButton("Button 2"));
          panel.add(Box.createVerticalStrut(10));
          panel.add(new JButton("Button 3"));
          panel.add(Box.createVerticalStrut(10));
          panel.add(new JButton("Button 4"));
  
          frame.add(panel);
          frame.setVisible(true);
      }
  }
  ```

这些示例展示了布局管理器在Java Swing中的基本用法。你可以根据实际需求选择合适的布局管理器来设计你的GUI应用程序。

##### 事件处理
在GUI编程中，事件的处理是通过事件监听器来完成的

常用的事件监听器对象
* 点击事件监听器ActionListener
* 按键事件见监听器KeyListener
* 鼠标事件监听器MouseListener
* ...

##### 事件的几种常见写法

第一种：直接提供实现类，用于创建事件监听对象

第二种：直接使用匿名内部类的对象，代表事件监听对象

第三种：自定义窗口，让窗口对象实现事件接口