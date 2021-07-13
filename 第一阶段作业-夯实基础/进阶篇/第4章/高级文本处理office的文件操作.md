## 图形图像生成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183806158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183818774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183829764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183841862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183852541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183911985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602183919521.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**示例代码如下**

**对图片的操作**
```java
package basic;

import java.awt.Rectangle;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Iterator;

import javax.imageio.ImageIO;
import javax.imageio.ImageReadParam;
import javax.imageio.ImageReader;
import javax.imageio.stream.ImageInputStream;

public class ImageTest {

	public static void main(String[] args) throws Exception {
		readAndWrite();
		readComparison();			
		cropImage("c:/temp/ecnu.jpg", "c:/temp/shida.jpg", 750, 250, 700, 300, "jpg", "jpg");
		combineImagesHorizontally("c:/temp/ecnu.jpg","c:/temp/ecnu.jpg","jpg", "c:/temp/ecnu2.jpg");
		combineImagesVertically("c:/temp/ecnu.jpg","c:/temp/ecnu.jpg","jpg", "c:/temp/ecnu3.jpg");
	}

	public static void readAndWrite() throws Exception {
		BufferedImage image = ImageIO.read(new File("c:/temp/ecnu.jpg"));
		System.out.println("Height: " + image.getHeight()); // 高度像素
		System.out.println("Width: " + image.getWidth()); // 宽度像素
		ImageIO.write(image, "png", new File("c:/temp/ecnu.png"));
	}

	public static void readComparison() throws Exception {
		System.out.println("===========加载速度测试==============");

		// ImageIO需要测试图片的类型，加载合适的ImageReader来读取图片，耗时更长
		long startTime = System.nanoTime();
		BufferedImage image = ImageIO.read(new File("c:/temp/ecnu.jpg"));
		System.out.println("Height: " + image.getHeight()); // 高度像素
		System.out.println("Width: " + image.getWidth()); // 宽度像素
		long endTime = System.nanoTime();
		System.out.println((endTime - startTime) / 1000000.0 + "毫秒");

		// 指定用jpg Reader来加载，速度会加快
		startTime = System.nanoTime();
		Iterator<ImageReader> readers = ImageIO.getImageReadersByFormatName("jpg");
		ImageReader reader = (ImageReader) readers.next();
		System.out.println(reader.getClass().getName());
		ImageInputStream iis = ImageIO.createImageInputStream(new File("c:/temp/ecnu.jpg"));
		reader.setInput(iis, true);
		System.out.println("Height:" + reader.getHeight(0));
		System.out.println("Width:" + reader.getWidth(0));
		endTime = System.nanoTime();
		System.out.println((endTime - startTime) / 1000000.0 + "毫秒");
	}

	/**
	 * cropImage 将原始图片文件切割一个矩形，并输出到目标图片文件
	 * @param fromPath 原始图片
	 * @param toPath  目标图片
	 * @param x       坐标起点x
	 * @param y       坐标起点y
	 * @param width   矩形宽度
	 * @param height  矩形高度
	 * @param readImageFormat  原始文件格式
	 * @param writeImageFormat 目标文件格式
	 * @throws Exception
	 */
	public static void cropImage(String fromPath, String toPath, int x, int y, int width, int height, String readImageFormat,
			String writeImageFormat) throws Exception {
		FileInputStream fis = null;
		ImageInputStream iis = null;
		try {
			// 读取原始图片文件
			fis = new FileInputStream(fromPath);
			Iterator<ImageReader> it = ImageIO.getImageReadersByFormatName(readImageFormat);
			ImageReader reader = it.next();			
			iis = ImageIO.createImageInputStream(fis);
			reader.setInput(iis, true);
			
			// 定义一个矩形 并放入切割参数中
			ImageReadParam param = reader.getDefaultReadParam();			
			Rectangle rect = new Rectangle(x, y, width, height);			
			param.setSourceRegion(rect);
			
			//从源文件读取一个矩形大小的图像
			BufferedImage bi = reader.read(0, param);
			
			//写入到目标文件
			ImageIO.write(bi, writeImageFormat, new File(toPath));
		} finally {
			fis.close();
			iis.close();
		}
	}

	/**
     * 横向拼接两张图片，并写入到目标文件
     * 拼接的本质，就是申请一个大的新空间，然后将原始的图片像素点拷贝到新空间，最后保存
     * @param firstPath 第一张图片的路径
     * @param secondPath    第二张图片的路径
     * @param imageFormat   拼接生成图片的格式
     * @param toPath    目标图片的路径
     */
    public static void combineImagesHorizontally(String firstPath, String secondPath,String imageFormat, String toPath){  
        try {  
            //读取第一张图片    
            File  first  =  new  File(firstPath);    
            BufferedImage  imageOne = ImageIO.read(first);    
            int  width1  =  imageOne.getWidth();//图片宽度    
            int  height1  =  imageOne.getHeight();//图片高度    
            //从第一张图片中读取RGB    
            int[]  firstRGB  =  new  int[width1*height1];    
            firstRGB  =  imageOne.getRGB(0,0,width1,height1,firstRGB,0,width1);    

            //对第二张图片做同样的处理    
            File  second  =  new  File(secondPath);    
            BufferedImage  imageTwo  =  ImageIO.read(second); 
            int width2 = imageTwo.getWidth();
            int height2 = imageTwo.getHeight();
            int[]   secondRGB  =  new  int[width2*height2];    
            secondRGB  =  imageTwo.getRGB(0,0,width2,height2,secondRGB,0,width2);   
            

            //生成新图片
            int height3 = (height1>height2)?height1:height2; //挑选高度大的，作为目标文件的高度
            int width3  = width1 + width2;                   //宽度，两张图片相加
            BufferedImage  imageNew  =  new  BufferedImage(width3,height3,BufferedImage.TYPE_INT_RGB);    
            
            //设置左半部分的RGB 从(0,0) 开始 
            imageNew.setRGB(0,0,width1,height1,firstRGB,0,width1); 
            //设置右半部分的RGB 从(width1, 0) 开始 
            imageNew.setRGB(width1,0,width2,height2,secondRGB,0,width2);
               
            //保存图片
            ImageIO.write(imageNew,  imageFormat,  new  File(toPath));
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
    }

    /**
     * 纵向拼接图片（两张）
     * 拼接的本质，就是申请一个大的新空间，然后将原始的图片像素点拷贝到新空间，最后保存
     * @param firstPath 读取的第一张图片
     * @param secondPath    读取的第二张图片
     * @param imageFormat 图片写入格式
     * @param toPath    图片写入路径
     */
    public static void combineImagesVertically(String firstPath, String secondPath,String imageFormat, String toPath){  
        try {  
            //读取第一张图片    
            File  first  =  new  File(firstPath);    
            BufferedImage  imageOne = ImageIO.read(first);    
            int  width1  =  imageOne.getWidth();//图片宽度    
            int  height1  =  imageOne.getHeight();//图片高度    
            //从图片中读取RGB    
            int[]  firstRGB  =  new  int[width1*height1];    
            firstRGB  =  imageOne.getRGB(0,0,width1,height1,firstRGB,0,width1);    

            //对第二张图片做相同的处理    
            File  second  =  new  File(secondPath);    
            BufferedImage  imageTwo  =  ImageIO.read(second); 
            int width2 = imageTwo.getWidth();
            int height2 = imageTwo.getHeight();
            int[]   secondRGB  =  new  int[width2*height2];    
            secondRGB  =  imageTwo.getRGB(0,0,width2,height2,secondRGB,0,width2); 

            //生成新图片
            int width3 = (width1>width2)?width1:width2; //挑选宽度大的，作为目标文件的宽度
            int height3 = height1+height2;              //高度，两张图片相加
            BufferedImage  imageNew  =  new  BufferedImage(width3,height3,BufferedImage.TYPE_INT_RGB);    
            //设置上半部分的RGB 从(0,0) 开始 
            imageNew.setRGB(0,0,width1,height1,firstRGB,0,width1);
            //设置下半部分的RGB 从(0, height1) 开始 
            imageNew.setRGB(0,height1,width2,height2,secondRGB,0,width2);  

            //保存图片
            ImageIO.write(imageNew, imageFormat, new File(toPath));
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
    }
}

```
**验证码制作**
```java

package code;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics2D;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.Random;

import javax.imageio.ImageIO;



public class ValidateCodeTest {

	//没有1 I L 0 o
	static char[] codeSequence = { 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'M', 'N', 'P', 'Q', 'R', 'S', 'T',
			'U', 'V', 'W', 'X', 'Y', 'Z', '2', '3', '4', '5', '6', '7', '8', '9' };
	static int charNum = codeSequence.length;
	
	public static void main(String[] a) throws IOException
	{
		generateCode("c:/temp/code.jpg");
	}	
	

	public static void generateCode(String filePath) throws IOException {
		// 首先定义验证码图片框  
		int width = 80; // 验证码图片的宽度
		int height = 32; // 验证码图片的高度
        BufferedImage buffImg = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB); 
        
        
        //定义图片上的图形和干扰线
        Graphics2D gd = buffImg.createGraphics();   
        gd.setColor(Color.LIGHT_GRAY);   // 将图像填充为浅灰色   
        gd.fillRect(0, 0, width, height);   
        gd.setColor(Color.BLACK);        // 画边框。   
        gd.drawRect(0, 0, width - 1, height - 1);   
        // 随机产生16条灰色干扰线，使图像中的认证码不易识别  
        gd.setColor(Color.gray); 
        // 创建一个随机数生成器类   用于随机产生干扰线
        Random random = new Random();   
        for (int i = 0; i < 16; i++) {   
            int x = random.nextInt(width);   
            int y = random.nextInt(height);   
            int xl = random.nextInt(12);   
            int yl = random.nextInt(12);   
            gd.drawLine(x, y, x + xl, y + yl);   
        }   
        
        
        //计算字的位置坐标
        int codeCount = 4; // 字符个数
    	int fontHeight; // 字体高度
    	int codeX; // 第一个字符的x坐标，因为后面的字符坐标依次递增，所以它们的x轴值是codeX的倍数
    	int codeY; // 验证字符的y坐标，因为并排所以值一样
    	// width-4 除去左右多余的位置，使验证码更加集中显示，减得越多越集中。
    	// codeCount+1 //等比分配显示的宽度，包括左右两边的空格
    	codeX = (width - 4) / (codeCount + 1); //第一个字母的起始位置    	
    	fontHeight = height - 10;  // height - 10 高度中间区域显示验证码
    	codeY = height - 7;
    			
    			
        // 创建字体，字体的大小应该根据图片的高度来定。   
        Font font = new Font("Fixedsys", Font.PLAIN, fontHeight);           
        gd.setFont(font);   
        
        // 随机产生codeCount数字的验证码。   
        for (int i = 0; i < codeCount; i++) {   
            // 每次随机拿一个字母，赋予随机的颜色  
            String strRand = String.valueOf(codeSequence[random.nextInt(charNum)]);   
            int red = random.nextInt(255);   
            int green = random.nextInt(255);   
            int blue = random.nextInt(255);   
            gd.setColor(new Color(red,green,blue));   
            //把字放到图片上!!!
            gd.drawString(strRand, (i + 1) * codeX, codeY);              
        }   
        
        ImageIO.write(buffImg, "jpg", new File(filePath));             
	}
}

```
**生成图表的操作**
```java
package charts;

import java.awt.Font;
import java.io.File;
import java.io.IOException;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.ChartUtilities;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.StandardChartTheme;
import org.jfree.chart.plot.PlotOrientation;
import org.jfree.data.category.DefaultCategoryDataset;
import org.jfree.data.general.DefaultPieDataset;

public class JFreeChartTest {

	public static void main(String[] args) {
		writeBar("c:/temp/bar.jpg"); // 柱状图
		writePie("c:/temp/pie.jpg"); // 饼图
		writeLine("c:/temp/line.jpg");// 折线图
	}
	
	public static StandardChartTheme getChineseTheme()
	{
		StandardChartTheme chineseTheme = new StandardChartTheme("CN");
		chineseTheme.setExtraLargeFont(new Font("隶书", Font.BOLD, 20));
		chineseTheme.setRegularFont(new Font("宋书", Font.PLAIN, 15));
		chineseTheme.setLargeFont(new Font("宋书", Font.PLAIN, 15));
		return chineseTheme;
	}
	
	public static void writeBar(String fileName) {
		DefaultCategoryDataset dataset = new DefaultCategoryDataset();
		dataset.addValue(11, "", "第一季度");
		dataset.addValue(41, "", "第二季度");
		dataset.addValue(51, "", "第三季度");
		dataset.addValue(4, "", "第四季度");

		// PlotOrientation.HORIZONTAL横向 PlotOrientation.VERTICAL 竖向
		// 引入中文主题样式
		ChartFactory.setChartTheme(getChineseTheme());
		JFreeChart chart = ChartFactory.createBarChart3D("柱状图", "2018年", "产品总量", dataset, PlotOrientation.VERTICAL,
				false, false, false);

		try {
			ChartUtilities.saveChartAsJPEG(new File(fileName), chart, 600, 300);
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

	public static void writePie(String fileName) {
		DefaultPieDataset pds = new DefaultPieDataset();
		pds.setValue("C人数", 100);
		pds.setValue("C++人数", 200);
		pds.setValue("Java人数", 300);
		try {
			ChartFactory.setChartTheme(getChineseTheme());
			JFreeChart chart = ChartFactory.createPieChart("饼图", pds);
			
			ChartUtilities.saveChartAsJPEG(new File(fileName), chart, 600, 300);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	public static void writeLine(String fileName) {
		DefaultCategoryDataset lines = new DefaultCategoryDataset();
		//第一条线
		lines.addValue(100, "Java核心技术", "1月");
		lines.addValue(200, "Java核心技术", "2月");
		lines.addValue(400, "Java核心技术", "3月");
		lines.addValue(500, "Java核心技术", "4月");
		
		//第二条线
		lines.addValue(100, "Java核心技术(进阶)", "1月");
		lines.addValue(400, "Java核心技术(进阶)", "2月");
		lines.addValue(900, "Java核心技术(进阶)", "3月");
		try {
			ChartFactory.setChartTheme(getChineseTheme());
			JFreeChart chart = ChartFactory.createLineChart("折线图", "时间", "人数", lines);
			ChartUtilities.saveChartAsJPEG(new File(fileName), chart, 600, 300);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```
