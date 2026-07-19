### 网络编程
可以让设备中的程序与网络上其他设备中的程序进行数据交互的技术（实现网络通信）

#### 基本的通信架构
有2种形式：C/S架构（Client客户端/Server服务端）、B/S架构（Browser浏览器/Server服务器）

#### 网络通信三要素

1. IP地址
   设备再网络中的地址，是设备在网络中的唯一标志
2. 端口
   应用程序在设备中的唯一标识
3. 协议
   连接和数据在网络中传输的规则

##### IP地址
全称“互联网协议地址”，是分配给上网设备的唯一标识

目前，被广泛采用的IP地址有两种：IPv4、IPv6

_IPv4_

IPv4是Internet Protocol version 4的缩写，它使用32位地址，通常使用点分十进制表示

_IPv6_

IPv6是Internet Protocol version 6的缩写，它使用128位地址，号称可以为地球上的每一粒沙子编号。

IPv6分成8段，每段每四位编码成一个十六进制位表示， 每段之间用冒号（:）分开，将这种方式称为冒分十六进制。

_IP域名_

用于在互联网上识别和定位网站的人类可读的名称。 如：www.baidu.com

_DNS域名解析_

是互联网中用于将域名转换为对应IP地址的分布式命名系统。它充当了互联网的“电话簿”，将易记的域名映射到数字化的IP地址，使得用户可以通过域名来访问网站和其他网络资源

_公网IP、内网IP_

公网IP：是可以连接到互联网的IP地址

内网IP：也叫局域网IP，是只能组织机构内部使用的IP地址；例如，192.168. 开头的就是常见的局域网地址，范围为192.168.0.0--192.168.255.255，专门为组织机构内部使用

_本机IP_

127.0.0.1、localhost：代表本机IP，只会寻找当前程序所在的主机

_IP常用命令_

ipconfig：查看本机IP地址。

ping IP地址：检查网络是否连通

##### InetAddress

代表IP地址

InetAdderss的常用方法

|InetAddress类的常用方法|说明|
|----|----|
|public static InetAddress getLocalHost() throws UnknownHostException|获取本机IP，返回一个InetAddress对象|
|public String getHostName()|获取该ip地址对象对应的主机名|
|public String getHostAddress()|获取该ip地址对象中的ip地址信息|
|public static InetAddress getByName(String host) throws UnknownHostException|根据ip地址或者域名，返回一个inetAddress对象|
|public boolean isReachable(int timeout) throws IOException|判断主机在指定毫秒内与该ip对应的主机是否能连通|

```java
public static void main(String[] args) {
        //目标：认识InetAddress获取本机IP对象和对方IP对象
        try {
            //获取本机IP对象
            InetAddress ip = InetAddress.getLocalHost();
            System.out.println(ip.getHostAddress());//优先公网IP
            System.out.println(ip.getHostName());

            //获取对方IP对象
            InetAddress ip2 = InetAddress.getByName("www.baidu.com");
            System.out.println(ip2.getHostAddress());
            System.out.println(ip2.getHostName());

            //判断本机与对方主机是否互通
            System.out.println(ip2.isReachable(5000));//ping
        }catch (Exception e){
            e.printStackTrace();
        }
    }
```

##### 端口
用来标记标记正在计算机设备上运行的应用程序，被规定为一个 16 位的二进制，范围是 0~65535

_端口分类_

周知端口：0~1023，被预先定义的知名应用占用（如：HTTP占用 80，FTP占用21） 

注册端口：1024~49151，分配给用户进程或某些应用程序

动态端口：49152到65535，之所以称为动态端口，是因为它一般不固定分配某种进程，而是动态分配

注意：我们自己开发的程序一般选择使用注册端口，且一个设备中不能出现两个程序的端口号一样，否则报错

##### 通信协议
网络上通信的设备，事先规定的连接规则，以及传输数据的规则被称为网络通信协议

_开放式网络互联标准：OSI网络参考模型_

OSI网络参考模型：全球网络互联标准

TCP/IP网络模型：事实上的国际标准

![alt test](/image/屏幕截图%202026-07-16%20145335.png)

