#Java基础复习--多线程



#多线程


##进程与线程
**进程**：进程指正在运行的程序。
**线程**：线程是进程中的一个执行单元，是程序 **使用cpu** 的基本单位（**调度**）。负责当前进程中程序的执行。是进程中单个顺序控制流（执行路径），是一条单独执行的路径

**一个程序运行后至少有一个进程，一个进程中可以包含多个线程**


个人理解线程的概念：
**进程就是工厂里的一条生产线，
线程就是生产线上的可以流动的工人。**

在操作系统中，**进程是资源分配的基本单位，线程是调度的基本单位。**

    在没有出现线程之前，进程既是操作系统进行资源分配的基本单位，又是调度的基本单位


**单线程程序**：若有多个任务只能依次执行。当上一个任务执行结束后，下一个任务开始执行**。程序只有一条执行路径**
**多线程程序**：若有多个任务可以同时执行。**程序有多条执行路径**


**5条执行路径**

 1. **操作系统发展**：单道批处理操作——>多道批处理操作系统——>分时操作系统(多进程的)——>线程
 3. **批处理**：程序在执行过程中，不会响应用户的请求
 4. **单道批处理操作**：一次只能运行一个程序，如果要在计算机运行多个程序，这多个程序，只能一个一个的顺序执行，如果这个正在运行的程序，在运行过程中，执行了一些非常耗时的IO操作(传输数据的过程是没有使用到cpu)，这样一来，cpu就闲下来了，但是cpu是计算机中，最为昂贵的
 4. **多道批处理操作系统**：同时运行多个程序，显著的提高了cpu的利用率，但是我们一旦一个，程序运行起来，都是是需要使用计算机资源的
 5. **分时操作系统**：每一个进程，有一个固定的时间片，在运行一个固定的时间片后，紧接着轮到下一个进程运行（切换）


</n>
**为什么还会有线程呢？**
答：
进程切换的代价太高了，这样一来，每一次进程切换，都需要付出不小的额外的代价，**为了减小进程切换的代价**，引入了线程，**提高CPU的利用率**


##线程

**线程**：线程是进程中的一个执行单元，是程序 **使用cpu** 的基本单位（**调度**）。负责当前进程中程序的执行。是进程中单个顺序控制流（执行路径），是一条单独执行的路径



###线程的一些控制方法

**线程睡眠：**

    static void sleep(long millis)

在指定的毫秒数内让当前正在执行的线程休眠（暂停执行）
```
//让主线程，休眠3秒
Thread.sleep(3000);
//sleep新的写法
TimeUnit.SECONDS.sleep(3);
System.out.println("睡醒了");
```
</n>
**线程加入**


    public final void join()

（让当前线程）等待该线程(新加入的线程终止)终止。

```
SecondThread sec = new SecondThread("zs");
SecondThread thr = new SecondThread("lsii");
SecondThread fou = new SecondThread("wangwu");

//启动这三个线程对象
sec.start();
//sec线程上调用join方法 ，让当前线程（main）等待sec线程执行完毕
sec.join();
thr.start();
fou.start();
```
</n>
**线程礼让**

    public static void yield()

暂停当前正在执行的线程对象，并执行其他线程

    只是让当前线程放弃cpu执行权，但是不能阻止它放弃后继续抢夺cpu执行权

</n>
**后台线程（守护线程）**

    public final void setDaemon(boolean on)

将该线程标记为**守护线程或用户线程**。当正在运行的线程都是守护线程时，Java 虚拟机退出；该方法必须在启动线程前调用。

```
SecondThread sec = new SecondThread("zs");
SecondThread thr = new SecondThread("lsii");
//将sec和thr线程设置为守护线程
sec.setDaemon(true);
thr.setDaemon(true);
//启动
sec.start();
thr.start();
//之后发现sec和thr是守护线程，就会中断
```

