# 并发

## 实现线程方式

1. 实现Runnable接口

   1. ```java
      public class RunnableThread implements Runnable{
          @Override
          public void run() {
              System.out.println("用实现Runnable接口实现线程");
          }
      }
      
      ```

   2. 实现Runnable接口

   3. 重写run方法

   4. 实现了run方法的实例传到Thread类中

2. 继承Thread类

   1. ```java
      public class ExtendsThread extends Thread{
          @Override
          public void run() {
              System.out.println("用Thread类实现线程");
          }
      }
      
      ```

   2. 直接继承Thread类，重写其中的run方法

3. 线程池创建线程

   1. ```java
      public class DefaultThreadFactory implements ThreadFactory {
      
          public DefaultThreadFactory() {
              SecurityManager s = System.getSecurityManager();
              group = (s != null) ? s.getThreadGroup() : Thread.currentThread().getThreadGroup();
              namePrefix = "pool-" + poolNumber.getAndIncrement() + "-thread-"
          }
      
          @Override
          public Thread newThread(Runnable r) {
              Thread t = new Thread(group, r, namePrefix + threadNumber.getAndIncrement(), 0);
              if(t.isDaemon())
                  t.setDaemon(false);
              if(t.getPriority() != Thread.NORM_PRIORITY)
                  t.setPriority(Thread.NORM_PRIORITY);
              return t;
          }
      }
      ```

   2. 默认采用DefaultThreadFactory

   3. 他会给我们的线程池设置一些默认的值，比如它的名字，它是不是守护线程，以及它的优先级

4. 有返回值的callable

   1. ```java
      public class CallableTask implements Callable<Integer> {
          @Override
          public Integer call() throws Exception {
              return new Random().nextInt();
          }
          
          //创建线程池
          ExecutorService service = Executors.newFixedThreadPool(10);
          Future<Integer> future = service.submit(new CallableTask());
      }
      ```

   2. 有返回值的callable也是一种新建线程的方式

      1. 实现了callable接口，并且给它的泛型设置成integer，然后它会返回一个随机数回来

5. 其他创建方式

   1. 定时器Timer

      1. TimerTask的实现了Runnable接口，Timer内部有个TimerThread继承自Thread因此绕回来还是Thread+

   2. 匿名内部类实现

      1. ```java
          new Thread(new Runnable() {
                     @Override
                     public void run() {
                         System.out.println(Thread.currentThread().getName());
                     }
                 });
         ```

      2. 他不是传入一个已经实例好的runnable对象，而是直接在这边去用一个匿名内部类的方式把需要传入的runnable给实现出来

   3. Lambda表达式

      1. ```java
          new Thread(()-> System.out.println(Thread.currentThread().getName()));
         ```

   4. 它的最终实现都是实现runnable接口或是继承Thread类

**创建线程的本质只有一种：构造Thread类**

* 方法一：实现Runnable接口的run方式，并把Runnable实例作为target对象，传给Thread类，最终调用target.run()
* 方法二：继承Thread类，重写Thread类的run方法，Thread.star()会执行run()

**总结**

1. 可以把不同的内容进行解耦，权责分明
2. 某些情况下可以提升性能，减小开销
3. 继承Thread类想当于限制了代码的未来的可扩展性

## 如何正确的停止线程

### 1.如何启动一个线程

启动线程需要调用Thread类的start()方法，并在run()方法中定义需要执行的任务

**通常情况下**

我们不会手动停止一个线程

而且允许线程运行到整个进程结束，然后让它**自然停止**但依然会有许多特殊的情况需要我们提前停止线程

比如：用户突然关闭程序或程序运行出错

**这种情况下**

即将停止的线程在很多业务场景下仍然很有价值

但是Java没有提供简单易用，能够直接安全停止线程的能力

**对Java而言**

最正确的停止线程的方式是使用**interrupt**

但interrupt**仅仅起到通知被停止线程得到作用**

**对于被停止的线程而言**

它拥有完全的自主权，既可以选择立即停止，也可以选择一段时间后停止，也可以选择压根不停止

**为什么不强制停止？而是通知、协作**

Java希望程序间能偶相互通知、相互协作的管理线程

### 2.如何用interrupt停止线程

```java
public class StopThread implements Runnable{
    @Override
    public void run() {
        int count = 0;
        while (!Thread.currentThread().isInterrupted() && count<1000) {
            System.out.println("count=" + count++);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new StopThread());
        thread.start();
        thread.sleep(5);
        thread.interrupt();
    }
}
```

* 首先判断线程是否被中断
* 然后判断count是否小于1000
* 该线程的工作内容就是打印0~999的数字，每车次循环开始钱检查是否被中断了
* 休眠5毫秒后立即中断线程，该线程会监测到中断信号，于是在还没打印完1000个数的时候就会停下来

**sleep期间是否能够感知到中断通知**

```java
 public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(()->{
            int num = 0;
            try {
                while (!Thread.currentThread().isInterrupted() && num < 1000) {
                    System.out.println(num);
                    num++;
                    Thread.sleep(1000000);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        thread.start();
        thread.sleep(5);
        thread.interrupt();
    }

打印结果：
    java.lang.InterruptedException: sleep interrupted
	at java.lang.Thread.sleep(Native Method)
	at StopThread.lambda$main$0(StopThread.java:24)
	at java.lang.Thread.run(Thread.java:748)
```

当在一个被阻塞的线程（调用sleep或wait等会让线程阻塞的方法）上调用interrupt方法时，阻塞调用将被Interrupted Exception异常中断，将中断标记为设计成false

>不用担心长时间休眠中线程感受不到中断
>
>即使线程还在休眠，仍然能够响应中断通知，并且抛出异常

**两种最佳处理方式**

```java
void subTask{
    try{
        sleep(delay);
    }catch(InterruptedException){}//在这里不处理是绯苍不好的
}
```

>如果我们负责编写的方法需要调用sleep或者wait时，就要求在方法中使用try/catch或在方法签名中声明throws InterruptedException

```java
void subTask throws InterruptedException{
   sleep(delay);
}
```

**方法一**

方法签名抛异常

* 用throws InterruptedException 标记你的方法，不采用try语句块捕获异常，以便于该异常可以传递到顶层，让run方法可以捕获这一异常
* 由于run方法内无法抛出checked Exception(只能用try catch)，所以在方法上抛出异常的好处是，顶层方法必须处理该异常，不能漏掉或被吞掉

**方法二**

```java
private void reInterrupt(){
    try{
        Thread.sleep(2000)
    }catch (InterruptedException e){
        Thread.currentThread().interrupt();
        e.printStackTrace();
    }
}
```

* 在catch语句块中调用Thread.currentThread().Interrupt()函数
* 因为如果线程在休眠期间被中断，那么会自动清除中断信号
* 这是如果手动添加中断信号，中断信号依然可以被捕捉到
* 这样后续执行的方法依然可以检测到这里发生过中断，可以做出相应的处理，整个线程可以正常退出

### 3.为什么用volatile表标记的停止方法是错误的

#### 错误的停止方法

stop(),suspend(),resume()已经被弃用

*  stop会把线程停止，导致任务戛然而止，有风险
* suspend容易导致死锁

