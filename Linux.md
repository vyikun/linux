





# Linux基础命令

[TOC]



## 0. bash基础

**Linux shell 主要分为以下几类**

```bash
交互式shell，等待用户输入执行的命令(终端操作,需要不断提示）
非交互式shell，执行shell脚本，脚本执行结束后shel自动退出
登陆shell，需要输入用户名和密码才能进入Shell，日常接触的最多的一种
非登陆shell，不需要输入用户和密码就能进入Shell,比如运行bash会开启一个新的会话窗口
```

2.bash shell 配置文件介绍（文件主要保存用户的工作环境）

```bash
个人配置文件: ~/.bash_profile  ~/.bashrc  
全局配置文件:/etc/profile /etc/profile.d/*.sh /etc/bashrc 
profile类文件，设定环境变量，登陆前运行的脚本和命令。
bashrc类文件，设定本地变量,定义命令别名
PS:如果全局配置和个人配置产生冲突，以个人配置为准。
```

3.登陆系统后，环境变量配置文件的应用顺序是?

```bash
登陆式shell配置文件执行顺序：/etc/profile->/etc/profile.d/*.sh->/root/.bash_profile->/etc/bashrc
非登录shelll配置文件执行顺序：/root/.bashrc->/etc/bashrc->/etc/profile.d/*.sh
```

**ps:验证使用echo在每行添加一个文件名输出即可**

### 0.1.快捷键

```bash
ctrl+a          将光标移动到行首
ctrl+e          将光标移动到行尾
ctrl+k          删除光标后的所有内容
ctrl+u          删除光标前的所有内容
ctrl+w          删除光标之前的内容，按单词进行删除
ctrl+l          清理屏幕 ==> clear
ctrl+r          搜索历史执行过的命令（按关键字搜索）
ctrl+c          结束当前正在Bash窗口前台运行的程序
ctrl+d          退出当前Bash Shell ==>logout
```

### 0.2.别名alias

1.临时设置

```bash
设置
#alias net='cat /etc/sysconfig/network-scripts/ifcfg-eth0'
取消
unalias net
```

2.永久设置 

```bash
当前用户下#cat ~/.bashrc文件会自动加载，这个文件也指向/etc/bashrc,所以添加在那个文件都可以
#设置方法
echo "alias net='cat /etc/sysconfig/network-scripts/ifcfg-eth0'" >>/etc/bashrc
```

### 0.3.历史记录history

```bash
#使用
!123                #调用history历史记录中123次的操作再次执行
!!                  #快速掉用上一条执行过的命令
按ecs在按 .          #快速调取上一条命令的参数

#参数
-w 保存当前shell执行过的历史命令，至文件中存储默认存放~/.bash_history文件
-c 清空命令历史记录，不会清空文件      实例：history -c
-d 删除命令历史的第n行               实例：history -d 123
```

### 0.4.帮助手册 --help | man

```
ls --help
man ls						--->按q退出
```

## 1.文件管理

**创建/复制/移动/删除/编辑**

### 1.1.创建文件 touch

```
#touch file						#无则创建有则修改创建时间
#touch file1 file2	
#touch /home/file3 file4
#touch file{a,b,c}				#{}集合,等价 touch a b c
#touch file{1..10}
#touch file{a..z}
```

### 1.2.创建目录 mkdir

目录通常显示为蓝色

```
命令:mkdir
选项: -p递归创建 -v显示创建过程		
参数: 路径,在什么地方创建

#mkdir /home/cc/123 /home/kk/456 -p
#mkdir /home/cc/{dir3,dir4}
#mkdir -pv /home/{cc/{xx,zz},oo}

tree 将目录以树状结构显示,如果没有的话yum instll tree -y 安装
```

### 1.3.拷贝文件 cp

```
命令: cp
选项: 
-v:详细显示命令执行的操作 
-r:递归处理目录与文件 
-p:保留源文件或目录的属性
```

### 1.4.移动文件 mv

```
移动文件: mv[OPTION]... SoURCE... DIRECTORY
```

### 1.5.删除文件或目录 rm

```
选项: 
-r:递归 
-f:强制删除 
-v:详细过程
```

实验:

```
1.创建了一推的文件，文件要进行分门别类存储起来。
	1)创建一推文件 		{/data/filea-filez}
	2)创建一个目录	 	{/data/files}
	3)将文件拷贝到对应目录
	4)删除文件			  {/data/files/*}

```

### 1.6.chmod 修改文件权限

```bash
#选项:  -R递归修改
# chmod 644 file
# chmod -R 755 dir/
```

### 1.7.chown 修改属主属组

```bash
#选项:  -R递归修改

1.变更文件的属主和属组
chown kk.kk access-2020-03-12.log

2.仅变更文件的属组
[root@web ~]# chown .root access-2020-03-12.log
[root@web ~]# ll access-2020-03-12.log
-rw-r--r--. 1 kk root 58112885 3月  13 09:25 access-2020-03-12.log

3.使用chgrp直接变更文件的属组（只能是属组，不能是其他）
[root@web ~]# chgrp kk access-2020-03-12.log
[root@web ~]# ll access-2020-03-12.log
-rw-r--r--. 1 kk kk 58112885 3月  13 09:25 access-2020-03-12.log
```

### 1.7.文件编辑vim

1.普通模式

```bash
vim一个文件默认是普通模式,主要是控制光标移动,可对文本进行复制、粘贴、删除等工作。
1、命令行跳转
G       #光标跳转至未端（文件的尾部）
gg      #光标跳转至顶端
Ngg     #光标跳转至当前文件内的N行  （指定光标跳转到多少行） 5gg 跳转到第五行
ctrl+f  #往下翻页（行比较多）
ctrl+b  #往下翻页

$       #光标跳转至当前光标所在行的尾部 （只是跳转，并不会进入编辑模式）
^|0     #光标跳转至当前光标所在行的首部

2、复制粘贴
yy      #复制与当前光标所在的行
Nyy     #复制带上当前光标所在的行，共N行

p(小写)   #粘贴至当前光标下一行
P(大写)   #粘贴至当前光标上一行
3、删除剪贴撤销
dd      #删除当前光标所在的行
4dd     #删除当前光标所在的行以及往下的3行
dG      #删除当前光标以后的所有行
D       #删除当前光标及光标以后的内容
x       #删除当前光标标记记住往后的字符
X       #删除当前光标标记记住往前的字符
dd & p  #剪贴 先删除后粘贴=剪贴
u       #撤销上一次的操作
4、替换
r       #替换当前光标标记的单个字符
R       #进入REPLACE模式，连续替换，ESC结束
```

2.编辑模式

```bash
从普通模式进入编辑模式，只需按一个键即可（i、I、a、A、o、O） esc退出编辑模式
i       #进入编辑模式，光标不做任何操作
a       #进入编辑模式，将当前光标往后一位
o       #进入编辑模式，并在当前光标下添加一行空白内容

I       #进入编辑模式，并且光标会跳转至本行的头部
A       #进入编辑模式，将光标移动至本行尾部
O       #进入编辑模式，并在当前光标上添加一行空白内容
```

3.末行模式

```bash
在普通模式下,输入 ":" 或者 "/" 即可进入末行模式.在末行模式下可进行的操作有:显示行号、搜索、替换、保存、退出。
#主要保存退出
:w          保存当前状态
:q          退出当前文档（文档必须保存才能退出）
:w!         强制保存当前状态
:q!         强制退出文档不会修改文件内容
:wq         先保存在退出
:wq!        强制保存并退出
:number     跳转至对应的行号

2.文件内容查找
/string     #需要搜索的内容（查找）
n           #按搜索到的内容依次往下进行查找
N           #按搜索到的内容依次往上查找
3.文件内容替换

:1,5s#sbin#test#g       #替换1-5行中包含sbin的内容为test
:%S#sbin#test#g         #替换整个文本文件中包含sbin的替换为test
4.文件内容保存
:w /root/test.txt       #将所有内容保存到/root/test.txt文件中

5.文件内容读入
:r /etc/hosts           #读入/etc/hosts文件至当前光标下面
:5r /etc/hosts          #指定插入/etc/hosts文件至当前文件的第五行下面
```

4.视图模式

```bash
从普通模式进入视图模式，主要进行批量操作

shift+v     进入可视化模式，选中整行内容可进行如下操作：
    1.复制：选中行内容后按y键即可复制。
    2.删除：选中行内容后按d键删除。
    
ctrl+v      进入可视快模式，选中需要注释的行
    1.插入：按shift+i(就是大写i)进入编辑模式，输入#，结束按ESC键
    2.删除：选中内容后，按x或者d键删除
    3.替换：选中需要替换的内容，按下r键，然后输入替换后的内容
```

5.扩展知识

```bash
1.环境变量临时生效

:set nu             #显示行号
:set ic             #忽略大小写,在搜索的时候有用
:set ai             #自动缩进
:set list           #显示制表符(空行、tab键)
:noho               #取消搜索出来的内容显示高亮
:set no[nu|ic|ai..] #取消临时设定的变量
2.环境变量永久生效. ~/.vimrc 个人环境变量(优先级高) /etc/vimrc 全局环境变量

# vim ~/.vimrc      #当下次再打开文件自动显示行号并忽略大小写
set nu
set ic

#如果个人vim环境没有配置,则使用全局vim环境变量配置.
#如果个人vim环境和全局环境变量产生冲突,优先使用个人vim环境变量.
3.如何同时编辑多个文件

vim -o file1 file2      #水平分隔
vim -O file1 file2      #垂直分隔

#ctrl+ww    文件切换
4.相同文件之间差异对比,通常用于对比修改前后差异

#diff           #文件对比
#vimdiff        #以vim方式打开两个文件对比,高亮显示不同的内容
5.如果vim非正常退出(ctrl+z)挂起或强制退出终端没关闭vim后

当我们去编辑一个文件的时，有时会出现网络中断、或者自己按了一下ctrl+z，造成异常情况。
    编辑文件时，可以选择r键。恢复到修改的状态。
        可以选择e键。恢复文件没保存的状态。
    记得文件恢复后，记得干掉.xxx的swp文件 （或者移动走）

#假设打开filename文件被意外关闭,需要删除同文件名的.swp文件即可解决
#rm -f .filename.swp
```



## 2.查看文件内容

### 2.1.查看文件内容 cat

```
参数: -n:查看文件内容
	 -A:查看文件特殊符号
```

### 2.2.查看大文件 less、more

```
less /etc/services	#使用光标上下翻动，空格进行翻页，q退出
more /etc/services	#使用回车上下翻动，空格进行翻页，q退出
```

### 2.3.查看文件前十行head

```
默认查看文件前十行
head /etc/passwd

参数： -n5	#查看头部前5行
```

### 2.4.查看文件后十行 tail

```
默认查看文件后十行
tali /etc/passwd

参数： -n5 #查看文件后五行
	  -f  #动态查看文件尾部信息 同等talif

tali -f /var/log/secure
```

### 2.5.过滤文件内容 grep

```
grep "root" /etc/passwd		#匹配包含root关键字的行

参数					示例
^ 以什么开头的行		grep "^root" /etc/passwd
$ 以什么结尾的行		grep "/bin/bash$" /etc/passwd
-v 取反			   grep  -v "/bin/bash$" /etc/passwd
-i 忽略大小写		 grep -i "root" /etc/passwd
-E 多条件过滤		 grep -E "sync|ftp" /etc/passwd


-n 查看过滤的文件所在行 grep -n "root" /etc/passwd

grep -n -A 2 "Failed" /var/log/secure #匹配文件中Failed字符串，并打印他的下两行
grep -n -B 2 "Failed" /var/log/secure #匹配文件中Failed字符串，并打印他的上两行
grep -n -C 2 "Failed" /var/log/secure #匹配文件中Failed字符串，并打印他的上下各两行
```

## 3.下载文件

### 3.1. wget curl 联网下载文件

**wget**

```
#centos7 系统最小化安装默认没有wget命令，需要安装
#yum install wget -y

#下载互联网上的文件至本地
#wget http://mirrors.aliyun.com/repo/Centos-7.repo

#-o 指定文件下载位置
#wget -o /etc/yum.repos.d/CentOs-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

**curl** 

```
只curl的话只会查看文件内容加-o下载

curl -o /etc/yum.repos.d/CentOs-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

### 3.2. rz sz 上传下载文件

```
yum install lrzsz -y

#sz /opt/cc		下载文件到本地
#rz				上传文件到服务器

```

## 4.文件或命令查找

### 4.1. which whereis 命令路径查找 

