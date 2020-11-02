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

##　解决linux下vim乱码的情况：(修改vimrc的内容）

全局的情况下：即所有用户都能用这个配置

文件地址：/etc/vimrc

在文件中添加：

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set hls