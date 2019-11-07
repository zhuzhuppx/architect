
当使用一个对象进行加锁的时候，要注意对象本身发生改变的时候，那么持有的锁就不同。如果对象本身不发生改变，那么依然是同步的，即使是对象的属性发生了改变。

/**
 * 锁对象的改变问题
 */
public class ChangeLock {

  private String lock = "lock";

  private void method(){
    synchronized (lock) {
      try {
        System.out.println("当前线程 : "  + Thread.currentThread().getName() + "开始");
        lock = "change lock";
        Thread.sleep(2000);
        System.out.println("当前线程 : "  + Thread.currentThread().getName() + "结束");
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }

  public static void main(String[] args) {

    final ChangeLock changeLock = new ChangeLock();
    Thread t1 = new Thread(new Runnable() {
      @Override
      public void run() {
        changeLock.method();
      }
    },"t1");
    Thread t2 = new Thread(new Runnable() {
      @Override
      public void run() {
        changeLock.method();
      }
    },"t2");
    t1.start();
    try {
      Thread.sleep(100);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    t2.start();
  }

}
/**
 * 同一对象属性的修改不会影响锁的情况
 */
public class ModifyLock {

  private String name ;
  private int age ;

  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  }
  public int getAge() {
    return age;
  }
  public void setAge(int age) {
    this.age = age;
  }

  public synchronized void changeAttributte(String name, int age) {
    try {
      System.out.println("当前线程 : "  + Thread.currentThread().getName() + " 开始");
      this.setName(name);
      this.setAge(age);

      System.out.println("当前线程 : "  + Thread.currentThread().getName() + " 修改对象内容为： " 
          + this.getName() + ", " + this.getAge());

      Thread.sleep(2000);
      System.out.println("当前线程 : "  + Thread.currentThread().getName() + " 结束");
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  public static void main(String[] args) {
    final ModifyLock modifyLock = new ModifyLock();
    Thread t1 = new Thread(new Runnable() {
      @Override
      public void run() {
        modifyLock.changeAttributte("张三", 20);
      }
    },"t1");
    Thread t2 = new Thread(new Runnable() {
      @Override
      public void run() {
        modifyLock.changeAttributte("李四", 21);
      }
    },"t2");

    t1.start();
    try {
      Thread.sleep(100);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    t2.start();
  } 
}