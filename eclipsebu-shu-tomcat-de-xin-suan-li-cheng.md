**Eclipse上tomcat部署**

**1:官网下载tomcat**

Tomcat**：**[http://tomcat.apache.org/download-70.cgi](http://tomcat.apache.org/download-70.cgi)

由于安装的elipse 是最新版  所以建议下载tomcat 9,具体原因下面介绍

或者可以采用 brew 直接安装

终端输入:brew install tomcat

两种方式任选其一,安装完成后在其安装目录下运行 startup.sh/start.sh 脚本

```
MacBookPro:~ alenfitz$ cd /usr/local/Cellar/apache-tomcat-9.0.1/bin/
MacBookPro:bin alenfitz$ sh startup.sh 
Using CATALINA_BASE:   /usr/local/Cellar/apache-tomcat-9.0.1
Using CATALINA_HOME:   /usr/local/Cellar/apache-tomcat-9.0.1
Using CATALINA_TMPDIR: /usr/local/Cellar/apache-tomcat-9.0.1/temp
Using JRE_HOME:        /Library/Java/JavaVirtualMachines/jdk1.8.0_152.jdk/Contents/Home
Using CLASSPATH:       /usr/local/Cellar/apache-tomcat-9.0.1/bin/bootstrap.jar:/usr/local/Cellar/apache-tomcat-9.0.1/bin/tomcat-juli.jar
Tomcat started.
MacBookPro:bin alenfitz$ 
```

打开浏览器 在地址栏输入 localhost:8080 验证 tomcat是否正常运行

其正常启动页面如下图:

![](/assets/Screen Shot 2017-11-22 at 10.03.56.png)若未能正常启动 一般该页面都会报404 page not found 的错误,建议查看Tomcat 日志文件

```
MacBookPro:~ alenfitz$ cd /usr/local/Cellar/apache-tomcat-9.0.1/logs/
MacBookPro:logs alenfitz$ ls
catalina.2017-11-21.log			localhost.2017-11-22.log
catalina.2017-11-22.log			localhost_access_log.2017-11-21.txt
catalina.out				localhost_access_log.2017-11-22.txt
host-manager.2017-11-21.log		manager.2017-11-21.log
host-manager.2017-11-22.log		manager.2017-11-22.log
localhost.2017-11-21.log
MacBookPro:logs alenfitz$ vim catalina.2017-11-22.log 
```

日志中一般会记录出现的问题，排查问题，使得tomcat正常启动\(一般都不会有问题的\)

**2:Eclipse 部署Tomcat Server**

打开 Eclipse **Preferences -&gt;Server -&gt;Runtime Environment**

选择 add.. 弹出的窗口里选择Apache 版本 ,选择 9.0版本

然后导入tomcat的安装目录  如下图

![](/assets/Screen Shot 2017-11-22 at 10.21.50.png)

一直强调需要下载9.0版本的Tomcat 以及选择apache tomcat v9 是因为**只有Tomcat 9.0 以上的版本才支持JDK 1.8+**

这里放出tomcat官方关于这方面的说明以及截图

Tomcat:[https://tomcat.apache.org/whichversion.html](https://tomcat.apache.org/whichversion.html)

截图如下

![](/assets/Screen Shot 2017-11-22 at 10.25.45.png)同样 关于Eclipse 里的Tomcat Server 版本的选择对于JDK版本依然有要求,这里放出两张对比图

apache tomcat v8.5

![](/assets/Screen Shot 2017-11-22 at 10.30.19.png)apache tomcat v9.0

![](/assets/Screen Shot 2017-11-22 at 10.30.29.png)

如果这三者未能满足，则会报错:~~**Unknown version of Tomcat was specified **~~

这里有一个帖子关于以前是如何解决这种问题的,当然,我说的这个帖子里其实是个bug,已经被官方修复了,不过这个帖子还是有一定的参考性,可以加深对tomcat的了解,有兴趣的可以看下：[How to use Tomcat 8.5.x and TomEE 7.x with Eclipse?](https://stackoverflow.com/questions/37024876/how-to-use-tomcat-8-5-x-and-tomee-7-x-with-eclipse)

到此,成功部署Tomcat Server

**3:启动 Tomcat Server **

在Eclipse Server里启动tomcat ,正常启动

在浏览器页面访问localhost:8080 页面直接报 **404 Page Not Found**

在网上搜索一番,发现原来是eclipse 将 tomcat 的项目发布目录（tomcat 目录中的webapp）重定向了,所以你会发现在tomcat安装目录下的webapp目录里面找不到你的项目文件。

解决办法:

重新配置下tomcat服务器：

在eclipse中的server页面，双击tomcat服务，会看到如图所示的配置页面：

![](/assets/Screen Shot 2017-11-22 at 10.45.11.png)  
①:更改Server Locations 为Use Tomcat installation 这里看到是灰色的无法更改,原因可能有两个:

1:tomcat server 里已经存在部署的项目,在tomcat Add and Remove 选项中 移除所有项目

2:tomcat server 仍在启动状态 停掉即可

②:将Deploy path ~~**wtpwebapps **~~改为 **webapps**

③:保存更改 重新启动 Tomcat Server 即可

### **总结**：

结合自己的实际情况，而不是一味地盲从,即使网上的教程是正确的,适合自己的才是最好的,在网上查找教程的同时，要学会自己思考和分析问题.

**相关链接:**

tomcat 不同版本匹配的JDK :[https://tomcat.apache.org/whichversion.html](https://tomcat.apache.org/whichversion.html)

~~解决Tomcat版本不匹配~~:[How to use Tomcat 8.5.x and TomEE 7.x with Eclipse?](https://stackoverflow.com/questions/37024876/how-to-use-tomcat-8-5-x-and-tomee-7-x-with-eclipse)

Eclipse启动Tomcat无法访问:[http://blog.csdn.net/wqjsir/article/details/7169838/](http://blog.csdn.net/wqjsir/article/details/7169838/)

  


