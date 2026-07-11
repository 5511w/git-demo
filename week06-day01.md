### 多线程

什么是线程？
* 线程(Thread)是一个程序内部的一条执行流程

```java
public static void main(String[] args) {
    //代码
    for (int i = 0; i < 5; i++) {
            System.out.println(i);
        }
    //代码...
}
```

* 程序中如果只有一条执行流程，那这个线程就是单线程的程序

什么是多线程？
* 是指从硬软件上实现的多条执行流程的技术（多条线程有CPU负责调度）

#### 创建线程

1. 创建方式一：继承Thread类
   1. 定义一个子类MyThread继承线程类java.lang.Thread，重写run()方法
   2. 创建MyThread类的对象
   3. 调用线程对象的start()方法启动线程（启动后还是执行run方的）

```java
public static void main(String[] args) {
        //目标：认识多线程，掌握创建线程的方式一：继承Thread类来实现
        //4.创建线程类的对象，代表线程
        Thread mt = new MyThread();
        //5.调用start方法启动线程
        mt.start();//启动线程，让线程执行run方法
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程任务代码" + i);
    }
}


//1，定义一个子类继承Thread类,成为一个线程类
class MyThread extends Thread{
        //2.重写Thread类的run方法
        @Override
        public void run() {
            //3.在run方法中编写线程的任务代码（线程要干的活）
            for (int i = 0; i < 5; i++) {
                System.out.println("子线程任务代码"+i);
        }
    }
}
```

方式一优缺点：

优点：编码简单

缺点：线程类已经继承Thread，无法继承其他类，不利于功能的扩展

注意事项

   1. 启动线程必须是调用start方法，不是调用run方法
      * 直接调用run方法会当成普通方法执行，此时相当于还是单线程执行
      * 只有调用start方法才会启动一个新的线程执行

   2. 不要把主线程任务放在子线程之前

2. 创建方式二：实现Runnable接口
   1. 定义一个线程任务类MyRunnable实现Runnable接口，重写run()方法
   2. 创建MyRunnable任务对象
   3. 把MyRunnable任务对象交给Thread处理
        public Thread(Runnable target) //封装Runnable对象成为线程对象
   4. 调用线程对象start()方法启动线程

```java
public class ThreadDemo2 {
    public static void main(String[] args) {
        //目标：掌握多线程的创建方式二：实现Runnable接口来创建
        //3.创建线程任务类对象代表一个线程任务
        Runnable mr = new MyRunnable();
        //4，把线程任务对象交给一个线程类对象来处理
        //Thread t = new Thread(mr);
        Thread t = new Thread(mr,"线程1");//public Thread(Runnable mr)
        //Thread t = new Thread(mr,"线程1");//public Thread(Runnable mr,String name)
        //5.启动线程
        t.start();
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程任务代码"+i);
        }
    }
}
```

```java
//1.定义一个线程任务类，实现Runnable接口
class MyRunnable implements Runnable {
    //2.重写run方法。设置线程任务
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程任务代码"+i);
        }
    }
}
```

方式二的优缺点
* 优点：任务类只是实现接口，可以继续继承其他类，实现其他接口，扩展性强
* 缺点：需要多一个Runnable对象

2-2. 实现Runnable接口的匿名内部类来创建
    
  1. 可以创建Runnable的匿名内部类对象
  2. 在交给Thread线程对象
  3. 再调用线程对象的start()启动线程   

```java
public static void main(String[] args) {
        //目标：掌握多线程的创建方式二：使用Runnable接口的匿名内部类来创建
        Runnable mr = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println("子线程1任务代码"+i);
                }
            }
        };

        Thread t = new Thread(mr);//public Thread(Runnable mr)
        t.start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println("子线程2任务代码"+i);
                }
            }
        }).start();

        new Thread(() -> {
                for (int i = 0; i < 5; i++) {
                    System.out.println("子线程3任务代码"+i);
                }
        }).start();

        for (int i = 0; i < 5; i++) {
            System.out.println("主线程任务代码"+i);
        }
    }
```