##### 传输层的2个通信协议

UDP(User Datagram Protocol)：用户数据报协议

TCP(Transmission Control Protocol) ：传输控制协议

_UDP协议_

通信效率高 视频直播 语音通话

特点：无连接、不可靠通信

不事先建立连接，数据按照包发，一包数据包含：自己的IP、端口、目的地IP、端口和数据（限制在64KB内）等

发送方不管对方是否在线，数据在中间丢失也不管，如果接收方收到数据也不返回确认，故是不可靠的 

_TCP协议_

特点：面向连接、可靠通信

TCP的最终目的：要保证在不可靠的信道上实现可靠的数据传

TCP主要有三个步骤实现可靠传输：三次握手建立连接，传输数据进行确认，四次挥手断开连接

可靠连接：确保通信的双方收发消息都是没问题的（全双工）

三次握手

1. C 发出连接请求到 S
2. S 返回一个响应到 C
3. C 再次发出确认信息，建立连接到 S

传输数据会进行确认，以保证数据传输的可靠性
    
四次挥手断开连接

![alt test](/image/屏幕截图%202026-07-16%20150611.png)

##### UDP通信
* 特点：无连接、不可靠通信
* 不事先建立连接；发送端每次把要发送的数据（限制在64KB内）、接收端IP、等信息封装成一个数据包，发出去就不管了
* Java提供了一个java.net.DatagramSocket类来实现UDP通信

客户端实现步骤
1. 创建DatagramSocket对象（客户端对象）
2. 创建DatagramPacket对象封装需要发送的数据（数据包对象）
3. 使用DatagramSocket对象的send方法，传入DatagramPacket对象
4. 释放资源

```java
public static void main(String[] args) throws Exception {
        //目标：完成UDP通信一发一收，客户端开发
        System.out.println("UDP客户端启动");
        //1，创建发送端对象（代表抛韭菜的人）
        //public DatagramSocket() 创建客户端的Socket对象, 系统会随机分配一个端口号
        DatagramSocket socket = new DatagramSocket();//随机端口

        //2.创建数据包对象封装发送的数据（韭菜盘子）
        byte[] bytes = "你好,UDP".getBytes();
        /**
         *  public DatagramPacket(byte[] buf, int length,
         *                          InetAddress address, int port)
         * 参数1：发送的数据，字节数组（韭菜）
         * 参数2：发送的字节长度
         * 参数3：目的地的IP地址
         * 参数4：服务端程序端口号
         */
        DatagramPacket packet = new DatagramPacket(bytes, bytes.length, InetAddress.getLocalHost(), 8080);

        //3.让发送端对象发送数据包的数据
        //public void send(DatagramPacket dp)
        socket.send(packet);

        socket.close();
    }
```

服务器实现步骤
1. 创建DatagramSocket对象并指定端口（服务端对象）
2. 创建DatagramPacket对象接收数据（数据包对象）
3. 使用DatagramSocket对象的receive方法，传入DatagramPacket对象
4. 释放资源

```java
public static void main(String[] args) throws Exception {
        //目标：完成UDP通信一发一收，服务端开发
        System.out.println("服务端启动了");
        //1.创建接收端对象，注册端口（接韭菜的人）
        //public DatagramSocket(int port)
        DatagramSocket socket = new DatagramSocket(8080);
        //2.创建一个数据包对象负责接收数据，（韭菜盘子）
        byte[] bytes = new byte[1024*64];
        //public DatagramPacket(byte[] buf, int length)
        DatagramPacket packet = new DatagramPacket(bytes, bytes.length);

        //3.接受数据，将数据封装到数据包对象的字节数组中
        socket.receive(packet);//public void receive(DatagramPacket p)

        //4.看看数据是否接收
        //获取当前收到的数据长度
        //public int getLength()
        int length = packet.getLength();//获取数据包，实际接收到的字节个数


        String data = new String(bytes, 0, length);
        System.out.println(data);

        //获取对方的IP对象和程序端口
        InetAddress ip = packet.getAddress();
        int port = packet.getPort();
        System.out.println(ip.getHostAddress() + ":" + port);

        socket.close();
    }
```