pom文件配置

```xml
<?xml version="1.0"?>

-<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">

<modelVersion>4.0.0</modelVersion>

<groupId>com.test</groupId>

<artifactId>MOOC16-04</artifactId>

<version>0.0.1-SNAPSHOT</version>


-<dependencies>


-<dependency>

<groupId>org.jfree</groupId>

<artifactId>jfreechart</artifactId>

<version>1.0.19</version>

</dependency>

</dependencies>

</project>
```
classpath

```xml
<classpath>
<classpathentry kind="src" output="target/classes" path="src/main/java">
<attributes>
<attribute name="optional" value="true"/>
<attribute name="maven.pomderived" value="true"/>
</attributes>
</classpathentry>
<classpathentry excluding="**" kind="src" output="target/classes" path="src/main/resources">
<attributes>
<attribute name="maven.pomderived" value="true"/>
</attributes>
</classpathentry>
<classpathentry kind="src" output="target/test-classes" path="src/test/java">
<attributes>
<attribute name="optional" value="true"/>
<attribute name="maven.pomderived" value="true"/>
<attribute name="test" value="true"/>
</attributes>
</classpathentry>
<classpathentry excluding="**" kind="src" output="target/test-classes" path="src/test/resources">
<attributes>
<attribute name="maven.pomderived" value="true"/>
<attribute name="test" value="true"/>
</attributes>
</classpathentry>
<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/J2SE-1.5">
<attributes>
<attribute name="maven.pomderived" value="true"/>
</attributes>
</classpathentry>
<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER">
<attributes>
<attribute name="maven.pomderived" value="true"/>
</attributes>
</classpathentry>
<classpathentry kind="output" path="target/classes"/>
</classpath>
```

