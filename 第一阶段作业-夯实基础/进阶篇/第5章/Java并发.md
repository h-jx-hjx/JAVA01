## 并发框架
### exector
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101130331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101149571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101224479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101309838.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101457172.png)
![ ](https://img-blog.csdnimg.cn/20210606101641332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101830152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606101841478.png)
**代码示例**
```java
package executor.example1;

public class Main {

	public static void main(String[] args) throws InterruptedException {
		// 创建一个执行服务器
		Server server=new Server();
		
		// 创建100个任务，并发给执行器，等待完成
		for (int i=0; i<100; i++){
			Task task=new Task("Task "+i);
			Thread.sleep(10);
			server.submitTask(task);
		}		
		server.endServer();
	}
}

```

```java
package executor.example1;

import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

/**
 * 执行服务器
 *
 */
public class Server {
	
	//线程池
	private ThreadPoolExecutor executor;
	
	public Server(){
		executor=(ThreadPoolExecutor)Executors.newCachedThreadPool();
		//executor=(ThreadPoolExecutor)Executors.newFixedThreadPool(5);
	}
	
	//向线程池提交任务
	public void submitTask(Task task){
		System.out.printf("Server: A new task has arrived\n");
		executor.execute(task); //执行  无返回值
		
		System.out.printf("Server: Pool Size: %d\n",executor.getPoolSize());
		System.out.printf("Server: Active Count: %d\n",executor.getActiveCount());
		System.out.printf("Server: Completed Tasks: %d\n",executor.getCompletedTaskCount());
	}

	public void endServer() {
		executor.shutdown();
	}
}

```

```java
package executor.example1;

import java.util.Date;
import java.util.concurrent.TimeUnit;
/**
 * Task 任务类
 * @author Tom
 *
 */
public class Task implements Runnable {

	private String name;
	
	public Task(String name){
		this.name=name;
	}
	
	public void run() {
		try {
			Long duration=(long)(Math.random()*1000);
			System.out.printf("%s: Task %s: Doing a task during %d seconds\n",Thread.currentThread().getName(),name,duration);
			Thread.sleep(duration);			
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.printf("%s: Task %s: Finished on: %s\n",Thread.currentThread().getName(),name,new Date());
	}

}

```
**代码示例2**

```java
package executor.example1;

import java.util.Date;
import java.util.concurrent.TimeUnit;
/**
 * Task 任务类
 * @author Tom
 *
 */
public class Task implements Runnable {

	private String name;
	
	public Task(String name){
		this.name=name;
	}
	
	public void run() {
		try {
			Long duration=(long)(Math.random()*1000);
			System.out.printf("%s: Task %s: Doing a task during %d seconds\n",Thread.currentThread().getName(),name,duration);
			Thread.sleep(duration);			
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.printf("%s: Task %s: Finished on: %s\n",Thread.currentThread().getName(),name,new Date());
	}

}

```

```java
package executor.example2;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.ThreadPoolExecutor;


public class SumTest {

	public static void main(String[] args) {
		
		// 执行线程池
		ThreadPoolExecutor executor=(ThreadPoolExecutor)Executors.newFixedThreadPool(4);
		
		List<Future<Integer>> resultList=new ArrayList<>();

		//统计1-1000总和，分成10个任务计算，提交任务
		for (int i=0; i<10; i++){
			SumTask calculator=new SumTask(i*100+1, (i+1)*100);
			Future<Integer> result=executor.submit(calculator);
			resultList.add(result);
		}
		
		// 每隔50毫秒，轮询等待10个任务结束
		do {
			System.out.printf("Main: 已经完成多少个任务: %d\n",executor.getCompletedTaskCount());
			for (int i=0; i<resultList.size(); i++) {
				Future<Integer> result=resultList.get(i);
				System.out.printf("Main: Task %d: %s\n",i,result.isDone());
			}
			try {
				Thread.sleep(50);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		} while (executor.getCompletedTaskCount()<resultList.size());
		
		// 所有任务都已经结束了，综合计算结果		
		int total = 0;
		for (int i=0; i<resultList.size(); i++) {
			Future<Integer> result=resultList.get(i);
			Integer sum=null;
			try {
				sum=result.get();
				total = total + sum;
			} catch (InterruptedException e) {
				e.printStackTrace();
			} catch (ExecutionException e) {
				e.printStackTrace();
			}
		}
		System.out.printf("1-1000的总和:" + total);
		
		// 关闭线程池
		executor.shutdown();
	}
}

```
### fork-join
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606104003444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606104051435.png)
**代码示例**

