# Tips for linux

## 动态链接库
- [linux动态链接库: lib .so .a](./sharedlib/README.md)

## linux下手动安装/升级GCC到较高版本
```
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


[root@localhost lib64]# ln -sf /opt/glibc-2.29/lib/libc-2.29.so /lib64/libc.so.6
[root@localhost lib64]# export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH
[root@localhost lib64]# ls
段错误
[root@localhost lib64]# export L^C
[root@localhost lib64]# export LD_LIBRARY_PATH=/lib64
[root@localhost lib64]# export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH ^C

ln -sf /lib64/libc-2.17.so /lib64/libc.so.6


/lib/x86_64-linux-gnu/ld-2.31.so /bin/ln -s /lib/x86_64-linux-gnu/ld-2.31.so /lib64/ld-linux-x86-64.so.2


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

## 每个版本的软件源都不同 - ubuntu16.04更新软件源

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

## source filename 与 sh filename 及./filename执行脚本的区别#
```
    1. 当shell脚本具有可执行权限时，用sh filename与./filename执行脚本是没有区别得。./filename是因为当前目录没有在PATH中，所有”.”是用来表示当前目录的。
    2. sh filename 重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。
    3. source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面。
```

##　解决linux下vim中文乱码的情况：(修改vimrc的内容）

全局的情况下：即所有用户都能用这个配置

文件地址：/etc/vimrc

在文件中添加：

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set hls

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

### 其他安装命令，但是有问题，可以参考
#### Ubuntu 16.04 TLS，执行以下命令：
```
sudo apt-get install nodejs
sudo apt install nodejs-legacy
sudo apt install npm
```

#### Ubuntu 18.04 TLS，执行以下命令：
```
sudo apt-get install nodejs
sudo apt install libssl1.0-dev nodejs-dev node-gyp npm
更新npm的包镜像源，方便快速下载
sudo npm config set registry https://registry.npm.taobao.org
sudo npm config list
安装n管理器(用于管理nodejs版本)
sudo npm install n -g
```

#### 安装最新的nodejs（stable版本）
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
　　# uname -a # 查看内核/操作系统/CPU信息
　　# head -n 1 /etc/issue # 查看操作系统版本
　　# cat /proc/cpuinfo # 查看CPU信息
　　# hostname # 查看计算机名
　　# lspci -tv # 列出所有PCI设备
　　# lsusb -tv # 列出所有USB设备
　　# lsmod # 列出加载的内核模块
　　# env # 查看环境变量
### 资源
　　# free -m # 查看内存使用量和交换区使用量
　　# df -h # 查看各分区使用情况
　　# du -sh <目录名> # 查看指定目录的大小
　　# grep MemTotal /proc/meminfo # 查看内存总量
　　# grep MemFree /proc/meminfo # 查看空闲内存量
　　# uptime # 查看系统运行时间、用户数、负载
　　# cat /proc/loadavg # 查看系统负载
### 磁盘和分区
　　# mount | column -t # 查看挂接的分区状态
　　# fdisk -l # 查看所有分区
　　# swapon -s # 查看所有交换分区
　　# hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备)
　　# dmesg | grep IDE # 查看启动时IDE设备检测状况
### 网络
　　# ifconfig # 查看所有网络接口的属性
　　# iptables -L # 查看防火墙设置
　　# route -n # 查看路由表
　　# netstat -lntp # 查看所有监听端口
　　# netstat -antp # 查看所有已经建立的连接
　　# netstat -s # 查看网络统计信息
### 进程
　　# ps -ef # 查看所有进程
　　# top # 实时显示进程状态
### 用户
　　# w # 查看活动用户
　　# id <用户名> # 查看指定用户信息
　　# last # 查看用户登录日志
　　# cut -d: -f1 /etc/passwd # 查看系统所有用户
　　# cut -d: -f1 /etc/group # 查看系统所有组
　　# crontab -l # 查看当前用户的计划任务
### 服务
　　# chkconfig --list # 列出所有系统服务
　　# chkconfig --list | grep on # 列出所有启动的系统服务
### 程序
　　# rpm -qa # 查看所有安装的软件包
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
```

### 备注： proc – process information pseudo-filesystem 进程信息伪装文件系统


## centos install

- [centos官网](https://www.centos.org/download/)
- [centos清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/centos/8.3.2011/isos/x86_64/)
- [centos中文网](https://www.centoschina.cn/downloads)

## debian install

- [制作debian U盘启动盘](https://blog.51cto.com/1827495/488844)
- [使用Etcher来创建可启动盘（可引导的USB盘或SD卡）的方法](https://ywnz.com/linuxjc/3010.html)

http://mirrors.163.com/debian-cd/10.7.0/amd64/iso-cd/

http://www.92os.com/post/34

https://cdimage.debian.org/debian-cd/10.7.0/amd64/iso-dvd/

### Debian 制作U盘启动安装盘
+ 1,工具：Universal-USB-Installer（据经验软碟通UltraISOl不是很100%成功）

    官网下载地址： http://www.pendrivelinux.com/  我下载的是 Universal-USB-Installer-1.9.9.0版本

+ 2 .U盘一个(4G/8G)根据系统的大小决定

+ 3.下载Debian镜像文件，目前最新的是debian-10.3.0-i386-netinst .iso 及debian-10.3.0-i386-xfce-CD-1.iso 及debian-10.3.0-i386-DVD-1.iso的DVD均可，从这里选择下载https://www.debian.org/distrib/  只需下载 下载第1个镜像文件 debian-8.1.0-amd64-DVD-1 或 CD即可 。

制作启动安装U盘
    
    在另一台电脑上制作U盘安装盘

        1）将U盘插入电脑，注意提前备份该U盘上的数据，制作安装盘的过程将格式化U盘

        2）直接启动下载的 Universal-USB-Installer-1.9.6.1软件 （可执行文件，无需安装）              

        直接I Agree       

        此处在提供的Debain选项中，只有Live 和Netinst两个选项，并非想安装的amd64。往下拉滚动条，直接拉到最后，选择Try Unlisted Linux ISO

        然后选择下载的ISO镜像文件，选择U盘，点击Create按钮。         

        接下来将解压ISO文件，制作安装U盘，此过程时间较长，大约15分钟左右。

        出现以下提示，表示安装启动盘已经成功制作完成

### debian镜像制作

-[这是为中国定制的Debian镜像](https://github.com/docker4cn/debian)

```
docker pull docker4cn/debian:buster-aliyun

```

- [如何制作debian(mips64el) docker镜像并上传到docker官方仓库](https://www.it610.com/article/1278555881058877440.htm)


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
## 安装多个版本gcc
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
    - 会看到如下的选项，有 3 个候选项可用于替换 gcc (提供 /usr/bin/gcc)。 
    ------------------------------------------------------------
        选择      路径            优先级           状态
      * 0       /usr/bin/gcc-5      50          自动模式
        1       /usr/bin/gcc-5      50          手动模式
        2       /usr/bin/gcc-4.9    40          手动模式
        
        要维持当前值[*]请按回车键，或者键入选择的编号： 

- 删除可选项
    - sudo update-alternatives --remove gcc /usr/bin/gcc-4.9


## 使用Visual Studio 2017开发Linux程序
- - [使用Visual Studio 2017开发Linux程序](https://www.cnblogs.com/dongc/p/6599461.html)
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