客户端反复发送数据

1. 创建DatagramSocket对象（发送端对象）
2. 使用while死循环不断的接收用户的数据输入，如果用户输入的exit则退出程序
3. 如果用户输入的不是exit,  把数据封装成DatagramPacket
4. 使用DatagramSocket对象的send方法将数据包对象进行发送
5. 释放资源

```java
public static void main(String[] args) throws Exception {
        //目标：完成UDP通信多发多收，客户端开发
        System.out.println("UDP客户端启动");
        //1，创建发送端对象（代表抛韭菜的人）
        DatagramSocket socket = new DatagramSocket();//随机端口

        Scanner sc = new Scanner(System.in);
        //2.创建数据包对象封装发送的数据（韭菜盘子）
        while (true) {
            System.out.println("请说：");
            String data = sc.nextLine();//读取直到回车符

            //如果用户输入的是exit，则结束发送
            if ("exit".equals(data)) {
                System.out.println("结束发送");
                socket.close();//结束发送
                break;
            }
            byte[] bytes = data.getBytes();
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length, InetAddress.getLocalHost(), 8080);

            //3.让发送端对象发送数据包的数据
            socket.send(packet);
        }
    }
```

接收端反复接收数据
1. 创建DatagramSocket对象并指定端口（接收端对象）
2. 创建DatagramPacket对象接收数据（数据包对象）
3. 使用DatagramSocket对象的receive方法传入DatagramPacket对象
4. 使用while死循环不断的进行第3步

```java
public static void main(String[] args) throws Exception {
        //目标：完成UDP通信多发多收，服务端开发
        System.out.println("服务端启动了");
        //1.创建接收端对象，注册端口（接韭菜的人）
        DatagramSocket socket = new DatagramSocket(8080);
        //2.创建一个数据包对象负责接收数据，（韭菜盘子）
        byte[] bytes = new byte[1024*64];
        DatagramPacket packet = new DatagramPacket(bytes, bytes.length);

        while (true) {
            //3.接受数据，将数据封装到数据包对象的字节数组中
            socket.receive(packet);//阻塞式接收数据

            //4.看看数据是否接收

            int length = packet.getLength(); //获取当前收到的数据长度
            String data = new String(bytes, 0, length);
            System.out.println("服务端收到"+data);

            //获取对方的IP对象和程序端口
            InetAddress ip = packet.getAddress();
            int port = packet.getPort();
            System.out.println(ip.getHostAddress() + ":" + port);

            System.out.println("------------------");
        }
    }
```

##### TCP通信
* 特点：面向连接、可靠通信
* 通信双方事先会采用“三次握手”方式建立可靠连接，实现端到端的通信；底层能保证数据成功传给服务端
* Java提供了一个java.net.Socket类来实现TCP通信

一发一收——客户端

1. 创建客户端的Socket对象，请求与服务端的连接
2. 使用socket对象调用getOutputStream()方法得到字节输出流
3. 使用字节输出流完成数据的发送
4. 释放资源：关闭socket管道

```java
public static void main(String[] args) throws Exception {
        //目标：实现TCP通信一发一收，客户端开发
        System.out.println("客户端启动");
        //1.常见Socket普通对象，请求与服务器的Socket连接，可靠连接
        Socket socket = new Socket("127.0.0.1", 9999);

        //2.从Socket通信管道中得到一个字节输出流
        OutputStream os = socket.getOutputStream();

        //3.特殊数据流
        DataOutputStream dos = new DataOutputStream(os);
        dos.writeInt(1);
        dos.writeUTF("张三");

        //4.关闭资源
        socket.close();
    }
```

一发一收——服务端开发
1. 创建ServerSocket对象，注册服务端端口
2. 调用ServerSocket对象的accept()方法，等待客户端的连接，并得到Socket管道对象
3. 通过Socket对象调用getInputStream()方法得到字节输入流、完成数据的接收
4. 释放资源：关闭socket管道