```java
import java.math.BigInteger;
import java.util.concurrent.RecursiveTask;

//分任务求和
public class SumTask extends RecursiveTask<Long> {
	
	private int start;
	private int end;

	public SumTask(int start, int end) {
		this.start = start;
		this.end = end;
	}

	public static final int threadhold = 5;

	@Override
	protected Long compute() {
		Long sum = 0L;
		
		// 如果任务足够小, 就直接执行
		boolean canCompute = (end - start) <= threadhold;
		if (canCompute) {
			for (int i = start; i <= end; i++) {
				sum = sum + i;				
			}
		} else {
			// 任务大于阈值, 分裂为2个任务
			int middle = (start + end) / 2;
			SumTask subTask1 = new SumTask(start, middle);
			SumTask subTask2 = new SumTask(middle + 1, end);

			invokeAll(subTask1, subTask2);

			Long sum1 = subTask1.join();
			Long sum2 = subTask2.join();

			// 结果合并
			sum = sum1 + sum2;
		}
		return sum;
	}
}

```

```java

import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;

//分任务求和
public class SumTest {
    
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //创建执行线程池
    	ForkJoinPool pool = new ForkJoinPool();
    	//ForkJoinPool pool = new ForkJoinPool(4);
    	
    	//创建任务
        SumTask task = new SumTask(1, 10000000);
        
        //提交任务
        ForkJoinTask<Long> result = pool.submit(task);
        
        //等待结果
        do {
			System.out.printf("Main: Thread Count: %d\n",pool.getActiveThreadCount());
			System.out.printf("Main: Paralelism: %d\n",pool.getParallelism());
			try {
				Thread.sleep(50);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		} while (!task.isDone());
        
        //输出结果
        System.out.println(result.get().toString());
    }
}

```
### 并发数据结构（是否安全）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606141044919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![ ](https://img-blog.csdnimg.cn/20210606141104142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![ ](https://img-blog.csdnimg.cn/2021060614331595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**代码示例**

```java
package list;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class ListTest {    
 
    public static void main(String[] args) throws InterruptedException{

        //线程不安全
        List<String> unsafeList = new ArrayList<String>();
        //线程安全
        List<String> safeList1 = Collections.synchronizedList(new ArrayList<String>());
        //线程安全
        CopyOnWriteArrayList<String> safeList2 = new CopyOnWriteArrayList<String>();

        ListThread t1 = new ListThread(unsafeList);
        ListThread t2 = new ListThread(safeList1);
        ListThread t3 = new ListThread(safeList2);

        for(int i = 0; i < 10; i++){
            Thread t = new Thread(t1, String.valueOf(i));
            t.start();
        }
        for(int i = 0; i < 10; i++) {
            Thread t = new Thread(t2, String.valueOf(i));
            t.start();
        }
        for(int i = 0; i < 10; i++) {
            Thread t = new Thread(t3, String.valueOf(i));
            t.start();
        }

        //等待子线程执行完
        Thread.sleep(2000);
 
        System.out.println("listThread1.list.size() = " + t1.list.size());
        System.out.println("listThread2.list.size() = " + t2.list.size());
        System.out.println("listThread3.list.size() = " + t3.list.size());

        //输出list中的值
        System.out.println("unsafeList：");
        for(String s : t1.list){
            if(s == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(s + "  ");
            }
        }
        System.out.println();
        System.out.println("safeList1：");
        for(String s : t2.list){
        	if(s == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(s + "  ");
            }
        }
        System.out.println();
        System.out.println("safeList2：");
        for(String s : t3.list){
        	if(s == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(s + "  ");
            }
        }
    }
}

class ListThread implements Runnable{
	public List<String> list;

    public ListThread(List<String> list){
        this.list = list;
    }

    @Override
    public void run() {
    	int i = 0;
    	while(i<10)
    	{
    		try {
                Thread.sleep(10);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
            //把当前线程名称加入list中
            list.add(Thread.currentThread().getName());
            i++;
    	}        
    }
}
```

