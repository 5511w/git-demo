### Java基础

#### 方法

特定的任务或操作代码块，代表一个功能，它可以接收数据进行处理，并返回一个处理后的结果。

```
修饰符 返回值类型 方法名（形参列表）{
    方法体代码（需要执行的功能代码）
    return返回值;
}
```

![alt text](image/屏幕截图%202026-05-23%20173934.png)

方法必须被调用才能执行，调用格式：方法名称（数据）

```java
public static void main(String[] args) {
     int sum = printSum(10, 20);
        System.out.println("和是:"+sum);
    }
 //定义一个方法：求任意两数的和
  public static int printSum(int a, int b) {
        return  a + b;
    }
```

##### 方法的其他类型

1. 方法是否需要接收数据？

    不需要接数据，形参列表为空

2. 方法是否需要返回数据?

    如果方法没有返回值，返回值类型必须声明成void（无返回值类型）方法内部不可以使用return返回数据。

```java
 //无参数、无返回值类型
 public static void printHelloWorld() {
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
    }
```

```java
//有参数、有返回值类型
public static String getVerifyCode(int len) {
        String code = "";
        for (int i = 0; i < len; i++) {
            code += (int)(Math.random() * 10);
        }
        return code;
    }
```

#### 方法的注意事项

1. 方法可以重载

    一个类中，出现多个方法的名称相同，但是它们的形参列表是不同的，那么这些方法就称为方法重载。

```java
//方法重载：方法名相同，参数列表不同（参数类型不同，参数个数不同，参数顺序不同）
//方法重载与返回值类型无关
public static void show(int a) {
        System.out.println(a);
    }
    public static void show(String a) {
        System.out.println(a);
    }
    public static void show(int a, int b) {
        System.out.println(a + b);
    }
    //重复的方法，错误
    /*
    public static void show(int a1, int b1) {
        System.out.println(a1 + b1);
    }*/
```

2. 在无返回值类型方法中，使用return提前结束方法

```java
public static void printDiv(int a, int b) {
        if (b == 0) {
            System.out.println("除数不能为0");
            return;//提前结束方法,否则除数为零程序错误     卫语言风格 
        }
        System.out.println(a / b);
    }
```

#### 自动类型转换

类型范围小的变量，可以直接赋值给类型范围大的变量。

```java
print(a);//自动类型转换
print2(a);//自动类型转换
......
public static void print(int b) {
        System.out.println(b);
    }
public static void print2(double c) {
        System.out.println(c);
    }
```

#### 强制类型转换
1.
```java
int i = 10;
//print3((i); //类型范围大的不能直接转换成类型范围小的

//强制类型转换,   类型 变量2 = (类型)变量1
byte d = (byte) i;//强制类型转换
print3(d);
......
public static void print3(byte d) {
        System.out.println(d);
    }
```

2. 
```java
int m = 1500;
byte n = (byte) m;        System.out.println(n);//-36,强转数据溢出！
System.out.println(m);
```
![alt text](image/屏幕截图%202026-05-23%20192221.png)

3. 
```java
double e = 10.5;
int f = (int) e;//浮点型强转整数，直接截取整数部分
System.out.println(f);//10
```

#### 表达式的自动类型提升

表达式的自动类型转换

在表达式中，小范围类型到达的变量，会自动转换成表达式中较大范围的类型，在参与运算

byte、short、char → int → long → float → double

* 表达式的最终结果类型由表达式中的最高类型决定。

```java
public static double calc(int a, int b,double c,char d) {
        //表达式的最终类型是由最高类型决定的
        return a+b+c+d;
    }
```

* 在表达式中byte、short、char 是直接转换成int类型参与运算的。

```java
public static int calc2(byte a, byte b) {
    //byte、short、char运算时自动提升为int运算
        return a + b;
    }
//或者强制转换
public static byte calc3(byte a, byte b) {
        byte c = (byte) (a + b);
        return c;//return (byte)(a+b);
    }
```

#### 输入输出

输出：把程序中的数据展示出来。
```java
System.out.println("Hello World!");
```

输入：程序读取用户键盘输入的数据。 通过Java提供的Scanner程序来实现

Scanner

1. 导包  
```
import java.util.Scanner;
```

![alt text](image/屏幕截图%202026-05-25%20151918.png)
勾选最下面两个，可以帮我们自动导包

2. 创建Scanner对象(抄写这一代码，得到Scanner扫描器对象)

```java
Scanner sc = new Scanner(System.in);
```

3. 等待接收用户输入数据

```java
System.out.println("请输入用户名：");
//让程序在这一行暂停，等待用户输入一个字符串名称，直到按了回车，把名字交给username记住再往下走
String username = sc.next();
System.out.println("用户名：" + username);
```

#### 基本算数运算符

|符号|作用|说明|
|----|----|----|
|+|加||
|-|减||
|*|乘||
|\|除|在Java中两个整数相处结果还是整数|
|%|取余|获取的是两个数据做除法的余数|

考虑数据类型

