# Tips for linux

## linux Centos7 下手动安装/升级GCC到较高版本
```shell
yum -y install gcc-c++
gcc -vg++ -v
 
wget http://ftp.gnu.org/gnu/gcc/gcc-7.2.0/gcc-7.2.0.tar.gz
tar -zxvf gcc-7.2.0.tar.gz
cd gcc-7.2.0

./contrib/download_prerequisites

./configure --prefix=/user/local/ --disable-multilib

3、make

4、make install
```

### 参考文章

- [Linux安装GCC 9.2.0](https://blog.csdn.net/lwc5411117/article/details/101200065)

- [CENTOS7编译安装GCC9.2.0及踩坑经历](https://www.cnblogs.com/liranowen/p/11639929.html)

- [第二部份：gcc升级到gcc-9.2.0](https://blog.csdn.net/u012480990/article/details/104277771)

- [CentOS7编译安装Gcc9.2.0，解决mysql等软件编译问题](https://www.51lowkey.com/note-14.html)

- [一次segfault错误的排查过程](https://blog.csdn.net/zhaohaijie600/article/details/45246569)

- [centos7 安装 GNU Make 4.1](https://blog.csdn.net/weixin_41565755/article/details/88564947)

- [在centos上安装最新的glibc](https://blog.csdn.net/zhangpeterx/article/details/96116219)

```shell
LD_PRELOAD=/lib64/libc-2.17.so; ln -sf /lib64/libc-2.17.so /lib64/libcso.6
export LD_PRELOAD=/opt/glibc-2.19/lib/libc-2.19.so;ln -sf /opt/glibc-219/lib/libc-2.19.so /lib64/libc.so.6
```

- [CentOS 7.6 编译安装最新版本glibc2.30 实录](https://blog.csdn.net/RyanFang/article/details/100984938?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

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

## 升级libc

- [/lib64/libstdc++.so.6: version `CXXABI_1.3.8’ not found](https://blog.csdn.net/EI__Nino/article/details/100086157)

```shell
ln -s /root/local/bin/gcc /usr/bin/gcc && ln -s /root/local/bin/g++ /usr/bin/g++ && ln -s /root/local/bin/c++ /usr/bin/c++ && ln -s /root/local/bin/gcc /usr/bin/cc && cp /root/local/lib64/libstdc++.so.6.0.27 /usr/lib64 && cd /usr/lib64 && ln -s libstdc++.so.6.0.27 libstdc++.so.6 

export LD_PRELOAD=/lib64/libc-2.17.so;ln -sf /lib64/libc-2.17.so /lib64/libc.so.6
```

- [linux下glibc库升级](https://blog.csdn.net/noobplayer/article/details/52790059)

```shell
export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH
运行ls， 出现段错误，表示有些软链接没创建好
export LD_LIBRARY_PATH=/lib64
export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH
ln -sf /opt/glibc-2.29/lib/libc-2.29.so /lib64/libc.so.6
ln -sf /lib64/libc-2.17.so /lib64/libc.so.6
/lib/x86_64-linux-gnu/ld-2.31.so /bin/ln -s /lib/x86_64-linux-gnu/ld-2.31.so /lib64/ld-linux-x86-64.so.2
```

+ 误删了/lib64/ld-linux-x86-64.so.2，ls, cd等等命令失效，命令都失效， 如何恢复：

```shell
/lib64/ld-2.17.so /bin/ln -s /lib64/ld-2.17.so /lib64/ld-linux-x86-64.so.2
LD_PRELOAD=/lib64/libc-2.17.so ln -s /lib64/libc-2.17.so /lib64/libc.so.6
```

## nvocation of python3.6 via ld-2.17.so segfaults

- [nvocation of python3.6 via ld-2.17.so segfaults](https://github.com/ContinuumIO/anaconda-issues/issues/8773)

The following actions will resolve these dependencies:

```shell
Keep the following packages at their current version:
 
1. libdbd-mysql-perl [Not Installed]

2. libdbi-perl [Not Installed]

3. libterm-readkey-perl [Not Installed]

4. mariadb-client [Not Installed]

5. mariadb-client-5.5 [Not Installed]

6. mariadb-server [Not Installed]

7. mariadb-server-5.5 [Not Installed]

Leave the following dependencies unresolved:

8. mariadb-client-5.5 recommends libdbd-mysql-perl (>= 1.2202)
```

## Ubuntu Server源码编译安装MariaDB[centos推荐使用repo安装]
```shell
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

## ubuntu更新软件源

- [ubuntu16.04更新软件源](https://blog.csdn.net/lxlong89940101/article/details/89488461)
- [ubuntu18.04更新软件源](https://www.zhangshilong.cn/work/312863.html)

## source filename 与 sh filename 及./filename执行脚本的区别

- 当shell脚本具有可执行权限时，用sh filename与./filename执行脚本是没有区别得。./filename是因为当前目录没有在PATH中，所有”.”是用来表示当前目录的。
- sh filename 重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。
- source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面。

## 解决linux下vim中文乱码的情况：(修改vimrc的内容）

全局的情况下：即所有用户都能用这个配置

文件地址：/etc/vimrc

在文件中添加：

```shell
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
set hls
```

## git log显示中文

```shell
git config --global i18n.commitencoding utf-8  # --注释：该命令表示提交命令的时候使用utf-8编码集提交

git config --global i18n.logoutputencoding utf-8 # --注释：该命令表示日志输出时使用utf-8编码集显示

export LESSCHARSET=utf-8  # --注释：设置LESS字符集为utf-8
```

## 安装npm （这个命令最管用）
curl -L https://npmjs.com/install.sh | sh

Ubuntu 16.04 TLS，执行以下命令：

```shell
sudo apt-get install nodejs
sudo apt install nodejs-legacy
sudo apt install npm
```

Ubuntu 18.04 TLS，执行以下命令：

```shell
sudo apt-get install nodejs
sudo apt install libssl1.0-dev nodejs-dev node-gyp npm
sudo npm config set registry https://registry.npm.taobao.org  # 更新npm的包镜像源，方便快速下载
sudo npm config list
sudo npm install n -g   # 安装n管理器(用于管理nodejs版本)
```

## 安装最新的nodejs（stable版本）
```shell
sudo n stable
sudo node -v
sudo npm -v
```

## vue环境配置
+ 方法1 (最好用的就是方法1)：
+ + 如果安装nodejs 8.x版本

```shell
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

+ + 如果安装nodejs 9.x版本

```shell
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
```

+ 方法2：

```shell
wget https://nodejs.org/dist/v8.1.0/node-v8.1.0-linux-x64.tar.xz
tar -xvf node-v8.1.0-linux-x64.tar.xz
cd node-v8.1.0-linux-x64/bin
./node -v
sudo ln /home/ubuntu/node-v8.1.0-linux-x64/bin/node /usr/local/bin/node
sudo ln /home/ubuntu/node-v8.1.0-linux-x64/bin/npm /usr/local/bin/npm
```

+ 方法3：

```shell
sudo apt-get install nodejs-legacy nodejs
sudo npm config set registry https://registry.npm.taobao.org # 把npm的包源设置为淘宝的镜像
sudo npm install n -g 										 # 来安装n这个工具，n这个工具是用于更新node版本的工具
sudo n stable												 # 安装最新稳定版的nodejs
node --version
npm  --version
```

## npm删除项目所有依赖和清缓存

清缓存的办法，一个是 npm cache verify, 还有一个方法npm cache clean --force

删除项目所有依赖  npm uninstall *

## centos install

- [centos官网](https://www.centos.org/download/)

- [centos清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/centos/8.3.2011/isos/x86_64/)

- [centos中文网](https://www.centoschina.cn/downloads)

```
yum -y install ncurses-devel
```

## debian install

- [制作debian U盘启动盘](https://blog.51cto.com/1827495/488844)

- [使用Etcher来创建可启动盘（可引导的USB盘或SD卡）的方法](https://ywnz.com/linuxjc/3010.html)

- http://mirrors.163.com/debian-cd/10.7.0/amd64/iso-cd/

- http://www.92os.com/post/34

- https://cdimage.debian.org/debian-cd/10.7.0/amd64/iso-dvd/

### Debian 制作U盘启动安装盘

+  工具：Universal-USB-Installer（据经验软碟通UltraISOl不是很100%成功）

    官网下载地址： http://www.pendrivelinux.com/  我下载的是 Universal-USB-Installer-1.9.9.0版本

+ U盘一个(4G/8G)根据系统的大小决定

+ 下载Debian镜像文件，目前最新的是debian-10.3.0-i386-netinst .iso 及debian-10.3.0-i386-xfce-CD-1.iso 及debian-10.3.0-i386-DVD-1.iso的DVD均可，从这里选择下载https://www.debian.org/distrib/  只需下载 下载第1个镜像文件 debian-8.1.0-amd64-DVD-1 或 CD即可 。

+ + 1）将U盘插入电脑，注意提前备份该U盘上的数据，制作安装盘的过程将格式化U盘
    
+ + 2）直接启动下载的 Universal-USB-Installer-1.9.6.1软件 （可执行文件，无需安装）              
    
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

```shell
docker pull docker4cn/debian:buster-aliyun
/etc/apt/sources.list
https://mirror.tuna.tsinghua.edu.cn/help/debian/
apt-get update
apt-get install vim -y
vi /etc/apt/sources.list
apt-get update
apt-get install certbot -y
certbot certonly --standalone -d sam-tech.com
```

## https 证书

- [Certbot-免费的HTTPS证书](https://zhuanlan.zhihu.com/p/80909555)

## 使用Visual Studio 2017开发Linux程序
- [使用Visual Studio 2017开发Linux程序](https://www.cnblogs.com/dongc/p/6599461.html)

## gdb调试报错warning: Error disabling address space randomization: Operation not permitted

```
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

- [gperftools](https://github.com/gperftools)/**[gperftools](https://github.com/gperftools/gperftools)**

- [善用工具-程序性能分析Gperftools初探(libwind+pprof+Kcachegrind)](https://blog.csdn.net/aganlengzi/article/details/62893533?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_aggregation-15-62893533.pc_agg_rank_aggregation&utm_term=kcachegrind+分析&spm=1000.2123.3001.4430)


## 函數邏輯關係圖

```shell
在对源代码走读的过程中，我们可以借助一些工具来帮助理解源代码的结构和函数调用关系，比如生成函数调用关系图。

cflow工具通过分析一组C源文件，绘制出程序的逻辑流程图和交叉引用列表，在此分析结果的基础上，通过其他工具生成可视化的图像文件，帮助我们理解源代码。

cflow安装

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

```shell
yum install graphviz
```

进入tree2dotx脚本所在目录，按照下面的步骤操作：

```shell
cflow -T -m main /root/freeswitch-1.8.7/src/switch.c > fs.txt
cat fs.txt | ./tree2dotx > fs.dot
dot -Tbmp fs.dot -o fs.bmp
```

for ALG/IMS

```shell
cd ssp/ds/ims/util  # I suppose ALG code is under this path

cflow -T -m main IMSpolicy_qos_state.cpp > IMSpolicy_qos_state.txt
cat IMSpolicy_qos_state.txt | ./tree2dotx > IMSpolicy_qos_state.dot
dot -Tjpg IMSpolicy_qos_state.dot -o IMSpolicy_qos_state.jpg
```

```shell
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
service networking restart
```

## 上下文切换的定义

- [Context Switch Definition](http://www.linfo.org/context_switch.html)
- [进程上下文切换 – 残酷的性能杀手（上）](https://www.cnblogs.com/emperor_zark/archive/2012/12/11/context_switch_1.html)
- [进程/线程上下文切换会用掉你多少CPU？](https://zhuanlan.zhihu.com/p/79772089)
- [这么多监控组件，总有一款适合你](https://cloud.tencent.com/developer/article/1511761)

## Add a User to a Group (or Second Group) on Linux

+  [Add a User to a Group (or Second Group) on Linux](http://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)

```
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
```

## 构建rpm包

- [如何构建 RPM 包](https://zhuanlan.zhihu.com/p/47868584)
- [Building RPMs with Mock](https://ithiriel.com/content/2011/10/13/building-rpms-mock)
- [在 Ubuntu 下直接将二进制文件制作成 rpm 包](https://blog.konghy.cn/2015/11/13/rpmbuild/)
- [ubuntu制作简陋的deb/rpm包](https://blog.csdn.net/evglow/article/details/103351348)
- [Centos RPM安装包制作](https://blog.csdn.net/q1009020096/article/details/110953465)
- [一步步制作RPM包](https://blog.51cto.com/laoguang/1103628)

## WSL ubuntu安装docker，不使用docker desktop, 可行， 使用下面的镜像。（debian失败）
1. ubuntu 18.04 网易镜像源- 可用
```
cp /etc/apt/sources.list /etc/apt/sources.list.back
vi /etc/apt/sources.list
```
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
deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
```
apt-get update

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
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
一顿操作已经安装结束,使用 sudo service docker start 开启docker守护进程
使用 docker version 查看版本
```
3. 以root启动wsl ubuntu
```
ubuntu1804.exe config --default-user root
```
4. 重启WSL ubuntu

```shell
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
yum install yum-utils -y
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

## 查找需要的RPM包和只下载不安装

```
rpm -Uvh --force --nodeps *rpm

yum install --downloadonly --downloaddir=./  libaio-devel
```

## [curl 支持 http2](https://www.cnblogs.com/brookin/p/10713166.html)

- [yum升级curl支持http2测试](https://www.cnblogs.com/lazyfang/p/8040493.html）- 这里是另一种安装方法
- [libcurl 使用 http2](ftxtool.org/2016/04/19/135/) - 这里有开发参数设置
- [HTTP协议 学习：2-基于libcurl的开发](https://www.cnblogs.com/schips/p/12597023.html) - 开发参数解释，及开发用例
- [C++使用libcurl做HttpClient](https://blog.csdn.net/huyiyang2010/article/details/7664201)
- [C++ 用libcurl库进行http通讯网络编程](https://www.cnblogs.com/moodlxs/archive/2012/10/15/2724318.html) - 例子很多
- [HTTP/2 with curl](https://curl.se/docs/http2.html) - 官方文档
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


方式一 you can install a develop version，这个方式不会导致curl编译找不到openssl
```
yum -y install openssl-devel
```

方式二 源码安装, 会导致curl编译找不到openssl
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

#### 验证

```
curl --version
curl 7.80.0 (x86_64-pc-linux-gnu) libcurl/7.80.0 OpenSSL/1.0.2k-fips zlib/1.2.7 nghttp2/1.47.0-DEV
Release-Date: 2021-11-10
Protocols: dict file ftp ftps gopher gophers http https imap imaps mqtt pop3 pop3s rtsp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS HSTS HTTP2 HTTPS-proxy IPv6 Largefile libz NTLM NTLM_WB SSL UnixSockets
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

#### 附注

```
 -I, --head 	(HTTP/FTP/FILE) Fetch the HTTP-header only!
```

## yum 安装 / yum源 / CentOS7.6镜像源

- [CentOS 7.6 使用yum安装软件及yum源的配置](https://blog.csdn.net/shengjie87/article/details/107043400)

- [Ubuntu下安装yum和配置yum源](https://blog.csdn.net/qq_38690917/article/details/115261819)

  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
  下载对应版本repo文件, 放入/etc/yum.repos.d/(操作前请做好相应备份)
  CentOS7   wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

```
下载对应版本repo文件, 放入/etc/yum.repos.d/(操作前请做好相应备份)

mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

mv CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo
```
#### 安装 yum 源

```
rpm -ivh http://mirror.city-fan.org/ftp/contrib/yum-repo/city-fan.org-release-2-1.rhel7.noarch.rpm
```

#### 新建 yum 源(功能与 安装 yum 源 相同)

vim /etc/yum.repos.d/city-fan.repo

```shell
[cityfan]  
name=cityfan 
baseurl=http://www.city-fan.org/ftp/contrib/yum-repo/rhel7/x86_64/
enabled=1  
gpgcheck=0
```

## 十个 SCP 传输命令例子

Linux系统管理员应该很熟悉**CLI**环境，因为通常在Linux服务器中是不安装**GUI**的。**SSH**可能是Linux系统管理员通过远程方式安全管理服务器的最流行协议。在**SSH**命令中内置了一种叫**SCP**的命令，用来在服务器之间安全传输文件。

![十个 SCP 传输命令例子_Linux](http://www.wenwenyun.com/uploads/allimg/c141227/1419Dc23Z260-123234.png)

以下命令可以解读为：用“**username account**”“**拷贝 source file name**”到“**destination host**”上的“**destination folder**”里。

### SCP命令的基本语法

```
scp source_file_name username@destination_host:destination_folder1.
```

**SCP**命令有很多可以使用的参数，这里指的是每次都会用到的参数。

### 用-v参数来提供SCP进程的详细信息

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

### 从源文件获取修改时间、访问时间和模式

“**-p**”参数会帮到把预计的时间和连接速度会显示在屏幕上。

```
pungki@mint ~/Documents $ scp -p Label.pdf mrarianto@202.x.x.x:.1.
```

### 部分输出

```
mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 126.6KB/s 00:291.2.
```

### 用-C参数来让文件传输更快

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

### 选择其它加密算法来加密文件

**SCP**默认是用“**AES-128**”加密算法来加密传输的。如果你想要改用其它加密算法来加密传输，你可以用“**-c**”参数。我们来瞧瞧。

```
pungki@mint ~/Documents $ scp -c 3des Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 282.5KB/s 00:131.2.3.4.
```

上述命令是告诉**SCP**用**3des algorithm**来加密文件。要注意这个参数是“**-c**”（小写）而不是“**-C**“（大写）。

### 限制带宽使用

还有一个很有用的参数是“**-l**”参数，它能限制使用带宽。如果你为了拷贝很多文件而去执行了一份自动化脚本又不希望带宽被**SCP**进程耗尽，那这个参数会非常管用。

```
pungki@mint ~/Documents $ scp -l 400 Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 50.3KB/s 01:131.2.3.4.
```

在“**-l**”参数后面的这个**400**值意思是我们给**SCP**进程限制了带宽为**50 KB/秒**。有一点要记住，带宽是以**千比特/秒** (**kbps**)表示的，而**8 比特**等于**1 字节**。

因为**SCP**是用**千字节/秒** (**KB/s**)计算的，所以如果你想要限制**SCP**的最大带宽只有**50 KB/s**，你就需要设置成**50 x 8 = 400**。

### 指定端口

通常**SCP**是把**22**作为默认端口。但是为了安全起见SSH 监听端口改成其它端口。比如说，我们想用**2249**端口，这种情况下就要指定端口。命令如下所示。

```
pungki@mint ~/Documents $ scp -P 2249 Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 262.3KB/s 00:141.2.3.4.
```

确认一下写的是大写字母“**P**”而不是“**p**“，因为“**p**”已经被用来保留源文件的修改时间和模式（LCTT 译注：和 ssh 命令不同了）。

### 递归拷贝文件和文件夹

有时我们需要拷贝文件夹及其内部的所有**文件**/**子文件夹**，我们如果能用一条命令解决问题那就更好了。**SCP**用“**-r**”参数就能做到。

```
pungki@mint ~/Documents $ scp -r documents mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
Label.pdf 100% 3672KB 282.5KB/s 00:13
scp.txt 100% 10KB 9.8KB/s 00:001.2.3.4.5.
```

拷贝完成后，你会在目标服务器中找到一个名为“**documents**”的文件夹，其中就是所拷贝的所有文件。“**documents**”是系统自动创建的文件夹。

### 禁用进度条和警告/诊断信息

如果你不想从SCP中看到进度条和警告/诊断信息，你可以用“**-q**”参数来静默它们，举例如下。

```
pungki@mint ~/Documents $ scp -q Label.pdf mrarianto@202.x.x.x:.

mrarianto@202.x.x.x's password:
pungki@mint ~/Documents $1.2.3.4.
```

正如你所看到的，在你输入密码之后，没有任何关于SCP进度的消息反馈。进度完成后，你也看不到任何提示。

### 用SCP通过代理来拷贝文件

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

### 选择不同的ssh_config文件

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

![image-20211017224849305](C:\Users\JerryZ\AppData\Roaming\Typora\typora-user-images\image-20211017224849305.png)

## Ubuntu Server 中resolv.conf重启时被覆盖的问题

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

- [dns配置文件 /etc/resolv.conf中search设置详解](https://blog.csdn.net/x356982611/article/details/105868570)

- [zhangmingda/etc/resolv.conf文件中的search项作用；如何保持resolv.conf文件内容不被修改](https://www.cnblogs.com/zhangmingda/p/13663541.html)

- [CentOS的DNS服务器配置文件/etc/resolv.conf重置问题](https://blog.csdn.net/hengrjgc/article/details/42774323?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-3.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-3.no_search_link


```

/etc/resolv.conf中设置dns之后每次重启Ubuntu Server时该文件会被覆盖，针对这种情况找了一些个解决方法

防止/etc/resolv.conf被覆盖的方法

方法一

1.需要创建一个文件/etc/resolvconf/resolv.conf.d/tail

sudo vi /etc/resolvconf/resolv.conf.d/tail

2.在该文件中写入自己需要的dns服务器，格式与/etc/resolv.conf相同

nameserver 8.8.8.8  

3.重启下resolvconf程序

sudo /etc/init.d/resolvconf restart 

再去看看/etc/resolv.conf文件,可以看到自己添加的dns服务器已经加到该文件中

方法二

在/etc/network/interfaces中

复制代码
###interfaces中#######
auto eth0    
iface eth0 inet static    
address 192.168.3.250    
netmask 255.255.255.0                  #子网掩码    
gateway 192.168.3.1                      #网关    
dns-nameservers 8.8.8.8 8.8.4.4    #设置dns服务器  

避免resolv.conf设置被覆盖(示例代码)
technologylife 2020-11-19


简介  这篇文章主要介绍了避免resolv.conf设置被覆盖(示例代码)以及相关的经验技巧，文章约974字，浏览量318，点赞数3，值得参考！

resolv.conf文件简介
/etc/resolv文件是系统指定dns服务器地址的配置文件。下面简称resolv.conf

当系统进行域名解析时，会先读取resolv.conf文件中设置的DNS地址，若DNS地址设置错误或没有resolv.conf文件都会导致域名解析失败。
通过ifcfg-eth0文件设置dns地址，将生成resolv.conf文件(若存在则覆盖)，若想不覆盖/etc/resolv.conf设置，在ifcfg-eth0中添加PEERDNS=no(系统默认设置为yes)，
若ifcfg-eth0设置为DHCP模式，同样需要设置PEERDNS=no，否则DHCP获取到的DNS地址会覆盖resolv.conf文件
保护DNS设置
在ifcfg配置文件中添加

PEERDNS=no
这样可防止网络服务使用从 DHCP 服务器接收的 DNS 服务器更新 /etc/resolv.conf。

在ifcfg配置文件中设置DNS

要配置一个接口以便使用具体 DNS 服务器，请如上所述设定 PEERDNS=no，并在 ifcfg 文件中添加以下行：

DNS1=ip-address
DNS2=ip-address
其中 ip-address 是 DNS 服务器的地址。这样就会让网络服务使用指定的 DNS 服务器更新 /etc/resolv.conf
```

## resolv.conf中search参数作用

```
reslov.conf中的search主要是用来补全hostname的，有时候域名太长，可以做一个短域名做主机名字，但是DNS解析需要的是FQDN，而在resolv.conf中设置search能进行补全。

# vim /etc/hosts<br>//添加下面这行
8.8.8.8 www
ping www能通，返回就是8.8.8.8，ping会首先解析hosts。

# vim /etc/resolv.conf<br>//添加下面行
search oliver.ren
nameserver 223.5.5.5
这时候

# nslookup www

Server:        223.5.5.5
Address:       223.5.5.5#53

Non-authoritative answer:
Name:    www.oliver.ren
Address: 8.8.8.8

看到没，search的作用就是补全要访问的短域名
正确的域名解析顺序是:
1. 查找/etc/hosts
2. 根据nameserver查找域名
3. 如果在nameserver查找不到域名就进行search补全，重新走1~2步

解决方法
/etc/sysconfig/network-scripts/ifcfg-eth0或者自己网卡配置文件增加一行：『PEERDNS=no』，然后重新启动网络即可。修改后，重启主机等操作便不会使/etc/resolv.conf被dhclient修改
```

## 什么是Peer DNS

先认识一下这三个配置文件：

```undefined
/etc/hosts ：这个是最早的 hostname 对应 IP 的存档；
/etc/resolv.conf ：当需要解析域名时，读取该文件获得DNS 服务器 IP；
/etc/nsswitch.conf：这个档案『决定』先使用 /etc/hosts 还是 /etc/resolv.conf 的设定！
```

当电脑要访问一个域名时，要将域名翻译成IP地址。
这个过程通常会先访问/etc/hosts，看本地是否有对应的hostname -- IP记录。
如果没有就去查询DNS服务器，通过/etc/resolv.conf 得到dns服务器地址。

![img](https://upload-images.jianshu.io/upload_images/3720094-5ff7da43182750f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/514/format/webp)

流程如图

当在eth接口启用DHCP后，本地resolv.conf文件将被修改，resolv.conf文件中的DNS地址将被改为从DHCP获取到的地址。这种从DHCP获得的DNS即是Peer DNS。

启用DHCP后即便修改/etc/resolv.conf，不久又恢复成原样。如何解决这个问题？此时，你得要在 /etc/sysconfig/network-scripts/ifcfg-eth0 等相关档案内，增加一行：『PEERDNS=no』，然后重新启动网络即可。

## Linux文件保护禁止修改、删除、移动文件等,使用chattr +i保护

**chattr命令的用法**：chattr [ -RV ] [ -v [version](https://so.csdn.net/so/search?q=version) ] [ mode ] files…
最关键的是在[mode]部分，[mode]部分是由+-=和[ASacDdIijsTtu]这些字符组合的，这部分是用来控制文件的
属性。
**+** ：在原有参数设定基础上，追加参数。
**-** ：在原有参数设定基础上，移除参数。
**=** ：更新为指定参数设定。
**A**：文件或目录的 atime (access time)不可被修改(modified), 可以有效预防例如手提电脑磁盘I/O错误的发生。
**S**：硬盘I/O同步选项，功能类似sync。
**a**：即append，设定该参数后，只能向文件中添加数据，而不能删除，多用于服务器日志文 件安全，只有root才能设定这个属性。
**c**：即compresse，设定文件是否经压缩后再存储。读取时需要经过自动解压操作。
**d**：即no dump，设定文件不能成为dump程序的备份目标。
**i**：设定文件不能被删除、改名、设定链接关系，同时不能写入或新增内容。i参数对于文件 系统的安全设置有很大帮助。
**j**：即journal，设定此参数使得当通过 mount参数：data=ordered 或者 data=writeback 挂 载的文件系统，文件在写入时会先被记录(在journal中)。如果filesystem被设定参数为 data=journal，则该参数自动失效。
**s**：保密性地删除文件或目录，即硬盘空间被全部收回。
**u**：与s相反，当设定为u时，数据内容其实还存在磁盘中，可以用于undeletion.
各参数选项中常用到的是a和i。a选项强制只可添加不可删除，多用于日志系统的安全设定。而i是更为严格的安全设定，只有superuser (root) 或具有CAP_LINUX_IMMUTABLE处理能力（标识）的进程能够施加该选项。

```
chattr +i /etc/passwd
```

```
lsattr /etc/group
—-i——–e- /etc/group
```

**如果需要修改密码,执行 chattr -i 消除权限**

```
chattr -i /etc/passwd
```

```
lsattr /etc/group
————-e- /etc/group
```

## hyper-v虚拟机centos7网络配置,解决虚拟机不能上网问题

[hyper-v虚拟机centos7网络配置,解决虚拟机不能上网问题](https://blog.csdn.net/weixin_34503526/article/details/104671956?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~OPENSEARCH~default-16.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~OPENSEARCH~default-16.control40)


## Install MariaDB Server 10 on CentOS 7 and RHEL 7 by using yum

```
https://sharadchhetri.com/install-mariadb-server-10-on-centos-7-and-rhel-7-by-using-yum/
repo采用下面的页面
https://mariadb.org/download/?t=repo-config&d=CentOS+7+%28x86_64%29
```

```
vi /etc/yum.repos.d/MariaDB.repo
```

```
# MariaDB 10.4 CentOS repository list - created 2021-11-23 15:46 UTC
# https://mariadb.org/download/
[mariadb]
name = MariaDB
baseurl = https://tw1.mirror.blendbyte.net/mariadb/yum/10.4/centos7-amd64
gpgkey=https://tw1.mirror.blendbyte.net/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

```
rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

```
yum install MariaDB-server MariaDB-client MariaDB-devel MariaDB-shared

mysql_secure_installation

/usr/bin/mysqladmin -u root password 'new-password'
```

## json-c / libevent

https://github.com/json-c/json-c

https://blog.csdn.net/xingyu97/article/details/97108108

https://blog.csdn.net/woniu211111/article/details/81839939

1.在http://libevent.org/下载libevent-2.1.8-stable.tar.gz

2.tar -zxvf libevent-2.1.8-stable.tar.gz

3.cd libevent-2.1.8-stable

4./configure --prefix=/usr --libdir=/usr/lib64

5.make

6.make install

[libjson-c编译及安装](turbock79.cn/?p=1923)

## CentOS 7.x iptables/firewalld防火墙
- [CentOS7:Unit iptables.service not loaded](https://blog.csdn.net/lbb88888888/article/details/102292349)

## Linux服务器，服务管理--systemctl命令详解，设置开机自启动

+ [Linux服务器，服务管理--systemctl命令详解，设置开机自启动](https://www.cnblogs.com/zdz8207/p/linux-systemctl.html)

syetemclt就是service和chkconfig这两个命令的整合，在CentOS 7就开始被使用了。

摘要: systemctl 是系统服务管理器命令，它实际上将 service 和 chkconfig 这两个命令组合到一起。

| 任务                 | 旧指令                        | 新指令                                                       |
| -------------------- | ----------------------------- | ------------------------------------------------------------ |
| 使某服务自动启动     | chkconfig --level 3 httpd on  | systemctl enable httpd.service                               |
| 使某服务不自动启动   | chkconfig --level 3 httpd off | systemctl disable httpd.service                              |
| 检查服务状态         | service httpd status          | systemctl status httpd.service （服务详细信息） systemctl is-active httpd.service （仅显示是否 Active) |
| 显示所有已启动的服务 | chkconfig --list              | systemctl list-units --type=service                          |
| 启动某服务           | service httpd start           | systemctl start httpd.service                                |
| 停止某服务           | service httpd stop            | systemctl stop httpd.service                                 |
| 重启某服务           | service httpd restart         | systemctl restart httpd.service                              |

下面以nfs服务为例：

1.启动nfs服务

```
systemctl start nfs-server.service
```

**2.设置开机自启动**

```
systemctl enable nfs-server.service
```

3.停止开机自启动

```
systemctl disable nfs-server.service
```

4.查看服务当前状态

```
systemctl status nfs-server.service
```

5.重新启动某服务

```
systemctl restart nfs-server.service
```

6.查看所有已启动的服务

```
systemctl list -units --type=service
```

开启防火墙22端口

```
iptables -I INPUT -p tcp --dport 22 -j ACCEPT
```

如果仍然有问题，就可能是SELinux导致的

关闭SElinux：

**修改/etc/selinux/config文件中的SELINUX=”” 为 disabled，然后重启**

彻底关闭防火墙：

```
sudo systemctl status  firewalld.service
sudo systemctl stop firewalld.service          
sudo systemctl disable firewalld.service
```

### 常用的systemctl命令

```以sshd服务为例，列出常用systemctl命令：
启动sshd服务：systemctl start ssh.service
停止sshd服务：systemctl stop ssh.service
查看sshd服务状态：systemctl status ssh.service
重启sshd服务：systemctl restart ssh.service
设置开机自启动：systemctl enable ssh.service
禁止开机自启动：systemctl disable ssh.service
查看所有已经启动的服务：systemctl list-units --type=service
重新加载配置文件：systemctl daemon-reload```
```

### systemctl启动服务编写

Centos7的服务systemctl脚本存放在：/usr/lib/systemd/目录下，有系统（system）和用户（user）之分，一般需要开机不登录就能运行的程序，就存放在/usr/lib/systemd/system/目录下

每一个服务以.service结尾，一般会分为3部分：[Unit]、[Service]和[Install]，以sshd为实例如下：

```[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
Type=notify
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

[Unit]部分主要是对这个服务的说明，内容包括Description和After，Description 用于描述服务，After用于描述服务类别;

[Service]部分是服务的关键，是服务的一些具体运行参数的设置;

[Install]部分是服务安装的相关设置，可设置为多用户的;

```
[Unit]
Description=HTTP/2 proxy
Documentation=man:nghttpx
After=network.target

[Service]
Type=notify
ExecStart=/usr/bin/nghttpx --conf=/etc/nghttpx/nghttpx.conf
ExecReload=/bin/kill --signal HUP $MAINPID
KillSignal=SIGQUIT
PrivateTmp=yes
ProtectHome=yes
ProtectSystem=full
Restart=always

[Install]
WantedBy=multi-user.target
```

### 配置文件详解

![image-20211201221403629](C:\Users\JerryZ\AppData\Roaming\Typora\typora-user-images\image-20211201221403629.png)

![image-20211201221348237](C:\Users\JerryZ\AppData\Roaming\Typora\typora-user-images\image-20211201221348237.png)
![image-20211201221331989](C:\Users\JerryZ\AppData\Roaming\Typora\typora-user-images\image-20211201221331989.png)

重新加载配置文件

systemctl daemon-reload

- [Linux开机时顺序启动项目并保证项目不挂机(systemctl+supervisor)](https://blog.csdn.net/qq_37822090/article/details/106690226)

```
  在/etc/resolv.conf中增加dns地址，重启网卡服务后，文件内容被清空。
  解决办法：
  关闭NetworkManager服务
  /etc/init.d/NetworkManager stop
  修改/etc/resolv.conf
  vim /etc/resolv.conf
  修改或新增dns地址：
  nameserver xxx.xxx.xxx.xxx
  保存退出
  重启网卡
  /etc/init.d/network restart
  避免重启服务器后配置被清空
  chkconfig NetworkManager off

  写保护你的 DNS 服务器
  通过修改/etc/resolv.conf来修改你的 DNS 服务器。一旦你作出了修改，写保护这个文件
  chattr +i /etc/resolv.conf
  这个+i的选项就是为文件/etc/resolv.conf添加了写保护，因此任何人都不可以修改他，甚至是 root 用户也不行！
  如果你需要取消写保护，使用下面的命令：
  chattr -i /etc/resolv.conf
```

## curl头部设置

- [libcurl长连接高并发高性能](https://zhuanlan.zhihu.com/p/254801697)
- [使用 nghttpx 搭建 HTTP/2 代理](https://wzyboy.im/post/1052.html)
- [常用libcurl异步使用方法](https://www.jianshu.com/p/d7609df995d2)
- [libcurl长连接高并发多线程](https://www.cnblogs.com/bclshuai/p/13693960.html)
- [libcurl库使用方法，好长，好详细](https://www.cnblogs.com/heluan/p/10177475.html)
- [libcurl库安装（Linux）](https://blog.csdn.net/simonyucsdy/article/details/82835268)
- [CURLOPT_HTTP_VERSION explained](https://curl.se/libcurl/c/CURLOPT_HTTP_VERSION.html)
- [HTTP协议 学习：2-基于libcurl的开发](https://www.cnblogs.com/schips/p/12597023.html) - 有代码

```
CURLOPT_HTTPHEADER

构造HTTP头部字段，或代替现有字段（从而移除已有字段）。该选项传递一个指针，这个指针指向HTTP请求中传给server的头部字段链表（linked list）。用curl_slist_append(3)来创建头部字段list，curl_slist_free(3)用来清除list 。例如：增加User-Authertication这个头部字段。

使用libcurl设置HTTP头部字段的方法：

#include

Struct curl_slist *slist = NULL;

Slist = curl_slist_append(slist, “Connection: Keep-Alive”); //http长连接

Curl_easy_setopt(handle, CURLOPT_HTTPHEADER, slist);

Curl_easy_perform(handle);

Curl_slist_free_all(slist);

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
./configure --prefix=/usr/local/
make
sudo make install

add below to .bashrc and source it before compile curl
export LD_LIBRARY_PATH=/usr/local/lib64:/usr/local/lib

```

#### 安装 openssl

```shell
yum install zlib-devel.x86_64

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

- [CentOS使用rpm离线安装mariadb](https://www.cnblogs.com/cobcmw/p/11420311.html)

![image-20211017224849305](C:\Users\JerryZ\AppData\Roaming\Typora\typora-user-images\image-20211017224849305.png)
```
- [hyper-v虚拟机centos7网络配置,解决虚拟机不能上网问题](https://blog.csdn.net/weixin_34503526/article/details/104671956?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~OPENSEARCH~default-16.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~OPENSEARCH~default-16.control40)


## 自启动
[Unit]
Description=HTTP/2 proxy
Documentation=man:nghttpx
After=network.target

[Service]
Type=notify
ExecStart=/usr/bin/nghttpx --conf=/etc/nghttpx/nghttpx.conf
ExecReload=/bin/kill --signal HUP $MAINPID
KillSignal=SIGQUIT
PrivateTmp=yes
ProtectHome=yes
ProtectSystem=full
Restart=always

[Install]
WantedBy=multi-user.target

Ubuntu Server 中resolv.conf重启时被覆盖的问题
/etc/resolv.conf中设置dns之后每次重启Ubuntu Server时该文件会被覆盖，针对这种情况找了一些个解决方法

防止/etc/resolv.conf被覆盖的方法

方法一

1.需要创建一个文件/etc/resolvconf/resolv.conf.d/tail

sudo vi /etc/resolvconf/resolv.conf.d/tail

2.在该文件中写入自己需要的dns服务器，格式与/etc/resolv.conf相同

nameserver 8.8.8.8  

3.重启下resolvconf程序

sudo /etc/init.d/resolvconf restart 

再去看看/etc/resolv.conf文件,可以看到自己添加的dns服务器已经加到该文件中

方法二

在/etc/network/interfaces中

复制代码
###interfaces中#######
auto eth0    
iface eth0 inet static    
address 192.168.3.250    
netmask 255.255.255.0                  #子网掩码    
gateway 192.168.3.1                      #网关    
dns-nameservers 8.8.8.8 8.8.4.4    #设置dns服务器  

避免resolv.conf设置被覆盖(示例代码)
technologylife 2020-11-19


简介  这篇文章主要介绍了避免resolv.conf设置被覆盖(示例代码)以及相关的经验技巧，文章约974字，浏览量318，点赞数3，值得参考！

resolv.conf文件简介
/etc/resolv文件是系统指定dns服务器地址的配置文件。下面简称resolv.conf

当系统进行域名解析时，会先读取resolv.conf文件中设置的DNS地址，若DNS地址设置错误或没有resolv.conf文件都会导致域名解析失败。
通过ifcfg-eth0文件设置dns地址，将生成resolv.conf文件(若存在则覆盖)，若想不覆盖/etc/resolv.conf设置，在ifcfg-eth0中添加PEERDNS=no(系统默认设置为yes)，
若ifcfg-eth0设置为DHCP模式，同样需要设置PEERDNS=no，否则DHCP获取到的DNS地址会覆盖resolv.conf文件
保护DNS设置
在ifcfg配置文件中添加

PEERDNS=no
这样可防止网络服务使用从 DHCP 服务器接收的 DNS 服务器更新 /etc/resolv.conf。

在ifcfg配置文件中设置DNS

要配置一个接口以便使用具体 DNS 服务器，请如上所述设定 PEERDNS=no，并在 ifcfg 文件中添加以下行：

DNS1=ip-address
DNS2=ip-address
其中 ip-address 是 DNS 服务器的地址。这样就会让网络服务使用指定的 DNS 服务器更新 /etc/resolv.conf
```

## Install MariaDB Server 10 on CentOS 7 and RHEL 7 by using yum
```
https://sharadchhetri.com/install-mariadb-server-10-on-centos-7-and-rhel-7-by-using-yum/
repo采用下面的页面
https://mariadb.org/download/?t=repo-config&d=CentOS+7+%28x86_64%29
```

```
vi /etc/yum.repos.d/MariaDB.repo
```

```
# MariaDB 10.4 CentOS repository list - created 2021-11-23 15:46 UTC
# https://mariadb.org/download/
[mariadb]
name = MariaDB
baseurl = https://tw1.mirror.blendbyte.net/mariadb/yum/10.4/centos7-amd64
gpgkey=https://tw1.mirror.blendbyte.net/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

```
rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

```
yum install MariaDB-server MariaDB-client MariaDB-devel MariaDB-shared

mysql_secure_installation

/usr/bin/mysqladmin -u root password 'new-password'
```
https://github.com/json-c/json-c

https://blog.csdn.net/xingyu97/article/details/97108108

https://blog.csdn.net/woniu211111/article/details/81839939

1.在http://libevent.org/下载libevent-2.1.8-stable.tar.gz

2.tar -zxvf libevent-2.1.8-stable.tar.gz

3.cd libevent-2.1.8-stable

4./configure --prefix=/usr --libdir=/usr/lib64

5.make

6.make install

- [libjson-c编译及安装](turbock79.cn/?p=1923)

1. 数据库 scscf location, pcscf location
2. kamailio数据库失效出现过一次
3. n5_aar_delete有一个发不到pcf
4. hss脚本配置数据库
5. 集群freeswitch, 会议模式，对讲机模式，电台集群通话。
6. sip收不到
7. 卫星: k8s + CentOS
8. 卫星: uler system


## CentOS 7.x iptables/firewalld防火墙
- [CentOS7:Unit iptables.service not loaded](https://blog.csdn.net/lbb88888888/article/details/102292349)

## resolv.conf被覆盖的问题
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```
- [dns配置文件 /etc/resolv.conf中search设置详解](https://blog.csdn.net/x356982611/article/details/105868570)
- [zhangmingda/etc/resolv.conf文件中的search项作用；如何保持resolv.conf文件内容不被修改](https://www.cnblogs.com/zhangmingda/p/13663541.html)
- [CentOS的DNS服务器配置文件/etc/resolv.conf重置问题](https://blog.csdn.net/hengrjgc/article/details/42774323?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-3.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-3.no_search_link

- resolv.conf中search参数作用 转载
```
reslov.conf中的search主要是用来补全hostname的，有时候域名太长，可以做一个短域名做主机名字，但是DNS解析需要的是FQDN，而在resolv.conf中设置search能进行补全。

# vim /etc/hosts<br>//添加下面这行
8.8.8.8 www
ping www能通，返回就是8.8.8.8，ping会首先解析hosts。

# vim /etc/resolv.conf<br>//添加下面行
search oliver.ren
nameserver 223.5.5.5
这时候

　　# nslookup www

Server:        223.5.5.5
Address:       223.5.5.5#53

Non-authoritative answer:
Name:    www.oliver.ren
Address: 8.8.8.8

看到没，search的作用就是补全要访问的短域名
正确的域名解析顺序是:
1. 查找/etc/hosts
2. 根据nameserver查找域名
3. 如果在nameserver查找不到域名就进行search补全，重新走1~2步
```

解决方法
/etc/sysconfig/network-scripts/ifcfg-eth0或者自己网卡配置文件增加一行：『PEERDNS=no』，然后重新启动网络即可。修改后，重启主机等操作便不会使/etc/resolv.conf被dhclient修改

## 什么是Peer DNS

先认识一下这三个配置文件：

```undefined
/etc/hosts ：这个是最早的 hostname 对应 IP 的存档；
/etc/resolv.conf ：当需要解析域名时，读取该文件获得DNS 服务器 IP；
/etc/nsswitch.conf：这个档案『决定』先使用 /etc/hosts 还是 /etc/resolv.conf 的设定！
```

当电脑要访问一个域名时，要将域名翻译成IP地址。
这个过程通常会先访问/etc/hosts，看本地是否有对应的hostname -- IP记录。
如果没有就去查询DNS服务器，通过/etc/resolv.conf 得到dns服务器地址。

![img](https://upload-images.jianshu.io/upload_images/3720094-5ff7da43182750f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/514/format/webp)

流程如图

当在eth接口启用DHCP后，本地resolv.conf文件将被修改，resolv.conf文件中的DNS地址将被改为从DHCP获取到的地址。这种从DHCP获得的DNS即是Peer DNS。

启用DHCP后即便修改/etc/resolv.conf，不久又恢复成原样。如何解决这个问题？此时，你得要在 /etc/sysconfig/network-scripts/ifcfg-eth0 等相关档案内，增加一行：『PEERDNS=no』，然后重新启动网络即可。

## 于keep-alive的几点疑惑
```
一、http的keep-alive与tcp的keep-alive
http keep-alive：
在一次tcp连接中可以连续发送多次数据，即可以保持一段时间的tcp连接，在这个保持的通道上有多个request、多个response。而不用每发一次数据就要重新进行三次握手连接，发完一次数据就要立即进行四次挥手释放连接。 这样可以提高性能和吞吐率。

tcp keep-alive：
为了检测tcp的连接状况。经过设定的时间之后，服务器会发出检测包去确认tcp连接是否还在。如果出现了问题就关闭连接。

小结：http的keep-alive和tcp的keep-alive是完全不同的东西。

二、request header中的http keep-alive与tcp连接
在http1.1版本之后都会默认设置connection为keep-alive，如果想要关闭这条设置，需要在头信息中更改connection为close。

但是在实际使用中，http头部有了keep-alive这个值并不代表一定会使用长连接，客户端和服务器端都可以无视这个值，每一条TCP通道，只有一次GET，GET完之后，立即有TCP关闭的四次握手，这样写代码更简单。这时候虽然http头有connection: keep-alive，但不能说是长连接。所以是否用了长连接，还是得用抓包工具分析tcp流。

小结：正常情况下客户端浏览器、web服务端都有实现这个标准，因为它们的文件又小又多，保持长连接减少重新开TCP连接的开销很有价值。但是最终到底有没有实现keep-alive还是得看tcp流的情况。

三、http keep-alive的tcp复用与websocket长连接
http keep-alive：
http keep-alive只是一种为了达到复用tcp连接的“协商”行为，双方并没有建立正真的连接会话，服务端也可以不认可，也可以随时（在任何一次请求完成后）关闭掉。它是指在一次 TCP 连接中完成多个 http请求，但是对每个请求仍然要单独发 header，所以除了真正的数据部分外，服务器和客户端还要大量交换http header，信息交换效率很低，这样建立的“长连接”都是伪长连接。

websocket：
websocket不同，它本身就规定了是正真的、双工的长连接，两边都必须要维持住连接的状态。

小结：http协议决定了浏览器端总是主动发起方，http的服务端总是被动的接受、响应请求。http提供的长连接服务器可以不接受。而websocket协议，在连接之后，客户端、服务端是完全平等的。websocket是真正的长连接。
```

#### 安装 openssl

```shell
yum install zlib-devel.x86_64

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
```

```
1、下载与解压

cd ~wget https://www.openssl.org/source/openssl-1.1.0f.tar.gz

tar-xzf openssl-1.1.0f.tar.gz

2、编译与安装

如果没有安装gcc可能会报错，可以直接使用yum安装一下gcc

yum install gcc

cd openssl-1.1.0f

./config

make

make install

3、尝试运行应该会出现下面的这个错误：

/usr/local/bin/openssl version/usr/local/bin/openssl: error while loading shared libraries: libcrypto.so.1.1: cannot open shared object file: No such file or directory

4、下面为相关的解决办法：

创建链接至libssl

ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/ln-s /usr/local/lib64/libcrypto.so.1.1 /usr/lib64/

5、创建链接至新的openssl

ln -s /usr/local/bin/openssl /usr/bin/openssl_latest

6、检查openssl_latest的版本号是否是新的版本

openssl_latest version

OpenSSL1.1.0f 25 May 2017

7、重命名旧的openssl文件名，并且将新的文件名改为openssl

cd /usr/bin/mv openssl openssl_old

mv openssl_latest openssl
```

### [使用VSCode插件CodeRunner一键编译运行Java](https://blog.csdn.net/wuyujin1997/article/details/89323627)

### 在命令的每行输出前添加时间戳

------

POSIX外壳
请记住，由于许多shell在内部将它们的字符串存储为cstring，因此，如果输入包含空字符（\0），则可能导致该行过早结束。

```shell
command | while IFS= read -r line; do printf '[%s] %s\n' "$(date '+%Y-%m-%d %H:%M:%S')" "$line"; done
```

GNU AWK

```shell
command | gawk '{ print strftime("[%Y-%m-%d %H:%M:%S]"), $0 }'
```
佩尔

```shell
command | perl -pe 'use POSIX strftime; print strftime "[%Y-%m-%d %H:%M:%S] ", localtime'
```

蟒蛇

```shell
command | python -c 'import sys,time;sys.stdout.write("".join(( " ".join((time.strftime("[%Y-%m-%d %H:%M:%S]", time.localtime()), line)) for line in sys.stdin )))'
```

红宝石

```shell
command | ruby -pe 'print Time.now.strftime("[%Y-%m-%d %H:%M:%S] ")'
```

## /var空间释放
第一步执行：
rm -rf ./file.log
如果空间没释放，再执行第二步：
lsof |grep deleted|awk '{print $2}'|xargs kill -9

## open file limit

```shell
Linux打开最大文件数限制

于 2021-07-20 11:14:28 发布

关于对 /etc/profile、/etc/security/limits.conf、/etc/sysctl.conf 三个配置文件的理解。

1、/etc/profile

2、/etc/security/limits.conf（用户进程级别的设置）

3、/etc/sysctl.conf （系统级别的设置）

1、/etc/profile
大部分用户环境变量配置都设置在这个配置文件

登入系统读取步骤：

当登入系统时候获得一个shell进程时，其读取环境设定档有三步 :

1.首先读入的是全局环境变量设定档/etc/profile，然后根据其内容读取额外的设定的文档，如 /etc/profile.d和/etc/inputrc

2.然后根据不同使用者帐号，去其家目录读取~/.bash_profile，如果这读取不了就读取~/.bash_login，这个也读取不了才会读取~/.profile，这三个文档设定基本上是一样的，读取有优先关系

3.然后在根据用户帐号读取~/.bashrc

/etc/*和~/.*区别：

/etc/profile，/etc/bashrc 是系统全局环境变量设定

~/.profile，~/.bashrc是用户家目录下的私有环境变量设定

~/.profile与~/.bashrc的区别:

都具有个性化定制功能

~/.profile可以设定本用户专有的路径，环境变量等，它只在登入的时候执行一次

~/.bashrc也是某用户专有设定文档，可以设定路径，命令别名，每次shell script的执行都会使用它一次

2、/etc/security/limits.conf（用户进程级别的设置）
利用ulimit命令可以对资源的可用性进行控制。

-H选项和-S选项分别表示对给定资源的硬限制（hard limit）和软限制（soft limit）进行设置。

硬限制（hard limit）一旦被设置以后就不能被非root用户修改，软限制（soft limit）可以增长达到硬限制（hard limit）。

如果既没有指定-H选项也没有指定-S选项，那么硬限制（hard limit）和软限制（soft limit）都会被设置。

limit的值可以是一个数值，也可以是一些特定的值，比如：hard，soft，unlimited，分别代表当前硬件限制、当前软件限制、不限制。

如果limit参数被省略，除非指定-H选项，否则资源当前的软限制（soft limit）将会被打印出来。

大小可设置为：

[root@mha-slave02 ~]# grep "MemTotal" /proc/meminfo  | awk -F' ' '{print $2}'
4045372
[root@mha-slave02 ~]# expr 4045372 \* 64 / 1024
252835
[root@mha-slave02 ~]# ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 15719
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 252835
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 252835
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
open files （-n） 1024 是linux操作系统对一个进程打开的文件句柄数量的限制（也包含打开的套接字数量）

这里只是对用户级别的限制，其实还有个是对系统的总限制，查看系统总限制：

# cat /proc/sys/fs/file-max

man proc，可得到file-max的描述：

/proc/sys/fs/file-max
              This  file defines a system-wide limit on the number of open files for all processes.  (See
              also setrlimit(2),  which  can  be  used  by  a  process  to  set  the  per-process  limit,
              RLIMIT_NOFILE,  on  the  number  of  files it may open.)  If you get lots of error messages
              about running out of file handles, try increasing this value:

即file-max是设置系统所有进程一共可以打开的文件数量 。同时一些程序可以通过setrlimit调用，设置每个进程的限制。如果得到大量使用完文件句柄的错误信息，是应该增加这个值。

也就是说，这项参数是系统级别的。

2、修改方法

 临时生效：

# ulimit -SHn 10000
其实ulimit 命令身是分软限制和硬限制，加-H就是硬限制，加-S就是软限制。默认显示的是软限制，如果运行ulimit 命令修改时没有加上-H或-S，就是两个参数一起改变。

软限制和硬限制的区别？

硬限制就是实际的限制，而软限制是警告限制，它只会给出警告。

永久生效

要想ulimits 的数值永久生效，必须修改配置文件/etc/security/limits.conf
在该配置文件中添加

* soft nofile 252835

* hard nofile 252835  

echo "* soft nofile 252835"  >> /etc/security/limits.conf

echo "* hard nofile 252835"  >> /etc/security/limits.conf

* 表示所用的用户

修改系统总限制

其实上的修改都是对一个进程打开的文件句柄数量的限制，我们还需要设置系统的总限制才可以。

假如，我们设置进程打开的文件句柄数是1024 ，但是系统总线制才500，所以所有进程最多能打开文件句柄数量500。从这里我们可以看出只设置进程的打开文件句柄的数量是不行的。所以需要修改系统的总限制才可以。

echo  6553560 > /proc/sys/fs/file-max

上面是临时生效方法，重启机器后会失效；

永久生效方法：

修改 /etc/sysctl.conf, 加入

fs.file-max = 6553560 重启生效

3、/etc/sysctl.conf （系统级别的设置）
# cat /etc/sysctl.conf 
kernel.sysrq = 0               ------------------是否启用kernel.sysrq（在大多数服务已无法 
                                                 响应的情况下，还能通过按键组合来完成一系列 
                                                 预先定义的系统操作,1启用，0禁用;
kernel.core_uses_pid = 1       ------------------可以控制core文件的文件名中是否添加pid作为扩 
                                                 展(文件内容为1，表示添加pid作为扩展名，生成 
                                                 的core文件格式为core.xxxx；为0则表示生成的 
                                                 core文件同一命名为core);
kernel.msgmnb  = 65536         ------------------限制一个队列的最大长度;
kernel.msgmax  = 65536         ------------------限制一条消息的最大长度;
kernel.shmmax  = 68719476736   ------------------最大共享内存段大小;
kernel.shmall  = 4294967296    ------------------可以使用的共享内存的总量;
kernel.shmmni  = 4096          ------------------整个系统共享内存段的最大数目;
kernel.sem     = 250 32000 100 128 --------------每个信号对象集的最大信号对象数；
                                                 系统范围内最大信号对象数；
                                                 每个信号对象支持的最大操数； 
                                                 系统范围内最大信号对象集数;
kernel.msgmni  = 256           ------------------决定了系统中同时运行的最大的消息队列的个数;
fs.file-max    = 65536         ------------------系统级打开最大文件句柄的数量;
 
net.ipv4.ip_forward     = 0    ------------------(0代表禁止进行IP转发；1代表可以进行IP转发);
net.ipv4.conf.default.rp_filter = 1  ------------控制系统是否开启对数据包源地址的校验;
net.ipv4.conf.default.accept_source_route = 0  --禁用icmp源路由选项;
net.ipv4.tcp_syncookies = 1    ------------------表示开启SYN Cookies。当出现SYN等待队列溢出 
                                                 时，启用cookies来处理，可防范少量SYN攻击， 
                                                 默认为0，表示关闭;
net.ipv4.conf.all.forwarding = 0  ---------------0代表不转发源路由帧，若做NAT建议开启;
net.ipv4.ip_local_port_range = 9000 65000  ------系统中的程序会选择这个范围内的端口来连接到目 
                                                 的端口（目的端口当然是用户指定的;
net.ipv4.tcp_fin_timeout = 30  ------------------表示如果套接字由本端要求关闭，这个参数决定了 
                                                 它保持在FIN-WAIT-2状态的时间,默认是60，降 
                                                 低这个值以提高系统性能;
net.ipv4.tcp_max_syn_backlog = 8192  ------------定义backlog队列容纳的最大半连接数，如果配置 
                                                 高可以设置的更高;
net.core.rmem_default   =   262144   ------------套接字接收缓冲区大小的缺省值;
net.core.rmem_max       =   4194304  ------------套接字接收缓冲区大小的最大值;
net.core.wmem_default   =   262144   ------------套接字发送缓冲区大小的缺省值;
net.core.wmem_max       =   262144   ------------套接字发送缓冲区大小的最大值;
修改后配置文件使之生效的办法：
sysctl -p
参考：

1、https://www.cnblogs.com/cjsblog/p/9367043.html

2、https://www.cnblogs.com/pangguoping/p/5791432.html

4、https://blog.csdn.net/u011495642/article/details/84109007
————————————————
版权声明：本文为CSDN博主「WFkwYu」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_35663625/article/details/118930299
```

## kamailio日志设置

```shell
开启日志，并将日志输出到/var/log/kamailio.log文件

修改配置文件vi /usr/local/etc/kamailio/kamailio.cfg，添加

log_facility=LOG_LOCAL0

log_prefix="{$mt $hdr(CSeq) $ci} "

这样它会把日志交给“LOG_LOCAL0”  ，接下来编辑CentOS的rsyslog配置

vi /etc/rsyslog.conf

插入下面这行

local0.* -/var/log/kamailio.log（可以注释修改人员和日期）

然后重启rsyslog 服务

systemctl stop   rsyslog.service    关闭日志服务

systemctl start   rsyslog.service     开启日志服务

也可直接systemctl restart rsyslog.service 重启日志服务；

重启 kamailio

kamctl restart

之后，通过 tail -f /var/log/kamailio.log 就可以查看日志最新输出了

-f 该参数用于监视File文件增长。退出，按下CTRL+C。
```

### Separate Log File for Kamailio

```shell
In order to create a separate log file for kamailio, you would need to edit two files.

First , set the log_facility directive :

vi kamailio.cfg

log_facility=LOG_LOCAL0

Edit the syslog configuration file depending upon your daemon

vi /etc/rsyslog.conf

or

vi /etc/syslog.conf

local0.* -/var/log/kamailio.log

service kamailio restart

service syslog/rsyslog restart

Rotating Kamailio Logs :

vi /etc/logrotate.d/kamailio.log

/var/log/kamailio.log { 
	missingok 
	size=50M 
	create 0644 root root 
	postrotate 
	/bin/kill -HUP cat /var/run/syslogd.pid 2> /dev/null 2> /dev/null || true 
	endscript 
}

logrotate  进行日志分隔的规则 具体见logrotate

cat /etc/logrotate.d/kamailio 

/var/log/kamailio.log {
        noolddir
        size 100M
        rotate 20
        sharedscripts
        postrotate
                /bin/kill -HUP `cat /var/run/syslogd.pid 2>/dev/null` 2>/dev/null || true
        endscript
} 

/usr/sbin/logrotate -f /etc/logrotate.d/kamailio    #使配置生效
```

### 参考文档：

+ https://www.kamailio.org/dokuwiki/doku.php/tutorials:debug-syslog-messages

+ http://thyrusgorges.com/post/kamailio-log-message-to-custom-log-file/

+ http://www.kamailio.org/events/2016-KamailioWorld/Day0/W04-Daniel-Constantin.Mierla-Debugging-Kamailio-Config.pdf

+ https://wiki.4psa.com/display/KB/How+to+debug+Asterisk+and+Kamailio


## CentOS镜像源
```
1. 备份原来的yum源

cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

2.设置aliyun的yum源

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

3.添加EPEL源

EPEL（http://fedoraproject.org/wiki/EPEL）是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上 EPEL后，可以像在 Fedora 上一样，可以通过 yum install package-name，安装更多软件。

wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo

4.清理缓存并生成新的缓存

yum clean all
yum makecache
```

## Hyper-v下Centos使用LVM实现动态扩容磁盘
- [Hyper-v下Centos使用LVM实现动态扩容磁盘](https://blog.csdn.net/u012151597/article/details/88041186)
- [hyper虚拟机下对centos进行动态扩容](https://www.cnblogs.com/theluther/p/4055593.html)
最后一步使用：
xfs_growfs /dev/mapper/centos-root


## 解决“Cmake error :generator: Ninja“问题

原因在于版本不统一，之前编译过CMakeLists.txt后，产生了缓存文件CMakeCache.txt，

解决方案：删除CMakeCache.txt文件，解决。

rm -f `find -name CMakeCache.txt`

## CentOS 安装 debuginfo-install

安装debuginfo相关的包步骤如下：

1、 修改文件/etc/yum.repos.d/CentOS-Debuginfo.repo中的enabled参数，将其值修改为1

2、 使用命令：

yum install nss-softokn-debuginfo –nogpgcheck
yum install yum-utils

## perf

```
perf top -p 98255 -v -g
perf top -p 98255 -v -g --no-children

perf top --call-graph graph

perf annotate显示perf.data函数代码；
perf annotate --stdio -l --symbol=IBCFproc_cac_query_rsp -p 25478

perf record -a -g ./fork：会在当前目录生成perf.data文件。
perf record -a -g -p 25478
perf report --call-graph none结果如下,后面结合perf timechart分析.
```

- [用Perf寻找程序中的性能热点](https://zhuanlan.zhihu.com/p/134721612)

- [在Linux下做性能分析3：perf](https://zhuanlan.zhihu.com/p/22194920)

- [红帽性能调优——perf使用](https://www.jianshu.com/p/675a850365eb)

- [Profiling CPU usage in real time with perf top](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/profiling-cpu-usage-in-real-time-with-top_monitoring-and-managing-system-status-and-performance)

- [手把手教你系统级性能分析工具perf的介绍与使用（超详细）](https://zhuanlan.zhihu.com/p/471379451)

- [perf-让CPU消耗无处遁形](https://blog.51cto.com/u_5646435/3172871)

- [火焰图](git clone https://github.com/brendangregg/FlameGraph)

- [如何用Perf解开服务器消耗的困境](https://rdc.hundsun.com/portal/article/637.html)

- [perf Tutorial](https://perf.wiki.kernel.org/index.php/Tutorial)