```java
package map;

import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

public class MapTest{    
 
    public static void main(String[] args) throws InterruptedException{

        //线程不安全
        Map<Integer,String> unsafeMap = new HashMap<Integer,String>();
        //线程安全
        Map<Integer,String> safeMap1 = Collections.synchronizedMap(new HashMap<Integer,String>());
        //线程安全
        ConcurrentHashMap<Integer,String> safeMap2 = new ConcurrentHashMap<Integer,String>();

        MapThread t1 = new MapThread(unsafeMap);
        MapThread t2 = new MapThread(safeMap1);
        MapThread t3 = new MapThread(safeMap2);

        //unsafeMap的运行测试
        for(int i = 0; i < 10; i++){
        	Thread t = new Thread(t1);
        	t.start();
        }       
        for(int i = 0; i < 10; i++) {
        	Thread t = new Thread(t2);
            t.start();
        }
        for(int i = 0; i < 10; i++) {
        	Thread t = new Thread(t3);
            t.start();
        }

        //等待子线程执行完
        Thread.sleep(2000);
 
        System.out.println("mapThread1.map.size() = " + t1.map.size());
        System.out.println("mapThread2.map.size() = " + t2.map.size());
        System.out.println("mapThread3.map.size() = " + t3.map.size());

        //输出set中的值
        System.out.println("unsafeMap：");
        Iterator iter = t1.map.entrySet().iterator();
        while(iter.hasNext()) {
            Map.Entry<Integer,String> entry = (Map.Entry<Integer,String>)iter.next();
            // 获取key
            System.out.print(entry.getKey() + ":");
            // 获取value
            System.out.print(entry.getValue() + " ");
        }
        System.out.println();
        
        System.out.println("safeMap1：");
        iter = t2.map.entrySet().iterator();
        while(iter.hasNext()) {
            Map.Entry<Integer,String> entry = (Map.Entry<Integer,String>)iter.next();
            // 获取key
            System.out.print(entry.getKey() + ":");
            // 获取value
            System.out.print(entry.getValue() + " ");
        }

        System.out.println();
        System.out.println("safeMap2：");
        iter = t3.map.entrySet().iterator();
        while(iter.hasNext()) {
            Map.Entry<Integer,String> entry = (Map.Entry<Integer,String>)iter.next();
            // 获取key
            System.out.print(entry.getKey() + ":");
            // 获取value
            System.out.print(entry.getValue() + " ");
        }
        System.out.println();
        System.out.println("mapThread1.map.size() = " + t1.map.size());
        System.out.println("mapThread2.map.size() = " + t2.map.size());
        System.out.println("mapThread3.map.size() = " + t3.map.size());
    }
}

class MapThread implements Runnable
{
	public Map<Integer,String> map;

    public MapThread(Map<Integer,String> map){
        this.map = map;
    }

    @Override
    public void run() {
        int i=0;
        
        while(i<100)
        {
        	//把当前线程名称加入map中
            map.put(i++,Thread.currentThread().getName());
            try {
                Thread.sleep(10);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }        
    }
}
```

```java
package queue;

import java.util.*;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ConcurrentLinkedDeque;

public class QueueTest {

    public static void main(String[] args) throws InterruptedException{

        //线程不安全
        Deque<String> unsafeQueue = new ArrayDeque<String>();
        //线程安全
        ConcurrentLinkedDeque<String> safeQueue1 = new ConcurrentLinkedDeque<String>();

        ArrayBlockingQueue<String> safeQueue2 = new ArrayBlockingQueue<String>(100);

        QueueThread t1 = new QueueThread(unsafeQueue);
        QueueThread t2 = new QueueThread(safeQueue1);
        QueueThread t3 = new QueueThread(safeQueue2);

        for(int i = 0; i < 10; i++){
            Thread thread1 = new Thread(t1, String.valueOf(i));
            thread1.start();
        }
        for(int i = 0; i < 10; i++) {
            Thread thread2 = new Thread(t2, String.valueOf(i));
            thread2.start();
        }
        for(int i = 0; i < 10; i++) {
            Thread thread3 = new Thread(t3, String.valueOf(i));
            thread3.start();
        }

        //等待子线程执行完
        Thread.sleep(2000);
 
        System.out.println("queueThread1.queue.size() = " + t1.queue.size());
        System.out.println("queueThread2.queue.size() = " + t2.queue.size());
        System.out.println("queueThread3.queue.size() = " + t3.queue.size());

        //输出queue中的值
        System.out.println("unsafeQueue：");
        for(String s:t1.queue)
        {
        	System.out.print(s + " ");
        }
        System.out.println();
        System.out.println("safeQueue1：");
        for(String s:t2.queue)
        {
        	System.out.print(s + " ");
        }
        System.out.println();
        System.out.println("safeQueue2：");
        for(String s:t3.queue)
        {
        	System.out.print(s + " ");
        }
    }
}

class QueueThread implements Runnable{
	public Queue<String> queue;

    public QueueThread(Queue<String> queue){
        this.queue = queue;
    }

    @Override
    public void run() {
    	int i = 0;
    	while(i<10)
    	{
    		i++;
    		try {
                Thread.sleep(10);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
            //把当前线程名称加入list中
            queue.add(Thread.currentThread().getName());
    	}        
    }
}
```