```xml
<projectDescription>
<name>MOOC16-04</name>
<comment/>
<projects> </projects>
<buildSpec>
<buildCommand>
<name>org.eclipse.jdt.core.javabuilder</name>
<arguments> </arguments>
</buildCommand>
<buildCommand>
<name>org.eclipse.m2e.core.maven2Builder</name>
<arguments> </arguments>
</buildCommand>
</buildSpec>
<natures>
<nature>org.eclipse.jdt.core.javanature</nature>
<nature>org.eclipse.m2e.core.maven2Nature</nature>
</natures>
</projectDescription>
```
## 条形码和二维码
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060218544471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602185453999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**代码示例**
**二维码**

```javascript
package zxing;

import com.google.zxing.*;
import com.google.zxing.client.j2se.BufferedImageLuminanceSource;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.common.HybridBinarizer;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.nio.file.Path;
import java.util.HashMap;
import java.util.Map;

public class QRCodeTest {
    /*
     * 定义二维码的宽高
     */
    private static int WIDTH = 300;
    private static int HEIGHT = 300;
    private static String FORMAT = "png";//二维码格式

    //生成二维码
    public static void generateQRCode(File file, String content) {
        //定义二维码参数
        Map<EncodeHintType, Object> hints = new HashMap<>();

        hints.put(EncodeHintType.CHARACTER_SET, "utf-8");//设置编码
        hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M);//设置容错等级
        hints.put(EncodeHintType.MARGIN, 2);//设置边距默认是5

        try {
            BitMatrix bitMatrix = new MultiFormatWriter().encode(content, BarcodeFormat.QR_CODE, WIDTH, HEIGHT, hints);
            Path path = file.toPath();
            MatrixToImageWriter.writeToPath(bitMatrix, FORMAT, path);//写到指定路径下

        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    //读取二维码
    public static void readQrCode(File file) {
        MultiFormatReader reader = new MultiFormatReader();
        try {
            BufferedImage image = ImageIO.read(file);
            BinaryBitmap binaryBitmap = new BinaryBitmap(new HybridBinarizer(new BufferedImageLuminanceSource(image)));
            Map<DecodeHintType, Object> hints = new HashMap<>();
            hints.put(DecodeHintType.CHARACTER_SET, "utf-8");//设置编码
            Result result = reader.decode(binaryBitmap, hints);
            System.out.println("解析结果:" + result.toString());
            System.out.println("二维码格式:" + result.getBarcodeFormat());
            System.out.println("二维码文本内容:" + result.getText());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        generateQRCode(new File("2dcode.png"), "https://www.baidu.com");
    	readQrCode(new File("2dcode.png"));
    	//readQrCode(new File("2dcode.jpg"));
    }
}

```

**一维码**
```java
package zxing;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.BinaryBitmap;
import com.google.zxing.DecodeHintType;
import com.google.zxing.EncodeHintType;
import com.google.zxing.LuminanceSource;
import com.google.zxing.MultiFormatReader;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.Result;
import com.google.zxing.WriterException;
import com.google.zxing.client.j2se.BufferedImageLuminanceSource;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.common.HybridBinarizer;

import javax.imageio.ImageIO;

import java.awt.image.BufferedImage;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.util.HashMap;
import java.util.Map;

public class BarCodeTest {
	/**
	 * generateCode 根据code生成相应的一维码
	 * @param file 一维码目标文件
	 * @param code 一维码内容
	 * @param width 图片宽度
	 * @param height 图片高度
	 */
    public static void generateCode(File file, String code, int width, int height) {
    	//定义位图矩阵BitMatrix
        BitMatrix matrix = null;
        try {
            // 使用code_128格式进行编码生成100*25的条形码
        	MultiFormatWriter writer = new MultiFormatWriter();
        	
            matrix = writer.encode(code,BarcodeFormat.CODE_128, width, height, null);
            //matrix = writer.encode(code,BarcodeFormat.EAN_13, width, height, null);
        } catch (WriterException e) {
            e.printStackTrace();
        }
        
        //将位图矩阵BitMatrix保存为图片
        try (FileOutputStream outStream = new FileOutputStream(file)) {
            ImageIO.write(MatrixToImageWriter.toBufferedImage(matrix), "png",
                    outStream);
            outStream.flush();
        } catch (Exception e) {
        	e.printStackTrace();
        }
    }
    
    /**
     * readCode 读取一张一维码图片
     * @param file 一维码图片名字
     */
    public static void readCode(File file){
        try {
        	BufferedImage image = ImageIO.read(file);
            if (image == null) {
                return;
            }
            LuminanceSource source = new BufferedImageLuminanceSource(image);
            BinaryBitmap bitmap = new BinaryBitmap(new HybridBinarizer(source));
            
            Map<DecodeHintType, Object> hints = new HashMap<>();
            hints.put(DecodeHintType.CHARACTER_SET, "GBK");
            hints.put(DecodeHintType.PURE_BARCODE, Boolean.TRUE);
            hints.put(DecodeHintType.TRY_HARDER, Boolean.TRUE);
            
            Result result = new MultiFormatReader().decode(bitmap, hints);
            System.out.println("条形码内容: "+result.getText());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws Exception {
        //generateCode(new File("1dcode.png"), "123456789012", 500, 250);
    	readCode(new File("1dcode.png"));
    }
}

```
**barcode4j逐渐没落，在这里就不写了**