```
which ls		#查找ls命令的绝对路径
whereis ls		#查找命令的绝对路径、帮助手册等
whereis -b ls	#-b 仅显示命令的所在路径
type -a ls		#查看命令的绝对路径（包括别名）
```

### 4.2. sort 文件内容排序

参数

```
-r：倒叙
-n：按数字排序
-t：指定分割符（默认空格）
-k：指定第几列，指定第几列几字符（指定1,1 3.1,3.3)
```

```
练习文件
cat >> file.txt <<EOF
b:3
c:2
a:4
e:5
d:1
f:11
EOF

sort -t ":" -k2 file.txt  #-t指定分割符 -k指定列

sort -t ":" -k2 -n file.txt  #-t指定分割符 -k指定列 -n按阿拉伯数字排序

sort -t ":" -k2 -n -r file.txt  #-t指定分割符 -k指定列 -n按阿拉伯数字排序 -r倒叙最大的在上面

sort -t ":" -k2 -nr file.txt | head -n3  #提取前三
ps：| 管道：将左边命令的输出结果通过管道交给右边命令的输入

练习二：
cat ip.txt
192.168.3.1 00:0F:AF:81:19:1F
192.168.3.2 00:0F:AF:85:6C:25
192.168.3.3 00:0F:AF:85:70:42
192.168.2.20 00:0F:AF:85:55:DE
192.168.2.21 00:0F:AF:85:6C:09
192.168.2.22 00:0F:AF:85:5C:41
192.168.0.151 00:0F:AF:85:6C:F6
192.168.0.152 00:0F:AF:83:1F:65
192.168.0.153 00:0F:AF:85:70:03
192.168.1.10 00:30:15:A2:3B:B6
192.168.1.11 00:30:15:A3:23:B7
192.168.1.12 00:30:15:A2:3A:A1
192.168.1.1 00:0F:AF:81:19:1F
192.168.2.2 00:0F:AF:85:6C:25
192.168.3.3 00:0F:AF:85:70:42
192.168.2.20 00:0F:AF:85:55:DE
192.168.1.21 00:0F:AF:85:6C:09
192.168.2.22 00:0F:AF:85:5C:41
192.168.0.151 00:0F:AF:85:6C:F6
192.168.1.152 00:0F:AF:83:1F:65
192.168.0.153 00:0F:AF:85:70:03
192.168.3.10 00:30:15:A2:3B:B6
192.168.1.11 00:30:15:A3:23:B7
192.168.3.12 00:30:15:A2:3A:A1

sort -t "." -k3.1,3.1 -k4.1,4.3 -n ip.txt
```

### 4.3.  uniq 去重并统计出现的次数

必须配合sort 一起使用，先排序，然后去重统计

**参数**

```
-c 计算重复的次数，必须挨着的相同行
```

```
练习文件
cat >>file3.txt <<EOF
abc
123
abc
123
EOF

sort file3.txt | uniq	#去重不统计
sort file3.txt | uniq -c	#-c去重统计出现的次数
```

### 4.4. cut 截取字段 配合简单了解使用sed swk

```
选项：
-d：指定分割符
-f：数字，取第几列 -f3,6三列和6列
-c：按字符取（空格也算）

1.产生文件
# echo "Im oldxu, is QQ 552408925" > oldboy.txt

2.需求：过滤出oldboy.txt文件里 oldxu以及552408925
3.如何实现：
实现方法1：
[root@oldboy ~]# cut -d " " -f 2,5 oldboy.txt
oldxu, 552408925
[root@oldboy ~]# cut -d " " -f 2,5 oldboy.txt  | sed 's#,##g'
oldxu 552408925

实现方法2：
[root@oldboy ~]# awk  '{print $2,$5}' oldboy.txt
oldxu, 552408925
[root@oldboy ~]# awk  '{print $2,$5}' oldboy.txt | sed 's#,##g'
oldxu 552408925

实现方法3： awk处理
[root@oldboy ~]# awk -F "," '{print $1,$2}' oldboy.txt  | awk '{print $2,$5}'
oldxu 552408925


[root@oldboy ~]# awk -F "[ ,]" '{print $2,$6}' oldboy.txt
oldxu 552408925

高级用法
[ ,]+   +表示重复	前面的字符一次或多次   
	空格算一个分隔符
	逗号算一个分隔符
	空格和逗号挨在一起，也算一个分隔符
	空格逗号空格，全算一个分隔符
[root@oldboy ~]# awk -F "[ ,]+" '{print $2,$5}' oldboy.txt
oldxu 552408925
```

### 4.5. wc 统计文件多少行

```
选项：
-l	显示文件行数
-c	显示文件字节
-w	显示文件单词
```

练习统计一个文件有多少行

```
方法一：
# wc -l /etc/services
11176 /etc/services

方法二：
# cat -n /etc/services |tail -1 |awk '{print $1}'

方法三：
# grep -n ".*" /etc/services |tail -1 |awk -F ":" '{print $1}'

方法四：仅供参考！！！！！！！不纠结
NR: 行号
$0: awk是逐行处理文件的，读入一行，然后将一行的内容赋值给$0变量，
# awk '{print NR,$0}' /etc/services  | tail -1 | awk '{print $1}'
11176

```

#练习题: 过滤出/etc/passwd以nologin结尾的内容，并统计有多少行

```
	过滤： grep   /etc/passwd
	条件： nologin结尾的
	并且：
	统计出现的内容总共有多少行： wc -l

[root@oldboy ~]# grep "nologin$" /etc/passwd | wc -l
19
```

练习题：

```
分析如下日志，统计每个域名被访问的次数。
[root@student tmp]# cat >>web.log<<EOF 
http://www.xuliangwei.com/index.html
http://www.xuliangwei.com/1.html
http://post.xuliangwei.com/index.html
http://mp3.xuliangwei.com/index.html
http://www.xuliangwei.com/3.html
http://post.xuliangwei.com/2.html
EOF

思路：
	1.要想办法提取域名
	2.排序，将相同的域名罗列在一起
	3.去重，统计

实现：
[root@oldboy ~]# awk -F '/' '{print $3}' web.log | sort | uniq -c
      1 mp3.xuliangwei.com
      2 post.xuliangwei.com
      3 www.xuliangwei.com

将访问次数最多的排在上面
[root@oldboy ~]# awk -F '/' '{print $3}' web.log | sort | uniq -c  | sort -nr
      3 www.xuliangwei.com
      2 post.xuliangwei.com
      1 mp3.xuliangwei.com


----------------------------下午-------------------------------
习题1: 使用awk取出系统的IP地址，思路如下:
	1.我要取的IP值在哪里	 ifconfig
	2.如何缩小取值范围(行)
	3.如何精确具体内容(列)

方式1：
[root@oldboy ~]# ifconfig eth0 | head -2 | tail -1 | awk '{print $2}'
172.16.1.53

方式2：
[root@oldboy ~]# ifconfig eth0 | grep "netmask" | awk '{print $2}'
172.16.1.53


习题2: 将/etc/passwd文件中的第一行中的第一列和最后一列位置进行交换。（自行指定以：为分隔符）
	源文件：root:x:0:0:root:/root:/bin/bash
	改变后：/bin/bash:x:0:0:root:/root:root


习题3: 将/etc/sysconfig/selinux 文件中的SELINUX=enforcing替换成SELINUX=disabled
```

## 5.用户管理

### 5.1.useradd 创建用户

```bash
adduser命令软连接指向useradd命令
#选项
# -u    指定要创建的用户UID，不允许冲突
# -g    指定要创建用户默认组
# -G    指定要创建用户附加组，逗号隔开可添加多个附加组
# -s    指定要创建用户的bash shell
# -c    指定要创建用户的注释信息

# -d    指定要创建用户的家目录
# -M    给创建的用户不创建家目录
# -r    创建系统用户，默认无家目录

1.创建kk用户，UID5001，基本组students，附加组sa 注释信息：2021 new student，登录shell:/bin/bash
2.创建mysql系统用户，-M不创建用户的家目录 -s指定nologin使用其他用户无法登录系统
方法二：-M不给用户创建家目录，并指定登录的bash是/sbin/nologin
```

### 5.2.usermod 修改用户

```bash
#选项
# -u 指定要修改用户的UID
# -g 指定要修改用户基本组
# -G 指定要修改用户附加组，使用逗号隔开多个附加组, 覆盖原有的附加组
# -d 指定要修改用户家目录
# -s 指定要修改用户的bash shell
# -c 指定要修改用户注释信息

# -l 指定要修改用户的登陆名
# -L 指定要锁定的用户
# -U 指定要解锁的用户

#4、锁定用户【扩展】
# echo "123" |passwd --stdin username
# usermod -L username  #锁定后会无法登陆系统

#5、解锁用户【扩展】
# usermod -U username
```

### 5.3.userdel 删除用户

```bash
#选项 -r 删除用户的家目录，以及用户的邮件 
```

### 5.4.passwd 修改用户密码

```bash
1、使用passwd命令修改用户密码
# passwd        #给当前用户修改密码
# passwd root   #给root用户修改密码(只能是root才有此权限，其他任何用户都没有该权限)
# passwd kk #给kk用户修改密码（root可以，或者kk用户自己给自己修改密码）

2、通过passwd --stdin读取输出的结果，将结果赋值给对应的用户
# echo "123" | passwd --stdin kk    #非交互式修改密码
```

**PS: 推荐密码保存套件工具，支持windows、MacOS、Iphone以及浏览器插件Lastpass官方网站**

### 5.5.mkpasswd 生成随机数

```bash
    选项：
    -l   设定密码长度,
    -d   数子,
    -c   小写字母,
    -C   大写字母,
    -s   特殊字符
    
[root@docker01 ~]# yum install expect -y

#注意：各种特殊字符的位数加起来，不能超过-l指定的长度
[root@docker01 ~]# mkpasswd -l 10 -d 2 -c 3 -C 3 -s 2
[ks1KOmZ8$
```

### 5.6. su sudo提权

**1、su 提权**

 **普通用户su -可以直接切换至root用户，但需要输入root用户的密码。 超级管理员root用户使用su - username切换普通用户不需要输入任何密码**

```bash
su - username	属于登陆式shell
su 	username	属于非登陆式shell
区别在于加载的环境变量不一样

#1.普通用户使用su切换root
[root@test01 home]$ su 

#2.普通用户使用su -切换到root，会加载root的环境变量
[kk@test01 home]$ su -

#3.以某个用户的身份执行某个服务，使用命令su -c username
[root@test01 ~]# su - kk -c 'ls /home'
```

**2、sudo 提权**

1.如何快速埋下hulk的种子呢？

```bash
#1.快速配置sudo方式[先睹为快]wheel组
[root@test01 ~]# usermod kk -G wheel
[root@test01 ~]# id kk
uid=1005(kk) gid=1005(kk) groups=1005(kk),10(wheel)
[root@test01 ~]# gpasswd -d kk wheel   #删除附加组
Removing user kk from group wheel
[root@test01 ~]# id kk
uid=1005(kk) gid=1005(kk) groups=1005(kk)

[root@node1 ~]$ sudo tail -f /var/log/secure    #sudo审计日志

#2.一般正常配置sudo方式
[root@test01 ~]# #visudo => vim /etc/sudoers
#1.用户名  2.主机名=(角色名）       4.命令名
bgx       ALL=(ALL)         /usr/bin/yum,/usr/sbin/useradd   #允许使用sudo执行命令
oldboy   ALL=(ALL)          NOPASSWD:/bin/cp, /bin/rm   #NOPASSWD不需要使用密码
```

2.埋下了hulk种子后又如何提权使用呢？

```bash
#1.切换普通用户
[root@test01 ~]# su - kk

#2.检查普通用户能提权的命令
[root@test01 ~]$ sudo -l
User kk may run the following commands on this host:
    (ALL) ALL

#3.普通用户正常情况下是无法删除opt目录的
[root@test01 ~]$ rm -rf /opt/
rm: cannot remove `/opt: Permission denied