```java
package set;

import java.util.*;
import java.util.concurrent.CopyOnWriteArraySet;

public class SetTest{  
 
    public static void main(String[] args) throws InterruptedException{

        //线程不安全
        Set<String> unsafeSet = new HashSet<String>();
        //线程安全
        Set<String> safeSet1 = Collections.synchronizedSet(new HashSet<String>());
        //线程安全
        CopyOnWriteArraySet<String> safeSet2 = new CopyOnWriteArraySet<String>();

        SetThread t1 = new SetThread(unsafeSet);
        SetThread t2 = new SetThread(safeSet1);
        SetThread t3 = new SetThread(safeSet2);

        //unsafeSet的运行测试
        for(int i = 0; i < 10; i++){
        	Thread t = new Thread(t1, String.valueOf(i));
        	t.start();
        }
        for(int i = 0; i < 10; i++) {
        	Thread t = new Thread(t2, String.valueOf(i));
            t.start();
        }
        for(int i = 0; i < 10; i++) {
        	Thread t = new Thread(t3, String.valueOf(i));
            t.start();
        }

        //等待子线程执行完
        Thread.sleep(2000);
 
        System.out.println("setThread1.set.size() = " + t1.set.size());
        System.out.println("setThread2.set.size() = " + t2.set.size());
        System.out.println("setThread3.set.size() = " + t3.set.size());

        //输出set中的值
        System.out.println("unsafeSet：");
        for(String element:t1.set){
            if(element == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(element + "  ");
            }
        }
        System.out.println();
        System.out.println("safeSet1：");
        for(String element:t2.set){
        	if(element == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(element + "  ");
            }
        }
        System.out.println();
        System.out.println("safeSet2：");
        for(String element:t3.set){
        	if(element == null){
            	System.out.print("null  ");
            }
            else
            {
            	System.out.print(element + "  ");
            }
        }
    }
}

class SetThread implements Runnable{
	public Set<String> set;

    public SetThread(Set<String> set){
        this.set = set;
    }

    @Override
    public void run() {
    	int i = 0;
    	while(i<10)
    	{
    		i++;
    		try {
                Thread.sleep(10);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
            //把当前线程名称加入list中
            set.add(Thread.currentThread().getName() + i);
    	}        
    }
}
```
### java并发协作控制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606144734989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060614481621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060614521146.png)
**代码示例**

 买奶茶
**Lock** 

