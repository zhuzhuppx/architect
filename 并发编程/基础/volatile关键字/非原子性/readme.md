架构师
并发编程
基础
 
  
 

用户评论55
 
 发表评论 
 
码帝
发布于 2019-09-12 10:08:05
架构师
架构师课程视频学习笔记，持续更新。
 架构师
举报
包含该模版的主题
volatile关键字虽然拥有多线程之间的可见性，但是却不具备同步性（也就是原子），可以算上是一个轻量的synchronized,性能要比synchronized强很多，不会造成阻塞。
一般volatile用于只针对于多个线程可见的变量操作，并不能代替synchronized的同步功能

/**
 * volatile关键字不具备synchronized关键字的原子性（同步）
 */
public class VolatileNoAtomic extends Thread{
    //private static volatile int count;
    private static AtomicInteger count = new AtomicInteger(0);
    private static void addCount(){
        for (int i = 0; i < 1000; i++) {
            //count++ ;
            count.incrementAndGet();
        }
        System.out.println(count);
    }

    public void run(){
        addCount();
    }

    public static void main(String[] args) {

        VolatileNoAtomic[] arr = new VolatileNoAtomic[100];
        for (int i = 0; i < 10; i++) {
            arr[i] = new VolatileNoAtomic();
        }

        for (int i = 0; i < 10; i++) {
            arr[i].start();
        }
    }