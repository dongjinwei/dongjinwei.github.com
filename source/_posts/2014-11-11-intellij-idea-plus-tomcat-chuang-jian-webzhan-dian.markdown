---
layout: post
title: "IntelliJ IDEA + TOMCAT 创建web站点"
date: 2014-11-11 15:48:23 +0800
comments: true
categories: 
---
###下载InterlliJ IDEA专业版
注意必须要是专业版，社区版是没有创建web application功能的，key可以从网上找到

###创建web application
![icon](http://img.hb.aicdn.com/3d31864c2f7e04020d7d2b847352c1818e6deed113511-JGktjx_fw658)

###本地下载tomcat并配置
![icon](http://img.hb.aicdn.com/ecbca7f88f75b9297ba5699eb6a022f712dd24df17fc0-zkmMnk_fw658)
![icon](http://img.hb.aicdn.com/83529faa9f07c39e58b690b5a782074bf70913331a69d-AA7HnV_fw658)
![icon](http://img.hb.aicdn.com/00cd60d2f7cc6bc1a8f5f794d3b54a164ec0360113681-Wxnsmx_fw658)

###运行web app
完整结构：![icon](http://img.hb.aicdn.com/71232e1c0dae6121a46280d585d666e1bb5874ef7000-qx9LRh_fw658)
点击运行后，会自动启动tomcat，可以输入<http://localhost:8080/>就行查看。

###丰富网页内容
默认生成的index.jsp是个空页面，而且我们对html/css又不熟悉，如何丰富一下我们的页面呢。那就只能抄了，打开某个你中意的网站，右键查看源代码，直接拷贝其html源代码到我们的index.jsp或者index.html中，点击运行，然后再对照页面删删改改，改成我们所期望的界面

###部署到云端
1. ssh登录到云主机
2. 下载tomcat并解压
	
		wget http://apache.communilink.net/tomcat/tomcat-8/v8.0.14/bin/apache-tomcat-8.0.14.zip
		unzip apache-tomcat-8.0.14.zip

3. 传送本地app文件至云端。一般将工程打成war包，并将其放到tomcat的webapps文件夹下，tomcat将会自动解压此包。我直接将web工程文件夹放到webapps下也可以，如下

		scp -r hello/ ssh root@xxx.xxx.xx.xx:/usr/local/apache-tomcat-8.0.14/webapps/
4. 启动tomcat	
        
        cd tomcat
        chmod 755 bin/*.sh
        bin/startup.sh
至此，云端部署完毕，可以通过云主机ip访问了
