## 多进程和多线程简介
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603131617288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060313181029.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
 ![ ](https://img-blog.csdnimg.cn/20210603131924145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
## 多线程实现
**多线程创建**
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106031329561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603133036677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603133226716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603133339718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603144715920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603144731804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
## 多线程信息共享
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145116827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145154464.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145346643.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145440163.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145524786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145836676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145850118.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603145907326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

**互斥，一次只有一个线程进去**![在这里插入图片描述](https://img-blog.csdnimg.cn/202106031500545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603150305593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**static共享**
```java

public class ThreadDemo0
{
	public static void main(String [] args)
	{
		new TestThread0().start();
		new TestThread0().start();
		new TestThread0().start();
		new TestThread0().start();
	}
}
class TestThread0 extends Thread  
{
	//private int tickets=100;           //每个线程卖100张，没有共享
	private static int tickets=100;  //static变量是共享的，所有的线程共享
	public void run()
	{
		while(true)
		{
			if(tickets>0)
			{
				System.out.println(Thread.currentThread().getName() +
				" is selling ticket " + tickets);
				tickets = tickets - 1;
			}
			else
			{
				break;
			}
		}
	}
}

```
**同步卖票，但无序而且容易重复，一直卖票**
```java

public class ThreadDemo1
{
	public static void main(String [] args)
	{
		TestThread1 t=new TestThread1();
		new Thread(t).start();
		new Thread(t).start();
		new Thread(t).start();
		new Thread(t).start();
	}
}
class TestThread1 implements Runnable
{
	private int tickets=100;
	public void run()
	{
		while(true)
		{
			if(tickets>0)
			{
				try {
					Thread.sleep(100);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				tickets--;
				System.out.println(Thread.currentThread().getName() +" is selling ticket " + tickets);
			}
			else
			{
				break;
			}
				
		}
	}
}

```
**volatile让大伙都乐一乐**
```java
public class ThreadDemo2
{
	public static void main(String args[]) throws Exception 
	{
		TestThread2 t = new TestThread2();
		t.start();
		Thread.sleep(2000);
		t.flag = false;
		System.out.println("main thread is exiting");
	}
}

class TestThread2 extends Thread
{
	//boolean flag = true;   //子线程不会停止
	volatile boolean flag = true;  //用volatile修饰的变量可以及时在各线程里面通知
	public void run() 
	{
		int i=0;
		while(flag)
		{
			i++;			
		}
		System.out.println("test thread3 is exiting");
	}	
} 

```
**同步且有序**
```java

public class ThreadDemo3 {
	public static void main(String[] args) {
		TestThread3 t = new TestThread3();
		new Thread(t, "Thread-0").start();
		new Thread(t, "Thread-1").start();
		new Thread(t, "Thread-2").start();
		new Thread(t, "Thread-3").start();
	}
}

class TestThread3 implements Runnable {
	private volatile int tickets = 100; // 多个 线程在共享的
	String str = new String("");

	public void run() {
		while (true) {
			sale();
			try {
				Thread.sleep(100);
			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
			if (tickets <= 0) {
				break;
			}
		}

	}

	public synchronized void sale() { // 同步函数
		if (tickets > 0) {
			System.out.println(Thread.currentThread().getName() + " is saling ticket " + tickets--);
		}
	}
}

```
## 多线程管理
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603150843191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603150930695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603151308503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**生产者和消费者案列**

```java
package product;
/**
 * 经典生产者与消费者问题
 * 生产者不断的往仓库中存放产品，消费者从仓库中消费产品。
 * 其中生产者和消费者都可以有若干个。
 * 仓库规则：容量有限，库满时不能存放，库空时不能取产品 。
 */

public class ProductTest {
	public static void main(String[] args) throws InterruptedException {
		Storage storage = new Storage();
		
		Thread consumer1 = new Thread(new Consumer(storage));
		consumer1.setName("消费者1");
		Thread consumer2 = new Thread(new Consumer(storage));
		consumer2.setName("消费者2");
		Thread producer1 = new Thread(new Producer(storage));
		producer1.setName("生产者1");
		Thread producer2 = new Thread(new Producer(storage));
		producer2.setName("生产者2");
		
		producer1.start();
		producer2.start();
		Thread.sleep(1000);		
		consumer1.start();
		consumer2.start();		
	}
}









```

```java
package product;

/**
 * 产品类
 */
class Product {
	private int id;// 产品id
	private String name;// 产品名称

	public Product(int id, String name) {
		this.id = id;
		this.name = name;
	}
	public String toString() {
		return "(产品ID：" + id + " 产品名称：" + name + ")";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}

```

```java
package product;

import java.util.Random;

/**
 * 生产者
 */
class Producer implements Runnable {
	private Storage storage;

	public Producer(Storage storage) {
		this.storage = storage;
	}

	@Override
	public void run() {
		int i = 0;
		Random r = new Random();
		while(i<10)
		{
			i++;
			Product product = new Product(i, "电话" + r.nextInt(100));
			storage.push(product);
		}		
	}
}

```

```java
package product;

/**
 * 消费者
 */
class Consumer implements Runnable {
	private Storage storage;

	public Consumer(Storage storage) {
		this.storage = storage;
	}

	public void run() {
		int i = 0;
		while(i<10)
		{
			i++;
			storage.pop();
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		
	}
}

```

```java
package product;

/**
 *仓库
 */
class Storage {
	// 仓库容量为10
	private Product[] products = new Product[10];
	private int top = 0;

	// 生产者往仓库中放入产品
	public synchronized void push(Product product) {
		while (top == products.length) {
			try {
				System.out.println("producer wait");
				wait();//仓库已满，等待
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
        //把产品放入仓库
		products[top++] = product;
		System.out.println(Thread.currentThread().getName() + " 生产了产品"
				+ product);
		System.out.println("producer notifyAll");
		notifyAll();//唤醒等待线程
		

	}

	// 消费者从仓库中取出产品
	public synchronized Product pop() {
		while (top == 0) {
			try {
				System.out.println("consumer wait");
				wait();//仓库空，等待
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}

		}

		//从仓库中取产品
		--top;
		Product p = new Product(products[top].getId(), products[top].getName());
		products[top] = null;
		System.out.println(Thread.currentThread().getName() + " 消费了产品" + p);
		System.out.println("comsumer notifyAll");
		notifyAll();//唤醒等待线程
		return p;
	}
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603152901816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**interrupt不如flag，前者会抛出错误**
```java
package interrupt;

public class InterruptTest {

	public static void main(String[] args) throws InterruptedException {
		TestThread1 t1 = new TestThread1();
		TestThread2 t2 = new TestThread2();

		t1.start();
		t2.start();

		// 让线程运行一会儿后中断
		Thread.sleep(2000);
		t1.interrupt();
		t2.flag = false;
		System.out.println("main thread is exiting");
	}

}

class TestThread1 extends Thread {
	public void run() {
		// 判断标志，当本线程被别人interrupt后，JVM会被本线程设置interrupted标记
		while (!interrupted()) {
			System.out.println("test thread1 is running");
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
				break;
			}
		}
		System.out.println("test thread1 is exiting");
	}
}

class TestThread2 extends Thread {
	public volatile boolean flag = true;
	public void run() {
		// 判断标志，当本线程被别人interrupt后，JVM会被本线程设置interrupted标记
		while (flag) {
			System.out.println("test thread2 is running");
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		System.out.println("test thread2 is exiting");
	}
}

```
![ ](https://img-blog.csdnimg.cn/20210603153439625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**守护线程，main结束线程结束 **
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060315550020.png)