3. 创建方式三：利用Callable接口、FutureTask类来实现

前两种方式都存在一个问题

假如线程执行完毕后有一些数据需要返回，它们重写的run方法均不能直接返回结果

1. 创建任务对象
   定义一个类实现Callable接口，重写call方法，封装要做的事情，和要返回的数据

   把Callable类型的对象封装成FutureTask（线程任务对象）

2. 把线程任务对象交给Thread对象
3. 调用Thread对象的start方法启动线程
4. 线程执行完毕后，通过FutureTask对象的get方法去获取线程任务执行的结果

```java
//1.定义一个线程任务类，实现Callable接口
class MyCallable implements Callable<String> {
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    //2.实现call方法，定义线程执行体
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i < n; i++) {
            System.out.println(i);
            sum += i;
        }
        return "子线程任务"+n+"结果："+sum;
    }
}
```

```java
public static void main(String[] args) {
        //目标：掌握多线程的创建方式三：使用Callable接口来创建，方式三的优势，可以获取线程执行完毕的结果
        //3.创建一个Callable接口的实现类对象
        Callable<String> c1 = new MyCallable(100);
        //4.把Callable对象封装成一个真正的线程任务对象FutureTask对象
        /**
         * 未来任务对象的作用？
         *  a.本质是一个Runnable线程的任务对象，可以实现Thread线程对象处理
         *  b.可以获取线程执行完毕的结果
         */
        FutureTask<String> f1 = new FutureTask<>(c1);//public FutureTask(Callable<V> callable)
        //5.把FutureTask对象作为参数传递给Thread线程对象
        Thread t1 = new Thread(f1);
        //6.启动线程
        t1.start();

        Callable<String> c2 = new MyCallable(50);
        FutureTask<String> f2 = new FutureTask<>(c2);//public FutureTask(Callable<V> callable)
        Thread t2 = new Thread(f2);
        t2.start();

        //7.获取线程执行完毕的结果

        try {
            //如果主线程发现第一个线程还没有执行完毕，会让出CPU，等第一个线程执行完毕后，才会往下执行
            System.out.println(f1.get()); // 获取线程1执行完毕的结果
        } catch (Exception e) {
           e.printStackTrace();
       }
        try {
            //如果主线程发现第二个线程还没有执行完毕，会让出CPU，等第二个线程执行完毕后，才会往下执行
            System.out.println(f2.get()); // 获取线程2执行完毕的结果
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
```

方式三的优缺点
* 优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强；可以再线程执行完毕后获取线程执行的结果
* 缺点：编码复杂

#### 线程的常用方法

|Thread提供的常用方法|说明|
|----|----|
|public void run()|线程的任务方法|
|public void start()|启动线程|
|public String getName()|获取当前线程的名称，线程名称默认是Thread-索引|
|public void setName(String name)|为线程设置名称|
|public static Thread currentThread()|获取当前执行的线程对象|
|public static void sleep(long time)|让当前执行的线程休眠多少毫秒，再继续执行|
|public final void join()|让调用当前这个方法的线程先执行完！|

|Thread提供的常见构造器|说明|
|----|----|
|public Thread(String name)|可以为当前线程指定名称|
|public Thread(Runnable target)|封装Runnable对象成为线程对象|
|public Thread(Runnable traget,String name)|封装Runnable对象成为线程对象吗，并指定线程名称|

```java
public class ThreadApiDemo1 {
    //main方法本身是由一条主线程负责推荐执行的
    public static void main(String[] args) {
        //目标：搞清楚线程的常用方法
        Thread mt = new MyThread("1号");
        //mt.setName("线程1");//线程启动之前设置线程名
        mt.start();
        System.out.println(mt.getName());//线程默认名：Thread-索引

        Thread mt2 = new MyThread("2号");
        //mt2.setName("线程2");
        mt2.start();
        System.out.println(mt2.getName());//线程默认名：Thread-索引

        //哪个线程调用这个代码，这个代码就拿到哪个线程
        Thread m = Thread.currentThread();//获取当前正在执行的线程对象 主线程
        m.setName("主线程");
        System.out.println(m.getName());//main

    }
}

//1，定义一个子类继承Thread类,成为一个线程类
class MyThread extends Thread{
    public MyThread(String name){
        super(name);//public Thread(String name)
    }
        //2.重写Thread类的run方法
        @Override
        public void run() {
        //3.在run方法中编写线程的任务代码（线程要干的活）
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()+"子线程任务代码"+i);
        }
    }
}
```

