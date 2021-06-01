# maven



1部分
完成一个java项目，需要做哪些工作
1.分析项目要做什么，知道项目有哪些组成部分。
2.设计项目，通过哪些步骤，使用哪些技术。需要多少人， 多长的时间。
3.组建团队，招人， 购置设备，服务器， 软件， 笔记本。
4.开发人员写代码。 开发人员需要测试自己写代码。 重复多次的工作。
5.测试人员，测试项目功能是否符合要求。
  测试开发人员提交代码-如果测试有问题--需要开发人员修改--在提交代码给测试
  --测试人员在测试代码-如果还有问题-在交给开发人员-开发人员在提交-在测试
  直到-测试代码通过。


2.传统开发项目的问题，没有使用maven【meivn】管理的项目
  1）很多模块，模块之间有关系， 手工管理关系，比较繁琐。
  2）需要很多第三方功能， 需要很多jar文件，需要手工从网络中获取各个jar
  3）需要管理jar的版本， 你需要的是mysql.5.1.5.jar 拿你不能给给一个mysql.4.0.jar
  4）管理jar文件之间的依赖， 你的项目要使用a.jar 需要使用b.jar里面的类。
     必须首先获取到b.jar才可以， 然后才能使用a.jar. 

     a.jar需要b.jar这个关系叫做依赖， 或者你的项目中要使用mysql的驱动， 也可以叫做项目依赖mysql驱动。
     a.class使用b.class， a依赖b类

 3.需要改进项目的开发和管理，需要maven
    1）maven可以管理jar文件
    2）自动下载jar和他的文档，源代码
    3）管理jar直接的依赖， a.jar需要b.jar ， maven会自动下载b.jar
    4）管理你需要的jar版本
    5）帮你编译程序，把java编译为class
    6）帮你测试你的代码是否正确。
    7）帮你打包文件，形成jar文件，或者war文件
    8）帮你部署项目

 4.构建： 项目的构建。
   构建是面向过程的，就是一些步骤，完成项目代码的编译，测试，运行，打包，部署等等。
   maven支持的构建包括有：
    1.清理， 把之前项目编译的东西删除掉，我新的编译代码做准备。
    2.编译， 把程序源代码编译为执行代码， java-class文件
             批量的，maven可以同时把成千上百的文件编译为class。
	     javac 不一样，javac一次编译一个文件。
    3.测试， maven可以执行测试程序代码，验证你的功能是否正确。
             批量的，maven同时执行多个测试代码，同时测试很多功能。
    4.报告， 生成测试结果的文件， 测试通过没有。
    5.打包， 把你的项目中所有的class文件，配置文件等所有资源放到一个压缩文件中。
              这个压缩文件就是项目的结果文件， 通常java程序，压缩文件是jar扩展名的。
	      对于web应用，压缩文件扩展名是.war
    6.安装， 把5中生成的文件jar，war安装到本机仓库
    7.部署， 把程序安装好可以执行。



 5.maven核心概念： 用好maven，了解这些概念
①POM ： 一个文件 名称是pom.xml ,  pom翻译过来叫做项目对象模型。 
         maven把一个项目当做一个模型使用。控制maven构建项目的过程，管理jar依赖。

②约定的目录结构 ： maven项目的目录和文件的位置都是规定的。

③坐标 ： 是一个唯一的字符串，用来表示资源的。

④依赖管理 ： 管理你的项目可以使用jar文件

⑤仓库管理（了解） ：你的资源存放的位置

⑥生命周期 (了解) ： maven工具构建项目的过程，就是生命周期。
⑦插件和目标（了解）：执行maven构建的时候用的工具是插件
⑧继承
⑨聚合

讲maven的使用，先难后易的。 难是说使用maven的命令，完成maven使用 ， 在idea中直接使用maven，代替命令。

6.maven工具的安装和配置。
  1）需要从maven的官网下载maven的安装包 apache-maven-3.3.9-bin.zip
  2）解压安装包，解压到一个目录，非中文目录。
      子目录 bin :执行程序，主要是mvn.cmd
		       conf :maven工具本身的配置文件 settings.xml
  3）配置环境变量
     在系统的环境变量中，指定一个M2_HOME的名称， 指定它的值是maven工具安装目录，bin之前的目录

    M2_HOME=D:\work\maven_work\apache-maven-3.3.9
    
     再把M2_HOME加入到path之中，在所有路径之前加入 %M2_HOME%\bin;

  4）验证，新的命令行中，执行mvn -v

	注意：需要配置JAVA_HOME ，指定jdk路径

  

	C:\Users\Administrator>mvn -v
	 出现如下内容，maven安装，配置正确。
	Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:4
	Maven home: D:\work\maven_work\apache-maven-3.3.9
	Java version: 1.8.0_40, vendor: Oracle Corporation
	Java home: C:\java\JDK8-64\jre
	Default locale: zh_CN, platform encoding: GBK
	OS name: "windows 7", version: "6.1", arch: "amd64", family: "dos"