```java


import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class LockExample {

	private static final ReentrantLock queueLock = new ReentrantLock(); //可重入锁
	private static final ReentrantReadWriteLock orderLock = new ReentrantReadWriteLock(); //可重入读写锁
	
	/**
	 * 有家奶茶店，点单有时需要排队 
	 * 假设想买奶茶的人如果看到需要排队，就决定不买
	 * 又假设奶茶店有老板和多名员工，记单方式比较原始，只有一个订单本
	 * 老板负责写新订单，员工不断地查看订单本得到信息来制作奶茶，在老板写新订单时员工不能看订单本
	 * 多个员工可同时看订单本，在员工看时老板不能写新订单
	 * @param args
	 * @throws InterruptedException 
	 */
	public static void main(String[] args) throws InterruptedException {
		//buyMilkTea();
		handleOrder(); //需手动关闭
	}
	
	public void tryToBuyMilkTea() throws InterruptedException {
		boolean flag = true;
		while(flag)
		{
			if (queueLock.tryLock()) {
				//queueLock.lock();
				long thinkingTime = (long) (Math.random() * 500);
				Thread.sleep(thinkingTime);
				System.out.println(Thread.currentThread().getName() + "： 来一杯珍珠奶茶，不要珍珠");
				flag = false;
				queueLock.unlock();
			} else {
				//System.out.println(Thread.currentThread().getName() + "：" + queueLock.getQueueLength() + "人在排队");
				System.out.println(Thread.currentThread().getName() + "： 再等等");
			}
			if(flag)
			{
				Thread.sleep(1000);
			}
		}
		
	}
	
	public void addOrder() throws InterruptedException {
		orderLock.writeLock().lock();
		long writingTime = (long) (Math.random() * 1000);
		Thread.sleep(writingTime);
		System.out.println("老板新加一笔订单");
		orderLock.writeLock().unlock();
	}
	
	public void viewOrder() throws InterruptedException {
		orderLock.readLock().lock();
			
		long readingTime = (long) (Math.random() * 500);
		Thread.sleep(readingTime);
		System.out.println(Thread.currentThread().getName() + ": 查看订单本");
		orderLock.readLock().unlock();			

	}
	
	public static void buyMilkTea() throws InterruptedException {
		LockExample lockExample = new LockExample();
		int STUDENTS_CNT = 10;
		
		Thread[] students = new Thread[STUDENTS_CNT];
		for (int i = 0; i < STUDENTS_CNT; i++) {
			students[i] = new Thread(new Runnable() {

				@Override
				public void run() {
					try {
						long walkingTime = (long) (Math.random() * 1000);
						Thread.sleep(walkingTime);
						lockExample.tryToBuyMilkTea();
					} catch(InterruptedException e) {
						System.out.println(e.getMessage());
					}
				}
				
			}
			);
			
			students[i].start();
		}
		
		for (int i = 0; i < STUDENTS_CNT; i++)
			students[i].join();

	}
	
	
	public static void handleOrder() throws InterruptedException {
		LockExample lockExample = new LockExample();
		
		
		Thread boss = new Thread(new Runnable() {

			@Override
			public void run() {
				while (true) {
					try {
						lockExample.addOrder();
						long waitingTime = (long) (Math.random() * 1000);
						Thread.sleep(waitingTime);
					} catch (InterruptedException e) {
						System.out.println(e.getMessage());
					}
				}
			}
		});
		boss.start();

		int workerCnt = 3;
		Thread[] workers = new Thread[workerCnt];
		for (int i = 0; i < workerCnt; i++)
		{
			workers[i] = new Thread(new Runnable() {

				@Override
				public void run() {
					while (true) {
						try {
								lockExample.viewOrder();
								long workingTime = (long) (Math.random() * 5000);
								Thread.sleep(workingTime);
							} catch (InterruptedException e) {
								System.out.println(e.getMessage());
							}
						}
				}
				
			});
			
			workers[i].start();
		}
		
	}
}

```
**Semaphore**
![ ](https://img-blog.csdnimg.cn/20210606145730337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**代码示例**
```java

import java.util.concurrent.Semaphore;

public class SemaphoreExample {

	private final Semaphore placeSemaphore = new Semaphore(5);
	
	public boolean parking() throws InterruptedException {
		if (placeSemaphore.tryAcquire()) {
			System.out.println(Thread.currentThread().getName() + ": 停车成功");
			return true;
		} else {
			System.out.println(Thread.currentThread().getName() + ": 没有空位");
			return false;
		}

	}
	
	public void leaving() throws InterruptedException {
		placeSemaphore.release();
		System.out.println(Thread.currentThread().getName() + ": 开走");
	}
	
	/**
	 * 现有一地下车库，共有车位5个，由10辆车需要停放，每次停放时，去申请信号量
	 * @param args
	 * @throws InterruptedException 
	 */
	public static void main(String[] args) throws InterruptedException {
		int tryToParkCnt = 10;
		
		SemaphoreExample semaphoreExample = new SemaphoreExample();
		
		Thread[] parkers = new Thread[tryToParkCnt];
		
		for (int i = 0; i < tryToParkCnt; i++) {
			parkers[i] = new Thread(new Runnable() {

				@Override
				public void run() {
					try {
						long randomTime = (long) (Math.random() * 1000);
						Thread.sleep(randomTime);
						if (semaphoreExample.parking()) {
							long parkingTime = (long) (Math.random() * 1200);
							Thread.sleep(parkingTime);
							semaphoreExample.leaving();
						}
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			});
			
			parkers[i].start();
		}

		for (int i = 0; i < tryToParkCnt; i++) {
			parkers[i].join();
		}	
	}
}

```
**Latch**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060615030566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606150441760.png)

```java

import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample {

	/**
	 * 设想百米赛跑比赛 发令枪发出信号后选手开始跑，全部选手跑到终点后比赛结束
	 * 
	 * @param args
	 * @throws InterruptedException
	 */
	public static void main(String[] args) throws InterruptedException {
		int runnerCnt = 10;
		CountDownLatch startSignal = new CountDownLatch(1);
		CountDownLatch doneSignal = new CountDownLatch(runnerCnt);

		for (int i = 0; i < runnerCnt; ++i) // create and start threads
			new Thread(new Worker(startSignal, doneSignal)).start();

		System.out.println("准备工作...");
		System.out.println("准备工作就绪");
		startSignal.countDown(); // let all threads proceed
		System.out.println("比赛开始");
		doneSignal.await(); // wait for all to finish
		System.out.println("比赛结束");
	}

	static class Worker implements Runnable {
		private final CountDownLatch startSignal;
		private final CountDownLatch doneSignal;

		Worker(CountDownLatch startSignal, CountDownLatch doneSignal) {
			this.startSignal = startSignal;
			this.doneSignal = doneSignal;
		}

		public void run() {
			try {
				startSignal.await();
				doWork();
				doneSignal.countDown();
			} catch (InterruptedException ex) {
			} // return;
		}

		void doWork() {
			System.out.println(Thread.currentThread().getName() + ": 跑完全程");
		}
	}
}

```
**Barrier**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606150613220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060615074552.png)

