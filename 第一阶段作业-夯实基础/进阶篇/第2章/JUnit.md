 **JUnit**

是一个可以进行单元测试的方式，在每一个单元前面加一个@test，一个单元出了错误并不影响其他单元工作 

1）单元测试（测试方法）：用的是junit， junit是一个专门测试的框架（工具）。
    junit测试的内容： 测试的是类中的方法， 每一个方法都是独立测试的。
              方法是测试的基本单位（单元）。

​     
​    maven借助单元测试，批量的测试你类中的大量方法是否符合预期的。


  2）使用步骤
    1.加入依赖，在pom.xml加入单元测试依赖
      <!-- 单元测试 -->
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.11</version>
       <scope>test</scope>
     </dependency>

   2.在maven项目中的src/test/java目录下，创建测试程序。
     推荐的创建类和方法的提示：
     1.测试类的名称 是Test + 你要测试的类名
     2.测试的方法名称 是：Test + 方法名称

​     例如你要测试HelloMaven ,
​     创建测试类 TestHelloMaven
​     @Test
​     public void testAdd(){
​      测试HelloMaven的add方法是否正确
​     }


     其中testAdd叫做测试方法，它的定义规则
     1.方法是public的，必须的
     2.方法没有返回值， 必须的
     3.方法名称是自定义的，推荐是Test + 方法名称
     4.在方法的上面加入 @Test

  

   3)mvn compile 
    编译main/java/目录下的java 为class文件， 同时把class拷贝到 target/classes目录下面
     把main/resources目录下的所有文件 都拷贝到target/classes目录下

）：用的是junit， junit是一个专门测试的框架（工具）。
    junit测试的内容： 测试的是类中的方法， 每一个方法都是独立测试的。
              方法是测试的基本单位（单元）。

​     
​    maven借助单元测试，批量的测试你类中的大量方法是否符合预期的。

 **2）使用步骤**
    1.加入依赖，在pom.xml加入单元测试依赖
      <!-- 单元测试 -->
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.11</version>
       <scope>test</scope>
     </dependency>

   2.在maven项目中的src/test/java目录下，创建测试程序。
     推荐的创建类和方法的提示：
     1.测试类的名称 是Test + 你要测试的类名
     2.测试的方法名称 是：Test + 方法名称

​     例如你要测试HelloMaven ,
​     创建测试类 TestHelloMaven
​     @Test
​     public void testAdd(){
​      测试HelloMaven的add方法是否正确
​     }


     其中testAdd叫做测试方法，它的定义规则
     1.方法是public的，必须的
     2.方法没有返回值， 必须的
     3.方法名称是自定义的，推荐是Test + 方法名称
     4.在方法的上面加入 @Test

**3)mvn compile** 
    编译main/java/目录下的java 为class文件， 同时把class拷贝到 target/classes目录下面
     把main/resources目录下的所有文件 都拷贝到target/classes目录下

```html
按下这个开始测试
```

![img](https://img-blog.csdnimg.cn/20210531234826385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**这是错误的测试结果**

**测试成功**![img](https://img-blog.csdnimg.cn/20210531234744264.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 

```java
import static org.junit.Assert.*; //导入Assert类的所有静态方法，自JDK1.5引入

import org.example.DateUtil;
import org.junit.Assert;
import org.junit.Test;

public class YearTest {
    @Test
    public void test1() {
        assertEquals(true, new DateUtil().isLeapYear(-100));
    }
    @Test
    public void test2() {
        assertEquals(false, new DateUtil().isLeapYear(1000));
    }
    @Test
    public void test3() {
        assertEquals(false, new DateUtil().isLeapYear(20000));
    }
    @Test
    public void test4() {
        assertEquals(true, new DateUtil().isLeapYear(2020));
    }
    @Test
    public void test5() {
        assertEquals(false, new DateUtil().isLeapYear(2019));
    }
    @Test
    public void test6() {
        assertEquals(true, new DateUtil().isLeapYear(2000));
    }
    @Test
    public void test7() {
        assertEquals(false, new DateUtil().isLeapYear(1900));
    }

}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
package org.example;

// DateUtil.java
public class DateUtil {
    public boolean isLeapYear(int year) {
        boolean result = false;
        if(year<=0||year>10000)
            return false;
        if(year%100==0) {
            if(year%400==0)
                result=true;
        }
        else
        if(year%4==0)
            result=true;
        return result;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**pom中的build暂时用不到，可以删掉**

```XML
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

	<modelVersion>4.0.0</modelVersion>
	<groupId>org.example</groupId>
	<artifactId>untitled2</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<!-- TODO project name  -->
	<name>quickstart</name>
	<description></description>

	<!-- TODO
		<organization>
		<name>company name</name>
		<url>company url</url>
		</organization>
	-->

	<licenses>
		<license>
			<name>The Apache Software License, Version 2.0</name>
			<url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
			<distribution>repo</distribution>
		</license>
	</licenses>

	<dependencies>
		<!--  WICKET DEPENDENCIES -->

		<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket</artifactId>
			<version>${wicket.version}</version>
		</dependency>
		<!-- OPTIONAL 
			<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket-extensions</artifactId>
			<version>${wicket.version}</version>
			</dependency>
		-->

		<!-- LOGGING DEPENDENCIES - LOG4J -->

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.4.2</version>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.14</version>
		</dependency>

		<!--  JUNIT DEPENDENCY FOR TESTING -->
		 <dependency>
				 <groupId>junit</groupId>
				 <artifactId>junit</artifactId>
				 <version>3.8.2</version>
				 <scope>test</scope>
		 </dependency>

		<!--  JETTY DEPENDENCIES FOR TESTING  -->

		<dependency>
			<groupId>org.mortbay.jetty</groupId>
			<artifactId>jetty</artifactId>
			<version>${jetty.version}</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.mortbay.jetty</groupId>
			<artifactId>jetty-util</artifactId>
			<version>${jetty.version}</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.mortbay.jetty</groupId>
			<artifactId>jetty-management</artifactId>
			<version>${jetty.version}</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

	
	<properties>
		<wicket.version>1.3.2</wicket.version>
		<jetty.version>6.1.4</jetty.version>
	</properties>

</project>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)