二部分
 1.maven约定的目录结构， 约定是大家都遵循的一个规则。

   每一个maven项目在磁盘中都是一个文件夹（项目-Hello）
	Hello/
	  ---/src
	  ------/main           #放你主程序java代码和配置文件
	  ----------/java       #你的程序包和包中的java文件
	  ----------/resources  #你的java程序中要使用的配置文件

	  ------/test  #放测试程序代码和文件的（可以没有）
	  ----------/java       #测试程序包和包中的java文件
	  ----------/resources  #测试java程序中要使用的配置文件
	
	  ---/pom.xml  #maven的核心文件（maven项目必须有）


 2.疑问： mvn compile 编译src/main目录下的所有java文件的。
   1）为什么要下载
	   maven工具执行的操作需要很多插件（java类--jar文件）完成的
	2）下载什么东西了
	    jar文件--叫做插件--插件是完成某些功能

	3）下载的东西存放到哪里了。
	    默认仓库（本机仓库）：
	   C:\Users\（登录操作系统的用户名）Administrator\.m2\repository



	Downloading: https://repo.maven.apache.org/maven2/org/apache/maven/maven-plugin-parameter-documenter-2.0.9.pom

   https://repo.maven.apache.org ：中央仓库的地址


	执行mvn compile， 结果是在项目的根目录下生成target目录（结果目录），
	maven编译的java程序，最后的class文件都放在target目录中


	设置本机存放资源的目录位置(设置本机仓库)：
	 1. 修改maven的配置文件， maven安装目录/conf/settings.xml
	    先备份 settings.xml
	
	 2. 修改 <localRepository>  指定你的目录（不要使用中文目录）
	
	 D:\work\maven_work\maven_repository

 3.仓库
  1）仓库是什么： 仓库是存放东西的， 存放maven使用的jar 和 我们项目使用的jar
     > maven使用的插件（各种jar）
	  > 我项目使用的jar(第三方的工具)

  2）仓库的分类
     >本地仓库， 就是你的个人计算机上的文件夹，存放各种jar
	  >远程仓库， 在互联网上的，使用网络才能使用的仓库
	      ①：中央仓库，最权威的， 所有的开发人员都共享使用的一个集中的仓库，
			      https://repo.maven.apache.org ：中央仓库的地址
			②：中央仓库的镜像：就是中央仓库的备份， 在各大洲，重要的城市都是镜像。

			③：私服，在公司内部，在局域网中使用的， 不是对外使用的。

  3）仓库的使用，maven仓库的使用不需要人为参与。 
       
		开发人员需要使用mysql驱动--->maven首先查本地仓库--->私服--->镜像--->中央仓库

 4.pom:项目对象模型，是一个pom.xml文件
    1）坐标：唯一值， 在互联网中唯一标识一个项目的
	  <groupId>公司域名的倒写</groupId>
     <artifactId>自定义项目名称</artifactId>
     <version>自定版本号</version>

	   https://mvnrepository.com/ 搜索使用的中央仓库， 使用groupId 或者 artifactId作为搜索条件


	  2） packaging： 打包后压缩文件的扩展名，默认是jar ，web应用是war 
	      packaging 可以不写， 默认是jar


	 3) 依赖
	    dependencies 和dependency ，相当于是 java代码中import
	
		 你的项目中要使用的各种资源说明， 比我的项目要使用mysql驱动
	
		 <dependencies>
			<!--依赖  java代码中 import -->
		  <dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.9</version>
		 </dependency>
	 
	 </dependencies>
	
	  4）properties：设置属性
	
	  5）build ： maven在进行项目的构建时， 配置信息，例如指定编译java代码使用的jdk的版本等

  5.maven生命周期， maven的命令，maven的插件
    maven的生命周期：就是maven构建项目的过程，清理，编译，测试，报告，打包，安装，部署
	 maven的命令：maven独立使用，通过命令，完成maven的生命周期的执行。
	              maven可以使用命令，完成项目的清理，编译，测试等等

	 maven的插件： maven命令执行时，真正完成功能的是插件，插件就是一些jar文件， 一些类。