</n>
**中断线程**
public void interrupt()
让一个线程控制另外一个线程（有条件的：受阻），可以利用该方法**终止**另一个线程的运行。


 1. 如果线程在调用 Object 类的 wait()、wait(long) 或 wait(long, int) 方法，或者该类的
    join()、join(long)、join(long, int)、sleep(long) 或 sleep(long, int)
    等阻塞方法处于阻塞状态，它还将收到一个 InterruptedException
 2. 中断一个不处于活动状态的线程不需要任何作用。 
中断一个不处于阻塞状态的线程，没有其他任何效果


```
public class Test {
    public static void main(String[] args) {
        FiveThread fiveThread = new FiveThread();
        fiveThread.start();
        //在主线程中，终止fiveThread，休眠状态
        fiveThread.interrupt();
    }
}

// 睡5秒之后，再让该线程输出一句话
class FiveThread extends Thread{
    @Override
    public void run() {
        try {
            //假如申请了很多的系统资源
            TimeUnit.SECONDS.sleep(5);
            System.out.println("FiveThread 睡醒了");
        } catch (InterruptedException e) {
            e.printStackTrace();
            //异常的意义：即使我的线程被异常终止，我也可以保证资源的正常释放
        }
    }
}

//会抛出 java.lang.InterruptedException: sleep interrupted
```





##线程的创建

创建新执行线程的两种方法

 1. 将类声明为 **Thread** 的子类。该子类应重写 Thread 类的 run
    方法。创建对象，开启线程。**run**方法相当于其他线程的main方法。
 2. 声明一个实现 **Runnable** 接口的类。该类然后实现 **run** 方法。然后创建 Runnable的子类对象，传入到某个线程的构造方法中，开启线程。


--------------------------------

    虽然实现线程有两种方式，其实从客观来讲，线程本身只代表独立的执行路径, 执行的具体内容其实是Task本身，和执行路径的实现本身没有联系；只是我们开发者，想将一个task放在某条独立的执行路径(Thread 类对象，也就是一个线程中)来运行

###介绍Thread类

 - Thread是程序中的执行线程。Java 虚拟机允许应用程序并发地运行多个执行线程
 - **不是抽象类**


**构造方法：**
**Thread()**： 分配新的 Thread 对象
**Thread(String name)**：分配新的 Thread 对象，将指定的 name 作为其线程名称

**常用方法：**

**void start()**：使该线程开始执行，Java虚拟机调用线程的 run 方法
</n>
**void run()**：该线程要执行的操作，
</n>
**static void sleep(long millis)**：
在指定毫秒内让当前正在执行的线程休眠，暂停执行
</n>
**static Thread currentThread()**：
返回**当前正在执行的线程对象**的引用Thread.currentThread()

###创建线程：继承Thread类

**创建线程的步骤**

 1. 定义一个类继承 Thread
 2. 重写 run方法
 3. 创建子类对象，就是创建线程对象
 4. 调用 start 方法，开启线程并让线程执行，同时还会告诉jvm去调用 run 方法

线程对象调用 **run方法 不开启线程**。仅是**对象调用方法**。 
线程对象调用 **start 开启线程**，并让 jvm 调用 run 方法在开启的线程中执行


```
//测试类
public class Test {
    public static void main(String[] args) {
        //创建自定义线程对象
        MyThread mt = new MyThread("新的线程！");

        //错误启动线程,这只是普通的方法调用
        //firstThread.run();

        //开启新线程
        mt.start();

        //再次启动一个线程，会抛异常IllegalThreadStateException 
        //因为一个线程对象只能启动一次，
        // 如果同一个线程对象，启动多次，就会抛出异常
        //firstThread.start();
        //只能创建一个新的对象
        new MyThread("第二个线程！").start();
        //获取主线程的名字
        System.out.println(Thread.currentThread().getName()+"：主线程！");
    }
}

//自定义线程
class MyThread extends Thread {
    //定义指定线程名称的构造方法
    public MyThread(String name) {
        //调用父类的String参数的构造方法，指定线程的名称
        super(name);
    }

    //重写run方法，完成该线程执行的逻辑
    @Override
    public void run() {
        //获取线程的名字getName()
        System.out.println(getName() + "：正在执行！");
    }
}
```