```java
public static void main(String[] args) {
        //目标：搞清楚线程的join方法，线程插队：让调用这个方法线程先执行
        Thread mt = new MyThread2();
        mt.start();

        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName()+"线程输出" + i);
            if (i == 1) {
                try {
                    mt.join();//插队 让mt线程先执行完毕，再继续执行主线程
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

//1，定义一个子类继承Thread类,成为一个线程类
class MyThread2 extends Thread {
    //2.重写Thread类的run方法
    @Override
    public void run() {
        //3.在run方法中编写线程的任务代码（线程要干的活）
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + "子线程任务代码" + i);
        }
    }
}
```

#### 线程安全问题

什么是多线程问题？
* 多个线程，同时操作同一个共享资源的时候，可能会出现业务安全问题

出现的原因？
* 存在多个线程同时执行
* 同时访问一个共享资源
* 存在修改该共享资源

#### 线程同步
线程同步时线程安全问题的解决方案

让多个线程先后依次访问共享资源，这样就可以避免出现线程安全问题

##### 线程同步的常见方案
加锁：每次只允许一个线程加锁，加锁后才能进入访问，访问完毕后自动解锁，然后其他线程才能再加锁进来

1. 方法一：同步代码块
    作用：把访问共享资源的核心代码给上锁，以此保证线程安全

        synchronized(同步锁){
            访问共享资源的核心代码
        }

    原理：每次只允许一个线程加锁后进入，执行完毕后自动解锁，其他线程才可以进来

    注意事项：对于当前同时执行的线程来说，同步锁必须是同一把（同一个对象），否则会出现bug

```java
//取钱
public void drawMoney(double drawMoney){
        //拿到当前谁来取钱
        String name = Thread.currentThread().getName();
        //判断余额是否充足
        synchronized (this) {
            if (money >= drawMoney){
                //吐出钱
                System.out.println(name+"取钱成功，取钱金额："+drawMoney);
                //更新余额
                money -= drawMoney;
                System.out.println(name+"取钱成功，余额为："+this.money);
            }else{
                //取钱失败
                System.out.println(name+"取钱失败，余额不足！");
            }
        }
}
```

锁对象随便选择一个唯一的对象好不好？
* 不好，会影响其他无关线程的执行

锁对象的使用规范
* 建议使用共享资源作为锁对象，对于实例方法建议使用this作为锁对象
* 对于静态方法建议使用字节码（类名.class）对象作为锁对象

2. 同步方法
    作用：把访问共享资源的核心方法给上锁，以此保证线程安全

    修饰符 synchronized 返回值类类型 方法名称（形参类型）{
        操作共享资源的代码
    }


```java
//取钱
    public synchronized void drawMoney(double drawMoney){
        //拿到当前谁来取钱
        String name = Thread.currentThread().getName();
        //判断余额是否充足
        if (money >= drawMoney){
            //吐出钱
            System.out.println(name+"取钱成功，取钱金额："+drawMoney);
            //更新余额
            money -= drawMoney;
            System.out.println(name+"取钱成功，余额为："+this.money);
        }else{
            //取钱失败
            System.out.println(name+"取钱失败，余额不足！");
        }
    }
```

同步代码好还是同步方法好？
* 范围上：同步代码块锁的范围更小，同步方法锁的范围更大
* 可读性：同步方法更好

3. 方法三：Lock锁
* Lock锁是JDK5开始提供的一个新的锁定操作，通过它可以创建出锁对象进行加锁和解锁，更灵活、更方便、更强大
* Lock是接口，不能直接实例化，可以采用它的实现类对象ReentrantLock来构建Lock锁对象