```java


import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample {
	
	/**
	 * 假定有三行数，用三个线程分别计算每一行的和，最终计算总和
	 * @param args
	 */
	public static void main(String[] args) {
		final int[][] numbers = new int[3][5];
		final int[] results = new int[3];
		int[] row1 = new int[]{1, 2, 3, 4, 5};
		int[] row2 = new int[]{6, 7, 8, 9, 10};
		int[] row3 = new int[]{11, 12, 13, 14, 15};
		numbers[0] = row1;
		numbers[1] = row2;
		numbers[2] = row3;
		
		CalculateFinalResult finalResultCalculator = new CalculateFinalResult(results);
		CyclicBarrier barrier = new CyclicBarrier(3, finalResultCalculator);
		//当有3个线程在barrier上await，就执行finalResultCalculator
		
		for(int i = 0; i < 3; i++) {
			CalculateEachRow rowCalculator = new CalculateEachRow(barrier, numbers, i, results);
			new Thread(rowCalculator).start();
		}		
	}
}

class CalculateEachRow implements Runnable {

	final int[][] numbers;
	final int rowNumber;
	final int[] res;
	final CyclicBarrier barrier;
	
	CalculateEachRow(CyclicBarrier barrier, int[][] numbers, int rowNumber, int[] res) {
		this.barrier = barrier;
		this.numbers = numbers;
		this.rowNumber = rowNumber;
		this.res = res;
	}
	
	@Override
	public void run() {
		int[] row = numbers[rowNumber];
		int sum = 0;
		for (int data : row) {
			sum += data;
			res[rowNumber] = sum;
		}
		try {
			System.out.println(Thread.currentThread().getName() + ": 计算第" + (rowNumber + 1) + "行结束，结果为: " + sum);
			barrier.await(); //等待！只要超过3个(Barrier的构造参数)，就放行。
		} catch (InterruptedException | BrokenBarrierException e) {
			e.printStackTrace();
		}
	}
	
}


class CalculateFinalResult implements Runnable {
	final int[] eachRowRes;
	int finalRes;
	public int getFinalResult() {
		return finalRes;
	}
	
	CalculateFinalResult(int[] eachRowRes) {
		this.eachRowRes = eachRowRes;
	}
	
	@Override
	public void run() {
		int sum = 0;
		for(int data : eachRowRes) {
			sum += data;
		}
		finalRes = sum;
		System.out.println("最终结果为: " + finalRes);
	}
	
}

```
**Phaser**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606150852606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java

import java.util.concurrent.Phaser;

public class PhaserExample {

	/**
	 * 假设举行考试，总共三道大题，每次下发一道题目，等所有学生完成后再进行下一道
	 * 
	 * @param args
	 */
	public static void main(String[] args) {

		int studentsCnt = 5;
		Phaser phaser = new Phaser(studentsCnt);

		for (int i = 0; i < studentsCnt; i++) {
			new Thread(new Student(phaser)).start();
		}
	}
}

class Student implements Runnable {

	private final Phaser phaser;

	public Student(Phaser phaser) {
		this.phaser = phaser;
	}