```xml
<?xml version="1.0"?>

-<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">

<modelVersion>4.0.0</modelVersion>

<groupId>com.test</groupId>

<artifactId>MOOC16-05</artifactId>

<version>0.0.1-SNAPSHOT</version>


-<dependencies>

<!-- https://mvnrepository.com/artifact/com.google.zxing/core -->



-<dependency>

<groupId>com.google.zxing</groupId>

<artifactId>core</artifactId>

<version>3.3.3</version>

</dependency>

<!-- https://mvnrepository.com/artifact/com.google.zxing/javase -->



-<dependency>

<groupId>com.google.zxing</groupId>

<artifactId>javase</artifactId>

<version>3.3.3</version>

</dependency>

<!-- https://mvnrepository.com/artifact/net.sf.barcode4j/barcode4j -->



-<dependency>

<groupId>net.sf.barcode4j</groupId>

<artifactId>barcode4j</artifactId>

<version>2.1</version>

</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.avalon.framework/avalon-framework-api -->



-<dependency>

<groupId>org.apache.avalon.framework</groupId>

<artifactId>avalon-framework-api</artifactId>

<version>4.3.1</version>

</dependency>

</dependencies>


-<properties>

<maven.compiler.source>1.8</maven.compiler.source>

<maven.compiler.target>1.8</maven.compiler.target>

<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>

</properties>

</project>
```

## docx
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602191136353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602191722955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602191738294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602191751368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**读取文本文档**

```java
package docx;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;

import javax.xml.namespace.QName;

import org.apache.poi.POIXMLDocumentPart;
import org.apache.poi.poifs.filesystem.POIFSFileSystem;
import org.apache.poi.util.IOUtils;
import org.apache.poi.xwpf.usermodel.BodyElementType;
import org.apache.poi.xwpf.usermodel.Document;
import org.apache.poi.xwpf.usermodel.IBodyElement;
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFParagraph;
import org.apache.poi.xwpf.usermodel.XWPFPicture;
import org.apache.poi.xwpf.usermodel.XWPFRun;
import org.apache.xmlbeans.XmlCursor;
import org.apache.xmlbeans.XmlObject;
import org.openxmlformats.schemas.wordprocessingml.x2006.main.CTObject;

public class TextRead {

	public static void main(String[] args) throws Exception {
		readDocx();
	}
	
	public static void readDocx() throws Exception {
		InputStream is;
		is = new FileInputStream("test.docx");
		XWPFDocument xwpf = new XWPFDocument(is);
		
		List<IBodyElement> ibs= xwpf.getBodyElements();
		for(IBodyElement ib:ibs)
    	{
			BodyElementType bet = ib.getElementType();
			if(bet== BodyElementType.TABLE)
    		{
				//表格
				System.out.println("table" + ib.getPart());
    		}
			else
			{				
				//段落
				XWPFParagraph para = (XWPFParagraph) ib;
				System.out.println("It is a new paragraph....The indention is " 
				         + para.getFirstLineIndent() + "," + para.getIndentationFirstLine() );
				//System.out.println(para.getCTP().xmlText());
				
				List<XWPFRun> res = para.getRuns();
				//System.out.println("run");
				if(res.size()<=0)
				{
					System.out.println("empty line");
				}
				for(XWPFRun re: res)
				{							
					if(null == re.text()||re.text().length()<=0)
					{
						if(re.getEmbeddedPictures().size()>0)
						{
							System.out.println("image***" + re.getEmbeddedPictures().size());							
						} else
						{
							System.out.println("objects:" + re.getCTR().getObjectList().size());
							if(re.getCTR().xmlText().indexOf("instrText") > 0) {
								System.out.println("there is an equation field");
							}
							else
							{
								//System.out.println(re.getCTR().xmlText());
							}												
						}
					}
					else
					{						
						System.out.println("==="+ re.getCharacterSpacing() + re.text());
					}
				}				
			}
    	}		
		is.close();
	}	
}

```
**读取图片**

```java
package docx;

/**
 * 本类 完成docx的图片读取工作
 */
import org.apache.poi.openxml4j.exceptions.InvalidFormatException;
import org.apache.poi.openxml4j.opc.OPCPackage;
import org.apache.poi.xwpf.usermodel.*;

import java.awt.image.BufferedImage;
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Iterator;

import javax.imageio.ImageIO;

public class ImageRead {


    public static void imageRead() throws IOException, InvalidFormatException {
        File docFile = new File("simple.docx");

        XWPFDocument doc = new XWPFDocument(OPCPackage.openOrCreate(docFile));
        
        int i = 0;
        for (XWPFParagraph p : doc.getParagraphs()) {
            for (XWPFRun run : p.getRuns()) {
            	System.out.println("a new run");
                for (XWPFPicture pic : run.getEmbeddedPictures()) {
                	System.out.println(pic.getCTPicture().xmlText());
                	//image EMU(English Metric Unit)
                	System.out.println(pic.getCTPicture().getSpPr().getXfrm().getExt().getCx());
                	System.out.println(pic.getCTPicture().getSpPr().getXfrm().getExt().getCy());
                	//image 显示大小  以厘米为单位
                	System.out.println(pic.getCTPicture().getSpPr().getXfrm().getExt().getCx()/360000.0);
                	System.out.println(pic.getCTPicture().getSpPr().getXfrm().getExt().getCy()/360000.0);
                	int type = pic.getPictureData().getPictureType();
                    byte [] img = pic.getPictureData().getData();
                    BufferedImage bufferedImage= ImageRead.byteArrayToImage(img);
                    System.out.println(bufferedImage.getWidth());
                    System.out.println(bufferedImage.getHeight());
                    String extension = "";
                    switch(type)
                    {
	                    case Document.PICTURE_TYPE_EMF: extension = ".emf";
	                    								break;
	                    case Document.PICTURE_TYPE_WMF: extension = ".wmf";
							break;
	                    case Document.PICTURE_TYPE_PICT: extension = ".pic";
							break;
	                    case Document.PICTURE_TYPE_PNG: extension = ".png";
							break;
	                    case Document.PICTURE_TYPE_DIB: extension = ".dib";
							break;
							default: extension = ".jpg";
                    }
                    //outputFile = new File ( );                
                    //BufferedImage image = ImageIO.read(new File(img));
                    //ImageIO.write(image , "png", outputfile);
                    FileOutputStream fos = new FileOutputStream("test" + i + extension);
                    fos.write(img);
                    fos.close();
                    i++;
                }
            }
        }
    }
    public static BufferedImage  byteArrayToImage(byte[] bytes){  
        BufferedImage bufferedImage=null;
        try {
            InputStream inputStream = new ByteArrayInputStream(bytes);
            bufferedImage = ImageIO.read(inputStream);
        } catch (IOException ex) {
            System.out.println(ex.getMessage());
        }
        return bufferedImage;
}
    public static void main(String[] args) throws Exception {
    	imageRead();
    }
}
```
**docx文件保存**

