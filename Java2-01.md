## 正则表达式
可以校验字符串是否满足一定的规则，并用来校验数据格式的合法性

作用一：校验字符串是否满足规则

* 字符类

|类型|说明|
|----|----|
|[abc]|只能式a,b或c|
|[^abc]|除了a,b,c之外的任何字符|
|[a-zA-Z]|a到z A到Z,包括(范围)|
|[a-d[m-p]]|a到d,或m到p|
|[a-z&&[def]]|a-z和def的交集.为:d,e,f|
|[a-z&&[^bc]]|a-z和非bc的交集.(等同于[ad-z])|
|[a-z&&[^m-p]]|a-z和除m到p的交集 (等同于[a-lq-z])|

* 预定义字符（只匹配一个字符）

|类型|说明|
|----|----|
|.|任何字符|
|\d|一个数字:[0-9]|
|\D|非数字:[^0-9]|
|\s|一个空白数字:[\t\n\x0B\f\r]|
|\S|非空白字符:[^\s]|
|\w|[a-zA-Z_0-9]英文、数字、下划线|
|\W|[^\w]一个非单词字符|

* 数量词

|类型|说明|
|----|----|
|X?|X,一次或0次|
|X*|X,零次或多次|
|X+|X,一次或多次|
|X{n}|X,正好n次|
|X{n,}|X,至少n次|
|X{n,m}|X,至少n次但不超过m次|

作用二：在一段文本中查找满足要求的内容

爬虫：可以爬起网络上的或本地的

Pattern：表示正则表达式

Matcher：文本匹配器，作用按照正则表达式的规则去读取字符串，从头开始读取，在数据中去找符号匹配规则的子串

```java
//获取正则表达式对象
String str = "还大茶壶发挥发挥放大Java啊达瓦让打打分Java22，达瓦发放给";

Pattern p = Pattern.compile("Java\\d{0,2}");

//获取文本匹配器对象
Matcher m = p.matcher(str);

//拿着文本匹配器对象从头开始读取，寻找是否有满足规则的子串
//没有返回false
//如果有返回true，在底层记录子串的起始索引和结束索引+1
boolean b = m.find();

//方法底层会根据find方法记录的索引进行字符串的截取
//substring(start,end)，包头不包尾
String s = m.group();
System.out.println(s);
```

改进

```java
//获取正则表达式对象
Pattern p = Pattern.compile("Java\\d{0,2}");

//获取文本匹配器对象
 Matcher m = p.matcher(str);

//循环
while (m.find()) {
        System.out.println(m.group());
}
```

### 带条件爬取、贪婪爬取和识别正则的两个方法

```java
//需求1：爬取版本号为8，11，17的Java，但是不显示版本号
        //需求2：爬取版本号为8，11，17的Java，但是显示版本号
        //需求3：爬取除了版本号为8，11，17的Java

        String s = "大海的哈佛java8和外地啊Java2啊大海JAva11放大hi法Java4海的JAVa17话上帝啊汉武帝1极大激发i无Java17法口味都到位大家";

        //1，定义正则表达式
        //理解为前面的数据Java
        //=表示Java后面要跟随的数据
        //但是在获取的时候，只获取前半部分
        //(?i)表示不区分大小写
        //需求1：
        String regex1 = "((?i)Java)(?=8|11|17)";
        //需求2：
        String regex2 = "((?i)Java)(8|11|17)";
        String regex3 = "((?i)Java)(?:8|11|17)";
        //需求3：
        String regex4 = "((?i)Java)(?!8|11|17)";

        Pattern pattern = Pattern.compile(regex3);
        Matcher matcher = pattern.matcher(s);
        while (matcher.find()) {
            System.out.println(matcher.group());
        }
```

只写+和*表示贪婪匹配

+?  非贪婪匹配

*?  非贪婪匹配

贪婪爬取:在爬取数据的时候尽可能的多获取数据

非贪婪爬取：在爬取数据的时候尽可能的少获取数据

ab+:

贪婪爬取：abbbbbbbbbbbbbbbbbbbb

非贪婪爬取：ab

在Java中，默认的就是贪婪爬取

如果我们在数量词+ * 的后面加上问号，那么此时就是非贪婪爬取

```java
public static void main(String[] args) {
        String s = "abbbbbbbbbbbbbbbbbbbb大海的哈佛java8和外地啊Java2啊大海JAva11放大hi法Java4海的JAVa17话上帝啊汉武帝1极大激发i无Java17法口味都到位大家";

        String regex = "ab+?";

        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(s);
        while (matcher.find()) {
            System.out.println(matcher.group());
        }

    }
```

