 

终端1发送数据到终端2：

*[localhost~]# echo 123456*

*[localhost~]# echo 123456 > /dev/tty2*

 

按CTRL+alt+F2进入终端

​                               

按CTRL+ALT+F1退出

 

nohup ping www.baidu.com 2>&1 >/dev/null &      # 在后台挂起

 

systemctl set-default graphical.target 转换到图形化界面

systemctl set-default multi-user.target 转换到命令行界面

 

```
bash4.2$界面错误

原因：误删用户家目录下的.bashrc .bash_profile文件

回复：将/etc/skel的文件拷贝到用户家目录下

cd~

pwd

mv /etc/skel/bashrc /etc/skel/bash_profile pwd显示的路径

man bash

vim 文件提示（只读）错误

原因：交换文件没有删除

恢复：删除交换文件（恢复：R 删除交换文件：D）
```



centos7无法挂起:

`getenforce`		#显示Enforcing

vim /etc/selinux/config

修改SELINUX=Permissive

![a8782032ac4ab437cb5e9cd052044a3](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\a8782032ac4ab437cb5e9cd052044a3.jpg)

重启虚拟机

或者`setenforce 0`		`#setenforce 0 将 SELinux 的执行模式设置为 Permissive，关闭 SELinux 防火墙的强制执行。`

`getenforce`		#显示Permissive



winsat disk -drive C		#查看磁盘速度

# Linux 基础命令

## 一、系统工作命令

### ps

·     ps -ef：查看所有进程 显示进程的pid

·    ps -aux：显示所有包含其他使用者的进程

·    ps -ef | grep tomcat：查看指定进程

top 

  实时监控进程

htop

**常用快捷键**