```java
package docx;

/**
 * 本类完成docx的图片保存工作
 */
import org.apache.poi.util.Units;
import org.apache.poi.xwpf.usermodel.*;

import java.io.FileInputStream;
import java.io.FileOutputStream;


public class ImageWrite {

    public static void main(String[] args) throws Exception {
        XWPFDocument doc = new XWPFDocument();
        XWPFParagraph p = doc.createParagraph();

        XWPFRun r = p.createRun();
        
        String[] imgFiles = new String[2];
        
        imgFiles[0] = "c:/temp/ecnu.jpg";
        imgFiles[1] = "c:/temp/shida.jpg";        		

        for(String imgFile : imgFiles) {
            int format;

            if(imgFile.endsWith(".emf")) format = XWPFDocument.PICTURE_TYPE_EMF;
            else if(imgFile.endsWith(".wmf")) format = XWPFDocument.PICTURE_TYPE_WMF;
            else if(imgFile.endsWith(".pict")) format = XWPFDocument.PICTURE_TYPE_PICT;
            else if(imgFile.endsWith(".jpeg") || imgFile.endsWith(".jpg")) format = XWPFDocument.PICTURE_TYPE_JPEG;
            else if(imgFile.endsWith(".png")) format = XWPFDocument.PICTURE_TYPE_PNG;
            else if(imgFile.endsWith(".dib")) format = XWPFDocument.PICTURE_TYPE_DIB;
            else if(imgFile.endsWith(".gif")) format = XWPFDocument.PICTURE_TYPE_GIF;
            else if(imgFile.endsWith(".tiff")) format = XWPFDocument.PICTURE_TYPE_TIFF;
            else if(imgFile.endsWith(".eps")) format = XWPFDocument.PICTURE_TYPE_EPS;
            else if(imgFile.endsWith(".bmp")) format = XWPFDocument.PICTURE_TYPE_BMP;
            else if(imgFile.endsWith(".wpg")) format = XWPFDocument.PICTURE_TYPE_WPG;
            else {
                System.err.println("Unsupported picture: " + imgFile +
                        ". Expected emf|wmf|pict|jpeg|png|dib|gif|tiff|eps|bmp|wpg");
                continue;
            }

            r.setText(imgFile);
            r.addBreak();
            r.addPicture(new FileInputStream(imgFile), format, imgFile, Units.toEMU(200), Units.toEMU(200)); // 200x200 pixels
            r.addBreak(BreakType.PAGE);
        }

        FileOutputStream out = new FileOutputStream("images.docx");
        doc.write(out);
        out.close();
    }


}
```
**合并单元格**

```java
package docx;
/**
 * 本类完成（合并单元格）的表格
 */

import java.io.FileOutputStream;
import java.io.IOException;

import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFTable;
import org.apache.poi.xwpf.usermodel.XWPFTableCell;
import org.apache.poi.xwpf.usermodel.XWPFTableRow;
import org.openxmlformats.schemas.wordprocessingml.x2006.main.STMerge;


public class RowSpanTable {

    public static void main(String[] args) {

        int rows = 5;
        int cols = 5;

        XWPFDocument document = new XWPFDocument();
        XWPFTable table = document.createTable(rows, cols);

        fillTable(table);

        mergeCellsVertically(table, 3, 1, 3);

        try {
            FileOutputStream out = new FileOutputStream("c:/temp/rowSpanTable.docx");
            document.write(out);
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    private static void fillTable(XWPFTable table) {
        for (int rowIndex = 0; rowIndex < table.getNumberOfRows(); rowIndex++) {
            XWPFTableRow row = table.getRow(rowIndex);

            for (int colIndex = 0; colIndex < row.getTableCells().size(); colIndex++) {
                XWPFTableCell cell = row.getCell(colIndex);
                cell.addParagraph().createRun().setText(" cell " + rowIndex + colIndex + " ");
            }
        }
    }

    private static void mergeCellsVertically(XWPFTable table, int col, int fromRow, int toRow) {

        for (int rowIndex = fromRow; rowIndex <= toRow; rowIndex++) {

            XWPFTableCell cell = table.getRow(rowIndex).getCell(col);

            if ( rowIndex == fromRow ) {
                // The first merged cell is set with RESTART merge value
                cell.getCTTc().addNewTcPr().addNewVMerge().setVal(STMerge.RESTART);
            } else {
                // Cells which join (merge) the first one, are set with CONTINUE
                cell.getCTTc().addNewTcPr().addNewVMerge().setVal(STMerge.CONTINUE);
            }
        }
    }

}
```
**docx表格读取**

```java
package docx;

/**
 * 本类完成docx的表格内容读取
 */
import java.io.FileInputStream;
import java.io.InputStream;
import java.util.Iterator;
import java.util.List;

import org.apache.poi.poifs.filesystem.POIFSFileSystem;
import org.apache.poi.xwpf.usermodel.BodyElementType;
import org.apache.poi.xwpf.usermodel.IBodyElement;
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFParagraph;
import org.apache.poi.xwpf.usermodel.XWPFRun;
import org.apache.poi.xwpf.usermodel.XWPFTable;
import org.apache.poi.xwpf.usermodel.XWPFTableCell;
import org.apache.poi.xwpf.usermodel.XWPFTableRow;


public class TableRead {
	public static void main(String[] args) throws Exception {
		testTable();
	}
	
	public static void testTable() throws Exception {
		InputStream is = new FileInputStream("simple2.docx");
		XWPFDocument xwpf = new XWPFDocument(is);
		List<XWPFParagraph> paras = xwpf.getParagraphs();
		//List<POIXMLDocumentPart> pdps = xwpf.getRelations();
		
		List<IBodyElement> ibs= xwpf.getBodyElements();
		for(IBodyElement ib:ibs)
    	{
			BodyElementType bet = ib.getElementType();
			if(bet== BodyElementType.TABLE)
    		{
				//表格
				System.out.println("table" + ib.getPart());
				XWPFTable table = (XWPFTable) ib;
	    		List<XWPFTableRow> rows=table.getRows(); 
	    		 //读取每一行数据
	    		for (int i = 0; i < rows.size(); i++) {
	    			XWPFTableRow  row = rows.get(i);
	    			//读取每一列数据
		    	    List<XWPFTableCell> cells = row.getTableCells(); 
		    	    for (int j = 0; j < cells.size(); j++) {
		    	    	XWPFTableCell cell=cells.get(j);
		    	    	System.out.println(cell.getText());
		    	    	List<XWPFParagraph> cps = cell.getParagraphs();
		    	    	System.out.println(cps.size());
					}
	    		}
    		}
			else
			{				
				//段落
				XWPFParagraph para = (XWPFParagraph) ib;
				System.out.println("It is a new paragraph....The indention is " 
				   + para.getFirstLineIndent() + "," + para.getIndentationFirstLine() + "," 
				   + para.getIndentationHanging()+"," + para.getIndentationLeft() + "," 
				   + para.getIndentationRight() + "," + para.getIndentFromLeft() + ","
				   + para.getIndentFromRight()+"," + para.getAlignment().getValue());
				
				//System.out.println(para.getAlignment());
				//System.out.println(para.getRuns().size());

				List<XWPFRun> res = para.getRuns();
				System.out.println("run");
				if(res.size()<=0)
				{
					System.out.println("empty line");
				}
				for(XWPFRun re: res)
				{					
					if(null == re.text()||re.text().length()<=0)
					{
						if(re.getEmbeddedPictures().size()>0)
						{
							System.out.println("image***" + re.getEmbeddedPictures().size());
							
						}
						else
						{
							System.out.println("objects:" + re.getCTR().getObjectList().size());
							System.out.println(re.getCTR().xmlText());
												
						}
					}
					else
					{
						System.out.println("===" + re.text());
					}
				}
				
			}
    	}		
		is.close();
	}
}

```
**写入表格**