```java
public static void main(String[] args) throws Exception {
        //目标：实现TCP通信一发一收，服务端开发
        System.out.println("服务端启动了");
        //1.创建一个服务器Socket对象，绑定监听端口，监听客户端连接
        ServerSocket ss = new ServerSocket(9999);
        //2.调用accept方法，堵塞等待客户端连接，一旦有客户端链接会返回一个Socket对象
        Socket socket = ss.accept();
        //3.获取输入流，读取客户端发送的数据
        InputStream is = socket.getInputStream();
        //4.把字节输入流包装成特殊字节输入流
        DataInputStream dis = new DataInputStream(is);
        //5.读取数据
        int id = dis.readInt();
        String msg = dis.readUTF();
        System.out.println("id:"+id+" msg:"+msg);
        //6.客户端的IP和端口（谁发给我）
        System.out.println(socket.getInetAddress().getHostAddress()+":"+socket.getPort());
    }
```

多发多收——客户端

客户端使用死循环，让用户不断输入消息

```java
public static void main(String[] args) throws Exception {
        //目标：实现TCP通信多发多收，客户端开发
        System.out.println("客户端启动");
        //1.常见Socket普通对象，请求与服务器的Socket连接，可靠连接
        Socket socket = new Socket("127.0.0.1", 9999);

        //2.从Socket通信管道中得到一个字节输出流
        OutputStream os = socket.getOutputStream();

        //3.特殊数据流
        DataOutputStream dos = new DataOutputStream(os);

        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请说：");
            String data = sc.nextLine();
            if ("exit".equals(data)) {
                System.out.println("结束发送");
                dos.close();//输出管道
                socket.close();//通信管道
                break;
            }
            dos.writeUTF(data);//发送
            dos.flush();//刷新
        }

    }
```

多发多收——服务端

服务端也使用死循环，控制服务端程序收完消息后，继续去接收下一个消息

```java
public static void main(String[] args) throws Exception {
        //目标：实现TCP通信多发多收，服务端开发
        System.out.println("服务端启动了");
        //1.创建一个服务器Socket对象，绑定监听端口，监听客户端连接
        ServerSocket ss = new ServerSocket(9999);
        //2.调用accept方法，堵塞等待客户端连接，一旦有客户端链接会返回一个Socket对象
        Socket socket = ss.accept();
        //3.获取输入流，读取客户端发送的数据
        InputStream is = socket.getInputStream();
        //4.把字节输入流包装成特殊字节输入流
        DataInputStream dis = new DataInputStream(is);
        //5.读取数据
        while (true) {
            String msg = dis.readUTF();//等待读取客户端发送的数据
            System.out.println(msg);
            //6.客户端的IP和端口（谁发给我）
            System.out.println(socket.getInetAddress().getHostAddress()+":"+socket.getPort());
            System.out.println("------------------");
        }
    }
```

_同时接收多个客户端消息_

多发多收——客户端 不变

服务端

```java
public static void main(String[] args) throws Exception {
        //目标：实现TCP通信多发多收，服务端开发
        System.out.println("服务端启动了");
        //1.创建一个服务器Socket对象，绑定监听端口，监听客户端连接
        ServerSocket ss = new ServerSocket(9999);
        //2.调用accept方法，堵塞等待客户端连接，一旦有客户端链接会返回一个Socket对象
        while (true) {
            Socket socket = ss.accept();
            System.out.println("一个客户端上线了"+ socket.getInetAddress().getHostAddress());
            //3.把这个客户端管道交给一个独立的子线程专门负责接收这个管道的消息
            new ServerReader(socket).start();
        }
    }
```

创建线程类
```java
package com.xixi.demo6tcp3;

import java.io.DataInputStream;
import java.io.InputStream;
import java.net.Socket;

public class ServerReader extends Thread{
    private Socket socket;
    public ServerReader(Socket socket) {
        this.socket = socket;
    }
    public void run() {
            try {
                //读取管道的信息
                //3.获取输入流，读取客户端发送的数据
                InputStream is = socket.getInputStream();
                //4.把字节输入流包装成特殊字节输入流
                DataInputStream dis = new DataInputStream(is);
                //5.读取数据
                while (true) {
                    String msg = dis.readUTF();//等待读取客户端发送的数据
                    System.out.println(msg);
                    //6.客户端的IP和端口（谁发给我）
                    System.out.println(socket.getInetAddress().getHostAddress()+":"+socket.getPort());
                    System.out.println("------------------");
                }
            } catch (Exception e) {
                System.out.println("客户端断开连接"+ socket.getInetAddress().getHostAddress());
        }
    }
}
```

