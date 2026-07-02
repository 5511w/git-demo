### Set集合
无序、不重复、无索引

* Set要用到的常用方法，基本上就是Collection提供的，自己几乎没有额外新增

#### HashSet
无序、不重复、无索引
##### HashSet集合的底层原理
基于哈希表存储数据

哈希值
* 就是一个int类型的随机值，java中每个对象都有一个哈希值
* Java中的所有对象，都可以调用Object类提供的hashCode方法，返回该对象自己的哈希值  public int hashCode();

对象哈希值特点
* 同一个对象多次调用hashCode方法返回的哈希值是相同的
* 不同对象，它们的哈希值大概率不相等，但也有可能会相等（哈希碰撞）
    int(-21亿多~21亿多)  如果有45亿个对象就会出现相等的情况

哈希表
* JDK8之前，哈希表=数组+链表
    ① 创建一个默认长度16的数组，默认加载因子为0.75（扩容：16×0.75=12），数组名table
    ② 使用元素的哈希值对数组的长度做运算计算出应存入的位置
    ③ 判断当前位置是否为null，如果是null直接存入
    ④ 如果不为null，表示有元素，则调用equals方法比较,相等，则不存；不相等，则存入数据
        JDK8之前，新元素存入数组，占老元素位置，老元素挂下面

        JDK8开始之后，新元素直接挂在老元素下面

* JDK8开始，哈希表=数组+链表+红黑树
    JDK8开始，当链表长度超过8，且数组长度》=64时，自动将链表转成红黑树（自平衡二叉树）
* 哈希表是一种增删改查数据，性能都非常好的数据结构

###### HashSet集合元素的去重操作

```java
@Override
public boolean equals(Object o) {
    //1.如果自己和自己比，则返回true
    if (this == o) return true;
    //2.如果o为null或者o的类和当前类不一致，则返回false
    if (o == null || this.getClass() != o.getClass()) return false;
    //3.比较两个对象的内容是否相同

    //3.1强转成Student对象
    Student student = (Student) o;
    return age == student.age && Objects.equals(name, student.name) && Objects.equals(address, student.address) && Objects.equals(phone, student.phone);
}

@Override
public int hashCode() {
    //包装了不同学生对象的属性值，如果属性值相同，则返回相同的hashCode值
    return Objects.hash(name, age, address, phone);
}
```

结论：如果希望Set集合认为2个内容一样的对象是重复的，必须重写对象的hashCode()和equals()方法

#### LinkHashSet
有序、不重复、无索引

##### LinkHashSet的底层原理
依然时基于哈希表（数组、链表、红黑树）实现的

但是，它的每个元素都额外的多了一个双链表机制记录它前后元素的位置


#### TreeSet
排序（默认大小升序排序）、不重复、无索引

底层基于红黑树实现的排序

注意：
* 对于数值类型：Integer，Double，默认按照数值本身的大小进行排序
* 对于字符串类型：默认按首字母的编号升序排序
* 对于自定义类型如Student对象，TreeSet默认是无法直接排序的

解决的两种方案：

1.让Teacher类实现Comparable比较接口，并重写compareTo方法，指定大小比规则

```java
//t2.compareTo(t1)
//t2 == this 比较者
//t1 == o 被比较者
//规定1：如果你认为左边大于右边，则返回正整数
//规定2：如果你认为左边小于右边，则返回负整数
//规定3：如果你认为两边相等，则返回0
@Override
    public int compareTo(Teacher o) {
        //按照年龄升序排列
//        if (this.age > o.age) {
//            return 1;
//        }
//        if (this.age < o.age) {
//            return -1;
//        }
//        return 0;

        return this.age - o.age;//升序
        //return o.age - this.age;//降序
    }
```
2.public TreeSet(Comparator c)集合自带的比较器Comparator对象，指定比较规则