MOOC的课程上关于maven的安装太简单了，导致这几天停更，一直在搞这些配置。

现在成功可以使用IDEA进行maven的一些操作了

我在b站上找到了关于maven的课程，进行了进一步学习，自己配置了set，把那个下载仓库的地方改成了阿里云，确实很快啊。下面是我的setting的代码

```html
<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<!--
 | This is the configuration file for Maven. It can be specified at two levels:
 |
 |  1. User Level. This settings.xml file provides configuration for a single user,
 |                 and is normally provided in ${user.home}/.m2/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -s /path/to/user/settings.xml
 |
 |  2. Global Level. This settings.xml file provides configuration for all Maven
 |                 users on a machine (assuming they're all using the same Maven
 |                 installation). It's normally provided in
 |                 ${maven.conf}/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -gs /path/to/global/settings.xml
 |
 | The sections in this sample file are intended to give you a running start at
 | getting the most out of your Maven installation. Where appropriate, the default
 | values (values used when the setting is not specified) are provided.
 |
 |-->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <!--本地仓库的地址,存放jar包 -->
  <localRepository>D:/Maven_jar</localRepository>

  <!-- interactiveMode
   | This will determine whether maven prompts you when it needs input. If set to false,
   | maven will use a sensible default value, perhaps based on some other setting, for
   | the parameter in question.
   |
   | Default: true
  <interactiveMode>true</interactiveMode>
  -->

  <!-- offline
   | Determines whether maven should attempt to connect to the network when executing a build.
   | This will have an effect on artifact downloads, artifact deployment, and others.
   |
   | Default: false
  <offline>false</offline>
  -->

  <!-- pluginGroups
   | This is a list of additional group identifiers that will be searched when resolving plugins by their prefix, i.e.
   | when invoking a command line like "mvn prefix:goal". Maven will automatically add the group identifiers
   | "org.apache.maven.plugins" and "org.codehaus.mojo" if these are not already contained in the list.
   |-->
  <pluginGroups>
    <!-- pluginGroup
     | Specifies a further group identifier to use for plugin lookup.
    <pluginGroup>com.your.plugins</pluginGroup>
    -->
  </pluginGroups>

  <!-- proxies
   | This is a list of proxies which can be used on this machine to connect to the network.
   | Unless otherwise specified (by system property or command-line switch), the first proxy
   | specification in this list marked as active will be used.
   |-->
  <proxies>
    <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies>

  <!-- servers
   | This is a list of authentication profiles, keyed by the server-id used within the system.
   | Authentication profiles can be used whenever maven must make a connection to a remote server.
   |-->
  <servers>
    <!-- server
     | Specifies the authentication information to use when connecting to a particular server, identified by
     | a unique name within the system (referred to by the 'id' attribute below).
     |
     | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
     |       used together.
     |
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->

    <!-- Another sample, using keys to authenticate.
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>

  <!-- mirrors
   | This is a list of mirrors to be used in downloading artifacts from remote repositories.
   |
   | It works like this: a POM may declare a repository to use in resolving certain artifacts.
   | However, this repository may have problems with heavy traffic at times, so people have mirrored
   | it to several places.
   |
   | That repository definition will have a unique id, so we can create a mirror reference for that
   | repository, to be used as an alternate download site. The mirror site will be the preferred
   | server for that repository.
   |-->
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <!--更换阿里巴巴镜像，更快下载-->
    <mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>

  </mirrors>

  <!-- profiles
   | This is a list of profiles which can be activated in a variety of ways, and which can modify
   | the build process. Profiles provided in the settings.xml are intended to provide local machine-
   | specific paths and repository locations which allow the build to work in the local environment.
   |
   | For example, if you have an integration testing plugin - like cactus - that needs to know where
   | your Tomcat instance is installed, you can provide a variable here such that the variable is
   | dereferenced during the build process to configure the cactus plugin.
   |
   | As noted above, profiles can be activated in a variety of ways. One way - the activeProfiles
   | section of this document (settings.xml) - will be discussed later. Another way essentially
   | relies on the detection of a system property, either matching a particular value for the property,
   | or merely testing its existence. Profiles can also be activated by JDK version prefix, where a
   | value of '1.4' might activate a profile when the build is executed on a JDK version of '1.4.2_07'.
   | Finally, the list of active profiles can be specified directly from the command line.
   |
   | NOTE: For profiles defined in the settings.xml, you are restricted to specifying only artifact
   |       repositories, plugin repositories, and free-form properties to be used as configuration
   |       variables for plugins in the POM.
   |
   |-->
  <profiles>
    <!-- profile
     | Specifies a set of introductions to the build process, to be activated using one or more of the
     | mechanisms described above. For inheritance purposes, and to activate profiles via <activatedProfiles/>
     | or the command line, profiles have to have an ID that is unique.
     |
     | An encouraged best practice for profile identification is to use a consistent naming convention
     | for profiles, such as 'env-dev', 'env-test', 'env-production', 'user-jdcasey', 'user-brett', etc.
     | This will make it more intuitive to understand what the set of introduced profiles is attempting
     | to accomplish, particularly when you only have a list of profile id's for debug.
     |
     | This profile example uses the JDK version to trigger activation, and provides a JDK-specific repo.
    <profile>
      <id>jdk-1.4</id>

      <activation>
        <jdk>1.4</jdk>
      </activation>

      <repositories>
        <repository>
          <id>jdk14</id>
          <name>Repository for JDK 1.4 builds</name>
          <url>http://www.myhost.com/maven/jdk14</url>
          <layout>default</layout>
          <snapshotPolicy>always</snapshotPolicy>
        </repository>
      </repositories>
    </profile>
    -->

    <!--
     | Here is another profile, activated by the system property 'target-env' with a value of 'dev',
     | which provides a specific path to the Tomcat instance. To use this, your plugin configuration
     | might hypothetically look like:
     |
     | ...
     | <plugin>
     |   <groupId>org.myco.myplugins</groupId>
     |   <artifactId>myplugin</artifactId>
     |
     |   <configuration>
     |     <tomcatLocation>${tomcatPath}</tomcatLocation>
     |   </configuration>
     | </plugin>
     | ...
     |
     | NOTE: If you just wanted to inject this configuration whenever someone set 'target-env' to
     |       anything, you could just leave off the <value/> inside the activation-property.
     |
    <profile>
      <id>env-dev</id>

      <activation>
        <property>
          <name>target-env</name>
          <value>dev</value>
        </property>
      </activation>

      <properties>
        <tomcatPath>/path/to/tomcat/instance</tomcatPath>
      </properties>
    </profile>
    -->
  </profiles>

  <!-- activeProfiles
   | List of profiles that are active for all builds.
   |
  <activeProfiles>
    <activeProfile>alwaysActiveProfile</activeProfile>
    <activeProfile>anotherAlwaysActiveProfile</activeProfile>
  </activeProfiles>
  -->
</settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

还有很多看不懂的地方，不过我设置了自己的保存仓库，解决了之前依赖报错的问题。高兴。学习这个真的很难，有很多不懂的地方，当时这样一点一点的进步，也是很高兴的。

下面是作业

给定一个汉字句子，可以输出句子的读音。可以借鉴第三方库：pinyin4j 。这个是网址：[https://mvnrepository.com/artifact/com.belerweb/pinyin4j](https://mvnrepository.com/artifact/com.belerweb/pinyin4j。) 。要求工程是Maven项目。需要Maven编译成功的截图、程序运行的截图、和代码截图。

**java部分**

```java
package com.xxxx;