###创建线程：实现Runnable接口

**Runnable 接口的构造方法**

    Thread(Runnable target)

分配新的 Thread 对象，以便将 target 作为其运行对象

    Thread(Runnable target,String name) 

分配新的 Thread 对象，以便将 target 作为其运行对象；并将指定的 name 作为其名称

**创建线程的步骤**

 1. 定义类实现 **Runnable** 接口。
 2. 覆盖接口中的 **run方法**
 3. 创建 Thread类的 **对象**
 4. 将 Runnable接口 的**子类对象**作为**参数**传递给 **Thread 类** 的**构造方法**。
 5. 调用 Thread类的 **start()** 开启线程。

-

    Thread 类的一些构造函数：
    1. Thread()： 分配新的 Thread 对象
    2. Thread(String name)：分配新的 Thread 对象，将指定的 name 作为其线程名称
    3.Thread(Runnable target)：Allocates a new Thread object.（就是分配一个新的线程）

Thread类的构造方法，方法，自己看Api,这里写的不够详细。

创建线程代码：
```
//测试类
public class Test {
    public static void main(String[] args) {
        //创建实现 Runnable 接口的子类对象
        MyRunnable myrunnable = new MyRunnable();
        //创建Thread实例，在Thread的构造方法中传递Runnable实例
        //Runnable就代表在 Thread 上运行的任务
        Thread thread = new Thread(myrunnable);
        //开启线程
        thread.start();
        for (int i = 0; i < 10; i++) {
            System.out.println("main线程：正在执行！"+i);
        }
    }
}

//自定义线程执行任务类
class MyRunnable implements Runnable{
    //定义线程要执行的run方法逻辑
    //run方法，不能抛出编译时异常
    //run方法，没有参数没有返回值
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("我的线程：正在执行！"+i);
        }
    }
}
```

**为什么需要定一个类去实现Runnable接口呢？继承Thread类和实现Runnable接口有啥区别呢？** 

答：
    这是实现Runnable接口的原理
    只有创建Thread类的对象才可以创建线程。线程任务已被封装到Runnable接口的run方法中，而这个run方法所属于Runnable接口的子类对象，所以将这个子类对象作为参数传递给Thread的构造函数，这样，线程对象创建时就可以明确要运行的线程的任务

###**两种方式的比较**

**继承 Thread 类方式**
   
 - 如果某个类已经有父类，则无法再继承 Thread 类

**实现 Runnable 接口方式**
    
 1. 解决了方式一的单继承的局限性
 2. 还有一个优点，便于多线程共享数据

    
    第二种方式实现Runnable接口避免了单继承的局限性，所以较为常用。实现Runnable接口的方式，更加的符合面向对象；线程分为两部分，一部分线程对象，一部分线程任务。

</n>

继承Thread类，线程对象和线程任务耦合在一起。一旦创建Thread类的子类对象，**既是线程对象，有又有线程任务**。

实现runnable接口，将线程任务单独分离出来**封装成对象**，**类型就是Runnable接口类型**。Runnable接口对线程对象和线程任务进行**解耦**。



##线程的调度

**分时调度**：所有线程 **轮流使用** CPU 的使用权，平均分配每个线程占用 CPU 的时间。
**抢占式调度**：优先让 **优先级高的** 线程使用 CPU，如果线程的优先级相同，那么会随机选择一个**(线程随机性)，Java使用的为抢占式调度**
体现了：**程序运行的不确定性**

    1. CPU 使用抢占式调度模式在多个线程间进行着·高速的切换。对于CPU的一个核而言，某个时刻，只能执行一个线程，而 CPU的在多个线程间切换速度相对我们的感觉要快，看上去就是在同一时刻运行。
    2. 多线程程序并不能提高程序的运行速度，但能够提高程序运行效率，让CPU的使用率更高。