#4.使用sudo提权，需要输入普通用户的密码。
[root@test01 ~]$ sudo rm -rf /opt
```

**3.提升的权限太大，能否有办法限制仅开启某个命令的使用权限？其他命令不允许？**

```bash
第一种方式:使用sudo中自带的别名操作,将多个用户定义成一个组,这个组只有sudo认可
#编辑sudo配置
[root@test01 ~]# visudo 
​
# 1.使用sudo定义分组,这个系统group没什么关系
User_Alias OPS = qq,ww
User_Alias DBA = kk,gg
​
# 2.定义可执行的命令组, 便于后续调用
Cmnd_Alias NETWORKING = /sbin/ifconfig, /bin/ping
Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/yum
Cmnd_Alias SERVICES = /sbin/service, /usr/bin/systemctl start
Cmnd_Alias STORAGE = /bin/mount, /bin/umount
Cmnd_Alias DELEGATING = /bin/chown, /bin/chmod, /bin/chgrp
Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall
​
# 3.使用sudo开始分配权限
OPS  ALL=(ALL) NETWORKING,SOFTWARE,SERVICES,STORAGE,DELEGATING,PROCESSES
DBA  ALL=(ALL) SOFTWARE,PROCESSES
​
#4.登陆对应的用户使用 sudo -l 验证权限
方式二：针对系统中真实的组来进行操作
```

### 5.7.-bash-4.1$ 恢复

```bash
#故障案例，在当前用户家目录执行了rm -rf .*，下次登录系统时出现-bash-4.1$，如何解决！
-bash-4.1$ cp -a /etc/skel/.bash* ./
-bash-4.1$ exit
[root@text01 ~]#   #重新连接即可恢复
```

## 6.组管理

### 6.1.groupadd 创建组

```bash
#选项
# -u 指定要创建用户的UID,不允许冲突
# -g 指定要创建用户默认组
# -G 指定要创建用户附加组,逗号隔开可添加多个附加组
# -d 指定要创建用户家目录
# -s 指定要创建用户的bash shell
# -c 指定要创建用户注释信息
# -M 给创建的用户不创建家目录
# -r 创建系统账户，默认无家目录
```



### 6.2.groupmod 修改组

```bash
#选项
# -u 指定要修改用户的UID
# -g 指定要修改用户基本组
# -G 指定要修改用户附加组，使用逗号隔开多个附加组, 覆盖原有的附加组
# -d 指定要修改用户家目录
# -s 指定要修改用户的bash shell
# -c 指定要修改用户注释信息
# -l 指定要修改用户的登陆名
# -L 指定要锁定的用户
# -U 指定要解锁的用户
```

### 6.3.groupdel 删除组

```bash
#选项 -r 删除用户同时删除它的家目录
[root@oldboy ~]# useradd ttboy -g active_gid
[root@oldboy ~]# useradd ggboy -G active_gid

[root@oldboy ~]# grep "active_gid" /etc/group
active_gid:x:12345:ggboy


[root@oldboy ~]# groupdel active_gid
groupdel：不能移除用户“ttboy”的主组

#移除主要的成员，就能删除该组
[root@oldboy ~]# userdel ttboy
[root@oldboy ~]# groupdel active_gid

[root@oldboy ~]# id ggboy
uid=6009(ggboy) gid=12346(ggboy) 组=12346(ggboy)

#1.删除user1用户，但不删除用户家目录和 mail spool
[root@bgx ~]# userdel user1

#2.-r参数可以连同用户家目录一起删除(慎用)
[root@bgx ~]# userdel -r user1

注意：删除一个组，必须确保该组的主要成员已经移除该组，就可以正常删除。（附加进来的成员无需考虑在内。）
```

## 7.权限管理

### 7.1chmod  修改文件rwx权限

```bash
#-R参数递归修改
#针对目录设定权限
[root@web ~]# mkdir dir
[root@web ~]# chmod 777 dir/    #修改目录允许所有人访问
[root@web ~]# chmod -R 755 dir/ #修改目录及子目录权限
[root@web ~]# ll -d dir/
drwxr-xr-x 2 root root 6 Apr 13 03:34 dir/

针对 hr 部门的访问目录/home/hr 设置权限，要求如下:
1.root 用户和 hr 组的员工可以读、写、执行
2.其他用户没有任何权限

[root@web ~]# groupadd hr
[root@web ~]# useradd hr01 -G hr
[root@web ~]# useradd hr02 -G hr
[root@web ~]# useradd hr03
[root@web ~]# mkdir /home/hr
[root@web ~]# chgrp hr /home/hr
[root@web ~]# chmod 770 /home/hr
[root@web ~]# ll -d /home/hr
drwxrwx--- 2 root hr 6 Apr 13 03:26 /home/hr



hr01和hr02都能完成如下操作
	touch /home/hr/file
	ll /home/hr

hr03： 他不会这个目录的所属主，也不是hr组的成员，所以按照其他权限的身份去访问：
	其他人的权限身份 --- ，所以 hr03 没有任何的权限
	hr03用户无法进入 /home/hr   也无法 ls /home/hr 浏览

       因为hr03用户，既不是属主身份，也不是他的属组身份，所以就按照其他人的身份访问，就要受到其他人权限约束
```

### 7.2 chown 修改文件属主属组

```bash
#参数-R 递归修改

#1.变更文件的属主和属组
[root@web ~]# chown kk.kk access-2020-03-12.log
[root@web ~]# ll access-2020-03-12.log
-rw-r--r--. 1 kk kk 58112885 3月  13 09:25 access-2020-03-12.log

#2.仅变更文件的属组
[root@web ~]# chown .root access-2020-03-12.log
[root@web ~]# ll access-2020-03-12.log
-rw-r--r--. 1 kk root 58112885 3月  13 09:25 access-2020-03-12.log

#3.使用chgrp直接变更文件的属组（只能是属组，不能是其他）

[root@web ~]# chgrp kk access-2020-03-12.log
[root@web ~]# ll access-2020-03-12.log
-rw-r--r--. 1 kk kk 58112885 3月  13 09:25 access-2020-03-12.log
```

### 7.3 特殊权限 suid sgid sbit

```bash
文件权限标志 suid（Set User ID），sgid（Set Group ID）和 sbit（Sticky Bit）是 UNIX 和类 UNIX 操作系统下一些特殊权限，用于处理特定场景的文件访问和执行。以下是对这些权限的简要说明：

#suid (Set User ID):
当一个可执行文件具有 suid 权限，并被执行时，进程的有效用户 ID 将被设置为文件所有者的用户 ID。这意味着，运行该程序的用户将获得与文件所有者相同的访问权限。
通常适用于允许非特权用户执行特权任务的情况。
使用 chmod u+s filename 或 chmod 4xxx（原权限） filename 给某个文件设置 suid 标志。
使用 ls 命令时，文件权限中的 "x"（执行权限）将被替换为 "s"。

#sgid (Set Group ID):
当一个可执行文件具有 sgid 权限，并被执行时，进程的有效组 ID 将被设置为文件所有者的组 ID。与 suid 类似，执行程序的用户将获得与文件所有者同组用户的访问权限。
当在目录上设置 sgid 标志时，新建的子目录和文件将继承目录的组 ID，而不是使用创建它们的用户的组 ID。
使用 chmod g+s filename 或 chmod 2xxx（原权限） filename 给某个文件或目录设置 sgid 标志。
使用 ls 命令时，文件权限中的 "x"（执行权限）将被替换为 "s"。

#sbit (Sticky Bit):
通常在目录上设置。Sticky Bit 用于保护目录下的文件和子目录。 当在目录上设置 Sticky Bit 标志时，只有文件或子目录的所有者、目录的所有者和 root 用户才能重命名、移动或删除其中的文件和子目录。 其他用户即使拥有写入权限也无法进行此类操作。
使用 chmod o+t dirname 或 chmod 1xxx（原权限） dirname 给某个目录设置 Sticky Bit 标志。
使用 ls 命令时，文件权限中的 "x"（执行权限）将被替换为 "t"。
在 UNIX 和类 UNIX 操作系统下通过命令 ls -l 可以查看文件的权限，这些特殊权限可显示为 "s" 或 "t"，取决于具体设置的标志。
```

## 8.重定向

当**运行一个程序**时通常会自动打开**三个标准文件**，分别是标准输入、标准输出、错误输出

```undefined
标准输入：键盘，输入的内容，或者是通过其他方式读入的内容
标准输出：当执行命令，返回的正确结果
错误输出：当执行命令，返回的错误结果
```

### 8.1 输出重定向

**输出重定向，改变输出内容的位置。输出重定向有如下几种方式，如表格所示**

| 类型               | 操作符 | 用途                                                         |
| :----------------- | :----- | :----------------------------------------------------------- |
| 标准覆盖输出重定向 | >      | 将程序输出的正确结果输出到指定的文件中,会覆盖文件原有的内容  |
| 标准追加输出重定向 | >>     | 将程序输出的正确结果以追加的方式输出到指定文件，不会覆盖原有文件 |
| 错误覆盖输出重定向 | 2>     | 将程序的错误结果输出到执行的文件中，会覆盖文件原有的内容     |
| 错误追加输出重定向 | 2>>    | 将程序输出的错误结果以追加的方式输出到指定文件，不会覆盖原有文件 |
| 标准输入重定向     | <<     | 将命令中接收输入的途径由默认的键盘更改为指定的文件或命令     |

```
请解释如下重定向含义：

	>		标准正确输出   （ 覆盖 ）
	>>		标准正确输出   （ 追加 ）
	&>		混合输出（标准输出、标准错误输出）	（ 覆盖 ）
	&>>		混合输出（标准输出、标准错误输出）	（ 追加 ）
	2>		标准错误输出	（ 覆盖 ）
	2>>		标准错误输出	（ 追加 ）
	1> te.txt 2>&1	正确和错误都输入到相同位置 错误->标准输出->te.txt  ( 覆盖，标准输出是覆盖的 )
	1>>te.txt 2>&1						错误->标准输出->>te.txt	( 追加，标准输出是追加的 )
```

### 8.2 输入重定向

**输入重定向，即原本从键盘等上获得的输入信息，重定向由命令的输出作为输入。< ==0<**

```
案例1: 从文件中读入输入的操作 
案例2:mysql如何恢复备份
案例3: 利用重定向建立多行数据的文件

```

### 8.3 管道技术 tee、xargs

**管道操作符号 "|" ，主要用来连接左右两个命令, 将左侧的命令的标准输出, 交给右侧命令的标准输**

**1、tee**

**tee将管道执行的结果保存一份并向后传递**

```bash
#选项: -a追加

tee和重定向的区别？

[root@xuliangwei ~]# date > 1.txt    #将date命令的输出结果写入到1.txt中

[root@xuliangwei ~]# date | tee 1.txt #命令执行会输出至屏幕，但会同时保存一份至1.txt文件中
```

### 2.xargs

**xargs参数传递，主要让一些不支持管道的命令可以使用管道技术**

```bash
# which cat|xargs ls- l

# ls |xargs rm -fv
```

## 9. find 文件查找

### 9.1 查找示例

```bash
#参数:
-name	文件名称查找
-iname	忽略大小写
-size	文件大小查找
-type	文件类型查找 f文件 d目录 l连接 b块设备 c字符设备 s套接字
-mtime	时间查找
打印当天的文件===计不计算本地当天时间文件
#2.查找7天以前的文件(不会打印当天的文件)=====( 保留了最近7天的数据)
# find ./ -iname "file-*" -mtime +7

#3.查找最近7天的文件，不建议使用(会打印当天的文件[当天时间计算])
# find ./ -iname "file-*" -mtime -7

#4.查找第7天文件(不会打印当天的文件)
# find ./ -iname "file-*" -mtime 7

#5.本地文件保留最近7天的备份文件, 备份服务器保留3个月的备份文件(实际使用方案)
find /backup/ -iname "*.bak" -mtime +7 -delete
find /backup/ -iname "*.bak" -mtime +90 -delete

-user	属主查询
-group	属组查询

#两个参数之间的关联
-a	并且关系，两个条件同时满足
-o	或关系，满足一个即可匹配
!	非 not
```

### 9.2 查找后处理动作（查看、列出、拷贝、删除等）

**find动作处理，比如查找到一个文件后，需要对文件进行如何处理, find的默认动作是 -print**

| 动作    | 含义                                     |
| :------ | :--------------------------------------- |
| -print  | 打印查找到的内容(默认)                   |
| -ls     | 以长格式显示的方式打印查找到的内容       |
| -delete | 删除查找到的文件(仅能删除空目录)         |
| -ok     | 后面跟自定义 shell 命令(会提示是否操作)  |
| -exec   | 后面跟自定义 shell 命令(标准写法 -exec 😉 |

```bash
#ps：-exec和xargs删除区别
exec  一个一个的删除
	rm -f ifcfg-eth0
	rm -f ifcfg-eth1
	rm -f ifcfg-eth2

xargs 一次干掉
	rm -f  ifcfg-eth0 ifcfg-eth1 ifcfg-eth2

