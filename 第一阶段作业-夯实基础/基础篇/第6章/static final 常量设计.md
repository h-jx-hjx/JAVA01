static
static java中的关键字，可用在变量，方法，类，匿名方法块
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021051916595890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519170020355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519170357811.png)
#单例模式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519170500881.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519170518246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
#final
父类用final方法，子类不能改写
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519170849488.png)

# 常量设计

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519161658882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519161854981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
常量池
![ ](https://img-blog.csdnimg.cn/20210519162156860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
优化常量
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519162541303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519162643496.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
栈内存读取速度快但是容量小，堆内存读取速度慢但是容量大
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519162930852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519162958664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**很神奇，自动拆箱**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519163047747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519163316751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**在内存是分开的**
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519163410377.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519163559119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
可变对象和不可变对象，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519163954689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519164035120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519164113929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021051916420638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**字符串用equals，指针用==** 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519165544278.png)

**作业**

1单选(3分)
下面关于变量及其范围的陈述哪些是错误的

得分/总分

A.
实例变量是类的成员变量


B.
实例变量用关键字static声明


C.
在方法中定义的局部变量在该方法被执行时创建


D.
局部变量在使用前必须被初始化

正确答案：B你没选择任何选项
解析：  D、静态成员变量通常也叫做类变量，因为它不从属于任何对象。非静态的成员变量，也叫做实例变量，是从属于某一个具体的对象。所以A是正确的(更严格的表述：实例变量是类的成员变量的一种)。实例变量不能用static，类变量采用static。方法中的临时变量是没有初始化的，在使用之前必须初始化。而类成员变量默认是有初始值的。普通基本变量可以参看第三章第二节《基本类型》视频。对象成员变量默认值是null.

下列说法错误的是

得分/总分

A.
声明为static的方法可以被重写

0.00/3.00

B.
声明为static的方法不可以调用非static变量


C.
声明为final的方法可以被重写


D.
声明为final的类不可以被继承

正确答案：C你错选为A
解析：  D、声明为final的方法是不能被重写的，只能在参数列表上做修改，形成重载，而非重写。

下列代码执行结果是

class NumTest{

  static int id = 1;

  int id2 = 1;



  NumTest(int id,int id2){

    this.id = id;
    
    this.id2 = id2;

  }



  void printId(){

    System.out.print(id+id2+" ");

  }



  public static void main(String[] args) {

    NumTest a = new NumTest(1,2);
    
    NumTest b = new NumTest(2,1);
    
    NumTest c = new NumTest(0,0);
    
    a.printId();
    
    b.printId();
    
    c.printId();

  }

}

得分/总分

A.
3 3 0

0.00/3.00

B.
1 2 0


C.
2 1 0


D.
编译报错

正确答案：C你错选为A
解析：  D、NumTest.id是一个静态变量，所有的NumTest对象都共享同一个。所以在c对象创建完成后，NumTest.id为0.而每个对象的id2是对立的。所以在每个对象的printId方法上，将0和id2相加再输出。
**static用完之后，变为0**
class NumTest{

  final static int num1 = 1;

  static int num2 = 1;



  void printNum1(){

    System.out.print(num1+" ");

  }



  void printNum2(){

    System.out.print(num2+" ");

  }



  public static void main(String[] args) {

    NumTest a = new NumTest();
    
    a.num2 ++;
    
    a.printNum1();
    
    NumTest b = new NumTest();
    
    b.printNum2();

 }

}

得分/总分

A.
1 1


B.
1 2


C.
2 2


D.
编译报错

0.00/3.00
正确答案：B你错选为D
解析：  D、num1和num2都是static，num1而且是final，无法修改值，所以num1一直保持是1.  a.num2和b.num2是同一个，a.num2++，导致num2变成2，因此b.num2也是2了。