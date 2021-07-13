

# xml

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601175844385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
实例
```xml
<?xml version="1.0" encoding="gb2312"?>
 
<class>
    <stu id="001">
        <name>杨过</name> 
        <sex>男</sex>
        <age>20</age>
    </stu>  
    <stu id="002">
        <name>小龙女</name>    
        <sex>女</sex>
        <age>21</age>
    </stu>
</class>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601175914959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**xml语法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180404701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180502803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**dom**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180514272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**空格也是一个元素**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180535827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180608639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**SAX**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180653428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601180742800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**dom示例**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601181200986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601181217648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601192412351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194014715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194105977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**dom解析方法示例**

```java
package com.xinsi.qi.utils;
 
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;
 
import java.io.File;
import java.util.List;
 
public class Dom4jXml {
    public void test(){
        try {
            File inputFile = new File("F:\\J2EE学习资料\\demoLes03\\web\\WEB-INF\\test.xml");
            SAXReader reader = new SAXReader();
            Document document = reader.read(inputFile);
            System.out.println("Root element :"+document.getRootElement().getName());
 
            Element classElement = document.getRootElement();
 
            List<Node> nodes = document.selectNodes("/class/part[@id='02']");
 
            System.out.println("--------------------");
 
            for (Node node:nodes){
                System.out.println("标签名=:"+node.getName());
                System.out.println("姓名:"+node.selectSingleNode("name").getText());
                System.out.println("年龄:"+node.selectSingleNode("age").getText());
                System.out.println("性别:"+node.selectSingleNode("sex").getText());
            }
        } catch (Exception e1) {
            e1.printStackTrace();
        }
    }
 
}
```
## JSON
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194346635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194402987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194502272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601194513313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**关于他俩的比较，我觉得下面这个哥说的很好**
https://blog.csdn.net/qq_41684621/article/details/113851644?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162253650516780271580772%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162253650516780271580772&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-113851644.first_rank_v2_pc_rank_v29&utm_term=JSON&spm=1018.2226.3001.4187


**作业**
读入以下的score.xml文件，并转化为JSON  Object对象，输出JSON Object的字符串表达式。最后，根据JSON Object 的值和score.xml的样式，重新生成score2.xml。



<student>

  <name>Tom</name>

  <subject>math</subject>

  <score>80</score>

</student>



读取和写入xml方法不限，JSON处理方法不限。



最后，需要提交源码，score.xml截图，json 字符串输出截图，score2.xml截图

```java
package com.hou;



        import java.util.List;

public class Student {
    private String name;
    private String subject;
    private String score;
    public Student() {
    }
    public Student(String name,String subject, String score) {
        this.name = name;
        this.subject=subject;
        this.score = score;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSubject() {
        return subject;
    }
    public void setSubject(String subject) {
        this.subject = subject;
    }
    public String getScore() {
        return score;
    }
    public void setScore(String score) {
        this.score = score;
    }
}

```

```java
package com.hou;


import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import java.io.File;
import java.util.*;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import com.google.gson.Gson;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.reflect.TypeToken;

public class Xml2json {
    static ArrayList<Student> S=new ArrayList<Student>();
    public static void readXml()
    {

        try
        {
            //采用Dom解析xml文件，固定格式
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
            DocumentBuilder db = dbf.newDocumentBuilder();
            Document document = db.parse("D:\\test\\score.xml");

            //获取所有的一级子节点
            NodeList usersList = document.getChildNodes();

            for (int i = 0; i < usersList.getLength(); i++)
            {
                Node users = usersList.item(i);         //一级子节点
                if (users.getNodeType() == Node.ELEMENT_NODE) {
                    NodeList userList = users.getChildNodes(); //获取二级子节点user的列表
                    Student tmp=new Student();
                    for (int j = 0; j < userList.getLength(); j++) //9
                    {
                        Node meta = userList.item(j);//二级子节点
                        if (meta.getNodeType() == Node.ELEMENT_NODE)
                        {
                            String tag=userList.item(j).getNodeName();

                            if(tag.equals("name")) {
                                tmp.setName(meta.getTextContent());

                            }
                            else if(tag.equals("subject")) {
                                tmp.setSubject(meta.getTextContent());

                            }
                            else {
                                tmp.setScore(meta.getTextContent());

                            }
                        }

                    }
                    S.add(tmp);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void createAndWrite() {
        Gson gson = new Gson();
        for(int i=0;i<S.size();i++) {
            String ob = gson.toJson(S.get(i));
            System.out.println(ob);
            JsonObject json = gson.toJsonTree(S.get(i)).getAsJsonObject();
            try {
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dbBuilder = dbFactory.newDocumentBuilder();

                //新创建一个Document节点
                Document document = dbBuilder.newDocument();
                if (document != null)
                {
                    Element student = document.createElement("student");	//都是采用Document创建元素
                    Element name = document.createElement("name");
                    Element score = document.createElement("score");
                    Element subject = document.createElement("subject");
                    name.appendChild(document.createTextNode(gson.fromJson(json.get("name"),String.class)));
                    score.appendChild(document.createTextNode(gson.fromJson(json.get("score"),String.class)));
                    subject.appendChild(document.createTextNode(gson.fromJson(json.get("subject"),String.class)));

                    student.appendChild(name);
                    student.appendChild(score);
                    student.appendChild(subject);
                    document.appendChild(student);    //把docx挂在document下

                    TransformerFactory transformerFactory = TransformerFactory.newInstance();
                    Transformer transformer = transformerFactory.newTransformer();
                    DOMSource source = new DOMSource(document);

                    //定义目标文件
                    File file = new File("C:\\Users\\86187\\desktop\\score2.xml");
                    StreamResult result = new StreamResult(file);

                    //将xml内容写入到文件中
                    transformer.transform(source, result);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

    public static void main(String[] args) {
        readXml();
        createAndWrite();
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.hou</groupId>
  <artifactId>untitled3</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>untitled3</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>


  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.12.1</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310 -->
    <dependency>
      <groupId>com.fasterxml.jackson.datatype</groupId>
      <artifactId>jackson-datatype-jsr310</artifactId>
      <version>2.12.3</version>
    </dependency>

    <dependency>

    <groupId>com.fasterxml.jackson.core</groupId>

    <artifactId>jackson-databind</artifactId>

    <version>2.9.6</version>

  </dependency>


    <dependency>

    <groupId>com.google.code.gson</groupId>

    <artifactId>gson</artifactId>

    <version>2.7</version>

  </dependency>


    <dependency>

    <groupId>org.json</groupId>

    <artifactId>json</artifactId>

    <version>20180813</version>

  </dependency>

  </dependencies>



</project>

```