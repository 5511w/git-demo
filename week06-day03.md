### 单元测试
就是针对最小的功能单元：方法，编写测试代码对其进行正确性测试

之前是如何进行单元测试的？有啥问题？
* 只能在main方法编写测试代码，去调用其他方法进行测试
* 无法实现自动化测试，一个方法测试失败，可能影响其他方法的测试
* 无法得到测试的报告，需要程序员自己去观察测试是否成功

#### Junit单元测试框架
可以用来对方法进行测试，它是第三方公司开源出来的（很多开发工具已经集成了Junit框架，比如IDEA）

优点
* 可以灵活的编写测试代码，可以针对某个方法执行测试，也支持一键完成对全部方法的自动化测试，且各自独立
* 不需要程序员去分析测试的结果，回自动生成测试报告出来

#### Junit单元测试的使用步骤

1. 将Junit框架的jar包导入项目中（注意IDEA集成了Junit框架，不需要自己手动导包）
2. 为需要测试的业务类，定义对应的测试类，并为每个业务方法，编写对应的测试方法（必须：公共、无参、无返回值）
3. 测试方法上必须声明@Test注解，然后在测试方法中，编写代码调用被测试的业务方法进行测试
4. 开始测试：选中测试方法，右键选择“Junit运行”，如果测试通过则是绿色；如果测试失败，则是红色
```java
/**
     * 求字符串的最大索引
     */
    public static int getMaxIndex(String data){
        if(data == null || "".equals(data)) {
            return -1;
        }
        return data.length();
    }
```
```java
@Test
    public void testGetMaxIndex(){
        //测试步骤
        int index = StringUtil.getMaxIndex("abc");//3
        //测试用例
        int index1 = StringUtil.getMaxIndex("");
        int index2 = StringUtil.getMaxIndex(null);

        System.out.println(index);
        System.out.println(index1);
        System.out.println(index2);

        //断言测试：测试结果与预期结果是否一致
        Assert.assertEquals("本轮测试失败，业务获取的最大索引有问题",2, index);
        Assert.assertEquals("本轮测试失败，业务获取的最大索引有问题",-1, index1);
        Assert.assertEquals("本轮测试失败，业务获取的最大索引有问题",-1, index2);
    }s
```

### 反射
反射就是：加载类，并允许以编程的方式解剖类中的各种成分（成员变量、方法、构造器等）

_反射具体学什么？_

1. 反射第一步：加载类，获取类的字节码：Class对象
    获取Class对象的三种方式
    * Class c1 = 类名.class
    * 调用Class提供的方法：public static Class forName(String package);
    * Object提供的方法：public Class getClass(); Class c3 = 对象.getClass();

```java
public static void main(String[] args) throws Exception {
        //目标：掌握反射第一步操作，获得类的class对象（获取类本身）
        //1.获取类本身：类.class
        Class c1 = Student.class;
        System.out.println(c1.getName());//类的全名 com.xixi.demo2reflect.Student
        System.out.println(c1.getSimpleName());//类名 Student
        System.out.println(c1);
        //2.获取类本身：Class.forName("类的全类名")
        Class c2 = Class.forName("com.xixi.demo2reflect.Student");
        System.out.println(c2);

        //3.获取类本身：对象，getClass()
        Student s =new Student();
        Class c3 = s.getClass();
        System.out.println(c3);

        System.out.println(c1 == c2);
        System.out.println(c2 == c3);
    }
```

2. 获取类的构造器：Constructor对象

获取构造器的方法：

|方法|说明|
|----|----|
|Constructor<?>[] getConstructors​()|获取全部构造器（只能获取public修饰的）|
|Constructor<?>[] getDeclaredConstructors​()|获取全部构造器（只要存在就能拿到）|
|Constructor<T> getConstructor​(Class<?>... parameterTypes)|获取某个构造器（只能获取public修饰的）|
|Constructor<T> getDeclaredConstructor​(Class<?>... parameterTypes)|获取某个构造器（只要存在就能拿到）|

获取构造器的作用依然是创建对象

|Constructor提供的方法|说明|
|----|----|
|T newInstance​(Object... initargs)|调用此构造器对象表示的构造器，并传入参数，完成对象的初始化并返回|
|public void  setAccessible(boolean flag)|设置为true，表示禁止检查访问控制（暴力反射）|

