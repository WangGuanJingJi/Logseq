- # 介绍
	- **注意：**本章节会涉及到 **操作系统** 相关知识。
	  
	  在了解多线程之前，让我们回顾一下`操作系统`中提到的进程概念：
	  
	  ![img](https://img0.baidu.com/it/u=2613039280,4140201323&fm=26&fmt=auto)
	  
	  进程是程序执行的实体，每一个进程都是一个应用程序（比如我们运行QQ、浏览器、LOL、网易云音乐等软件），都有自己的内存空间，CPU一个核心同时只能处理一件事情，当出现多个进程需要同时运行时，CPU一般通过`时间片轮转调度`算法，来实现多个进程的同时运行。
	  
	  ![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fhiphotos.baidu.com%2Fdoc%2Fpic%2Fitem%2Faec379310a55b3193e6caaf24aa98226cefc179b.jpg&refer=http%3A%2F%2Fhiphotos.baidu.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1637499744&t=1df3c2095bc9a8cbe8cd9d0974644b7c)
	  
	  在早期的计算机中，进程是拥有资源和独立运行的最小单位，也是程序执行的最小单位。但是，如果我希望两个任务同时进行，就必须运行两个进程，由于每个进程都有一个自己的内存空间，进程之间的通信就变得非常麻烦（比如要共享某些数据）而且执行不同进程会产生上下文切换，非常耗时，那么能否实现在一个进程中就能够执行多个任务呢？
	  
	  ![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fs2.51cto.com%2Fwyfs02%2FM00%2F84%2F3A%2FwKiom1eIqY7il2J7AAAyvcssSjs721.gif&refer=http%3A%2F%2Fs2.51cto.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1637474421&t=aef9a39ea3a09d6d67e8d4b769036446)
	  
	  后来，线程横空出世，一个进程可以有多个线程，线程是程序执行中一个单一的顺序控制流程，现在线程才是程序执行流的最小单元，各个线程之间共享程序的内存空间（也就是所在进程的内存空间），上下文切换速度也高于进程。
	  
	  在Java中，我们从开始，一直以来编写的都是单线程应用程序（运行`main()`方法的内容），也就是说只能同时执行一个任务（无论你是调用方法、还是进行计算，始终都是依次进行的，也就是同步的），而如果我们希望同时执行多个任务（两个方法**同时**在运行或者是两个计算同时在进行，也就是异步的），就需要用到Java多线程框架。实际上一个Java程序启动后，会创建很多线程，不仅仅只运行一个主线程：
	  
	  ```java
	  public static void main(String[] args) {
	      ThreadMXBean bean = ManagementFactory.getThreadMXBean();
	      long[] ids = bean.getAllThreadIds();
	      ThreadInfo[] infos = bean.getThreadInfo(ids);
	      for (ThreadInfo info : infos) {
	          System.out.println(info.getThreadName());
	      }
	  }
	  ```
	  
	  关于除了main线程默认以外的线程，涉及到JVM相关底层原理，在这里不做讲解，了解就行。
	  
	  ***