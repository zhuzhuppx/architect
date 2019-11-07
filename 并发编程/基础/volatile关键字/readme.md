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
使变量在多个线程间可见

public class RunThread extends Thread{

  private volatile boolean isRunning = true;
  private void setRunning(boolean isRunning){
    this.isRunning = isRunning;
  }

  public void run(){
    System.out.println("进入run方法..");
    int i = 0;
    while(isRunning == true){
      //..
    }
    System.out.println("线程停止");
  }

  public static void main(String[] args) throws InterruptedException {
    RunThread rt = new RunThread();
    rt.start();
    Thread.sleep(1000);
    rt.setRunning(false);
    System.out.println("isRunning的值已经被设置了false");
  } 
}
在Java中，每一个线程都会有一块工作内存区，其中存放着所有线程共享的主内存中的变量值的拷贝。当线程执行时，他在自已工作内存区中操作这些变量。为了存取一个共享的变量，一个线程通常先获取锁定并去清除它的内存工作区，把这些共享变量从所有线程的共享内存区中正确的装入到他自已所在的工作内存区中，当线程解锁时保证该工作内存区中变量的值写回到共享内存中。
一个线程可以执行的操作有使用(use)、赋值(assign)、装载(load)、存储(store)、锁定(lock)、解锁(unlock).
而主内存可以执行的操作有读(read)、写(write)、锁定(lock)、解锁(unlock),每个操作都是原子的。

volatile关键字的作用就是强制线程到主内存(共享内存)里去读取变量，而不去线程工作内存区里去读取，从而实现了多个线程间的变量可见。也就是满足线程安全的可见性。