##### B/S架构的原理
注意：服务器必须给浏览器响应HTTP协议规定的数据格式，否则浏览器不识别返回的数据

![alt test](/image/屏幕截图%202026-07-16%20170127.png)

```java
public class ServerDemo2 {
    public static void main(String[] args) throws Exception {
        //目标：B/S架构原理的理解
        System.out.println("服务端启动了");
        //1.创建一个服务器Socket对象，绑定监听端口，监听客户端连接
        ServerSocket ss = new ServerSocket(8080);

        //2.调用accept方法，堵塞等待客户端连接，一旦有客户端链接会返回一个Socket对象
        while (true) {
            Socket socket = ss.accept();
            System.out.println("一个客户端上线了"+ socket.getInetAddress().getHostAddress());
            //3.把这个客户端管道交给一个独立的子线程专门负责接收这个管道的消息
            new ServerReader(socket).start();
        }
    }
}
```

```java
package com.xixi.demo7tcp4;

import java.io.OutputStream;
import java.io.PrintStream;
import java.net.Socket;

//实际上Thread类本身就可以做任务对象（实现Runnable接口）

public class ServerReaderRunnable implements Runnable{
    private Socket socket;
    public ServerReaderRunnable(Socket socket) {
        this.socket = socket;
    }
    public void run() {
            try {
                //给当前对应的浏览器管道响应一个网页数据回去
                OutputStream os = socket.getOutputStream();
                //通过字节输出流包装写出去数据给浏览器
                //把字节输出流包装成打印流
                PrintStream ps = new PrintStream(os);
                //写响应的网页数据出去
                ps.println("HTTP/1.1 200 OK");
                ps.println("Content-Type:text/html;charset=utf-8");
                ps.println();//必须换一行
                ps.println("<html>");
                ps.println("<head>");
                ps.println("<title>");
                ps.println("java");
                ps.println("</title>");
                ps.println("</head>");
                ps.println("<body>");
                ps.println("<h1 style='color:red;font-size=20ox'>hello,java</h1>");
                ps.println("</body>");
                ps.println("</html>");
                ps.close();
                socket.close();
            } catch (Exception e) {
                System.out.println("客户端断开连接"+ socket.getInetAddress().getHostAddress());
        }
    }
}
```

使用线程池优化

```Java
public static void main(String[] args) throws Exception {
        //目标：B/S架构原理的理解
        System.out.println("服务端启动了");
        //1.创建一个服务器Socket对象，绑定监听端口，监听客户端连接
        ServerSocket ss = new ServerSocket(8080);

        //创建线程池
        ExecutorService es = new ThreadPoolExecutor(3, 10, 10, TimeUnit.SECONDS,new ArrayBlockingQueue<>(100), Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());

        //2.调用accept方法，堵塞等待客户端连接，一旦有客户端链接会返回一个Socket对象
        while (true) {
            Socket socket = ss.accept();
            System.out.println("一个客户端上线了"+ socket.getInetAddress().getHostAddress());
            //3.把这个客户端管道包装成一个任务交给线程池处理
            es.submit(new ServerReaderRunnable(socket));
        }
    }
```

### 时间相关的获取方案

LocalDate：代表本地日期（年、月、日、星期）

LocalTime：代表本地时间（时、分、秒、纳秒）

LocalDateTime：代表本地日期、时间（年、月、日、星期、时、分、秒、纳秒）

