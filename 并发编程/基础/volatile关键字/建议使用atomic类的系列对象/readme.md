架构师
并发编程
基础
volatile关键字
建议使用atomic类的系列对象
 
  
 
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
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicUse {

  private static AtomicInteger count = new AtomicInteger(0);

  //多个addAndGet在一个方法内是非原子性的，需要加synchronized进行修饰，保证4个addAndGet整体原子性
  /**synchronized*/
  public synchronized int multiAdd(){
      try {
        Thread.sleep(100);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      count.addAndGet(1);
      count.addAndGet(2);
      count.addAndGet(3);
      count.addAndGet(4); //+10
      return count.get();
  }


  public static void main(String[] args) {

    final AtomicUse au = new AtomicUse();

    List<Thread> ts = new ArrayList<Thread>();
    for (int i = 0; i < 100; i++) {
      ts.add(new Thread(new Runnable() {
        @Override
        public void run() {
          System.out.println(au.multiAdd());
        }
      }));
    }

    for(Thread t : ts){
      t.start();
    }   
  }
}
注意atomic类只保证本身方法原子性，并不保证多次操作的原子性