### File
File是java.io.包下的类，File类的对象，用于代表当前操作系统的文件(可以是文件或文件夹)

File类只能对文件本身进行操作，不能读写文件里面的数据

![alt test](/image/屏幕截图%202026-07-07%20143622.png)

* File封装的对象仅仅是一个路径名，这个路径可以是存在的，也允许是不存在的
* 相对路径、绝对路径
    绝对路径：从盘符开始    D:\\xixi\\..

    相对路径：不带盘符，默认直接到当前工程下的目录寻找文件  stone-maze/src/image/

方法：

![alt test](/image/屏幕截图%202026-07-07%20150204.png)

```java

public static void main(String[] args) throws IOException {
        //目标：创建File创建对象代表文件（文件/目录），搞清楚其提供的对文件操作的方法
        //1，创建File对象，去获取某个文件的信息
        //File f1 = new File("D:\\OneDrive\\图片\\屏幕快照\\屏幕截图 2025-06-29 153057.png");
        File f1 = new File("D:/OneDrive/图片/屏幕快照/屏幕截图 2025-06-29 153057.png");

        System.out.println(f1.length());//字节个数
        System.out.println(f1.getName());//文件名
        System.out.println(f1.isFile());//是否是文件  true
        System.out.println(f1.isDirectory());//是否是目录  false

        //2.可以使用相对路径定位文件对象
        //只要带盘符的都称之为：绝对路径D:\\....
        //相对路径：不带盘符，默认是到你的idea工程下直接寻找文件，一般用来找工程下的项目文件
        File f2 = new File("day03-file-io\\src\\xixi");
        System.out.println(f2.length());

        //3.创建对象代表不存在的文件路径
        File f3 = new File("D:\\暑假项目\\ces.txt");
        System.out.println(f3.exists());//判断文件是否存在
        System.out.println(f3.createNewFile());//创建文件

        //4.创建对象代表不存在的文件夹路径
        File f4 = new File("D:\\暑假项目\\ces");
        System.out.println(f4.exists());
        System.out.println(f4.mkdir());//只能创建一级文件夹

        File f5 = new File("D:\\暑假项目\\ces\\ces1\\sss\\cca");
        System.out.println(f5.mkdirs());//可以创建多级文件夹

        //5.创建File对象代表存在的文件，然后删除它
        File f6 = new File("D:\\暑假项目\\ces\\ces1\\sss\\cca\\ces.txt");
        System.out.println(f6.delete());//删除文件

        //6.创建File对象代表存在文件夹，然后删除它
        File f7 = new File("D:\\暑假项目\\ces\\ces1\\sss\\cca");
        System.out.println(f7.delete());//只能删除空文件夹，不能删除有文件的文件夹，删除后的文件不会进入回收站

        //7.可以获得某个目录下的全部一级文件名称
        File f8 = new File("D:\\暑假项目");
        String[] names = f8.list();
        for (String name : names) {
            System.out.println(name);
        }
        //可以获得某个目录下的全部一级文件对象
        File[] files = f8.listFiles();
        for (File file : files) {
            System.out.println(file.getName());
            System.out.println(file.getAbsolutePath());//获取绝对路径
        }
        
        /**
         * 使用listFiles的注意事项
         * 当主调是文件，或者路径是不存在时，返回null
         * 当主调是空文件夹时，返回一个长度为0的数组
         * 当主调是有内容的文件夹时，将里面所有一级文件夹的路径放在File数组中返回
         * 当主调是一个文件夹时，且里面有隐藏文件时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件
         * 当主调是一个文件夹，但是没有权限访问时，返回null
         */
}
```

### 方法递归
递归是一种算法，在程序设计语言中广泛应用

从形式上说：方法调用自身的形式称为方法递归（recursion）

递归的形式
* 直接递归：方法自己调用自己。 
* 间接递归：方法调用其他方法，其他方法又回调方法自己

* 注意：递归如果没有控制好，可能出现死循环，导致出现栈溢出

#### 递归-算法

```java
public static int f2(int n){
        if (n == 1){
            return 1;
        }
        return factorial(n-1) + n;//递归调用
    }
```

##### 递归算法的三要素
* 递归的公式：f(n)=(n-1) + n
* 递归的终结点：f(1)
* 递归的方向必须走向终结点