touch file-{1..50000}
# time find ./ -type f -name "file-*" -exec rm -f {} \;
# time find ./ -type f -name "file-*" |xargs rm -f
```

9.3 find逻辑运算符【-a -o ！】

```bash
#符号 作用

-a 与 and 默认写法不需加

-o 或 or

! 非 -not
```

## 10.文件打包压缩

**常见压缩包格式**

```bash
格式				压缩工具
.zip			zip压缩工具
.gz				gzip压缩工具，只能压缩文件，会删除原文件(通常配合tar使用)
.bz2			bzip2压缩工具，只能压缩文件，会删除原文件(通常配合tar使用)
.tar.gz			先使用tar命令归档打包，然后使用gzip压缩
.tar.bz2		先使用tar命令归档打包，然后使用bzip压缩
```

### 10.1 gzip【.gz】

使用gzip方式进行压缩文件 ( **只能压缩文件，并且文件被压缩后，源文件没有了**)

```bash
#参数
-d			#解压gzip压缩包

yum install gzip -y				#安装gzip压缩工具

gzip file		#压缩文件
zcat file.gz	#查看压缩文件
gzip -d file.gz	#解压压缩文件
```

### 10.2 gip【.zip】



**zip, 使用zip命令可以对文件进行压缩打包，解压则需要使用unzip命令**

```bash
yum install zip unzip -y

#zip参数
-r			#递归打包
-T			#查看压缩包是否完整

#unzip 参数
-l / -t		#不解压查看压缩包内容
-d			#指定解压目录

zip  filename.zip  filename 
zip -r  dir.zip dir/
zip -T  filename.zip

# unzip -l  filename.zip
# unzip -t  filename.zip
# unzip  filename.zip
# unzip filename.zip  -d /opt/
```

### 10.3 tar【.tar.gz|.tar.bz2】

**tar是linux下最常用的压缩与解压缩, 支持文件和目录的压缩归档**

```bash
c   #创建新的归档文件
x   #对归档文件解包
t   #列出归档文件里的文件列表
v   #输出命令的归档或解包的过程
f   #指定包文件名，多参数f写最后

z   #使用gzip压缩归档后的文件(.tar.gz)
j   #使用bzip2压缩归档后的文件(.tar.bz2)
J   #使用xz压缩归档后的文件(tar.xz)
C   #指定解压目录位置
X   #排除多个文件(写入需要排除的文件名称)
h   #打包软链接
--exclude   #在打包的时候写入需要排除文件或目录
```

### 常用打包与压缩组合

```bash
czf     #打包tar.gz格式
cjf     #打包tar.bz格式
cJf     #打包tar.xz格式

zxf     #解压tar.gz格式
jxf     #解压tar.bz格式
xf      #自动选择解压模式   常用写法自动选择解压
tf      #查看压缩包内容
```

```bash
#1.以gzip归档方式打包并压缩
tar czf  test.tar.gz  test/ test2/

#2.以bz2方式压缩
yum install bzip2 -y
tar cjf  test.tar.bz2 dir.txt dir/

#3.打包链接文件,打包链接文件的真实文件[加参数-h]
cd /
[root@ /]# tar czfh local.tar.gz  etc/rc.local

#4.打包/tmp下所有文件
cd /
[root@ /]# find tmp/ -type f | xargs tar czf tmp.tar.gz

方法二:
[root@ /]# tar czf tmp2.tar.gz $(find /tmp/ -type f)    #会优先执行$()里的内容并作为参数传递
```

总结

```bash
tar 
	czf	#tar.gz    
	cjf	#tar.bz2
tar
	tf	浏览包内容

tar
	xf	智能解压
	tar  xf xx.tar.gz -C /opt   #-C 指定解压到opt目录


打包一个tar.gz的压缩包，压缩/etc /opt /tmp  ,检查压缩包的中内容，最后解压到当前目录。
 tar czf eot.tar.gz /etc/ /opt/ /tmp/
 tar tf eot.tar.gz
 tar xf eot.tar.gz
```

### 10.4 --exclude 排除打包文件

```bash
#排除单个文件
 tar czf etc.tar.gz --exclude=etc/services etc/
 
将需要排除的文件写入文件中
[root@ opt]# cat ~/pc.txt
etc/sysconfig/network-scripts/ifcfg-eth0
etc/services
etc/rc.local

#指定需要排除的文件列表, 最后进行打包压缩
语法:tar 参数 压缩包名称 要排除的文件 打包的内容
[root@ ~]# tar czfX oo3.tar.gz  pc.txt  /etc/
tar: 从成员名中删除开头的“/”
#由于打包时使用的是全路径，默认系统会把开头的/删除，但是我们在解压时 指定解压到 / 下面
[root@db01-172 ~]# cp oo3.tar.gz  /opt/
[root@db01-172 ~]# cd /opt/
[root@db01-172 opt]# tar xf oo3.tar.gz
[root@db01-172 opt]# find etc/ -type f |grep "ifcg*"
etc/sysconfig/network-scripts/ifcfg-lo
[root@db01-172 opt]# find etc/ -type f |grep "services"
[root@db01-172 opt]# find etc/ -type f |grep "rc.local"
etc/rc.d/rc.local
```

## 11. rpm包管理

**rpm软件包的组成部分有哪些？**

```bash
bash-doc-4.2.46-34.el7.x86_64.rpm

bash-doc：包名称
4.2.46：版本号
-34：发布次数
el7：表示软件包可以在 Red Hat 7.x，CentOS 7.x，CloudLinux 7.x 进行安装
.x86_64：硬件平台https://blog.csdn.net/sunny_day_day/article/details/108714946
.rpm:扩展名
```

**使用rpm工具安装rpm包**

| 选项     | 描述             |
| :------- | :--------------- |
| -i       | 安装rpm          |
| -v       | 显示安装详细信息 |
| -h       | 显示安装rpm进度  |
| --force  | 强制重新安装     |
| --nodeps | 忽略依赖关系     |

```bash
#参数 
-ivh 		安装npm包
-iUh		升级安装的npm包
-e			卸载软件

rpm -q		查看指定软件包是否安装
rpm -qa		查看系统中已安装的所有RPM软件包列表
rpm -qi		查看指定软件的详细信息
rpm -ql		查询指定软件包所安装的目录、文件列表
rpm -qc		查询指定软件包的配置文件
rpm -qf		查询文件或目录属于哪个RPM软件
rpm -qip	查询未安装的rpm包详细信息
rpm -qlp	查询未安装的软件包会产生哪些文件
```

## 12. yum包管理

```bash
yum localinstall		#安装本地rpm包会自动解决依赖

yum install			安装
yum reinstall		重装
yum update			更新
yum remove 			删除
yum repolist 	  	查看仓库总和的rpm包
yum repolist all 	查看所有的仓库 （ 包括启用和禁用 ）
yum provides		查询命令是 哪个仓库下的哪个软件包提供
yum groups install	安装组包
yum groups remove 	移除组包
yum history 
yum history info
yum history undo 
yum clean all		清理所有的缓存
yum clean packages	清理所有已缓存的rpm包

#更新缓存
yum clean all
yum makecache
```

## 13. 磁盘管理

### 13.1、fdisk 磁盘分区

**fdisk只能划分小于2TB的磁盘，所以要添加一块小于2TB的硬盘**

```bash
输入m进入帮助显示信息如下：
[root@web01 ~]# fdisk -l
[root@web01 ~]# fdisk  /dev/sdb
Command (m for help): m         #输入m列出常用的命令
Command action
   a   toggle a bootable flag               #切换分区启动标记
   b   edit bsd disklabel                   #编辑sdb磁盘标签
   c   toggle the dos compatibility flag    #切换dos兼容模式
   d   delete a partition                   #删除分区
   l   list known partition types           #显示分区类型
   m   print this menu                      #显示帮助菜单
   n   add a new partition                  #新建分区
   o   create a new empty DOS partition table   #创建新的空白分区表
   p   print the partition table            #显示分区表的信息
   q   quit without saving changes          #不保存退出
   s   create a new empty Sun disklabel     #创建新的Sun磁盘标签
   t   change a partitions system id        #修改分区ID,可以通过l查看id
   u   change display/entry units           #修改容量单位,磁柱或扇区
   v   verify the partition table           #检验分区表
   w   write table to disk and exit         #保存退出
   x   extra functionality (experts only)   #拓展功能

#1.fdisk创建主分区
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)  #主分区
   e   extended  #扩展分区
Select (default p): p   #选择创建主分区
Partition number (1-4, default 1):  #默认创建第一个主分区
First sector (2048-2097151, default 2048): #默认扇区回车
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151): +50M #分配空间50MB


#2.fdisk创建扩展分区
Command (m for help): n  #新建分区
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): e   #创建扩展分区
Partition number (2-4, default 2):
First sector (104448-2097151, default 104448):
Using default value 104448
Last sector, +sectors or +size{K,M,G} (104448-2097151, default 2097151): #空间都给到扩展分区

#3.fdisk创建逻辑分区
Command (m for help): n  #新建分区
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l   #创建逻辑分区
Adding logical partition 5
First sector (106496-2097151, default 106496):
Using default value 106496
Last sector, +sectors or +size{K,M,G} (106496-2097151, default 2097151): +100M  #分配100MB空间

#4.fdisk查看分区情况，并保存
Command (m for help): p #查看分区创建
Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048      104447       51200   83  Linux
/dev/sdb2          104448     2097151      996352    5  Extended
/dev/sdb5          106496      311295      102400   83  Linux

#保存分区
Command (m for help): w
The partition table has been altered!
Calling ioctl() to re-read partition table.
Syncing disks.
```

小坑：分区失败删除了,系统为生效,可以安装partprobe指令刷新内核创建完成后，可以尝试检查磁盘是否为gpt格式

```bash
[root@xuliangwei-node1 /]# fdisk /dev/sdb -l|grep type
Disk label type: gpt

安装parted, 刷新内核立即生效,无需重启
[root@ ~]# yum -y install parted
[root@ ~]# partprobe
```

### 13.2、mkfs 格式分区

**mkfs格式化磁盘，实质创建文件系统，文件系统类似于将房子装修成3室一厅，还是2室一厅。**

```bash
#选项: 
# -b  设定数据区块占用空间大小，目前支持1024、2048、4096 bytes每个块。
# -t  用来指定什么类型的文件系统，可以是ext4, xfs
# -i  设定inode的大小
# -N  设定inode数量，防止Inode数量不够导致磁盘不足

#1.格式化整个磁盘
[root@xuliangwei ~]# mkfs.ext4  /dev/sdb 

#2.格式化磁盘的某个分区
[root@xuliangwei ~]# mkfs.xfs  /dev/sdb1
```

### 13.3、mount 挂载

如果需要使用该磁盘的空间，需要准备一个空的目录作为挂载点，与该设备进行关联。

**1.临时挂载**

```bash
# 选项：-t指定文件系统挂载分区 -a 挂载/etc/fstab中的配置文件 -o 指定挂载参数
# 挂载/dev/sdb1至db1目录
[root@ ~]# mkdir /db1
[root@ ~]# mount -t xfs /dev/sdb1  /db1/ 
```

**2.umount卸载**

挂载的磁盘，如果不想使用可以使用umount进行卸载。

```bash
#选项：
-l 强制卸载
-o 指定挂载参数，默认是读写rw，只读or
mount -o or /dev/sdc1 /data-c/            

#1.卸载目录方式
[root@ ~]# umount /db1

#2.卸载设备方式
[root@ ~]# umount /dev/sdb1

