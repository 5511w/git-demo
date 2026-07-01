### 异常
异常代表程序出现的问题

![alt test](/image/屏幕截图%202026-06-27%20114018.png)

#### 运行时异常
编译阶段不报错，运行时出现异常，继承自RuntimeException 

```java
int[] arr = new int[]{1,2,3};
System.out.println(arr[3]);//ArrayIndexOutOfBoundsException

System.out.println(10/0);//ArithmeticException

//空值异常
String str = null;
System.out.println(str);
System.out.println(str.length());//NullPointerException
System.out.println("show方法结束");
```

#### 编译时异常
编译阶段报错，运行时不会报错 提醒作用

#### 异常的基本处理

1. 抛出异常（throws）
   * 在方法上使用throws关键字，可以将方法内部出现的异常抛出去给调用者处理
    ```java
    public static void show2() throws ParseException{
        ...
    }
    ```
2. 捕获异常（try...catch）
   * 直接捕获程序出现的异常
    ```java
    try{
        //监视可能出现异常的代码
    }catch(){
        //处理异常
    }catch(){
        //处理异常
    }...
    ```

同时出现多个异常：

```java
...
try {
            //监视代码，出现异常，会被catch拦截这个异常
            show2();
//        } catch (ParseException e) {
//            e.printStackTrace();//打印异常信息
//        } catch (FileNotFoundException e) {
//            e.printStackTrace();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
...
public static void show2() throws Exception {
        System.out.println("show2方法开始");
        //编译异常：编译阶段报错，运行时不会报错
        String str = "2024-01-01";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date date = sdf.parse(str);//编译时异常，提醒这里的代码容易出错
        System.out.println(date);

        InputStream is = new FileInputStream("D:/a.txt");//FileNotFoundException
    }
```

#### 异常的作用

1. 异常是用来定位程序bug的关键信息

2. 可以作为方法内部的一种特殊返回值以便通知上层调用者，方法的执行问题
    ```java
    public class Exception2 {
    public static void main(String[] args) {
        //目标：搞清楚异常的作用
        System.out.println("开始执行");
        try {
            System.out.println(getResult(10,0));
            System.out.println("底层方法成功");
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("底层方法出现异常");
        }
        System.out.println("方法结束");
    }

    //需求：求2个的除的结果，并返回这个结果
    public static int getResult(int a,int b) throws Exception {
        if(b==0){
            System.out.println("除数不能为0");
            //可以返回一个异常给上层调用者，返回的异常还能告知上层或底层时执行成功还是失败
            throw new Exception("除数不能为0");
        }
        int res = a/b;
        return res;
    }
    }
    ```

#### 自定义异常

![alt test](/image/屏幕截图%202026-06-27%20143805.png)

1. 自定义编辑异常
    ```java
     /**
    * 创建一个自定义异常类
    * 1.继承Exception
    * 2.重写Exception的构造器
    * 3，哪里需要用这个异常，哪里就throw这个异常
     */
    public class AgeException extends Exception{
        public AgeException() {
        }
        public AgeException(String message) {
        super(message);
        }
    }
    ```
2. 自定义运行异常
    ```java
    /**
    * 创建一个自定义运行时异常
    * 1.继承RuntimeException
    * 2.重写RuntimeException的构造器
    * 3，哪里需要用这个异常，哪里就throw这个异常
     */
    public class AgeRunTimeException extends RuntimeException{
        public AgeRunTimeException() {
        }
        public AgeRunTimeException(String message) {
            super(message);
        }
    }
    ```

#### 异常的处理方法

方案1：底层异常层层往上抛，最外层捕获异常，记录异常，并响应合适信息给用户提示

方案2：最外层捕获异常后，尝试重新修复

### 泛型
定义类、接口、方法时，同时声明了一个或多个类型变量（如：<E>） 称为泛型类、泛型接口、泛型方法，他们统称为泛型

作用：泛型提供了在编译阶段约束所能操作的数据类型，并自动进行检查的能力，这样可以避免强制类型转换，及可能出现的异常


#### 泛型类

修饰符 class 类名<类型变量,类型变量...>{

}

* 注意：类型变量建议用大写的英文字母，常用的有：E、T、K、V等

#### 泛型接口

修饰符 interface 接口名<类型变量,类型变量...>{

}

* 注意：类型变量建议用大写的英文字母，常用的有：E、T、K、V等

#### 泛型方法、通配符、上下限