##线程同步

个人理解线程同步：

前面比喻是  
**线程= 工厂生产线上的工人**
那**线程同步**就是
**生产线上的某个步骤只能某个工人执行**

同步为了防止多个线程同时访问同一个数据对象时，对数据造成破坏（就是多人做某个步骤会破坏生产机器）。

**问题引入——线程安全问题**

发生线程安全问题的**3个条件**

1. **多线程运行环境**
2. 多线程访问**线程共享数据**（存在共享数据）
3. 访问共享数据的操作不是**原子操作**。 
注：原子操作：**不可分割的操作** ，相当于一次性完成的操作

当这三个条件**同时满足**的时候，才会发生多线程的数据安全问题


**解决多线程的线程安全问题**：如何实现线程对共享资源的**排他性访问**（只有我访问完了你们才能修改）？

答：
使用 同步代码块


###同步代码块

同步代码块实现 **线程同步**

**线程同步**：就是利用锁对象，完成多线程运行环境中，对共享资源的排他性访问（我走你不能走， 你走我不能走）

优点：解决了多线程的安全问题
缺点：消耗资源（当线程很多时，每个线程运行的时候都需要去判断同步锁，这个是很耗费系统资源的）


**线程异步**：线程之间，互不干扰，各自独立运行（我走我的，你走你的）


代码格式：
```
synchronized（对象）{
    //critical section
        对共享资源的访问的代码
}
```
1. 同步代码块中所使用的对象，称之为**锁对象** ——> 锁的角色 
锁对象中，有一个**标志位**：可以表示两种状态，**加锁** 和 **解锁**

2. **持有锁的是线程**（一个线程给一个锁对象加锁，我们就说这个线程持有了锁对象）

3. **同步代码块何时给锁对象加锁？**
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  进入同步代码块的同时，就给锁对象加锁

4. **线程何时释放持有的锁？**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当执行完同步代码块的时候，就会释放同步代码块的锁对象

5. **判断和修改**锁对象**标志位**的操作，这是由 jvm **保证**一定是原子操作 
</n>
6. **锁对象，究竟是什么对象？**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;java语言中任意一个对象，都可以充当锁对象的角色（因为任意一个对象中都有一个表示加锁，解锁状态的标志位）


注意点：
虽然锁对象可以是任意对象，但是**针对同一个（同样的多个共享变量）** 的所有操作，都必须保证在同步代码块中，使用**同一个锁对象**，才能避免线程安全问题。


###同步方法

 - 当同步代码块的范围扩大到**整个方法的方法体**的时候，我们可以将整个方法定义成**同步方法**，在**方法声明**上加上**synchronized**

**普通的同步代码块**： 

    synchronized(锁对象) {}

**同步方法**：

    void synchronized 方法名(){}

同步方法的锁对象就是 
this

**静态方法是否可以定义为同步方法？** 
静态的同步方法， 在方法声明上加上`static synchronized` 
它的所使用的**锁对象**是：当前类的字节码文件对象：`类名.class`

 - Lock方法

Java.util.concurrent.locks 中的 Lock 是一个接口，它的实现类是一个 Java 类，而不是作为语言的特性（关键字）来现 。

**Lock锁实现同步的步骤**

1) 创建 Lock 对象---使用**实现类ReentrantLock**

2) 调用 lock()方法上锁

3) 调用 unlock()方法解锁

注意：如果同步代码有异常，要将 unLock() 放到 finally 中。