#3.umount不能卸载的情况
[root@ db1]# umount /db1  
umount: /db1: device is busy.
        (In some cases useful info about processes that use
         the device is found by lsof(8) or fuser(1)

#PS: 如上情况解决办法有两种, 1.切换至其他目录 2.使用'-l'选项强制卸载    
[root@student db1]# umount -l /db1
```

**2.永久挂载**

永久生效，就需要配置一个文件   /etc/fstab   （开机会加载该文件中的设备）

**/etc/fstab配置文件编写格式**

```bash
要挂载的设备	挂载点(入口)	文件系统类型	挂载参数		是否备份	是否检查
/dev/sdb1	/db1		  xfs		defaults		0			0

#第一列：可以使用UUID也可以使用设备名称
[root@container ~]# blkid                                   #查看磁盘的UUID
/dev/sda1: UUID="723eb45d-9a1b-4e8f-b06d-cf9024302147" TYPE="xfs"
/dev/sda2: UUID="yVHSU3-Cmkf-qsg9-6DNg-P87L-mCDN-VOHvcO" TYPE="LVM2_member"
/dev/sdb1: UUID="3b0143a3-39af-4991-afab-01227fbe767c" TYPE="xfs"

#第四列：挂载参数。挂载参数有很多，在这块我们了解即可，不必深究。【一般使用default】
参数			含义
async/sync	是否为同步方式运行。默认async
user/nouser	是否允许普通用户使用mount命令挂载。默认nouser
exec/noexe	是否允许可执行文件执行。默认exec
suid/nosuid	是否允许存在suid属性的文件。默认suid
auto/noauto	执行mount -a 命令时，此文件系统是否被主动挂载。默认auto
rw/ro		是否以只读或者读写模式进行挂载。默认rw
default		具有rw,suid,dev,exec,auto,nouser,async等默认参数的设定。

#第五列：是否进行备份。通常这个参数的值为0或者1				【不备份】
选项	含义
0	代表不做备份
1	代表要每天进行备份操作
2	代表不定日期的进行备份操作

#第六列：是否检验扇区：开机的过程中，系统默认会以fsck检验我们系统是否为完整	【不检查】
选项	含义
0	不要检验磁盘是否有坏道
1	检验
2	校验 (当1级别检验完成之后进行2级别检验)


#示例：
[root@container ~]# tail  /etc/fstab           #进入开机加载文件添加一个挂载
/dev/sdb1 	/data1		xfs	defaults 0 0
```

**如何知道写的对于不对   mount -a    (   会去加载/etc/fstab中的 挂载信息   )**

```bash
[root@container ~]# mount -a

[root@container ~]# df -h               #检查挂载点
文件系统                 容量  已用  可用 已用% 挂载点
/dev/sdb1               20G   33M  20G  1% /data1		《--读取/etc/fstab 加载该设备
```

### 13.4、gdisk 磁盘分区(大于2T)

1、能够识别大于2TB的磁盘存储空间

2、可以支持分区的数量  高达  128个 主分区      没有扩展，逻辑分区的概念。

3、MBR 磁盘  和  GPT  不能互相转换、容易造成数据丢失。

**使用gdisk工具分区**

```bash
#1.安装gdisk，没有gdisk需要yum下载一个
yum install gdisk -y


#2.创建一个新分区，500MB大小
[root@ ~]# gdisk /dev/sdb
Command (? for help): n     #创建新分区
Partition number (1-128, default 1):
First sector (34-2097118, default = 2048) or {+-}size{KMGTP}:
Last sector (2048-2097118, default = 2097118) or {+-}size{KMGTP}: +500M #分配500M大小

Command (? for help): p #打印查看
Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048         1026047   500.0 MiB   8300  Linux filesystem

Command (? for help): w #保存分区
Do you want to proceed? (Y/N): y    #确认
OK; writing new GUID partition table (GPT) to /dev/sdb.
The operation has completed successfully.


#3.创建完成后，可以尝试检查磁盘是否为gpt格式
[root@xuliangwei-node1 /]# fdisk /dev/sdb -l|grep type
Disk label type: gpt

#4.安装parted, 刷新内核立即生效,无需重启
[root@ ~]# yum -y install parted
[root@ ~]# partprobe /dev/sdb

2.使用mkfs进行格式化磁盘。前面已经介绍过，此处不反复介绍。
[root@xuliangwei ~]# mkfs.xfs  /dev/sdb

3.使用mount命令将某个目录挂载该分区，进行使用。
[root@ ~]# mkdir /data_gdisk
[root@ ~]# mount /dev/sdb /data_gdisk
```

### 13.5、swap 物理内存

Swap分区在系统的物理内存不够时，将硬盘空间中的一部分空间释放出来，以供当前运行的程序使用。

**PS: 当物理内存不够时会随机kill占用内存的进程，从而产生oom，临时使用swap可以解决。**

```bash
[root@container ~]# swapoff -a          #临时取消swap分区
[root@container ~]# free -m		    	#查看swap分区
[root@container ~]# swapon -a           #开启swap分区
```

**如何将磁盘空间划分一部分给swap使用**

```
1.先找一块硬盘，划分1Gb空间		  fdisk
2.格式化为一个swap的设备     	   mkswap /dev/sdb1                #格式化前要先卸载挂载
3.通过						    swapon -a /dev/sdb1  将该设备加入swap
4.使用free -m 检查swap的大小		 free -m
5.如果不想使用swap了				 swapoff /dev/sdb1   移除
```

**------如上操作都是临时的，如需永久生效，需要添加到/etc/fstab！！！！！！**

### 13.6 、df 、du查看磁盘空间

**df 使用**

```bash
#参数
-h			#以人类可读的方式显示(GB\MB\KB)磁盘使用情况
-T			#显示各个文件系统的类型。
--total		#将在输出列表的底部显示总计信息
-i			#显示 inode 信息
```

**du 使用**

 `du` 命令查找目录下占用空间较大的文件和子目录 

```bash
du -a /path/to/directory | sort -n -r | head -n N

其中：
/path/to/directory 是您要检查的目录路径，可以是相对路径或绝对路径。
N 是您希望查看的最大文件／子目录的数量。
```

区别

```bash
du 和 df 都是用于检查磁盘空间的 Unix、Linux 和 macOS 系统命令。尽管它们都与磁盘空间分析相关，但它们的功能和目标有所不同。

#du（Disk Usage）:
du 主要用于分析文件和目录占用的磁盘空间。它可以报告给定文件或目录（及其子目录）的磁盘使用情况，并显示详细信息。du 允许按文件和目录查找磁盘使用情况，并按需深入到嵌套子目录中。

一些常见的 du 用法：
显示指定目录及其子目录的大小：du /path/to/directory
以人类可读的格式显示目录大小：du -h /path/to/directory
显示指定文件的大小：du -h /path/to/file

#df（Disk Free）:
df 主要用于显示整个文件系统、挂载点或磁盘分区的磁盘空间使用情况。它提供了关于整个文件系统的统计数据，如总空间、已用空间、可用空间等。df 不会深入到文件或子目录级别，但可以给出关于整个文件系统的总体概述。

一些常见的 df 用法：
显示所有文件系统的磁盘空间使用情况：df
以人类可读的格式显示磁盘使用情况：df -h
显示指定文件系统的磁盘使用情况：df /path/to/mount/point

#总之，du 用于深入查找特定文件和目录的磁盘占用情况，而 df 用于查看整个文件系统或磁盘分区的磁盘空间使用概况。两者都是磁盘空间分析工具，但用途和焦点不同。
```

### 13.7  inode使用查询

```bash
1、使用 df 命令查看文件系统的 inode 使用情况。使用 -i 选项显示 inode 信息：
# df -i

2、要查找拥有大量 inode 的目录，可以使用 find 命令配合 wc -l 命令，例如：
for i in /*; do echo -n "$i: " && find "$i" -print | wc -l; done | sort -n -k2 -r

这条命令递归遍历根目录（/*）下的所有子目录，并显示每个目录占用的 inode 数量，按降序排列。
```

**inode 的管理对于确保文件系统正常工作和优化磁盘空间使用极为重要。在进行磁盘监控和优化时，请确保关注 inode 使用情况。**

## 14. 进程管理

**程序在运行后，我们需要了解进程的运行状态。查看进程的状态分为: 静态和动态两种方式**

 **ps:每个进程的父进程都叫PPID,子进程则叫PID**

### 14.1 ps aux 静态进程信息查看

```bash
#常用参数
ps				#显示当前终端内的进程
ps -e			#显示全部进程
ps -ef			#以完整的格式显示全部进程
ps -u username	#显示指定用户运行的进程
ps -l			#长格式显示当前终端内的进程信息
ps -aux			#显示系统中所有用户的进程信息，而无论它们是否与终端关联。
```

```bash
USER  	 		进程运行的用户身份（ 每一个进程，都需要一个特定的用户身份来运行 ）
PID				子进程的身份标识 （ 就是一种标识，用来区分不同的进程 ）
%CPU			该进程占用CPU的百分比是多少
%MEM			该进程占用内存的百分比是多少
VSZ				虚拟内存
RSS				实际占用内存   
TTY				该进程是哪个终端运行的  ? 表示是系统运行的	pts/0 pts/1 来源的终端是哪一个
STAT			进程所表示的状态（ 运行 暂停 停止 .......）
START			进程启动时间
TIME			进程占用CPU的时间
COMMAND			运行该进程需要执行的命令	[ ] 表示内核启动的进程


#STAT状态
- STAT基本状态——描述						
  R	：进程运行								
  S	：可中断进程						
  T	：进程被暂停							
  D	：不可中断进程						
  Z	：僵尸进程								
- STAT状态+符号——描述
  s	：进程是控制进程， Ss进程的领导者，父进程
  <	：进程运行在高优先级上，S<优先级较高的进程
  N	：进程运行在低优先级上，SN优先级较低的进程
  ＋：当前进程运行在前台，R+该表示进程在前台运行
  l	：进程是多线程的，Sl表示进程是以线程方式运行
```

### 14.2 top 动态查看

w:
	11:20:10 up 6 days,  7:46,  2 users,  load average: 0.01, 0.02, 0.05
uptime:
	11:20:14 up 6 days,  7:46,  2 users,  load average: 0.01, 0.02, 0.05

top:查看前三行显示信息
第一行,任务对列信息同uptime命令执行结果一样

- top - 11:18:15 up 6 days,  7:44,  2 users,  load average: 0.08, 0.03, 0.06

> 11:18:15					当前系统时间 
> up 6 days,7:44	 	系统运行天数,小时
> 2 users 					当前2个用户登录
> load average: 0.08, 0.03, 0.06	后面三组数分别是1分钟,五分钟,十五分钟的负载情况

第二行,tasks-任务(进程)
Tasks: 112 total,   1 running, 110 sleeping,   1 stopped,   0 zombie

> 系统共有112个进程,其中处于运行的1个,110个在休眠(sleep),stopped状态的1个,zombie状态(僵尸)的有零个

第三行,CPU状态信息
%Cpu(s):  0.3 us,  0.3 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

```bash
us:	用户进程占用cpu百分比（ 视频加密、视频解码 ）
sy:	内核进程占用cpu百分比 （ 比如 网卡、硬件设备、硬盘 ）
ni:	优先级高的进程占用cpu百分比  
id:	空闲的百分比 （ 值越大、说明服务器越空闲 ）
wa:	用户请求磁盘资源，磁盘很慢慢慢，请求的资源很多,会造成大量的等待程序    （ 数据库 ）
hi:	硬中断
si:	软中断
st:	当该服务器运行了很多的虚拟机，这些虚拟机总共占用当前物理服务器的百分比是多少
```

不可能用top去每台服务器观察，后期将所有的服务器通过监控，统一的监控起来。
top 常见指令

```bash
字母					        含义
h						查看帮助
1						数字1，显示所有CPU核心的负载
z						以高亮显示数据
b						高亮显示处于R状态的进程
M						按内存使用百分比排序输出
P						按CPU使用百分比排序输出
q						退出top
```

总结

```bash
>1.进程的生命周期
	僵尸进程（会占用资源）
	孤儿进程（）
2.系统指标监控
	ps  静态
	top 动态
3.中断
	软中断	硬中断
```

#### nice 进程优先级

在启动进程时，为不同的进程使用不同的调度策略。
nice 值越高： 表示优先级越低，例如+19，该进程容易将CPU 使用量让给其他进程。
nice 值越低： 表示优先级越高，例如-20，该进程不倾向于让出CPU。 最高的优先级

```bash
低       正常      高
+20      0       -20
```

**nice 指定程序运行优先级**

```bash
#语法格式 
nice -n 优先级数字 进程名称

#1.开启vim并且指定程序优先级为-5
[root@m01 ~]# nice -n -5 vim &
[1] 98417

#2.查看该进程的优先级情况
[root@m01 ~]# ps axo pid,command,nice |grep 98417
 98417 vim                         -5
```

**renice 修改正在运行程序的优先级**

```bash
#语法格式 
renice -n 优先级数字 进程pid

#1.查看vim的进程id,名称,nice(优先级)
[root@syc~]# ps axo pid,command,nice |grep vim |grep -v grep	#查找后进行过滤
 3621 vim                         -20

#2.修改vim的优先级为-20
[root@syc~]# renice -n -20 3621
3621 (进程 ID) 旧优先级为 0，新优先级为 -20

#3.查看vim的优先级(这个过滤的是id号,什么过滤的是名字)
[root@syc~]# ps axo pid,command,nice |grep 3621
 3621 vim                         -20
 3629 grep --color=auto 3621      -20
```



### 14.3 kill 管理进程状态

当程序运行为进程后，如果希望停止进程，怎么办呢? 那么此时我们可以使用linux的kill命令对进程发送关闭信号。当然除了kill、还有killall，pkill

1.使用kill -l列出当前系统所支持的信号

```bash
# kill -l
```

虽然linux支持信号很多，但是我们仅列出我们最为常用的3个信号

| 数字编号 | 信号含义 | 信号翻译                     |
| :------- | :------- | :--------------------------- |
| 1        | SIGHUP   | 通常用来重新加载配置文件     |
| 9        | SIGKILL  | 强制杀死进程                 |
| 15       | SIGTERM  | 终止进程，默认kill使用该信号 |

Linux系统中的killall、pkill命令用于杀死指定名字的进程。我们可以使用kill命令杀死指定进程PID的进程，如果要找到我们需要杀死的进程，我们还需要在之前使用ps等命令再配合grep来查找进程，而**killall**、**pkill**把这两个过程合二为一，是一个很好用的命令。

```bash
#1、通过服务名称杀掉进程
[root@syc~]# killall vsftpd
[root@syc~]# pkill vsftpd
#2.使用pkill踢出从远程登录到本机的用户，终止pts/0上所有进程, 并且bash也结束（用户被强制退出）
[root@syc~]# pkill -9 -t pts/0
```

### 14.4 jobs、bg、fg的使用

```bash
[root@xuliangwei ~]# sleep 3000 & 		//运行程序(时)，让其在后台执行 
[root@xuliangwei ~]# sleep 4000 		//^Z,将前台的程序挂起(暂停)到后台 
[2]+ Stopped sleep 4000
[root@xuliangwei ~]# ps aux |grep sleep
[root@xuliangwei ~]# jobs  				//查看后台作业
[1]- Running sleep 3000 & 
[2]+ Stopped sleep 4000

[root@xuliangwei ~]# bg %2     			//让作业 2 在后台运行
[root@xuliangwei ~]# fg %1     			//将作业 1 调回到前台
[root@xuliangwei ~]# kill %1   			//kill 1，终止 PID 为 1 的进程

[root@xuliangwei ~]# (while :; do date; sleep 2; done) & //进程在后台运行，但输出依然在当前终端
[root@xuliangwei ~]# (while :; do date; sleep 2; done) &>/dev/null &
```



### 14.5 screen 管理后台进程

```bash
选项:
-list		查看打开的新窗口
-r			加窗口id	进入窗口
-s			指定窗口名称

#1.打开新的窗口
[root@syc~]# screen 
2.指定打开一个名字为wyk的窗口-----加大S
[root@syc~]# screen -S WYK
[root@syc~]# screen -list				##查看所有窗口--------加-list
There is a screen on:
        3285.WYK        (Attached)
1 Socket in /var/run/screen/S-root.

现在已经在新打开的wyk窗口下可以执行一个top查看进程状态,Ctrl+a+d平滑退出窗口,这个命令还会在后台运行,
就算关了终端也不会退出,也是需要exit来关闭新建的screen窗口

#2平滑的退出screen,但不会终止screen中的任务。注意: 如果使用exit 才算真的关闭screen窗口

#3进入正在运行的screen	 -r可以加id号也可以加名字进入
[root@syc~]# screen -r wyk
[root@syc~]# screen -r 3285
```



## 15. crond 计划任务

1.系统级别的定时任务： 临时文件清理、系统信息采集、日志文件切割

2.用户级别的定时任务： 定时向互联网同步时间、定时备份系统配置文件、定时备份数据库的数据

```bash
[root@ ~]# vim /etc/crontab
SHELL=/bin/bash                     #执行命令的解释器
PATH=/sbin:/bin:/usr/sbin:/usr/bin  #环境变量
MAILTO=root                         #邮件发给谁
# Example of job definition:
# .---------------- minute (0 - 59) #分钟
# |  .------------- hour (0 - 23)   #小时
# |  |  .---------- day of month (1 - 31)   #日期
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr #月份
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat  #星期
# |  |  |  |  |
# *  *  *  *  *   command to be executed

# *  表示任意的(分、时、日、月、周)时间都执行
# -  表示一个时间范围段, 如5-7点
# ,  表示分隔时段, 如6,0,4表示周六、日、四
# /1 表示每隔n单位时间, 如*/10 每10分钟
```

2.了解crontab的时间编写规范

```bash
00 02 * * * ls      #每天的凌晨2点整执行
00 02 1 * * ls      #每月的1日的凌晨2点整执行
00 02 14 2 * ls     #每年的2月14日凌晨2点执行
00 02 * * 7 ls      #每周天的凌晨2点整执行
00 02 * 6 5 ls      #每年的6月周五凌晨2点执行
00 02 14 * 7 ls     #每月14日或每周日的凌晨2点都执行
00 02 14 2 7 ls     #每年的2月14日或每年2月的周天的凌晨2点执行   
*/10  02 * * * ls   #每天凌晨2点，每隔10分钟执行一次
* * * * *  ls       #每分钟都执行
00 00 14 2 *  ls    #每年2月14日的凌晨执行命令 
*/5 * * * *  ls     #每隔5分钟执行一次
00 02 * 1,5,8 * ls  #每年的1月5月8月凌晨2点执行
00 02 1-8 * *  ls    #每月1号到8号凌晨2点执行
0 21 * * * ls       #每天晚上21:00执行
45 4 1,10,22 * * ls #每月1、10、22日的4:45执行
45 4 1-10 * * l     #每月1到10日的4:45执行
3,15 8-11 */2 * * ls #每隔两天的上午8点到11点的第3和第15分钟执行
0 23-7/1 * * * ls   #晚上11点到早上7点之间，每隔一小时执行
15 21 * * 1-5 ls    #周一到周五每天晚上21:15执行
```

### **crontab使用**

```bash
#参数	含义
-e	编辑定时任务
-l	查看定时任务
-r	删除定时任务
-u	指定其他用户

1.使用root用户每5分钟执行一次时间同步

#1.如何同步时间
[root@xuliangwei ~]# ntpdate time.windows.com &>/dev/null
#2.配置定时任务
[root@xuliangwei ~]# crontab -e -u root
[root@xuliangwei ~]# crontab -l -u root
*/5 * * * * ntpdate time.windows.com &>/dev/null

2.每天的下午3,5点，每隔半小时执行一次sync命令
[root@xuliangwei ~]# crontab -l
*/30 15,17 * * * sync &>/dev/null


3.案例：每天凌晨3点做一次备份？备份/etc/目录到/backup下面
1) 将备份命令写入一个脚本中
2) 每天备份文件名要求格式: 2019-05-01_hostname_etc.tar.gz
3) 在执行计划任务时，不要输出任务信息
4) 存放备份内容的目录要求只保留三天的数据
#1.实现如上备份需求
[root@xuliangwei ~]# mkdir /backup
[root@xuliangwei ~]# tar zcf $(date +%F)_$(hostname)_etc.tar.gz /etc
[root@xuliangwei ~]# find /backup -name “*.tar.gz” -mtime +3 -exec rm -f {}\;