泛型方法：

修饰符 <类型变量,类型变量...> 返回值类型 方法名(形参列表){

}

```java
public static <T> void printArray(T[] names){

    }
```

通配符：

就是“?”,可以在“使用泛型”的时候代表一切类型；E、T、K、V是在定义泛型的时候使用

泛型上下限：

* 泛型上限：?extends Car: ?能接收的必须是Car或者子类
* 泛型下限：?super Car : ?能接收的必须是Car或者其父类

```java
public class GenecriDemo5 {
    public static void main(String[] args) {
        //目标：理解通配符和上下限
        ArrayList<Xiaomi> xiaomis = new ArrayList<>();
        xiaomis.add(new Xiaomi());
        xiaomis.add(new Xiaomi());
        xiaomis.add(new Xiaomi());
        go(xiaomis);

        ArrayList<BYD> byds = new ArrayList<>();
        byds.add(new BYD());
        byds.add(new BYD());
        byds.add(new BYD());
        go(byds);

//        ArrayList<Dog> dogs = new ArrayList<>();
//        dogs.add(new Dog());
//        dogs.add(new Dog());
//        dogs.add(new Dog());
//        go(dogs);

    }
    //需求开发一个极品飞车的游戏
    //虽然Xiaomi和BYD都是子类，但是ArrayList<Xiaomi>、ArrayList<BYD>和ArrayList<Car> 是没有关系的
    public static void go(ArrayList<? extends Car> cars){

    }
}
```

#### 泛型支持的类型
泛型不支持基本数据类型，只能支持对象类型（引用数据类型）

泛型擦除：泛型工作在编译阶段，等编译完之后，泛型就没用了，所以泛型在编译后都会被擦除，所有类型会回复成Object类型

##### 包装类
把基本类型的数据包装成对象的类型

泛型和集合都不支持基本类型，支持包装类

|基本数据类型|对应的包装类（引用数据类型）|
|----|----|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|char|Character|
|float|Float|
|double|Double|
|boolean|Boolean|

```java
//把基本数据类型变成包装类对象
//手工包装
//Integer i = new Integer(10);//过时
Integer i = Integer.valueOf(130);// 推荐 -128-127
Integer i2 = Integer.valueOf(130);// 推荐
System.out.println(i == i2);

//自动装箱成对象：基本数据类型的数据可以直接变成包装对象的数据，不需要额外做任何事情
Integer i3 = 130;
Integer i4 = 130;
System.out.println(i3 == i4);

//自动拆箱成基本数据类型：包装对象数据可以直接变成基本数据类型，不需要额外做任何事情
int i5 = i3;
System.out.println(i5);

ArrayList<Integer> list = new ArrayList<>();
list.add(123);//自动装箱成对象
list.add(456);//自动装箱成对象

int rs = list.get(0);//自动拆箱
```

```java
//包装类的新功能
///1.把基本类型的数据变成字符串
int j = 10;
String rs2 = Integer.toString(j);//"10"
System.out.println(rs2 + 1);//101

Integer a3 = j;
String rs3 = i3.toString(j);//"10"
System.out.println(rs3 + 1);//101

String rs4 = j + "";//"10"
System.out.println(rs4 + 1);//101

System.out.println("======================");

//2.把字符串数值转换成对应的基本数据类型（很有用）
String s = "123";
//int b = Integer.parseInt(s);
int b = Integer.valueOf(s);
System.out.println(b + 1);

String s2 = "123.456";
//double c = Double.parseDouble(s2);
double c = Double.valueOf(s2);
System.out.println(c + 1);
```

### 集合
集合是一种容器，用来装数据，类似与数组，但集合的大小可以改变，开发中也非常常用

![alt test](/image/屏幕截图%202026-06-27%20180020.png)

#### Collection常用功能
Collection是单列集合的祖宗，它规定的方法全部单例集合都会继承

```java
Collection<String> list = new ArrayList<>();
//1，添加元素
list.add("hello");
//2.获取集合元素的个数
int size = list.size();
//3.删除集合
list.remove("hello");
//4.判断集合是否为空
boolean empty = list.isEmpty();//false
//5.判断集合中是否存在某个元素
boolean contains = list.contains("world");
//6，清空集合
list.clear();
//7.把集合转换成数组
Object[] array = list.toArray();
System.out.println(Arrays.toString(array));
//把集合转换成字符串数组
String[] arr2 = list.toArray(String[]::new);
```