#### 文件搜索
```java
public class RecursionTest4 {
    public static void main(String[] args) {
        //目标：完成文件搜索：找出D:盘下的QQ.exe的位置
        try {
            File dir=new File("D:\\");
            findFile(dir,"QQ.exe");
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
    /**
     * 递归查找文件
     * @param dir ：要搜索的目录
     * @param filename ：要搜索的文件名
     */
    public static void findFile(File dir,String filename) throws IOException{
        //1.判断极端情况
        if (dir == null || !dir.exists()|| !dir.isFile()){
            return;//不搜索
        }

        //2.获取当前目录下的所有一级文件或文件夹
        File[] files = dir.listFiles();

        //3.判断当前目录下是否存在一级文件对象，存在才可以遍历
        if (files != null && files.length > 0){
            //4.遍历一级文件对象
            for (File file : files) {
                //5.判断当前文件对象是否是文件
                if (file.isFile()){
                    //6.判断文件名是否相同
                    if (file.getName().contains(filename)){
                        System.out.println("找到文件："+file.getAbsolutePath());
                        Runtime r = Runtime.getRuntime();//获取当前运行环境
                        r.exec(file.getAbsolutePath());
                    }else{
                        //6.如果是文件夹，递归调用
                        findFile(file,filename);
                    }
                }

            }
        }
    }
}
```

### 字符集
#### 常见字符集

标准ASCII字符集

ASCII：美国信息交换标准代码，包括了英文、符号等

标准ASCII使用1个字节存储一个字符，首位是0，因此，总共可表示128个字符

GBK（汉字内码扩展规范，国标）

汉字编码字符集，包含2万多个汉字等字符，GBK中一个中文字符编码成两个字节的形式存储

GBK规定汉字的第一个字节的第一位必须是1

GBK兼容ASCII字符集

Unicode字符集（统一码，也叫万国码）

Unicode是国际组织制定的，可以容纳世界上所有文字、符号的字符集

UTF-32 4个字节表示一个字符

UTF-8 是Unicode字符集的一种编码方案，采用可变长编码方案，共分为四个长度区：1个字节，2个字节，3个字节，4个字节

英文字符、数字等只占一个字节（兼容ASCII编码），汉字字符占3个字节

|0xxxxxxx||||   
|110xxxxx|10xxxxxx|||   
|1110xxxx|10xxxxxx|10xxxxxx||   
|11110xxx|10xxxxxx|10xxxxxx|10xxxxxx|

* 字符编码是使用的字符集，和解码时使用的字符集必须一致，否则会出现乱码
* 英文、数字一般不会乱码，因为很多都兼容ASCII

#### 对字符进行编码和解码

编码

```java
//1.编码
        String s = "你好hello";

        //byte[] bytes = s.getBytes();//平台的UTF-8
        byte[] bytes = s.getBytes("GBK");//指定GBK编码
        System.out.println(bytes.length);
        System.out.println(Arrays.toString(bytes));
```

解码

```java
//2.解码
//        String s1 = new String(bytes);//平台的UTF-8
        String s1 = new String(bytes,"GBK");//指定GBK解码
        System.out.println(s1);
```

### IO流
用于读写数据的（可以读写文件或网络中的数据....）

I指Input,称为输入流：负责把数据读到内存

O是指Output，称为输出流：负责写数据出去

io流的分类

![alt test](/image/屏幕截图%202026-07-07%20175343.png)

io流的体系

![alt test](/image/屏幕截图%202026-07-07%20175804.png)

#### FileInputStream(文件字节输入流)
作用：以内存为基准，可以把磁盘文件中的数据以字节的形式读入到内存中去

![alt test](/image/屏幕截图%202026-07-07%20180054.png)

```java
//1.创建文件字节输入流管道于源文件接通
        //FileInputStream is = new FileInputStream(new File("day03-file-io\\src\\xixi2"));
        InputStream is = new FileInputStream("day03-file-io\\src\\xixi2");//简化写法

        //2.开始读取文件中的字节并输出，每次读一个
        //定义一个变量记住每次读取的一个字节
        int b;
        while ((b = is.read()) != -1) {
            System.out.print((char) b);
        }
        //每次读取一个字节的问题：性能较低、读取汉字输出一定会乱码
        is.close();//关闭流 释放资源
```