#2.将命令写入至一个文件中
[root@xuliangwei ~]# vim /root/back.sh
mkdir /backup
tar zcf $(date +%F)_$(hostname)_etc.tar.gz /etc
find /backup -name “*.tar.gz” -mtime +3 -exec rm -f {}\;

#3.配置定时任务
[root@xuliangwei ~]# crontab -l
00 03 * * * bash /root/back.sh  &>/dev/null

#3.备份脚本
[root@container ~]# cat /opt/backup.sh            
#!/usr/bin/bash
 
#1.使用tar命令备份/etc目录到backup目录
mkdir -p /backup
tar czf /backup/$(date +%F_%H_%M)_$(hostname)_etc.tar.gz /etc/
 
#2.保留最近3天的数据，其余全部删除
find /backup -type f -name "*.tar.gz" -mtime +3 -delete
```

**注意事项**

```bash
1) 给定时任务注释
2) 将需要定期执行的任务写入Shell脚本中，避免直接使用命令无法执行的情况tar date
3) 定时任务的结尾一定要有&>/dev/null或者将结果追加重定向>>/tmp/date.log文件
4) 注意有些命令是无法成功执行的 echo "123" >>/tmp/test.log &>/dev/null
5.如果一定要是用命令，命令必须使用绝对路径

5.crond如何备份
1) 通过查找/var/log/cron中执行的记录，去推算任务执行的时间
2) 定时的备份/var/spool/cron/{usernmae}

6.crond如何拒绝某个用户使用
#1.使用root将需要拒绝的用户加入/etc/cron.deny
[root@ ~]# echo "yikun" >> /etc/cron.deny

#2.登陆该普通用户，测试是否能编写定时任务
[oldboy@yikun ~]$ crontab -e
You (yikun) are not allowed to use this program (crontab)
See crontab(1) for more information
```

**计划任务如何调试**

```bash
1.手动执行命令，然后保留执行成功的结果。
2.编写脚本
脚本需要统一路径/scripts
脚本内容复制执行成功的命令(减少每个环节出错几率)
脚本执行的输出信息可以重定向至其他位置保留或写入/dev/null
3.执行脚本
使用bash命令执行, 防止脚本没有增加执行权限(/usr/bin/bash)
执行脚本成功后，复制该执行的命令，以便写入cron
4.编写计划任务
加上必要的注释信息, 人、时间、任务
设定计划任务执行的周期
粘贴执行脚本的命令(不要手敲)
5.调试计划任务(进行确认检查)
增加任务频率测试
检查环境变量问题
检查crond服务日志
```

## 16. 系统排查

 **我们以三个示例分别来看这三种情况，并用 stress、mpstat、pidstat 等工具，找出平均负载升高的根源。**
stress 是 Linux 系统压力测试工具，这里我们用作异常进程模拟平均负载升高的场景。
mpstat 是多核 CPU 性能分析工具，用来实时查看每个 CPU 的性能指标，以及所有 CPU 的平均指标。
pidstat 是一个常用的进程性能分析工具，用来实时查看进程的 CPU、内存、I/O 以及上下文切换等性能指标。 

```bash
#如果出现无法使用mpstat、pidstat命令查看%wait指标建议更新下软件包
wget http://pagesperso-orange.fr/sebastien.godard/sysstat-11.7.3-1.x86_64.rpm
rpm -Uvh sysstat-11.7.3-1.x86_64.rpm
```

### 16.1 CPU 密集型进程

*1.首先，我们在第一个终端运行 stress 命令，模拟一个 CPU 使用率 100% 的场景：*

```bash
[root@m01 ~]# stress --cpu 1 --timeout 600
```

*2.接着，在第二个终端运行 uptime 查看平均负载的变化情况*

```bash
# 使用watch -d 参数表示高亮显示变化的区域(注意负载会持续升高)
[root@m01 ~]# watch -d uptime
17:27:44 up 2 days,  3:11,  3 users,  load average: 1.10, 0.30, 0.17
```

*3.最后，在第三个终端运行 mpstat 查看 CPU 使用率的变化情况*

```bash
# -P ALL 表示监控所有 CPU，后面数字 5 表示间隔 5 秒后输出一组数据
[root@m01 ~]# mpstat -P ALL 5
Linux 3.10.0-957.1.3.el7.x86_64 (m01)   2019年04月29日     _x86_64_    (1 CPU)

17时32分03秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
17时32分08秒  all   99.80    0.00    0.20    0.00    0.00    0.00    0.00    0.00    0.00    0.00
17时32分08秒    0   99.80    0.00    0.20    0.00    0.00    0.00    0.00    0.00    0.00    0.00

#单核CPU所以只有一个all和0
```

*4.从终端二中可以看到，1 分钟的平均负载会慢慢增加到 1.00，而从终端三中还可以看到，正好有一个 CPU 的使用率为 100%，但它的 iowait 只有 0。这说明，平均负载的升高正是由于 CPU 使用率为 100% 。那么，到底是哪个进程导致了 CPU 使用率为 100% 呢？可以使用 pidstat 来查询*

```bash
# 间隔 5 秒后输出一组数据
[root@m01 ~]# pidstat -u 5 1
Linux 3.10.0-957.1.3.el7.x86_64 (m01)   2019年04月29日     _x86_64_(1 CPU)

17时33分21秒   UID       PID    %usr %system  %guest    %CPU   CPU  Command
17时33分26秒     0    110019   98.80    0.00    0.00   98.80     0  stress

