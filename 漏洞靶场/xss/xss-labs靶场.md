参考链接：

https://blog.csdn.net/l2872253606/article/details/125638898

https://blog.csdn.net/qq_45939134/article/details/127563979?spm=1001.2014.3001.5502

[xss常见的触发标签](https://blog.csdn.net/LYJ20010728/article/details/116462782)

# level1

![img](https://img-blog.csdnimg.cn/img_convert/4798d711205f8f1e9ea1e4a74e416984.png)

查看网站源码，可以发现get传参name的值test插入了html里头，还回显了payload的长度

直接上payload，插入一段js代码，get传参

```
url?name=<script>alert()</script>
```

弹窗成功

当然也能传其他的东西过掉第一关，建议参考[XSS常见的触发标签](https://blog.csdn.net/LYJ20010728/article/details/116462782) 

我们在看一下这关的源码

![img](https://img-blog.csdnimg.cn/fd46e7882ada4b59a1ba8ac2f2cf3a94.png)

没有啥过滤的，很普通，单纯插入即可

**本关小结**： JS弹窗函数alert()

# level2

查看网站源码 

![img](https://img-blog.csdnimg.cn/c08795690c7f4f85a013e89d11c12877.png)

第一个test可以跟上次一样直接插入js即可，我们先试试看

```html
<script>alert()</script>
```

没成功，看一下源码

![img](https://img-blog.csdnimg.cn/bf7314876c0543c1ad3e7e7da77fb55e.png)

```html
">  <script>alert()</script>  <"
```

弹窗成功

再看一下源码

![img](https://img-blog.csdnimg.cn/7c6002cf287d4dcf8ad2332dbe1d29a4.png)

果然进行了html实体转化 

**本关小结**：闭合绕过 

# level3

```
<script>alert()</script>
```

测试

![image-20230928145650868](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928145650868.png)

这里是单引号闭合，符号也被实体化了

尝试单引号闭合，并且用无特殊符号的弹窗语句

```
'onmouseover='alert(1)
```

弹窗成功

我们看一下源码

![img](https://img-blog.csdnimg.cn/7f01e57fc44b49faa70d9b58345a0ec1.png)果然被实体化了，但是htmlspecialchars函数只针对<>大于小于号进行html实体化

**本关小结** ：单引号闭合，onmouseover可以绕过html实体化（即<>号的过滤）

# level4

```
"onmouseover="alert(1)
```

成功弹窗

看一下这关的源码

![img](https://img-blog.csdnimg.cn/1a52450856734db6b75006b891388913.png)这里只是把<>号给删掉了，没多做过滤

**本关小结** ：双引号闭合，onmouseover可以绕过html实体化（即<>号的过滤），类level3

# level5

先尝试js弹窗

![image-20230928151026550](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928151026550.png)

<>和script做了过滤，**下面那个没有过滤双引号和尖角号**，尝试双引号和onmouseover

```
"onmouseover="alert(1)
```

![image-20230928151358481](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928151358481.png)

onmouseover也被替换

尝试双引号加a标签，a标签写入js伪协议

```
"><a href="javascript:alert(1)">123</a>
```

出现123链接，点击链接，弹窗成功

![img](https://img-blog.csdnimg.cn/1419c7de7dfa4a8b8cc40463723dd56a.png)

href属性的意思是 当标签<a>被点击的时候，就会触发执行转跳，上面是转跳到一个网站，我们还可以触发执行一段js代码

**本关小结**：可以插入标签（如<a>标签的href属性）达到js执行的效果，前提是闭合号<"">没失效

# level6

先尝试js弹窗

![image-20230928152844991](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928152844991.png)

发现15行过滤尖角号，17行过滤script，ri替换为r_i

尝试双引号闭合，onmouseover，a标签

发现on被替换成o_n，href替换为hr_ef

在尝试img标签`<img src="x” onerror=alert(1)>`，src被替换为sr_c

尝试大小写

```
"Onmouseover="alert()
```

弹窗成功

查看源码

![img](https://img-blog.csdnimg.cn/2e2979f0e3304d7ca97a429eaa377d56.png)

这关甚至还过滤掉了data，但是没有添加小写转化函数 ，导致能用大写绕过 

**本关小结**：大小写法绕过str_replace()函数 

# level 7

先上关键字试试看

```xml
" OnFocus <sCriPt> <a hReF=javascript:alert()>
```

![image-20230928154452747](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928154452747.png)

发现17行on和script消失了，所以用双写绕过,加上引号闭合

```
"oonnmouseover="alert()
```

弹窗成功

查看源码

![img](https://img-blog.csdnimg.cn/6238c326b8dd4209a44c00d66fa9621e.png)

str_replace替换字符过滤，strtolower所有字母转为小写

**小结**：他会识别on并删除所以在on中间加一个on，这样，他删除on后就变成了onmouseover了，就可以执行代码了

# level 8

![image-20230928155636685](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928155636685.png)

让添加链接

尝试a标签

```
">"<a href="javascript:alert(1)">123</a>
```

![image-20230928155946156](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928155946156.png)

20行发现他将输入的放到了a标签，并且script做了替换过滤

利用href的隐藏属性自动Unicode解码，我们可以插入一段js伪协议

```
javascript:alert()
```

利用在线工具进行Unicode编码后得到，[在线Unicode编码解码](https://www.matools.com/code-convert-unicode)

```
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#41;
```

接着我们插入href里面

![img](https://img-blog.csdnimg.cn/fa508fc6addb458cb4bef2043372e5c3.png)

点击友情链接，弹窗成功

查看源码

![img](https://img-blog.csdnimg.cn/cf20c96c9ba543c2a8b43876a6eaabfe.png)

**本关小结**： href属性自动解析Unicode编码

# level9

![image-20230928161504795](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928161504795.png)

输入框让添加链接

看看都过滤了啥

```
" sRc DaTa OnFocus <sCriPt> <a hReF=javascript:alert()> &#106;
```

![image-20230928162003027](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928162003027.png)

一个进行了编码，另一个肯定也是做了某种处理，我们想象一下，会不会是检测我们输入的字符进行了某种匹配。

我尝试输入一下www.baidu.com，也是链接不合法，输入http://www.baidu.com，发现可以跳转，到这里我们就会发现极其有可能是对http://或者是https://进行了匹配，发现是http://

既然知道了它的规则，那么我们尝试攻击一下

```
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;//http://
```

弹窗成功

查看源代码

![img](https://img-blog.csdnimg.cn/2b1d1f20f1494720bab701dbcae70a44.png)

看一下这关的源码里呢，当false等于false的时候(就是传入的值没有http://)就会执行if，为了防止false===false，我们需要向传入的值里面添加http://并用注释符注释掉否则会执行不了无法弹窗，让函数strpos返回一个数字，构造payload

```
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;//http://
```

弹窗成功

**本关小结**：**插入指定内容**（本关是http://）绕过检测，再将指定内容用注释符注释掉即可 

# level10

![image-20230928164144392](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928164144392.png)

不管那么多，老规矩，先测试一下关键字

```xml
" sRc DaTa OnFocus <sCriPt> <a hReF=javascript:alert()> &#106;
```

![img](https://img-blog.csdnimg.cn/cac51a8eb6fd47de8782e2b57d225cf8.png)

一个进行了编码，还有三个隐藏的input。

那么根据他们的name构造传值，让它们的type改变，看一下谁可以被改变。

```
http://localhost/xss/level10.php?t_link="type='text'>//&t_history="type='text'>//&t_sort="type='text'
```

![image-20230928170247215](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928170247215.png)

可看到t_sort发生了改变

![image-20230928170818508](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928170818508.png)

接下来我们就向t_sort传值。

```
http://localhost/xss/level10.php?&t_sort="type="text"onmouseover="alert()
```

![image-20230928172124203](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928172124203.png)

**本关小结**：根据源码猜解传参的参数名，隐藏的input标签可以插入type="text"显示 

# level11

![image-20230928211345625](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928211345625.png)

在url中输入我们的恶意代码，发现并没有执行，

看一下前端代码

![img](https://img-blog.csdnimg.cn/img_convert/e718d2dc5369199af288feb924cb90bc.png)

发现一个是编码和四个隐藏的input

可以看出，第四个名为t_ref的<input>标签是http头referer的参数（就是由啥地址转跳到这里的，http头的referer会记录有）

还是去根据他们的name构造传值，让它们的type改变，看一下谁可以被改变。

```
http://localhost/xss/level11.php?t_link="type='text'>//&t_history="type='text'>//&t_sort="type='text'>//&t_ref="type='text'
```

![image-20230928211955760](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928211955760.png)

![image-20230928212017550](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928212017550.png)

发现t_sort值过滤了双引号和尖角号，那就是无法进行闭合。其他的也没有回显

get没有回显，试试post传参

![img](https://img-blog.csdnimg.cn/13ffecadc30b41feb6ebc7ef61295802.png)

POST传参也不得，那应该就referer头了，用burpsuite抓包一下，添加http头

这里我们就开始使用工具BurpSuite进行抓包

添加

```
referer: "onmouseover=alert() type="text
```

![image-20230928214038631](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928214038631.png)

弹窗成功

**本关小结**：考虑一下http头传值，本关是referer，但接下来也有可能是其他头，如Cookie等

# level12

在url中输入参数发现没有执行，查看前端代码

![image-20230928214601965](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928214601965.png)

![image-20230928214719136](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928214719136.png)

有一个编码和四个隐藏的

t_ua猜测是http头的UA头

先get传参试一下

```
http://localhost/xss/level12.php?t_link="type='text'>//&t_history="type='text'>//&t_sort="type='text'>//&t_ua="type='text'>//
```

![image-20230928215226391](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928215226391.png)

发现

t_sort过滤了双引号，无法闭合

接下来尝试抓包修改ua头

```
"onmouseover=alert() type="text
```

![image-20230928215442544](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928215442544.png)

成功弹窗



# level13

![image-20230928215740341](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928215740341.png)

有个t_cook,猜测是cookie头

![image-20230928220119433](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928220119433.png)

放行，弹窗成功

# level-14

题有问题，不做测试

# level-15

![image-20230928221210987](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928221210987.png)

![image-20230928221239018](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928221239018.png)

有个陌生的东西ng-include

```
ng-include 指令用于包含外部的 HTML文件。
包含的内容将作为指定元素的子节点。
ng-include 属性的值可以是一个表达式，返回一个文件名。
默认情况下，包含的文件需要包含在同一个域名下。

ng-include,如果单纯指定地址，必须要加引号
ng-include,加载外部html，script标签中的内容不执行
ng-include,加载外部html中含有style标签样式可以识别
localhost/xss-labs-master/level15.php?src='level1.php?name=<img src=0 οnerrοr=alert(/tubage/)>'
```

输入

```
?src='level1.php?name=<img src=0 onerror=alert()>'
```

弹窗成功

也能用p标签

```
?src='/level1.php?name=<p onmousedown=alert()>哈哈哈</p>'
```

点击哈哈哈成功弹窗

实体化函数，形同虚设

 **本关小结**：ng-include文件包涵，可以无视html实体化

# level16

![image-20230928224420822](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928224420822.png)

参数在标签里，所以不用闭合了

试一下过滤了哪些

```
" ' sRc DaTa OnFocus OnmOuseOver OnMouseDoWn P <sCriPt> <a hReF=javascript:alert()> &#106; 
```

![image-20230928225110992](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928225110992.png)

可见关键字**script**以及**/**和**空格**都被编码成同样的空格字符实体了。

这样也没办法去闭合前面的标签了，所以现在我需要的是一个不需要闭合的标签

```
<img src="x” onerror=alert(1)>
```

空格还需要用回车绕过，回车的url编码为%0a，所以输入

```
http://localhost/xss/level16.php?keyword=<img%0asrc="x"%0aonerror=alert(1)>
```

成功弹窗

**本关小结**：回车代替空格绕过检测

# level17

![image-20230928231041828](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928231041828.png)

![image-20230928231133486](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230928231133486.png)

embed 标签定义嵌入的内容，比如插件。打开后缀名为swf的文件（FLASH插件的文件，现在很多浏览器都不支持FLASH插件了）
绕过思路：
可以用onclick或onmouseover绕过。
因为这两个变量是互相拼接起来的，所以在arg02=b后**加一个空格，当浏览器解析到b的时候就停止判断**，然后将onclick或onmouseover看作另外一个属性。

由于当前大部分浏览器不兼容embed，所以我们在使用浏览器的时候可能只有onmouseover会生效。
火狐浏览器不管那个都不会生效！

输入恶意代码

```
?arg01=a&arg02=b onmouseover=alert(1)
```

成功弹窗

**本关小结**：embed标签，在arg02=b后**加一个空格**，当浏览器解析到b的时候就停止判断

# level18

和level-17相同

# level19、level20

flash xss

涉及反编译

```
加载第三方资源

与javascript通信引发XSS。

常见的可触发xss的危险函数有：
getURL navigateToURL ExternalInterface.call htmlText loadMovie等
想要查看是否属于flash xss，只需要对引用的swf文件
进行反编译然后进行源码分析
具体对应的方法请自行分析
```

