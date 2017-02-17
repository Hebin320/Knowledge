## 引言 ##
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; color:rgb(85,85,85); font-family:微软雅黑,宋体,黑体,Arial; font-size:14px; line-height:23.8px">
　　Android开发的基础语言是基于Java语言的，所以，在搭建Android的开发环境之前，应该先搭建Java的开发环境。Java开发环境的搭建其实就是对JDK、JRE的安装以及环境变量的配置。总的来说Android开发环境可以分为以下四步：</p>
<font color="#1E90FF" size = 4>第一步：安装JDK（jdk  &&  jre）</font>
<font color="#1E90FF" size = 4>第二步：配置Windows上JDK的变量环境</font>
<font color="#1E90FF" size = 4>第三步：下载安装Android Studio以及Android的Sdk</font>
<font color="#1E90FF" size = 4>第四步：安装配置Android模拟器</font>
</br>
</br>
</br>
## 第一步：安装JDK（jdk  &&  jre） ##

 1. JDK可以到官网下载，根据自己的电脑，选择合适的JDK版本，下载地址是：
 </br>
 [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
 </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217100257563?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 </br>
 </br>
 2. 下载完后就选择路径安装，需要注意的是，当jdk安装完之后，会继续安装jre，这时候，jre的路径可以选择在jdk文件夹的同一目录下，安装完可以得到以下两个文件夹：
 </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217100738310?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 </br>
 </br>
## 第二步：配置Windows上JDK的变量环境##

 3. 在“我的电脑”右键的弹出菜单中选择“属性”
 4. 然后在左边的菜单栏上选择“高级系统设置”
 </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217101817043?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
</br>
</br>
 5. 然后，选择“环境变量”
 </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217101940071?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
</br>
</br>
 6. 在“系统变量”中，新建一个系统变量，名为“JAVA_HOME”，路径是JDK的安装路径：
 </br>
 ![这里写图片描述](http://img.blog.csdn.net/20170217102331448?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 </br>
 </br>
 7. 在“系统变量”中，修改“Path”的值，在原来的变量值中加上
*;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin*

 8. 在“系统变量”中，新建一个系统变量，名为“CLASSPATH ”,它的值设置为：
 *.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar*

 9. 开始-运行-CMD
     在命令行输入Java -version看有没有相应版本信息出现，如果跟下图一样，则证明Java开发环境搭建完成：
     ![这里写图片描述](http://img.blog.csdn.net/20170217103227687?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 下载安装Android Studio以及Android的Sdk ##
<p>
　　由于我们大天朝的原因，很多人都过不去那道厚厚的墙，所以很多人都上不了Android的官网去下载Android Studio，这里提供一个Android官网的中国镜像网站，你可以在这个网站上下载到你所需要的工具</p>
　　[http://www.androiddevtools.cn/](http://www.androiddevtools.cn/)
　　</br>
　　<p>
　　如果电脑上有Android的sdk，那下载Android Studio的时候可以选择不带sdk的版本，如果电脑上没有sdk 的话，就选择带sdk 的版本。Android Studio下载完，按照自己想要放置的路径安装并下载安装sdk即可。如果想要更多的sdk版本，可以到SDK Manager中下载</p>
　　![这里写图片描述](http://img.blog.csdn.net/20170217105346654?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

</br>
## 第四步：安装配置Android模拟器 ##
<p>
　　安装完Android Studio，打开Android Studio并随便创建一个项目，进入主面板之后，点击这个按钮，弹出一个框，在里面就可以创建Android的模拟器</p>
　　![这里写图片描述](http://img.blog.csdn.net/20170217104601643?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　

![这里写图片描述](http://img.blog.csdn.net/20170217104706785?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
</br>
然后，先选择“Phone”，再选择自己想要的分辨率，然后进行下一步;
</br>
![这里写图片描述](http://img.blog.csdn.net/20170217104856398?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
</br>
然后，选择自己想要的Android版本，即可创建模拟器；
</br>
![这里写图片描述](http://img.blog.csdn.net/20170217105022680?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSGViaW4zMjAzMjA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