##### Collection的三种遍历方式

方式一：迭代器遍历

迭代器是用来遍历集合的专用方法（数组没有迭代器），在Java中迭代器的代表是lterator

```java
Collection<String> names = new ArrayList<>();
    names.add("小王");
    names.add("小张");
    names.add("小李");
    names.add("小赵");
    System.out.println(names);//[小王, 小张, 小李, 小赵]
                        //        it
while (it.hasNext()) {
        String name = it.next();
        System.out.println(name);
    }
```

方式二：增强for循环

格式：

for(元素的数据类型 变量名 : 数组或者集合){

}

增强for可以用来变量集合或者数组

```java
for (String name : names) {
    System.out.println(name);
}

String[] users = {"小王", "小张", "小李", "小赵"};
for (String user : users) {
    System.out.println(user);
}
```

方式三：Lambda表达式

![alt test](/image/屏幕截图%202026-06-27%20182832.png)

##### 三种遍历方式的区别

并发修改异常问题
* 遍历集合的同时又存在增删集合元素的行为时可能出现业务异常，这种现象被称之为并发修改异常问题

```java
ArrayList<String> list = new ArrayList<>();
list.add("Java入门");
list.add("宁夏枸杞");
list.add("黑枸杞");
list.add("特级枸杞");
list.add("人字拖");
list.add("枸杞子");
list.add("西洋参");
```

需求：删除全部枸杞

```java
for (int i = 0; i < list.size(); i++) {
    String name = list.get(i);
    if (name.contains("枸杞")) {
        list.remove(name);
    }
}
System.out.println(list);//出现并发修改异常
//[Java入门,黑枸杞,人字拖,西洋参]
```

解决方法1：删除数据后，做一步i--

```java
for (int i = 0; i < list2.size(); i++) {
    String name = list2.get(i);
    if (name.contains("枸杞")) {
        list2.remove(name);
        i--;//解决方法1：删除数据后，做一步i--
    }
}
```

解决方法2：倒着遍历并删除（前提是支持索引）

```java
 for (int i = list3.size() - 1; i >= 0; i--) {
    String name = list3.get(i);
    if (name.contains("枸杞")) {
        list3.remove(name);
    }
}
System.out.println(list3);
```

方案一:迭代器遍历并删除默认也存在并发修改异常问题

可以解决3：使用迭代器的remove方法

```java
Iterator<String> it = list4.iterator();
while (it.hasNext()) {
    String name = it.next();
    if (name.contains("枸杞")) {
        it.remove();
    }
}
System.out.println(list4);
```

方案二和三:增强for还有Lambda(都没有办法解决并发修改异常问题)

增强for和Lambda只适合做遍历，不适合做遍历并修改操作

```java
for (String name : list5) {
    if (name.contains("枸杞")) {
        list5.remove(name);
    }
}

list5.forEach(name -> {
    if (name.contains("枸杞")) {
        list5.remove(name);
    }
});
System.out.println(list5);
```

#### List集合

![alt test](/image/屏幕截图%202026-06-28%20142502.png)

List集合特有方法

```java
List<String> list = new ArrayList<>();//一行经典代码

//1.添加数据
list.add("小王");
//给第三个位置添加数据，朝气
list.add(2, "朝气");
//删除：小李
System.out.println(list.remove(2));
//把小王修改成：小溪
System.out.println(list.set(0, "小溪"));
//获取小张
System.out.println(list.get(1));
```

list 集合支持的遍历方式

1. for循环
2. 迭代器
3. 增强for
4. Lambda表达式

##### ArrayList底层原理
基于数组存储数据，大小可变

数组的特点：

查询速度快（注意：是根据索引查询数据快）：查询数据通过地址值和索引定位，查询任意数据耗时相同

增删数据效率低：可能需要把后面很多的数据进行前移

ArrayList集合第一次添加数据的时候扩容为10，后面加满后会扩容成原来的1.5倍

##### LinkList底层原理
基于链表存储数据，基于双链表实现的

链表的特点

链表中的数据是一个一个独立的结点组成的，结点在内存中是不连续的，每个结点包含数据值和下一个结点的地址

特点1：查询慢，无论查询哪个数据都要从头开始找

特点2：链表增删相对快

双链表特点：对首尾元素进行增删改查的速度是极快的

![lat test](/image/屏幕截图%202026-06-28%20150609.png)
