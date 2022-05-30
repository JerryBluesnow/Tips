## Sonarqube

- 最新版的应该是需要java11

- [Linux下部署SonarQube+PostgreSQL+sonnar-scanner（记录(吐槽)下让我崩溃的一些坑）](https://blog.csdn.net/qq_42207325/article/details/100998453?spm=1001.2101.3001.6650.10&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-10.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-10.pc_relevant_default&utm_relevant_index=13)

- [linux配置sonarqube遇到的坑](https://blog.csdn.net/seanyang_/article/details/120441828)

- [Linux downloads (Red Hat family) ](https://www.postgresql.org/download/linux/redhat/)

- sonar-scanner -Dsonar.projectKey=sbc-sig -Dsonar.sources=/root/sbc-sig/ssp -Dsonar.host.url=http://localhost:9000 -Dsonar.login=8f78754d81cf63ece126c6be26ec8433e28b6e85

- [elasticsearch运行错误 can not run elasticsearch as root](https://blog.csdn.net/weixin_37391237/article/details/121392642?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-2.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-2.pc_relevant_default&utm_relevant_index=5)

- [Linux环境下使用Docker安装SonarQube的完整教程](https://blog.csdn.net/java_chris/article/details/109736927)

- [sonar安装问题记录](t.zoukankan.com/gcgc-p-10239590.html)

- [Linux系统下sonarqube启动失败的解决办法](https://blog.csdn.net/weixin_43557605/article/details/95252397)

- [Sonarqube](https://www.sonarqube.org/success-download-developer-edition/?accept-terms-check=on)

- [Sonarqube install-plugin](https://docs.sonarqube.org/latest/setup/install-plugin/)

- [使用docker 搭建 SonarQube 代码质量管理平台](https://www.jianshu.com/p/2c60284125ed)

- [SonarQube - 以Docker方式启动SonarQube](https://www.bbsmax.com/A/GBJrKB4B50/#google_vignette)
- [sonar.cxx.clangtidy.reportPaths](https://github.com/SonarOpenCommunity/sonar-cxx/wiki/sonar.cxx.clangtidy.reportPaths)
- [SonarQube - 以Docker方式启动SonarQube](https://www.cnblogs.com/anliven/p/12075636.html)
- [Linux环境下使用Docker安装SonarQube的完整教程](https://blog.csdn.net/java_chris/article/details/109736927)
- [docker 安装sonarqube,maven,sonar-scanner](https://blog.csdn.net/LANNY8588/article/details/108296485)
- [SonarQube+cppcheck实现C++代码扫描](https://blog.csdn.net/qq_15559817/article/details/100736498?spm=1001.2101.3001.6650.7&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-7.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-7.pc_relevant_default&utm_relevant_index=14)
- [Linux下部署SonarQube+PostgreSQL+sonnar-scanner（记录(吐槽)下让我崩溃的一些坑）](https://blog.csdn.net/qq_42207325/article/details/100998453?spm=1001.2101.3001.6650.10&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-10.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-10.pc_relevant_default&utm_relevant_index=13)
- [sonar-cxx/wiki/Install-the-Plugin](https://github.com/SonarOpenCommunity/sonar-cxx/wiki/Install-the-Plugin)
- [Sonarqube扫描C/C++语言的项目代码](https://zhuanlan.zhihu.com/p/356949390)
- [Linux系统下sonarqube启动失败的解决办法](www.javashuo.com/article/p-kkdeddoq-ds.html)
- [sonar-cxx](https://github.com/SonarOpenCommunity/sonar-cxx)

- [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
- [clang-tidy——静态代码分析框架](https://blog.csdn.net/wads23456/article/details/108576845)
- [how-to-integrate-clang-tidy-to-cmake-and-gcc](https://stackoverflow.com/questions/57082124/how-to-integrate-clang-tidy-to-cmake-and-gcc)
- [Visual Studio Code 配置 C/C++ 开发环境最佳实践(VSCode+Clang+Clangd+LLDB)](https://zhuanlan.zhihu.com/p/398790625)
- [微软技术博客：扩展Clang-Tidy](https://blog.csdn.net/wads23456/article/details/107981976)
- [深入研究Clang(十四) clang-tidy的使用](https://zhuanlan.zhihu.com/p/105703209)
- [Visual Studio Code C++扩展更新：clang-tidy](https://baijiahao.baidu.com/s?id=1719275944933158801&wfr=spider&for=pc)
- [最终，我看向了clangd](https://zhuanlan.zhihu.com/p/364518020)
- [在vscode配置C++环境(clang编译器) 傻瓜式配置向导](https://www.cnblogs.com/nikiss/p/14208341.html)
- [VSCode 配置 C/C++：VSCode + Clang + Clangd + LLDB + CMake + Git](https://www.233tw.com/cpp/62050)
- [C++代码自动检测工具clang-format和clang-tidy](https://blog.csdn.net/weixin_43721070/article/details/122638851)

### SonarQube Docker

```cpp
docker pull postgres

docker pull sonarqube

docker run --name db -e POSTGRES_USER=sonar -e POSTGRES_PASSWORD=sonar -p 5432:5432 -d postgres
docker run --name sq --link db -e SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar -p 9000:9000 -d sonarqube

http://localhost:9000/ , 点击 "Log in"

登录账号：admin 密码：admin
```

```
Installing SonarQube from the Docker Image
Follow these steps for your first installation:

Creating the following volumes helps prevent the loss of information when updating to a new version or upgrading to a higher edition:

sonarqube_data – contains data files, such as the embedded H2 database and Elasticsearch indexes
sonarqube_logs – contains SonarQube logs about access, web process, CE process, and Elasticsearch
sonarqube_extensions – will contain any plugins you install and the Oracle JDBC driver if necessary.
Create the volumes with the following commands:

$> docker volume create --name sonarqube_data
$> docker volume create --name sonarqube_logs
$> docker volume create --name sonarqube_extensions

docker run -d --name sonarqube \
    -p 9000:9000 \
    -e SONAR_JDBC_URL=jdbc:postgresql://db:5432/sonar \
    -e SONAR_JDBC_USERNAME=sonar \
    -e SONAR_JDBC_PASSWORD=sonar \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    sonarqube:latest

docker run -d --name sonarqube \
-p 9000:9000 \
-v /opt/sonarqube/data:/opt/sonarqube/data \
-v /opt/sonarqube/extensions:/opt/sonarqube/extensions \
-v /opt/sonarqube/logs:/opt/sonarqube/logs \
-v /opt/sonarqube/temp:/opt/sonarqube/temp \
-e SONAR_JDBC_URL="jdbc:postgresql://db:5432/sonar?useUnicode=true&characterEncoding=utf-8" \
-e SONAR_JDBC_USERNAME=sonar \
-e SONAR_JDBC_PASSWORD=sonar \
-e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true \
sonarqube:latest

3c4d17eaeac9a6b1cf9fa368e83ca1b59d42a61b
```

### 准备工作
```
永久修改Linux系统级别的参数

vim /etc/sysctl.conf
vm.max_map_count = 262144
fs.file-max = 65536

vim /etc/security/limits.conf
*    soft    nofile    65536
*    hard    nofile    65536

then, reboot
```

下面这条命令能起来
```cpp
docker run --name db -e POSTGRES_USER=sonar -e POSTGRES_PASSWORD=sonar -p 5432:5432 -d postgres
docker run --name sq --link db -e SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar -p 9000:9000 -d sonarqube:7.9.1-community
注意： 这里端口映射成别的port就会导致连接失败

then can view the webpage(localhost could be the ifconfig output of the docker):
http://localhost:9000

Download sonar-cxx-plugin-2.0.7.3119.jar from :
https://github.com/SonarOpenCommunity/sonar-cxx/releases

and scp it to sonarqube docker from host:
docker cp sonar-cxx-plugin-2.0.7.3119.jar sq:/opt/sonarqube/extensions/plugins

enable the plugin by restart docker:
docker restart sq

and then go to webpage

install clang from https://github.com/llvm/llvm-project/releases/tag/llvmorg-13.0.1:
or run: apt-get install cppcheck

download sonar-scanner from and unzip it in the host @ /root/src/sonar-scanner-4.7.0.2747-linux:
https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.7.0.2747-linux.zip

unzip sonar-scanner-cli-4.7.0.2747-linux.zip

create soft link:
ls -s /root/src/sonar-scanner-4.7.0.2747-linux/bin/sonar-scanner /usr/bin/sonar-scanner

create the project in the web:

and then put below script in /usr/bin/runscan
-----------------------------------------------------
cd /root/src/sbc-sig/ssp
sonar-scanner \
  -Dsonar.projectKey=sbc-sig \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://172.27.87.105:9000 \
  -Dsonar.login=7f95d9a504420b2353079d3a0f8740ac7b7d7a4e \
  -Dsonar.projectName=sbc-sig \
  -Dsonar.projectVersion=1.0 \
  -Dsonar.language=c++ \
  -Dsonar.exclusions=**/*.java \
  -Dsonar.cxx.cppcheck.path=/usr/bin/cppcheck \
  -Dsonar.cxx.cppcheck.reportPath=/root/src/sbc-sig/cppcheck-reports/cppcheck.xml \
  -Dsonar.sourceEncoding=UTF-8
-----------------------------------------------------
扫描之前注意在web->Administration->CXX->File suffixes页面添加文件后缀，每个文件后缀占用一行

chmod +x /usr/bin/runscan

扫描之前使用cmake+clang-tidy的方式扫描，结果用tee的方式保存在clang-tidy

执行扫描
runscan

docker exec -it -u root sq bash

-----------------------------------------------------
A. 在CentOS上装clang-tidy， 版本太低了，转step B看看
在Ubuntu上安装clang-tidy很同意，一条命令就出来了：sudo apt install clang-tidy-5.0

但是在centos7上安装clang-tidy的资料比较少，没有Ubuntu那么方便。总结了一下如何在centos上安装clang-tidy：

具体步骤执行一下几个命令：

（1）sudo yum install centos-release-scl
（2）sudo yum install  llvm-toolset-7
（3）sudo yum install llvm-toolset-7-clang-analyzer llvm-toolset-7-clang-tools-extra
（4）scl enable llvm-toolset-7 'clang -v'
（5）scl enable llvm-toolset-7 'lldb -v'
（6）scl enable llvm-toolset-7 bash
-----------------------------------------------------
B. 下载llvm-project
https://github.com/llvm/llvm-project/releases/download/llvmorg-11.1.0/llvm-project-11.1.0.src.tar.xz

xz -d llvm-project-11.0.0.tar.xz

tar -xvf llvm-project-11.0.0.tar

cd llvm-project-11.0.0

B.1. 升级gcc版本, 编译llvm，需要gcc至少为 5.1版本，centos默认安装的是 gcc 4.8.5。按照如下步骤升级了 gcc 到 7.3.1。

yum install centos-release-scl

yum install -y devtoolset-7

scl enable devtoolset-7 bash

source /opt/rh/devtoolset-7/enable

echo "source /opt/rh/devtoolset-7/enable" >> ~/.bash_profile

source /opt/rh/devtoolset-7/enable

查看版本，已经是 7.3.1 了:
gcc --version
gcc (GCC) 7.3.1 20180303 (Red Hat 7.3.1-5)

B.2. 编译安装llvm

在 llvm-project-11.0.0 目录中创建一个build目录

mkdir build

cd build

yum install cmake3( if not found, yum install epel-release, then rerun)

ln -s /usr/bin/cmake3 /usr/bin/cmake

然后开始编译:

cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;libcxx;libcxxabi" -G "Unix Makefiles" ../llvm

make

make install

clang 11.0.0，安装成功

```

## Clang Static Analyze 

- https://github.com/guwirth/sonar-cxx/tree/clang-tidy-15
- [scan-build: running the analyzer from the command line](https://clang-analyzer.llvm.org/scan-build.html)

```
yum -y install perl-Digest-MD5

scan-build --use-analyzer `which clang` make
scan-build --use-analyzer=D:\LLVM\build\Debug\bin\clang.exe make
```

- 该命令会显著拖慢编译速度

- [Clang &IOS 静态代码分析工具scan-build](https://blog.csdn.net/chen19870707/article/details/42394555)

- 用clang -cc1 -analyze test.c, 这样的命令只能分析一个单独的文件，如果要分析一个project，文件之间有组织和结构，也就是我们上面说到的codebase，就需要用scan-build来分析了

```cpp
clang -help
clang -cc1 -analyzer-checker-help
clang --analyze -Xclang -analyzer-checker=alpha.core.FixedAddr test.c
clang --analyze -Xanalyzer -analyzer-checker=alpha.unix.SimpleStream dblclose.c
```

```cpp
# must be unique in a given SonarQube instance
sonar.projectKey=sbc-sig

# --- optional properties ---

# defaults to project key
sonar.projectName=sbc-sig-2

# defaults to 'not provided'
sonar.projectVersion=1.0

# Path is relative to the sonar-project.properties file. Defaults to .
sonar.sources=./

sonar.language=c++

sonar.exclusions=**/*.ipch, **/**/*.rc
sonar.cxx.cppcheck.path=/usr/bin/cppcheck
sonar.cxx.cppcheck.reportPath=/root/sbc-sig/cppcheck-reports/cppcheck.xml

# Encoding of the source code. Default is default system encoding
sonar.sourceEncoding=UTF-8

```

## java.lang.RuntimeException: can not run elasticsearch as root
```
elaticsearch默认不能用root用户启动，所以会报java.lang.RuntimeException: can not run elasticsearch as root异常。

解决方法有两类：
1、修改elaticsearch配置，使其可以允许root用户启动（不建议）

#在执行elasticSearch时加上参数-Des.insecure.allow.root=true，完整命令如下

./elasticsearch -Des.insecure.allow.root=true

#或者 用vi打开elasicsearch执行文件，在变量ES_JAVA_OPTS使用前添加以下命令

ES_JAVA_OPTS="-Des.insecure.allow.root=true"

2、为elaticsearch创建用户并赋予相应权限
命令如下 具体介绍参考我的另一篇博客linux创建新用户并将为其赋予权限

adduser es

passwd es

chown -R es:es elasticsearch-6.3.2/

chmod 770 elasticsearch-6.3.2/
```

### clang-tidy-12安装(ubuntu)
```shell
echo "deb http://ftp.cn.debian.org/debian sid main" >> /etc/apt/sources.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 04EE7237B7D453EC
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 648ACFD622F3D138
sudo apt update
sudo apt-get install clang-tidy-12
```

安装成功后，可以看到usr/bin目录下出现了相关的文件。重命名将-12去掉。执行 clang-tidy --version 会显示版本信息，执行 clang-tidy -list-checks 会列出默认生效的check
```shell
ls /usr/bin | grep clang-tidy
mv /usr/bin/clang-tidy-12 /usr/bin/clang-tidy
mv /usr/bin/clang-tidy-diff-12.py /usr/bin/clang-tidy-diff.py
mv /usr/bin/run-clang-tidy-12 /usr/bin/run-clang-tidy
mv /usr/bin/run-clang-tidy-12.py /usr/bin/run-clang-tidy.py
clang-tidy --version
clang-tidy -list-checks
```

## cppcheck

- [SonarOpenCommunity sonar-cxx Code checkers](https://github.com/SonarOpenCommunity/sonar-cxx/wiki/Code-checkers)

- [cppcheck github repo](https://github.com/danmar/cppcheck)
- - how to build cppcheck is in the github above
- - could also install with
- - - apt-get install cppcheck
- - - yum install cppcheck 
- - - apk add cppcheck
- [使用cppcheck检测代码警告、错误](https://latelee.org/using-gnu-linux/check-code-using-cppcheck.html)

- [静态代码检查工具cppcheck初探](https://blog.csdn.net/linux4fun/article/details/44654983?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-4.pc_relevant_antiscanv2&spm=1001.2101.3001.4242.3&utm_relevant_index=6)

- [SonarQube通过sonar-cxx集成Cppcheck插件](https://blog.csdn.net/qq_42207325/article/details/101026788?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-101026788.pc_agg_new_rank&utm_term=cppcheck+识别错误类型&spm=1000.2123.3001.4430)

- [Sonarqube扫描C/C++语言的项目代码](https://zhuanlan.zhihu.com/p/356949390)

- [Sonar+cppCheck+cxxPlugin：实现C++检索](https://blog.csdn.net/cxqiuWind/article/details/122229631?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3.pc_relevant_aa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3.pc_relevant_aa&utm_relevant_index=6)

- [静态代码检查cppcheck之(1)——Cppcheck 架构分析](blog.sina.com.cn/s/blog_7a4cdec80100s661.html)

- [静态代码检查cppcheck之(2)——代码流解析与内存泄露检查](blog.sina.com.cn/s/blog_7a4cdec80100s6yq.html)

```cpp
cppcheck是一个C/C++静态检查工具。它可以帮助我们检测出代码存在(潜在)的问题，比如数组越界、内存申请未释放、文件打开未关闭。注意，cppcheck不是编译器，替代不了gcc。

使用：
cppcheck <工程目录> <选项> 2>&1 | tee <输出文件>

默认：
cppcheck ./ 

输出信息到终端及文件：
cppcheck ./ 2>&1 | tee check.txt

输出到xml文件（好像浏览器不能很好解析得到的xml）
--xml --xml-version=2 . 2>&1 | tee check.xml

选项

所有：
--enable=all

过滤src/xml目录
--suppress='*:src/xml/*'

忽略一些警告：
--suppress=missingIncludeSystem --suppress=variableScope --suppress=ConfigurationNotChecked --suppress=unusedFunction
（其中missingIncludeSystem等可在输出结果中查看）

一些示例命令：

./ --enable=all --suppress='*:src/xml/*' --suppress=variableScope  2>&1 | tee check.txt
安装
在ubuntu下安装cppcheck十分简单，命令如下：

root@latelee:latelee# apt-get install cppcheck 
测试
下面代码片段包含了数组越界、内存申请未释放、文件打开未关闭等错误。由于是演示用，不必太在意代码细节。

#include <stdlib.h>
#include <stdio.h>
#include <string.h>

void init_buffer(void)
{
    char filename[128] = {""}; // 这样初始为0，如果是{"1"}，则只有第1个字节为1，其它为0 --不知其它编译器会怎样
    
    printf("test of buffer\n");

    dump(filename, 128);
    
    char unused_buffer[7*1024*1024] = {0};   // 没有使用的缓冲区，超过栈最大值，有coredump。
    char unused_buffer1[1*1024*1024] = {0};
    
    strcpy(unused_buffer1, "hello");
}

// 数组范围越界
void out_of_array(void)
{
    int foo[2]; // 范围不够
    int aaa = 250;
    
    foo[0] = 1;
    foo[1] = 2;
    // 下面这些是不行的
    foo[2] = 3;
    foo[3] = 4;
    foo[4] = 5;
    foo[5] = 6;
    
    printf("%d %d \n", foo[0], foo[1]);
}

#include <sys/types.h>
#include <dirent.h>
// 打开未关闭
void open_not_close()
{
    // 内存泄漏
    char* p = new char[100];
    strcpy(p, "hello");
    printf("p:%s\n", p);
    

    FILE* fp = NULL;
    fp = fopen("aaa", "a");
    
    if (fp)
    {
        // 注：这里返回时没有关闭文件
        return;
    }
    
    fclose(fp);
    

    DIR *dir = NULL;
    dir = opendir("./");
    
}

int main(void)
{
    int ret = 0;

    foo();

    init_buffer();
    out_of_array();
    open_not_close()
    return 0;
}
注意，在open_not_close函数中的return前并没有关闭fp文件指针。这种错误特别容易发生，幸运的是，cppcheck可以检查出来。下面是是检测结果：

root@latelee~/test# cppcheck gcc_warning.cpp  --enable=all
Checking gcc_warning.cpp...
[gcc_warning.cpp:56] -> [gcc_warning.cpp:50]: (warning) Possible null pointer dereference: fp - otherwise it is redundant to check it against null.
[gcc_warning.cpp:13]: (style) Variable 'unused_buffer' is assigned a value that is never used.
[gcc_warning.cpp:23]: (style) Variable 'aaa' is assigned a value that is never used.
[gcc_warning.cpp:59]: (style) Variable 'dir' is assigned a value that is never used.
[gcc_warning.cpp:65]: (style) Variable 'ret' is assigned a value that is never used.
[gcc_warning.cpp:28]: (error) Array 'foo[2]' accessed at index 2, which is out of bounds.
[gcc_warning.cpp:29]: (error) Array 'foo[2]' accessed at index 3, which is out of bounds.
[gcc_warning.cpp:30]: (error) Array 'foo[2]' accessed at index 4, which is out of bounds.
[gcc_warning.cpp:31]: (error) Array 'foo[2]' accessed at index 5, which is out of bounds.
[gcc_warning.cpp:53]: (error) Memory leak: p
[gcc_warning.cpp:53]: (error) Resource leak: fp
[gcc_warning.cpp:61]: (error) Resource leak: dir
Checking usage of global functions..
(information) Cppcheck cannot find all the include files (use --check-config for details)
从上述信息中可以清晰地看到哪些代码存在问题，原因也一一给出。根据提示去修改代码即可。

附
注1：只要是人写的代码，都有可能存在这种那种问题。代码问题关键因素还是人，但可以借助工具帮助我们减少错误。——至少，如果在某个角落中数组越界了，cppcheck能检测出来。
注2：上述代码无法通过g++编译，因为有很多错误。但cppcheck不是编译器，所以无法检测出来。
注3：cppcheck不是万能的，比如在一个函数申请二级内存，在另一个函数不释放，此情况有内存泄漏，但cppcheck检测不出来。
另外也可以参考笔者的文章：
《gcc较高版本的一些编译警告收集》
《继续收集gcc一些编译警告》
《GCC编译警告选项的学习》
```

```cpp
cppcheck是一个C++开源的静态代码检查工具。基本上编译器不检查的问题他都检查，效果还是不错的。

工作中用到cppcheck作为代码检查，网上现在能搜到的关于cppcheck相关信息也不多，自己也在这里记录一下。其实引入cppcheck确实能为代码提供一些基本风险检测

比如

自动变量检查
数组的边界检查
class类检查
过期的函数，废弃函数调用检查
异常内存使用，释放检查
内存泄漏检查，主要是通过内存引用指针
操作系统资源释放检查，中断，文件描述符等
异常STL 函数使用检查
代码格式错误，以及性能因素检查
最重要的是还能自己定制项目中对应的规则，这也是我们引入cppcheck的原因。

参考了一些网上的资源cppcheck的基本使用：

https://www.cnblogs.com/freedomabcd/p/7771121.html

http://blog.csdn.net/liang19890820/article/details/52778149

https://sourceforge.net/p/cppcheck/wiki/ListOfChecks/

上面得连接都已经介绍一些cppcheck的基本使用。

在这里我就记录一下如何用cppcheck开发定制自己的规则。

原理：Cppcheck先是分析拆解代码，将每个有效字符作为一个token（token是抽象代码中所有字符的类，包含字符的字符串，类型等），提供tokenlist，规则实现者通过匹配需要的字符找到感兴趣的代码，然后通过计算查找bug注意点：

其中Cppcheck会做预处理和简化代码的操作，比如include头文件，展开宏，在每一个token直接用一个空格分隔等。

u匹配token一般用Token::Match这个方法，支持一些特定pattern，在声明处有注释

开发中使用主要的类有：

Tokenizer类： 代码token化， 计划代码

SymbolDatabase类：符号数据库，生成和存储各种符号：scope,function, variable等

Scope类： 各种代码block。最常用的有functionScopes， classAndStructScopes等

Token类： 里面有str(), next(), previous(), tokAt(), link(),Match()等常用函数

Variable类：getTypeString()  --C++相关的代码经常需要

Function类：可以找到实现的scope

Value类： token可以通过getValue()得到可能的值

开发其实就是按套路走：

举个例子：除0 bug

1.从函数scope（函数的list）出发，遍历每一个token（符号）

2.使用Token::Match方法和变量类型来判断是否是除号

3.检查使用除号除以的变量是否判0

4.如果没有的话，如果没有判别式否是0那就是bug

好多规则都可以按这个套路写。只是如果判别是否是除号，以及如何判断被除的变量是否判0，这个有很多不同的形式，这得根据项目中的代码风格、规则来做对应的判断了。判断的不好会有很多误报或者遗漏。所以自定义规则可能更适合长期开发的大项目。
```