```java
public static void main(String[] args) {
        //目标：掌握Java提供的获取时间方案
        //JDK8之前的方案，Date 获取此刻日期时间，老项目还有
        Date d = new Date();
        System.out.println(d);

        //格式化：SimpleDateFormat 简单日期格式化 格式化日期对象成为我们喜欢的格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String result = sdf.format(d);
        System.out.println(result);

        //JDK8之后，LocalDate LocalTime LocalDateTime 获取此刻日期时间 新项目建议使用
        //获取此刻日期时间
        LocalDateTime ld = LocalDateTime.now();
        System.out.println(ld);
        System.out.println(ld.getYear());
        System.out.println(ld.getDayOfYear());

        LocalDateTime ld2 = ld.plusSeconds(60);//60秒后
        System.out.println(ld2);
        System.out.println(ld);

        //格式化：DateTimeFormatter
        //1.创建一个格式化对象
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        //2.格式化ld对象的时间
        String result2 = dtf.format(ld);
        System.out.println(result2);

    }
```

### 字符串的高效操作方案

```java
// + 号拼接字符串内容，如果拼接的字符串内容很多，效率会很低
    //String的对象是不可变对象  共享数据性能可以，但是修改数据性能差
    String s = "";
    for (int i = 0; i < 10000; i++) {
        s =s + "abc";
    }
    System.out.println(s);
```

StringBuilder
* StringBuilder代表可变字符串，相当于是一个容器，它里面装的字符串是可以改变的，就是用来操作字符串的

|方法名称|说明|
|----|----|
|public StringBuilder append(任意类型)|添加数据并返回StringBuilder对象本身|
|public StringBuilder reverse()|将对象的内容反转|
|public int length()|返回对象内容长度|
|public String toString()|通过toString()就可以实现StringBuilder转换为String|

```java
//定义字符串可以用String变量，但是操作字符串建议使用StringBuilder（性能好）
        StringBuilder sb = new StringBuilder();//StringBuilder对象是可变内容的容器 sb="";
        for (int i = 0; i < 10000; i++) {
            sb.append("abc");
        }
        System.out.println(sb);
        //StringBuilder对象只是拼接字符串的手段，结果还是要恢复成字符串
        String s = sb.toString();
        System.out.println(s);

        StringBuffer sf = new StringBuffer();
        StringBuffer result = sf.append("abc").append("张三").append("张三");//链式
        System.out.println(result);
```

### BigDecimal
用于解决浮点型运算时，出现结果失真的问题

|构造器|说明|
|----|----|
|public BigDecimal（double val） 注意：不推荐使用这个|将double转换为BigDecimal|
|public BigDecimal（String val）|把String转成BigDecimal|

|方法名称|说明|
|----|----|
|public static BigDecimal valueOf(double val)|转换一个double成BigDecimal|
|public BigDecimal add(BigDecimal b)|加法|
|public BigDecimal subtract(BigDecimal b)|减法|
|public BigDecimal multiple(BigDecimal b)|乘法|
|public BigDecimal divide(BigDecimal b)|除法|
|public BigDecimal divide(另一个BigDecimal对象，精确几位，舍入几位)|除法，可以控制精确到小数几位|
|public double doubleValue()|将BigDecimal转换为double|

```java
public static void main(String[] args) {
        //目标：掌握BigDecimal解决小数运算结果失真问题
        double a = 0.1;
        double b = 0.2;
        System.out.println(a+b);//0.30000000000000004

        //如何解决？使用BigDecimal
        //1.把小数包装成BigDecimal对象来运算才可以
        //必须使用public BigDecimal(String val)字符串构造器才能解决失真问题
//        BigDecimal a1 = new BigDecimal(Double.toString(a));
//        BigDecimal b1 = new BigDecimal(Double.toString(b));
        //优化方案：可以直接调用valueOf方法，内部使用的就是public BigDecimal(String val)字符串的构造器
        BigDecimal a1 = BigDecimal.valueOf(a);
        BigDecimal b1 = BigDecimal.valueOf(b);
        BigDecimal c = a1.add(b1);//解决精度问题的手段
        double c1 = c.doubleValue();//目的 把BigDecimal对象转换成double类型
        System.out.println(c1);

        System.out.println("--------------");

        BigDecimal a2 = BigDecimal.valueOf(0.1);
        BigDecimal b2 = BigDecimal.valueOf(0.3);
        //除法
        //BigDecimal c2 = a2.divide(b2);//抛异常
        BigDecimal c2 = a2.divide(b2,2, RoundingMode.HALF_UP);
        System.out.println(c2);
    }
```