```java
...
Set<Teacher> set = new TreeSet<>(new Comparator<Teacher>() {
            @Override
            public int compare(Teacher o1, Teacher o2) {
                //return o2.getAge() - o1.getAge();//降序
//                if(o1.getSalary() > o2.getSalary()){
//                    return 1;
//                }else if(o1.getSalary() < o2.getSalary()){
//                    return -1;
//                }else
//                    return 0;
                return Double.compare(o1.getSalary(), o2.getSalary());//薪水升序
                //return Double.compare(o2.getSalary(), o1.getSalary());//薪水降序
            }
        });

        //简化形式
//        Set<Teacher> set = new TreeSet<>((o1, o2) -> Double.compare(o1.getSalary(), o2.getSalary()));
```

* 如果类本身有实现Comparable接口，TreeSet集合同时也自带比较器，默认使用集合自带的比较器排序

### Map集合
* Map集合也被叫做“键值对集合”，格式：{key1=value1,key2=value2,key3=value3...}
* Map集合的所有键是不允许重复的，但值可以重复，键和值是一一对应的，每一个键只能找到自己对应的值

存储一一对应的数据

Map系列集合的**特点都是由键决定**的，值只是一个附属品，值是不做要求的

Map集合的常用方法

```java
public static void main(String[] args) {
        //目标：掌握Map的常用方法
        Map<String, Integer> map = new HashMap<>();
        map.put("小王", 18);
        map.put("小xi", 19);
        map.put("小王", 28);
        map.put("小h", 20);
        map.put("小m", 18);
        map.put(null,null);
        System.out.println(map);//{null=null, 小h=20, 小m=18, 小王=28, 小xi=19}

        //写代码演示Map常用方法
        System.out.println(map.get("小王"));// 根据键获取值 28
        System.out.println(map.containsKey("小王"));// 判断键是否存在 true
        System.out.println(map.containsValue(18));// 判断值是否存在 true
        System.out.println(map.size());// 获取键值对个数 5
        System.out.println(map.isEmpty());// 判断集合是否为空 false
        System.out.println(map.remove("小王"));// 删除键值对并返回值 28
        System.out.println(map.remove("小王", 28));// 删除键值对并返回结果 true
//        map.clear();// 清空集合
//        System.out.println(map);
        System.out.println(map.values());// 获取所有的值
        System.out.println(map.entrySet());// 获取所有的键值对对象

        //获取所有的键放到一个Set集合返回给我们
        Set<String> keys = map.keySet();
        for (String key : keys) {
            System.out.println(key);
        }

        //获取所有值放到一个Collection集合返回给我们
        Collection<Integer> values = map.values();
        for (Integer value : values) {
            System.out.println(value);
        }
    }
```

Map集合的遍历方式

1. 键找值
    ```java
    ...
    //1提起Map集合的全部键到一个Set集合中
        Set<String> keys = map.keySet();

    //2.遍历Set集合，得到每一个键
    for (String key : keys) {
        //3.根据键获取对应的值
        Integer value = map.get(key);
        System.out.println(key + "=" + value);
    }
    ```

2. 键值对
    ```java
    //1.把Map集合转换成Set集合，里面的元素类型都是键值对类型（Map.Entry<String,Integer>）
    /**
    *  map = {小h=20, 小m=18, 小王=28, 小xi=19}
    *  map.entrySet()
    *  Set<Map.Entry<String,Integer>> entrySet = [(小h=20), (小m=18), (小王=28), (小xi=19)]
    */
    Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
    //2.遍历Set集合，得到每一个键值对类型元素
    for (Map.Entry<String, Integer> entry : entrySet) {
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.println(key + "=" + value);
    }
    ```

3. Lambda表达式
    ```java
    //1,直接调用集合的forEach方法完成遍历
//        map.forEach(new BiConsumer<String, Integer>() {
//            @Override
//            public void accept(String key, Integer value) {
//                System.out.println(key + "=" + value);
//            }
//        });

    map.forEach((k,v) -> System.out.println(k + "=" + v));
    ```
#### Map集合的实现类
##### HashMap
无序、不重复、无索引

底层原理

实际上：原来学的Set系列集合的底层就是基于Map实现的，只是Set集合中的元素只要键值，不要数据而已

