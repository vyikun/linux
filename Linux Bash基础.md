# Linux基础命令

## 0. bash基础

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

## 14. 进程管理

## 15. crond 计划任务

## 16. 系统排查

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

## 6. 系统服务管理

