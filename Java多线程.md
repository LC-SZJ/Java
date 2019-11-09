[TOC]

# Java 多线程

## 1. 并发与并行

+ 并发：指两个或多个事件在**同一时间段内**发生（交替执行）。
+ 并行：指两个或多个事件在**同一时刻**发生（同时执行)。

## 2. 线程与进程

+ 进程：是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程；进程也是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。
+ 线程：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。
简而言之：一个程序运行后至少有一个进程，一个进程中可以包含多个线程。

硬盘：永久存储（ROM）
内存：所有的应用程序都需要进入到内存中执行，临时存储（RAM）
点击应用程序，进入到内存当中，占用一些内存执行
进入到内存的程序叫进程
任务管理器 --> 结束进程，那么就把进程从内存中清除了。

**线程概念图**


![线程概念图](https://ae01.alicdn.com/kf/Ha58b5323084b4079aa9405ae7df50cdfE.png)


**线程调度**

+ 分时调度
	所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间。

+ 抢占式调度
	优先让优先级高的线程使用CPU，如果线程的优先级相同，那么会随机选择一个（线程随机性），Java使用抢占式调度。
	

**抢占式调度详解**

多个程序同时运行，“感觉这些软件好像在同一时刻运行着”
实际上，CPU（中央处理器）使用抢占式调度模式在多个线程之间进行高速的切换发，对于CPU的一个核而言，某个时刻，只能执行一个线程，而CPU的多个线程间切换速度相对我们的感觉要快，看上去就是在同一时刻运行。

其实，多线程并不能提高程序的运行速度，但能够提高程序的运行效率，让CPU的使用率更高。

**每个线程都有自己独立的栈空间**

## 3. 创建线程类

主线程：执行主（main）方法的线程。

单线程程序：java程序中只有一个线程，执行从main方法开始，从上到下依次执行。
JVM执行main方法，main方法会进入栈内存，JVM会找操作系统开辟一条main方法通向CPU的路径，CPU就可以通过这个路径来执行main方法，这个路径叫做主线程。

### 创建多线程的第一种方式：创建Thread类的子类

+ java.lang.Thread类：是描述线程的类，我们想要实现多线程程序，就必须继承Thread类。
+ 实现步骤：
    1. 创建一个Thread类的子类
    2. 在Thread类的子类中重写 **run**方法，设置线程任务（开启线程要做什么）
    3. 创建Thread类的**子类对象**
    4. 调用Thread类中的方法**start**方法，开启新的线程，执行run方法。

+ void start()：使该线程开始执行；Java虚拟机调用该线程的run方法。
结果是两个线程并发的运行，当前线程（main线程）和另一个线程（新创建的线程，执行其中的run方法）
+ 多次启动一个线程是非法的。特别是当线程已经结束执行后，不能再重新启动。 
+ java程序属于抢占式调度，那个线程优先级高，那个线程先执行；同一个优先级，随机选择一个线程执行。

**示例：**

```java
// 1. 创建一个Thread类的子类
public class MyThread extends Thread{

    // 2. 重写Thread类的run方法
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println("run -- > " + i);
        }
    }
}

public class Demo01Thread {
    public static void main(String[] args) {
       // 3. 创建Thread类的子类对象
       MyThread mt = new MyThread();
       // 4. 调用Thread类的start方法; JVM会自动调用run方法
       mt.start();

        for (int i = 0; i < 20; i++) {
            System.out.println("main -- > " + i);
        }
    }
}
// 结果：随机性打印
```



### 多线程的原理

+ 随机性打印
    两个线程，一个main线程，一个新线程一起抢夺CPU的执行权（执行时间），谁抢到了谁执行对应的代码

+ 多线程内存图解

![多线程内存图解](https://ae01.alicdn.com/kf/He1520a8b448d40d68769a24f358b92adt.png)


### Thread类的常用方法

+ 获取线程名称
    1. 使用Thread类中的方法getName()： 
        String getName() ：返回该线程的名称。 
    2. 可以先获取到当前正在执行的线程，使用线程中的方法getName()获取线程名称
        static Thread currentThread() ：返回对当前正在执行的线程对象的引用。 
    
```java
// 1. 创建一个Thread类的子类
public class MyThread extends Thread{

    // 2. 重写Thread类的run方法
    @Override
    public void run() {
        String name = getName();
        System.out.println("新线程名称：" + name);
    }
}
// =================
public class Demo02GetThreadName {
    public static void main(String[] args) {
        MyThread mt = new MyThread();
        mt.start();
        System.out.println(new Thread().getName());
        System.out.println(new Thread().getName());
        System.out.println("主线程名称：" + Thread.currentThread().getName());
    }
}


/*
结果：
Thread-1
Thread-2
主线程名称：main
新线程名称：Thread-0
*/
```

+ 设置线程名称（了解）

    1. 使用Thread类中的方法 setName(名字)
        void setName(String name) ：改变线程名称，使之与参数 name 相同。 
    2. 创建一个带参数的构造方法，参数传递线程的名称，调用父类的带参构造方法，把线程名称传递给父类，让父类（Thread）给子线程起一个名字。
        Thread(String name) ：分配新的 Thread 对象。

+ sleep
     static void sleep(long millis) ：在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。 

```java
public class Demo01Sleep {
    public static void main(String[] args) {
        for (int i = 1; i <= 60; i++){
            System.out.println(i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 创建多线程的第二种方式：实现Runnable接口

+ java.lang.Runnable
+ Runnable 接口应该由那些打算通过某一线程执行其实例的类实现。类必须定义一个称为 run 的无参方法。


Java.lang.Thread类的构造方法

+ Thread(Runnable target)：分配新的 Thread 对象
+ Thread(Runnable target, String name)：分配新的 Thread 对象

实现步骤：

1. 创建一个Runnable接口的实现类。
2. 在实现类中重写Runnable接口中的 run方法，设置线程任务。
3. 创建一个Runnable接口的实现类对象。
4. 创建Thread类对象，构造方法中传递Runnable接口的实现类对象。
5. 调用Thread类中的 start方法，开启新的线程执行run方法。

```java
public class RunnableImpl implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println(Thread.currentThread().getName()+"-->"+i);
        }
    }
}