```java

//2.开始读取文件中的字节并输出，每次读多个
        //定义一个字节数组用于记住每次读取了多少字节
        byte[] buffer = new byte[3];
        //定义一个变量记住每次读取了多少字节
        int len;
        while ((len = is.read(buffer)) != -1) {
            //3.把读取的字节数组转换为字符串输出,读取多少倒出多少
            String s = new String(buffer,0,len);
            System.out.print(s);
        }

        //拓展：每次读取多个字节，性能得到提升，因为每次读取多个字节，可以减少硬盘和内存的交互次数，提升性能
        //依然无法避免读取汉字时乱码，存在截断汉字字节的可能性
```

使用字节读取中文，如何保证不乱码，怎么解决？
* 定义一个与文件一样大的字节数组，一次性读取完文件的全部字节

```java

//2.一次性读取文件的全部字节：可以避免读取汉字时乱码的问题
        byte[] bytes = is.readAllBytes();

        String s = new String(bytes);
        System.out.println(s);

        //文件过大时，一次性读取全部字节性能较低，容易导致内存溢出
```

读文本适合用字符流；字节流适合做数据转移

#### FIleOutputStream文件字节输出流

![alt test](/image/屏幕截图%202026-07-08%20144305.png)

```java

//1.创建我文件字节输出管道于目标文件接通
        //OutputStream os = new FileOutputStream("day03-file-io\\src\\xixi05.txt");//覆盖管道
        OutputStream os = new FileOutputStream("day03-file-io\\src\\xixi05.txt", true);//追加数据

        //2.写入数据
        // public void write(int b)
        os.write(97);//写入一个字节
        os.write('b');//写入一个字符数据
        //os.write('我');//写入一个字符串数据 乱码
        os.write("\r\n".getBytes());//换行

        //3.写一个字节数组出去
        //public void write(byte[] b)
        byte[] buffer = "我爱你中国baba11".getBytes();
        os.write(buffer);
        os.write("\r\n".getBytes());//换行

        //4.写一个字节数组的一部分出去
        //public void write(byte[] b, int pos, int len)
        os.write(buffer,0,3);
        os.write("\r\n".getBytes());//换行

        os.close();//关闭管道
```

#### 字节流复制文件
任何文件的底层都是字节，字节流做复制，是一字不漏的转移完全部字节，只要复制后的文件格式一致就没问题

```java
    public static void main(String[] args) {
        //目标：使用字节流完成文件的复制
        //源文件：D:\OneDrive\图片\屏幕快照\大虫子.png
        //目标文件：D:\OneDrive\图片\屏幕快照\大虫子_复制.png（复制过去的时候必须带文件名的，无法自动生成文件名）
            copyFile("D:\\OneDrive\\图片\\屏幕快照\\大虫子.png","D:\\OneDrive\\图片\\屏幕快照\\大虫子_复制.png");
    }

    //复制文件
    public static void copyFile(String srcPath,String destPath) {
        InputStream fis = null;
        OutputStream fos = null;

        try {
            //创建输入流
            fis = new FileInputStream(srcPath);
            //创建输出流
            fos = new FileOutputStream(destPath);

            //2.读取一个字节组，写入一个字节组   1024 + 1024 + 3
            byte[] buf = new byte[1024];
            int len;
            while ((len = fis.read(buf)) != -1){
                fos.write(buf,0,len);//读取多少个字节就倒多少个字节
            }
            System.out.println("复制完成");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //最后一定会执行一次，即便程序出现异常
            //3.释放资源
            try {
                if (fos != null) fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (fis != null) fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
```

#### 资源释放的方案

![alt test](/image/屏幕截图%202026-07-08%20145611.png)

* finally代码区的特点：无论try中的程序时正常执行了，还是出现异常，最后都一定会执行finally区，除非JVM终止
* 作用：一般用于在程序执行完成后进行资源释放操作

JDK7开始提供了更简单的资源释放方案：try-with-resource

try(定义资源1;定义资源2;.....){
    可能出现异常的代码;
}catch(异常类名 变量名){
    异常的处理代码;
}

该资源使用完毕后，会自动调用其close()方法，完成对资源的释放！

()中只能放资源，否则报错

什么是资源？

资源一般是指的是最终实现了AutoCloseable接口

