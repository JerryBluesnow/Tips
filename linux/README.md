# Tips for linux

## 动态链接库
- [linux动态链接库: lib .so .a](./sharedlib/README.md)

## linux下手动安装/升级GCC到较高版本
```shell
一、环境
VMWare+Centos7
二、写在前面的话
安装GCC最简单的方式当然是【yum -y install gcc】
但是我的机器上安装下来后，其版本是4.8.5，感觉有点低，所以想升级一下（7.2.0, 8.2.0之类的版本）。
于是需要手动安装。
三、吃过的坑
1. 本地没有GCC导致编译不通过
原因分析：
安装高版本GCC时，需要依赖其它GCC，所以需要保证有一个较低版本的GCC
解决方法：
这个最简单的当然就是通过上面的【yum -y install gcc】进行安装
g++也一起安装了吧，命令【yum -y install gcc-c++】
安装后可以【gcc -v】、【g++ -v】进行测试，能打出正常版本表示成功
 
2. 上一步中偷懒，没有安装g++
会有如下报错：
checking how to run the C++ preprocessor... /lib/cpp
configure: error: in `/usr/cyh/gcc-8.2.0/host-x86_64-pc-linux-gnu/gcc':
configure: error: C++ preprocessor "/lib/cpp" fails sanity check
See `config.log' for more details.
make[2]: *** [configure-stage1-gcc] 错误 1
make[2]: 离开目录“/usr/cyh/gcc-8.2.0”
make[1]: *** [stage1-bubble] 错误 2
make[1]: 离开目录“/usr/cyh/gcc-8.2.0”
make: *** [all] 错误 2
通过【fails sanity check】进行搜索了一上，其实就是没有安装C++编译器
也就是上面的g++也要一起安装一下，不然一直报这个错

3. 直接在新下载的GCC源码路径中编译
原因分析：
GCC的源码目录和安装目录，不要在同一个路径树中
正例：
源码目录=/home/cyh/study/, 安装目录=/usr/local/
官方文档：
https://gcc.gnu.org/install/configure.html
原文是【First, we highly recommend that GCC be built into a separate directory from the sources which does not reside within the source tree.】
四、正式开始安装
1、下载GCC
方式有很多，可以通过网页下载再上传到VM、可以直接wget等等
假设我下载到 /home/cyh/study 目录，分别执行了以下命令：
cd /home/cyh/study
wget http://ftp.gnu.org/gnu/gcc/gcc-7.2.0/gcc-7.2.0.tar.gz
tar -zxvf gcc-7.2.0.tar.gz
cd gcc-7.2.0
 
2、配置（不推荐）
此时可以执行【./configure --prefix=/user/local/】，但是会报错，如下：
【configure: error: Building GCC requires GMP 4.2+, MPFR 2.4.0+ and MPC 0.8.0+.】
表示需要这些依赖包，所以继续下载
GCC 源码里自带脚本可以轻松下载依赖包，执行【./contrib/download_prerequisites】
如果自动安装成功，会有如下输出：
【All prerequisites downloaded successfully.】
依赖下载完成后，再执行【./configure --prefix=/user/local/】
如果有【configure: error: I suspect your system does not have 32-bit development libraries (libc and headers). If you have them, rerun configure with --enable-multilib. If you do not have them, and want to build a 64-bit-only compiler, rerun configure with --disable-multilib.】这样的报错，则要在上面的命令中加入【--disable-multilib】参数，所以命令变为下面这样【./configure --prefix=/user/local/ --disable-multilib】
 
3、配置（推荐）
既然已经知道了GCC安装时有依赖，那就直接先搞定依赖再来配置
所以先执行【./contrib/download_prerequisites】
如果一切顺利，再执行【./configure --prefix=/user/local/ --disable-multilib】即可
 
4、make
直接执行 make 命令（我机器上执行了3小时，OMG）
 
5、make install
直接执行 make install 命令
```
- [/lib64/libstdc++.so.6: version `CXXABI_1.3.8’ not found](https://blog.csdn.net/EI__Nino/article/details/100086157)

ln -s /root/local/bin/gcc /usr/bin/gcc && ln -s /root/local/bin/g++ /usr/bin/g++ && ln -s /root/local/bin/c++ /usr/bin/c++ && ln -s /root/local/bin/gcc /usr/bin/cc && cp /root/local/lib64/libstdc++.so.6.0.27 /usr/lib64 && cd /usr/lib64 && ln -s libstdc++.so.6.0.27 libstdc++.so.6

- [Linux安装GCC 9.2.0](https://blog.csdn.net/lwc5411117/article/details/101200065)
- [CENTOS7编译安装GCC9.2.0及踩坑经历](https://www.cnblogs.com/liranowen/p/11639929.html)
- [第二部份：gcc升级到gcc-9.2.0](https://blog.csdn.net/u012480990/article/details/104277771)
- [CentOS7编译安装Gcc9.2.0，解决mysql等软件编译问题](https://www.51lowkey.com/note-14.html)

- [一次segfault错误的排查过程](https://blog.csdn.net/zhaohaijie600/article/details/45246569)
- [centos7 安装 GNU Make 4.1](https://blog.csdn.net/weixin_41565755/article/details/88564947)
- [在centos上安装最新的glibc](https://blog.csdn.net/zhangpeterx/article/details/96116219)
export LD_PRELOAD=/opt/glibc-2.19/lib/libc-2.19.so;ln -sf /opt/glibc-2.19/lib/libc-2.19.so /lib64/libc.so.6
LD_PRELOAD=/lib64/libc-2.17.so; ln -sf /lib64/libc-2.17.so /lib64/libc.so.6

- [CentOS 7.6 编译安装最新版本glibc2.30 实录](https://blog.csdn.net/RyanFang/article/details/100984938?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

## back
export LD_PRELOAD=/lib64/libc-2.17.so;ln -sf /lib64/libc-2.17.so /lib64/libc.so.6

- [linux下glibc库升级](https://blog.csdn.net/noobplayer/article/details/52790059)

```[root@localhost lib64]# ln -sf /opt/glibc-2.29/lib/libc-2.29.so /lib64/libc.so.6
[root@localhost lib64]# export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH
[root@localhost lib64]# ls
段错误
[root@localhost lib64]# export L^C
[root@localhost lib64]# export LD_LIBRARY_PATH=/lib64
[root@localhost lib64]# export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH ^C
```

```
ln -sf /lib64/libc-2.17.so /lib64/libc.so.6
/lib/x86_64-linux-gnu/ld-2.31.so /bin/ln -s /lib/x86_64-linux-gnu/ld-2.31.so /lib64/ld-linux-x86-64.so.2
```


## 误删了/lib64/ld-linux-x86-64.so.2，ls, cd等等命令失效，命令都失效

    --恢复
    /lib64/ld-2.17.so /bin/ln -s /lib64/ld-2.17.so /lib64/ld-linux-x86-64.so.2

## 恢复

    LD_PRELOAD=/lib64/libc-2.17.so ln -s /lib64/libc-2.17.so /lib64/libc.so.6

## nvocation of python3.6 via ld-2.17.so segfaults
- [nvocation of python3.6 via ld-2.17.so segfaults](https://github.com/ContinuumIO/anaconda-issues/issues/8773)


The following actions will resolve these dependencies:

     Keep the following packages at their current version:
1)     libdbd-mysql-perl [Not Installed]
2)     libdbi-perl [Not Installed]
3)     libterm-readkey-perl [Not Installed]
4)     mariadb-client [Not Installed]
5)     mariadb-client-5.5 [Not Installed]
6)     mariadb-server [Not Installed]
7)     mariadb-server-5.5 [Not Installed]

     Leave the following dependencies unresolved:
8)     mariadb-client-5.5 recommends libdbd-mysql-perl (>= 1.2202)


## Ubuntu Server源码编译安装MariaDB
```
 sudo apt-get update

 sudo apt-get install make

 sudo apt-get install cmake 

 sudo apt-get install automake 

 sudo apt-get install autoconf 

 sudo apt-get install libtool 

 sudo apt-get install gcc 

 sudo apt-get install bison

 sudo apt-get install g++

 sudo apt-get install libncurses5-dev

 sudo apt-get install libssl-dev

 sudo apt-get install libjemalloc-dev

```

每个版本的软件源都不同 - ubuntu16.04更新软件源

- [ubuntu16.04更新软件源](https://blog.csdn.net/lxlong89940101/article/details/89488461)

```
 sudo groupadd mysql

 sudo useradd -g mysql mysql

 cd /usr/local/src/mariadb-*

 sudo cmake .

 sudo make

 sudo make install

 sudo cp support-files/my-huge.cnf /usr/local/mysql/my.cnf

 cd /usr/local/mysql

 sudo chown -R root .

 sudo chown -R mysql data

 sudo chgrp -R mysql .

 sudo scripts/mysql_install_db --user=mysql

 sudo ./bin/mysqld_safe --user=mysql

 sudo ./bin/mysqladmin -u root password ''

 sudo ./bin/mysql -u root
```

## source filename 与 sh filename 及./filename执行脚本的区别
- 当shell脚本具有可执行权限时，用sh filename与./filename执行脚本是没有区别得。./filename是因为当前目录没有在PATH中，所有”.”是用来表示当前目录的。
- sh filename 重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。
- source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面。

##　解决linux下vim中文乱码的情况：(修改vimrc的内容）

全局的情况下：即所有用户都能用这个配置

文件地址：/etc/vimrc

在文件中添加：

```
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set hls
```

## git log显示中文

后查询相关资料，现将解决办法总结如下：

1、运行Git Bash窗口，在该窗口导航条（即最上面）右键，选择Options−>Text，找到下面两处
　　Locale:选择 zh_CN 
　　Charector set:选择 UTF-8 
2、到Git Bash命令窗口输入如下设置命令语句

git config --global i18n.commitencoding utf-8  --注释：该命令表示提交命令的时候使用utf-8编码集提交

git config --global i18n.logoutputencoding utf-8 --注释：该命令表示日志输出时使用utf-8编码集显示

export LESSCHARSET=utf-8  --注释：设置LESS字符集为utf-8

## 安装npm （这个命令最管用）
curl -L https://npmjs.com/install.sh | sh

Ubuntu 16.04 TLS，执行以下命令：

```
sudo apt-get install nodejs
sudo apt install nodejs-legacy
sudo apt install npm
```

Ubuntu 18.04 TLS，执行以下命令：

```
sudo apt-get install nodejs
sudo apt install libssl1.0-dev nodejs-dev node-gyp npm
更新npm的包镜像源，方便快速下载
sudo npm config set registry https://registry.npm.taobao.org
sudo npm config list
安装n管理器(用于管理nodejs版本)
sudo npm install n -g
```

## 安装最新的nodejs（stable版本）
```
sudo n stable
sudo node -v
sudo npm -v
```

## vue环境配置
最好用的就是方法1
```
方法1：

curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
如果安装nodejs 9.x版本

curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
方法2：
下载nodejs压缩文件
wget https://nodejs.org/dist/v8.1.0/node-v8.1.0-linux-x64.tar.xz
解压
tar -xvf node-v8.1.0-linux-x64.tar.xz
切换并查看当前node所在路径
cd node-v8.1.0-linux-x64/bin
pwd
查看node版本
./node -v
将node和npm设置为全局
sudo ln /home/ubuntu/node-v8.1.0-linux-x64/bin/node /usr/local/bin/node
sudo ln /home/ubuntu/node-v8.1.0-linux-x64/bin/npm /usr/local/bin/npm
pwd

方法三：
也可以使用ubuntu自带apt-get安装,安装后使用node -v查看版本

sudo apt-get install nodejs-legacy nodejs

1，输入命令sudo npm config set registry https://registry.npm.taobao.org，把npm的包源设置为淘宝的镜像
2，输入命令sudo npm install n -g，来安装n这个工具，n这个工具是用于更新node版本的工具
3，输入命令sudo n stable，安装最新稳定版的nodejs
4，重启终端，查看node.js和npm版本,node --version,npm  --version
```

## linux查看系统信息硬件配置
### 系统

```
　　# uname -a # 查看内核/操作系统/CPU信息

　　# head -n 1 /etc/issue # 查看操作系统版本

　　# cat /proc/cpuinfo # 查看CPU信息

　　# hostname # 查看计算机名

　　# lspci -tv # 列出所有PCI设备

　　# lsusb -tv # 列出所有USB设备

　　# lsmod # 列出加载的内核模块

　　# env # 查看环境变量
```

### 资源

```
　　# free -m # 查看内存使用量和交换区使用量

　　# df -h # 查看各分区使用情况

　　# du -sh <目录名> # 查看指定目录的大小

　　# grep MemTotal /proc/meminfo # 查看内存总量

　　# grep MemFree /proc/meminfo # 查看空闲内存量

　　# uptime # 查看系统运行时间、用户数、负载

　　# cat /proc/loadavg # 查看系统负载
```

### 磁盘和分区

```
　　# mount | column -t # 查看挂接的分区状态

　　# fdisk -l # 查看所有分区

　　# swapon -s # 查看所有交换分区

　　# hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备)

　　# dmesg | grep IDE # 查看启动时IDE设备检测状况
```

### 网络

```
　　# ifconfig # 查看所有网络接口的属性

　　# iptables -L # 查看防火墙设置

　　# route -n # 查看路由表

　　# netstat -lntp # 查看所有监听端口

　　# netstat -antp # 查看所有已经建立的连接

　　# netstat -s # 查看网络统计信息
```

### 进程
```
　　# ps -ef # 查看所有进程

　　# top # 实时显示进程状态
```

### 用户

```
　　# w # 查看活动用户

　　# id <用户名> # 查看指定用户信息

　　# last # 查看用户登录日志

　　# cut -d: -f1 /etc/passwd # 查看系统所有用户

　　# cut -d: -f1 /etc/group # 查看系统所有组

　　# crontab -l # 查看当前用户的计划任务
```

### 服务

```
　　# chkconfig --list # 列出所有系统服务

　　# chkconfig --list | grep on # 列出所有启动的系统服务
```

### 程序

```
rpm -qa # 查看所有安装的软件包
```

### 其他常用命令整理如下：
```
　　查看主板的序列号：dmidecode | grep -i 'serial number'
　　用硬件检测程序kuduz探测新硬件：service kudzu start ( or restart)
　　查看CPU信息：cat /proc/cpuinfo [dmesg | grep -i 'cpu'][dmidecode -t processor]
　　查看内存信息：cat /proc/meminfo [free -m][vmstat]
　　查看板卡信息：cat /proc/pci
　　查看显卡/声卡信息：lspci |grep -i 'VGA'[dmesg | grep -i 'VGA']
　　查看网卡信息：dmesg | grep -i 'eth'[cat /etc/sysconfig/hwconf | grep -i eth][lspci | grep -i 'eth']
　　查看PCI信息：lspci (相比cat /proc/pci更直观)
　　查看USB设备：cat /proc/bus/usb/devices
　　查看键盘和鼠标：cat /proc/bus/input/devices
　　查看系统硬盘信息和使用情况：fdisk & disk – l & df
　　查看各设备的中断请求(IRQ)：cat /proc/interrupts
　　查看系统体系结构：uname -a
　　查看及启动系统的32位或64位内核模式：isalist –v [isainfo –v][isainfo –b]
　　查看硬件信息，包括bios、cpu、内存等信息：dmidecode
　　测定当前的显示器刷新频率：/usr/sbin/ffbconfig –rev ?
　　查看系统配置：/usr/platform/sun4u/sbin/prtdiag –v
　　查看当前系统中已经应用的补丁：showrev –p
　　显示当前的运行级别：who –rH
　　查看当前的bind版本信息：nslookup –class=chaos –q=txt version.bind
　　查看硬件信息：dmesg | more
　　显示外设信息， 如usb，网卡等信息：lspci
　　查看已加载的驱动：
　　lsnod
　　lshw
　　查看当前处理器的类型和速度(主频)：psrinfo -v
　　打印当前的OBP版本号：prtconf -v
　　查看硬盘物理信息(vendor， RPM， Capacity)：iostat –E
　　查看磁盘的几何参数和分区信息：prtvtoc /dev/rdsk/c0t0d0s
　　显示已经使用和未使用的i-node数目：
　　df –F ufs –o i
　　isalist –v
　　对于“/proc”中文件可使用文件查看命令浏览其内容，文件中包含系统特定信息：
　　主机CPU信息：Cpuinfo
　　主机DMA通道信息：Dma
　　文件系统信息：Filesystems
　　主机中断信息：Interrupts
　　主机I/O端口号信息：Ioprots
　　主机内存信息：Meninfo
　　Linux内存版本信息：Version
　　proc – process information pseudo-filesystem 进程信息伪装文件系统
```


## centos install

- [centos官网](https://www.centos.org/download/)
- [centos清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/centos/8.3.2011/isos/x86_64/)
- [centos中文网](https://www.centoschina.cn/downloads)

## debian install

- [制作debian U盘启动盘](https://blog.51cto.com/1827495/488844)
- [使用Etcher来创建可启动盘（可引导的USB盘或SD卡）的方法](https://ywnz.com/linuxjc/3010.html)

- http://mirrors.163.com/debian-cd/10.7.0/amd64/iso-cd/

- http://www.92os.com/post/34

- https://cdimage.debian.org/debian-cd/10.7.0/amd64/iso-dvd/


### Debian 制作U盘启动安装盘
+ 1,工具：Universal-USB-Installer（据经验软碟通UltraISOl不是很100%成功）

    官网下载地址： http://www.pendrivelinux.com/  我下载的是 Universal-USB-Installer-1.9.9.0版本

+ 2 .U盘一个(4G/8G)根据系统的大小决定

+ 3.下载Debian镜像文件，目前最新的是debian-10.3.0-i386-netinst .iso 及debian-10.3.0-i386-xfce-CD-1.iso 及debian-10.3.0-i386-DVD-1.iso的DVD均可，从这里选择下载https://www.debian.org/distrib/  只需下载 下载第1个镜像文件 debian-8.1.0-amd64-DVD-1 或 CD即可 。

    1）将U盘插入电脑，注意提前备份该U盘上的数据，制作安装盘的过程将格式化U盘
    
    2）直接启动下载的 Universal-USB-Installer-1.9.6.1软件 （可执行文件，无需安装）              
    
    直接I Agree       
    
    此处在提供的Debain选项中，只有Live 和Netinst两个选项，并非想安装的amd64。往下拉滚动条，直接拉到最后，选择Try Unlisted Linux ISO
    
    然后选择下载的ISO镜像文件，选择U盘，点击Create按钮。         
    
    接下来将解压ISO文件，制作安装U盘，此过程时间较长，大约15分钟左右。

### debian镜像制作

- [这是为中国定制的Debian镜像](https://github.com/docker4cn/debian)

```
docker pull docker4cn/debian:buster-aliyun
```

- [如何制作debian(mips64el) docker镜像并上传到docker官方仓库](https://www.it610.com/article/1278555881058877440.htm)

```
docker pull docker4cn/debian:buster-aliyun
/etc/apt/sources.list
https://mirror.tuna.tsinghua.edu.cn/help/debian/
apt-get update
apt-get install vim -y
vi /etc/apt/sources.list
apt-get update
apt-get install certbot -y
certbot certonly --standalone -d sam-tech.com

- [Certbot-免费的HTTPS证书](https://zhuanlan.zhihu.com/p/80909555)

- [学习资料之Kaimailio and rtpengine安装使用](https://blog.csdn.net/weixin_41486034/article/details/106249598)


链接: https://pan.baidu.com/s/1yNdDQ3WgpdxFwRpUQSka_g 提取码: r9sj 复制这段内容后打开百度网盘手机App，操作更方便哦
## ubuntu/debian安装多个版本gcc
- [reference link](https://blog.csdn.net/qq_23084801/article/details/80726643)

- 查看系统gcc和g++是什么版本 
    - gcc -v
    - ls /usr/bin/gcc*
- 安装gcc 4.9
    - sudo apt-get install gcc-4.9 gcc-4.9-multilib g++-4.9 g++-4.9-multilib

- 修改优先级
    - sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 40 
    - sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50
    - sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 50 
    - sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 40

- 查看优先级 
    - sudo update-alternatives --config gcc
    - 会看到如下的选项，有 3 个候选项可用于替换 gcc (提供 /usr/bin/gcc)。(要维持当前值[*]请按回车键，或者键入选择的编号)

    | 选择 | 路径             | 优先级 | 状态     |
    | ---- | ---------------- | ------ | -------- |
    | 0    | /usr/bin/gcc-5   | 50     | 自动模式 |
    | 1    | /usr/bin/gcc-5   | 50     | 手动模式 |
    | 2    | /usr/bin/gcc-4.9 | 50     | 手动模式 |

- 删除可选项
    - sudo update-alternatives --remove gcc /usr/bin/gcc-4.9


## 使用Visual Studio 2017开发Linux程序
- [使用Visual Studio 2017开发Linux程序](https://www.cnblogs.com/dongc/p/6599461.html)
- ssh servers使用docker搭建
```
gdb调试报错warning: Error disabling address space randomization: Operation not permitted
 Canceled
#I1KPAQ
Bug
独奏曲
Opened this issue  
2020-06-15 19:47
这是一个bug还是新特性?:

bug
发生结果:

Starting program: /root/rpmbuild/BUILD/libmediaart-1.9.4/tests/.libs/lt-mediaarttest -k --tap
warning: Error disabling address space randomization: Operation not permitted
warning: Could not trace the inferior process.
Error: 
warning: ptrace: Operation not permitted
During startup program exited with code 127.
[图片上传中…(image-qhzUMEQsDjXOs8QpqEif)]

期望结果:

gdb正常调试

如何重现（尽量详细）:

gdb --args .libs/lt-mediaarttest -k --tap

补充说明?:

环境情况:

版本:
操作系统版本 (e.g. from /etc/os-release):openEuler 20.03 (LTS)
内核版本 (e.g. uname -a):Linux openEuler 4.19.36-vhulk1907.1.0.h619.eulerosv2r8.aarch64 #1 SMP Mon Jul 22 00:00:00 UTC 2019 aarch64 aarch64 aarch64 GNU/Linux
其它:
Attachments
lt-mediaarttest
(93.66 KB)
Download
独奏曲
10 months ago
独奏曲-speacher
独奏曲 created缺陷 10 months ago
独奏曲-speacher
独奏曲 set related repository to openEuler/community 10 months ago
展开全部操作日志 
5329419 openeuler ci bot 1578984659
openeuler-ci-bot
member
10 months ago
Hey @独奏曲 , Welcome to openEuler Community.
All of the projects in openEuler Community are maintained by @openeuler-ci-bot .
That means the developers can comment below every pull request or issue to trigger Bot Commands.
Please follow instructions at https://gitee.com/openeuler/community/blob/master/en/sig-infrastructure/command.md to find the details.

独奏曲-speacher
独奏曲
10 months ago
启动容器时加上--cap-add=SYS_PTRACE --security-opt seccomp=unconfined选项可以解决该问题
```

## performance 

[gperftools](https://github.com/gperftools)/**[gperftools](https://github.com/gperftools/gperftools)**

[善用工具-程序性能分析Gperftools初探(libwind+pprof+Kcachegrind)](https://blog.csdn.net/aganlengzi/article/details/62893533?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_aggregation-15-62893533.pc_agg_rank_aggregation&utm_term=kcachegrind+分析&spm=1000.2123.3001.4430)

```

```

## 函數邏輯關係圖

在对源代码走读的过程中，我们可以借助一些工具来帮助理解源代码的结构和函数调用关系，比如生成函数调用关系图。

cflow工具通过分析一组C源文件，绘制出程序的逻辑流程图和交叉引用列表，在此分析结果的基础上，通过其他工具生成可视化的图像文件，帮助我们理解源代码。

cflow安装

```text
wget https://ftp.gnu.org/gnu/cflow/cflow-latest.tar.gz
tar -zxvf cflow-latest.tar.gz
cd cflow-1.6
./configure
make
make install
```

tree2dotx脚本地址，shell脚本，拷贝到本地文件直接使用

```text
https://github.com/tinyclub/linux-0.11-lab/blob/master/tools/tree2dotx
```

graphviz安装

```text
yum install graphviz
```

进入tree2dotx脚本所在目录，按照下面的步骤操作：

```text
cflow -T -m main /root/freeswitch-1.8.7/src/switch.c > fs.txt
cat fs.txt | ./tree2dotx > fs.dot
dot -Tbmp fs.dot -o fs.bmp
```

for ALG/IMS

```
cd ssp/ds/ims/util  # I suppose ALG code is under this path

cflow -T -m main IMSpolicy_qos_state.cpp > IMSpolicy_qos_state.txt
cat IMSpolicy_qos_state.txt | ./tree2dotx > IMSpolicy_qos_state.dot
dot -Tjpg IMSpolicy_qos_state.dot -o IMSpolicy_qos_state.jpg
```

```
多個文件：
cflow -m= *.c > 2.txt
cat 2.txt | tree2dotx > 2.dot
dot -Tjpg 2.dot -o 2.jpg
```

```
cflow --help
我们可以得到如下提示
Usage: cflow [OPTION...] [FILE]...
generate a program flowgraph

 General options:
  -d, --depth=NUMBER         Set the depth at which the flowgraph is cut off
  -f, --format=NAME          Use given output format NAME. Valid names are
                             `gnu' (default) and `posix'
  -i, --include=CLASSES      Include specified classes of symbols (see below).
                             Prepend CLASSES with ^ or - to exclude them from
                             the output
  -o, --output=FILE          Set output file name (default -, meaning stdout)
  -r, --reverse              * Print reverse call tree
  -x, --xref                 Produce cross-reference listing only

 Symbols classes for --include argument

    _                        symbols whose names begin with an underscore
    s                        static symbols
    t                        typedefs (for cross-references only)
    x                        all data symbols, both external and static

 Parser control:

  -a, --ansi                 * Accept only sources in ANSI C
  -D, --define=NAME[=DEFN]   Predefine NAME as a macro
  -I, --include-dir=DIR      Add the directory DIR to the list of directories
                             to be searched for header files.
  -m, --main=NAME            Assume main function to be called NAME
  -p, --pushdown=NUMBER      Set initial token stack size to NUMBER
      --preprocess[=COMMAND], --cpp[=COMMAND]
                             * Run the specified preprocessor command
  -s, --symbol=SYMBOL:[=]TYPE   Register SYMBOL with given TYPE, or define an
                             alias (if := is used). Valid types are: keyword
                             (or kw), modifier, qualifier, identifier, type,
                             wrapper. Any unambiguous abbreviation of the above
                             is also accepted
  -S, --use-indentation      * Rely on indentation
  -U, --undefine=NAME        Cancel any previous definition of NAME

 Output control:

  -b, --brief                * Brief output
      --emacs                * Additionally format output for use with GNU
                             Emacs
  -l, --print-level          * Print nesting level along with the call tree
      --level-indent=ELEMENT Control graph appearance
  -n, --number               * Print line numbers
      --omit-arguments       * Do not print argument lists in function
                             declarations
      --omit-symbol-names    * Do not print symbol names in declaration strings
                            
  -T, --tree                 * Draw ASCII art tree

 Informational options:

      --debug[=NUMBER]       Set debugging level

  -v, --verbose              * Verbose error diagnostics

  -?, --help                 give this help list
      --usage                give a short usage message
  -V, --version              print program version

Mandatory or optional arguments to long options are also mandatory or optional
for any corresponding short options.

* The effect of each option marked with an asterisk is reversed if the option's
  long name is prefixed with `no-'. For example, --no-cpp cancels --cpp.

Report bugs to <bug-cflow@gnu.org>.

列出cflow的所有选项，包括简要的说明。所有的长选项和短选项都被列出了，所以你可以将这个表作为快速参考。
大部分的选项都有一个相反意义的负选项对应，负选项的命名是对相应的长选项加前缀no-.这个特性用于取消在配置文件中定义的选项。
-a (--ansi)
假设输入文件使用ANSI C编写。目前这意味着不能解析K&R声明的函数。这在某些情况下可以加快处理进度。
-b (--brief)
简要输出
--cpp[=command]
运行指定的预处理命令
-D name[=defn] (--define=name[=defn])
预定义名字作为宏。
-d number (--depth=number)
设置流图中嵌套的最大层数。
--debug[=number]
设置调试级别。默认值是1，如果你开发或调试cflow时使用这个选项。
--emacs
让访问文件时告诉Emacs使用cflow模式输出。
-f name (--format=name)
使用给定的输出格式名。合法的名字是gnu和posix。
-? (--help)
帮助，对每个选项作简要的说明。
-I dir (--include-dir=dir)
增加搜索头文件时，所需要的头文件所在目录。
-i spec (--include=spec)
控制包含符号的数量。spec是一个字符串，指定了哪一类符号应该包含在输出里。合法字符如下：
- ^ 输出中排除后接字符
+ 输出中包含后接字符（缺省）
_ 以下划线开头的符号
s 静态符号
t 类型定义（只在交叉引用时使用）
x 所有的数据符号，包括外部符号和静态符号
-l
--level-indent=string 指定每个级别缩进时使用的字符串
-m name (--main=name) 设定最开始调用的函数名。
-n (--number) 打印行号
-o file (--output=file) 指定输出文件，默认是’-’,即标准输出
--ommit-arguments 不打印函数声明中的参数列表
--omit-symbol-names 不打印所指定的符号名字，在posix模式下可用。
-r (--reverse) 打印逆向调用图
-x (--xref) 只生成交叉引用列表
-p number (--pushdown=number) 初始化令牌栈的大小。默认值64.令牌栈会自动增长，所以这个选项很少使用。
--preprocess[=command] 使用预处理
-s sym:class
--symbol=sym:class
--symbol=newsym:=oldsym
第一种形式，在语法类class中注册符号sym。合法的额类名是‘keyword’ (or ‘kw’), ‘modifier’, ‘qualifier’, ‘identifier’, ‘type’, ‘wrapper’。任何明确的缩写都是可接受的。
第二种形式(使用’:=’分割)，定义newsym作为oldsym的别名。
-S (--use-indentation) 使用文件缩进作为提示。目前这个意思是右大括号 (‘}’) 在第零列强制cflow结束当前的函数定义。使用这个选项解析可能会对某些远产生误解。
-U name (--undefine=name) 取消之前所做的name的定义
-l (--print-level) 打印嵌套层数。层数在输出行的最后打印(如果使用了--number 或 --format=posix，层数会使用大括号括起来)。
-T (--tree) 使用ASCII码打印，调用树。
--usage 提供简短的使用信息。
-v (--verbose) 详细的打印出所有的错误信息。cflow中的错误信息与c编译器的错误信息是不一样的，所以这个选项默认是关闭的。
-V (--version) 打印程序的版本信息
```

下载jpg文件，打开查看

![img](https://pic1.zhimg.com/50/v2-f2d26c2f3193a26f6108c9e644b73fdd_720w.jpg?source=1940ef5c)

工具挺有趣的，但是对于freeswitch这种调用比较复杂的流程，图片看起来也比较复杂，和我使用工具的本意有差距，希望对大家有所帮助。

## Debian设置开机启动命令行模式，关闭图形界面
```
debian10开机默认命令行需要如下，

grub修改，grub修改有两种办法和centos7比较类似，

a)修改/etc/default/grub 然后update-grub2

或者直接修改/boot/grub/grub.cfg(修改之前建议先备份）

一般建议修改/etc/default/grub（第一行是让用户登录时命令行login，第二行时grub选择系统界面为命令行）



GRUB_CMDLINE_LINUX_DEFAULT="quiet text"

GRUB_TERMINAL=console

b)改完以后执行update-grub2 这个时候去/boot/grub/grub.cfg检查是否cmd line增加了text，等开机了也检查下/proc/cmdline

c)然后设置多用户的配置

sudo systemctl set-default multi-user.target

如果要切回图形界面，

sudo systemctl set-default graphical.target
```

## Debian 配置静态IP

```
vi /etc/network/interfaces
```

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
auto eth0 #开启自动连接网络
iface lo inet loopback
allow-hotplug eth0
iface eth0 inet static
address 192.168.1.104 # IP
netmask 255.255.255.0 # 子网掩码
gateway 192.168.1.1 # 网关
```

```
service networking restart
## 后台运行
```
nohup npm run dev >/dev/null 2>&1
```

## 上下文切换的定义
- [Context Switch Definition](http://www.linfo.org/context_switch.html)
- [进程上下文切换 – 残酷的性能杀手（上）](https://www.cnblogs.com/emperor_zark/archive/2012/12/11/context_switch_1.html)
- [进程/线程上下文切换会用掉你多少CPU？](https://zhuanlan.zhihu.com/p/79772089)
- [这么多监控组件，总有一款适合你](https://cloud.tencent.com/developer/article/1511761)


## [Add a User to a Group (or Second Group) on Linux](http://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)

```
Add a User to a Group (or Second Group) on Linux
Changing the group a user is associated to is a fairly easy task, but not everybody knows the commands, especially to add a user to a secondary group. We’ll walk through all the scenarios for you.

Add a New Group

To add a new group, all you need to do is use the groupadd command like so:

groupadd <groupname>

Add an Existing User to a Group

Next we’ll add a user to the group, using this syntax:

usermod -a -G <groupname> username

For example, to add user geek to the group admins, use the following command:

usermod -a -G admins geek

Change a User’s Primary Group

Sometimes you might want to switch out the primary group that a user is assigned to, which you can do with this command:

usermod -g <groupname> username

View a User’s Group Assignments

If you’re trying to figure out a permissions issue, you’ll want to use the id command to see what groups the user is assigned to:

id <username>

This will display output something like this:

uid=500(howtogeek) gid=500(howtogeek) groups=500(howtogeek), 1093(admins)

You can also use the groups command if you prefer, though it is the same as using id -Gn <username>.

groups <username>

View a List of All Groups

To view all the groups on the system, you can just use the groups command:

groups

Add a New User and Assign a Group in One Command

Sometimes you might need to add a new user that has access to a particular resource or directory, like adding a new FTP user. You can do so with the useradd command:

useradd -g <groupname> username

For instance, lets say you wanted to add a new user named jsmith to the ftp group:

useradd -G ftp jsmith

And then you’ll want to assign a password for that user, of course:

passwd jsmith

Add a User to Multiple Groups

You can easily add a user to more than one group by simply specifying them in a comma-delimited list, as long as you are assigning the secondary groups:

usermod -a -G ftp,admins,othergroup <username>

That should cover everything you need to know about adding users to groups on Linux.

## 构建rpm包

```
- [如何构建 RPM 包](https://zhuanlan.zhihu.com/p/47868584)
- [Building RPMs with Mock](https://ithiriel.com/content/2011/10/13/building-rpms-mock)
- [在 Ubuntu 下直接将二进制文件制作成 rpm 包](https://blog.konghy.cn/2015/11/13/rpmbuild/)
- [ubuntu制作简陋的deb/rpm包](https://blog.csdn.net/evglow/article/details/103351348)
- [Centos RPM安装包制作](https://blog.csdn.net/q1009020096/article/details/110953465)
- [一步步制作RPM包](https://blog.51cto.com/laoguang/1103628)
```
## yum源
- [Ubuntu下安装yum和配置yum源](https://blog.csdn.net/qq_38690917/article/details/115261819)

  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
  下载对应版本repo文件, 放入/etc/yum.repos.d/(操作前请做好相应备份)
  CentOS7   wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

## kamailio

```
以前装好的kamailio sip服务器经常在启动的时候经常遇到这个错误：
ERROR: PID file /var/run/kamailio/kamailio.pid does not exist -- Kamailio start failed
经过自己尝试发现可能引起的原因是因为在使用kamctl start之前使用了kamailio 导致启动了一些进程。
使用PS 命令查看kamialio相关进程：ps axw | /usr/bin/egrep kamailio
```
ps -ef|grep kamailio|grep -v grep|cut -c 9-15|xargs kill -9
```
- [ubuntu上kamailio+rtpproxy+mediaproxy环境搭建](https://www.jianshu.com/p/9e2ffbf853fc)

- [kamailio docs](http://kamailio.org/docs/db-tables/kamailio-db-5.5.x.html)
- [kamailio modules](https://www.kamailio.org/docs/modules/stable/)
- [imsi和手机号码的关系](https://blog.csdn.net/qq276726581/article/details/53057078)
- [kamailio debug日志设置](https://blog.csdn.net/zhangtuo/article/details/81937791)
- [VoLTE Setup with Kamailio IMS and Open5GS](https://open5gs.org/open5gs/docs/tutorial/02-VoLTE-setup/)

## centos 7.6 安装kamailio n5记录

```
yum list java

yum install -y java-1.8.0-openjdk-devel.x86_64

yum groupinstall -y "Development Tools"

yum install -y json-c json-c-devel

yum install -y gcc

yum install -y bison

yum install -y flex

yum install -y MariaDB-devel MariaDB-client MariaDB-shared

yum install -y libxml2-devel

yum install -y libmnl-devel

yum install -y libcurl libcurl-devel

make include_modules="db_mysql xmlrpc cdp cdp_avp ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf presence http_async_client" cfg

make all

make install

vi /usr/local/etc/kamailio/kamctlrc
change below value:
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL

then, 
kamdbctl create

$ kamdbctl create
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;

$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
No Subscribers are added yet

The kamdbctl will add two users in MySQL user tables:

- kamailio   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- kamailioro - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database

```

### Kamailio服务器安装配置
```
Kamailio是一个开源的SIP服务器，原名OpenSER 

Kamailio is an Open Source, GPL2, SIP Server Routing Platform. It is written in C for Linux/Unix plaforms and focuses on performance, flexibility and security.

Home page with new project name: http://www.kamailio.org
Home page with old project name: http://www.openser-project.org
SourceForge.net Project page: http://sourceforge.net/projects/openser/
Features

SIP proxy/registrar/redirect server (RFC3261, RFC3263)
UDP/TCP/TLS/SCTP support
Transactional stateful proxy
Modular architecture
Programmable configuration file
ENUM support
Call Processing Language (CPL)
Gateway to sms or xmpp
Authentication, authorization and accounting via Radius or database
NAT traversal system
Least cost routing
Load balancing
Carrier routing
Multiple database backends: MySQL, Postgres, Oracle, BDB or flat files
SIMPLE Presence Server (IETF SIMPLE extensions - rich presence)
Dialog Info Presence - SLA/BLA
XCAP and RLS
Presence User Agent
Dialog Stateful Proxy
Instant Messaging
Offline message storage
Instant messaging conferencing
SNMP support
Perl Programming Interface
Java SIP Servlet Application server
Over 80 modules (extensions)
Documentation

Main Documentation Page - http://www.kamailio.org/docs/
Dokuwiki Page - http://www.kamailio.org/dokuwiki/
我们使用Kamailio主要用在SIP dispatcher server，即SIP redirect server
安装及配置手册如下

一．安装
1．依赖包：
libmysqlclient & libz (zlib) ：mysql DB support (the db_mysql module) Shared libraries

```shell
MySQL-shared-5.1.32-0.glibc23.i386.rpm

MySQL-devel-community-5.1.32-0.rhel5.i386.rpm
```

libxml2：cpl-c (Call Processing Language) or the presence modules (presence and pua*)
libperl：perl scripting from you config file (perl module)
2．源代码安装
make，make modules，make install
或者make all，make install
参考：
3．启动：kamctl start
4．重启：kamctl restart
5．监控服务状态：kamctl moni
6．MySQL配置：
1）安装：
edit Makefile.var files to include the MySQL module
vim Makefile.vars
Uncomment the next line in the file:
MODS_MYSQL=on
cp /usr/local/lib/mysql/libmysqlclient.so.16 /usr/lib

Edit now /usr/local/etc/kamailio/kamctlrc and add:
DBENGINE=MYSQL
SIP_DOMAIN=pryko.com
6.1 创建数据库：kamdbctl create
6.2管理员登录：user 'admin' with password ' openserrw '
6.3 添加用户：kamctl add <name> <password> <email>
6.4 默认值：database url, users and passwords
  - DEFAULT_DB_URL="mysql://opensips:opensipsrw@localhost/opensips"
  - r/w user: openser; passwd: openserrw
  - r/o user: openserro; passwd: openserro

二．配置
1．配置文件 kamailio.cfg
/usr/local/etc/kamailio/kamailio.cfg
2．配置文件 kamctlrc
/usr/local/etc/kamailio/kamctlrc

三．脚本
参考文档：
Kamailio Wiki
http://www.kamailio.com/dokuwiki
Cookbooks and Reference
http://www.kamailio.com/dokuwiki/doku.php/core-cookbook:1.5.x
Kamalio 1.5.x Module Functions Index
http://www.kamailio.com/dokuwiki/doku.php/modules:1.5.x:index-functions


四．负载均衡Load Balancing
参考：http://www.kamailio.org/dokuwiki/doku.php/asterisk:load-balancing-and-ha
4.1配置文件 kamailio.cfg
loadmodule("dispatcher.so")
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
modparam("dispatcher", "force_dst", 1)
4.2 ---dispatcher.list----文件
# group sip addresses of your * units
1 sip:221.5.152.171:5060
1 sip:221.5.152.170:5060
4.3 kamctl命令：kamctl dispatcher show
-- command 'dispatcher' - manage dispatcher
  * Examples:  dispatcher addgw 1 sip:1.2.3.1:5050 1 'outbound gateway'
  *            dispatcher addgw 2 sip:1.2.3.4:5050 3 ''
  *            dispatcher rmgw 4
dispatcher show ..................... show dispatcher gateways
dispatcher reload ................... reload dispatcher gateways
dispatcher dump ..................... show in memory dispatcher gateways
dispatcher addgw <setid> <destination> <flags> <description>
            .......................... add gateway
dispatcher rmgw <id> ................ delete gateway

查看载入的配置：kamctl dispatcher dump
修改后重新载入配置：kamctl dispatcher reload

 

如需使用，需安装MySQL-client-community-5.1.32-0.rhel5.i386.rpm
否则报错：ERROR: This command requires a database engine - none was loaded



五．与Asterisk对接负载均衡
注意事项：sip.conf
注释如下行
;canreinvite=no ; Asterisk by default tries to redirect

Asterisk#1  10.10.10.56
配置sip.conf
[5000]
type=friend
;username=5000
secret=5000_phone2
callerid=5000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000@10.10.10.57,60)
exten => 5000,1,Dial(SIP/5000,60)

Asterisk#2  10.10.10.57
配置sip.conf
[6000]
type=friend
;username=6000
secret=6000_phone2
callerid=6000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000,60)
exten => 5000,1,Dial(SIP/5000@10.10.10.136,60)

Kamailio 10.10.10.136
配置kamailio.cfg
…
loadmodule "dispatcher.so"
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
…
route{
if ( !mf_process_maxfwd_header("10") )
{
  sl_send_reply("483","To Many Hops");
  drop();
};
ds_select_dst("1", "0");
forward();
}
配置dispatcher.list
# line format
# setit(integer) destination(sip uri) flags (integer, optional)
1 sip:10.10.10.56:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56

六．按号码段重定向网关
配置kamailio.cfg
使用正则表达式
route{
        if (!mf_process_maxfwd_header("10")) {
                sl_send_reply("483","Too Many Hops");
                exit;
        }
if (uri=~"^sip:5[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("1", "0");
  forward();
  exit;
  }
}
if (uri=~"^sip:8[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("2", "0");
  forward();
  exit;
  }
}
sl_send_reply("404","Not here");
exit;
}
配置dispatcher.list
# line format
# setit(integer) destination(sip uri) flags (integer, optional)
1 sip:10.10.10.56:5060 #1
2 sip:10.10.10.54:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56
从6000呼叫8002，会呼叫10.10.10.136上的8002，10.136重定向到10.54


Kamailio 3.3.0 是一个主要的版本，包含众多新特性，包括通用数据库集群，可选的 Cassandra 后端，SIP Outbound and GRUU, MSRP relaying, detection of broken active calls 以及一个嵌入式的 C# 解释器。
```
## 安装RTPProxy
```
安装依赖包:

gcc编译器大于4.9，如gcc5.4版本的安装：

yum install centos-release-scl -y
yum install devtoolset-4-toolchain -y
scl enable devtoolset-4 bash
安装rtpproxy app:

git clone -b master https://github.com/sippy/rtpproxy.git
cd rtpproxy
git -c rtpproxy submodule update --init --recursive
./configure
make
make install
启动rtpproxy:

rtpproxy -l 182.XX.10.17 -s udp:127.0.0.1 7078 -F 
```

## VoLTE Setup with Kamailio IMS and Open5GS

Setup description:

+ MCC: 001, MNC: 01
+ Single OpenStack VM with Kamailio IMS and Open5GS (Internal IP 10.4.128.21 and Floating IP 172.24.15.30)
+ 4G Casa Smallcell
+ Sysmocom USIM - sysmoUSIM-SJS1
+ Oneplus 5 as UE
```
OpenStack is not required for VoLTE setup. If you are using one machine without OpenStack, you can start from step 3.
```

### 1. Start from Bionic Ubuntu cloud image

### 2. Use the following Cloud init config while spawning an instance
```
#cloud-config
disable_root: 0
ssh_pwauth: True
users:
  - name: root
chpasswd:
    list: |
    root:admin
    expire: False
runcmd:
  - sed -i -e '/^#PermitRootLogin/s/^.*$/PermitRootLogin yes/' /etc/ssh/sshd_config
  - systemctl restart sshd
```
```
This removes all existing cloud users and allows only root user and sets a password
```
### 3. Install following packages
```
$ apt update && apt upgrade -y && apt install -y mysql-server tcpdump screen ntp ntpdate git-core dkms gcc flex bison libmysqlclient-dev make \
libssl-dev libcurl4-openssl-dev libxml2-dev libpcre3-dev bash-completion g++ autoconf rtpproxy libmnl-dev libsctp-dev ipsec-tools libradcli-dev \
libradcli4
```
### 4. Clone Kamailio repository and checkout 5.3 version of repository
```
$ mkdir -p /usr/local/src/
$ cd /usr/local/src/
$ git clone https://github.com/herlesupreeth/kamailio
$ cd kamailio
$ git checkout -b 5.3 origin/5.3
```
### 5. Generate build config files
```
$ cd /usr/local/src/kamailio
$ make cfg
```
### 6. Enable MySQL module and all required IMS modules.
```
Edit modules.lst file present at /usr/local/src/kamailio/src
```
The contents of modules.lst should be as follows:
```
# this file is autogenerated by make modules-cfg

# the list of sub-directories with modules
modules_dirs:=modules

# the list of module groups to compile
cfg_group_include=

# the list of extra modules to compile
include_modules= cdp cdp_avp db_mysql dialplan ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_ocs ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf outbound presence presence_conference presence_dialoginfo presence_mwi presence_profile presence_reginfo presence_xml pua pua_bla pua_dialoginfo pua_reginfo pua_rpc pua_usrloc pua_xmpp sctp tls utils xcap_client xcap_server xmlops xmlrpc

# the list of static modules
static_modules=

# the list of modules to skip from compile list
skip_modules=

# the list of modules to exclude from compile list
exclude_modules= acc_json acc_radius app_java app_lua app_lua_sr app_mono app_perl app_python app_python3 app_ruby auth_ephemeral auth_identity auth_radius cnxcc cplc crypto db2_ldap db_berkeley db_cassandra db_mongodb db_oracle db_perlvdb db_postgres db_redis db_sqlite db_unixodbc dnssec erlang evapi geoip geoip2 gzcompress h350 http_async_client http_client jansson janssonrpcc json jsonrpcc kafka kazoo lcr ldap log_systemd lost memcached misc_radius ndb_cassandra ndb_mongodb ndb_redis nsq osp peering phonenum pua_json rabbitmq regex rls rtp_media_server snmpstats systemdops topos_redis uuid websocket xhttp_pi xmpp $(skip_modules)

modules_all= $(filter-out modules/CVS,$(wildcard modules/*))
modules_noinc= $(filter-out $(addprefix modules/, $(exclude_modules) $(static_modules)), $(modules_all)) 
modules= $(filter-out $(modules_noinc), $(addprefix modules/, $(include_modules) )) $(modules_noinc) 
modules_configured:=1
```

### 7. Compile and install Kamailio
```
$ cd /usr/local/src/kamailio
$ export RADCLI=1
$ make Q=0 all | tee make_all.txt
$ make install | tee make_install.txt
$ ldconfig
```
The binaries and executable scripts are installed in: /usr/local/sbin
```
kamailio - Kamailio SIP server
kamdbctl - script to create and manage the Databases
kamctl - script to manage and control Kamailio SIP server
kamcmd - CLI - command line tool to interface with Kamailio SIP server
```
To be able to use the binaries from command line, make sure that /usr/local/sbin is set in PATH environment variable. You can check that with echo $PATH. If not and you are using bash, open /root/.bash_profile and at the end add:

```
  PATH=$PATH:/usr/local/sbin
  export PATH
```
Kamailio modules are installed at: <font color="Hotpink">/usr/local/lib64/kamailio/modules</font>

The documentation and readme files are installed at: <font color="Hotpink">/usr/local/share/doc/kamailio</font>

The configuration files are installed at: <font color="Hotpink">/usr/local/etc/kamailio</font>

In case you set the PREFIX variable in <font color="Hotpink">make cfg</font> command, then replace <font color="Hotpink">/usr/local</font> in all paths above with the value of PREFIX in order to locate the files installed.

### 8. Populate MySQL database using kamctlrc command

Edit SIP_DOMAIN and DBENGINE in the <font color="Hotpink">/usr/local/etc/kamailio/kamctlrc</font> configuration file (Used by kamctl and kamdbctl tools).

Set the SIP_DOMAIN to your SIP service domain (or IP address if you don’t have a DNS hostname associated with your SIP service). Set the DBENGINE to be MYSQL and adjust other setting as you want. Finally, uncomment both SIP_DOMAIN and DBENGINE.

In example above, the following values are set for SIP_DOMAIN and DBENGINE

```
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL
```
You can change other values in kamctlrc file. Once you are done updating kamctlrc file, run the script to create the database used by Kamailio:
```
$ kamdbctl create
```
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;
```
$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
```
No Subscribers are added yet

The kamdbctl will add two users in MySQL user tables:
```
- kamailio   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- kamailioro - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database
```
### 9. Edit /etc/default/rtpproxy file as follows:
```
# Defaults for rtpproxy

# The control socket.
#CONTROL_SOCK="unix:/var/run/rtpproxy/rtpproxy.sock"
# To listen on an UDP socket, uncomment this line:
#CONTROL_SOCK=udp:127.0.0.1:22222
CONTROL_SOCK=udp:127.0.0.1:7722

# Additional options that are passed to the daemon.
EXTRA_OPTS="-l 172.24.15.30 -d DBUG:LOG_LOCAL0"
```
here, <font color="Hotpink">-l <PUBLIC_IP></font>

Then run,
```
$ systemctl restart rtpproxy
```
### 10. Edit configuration file to fit your requirements for the VoIP platform:

You have to edit the /usr/local/etc/kamailio/kamailio.cfg configuration file.

Follow the instruction in the comments to enable usage of MySQL. Basically you have to add several lines at the top of config file, like:
```
#!define WITH_MYSQL
#!define WITH_AUTH
#!define WITH_USRLOCDB
#!define WITH_NAT

(uncomment this line)
auto_aliases=no

(uncomment this line and enter the DNS domain created above)
alias="ims.mnc000.mcc460.3gppnetwork.org"

(uncomment this line, 10.4.128.21 is the internal IP and 172.24.15.30 is the Public/Floating IP)
listen=udp:10.4.128.21:5060 advertise 172.24.15.30:5060
listen=tcp:10.4.128.21:5060 advertise 172.24.15.30:5060

(Further down, we will need to modify the rtpproxy_sock value to match the CONTROL_SOCK option we set for RTPProxy in /etc/default/rtpproxy)
modparam("rtpproxy", "rtpproxy_sock", "udp:127.0.0.1:7722")
```
If you changed the password for the ‘kamailio’ user of MySQL, you have to update the value for ‘DBURL’ parameters.

### 11. The init.d script
The init.d script can be used to start/stop the Kamailio server in a nicer way. A sample of init.d script for Kamailio is provided at:
```
/usr/local/src/kamailio/pkg/kamailio/deb/debian/kamailio.init
```
Just copy the init file into the /etc/init.d/kamailio. Then change the permissions:
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.init /etc/init.d/kamailio
$ chmod 755 /etc/init.d/kamailio
```
Then edit the /etc/init.d/kamailio file updating the $DAEMON and $CFGFILE values:
```
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin
DAEMON=/usr/local/sbin/kamailio
CFGFILE=/usr/local/etc/kamailio/kamailio.cfg
```
You need to setup a configuration file in the /etc/default/ directory. This file can be found at:

```
/usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default
```
You need to rename the /etc/default/kamailio file to ‘kamailio’ after you’ve copied it. Then edit this file and set RUN_KAMAILIO=yes. Edit the other options as per your setup.
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default /etc/default/kamailio
$ systemctl daemon-reload
```
Create the directory for pid file:
```
$ mkdir -p /var/run/kamailio
```
Default setting is to run Kamailio as user kamailio and group kamailio. For that you need to create the user and set ownership
```
$ adduser --quiet --system --group --disabled-password \
        --shell /bin/false --gecos "Kamailio" \
        --home /var/run/kamailio kamailio
$ chown kamailio:kamailio /var/run/kamailio
```
Then you can start Kamailio using the following commands:
```
$ systemctl start kamailio.service
```
check running processes with: ps axw egrep kamailio

### 12. A quick check for the basic working of SIP server can be done as follows:
Create new subscriber accounts. A new account can be added using kamctl tool via kamctl add <username> <password> (When asked for entering MySQL password for user kamailio@localhost: type kamailiorw, as provided in kamailio.cfg)
```
$ kamctl add test testpasswd
$ kamctl add test2 testpasswd
Setting on OnePlus phones
```
Connect to a network through which SIP server is reachable (either Wi-Fi or LTE)
Goto phone dialer and select the Settings in the menu on top right corner
Then select Call settings
Configure SIP accounts in phones as added above using kamctl:
In Phone 1:
```
Username: test
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
In Phone 2:
```
Username: test2
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test2
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
+ + Set “Receive incoming calls” option to enabled state in both phones
+ + Set “Use SIP calling” to “For all calls”
+ + Add a new contact as follows:

In Phone 1:

Select more option
```
Name: SIP Contact test2 (Any arbitary name)
SIP: test2@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
In Phone 2:

Select more option
```
Name: SIP Contact test (Any arbitary name)
SIP: test@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
Now try calling from either phone
```
Upon completion of this test, set “Receive incoming calls” option to disabled state and set “Use SIP calling” to “Only for SIP calls”
```
### 13. Create new mysql database for pcscf, scscf and icscf, populate databases and grant permissions to respective users identified by a password
```
$ mysql
<mysql> CREATE DATABASE  `pcscf`;
<mysl> CREATE DATABASE  `scscf`;
<mysl> CREATE DATABASE  `icscf`;
```
In all of the below steps, when prompted for mysql root user password, leave it blank i.e. Press Enter
```
$ cd /usr/local/src/kamailio/utils/kamctl/mysql
$ mysql -u root -p pcscf < standard-create.sql
$ mysql -u root -p pcscf < presence-create.sql
$ mysql -u root -p pcscf < ims_usrloc_pcscf-create.sql
$ mysql -u root -p pcscf < ims_dialog-create.sql

$ mysql -u root -p scscf < standard-create.sql
$ mysql -u root -p scscf < presence-create.sql
$ mysql -u root -p scscf < ims_usrloc_scscf-create.sql
$ mysql -u root -p scscf < ims_dialog-create.sql
$ mysql -u root -p scscf < ims_charging-create.sql

$ cd /usr/local/src/kamailio/misc/examples/ims/icscf
$ mysql -u root -p icscf < icscf.sql
```

Verify that following tables are present in respective databases by logging into mysql
```
+-----------------+
| Tables_in_pcscf |
+-----------------+
| active_watchers |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| location        |
| presentity      |
| pua             |
| version         |
| watchers        |
| xcap            |
+-----------------+

+-----------------+
| Tables_in_scscf |
+-----------------+
| active_watchers |
| contact         |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| impu            |
| impu_contact    |
| impu_subscriber |
| presentity      |
| pua             |
| ro_session      |
| subscriber      |
| version         |
| watchers        |
| xcap            |
+-----------------+

+---------------------+
| Tables_in_icscf     |
+---------------------+
| nds_trusted_domains |
| s_cscf              |
| s_cscf_capabilities |
+---------------------+

<mysql> grant delete,insert,select,update on pcscf.* to pcscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on scscf.* to scscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to icscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to provisioning@localhost identified by 'provi';
<mysql> GRANT ALL PRIVILEGES ON pcscf.* TO 'pcscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON scscf.* TO 'scscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'icscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'provisioning'@'%' identified by 'provi';
<mysql> FLUSH PRIVILEGES;
```
Then,
```
$ mysql
<mysql> use icscf;
<mysql> INSERT INTO `nds_trusted_domains` VALUES (1,'ims.mnc000.mcc460.3gppnetwork.org');
<mysql> INSERT INTO `s_cscf` VALUES (1,'First and only S-CSCF','sip:scscf.ims.mnc000.mcc460.3gppnetwork.org:6060');
<mysql> INSERT INTO `s_cscf_capabilities` VALUES (1,1,0),(2,1,1);
```
### 14. Copy pcscf, icscf and scscf configuration files to /etc folder and edit accordingly
```
$ cd ~ && git clone https://github.com/herlesupreeth/Kamailio_IMS_Config
$ cd Kamailio_IMS_Config
$ cp -r kamailio_icscf /etc
$ cp -r kamailio_pcscf /etc
$ cp -r kamailio_scscf /etc
```
### 15. Setup the DNS for resolving IMS and EPC components names
```
$ apt install -y bind9
```
Use the below example DNS Zone file to create a DNS Zone file into the bind folder and edit /etc/bind/named.conf.local and /etc/bind/named.conf.options accordingly:
```
$ cd /etc/bind
```
In the below example: Kamailio IMS & DNS server running at 10.4.128.21/172.24.15.30 (Floating IP) and PCRF also at 10.4.128.21/172.24.15.30 (Floating IP)
```
$ cat ims.mnc000.mcc460.3gppnetwork.org
```
```
$ORIGIN ims.mnc000.mcc460.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        ns
ns                      1D IN A         10.4.128.21

pcscf                   1D IN A         10.4.128.21
_sip._udp.pcscf         1D SRV 0 0 5060 pcscf
_sip._tcp.pcscf         1D SRV 0 0 5060 pcscf

icscf                   1D IN A         10.4.128.21
_sip._udp               1D SRV 0 0 4060 icscf
_sip._tcp               1D SRV 0 0 4060 icscf

scscf                   1D IN A         10.4.128.21
_sip._udp.scscf         1D SRV 0 0 6060 scscf
_sip._tcp.scscf         1D SRV 0 0 6060 scscf

hss                     1D IN A         10.4.128.21
Create another DNS zone for resolving pcrf domain as follows:
```
```
$ cat epc.mnc001.mcc001.3gppnetwork.org
```
```
$ORIGIN epc.mnc001.mcc001.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        epcns
epcns                   1D IN A         10.4.128.21

pcrf                    1D IN A         127.0.0.5
Edit /etc/bind/named.conf.local file as follows:

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "ims.mnc000.mcc460.3gppnetwork.org" {
        type master;
        file "/etc/bind/ims.mnc000.mcc460.3gppnetwork.org";
};

zone "epc.mnc001.mcc001.3gppnetwork.org" {
        type master;
        file "/etc/bind/epc.mnc001.mcc001.3gppnetwork.org";
};
```
Edit /etc/bind/named.conf.options file as follows:
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
    
        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.
    
        //forwarders {
    	// Put here the IP address of other DNS server which could be used if name cannot be resolved with DNS server running in this machine (Optional)
    	//10.4.128.2;
        //};
    
        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation no;
        allow-query { any; };
    
        auth-nxdomain no;    # conform to RFC1035
        //listen-on-v6 { any; };
};
```
```
cp /etc/bind/epc.mnc000.mcc460.3gppnetwork.org /var/named/
cp /etc/bind/ims.mnc000.mcc460.3gppnetwork.org /var/named/
```
```
$ systemctl restart bind9
```
Then, test DNS resolution by adding following entries on top of all other entries in /etc/resolv.conf (make sure it persist across reboots)
```
search ims.mnc000.mcc460.3gppnetwork.org
nameserver 10.4.128.21
```
Finally, ping to ensure

```
$ ping pcscf
PING pcscf.ims.mnc000.mcc460.3gppnetwork.org (10.4.128.21) 56(84) bytes of data.
64 bytes from localhost (10.4.128.21): icmp_seq=1 ttl=64 time=0.017 ms
64 bytes from localhost (10.4.128.21): icmp_seq=2 ttl=64 time=0.041 ms
```
To make changes in /etc/resolv.conf be persistent across reboot edit the /etc/netplan/50-cloud-init.yaml file as follows:
```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        ens3:
            dhcp4: true
            match:
                macaddress: fa:16:3e:99:f5:67
            set-name: ens3
            nameservers:
                search: [ims.mnc000.mcc460.3gppnetwork.org,epc.mnc001.mcc001.3gppnetwork.org]
                addresses:
                      - 10.4.128.21
    version: 2
```
$ netplan apply
$ ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
$ systemctl restart systemd-resolved.service
16. Install RTPEngine
Check for dependencies, install dependencies and build .deb packages

$ export DEB_BUILD_PROFILES="pkg.ngcp-rtpengine.nobcg729"
$ apt install dpkg-dev
$ git clone https://github.com/sipwise/rtpengine
$ cd rtpengine && git checkout mr7.4.1
$ dpkg-checkbuilddeps
The above command checks for dependencies and give you a list of dependencies which are missing in the system. The below list is the result of this command

$ apt install debhelper default-libmysqlclient-dev gperf iptables-dev libavcodec-dev libavfilter-dev libavformat-dev libavutil-dev libbencode-perl libcrypt-openssl-rsa-perl libcrypt-rijndael-perl libdigest-crc-perl libdigest-hmac-perl libevent-dev libhiredis-dev libio-multiplex-perl libio-socket-inet6-perl libiptc-dev libjson-glib-dev libnet-interface-perl libpcap0.8-dev libsocket6-perl libspandsp-dev libswresample-dev libsystemd-dev libxmlrpc-core-c3-dev markdown dkms module-assistant keyutils libnfsidmap2 libtirpc1 nfs-common rpcbind
After installing dependencies run the below command again and verify that no dependencies are left out

$ dpkg-checkbuilddeps
This should just return back to shell with no output if all depedencies are met

$ dpkg-buildpackage -uc -us
$ cd ..
$ dpkg -i *.deb
$ cp /etc/rtpengine/rtpengine.sample.conf /etc/rtpengine/rtpengine.conf
Edit this file as follows under [rtpengine]:

interface = 10.4.128.21
Port on which rtpengine binds i.e. listen_ng parameter is udp port 2223. This should be updated in kamailio_pcscf.cfg file at modparam(rtpengine …)

# ----- rtpproxy params -----
modparam("rtpengine", "rtpengine_sock", "1 == udp:localhost:2223")
Edit /etc/default/ngcp-rtpengine-daemon and /etc/default/ngcp-rtpengine-recording-daemon as follows in respective files:

RUN_RTPENGINE=yes
RUN_RTPENGINE_RECORDING=yes
$ cp /etc/rtpengine/rtpengine-recording.sample.conf /etc/rtpengine/rtpengine-recording.conf
$ mkdir /var/spool/rtpengine
$ systemctl restart ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service
$ systemctl enable ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service

$ systemctl stop rtpproxy
$ systemctl disable rtpproxy
$ systemctl mask rtpproxy
Second instance of RTPENGINE can be run as follows (Optional)

$ iptables -I rtpengine -p udp -j RTPENGINE --id 1
$ ip6tables -I INPUT -p udp -j RTPENGINE --id 1
$ echo 'del 1' > /proc/rtpengine/control
$ /usr/sbin/rtpengine --table=1 --interface=10.4.128.21 --listen-ng=127.0.0.1:2224 --tos=184 --pidfile=ngcp-rtpengine-daemon2.pid --no-fallback --foreground
17. Running I-CSCF, P-CSCF and S-CSCF as separate process
First, stop the default kamailio SIP server

$ systemctl stop kamailio
$ systemctl disable kamailio
$ systemctl mask kamailio
Run all the process as root and NOT sudo

$ mkdir -p /var/run/kamailio_pcscf
$ kamailio -f /etc/kamailio_pcscf/kamailio_pcscf.cfg -P /kamailio_pcscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_scscf
$ kamailio -f /etc/kamailio_scscf/kamailio_scscf.cfg -P /kamailio_scscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_icscf
$ kamailio -f /etc/kamailio_icscf/kamailio_icscf.cfg -P /kamailio_icscf.pid -DD -E -e
18. Install Open5GS in the same machine as Kamailio IMS - Install Open5GS from source
Please refer to instructions at https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/

If you are using OpenStack, installing Open5GS and Kamailio IMS on the same machine is very important because the Framed-IP-Address in the AAR request via Rx interface takes received IP address and port in ims_qos module, hence, if the Open5GS is on a separate VM/machine, the IP and port received in received_ip and received_port values seen by Kamailio IMS will be the NATed IP of the Open5GS machine resulting in failing of AAR request.

Modify below mentioned parts of configuration files in addition to Configure Open5GS section. For reference, look at the configuration files at https://github.com/herlesupreeth/Open5gs_Config. These configuration only holds for open5gs tag v1.3.0, please tweak configuration files based on the open5gs tag you use.

Change realm of components to epc.mnc001.mcc001.3gppnetwork.org
Define IP pools for APNs used i.e one for default APN and another for IMS apn
Define P-CSCF address in the pgw configuration
Define a ConnectPeer for pcscf.ims.mnc000.mcc460.3gppnetwork.org with its IP and port in PCRF freediameter configuration
Setup IP tables for the UE pools defined and create appropriate tun interfaces
Below startup script can be used for setting up interfaces:

#!/bin/bash

sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl -w net.ipv6.conf.all.forwarding=1

ip tuntap add name ogstun mode tun
ip addr add 192.168.100.1/24 dev ogstun
ip addr add fd84:6aea:c36e:2b69::/48 dev ogstun
ip link set ogstun mtu 1400
ip link set ogstun up
iptables -t nat -A POSTROUTING -s 192.168.100.0/24 ! -o ogstun -j MASQUERADE
ip6tables -t nat -A POSTROUTING -s fd84:6aea:c36e:2b69::/48 ! -o ogstun -j MASQUERADE
iptables -I INPUT -i ogstun -j ACCEPT
ip6tables -I INPUT -i ogstun -j ACCEPT

ip tuntap add name ogstun2 mode tun
ip addr add 192.168.101.1/24 dev ogstun2
ip addr add fd1f:76f3:da9b:0101::/48 dev ogstun2
ip link set ogstun2 mtu 1400
ip link set ogstun2 up
iptables -I INPUT -i ogstun2 -j ACCEPT
ip6tables -I INPUT -i ogstun2 -j ACCEPT
Add users with following APN settings in Open5GS:

APN Configuration:
---------------------------------------------------------------------------------------------------------------------
| APN      | Type | QCI | ARP | Capability | Vulnerablility | MBR DL/UL(Kbps)     | GBR DL/UL(Kbps) | PGW IP        |
---------------------------------------------------------------------------------------------------------------------
| internet | IPv4 | 9   | 8   | Disabled   | Disabled       | unlimited/unlimited |                 |               |
---------------------------------------------------------------------------------------------------------------------
| ims      | IPv4 | 5   | 1   | Disabled   | Disabled       | 3850/1530           |                 |               |
|          |      | 1   | 2   | Enabled    | Enabled        | 128/128             | 128/128         |               |
|          |      | 2   | 4   | Enabled    | Enabled        | 128/128             | 128/128         |               |
---------------------------------------------------------------------------------------------------------------------
Finally, make sure of the following in Open5GS

PCO options which indicate the address of the Proxy-CSCF
Need to indicate support for Voice-over-Packet-Switched (VoPS) in NAS message to UE from EPC
19. Setup FoHSS in order to talk with I-CSCF and S-CSCF
Requirements for FoHSS: Install Java JDK and ant

Download Oracle Java 7 JDK from following link using a browser:

https://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html
$ mkdir -p  /usr/lib/jvm/
$ tar -zxf jdk-7u79-linux-x64.tar.gz -C /usr/lib/jvm/
$ update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_79/bin/java 100
$ update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_79/bin/javac 100
Verify that java has been successfully configured by running:

$ update-alternatives --display java
java - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/java
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/java
  link java is /usr/bin/java
/usr/lib/jvm/jdk1.7.0_79/bin/java - priority 100

$ update-alternatives --display javac
javac - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link javac is /usr/bin/javac
/usr/lib/jvm/jdk1.7.0_79/bin/javac - priority 100

$ update-alternatives --config java
(select java jdk1.7.0_79)
$ update-alternatives --config javac
Check java version

$ java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
Install Ant

$ cd ~
$ wget http://archive.apache.org/dist/ant/binaries/apache-ant-1.9.14-bin.tar.gz
$ tar xvfvz apache-ant-1.9.14-bin.tar.gz
$ mv apache-ant-1.9.14 /usr/local/
$ sh -c 'echo ANT_HOME=/usr/local/  >> /etc/environment'
$ ln -s /usr/local/apache-ant-1.9.14/bin/ant /usr/bin/ant
Verfiy ant version as follows:

$ ant -version
Apache Ant(TM) version 1.9.14 compiled on March 12 2019
Create working directories for OpenIMSCore:

$ mkdir /opt/OpenIMSCore
$ cd /opt/OpenIMSCore
Download:

$ git clone https://github.com/herlesupreeth/FHoSS
Compile:
```
$ cd FHoSS
$ export JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79"
$ export CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
$ ant compile deploy | tee ant_compile_deploy.txt
```
Create configurator.sh using below script to change domain names and IP address in all configuration files
```
$ cd deploy
$ vim configurator.sh
```
```
#!/bin/bash

# Initialization & global vars
# if you execute this script for the second time
# you should change these variables to the latest
# domain name and ip address
DDOMAIN="open-ims\.test"
DSDOMAIN="open-ims\\\.test"
DEFAULTIP="127\.0\.0\.1"
CONFFILES=`ls *.cfg *.xml *.sql *.properties 2>/dev/null`

# Interaction
printf "Domain Name:"
read domainname 
printf "IP Adress:"
read ip_address

# input domain is to be slashed for cfg regexes 
slasheddomain=`echo $domainname | sed 's/\./\\\\\\\\\./g'`

  if [ $# != 0 ] 
  then 
  printf "changing: "
      for j in $* 
      do
    sed -i -e "s/$DDOMAIN/$domainname/g" $j
    sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $j
    sed -i -e "s/$DEFAULTIP/$ip_address/g" $j
    printf "$j " 
      done
  echo 
  else 
  printf "File to change [\"all\" for everything, \"exit\" to quit]:"
  # loop
      while read filename ;
      do
        if [ "$filename" = "exit" ] 
        then 
        printf "exitting...\n"
        break ;
    
      elif [ "$filename" = "all" ]
      then    
          printf "changing: "
         for i in $CONFFILES 
         do
        sed -i -e "s/$DDOMAIN/$domainname/g" $i
        sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $i
        sed -i -e "s/$DEFAULTIP/$ip_address/g" $i
        
        printf "$i " 
         done 
         echo 
         break;
    
        elif [ -w $filename ] 
        then
            printf "changing $filename \n"
            sed -i -e "s/$DDOMAIN/$domainname/g" $filename
            sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $filename
            sed -i -e "s/$DEFAULTIP/$ip_address/g" $filename
    
          else 
          printf "cannot access file $filename. skipping... \n" 
        fi
        printf "File to Change:"
      done 
  fi
```
```
$ chmod +x configurator.sh
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:10.4.128.21

$ grep -r "open-ims"
(Change realm name in the below file from open-ims.test to ims.mnc000.mcc460.3gppnetwork.org)
$ vim webapps/hss.web.console/WEB-INF/web.xml
$ vim hibernate.properties
```
And, change the following line:
```
hibernate.connection.url=jdbc:mysql://127.0.0.1:3306/hss_db
$ cp configurator.sh ../scripts/
$ cd ../scripts
$ grep -r "open-ims"
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:10.4.128.21

$ cp configurator.sh ../config/
$ cd ../config
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:10.4.128.21

$ cd ../src-web
$ vim WEB-INF/web.xml
```
And, change open-ims.test to ims.mnc000.mcc460.3gppnetwork.org

Prepare mysql database:
```
$ mysql
<mysql> drop database hss_db;
<mysql> create database hss_db;
<mysql> quit
```
Import database located at /opt/OpenIMSCore into hss_db
```
$ cd /opt/OpenIMSCore
$ mysql -u root -p hss_db < FHoSS/scripts/hss_db.sql
$ mysql -u root -p hss_db < FHoSS/scripts/userdata.sql
```
Check grants for mysql access rights at first time installation:
```
$ mysql
# See last line in hss_db.sql:
<mysql> grant delete,insert,select,update on hss_db.* to hss@localhost identified by 'hss';
<mysql> grant delete,insert,select,update on hss_db.* to hss@'%' identified by 'hss';
```
Check database if domain names are o.k. in various entries and privileges
```
$ mysql -u hss -p
<mysql> show databases;
<mysql> use hss_db;
<mysql> select * from impu;
```
Prepare script-file, start HSS

Copy startup.sh to hss.sh in root directory
```
$ cp /opt/OpenIMSCore/FHoSS/deploy/startup.sh /root/hss.sh
```
And, add the following to hss.sh <font color="Hotpink">before echo Building Classpath</font>
```
cd /opt/OpenIMSCore/FHoSS/deploy
JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79"
CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
```
Start HSS using hss.sh

```
$ /root/hss.sh
```
Access the web-interface of HSS: <font color="Hotpink">http://<IMS_VM_FLOATING_IP>:8080/hss.web.console/</font>

For example, http://172.24.15.30:8080/hss.web.console/
```
user:      hssAdmin
password:  hss
```

Then, edit the /etc/hosts file as follows:

In the below example. epc-ims is the hostname of the machine
```
root@epc-ims:~# cat /etc/hosts
127.0.0.1	localhost
127.0.0.1	epc-ims
```
20. Add IMS subscription use in FoHSS as follows from the Web GUI
Assuming IMSI of the user as 001010123456791 and MSISDN is 0198765432100

Login to the HSS web console.
Navigate to the User Identities page	
Create the IMSU 
Click IMS Subscription / Create
Enter:
Name = 001010123456791
Capabilities Set = cap_set1
Preferred S-CSCF = scsf1
Click Save

Create the IMPI and Associate the IMPI to the IMSU
Click Create & Bind new IMPI
Enter:
Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Secret Key = 8baf473f2f8fd09487cccbd7097c6862 (Ki value as in Open5GS HSS database)
Authentication Schemes - All
Default = Digest-AKAv1-MD5
AMF = 8000 (As in Open5GS HSS database)
OP = 11111111111111111111111111111111 (As in Open5GS HSS database)
SQN = 000000021090 (SQN value as in Open5GS HSS database)
Click Save

Create and Associate IMPI to IMPU
Click Create & Bind new IMPU
Enter:
Identity = sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Barring = Yes
Service Profile = default_sp
Charging-Info Set = default_charging_set
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Now, goto Public User Identity and create further IMPUs as following

1. tel:0198765432100

Public User Identity -IMPU-
Identity = tel:0198765432100
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add

2. sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org

Public User Identity -IMPU-
Identity = sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add

And, finally add these IMPUs as implicit set of IMSI derived IMPU in HSS i.e sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org as follows:

1. Goto to IMPU sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
2. In "Add IMPU(s) to Implicit-Set" section give IMPU Identity created above to be added to this IMPU
21. APN settings
Clear all previous APN settings

Then, create APN as follows:

First create internet APN, APN name: internet, APN type: default –> Save APN
Then, create ims APN, APN name: ims, APN type: ims –> Save APN
22. eNB settings
Must have in the eNB:

Support for QoS
Support for Dedicated radio bearer creation
Make sure to check the DRB configuration with respect to QCI of APN accordingly (QCI 5 for ims)
On the eNB machine have the following static routes (since internal IP of the VM is advertised in S1AP messages and UE wont find the core in Uplink)

$ ip r add 10.4.128.21/32 via 172.24.15.30
23. USIM and UE settings
Make sure to disable SQN check in Sysmocom SIM cards using sysmo-usim-tool tool https://github.com/herlesupreeth/sysmo-usim-tool
Tested with OnePlus 5 with following methods (Official Google method is the recommended method to prevent damage to phone)
(Official Google method) - Please follow the instructions in the following link @herlesupreeth/CoIMS_Wiki to force enable VoLTE using Carrier Privileges
(Risky method) With modfication to enable force IMS registration is a must or else UE will not even attempt to connect to P-CSCF. Need to apply the fix back after each update. https://forum.xda-developers.com/oneplus-5t/how-to/guide-volte-vowifi-german-carriers-t3817542
24. Start IMS components and FoHSS followed by Open5GS and eNB, then try connecting the phones
25. Test voice call
Assuming IMSI of the user1 as 001010123456791 and MSISDN is 0198765432100 and IMSI of the user2 as 001010123456792 and MSISDN is 0298765432100. Try calling user2 from user1 by dialing its MSISDN ie. 0298765432100

You can see the sample traffic. – [volte.pcapng].
26. For debugging
Debug using wireshark at Open5GS machine and following wireshark display filter

s1ap || gtpv2 || pfcp || diameter || diameter.3gpp || sip
Also,

Debugging Diameter messages between PCRF and P-CSCF in Wireshark if the TCP/SCTP port other than 3868

Open Wireshark –> Preferences –> Protocols –> Diameter –> Change to whatever ports are being used

Appendix
Open5GS
Sukchan Lee
acetcom@gmail.com
open5gs
Open5GS is a C-language implementation of 5G Core and EPC, i.e. the core network of NR/LTE network (Release-16)
```
```

## 安装rtpengine

```
#!/bin/bash
# Install the packages required to compile RTP engine
set -e
yum install iptables-devel kernel-devel kernel-headers xmlrpc-c-devel
yum install "kernel-devel-uname-r == $(uname -r)"
yum install glib glib-devel gcc zlib zlib-devel openssl openssl-devel pcre pcre-devel libcurl libcurl-devel xmlrpc-c xmlrpc-c-devel
yum install libevent-devel glib2-devel json-glib-devel gperf libpcap-devel git hiredis hiredis-devel redis perl-IPC-Cmd
# MariaDB ver 10+
yum install MariaDB-devel MariaDB-client MariaDB-shared MariaDB-server
# Spandsp
yum install spandsp-devel spandsp
# epel
yum install epel-release
# libwebsockets
yum install libwebsockets libwebsockets-devel
# ffmpeg
rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Fvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
# check for Nux desktop repo by yum repolist
yum -y install ffmpeg ffmpeg-devel
# Get The Latest Release Of RTPEngine Source From RTPEngine’s GitHub Repository
cd /usr/local/src
if [ -d rtpengine ]; then
    cd rtpengine
    make clean
    git pull
else
    git clone https://github.com/sipwise/rtpengine.git
fi
# Compile and install the daemon
cd /usr/local/src/rtpengine/daemon/
make
cp rtpengine /usr/sbin/rtpengine
# Compile and install iptables extension
cd /usr/local/src/rtpengine/iptables-extension
make all
cp libxt_RTPENGINE.so /usr/lib64/xtables/.
# Compile and install the kernel module xt_RTPENGINE
cd /usr/local/src/rtpengine/kernel-module
make
# determine kernel release
uname -a
kernel_ver=`uname -r`
cp xt_RTPENGINE.ko /lib/modules/${kernel_ver}/extra/xt_RTPENGINE.ko
depmod -a
# load module at boot time
# Add the following lines to /etc/modules-load.d/rtpengine.conf (add the following lines)
CONF_FILE=/etc/modules-load.d/rtpengine.conf
echo '# load xt_RTPENGINE module' > ${CONF_FILE}
echo 'xt_RTPENGINE' >> ${CONF_FILE}
# load the module and check
modprobe xt_RTPENGINE
lsmod | grep xt_RTPENGINE
# check files
ls -l /proc/rtpengine/control
ls -l /proc/rtpengine/list

TableID=0
# add forwarding table with an ID=$TableID. $TableID is a number between 0-63
echo 'add 0' > /proc/rtpengine/control
# Adding iptables rules to forward the incoming packets to xt_RTPENGINE module
iptables -I INPUT -p udp -j RTPENGINE --id $TableID
ip6tables -I INPUT -p udp -j RTPENGINE --id $TableID
```
```

## WSL ubuntu安装docker，不使用docker desktop, 可行， 使用下面的镜像。（debian失败）
1. ubuntu 18.04 网易镜像源- 可用
```
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
```
2. docker 安装步骤
```
https://www.cnblogs.com/LiangSW/p/9842295.html

安装docker
linux子系统已经收工，下面安装docker。我们可以直接根据docker文档中提供的ubuntu安装docker的方法进行操作 ps：使用管理员打开ubuntu1804

因为都知道的网络原因安装时可能会timeout等其他情况,我们可以使用国内镜像 https://mirrors.tuna.tsinghua.edu.cn/ 清华大学开源软件镜像站替换下面的链接
https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu/gpg
https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu/

sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt-get install \ apt-transport-https \ ca-certificates \ curl \ software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \ "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) \ stable"
sudo apt-get update
sudo apt-get install docker-ce
一顿操作已经安装结束,使用 sudo service docker start 开启docker守护进程
使用 docker version 查看版本
```
3. 以root启动wsl ubuntu
```
ubuntu1804.exe config --default-user root

```
4. 重启WSL ubuntu

```
PS C:\WINDOWS\system32> net stop LxssManager
The LxssManager service is stopping.
The LxssManager service was stopped successfully.

PS C:\WINDOWS\system32> net start LxssManager
The LxssManager service is starting.
The LxssManager service was started successfully.
```

## libcurl

- [libcurl库使用方法，好长，好详细](https://www.cnblogs.com/heluan/p/10177475.html)

- [libcurl库安装（Linux）](https://blog.csdn.net/simonyucsdy/article/details/82835268)

## yum只下载不安装

- yum install --downloadonly --downloaddir=/download python-devel

### [Linux-yum只下载不安装](https://www.cnblogs.com/lizhewei/p/11763053.html)

**目录**

- 通过yum命令只下载rpm包不安装
  - [方法一：yumdownloader](https://www.cnblogs.com/lizhewei/p/11763053.html#_label0_0)
  - [方法二：yum --downloadonly](https://www.cnblogs.com/lizhewei/p/11763053.html#_label0_1)
  - [方法三：reposync](https://www.cnblogs.com/lizhewei/p/11763053.html#_label0_2)

 

#### 通过yum命令只下载rpm包不安装



##### 方法一：yumdownloader

如果只想通过 yum 下载软件的软件包，但是不需要进行安装的话，可以使用 yumdownloader 命令；  yumdownloader 命令在软件包 yum-utils 里面。

```
# yum install yum-utils -y
```

常用参数说明：

```
--destdir 指定下载的软件包存放路径
--resolve 解决依赖关系并下载所需的包
```

示例：

```shell
# yumdownloader --destdir=/tmp --resolve httpd
```

##### 方法二：yum --downloadonly

yum命令的参数有很多，其中就有只是下载而不需要安装的命令，并且也会自动解决依赖；通常和 --downloaddir 参数一起使用。

示例：

```shell
# yum install --downloadonly --downloaddir=/tmp/ vsftpd

# yum reinstall --downloadonly --downloaddir=/tmp/ vsftpd
```

说明：如果该服务器已经安装了需要下载的软件包，那么使用 install下载就不行，可以使用reinstall下载。 放心（不会真的安装和重新安装，因为后面加了 --downloadonly，表明只是下载。

如果提示没有--downloadonly选项则需要安装yum-plugin-downloadonly软件包；

```shell
# yum install yum-plugin-downloadonly
```



##### 方法三：reposync

该命令更加强大，可以将远端yum仓库里面的包全部下载到本地。这样构建自己的yum仓库，就不会遇到网络经常更新包而头痛的事情了。 该命令也是来自与 yum-utils 里面。

```shell
# yum install yum-utils -y
```

常用参数说明：

```
-r    指定已经本地已经配置的 yum 仓库的 repo源的名称。
-p    指定下载的路径
```

示例：

```shell
# reposync -r epel -p /opt/local_epel
```

## [curl 支持 http2](https://www.cnblogs.com/brookin/p/10713166.html)

### 源码安装

#### 安装 nghttp2

```shell
git clone https://github.com/tatsuhiro-t/nghttp2.git
cd nghttp2
autoreconf -i
automake
autoconf
./configure --prefix=/user/local/
make
sudo make install

add below to .bashrc and source it before compile curl
export LD_LIBRARY_PATH=/usr/local/lib64:/usr/local/lib
yum install zlib-devel.x86_64
```

#### 安装 openssl

```shell
yum install zlib-devel.x86_6

wget  http://www.openssl.org/source/openssl-1.1.0e.tar.gz
tar -zxvf openssl-1.1.0e.tar.gz
cd  ./openssl-1.1.0e
./config shared zlib  --prefix=/usr/local/openssl && make && make install
./config -t
make depend
cd /usr/local
ln -s openssl ssl
echo "/usr/local/openssl/lib" >> /etc/ld.so.conf
ldconfig

echo "export OPENSSL=/usr/local/openssl/bin" >> /etc/profile

echo "export PATH=$OPENSSL:$PATH:$HOME/bin" >> /etc/profile

source /etc/profile

13、移除老版本的openssl，创建新的软连接；这个地方注意路径

mv /usr/bin/openssl /usr/bin/openssl.old
mv /usr/include/openssl /usr/include/openssl.old
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl
ln -s /usr/local/openssl/include/openssl /usr/include/openssl
ln -sf /usr/local/openssl/lib/libcrypto.so.1.0.0 /lib/libcrypto.so.6
echo "/usr/local/openssl/lib" >>/etc/ld.so.conf 

ldconfig -v

14、依次如下执行：

ldd /usr/local/openssl/bin/openssl

会出现类似如下信息：

        linux-gate.so.1 =>  (0x0079f000)
        libssl.so.1.1 => /usr/local/openssl/lib/libssl.so.1.1 (0x002a8000)
        libcrypto.so.1.1 => /usr/local/openssl/lib/libcrypto.so.1.1 (0x00306000)
        libz.so.1 => /lib/libz.so.1 (0x00775000)
        libdl.so.2 => /lib/libdl.so.2 (0x00725000)
        libpthread.so.0 => /lib/libpthread.so.0 (0x0072c000)
        libc.so.6 => /lib/libc.so.6 (0x00593000)
        /lib/ld-linux.so.2 (0x0056d000)

15、查看路径
which openssl

/usr/local/openssl/bin/openssl

16、查看版本
openssl version
OpenSSL 1.1.0e  16 Feb 2017

or you can install a develop version
yum -y install openssl-devel
```

#### 编译 curl

```shell
wget http://curl.haxx.se/download/curl-7.80.0.tar.gz
tar -xvjf curl-7.80.0.tar.gz
cd curl-7.80.0
./configure --with-nghttp2=/usr/local --with-ssl
make && make install

编译时注意观察
HTTP2 support: enabled (nghttp2)

echo '/usr/local/lib' > /etc/ld.so.conf.d/local.conf
ldconfig
```

验证

```
curl --version
curl 7.64.1 (x86_64-pc-linux-gnu) libcurl/7.64.1 OpenSSL/1.0.2k zlib/1.2.7 nghttp2/1.38.0-DEV
Release-Date: 2019-03-27
Protocols: dict file ftp ftps gopher http https imap imaps pop3 pop3s rtsp smb smbs smtp smtps telnet tftp 
Features: AsynchDNS HTTP2 HTTPS-proxy IPv6 Largefile libz NTLM NTLM_WB SSL UnixSockets
```

#### 测试

```
curl --http2 -I https://nghttp2.org

HTTP/2 200 
date: Mon, 15 Apr 2019 12:53:49 GMT
content-type: text/html
last-modified: Fri, 08 Mar 2019 12:33:02 GMT
etag: "5c8260fe-19d8"
accept-ranges: bytes
content-length: 6616
x-backend-header-rtt: 0.011655
strict-transport-security: max-age=31536000
server: nghttpx
via: 2 nghttpx
x-frame-options: SAMEORIGIN
x-xss-protection: 1; mode=block
x-content-type-options: nosniff
```

附注

```
 -I, --head 	(HTTP/FTP/FILE) Fetch the HTTP-header only!
```

------

### yum 安装

#### 安装 yum 源

```
rpm -ivh http://mirror.city-fan.org/ftp/contrib/yum-repo/city-fan.org-release-2-1.rhel7.noarch.rpm
```

#### 新建 yum 源(功能与 安装 yum 源 相同)

vim /etc/yum.repos.d/city-fan.repo

```
[cityfan]  
name=cityfan 
baseurl=http://www.city-fan.org/ftp/contrib/yum-repo/rhel7/x86_64/
enabled=1  
gpgcheck=0
```

#### 更新curl

```
yum update curl
```
- [CentOS使用rpm离线安装mariadb](https://www.cnblogs.com/cobcmw/p/11420311.html)



# 十个 SCP 传输命令例子

Linux系统管理员应该很熟悉**CLI**环境，因为通常在Linux服务器中是不安装**GUI**的。**SSH**可能是Linux系统管理员通过远程方式安全管理服务器的最流行协议。在**SSH**命令中内置了一种叫**SCP**的命令，用来在服务器之间安全传输文件。

![十个 SCP 传输命令例子_Linux](http://www.wenwenyun.com/uploads/allimg/c141227/1419Dc23Z260-123234.png)

以下命令可以解读为：用“**username account**”“**拷贝 source file name**”到“**destination host**”上的“**destination folder**”里。

### SCP命令的基本语法

```
scp source_file_name username@destination_host:destination_folder1.
```

**SCP**命令有很多可以使用的参数，这里指的是每次都会用到的参数。

## 用-v参数来提供SCP进程的详细信息

不带参数的基本**SCP**命令会在后台拷贝文件，除非操作完成或者有错误出现，否则用户在界面上是看不到任何提示信息的。你可以用“**-v**”参数来在屏幕上打印出调试信息，这能帮助你调试连接、认证和配置的一些问题。

```
pungki@mint ~/Documents $ scp -v Label.pdf mrarianto@202.x.x.x:.1.
```

### 部分输出

```
Executing: program /usr/bin/ssh host 202.x.x.x, user mrarianto, command scp -v -t .
OpenSSH_6.0p1 Debian-3, OpenSSL 1.0.1c 10 May 2012
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to 202.x.x.x [202.x.x.x] port 22.
debug1: Connection established.
debug1: Host '202.x.x.x' is known and matches the RSA host key.
debug1: Found key in /home/pungki/.ssh/known_hosts:1
debug1: ssh_rsa_verify: signature correct
debug1: Next authentication method: password
mrarianto@202.x.x.x's password:
debug1: Authentication succeeded (password).
Authenticated to 202.x.x.x ([202.x.x.x]:22).
Sending file modes: C0770 3760348 Label.pdf
Sink: C0770 3760348 Label.pdf
Label.pdf 100% 3672KB 136.0KB/s 00:27
Transferred: sent 3766304, received 3000 bytes, in 65.2 seconds
Bytes per second: sent 57766.4, received 46.0
debug1: Exit status 01.2.3.4.5.6.7.8.9.10.11.12.13.14.15.16.17.18.19.
```

## 从源文件获取修改时间、访问时间和模式

“**-p**”参数会帮到把预计的时间和连接速度会显示在屏幕上。

```
pungki@mint ~/Documents $ scp -p Label.pdf mrarianto@202.x.x.x:.1.
```

### 部分输出

```
mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 126.6KB/s 00:291.2.
```

## 用-C参数来让文件传输更快

有一个参数能让传输文件更快，就是“**-C**”参数，它的作用是不停压缩所传输的文件。它特别之处在于压缩是在网络传输中进行，当文件传到目标服务器时，它会变回压缩之前的原始大小。

来看看这些命令，我们使用一个**93 Mb**的单一文件来做例子。

```
pungki@mint ~/Documents $ scp -pv messages.log mrarianto@202.x.x.x:.1.
```

### 部分输出

```
Executing: program /usr/bin/ssh host 202.x.x.x, user mrarianto, command scp -v -p -t .
OpenSSH_6.0p1 Debian-3, OpenSSL 1.0.1c 10 May 2012
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to 202.x.x.x [202.x.x.x] port 22.
debug1: Connection established.
debug1: identity file /home/pungki/.ssh/id_rsa type -1
debug1: Found key in /home/pungki/.ssh/known_hosts:1
debug1: ssh_rsa_verify: signature correct
debug1: Trying private key: /home/pungki/.ssh/id_rsa
debug1: Next authentication method: password
mrarianto@202.x.x.x's password:
debug1: Authentication succeeded (password).
Authenticated to 202.x.x.x ([202.x.x.x]:22).
debug1: Sending command: scp -v -p -t .
File mtime 1323853868 atime 1380425711
Sending file timestamps: T1323853868 0 1380425711 0
messages.log 100% 93MB 58.6KB/s 27:05
Transferred: sent 97614832, received 25976 bytes, in 1661.3 seconds
Bytes per second: sent 58758.4, received 15.6
debug1: Exit status 01.2.3.4.5.6.7.8.9.10.11.12.13.14.15.16.17.18.19.20.21.
```

不用“**-C**”参数来拷贝文件，结果用了**1661.3**秒，你可以比较下用了“**-C**”参数之后的结果。

```
pungki@mint ~/Documents $ scp -Cpv messages.log mrarianto@202.x.x.x:.1.
```

### 部分输出

```
Executing: program /usr/bin/ssh host 202.x.x.x, user mrarianto, command scp -v -p -t .
OpenSSH_6.0p1 Debian-3, OpenSSL 1.0.1c 10 May 2012
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to 202.x.x.x [202.x.x.x] port 22.
debug1: Connection established.
debug1: identity file /home/pungki/.ssh/id_rsa type -1
debug1: Host '202.x.x.x' is known and matches the RSA host key.
debug1: Found key in /home/pungki/.ssh/known_hosts:1
debug1: ssh_rsa_verify: signature correct
debug1: Next authentication method: publickey
debug1: Trying private key: /home/pungki/.ssh/id_rsa
debug1: Next authentication method: password
mrarianto@202.x.x.x's password:
debug1: Enabling compression at level 6.
debug1: Authentication succeeded (password).
Authenticated to 202.x.x.x ([202.x.x.x]:22).
debug1: channel 0: new [client-session]
debug1: Sending command: scp -v -p -t .
File mtime 1323853868 atime 1380428748
Sending file timestamps: T1323853868 0 1380428748 0
Sink: T1323853868 0 1380428748 0
Sending file modes: C0600 97517300 messages.log
messages.log 100% 93MB 602.7KB/s 02:38
Transferred: sent 8905840, received 15768 bytes, in 162.5 seconds
Bytes per second: sent 54813.9, received 97.0
debug1: Exit status 0
debug1: compress outgoing: raw data 97571111, compressed 8806191, factor 0.09
debug1: compress incoming: raw data 7885, compressed 3821, factor 0.481.2.3.4.5.6.7.8.9.10.11.12.13.14.15.16.17.18.19.20.21.22.23.24.25.26.27.28.29.
```

看到了吧，压缩了文件之后，传输过程在**162.5**秒内就完成了，速度是不用“**-C**”参数的10倍。如果你要通过网络拷贝很多份文件，那么“**-C**”参数能帮你节省掉很多时间。

有一点我们需要注意，这个压缩的方法不是适用于所有文件。当源文件已经被压缩过了，那就没办法再压缩很多了。诸如那些像**.zip**，**.rar**，**pictures**和**.iso**的文件，用“**-C**”参数就没什么意义。

## 选择其它加密算法来加密文件

**SCP**默认是用“**AES-128**”加密算法来加密传输的。如果你想要改用其它加密算法来加密传输，你可以用“**-c**”参数。我们来瞧瞧。

```
pungki@mint ~/Documents $ scp -c 3des Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 282.5KB/s 00:131.2.3.4.
```

上述命令是告诉**SCP**用**3des algorithm**来加密文件。要注意这个参数是“**-c**”（小写）而不是“**-C**“（大写）。

## 限制带宽使用

还有一个很有用的参数是“**-l**”参数，它能限制使用带宽。如果你为了拷贝很多文件而去执行了一份自动化脚本又不希望带宽被**SCP**进程耗尽，那这个参数会非常管用。

```
pungki@mint ~/Documents $ scp -l 400 Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 50.3KB/s 01:131.2.3.4.
```

在“**-l**”参数后面的这个**400**值意思是我们给**SCP**进程限制了带宽为**50 KB/秒**。有一点要记住，带宽是以**千比特/秒** (**kbps**)表示的，而**8 比特**等于**1 字节**。

因为**SCP**是用**千字节/秒** (**KB/s**)计算的，所以如果你想要限制**SCP**的最大带宽只有**50 KB/s**，你就需要设置成**50 x 8 = 400**。

## 指定端口

通常**SCP**是把**22**作为默认端口。但是为了安全起见SSH 监听端口改成其它端口。比如说，我们想用**2249**端口，这种情况下就要指定端口。命令如下所示。

```
pungki@mint ~/Documents $ scp -P 2249 Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 262.3KB/s 00:141.2.3.4.
```

确认一下写的是大写字母“**P**”而不是“**p**“，因为“**p**”已经被用来保留源文件的修改时间和模式（LCTT 译注：和 ssh 命令不同了）。

## 递归拷贝文件和文件夹

有时我们需要拷贝文件夹及其内部的所有**文件**/**子文件夹**，我们如果能用一条命令解决问题那就更好了。**SCP**用“**-r**”参数就能做到。

```
pungki@mint ~/Documents $ scp -r documents mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 282.5KB/s 00:13
scp.txt 100% 10KB 9.8KB/s 00:001.2.3.4.5.
```

拷贝完成后，你会在目标服务器中找到一个名为“**documents**”的文件夹，其中就是所拷贝的所有文件。“**documents**”是系统自动创建的文件夹。

## 禁用进度条和警告/诊断信息

如果你不想从SCP中看到进度条和警告/诊断信息，你可以用“**-q**”参数来静默它们，举例如下。

```
pungki@mint ~/Documents $ scp -q Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
pungki@mint ~/Documents $1.2.3.4.
```

正如你所看到的，在你输入密码之后，没有任何关于SCP进度的消息反馈。进度完成后，你也看不到任何提示。

## 用SCP通过代理来拷贝文件

代理服务器经常用于办公环境，SCP自然是没有经过代理方面的配置的。当你的环境正在使用代理，那么你就必须要“告诉”SCP与代理关联起来。

场景如下：代理的地址是**10.0.96.6**，端口是8080。该代理还实现了用户认证功能。首先，你需要创建一个“**~/.ssh/config**”文件，其次把以下命令输入进该文件。

```
ProxyCommand /usr/bin/corkscrew 10.0.96.6 8080 %h %p ~/.ssh/proxyauth1.
```

接着你需要创建一个同样包括以下命令的“**~/.ssh/proxyauth**”文件。

```
myusername:mypassword1.
```

然后你就可以像往常一样使用SCP了。

请注意corkscrew可能还没有安装在你的系统中。在我的Linux Mint中，我需要首先先用标准Linux Mint安装程序来安装它。

```
$ apt-get install corkscrew1.
```

对于其它的一些基于yum安装的系统，用户能用以下的命令来安装corkscrew。

```
# yum install corkscrew1.
```

还有一点就是因为“**~/.ssh/proxyauth**”文件中以明文的格式包含了你的“**用户名**”和“**密码**”，所以请确保该文件只能你来查看。

## 选择不同的ssh_config文件

对于经常在公司网络和公共网络之间切换的移动用户来说，一直改变SCP的设置显然是很痛苦的。如果我们能放一个保存不同配置的**ssh_config**文件来匹配我们的需求那就很好了。

### 以下是一个简单的场景

代理是被用来在公司网络但不是公共网络并且你会定期切换网络时候使用的。

```
pungki@mint ~/Documents $ scp -F /home/pungki/proxy_ssh_config Label.pdf

mrarianto@202.x.x.x:.
mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 282.5KB/s 00:131.2.3.4.5.
```

默认情况下每个用户会把“**ssh_config**”文件放在“**~/.ssh/config**“路径下。用兼容的代理创建一个特定的“**ssh_config**”文件，能让你切换网络时更加方便容易。

当你处于公司网络时，你可以用“**-F**”参数，当你处于公共网络时，你可以忽略掉“**-F**”参数。

以上就是关于**SCP**的全部内容了，你可以查看**SCP**的**man页面**来获取更多内容，请随意留下您的评论及建议。