```java
package docx;

/*
 * 本类测试写入表格
 */

import java.io.FileOutputStream;
import java.io.OutputStream;
import java.math.BigInteger;
import java.util.List;

import org.apache.poi.xwpf.usermodel.*;
import org.openxmlformats.schemas.wordprocessingml.x2006.main.*;

public class TableWrite {

 public static void main(String[] args) throws Exception {
 	try {
 		createSimpleTable();
 	}
 	catch(Exception e) {
 		System.out.println("Error trying to create simple table.");
 		throw(e);
 	} 	
 }

 public static void createSimpleTable() throws Exception {
     XWPFDocument doc = new XWPFDocument();

     try {
         XWPFTable table = doc.createTable(3, 3);

         table.getRow(1).getCell(1).setText("表格示例");

         XWPFParagraph p1 = table.getRow(0).getCell(0).getParagraphs().get(0);

         XWPFRun r1 = p1.createRun();
         r1.setBold(true);
         r1.setText("The quick brown fox");
         r1.setItalic(true);
         r1.setFontFamily("Courier");
         r1.setUnderline(UnderlinePatterns.DOT_DOT_DASH);
         r1.setTextPosition(100);

         table.getRow(2).getCell(2).setText("only text");

         OutputStream out = new FileOutputStream("simpleTable.docx");
         try {
             doc.write(out);
         } finally {
             out.close();
         }
     } finally {
         doc.close();
     }
 } 
}
```
**模板文件操作**

```java
package docx;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URL;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import org.apache.poi.openxml4j.exceptions.InvalidFormatException;
import org.apache.poi.util.Units;
import org.apache.poi.xwpf.usermodel.BodyElementType;
import org.apache.poi.xwpf.usermodel.Document;
import org.apache.poi.xwpf.usermodel.IBodyElement;
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFParagraph;
import org.apache.poi.xwpf.usermodel.XWPFRun;
import org.apache.poi.xwpf.usermodel.XWPFTable;
import org.apache.poi.xwpf.usermodel.XWPFTableCell;
import org.apache.poi.xwpf.usermodel.XWPFTableRow;


public class TemplateTest {

	public static void main(String[] args) throws Exception {
		XWPFDocument doc = openDocx("template.docx");//导入模板文件
		Map<String, Object> params = new HashMap<>();//文字类 key-value
		params.put("${name}", "Tom");
		params.put("${sex}", "男");
		Map<String,String> picParams = new HashMap<>();//图片类 key-url
		picParams.put("${pic}", "c:/temp/ecnu.jpg");
		List<IBodyElement> ibes = doc.getBodyElements();
		for (IBodyElement ib : ibes) {
			if (ib.getElementType() == BodyElementType.TABLE) {
				replaceTable(ib, params, picParams, doc);
			}
		}
		writeDocx(doc, new FileOutputStream("template2.docx"));//输出
	}
	
	public static XWPFDocument openDocx(String url) {
		InputStream in = null;
		try {
			in = new FileInputStream(url);
			XWPFDocument doc = new XWPFDocument(in);
			return doc;
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if (in != null) {
				try {
					in.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
		return null;
	}
	
	public static void writeDocx(XWPFDocument outDoc, OutputStream out) {
		try {
			outDoc.write(out);
			out.flush();
			if (out != null) {
				try {
					out.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	private static Matcher matcher(String str) {
		Pattern pattern = Pattern.compile("\\$\\{(.+?)\\}", Pattern.CASE_INSENSITIVE);
		Matcher matcher = pattern.matcher(str);
		return matcher;
	}
	
	/**
	 * 写入image
	 * @param run
	 * @param imgFile
	 * @param doc
	 * @throws InvalidFormatException
	 * @throws FileNotFoundException
	 * @throws IOException
	 */
	public static void replacePic(XWPFRun run,  String imgFile,  XWPFDocument doc) throws Exception {
		int format;
		if (imgFile.endsWith(".emf"))
			format = Document.PICTURE_TYPE_EMF;
		else if (imgFile.endsWith(".wmf"))
			format = Document.PICTURE_TYPE_WMF;
		else if (imgFile.endsWith(".pict"))
			format = Document.PICTURE_TYPE_PICT;
		else if (imgFile.endsWith(".jpeg") || imgFile.endsWith(".jpg"))
			format = Document.PICTURE_TYPE_JPEG;
		else if (imgFile.endsWith(".png"))
			format = Document.PICTURE_TYPE_PNG;
		else if (imgFile.endsWith(".dib"))
			format = Document.PICTURE_TYPE_DIB;
		else if (imgFile.endsWith(".gif"))
			format = Document.PICTURE_TYPE_GIF;
		else if (imgFile.endsWith(".tiff"))
			format = Document.PICTURE_TYPE_TIFF;
		else if (imgFile.endsWith(".eps"))
			format = Document.PICTURE_TYPE_EPS;
		else if (imgFile.endsWith(".bmp"))
			format = Document.PICTURE_TYPE_BMP;
		else if (imgFile.endsWith(".wpg"))
			format = Document.PICTURE_TYPE_WPG;
		else {
			System.err.println(
					"Unsupported picture: " + imgFile + ". Expected emf|wmf|pict|jpeg|png|dib|gif|tiff|eps|bmp|wpg");
			return;
		}
		if(imgFile.startsWith("http")||imgFile.startsWith("https")){
			run.addPicture(new URL(imgFile).openConnection().getInputStream(), format, "rpic",Units.toEMU(100),Units.toEMU(100));
		}else{
			run.addPicture(new FileInputStream(imgFile), format, "rpic",Units.toEMU(100),Units.toEMU(100));
		}
	}
	
	/**
	 * 替换表格内占位符
	 * @param para 表格对象
	 * @param params 文字替换map
	 * @param picParams 图片替换map
	 * @param indoc
	 * @throws Exception 
	 */
	public static void replaceTable(IBodyElement para ,Map<String, Object> params, 
			Map<String, String> picParams, XWPFDocument indoc)
			throws Exception {
		Matcher matcher;
		XWPFTable table;
		List<XWPFTableRow> rows;
		List<XWPFTableCell> cells;
		table = (XWPFTable) para;
		rows = table.getRows();
		for (XWPFTableRow row : rows) {
			cells = row.getTableCells();
			int cellsize = cells.size();
			int cellcount = 0;
			for(cellcount = 0; cellcount<cellsize;cellcount++){
				XWPFTableCell cell = cells.get(cellcount);
				String runtext = "";
				List<XWPFParagraph> ps = cell.getParagraphs();
				for (XWPFParagraph p : ps) {
					for(XWPFRun run : p.getRuns()){
						runtext = run.text();
						matcher = matcher(runtext);
						if (matcher.find()) {
							if (picParams != null) {
								for (String pickey : picParams.keySet()) {
									if (matcher.group().equals(pickey)) {
										run.setText("",0);
										replacePic(run, picParams.get(pickey), indoc);
									}
								}
							}
							if (params != null) {
								for (String pickey : params.keySet()) {
									if (matcher.group().equals(pickey)) {
										run.setText(params.get(pickey)+"",0);
									}
								}
							}
						}
					}
				}
			}
		}
	}
}

```

