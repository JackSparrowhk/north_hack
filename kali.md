# [kali linux 的ssh服务器拒绝了密码 请再试一次](https://www.cnblogs.com/pandana/p/15063892.html)

1.配置kali linux下的SSH，默认情况下kali下的SSH不允许root用户远程登录SSH，需要修改配置文件Port 22中的#需要去掉， **/etc/ssh/sshd_config**，
修改**PermitRootLogin** yes，**PasswordAuthentication** yes 两个参数，把 #去掉

只修改这两个参数为yes（注意空格），其他配置不要变，然后点击右上角的Save，再退出。

重启ssh：`systemctl start ssh`

# 打不开sqli靶场网站

kali里http://主机ip/sqli-labs-master打开报错

新加一条
ip   域名
的解析记录

![98b6e5e6ea807bc5698fa64f729c279](C:\Users\16159\AppData\Local\Temp\WeChat Files\98b6e5e6ea807bc5698fa64f729c279.png)

![078ef41beaba032f3d3203e7fe6be71](C:\Users\16159\AppData\Local\Temp\WeChat Files\078ef41beaba032f3d3203e7fe6be71.png)

然后http://sqli即可正常打开