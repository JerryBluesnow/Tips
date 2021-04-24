# Docker Tips

## Docker setup tips

- [Docker 教程](https://www.runoob.com/docker/docker-tutorial.html)

- [Docker Forum](https://forums.docker.com/)

- [Install Docker ToolBox on Windows](https://docs.docker.com/toolbox/toolbox_install_windows/)

- [Get Docker Toolbox for Windows](https://download.docker.com/win/stable/DockerToolbox.exe)

- [jerry.hub.docker.com](https://hub.docker.com/r/jerry4docker/ubuntu/)

- [完整记录在 windows7 下使用 docker 的过程](https://www.jianshu.com/p/d809971b1fc1)

- [docker~在centos容器中安装新程序](https://www.cnblogs.com/lori/p/6703174.html)

- [docker toolbox relase](https://github.com/docker/toolbox/releases)

## 如何修改Windows上Docker的镜像源

- [如何修改Windows上Docker的镜像源](https://blog.csdn.net/wangdandong/article/details/68958210)

    docker-machine create --engine-registry-mirror=https://xxxxxxxx.mirror.aliyuncs.com -d virtualbox default

    EXTRA_ARGS='
    --registry-mirror=https://hub.docker.com/r
    --label provider=virtualbox

    '
    CACERT=/var/lib/boot2docker/ca.pem
    DOCKER_HOST='-H tcp://0.0.0.0:2376'
    DOCKER_STORAGE=aufs
    DOCKER_TLS=auto
    SERVERKEY=/var/lib/boot2docker/server-key.pem
    SERVERCERT=/var/lib/boot2docker/server.pem

    export "NO_PROXY=192.168.99.100"

## Win7 修改为阿里云镜像加速
### 1. 在阿里云查看给个人分配的镜像源地址: 
- [俊杰的阿里云](https://cr.console.aliyun.com/cn-hangzhou/mirrors)

### 2. docker-machine ssh default
```
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://r19oqqqv.mirror.aliyuncs.com"]
    }
    EOF 
```

## Docker: How to enable/disable HTTP Proxy in Toolbox

### step 1. 
    docker-machine ssh default

### step 2.
    sudo vi /var/lib/boot2docker/profile

### step 3.
    # replace with your office's proxy environment
    export "HTTP_PROXY=http://PROXY:PORT"
    export "HTTPS_PROXY=http://PROXY:PORT"
    # you can add more no_proxy with your environment.
### Please make sure disable NO_PROXY line in the /var/lib/boot2docker/profile
    if not, the docker could not connect to docker hub as docker ip is 192.168.*.*
    #export "NO_PROXY=192.168.99.*,*.local,169.254/16,*.example.com,192.168.59.*"

### step 4.
    sudo /etc/init.d/docker restart

### step 5. 
    exit

## After modify profile, for any docker command, there is:
    error during connect: Get https://192.168.99.100:2376/v1.37/info: dial tcp 192.168.99.100:2376: connectex: No connection could be made because the target machine actively refused it.
### in Win10, Please follow
    https://blog.csdn.net/pangdongh/article/details/80203103

### in Win7, Please follow
    docker-machine create -d virtualbox --engine-env HTTP_PROXY=http://xxx.xxx.xxx.xxx:8000 --engine-env HTTPS_PROXY=http://xxx.xxx.xxx.xxx:8000 --engine-env NO_PROXY=192.168.99.100 default

## sharefolder
[配置共享文件夹](https://blog.csdn.net/weixin_33714884/article/details/86345700)

## Error response from daemon: error parsing HTTP 404 response body
![Error response from daemon: error parsing HTTP 404 response body](pic/pic_1.png)

## Cannot perform an interactive login from a non TTY device
![Cannot perform an interactive login from a non TTY device](pic/pic_1.png)

[Docker Hub 仓库使用，及搭建 Docker Registry](https://segmentfault.com/a/1190000012662268)

[docker 学习笔记21：docker连接网络的设置](https://www.cnblogs.com/51kata/p/5268951.html)
    可以修改 /etc/default/docker 配置文件

    If you need Docker to use an HTTP proxy, it can also be specified here.
    export http_proxy="http://127.0.0.1:3128/"
    export http_proxy="http://代理地址:端口"
    docker run -it --rm ubuntu bash

### the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'

    switch from git bash to powershell

# ISSUE
    PS C:\Users\jzhan107> docker run jerry4docker/ubuntu4cplus
    Unable to find image 'jerry4docker/ubuntu4cplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: manifest for jerry4docker/ubuntu4cplus:latest not found.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.
    PS C:\Users\jzhan107> docker run -it --rm ubuntu4cplusplus /bin/bash
    Unable to find image 'ubuntu4cplusplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: pull access denied for ubuntu4cplusplus, repository does not exist or may require 'docker login'.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.
    PS C:\Users\jzhan107> docker login
    Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
    Username (jerry4docker): jerry4docker
    Password:
    Login Succeeded
    PS C:\Users\jzhan107> docker run -it --rm ubuntu4cplusplus /bin/bash
    Unable to find image 'ubuntu4cplusplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: pull access denied for ubuntu4cplusplus, repository does not exist or may require 'docker login'.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.

    >> docker run jerry4docker/ubuntu4cplus
    Unable to find image 'jerry4docker/ubuntu4cplus:latest' locally
    docker: Error response from daemon: Get https://registry-1.docker.io/v2/: proxyconnect tcp: EOF.
    See 'docker run --help'.
    it is network issue, please try to fix it with proxy configured

# Command 
    docker-machine ip
    docker info
    docker run -i -t ubuntu /bin/bash
    apt-get update && apt-get install vim
    docker ps -a
    docker stop XX
    docker rm `docker ps -a -q`

## the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'
    jerry@workingpc MINGW64 ~
    $ docker attach e9a932d44224
    the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'

    jerry@workingpc MINGW64 ~
    $ winpty docker attach e9a932d44224
    root@e9a932d44224:/#

##  Unable to locate package vim-gtk
    root@e9a932d44224:/# apt-get install vim-gtk
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    E: Unable to locate package vim-gtk
### first, 
    apt-get update
### then,
    apt-get install vim-gtk

### apt-get update连接不上
    进入 vi /etc/apt/sources.list 
    将网址修改为如下之一即可。

    163源

    deb http://mirrors.163.com/debian/ jessie main non-free contrib 
    deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib 
    deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib 
    deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib 
    deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib

    中科大源

    deb http://mirrors.ustc.edu.cn/debian jessie main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie main contrib non-free 
    deb http://mirrors.ustc.edu.cn/debian jessie-proposed-updates main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie-proposed-updates main contrib non-free 
    deb http://mirrors.ustc.edu.cn/debian jessie-updates main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie-updates main contrib non-free

### Single User Persistent Proxy Settings
#### Open your bash profile file into a text editor.
    
    vi ~/.bash_profile

#### Add the following lines, modifying them to match your environment.

    export http_proxy=username:password@proxyhost.com:8080
    export https_proxy=username:password@proxyhost.com:8081
    exprot no_proxy=localhost, 127.0.0.1, *.my.lan

#### Save your settings.

    The proxy settings will be applied the next time you start a session, by logging into the server or opening a new Terminal window from a Desktop.
    To force apply your new proxy settings in the current Terminal session, execute the source command against your bash profile.

    source ~/.bash_profile

### windows下装的docker,pull下来的ubuntu:last没有vi这种命令啊，怎么办？vim 
    用 “docker attach 容器id” 进入容器后apt install vim就可以了。
    不过国内访问archive.ubuntu.com特别的慢，所以建议在建立容器时 docker run命令跟一个参数 
    -v 宿主机地址:容器内地址 这样的方式建立一个挂载关系
    如：docker run -it -p 3306:3306 --name=ubuntu -v /User/tester/dockerVolumns:/tmp
    这样就建立了自己主机上/User/tester/dockerVolumns目录和容器内tmp目录的绑定关系，那么tmp里面有任何的改动都会在主机的绑定目录下有反应，这个时候可以把/etc/apt/source.list复制出来，到／tmp里面去再 在主机上打开编辑器编辑好镜像仓库，再复制回/etc/apt这个目录里面去，最后apt update一下就可以使用国内的镜像仓库了。
    然后按照刚才说的 apt install vim就可以安装了。
    Linux的经典文本编辑器vi的使用,	基本的文件内容查看命令

### win下Docker默认存储位置修改
[win下Docker默认存储位置修改](https://chybeta.github.io/2017/02/14/win%E4%B8%8BDocker%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E4%BD%8D%E7%BD%AE%E4%BF%AE%E6%94%B9/)

 docker commit -m="new Linux system with gcc g++ ping openssl make vim installed" 49efe8bedaf4 jerry4docker/jerryubuntu:first

### install dig

    sudo apt-get install dnsutils

### ISSUE: fatal error: Python.h: No such file or directory
    refer to https://stackoverflow.com/questions/21530577/fatal-error-python-h-no-such-file-or-directory

    should :
    apt-get install python-dev   # for python2.x installs
    apt-get install python3-dev  # for python3.x installs

### pip install Scrapy in ubuntu
    
    first need to
                |
                +---apt-get install python-dev

# [docker中，如何将镜像保存为tar文件或者将镜像保存为文件，将tar文件导入到docker中](https://www.cnblogs.com/chuanzhang053/p/10084156.html)

## docker images 存在哪里
- [docker images 存在哪里](https://codeday.me/bug/20180702/187031.html)
- [docker在本地如何管理image?](https://segmentfault.com/a/1190000009730986)
- [Docker 基础 : 镜像](https://www.cnblogs.com/sparkdev/p/8901728.html)
- [docker 在本地如何管理 image（镜像）?](https://www.yangcs.net/posts/how-manage-image/)
```
    docker-machine ssh default
    sudo -i
    root@default:/mnt/sda1/var/lib/docker/aufs/diff# du -sh * |sort -u
    root@default:/mnt/sda1/var/lib/docker/aufs/diff#
```

## prepare in jerryubuntu
```
    docker search boonyadocker/tomcat-allow-remote  
    docker pull boonyadocker/tomcat-allow-remote 
    
    docker run -it jerry4docker/jerryubuntu:first /bin/bash
    docker run -it jerry4docker/jerryubuntu:latest /bin/bash
    docker run -d -p 10000:22 -it jerry4docker/jerryubuntu:v3 /bin/bash
    docker run -d -p 23:22 -itd jerry4docker/jerryubuntu:v3 /bin/bash
    docker run -it debian /bin/bash --registry-mirror=https://docker.mirrors.ustc.edu.cn
    docker-machine ssh default

    apt-get install libncurses5-dev
    apt-get install cscope
    apt-get install git
    docker ps
    docker attach XXXX

    cd /jerry/work/.ssh

    copy pub key to gerrit

    winpty docker run --name devhub -it jerry4docker/jerry4docker:v4 /bin/bash
```

## docker安装Ubuntu以及ssh连接
- [docker安装Ubuntu以及ssh连接](https://www.cnblogs.com/mengw/p/11413461.html)

## commit and push docker images
    
    jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
    $ docker ps
    CONTAINER ID        IMAGE                            COMMAND             CREATED                 STATUS              PORTS               NAMES
    4bc58592fccd        jerry4docker/jerryubuntu:first   "/bin/bash"         4 hours ago             Up 4 hours                              sharp_lichterman

    jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
    $ docker commit 4bc58592fccd  jerry4docker/jerryubuntu:latest
    sha256:788c7e711aa3e14822610b85184d3fa71b52f7c7b37e7c249c84064fcbb6d4c8

    jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
    $ docker images
    REPOSITORY                 TAG                 IMAGE ID            CREATED              SIZE
    jerry4docker/jerryubuntu   latest              788c7e711aa3        About a minute ago       3.14GB
    jerry4docker/jerryubuntu   first               94fd38a8c1ee        11 months ago            752MB

   [Reference push docker images](https://blog.csdn.net/boonya/article/details/74906927)

    docker push jerry4docker/jerry4docker:v7

## configuration of vscode of gtags
    
   [reference link](https://www.cnblogs.com/hgwang/p/10279023.html)

## Docker configure ssh login
- [Linux系统安装docker并用ssh登录docker容器的操作方法](https://www.jb51.net/article/164010.htm)
- [Linux主机如何用ssh去登录docker容器的步骤](https://blog.csdn.net/ypbsyy/article/details/80529101)
- [docker新建ubuntu容器，设置ssh与物理机登陆](https://www.cnblogs.com/winchua/p/4942837.html)
- [Linux系统安装docker并用ssh登录docker容器的操作方法](https://www.jb51.net/article/164010.htm)

```
    安装容器的openssh-server，输入 apt-get install openssh-server -y

    成功安装后，vim /etc/ssh/sshd_config，修改下面两个配置

    PermitRootLogin yes  
    UsePAM no

    启动ssh服务，service ssh start

    退出容器，输入exit，然后输入docker ps -a，查看容器的ID

    提交容器成为新的镜像，例如叫做ubuntu-ssh，输入docker commit 容器ID ubuntu-ssh

    启动这个镜像的容器，并映射本地的一个闲置的端口（例如10000）到容器的22端口，并启动容器的sshd docker run -d -p 10000:22 ubuntu-ssh /usr/sbin/sshd -D

    现在打开新的终端，输入ssh root@127.0.0.1 -p 10000，如果能链接成功，会要求输入密码的，输入刚才的123456就可以进入容器的终端了
```

## rename a container
```  
    docker rename bbb76c123a97 ubuntu-v3
```

## Docker容器进入的4种方式
  [Docker容器进入的4种方式](https://www.cnblogs.com/xhyan/p/6593075.html)
  [为什么不需要在 Docker 容器中运行 sshd](https://www.oschina.net/translate/why-you-dont-need-to-run-sshd-in-docker?cmp)

  docker run -v /usr/local/bin:/target jpetazzo/nsenter
  PID=$(docker inspect --format {{.State.Pid}} <container_name_or_ID>)
  nsenter --target $PID --mount --uts --ipc --net --pid

## 环境变量
- [解决docker容器不能自动加载环境变量问题](https://www.jianshu.com/p/3b50f23b6f38)

## Docker 网络
### 宿主机访问Docker container网络
- [windows10配置Docker容器独立IP地址互相通信](https://www.cnblogs.com/fusheng11711/archive/2004/01/13/11003124.html)
- [WIN10系统和Docker内部容器IP互通](https://blog.csdn.net/u014104286/article/details/82961203)
- [内网环境下修改Docker Toolbox的访问地址并暴露端口](https://blog.csdn.net/weixin_38187317/article/details/102918056)
- [Win10 Ping通Docker Toolbox容器](https://zhuanlan.zhihu.com/p/95290602)
- [理解 Docker 网络(一) -- Docker 对宿主机网络环境的影响](https://zhuanlan.zhihu.com/p/59538531)
- [Docker容器跨主机通信之：直接路由方式](https://www.cnblogs.com/xiao987334176/p/10049844.html)
- [docker安装使用全教程包含独立ip](https://blog.csdn.net/mergerly/article/details/54926127) -- 很好
- [Linux之Docker容器中的网络](https://blog.csdn.net/qq_43830639/article/details/98481670?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-1-98481670.nonecase&utm_term=安装docker后没有eth1&spm=1000.2123.3001.4430) -很好
```
    docker-machine ssh default
    sudo -i
    #增加新IP，eth1为192.168.99.101的网卡
    ifconfig eth1:0 192.168.99.101 netmask 255.255.255.0 up
    echo "ifconfig eth1:0 192.168.99.101 netmask 255.255.255.0 up" >> /opt/bootlocal.sh
    #查看容器IP
    docker inspect -f '{{.NetworkSettings.IPAddress}}' centoslatest
    #增加转发，192.168.99.101为新增地址，172.17.0.2为容器IP
    iptables -t nat -A PREROUTING -d 192.168.99.101 -j DNAT --to-destination 172.17.0.2
    iptables -t nat -A POSTROUTING -d 172.17.0.2 -j SNAT --to 172.17.0.1
    #开启IP转发
    echo 1 > /proc/sys/net/ipv4/ip_forward  
    echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
    sysctl -p
    #防火墙开启转发
    iptables -F
    至此访问192.168.99.101即访问容器
```
## 环境变量
- [解决docker容器不能自动加载环境变量问题](https://www.jianshu.com/p/3b50f23b6f38)
 
## docker文件迁移到其他磁盘解决c盘空间不足的问题

- [docker文件迁移到其他磁盘解决c盘空间不足的问题](https://blog.csdn.net/iteye_10432/article/details/103288296)


## Ubuntu Linux下修改docker镜像源

- [Ubuntu Linux下修改docker镜像源](https://www.runoob.com/docker/ubuntu-docker-install.html)
```
在国内访问国外的Docker镜像源通常都是非常慢的，特别是最近GFW升级后，就变得更加慢了，因为要使用Docker中的镜像，这个时候最好就是将镜像指向国内的资源。

国内亲测可用的几个镜像源：

Docker 官方中国区：https://registry.docker-cn.com
网易：http://hub-mirror.c.163.com
中国科技大学：https://docker.mirrors.ustc.edu.cn
阿里云：https://y0qd3iq.mirror.aliyuncs.com
增加Docker的镜像源配置文件 /etc/docker/daemon.json，如果没有配置过镜像该文件默认是不存的，在其中增加如下内容：

{
  "registry-mirrors": ["https://y0qd3iq.mirror.aliyuncs.com"]
}
其中的URL就是指定的镜像源，可以将其设置为上面说的四个镜像源中的任何一个。

然后重启Docker服务：

service docker restart
然后通过以下命令查看配置是否生效：

docker info|grep Mirrors -A 1
可以看到如下的输出：

Registry Mirrors:
 https://y0qd3iq.mirror.aliyuncs.com/
就表示镜像配置成功，然后再执行docker pull操作，就会很快了。
```

## docker如何将运行中的容器保存为docker镜像?
```
答: 使用docker commit和docker save保存镜像

$ sudo docker commit <当前运行的container id> <仓库名称>:<tag>
$ sudo docker save -o <仓库名称>-<tag>.img <仓库名称>:<tag>
示例如下:
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
111111111111        222222222222        "/bin/bash"   5 minutes ago       Up 5 minutes                                       jello
$ sudo docker commit 111111111111 bash:1.0
$ sudo docker save -o bash-1.0.img bash:1.0
```