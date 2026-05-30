## 程序流程控制
### 顺序结构

自上而下的执行程序

### 分支结构

根据条件，选择对应的代码执行

#### if

根据条件的真或假，来决定某段代码

if三种写法

1.
 ```java
if (条件表达式){    //if(条件){}后不能跟“;”，否则{}中的代码将不受if控制
    代码；
}
//如果if语句中只有一行代码，那么大括号可以省略
```

执行流程：首先判断条件表达式的结果，如果为true执行语句体，为false就不执行语句体

2. 
```java
if (条件表达式){
    代码1;
}else{
    代码2;
}
```

执行流程：首先判断条件表达式的结果，如果为true执行语句体1，为false就执行语句体2。

3. 
```java
if (条件表达式){
    代码1;
}else if(条件表达式){
    代码2;
}else if(条件表达式){
    代码3;
}
...
else{
    代码n;
}
```

执行流程：首先判断条件1的结果，如果为true执行语句体1，如果为false则判断条件2的值。

如果值为true就执行语句体2，分支结束，如果为false则判断条件3的值。

....

如果没有任何条件为true，就执行else分支的语句体n。

#### switch
通过比较值是否相等，来决定执行那条分支

```java
switch(表达式){
    case 值1:
    执行代码...;
    break;
    case 值2:
    执行代码...;
    break;
    .....
    default:
        执行代码n;
}
```
执行流程：

1. 先执行表达式的值，再拿着这个值去与case后的值进行匹配
2. 与那个case后的值匹配为true就执行那个case的代码，遇到break就跳出switch分支
3. 如果全部case后的值与之匹配都是false，则执行default块的代码

if在功能上远大于switch

当前条件是区间的时候，建议使用if分支结构实现

当条件是与一个一个值比较的时候，建议用switch更合适

##### switch的注意事项、穿透性
表达式类型只能是byte、short、int、char,JDK5开始支持枚举类型，JDK7开始支持string，不支持float、double、long

case给出的值不允许重复，且只能是字面量，不能是变量

正常使用switch的时候，不要忘记break，否则会穿透，执行所有case后面的代码

穿透性的作用：想通程序的case块，可以通过穿透性进行合并，从而减少重复代码的书写

```java
public static void printInfo2() {
        String day = "周一";
        switch (day) {
            case "周一":
                System.out.println("埋头苦干，解决bug");
                break;
            case "周二":
            case "周三":
            case "周四":
                System.out.println("请求大牛程序员帮忙");
                break;
            case "周五":
                System.out.println("自己整理代码");
                break;
            case "周六":
            case "周天":
                System.out.println("打游戏");
                break;
            default:
                System.out.println("没有匹配的");
                break;
        }
    }
```

### 循环结构

控制某段代码重复的执行

#### for
控制一段代码反复执行很多次

格式

```java
for (初始化语句;循环条件;迭代语句){
    循环体语句(重复执行的代码);
}
```


#### while

```java
初始化语句;
while(循环条件){
    循环体语句(被重复执行的代码);
    迭代语句;
}
```

##### 什么时候用for，什么时候有while

功能上是完全一样的，for能解决的while也能解决，反之亦然

知到循环几次：使用for；不知道循环几次：使用while

#### do-while
```java
初始化语句
do{
    循环体语句;
    迭代语句;
}while(循环条件);
```

#### 三种循环小结
for循环 和 while循环（先判断后执行）；do....while（先执行后判断）

for循环和while循环的执行流程是一样的，功能上无区别

知到循环几次：使用for；不知道循环几次：使用while

for循环中，控制循环变量只能在循环之中使用。while循环中，控制循环的变量在循环后还可以继续使用。

```java
for (int i = 0; i < 3; i++){
            System.out.println("hello world");
}
System.out.println(i);//报错


int i = 1;
        while (i < 3) {
            System.out.println("hello world");
            i++;
        }
System.out.println(i);
```

#### 死循环

可以一直执行下去的一种循环，如果没有干预不会停下来

```java
public static void test1() {
        //1.for循环实现
        for (;;) {
            System.out.println("hello world");
        }

        //2.while循环实现死循环（经典写法）
        while (true) {
            System.out.println("hello world");
        }

        //3.do...while循环实现死循环
        do {
            System.out.println("hello world");
        } while (true);
    }
```

#### 循环嵌套

循环中有包含循环

外部循环每循环一次，内部循环会全部执行完一轮

```java
//九九乘法表
public static void test2() {
        //1.定义一个循环控制打印几行
        for (int i = 1; i <=9 ; i++) {
            //2.定义一个循环控制打印当前行的列
            for (int j = 1; j <=i ; j++) {
                //3.打印每列信息
                System.out.print(j+"*"+i+"="+(i*j)+"\t");//不换行，去ln  空格
            }
            System.out.println();//每一列换行
        }
    }   
```

#### break、continue
break：跳出并结束当前所在循环的执行

只能用于结束所在循环，或者结束所在switch分支的执行

continue：用于跳出当前循环的当次执行，直接进入循环的下一次执行

只能在循环中进行使用

```java
public static void test1(){
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                break;//跳出并结束当前循环
            }
            System.out.println(i);
        }
    }
    public static void test2(){
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                continue;//跳出本次循环，继续下一次循环
            }
            System.out.println(i);
        }
    }
```