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
/**
 * 业务整体需要使用完整的synchronized，保持业务的原子性。
 */
public class DirtyRead {

  private String username = "bjsxt";
  private String password = "123";

  public synchronized void setValue(String username, String password){
    this.username = username;

    try {
      Thread.sleep(2000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }

    this.password = password;

    System.out.println("setValue最终结果：username = " + username + " , password = " + password);
  }

  public void getValue(){
    System.out.println("getValue方法得到：username = " + this.username + " , password = " + this.password);
  }


  public static void main(String[] args) throws Exception{

    final DirtyRead dr = new DirtyRead();
    Thread t1 = new Thread(new Runnable() {
      @Override
      public void run() {
        dr.setValue("z3", "456");   
      }
    });
    t1.start();
    Thread.sleep(1000);

    dr.getValue();
  } 
}