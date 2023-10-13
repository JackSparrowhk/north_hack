[sql注入教程](https://blog.csdn.net/dreamthe/article/details/123795302)

[Sqli-labs全通关教程（手工注入+工具使用sqlmap）](https://blog.csdn.net/qq_57223070/article/details/129189398?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169357619016800213063329%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169357619016800213063329&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-129189398-null-null.142^v93^chatsearchT3_1&utm_term=sqlmap%E9%80%9A%E5%85%B3sqli-labs%E9%9D%B6%E5%9C%BA&spm=1018.2226.3001.4187)

# 第一关

?id=1’联合注入

```php
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";		#源码
```



# 第二关

?id=1联合注入

```php
$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";		#源码
```



# 第三关

?id=‘)联合注入

```php
$sql="SELECT * FROM users WHERE id=('$id') LIMIT 0,1"；		#源码
```



# *第四关

?id=1”)联合注入

```php
$id = '"' . $id . '"';
$sql="SELECT * FROM users WHERE id=($id) LIMIT 0,1";		#源码
```

?id=1)不可以

# 第五关

?id=1’报错注入

```php
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";	#源码
```

# *第六关

?id=1”报错注入

```php
$id = '"'.$id.'"';
$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";		#源码
```

不可以和第二关一样id=1注入

# 第七关

?id=1’))get布尔盲注

```php
$sql="SELECT * FROM users WHERE id=(('$id')) LIMIT 0,1";	#源码
```

# 第八关

?id=1’get布尔盲注

```php
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";	
```

# 第九关

第九关会发现我们不管输入什么页面显示的东西都是一样的，这个时候布尔盲注就不适合我们用，布尔盲注适合页面对于错误和正确结果有不同反应。如果页面一直不变这个时候我们可以使用时间注入

?id=1’get时间盲注

# 第十关

第十关和第九关一样只需要将单引号换成双引号。

?id=1“get时间盲注

# 第十一关

POST请求

?id=1’进行联合注入

当我们输入1',出现报错信息。根据报错信息可以推断该sql语句username='参数' and password='参数'

![img](https://img-blog.csdnimg.cn/1b3dd94671ab41a1a63de6b72c70635a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA57OK5raC5piv56aPeXl5eQ==,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![img](https://img-blog.csdnimg.cn/2dc8a864fb3e43a9ac7a6b8c24f80354.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA57OK5raC5piv56aPeXl5eQ==,size_17,color_FFFFFF,t_70,g_se,x_16)

 知道sql语句我们可以构造一个恒成立的sql语句，看的查询出什么。这里我们使用--+注释就不行，需要换成#来注释， 这个就和我们第一关是一样了。使用联合注入就可以获取数据库信息。

![img](https://img-blog.csdnimg.cn/95be9ac665784c51b80cce19b078cc1f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA57OK5raC5piv56aPeXl5eQ==,size_20,color_FFFFFF,t_70,g_se,x_16)

![img](https://img-blog.csdnimg.cn/ef2e4b4ffe4b426e9b705df79fec3bbe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA57OK5raC5piv56aPeXl5eQ==,size_19,color_FFFFFF,t_70,g_se,x_16)

然后用?id=1’进行联合注入

# 第十二关

POST请求 ?id=1”)联合注入

# 第十三关

POST请求 ?id=1‘)报错注入	#联合注入 注入正确后不回显