```java
....
private final Lock lo = new ReentrantLock();//保护锁对象

    //取钱
    public void drawMoney(double drawMoney){
        //拿到当前谁来取钱
        String name = Thread.currentThread().getName();
        lo.lock();//上锁
        //判断余额是否充足
        try {
            if (money >= drawMoney){
                //吐出钱
                System.out.println(name+"取钱成功，取钱金额："+drawMoney);
                //更新余额
                money -= drawMoney;
                System.out.println(name+"取钱成功，余额为："+this.money);
            }else{
                //取钱失败
                System.out.println(name+"取钱失败，余额不足！");
            }
        } finally {
            lo.unlock();//解锁
        }
    }
```

### 线程池
一个可以复用线程的技术

不使用线程池的问题
* 用户每发起一个请求，后台就需要创建一个新线程来处理，下次新任务来了肯定又要创建新线程处理，创建新线程的开销是很大的，并且请求过多时，肯定会产生大量线程出来，这样会严重影响系统的性能

#### 创建线程池
JDK5起提供了代表线程池的接口：ExecutorService

|方法名称|说明|
|----|----|
|void execute(Runnable command)|执行 Runnable 任务|
|Future<T> submit (Callable<T> task)|执行 Callable 任务，返回未来任务对象，用于获取线程返回的结果|
|void shutdown()|等全部任务执行完毕后，在关闭线程池|
|List<Runnable> shutdowmNow()|立刻关闭线程池，停止正在执行的任务，并返回队列中未执行的任务|

如何创建线程池对象？

* 方法一：使用ExecutorService的实现类ThreadPoolExecutor自创建一个线程池对象
* 方法二：使用Executors（线程池的工具类）调用方法返回不同特点的线程池对象

```java
public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime，TimeUnit unit，BlockingQueue<Runnable> workQueue，ThreadFactory threadFactory，RejectedExecutionHandler handler)
```

参数一：corePoolSize : 指定线程池的核心线程的数量   正式工

参数二：maximumPoolSize：指定线程池的最大线程数量   最大员工数

参数三：keepAliveTime ：指定临时线程的存活时间  临时工空闲多久被开除

参数四：unit：指定临时线程存活的时间单位（秒、分、时、天）

参数五：workQueue：指定线程池的任务队列 客人排队的地方

参数六：threadFactory：指定线程池的线程工厂 负责招聘员工的hr

参数七：handler：指定线程池的任务拒绝策略（线程都在忙，任务队列也满了的时候，新任务来了该怎么处理） 忙不过来怎么办？

处理Runnable任务