	@Override
	public void run() {
		try {
			doTesting(1);
			phaser.arriveAndAwaitAdvance(); //等到5个线程都到了，才放行
			doTesting(2);
			phaser.arriveAndAwaitAdvance();
			doTesting(3);
			phaser.arriveAndAwaitAdvance();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

	private void doTesting(int i) throws InterruptedException {
		String name = Thread.currentThread().getName();
		System.out.println(name + "开始答第" + i + "题");
		long thinkingTime = (long) (Math.random() * 1000);
		Thread.sleep(thinkingTime);
		System.out.println(name + "第" + i + "道题答题结束");
	}
}
```
**Exchanger**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606151014939.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java


import java.util.Scanner;
import java.util.concurrent.Exchanger;

public class ExchangerExample {
	
	/**
	 * 本例通过Exchanger实现学生成绩查询，简单线程间数据的交换
	 * @param args
	 * @throws InterruptedException
	 */
	public static void main(String[] args) throws InterruptedException {
		Exchanger<String> exchanger = new Exchanger<String>();
		BackgroundWorker worker = new BackgroundWorker(exchanger);
		new Thread(worker).start();
		
		Scanner scanner = new Scanner(System.in);
		while(true) {
			System.out.println("输入要查询的属性学生姓名：");
			String input = scanner.nextLine().trim();
			exchanger.exchange(input); //把用户输入传递给线程
			String value = exchanger.exchange(null); //拿到线程反馈结果
			if ("exit".equals(value)) {
				break;
			}
			System.out.println("查询结果：" + value);
		}
		scanner.close();
	} 
}

class BackgroundWorker implements Runnable {

	final Exchanger<String> exchanger;
	BackgroundWorker(Exchanger<String> exchanger) {
		this.exchanger = exchanger;
	}
	@Override
	public void run() {
		while (true) {
			try {
				String item = exchanger.exchange(null);
				switch (item) {
				case "zhangsan": 
					exchanger.exchange("90");
					break;
				case "lisi":
					exchanger.exchange("80");
					break;
				case "wangwu":
					exchanger.exchange("70");
					break;
				case "exit":
					exchanger.exchange("exit");
					return;
				default:
					exchanger.exchange("查无此人");
				}					
			} catch (InterruptedException e) {
				e.printStackTrace();
			}				
		}
	}		
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606151216966.png)
### 定时任务执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060615334169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606154130654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
package quartz;

import org.quartz.Job;
import org.quartz.JobDetail;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

import java.util.Date;

public class HelloJob implements Job {
    public void execute(JobExecutionContext context) throws JobExecutionException {
        JobDetail detail = context.getJobDetail();
        String name = detail.getJobDataMap().getString("name");
        System.out.println("hello from " + name + " at " + new Date());
    }
}
```

```java
package quartz;

import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.Trigger;
import org.quartz.impl.StdSchedulerFactory;

import static org.quartz.JobBuilder.newJob;
import static org.quartz.SimpleScheduleBuilder.simpleSchedule;
import static org.quartz.TriggerBuilder.newTrigger;

public class QuartzTest {

    public static void main(String[] args) {
        try {
            //创建scheduler
            Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

            //定义一个Trigger
            Trigger trigger = newTrigger().withIdentity("trigger1", "group1") //定义name/group
                    .startNow()//一旦加入scheduler，立即生效
                    .withSchedule(simpleSchedule() //使用SimpleTrigger
                            .withIntervalInSeconds(2) //每隔2秒执行一次
                            .repeatForever()) //一直执行
                    .build();

            //定义一个JobDetail
            JobDetail job = newJob(HelloJob.class) //定义Job类为HelloQuartz类
                    .withIdentity("job1", "group1") //定义name/group
                    .usingJobData("name", "quartz") //定义属性
                    .build();

            //加入这个调度
            scheduler.scheduleJob(job, trigger);

            //启动
            scheduler.start();

            //运行一段时间后关闭
            Thread.sleep(10000);
            scheduler.shutdown(true);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

```java
package schedule;

import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class ScheduledExecutorTest {
    
	public static void main(String[] a) throws Exception
    {
    	//executeAtFixTime();
    	//executeFixedRate();  //3s
    	executeFixedDelay();  //4s
    }
	
	public static void executeAtFixTime() throws Exception {
        ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
        executor.schedule(
                new MyTask(),
                1,                
                TimeUnit.SECONDS);
        
        Thread.sleep(20000);
        executor.shutdown();
    }
	
	/**
     * 周期任务 固定速率 是以上一个任务开始的时间计时，period时间过去后，检测上一个任务是否执行完毕，
     * 如果上一个任务执行完毕，则当前任务立即执行，如果上一个任务没有执行完毕，则需要等上一个任务执行完毕后立即执行。
	 * @throws Exception 
     */
    public static void executeFixedRate() throws Exception {
        ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
        executor.scheduleAtFixedRate(
                new MyTask(),
                1,
                3000,
                TimeUnit.MILLISECONDS);
        
        Thread.sleep(20000);
        executor.shutdown();
    }

    /**
     * 周期任务 固定延时 是以上一个任务结束时开始计时，period时间过去后，立即执行。
     * @throws Exception 
     */
    public static void executeFixedDelay() throws Exception {
        ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
        executor.scheduleWithFixedDelay(
                new MyTask(),
                1,
                3000,
                TimeUnit.MILLISECONDS);
        
        Thread.sleep(20000);
        executor.shutdown();
    }    
}

class MyTask implements Runnable {
	public void run() {
		System.out.println("时间为：" + new Date());
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		//System.out.println("时间为：" + new Date());
	}
}

```

```java
package timer;

import java.util.Calendar;
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

public class TimerTest {
	public static void main(String[] args) throws InterruptedException {
			MyTask task = new MyTask();
			Timer timer = new Timer();
			
			System.out.println("当前时间："+new Date().toLocaleString());
			//当前时间1秒后，每2秒执行一次
			timer.schedule(task, 1000, 2000);
			
			Thread.sleep(10000);
			task.cancel();  //取消当前的任务
			
			System.out.println("================================");
			
			Calendar now = Calendar.getInstance();
			now.set(Calendar.SECOND,now.get(Calendar.SECOND)+3);
	        Date runDate = now.getTime();
	        MyTask2 task2 = new MyTask2();
	        timer.scheduleAtFixedRate(task2,runDate,3000); //固定速率
	        
			
	        Thread.sleep(20000);
			timer.cancel();  //取消定时器
	}
}

class MyTask extends TimerTask {
	public void run() {
		System.out.println("运行了！时间为：" + new Date());
	}
}

class MyTask2 extends TimerTask {
	public void run() {
		System.out.println("运行了！时间为：" + new Date());
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```
### 作业
给定一个int数组，假设有10000个长度，里面放满1-100的随机整数。需要用串行循环计算、Executors框架和Fork-Join框架三种方法，实现查找并输出该数组中50的出现个数。



提交源程序和和执行结果截图。

预期执行结果如下(具体数量根据每个程序随机赋值决定)

串行搜索得到50的个数是5个。

Executors搜索得到50的个数是5个。

Fork-Join搜索得到50的个数是5个。

```java
package org.example;

import java.util.concurrent.*;

public class FindNumberCount {
    public static void main(String[] args) throws InterruptedException, ExecutionException {
        int[] numbers = new int[10000];
        int targetNumber = 50;
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = (int) (Math.random() * 100);
        }
        System.out.println("findBySerial = " + findBySerial(numbers, targetNumber));
        System.out.println("findByExecutor = " + findByExecutor(numbers, targetNumber));
        System.out.println("findByForkJoin = " + findByForkJoin(numbers, targetNumber));
    }

    public static int findBySerial(int[] numbers, int targetNumber) {
        int cnt = 0;
        for (int i = 0; i < numbers.length; i++) {
            if (numbers[i] == targetNumber)
                cnt++;

        }
        return cnt;
    }

    public static int findByExecutor(int[] numbers, int targetNumber) throws InterruptedException {
        ExecutorService executor = Executors.newCachedThreadPool();

        int increment = 1000; //每1000长度为一组
        int numOfThread = numbers.length / increment;
        CountDownLatch latch = new CountDownLatch(numOfThread);

        int startIndex = 0, endIndex = increment - 1;
        for (int i = 0; i < numOfThread; i++) {
            if (i == numOfThread - 1)
                endIndex = numbers.length - 1;
            executor.execute(new ExecutorTask(numbers, startIndex, endIndex, targetNumber, latch));
//            new Thread(new ExecutorTask(numbers, startIndex, endIndex, targetNumber, latch)).start();
            startIndex = endIndex + 1;
            endIndex = endIndex + increment;
        }

        latch.await();
        executor.shutdown();
        return ExecutorTask.getSumCnt();
    }

    public static int findByForkJoin(int[] numbers, int targetNumber) throws InterruptedException, ExecutionException {
        ForkJoinPool pool = new ForkJoinPool();
        ForkJoinTask<Integer> task = new MyForkJoinTask(numbers, 0, numbers.length - 1, targetNumber);
        task = pool.submit(task);
        Integer cnt = task.get();
        pool.shutdown();
        return cnt;
    }

}

class ExecutorTask implements Runnable {
    private static int sumCnt = 0;
    private int[] numbers;
    private int startIndex;
    private int endIndex;
    private int targetNumber;
    private CountDownLatch latch;

    public ExecutorTask(int[] numbers, int startIndex, int endIndex, int targetNumber, CountDownLatch latch) {
        this.numbers = numbers;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
        this.targetNumber = targetNumber;
        this.latch = latch;
    }

    @Override
    public void run() {
        int cnt = 0;
        for (int i = startIndex; i <= endIndex; i++) {
            if (numbers[i] == targetNumber)
                cnt++;
        }

        synchronized (ExecutorTask.class) {
            sumCnt += cnt;
        }
        latch.countDown();
    }

    public static int getSumCnt() {
        return sumCnt;
    }
}

class MyForkJoinTask extends RecursiveTask<Integer> {
    int[] numbers;
    int startIndex;
    int endIndex;
    int targetNumber;

    public MyForkJoinTask(int[] numbers, int startIndex, int endIndex, int targetNumber) {
        this.numbers = numbers;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
        this.targetNumber = targetNumber;
    }

    @Override
    protected Integer compute() {
        int cnt = 0;
        if (endIndex - startIndex < 1000) {
            for (int i = startIndex; i <= endIndex; i++) {
                if (numbers[i] == targetNumber)
                    cnt++;
            }
        } else {
            int midIndex = (startIndex + endIndex) / 2;
            MyForkJoinTask leftTask = new MyForkJoinTask(numbers, startIndex, midIndex, targetNumber);
            MyForkJoinTask rightTask = new MyForkJoinTask(numbers, midIndex + 1, endIndex, targetNumber);

//            leftTask.fork();
//            cnt = rightTask.compute() + leftTask.join();

            invokeAll(leftTask, rightTask);
            cnt = rightTask.join() + leftTask.join();
        }

        return cnt;

    }

}




```