import net.sourceforge.pinyin4j.*;
import net.sourceforge.pinyin4j.format.HanyuPinyinCaseType;
import net.sourceforge.pinyin4j.format.HanyuPinyinOutputFormat;
import net.sourceforge.pinyin4j.format.HanyuPinyinToneType;
import net.sourceforge.pinyin4j.format.HanyuPinyinVCharType;
import net.sourceforge.pinyin4j.format.exception.BadHanyuPinyinOutputFormatCombination;

public class Test {

    public static void main(String[] args) throws BadHanyuPinyinOutputFormatCombination {
        String chineseString = "我学会了maven";

        HanyuPinyinOutputFormat format = new HanyuPinyinOutputFormat();

        format.setCaseType(HanyuPinyinCaseType.LOWERCASE);


        format.setToneType(HanyuPinyinToneType.WITH_TONE_MARK);


        format.setVCharType(HanyuPinyinVCharType.WITH_U_UNICODE);

        String pinyinString = PinyinHelper.toHanyuPinyinString(chineseString, format," ");
        System.out.println(chineseString);
        System.out.println(pinyinString);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 

pom

```TypeScript
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.xxxx</groupId>
  <artifactId>maven01</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>maven01</name>
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
    <!-- https://mvnrepository.com/artifact/com.belerweb/pinyin4j  -->
    <dependency>
      <groupId>com.belerweb</groupId>
      <artifactId>pinyin4j</artifactId>
      <version>2.5.0</version>
    </dependency>


  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 