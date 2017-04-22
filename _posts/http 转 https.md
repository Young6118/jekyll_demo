---
title: https #文章標題
date: 2016-11-15 #文章生成時間
categories: "网络" #文章分類目錄 可以省略
tags: https http #文章標籤 可以省略
description: #你對本頁的描述 可以省略
---
###   Http转Https
对于用户来说一般习惯于直接输入网址访问网站，如 xxx.xxx.xxx ，浏览器会补全所使用的协议，浏览器地址栏中自动显示为 http://xxx.xxx.xxx,这样看来浏览器默认用户使用的访问协议是http协议。
现在有如下需求，客户要求输入 xxx.xxx.xxx 或输入 http://xxx.xxx.xxx 时，系统自动转到 https://xxx.xxx.xxx 页面中。
xxx.xxx.xxx 对应的是一个ip地址，前面的http及https协议名称只是该地址上不同的端口80、433处理用户的请求而已，https一般经由ssl服务器才能进入后面的web服务器及应用服务器进行处理，而ssl服务器又并不处理http服务。
1。我们需要在请求到达ssl服务之前，对80端口和443端口的请求进行区分，比如在防火墙上根据端口做NAT，将443端口的请求映射到ssl服务器上，而将80端口的请求映射到DMZ区的任何一台可以对外提供http服务的服务器上，在不增加新硬件的基础之上，可以选则门户服务器。当然不光防火墙，另外一些设备如F5、Redware等设备均可以实现NAT。
2。http服务到达门户服务器后，因门户服务器已对外提供了http服务，并且有自己的首页，我们需要根据客户访问的域名区分客户所访问的首页，即客户在浏览器中输入www.xxx.xxx跳转到门户首页，客户输入xxx.xxx.xxx跳转到新的首页。在apache http server上可以实现该功能
在httpd.conf中增加如下内容即可  对于用户来说一般习惯于直接输入网址访问网站，如 xxx.xxx.xxx ，浏览器会补全所使用的协议，浏览器地址栏中自动显示为 http://xxx.xxx.xxx,这样看来浏览器默认用户使用的访问协议是http协议。
现在有如下需求，客户要求输入 xxx.xxx.xxx 或输入 http://xxx.xxx.xxx 时，系统自动转到 https://xxx.xxx.xxx 页面中。

xxx.xxx.xxx 对应的是一个ip地址，前面的http及https协议名称只是该地址上不同的端口80、433处理用户的请求而已，https一般经由ssl服务器才能进入后面的web服务器及应用服务器进行处理，而ssl服务器又并不处理http服务。
1。我们需要在请求到达ssl服务之前，对80端口和443端口的请求进行区分，比如在防火墙上根据端口做NAT，将443端口的请求映射到ssl服务器上，而将80端口的请求映射到DMZ区的任何一台可以对外提供http服务的服务器上，在不增加新硬件的基础之上，可以选则门户服务器。当然不光防火墙，另外一些设备如F5、Redware等设备均可以实现NAT。

2。http服务到达门户服务器后，因门户服务器已对外提供了http服务，并且有自己的首页，我们需要根据客户访问的域名区分客户所访问的首页，即客户在浏览器中输入www.xxx.xxx跳转到门户首页，客户输入xxx.xxx.xxx跳转到新的首页。在apache http server上可以实现该功能
在httpd.conf中增加如下内容即可


```
[c-sharp] view plain copy
print?

NameVirtualHost *:80
  <VirtualHost *:80>
      DocumentRoot /xxx/www
      ServerName www.xxx.xxx
      ErrorLog logs/www.xxx.xxx-error_log
      CustomLog logs/www.xxx.xxx-access_log common
  </VirtualHost>
  <VirtualHost *:80>
      DocumentRoot /xxx/xxx
      ServerName xxx.xxx.xxx
      ErrorLog logs/xxx.xxx.xxx-error_log
      CustomLog logs/xxx.xxx.xxx-access_log common
  </VirtualHost>


NameVirtualHost *:80<br />
  <VirtualHost *:80><br />
      DocumentRoot /xxx/www<br />
      ServerName www.xxx.xxx<br />
      ErrorLog logs/www.xxx.xxx-error_log<br />
      CustomLog logs/www.xxx.xxx-access_log common<br />
  </VirtualHost><br />
  <VirtualHost *:80><br />
      DocumentRoot /xxx/xxx<br />
      ServerName xxx.xxx.xxx<br />
      ErrorLog logs/xxx.xxx.xxx-error_log<br />
      CustomLog logs/xxx.xxx.xxx-access_log common<br />
  </VirtualHost>

```
3。我们将新的首页放到/xxx/xxx目录下，新的首页可以通过refresh方式实现从http->https的自动跳转
```
[xhtml] view plain copy
print?

<html xmlns=“http://www.w3.org/1999/xhtml”>
<head><title></title>
<meta http-equiv=“refresh” content=“0;url=https://xxx.xxx.xxx”/>
<meta http-equiv=“Content-Type” content=“application/xhtml+xml;charset=utf-8”/>
</head>
<body>
<p>
<span></span>
</p>
<p>
<span></span>
</p>
<p>
<span><a href=“https://xxx.xxx.xxx” mce_href=“https://xxx.xxx.xxx”>手动跳转</a></span>
</p>
<p>
<br>
</p>
</body>
</html>


  <html xmlns=”http://www.w3.org/1999/xhtml”><br />
  <head><title></title><br />
  <meta http-equiv=”refresh” <span id=”mce_marker” data-mce-type=”bookmark”></span><span id=”__caret”>_</span>content=”0;url=https://xxx.xxx.xxx”/><br />
  <meta http-equiv=”Content-Type” content=”application/xhtml+xml;charset=utf-8″/><br />
  </head><br />
  <body><br />
  <p><br />
  <span></span><br />
  </p><br />
  <p><br />
  <span></span><br />
  </p><br />
  <p><br />
  <span><a href=”https://xxx.xxx.xxx” mce_href=”https://xxx.xxx.xxx”>手动跳转</a></span><br />
  </p><br />
  <p><br />
  <br><br />
  </p><br />
  </body><br />
  </html>
```