```java
@Test
    public void getConstructor() throws Exception {
        //目标：获取类的构造器对象并对其进行操作
        //1.反射第一步：获得Class对象，代表拿到类
        Class c1 = Dog.class;
        //2.获取类的构造器对象
        Constructor[] cons = c1.getDeclaredConstructors();
        for (Constructor con : cons) {
            System.out.println(con.getName()+"("+con.getParameterCount()+")");
        }
        //3.获取单个构造器
        Constructor con = c1.getDeclaredConstructor();//无参构造器
        System.out.println(con.getName()+"("+con.getParameterCount()+")");

        Constructor con2 = c1.getDeclaredConstructor(String.class,int.class);//2个参数 有参构造器
        System.out.println(con2.getName()+"("+con2.getParameterCount()+")");

        //4.获取构造器的作用依然是创建对象
        //暴力反射，暴力反射可以访问私有的构造器、方法、成员变量
        con.setAccessible(true);//绕过访问权限，直接访问
        Dog d1 = (Dog) con.newInstance();
        System.out.println(d1);

        Dog d2 = (Dog) con2.newInstance("小黑", 10);
        System.out.println(d2);

    }
```
3. 获取类的成员变量：Field对象

Class提供了从类中获取成员变量的方法

|方法|说明|
|----|----|
|public Field[] getFields​()|获取类的全部成员变量（只能获取public修饰的）|
|public Field[] getDeclaredFields​()|获取类的全部成员变量（只要存在就能拿到）|
|public Field getField​(String name)|获取类的某个成员变量（只能获取public修饰的）|
|public Field getDeclaredField​(String name)|获取类的某个成员变量|

获取成员变量的目的依然是赋值和取值

|方法|说明|
|----|----|
|void set​(Object obj, Object value)|赋值|
|Object get​(Object obj)|取值|
|public void  setAccessible(boolean flag)|设置为true，表示禁止检查访问控制（暴力反射）|

```java
@Test
    public void getField() throws Exception {
        //目标：获取类的成员变量对象并对其进行操作
        //1.反射第一步：获得Class对象，代表拿到类
        Class c1 = Dog.class;
        //2.获取成员变量对象
        Field[] fields = c1.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(field.getName());
        }

        //3.获取单个成员变量对象
        Field field = c1.getDeclaredField("hobby");
        System.out.println(field.getName()+"("+field.getType().getName()+")");

        Field field2 = c1.getDeclaredField("name");
        System.out.println(field2.getName()+"("+field2.getType().getName()+")");

        //4.获取成员变量的目的依然是赋值和取值
        Dog d = new Dog("泰迪",3);
        field.setAccessible(true);//绕过访问权限，直接访问
        field.set(d,"追风筝"); //d.setHobby("追风筝");
        System.out.println(d);

        String hobby = (String) field.get(d);
        System.out.println(hobby);
    }
```
4. 获取类的成员方法：Method对象

Class提供了从类中获取成员方法的API

|方法|说明|
|----|----|
|Method[] getMethods​()|获取类的全部成员方法（只能获取public修饰的）|
|Method[] getDeclaredMethods​()|获取类的全部成员方法（只要存在就能拿到）|
|Method getMethod​(String name, Class<?>... parameterTypes) |获取类的某个成员方法（只能获取public修饰的）|
|Method getDeclaredMethod​(String name, Class<?>... parameterTypes)|获取类的某个成员方法（只要存在就能拿到）|

获取方法的目的依然是调用方法

|方法|说明|
|----|----|
|public Object invoke​(Object obj, Object... args)|触发某个对象的该放执行|
|public void  setAccessible(boolean flag)|设置为true，表示禁止检查访问控制（暴力反射）|

```java
@Test
    public void getMethodInfo() throws Exception {
        //目标：获取类的成员方法对象并对其进行操作
        //1.反射第一步：获得Class对象，代表拿到类
        Class c1 = Dog.class;
        //2.获取成员方法对象
        Method[] methods = c1.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method.getName()+"("+method.getParameterCount()+")");
        }

        //3.获取单个成员方法对象
        Method e1 = c1.getDeclaredMethod("eat");//获取无参数的eat方法
        Method e2 = c1.getDeclaredMethod("eat", String.class);//获取有参数的eat方法
        System.out.println(e1.getName()+"("+e1.getParameterCount()+")");
        System.out.println(e2.getName()+"("+e2.getParameterCount()+")");

        //4.获取方法的目的依然是调用方法
        Dog d = new Dog("泰迪",3);
        e1.setAccessible(true);//绕过访问权限，直接访问
        Object rs1 = e1.invoke(d);//唤醒对象d的eat方法执行，相当于d.eat();
        System.out.println(rs1);//null

        Object rs2 = e2.invoke(d,"骨头");//唤醒对象d的eat方法执行，相当于d.eat("骨头");
        System.out.println(rs2);//"www"
    }
```

