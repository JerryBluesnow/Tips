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