```java
public static void main(String[] args) {
        //目标：创建线程池对象来使用
        //1，使用线程池的实现类ThreadPoolExecutor声明七个参数来创建线程池对象
        ExecutorService es = new ThreadPoolExecutor(3, 5, 10, TimeUnit.MILLISECONDS, new ArrayBlockingQueue<>(3), new ThreadPoolExecutor.AbortPolicy());

        //2.使用线程池处理任务！看会不会复用线程
        Runnable mr = new MyRunnable();
        es.submit(mr);//提交第一个任务 创建第1个线程 自动启动线程处理这个任务
        es.submit(mr);//提交第二个任务 创建第2个线程 自动启动线程处理这个任务
        es.submit(mr);//提交第三个任务 创建第3个线程 自动启动线程处理这个任务、
        es.submit(mr);//复用线程
        es.submit(mr);
        es.submit(mr);
        es.submit(mr);//到了临时线程的创建时机
        es.submit(mr);//到了临时线程的创建时机
        es.submit(mr);//到了任务拒绝策略了，忙不过来


        //3.关闭线程池 一般不关闭线程池
        //es.shutdown();//等所有任务执行完毕后再关闭线程池

        //es.shutdownNow();//立即停止所有正在执行的任务，并关闭线程池
    }

//1.定义一个线程任务类，实现Runnable接口
public class MyRunnable implements Runnable {
    //2.重写run方法。设置线程任务
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()+"线程"+i);
            try {
                Thread.sleep(Integer.MAX_VALUE);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

线程池的注意事项

什么时候开始创建线程？
    新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程

什么时候会拒绝新任务？
    核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始拒绝任务

任务拒绝策略

|策略|说明|
|----|----|
|ThreadPoolExecutor.AbortPolicy()|丢弃任务并抛出RejectedExecutorException异常。是默认的策略|
|ThreadPoolExecutor.DiscardPolicy()|抛弃任务，但不抛异常，这是不推荐的做法|
|ThreadPoolExecutor.DiscardOldestPolicy()|抛弃队列中等待最久的任务，然后把当前任务加入队列中|
|ThreadPoolExecutor.CallerRunsPolicy()|由主线程负责调用任务的run()方法从而绕过线程池直接执行|

处理Callable任务

```java
public static void main(String[] args) {
        //目标：创建线程池对象来使用
        //1，使用线程池的实现类ThreadPoolExecutor声明七个参数来创建线程池对象
        ExecutorService es = new ThreadPoolExecutor(3, 5, 10, TimeUnit.MILLISECONDS, new ArrayBlockingQueue<>(3), new ThreadPoolExecutor.AbortPolicy());

        //2.使用线程池处理Callable任务
        Future<String> f1 = es.submit(new MyCallable(100));
        Future<String> f2 = es.submit(new MyCallable(200));
        Future<String> f3 = es.submit(new MyCallable(300));
        Future<String> f4 = es.submit(new MyCallable(400));

        try {
            System.out.println(f1.get());
            System.out.println(f2.get());
            System.out.println(f3.get());
            System.out.println(f4.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

//1.定义一个线程任务类，实现Callable接口
class MyCallable implements Callable<String> {
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    //2.实现call方法，定义线程执行体
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i < n; i++) {
            sum += i;
        }
        return Thread.currentThread().getName()+"计算1-"+n+"的和是："+sum;
    }
}
```    

##### 通过Executors创建线程池
是一个线程池工具类，提供了很多静态方法用于返回不同特点的线程池对象

|方法名称|说明|
|----|----|
|public static ExecutorService newFixedThreadPool​(int nThreads)
|创建固定线程数量的线程池，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程替代它|
|public static ExecutorService newSingleThreadExecutor()
|创建只有一个线程的线程池对象，如果该线程出现异常而结束，那么线程池会补充一个新线程|
|public static ExecutorService newCachedThreadPool()
|线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了60s则会被回收掉|
|public static ScheduledExecutorService newScheduledThreadPool​(int corePoolSize)
|创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务|

* 注意 ：这些方法的底层，都是通过线程池的实现类ThreadPoolExecutor创建的线程池对象

```java
public static void main(String[] args) {
        //目标：通过线程池工具类：Executors,调用其静态方法直接得到线程池
        ExecutorService es = Executors.newFixedThreadPool(3);

        Future<String> f1 = es.submit(new MyCallable(100));
        Future<String> f2 = es.submit(new MyCallable(200));
        Future<String> f3 = es.submit(new MyCallable(300));
        Future<String> f4 = es.submit(new MyCallable(400));

        try {
            System.out.println(f1.get());
            System.out.println(f2.get());
            System.out.println(f3.get());
            System.out.println(f4.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

##### Executors使用可能存在的陷阱

![alt test](/image/图片1.png)

### 并发/并行

进程
* 正在运行的程序（软件）就是一个独立的进程
* 线程是属于进程的，一个进程中可以同时运行很多个线程
* 进程中的多个线程其实是并发和并行执行的

并发的含义

进程中的线程是由CPU负责调度执行的，但CPU能同时处理线程的数量有限，为了保证全部线程都能往前执行，CPU会轮询为系统的每个线程服务，由于CPU切换的速度很快，给我们的感觉这些线程在同时执行，这就是并发

并行的理解

在同一个时刻上，同时有多个线程在被CPU调度执行