```xml
<?xml version="1.0"?>

-<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">

<modelVersion>4.0.0</modelVersion>

<groupId>com.test</groupId>

<artifactId>MOOC16-06</artifactId>

<version>0.0.1-SNAPSHOT</version>


-<dependencies>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi</artifactId>

<version>3.14</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>ooxml-schemas</artifactId>

<version>1.3</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi-ooxml-schemas</artifactId>

<version>3.14</version>

</dependency>


-<dependency>

<groupId>org.apache.xmlbeans</groupId>

<artifactId>xmlbeans</artifactId>

<version>2.6.0</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi-examples</artifactId>

<version>3.14</version>

</dependency>

</dependencies>

</project>
```

## 表格
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603000950661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)

```java
package xlsx;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Iterator;

import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

public class ReadWriteExcelFile
{

  public static void readXLSFile() throws IOException
  {
    InputStream ExcelFileToRead = new FileInputStream("C:/Test.xls");
    HSSFWorkbook wb = new HSSFWorkbook(ExcelFileToRead);

    HSSFSheet sheet = wb.getSheetAt(0);
    HSSFRow row;
    HSSFCell cell;

    Iterator rows = sheet.rowIterator();

    while (rows.hasNext())
    {
      row = (HSSFRow) rows.next();
      Iterator cells = row.cellIterator();

      while (cells.hasNext())
      {
        cell = (HSSFCell) cells.next();

        if (cell.getCellType() == HSSFCell.CELL_TYPE_STRING)
        {
          System.out.print(cell.getStringCellValue() + " ");
        }
        else if (cell.getCellType() == HSSFCell.CELL_TYPE_NUMERIC)
        {
          System.out.print(cell.getNumericCellValue() + " ");
        }
        else
        {
          // U Can Handel Boolean, Formula, Errors
        }
      }
      System.out.println();
    }
  }

  public static void writeXLSFile() throws IOException
  {

    String excelFileName = "C:/Test.xls";// name of excel file

    String sheetName = "Sheet1";// name of sheet

    HSSFWorkbook wb = new HSSFWorkbook();
    HSSFSheet sheet = wb.createSheet(sheetName);

    // iterating r number of rows
    for (int r = 0; r < 5; r++)
    {
      HSSFRow row = sheet.createRow(r);

      // iterating c number of columns
      for (int c = 0; c < 5; c++)
      {
        HSSFCell cell = row.createCell(c);

        cell.setCellValue("Cell " + r + " " + c);
      }
    }

    FileOutputStream fileOut = new FileOutputStream(excelFileName);

    // write this workbook to an Outputstream.
    wb.write(fileOut);
    fileOut.flush();
    fileOut.close();
  }

  public static void readXLSXFile() throws IOException
  {
    InputStream ExcelFileToRead = new FileInputStream("Test.xlsx");
    XSSFWorkbook wb = new XSSFWorkbook(ExcelFileToRead);

    XSSFSheet sheet = wb.getSheetAt(0);
    XSSFRow row;
    XSSFCell cell;

    Iterator rows = sheet.rowIterator();

    while (rows.hasNext())
    {
      row = (XSSFRow) rows.next();
      Iterator cells = row.cellIterator();
      while (cells.hasNext())
      {
        cell = (XSSFCell) cells.next();

        if (cell.getCellType() == XSSFCell.CELL_TYPE_STRING)
        {
          System.out.print(cell.getStringCellValue() + " ");
        }
        else if (cell.getCellType() == XSSFCell.CELL_TYPE_NUMERIC)
        {
          System.out.print(cell.getNumericCellValue() + " ");
        }
        else
        {
          // U Can Handel Boolean, Formula, Errors
        }
      }
      System.out.println();
    }

  }

  public static void writeXLSXFile() throws IOException
  {

    String excelFileName = "Test.xlsx";// name of excel file

    String sheetName = "Sheet1";// name of sheet

    XSSFWorkbook wb = new XSSFWorkbook();
    XSSFSheet sheet = wb.createSheet(sheetName);

    // iterating r number of rows
    for (int r = 0; r < 5; r++)
    {
      XSSFRow row = sheet.createRow(r);

      // iterating c number of columns
      for (int c = 0; c < 5; c++)
      {
        XSSFCell cell = row.createCell(c);

        cell.setCellValue("Cell " + r + " " + c);
      }
    }

    FileOutputStream fileOut = new FileOutputStream(excelFileName);

    // write this workbook to an Outputstream.
    wb.write(fileOut);
    fileOut.flush();
    fileOut.close();
  }

  public static void main(String[] args) throws IOException
  {
    writeXLSFile();
    readXLSFile();

    writeXLSXFile();
    readXLSXFile();
  }
}
```

```java
package csv;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.Reader;
import java.time.LocalDate;

import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVPrinter;
import org.apache.commons.csv.CSVRecord;

public class CSVTest {

	public static void main(String[] args) throws Exception {
		readCSVWithIndex();
		System.out.println("===========华丽丽的分割线1===================");
		readCSVWithName();
		System.out.println("===========华丽丽的分割线2===================");
		writeCSV();
		System.out.println("write done");
	}
	
	public static void readCSVWithIndex() throws Exception {
		Reader in = new FileReader("c:/temp/score.csv");
		Iterable<CSVRecord> records = CSVFormat.EXCEL.parse(in);
		for (CSVRecord record : records) {
		    System.out.println(record.get(0)); //0 代表第一列
		}
	}
	
	public static void readCSVWithName() throws Exception {
		Reader in = new FileReader("c:/temp/score.csv");
		Iterable<CSVRecord> records = CSVFormat.RFC4180.withHeader("Name", "Subject", "Score").parse(in);
		for (CSVRecord record : records) {
		    System.out.println(record.get("Subject")); 
		}
	}
	
	public static void writeCSV() throws Exception {
		try (CSVPrinter printer = new CSVPrinter(new FileWriter("person.csv"), CSVFormat.EXCEL)) {
		     printer.printRecord("id", "userName", "firstName", "lastName", "birthday");
		     printer.printRecord(1, "john73", "John", "Doe", LocalDate.of(1973, 9, 15));
		     printer.println();  //空白行
		     printer.printRecord(2, "mary", "Mary", "Meyer", LocalDate.of(1985, 3, 29));
		 } catch (IOException ex) {
		     ex.printStackTrace();
		 }
	}
}

```

