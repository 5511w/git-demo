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

### 分组

```javq
String regex1 = "\\w+@[\\w&&[^_]]{2,6}(\\.[a-zA-Z]{2,3}){1,2}";

        String regex2 = "[1-9]\\d{16}(\\d|X|x)";

        String regex3 = "([01]\\d|2[0-3]):[0-5]\\d:[0-5]\\d";

        String regex4 = "([01]\\d|2[0-3])(:[0-5]\\d){2}";

        //每组是有组号，也就是序号
        //规则1：从1开始，连续不间断
        //规则2：以左括号为基准，最左边的是第一组，其次为第二组，以此类推
```

* 捕获分组

```java
//需求1：判断一个字符串的开始字符和结束字符是否一致？只考虑一个字符
        //举例 a431a n2134n c1234c $dar#
        //   \\组号：表示把第x组的内容再拿出来用一次
        String regex1 = "(.).+\\1";
        System.out.println("a431a".matches(regex1));
        System.out.println("n2134n".matches(regex1));
        System.out.println("c1234c".matches(regex1));
        System.out.println("$dar#".matches(regex1));

        //需求2：判断一个字符串的开始字符和结束字符是否一致？可以有多个字符
        //举例: ab431ab n2134n c?@1234c?@ $@dar$!
        String regex2 = "(.+).+\\1";
        System.out.println("ab431ab".matches(regex2));
        System.out.println("n2134n".matches(regex2));
        System.out.println("c?@1234c?@".matches(regex2));
        System.out.println("$@dar$!".matches(regex2));
        
        //需求3：判断一个字符串的开始字符和结束字符是否一致？开始部分内部每个字符也需要一致
        //举例:aaa123aaa nnn13545nnn 111324111 &&sfd&&

        // (.)：把首字母看作一行
        //  \\2 把首字母拿出来再次使用
        //  * :作用与\\2，表示后面重复的内容出现0次或多次
        String regex3 = "((.)\\2*).+\\1";
        System.out.println("aaa123aaa".matches(regex3));
        System.out.println("nnn13545nnn".matches(regex3));
        System.out.println("111324111".matches(regex3));
        System.out.println("&&sfd&&".matches(regex3));
```

后续还要继续使用本组的数据

正则内部使用：\\组号

正则外部使用：$组号

* 非捕获分组

分组之后不需要再使用本组数据，仅仅是把数据括起来

```Java
//身份证号：420105199003048888

        //这里\\1报错的原因：（?:）就是非捕获分组，此时是不占用组号的
        //(?:) (?=) (?!)
        //更多是使用第一个
        String regex4 = "[1-9]\\d{16}(?:\\d|X|x)\\1";
        String regex5 = "[1-9]\\d{16}(?\\d|X|x)\\1";

        System.out.println("420105199003048888".matches(regex4));
```