// ========================
public class Demo01Runnable {
    public static void main(String[] args) {
        RunnableImpl run = new RunnableImpl();
        Thread t = new Thread(run);
        t.start();
        for (int i = 0; i < 20; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

### 实现Runnable接口创建多线程程序的好处

1. 避免了单继承的局限性。
    一个类只能继承一个类（一个人只能有一个亲爹），类继承了Thread类就不能继承其他的类。
    实现Runnable接口，还可以继承其他的类，实现其他的接口。
2. 增强了程序的扩展性，降低了程序的耦合性（解耦）
    实现Runnable接口的方式，把设置线程任务和开启新线程进行了分离（解耦）
    实现类中，重写了 run 方法：用来设置线程任务。
    创建Thread类对象，调用 start 方法：用来开启新线程。
    
    
> 扩充：
>
> 1. 在 java中，每次程序运行至少启动 2 个线程。一个是 main 线程，一个是垃圾收集线程。因为每当使用 java 命令执行一个类的时候，实际上都会启动一个 JVM ，每一个 JVM 其实就是在操作系统中启动了一个进程。
> 2. 如果一个类继承 Thread 类，则不适合资源共享。但是如果实现了 Runnable 接口的话，则很容易实现资源共享。
> 3. 线程池只能放入实现 Runnable 或 Callable 类线程，不能直接放入继承 Thread 的类。

## 4. 匿名内部类方式实现线程的创建

+ 匿名：没有名字
+ 内部类：写在其他类内部的类

+ 匿名内部类的作用：简化代码
    把子类继承父类，重写父类的方法，创建子类对象，一步完成
    把实现类实现类接口，重写接口中的方法，创建实现类对象，一步完成

+ 匿名内部类的最终产物：子类对象 / 实现类对象 ，而这个类没有名字
+ 格式：
```java
    new 父类 / 接口(){
        重复父类 / 接口中的方法
        };
```

+ 示例：

```java
public class DemoInnerClassThread {
    public static void main(String[] args) {
        // 使用匿名内部类实现继承方式
        new Thread(){
            @Override
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println(Thread.currentThread().getName() + "--> 传智播客");
                }
            }
        }.start();
        // 使用匿名内部类实现接口方式
        Runnable r = new Runnable(){

            @Override
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println(Thread.currentThread().getName()+"--> 黑马程序员");
                }
            }
        };
        new Thread(r).start();
		// 使用匿名内部类实现接口方式
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println(Thread.currentThread().getName() + "--> 阿里巴巴");
                }
            }
        }).start();

    }
}
```


## 5. 线程安全

**线程安全问题的概述：前提：多线程程序访问共享数据。**

线程安全问题代码实现：

```java
public class RunableImpl implements Runnable {
    private int ticket = 1;

    @Override
    public void run() {
        // 使用死循环，让买票操作重复执行
        while (true) {
            if (ticket > 0) {
                // 提高线程安全问题出现的概率，可有可无
                try {
                    Thread.sleep(20);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName()+"-->正在买第"+ticket+"张票");
                ticket--;
            }

        }
    }
}
// ===================

public class Demo01Ticket {
    public static void main(String[] args) {
        // 创建 Runnable接口的实现类对象
        RunableImpl run = new RunableImpl();
        // 创建 Thread类对象，构造方法中传递 Runnable接口的实现类对象
        Thread t0 = new Thread(run);
        Thread t1 = new Thread(run);
        Thread t2 = new Thread(run);
		// 调用start方法，开启线程
        t0.start();
        t1.start();
        t2.start();

    }
}
```


多线程安全问题产生的原理：
  在上述代码中，让程序休眠，这个线程就失去了对CPU的执行权，等到其中某一个线程醒来，会继续执行代码，此线程结束，另一个线程醒来，继续执行，就会出现不可存在的票（0，-1），也有可能同时打印同一张票。

**【注意】**：
线程安全问题是不能产生的，我们可以让一个线程在访问共享数据的时候，无论是否失去了对CPU的执行权，让其他的线程只能等待，等待当前线程完成，其他线程再继续买票。
保证：使用一个线程在执行买票。


## 6. 解决线程安全问题

解决线程安全问题的三个方法：

+ 同步代码块
+ 同步方法
+ Lock锁

### 1. 同步代码块

+ 格式：

```java
	synchronized (锁对象) {
		可能会出现线程安全问题的代码（访问了共享数据的代码）
	}
```

> 注意事项：
> 1. 同步代码块中的锁对象，可以是任意的对象。
> 2. 必须保证多个线程使用的锁对象是同一个。
> 3. 锁对象作用：把同步代码块锁住，只让一个线程在同步代码块中执行。
> 

```java
public class RunnableImpl implements Runnable {
    // 定义一个多线程共享的票源
    private int ticket = 100;
    // 创建一个锁对象
    Object obj = new Object();
    // 设置线程任务：买票
    @Override
    public void run() {
        while (true) {
            synchronized (obj) {
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "正在买第" + ticket + "张票");
                    ticket--;
                }
            }
        }
    }
}
// ==============

public class Demo01SaleTicket {
    public static void main(String[] args) {
        // 创建一个Runnable的实现类对象
        RunnableImpl run = new RunnableImpl();
        // 创建一个Thread类对象，在构造方法中传递Runnable的实现类对象
        Thread t0 = new Thread(run);
        Thread t1 = new Thread(run);
        Thread t2 = new Thread(run);

        // 调用start方法，开启新线程
        t0.start();
        t1.start();
        t2.start();
    }
}
```

**同步技术的原理：**
  使用了一个锁对象，这个锁对象叫同步锁，也叫对象锁，也叫对象监视器。多个线程一起抢夺CPU的执行权，谁抢到了，谁就执行 run方法，遇到 synchronized代码块，这是（抢到CPU执行圈的）线程会检查是否有锁对象，**发现有：**就会获取锁对象，进入到同步中执行。**发现没有：**该线程就会进入到***堵塞***状态，一直等待拿到锁对象的线程***归还***锁对象，才能进入到同步中执行。

***总结：同步中的线程，没有执行完毕不会释放锁，同步外的线程没有锁进不去同步。***


### 2. 同步方法

#### 普通同步方法

+ 同步方法：使用 synchronized 修饰的方法，叫做同步方法。保证A线程执行该方法的时候，其他线程只能在方法外等着。
+ 格式：
```java
修饰符 synchronized 返回值类型 方法名称(参数列表){
		可能会出现线程安全问题的代码（访问了共享数据的代码）	
}
```

+ 使用步骤：
    1. 把访问了共享数据的代码抽取出来，放到一个方法中。
    2. 在方法的修饰符后面添加 synchronized 关键字。
+ 示例：
```java
public class RunnableImpl implements Runnable{
    private int ticket = 100;
    @Override
    public void run() {
       while (true) {
          payTickets();
       }
    }
    /*
        定义一个同步方法
        同步方法也会把方法内部的代码锁住
        只让一个线程执行
        同步方法的对象是谁？
        就是实现类对象：new RunnableImpl();
        也就是：this
     */
    public /*synchronized*/ void payTickets(){
        synchronized (this){
            if (ticket > 0){
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName()+"正在买第"+ticket+"张票");
                ticket--;
            }

        }
    }
}
// ============

public class Demo01SaleTicket {
    public static void main(String[] args) {
        // 创建一个Runnable实现类的对象
        RunnableImpl run = new RunnableImpl();

        // 创建Thread类对象，将Runnable实现类对象作为Thread类的构造方法传递过去
        Thread t0 = new Thread(run);
        Thread t1 = new Thread(run);
        Thread t2 = new Thread(run);

        // 调用Thread类的start方法，开启新线程
        t0.start();
        t1.start();
        t2.start();
    }
}
```

#### 静态同步方法

```java
  /*
    静态同步方法
    锁对象不能是：this
    this是创建对象之后产生的，静态方法优先于对象
    静态方法的锁对象是本类的 class 属性 --> class 文件对象（反射要学）
     */
    public static /*synchronized*/ void payTicketsStatic() {
        synchronized (RunnableImpl.class) {
            if (ticket > 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "正在买第" + ticket + "张票");
                ticket--;
            }
        }
    }
    
```

### 3. Lock锁

+ java.util.concurrent.locks.Lock接口
+ Lock 实现提供了比使用 synchronized 方法和语句可获得的更广泛的锁定操作。
+ Lock接口中的方法：
    + void lock()：获取锁。
    + void unlock()：释放锁。
+ java.util.concurrent.locks.ReentrantLock implements Lock接口
+ 使用步骤：
    1. 在成员位置创建一个ReentrantLock对象。
    2. 在有可能出现安全问题的代码前，调用 Lock 接口中的方法，lock()获取锁。
    3. 在有可能出现安全问题的代码后，调用 Lock 接口中的方法，unlock()s释放锁。

+ 示例：

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class RunnableImpl implements Runnable{
    private int ticket = 100;
    // 1. 使用多态，获取 Lock接口的实现类对象
    Lock l = new ReentrantLock();

    /*@Override
    public void run() {
        while (true) {
            // 2. 获取锁
            l.l();
            if (ticket > 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName()+"正在买第"+ticket+"张票");
                ticket--;
            }
            // 释放锁
            l.unlock();
        }
    }*/

    /*
        更优的写法
     */

    @Override
    public void run() {
        while (true) {
           l.lock();
            if (ticket > 0) {
                try {
                    Thread.sleep(10);
                    System.out.println(Thread.currentThread().getName()+"正在买第"+ticket+"张票");
                    ticket--;
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    l.unlock(); // 无论程序是否异常，都会把锁释放
                }
            }
        }
    }
}

// ===================

public class Demo01SaleTickets {
    public static void main(String[] args) {
        // 创建一个Runnablke接口的实现类对象
        RunnableImpl run = new RunnableImpl();
        // 创建一个Thread类对象，将Runnbale接口的实现类对象作为Thread类的构造方法传递过去
        Thread t0 = new Thread(run);
        Thread t2 = new Thread(run);
        Thread t1 = new Thread(run);

        // 调用 Thread类的start方法，开启新线程
        t0.start();
        t1.start();
        t2.start();

    }
}
```

## 7. 线程状态

### 线程的状态图


线程状态。线程可以处于下列状态之一： 

+ NEW
    至今尚未启动的线程处于这种状态。 
+ RUNNABLE
    正在 Java 虚拟机中执行的线程处于这种状态。 
+ BLOCKED
    受阻塞并等待某个监视器锁的线程处于这种状态。 
+ WAITING
    无限期地等待另一个线程来执行某一特定操作的线程处于这种状态。 
+ TIMED_WAITING
    等待另一个线程来执行取决于指定等待时间的操作的线程处于这种状态。 
+ TERMINATED
    已退出的线程处于这种状态。 

在给定时间点上，一个线程只能处于一种状态。这些状态是虚拟机状态，它们并没有反映所有操作系统线程状态。 

**线程状态图**


![线程的状态图](https://ae01.alicdn.com/kf/Hcfcef7c96151412285b19615d3ca0190A.png)




### Timed_Waitting(计时等待)

![计时等待状态](https://ae01.alicdn.com/kf/H4ba24536c24f4598930209dbd659fd13Q.png)

### Blocked(锁阻塞)

![锁堵塞状态](https://ae01.alicdn.com/kf/H22c37a4231b4422a93e426b40efdca16T.png)

### Waitting(无限等待状态)

+  void wait() ：在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。
+   void notify()：唤醒在此对象监视器上等待的单个线程，会继续执行 notify() 方法之后的代码。

+ 等待唤醒案例：线程之间的通信

```java
/*
等待唤醒案例：线程之间的通信
创建一个顾客线程：告知老板买包子的种类和数量，调用wait方法，放弃cpu的执行，进入Waitting状态
创建一个老板线程：花了5秒中做包子，做好包子之后，调用notify方法，唤醒顾客吃包子

【注意事项】：
1. 同步使用的锁对象必须保证唯一
2. 只有锁对象才能使用 wait()和notify()方法

 */
public class Demo1WaitAndNotify {

    public static void main(String[] args) {
        // 创建锁对象
        Object obj = new Object();
        // 创建顾客线程
        new Thread() {
            @Override
            public void run() {
                while (true) {
                    // 保证等待和唤醒的线程只能有一个执行，使用同步技术
                    synchronized (obj) {
                        System.out.println("告知老板包子的种类和数量");
                        try {
                            /*
                             调用 wait方法，进入无限等待状态
                             wait方法不能 throws,因为父类 Thread 的 run方法没有将异常抛出
                             */
                            obj.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    // 唤醒之后，执行的代码
                    System.out.println("包子已经做好了，开吃！");
                    System.out.println("=====================");
                }
            }
        }.start();
        // 老板
        new Thread() {
            @Override
            public void run() {
                while (true) {
                    // 花 5 秒中做包子
                    try {
                        Thread.sleep(5000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (obj) {
                        System.out.println("老板5秒中之后做好包子，可以吃包子了！");
                        // 唤醒顾客
                        obj.notify();
                        System.out.println("唤醒之后，这段代码还会不会执行");
                    }
                }
            }
        }.start();
    }
}
```

进入到 Timed_Waitting（计时等待）有两种方式：

1. 使用Thread类中的 sleep(long m)方法：在毫秒值结束之后，线程睡醒进入到Runnable / Blocked 状态。
2. 使用Object类中的 wait(long m)方法：wait 带参方法如果在毫秒值结束之后，没有被 notify 方法唤醒，就会自动醒来，线程睡醒进入到Runnable / Blocked 状态。

唤醒的方法：

1. void notify()：唤醒在此对象监视器上等待的单个线程。
2. void notifyAll()：唤醒在此对象监视器上等待的所有线程。


## 8. 等待唤醒机制


### 线程间的通信

+ 概念：多个线程在处理同一个资源，但是处理的动作（线程的任务）却不相同。

为什么要处理线程间通信？
  多个线程并发执行时，在默认情况下CPU是随机切换线程的，当我们需要多个线程来共同完成同一任务时，并且我们希望它们有规律的执行，那么多线程之间需要一些协调通信，依次来帮助我们达到多线程操作同一份数据。

如何保证线程间通信有效利用资源？
  多线程在处理同一资源，并且任务不同时，需要线程通信来帮助解决线程之间对同一变量的使用或操作。就是多线程在操作同一份数据时，避免对同一共享变量的争夺，也就是我们要通过一定的手段来有效利用资源，而这种手段即——**等待唤醒机制**。


### 等待唤醒机制

等待唤醒中的方法：

+ void wait()：让此线程进入到 wait_set 中，进入到 Waitting 状态
+ void notify()：唤醒睡得最久的一个
+ void notifyAll()：唤醒所有等待的线程

> 注意事项：
> 哪怕只通知了一个等待的线程，被通知的线程也不能立即恢复执行，因为它当初中断的地方是在同步代码块内，而此刻它已经不再持有锁，所以它需要再次去尝试获取锁（很可能面临和其他线程的竞争），成功之后才能在当初调用 wait 方法之后的地方恢复执行。
> 总结如下：
> 1. 如果能获取锁，线程就从 WAITTING 状态 变成 RUNNABLE 状态。
> 2. 否则，从 wait_set 出来，又进入到 entry_set，线程就从 WAITTING 状态 变成 BLOCKED 状态。
> 

**调用 notify 和 wait 方法需要注意的细节**
1. wait 方法和 notify 方法必须有同一个锁对象调用。因为：对应的锁对象可以通过 notify 唤醒使用同一个锁对象调用的 wait 方法后的线程。
2. wait 方法和 notify 方法是属于 Object 类的方法。所以：锁对象可以是任意对象，而任意对象所属的类都是继承了 Object 类的。
3. wait 方法和 notify 方法必须要在同步代码块 / 同步函数中使用。因为：必须通过锁对象调用这两个方法。

### 等待唤醒机制案例

+ 包子类
+ 包子铺类
+ 吃货类
+ 测试类


包子类：

```java
/*
包子类
 */
public class BaoZi {
    // 包子的皮
    String pi;
    // 包子的馅
    String xian;
    // 包子的状态
    boolean flag = false;

}
```
包子铺类：

```java
/*
包子铺类：生产包子
 */
public class BaoZiPu extends Thread {
    // 在成员位置创建一个包子对象
    private BaoZi bz;

    // 使用带参数的构造方法，为这个包子变量赋值
    public BaoZiPu(BaoZi bz) {
        this.bz = bz;
    }

    // 设置线程任务(run)，生产包子
    @Override
    public void run() {
        int count = 0;
        // 让包子铺一直生产包子
        while (true) {
            synchronized (bz) {
                if (bz.flag == true) {
                    // 有包子，进入Waitting状态
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 没有包子，做包子，交替生产两种包子
                if (count % 2 == 0) {
                    // 生产 薄皮三鲜包子
                    bz.pi = "薄皮";
                    bz.xian = "三鲜";
                } else {
                    // 生产 冰皮牛肉大葱馅
                    bz.pi = "冰皮";
                    bz.xian = "牛肉大葱";
                }
                count++;
                System.out.println("包子铺正在生产" + bz.pi + bz.xian + "包子");
                // 生产包子的时间 3 秒
                try {
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                // 修改包子的状态为：有
                bz.flag = true;
                // 唤醒吃货类，让吃货线程吃包子
                bz.notify();
                System.out.println("包子铺已经生产好了：" + bz.pi + bz.xian + "包子，吃货可以开始吃了!");


            }
        }
    }
}
```
吃货类：

```java
/*
吃货类：吃包子
 */
public class ChiHuo extends Thread{
    private BaoZi bz;

    public ChiHuo(BaoZi bz){
        this.bz = bz;
    }

    // 开启线程任务：吃包子
    @Override
    public void run() {
        // 死循环，一直吃包子
        while (true) {
            // 同步技术
            synchronized (bz) {
                // 对包子的状态进行判断
                if (bz.flag == false) {
                   // 没有包子，进入等待状态
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 被唤醒之后执行的代码：吃包子
                System.out.println("吃货正在吃"+bz.pi+bz.xian+"包子");
                // 吃完之后改变包子的状态
                bz.flag = false;
                // 唤醒包子铺，生产包子
                bz.notify();
                System.out.println("吃货已经把"+bz.pi+bz.xian+"包子吃完了,包子铺已经开始做包子了。");
                System.out.println("===============================================");
            }
        }
    }
}
```

测试类：

```java
public class DemoMain {
    public static void main(String[] args) {
        // 创建包子对象，作为锁对象
        BaoZi bz = new BaoZi();
        // 创建包子铺线程
        new BaoZiPu(bz).start();
        // 创建吃货线程
        new ChiHuo(bz).start();
    }
}
```

## 9. 线程池

### 线程池的概念

+ 线程池：其实就是一个容纳多个线程的容器，其中的线程可以反复使用，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源。

线程池中有很多操作都是与优化资源相关的。

### 线程池的原理

线程池：容器 --> 集合（ArrayList、HashSet、LinkedList、HashMap）
LinkedList`<Thread>`
当程序第一次启动的时候，创建多个线程，保存到一个集合中，当我们想要使用线程的时候 ，就可以从集合中取出线程来使用。
```java
Thread t = list.remove(0); // 返回的是被移除的元素（线程只能被一个任务使用）
Thread t = LinkedList.removeFirst();
// 当我们使用完毕线程后，需要将线程归还给线程池
list.add();
LinkedList.addLast();
```

> 注意：在 JDK 1.5 之后，JDK内置了线程池，我们可以直接使用。
> 

合理使用线程的好处：
1. 降低资源消耗。
2. 提高响应速度。
3. 提高线程的可管理性。


### 线程池的使用

+ 线程池：JDK 1.5之后
+ java.util.concurrent.Executors：线程池的工厂类，用来生产线程池。
+ Executors中的静态方法：
    + static ExecutorService newFixedThreadPool(int nThreads) ：创建一个可重用固定线程数的线程池，以共享的无界队列方式来运行这些线程。 
    + 参数：int nThreads：创建线程池中包含的线程数量。
    + 返回值：ExecutorService接口：返回的是 ExecutorService 接口的实现类对象，可以使用 ExecutorService 接口来接收（面向接口编程）

+ java.util.concurrent.ExecutorService：线程池接口。
+ 用来从线程池中获取线程，调用 start 方法，执行线程任务：submit(Runnable task) ： 提交一个 Runnable 任务用于执行
+ 关闭 / 销毁 线程池的方法： void shutdown() 

**线程池的使用步骤：**
1. 使用线程池的工厂类 Executors 里边提供的静态方法 newFixedThreadPool 生产一个指定线程数量的线程池。
2. 创建一个类，实现 Runnable 接口，重写 run方法，设置线程任务。
3. 调用 ExecutorService 接口中的方法 submit，传递线程任务（实现类），开启线程，执行 run方法。
4. 调用 ExecutorService 接口中的方法 shutdown，销毁线程池（不建议执行）

+ 示例：
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Demo01ThreadPool {
    public static void main(String[] args) {
        // 1. 创建一个指定数量的线程池：Executors中的静态方法： newFixedThreadPool
        ExecutorService es = Executors.newFixedThreadPool(2);
        // 3. 调用 ExecutorService 中的方法 submit，传递线程任务，开启线程，执行 run 方法
        // 线程池会一直开启，使用完了线程，会自动把线程归还给线程池，线程可以继续使用
        es.submit(new RunnableImpl()); // pool-1-thread-2--> 开启新线程，执行任务
        es.submit(new RunnableImpl()); // pool-1-thread-1--> 开启新线程，执行任务
        es.submit(new RunnableImpl()); // pool-1-thread-1--> 开启新线程，执行任务
    }
}

// =================
// 2. 实现 Runnable接口，重写 run方法，设置线程任务
public class RunnableImpl implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "--> 开启新线程，执行任务");
    }
}
```