```java
....
//复制文件
    public static void copyFile(String srcPath,String destPath) {

        try (
                //这里只能放置资源对象，用完后，最终会自动调用其close()方法关闭
            //创建输入流
            InputStream fis = new FileInputStream(srcPath);
            //创建输出流
            OutputStream fos = new FileOutputStream(destPath);
        ){
            //2.读取一个字节组，写入一个字节组   1024 + 1024 + 3
            byte[] buf = new byte[1024];
            int len;
            while ((len = fis.read(buf)) != -1){
                fos.write(buf,0,len);//读取多少个字节就倒多少个字节
            }
            System.out.println("复制完成");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

#### FileReader 文件字符输入流
作用：以内存为基准，可以把文件中的数据以字符的形式读入到内存中去

![alt test](/image/屏幕截图%202026-07-08%20151433.png)

```java
public static void main(String[] args) {
        //目标：掌握文件字符输入流读取字符内容到程序中去
        try (
            //1，创建文件子符输入流读取字符内容到程序中来
            Reader fr = new FileReader("day03-file-io\\src\\xixi06.txt");
        ) {
            //2.定义一个字符数组，每次读取多个字符
            char[] buffer = new char[3];
            int len;//定义一个变量记住每次读取了多少字符
            while ((len = fr.read(buffer)) != -1){
                //3.每次读取多个字符，并把字符数组转换成字符串输出
                String s = new String(buffer,0,len);
                System.out.print(s);
            }
            //拓展：文件字符输入流每次读取多少个字符，性能较好，而且读取中文
            //是按照字符读取，不会出现乱码，这是一种读取中文很好的方案
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

#### FileWriter文件字符输出流
作用：以内存为基准，把内存中的数据以字符的形式写出到文件中去

![alt test](/image/屏幕截图%202026-07-08%20153021.png)

```java
public static void main(String[] args) {
        //目标：搞清楚我文件字符输出流的使用，写字符出去的流
        try (
                //1.创建一个字符输出流对象，指定写出的目的地
               // Writer fw = new FileWriter("day03-file-i;o\\src\\xixi07.txt");//覆盖管道
                Writer fw = new FileWriter("day03-file-i;o\\src\\xixi07.txt", true);//追加管道
                ){
            //2.写一个字符出去：  public void write(int c)
            fw.write('a');
            fw.write(90);
            fw.write('我');
            fw.write("\r\n");//换行

            //3.写一个字符串出去：  public void write(String str)
            fw.write("hello world");
            fw.write("Java");
            fw.write("\r\n");

            //4.写字符串的一部分出去：  public void write(String str, int pos, int len)
            fw.write("hello world",0,5);
            fw.write("\r\n");

            //5.写一个字符数组出去：  public void write(char[] cbuf)
            char[] buffer = "我爱你中国baba11".toCharArray();
            fw.write(buffer);
            fw.write("\r\n");

            //6.写字符数组的一部分出去：  public void write(char[] cbuf, int off, int len)
            fw.write(buffer,0,5);
            fw.write("\r\n");

            //fw.flush();//刷新缓冲区,将缓冲区中的数据全部写出去
            //刷新后，流可以继续使用
            //fw.close();//关闭资源，关闭包含了刷新！关闭后流不能继续使用

        }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

字符输出流的注意事项

字符输出流写出数据后，必须刷新流，或者关闭流，写出去的数据才能生效

#### 缓冲字节流

BufferedInputStream 缓冲字节输入流

作用：可以提高字节输入流读取数据的性能

原理：缓冲字节输入流自带8KB缓冲池；缓冲字节输入流也自带了8KB缓冲池

![alt test](/image/屏幕截图%202026-07-08%20155402.png)

```java
try (
        //这里只能放置资源对象，用完后，最终会自动调用其close()方法关闭
        //创建输入流
        InputStream fis = new FileInputStream(srcPath);
        //把低级的字节输入流转包装成高级的字节输入缓冲流
            BufferedInputStream bis = new BufferedInputStream(fis);
        //创建输出流
        OutputStream fos = new FileOutputStream(destPath);
        //把低级的字节输出流转包装成高级的字节输出缓冲流
            BufferedOutputStream bos = new BufferedOutputStream(fos);
    ){
        //2.读取一个字节组，写入一个字节组   1024 + 1024 + 3
        byte[] buf = new byte[1024];
        int len;
        while ((len = bis.read(buf)) != -1){
            bos.write(buf,0,len);//读取多少个字节就倒多少个字节
        }
        System.out.println("复制完成");
    } catch (IOException e) {
        e.printStackTrace();
    }
```

#### 缓冲字符流

BufferedReader缓冲字符输入流

作用：自带8K（8192）的字符缓冲池，可以提高字符输入流读取字符数据的性能

![alt test](/image/屏幕截图%202026-07-08%20155946.png)

```java
public static void main(String[] args) {
        //目标：搞清楚缓冲字符输入流读取字符内容：性能提升了，多了按照行读取文本的能力
        try (
                //1，创建文件子符输入流读取字符内容到程序中来
                Reader fr = new FileReader("day03-file-io\\src\\xixi08.txt");
                //2.创建缓冲字符输入流包装低级字符输入流
                BufferedReader br = new BufferedReader(fr);
        ) {
//            //2.定义一个字符数组，每次读取多个字符
//            char[] buffer = new char[3];
//            int len;//定义一个变量记住每次读取了多少字符
//            while ((len = br.read(buffer)) != -1){
//                //3.每次读取多个字符，并把字符数组转换成字符串输出
//                String s = new String(buffer,0,len);
//                System.out.print(s);
//            }

//            System.out.println(br.readLine());
//            System.out.println(br.readLine());
//            System.out.println(br.readLine());//null

            //使用循环改进，来按照行读取数据
            //定义一个字符串变量用于记住每次读取的一行数据
            String line;
            while ((line = br.readLine()) != null){
                System.out.println(line);
            }
            //目前读取文本最优雅的方案：性能好，不乱码，可以按行读

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

BufferedWriter缓冲字符输出流

作用：自带8K的字符缓冲池，可以提高字符输出流写字符数据的性能

![alt test](/image/屏幕截图%202026-07-08%20161036.png)

```java
public static void main(String[] args) {
        //目标：搞清楚缓冲文件字符输出流的使用：提升了字符输出流的写出性能，多了换行功能
        try (
                //1.创建一个字符输出流对象，指定写出的目的地
                // Writer fw = new FileWriter("day03-file-i;o\\src\\xixi07.txt");//覆盖管道
                Writer fw = new FileWriter("day03-file-io\\src\\xixi07.txt", true);//追加管道

                //2.创建缓冲字符输出流包装低级字符输出流
                BufferedWriter bw = new BufferedWriter(fw);
                ){
            //2.写一个字符出去：  public void write(int c)
            bw.write('a');
            bw.write(90);
            bw.write('我');
            bw.newLine();//换行

            //3.写一个字符串出去：  public void write(String str)
            bw.write("hello world");
            bw.write("Java");
            bw.newLine();

            //4.写字符串的一部分出去：  public void write(String str, int pos, int len)
            bw.write("hello world",0,5);
            bw.newLine();

            //5.写一个字符数组出去：  public void write(char[] cbuf)
            char[] buffer = "我爱你中国baba11".toCharArray();
            bw.write(buffer);
            bw.newLine();

            //6.写字符数组的一部分出去：  public void write(char[] cbuf, int off, int len)
            bw.write(buffer,0,5);
            bw.newLine();

            //fw.flush();//刷新缓冲区,将缓冲区中的数据全部写出去
            //刷新后，流可以继续使用
            //fw.close();//关闭资源，关闭包含了刷新！关闭后流不能继续使用

        }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

#### 性能分析

```java
//拿系统当前时间
long start = System.currentTimeMillis();//此刻时间毫秒值 从1970-01-01 00:00:00.000 开始走到此刻的总毫秒值 1s=1000ms
....
long end = System.currentTimeMillis();//此刻时间毫秒值 从1970-01-01 00:00:00.000 开始走到此刻的总毫秒值
```

建议使用字节缓冲输入流、字节缓冲输出流，结合字节数组的方式，目前来看是性能最优的组合。

#### InputStreamReader字符输入转换流
解决不同编码时，字符流读取文本内容乱码的问题

* 解决思路：先获取文件的原始字节流，再将其按真实的字符集编码转换成字符输入流，这样字符输入流中的字符就不乱码了

![alt test](/image/屏幕截图%202026-07-08%20172512.png)

```java
public static void main(String[] args) {
        //目标：使用字符输入转换流InputStreamReader解决不同编码读取乱码的问题
        //代码：UTF-8  文件：UTF-8  读取不乱码
        //代码：UTF-8  文件：GBK  读取乱码
        try (
                //先提取文件的原始字节流
                InputStream is = new FileInputStream("day03-file-io\\src\\xixi09.txt");
                //指定字符集把原始字节流转换成字符输入流
                Reader isr = new InputStreamReader(is,"GBK");

                //2.创建缓冲字符输入流包装低级字符输入流
                BufferedReader br = new BufferedReader(isr);
        ) {

            //定义一个字符串变量用于记住每次读取的一行数据
            String line;
            while ((line = br.readLine()) != null){
                System.out.println(line);
            }
            //目前读取文本最优雅的方案：性能好，不乱码，可以按行读

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

#### 打印流
PrintStream/PrintWriter

作用：打印流可以实现更方便、更高效的打印数据出去，能实现打印啥出去就是啥出去

![alt test](/image/屏幕截图%202026-07-08%20172917.png)

![alt test](/image/屏幕截图%202026-07-08%20172949.png)

```java
public static void main(String[] args) {
        //目标：打印流使用
        try (
                //PrintStream ps = new PrintStream("day03-file-io\\src\\xixi10.txt");
                PrintStream ps = new PrintStream(new FileOutputStream("day03-file-io\\src\\xixi10.txt", true));
                //PrintWriter ps = new PrintWriter("day03-file-io\\src\\xixi10.txt");
                ) {
            ps.println(97);
            ps.println('a');
            ps.println("嘻嘻");
            ps.println(3.14);
            ps.println(true);
                }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

#### 特殊数据流

DataOutputStream(数据输出流)

允许把数据和其类型一并写出去。

![alt test](/image/屏幕截图%202026-07-08%20173340.png)

```java
public static void main(String[] args) {
        //目标：特殊数据流的使用
        try (
                DataOutputStream dos = new DataOutputStream(new FileOutputStream("day03-file-io\\src\\xixi11.txt"));
                ){
            dos.writeByte(10);
            dos.writeInt(100);
            dos.writeBoolean(true);
            dos.writeChar('a');
            dos.writeDouble(9.5);
            dos.writeUTF("hello world");

        }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

DataInputStream(数据输入流)

用于读取数据输出流写出去的数据。 

![alt test](/image/屏幕截图%202026-07-08%20173601.png)

```java
public static void main(String[] args) {
        //目标：特殊数据流的使用
        try (
                DataInputStream dis = new DataInputStream(new FileInputStream("day03-file-io\\src\\xixi11.txt"));
                ){
            System.out.println(dis.readByte());
            System.out.println(dis.readInt());
            System.out.println(dis.readBoolean());
            System.out.println(dis.readChar());
            System.out.println(dis.readDouble());
            System.out.println(dis.readUTF());
        }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

#### IO框架

什么是框架？
* 框架是一个预先写好的代码库或一组工具，旨在简化和加速开发过程
* 框架的形式：一般是把类、接口等编译成class形式，再压缩成一个.jar结尾的文件发行出去

什么是IO框架？
* 封装了Java提供的对文件、数据进行操作的代码，对外提供了更简单的方式来对文件进行操作，对数据进行读写等。

导入commons-io-2.11.0.jar框架到项目中去。

    在项目中创建一个文件夹：lib

    将commons-io-2.6.jar文件复制到lib文件夹

    在jar文件上点右键，选择 Add as Library -> 点击OK

    在类中导包使用

![alt test](/image/屏幕截图%202026-07-08%20174335.png)

```java
public static void main(String[] args) throws Exception{
        //目标：Io框架使用
        FileUtils.copyFile(new File("day03-file-io\\src\\csb.txt"), new File("day03-file-io\\src\\csb02.txt"));

        //JDK 7提供的
        // Files.copy(Path.of("day03-file-io\\src\\csb.txt"), Path.of("day03-file-io\\src\\csb03.txt"));

        FileUtils.deleteDirectory(new File("D:\\暑假项目\\Test\\aa"));
    }
```

