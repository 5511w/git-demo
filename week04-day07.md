### 数组
数组是一个数据容器，可以用来存储一批同类型的数据
#### 静态初始化数组
数据类型[] 数组名 = {元素1,元素2,元素3,....}

```java
String[] names = {"小王","小李","小张","小赵","小钱","小孙","小李"};
```

#### 完整格式
数据类型[] 数组名 = new 数据类型[] {元素1,元素2,元素3,....}
```java
String[] names = new String[]{"小王","小李","小张","小赵","小钱","小孙","小李"};
```

```java
String names[] = new String[]{"小王","小李","小张","小赵","小钱","小孙","小李"};
```

#### 数组的访问
数组名[索引]
```
System.out.println(arr[0]);

System.out.println(arr[12]);
```

arr[1] = 100;

#### 获取数组的长度
```
System.out.println(name.length);
```

索引  0 1 2 3 4....

获取一个索引值

```java
//Math.random(): [0-1) 之间的小数
//names.length: 数组中元素的个数
int index = (int)(Math.random()*names.length);
//根据索引值获取对应的元素：数组名称[索引值]
String name = names[index];
```

#### 动态初始化数组
定义数组是先不存入具体的元素值，只确定数组存储的数据类型和数组的长度

数据类型 [] 数组名 = new 数据类型[长度];

默认规则：

|数据类型|明细|默认值|
|----|----|----|
|基本类型|byte、short、char、int、long|0|
||float、double|0.0|
||boolean|false|
|引用类型|类、接口、String|null|

#### 数据的遍历
一个一个数据的访问

通过遍历索引访问

```java
int[] arr = {20,30,40,50};
for (int i = 0;i < arr.length; i++){
    System.out.println(arr[i]);
}
```

```java
让index1和index2进行交换
//1.创建一个临时变量，将index2处的牌暂存起来
String temp = cards[index2];
//2.将index1索引位置的数据，存入index2索引位置
cards[index2] = cards[index1];
//3.将临时变量，赋值给index1索引位置
cards[index1] = temp;
```

#### 二维数组
数组中的每个元素都是一个一维数组

1. 静态初始化

数据类型[][] 数组名 =new 数据类型[][] {元素1,元素2,元素3,....};

2. 动态初始化

数据类型[][] 数组名 =new 数据类型[长度1][长度2]；

#### 二维数组的访问
数组名称[行索引];

```java
//访问：数组名[索引行]
String[] name1 = names[2];
for (int i = 0; i < name1.length; i++) {
    System.out.print(name1[i]);
}
 
//访问2：数组名[索引行][索引列]
System.out.println(names[2][3]);
System.out.println(names[3][1]);

//长度访问：数组名.length
System.out.println(names.length);//行数
System.out.println(names[0].length);//第一行几个
```

#### 遍历二维数组

```java
for (int i = 0; i < names.length; i++) {
    String[] name = names[i];
    //遍历一维数组
    for (int j = 0; j < name.length; j++) {
        System.out.print(name[j]+" ");
    }
    System.out.println();
}

//另一种遍历方式
for (int i = 0; i < names.length; i++) {
    for (int j = 0; j < names[i].length; j++) {
        System.out.print(names[i][j]+" ");
    }
    System.out.println();
}
```

#### 打乱二维数组

```java
//打乱二维数组中的元素
for (int i = 0; i < array.length; i++) {
    for (int j = 0; j < array[i].length; j++) {
        //遍历到当前位置：array[i][j]
        
        //随机的索引位置处：m(随机的行)，p(随机的列)
        int m = (int)(Math.random() * array.length);
        int p = (int)(Math.random() * array.length);

        //定义一个临时变量，保存当前位置的元素
        int temp = array[m][p];
        //把i j 位置处的数据赋给m p 索引位置处
        array[m][p] = array[i][j];
        //把临时变量保存的元素赋给i j 索引位置处
        array[i][j] = temp;
    }
}
```