### 反射的基本作用
可以得到一个类的全部成分然后操作

可以破坏封装性

可以绕过泛型的约束

```java
ArrayList<String> list = new ArrayList<>();
        list.add("hello");
        list.add("world");
//        list.add(123);
//        list.add(true);

        Class c1 = list.getClass();//c1 == ArrayList.class
        //获取ArrayList类的add方法
        Method add = c1.getDeclaredMethod("add", Object.class);
        //触发List集合对象的add方法执行
        add.invoke(list, 123);//翻墙
        add.invoke(list,true);//翻墙
        System.out.println(list);
```

* 最重要的用途是：适合做Java的框架，基本上，主流的框架都会基于反射设计出一些通用的功能

### 注解
就是Java代码里的特殊标记，比如：@Override、@Test等，作用是：让其他程序根据注解信息来决定怎么执行该程序

注意：注解可以用在类上、构造器上、方法上、成员变量上、参数上等位置处

#### 注解的概述-自定义注解

```java
public @interface 注解名称{
    public 属性类型 属性名() default 默认值;
}
```

```java
@MyBook(name = "《活着是为了快乐》", age = 18, address = {"北京", "上海"})
//@A("hello")//特殊属性：在使用时如果只有一个value属性，value名称可以不写
//@A(value = "hello", hobby = "打篮球")
//@A("hello")
public class AnnotationDemo1 {

    @MyBook(name = "王菲", age = 18, address = {"北京", "上海"})
    public static void main(@A("hello")String[] args) {
        //目标:自定义注解
        @A("hello")
        int a;
    }
}
```


```java
public @interface A {
    String value();//特殊属性：在使用时如果只有一个value属性，value名称可以不写
    String hobby() default "打篮球";//有默认值可以不写
}
```

![alt test](/image/屏幕截图%202026-07-22%20181854.png)

#### 元注解
注解注解的注解

```java
@Target({ElementType.METHOD,ElementType.FIELD})//指定注解只能用在方法,成员变量上
@Retention(RetentionPolicy.RUNTIME)//一直活着
public @interface MyTest {
    int count() default 1;
}
```

    @Target

    作用：声明被修饰的注解只能在那些位置使用

    1. TYPE，类，接口
    2. FUELD，成员变量
    3. METHOD，成员方法
    4. PARAMETER，方法参数
    5. CONSTRUCTOR，构造器
    6. LOCAL_VARIABLE,局部变量
   
    @Retention
    作用：声明注解的保留周期
    
    1. SOURCE：只作用在源码阶段，字节码文件中不存在
    2. CLASS（默认值）：保留到字节码文件阶段，运行阶段不存在
    3. RUNTIME（开发常用）：一直保留

#### 注解的解析
就是判断类上、方法上、成员变量上是否存在注解，并把注解里的内容给解析出来

_如何解析注解？_
* 指导思想：要解析谁上面的注解，就应该先拿到谁
* 比如要解析类上面的注解，则应该先获取类的Class对象，再通过Class对象解析其上面的注解
* 比如要解析成员方法上的注解，则应该获取到该成员方法的Method对象，再通过Method对象解析其上面的注解
* Class、Method、Field、Constructor都实现了AnnotatedElement接口，它们都拥有解析注解的能力

|AnnotatedElement接口提供了解析注解的方法|说明|
|----|----|
|public Annotation[] getDeclaredAnnotations()|获取当前对象上面的注解|
|public T getDeclaredAnnotation(Class<T> annotationClass)|获取指定的注解对象|
|public boolean isAnnotationPresent(Class<Annotation> annotationClass)|判断当前对象上是否存在某个注解|

```java
    @Test
    public void parseClass() throws Exception{
        //1.获取类对象
        Class c1 = Demo.class;
        //2.使用isAnnotationPresent判断类上是否陈列MyTest2注解
        if (c1.isAnnotationPresent(MyTest2.class)) {
            //3.获取注解对象
            MyTest2 myTest2 = (MyTest2) c1.getDeclaredAnnotation(MyTest2.class);
            //4.获取注解对象的属性值
            String value = myTest2.value();
            double price = myTest2.price();
            String[] address = myTest2.address();

            //5.打印注解属性值
            System.out.println(Arrays.toString(address));
            System.out.println(price);
            System.out.println(value);
        }
    }

    @Test
    public void parseMethod() throws Exception{
        //1.获取类对象
        Class c1 = Demo.class;
        //2.获取方法对象
        Method method = c1.getMethod("go");
        //3.使用isAnnotationPresent判断方法上是否使用了MyTest2注解
        if (method.isAnnotationPresent(MyTest2.class)) {
            //4.获取注解对象
            MyTest2 myTest2 = method.getDeclaredAnnotation(MyTest2.class);
            //5.获取注解对象的属性值
            String value = myTest2.value();
            double price = myTest2.price();
            String[] address = myTest2.address();

            //6.打印注解属性值
            System.out.println(Arrays.toString(address));
            System.out.println(price);
            System.out.println(value);
        }
    }
```

