## 网络基础知识
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607161252937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![  ](https://img-blog.csdnimg.cn/20210607161427390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607161615327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607161731584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
## UDP编程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607162004257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607162112434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607162120185.png)
**netstat -an查看端口**

```java

import java.net.*;
public class UdpRecv
{
	public static void main(String[] args) throws Exception
	{
		DatagramSocket	ds=new DatagramSocket(3000);
		byte [] buf=new byte[1024];
		DatagramPacket dp=new DatagramPacket(buf,1024);
		
		System.out.println("UdpRecv: 我在等待信息");
		ds.receive(dp);
		System.out.println("UdpRecv: 我接收到信息");
		String strRecv=new String(dp.getData(),0,dp.getLength()) +
		" from " + dp.getAddress().getHostAddress()+":"+dp.getPort(); 
		System.out.println(strRecv);
		
		Thread.sleep(1000);
		System.out.println("UdpRecv: 我要发送信息");
		String str="hello world 222";
		DatagramPacket dp2=new DatagramPacket(str.getBytes(),str.length(), 
				InetAddress.getByName("127.0.0.1"),dp.getPort());
		ds.send(dp2);
		System.out.println("UdpRecv: 我发送信息结束");
		ds.close();
	}
}
```

```java
import java.net.*;
public class UdpSend
{
	public static void main(String [] args) throws Exception
	{
		DatagramSocket ds=new DatagramSocket();
		String str="hello world";
		DatagramPacket dp=new DatagramPacket(str.getBytes(),str.length(),
				InetAddress.getByName("127.0.0.1"),3000);
		
		System.out.println("UdpSend: 我要发送信息");
		ds.send(dp);
		System.out.println("UdpSend: 我发送信息结束");
		
		Thread.sleep(1000);
		byte [] buf=new byte[1024];
		DatagramPacket dp2=new DatagramPacket(buf,1024);
		System.out.println("UdpSend: 我在等待信息");
		ds.receive(dp2);
		System.out.println("UdpSend: 我接收到信息");
		String str2=new String(dp2.getData(),0,dp2.getLength()) +
				" from " + dp2.getAddress().getHostAddress()+":"+dp2.getPort(); 
		System.out.println(str2);
				
		ds.close();
	}
}
```

## TCP
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607163354626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607163605616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607163818608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060716391696.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607163925386.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
import java.net.*;
import java.io.*;

public class TcpClient {
	public static void main(String[] args) {
		try {
			Socket s = new Socket(InetAddress.getByName("127.0.0.1"), 8001); //需要服务端先开启
			
			//同一个通道，服务端的输出流就是客户端的输入流；服务端的输入流就是客户端的输出流
			InputStream ips = s.getInputStream();    //开启通道的输入流
			BufferedReader brNet = new BufferedReader(new InputStreamReader(ips));
			
			OutputStream ops = s.getOutputStream();  //开启通道的输出流
			DataOutputStream dos = new DataOutputStream(ops);			

			BufferedReader brKey = new BufferedReader(new InputStreamReader(System.in));
			while (true) 
			{
				String strWord = brKey.readLine();
				if (strWord.equalsIgnoreCase("quit"))
				{
					break;
				}
				else
				{
					System.out.println("I want to send: " + strWord);
					dos.writeBytes(strWord + System.getProperty("line.separator"));
					
					System.out.println("Server said: " + brNet.readLine());
				}
				
			}
			
			dos.close();
			brNet.close();
			brKey.close();
			s.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```

```java

import java.net.*;
import java.io.*;
public class TcpServer
{
	public static void main(String [] args) 
	{
		try
		{
			ServerSocket ss=new ServerSocket(8001); //驻守在8001端口
			Socket s=ss.accept();                   //阻塞，等到有客户端连接上来
			System.out.println("welcome to the java world");
			InputStream ips=s.getInputStream();     //有人连上来，打开输入流
			OutputStream ops=s.getOutputStream();   //打开输出流
			//同一个通道，服务端的输出流就是客户端的输入流；服务端的输入流就是客户端的输出流
			
			ops.write("Hello, Client!".getBytes());  //输出一句话给客户端
			
			
			BufferedReader br = new BufferedReader(new InputStreamReader(ips));
			//从客户端读取一句话			
			System.out.println("Client said: " + br.readLine());
			
			
			ips.close();          
			ops.close();
			s.close();
			ss.close();
		}
		catch(Exception e)
		{
			e.printStackTrace();
		}
	}
}
```

```java
import java.net.*;
public class TcpServer2
{
	public static void main(String [] args)
	{
		try
		{
			ServerSocket ss=new ServerSocket(8001);
			while(true)
			{
				Socket s=ss.accept();
				System.out.println("来了一个client");
				new Thread(new Worker(s)).start();
			}
			//ss.close();
		}
		catch(Exception e)
		{
			e.printStackTrace();
		}
	}
}
```

```java

import java.net.*;
import java.io.*;

class Worker implements Runnable {
	Socket s;

	public Worker(Socket s) {
		this.s = s;
	}

	public void run() {
		try {
			System.out.println("服务人员已经启动");
			InputStream ips = s.getInputStream();
			OutputStream ops = s.getOutputStream();

			BufferedReader br = new BufferedReader(new InputStreamReader(ips));
			DataOutputStream dos = new DataOutputStream(ops);
			while (true) {
				String strWord = br.readLine();
				System.out.println("client said:" + strWord +":" + strWord.length());
				if (strWord.equalsIgnoreCase("quit"))
					break;
				String strEcho = strWord + " 666";
				// dos.writeBytes(strWord +"---->"+ strEcho +"\r\n");
				System.out.println("server said:" + strWord + "---->" + strEcho);
				dos.writeBytes(strWord + "---->" + strEcho + System.getProperty("line.separator"));
			}
			br.close();
			// 关闭包装类，会自动关闭包装类中所包装的底层类。所以不用调用ips.close()
			dos.close();
			s.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
## HTTP

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607170834738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![ ](https://img-blog.csdnimg.cn/20210607171145740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607171328192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607171408468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210607172009495.png)

```java

import java.io.*;
import java.net.*;
import java.util.*;


public class URLConnectionGetTest
{
   public static void main(String[] args)
   {
      try
      {
         String urlName = "http://www.baidu.com";

         URL url = new URL(urlName);
         URLConnection connection = url.openConnection(); 
         connection.connect();

         // 打印http的头部信息

         Map<String, List<String>> headers = connection.getHeaderFields();
         for (Map.Entry<String, List<String>> entry : headers.entrySet())
         {
            String key = entry.getKey();
            for (String value : entry.getValue())
               System.out.println(key + ": " + value);
         }

         // 输出将要收到的内容属性信息

         System.out.println("----------");
         System.out.println("getContentType: " + connection.getContentType());
         System.out.println("getContentLength: " + connection.getContentLength());
         System.out.println("getContentEncoding: " + connection.getContentEncoding());
         System.out.println("getDate: " + connection.getDate());
         System.out.println("getExpiration: " + connection.getExpiration());
         System.out.println("getLastModifed: " + connection.getLastModified());
         System.out.println("----------");

         BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream(), "UTF-8"));

         // 输出收到的内容
         String line = "";
         while((line=br.readLine()) != null)
         {
        	 System.out.println(line);
         }
         br.close();
      }
      catch (IOException e)
      {
         e.printStackTrace();
      }
   }
}
   
```

```java

import java.io.*;
import java.net.*;
import java.nio.file.*;
import java.util.*;

public class URLConnectionPostTest
{
   public static void main(String[] args) throws IOException
   {
      String urlString = "https://tools.usps.com/go/ZipLookupAction.action";
      Object userAgent = "HTTPie/0.9.2";
      Object redirects = "1";
      CookieHandler.setDefault(new CookieManager(null, CookiePolicy.ACCEPT_ALL));
      
      Map<String, String> params = new HashMap<String, String>();
      params.put("tAddress", "1 Market Street");  
      params.put("tCity", "San Francisco");
      params.put("sState", "CA");
      String result = doPost(new URL(urlString), params, 
         userAgent == null ? null : userAgent.toString(), 
         redirects == null ? -1 : Integer.parseInt(redirects.toString()));
      System.out.println(result);
   }   

   public static String doPost(URL url, Map<String, String> nameValuePairs, String userAgent, int redirects)
         throws IOException
   {        
      HttpURLConnection connection = (HttpURLConnection) url.openConnection();
      if (userAgent != null)
         connection.setRequestProperty("User-Agent", userAgent);
      
      if (redirects >= 0)
         connection.setInstanceFollowRedirects(false);
      
      connection.setDoOutput(true);
      
      //输出请求的参数
      try (PrintWriter out = new PrintWriter(connection.getOutputStream()))
      {
         boolean first = true;
         for (Map.Entry<String, String> pair : nameValuePairs.entrySet())
         {
        	//参数必须这样拼接 a=1&b=2&c=3
            if (first) 
            {
            	first = false;
            }
            else
            {
            	out.print('&');
            }
            String name = pair.getKey();
            String value = pair.getValue();
            out.print(name);
            out.print('=');
            out.print(URLEncoder.encode(value, "UTF-8"));
         }
      }      
      String encoding = connection.getContentEncoding();
      if (encoding == null) 
      {
    	  encoding = "UTF-8";
      }
            
      if (redirects > 0)
      {
         int responseCode = connection.getResponseCode();
         System.out.println("responseCode: " + responseCode);
         if (responseCode == HttpURLConnection.HTTP_MOVED_PERM 
               || responseCode == HttpURLConnection.HTTP_MOVED_TEMP
               || responseCode == HttpURLConnection.HTTP_SEE_OTHER) 
         {
            String location = connection.getHeaderField("Location");
            if (location != null)
            {
               URL base = connection.getURL();
               connection.disconnect();
               return doPost(new URL(base, location), nameValuePairs, userAgent, redirects - 1);
            }
            
         }
      }
      else if (redirects == 0)
      {
         throw new IOException("Too many redirects");
      }
      
      //接下来获取html 内容
      StringBuilder response = new StringBuilder();
      try (Scanner in = new Scanner(connection.getInputStream(), encoding))
      {
         while (in.hasNextLine())
         {
            response.append(in.nextLine());
            response.append("\n");
         }         
      }
      catch (IOException e)
      {
         InputStream err = connection.getErrorStream();
         if (err == null) throw e;
         try (Scanner in = new Scanner(err))
         {
            response.append(in.nextLine());
            response.append("\n");
         }
      }

      return response.toString();
   }
}

```
### JDK HttpClient 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608091931462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java


import java.io.IOException;

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.ResponseHandler;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class HttpComponentsGetTest {

    public final static void main(String[] args) throws Exception {
    	
    	CloseableHttpClient httpClient = HttpClients.createDefault();
        RequestConfig requestConfig = RequestConfig.custom()
                .setConnectTimeout(5000)   //设置连接超时时间
                .setConnectionRequestTimeout(5000) // 设置请求超时时间
                .setSocketTimeout(5000)
                .setRedirectsEnabled(true)//默认允许自动重定向
                .build();
        
        HttpGet httpGet = new HttpGet("http://www.baidu.com");
        httpGet.setConfig(requestConfig);
        String srtResult = "";
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            if(httpResponse.getStatusLine().getStatusCode() == 200){
                srtResult = EntityUtils.toString(httpResponse.getEntity(), "UTF-8");//获得返回的结果                
                System.out.println(srtResult);
            }else
            {
                //异常处理
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                httpClient.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}


```

```java


import java.io.IOException;
import java.net.URLEncoder;
import java.util.ArrayList;
import java.util.List;

import org.apache.http.HttpResponse;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.impl.client.LaxRedirectStrategy;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

public class HttpComponentsPostTest {

    public final static void main(String[] args) throws Exception {
    	
    	//获取可关闭的 httpCilent
        //CloseableHttpClient httpClient = HttpClients.createDefault();
    	CloseableHttpClient httpClient = HttpClientBuilder.create().setRedirectStrategy(new LaxRedirectStrategy()).build();
    	//配置超时时间
        RequestConfig requestConfig = RequestConfig.custom().
                setConnectTimeout(10000).setConnectionRequestTimeout(10000)
                .setSocketTimeout(10000).setRedirectsEnabled(false).build();
         
        HttpPost httpPost = new HttpPost("https://tools.usps.com/go/ZipLookupAction.action");
        //设置超时时间
        httpPost.setConfig(requestConfig);
        
        //装配post请求参数
        List<BasicNameValuePair> list = new ArrayList<BasicNameValuePair>(); 
        list.add(new BasicNameValuePair("tAddress", URLEncoder.encode("1 Market Street", "UTF-8")));  //请求参数
        list.add(new BasicNameValuePair("tCity", URLEncoder.encode("San Francisco", "UTF-8"))); //请求参数
        list.add(new BasicNameValuePair("sState", "CA")); //请求参数
        try {
            UrlEncodedFormEntity entity = new UrlEncodedFormEntity(list,"UTF-8"); 
            //设置post求情参数
            httpPost.setEntity(entity);
            httpPost.setHeader("User-Agent", "HTTPie/0.9.2");
            //httpPost.setHeader("Content-Type","application/form-data");
            HttpResponse httpResponse = httpClient.execute(httpPost);
            String strResult = "";
            if(httpResponse != null){ 
                System.out.println(httpResponse.getStatusLine().getStatusCode());
                if (httpResponse.getStatusLine().getStatusCode() == 200) {
                    strResult = EntityUtils.toString(httpResponse.getEntity());
                }
                else {
                    strResult = "Error Response: " + httpResponse.getStatusLine().toString();
                } 
            }else{
                 
            }
            System.out.println(strResult);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                if(httpClient != null){
                    httpClient.close(); //释放资源
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}


```

```java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.Charset;

public class JDKHttpClientGetTest {

	public static void main(String[] args) throws IOException, InterruptedException {
		doGet();
	}
	
	public static void doGet() {
		try{
			HttpClient client = HttpClient.newHttpClient();
			HttpRequest request = HttpRequest.newBuilder(URI.create("http://www.baidu.com")).build();
			HttpResponse response = client.send(request, HttpResponse.BodyHandlers.ofString());
			System.out.println(response.body());
		}
		catch(Exception e) {
			e.printStackTrace();
		}
	}
}

```

```java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class JDKHttpClientPostTest {

	public static void main(String[] args) throws IOException, InterruptedException {
		doPost();
	}
	
	public static void doPost() {
		try {
			HttpClient client = HttpClient.newBuilder().build();
			HttpRequest request = HttpRequest.newBuilder()
					.uri(URI.create("https://tools.usps.com/go/ZipLookupAction.action"))
					//.header("Content-Type","application/x-www-form-urlencoded")
					.header("User-Agent", "HTTPie/0.9.2")
					.header("Content-Type","application/x-www-form-urlencoded;charset=utf-8")
					//.method("POST", HttpRequest.BodyPublishers.ofString("tAddress=1 Market Street&tCity=San Francisco&sState=CA"))
					//.version(Version.HTTP_1_1)
					.POST(HttpRequest.BodyPublishers.ofString("tAddress=" 
					    + URLEncoder.encode("1 Market Street", "UTF-8") 
					    + "&tCity=" + URLEncoder.encode("San Francisco", "UTF-8") + "&sState=CA"))
					//.POST(HttpRequest.BodyPublishers.ofString("tAddress=" + URLEncoder.encode("1 Market Street", "UTF-8") + "&tCity=" + URLEncoder.encode("San Francisco", "UTF-8") + "&sState=CA"))
					.build();
			HttpResponse response = client.send(request, HttpResponse.BodyHandlers.ofString());
			System.out.println(response.statusCode());
			System.out.println(response.headers());
			System.out.println(response.body().toString());

		}
		catch(Exception e) {
			e.printStackTrace();
		}
	}	
}

```
## 作业
请编写一个群聊的程序，包括服务端程序和客户端程序。 

服务端功能：收到某客户端的信息，将消息在控制台输出，然后，发给其他另外的客户端。

客户端功能：每隔5秒发送一条信息给服务端。然后接收服务器转发过来的消息，并在控制台输出。
```java
package org.example;

import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
/**
 * 请编写一个群聊的程序，包括服务端程序和客户端程序。
 * 服务端功能：收到某客户端的信息，将消息在控制台输出，然后，发给其他另外的客户端。
 * 客户端功能：每隔5秒发送一条信息给服务端。然后接收服务器转发过来的消息，并在控制台输出。
 * @author MSS
 *
 */
public class ChatRoom {
    public static void main(String[] args) {
        //线程池
        ThreadPoolExecutor executor=(ThreadPoolExecutor)Executors.newFixedThreadPool(4);
        List<Socket> slist=new ArrayList<Socket>();
        int num=0;
        try {
            ServerSocket ss=new ServerSocket(8001);
            while(true) {
                Socket s=ss.accept();
                slist.add(s);
                System.out.println("来了一个Client");
                executor.execute(new Worker(s,slist,num));
                num++;
            }
        }catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
package org.example;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.Socket;
import java.util.List;

class Worker implements Runnable{
    Socket s;
    List<Socket> slist;
    int num;

    public Worker(Socket s,List<Socket> slist,int num) {
        this.s=s;
        this.slist=slist;
        this.num=num;
    }
    @Override
    public void run() {
        try {
            System.out.println("服务人员已经启动");
            InputStream ips=s.getInputStream();
            BufferedReader br=new BufferedReader(new InputStreamReader(ips));


            while(true) {
                String strWord=br.readLine();
                System.out.println("No."+num+" client said:"+strWord+":"+strWord.length());
                if(strWord.equals("quit")) {
                    break;
                }
                if(strWord!=null) {
                    for(int i=0;i<slist.size();i++) {
                        Socket sc=slist.get(i);
                        OutputStream ops=sc.getOutputStream();
                        DataOutputStream dos=new DataOutputStream(ops);
                        dos.writeBytes("No."+num+" client said"+strWord+"---->"+System.getProperty("line.separator"));//向所有客户端发送消息
                    }
                }
            }
            br.close();
            //关闭包装类，会自动关闭包装类的底层类。所以不用调用ips.close()

            s.close();
        }catch(Exception e) {
            e.printStackTrace();
        }

    }

}
```

```java
package org.example;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class Client {

    public static void main(String[] args) {
        try {
            Socket s=new Socket(InetAddress.getByName("127.0.0.1"),8001);//需要服务端先开启

            //同一个通道，服务端的输出流就是客户端的输入流，服务端的输入流就是客户端的输出流

            InputStream ips=s.getInputStream(); //开启通道的输入流
            BufferedReader brNet=new BufferedReader(new InputStreamReader(ips));

            OutputStream ops=s.getOutputStream();//开启通道的输入流

            DataOutputStream dos=new DataOutputStream(ops);

            String msg="6666";
            while(true) {
                Thread.sleep(10000);

                dos.writeBytes(msg+System.getProperty("line.separator"));
                System.out.println("Server said:"+brNet.readLine());
            }

        }catch(Exception e) {
            e.printStackTrace();
        }

    }

}

```
## BIO
Blocking IO 堵塞，需要等待
![b ](https://img-blog.csdnimg.cn/20210608100427187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608100818483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608100911792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060810094240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608101009326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
package nio;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;
import java.util.UUID;

public class NioClient {

	public static void main(String[] args) {

		String host = "127.0.0.1";
		int port = 8001;

		Selector selector = null;
		SocketChannel socketChannel = null;

		try 
		{
			selector = Selector.open();
			socketChannel = SocketChannel.open();
			socketChannel.configureBlocking(false); // 非阻塞

			// 如果直接连接成功，则注册到多路复用器上，发送请求消息，读应答
			if (socketChannel.connect(new InetSocketAddress(host, port))) 
			{
				socketChannel.register(selector, SelectionKey.OP_READ);
				doWrite(socketChannel);
			} 
			else 
			{
				socketChannel.register(selector, SelectionKey.OP_CONNECT);
			}

		} catch (IOException e) {
			e.printStackTrace();
			System.exit(1);
		}

		while (true) 
		{
			try 
			{
				selector.select(1000);
				Set<SelectionKey> selectedKeys = selector.selectedKeys();
				Iterator<SelectionKey> it = selectedKeys.iterator();
				SelectionKey key = null;
				while (it.hasNext()) 
				{
					key = it.next();
					it.remove();
					try 
					{
						//处理每一个channel
						handleInput(selector, key);
					} 
					catch (Exception e) {
						if (key != null) {
							key.cancel();
							if (key.channel() != null)
								key.channel().close();
						}
					}
				}
			} 
			catch (Exception e) 
			{
				e.printStackTrace();
			}
		}
	

		// 多路复用器关闭后，所有注册在上面的Channel资源都会被自动去注册并关闭
//		if (selector != null)
//			try {
//				selector.close();
//			} catch (IOException e) {
//				e.printStackTrace();
//			}
//
//		}
	}

	public static void doWrite(SocketChannel sc) throws IOException {
		byte[] str = UUID.randomUUID().toString().getBytes();
		ByteBuffer writeBuffer = ByteBuffer.allocate(str.length);
		writeBuffer.put(str);
		writeBuffer.flip();
		sc.write(writeBuffer);
	}

	public static void handleInput(Selector selector, SelectionKey key) throws Exception {

		if (key.isValid()) {
			// 判断是否连接成功
			SocketChannel sc = (SocketChannel) key.channel();
			if (key.isConnectable()) {
				if (sc.finishConnect()) {
					sc.register(selector, SelectionKey.OP_READ);					
				} 				
			}
			if (key.isReadable()) {
				ByteBuffer readBuffer = ByteBuffer.allocate(1024);
				int readBytes = sc.read(readBuffer);
				if (readBytes > 0) {
					readBuffer.flip();
					byte[] bytes = new byte[readBuffer.remaining()];
					readBuffer.get(bytes);
					String body = new String(bytes, "UTF-8");
					System.out.println("Server said : " + body);
				} else if (readBytes < 0) {
					// 对端链路关闭
					key.cancel();
					sc.close();
				} else
					; // 读到0字节，忽略
			}
			Thread.sleep(3000);
			doWrite(sc);
		}
	}
}

```

```java
package nio;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class NioServer {

    public static void main(String[] args) throws IOException {
    	int port = 8001;
    	Selector selector = null;
    	ServerSocketChannel servChannel = null;
    	
    	try {
			selector = Selector.open();
			servChannel = ServerSocketChannel.open();
			servChannel.configureBlocking(false);
			servChannel.socket().bind(new InetSocketAddress(port), 1024);
			servChannel.register(selector, SelectionKey.OP_ACCEPT);
			System.out.println("服务器在8001端口守候");
		} catch (IOException e) {
			e.printStackTrace();
			System.exit(1);
		}
    	
    	while(true)
    	{
    		try {
    			selector.select(1000);
    			Set<SelectionKey> selectedKeys = selector.selectedKeys();
    			Iterator<SelectionKey> it = selectedKeys.iterator();
    			SelectionKey key = null;
    			while (it.hasNext()) {
    				key = it.next();
    				it.remove();
    				try {
    					handleInput(selector,key);
    				} catch (Exception e) {
    					if (key != null) {
    						key.cancel();
    						if (key.channel() != null)
    							key.channel().close();
    					}
    				}
    			}
    		} 
    		catch(Exception ex)
    		{
    			ex.printStackTrace();    			
    		}
    		
    		try
    		{
    			Thread.sleep(500);
    		}
    		catch(Exception ex)
    		{
    			ex.printStackTrace();    			
    		}
    	}
    }
    
    public static void handleInput(Selector selector, SelectionKey key) throws IOException {

		if (key.isValid()) {
			// 处理新接入的请求消息
			if (key.isAcceptable()) {
				// Accept the new connection
				ServerSocketChannel ssc = (ServerSocketChannel) key.channel();
				SocketChannel sc = ssc.accept();
				sc.configureBlocking(false);
				// Add the new connection to the selector
				sc.register(selector, SelectionKey.OP_READ);
			}
			if (key.isReadable()) {
				// Read the data
				SocketChannel sc = (SocketChannel) key.channel();
				ByteBuffer readBuffer = ByteBuffer.allocate(1024);
				int readBytes = sc.read(readBuffer);
				if (readBytes > 0) {
					readBuffer.flip();
					byte[] bytes = new byte[readBuffer.remaining()];
					readBuffer.get(bytes);
					String request = new String(bytes, "UTF-8"); //接收到的输入
					System.out.println("client said: " + request);
					
					String response = request + " 666";
					doWrite(sc, response);
				} else if (readBytes < 0) {
					// 对端链路关闭
					key.cancel();
					sc.close();
				} else
					; // 读到0字节，忽略
			}
		}
	}

	public static void doWrite(SocketChannel channel, String response) throws IOException {
		if (response != null && response.trim().length() > 0) {
			byte[] bytes = response.getBytes();
			ByteBuffer writeBuffer = ByteBuffer.allocate(bytes.length);
			writeBuffer.put(bytes);
			writeBuffer.flip();
			channel.write(writeBuffer);
		}
	}
}

```
## AIO
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608102245311.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608102325265.png)
![ ](https://img-blog.csdnimg.cn/20210608102426985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608103111334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
package aio;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.CharBuffer;
import java.nio.channels.AsynchronousSocketChannel;
import java.nio.channels.CompletionHandler;
import java.nio.charset.Charset;
import java.nio.charset.CharsetDecoder;
import java.util.UUID;


public class AioClient {

	public static void main(String[] a) {
		try
		{
			AsynchronousSocketChannel channel = AsynchronousSocketChannel.open();
			
			//18行连接成功后，自动做20行任务
			channel.connect(new InetSocketAddress("localhost", 8001), null, new CompletionHandler<Void, Void>() {

				public void completed(Void result, Void attachment) {
					String str = UUID.randomUUID().toString();
					
					//24行向服务器写数据成功后，自动做28行任务
					channel.write(ByteBuffer.wrap(str.getBytes()), null,
							new CompletionHandler<Integer, Object>() {

								@Override
								public void completed(Integer result, Object attachment) {
									try {
										System.out.println("write " + str + ", and wait response");
										//等待服务器响应
										ByteBuffer buffer = ByteBuffer.allocate(1024); //准备读取空间
						                 //开始读取服务器反馈内容，一旦读取结束，做39行任务
										channel.read(buffer, buffer, new CompletionHandler<Integer, ByteBuffer>() {
						                     @Override
						                     public void completed(Integer result_num, ByteBuffer attachment) {
						                         attachment.flip(); //反转此Buffer 
						                         CharBuffer charBuffer = CharBuffer.allocate(1024);
						                         CharsetDecoder decoder = Charset.defaultCharset().newDecoder();
						                         decoder.decode(attachment,charBuffer,false);
						                         charBuffer.flip();
						                         String data = new String(charBuffer.array(),0, charBuffer.limit());
						                         System.out.println("server said: " + data);
						                         try{
						                             channel.close();
						                         }catch (Exception e){
						                        	 e.printStackTrace();
						                         }
						                     }
						      
						                     @Override
						                     public void failed(Throwable exc, ByteBuffer attachment) {
						                         System.out.println("read error "+exc.getMessage());
						                     }
						                 });
						                 
										channel.close();
									} catch (Exception e) {
										e.printStackTrace();
									}
								}

								@Override
								public void failed(Throwable exc, Object attachment) {
									System.out.println("write error");
								}

							});
				}

				public void failed(Throwable exc, Void attachment) {
					System.out.println("fail");
				}

			});
			Thread.sleep(10000);
		}
		catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

```java
package aio;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.CharBuffer;
import java.nio.channels.AsynchronousChannelGroup;
import java.nio.channels.AsynchronousServerSocketChannel;
import java.nio.channels.AsynchronousSocketChannel;
import java.nio.channels.CompletionHandler;
import java.nio.charset.Charset;
import java.nio.charset.CharsetDecoder;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class AioServer {

    public static void main(String[] args) throws IOException {  
    	AsynchronousServerSocketChannel server = AsynchronousServerSocketChannel.open();   
        server.bind(new InetSocketAddress("localhost", 8001));  
        System.out.println("服务器在8001端口守候");
        
        //开始等待客户端连接，一旦有连接，做26行任务
        server.accept(null, new CompletionHandler<AsynchronousSocketChannel, Object>() {  
            @Override  
            public void completed(AsynchronousSocketChannel channel, Object attachment) {  
            	 server.accept(null, this); //持续接收新的客户端请求
            	 
                 ByteBuffer buffer = ByteBuffer.allocate(1024); //准备读取空间
                 //开始读取客户端内容，一旦读取结束，做33行任务
                 channel.read(buffer, buffer, new CompletionHandler<Integer, ByteBuffer>() {
                     @Override
                     public void completed(Integer result_num, ByteBuffer attachment) {
                         attachment.flip(); //反转此Buffer 
                         CharBuffer charBuffer = CharBuffer.allocate(1024);
                         CharsetDecoder decoder = Charset.defaultCharset().newDecoder();
                         decoder.decode(attachment,charBuffer,false);
                         charBuffer.flip();
                         String data = new String(charBuffer.array(),0, charBuffer.limit());
                         System.out.println("client said: " + data);
                         channel.write(ByteBuffer.wrap((data + " 666").getBytes())); //返回结果给客户端
                         try{
                             channel.close();
                         }catch (Exception e){
                        	 e.printStackTrace();
                         }
                     }
      
                     @Override
                     public void failed(Throwable exc, ByteBuffer attachment) {
                         System.out.println("read error "+exc.getMessage());
                     }
                 });
                 

            }  
  
            @Override  
            public void failed(Throwable exc, Object attachment) {  
                System.out.print("failed: " + exc.getMessage());  
            }  
        });  

        while(true){
        	try {
				Thread.sleep(5000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
        }
    }  
}

```
## Netty
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608103843141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608103922809.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608104035696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608104046920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608104321233.png)


```java
package netty1;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;

import java.net.InetSocketAddress;

public class EchoClient {
    
	public static void main(String[] args) throws Exception {
    	String host = "localhost";
        int port = 8001;
        
        EventLoopGroup group = new NioEventLoopGroup();
        try {
            Bootstrap b = new Bootstrap();
            b.group(group)
                .channel(NioSocketChannel.class)
                .remoteAddress(new InetSocketAddress(host, port))
                .handler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    public void initChannel(SocketChannel ch)
                        throws Exception {
                        ch.pipeline().addLast(new EchoClientHandler());
                    }
                });
            ChannelFuture f = b.connect().sync();
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }   
}


```

```java
package netty1;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandler.Sharable;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

public class EchoClientHandler
    extends SimpleChannelInboundHandler<ByteBuf> {
    @Override
    public void channelActive(ChannelHandlerContext ctx) {
        ctx.writeAndFlush(Unpooled.copiedBuffer("Netty rocks!",
                CharsetUtil.UTF_8));
    }

    @Override
    public void channelRead0(ChannelHandlerContext ctx, ByteBuf in) {
        System.out.println(
                "Client received: " + in.toString(CharsetUtil.UTF_8));
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx,
        Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}

```

```java
package netty1;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;

import java.net.InetSocketAddress;

public class EchoServer {
    public static void main(String[] args) throws Exception {
        int port = 8001;
        final EchoServerHandler serverHandler = new EchoServerHandler();
        EventLoopGroup group = new NioEventLoopGroup();
        try {
        	//ServerBootstrap是netty中的一个服务器引导类
            ServerBootstrap b = new ServerBootstrap();
            b.group(group)
                .channel(NioServerSocketChannel.class)  //设置通道类型
                .localAddress(new InetSocketAddress(port))  //设置监听端口
                .childHandler(new ChannelInitializer<SocketChannel>() { //初始化责任链
                    @Override
                    public void initChannel(SocketChannel ch) throws Exception {
                        ch.pipeline().addLast(serverHandler); //添加处理类
                    }
                });

            ChannelFuture f = b.bind().sync();  //开启监听
            if(f.isSuccess()){
            	System.out.println(EchoServer.class.getName() +
                        " started and listening for connections on " + f.channel().localAddress());
            }
            
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully().sync();
        }
    }    
}

```

```java
package netty1;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelFutureListener;
import io.netty.channel.ChannelHandler.Sharable;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

public class EchoServerHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ByteBuf in = (ByteBuf) msg;
        String content = in.toString(CharsetUtil.UTF_8);
        System.out.println("Server received: " + content);
        
        ByteBuf out = ctx.alloc().buffer(1024);
        out.writeBytes((content + " 666").getBytes());
        ctx.write(out);
    }

    @Override
    public void channelReadComplete(ChannelHandlerContext ctx)
            throws Exception {   	
        ctx.writeAndFlush(Unpooled.EMPTY_BUFFER)
                .addListener(ChannelFutureListener.CLOSE);
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx,
        Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}

```
## 邮件工作原理
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110220733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110310527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110439166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110526497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110643858.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608110837321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
package tools;

import javax.mail.*;
import javax.mail.internet.*;
import javax.activation.*;
import java.util.*;

public class MailClientRecv {
  private Session session;
  private Store store;
  private String username = "chenliangyu1980@126.com";
  private String password = "1234567899";
  private String popServer = "pop.126.com";
  
  public void init()throws Exception
  {
    //设置属性
    Properties  props = new Properties();
    props.put("mail.store.protocol", "pop3");
    props.put("mail.imap.class", "com.sun.mail.imap.IMAPStore");
    props.put("mail.pop3.class", "com.sun.mail.pop3.POP3Store");    

    // 创建Session对象
    session = Session.getInstance(props,null);
    session.setDebug(false); //输出跟踪日志

    // 创建Store对象
    store = session.getStore("pop3");
    
    //连接到收邮件服务器
    store.connect(popServer,username,password);
  }  
  
  public void receiveMessage()throws Exception
  {
	String folderName = "inbox";
    Folder folder=store.getFolder(folderName);
    if(folder==null)
    {
    	throw new Exception(folderName+"邮件夹不存在");
    }
    //打开信箱
    folder.open(Folder.READ_ONLY);
    System.out.println("您的收件箱有"+folder.getMessageCount()+"封邮件.");
    System.out.println("您的收件箱有"+folder.getUnreadMessageCount()+"封未读的邮件.");

    //读邮件
    Message[] messages=folder.getMessages();
    //for(int i=1;i<=messages.length;i++)
    for(int i=1;i<=3;i++)  
    {
      System.out.println("------第"+i+"封邮件-------");
      //打印邮件信息
      Message message = messages[i];
      //folder.getMessage(i).writeTo(System.out);
      System.out.println((message.getFrom())[0]);
      System.out.println(message.getSubject());
      System.out.println();
    }
    folder.close(false);  //关闭邮件夹
  }
  
  public void close()throws Exception
  {
	store.close();
  }
  
  public static void main(String[] args)throws Exception {
    MailClientRecv client=new MailClientRecv();
    //初始化
    client.init();
    //接收邮件
    client.receiveMessage();
    //关闭连接
    client.close();
  }
}






```

 

```java
package tools;

import javax.mail.*;
import java.util.*;
import messages.*;


public class MailClientSend {
  private Session session;
  private Transport transport;
  private String username = "lychen@sei.ecnu.edu.cn";
  private String password = "1234567899";
  private String smtpServer = "webmail.ecnu.edu.cn";
  
  public void init()throws Exception
  {
	//设置属性
    Properties  props = new Properties();
    props.put("mail.transport.protocol", "smtp");
    props.put("mail.smtp.class", "com.sun.mail.smtp.SMTPTransport");
    props.put("mail.smtp.host", smtpServer); //设置发送邮件服务器
    props.put("mail.smtp.port", "25");
    props.put("mail.smtp.auth", "true"); //SMTP服务器需要身份验证    

    // 创建Session对象
    session = Session.getInstance(props,new Authenticator(){   //验账账户 
        public PasswordAuthentication getPasswordAuthentication() { 
          return new PasswordAuthentication(username, password); 
        }            
 });
    session.setDebug(true); //输出跟踪日志
    
    // 创建Transport对象
    transport = session.getTransport();           
  }
  
  public void sendMessage()throws Exception{
    //创建一个邮件
    //Message msg = TextMessage.generate();
	//Message msg = HtmlMessage.generate();
	Message msg = AttachmentMessage.generate();
    //发送邮件    
    transport.connect();
    transport.sendMessage(msg, msg.getAllRecipients());
    //打印结果
    System.out.println("邮件已经成功发送");
  } 
  
  public void close()throws Exception
  {
	transport.close();
  }
  
  public static void main(String[] args)throws Exception {
	  
    MailClientSend client=new MailClientSend();
    //初始化
    client.init();
    //发送邮件
    client.sendMessage();
    //关闭连接
    client.close();
  }
}






```
**带有附件**
```java
package messages;

import java.io.FileOutputStream;
import java.util.Properties;
import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.Message;
import javax.mail.Session;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
public class AttachmentMessage 
{
	public static MimeMessage generate() throws Exception
	{
		String from = "lychen@sei.ecnu.edu.cn "; // 发件人地址
		String to = "chenliangyu1980@126.com"; // 收件人地址
		
        String subject = "多附件邮件";        //邮件主题
        String body = "<a href=http://www.ecnu.edu.cn>" +
        			  "欢迎大家访问我们的网站</a></br>"; 
      
        // 创建Session实例对象
        Session session = Session.getDefaultInstance(new Properties());
     	// 创建MimeMessage实例对象
     	MimeMessage message = new MimeMessage(session);            
        message.setFrom(new InternetAddress(from));
        message.setRecipients(Message.RecipientType.TO,
        		InternetAddress.parse(to));
        message.setSubject(subject);
        
        //创建代表邮件正文和附件的各个MimeBodyPart对象
        MimeBodyPart contentPart = createContent(body);
        MimeBodyPart attachPart1 = createAttachment("c:/temp/ecnu4.jpg");
        MimeBodyPart attachPart2 = createAttachment("c:/temp/ecnu5.jpg");
        
        //创建用于组合邮件正文和附件的MimeMultipart对象
        MimeMultipart allMultipart = new MimeMultipart("mixed");
        allMultipart.addBodyPart(contentPart);
        allMultipart.addBodyPart(attachPart1);
        allMultipart.addBodyPart(attachPart2);
        
        //设置整个邮件内容为最终组合出的MimeMultipart对象
        message.setContent(allMultipart);
        message.saveChanges();
        
        //message.writeTo(new FileOutputStream("e:/ComplexMessage.eml"));
        return message;
	}
	
	public static MimeBodyPart createContent(String body) throws Exception
	{
        MimeBodyPart htmlBodyPart = new MimeBodyPart();          
        htmlBodyPart.setContent(body,"text/html;charset=gb2312");
        return htmlBodyPart;
	}
	
	public static MimeBodyPart createAttachment(String filename) throws Exception
	{
		//创建保存附件的MimeBodyPart对象，并加入附件内容和相应信息
		MimeBodyPart attachPart = new MimeBodyPart();
        FileDataSource fds = new FileDataSource(filename);
        attachPart.setDataHandler(new DataHandler(fds));
        attachPart.setFileName(fds.getName());
		return attachPart;
	}
}

```
**带有图片**
```java
package messages;

import java.util.Date;
import java.util.Properties;
import javax.mail.Message;
import javax.mail.Session;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.io.FileOutputStream;

public class HtmlMessage {
	public static MimeMessage generate() throws Exception 
	{
		String from = "lychen@sei.ecnu.edu.cn "; // 发件人地址
		String to = "chenliangyu1980@126.com"; // 收件人地址
		
		String subject = "HTML邮件";
		String body = "<a href=http://www.ecnu.edu.cn>" 
		  + "<h4>欢迎大家访问我们的网站</h4></a></br>" 
		  + "<img src=\"https://news.ecnu.edu.cn/_upload/article/images/2e/e2/6b554d034c9192101208c732195e/16a6ec66-6729-4469-a5f4-0435e0f2e66a.jpg\">";

		// 创建Session实例对象
		Session session = Session.getDefaultInstance(new Properties());
		// 创建MimeMessage实例对象
		MimeMessage message = new MimeMessage(session);
		// 设置发件人
		message.setFrom(new InternetAddress(from));
		// 设置收件人
		message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(to));
		// 设置发送日期
		message.setSentDate(new Date());
		// 设置邮件主题
		message.setSubject(subject);
		// 设置HTML格式的邮件正文
		message.setContent(body, "text/html;charset=gb2312");
		// 保存并生成最终的邮件内容
		message.saveChanges();

		// 把MimeMessage对象中的内容写入到文件中
		//msg.writeTo(new FileOutputStream("e:/HtmlMessage.eml"));
		return message;
	}
}

```
**正常邮件**

```java
package messages;

import java.util.Date;
import java.util.Properties;
import javax.mail.Message;
import javax.mail.Session;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.io.FileOutputStream;

public class TextMessage {
	public static MimeMessage generate() throws Exception {
		String from = "lychen@sei.ecnu.edu.cn "; // 发件人地址
		String to = "chenliangyu1980@126.com"; // 收件人地址
		
		String subject = "test";
		String body = "您好,这是来自一封chenliangyu的测试邮件";

		// 创建Session实例对象
		Session session = Session.getDefaultInstance(new Properties());
		// 创建MimeMessage实例对象
		MimeMessage message = new MimeMessage(session);
		// 设置发件人
		message.setFrom(new InternetAddress(from));
		// 设置收件人
		message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(to));
		// 设置发送日期
		message.setSentDate(new Date());
		// 设置邮件主题
		message.setSubject(subject);
		// 设置纯文本内容的邮件正文
		message.setText(body);
		// 保存并生成最终的邮件内容
		message.saveChanges();

		// 把MimeMessage对象中的内容写入到文件中
		//msg.writeTo(new FileOutputStream("e:/test.eml"));
		return message;
	}
}

```

请基于NIO(第6章第六节的NIO，非AIO)编写一个群聊的程序，包括服务端程序和客户端程序。 



服务端功能：只用一个线程，收到某客户端的信息，将消息在控制台输出，然后，发给其他另外的客户端。



客户端功能：每隔5秒发送一条信息给服务端。然后接收服务器转发过来的消息，并在控制台输出。
```java
package com.yu.nio;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;
import java.util.Set;
/**
* @author yucheng
* @create 2020-07-03 13:15
* 使用nio编写一个群聊系统，实现服务器端和客户端的数据通信，
* 服务器端的功能：接收客户端发送的信息，并且转发给其余的客户端
*/
public class GroupChatServerDemo {
//定义属性信息
private ServerSocketChannel serverSocketChannel; //在服务器端监听新的sockerChannnel连接
private Selector selector; //选择器
private static final Integer PORT = 8080; //监听的端口信息
/**
* 定义构造方法
*/
public GroupChatServerDemo(){
try {
serverSocketChannel = ServerSocketChannel.open();
selector = Selector.open();
serverSocketChannel.configureBlocking(false); //设置管道为非阻塞模式
serverSocketChannel.socket().bind(new InetSocketAddress(PORT));//设置端口信息
//把通道注册到selector上，事件为监听事件
serverSocketChannel.register(selector,SelectionKey.OP_ACCEPT);
System.out.println("服务器端准备好了");
}catch (Exception e){
e.printStackTrace();
}
}
/**
* 定义监听客户端有连接请求的方法
*/
public void seleorSocketChannel(){
try {
//循环进行处理
while(true){
//如果连接大于0，有新的连接请求
if(selector.select() > 0){
Iterator<SelectionKey> selectionKeyIterator = selector.selectedKeys().iterator();
while (selectionKeyIterator.hasNext()) {
SelectionKey selectionKey = selectionKeyIterator.next();
//如果事件是监听事件，则把事件注册到选择器，事件为读
if(selectionKey.isAcceptable()){
SocketChannel socketChannel = serverSocketChannel.accept();
socketChannel.configureBlocking(false);
socketChannel.register(selector,SelectionKey.OP_READ);
//有新的用户上线
System.out.println("有新用户上线了！！！");
}
//如果事件是读的事件，把读取获取到的数据信息
if(selectionKey.isReadable()){
readData(selectionKey);
}
}
//移除使用过的selectionKeyIterator
selectionKeyIterator.remove();
}
}
}catch (Exception e){
e.printStackTrace();
}
}
/**
* 读取传输过来的数据
* @param selectionKey 注册关系
*/
private void readData(SelectionKey selectionKey) {
SocketChannel channel = null;
try {
channel = (SocketChannel)selectionKey.channel();//获取通道信息
//ByteBuffer buffer = (ByteBuffer)selectionKey.attachment();//获取缓冲区
ByteBuffer buffer = ByteBuffer.allocate(1024);
//开始读取数据信息
if(channel.read(buffer) > 0){
String msg = new String(buffer.array());
System.out.println(channel.getLocalAddress().toString().substring(1)+"：说" + msg);
//把消息转发给其他客户端
sendOtherClients(channel,msg);
}
}catch (Exception e){
try {
System.out.println(channel.getLocalAddress().toString().substring(1)+"离线了!!!");
selectionKey.cancel();//关闭selectionKey
channel.close();//关闭channel
} catch (Exception e1) {
e1.printStackTrace();
}
e.printStackTrace();
}
}
/**
* 转发信息给塔器客户端，但是要排除自己
* @param channel 通道
* @param msg 发送的信息
*/
private void sendOtherClients(SocketChannel channel, String msg) {
//selector.keys()代表注册到选择器上的所有通道
//selector.selectedKeys()代表监听的时候注册到选择器上的所有通道
try {
Set<SelectionKey> keys = selector.keys();
for (SelectionKey key : keys) {
SelectableChannel selectableChannel = key.channel();
if(selectableChannel instanceof SocketChannel && selectableChannel != channel) {
SocketChannel socketChannel = (SocketChannel)selectableChannel;
ByteBuffer byteBuffer = ByteBuffer.wrap(msg.getBytes());
socketChannel.write(byteBuffer);
}
}
} catch (Exception e) {
e.printStackTrace();
}
}
public static void main(String[] args) {
GroupChatServerDemo groupChatServerDemo = new GroupChatServerDemo();
groupChatServerDemo.seleorSocketChannel();
}
}
```

```java
package com.yu.nio;
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Scanner;
/**
* @author yucheng
* @create 2020-07-03 13:15
* 使用nio编写一个群聊系统，实现服务器端和客户端的数据通信，
*服务器端的功能：接收客户端发送的信息，并且转发给其余的客户端
*/
public class GroupChatClientDemo {
//定义属性信息
private SocketChannel socketChannel;//定义网络IO通道
private Selector selector;//定义选择器
private static final Integer PORT = 8080; //监听的端口信息
private final String HOST = "127.0.0.1";//定义ip地址
private String userName;//用户名
/**
* 定义构造方法
*/
public GroupChatClientDemo(){
try {
socketChannel = SocketChannel.open(new InetSocketAddress(HOST,PORT));
selector = Selector.open();
socketChannel.configureBlocking(false);
socketChannel.register(selector,SelectionKey.OP_READ);
userName = socketChannel.getLocalAddress().toString().substring(1);
System.out.println(userName + "：准备好了");
}catch (Exception e){
e.printStackTrace();
}
}
/**
* 定义发送消息的方法
* @param msg 消息
*/
public void sendMsg(String msg){
try {
msg = userName + "说：" + msg;
ByteBuffer byteBuffer = ByteBuffer.wrap(msg.getBytes());
socketChannel.write(byteBuffer);
} catch (IOException e) {
e.printStackTrace();
}
}
/**
* 获取服务器转发的消息
*/
public void readMsg(){
try {
//大于0代表有通道信息
if(selector.select() > 0){
Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
while(iterator.hasNext()){
SelectionKey selectionKey = iterator.next();
//获取读的操作
if(selectionKey.isReadable()){
SocketChannel socketChannel = (SocketChannel)selectionKey.channel();
ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
socketChannel.read(byteBuffer);
System.out.println("服务器转发的信息为:" + new String(byteBuffer.array()));
}
}
iterator.remove();
}
}catch (Exception e){
e.printStackTrace();
}
}
public static void main(String[] args) {
final GroupChatClientDemo groupChatClientDemo = new GroupChatClientDemo();
//启动一个线程循环去读取服务器转发的信息
new Thread(new Runnable() {
public void run() {
while(true){
groupChatClientDemo.readMsg();
try {
Thread.sleep(3000);
} catch (InterruptedException e) {
e.printStackTrace();
}
}
}
}).start();
//调用发送信息的方法
Scanner scanner = new Scanner(System.in);
if(scanner.hasNextLine()){
groupChatClientDemo.sendMsg(scanner.nextLine());
}
}
}
```