#从这里可以明显看到，stress 进程的 CPU 使用率为 100%。
```

### 16.2 I/O 密集型进程

*1.首先还是运行 stress 命令，但这次模拟 I/O 压力，即不停地执行 sync*

```bash
[root@m01 ~]# stress  --io 1 --timeout 600s
```

*2.然后在第二个终端运行 uptime 查看平均负载的变化情况：*

```bash
[root@m01 ~]# watch -d uptime
18:43:51 up 2 days,  4:27,  3 users,  load average: 1.12, 0.65, 0.00
```

*3.最后第三个终端运行 mpstat 查看 CPU 使用率的变化情况：*

```bash
# 显示所有 CPU 的指标，并在间隔 5 秒输出一组数据
[root@m01 ~]# mpstat -P ALL 5
Linux 3.10.0-693.2.2.el7.x86_64 (bgx.com)   2019年05月07日     _x86_64_    (1 CPU)

14时20分07秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
14时20分12秒  all    0.20    0.00   82.45   17.35    0.00    0.00    0.00    0.00    0.00    0.00
14时20分12秒    0    0.20    0.00   82.45   17.35    0.00    0.00    0.00    0.00    0.00    0.00

#会发现cpu的与内核打交道的sys占用非常高
```

*4.那么到底是哪个进程，导致 iowait 这么高呢？我们还是用 pidstat 来查询*

```bash
# 间隔 5 秒后输出一组数据，-u 表示 CPU 指标
[root@m01 ~]# pidstat -u 5 1
Linux 3.10.0-957.1.3.el7.x86_64 (m01)   2019年04月29日     _x86_64_(1 CPU)
18时29分37秒   UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
18时29分42秒     0    127259   32.60    0.20    0.00   67.20   32.80     0  stress
18时29分42秒     0    127261    4.60   28.20    0.00   67.20   32.80     0  stress
18时29分42秒     0    127262    4.20   28.60    0.00   67.20   32.80     0  stress

#可以发现，还是 stress 进程导致的。
```

### 16.3 大量进程场景

*当系统中运行进程超出 CPU 运行能力时，就会出现等待 CPU 的进程。*

*1.首先，我们还是使用 stress，但这次模拟的是 4 个进程*

```bash
[root@m01 ~]# stress -c 4 --timeout 600
```

*2.由于系统只有 1 个 CPU，明显比 4 个进程要少得多，因而，系统的 CPU 处于严重过载状态*

```bash
[root@m01 ~]# watch -d uptime
19:11:07 up 2 days,  4:45,  3 users,  load average: 4.65, 2.65, 4.65
```

*3.然后，再运行 pidstat 来看一下进程的情况：*

```bash
# 间隔 5 秒后输出一组数据
[root@m01 ~]# pidstat -u 5 1
平均时间:   UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
平均时间:     0    130290   24.55    0.00    0.00   75.25   24.55     -  stress
平均时间:     0    130291   24.95    0.00    0.00   75.25   24.95     -  stress
平均时间:     0    130292   24.95    0.00    0.00   75.25   24.95     -  stress
平均时间:     0    130293   24.75    0.00    0.00   74.65   24.75     -  stress
```

*可以看出，4 个进程在争抢 1 个 CPU，每个进程等待 CPU 的时间（也就是代码块中的 %wait 列）高达 75%。这些超出 CPU 计算能力的进程，最终导致 CPU 过载。*

#### 总结

https://www.cnblogs.com/muahao/p/6346775.html

```bash
平均负载提供了一个快速查看系统整体性能的手段，反映了整体的负载情况。但只看平均负载本身，我们并不能直接发现，到底是哪里出现了瓶颈。所以，在理解平均负载时，也要注意：
平均负载高有可能是 CPU 密集型进程导致的；
平均负载高并不一定代表 CPU 使用率高，还有可能是 I/O 更繁忙了；
当发现负载高的时候，你可以使用 mpstat、pidstat 等工具，辅助分析负载的来源
```

## 17. systemctl 系统管理

**systemd相关配置文件**

```
/usr/lib/systemd/system/ 	#类似Centos6系统的启动脚本，/etc/init.d/
/etc/systemd/system/ 		#类似Centos6系统的/etc/rc.d/rcN.d/
/etc/systemd/system/multi-user.target.wants/
```

**systemd管理服务相关命令**
systemctl管理服务的启动、重启、停止、重载、查看状态等常用命令
针对当前正在运行的程 序

| systemctl命令                    | 作用                 |
| :------------------------------- | :------------------- |
| systemctl start crond.service    | 启动服务             |
| systemctl stop crond.service     | 停止服务             |
| systemctl restart crond.service  | 重启服务             |
| systemctl reload crond.service   | 重新加载配置         |
| systemctl status crond.servre    | 查看服务运行状态     |
| systemctl is-active sshd.service | 查看服务是否在运行中 |
| systemctl mask crond.servre      | 禁止服务运行         |
| systemctl unmask crond.servre    | 取消禁止服务运行     |

**当我们使用systemctl启动一个守护进程后，可以通过sysctemctl status查看此守护进程的状态**

| 状态            | 描述                                     |
| :-------------- | :--------------------------------------- |
| loaded          | 服务单元的配置文件已经被处理             |
| active(running) | 服务持续运行                             |
| active(exited)  | 服务成功完成一次的配置                   |
| active(waiting) | 服务已经运行但在等待某个事件             |
| inactive        | 服务没有在运行                           |
| enabled         | 服务设定为开机运行                       |
| disabled        | 服务设定为开机不运行                     |
| static          | 服务开机不启动，但可以被其他服务调用启动 |

**systemctl 设置服务开机启动、不启动、查看各级别下服务启动状态等常用命令**

```bash
systemctl命令（7系统）						作用

systemctl enable crond.service				开机自动启动
systemctl disable crond.service				开机不自动启动
systemctl list-unit-files					查看各个级别下服务的启动与禁用
systemctl is-enabled crond.service			查看特定服务是否为开机自启动
systemctl daemon-reload						创建新服务文件需要重载变更(更改服务后要启动不成功时会提示执行这个命令)
```
**systemctl的journalctl日志**

```
journalctl -n 20    #查看最后20行
journalctl -f       #动态查看日志
journalctl -p err   #查看日志的级别
journalctl -u crond #查看某个服务的单元的日志
```

# Linux基础知识

## 0.linux目录结构

```bash
[root@wyk ~]# ll /
usr								系统文件目录相当于c:\windows
lib -> usr/lib					库文件Glibc 32bit  汽车的螺丝			
lib64 -> usr/lib64				库文件Glibc 64bit  汽车的螺丝	
sbin -> usr/sbin   				管理员使用的命令    /sbin/shutdown, /sbin/reboot
bin -> usr/bin     				普通用户使用的命令   /bin/ls, /bin/date 
boot							存放系统启动相关的文件, 例如:kernel, grub(引导装载程序)
								grub（ 告诉你 有哪些内核，你可以选择需要加载的那一个）
   							 	vmlinuz-3.10.0-957.12.2.el7.x86_64  正常的系统内核
    							vmlinuz-0-rescue-93f219319dd5bdb4xx 统的救援内核
etc								系统配置文件
home							普通用户家目录
root							系统管理员家目录
proc							进程文件
tmp								临时目录
var								日志文件
/var/tmp						进程产生的临时文件
dev								设备目录文件(硬盘\光驱等等)
/dev   							存放设备文件,比如硬盘,硬盘分区,光驱,等等
/dev/null  						黑洞设备,只进不出.类似于垃圾回收站
/dev/random    					生成随机数设备
/dev/zero  						能源源不断的产生数据,类似于取款机,随时随地取钱

--------------------------------------------------------------
media       					挂载我们的u盘、或者其他设备
mnt     						挂载我们的u盘、或者其他设备
opt     						三方厂商包目录oracle  gitlab
run     						包含系统运行时所需要的文件。以前 /var/run
srv     						服务启动后需要访问的数据目录。使用很少| saltstack
sys     						sys和proc一样是虚拟文件系统，记录核心系统硬件信息。
```

## 1.文件属性类型

1.1.文件属性

```bash
整个文件的属性分为十列
#ls -l
-rw-r--r-- 1 root root 1019 Apr 28 15:41 pass.txt

-                   1：文件类型              
rw-r--r--           2：权限    
1                   3：表示硬链接数                
root                4：这个文件的拥有人是谁       （个人）
root                5：这个文件的拥有组是谁       （小组）
1019                6：文件大小
Apr 28 15:41        7、8、9：文件创建或修改的时间
pass.txt            10:文件的名称
```

1.2.文件类型

```bash
使用ll 或者 ls -l 能够区分出来的效果
-:  表示是一个文件( 普通文件、脚本文件、压缩文件、命令文件)
s:  socket,进程与进程之间的通讯协议
c:  字符设备（终端、键盘）
b:  块设备（磁盘）
l:  软链接（快捷方式）
d:  表示一个目录
```

1.3.file 判断文件类型

```bash
我们无法通过ls -l文件的类型，比如一个文件，它可能是普通文件、也可能是压缩文件、或者是命令文件等，那么此时就需要使用file来更加精准的判断这个文件的类型

file命令判断最准确
通过后缀判断文件类型 mp4 sh zip txt log........
```

## 2.用户、组管理

**1.用户管理**

**/etc/passwd**,记录了用户的信息 **/etc/shadow** 记录了用户的密码

```
# head -n1 /etc/passwd
root:x:0:0:root:/root:/bin/bash         #以:作为分隔符,总共七列
  1  2 3 4  5      6      7
分别为：
用户名称:密码占位符:用户UID:组GID:注释信息:用户家目录:登录shell
```

**系统对用户有一个约定**

| 用户UID | 系统中约定的含义                                          |
| ------- | --------------------------------------------------------- |
| 0       | 超级管理,最高权限,有着极强的破坏力                        |
| 1~200   | 系统用户,用来运行系统自带的进程,默认已创建                |
| 201~999 | 系统用户,用来运行用户安装的程序,所有此类用户无需登录系统  |
| 1000+   | 普通用户,正常可以登录系统用户,权限比较小,能执行的任务有限 |

 **用户创建流程**

```shell
#useradd创建用户时，系统会以/etc/login.defs、/etc/default/useradd 两个配置文件作为参照物，如果在创建用户时指定了参数则会覆盖etc/login.defs、/etc/default/useradd文件默认配置，如未指定则使用默认。

[root@test01 ~]# grep -E -v "^#|^$" /etc/login.defs 
MAIL_DIR        /var/spool/mail         创建邮箱的位置
PASS_MAX_DAYS   99999                   密码最长使用的天数
PASS_MIN_DAYS   0                       密码最短使用的天数
PASS_MIN_LEN    5                       密码长度
PASS_WARN_AGE   7                       密码到期前7天警告
UID_MIN                  1000           用户uid最小值
UID_MAX                 60000           用户uid最大值
SYS_UID_MIN               201           系统uid最小值
SYS_UID_MAX               999           系统uid最大值
GID_MIN                  1000           组id最小值
GID_MAX                 60000           组id最大值
SYS_GID_MIN               201           系统GID最大值
SYS_GID_MAX               999           系统GID最大值
CREATE_HOME yes                         是否创建加目录开关
UMASK           077                     权限位
USERGROUPS_ENAB yes                     创建用户默认组开关
ENCRYPT_METHOD SHA512                   密码加密算法

[root@test01 ~]# cat /etc/default/useradd 
# useradd defaults file
GROUP=100                   关闭默认创建组开关后，默认分配GID100的组
HOME=/home                  默认家目录
INACTIVE=-1                 用户失效时间，-1永久不失效
EXPIRE=                     过期时间
SHELL=/bin/bash             默认登录shell
SKEL=/etc/skel              默认用户拷贝的环境变量（命令提示符）
CREATE_MAIL_SPOOL=yes       是否开启邮箱开关

```

**2.组管理**

```
基本组：创建时指定组，就叫基本组，创建时可通过-g指定，如未指定则创建一个默认的组与用户同名  （只能有1个）
私有组：创建时没有指定组，默认创建与用户同名的组        （只有有1个）
附加组：基本组如果无法满足授权要求，可以将用户加入有权限的附加组，用户顺势继承该组的权限。当然附加组可以（有N多个)

   ps:
        1.首先得有组。
        2.在进行加入。