HashMap很HashSet的底层原理式一样的，都是基于哈希表实现的

##### LinkedHashMap
有序、不重复、无索引

底层原理

来学的LinkedHashSet集合的底层原理就是LinkedHashMap
* 底层数据结构依然式基于哈希表实现，只是每个键值对元素又额外多了一个双链表的机制记录元素顺序（保证有序）



##### TreeMap
按照大小默认升序排序、不重复、无索引

底层原理

跟TreeSet集合一样，都是基于红黑树实现的排序

TreeMap集合同样也支持两种方式来指定排序规则

* 让类实现Comparable接口，重写比较规则
* TreeMap集合又一个有参数构造器，支持创建Comparator比较器对象，以便用来指定比较规则

### Stream流
是JDK8开始新增的一套API（java.util.stream.*），可以用于操作集合或数组的数据

优势：Stream流大量的结合了Lambda的语法风格来编程，功能强大，性能高效，代码简洁，可读性好

#### Stream流的使用步骤

![alt test](/image/屏幕截图%202026-07-02%20145254.png)

1. 获取Stream流

```java
    //1.获取集合的stream流，调用集合提供的stream()方法
    Collection<String> list = new ArrayList<>();
    Stream<String> s1 = list.stream();

    //2.Map集合，怎么拿stream流
    Map<String,String> map = new HashMap<>();
    //获取键流
    Stream<String> s2 = map.keySet().stream();
    //获取值流
    Stream<String> s3 = map.values().stream();
    //获取键值对流
    Stream<Map.Entry<String, String>> s4 = map.entrySet().stream();

    //3.数组，怎么获取stream流
    String[] arr = {"张三","李四","王五"};
    Stream<String> s5 = Arrays.stream(arr);
    System.out.println(s5.count());//3
    Stream<String> s6 = Stream.of(arr);
    Stream<String> s7 = Stream.of("张三","李四","王五");
    //Stream.of(T...values)可变参数
```

2. Stream流的常用中间方法

* 中间方法指的是调用完成后会返回新的Stream流，可以继续使用（支持链式编程）

![alt test](/image/屏幕截图%202026-07-02%20150338.png)

```java
public static void main(String[] args) {
        //目标：掌握stream流中的常用方法，对流上的数据进行封装（返回新值，支持链式编程）
        List<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("周芷若");
        list.add("赵敏");
        list.add("张强");
        list.add("张三丰");
        list.add("张小四");

        //1.过滤方法
        list.stream().filter(str -> str.startsWith("张")&& str.length()==3).forEach(System.out::println);

        //2.排序方法
        List<Double> scores = new ArrayList<>();
        scores.add(22.1);
        scores.add(33.2);
        scores.add(33.2);
        scores.add(44.3);
        scores.add(55.4);

        scores.stream().sorted().forEach(System.out::println);//默认升序

        scores.stream().sorted((s1,s2)->Double.compare(s2, s1)).forEach(System.out::println);//降序
        scores.stream().sorted((s1,s2)->Double.compare(s2, s1)).limit(2).forEach(System.out::println);//降序，只要前两名
        scores.stream().sorted((s1,s2)->Double.compare(s2, s1)).skip(2).forEach(System.out::println);//降序,跳过前两名

        //如果希望自定义对象能够去重复，要重写equals和hashCode方法，才可以
        scores.stream().sorted((s1,s2)->Double.compare(s2, s1)).distinct().forEach(System.out::println);//降序,去重

        //3.映射/加工方法，把流上原来的数据拿出来变成新数据又放到流上去
        scores.stream().map(score -> "加十分"+(score+10)).forEach(System.out::println);

        //4.合并流
        Stream<String> s1 = Stream.of("张无忌","周芷若","赵敏","张强","张三丰","张小四");
        Stream<Integer> s2 = Stream.of(1,2,3,4,5,6);
        Stream<Object> s3 = Stream.concat(s1,s2);
        System.out.println(s3.count());
    }
```

3. Stream流的终结方法
* 终结方法指的是调用完成后，不会返回新Stream了，没法继续用流了