代码：
```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class CountRunnable implements Runnable {
	private int count = 0;
	//创建一个Lock对象
	Lock lock = new ReentrantLock();
	@Override
	public void run() {
		for (int i = 1; i <= 10; i++) {
			try {
				lock.lock();//加锁
//			synchronized (this) {
				count++;
				try {
					Thread.sleep(300);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				System.out.println(Thread.currentThread().getName() + "执行操作：count=" + count);
//			}
			}finally {
				//确保解锁成功放入finally语句块中
				lock.unlock();//解锁
			}
		}
	}
}
```
测试类：
```
public class Test {
	public static void main(String[] args) {
		CountRunnable cr = new CountRunnable();
		//创建代理类对象
		Thread t1 = new Thread(cr, "A");
		Thread t2 = new Thread(cr, "B");
		Thread t3 = new Thread(cr, "C");
		t1.start();
		t2.start();
		t3.start();
	}
}
```

####Lock 与 synchronized 的区别

1) Lock 是显式锁 (手动开启和关闭锁 ,别忘关闭锁)，synchronized 是隐式锁

2) Lock 只有代码块锁，synchronized 有代码块锁和方法锁

3) 使用 Lock 锁，JVM 将花费较少的时间来调度线程，性能更好，并且具有更好的扩展性（提供更多的子类）

4) Lock 确保当一个线程位于代码的临界区时，另一个线程不进入临界区。如果其他线程试图进入锁定的代码，则它将一直等待(即被阻止)，直到该对象被释放。lock()方法会对 Lock 实例对象进行加锁，因此所有对该对象调用 lock() 方法的线程都会被阻塞，直到该 Lock 对象的 unlock() 方法被调用。

5) Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。Lock可以提高多个线程进行读操作的效率。

6) ReentrantLock可重入锁是一种递归无阻塞的同步锁机制，简单意思就是说可重入锁就是当前持有该锁的线程能够多次获取该锁，无需等待。



##线程间通信

### wait()

 - 导致当前线程处于 **等待状态**
 - 只有在其他线程线程中调用了 `notify()`方法或 `notifyAll()` 来唤醒，因为 `wait()`方法阻塞了线程
 - 在调用 **wait方法** 之前，当前线程**必须拥有**此对象监视器(锁对象)，换句话说，必须在 **锁对象** 上调用**wait方法**（此方法只应该由作为此对象监视器的所有者的线程 来调用）
 - 一旦在锁对象上调用了 wait方法，紧接着：
 -- 当前线程放弃 cpu 执行权，并等待
 -- 放弃持有的 **锁对象**

</n>

####**wait() 和 sleep() 比较** 

**相同点**：
使当前线程放弃cpu执行权，处于阻塞状态 

**不同点**：
1. 线程因为 **sleep()** 方法 处于阻塞状态的时候，**不会放弃所持有的锁对象**；线程因为**wait()** 处于阻塞状态的时候，**会放弃锁对象**
2. **使用条件**： 
    </n>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**sleep()** 没有任何特殊条件； 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用 **wait()** 则必须持有锁，在锁对象上调用 wait() 方法
3. 唤醒条件：
    </n>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**sleep()** 的唤醒条件是休眠时间结束；
    </n>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**wait()** 被唤醒，只能是在其它线程中调用了同一个锁对象的 **notify()** 或者**notifyAll()**方法

###nofity()

 - 唤醒在**此对象(调用wait的同一个锁对象)** 监视器上等待的 单个线程；如果所有线程都在此对象上等待，则会选择唤醒 **其中一个线程**
 - 直到当前线程 **放弃此对象上的锁定** ，才能继续执行被唤醒的线程
 - 被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行 **竞争**



###notifyAll()

唤醒在此对象监视器上等待的 所有线程

