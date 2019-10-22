---
title: Java多线程入门
tags: 
- java
- 多线程
categories:
- 线程
date: 2019-09-12 10:17:23
comments: true
---
{% blockquote 原文来源： SnailClimb https://juejin.im/post/5ab116875188255561411b8a#heading-25 Java多线程入门 %}
{% endblockquote %} 
<br>

### 进程和多线程简介

#### 相关概念

##### 何为进程？
<font color=#FF0000>进程是程序的一次执行过程，是系统运行程序的基本单位</font>，因此进程是动态的。<font color=#FF0000>系统运行一个程序即是一个进程从创建，运行到消亡的过程</font>。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如CPU时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。

##### 何为线程？
线程与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是<font color=#FF0000>同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多</font>，也正因为如此，线程也被称为<font color=#FF0000>轻量级进程</font>。

##### 线程和进程有何不同？
线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上<font color=#FF0000>各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响</font>。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。

<!-- more -->

#### 多线程

##### 何为多线程？
多线程就是<font color=#FF0000>几乎同时</font>执行多个线程（一个处理器在某一个时间点上永远都只能是一个线程！即使这个处理器是多核的，除非有多个处理器才能实现多个线程同时运行。）。<font color=#FF0000>几乎同时是因为实际上多线程程序中的多个线程实际上是一个线程执行一会然后其他的线程再执行</font>，并不是很多书籍所谓的同时执行。

##### 为什么多线程是必要的？
1. 使用线程可以把占据长时间的程序中的任务放到后台去处理
2. 用户界面可以更加吸引人，这样比如用户点击了一个按钮去触发某些事件的处理，可以弹出一个进度条来显示处理的进度
3. 程序的运行速度可能加快


### 使用多线程

#### 继承Thread类

MyThread.java
```java
public class MyThread extends Thread {
	@Override
	public void run() {
		super.run();
		System.out.println("MyThread");
	}
}
```

Run.java
```java
public class Run {

	public static void main(String[] args) {
		MyThread mythread = new MyThread();
		mythread.start();
		System.out.println("运行结束");
	}

}
```

运行结果：
{% asset_img threadRun.png 继承Thread类 %}

从上面的运行结果可以看出：线程是一个子任务，CPU以不确定的方式，或者说是以随机的时间来调用线程中的run方法。

#### 实现Runnable接口

推荐实现Runnable接口方式开发多线程，因为Java单继承但是可以实现多个接口。

MyRunnable.java
```java
public class MyRunnable implements Runnable {
	@Override
	public void run() {
		System.out.println("MyRunnable");
	}
}
```

Run.java
```java
public class Run {

	public static void main(String[] args) {
		Runnable runnable=new MyRunnable();
		Thread thread=new Thread(runnable);
		thread.start();
		System.out.println("运行结束！");
	}

}
```
运行结果：
{% asset_img runnableRun.png 实现Runnable接口 %}

### 实例变量和线程安全

定义在线程类中的实例变量针对其他线程可以有共享和不共享之分

#### 不共享数据的情况


MyThread.java
```java
public class MyThread extends Thread {

	private int count = 5;

	public MyThread(String name) {
		super();
		this.setName(name);
	}

	@Override
	public void run() {
		super.run();
		while (count > 0) {
			count--;
			System.out.println("由 " + MyThread.currentThread().getName()
					+ " 计算，count=" + count);
		}
	}
}
```

Run.java
```java
public class Run {
	public static void main(String[] args) {
		MyThread a = new MyThread("A");
		MyThread b = new MyThread("B");
		MyThread c = new MyThread("C");
		a.start();
		b.start();
		c.start();
	}
}
```

运行结果：
{% asset_img threadbugongxiang.png 线程不共享数据 %}


可以看出每个线程都有一个属于自己的实例变量count，它们之间互不影响。我们再来看看另一种情况。

#### 共享数据的情况

MyThread.java
```java
public class MyThread extends Thread {

	private int count = 5;

	@Override
	 public void run() {
		super.run();
		count--;
		System.out.println("由 " + MyThread.currentThread().getName() + " 计算，count=" + count);
	}
}
```

Run.java
```java
public class Run {
	public static void main(String[] args) {
		
		MyThread mythread=new MyThread();
        //下列线程都是通过mythread对象创建的
		Thread a=new Thread(mythread,"A");
		Thread b=new Thread(mythread,"B");
		Thread c=new Thread(mythread,"C");
		Thread d=new Thread(mythread,"D");
		Thread e=new Thread(mythread,"E");
		a.start();
		b.start();
		c.start();
		d.start();
		e.start();
	}
}
```

运行结果：
{% asset_img threadgongxiang1.png 线程共享数据 %}

{% asset_img threadgongxiang2.png 线程共享数据 %}


可以看出这里已经出现了错误


因为在大多数jvm中，count--的操作分为如下下三步：

1，取得原有count值

2，计算i -1

3，对i进行赋值

所以多个线程同时访问时出现问题就是难以避免的了。

那么有没有什么解决办法呢？

答案是：当然有，而且很简单。

在run方法前加上synchronized关键字即可得到正确答案。


### 一些常用方法

#### currentThread()
返回对当前正在执行的线程对象的引用。

#### getId()
返回此线程的标识符

#### getName()
返回此线程的名称

#### getPriority()
返回此线程的优先级

#### isAlive()
测试这个线程是否还处于活动状态。

什么是活动状态呢？

活动状态就是线程已经启动且尚未终止。线程处于正在运行或准备运行的状态。

#### sleep(long millis)
使当前正在执行的线程以指定的毫秒数“休眠”（暂时停止执行），具体取决于系统定时器和调度程序的精度和准确性。

#### interrupt()
中断这个线程。

#### interrupted() 和isInterrupted()
interrupted()：测试当前线程是否已经是中断状态，执行后具有将状态标志清除为false的功能。

isInterrupted()： 测试线程Thread对相关是否已经是中断状态，但不清楚状态标志

#### setName(String name)
将此线程的名称更改为等于参数 name 。

#### isDaemon()
测试这个线程是否是守护线程。

#### setDaemon(boolean on)
将此线程标记为 daemon线程或用户线程。

#### join()
在很多情况下，主线程生成并启动了子线程，如果子线程里要进行大量的耗时的运算，主线程往往将于子线程之前结束，但是如果主线程处理完其他的事务后，需要用到子线程的处理结果，也就是主线程需要等待子线程执行完成之后再结束，这个时候就要用到join()方法了。

join()的作用是：“等待该线程终止”，这里需要理解的就是该线程是指的主线程等待子线程的终止。也就是在子线程调用了join()方法后面的代码，只有等到子线程结束了才能执行

#### yield()
yield()方法的作用是放弃当前的CPU资源，将它让给其他的任务去占用CPU时间。

注意：放弃的时间不确定，可能一会就会重新获得CPU时间片。

#### setPriority(int newPriority)
更改此线程的优先级


### 如何停止一个线程呢？

stop(),suspend(),resume()（仅用于与suspend()一起使用）这些方法已被弃用，所以我这里不予讲解。

{% asset_img zuofei.png 已舍弃的方法 %}

#### 使用interrupt()方法

我们上面提到了interrupt()方法，先来试一下interrupt()方法能不能停止线程

MyThread.java
```java
public class MyThread extends Thread {
	@Override
    public void run() {
        super.run();
        for (int i = 0; i < 1000; i++) {
            System.out.println("i=" + (i + 1));
            try{
                Thread.sleep(100);
            }catch (Exception e){

            }
        }
    }
}
```

Run.java
```java
public class Run {
	public static void main(String[] args) {
		try {
            MyThread thread = new MyThread();
            System.out.println("start");
            thread.start();
            Thread.sleep(2000);
            System.out.println("sleep");
            thread.interrupt();
        } catch (InterruptedException e) {
            System.out.println("main catch");
            e.printStackTrace();
        }
	}
}
```
运行结果：

{% asset_img interruptNotStop.png interrupt方法 %}

运行上诉代码你会发现，线程并不会终止。

针对上面代码的一个改进：

interrupted()方法判断线程是否停止，如果是停止状态则break

MyThread.java
```java
public class MyThread extends Thread {
	@Override
	public void run() {
		super.run();
		for (int i = 0; i < 1000; i++) {
			if (MyThread.interrupted()) {
				System.out.println("已经是停止状态了!我要退出了!");
				break;
			}
			System.out.println("i=" + (i + 1));
		}
		System.out.println("看到这句话说明线程并未终止------");
	}
}
```

Run.java
```java
public class Run {
	public static void main(String[] args) {
		try {
            MyThread thread = new MyThread();
            System.out.println("start");
            thread.start();
            Thread.sleep(2000);
            System.out.println("sleep");
            thread.interrupt();
        } catch (InterruptedException e) {
            System.out.println("main catch");
            e.printStackTrace();
        }
		System.out.println("end!");
	}
}
```

运行结果：

{% asset_img breakNotStop.png 线程未停止 %}

for循环虽然停止执行了，但是for循环下面的语句还是会执行，说明线程并未被停止。

#### 使用return停止线程

MyThread.java
```java
public class MyThread extends Thread {
	@Override
	public void run() {
			while (true) {
				if (this.isInterrupted()) {
					System.out.println("停止了!");
					return;
				}
				System.out.println("timer=" + System.currentTimeMillis());
			}
	}
}
```

Run.java
```java
public class Run {
	public static void main(String[] args) throws InterruptedException {
		MyThread t=new MyThread();
		t.start();
		Thread.sleep(2000);
		t.interrupt();
	}
}
```

运行结果：

{% asset_img returnStop.png 线程停止 %}

线程终止。


### 线程的优先级

每个线程都具有各自的优先级，线程的优先级可以在程序中表明该线程的重要性，如果有很多线程处于就绪状态，系统会根据优先级来决定首先使哪个线程进入运行状态。但这个==并不意味着低优先级的线程得不到运行，而只是它运行的几率比较小==，如垃圾回收机制线程的优先级就比较低。所以很多垃圾得不到及时的回收处理。

<font color=#FF0000>线程优先级具有继承特性比如A线程启动B线程，则B线程的优先级和A是一样的</font>。

<font color=#FF0000>线程优先级具有随机性也就是说线程优先级高的不一定每一次都先执行完</font>。

Thread类中包含的成员变量代表了线程的某些优先级。如Thread.MIN_PRIORITY（常数1），Thread.NORM_PRIORITY（常数5）,
Thread.MAX_PRIORITY（常数10）。其中每个线程的优先级都在Thread.MIN_PRIORITY（常数1） 到Thread.MAX_PRIORITY（常数10） 之间，<font color=#FF0000>在默认情况下优先级都是Thread.NORM_PRIORITY（常数5）</font>。


线程优先级具有继承特性测试代码如下：

MyThread1.java：
```java
public class MyThread1 extends Thread {
	@Override
	public void run() {
		System.out.println("MyThread1 run priority=" + this.getPriority());
		MyThread2 thread2 = new MyThread2();
		thread2.start();
	}
}
```

MyThread2.java：
```java
public class MyThread2 extends Thread {
	@Override
	public void run() {
		System.out.println("MyThread2 run priority=" + this.getPriority());
	}
}
```

Run.java：
```java
public class Run {
	public static void main(String[] args) {
		System.out.println("main thread begin priority="
				+ Thread.currentThread().getPriority());
		MyThread1 thread1 = new MyThread1();
		thread1.start();
	}
}
```

运行结果：

{% asset_img youxianji1.png 线程优先级继承性 %}

Run.java：
```java
public class Run {
	public static void main(String[] args) {
		System.out.println("main thread begin priority="
				+ Thread.currentThread().getPriority());
		Thread.currentThread().setPriority(6);
		System.out.println("main thread end   priority="
				+ Thread.currentThread().getPriority());
		MyThread1 thread1 = new MyThread1();
		thread1.start();
	}
}
```

运行结果：

{% asset_img youxianji2.png 线程优先级继承性 %}

### Java多线程分类

#### 多线程分类

##### 用户线程
运行在前台，执行具体的任务，如程序的主线程、连接网络的子线程等都是用户线程

##### 守护线程
运行在后台，为其他前台线程服务.也可以说守护线程是JVM中非守护线程的 “佣人”。

<font color=#FF0000>特点： 一旦所有用户线程都结束运行，守护线程会随JVM一起结束工作</font>

应用：数据库连接池中的检测线程，JVM虚拟机启动后的检测线程

<font color=#FF0000>最常见的守护线程：垃圾回收线程</font>

#### 如何设置守护线程？

可以通过调用Thead类的setDaemon(true)方法设置当前的线程为守护线程

```
1.  setDaemon(true)必须在start（）方法前执行，否则会抛出IllegalThreadStateException异常
2. 在守护线程中产生的新线程也是守护线程
3. 不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑
```

MyThread.java：
```java
public class MyThread extends Thread {
	private int i = 0;

	@Override
	public void run() {
		try {
			while (true) {
				i++;
				System.out.println("i=" + (i));
				Thread.sleep(100);
			}
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

Run.java：
```java
public class Run {
	public static void main(String[] args) {
		try {
			MyThread thread = new MyThread();
			thread.setDaemon(true);
			thread.start();
			Thread.sleep(5000);
			System.out.println("我离开thread对象也不再打印了，也就是停止了！");
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

运行结果：

{% asset_img shouhuThread.png 守护线程 %}

由运行结果可知，主线程结束后，守护线程也跟着结束。