shift+e 切换内存显示模式 (可重复按键切换

z切换是否彩色显示(可重复按键切换)

m 切换内存显示模式(可重复按键切换

e切换底部进程中单位的显示模式 (可重复按键切换)

b 切换高亮选中 (可重复按键切换

w把当前配置保存到文件中，下次启动top会使用当前的配置

h进入帮助菜单(进入菜单后，可按ESC或q退出帮助菜单)

q退出 top命令

### kill

 -9 

 killall 以名字的方式杀死进程

 pkill 以进程的方式杀死进程

### reboot

 重新启动系统

 直接中断了进程

### shutdown

 -H （now） 关闭系统（切断电源）

 -P  关机

   now

 -r （now） 重启（立即）

 -c 取消关闭指令

 wall->killall->sync->halt

 

### Linux系统服务控制

 

#### 一、系统服务控制

```
systemctl 控制类型 服务名称
```

·    1

##### 1、控制类型

**●start****：启动****
 \**●stop\******：停止****
 \**●restart\******：重新启动****
 \**●reload\******：重新加载****
 \**●status\******：查看服务状态**
 **●……**



#### 二、Linux系统的运行级别



##### 1、查看运行级别

```
1.  runlevel 命令    #runlevel只能查看切换运行级别与当前运行级别
2.  systemctl 工具   #ststemctl能查看默认的运行级别
```

·    1

·    2



##### 2、运行级别所对应的Systemd目标



| **运行级别** | **Systemd****的target** | **说明**                                               |
| ------------ | ----------------------- | ------------------------------------------------------ |
| 0            | target                  | 关机状态，使用该级别时将会关闭主机                     |
| 1            | rescue.target           | 单用户模式，不需要密码验证即可登录系统，多用于系统维护 |
| 2            | multi-user.target       | 用户定义 / 域特定运行级别。默认等同于3                 |
| 3            | multi-user.target       | 字符界面的完整多用户模式，大多数服务器主机运行在此级别 |
| 4            | multi-user.target       | 用户定义 / 域特定运行级别。默认等同于3                 |
| 5            | graphical.target        | 图形界面的多用户模式，提供了图形桌面操作环境           |
| 6            | reboot.target           | 重新启动，使用该级别时将会重启主机                     |



##### （1）runlevel命令-图文详解

**1****、先使用****runlevel****查看运行级别，显示****N 5****，说明之前是****N(none)****没有切换过，****5****代表现在是图形界面，然后我们****init 3****切换至字符界面，再次查看，可以看到****5 3****，说明之前是****5****，现在是****3**

![image-20230829170107188](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170107188.png)

 **可以看到我们虚拟机进入了字符界面**

![image-20230829170242485](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170242485.png)

 **2****、我们再次切换回级别5看一下**    

  ![image-20230829170250808](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170250808.png)

![image-20230829170256788](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170256788.png)

##### （2）systemctl命令-图文详解

●**以下是各级别对应的可用命令**



| **init 0** | **systemctl   isolate poweroff.target** | **systemctl   poweroff** | **poweroff** |
| ---------- | --------------------------------------- | ------------------------ | ------------ |
| init 1     | systemctl isolate  rescue.target        | ————                     | ————         |
| init 3     | systemctl isolate  multi-user.target    | ————                     | ————         |
| init 5     | systemctl isolate  graphical.target     | ————                     | ————         |
| init 6     | systemctl isolate  reboot.target        | systemctl reboot         | reboot       |



###### ①查看系统默认的运行级别

```
systemctl get-default      #查看系统默认运行级别
```

·    1![image-20230829170314269](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170314269.png)

 



###### ②设置永久运行级别

```
1.  ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
2.  或                                           ##————设置永久运行级别
3.  systemctl set-default multi-user.target
```



第一种方法：![image-20230829170324287](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170324287.png)


第二种方法：![image-20230829170329729](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170329729.png)




#### 三、优化启动过程

##### 1、ntsysv工具

●**提供一个交互式、可视化窗口****
 \**●\******可以在字符终端运行****
 \**●\******便于集中管理多个服务****
 \**●\******用于控制服务是否开机自启动**![image-20230829170340337](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170340337.png)

 即可进入可视化窗口，自己按需选择开机自启服务（上下键选择，空格键开启或关闭，tab键切换确定或取消回车返回Xshell界面）![image-20230829170351285](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170351285.png)

 ●**验证服务是否开启方法**

```
1.  systemctl is-enabled 【服务名称】    #查看系统服务启动状态
2.  例：systemctl is-enabled firewalld.service   
```

 ![image-20230829170405315](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170405315.png)



##### 2、systectl工具

不提供交互式、可视化窗口
管理单个服务效率更高

```
1.  systemctl enable 【服务名称】      #开启开机自启动
2.  systemctl disable 【服务名称】     #关闭开机自启动
```

 ![image-20230829170411858](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230829170411858.png)



#### 补充

**●****永久修改主机名**

```
hostnamectl set-hostname  （新名称）
```

·    1

**●****查看主机名的状态**

```
hostnamectl status 
```

·    1

**●****设置系统语言为中文**

```
localectl set-locale LANG=zh_CN.utf8
```

·    1

**●****查看当前系统使用的语言**

```
localectl [status]
```

·    1

**●****查看系统启动耗时**

```
systemd-analyze 
```

## 二、系统状态命令

### df

 -h 可读式的查看磁盘的使用情况

### free

 查看内存的使用情况（Memory Swap）

### uptime

 查看系统的运行时间

### ifconfig

 查看网络状态，IP地址

   nmcli

   ip addr

### netstat

 查看现在开放的端口号

 -ltunp  tcp udp -anp

### history

 查看历史命令

 history 10:查看最近10条命令

### who

 查看现在登录的终端

### uname

 -a 查看内核的版本及日期

### cal

 显示日历  cal 月份 年

### timedatectl

 查看并设置时区

 list-timezones

 set-timezone

### hostnamectl

 set-hostname 修改主机名

### & 或ctrl+z

 挂起一个执行的命令

### systemctl

[命令汇总](https://blog.csdn.net/carefree2005/article/details/125886811?ops_request_misc=%7B%22request%5Fid%22%3A%22169329538416800197091920%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169329538416800197091920&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-125886811-null-null.142^v93^chatsearchT3_1&utm_term=systemctl&spm=1018.2226.3001.4187)



## 三、查找文件定位命令

### find 

 find 起始路径 -name 被查找的文件名 查找完全匹配的文件

 -type 查找指定文件类型的文件

   d  查找目录  f 查找文件

find / -maxdepth 10 -name etc

find 目录 -maxdepth 想要查找的深度值 -name 

### locate 

 通过名字查找文件

 updatedb：更新locate的查找库

### grep

 过滤（通过给定的字符串）

### which

 查看命令的位置

### whereis 

 -b/m/s 搜索二进制/手册/源代码文件

### reset

 初始化终端

### time

 统计命令执行的时间

\```

 

练习：在自定目录下创建test文件夹，包含test1到3的文件，更新locate数据库，使用find和locate查找并**统计**时间

 

## 四、文件目录相关命令

### ls 

 列出指定路径下的文件

 -a 列出隐藏文件（全部文件）

 -R 递归列出文件

 -h 可读式的列出文件

 -l 列出文件的权限、用户、属组、时间、大小等信息（ll）

### cd （change directory）

 切换/改变目录

 相对路径

 . 当前目录

 .. 上一级目录

 绝对路径

 从根目录逐级显示的路径

 

 \- 回到上次命令执行的目录  $OLDPWD

 ~ 切换到用户的家目录

### pwd

 列出当初路径（绝对路径）

### mkdir 

 创建文件夹

 -p --parents 递归的创建文件夹

### rm 

 (remove) 删除文件/文件夹

 -f --force 无提示删除

 -r 删除文件夹

### rmdir

 删除空目录

### cp 

 复制文件

 -R 递归复制（复制文件夹）

### mv 

 移动文件为...  mv file1 file2 newfolder

 重命名：

 例：mv /etc/skel/bashrc /etc/skel/.bashrc

### du

 查看磁盘空间

 -sh 统计文件夹大小（包括文件）

 

 

 

## 五、文本文件相关命令

### cat

 列出文件的所有内容

 -n 显示行号

### touch

 创建文件

### echo

 显示指定的内容

 向touch1文件里写入112345

   例：echo 112345 > touch1

### less

 逐页显示内容

 Enter 逐行查看 q 退出 h 帮助文档

 f 下一页  b 上一页  j 下一行 k 上一行

 v 切换到vim模式

### more

 Enter 逐行显示内容

 q 退出 h 帮助

 备份/etc/passwd到/root

 more /etc/passwd > /root/passwd

### head

 查看文件的前10行

 -n 指定查看的行数

### tail

 查看文件的末10行

 -n 指定查看的行数

### wc

 查看文件的行数、字节数、单词数

 -l --line 显示行数

 -L 查看文件最长行的长度

### sort

 对输出的内容或文件进行排序

 -n 按照大小进行排序

### cut

 -d 指定分隔符 -f 指定分隔的域

 列出passwd文件的第一列

 例：cut -d ":" -f 1 passwd

 将passswd文件用":"分割成不同区域并指定显示第一个区域

  

### tar

 压缩率：bzip2 > gzip > zip

 打包，解包，压缩，解压

 -c 归档文件 -f 指定文件 -v 详细信息

 打包：tar -cf .tar 要打包的文件

 解包：tar -xf 要解包的tar文件

 gzip压缩：tar -zcf .tar.gz tar文件/文件夹

 gzip解缩：tar -zxf tar.gz文件

 bzip2压缩：tar -jcf .tar.bz tar文件/文件夹

 bzip2解缩：tar -jxf tar.bz文件

 

### zip

 使用zip格式压缩文件

 -r 压缩文件夹

 格式：zip 文件名.zip -r 文件夹

 

### unzip

 解压zip格式的压缩文件

 格式：unzip 文件名.zip

 

### dd

 拷贝文件

 格式：dd if=/dev/zero of=./kaobei bs=1G count=2 

 

### sed

[详解](https://blog.csdn.net/weixin_45842494/article/details/124699219?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169336116016800180675168%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169336116016800180675168&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124699219-null-null.142^v93^chatsearchT3_1&utm_term=linux%20sed&spm=1018.2226.3001.4187)

 替换，打印，删除文件行的内容

 -n 不修改文件，只将修改的操作显示到终端

 -i 修改文件

-f 脚本文件

 d 删除 a 增加 c 行替换（单行、全部行）

 i 插入 p 打印 s 字符替换

 

删除行

​    格式：sed -i '1,5d' passwd

  将所有有jin字符的行替换成root

​    格式：sed -i '/jin/c root' passwd

  将所有jin的字符替换成root

​    格式：sed -i 's/jin/root/' passwd

  

```
命令选项
-e script 将脚本中指定的命令添加到处理输入时执行的命令中,  多条件，一行中要有多个操作
-f script 将文件中指定的命令添加到处理输入时执行的命令中
-n        抑制自动输出
-i        编辑文件内容
-i.bak    修改时同时创建.bak备份文件。

sed常用内部命令
a   在匹配后面添加
i   在匹配前面添加
p   打印
d   删除
s   查找替换
c   更改
y   转换   N D P 

flags
数字             表示新文本替换的模式
g：             表示用新文本替换现有文本的全部实例
p：             表示打印原始的内容
w filename:     将替换的结果写入文件
```

 

## 六、其他常用命令

### clear

 清屏

### man

 查看命令手册（外置命令）

### alias

给命令起一个别名

注意：别名只在当前的 shell 会话中有效，如果你关闭了终端或会话，别名就会失效。如果你希望别名在每次会话中都有效，你可以在 shell 的初始化脚本（例如 bash 的 ~/.bashrc 或 ~/.bash_profile）中添加该命令。

 

alias nmap="grc nmap"   #

 \#GRC 为 cli 应用程序着色，这将使 nmap 非常漂亮，并使响应具有一定的可读性

 

###  配置文件

 \# 当前用户

 ~/.bashrc ~/.bash_profile

 \# 所有用户

 /etc/bashrc /etc/bash_profile



### 快捷键

ctrl+w：删除前一个单词

ctrl+u/k:删除光标前/后所有单词

ctrl+a/e：光标移动到行首/行尾

Ctrl+Insert：复制命令行内容*

Shift+Insert：粘贴命令行内容*

Ctrl+c：中断终端正在执行的任务或者删除整行*

Ctrl+r：搜索命令行使用过的历史命令记录*

Ctrl+l：清除屏幕所有内容，并在屏幕最上面开始一个新行，等同clear命令*

Ctrl+z：暂停执行在终端运行的任务*

!号开头的快捷命令
!!：执行上一条命令
!pw：执行最近以pw开头的命令
!pw:p	：仅打印最近pw开头的命令，但不执行
!num	：执行历史命令列表的第num(数字)条命令

### unalia

 取消别名

### wait

 等待指定的进程

 格式：wait PID进程id

 

### lsof

[详细教程](https://blog.csdn.net/humanhaunt/article/details/120067811?ops_request_misc=%7B%22request%5Fid%22%3A%22169275580316800226597343%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169275580316800226597343&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-120067811-null-null.142^v93^chatsearchT3_1&utm_term=lsof教程&spm=1018.2226.3001.4187)

 

lsof -i :端口号		#查看指定端口的程序

 lsof -p pid号		#查看pid号的调用的进程情况

### history

```
使history命令显示执行时间、用户
vim /root/.bashrc
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S  `who am i | awk '{print $1,$5}'` "

source .bashrc	#执行.bashrc文件
```

###  炫酷命令

apt-get install hollywood		#炫酷跑代码页

apt-get install cmatrix				#代码雨

 apt-get install sl							#火车

yes hello,world				#yes后面自定义，功能是持续输出后面的文字，ctrl c 停止

apt-get install bastet					#俄罗斯方块

apt-get install rig							#随机生成一个人名、地址等信息



用ASCII码格式显示一张图片

```
apt-get install aview
asciiview 图片名.jpg -driver curses

选项：
   -driver 选择驱动程序
                   可用的驱动程序：linux slangcurses X11 stdout stderr
   -kbddriver 选择键盘驱动程序
                   可用的驱动程序：slang curses X11 stdin
   -mousedriver 选择鼠标驱动程序
                   可用驱动程序：X11 gpm cursesdos

尺寸选项：
   -width 设置宽度
   -height 设置高度
   -minwidth 设置最小宽度
   -minheight 设置最小高度
   -maxwidth 设置最大宽度
   -maxheight 设置最大高度
   -recwidth 设置推荐宽度
   -recheight 设置推荐高度

属性：
   -dim 启用暗淡（半亮）属性的使用
   -bold 启用粗体（双亮）属性的使用
   -reverse 启用反向属性的使用
   -normal 启用正常属性的使用
   -boldfont 启用粗体属性的使用
   -no<attr> 禁用（即 -nobold）

字体渲染选项：
   -extended 使用全部 256 个字符
   -eight 使用八位 ASCII
   -font <font> 选择字体(此选项仅对硬件有效
                   aalib 无法确定当前字体
                   可用字体：vga8 vga9 mda14 vga14 X8x13 X8x16
                    X8x13bold vgagl8 line

渲染选项：
   -inverse 启用逆渲染
   -noinverse 禁用逆向渲染
   -bright <val> 设置明亮 (0-255)
   -contrast <val>设置对比度 (0-255)
   -gamma <val> 设置gamma校正值(0-1)

抖动选项：
   -nodither 禁用抖动
   -floyd_steinberg 弗洛伊德·斯坦伯格抖动
   -error_distribution 误差分布抖动
   -random <val> 设置随机抖动值(0-inf)
监控参数：
   -dimmul <val> 暗淡颜色的倍增因子 (5.3)
   -boldmul <val> 暗淡颜色的倍数 (2.7)
   默认参数基于我的 15" Goldstar 显示器，）
   将对比度设置为最大，将亮度设置为最小。
   该值取决于您的特定显示器以及控件的设置方式。
   默认设置对于大多数 PC 显示器来说应该是可以的，但理想的显示器
   需要dimmul=1.71、boldmul=1.43。 例如，SGI 显示器非常
   接近这个值。 旧的 14 英寸 VGA 显示器需要更高的值。
```



### 安中文输入法

```
apt install fcitx
apt-get install fcitx-googlepinyin		#安装google输入法
reboot
```



## 七、重定向

 

标准输出（1）

**>** 

 覆盖输入到文件中  

**>>** 

 追加输入到文件中

 

标准输入（0）

 

\```

**<** 

 将后续内容当作输入传递给前面

\```

 

标准错误（2）

 

\```l

**2>&1**

 将标准错误重定向到标准输出

\```

 

 

 

Here Document

 

\```

**<<** 

 划定一个范围，当做前面的输入

 定界符：delimeter、EOF

\```

 

 

 

## 八、管道符

 

\```

### |              

 将前面命令的输出传递给后面

 常用操作：

   ls /|grep etc

 

### xargs

 将前面的输出重建并执行命令行

\```

## 九、通配符

 

\```

### * 

 匹配任意字符

 格式：a*

### ?

 匹配任意单个字符

 格式：a?

### [] 

 匹配括号内出现的字符(正则表达式)

 格式：ls |egrep [0-9]

### [^]

 不匹配括号内出现的字符(正则表达式)

 格式：ls |grep -E [^0-9]

 

## 十、扩展字符

### {}  生成序列 

  用逗号，分隔并创建指定名称的序列

  格式：test{.txt,.bin,.html,.js,.css}

  

扩展（正则）

  格式：[]{n，m}

​    中括号内的字符一共出现了n到m次

  ():将里面的内容组合看待

\```

 

## 十一、转义字符

 

\```

### \

 将特殊字符还原成文本

 \# 创建带空格的文件"real test"

   格式：touch real\ test

### ''

 划定一个范围作为文本

 格式：echo '\>'

 

### ""

 划定一个范围作为文本，但会执行内部的特殊字符

 格式：printf "string\n"

 

### ``

 执行反引号中的命令（系统命令）

\```

 

 

 

## 十二、环境变量

 

\```

变量

 存储数据的空间

 \#定义：

   m=3   echo $m

### export

 显示declare定义的变量

 格式：export m=5

   定义了一个在当前bash和子bash下的变量。

### env(enviroment)

 查看当前用户可使用的环境变量

### set

 查看当前环境所有的变量

### unset

 取消/删除一个变量

 

### readonly

 定义一个无法修改的变量，无法用unset来删除

 

\#小技巧：

   可以自定义一个环境变量文件，并通过配置文件调用source自动加载

 

\# 永久（permanent）

全局配置：/etc/bashrc  /etc/profile

针对用户的环境配置：~/.bashrc   ~/.bash_profile

\# 用户的环境变量优先于全局变量（自定的用户环境变量要在if语句下面

### nano

 文本编辑器

\```

 

### 环境变量练习一

 

**```shell**

**#** **第一步：使用nano打开全局变量的配置文件**

**#** **第二步：在最后一行添加一条变量的定义，如：**

 **n=4**

 **[root@localhost ~] . /etc/bashrc**

**#** **第三步：使用su切换到任一用户，输出变量n的值，如：**

 **[root@localhost ~] echo $n**

 **4**

 **[root@localhost ~]**

**#** **第四步：使用nano打开环境变量的配置文件，如：**

 **[root@localhost ~] nano .bashrc**

**#** **第五步：在最后一行添加一条变量的定义，如：**

 **n=5**

 **[root@localhost ~] . .bashrc**

**#** **第六步：查看n的值，如:**

 **[root@localhost ~] echo $n**

 **5**

 **[root@localhost ~]**

 

**原因：**

 **由于配置了两个文件，.bashrc文件顺序执行，通过if语句调用了/etc/bashrc，实际上做了两次赋值操作（n=4 n=5）,后一个赋值覆盖了前一个。**

**#** **思考**

 **如果将/etc/bashrc中的变量设置为只读（readonly）会发生什么？**

 

\```

 

 

 

## 十三、拼接符

 

\```

### 分号;

  格式：Cmd1;Cmd2;Cmd3  依次执行命令1、2、3

### &

  格式：Cmd1&Cmd2 同时执行命令1、2

 

 

 

 

## 十四、Vim命令介绍

**----****支持模态编辑**

 

### 一、普通模式

| h j k l   | 左 下 上 右         |

| ----------- | ---------------------------- |

| nyy     | 复制n行（yy 复制一行）   |

| p      | 粘贴             |

| d      | 一直往下删（按住）      |

| ndd     | 删除n行（dd 删除一行）    |

| nu     | 撤销上n步          |

| Ctrl + R  | 取消撤销           |

| gg     | 回到文档开头         |

| G      | 移动到尾行          |

| x      | 删除字符           |

| 0/$     | 行首/行尾          |

| ZZ     | 有改动保存退出，否则直接退出 |

| Ctrl + F/B | 向下/上翻页         |

| .      | 重复操作           |

| **>>  <<** | 增加/减少缩进        |

 

 

 

 

 

### 二、插入模式

 

**如何进入插入模式**

 

| i  I   | 光标前 /第一个非空字符处  插入 |

| ---------- | ------------------------------- |

| a A    | 光标后 /行尾  插入       |

| o O    | 另起下一行/上一行  插入    |

| Ctrl + T/D | 增加/减少缩进          |

 

 

 

### 三、命令行模式

 

| :w      | 保存                |

| ------------- | ----------------------------------- |

| :q      | 退出（:q ! 强制退出）        |

| :e!      | 回到文件的初始状态         |

| /字符     | 搜索字符（n 下一个  N 上一个）  |

| :set     | 设置 行号、语法高亮、主题颜色、鼠标 |

| :nohls    | 取消搜索高亮            |

| :%s/old/new/g | 替换所有的old为new         |

 

### 四、可视化模式（v）

 

### 五、替换模式（r/R）

 

 

### 六、Ex模式（Q）

 

| **visual** | 退出Ex模式 |

| ---------- | ---------- |

 

## 十五、Vim的操作

 

### 1、更改主机名称

 

\``` 

\# 需要修改三个配置文件

/etc/hostname

  更改主机名为 local

/etc/sysconfig/network

  添加 HOSTNAME=local

/etc/hosts

  将localhost.localdomain 改成 local

  

 

 

\# 查看主机名

hostnamectl

 

\# NAT模式需要修改

/etc/resolv.conf

  删除search localdomain这一行

\```

 

### 2、更改 yum 源

 

\```shell

/etc/yum.repos.d/CentOS-Base.repo

 

\# 修改配置文件，配置阿里云

[base]

name=CentOS-$releasever - Base - mirrors.aliyun.com

failovermethod=priority

baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/

 

yum clean all

yum makecache

 

yum list all 

yum search gcc

\# 查看系统中是否下载了wget

rpm -q wget

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

 

\# 下载

yum install 

  -y 自动选择yes

\# 删除

yum remove

  autoremove # 删除依赖项  （最好不要用-y）

 

\```

 

### 3、配置静态IP

 

```
\# 配置文件

/etc/sysconfig/network-scripts/ifcfg-ens33

 

BOOTPROTO=dhcp/static/none # 协议类型

默认路由：当数据包没有指定具体的地址时，那么发送给默认路由 0.0.0.0

ONBOOT=yes/no

  启动网络时，是否开启此块网卡  （ifup/ifdown 启用/停用指定网卡）

 

\# 配置静态IP

IPADDR1=192.168.95.10

NETMASK1=255.255.255.0 # PREFIX=24

GATEWAY=192.168.95.2

DNS1=

DNS2=

 

\#重新启动网络服务

systemctl restart network

start 开启服务 stop 停止服务  status 查看服务的状态
```

## 十六、Shell脚本

 

格式：Shebang

 

\#!  (Sharp bang)

Shebang也叫做 Shabang 、Hashbang

告诉系统使用的解释器是哪一个（绝对路径）

 

特点：弱类型的语言（不需要去指定数据类型）

 

脚本的使用：

 

\``` 

bash ./Less_1.sh

./Less_1.sh   # 要有可执行的权限

. ./Less_1.sh

source ./Less_1.sh

 

\# bash和直接执行会调用子shell环境

\# source和. 直接运行在当前shell环境

  $SHLVL

\```

 

### 1、基础命令

 

\```shell

\# 变量定义

name=1 # 除了下划线，不能有特殊字符，数字不能作为首位

read

  从键盘读取输入并存到变量中

  -p 给出提示信息

  格式：read -p "请输入你的名字" name

  echo "$name"  # 输出变量

\```

 

### 2、参数变量

 

man bash

 

通过 /Special Parameter 来查看

 

#### 1. 位置参数

 

\```shell

$0:代表了脚本文件本身

$1...:代表了传递给脚本的参数

\```

#### 2. 特殊变量

 

\```shell

\#在没有用双引号引起的时候 $@ 和 $* 没有任何区别

$* # 将所有传入的参数看作一个整体

$@ # 将所有传入的参数分别看待

$$ # 当前shell的进程ID

$- # 当前的shell选项（himBH）

$? # 上一次命令执行结果，0代表执行成功，1~255代表未成功

\```

 

### 3、逻辑语句

 

逻辑语句就是判断、比较字符串、数值和文件的语句，它一般包含在中括号 ( [ ] ) 内。

 

注：双小括号 (( )) 内的语句可以是C语言的语句

 

#### 1. 文件

 

\```shell

-f  判断文件是否存在

-d  判断目录是否存在

\```

 

#### 2. 逻辑

 

\```shell

&&  与 # 前一条执行失败，则不再继续执行

||  或 # 执行成功则不继续执行

!   非 # 取反

== 比较 # 比较等号两边字符串是否相等

-a  与 # 相当于与（and）

-o  或 # 相当于或（or）

\```

 

#### 3. 比较

 

| -eq | 等于   | -ne | 不等于  |

| ---- | -------- | ---- | -------- |

| -lt | 小于   | -gt | 大于   |

| -le | 小于等于 | -ge | 大于等于 |

 

### 4、if 语句

 

判断语句并执行 if 内的命令

 

\```shell

\# 单分支

if [];then

fi

\# 双分支

if [];then

else

fi

\# 多分支

if [];then

elif [];then

elif [];then

fi

\```

 

### 5、for 语句

 

\```shell

\# 循环

range={1..10}

for i in $range

do;done

 

for i in `seq 10`

do;done

\```

 

### 6、while语句

 

\```shell

while [ ]

do

 

done

 

 

break 退出循环

continue 继续循环

exit 退出shell

 

\```

 

### 7、Case语句

 

\```shell

\# 在进行选项选择时使用

case i in

num1)

;;

num2)

;;

*)

;;

esac

\```

 

### 8、计划任务

 

定期执行的命令，分为一次性计划任务和周期计划任务

 

一次性计划任务：at

```shell
\# 格式

at 时间 命令

时间：相对时间 绝对时间

  \#绝对时间

  格式：时:分 年-月-日

  \#相对时间

  绝对时间 + 偏移量 偏移单位

atq：查看当前的计划任务
```



```shell
\# 示例

at 16:11

at>echo 222

Ctrl+D

\# 过两分钟执行任务

at now+2 minutes
```

 

注：未指定输出的情况下，默认发送输出到当前用户的邮件池中，即/var/spool/mail/kaven

 

周期计划任务：crontab

 

```shell
/etc/crontab  # 配置文件

 

crontab -e   # 编辑crontab文件

crontab -l   # 查看当前用户的crontab文件

crontab -r    # 删除工作

格式：* * * * * 用户 命令 （分、时、天、月、周）
```

### 9、脚本实操

 

#### 1. 简单脚本

 

\```shell

\# 一个简单的脚本

 

\#!/bin/bash

read -p "请输入你的名字: " name

read -p "你的ID是：" ID

echo "你的名字是，${name}，ID是$ID"

 

\# 大括号用来划分变量的区域

\```

 

#### 2. if 语句

 

 

\```shell

\#!bin/bash

read -p "请输入您的成绩：" score

 

if [ $score -lt 60 ];then

​    echo "不合格"

  elif (($score >= 60 && $score <= 85));then

​    echo "良好"

  elif [ $score -gt 85 ];then

​    echo "优秀"

fi

\```

 

#### 3. while语句

 

\```shell

\#!/bin/bash

read -p "猜一猜我想的数字是什么？" num

random=${RANDOM:1:2}

while [ $num -ne $random ]

do

  if [ $num -gt $random ];then

​    echo -e "\033[41;37m 猜大了！\033[0m"

​    read -p "再猜一猜:" num

​    continue

  else

​    echo "猜小了！"

​    read -p "再猜一猜:" num

​    continue

  fi

done

echo "猜对了！恭喜！！"

 

\```

 

#### 4. case语句

 

\```shell

\#!/bin/bash

echo "请选择以下任意一个选项："

echo "1.显示用户名"

echo "2.显示密码"

read -p "请输入... " i

 

case $i in

1)

  echo `whoami`

  ;; 

2)

  tmp=`whoami`

  text=`sed -n "/$tmp/p" /etc/shadow`

  echo $text > test

  echo `cut -d ":" -f 2 test`

  ;; 

*)

  echo "$0"

  ;; 

esac

\```

 

 

### 练习作业

 

shell实现自动化配置静态/dhcp

 

基础命令：

 

| sed -i "s/old/new/g" 文件名     | 替换文件中的old字符为new   |

| ------------------------------------ | ----------------------------- |

| **sed -i "${var1},${var2}d" 文件名** | **删除文件的第var1行到var行** |

 

**目的**：熟悉case并写出满足需求的shell自动化脚本

 

子项一：备份网卡配置文件为 ifcfg-ens33.bk到/etc/sysconfig/network-scripts/backup文件夹中

 

子项二：包含恢复、静态ip、dhcp、退出的功能

 

  恢复：通过备份文件 ifcfg-ens33.bk 文件恢复（cp）

 

  静态ip：if条件判断是否存在IP，通过echo重定向（>>）实现

 

  dhcp：sed替换

 

  退出：exit

 

## 十七、文件及用户权限

 

### 1、 用户和组

#### 1.查看用户与组的信息

```shell
# 配置文件 
/etc/passwd 	# 用户的基本信息（Uid，Gid，家目录，shell）
/etc/shadow		
	# 用户的密码，用户修改时间，最小最大期限，警告日期，宽限日期
	# 密码的位置为空，则代表可以无密码登录，为!!则无法登录
	# 时间戳（1970年1月1日）
/etc/group		# 最后接的是组内的用户

# 查看用户ID信息
id
	-a 列出所有信息
	-Gn 查看用户所在的组（主组和附加组）
# 查看当前登录的终端用户
who
[root@js ~]# who
root     :0           2023-09-14 03:52 (:0)
root     pts/0        2023-09-14 04:21 (:0)
root     pts/1        2023-09-14 04:23 (192.168.179.1)

这是一个Linux系统命令。'who'命令用于显示当前登录的用户信息。在输出中，每一行代表一个已登录的用户，各字段的含义如下：",
"字段解释": {
"用户名": "当前登录的用户名。",
"终端类型": "用户登录的终端类型，其中':0'表示本地登录(GUI会话界面)，'pts/0'、'pts/1'等表示虚拟终端，IP地址表示通过网络登录。",
"登录时间": "用户登录系统的时间。",
"登录来源": "用户从何处登录系统，':0'表示本地登录，IP地址表示通过网络从该IP地址登录。"

w
'w'命令用于显示当前登录的用户及其进程信息。在输出中，第一行显示了系统的运行时间、当前登录的用户数以及系统的平均负载。接下来的行显示了每个登录用户的信息，各字段的含义如下：",
"字段解释": {
"USER": "当前登录的用户名。",
"TTY": "用户登录的终端类型，其中':0'表示本地登录，'pts/0'、'pts/1'等表示虚拟终端，IP地址表示通过网络登录。",
"FROM": "用户从何处登录系统，':0'表示本地登录，IP地址表示通过网络从该IP地址登录。",
"LOGIN@": "用户登录系统的时间。",
"IDLE": "用户空闲的时间。",
"JCPU": "用户进程在系统中运行的总时间。",
"PCPU": "用户进程在系统中运行的当前时间。",
"WHAT": "用户当前正在运行的进程。"
}

who am i	# 只列出当前使用的用户

# 查看用户信息
whoami

# 查看用户所在的组（主组和附加组）
groups
```

**/etc/shadow**

1. **用户名**：这是用户的登录名，用于标识用户账号。
2. **密码字段**：这个字段通常是用户密码的哈希值，或者可以是特殊字符，如 `*` 表示账号被锁定，或 `!` 表示密码已过期。
   1. `$1$`：MD5 加密。这是使用 MD5 算法对密码进行哈希的标识符。
   2. `$5$`：SHA-256 加密。这是使用 SHA-256 算法对密码进行哈希的标识符。
   3. `$6$`：SHA-512 加密。这是使用 SHA-512 算法对密码进行哈希的标识符。
   4. `!!`：密码已被禁用，用户不能登录。
   5. `!`：密码已过期，用户需要更改密码。
   6. **空白**：表示没有密码，用户可以登录但没有密码验证。
   7. `*`：表示账户被锁定。
3. **上次修改密码的日期**：表示自 1970 年 1 月 1 日以来的天数的数字，表示用户上次修改密码的日期。
4. **密码最小生存天数**：用户修改密码后需要等待的天数，通常是密码有效期。
5. **密码最大生存天数**：用户需要再次修改密码的天数。
6. **密码需要更改前的提前通知天数**：密码过期前多少天开始提醒用户需要修改密码。
7. **密码过期后的宽限天数**：如果用户的密码过期，这个字段指示用户仍然可以登录系统的天数，但要求立即更改密码。
8. **保留字段**：这个字段目前没有特定的用途，通常是空白。
9. **预留字段**：这个字段目前没有特定的用途，通常是空白。

**/etc/passwd**

```
/etc/passwd文件是Linux/UNIX系统中的关键文件之一，用于记录和存储用户信息。该文件中的每行代表一个用户，每行包含多个字段，这些字段之间用冒号（:）分隔。各字段的含义如下：

登录名：用户的登录名。
加密后的口令：用户的加密密码，但在实际应用中，该字段会被替换为“*”或“x”，表示密码被加密存储在/etc/shadow文件中。
UID：用户ID，是一个唯一的整数值，用于标识用户。
GID：用户组ID，是一个唯一的整数值，用于标识用户所属的组。
USERINFO：系统管理员想要记录的关于该用户的任何信息。
HOME：用户的主目录，即用户登录系统后默认的工作目录。
SHELL：用户登录后将执行的shell，通常为/bin/sh，如果为空格则表示缺省为/bin/sh。
除了用户登录名和加密密码之外，其他字段的值都可以由系统管理员进行修改和定制。
```



#### 2.用户与组的增删改查

```shell
#创建/删除用户
useradd 用户名
	-p 创建时指定密码（明文的）
	-d 创建时指定家目录
userdel 用户名
	-r 删除家目录以及邮件池
#创建/删除组
groupadd 组名
groupdel 组名

# 修改用户/组
usermod
	-g 修改用户的主要组（usermod -g 组 用户）
	-aG 给用户添加一个附加组（usermod -aG 组 用户）
	-u 修改用户的uid
	-L 锁定用户
	-U 解锁用户
groupmod
	-g 修改gID
	-n 重命名组（groupmod -n 新名字 原有组名）

# 临时修改用户登录的组
newgrp
	格式：newgrp 组名
```

 

#### 3.口令修改

```shell
passwd
# 用法一：修改密码或给刚创建的用户添加密码
passwd 用户名

# 用法二：锁定解锁用户
passwd -l		# 锁定用户密码
passwd -u		# 解锁用户密码

# 用法三：取消用户的登录密码
passwd -d		# 用户可以无密码登录 用户的锁定状态会消失
```



### 2、文件权限

 UGO与RWX

 角色：User，Group,Others

 权限：Read，Write，eXecute（可执行）

#### 1、普通权限

 

```shell
ll		# 查看文件的权限以及其他信息
-rw-r--r--. 1 user group size month day time file

# 第一位的 - 文件，d 文件夹，l 符号链接
# 1 代表着文件夹数目
# . 代表着没有开启ACL访问控制模型

chmod 	# 更改文件/目录的权限
	-R 递归更改权限
# 方法一：
	+ 添加权限（ chmod ugo+rwx ）
	- 删除权限（ chmod ugo-rwx ）
	= 修改权限（ chmod ugo=rwx ）
# 方法二：（ chmod 755 file ）
r：100	4
w：010	2
x：001	1
# 方法三：
umask
权限掩码：根据掩码的值让用户创建文件时分配不包含掩码的权限
例：umask 022
	创建文件不包含 w 的权限（临时的）
例：修改/etc/bashrc中的umask

chown	# 更改文件的所有者以及所属组
# 需要root权限
	-R 递归的去更改
	格式: chown user:group file
```

#### 2、特殊权限

suid(s)、sgid(s)、stichy(t)

suid    # 让其他用户运行文件时用的是所有者的身份（临时）

sgid    # 让用户在文件夹下创建的文件的组变为其所属组

stichy   # 只允许所有者修改文件

 **SUID（s）**

​	当一个可执行文件被设置了SUID权限后，该文件将以文件所有者的身份执行，而不是以实际调用该程序的用户的身份执行。

例如：

```bash
chmod u+s /usr/bin/passwd
```

**SGID（s）**

​	与SUID类似，SGID权限影响的是文件的组权限。当一个文件具有SGID权限时，该文件以其所属组的身份执行。

例如：

```bash
chmod g+s /usr/bin/wall
```

**Sticky Bit**

​	Sticky Bit通常用于目录。在设置了Sticky Bit的目录中，只有文件的所有者或者root用户能删除或重命名文件。

例如：

```bash
chmod o+t /tmp
```

**数字表示法**

- SUID：4
- SGID：2
- Sticky Bit：1

这些数字可以添加到三位数的权限代码前面，例如：

```bash
chmod 4755 文件名  # 设置SUID
chmod 2755 文件名  # 设置SGID
chmod 1755 目录名  # 设置Sticky Bit
```

#### 

 

#### 3、文件的隐藏属性

\# 查看文件的隐藏属性

**lsattr**

 文件的隐藏权限用ls -[是无法查看的，查看隐藏权限需要用到lsattr命令格式;lsattr [adR]文件/目录
命令的选项参数如下:
-a: 将隐藏文件的属性显现出来
-d:查看目录本身的权限，而不是目录内的文件
-R;连同子目录的属性信息一并显现出来

\# 修改文件的隐藏属性

**chattr**

  -R # 递归修改属性

​      +i # 让文件不可修改，目录不可添加删除文件

​      +a # 只允许追加内容

​      +A # 锁定访问时间

`chattr =i 文件名`

#### 4、ACL

ACL（访问控制列表），即在客体（文件）上添加一个列表，指定哪些用户对它有什么权限

\```

```shell
getfacl   # 查看文件的ACL

格式：getfacl [选项] 文件名

-a, --access：仅显示文件的访问控制列表。
-d, --default：仅显示默认的访问控制列表。
-c, --omit-header：不显示注释标题。
-e, --all-effective：打印所有有效的权限。
-E, --no-effective：不打印有效的权限。
-R, --recursive：递归进入子目录。
-t, --tabular：使用表格输出格式。
-n, --numeric：以数字形式打印用户/组标识符。

setfacl  # 修改设置文件的ACL权限

setfacl [选项] 文件名

-m, --modify=acl：修改文件（们）的当前ACL。
-M, --modify-file=file：从文件中读取ACL条目进行修改。
-x, --remove=acl：从文件（们）的ACL中移除条目。
-X, --remove-file=file：从文件中读取ACL条目进行移除。
-b, --remove-all：移除所有的扩展ACL条目。
-k, --remove-default：移除默认的ACL。
--set=acl：设置文件（们）的ACL，替换当前的ACL。
--set-file=file：从文件中读取ACL条目进行设置。
--mask：重新计算有效权限掩码。
-n, --no-mask：不重新计算有效权限掩码。
-d, --default：操作应用于默认的ACL。
-R, --recursive：递归进入子目录。
--restore=file：恢复ACL（与getfacl -R 相反）。
--test：测试模式（不修改ACL）。

\# 修改ACL

格式：setfacl -m ug：ug：rwx file

​    \# 删除指定的ACL项

​    格式：setfacl -x ug:ug file

​    \# 删除所有ACL配置

​    格式：setfacl -b file
```

 

#### 5、su和sudo

​      su：切换用户

​       \# 输入的是要切换用户的密码

​       \# root使用su来切换，不需要密码

​           -：切换时更改路径和环境变量

​           -c:使用某用户的权限执行命令

​           \# 格式：su user1 -c “ps -ef”

​       sudo   #使用root权限执行命令

​       \# 输入的是当前用户的密码

​       格式：sudo cmd

​       -i 以root身份登录

​       

​       配置文件：/etc/sudoers

​       添加：user1 ALL=（ALL） ALL

## 十八、存储结构以及管理硬盘

[存储结构与管理硬盘](https://blog.csdn.net/Roy_Allen/article/details/121527588?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169396312116800180641668%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=169396312116800180641668&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-121527588-null-null.142^v93^chatsearchT3_1&utm_term=linux%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84%E4%BB%A5%E5%8F%8A%E7%AE%A1%E7%90%86%E7%A1%AC%E7%9B%98&spm=1018.2226.3001.4187)

### 1、磁盘的挂载使用

设备主要有块设备（b）和字符设备（c）

块设备：以块为单位的硬盘设备   字符设备：以字符为单位的终端设备

lsblk -f # 查看磁盘分区、文件系统、格式化和挂载情况

blkid    # 用于查询系统中已挂载的块设备的属性

free -h  # 用于显示系统中空闲、交换分区和已使用的内存量

df -h   # 用于检查系统中每个文件系统磁盘空间的可用空间和已用空间，以及文件系统的挂载点等其他信息。

fdisk -l   用于显示指定磁盘设备的分区表信息

 

#### 1、分区

##### 1.MBR

​    仅支持4个分区，或3个主分区和1个扩展分区，扩展分区可以继续分为逻辑分区，大小不超过2TB

 

​              \# MBR 的分区工具

​              fdisk

​              fdisk格式：fdisk /dev/sdb

​              命令操作：

​                     m：查看帮助手册

​                     n：添加一个新的分区

​                     p：查看当前分区状态

​                     d：删除一个分区

​                     w：保存

​                     q：不保存退出

​              操作顺序：n --> p --> 1 --> 默认 --> 1G --> p --> w

##### 2.GPT

​              理论上支持128个分区

​              \# GPT 分盘工具

​              parted

​              格式：parted /dev/sdc

​              命令操作：

​                  help：查看帮助手册

​                  mklab：改变分区类型（gpt、msdos）

​                  mkpart：创建分区

​                  rm：删除创建的分区

​                  rescue：恢复分区（知道起始点和结束点）

​       

 

#### 2、格式化

​           \# 格式化磁盘，不格式化没法挂载

​           mkfs

​           格式：mkfs -t ext4 /dev/sdc1

​           格式：mkfs.xfs /dev/sdb1

#### 3、挂载

要使用设备中的数据需要先将其与文件系统中一个已存在的目录关联，这就是挂载

eg:

```bash
# 将设备挂载到backup目录下
mount /dev/sdb2 /backup
# 这个命令有一些参数
-a 	# 挂载所有在/etc/fstab中定义的设备，开机会自检这个文件
-t	# 指定挂载的文件系统类型，一般会自动识别
```

- 为了永久挂载，就要在刚才提到的fstab文件中按格式指定，各字段如下表格
- 开机会自检这个文件，没mount的都会自动挂载
  ![3](https://img-blog.csdnimg.cn/88f26aa72e2b4f35953a0e783f3c6698.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAUm95X0FsbGVu,size_20,color_FFFFFF,t_70,g_se,x_16)

  

​         挂载上的磁盘无法分区

​           文件可以挂载到多个目录

​           \# 挂载磁盘到目录下

​           mount

​           格式：mount /dev/sdb1 /mnt/disk1

​              -o 指定mount可以使用的选项

​                  -o ro 以只读模式加载

​                  -o rw 以可读写模式加载

​                  -o remount 再次挂载

​                     -o remount,rw 再次挂载，以可读写模式加载

​           用法:mount |grep sdb

​           

\# 取消目录下的磁盘挂载

umount

格式：umount /dev/sdb1 解除挂载的目录

​     umount /mnt/disk1/file 解除挂载的文件

配置文件：/etc/fstab

/dev/sdb1   /mnt/disk1 xfs defaults 0 0

### 2、交换分区

​       作用：扩展虚拟内存，将不用的进程放到分区里，从而节省磁盘空间，调用更多进程

 

\# 查看内存和交换分区的使用情况

​       free

​       

​       \# 设置设备为交换空间

​       mkswap /dev/sdb5

​       

​       \# 启用交换分区

​       swapon /dev/sdb5

​       

​       \# 移除交换分区

​       swapoff /dev/sdb5

 

也可以通过文件创建交换分区

​       dd if=/dev/zero of=swap bs=1G count=1

​       mkswap swap

​       

\#设置交换文件的权限

​       chmod 0600 swap

​       swapon swap

​       free

​       swapoff swap

 

#### 练习

一、创建一个xfs的分区

二、挂载，并在分区中创建一个文件

三、将文件设置为交换文件

四、启动交换文件

五、free -h查看是否成功

六、把交换分区回到初始状态

 

练习2.

使用parted恢复删除的分区（分区需要格式化才能恢复）

 

 

### 3、磁盘配额

磁盘配额（quota）：在磁盘给用户或租配额分配可以供用户或组使用的文件大小和数目。它分为软配额和硬配额，并根据**块（block）**和**节点（inode）**进行配额

 常用的文件系统是Ext4（RHEL6默认）和XFS（RHEL7/8默认）

​	XFS支持高达18EB的存储容量，Ext4支持1EB存储容量

#### 1、xfs

\# 查看是否有xfs配额工具并下载

rpm -q quota xfsprogs

yum -install xfsprogs

 

\# 对用户进行配额

xfs_quota -x -c “limit -u bsoft=1M bhard=2M isoft=3 ihard=5 user1” /mnt/disk1

 

\# 对组进行配额

xfs_quota -x -c “limit -g bsoft=1M bhard=2M isoft=3 ihard=5 user” /mnt/disk1

 

\# 查看配额状态

xfs_quota -c “quota -ib -uv user1” /mnt/disk1

xfs_quota -c “quota -ib -gv user” /mnt/disk1

 

 

#### 2、ext4

```
    \# 检查支持quota磁盘并初始化

quotacheck -a检查所有

​         -c 初始化

​           -g 创建aquota。group

​           -u 创建aquota。user

\#启动配额

guotaon 

-a 启动所有

\# 查看配额文件

edquota

-u 查看用户 -g 查看组

-p 拷贝用户的配额信息到其他用户

格式: edquota -p user1 -u user2

\# 关团配额

quotaoff 

-a 关闭所有
```



### 4、软硬链接

链接（Link）它指向一个文件，分为硬连接和软链接

硬链接：在Linux系统中复制了一份一模一样的文件，修改时同时对这两个文件进行修改

 

硬链接在文件删除时不会失效

 

软链接（符号链接）：创建了一个指向文件的快捷方式，存储的是文件地址，同时创建了一个新的inode

 

​    ln

​       ln默认创建硬链接

​       格式：ln 源文件 硬/软链接文件

​       -s 创建符号链接

## 十九、RAID

### 1、RAID5

廉价磁盘冗余阵列，Linux中通过mdadm工具来创建、备份、添加、损坏、移除操作，可以创建的类型有Raid0，1，5，6，10

 

\#RAID5 阵列创建

mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sd[b-e]1

注：-C: 创建一个新的RAID设备。

-v: 显示创建RAID设备的详细信息。

/dev/md0: 指定创建的RAID设备的设备文件 路径。

-n 3: 指定阵列中的物理磁盘数量。

-l 5: 指定RAID级别，这里为RAID 5。

-x 1: 指定镜像条带的数量，这里为1。

/dev/sd[b-e]1: 指定参与创建RAID设备的物理磁盘设备文件路径，这里为/dev/sdb1、/dev/sdc1、/dev/sdd1、/dev/sde1

\# 查看创建的阵列

mdadm -D /dev/md0

\# 挂载

mount /dev/md0 /mnt/raid5

\# 模拟损坏磁盘

mdadm -f /dev/md0 /dev/sdb1

\# 移除并添加磁盘

mdadm -r /dev/md0 /dev/sdb1

mdadm -a /dev/md0 /dev/sdb1

\# 停止磁盘阵列

mdadm -S /dev/md0

 

### 2、RAID10

\#RAID10 阵列创建

mdadm -Cv /dev/md1 -l 10 -n 4 /dev/sd[b-e]2

\# 查看创建的阵列

mdadm -D /dev/md1

\# 挂载

mount /dev/md1 /mnt/raid10

\# 模拟损坏磁盘

mdadm -f /dev/md1 /dev/sdb2

\# 移除并添加磁盘

mdadm -r /dev/md1 /dev/sdb2

mdadm -a /dev/md1 /dev/sdb2

 

 

 

### 3、删除RAID阵列

​    \# 卸载设备

umount /dev/md0

​    

mdadm -S /dev/md0     # 停用RAID设备

 

\# 清除超级块（记录了文件系统中数据的属性等信息）

​    mdadm --misc --zero-superblock /dev/sd[b-e]1

​           注：misc（擦除）  --zero-superblock（超级块归零）

​    

​    \# 删除配置文件

​    rm -rf /etc/mdadm.conf

​    最后要查看/etc/fstab

 

 

 

 

 

## 二十、firewall与iptables

防火墙作为公网与内网之间的屏障，Linux中提供了iptables和firewall的防火墙管理工具，用于配置策略（policy）和规则链（chain）

### 1、iptables

根据配置的策略规则，放行（ACCEPT）或阻止流量（数据包）进入、转发、流出

 

开启iptables服务，并清空原有的数据

\# 下载iptables-services

yum -y install iptables-services

 

\# 启动iptables

systemctl start iptables

 

\# 查看策略

iptables -L

 

\# 清空策略

iptables -F

 

\# 保存配置

service iptables save

 

#### 配置策略

\# 将入站流量默认为阻止

iptables -P INPUT DROP

 

\# 允许ICMP流量进入

iptables -A INPUT -p ICMP -j ACCEPT

 

\# 删除策略

iptables -D INPUT -p ICMP -j ACCEPT

 

\# 针对IP，协议，端口

iptables -A INPUT -p tcp -s 192.168.1.121 --dport 20:30 -j ACCEPT

​    注：-A INPUT：在 iptables 的 INPUT 链中添加一条规则。

-p tcp：只允许 TCP 协议的数据包通过。

-s 192.168.1.121：只允许来自 IP 地址为 192.168.1.121 的主机。

--dport 20:30：只允许到端口范围从 20 到 30 的数据包通过。

-j ACCEPT：如果数据包符合上述条件，则接受该数据包

\# 测试目标机器端口是否允许连接

telnet 192.168.136.100:22

 

### 2、firewalld

安装图形化界面：`yum install firewall-config`

打开图形化界面：`firewall-config`

firewalld配置了九个区域，并通过区域来过滤相应网卡的流量

配置文件路径：/etc/firewalld

​    主配置文件：firewalld.conf    区域配置文件：zones/external.xml

 

服务存放在/usr/lib/firewalld/services目录中，通过单个的xml配置文件来指定

***xml文件: service-name.xml

(1) --zone=区域名称--list-services显示指定区域内允许访问的所有服务

(2) --zone=区域名称--add-service=服务名称为指定区域设置允许访问的某项服务

(3) --zone=区域名称--remove-service=服务名称删除指定区域已设置的允许访问的某项服务

(4) --zone=区域名称 --list-ports显示指定区域内允许访问的所有端口号

(5) --zone=区域名称--add-port=端口号-端口号/协议名中间的-表示从多少到多少端口号，/和后面跟端口的协议。（放行端口）

(6) --zone=区域名称--remove-port-端口号端口号/协议名(中间的-表示从多少到多少端口号，/和后面跟端口的协议)

(7) --zone=区域名称 --list-icmp-blocks显示指定区域内拒绝访问的所有ICMP类型

(8) --zone=区域名称--add-icmp-block=icmp类型为指定区域设置拒绝访问的某项ICMP类型

(9) --zone=区域名称--remove-icmp-block=icmp类型删除指定区域已设置的拒绝访问的某项ICMP类型，省略 --zone=区域名称时表示对默认区域操作

#### 1.firewall-cmd

##### 基本配置

```shell
# 查看并设置默认区域

firewall-cmd –get-default-zone

firewall-cmd –set-default-zone external

 

# 更改网卡的区域

firewall-cmd --change-interface=ens33 --zones=externall --permanent

 

# 查看当前激活区域的网卡、协议等信息

firewall-cmd –list-all  

firewall-cmd --list-services

firewall-cmd --list-port

 

# 列出所有区域的信息

firewall-cmd --list-all-zones       # 列出指定区域（--list-all zone=public）

 

# 添加、删除协议、端口的信息

firewall-cmd --add-service=http **–permanent**

firewall-cmd --reload     # 永久配置需要重新加载配置文件

firewall-cmd --remove-service=http

 

firewall-cmd --add-port=8000-10000/tcp   # 添加端口范围

 

# 查看端口、服务、协议的状态

firewall-cmd --query-port=80/tcp  #查看端口

firewall-cmd --query-13service=http   # 查看服务

 

# 设置888端口转发到22端口

firewall-cmd --add-forward-port=port=888:proto=tcp:toport=22 --permanent
```

 

##### 富规则（rich-rule）

定义一组规则的集合，以xml的格式存储在配置文件中

```shell
# 拒绝目标主机使用连接本机的22端口

firewall-cmd --add-rich-rule=’rule family=ipv4 source address=目标主机ip port port=22 protocol=tcp drop' --permanent

# 拒绝目标主机使用ssh服务连接本机

firewall-cmd --add-rich-rule='rule family=ipv4 source address=目标主机ip servicename=ssh drop' --permanent

#删除是把add改成remove
```



##### 应急模式

​    开启后会将所有的包丢弃

​    firewall-cmd --panic-on      # 开启

​    firewall-cmd --panic-off      # 关闭

​    firewall-cmd --query-panic   # 查看

#### 2、firewall-config

\# 下载安装

yum -y install firewall-config

 

 

## 二十一、ssh

远程加密连接到目标主机，访问其资源并操作使用

服务端：openssh-server

客户端：openssh-clients

### 1、信息的传输

明文：不经过加密传输的信息，比如http协议传输的报文

 

密文：经过加密处理的信息

 

密钥：实现加密处理的一种算法

公钥：公开的密钥   私钥：私人的密钥

 

### 2、密钥算法

对称加密算法：公钥加密，公钥解密

非对称加密算法：公钥加密，私钥解密

### 3、网卡的配置

#### 1、nmcli

network(n)

​    connectivity(c)       #查看当前网络配置状态

​       full            # 所有internet访问

​       portal          # 网络不可达

​       unknown       # 找不到当前连接的状态

​       limited        # 网络已连接，但无法访问internet

​       null           # 网络无法连接

​    on/off             # 启动/关闭NetworkManager，来决定是否接管了网络配置

 

 

connection(c)

​    show(s)            #查看当前网络配置信息

​    modify(m)          # 修改网络配置(+/-)

​    格式：nmcli c m ens33 (+/-) ……

​       ip.method   static/auto/manual

ip.address       # ip地址

​       ip.netmask      # 子网掩码

​       ip.gateway      # 网关

​       ip.dns          # DNS服务器

​    addcon-name ens40 ifname ens40 type ethernet autoconnect yes

device(d)

​    show(s)            # 查看设备信息

 

 

##### 双网卡绑定

​    将多个网络接口卡（NIC）绑定为一个逻辑接口，提供了更高的带宽和负载均衡

​    

​    

```
nmcli connection

​       \# 添加一块用于绑定的逻辑接口，生成配置文件

add type bond ifname bond1 mode active-backup

\# 添加网卡到逻辑接口

​       nmcli c a type bond-slave ifname ens33 master bond1

​       nmcli c a type bond-slave ifname ens36 master bond1

​       

\# 关闭正在连接的绑定网卡

down ens33        down ens36

 

\# 启动逻辑接口和网卡

​       up bond-bond1     up bond-slave-ens33     up bond-slave-ens36

​       

​       \# 添加静态IP

​       nmcli c m bond-bond1 ipv4.method manual ipv4.address 192.168.136.100/24 ipv4.gateway 192.168.136.2 ipv4.dns 192.168.136.2

​       nmcli c re

​       

​       \# 删除逻辑接口和网卡

​       down bond-bond1       down bond-slave-ens33                            down bond-slave-ens36

delete bond-bond1      delete bond-slave-ens33

delete bond-slave-ens36

 

up

​       down

​       delete
```

 2.lsmod |grep bonding

3.手动加载bonding模块：

```shell
echo "modprobe bonding" >> /etc/sysconfig/modules/bonding.modules
cat /etc/sysconfig/modules/bonding.modules
chmod 755 /etc/sysconfig/modules/bonding.modules
reboot
```

4.cd /etc/sysconfig/network-scripts

ls |grep ifcfg

cp ifcfg-ens33 ifcfg-bond0



TYPE=Bond

PROXY_METHOD=none

BROWSER_ONLY=no

BOOTPROTO=static

DEFROUTE=yes

IPV4_FAILURE_FATAL=no

IPV6INIT=yes

IPV6_AUTOCONF=yes

IPV6_DEFROUTE=yes

IPV6_FAILURE_FATAL=no

IPV6_ADDR_GEN_MODE=stable-privacy

NAME=bond0

DEVICE=bond0

ONBOOT=yes

IPADDR=192.168.100.10

NETMASK=255.255.255.0

GATEWAY=192.168.100.2

DNS1=8.8.8.8

DNS2=114.114.114.114

NM_CONTROLLED=no

BONDING_MASTER=yes

BONDING_OPTS="mode=1 miimon=100"



模式选定
绑定网卡的模式一共有七种
模式0：轮询策略（round robin），mode=0，
优点：流量提高一倍
缺点：需要接入交换机做端口聚合，否则可能无法使用
特点：增加了带宽，同时支持容错能力，当有链路出问题，会把流量切换到正常的链路上

模式1：主备策略（active-backup），mode=1，
只有主网卡处于工作状态，备网卡处于备用状态，主网卡坏掉后备网卡开始工作，提供容错能力。
优点：冗余性高
缺点：链路利用率低，两块网卡只有1块在工作
不需要交换机端支持

模式2：异或策略（load balancing (xor)），mode=2
根据源MAC地址和目的MAC地址进行异或计算的结果来选择传输设备，提供负载均衡和容错能力。
需要交换机配置聚合口

模式3：广播策略（fault-tolerance (broadcast)），mode=3
将所有数据包传输给所有接口通过全部设备来传输所有数据，一个报文会复制两份通过bond下的两个网卡分别发送出去，提供高容错能力。
需要交换机配置聚合口

模式4：动态链接聚合（lacp），mode=4，按照802.3ad协议的聚合自动配置来共享相同的传输速度，网卡带宽最高可以翻倍，链路聚合控制协议（LACP）自动通知交换机聚合哪些端口，需要交换机支持 802.3ad协议，提供容错能力。

模式5：输出负载均衡模式（transmit load balancing），mode=5，输出负载均衡模式，只有输出实现负载均衡，输入数据时则只选定其中一块网卡接收，需要网卡和驱动支持ethtool命令。

模式6：输入/输出负载均衡模式（adaptive load balancing），mode=6，输入和输出都实现负载均衡，需要网卡和驱动支持ethtool命令。

mode5和mode6不需要交换机端的设置，网卡能自动聚合。mode4需要支持802.3ad。mode0，mode2和mode3理论上需要静态聚合方式。
#### 2、nmtui

​    nmtui

双网卡绑定

![image-20230907171902159](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230907171902159.png)

### 4、RSC密钥登陆

id_rsa私钥	id_rsa.pub公钥

yum install openssh-server

```shell
受害机
# 生成公钥和私钥，公钥：/root/.ssh/id_rsa.pub
ssh-keygen -t rsa		#一路默认
cat id_rsa > authorized_keys
攻击机
#然后将id_rsa内容复制到攻击机桌面,然后xshell连接，用户身份验证->方法里将public key勾上，点击连接，然后选择使用密钥登录，选上密钥，连接成功

# 把公钥传输到攻击机
ssh-copy-id -i /root/.ssh/id_rsa.pub root@攻击机ip

#在受害机.ssh目录下创一个config
Host centos7	#攻击机名
    Hostname 攻击机ip
    User root
    Port 22
    IdentityFile C:\Users\16159\.ssh\id_rsa		#私钥路径

#ssh远程连接主机
ssh 攻击机名
    -X 调用远程主机的图形化应用程序	#不要在win下运行
```



### 5、scp远程下载

```shell
远程后使用ssh协议传输文件

# 把文件从本机传输到远程主机
scp 受害机文件的路径 root@*主机IP*:/root/

# 把文件从远程主机传输到本机
scp root@*主机IP*:/root/test /root/test
```



## 二十二、Apache

Apache是一款Web服务器软件

​    安装：`yum -y install httpd`

`firewall-cmd --add-port=80/tcp`

`firewall-cmd --add-service=http`

目录及参数

| 作用         | 文件名称                   |                                |
| ------------ | -------------------------- | ------------------------------ |
| 服务目录     | /etc/httpd                 | 主要的配置文件存储在网站目录下 |
| 主配置文件   | /etc/httpd/conf/httpd.conf |                                |
| 网站数据目录 | /var/www/html              | 默认网站目录                   |
| 访问日志     | /var/log/httpd/access_log  |                                |
| 错误日志     | /var/log/httpd/error_log   |                                |



Linux系统中的配置参数

### 1、配置文件

Apache的配置文件存放在/etc/httpd目录下或网站的根目录下

#### **实验**：实现身份认证

```bash
# 修改主配置文件 /etc/httpd/conf/httpd.conf

AllowOverride AuthConfig
	# 允许网站目录下的配置文件覆盖（ALL），允许身份验证（AuthConfig）

# 生成root用户密码
htpasswd -c /etc/httpd/.htpasswd root

# 编辑配置文件 /home/www/html/.htaccess
  	AuthName “Apache” 
	AuthType Basic
    AuthUserFile /etc/httpd/.htpasswd
    Require valid-user

# 重新加载配置
systemctl reload httpd
```

 

### 2、SElinux

SElinux是一个一种细粒度更高的安全框架，由美国安全局（NSA），它基于MAC强制访问控制模型和RBAC角色访问控制模型来设计,通过安全上下文（context）来进行控制。有以下三种模式target、minium、MLS

安全标准：D C1 C2 C3 B1 B2 B3 A	#从小到大



ls -Z查看

system_u：object_r：http_sys_content_t：s0：[类别]

身份字段：角色：类型： 灵敏度：[类别]



#### **配置模式：**

 SELinux在运行时会在系统中创建一个安全上下文，为文件、进程和其他系统资源分配安全标签。当进程尝试访问受保护的文件或资源时，SELinux会检查该访问是否符合安全上下文规则，从而防止潜在的恶意行为。

`setenforce` 命令用于设置SELinux的执行模式。执行模式决定了SELinux是否对系统进行强制执行。执行模式有三种种：强制执行（Enforcing）和宽容执行（Permissive）和disabled

- 当执行模式为 Enforcing 时，SELinux 会强制执行安全上下文规则，阻止未经授权的访问。
- 当执行模式为 Permissive 时，SELinux 会记录所有违反安全上下文规则的访问，但不会阻止这些访问。这有助于管理员识别潜在的安全问题。
- disabled：对于越权的行为不警告也不拦截。

 

#### **实验**：修改httpd的默认网站目录

把var改成home

![image-20230908102857026](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230908102857026.png)

![image-20230908142614297](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230908142614297.png)



1.修改主配置文件中的网站目录为/home/www/html

2.查看用户的规则

```bash

# 查看所有的身份、角色、类型
seinfo -u 身份   -r 角色     -t 类型

# 查看指定类型的规则
search --all/allow/neverallow -s httpd_t -t httpd_sys_content_t
```

3.修改文件的安全上下文（类型）

```bash
# 修改默认的安全上下文
semanage fcontext *指定文件*
	-l 查看默认策略
	
# 临时修改
chcon
   -R 递归修改
   格式：chcon -R -t httpd_sys_content_t /home/www

# 恢复默认
restorecon
```

 

### 3、虚拟主机

 

Apache通过配置虚拟主机来实现多网站的访问，可以使用不同的ip、端口和主机名（域名）来访问不同目录下的网页。在主配置文件的最后一行指定了一个配置文件的目录：IncludeOptional conf.d/*.conf

 

配置文件：/etc/httpd/conf.d/virtual_host.conf

#### 1.根据IP进行配置

```
 NameVirtualHost *:80
 
 <VirtualHost 192.168.95.8:80>
    ServerName www.example.com
    DocumentRoot /home/www/html/example
 </VirtualHost>
 
 <VirtualHost 192.168.95.9:80>		#一个网卡可以有多个ip
 	ServerName www.test.com
	DocumentRoot /home/www/html/test
 </VirtualHost>

# 添加IP地址
nmcli c a ens37 ipv4.address 192.168.95.8/24 ipv4.gateway 192.168.95.2
nmcli c a ens37 ipv4.address 192.168.95.9/24 ipv4.gateway 192.168.95.2
或nmtui添加
```

#### 2.根据端口进行配置

```shell
 NameVirtualHost *:80
 Listen 8081
 Listen 8082
 
 <VirtualHost 虚拟机ip:8081>
    ServerName www.example.com
    DocumentRoot /home/www/html/example
 </VirtualHost>
 
 <VirtualHost 虚拟机ip:8082>
 	ServerName www.test.com
	DocumentRoot /home/www/html/test
 </VirtualHost>
```



#### 3. 根据主机名（域名）进行配置

```shell
 NameVirtualHost *:80
 
 <VirtualHost 主机ip:80>
    ServerName www.example.com
    DocumentRoot /home/www/html/example
 </VirtualHost>
 
 <VirtualHost 主机ip:80>
 	ServerName www.test.com
	DocumentRoot /home/www/html/test
 </VirtualHost>

# 修改/etc/hosts
主机ip www.test.com www.example.com
```



#### 4. 访问控制

​	对IP、主机名（域名）进行控制，限定用户访问网站

​	配置文件：/home/www/html/.htaccess

```shell
Require all granted # 允许所有的访问
Require all denied	# 拒绝所有的访问

# 限制指定IP访问
Require not ip 192.168.95.120

# 限制指定主机访问
Require not host root@192.168.95.132
```

#### 5. 个人主页

1. 新建文件夹：/home/用户名/public_html
2. 在其中添加一个index.html的文件并添加内容

3. 修改安全上下文为httpd_sys_content_t

4. 配置文件：/etc/httpd/conf.d/userdir.conf

​		添加/取消注释：UserDir public_html

4. 访问格式：http://192.168.95.10/~username

## 二十三、 docker

### docker介绍

docker三要素
镜像: image镜像是docker的前置条件，在docker中建立一个容器，就需要先准备一个镜像;
容器: container，Docker客器是类似于一个轻量级的沙箱子，Docker是基于Linux内核的虚拟技术，消耗资源非常小，Dokcer利用容器来进行运行和隔离应用，一个镜像可以有多个容器;
仓库: respostory，Docker仓库类似于代码仓库，是Docker集中存放镜像文件的场
所

为docker添加一个阿里云的镜像加速器（每个账户都有一个）

![image-20230908155325418](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\image-20230908155325418.png)

将{}这三行粘贴到daemon配置文件：/etc/docker/daemon.json



docker运行流程：

build构建

pull拉取

run运行

![bcbd6eaebc55c18f67bb5029c146127](C:\Users\16159\AppData\Roaming\Typora\typora-user-images\bcbd6eaebc55c18f67bb5029c146127.jpg)

C/S架构模式
Docker客户端:
用于和Docker守护进程 ( Daemon) 建立通信的客户端。Docker客户端只需要向docker服务器或者守护进程发出请求 (构建、拉取、启动等指令)，服务或守护进程将完成所有工作，并返回结果
1.橙色流程: 执行docker构建指令会根据docker文件构建一个镜像存放于本地主机2.蓝色流程: 执行docker拉取指令会从云端镜像仓库拉取镜像至本地主机，或将本地镜像推送至端镜像仓库
3.黑色流程:执行docker启动命令会将镜像安装至容器并启动容器

### **docker优势：**

1.轻量级和高效
Docker 利用容器化技术，可以在同一个操作系统内同时运行多个容器。Docker 容器更轻量级，因为它们共享主机操作系统的内核和系统库

2.快速部署
Docker 容器是通过镜像进行部署的，镜像包含了应用程序及其所有依赖项和配置。通过预先配置的镜像，可以快速部署和启动容器，并且容器的水平扩展也相对容易实现。

3.灵活性和可移植性Docker 容器可以在不同的平台和环境中运行，包括物理机、虚拟机、公共云和私有云等。容器提供了一致的运行环境，使得应用程序更易于在不同环境中进行部署和迁移

4.生态系统和社区支持
Docker 拥有庞大的生态系统和活跃的社区提供了许多现成的容器镜像和工具。可以从Docker Hub或其他镜像仓库中获取各种应用程序的官方或社区维护的镜像

### **docker安装：**

1、下载阿里云的EPEL源并保存到/etc/yum.repos.d/epel.repo文件中
2、更新本地yum软件包缓存
3、查找提供docker-compose命令的软件包
4、安装docker-compose

### **配置加速器：**

比较知名的是有网易蜂巢提供的
https://c.163yun.com/hub#/m/home/以及daocloud; http://hub.daocloud.io (推荐使用daocloud)

docker配置阿里云镜像加速
1.阿里云控制台搜索容器镜像服务
2.镜像工具中的镜像加速器
3.找到相应的系统版本
4.根据提示进行操作

### **docker的基本命令**

```bash
docker info
容器生命周期管理：
run: 创建一个新的容器并运行一个命令
start/stop/restart: 启动一个或多个已经被停止的容器重启容器
stop :停止一个运行中的容器
kill;杀掉一个运行中的容器。-s :向容器发送一个信号 (docker kill -s KILL mynginx)
rm: 删除一个或多个容器。-f:通过 SIGKILL 信号强制删除一个运行中的容器
pause/unpause: 暂停容器中所有的进程/恢复容器中所有的进程
create:创建一个新的容器但不启动它
exec: 在运行的容器中执行命令
docker exec -it 容器名 /bin/sh		#进入容器
docker exec -it 容器id /bin/sh		#进入容器

docker rm $(docker ps -aq)		#删除所有docker容器
docker run -it -d --name alpine --rm alpine:latest
docker cp /path/filename 容器id或名称:/path/filename		#把宿主机上的文件复制到docker容器内部
docker cp 容器id或名称:/path/filename /path/filename		#把docker容器内部的文件复制到本地


'--rm' 停止容器之后就删除该容器
'-it' 参数让Docker在新容器中启动一个伪终端(pseudo-TTY)，并绑定到用户的标准输入上，这就允许用户与容器进行交互。
'-d' 参数让容器在后台运行（即“分离模式”），并返回容器ID。
'--name alpine' 参数是用来给创建的容器设定一个名字，这里的名字是 'alpine'。
'-v /root:/root' 参数是设置卷(volume)，这里是将主机的 '/root' 目录挂载到容器的 '/root' 目录。
'alpine:latest' 是要运行的Docker镜像的名字和标签，这里使用的是 'alpine' 镜像的 'latest' 标签。
'-p 5000:5000' 是端口映射，将主机的5000端口映射到容器的5000端口。
```

### docker私有镜像仓库

```bash
docker run -it -d --name registry -p 5000:5000 -v /mnt/registry:/var/lib/registry registry:latest
这个命令是用来在Docker中运行一个容器，该容器用作私有Docker镜像仓库

"insecure-registries":["192.168.179.144:5000"]添加到/etc/docker/daemon.json

docker tag alpine:latest 192.168.179.144:5000/alpine:latest		#用于给一个已有的镜像打上新的标签。'alpine:latest'这个已有的镜像被打上了新的标签'192.168.179.144:5000/alpine:latest'

docker push 192.168.179.144:5000/alpine:latest	#目的是推送镜像到本地，之后再拉取镜像就会很快


#然后就可以试验一下本地拉取镜像了
docker rmi alpine		#先删除镜像
docker pull 192.168.179.144:5000/alpine		#实验从本地拉取镜像

#如何查看本地镜像
/mnt/registry/docker/registry/v2/repositories里有本地镜像
```



## 路径说明：

**/etc/profile**:配置永久命令

若配置完成想要立即执行：source /etc/profile

执行命令：1) . /etc/profile		2) source /etc/profile		3)bash /etc/profile

Linux根目录下的文件功能如下:

1 **/bin** 目录:存放二进制可执行文件

2.**/boot** 目录:存放用于系统引导时使用的文件

3.**/dev** 目录: 这个目录包含了Linux系统的所有外部设备，本质上是访问这些外部设备的端口（其实和访问一个文件夹没啥区别。）

4.**/etc** 目录: 存放系统级配置文件和系统服务的启动脚本

5.**/home** 目录:存放普通用户的主目录

6.**/lib**和**/lib64**目录:存放系统软件库文件

7.**/ost+found** 目录: 存放因系统意外关机或者发生其他问题而丢失的文件

8.**/media** 和**/mnt** 目录: 用于挂载外部设备或者其他文件系统

9.**/opt**目录:存放第三方软件的安装目录

10**./proc** 目录:存放系统内存映射的虚拟文件系统

11.**/root** 目录: 超级用户root 的主目录

12.**/run**目录:存放系统运行时期需要动态更新的文件

13.**/sbin** 目录:存放系统管理员使用的二进制命令文件

14.**/srv** 目录:存放系统服务提供的数据

15**/sys**目录:存放与系统硬件相关的信息和操作

16.**/tmp**目录:用于存放临时文件

17.**/usr**目录:存放用户级应用程序和文件

18**./ar**目录:存放系统运行过程中产生的文件和日志

19.**/cdrom**:光驱文件系统挂在这个目录下。（但是这个在刚安装系统的时候是空的）





## 其他命令

```
使history命令显示执行时间、用户
vim /root/.bashrc
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S  `who am i | awk '{print $1,$5}'` "

source .bashrc	#执行.bashrc文件
```

## 遇到的问题

### linux复制虚拟机后没网

#### [VMWare 虚拟机克隆或复制后 eth0 不存在，不能上网的解决办法](https://my.oschina.net/MIKEWOO/blog/1589393)

原创

[明MikeWoo](https://my.oschina.net/MIKEWOO)

[Linux服务器](https://my.oschina.net/MIKEWOO?tab=newest&catalogId=5704857)

2017/12/13 19:41

阅读数 2.2W

[![img](https://oscimg.oschina.net/oscnet/up-dd456b947d6db0f1833fd5df883c14fd80f.jpg)](https://www.oschina.net/group/devops)

本文被收录于专区

[DevOps](https://www.oschina.net/group/devops)

[进入专区参与更多专题讨论 ](https://www.oschina.net/group/devops)



##### 1. 遇到的问题

在 Vmware 装了 Centos，今天在启动的时候，发现找不到网卡 eth0, 在输入 ifconfig 的时候，也没有任何 Ethnet 的信息，我检查了 VMware 的 EtherNET 配置的时候，仍旧是 NAT 网络设置，和我原来的一样没有了网卡，我就无法通过 SecureCRT 来连接虚拟机。截图如下：

![img](https://static.oschina.net/uploads/space/2017/1214/094145_AVDF_1784946.png)



##### 2. 原因分析

如果 VM 虚拟机已经复制了，虚拟机会在开机时检查网卡物理地址是否已经存在，如果存在就生成另一个物理地址并把网卡 eth0-->eht1; 如果复制多次将是 eth1-->eth2...... 依次往后；你复制虚拟机次数越多，ifconfig 看到的网卡号越大。

VM 虚拟机在复制时，VM 会新建一个 UUID, 一些新的 Linux 版本是把以太网与 Mac 地址绑定，当新的 UUID 建立的时候，Mac 地址就被改变了。但由于 eth0 设备所装载的配置与读取默认配置的 Mac 地址不一致， 所以会提示 no device found (设备没法找到) ，可以通过修改 eth0 网卡配置来修复此问题。



##### 3. 解决方法 



###### 3.1. 复制网卡物理地址

复制完虚拟机后，要正常上网，首先要重新生成新虚拟机的物理网卡地址，如下图所示，一次点击编辑虚拟机设置（1）--> 选择网络适配器（2）--> 点击高级选项（3）--> 点击生成按钮（4），即可生成新的物理网卡地址，然后复制当前生成的网卡地址，以备后续使用。

![img](https://static.oschina.net/uploads/space/2017/1214/093025_38bG_1784946.png)



###### 3.2. 编辑 /etc/udev/rules.d/70-presistent-net.rules 文件

```bash
[root@server01 ]# vim /etc/udev/rules.d/70-persistent-net.rules
```

把其他注释掉或删掉只留最后一个，而且你会发现你刚生产的网卡地址就在这里，修改最后一行的最后参数 NAME="eth0"， 修改后如下图：

![img](https://static.oschina.net/uploads/space/2017/1213/192232_AlYJ_1784946.png)



###### 3.3. 修改 ifcfg-eth0 配置文件

```bash
[root@server01 ]# vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

将 HWADDR 值修改为 /etc/udev/rules.d/70-presistent.rules 文件中的新值：

HWADDR=00:50:56:39:9b:f1--> HWADDR=00:50:56:2c:13:ae

修改后配置如下图：

![img](https://static.oschina.net/uploads/space/2017/1213/192847_GrqU_1784946.png)



###### 3.4. 重启生效

```bash
[root@server01 ]# service network restart
```

如果重启网卡有一些问题，最好重启操作系统。

重启 Linux 后可以正常上网了，截图如下：

![img](https://static.oschina.net/uploads/space/2017/1213/193528_u51O_1784946.png)

### kali复制虚拟机后没网

基本思路:
0.重启
1.vmware左上角–虚拟机–可移动设备–网络设备–设置中切换网络连接方式 NAT或者桥接模式
2.编辑/etc/network/interfaces文件

```
vim /etc/network/interfaces 
```

使用vim命令修改这个文件 eth0为dhcp模式，（手动获取IP也可以，不要出现问题就行）

使用i修改文本 :wq退出并保存

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191001213746993.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMzMxNjA4,size_16,color_FFFFFF,t_70)

3.重启网卡

```
/etc/init.d/networking restart 
ifconfig eth0 down
ifconfig eth0 up
```