```

| 用户UID | 系统中约定的含义                                             |
| ------- | ------------------------------------------------------------ |
| 0       | 超级管理员，最高权限，有着极强的破坏能力                     |
| 1~200   | 系统用户，用来运行系统自带的进程，默认已创建                 |
| 201~999 | 系统用户，用来运行用户安装的程序，所以此类用户无需登录系统   |
| 1000+   | 普通用户，正常可以登陆系统的用户，权限比较小，能执行的任务有限 |

## 3. 文件权限管理

1、权限中的rwx是干什么的

| 字母        | 含义     | 对应权限 |
| :---------- | :------- | :------- |
| r(read)     | 读取权限 | 4        |
| w(write)    | 写入权限 | 2        |
| x(execute)  | 执行权限 | 1        |
| -(没有权限) | 没有权限 | 0        |

2、**权限中的rwx对文件的影响：**

```
读取权限（r）具有读取\阅读文件内容权限
1.只能使用查看类命令cat、head、tail、less、more

写入权限（w）具有新增、修改文件内容的权限
1.使用vim编辑会提示权限拒绝, 但可强制保存,会覆盖文件的所有内容
2.使用echo命令重定向的方式可以往文件内写入数据,  >>可以进行追加
3.不能删除文件,因为删除文件看的不是文件的属性,  需要看上级目录是否有w的权限

执行权限（x）具有执行文件的权限
1.执行权限什么用都没有
2.如果普通用户需要执行文件,需要配合r权限   rx （命令）  rw（配置文件） r(单纯的普通只看不改不执行)
```

3、**rwx对目录的影响**

```
读取权限（r），如果目录只有r权限: 具有浏览目录及子目录权限
	1.可以使用ls命令浏览目录及子目录， 但同时也会提示权限拒绝
	2.使用ls -l命令浏览目录及子目录，文件属性会带问号，并且只能看到文件名
	总结: 目录只有r权限，仅仅只能浏览内的文件名，无其他操作权限

写入权限（w），如果目录只有w权限: 具有增加、删除或修改目录内文件名权限(需要x权限配合)
	PS: 如果目录有w权限, 可以在目录内创建文件, 删除文件(跟文件本身权限无关)
	不能进入目录、不能复制目录、不能删除目录、不能移动目录

执行权限（x），如果目录只有x权限
	1.只能进入目录
	2.不能浏览、复制、移动、删除
```

总结

```bash
目录： 给予rx
1.拥有进入该目录的权限
2.如果想操作该目录下文件，那么就取决于该文件是否允许对应的用户进行操作
3.无法对该目录进行新增和删除文件

rx	目录	
	rw	文件（配置）
	rx	文件（脚本）
	r	文件（重要的文件，只能看，不能改，也不能执行。）
	
PS: 文件的 x权限小心给予，目录的 w权限小心给予。
PS: 文件通常设定的权限是644,目录设定的权限是755
PS: 控制目录权限755, 如果有普通用户需要操作目录里面的文件，在来看文件的权限
```

## 4. 输入输出

| 名称                 | 文件描述符 | 作用                                       |
| :------------------- | :--------- | :----------------------------------------- |
| 标准输入（STDIN）    | 0          | 默认是键盘，也可以是文件或其他命令的输出。 |
| 标准输出（STDOUT）   | 1          | 默认输出到屏幕。                           |
| 错误输出（STDERR）   | 2          | 默认输出到屏幕。                           |
| 文件名称（filename） | 3+....     |                                            |

**输出重定向，改变输出内容的位置。输出重定向有如下几种方式，如表格所示**

| 类型               | 操作符 | 用途                                                         |
| :----------------- | :----- | :----------------------------------------------------------- |
| 标准覆盖输出重定向 | >      | 将程序输出的正确结果输出到指定的文件中,会覆盖文件原有的内容  |
| 标准追加输出重定向 | >>     | 将程序输出的正确结果以追加的方式输出到指定文件，不会覆盖原有文件 |
| 错误覆盖输出重定向 | 2>     | 将程序的错误结果输出到执行的文件中，会覆盖文件原有的内容     |
| 错误追加输出重定向 | 2>>    | 将程序输出的错误结果以追加的方式输出到指定文件，不会覆盖原有文件 |
| 标准输入重定向     | <<     | 将命令中接收输入的途径由默认的键盘更改为指定的文件或命令     |

## 5. yum包管理

**一个repo文件内容**

```bash
[root@container ~]# cat /etc/yum.repos.d/test.repo
[local-oldboy]				#仓库名称，可随意表示
name = Local Packages		#仓库的描述 可通过 yum repolist查看
baseurl = file:///mnt       #我们的仓库在那里，使用什么协议访问有http:// ftp://  file://
enabled = 1				    #是否启用该仓库   1 表示启动  0表示不启用
gpgcheck = 0				#是否要校验软件包的合法性  （ 0 不校验 ）
```

## 6. 磁盘管理

 远程控制卡操作参考链接
https://www.bilibili.com/video/av46656120/?p=7 

**1.如何使用磁盘**

```bash
1.1 有一个磁盘
1.2 使用fdisk分区  大于2tb 使用 gdisk分区   （分一个区，或者直接使用整个磁盘）
1.3 需要进行格式化   mkfs.xfs
1.4 使用mount进行挂载
1.5 将挂载的信息添加到/etc/fstab  
1.6 使用 mount -a 执行没有任何错误，代表编写正常。重启设备会自动挂载上来
```

**2.SWAP**

```bash
2.1 SWAP是当物理内存不够时，临时将磁盘空间作为内存顶替使用
2.2 如果没有swap 物理不够，系统会启用保护机制，然后kill掉某个占用内存的程序
2.3 如果有swap，物理不够，会使用swap作为内存（系统就会开始变得比较的卡顿了）
16Gb    | 云主机 基本没有swap
```

**3.RAID**

```bash
3.1 磁盘阵列技术，对磁盘进行编排，提供更高的读写速度、以及冗余能力。
3.2 RAID 0   1  5  10 
3.3 RAID 0 快、大。   坏一个磁盘，都结束了。
3.4 RAID 1 有冗余，允许坏一个盘、写不快，读比较快。 容量仅能使用 百分之 50%
3.5 RAID 5 既能保证速度、还能保证冗余、空间有1/3的浪费。  成本可控。   （ 使用较多 ）
3.6 RAID 10   先做RAID 1  在做RAID0  （做2个RAID1 4快盘，==>RAID0  空间 50% ）

冗余从好到坏：Raid1==>Raid10==>Raid5==>Raid0
性能从好到坏：Raid0==>Raid10==>Raid5==>Raid1
成本从低到高：Raid0==>Raid5==>Raid1==>Raid10
```

## 7. systemctl系统服务管理

**7.1 开机启动流程**

1. Linux系统的启动过程并不是大家想象中的那么复杂，其过程可以分为5个阶段：

- 内核的引导。

- 运行 init(6)。 sentos7 运行父进程(systemd) pstree进行查看父进程systemd -sp展开查看

- 系统初始化。

- 建立终端 。

- 用户登录系统。

  centos7
  6和7启动的祖宗进程不一样,加载初始化脚本的位置也不一样,其他都一样

**7.2 运行级别**

什么是运行级别，运行级别就是操作系统当前正在运行的功能级别(一般用3命令行)

```bash
 System V init运行级别			systemd目标名称						作用
  0						runlevel0.target, poweroff.target			关机
  1						runlevel1.target, rescue.target				单用户模式
  2						runlevel2.target, multi-user.target			没有使用
  3						runlevel3.target, multi-user.target			多用户的文本界面
  4						runlevel4.target, multi-user.target			没有使用
  5						runlevel5.target, graphical.target			多用户的图形界面
  6						runlevel6.target, reboot.target				重启
```

查看运行级别

```bash
#centos6系统的运行级别，/etc/inittab #运行级别文件,修改下面数字就可以
1.临时
	runlevel	查看当前级别   

	init Number(级别数字) 	切换级别

2.永久  /etc/inittab
	id:5:initdefault #开机启动什么级别(5就是图形化界面)

#centos7运行级别
multi-user.target: analogous to runlevel 3
graphical.target: analogous to runlevel 5

#systemctl get-default    查看运行级别
multi-user.target   3

#修改默认运行级别
[root@syc~]# systemctl set-default graphical.target
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/graphical.target.
#进行查看级别
[root@syc~]# systemctl get-default
graphical.target
```



## 8.系统平均负载

 每次发现系统变慢时，我们通常做的第一件事，就是执行 top 或者 uptime 命令，来了解系统的负载情况。比如像下面这样，我在命令行里输入了 uptime 命令，系统也随即给出了结果。 

```bash
[root@m01 ~]# uptime
04:49:26 up 2 days, 2:33, 2 users, load average: 0.70, 0.04, 0.05

#我们已经比较熟悉前面几列，它们分别是当前时间、系统运行时间以及正在登录用户数。
而最后三个数字呢，依次则是过去 1 分钟、5 分钟、15 分钟的平均负载（Load Average）。
```

**8.1 什么是平均负载**

- 平均负载是指单位时间内，系统处于 可运行状态R 和 不可中断状态D 的平均进程数，也就是平均活跃进程数
- 平均负载其实就是单位时间内的活跃进程数。 ( 可运行状态R + 不可中断状态D )
- 平均负载要看的是三个值(1,5,15分钟)，不是一个。

**8.2 可运行状态和不可中断状态是什么**

- 1.可运行状态进程，是指正在使用 CPU 或者正在等待 CPU 的进程，也就是我们ps 命令看到处于 R 状态的进程。
- 2.不可中断进程，(你做什么事情的时候是不能打断的?) 系统中最常见的是等待硬件设备的 I/O 响应，也就是我们 ps 命令中看到的 D 状态（也称为 Disk Sleep）的进程。

**8.3 平均负载多少时合理**

 最理想的状态是每个 CPU 上都刚好运行着一个进程，这样每个 CPU 都得到了充分利用。所以在评判平均负载时，首先你要知道系统有几个 CPU，这可以通过 top 命令获取，或grep ‘model name’ /proc/cpuinfo 

```bash
例1、假设现在在 4、2、1核的CPU上，如果平均负载为 2 时，意味着什么呢？
Q1.在4 个 CPU 的系统上，意味着 CPU 有 50% 的空闲。
Q2.在2 个 CPU 的系统上，意味着所有的 CPU 都刚好被完全占用。
Q3.而1 个 CPU 的系统上，则意味着有一半的进程竞争不到 CPU。


1.如果 1 分钟、5 分钟、15 分钟的三个值基本相同，或者相差不大，那就说明系统负载很平稳。
2.但如果 1 分钟的值远小于 15 分钟的值，就说明系统最近 1 分钟的负载在减少，而过去 15 分钟内却有很大的负载。
3.反过来，如果 1 分钟的值远大于 15 分钟的值，就说明最近 1 分钟的负载在增加，这种增加有可能只是临时性的，也有可能还会持续上升，所以就需要持续观察。

PS: 一旦 1 分钟的平均负载接近或超过了 CPU 的个数，就意味着系统正在发生过载的问题，这时就得分析问题，并要想办法优化了
在来看个例子3、假设我们在有2个 CPU 系统上看到平均负载为 2.73，6.90，12.98
那么说明在过去1 分钟内，系统有 136% 的超载 (2.73/2=136%)
而在过去 5 分钟内，有 345% 的超载 (6.90/2=345%)
而在过去15 分钟内，有 649% 的超载，(12.98/2=649%)
但从整体趋势来看，系统的负载是在逐步的降低。
```

**8.4 平均负载和CPU使用率有什么关系**

Q: 平均负载高了，不就意味着 CPU 使用率高吗？ 

 A: 回到平均负载的含义上来，平均负载是指单位时间内，处于可运行状态和不可中断状态的进程数。所以，它不仅包括了正在使用 CPU 的进程，还包括等待 CPU 和等待 I/O 的进程。 

而 CPU 使用率，是单位时间内 CPU 繁忙情况的统计，跟平均负载并不一定完全对应。比如：

- 1.CPU 密集型进程，使用大量 CPU 计算会导致平均负载升高，此时这两者是一致的； （ 视频的转码 加密 计算圆周率 ）
- 2.I/O 密集型进程，等待 I/O 也会导致平均负载升高，但 CPU 使用率不一定很高； 关注wa
- 3.大量的 CPU 进程调度也会导致平均负载升高，此时的 CPU 使用率也会比较高。

**总结: 排查步骤**

```bash
查看步骤:
1.uptime看看负载的情况 （ 超过了 cpu的核心 ）
2.top看是cpu的使用率高 还是 wa等待高 还是内核态占用cpu高 、软中断高 、nice优先级进程占用cpu、
3.top看是哪个进程
4.追踪这个进程的情况。
5.看看是否存在异常日志。
```