```java
System.out.println(a / b);//3
System.out.println((double) a / b);//3.3333333333...
System.out.println(1.0 * a / b);//3.3333333333...
```

“ + ”符号在有些情况下可以做连接符

“ + ”符号与字符运算符的时候是做连接符的，其结果依然是一个字符串。

能算则算，不能算则连接
```java
System.out.println(a + 'a' + "itheima");//107aitheima, 因为字符a是97，10+'a' = 107
```

#### 自增自减运算符

|符号|作用|
|----|----|
|自增：++|放在每个变量前面或后面，对变量自身的值加1|
|自减：--|放在某个变量前面或后面，对变量自身的值减一|

* ++、-- 如果在变量前后单独使用是没有区别的。
* ++、--如果不是单独使用（如在表达式中，或同时有其它操作），放在变量前后会存在明显区别

```java
public static void print2(int a) {
        int b = a++;//先用后加
        System.out.println(b);//10
        System.out.println(a);//11

        int c = ++a;//先加后用
        System.out.println(c);//12
        System.out.println(a);//12
    }
```

#### 赋值运算符

基本赋值运算符

“ = ”，从右往左看

扩展运算符

|符号|用法|作用|底层代码形式|
|----|----|----|----|
|+=|a+=b|加后赋值|a = (a的类型)（a+b）；|
|-=|a-=b|减后赋值|a = (a的类型)（a-b）；|
|*=|a*=b|乘后赋值|a = (a的类型)（a*b）；|
|/=|a/=b|除后赋值|a = (a的类型)（a/b）；|
|%=|a%=b|取余后赋值|a = (a的类型)（a%b）；|

```java
byte a1 = 10;
byte a2 = 20;
a1 += a2;//等价于a1 = （a1的类型）（a1 + a2）;扩展赋值运算符自带强制类型转换
System.out.println(a1);
```

```java
public static void demo() {
        int a = 10;
        a -= 5;//a =(a的类型)（a - 5）
        System.out.println(a);//5

        a *= 5;//a = （a的类型）（a * 5）
        System.out.println(a);//50

        a /= 5;//a = （a的类型）（a / 5）
        System.out.println(a);//2

        a %= 5;//a = （a的类型）（a % 5）
        System.out.println(a);//0
    }
```

 #### 关系运算符、三元关系运算符

 |符号|例子|作用|结果|
|----|----|----|----|
|>|a>b|判断a是否大于b|成立返回true，不成立返回false|
|>=|a>=b|判断a是否大于或等于b|成立返回true，不成立返回false|
|<|a<b|判断a是否小于b|成立返回true，不成立返回false|
|<|a<=b|判断a是否小于或等于b|成立返回true，不成立返回false|
|==|a==b|判断a是否等于b|成立返回true，不成立返回false|
|!=|a!=b|判断a是否不等于b|成立返回true，不成立返回false|

* 判断数据是否满足条件，最终会返回一个结果，这个结果是布尔类型的值：true或false

注意：在Java中判断是否相等一定是“ == ”

三元运算符

格式：条件表达式？值1：值2；

执行流程：首先计算关系表达式的值，如果为true，返回值1，如果为false，返回值2。

```java
//需求；求三个整数的最大值
public static int print(int a, int b, int c) {
        int max = a > b ? a : b;
        int max2 = max > c ? max : c;
        return max2;
    }
public static int print2(int a, int b, int c) {
        //三元运算符的嵌套
        int max = a > b ? (a > c ? a : c) : (b>c ? b : c);
        return max;
    }
```

#### 逻辑运算符

把多个条件放在一起运算，最终返回布尔类型的值：true、false

|符号|例子|作用|运算逻辑|
|----|----|----|----|
|&|逻辑与|2>1&3>2|多个条件必须都是true，结果才是true；有一个结果是false，结果就是false|
| \| |逻辑与|2>1 \| 3<5|多个条件只要有一个是true，结果就是true|
|！|逻辑非|！（2>1）|取反，你真我假，你假我真。！true false、！false == true|
|^|逻辑异或|2>1 ^ 3>1|前后条件的结果相同，返回true，前后条件的结果不同，才返回false|
|&&|短路与|2>10 && 3>2|判断结果与” & “一样，过程不同；左边为false，右边则不执行|
|\|\||短路或|2>1 \|\| 3>2|判断结果与” \| “一样，过程不同；左边为false，右边则不执行|


```java
//判断&&、||与&、|的区别
public static void print5() {
    int a = 111;
    int b = 2;
    //System.out.println(a > 1000 && ++b > 2);//false &&发现左边为false，则右边直接不执行
    System.out.println(a > 1000 & ++b > 2);//false &即使左边为false，右边也会执行
    System.out.println(b);

    int c = 10;
    int d = 20;
    //System.out.println(c > 6 || ++d > 1);//true ||发现左边为true，则右边直接不执行
    System.out.println(c > 6 | ++d > 1);//true |即使左边为true，右边也会执行
    System.out.println(d);
    }
```

实际开发中常用：&&、\|\|、！