```xml
<?xml version="1.0"?>

-<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">

<modelVersion>4.0.0</modelVersion>

<groupId>com.test</groupId>

<artifactId>MOOC16-07</artifactId>

<version>0.0.1-SNAPSHOT</version>


-<dependencies>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi</artifactId>

<version>3.14</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>ooxml-schemas</artifactId>

<version>1.3</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi-ooxml-schemas</artifactId>

<version>3.14</version>

</dependency>


-<dependency>

<groupId>org.apache.xmlbeans</groupId>

<artifactId>xmlbeans</artifactId>

<version>2.6.0</version>

</dependency>


-<dependency>

<groupId>org.apache.poi</groupId>

<artifactId>poi-examples</artifactId>

<version>3.14</version>

</dependency>


-<dependency>

<groupId>org.apache.commons</groupId>

<artifactId>commons-csv</artifactId>

<version>1.6</version>

</dependency>

</dependencies>

</project>
```
## pdf
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060300152490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060300154443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603001554403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3MjFzanJj,size_16,color_FFFFFF,t_70)
**抽取pdf文本**
```java
package pdfbox;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

import org.apache.pdfbox.io.RandomAccessBuffer;
import org.apache.pdfbox.pdfparser.PDFParser;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.encryption.AccessPermission;
import org.apache.pdfbox.text.PDFTextStripper;

/**
 * PdfReader 抽取pdf的文本
 * @author Tom
 *
 */
public class PdfReader {

    public static void main(String[] args){

        File pdfFile = new File("simple.pdf");
        PDDocument document = null;
        try
        {
            document=PDDocument.load(pdfFile);
            
            AccessPermission ap = document.getCurrentAccessPermission();
            if (!ap.canExtractContent())
            {
                throw new IOException("你没有权限抽取文本");
            }
            // 获取页码
            int pages = document.getNumberOfPages();

            // 读文本内容
            PDFTextStripper stripper=new PDFTextStripper();
            // 设置按顺序输出
            stripper.setSortByPosition(true);
            stripper.setStartPage(1);  //起始页
            stripper.setEndPage(pages);//结束页
            String content = stripper.getText(document);
            System.out.println(content);     
        }
        catch(Exception e)
        {
            System.out.println(e);
        }
    }
}
```
**写入pdf文本**
```java
package pdfbox;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.font.PDFont;
import org.apache.pdfbox.pdmodel.font.PDType1Font;

public class PdfWriter {

	public static void main(String[] args) {
		createHelloPDF();
	}
	public static void createHelloPDF() {
        PDDocument doc = null;
        PDPage page = null;

        try {
            doc = new PDDocument();
            page = new PDPage();
            doc.addPage(page);
            PDFont font = PDType1Font.HELVETICA_BOLD;
            PDPageContentStream content = new PDPageContentStream(doc, page);
            content.beginText();
            content.setFont(font, 12);
            content.moveTextPositionByAmount(100, 700);
            content.showText("hello world");
            

            content.endText();
            content.close();
            doc.save("test.pdf");
            doc.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}

```
**合并pdf**
```java
/*
 * Copyright 2016 The Apache Software Foundation.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
package pdfbox;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import org.apache.pdfbox.cos.COSStream;
import org.apache.pdfbox.io.MemoryUsageSetting;
import org.apache.pdfbox.multipdf.PDFMergerUtility;
import org.apache.pdfbox.pdmodel.PDDocumentInformation;
import org.apache.pdfbox.pdmodel.common.PDMetadata;
import org.apache.xmpbox.XMPMetadata;
import org.apache.xmpbox.schema.DublinCoreSchema;
import org.apache.xmpbox.schema.PDFAIdentificationSchema;
import org.apache.xmpbox.schema.XMPBasicSchema;
import org.apache.xmpbox.type.BadFieldValueException;
import org.apache.xmpbox.xml.XmpSerializer;

import java.util.ArrayList;
import java.util.Calendar;
import java.util.List;
import javax.xml.transform.TransformerException;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.pdfbox.io.IOUtils;


public class NewMergePdfs
{
    private static final Log LOG = LogFactory.getLog(NewMergePdfs.class);
    
    public static void main(String[] a) throws Exception 
    {
    	merge();    	
    }
    
    public static void merge() throws Exception
    {
    	FileOutputStream  fos = new FileOutputStream(new File("merge.pdf"));
    	
    	ByteArrayOutputStream mergedPDFOutputStream = null;
    	File file1 = new File("sample1.pdf");
    	File file2 = new File("sample2.pdf");
    	
    	List<InputStream> sources = new ArrayList<InputStream>();
    	
        try
        {
        	sources.add(new FileInputStream(file1));
        	sources.add(new FileInputStream(file2));
        	
            mergedPDFOutputStream = new ByteArrayOutputStream();
           
            //设定来源和目标
            PDFMergerUtility pdfMerger = new PDFMergerUtility();
            pdfMerger.addSources(sources);
            pdfMerger.setDestinationStream(mergedPDFOutputStream);
            
            //设置合并选项
            PDDocumentInformation pdfDocumentInfo = new PDDocumentInformation();
            pdfMerger.setDestinationDocumentInformation(pdfDocumentInfo);

            //合并
            pdfMerger.mergeDocuments(MemoryUsageSetting.setupMainMemoryOnly());
            
            fos.write(mergedPDFOutputStream.toByteArray());
            fos.close();
        }
        catch (Exception e)
        {
            throw new IOException("PDF merge problem", e);
        }        
        finally
        {
            for (InputStream source : sources)
            {
                IOUtils.closeQuietly(source);
            }            
            IOUtils.closeQuietly(mergedPDFOutputStream);
            IOUtils.closeQuietly(fos);
        }
    }    
}

```
**删除pdf**
```java
package pdfbox;

import java.io.File;

import org.apache.pdfbox.pdmodel.PDDocument;


public class RemovePdf {

	public static void main(String[] args) throws Exception {
		File file = new File("merge.pdf");
		PDDocument document = PDDocument.load(file);

		int noOfPages = document.getNumberOfPages();
		System.out.println("total pages: " + noOfPages);

		// 删除第1页
		document.removePage(1);  // 页码索引从0开始算

		System.out.println("page removed");

		// 另存为新文档
		document.save("merge2.pdf");

		document.close();
	}
}

```
**将docx转化为pdf**

```java
import java.awt.Color;
import java.io.FileInputStream;
import java.io.FileOutputStream;

import org.apache.poi.xwpf.usermodel.XWPFDocument;

import com.lowagie.text.Font;
import com.lowagie.text.pdf.BaseFont;

import org.apache.poi.xwpf.converter.pdf.PdfConverter;
import org.apache.poi.xwpf.converter.pdf.PdfOptions;
import fr.opensagres.xdocreport.itext.extension.font.IFontProvider;
import fr.opensagres.xdocreport.itext.extension.font.ITextFontRegistry;

/**
 * XDocReportTest 将docx文档转为pdf
 * @author Tom
 *
 */
public class XDocReportTest {

	public static void main(String[] args) throws Exception {
		XWPFDocument doc = new XWPFDocument(new FileInputStream("template.docx"));// docx
		PdfOptions options = PdfOptions.create();
		options.fontProvider(new IFontProvider() {
			// 设置中文字体
			public Font getFont(String familyName, String encoding, float size, int style, Color color) {
				try {
					BaseFont bfChinese = BaseFont.createFont(
							"C:\\Program Files (x86)\\Microsoft Office\\root\\VFS\\Fonts\\private\\STSONG.TTF",
							BaseFont.IDENTITY_H, BaseFont.EMBEDDED);
					Font fontChinese = new Font(bfChinese, size, style, color);
					if (familyName != null)
						fontChinese.setFamily(familyName);
					return fontChinese;
				} catch (Throwable e) {
					e.printStackTrace();
					return ITextFontRegistry.getRegistry().getFont(familyName, encoding, size, style, color);
				}
			}
		});
		PdfConverter.getInstance().convert(doc, new FileOutputStream("template.pdf"), options);// pdf
	}
}

```