案例

```java
public class AnnotationDemo4 {
    //目标：搞清楚注解的应用场景：模拟junit框架，带MyTest注解的方法就执行
    public static void main(String[] args) throws Exception{
        AnnotationDemo4 ad4 = new AnnotationDemo4();
        //1.获取类对象
        Class c1 = AnnotationDemo4.class;
        //2.获取所有方法
        Method[] methods = c1.getMethods();
        //3.遍历所有方法，判断是否带MyTest注解，如果带就执行
        for (Method method : methods) {
            //4.判断方法上是否有MyTest注解
            if (method.isAnnotationPresent(MyTest.class)) {
                //获取注解对象
                MyTest myTest = method.getDeclaredAnnotation(MyTest.class);
                int count = myTest.count();
                //5.执行方法
                for (int i = 0; i < count; i++) {
                    method.invoke(ad4);
                }
            }
        }
    }

    //测试方法：public 无参 无返回值
    @MyTest(count = 3)
    public static void test() {
        System.out.println("test方法执行了！");
    }

    public static void test2() {
        System.out.println("test2方法执行了！");
    }

    @MyTest
    public static void test3() {
        System.out.println("test3方法执行了！");
    }

    public static void test4() {
        System.out.println("test4方法执行了！");
    }
}
```

```java
@Target({ElementType.METHOD, ElementType.FIELD})//指定注解只能用在方法,成员变量上
@Retention(RetentionPolicy.RUNTIME)//一直活着
public @interface MyTest {
    int count() default 1;
}
```

### 代理

程序为什么需要代理？代理长什么样？

对象如果嫌身上干的事情太多的话，可以通过代理来转移部分职责

对象有什么方法想被代理，代理就一定要有对应的方法

中介如何知到要派有唱歌、跳舞方法的代理呢？

接口

#### 如何为Java对象创建一个代理对象？
java.lang.reflect.proxy类：提供了为对象产生代理对象的方法：

```java
public class ProxyUtil {
    //创建一个明星对象的代理对象返回
    /**
     *public static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h)
     *
     * 参数一：用于执行哪个类加载器去加载生成的代理类
     * 参数二：用于指定代理类需要实现的接口：明星类实现了那些接口，代理类就实现那些接口、
     * 参数三：用于指定代理类需要如何去代理（代理类要做的事情）
     */
    public static StarServer createProxy(StarServer s) {
        StarServer proxy = (StarServer) Proxy.newProxyInstance(ProxyUtil.class.getClassLoader(),
                s.getClass().getInterfaces(), new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //声明代理对象要干的事情
                        //参数一：proxy接收代理对象本身（暂时用处不大）
                        //参数二：method正在被代理的方法
                        //参数三：args接收代理对象调用方法时传递的参数
                        String methodName = method.getName();
                        if ("sing".equals(methodName)) {
                            System.out.println("准备话筒，收钱");
                        } else if ("dance".equals(methodName)) {
                            System.out.println("准备场地，收钱");
                        }
                        //真正干活（把真正的对象叫过来正式干活）
                        //真正的明星对象来执行被代理的行为：method
                        Object result = method.invoke(s, args);
                        return result;
                    }
                });
        return proxy;
    }
}
```

```java
public class ProxyUtil {
    public static <T> T createProxy(T obj) {
        T proxy = (T) Proxy.newProxyInstance(ProxyUtil.class.getClassLoader(),
                obj.getClass().getInterfaces(), new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        long start = System.currentTimeMillis(); // 记录开始时间 1970年1月1日0时0分0秒 走到此刻的总毫秒值
                        // 真正调用业务对象被代理的方法
                        Object result = method.invoke(obj, args);

                        long end = System.currentTimeMillis();
                        System.out.println(method.getName() + "方法耗时："+(end-start)/1000.0+"秒");
                        return result;
                    }
                });
        return proxy;
    }
}
```

spring 加强思想

Aop 切面编程