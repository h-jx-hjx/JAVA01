![ ](https://img-blog.csdnimg.cn/20210521192558557.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521192755370.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021052119281942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**异常处理**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521193652981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521194000462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521194424469.png)
**自定义异常**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521194714625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521194729922.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521194855924.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210521195056897.png)
**作业**
class MultiCatch

{

     public static void main(String args[])
    
    {
    
        try
    
        {
    
              int a=args.length;
    
              int b=42/a;
    
              int c[]={1};
    
              c[42]=99;  //10行
    
              System.out.println(“b=”+b);
    
            }
    
        catch(ArithmeticException e)
    
        {
    
                System.out.println(“除0异常：”+e);  //15行
    
        }
    
        catch(ArrayIndexOutOfBoundsException e)
    
        {
    
                System.out.println(“数组超越边界异常：”+e); //19行
    
            }
    
         }    
    
      }
     **第十五行没问题**
     请问所有的异常(Exception)和错误(Error)类皆继承哪一个类？（    ）

得分/总分

A.
java.lang.Throwable


B.
java.lang.Exception

0.00/3.00

C.
java.lang.Error


D.
java.io.Exception

正确答案：A你错选为B
不管Error还是Exception都是Throwable的子类。

import java.io.*;

class Master {

String doFileStuff() throws FileNotFoundException { 

        return "a";

  }

}

class Slave extends Master {

   public static void main(String[] args){

    String s = null;
    
    try { 
    
        s = new Slave().doFileStuff();
    
    }catch ( Exception x){
    
    s = "b"; 
    
    }
    
    System.out.println(s);
    
    }
    
    // insert code here

   }

Which, inserted independently at // insert code here, will compile, and produce the output b? (Choose all that apply.)



得分/总分

A.
String doFileStuff() { return "b"; }


B.
String doFileStuff() throws IOException { return "b"; }//新


C.
String doFileStuff(int x) throws IOException { return "b"; }//新，且不是重写


D.
String doFileStuff() throws Exception { return "b"; }//大

选	A

```java
class Plane {

    static String s = "-";

    public static void main(String[] args){

        new Plane().s1();

        System.out.println(s);

    }

    void s1() {

        try {s2();}catch (Exception e){

         s += "c"; 

        }

    }

    void s2() throws Exception {

        s3(); //直接抛出异常

        s += "2";

        s3();

        s += "2b";

    }

    void s3() throws Exception{

          throw new Exception();

    }

}
```
**结果为-c**
 //会直接抛出异常，下面的代码不会执行