![alt test](/image/屏幕截图%202026-07-02%20160708.png)

```java
//获取薪水高于6000的
teachers.stream().filter(t -> t.getSalary() > 6000.0).forEach(System.out::println);
//获取薪水高于6000教师数量
long count = teachers.stream().filter(t -> t.getSalary() > 6000.0).count();
System.out.println(count);
//获取薪水最高的老师对象
Optional<Teacher> max = teachers.stream().max((t1, t2) -> Double.compare(t1.getSalary(), t2.getSalary()));
Teacher maxTeacher = max.get();//获取Optional对象中的数据
System.out.println(maxTeacher);
//获取薪水最低的老师对象
Optional<Teacher> min = teachers.stream().min((t1, t2) -> Double.compare(t1.getSalary(), t2.getSalary()));
Teacher minTeacher = min.get();//获取Optional对象中的数据

```

#### 收集Stream流
就是把Stream流操作后的结果转回到集合或数组中去返回
* Stream流：方便操作集合/数组的手段；集合/数组：才是开发中的目的

```java
List<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("张无忌");
        list.add("周芷若");
        list.add("赵敏");
        list.add("张强");
        list.add("张三丰");
        list.add("张小四");

        //流只能收集一次

        //收集到集合或数组中
        Stream<String> s1 = list.stream().filter(s-> s.startsWith("张"));
        //收集到list集合
        List<String> newList = s1.collect(Collectors.toList());
        System.out.println(newList);

//        Set<String> set2 = new HashSet<>();
//        set2.addAll(list);

        //收集到Set集合
        Stream<String> s2 = list.stream().filter(s-> s.startsWith("张"));
        Set<String> newSet = s2.collect(Collectors.toSet());
        System.out.println(newSet);

        //收集到数组中
        Stream<String> s3 = list.stream().filter(s-> s.startsWith("张"));
        Object[] array = s3.toArray();
        System.out.println(Arrays.toString(array));

        System.out.println("--------------------");

        //收集到Map集合中
        //收集到Map集合中，键是老师名，值是老师薪水
//        Map<String, Double> map = teachers.stream().collect(Collectors.toMap(Teacher::getName, Teacher::getSalary));
        Map<String, Double> map = teachers.stream().collect(Collectors.toMap(new Function<Teacher, String>() {
            @Override
            public String apply(Teacher teacher) {
                return teacher.getName();
            }
        }, new Function<Teacher, Double>() {
            @Override
            public Double apply(Teacher teacher) {
                return teacher.getSalary();
            }
        }));
        System.out.println(map);
```

### 方法中的可变参数
就是一种特殊的形参，定义在方法、构造器的形参列表里，格式是：数据类型...参数名称;

特点和好处
* 特点：可以不传数据给它；可以传一个或者同时传多个数据给它；也可以传一个数组给它
* 好处：常常用来灵活接收数据

注意事项：可变参数在形参列表中只能有一个，多个参数只能放在形参列表的最后面

```java
public static void show(int... nums){
        //内部怎么拿到数据
        //可变参数对内实际上就是一个数组，nums就是数组
        System.out.println(nums.length);
        System.out.println(Arrays.toString(nums));
        System.out.println("===========");
    }
```

### Cllections工具类
是一个用来操作集合的工具类

![alt test](/image/屏幕截图%202026-07-02%20163200.png)

```java
public class CollectionsDemo2 {
    public static void main(String[] args) {
        //目标：Collections工具类
        List<String> list = new ArrayList<>();
//        list.add("张无忌");
//        list.add("周芷若");
//        list.add("赵敏");
//        list.add("张强");
//        list.add("张三丰");
//        list.add("张小四");
        //1.Collections的方法批量加
        Collections.addAll(list,"张无忌","周芷若","赵敏","张强","张三丰","张小四");
        System.out.println(list);

        //2.打乱顺序
        Collections.shuffle(list);
        System.out.println(list);
    }
}
```
![alt test](/image/屏幕截图%202026-07-02%20163546.png)