##线程的生产者消费者问题
```
/**
 *
 * 在理解线程的创建后，就可以理解这个面包线程了
 *
 * 重点在于理解  面包需要容器来装和执行了Runnable接口的类的构造函数，这是没有想到的。
 *
        1       创建生产者，消费者这2个类，都执行了Runnable接口
        2
 *
 * @author : lenovo
 * @date: 2019/09/30--9:31
 */
public class CuProTest {

    public static void main(String[] args) {
        //先有容器装东西
        rongqi wan = new rongqi();

        //2个类 生产者和消费者
        Producter P = new Producter(wan);
        Consumer C = new Consumer(wan);

        Thread PT = new Thread(P);
        Thread CT = new Thread(C);

        PT.start();
        CT.start();
    }
}
/**

 支持多线程同步操作的堆栈的实现
 */
class rongqi {
    //容器当前有多少个面包
    private int index;
    //装面包的数组，也就是真正的碗/容器
    private char[] data = new char[9];


    //放面包进入容器中，有判断容器的最大值
    public  synchronized void push(char c) {
        //当容器满了的时候，进入等待
        if (index == data.length) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //此处的提醒，是提醒拿到的线程开始工作了，因为线程等待状态才会给这个方法使用的。
        this.notify();
        data[index] = c;
        index++;
    }

    //从容器中拿出面包，有判断容器的最小值
    public synchronized char pop() {
        //当容器里面空了的时候，进入等待
        if (index == 0) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //此处的提醒，是提醒拿到的线程开始工作了，因为线程等待状态才会给这个方法使用的。
        this.notify();
        index--;
        return data[index];
    }

}

class Producter implements Runnable {

    rongqi wan;
    public Producter(rongqi rq) {
        wan = rq;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            char c = (char) (Math.random() * 26 + 'A');
            //把面包放入碗中
            wan.push(c);
            System.out.println("producted: "+c);
            try {
                Thread.sleep((int) (Math.random()* 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer implements Runnable {

    rongqi wan;

    public Consumer(rongqi rq) {
        wan = rq;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            char c = wan.pop();
            System.out.println("吃了面包："+c);
            try {
                Thread.sleep((int) (Math.random()* 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

##线程的生命周期

**新建状态**
创建线程对象


**就绪状态**

 - start() 之后
 - sleep() 睡醒之后
 - yield() 之后
 - 有执行资格，等待cpu调度

**运行状态**

取得执行权，开始执行

**阻塞状态**

无执行资格，无执行权

**死亡状态**

执行完毕，等待垃圾回收

[![](https://ae01.alicdn.com/kf/H6535ee46d00543d5adcd228764543fb8V.png)](https://ae01.alicdn.com/kf/H6535ee46d00543d5adcd228764543fb8V.png)



##线程池

线程池的简单概念：
 - **我们创建一个线程，只能使用一次**
 - **线程池**
其实就是一个容纳多个线程的容器，其中的线程可以 **反复使用**，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源
 - **线程池的原理**
线程池里每一个线程代码结束后，并不会死亡，而是再次回到线程池中成为空闲状态，等待下一次被使用


为什么要使用线程池？
答：
**线程池主要用来解决线程生命周期开销问题和资源不足问题。**


###使用线程池中线程对象的步骤：

创建线程池对象
创建 Runnable 接口/Callable接口 子类对象
提交 Runnable 接口/Callable接口 子类对象
关闭线程池

代码：
```
public class ThreadPool {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //创建一个newCachedThreadPool
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        //操作线程池
        //向线程池提交任务
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                System.out.println("hello, thread pool");
            }
        });

        //Callable接口的使用
        Future<Integer> future = executorService.submit(new Callable<Integer>() {

            @Override
            public Integer call() throws Exception {
                TimeUnit.SECONDS.sleep(3);
                int sum = 0;
                for (int i = 0; i < 10; i++) {
                    sum += i;
                }
                return sum;
            }
        });
        System.out.println(future);
        System.out.println(future.get());
    }
}
```


#总结

非常简单的不完全的一次复盘线程，出现问题之后就会进行更新。



#参考：
参考的大体框架：
https://zhuanlan.zhihu.com/p/39204864
https://zhuanlan.zhihu.com/p/39788456


https://www.cnblogs.com/xdp-gacl/p/3634382.html（生产者消费者问题）





