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

```
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
```

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
- [配置共享文件夹](https://blog.csdn.net/weixin_33714884/article/details/86345700)

## Error response from daemon: error parsing HTTP 404 response body
![Error response from daemon: error parsing HTTP 404 response body](pic/pic_1.png)

## Cannot perform an interactive login from a non TTY device
![Cannot perform an interactive login from a non TTY device](pic/pic_1.png)

- [Docker Hub 仓库使用，及搭建 Docker Registry](https://segmentfault.com/a/1190000012662268)

- [docker 学习笔记21：docker连接网络的设置](https://www.cnblogs.com/51kata/p/5268951.html)
    可以修改 /etc/default/docker 配置文件

    If you need Docker to use an HTTP proxy, it can also be specified here.
    export http_proxy="http://127.0.0.1:3128/"
    export http_proxy="http://代理地址:端口"
    docker run -it --rm ubuntu bash

### the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'

    switch from git bash to powershell

## ISSUE
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

## Command 
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
- [win下Docker默认存储位置修改](https://chybeta.github.io/2017/02/14/win%E4%B8%8BDocker%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E4%BD%8D%E7%BD%AE%E4%BF%AE%E6%94%B9/)

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

## docker images 存在哪里
- [docker images 存在哪里](https://codeday.me/bug/20180702/187031.html)
- [docker在本地如何管理image?](https://segmentfault.com/a/1190000009730986)
- [Docker 基础 : 镜像](https://www.cnblogs.com/sparkdev/p/8901728.html)
- [docker 在本地如何管理 image（镜像）?](https://www.yangcs.net/posts/how-manage-image/)
- [docker中，如何将镜像保存为tar文件或者将镜像保存为文件，将tar文件导入到docker中](https://www.cnblogs.com/chuanzhang053/p/10084156.html)
```
docker-machine ssh default
sudo -i
root@default:/mnt/sda1/var/lib/docker/aufs/diff# du -sh * |sort -u
root@default:/mnt/sda1/var/lib/docker/aufs/diff#
```

## prepare in jerryubuntu

- [docker容器配置ssh](https://blog.csdn.net/cheney__chen/article/details/81639203)

```
    docker search boonyadocker/tomcat-allow-remote  
    docker pull boonyadocker/tomcat-allow-remote 
    
    docker run -it jerry4docker/jerryubuntu:first /bin/bash
    docker run -it jerry4docker/jerryubuntu:latest /bin/bash
    docker run -d -p 10000:22 -it jerry4docker/jerryubuntu:v3 /bin/bash
    docker run -d -p 23:22 -itd jerry4docker/jerryubuntu:v3 /bin/bash
    docker run -it debian /bin/bash --registry-mirror=https://docker.mirrors.ustc.edu.cn
    docker run -it --restart=always --privileged --network=host --name vonr
     -v /etc/resolv.conf:/etc/resolv.conf centos:centos7.6.1810 /usr/sbin/init
    docker run -it --restart=always --privileged --network=host --name vonrbd -v /etc/resolv.conf:/etc/resolv.conf jerry4docker/kamailiocentos:v1 /usr/sbin/init
    docker run -it --restart=always --privileged --network=host --name sipp-build-test alpine:3.13 /usr/sbin/init
    docker run -it --restart=always --privileged --network=host --name vonras2 centos:centos7.6.1810 /usr/sbin/init

    docker run -it --privileged --network=host --name baon debian:10.13 /usr/sbin/init

    docker run -d -p 9982:22 --name=devhub --privileged --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it jerry4docker/jerry4docker:v6
    docker run -d -p 9982:22 -p 9528:9528 -p 9529:9529 -p 9530:9530 -p 9531:9531 -p 9906:3306 -p 9004:9004 --name=devhub --privileged --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it jerry4docker/jerry4docker:v9
    docker run -d -p 3306:3306 --name=mysql --privileged --cap-add=SYS_PTRACE -e MYSQL_ROOT_PASSWORD=admin --security-opt seccomp=unconfined -it jerry4docker/jerry4docker:v9

    docker run -d -p 9822:22 --name=vonr --privileged=true --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it centos:centos7.6.1810 /usr/sbin/init

    docker run -d --name=sbc_cmake --privileged=true -it 9db7bc2e6245 /usr/sbin/init

    [SBC CMAKE UT]
    docker run -it --name sbcx --privileged=true -v /home/jzhan107/sbc-sig:/home/jzhan107/sbc-sig d51fbc83a86c /usr/sbin/init
    docker run -d -p 9722:22 --name=diameter --privileged=true --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it centos:7.6.1810 /usr/sbin/init

    /var/run/mysqld/mysqld.sock
    ssh连接容器
    通过主机端口映射连接：ssh -p 9022 root@主机ip
    直接连接容器（需要网络通）：ssh -p 22 root@容器ip

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

- [Reference push docker images](https://blog.csdn.net/boonya/article/details/74906927)

```
jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
$ docker ps
CONTAINER ID        IMAGE                            COMMAND        CREATED       STATUS       PORTS  NAMES
4bc58592fccd        jerry4docker/jerryubuntu:first   "/bin/bash"    4 hours ago   Up 4 hours          sharp_lichterman
    
jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
$ docker commit 4bc58592fccd  jerry4docker/jerryubuntu:latest
sha256:788c7e711aa3e14822610b85184d3fa71b52f7c7b37e7c249c84064fcbb6d4c8
    
jerry@workingpc MINGW64 /c/Program Files/Docker Toolbox
$ docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED              SIZE
jerry4docker/jerryubuntu   latest              788c7e711aa3        About a minute ago       3.14GB
jerry4docker/jerryubuntu   first               94fd38a8c1ee        11 months ago            752MB

docker push jerry4docker/jerry4docker:v7
```

## configuration of vscode of gtags

- [reference link](https://www.cnblogs.com/hgwang/p/10279023.html)

## Docker configure ssh login

- [Linux系统安装docker并用ssh登录docker容器的操作方法](https://www.jb51.net/article/164010.htm)

- [Linux主机如何用ssh去登录docker容器的步骤](https://blog.csdn.net/ypbsyy/article/details/80529101)

- [docker新建ubuntu容器，设置ssh与物理机登陆](https://www.cnblogs.com/winchua/p/4942837.html)

- [Linux系统安装docker并用ssh登录docker容器的操作方法](https://www.jb51.net/article/164010.htm)

- [docker容器配置ssh](https://blog.csdn.net/cheney__chen/article/details/81639203)

- [docker安装Ubuntu以及ssh连接](https://www.cnblogs.com/mengw/p/11413461.html)

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

```
docker run -d -p 9982:22 --name=devhub --privileged --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it jerry4docker/jerry4docker:v6

docker run -d -p 22:22 --name=devhub2 --privileged --cap-add=SYS_PTRACE --security-opt seccomp=unconfined -it jerry4docker/jerry4docker:v9

docker run -itd -p 22:22 --name=devhub3 --privileged --cap-add=SYS_PTRACE --security-opt seccomp=unconfined jerry4docker/jerry4docker:v9 /sbin/init
```

```
ssh连接容器
通过主机端口映射连接：ssh -p 9022 root@主机ip
直接连接容器（需要网络通）：ssh -p 22 root@容器ip
```

```
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

## rename a container
```  
docker rename bbb76c123a97 ubuntu-v3
```

## Docker容器进入的4种方式
- [Docker容器进入的4种方式](https://www.cnblogs.com/xhyan/p/6593075.html)

- [为什么不需要在 Docker 容器中运行 sshd](https://www.oschina.net/translate/why-you-dont-need-to-run-sshd-in-docker?cmp)

  ```
  docker run -v /usr/local/bin:/target jpetazzo/nsenter
  PID=$(docker inspect --format {{.State.Pid}} <container_name_or_ID>)
  nsenter --target $PID --mount --uts --ipc --net --pid
  ```

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

## Windows下VirtualBox与Docker冲突
```
1、使用 VirtualBox：
bcdedit /set hypervisorlaunchtype off

2、使用 Docker：

bcdedit /set hypervisorlaunchtype auto
```
grep "ngss\:" per_call.log |awk -F "ACTIVE" '{ print $2 }' | awk -F "E\:" '{ print $1 }' |sort -u

https://update.code.visualstudio.com/commit:507ce72a4466fbb27b715c3722558bb15afa9f48/server-linux-x64/stable
```

## docker容器中无法使用systemctl命令启动进程

docker容器中安装了一些服务之后需要将其启动，但执行systemctl命令之后却提示如下报错：
System has not been booted with systemd as init system (PID 1). Can’t operate.
Failed to connect to bus: Host is down

原因是 1号进程不是 init ，而是其他例如 /bin/bash ，所以导致缺少相关文件无法运行。

解决方案：/sbin/init

例如：centos

docker run -d --name vonr --privileged=true centos /sbin/init
docker exec -it vonr /bin/bash

PS:–privilaged=true一定要加上的。

docker run -it --name vonr2 --privileged=true centos:centos7.6.1810 /usr/sbin/init

docker run -it --name vonrx --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup:ro centos:centos7.6.1810 /usr/sbin/init

docker run -it --name vonransible --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup:ro centos:centos7.9.2009 /usr/sbin/init
docker run -it --name vonransible2 --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup:ro centos:centos7.9.2009 /usr/sbin/init
docker run -it -d --name shaken --privileged=true --restart=always -v /home/jenkins/shaken:/home/shaken --net=host sbc-docker-releases.repo.lab.pl.alcatel-lucent.com/shaken:latest

docker run -it -d --name shaken2 --privileged=true --restart=always --net=host sbc-docker-releases.repo.lab.pl.alcatel-lucent.com/shaken:v1.0

docker build -t sbc-docker-releases.repo.lab.pl.alcatel-lucent.com/shaken:v1.0 -f ./shaken.dockerfile .

docker build -t cns-sbc-docker-local.artifactory-hz1.int.net.nokia.com/jzhan107/shaken:latest -f ./shaken.dockerfile .
docker build -t cns-sbc-docker-local.artifactory-hz1.int.net.nokia.com/jzhan107/shaken:latest -f ./shaken.dockerfile .
## 
1.修改数据库字符集
ALTER DATABASE database_name CHARACTER SET utf8
1
这一行SQL语句可以修改对应的数据库的默认字符集，这之后的表均会采用这一字符集，不过我们还需要修改现有的表。
当然，也可以在创建表的时候就指定默认字符集：

CREATE DATABASE database_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci
1
2.修改表的字符集
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8
1
这一行语句会修改一个表以及其中所有字段的字符集，如果不想修改现有的字段，只想修改表的默认字符集，可以使用下面这一行语句：

AlTER TABLE table_name DEFAULT CHARACTER SET utf8
————————————————
版权声明：本文为CSDN博主「gooding300」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/gooding300/article/details/88607534


## 排障集锦：九九八十一难之第十八难！-----System has not been booted with systemd as init system (PID 1). Can‘t operat

不吃小白菜 2020-09-21 23:04:53  9516  收藏 4
分类专栏： 排障集锦 文章标签： docker shell linux bash
版权
报错现象如下
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
1
2
解决方案一
检查启动命令 加参数 -itd --privileged 如果dockerfile中CMD中没有执行 要在后面命令加/usr/sbin/init

dockerun --privileged -itd --name systemctl3 -v /sys/fs/cgroup:/sys/fs/cgroup:ro systemctl:test

解决方案二
重启一个docker在后台运行 执行上面的命令
dockerun --privileged -itd --name systemctl3 -v /sys/fs/cgroup:/sys/fs/cgroup:ro systemctl:test

原因详解
–privateged 使container内的root拥有真正的root权限，不进行降权处理。否则，容器内的用户只是外部的一个普通用户，普通用户还想访问内核？让systemctl管理系统？ 而且默认情况下，在第一步执行的是 /bin/bash 所以我们使用了 /usr/sbin/init覆盖/bin/bash

同时 只能使用 docker exec -it systemctl5 /bin/bash 因为 exec 可以让我们执行被覆盖掉的默认命令 /bin/bash 同时 -it 也是必须的。
————————————————
版权声明：本文为CSDN博主「不吃小白菜」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_47219935/article/details/108720455



## 使用Docker搭建MySQL服务
一、安装docker#
windows 和 mac 版可以直接到官网下载 docker desktop

linux 的安装方法可以参考 https://www.cnblogs.com/myzony/p/9071210.html

可以在shell中输入以下命令检查是否成功安装： sudo docker version

二、建立镜像#
拉取官方镜像（我们这里选择5.7，如果不写后面的版本号则会自动拉取最新版）

docker pull mysql:5.7   # 拉取 mysql 5.7
docker pull mysql       # 拉取最新版mysql镜像
MySQL文档地址

检查是否拉取成功

$ sudo docker images
一般来说数据库容器不需要建立目录映射

docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=admin -d mariadb:latest 
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:5.7

docker run -p 3306:3306 --name mysql  -v /d/WorkSpace/mysql/my.cnf:/etc/mysql/my.cnf -e MYSQL_ROOT_PASSWORD=admin -d mysql:latest
–name：容器名，此处命名为mysql
-e：配置信息，此处配置mysql的root用户的登陆密码
-p：端口映射，此处映射 主机3306端口 到 容器的3306端口
-d：后台运行容器，保证在退出终端后容器继续运行
如果要建立目录映射

sudo docker run -p 3306:3306 --name mysql \
--privileged=true \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=admin \
-d mariadb:latest
-v：主机和容器的目录映射关系，":"前为主机目录，之后为容器目录
检查容器是否正确运行

docker container ls
可以看到容器ID，容器的源镜像，启动命令，创建时间，状态，端口映射信息，容器名字
三、连接mysql#
进入docker本地连接mysql客户端

sudo docker exec -it mysql bash
mysql -uroot -p123456
使用远程连接软件时要注意一个问题

我们在创建容器的时候已经将容器的3306端口和主机的3306端口映射到一起，所以我们应该访问：

host: 127.0.0.1
port: 3306
user: root
password: 123456
如果你的容器运行正常，但是无法访问到MySQL，一般有以下几个可能的原因：

防火墙阻拦

### 开放端口：
$ systemctl status firewalld
$ firewall-cmd  --zone=public --add-port=3306/tcp -permanent
$ firewall-cmd  --reload
### 关闭防火墙：
$ sudo systemctl stop firewalld
需要进入docker本地客户端设置远程访问账号

$ sudo docker exec -it mysql bash
$ mysql -uroot -p123456
mysql> grant all privileges on *.* to root@'%' identified by "password";
原理：

### mysql使用mysql数据库中的user表来管理权限，修改user表就可以修改权限（只有root账号可以修改）

mysql> use mysql;
Database changed

mysql> select host,user,password from user;
+--------------+------+-------------------------------------------+
| host                    | user      | password                                                                 |
+--------------+------+-------------------------------------------+
| localhost              | root     | *A731AEBFB621E354CD41BAF207D884A609E81F5E      |
| 192.168.1.1            | root     | *A731AEBFB621E354CD41BAF207D884A609E81F5E      |
+--------------+------+-------------------------------------------+
2 rows in set (0.00 sec)

mysql> grant all privileges  on *.* to root@'%' identified by "password";
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> select host,user,password from user;
+--------------+------+-------------------------------------------+
| host                    | user      | password                                                                 |
+--------------+------+-------------------------------------------+
| localhost              | root      | *A731AEBFB621E354CD41BAF207D884A609E81F5E     |
| 192.168.1.1            | root      | *A731AEBFB621E354CD41BAF207D884A609E81F5E     |
| %                       | root      | *A731AEBFB621E354CD41BAF207D884A609E81F5E     |
+--------------+------+-------------------------------------------+
3 rows in set (0.00 sec)
参考连接：#https://blog.csdn.net/jor_ivy/article/details/81323199

https://www.52pojie.cn/thread-727433-1-1.html


```
show variables like '%char%';
D://WorkSpace//ido-shop//cereshop//doc//1.4//cereshop.sql

[mysqld]
# 注意你的安装位置
datadir=C:/soft/a/MySQL/MariaDB 10.5/data
port=3306
innodb_buffer_pool_size=2035M
character-set-client-handshake = false  

character_set_server = utf8mb4  
character_set_filesystem = binary  
character_set_client = utf8mb4  
collation_server = utf8mb4_unicode_ci  
init_connect='SET NAMES utf8mb4'  
[client]
port=3306
# 注意位置
plugin-dir=C:/soft/a/MySQL/MariaDB 10.5/lib/plugin

default-character-set=utf8mb4  
  
[mysqldump]  
character_set_client=utf8mb  
  
[mysql]  
default-character-set=utf8mb4  
```

# minio
[Minio](https://www.jianshu.com/p/52dbc679094a)

docker run -it -p 9000:9000 --name minio \
-d --restart=always --privileged --network=host \
-e "MINIO_ACCESS_KEY=AU1KIRAXJNT5Z7ZWA2CY3ZTD" \
-e "MINIO_SECRET_KEY=glhL2C8am6GACEJJ9hhYikWA1NywYESqu9vNgXEiFoU2nTJn" \
minio/minio server /data

## [Hyper-V + WSL2与 VirtualBox 共存](https://www.cnblogs.com/Luad/p/13332170.html)

## wsl2安装docker

```bash

```

开刚刚安装的Ubuntu，安装依赖：

```bash
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```

信任 Docker 的 GPG 公钥：

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

对于 amd64 架构的计算机，添加软件仓库：

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

最后安装

```bash
sudo apt-get update
sudo apt-get install docker-ce
```

## CentOS 7通过yum安装Docker和docker-compose
- [CentOS 7通过yum安装Docker和docker-compose](https://blog.csdn.net/hbtj_1216/article/details/104159936)

## [Docker]max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
查看该文件夹配置
grep vm.max_map_count /etc/sysctl.conf
添加配置
[root@Test elasticsearch]# echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
[root@Test elasticsearch]# grep vm.max_map_count /etc/sysctl.conf
vm.max_map_count=262144
立即生效
sysctl -w vm.max_map_count=262144
```
## WSL2安装使用

https://links.jianshu.com/go?to=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F144583887)

### wsl2是windows内置的linux子系统，安装步骤如下：

#### 1.Win10 版本号为 2004（内部版本19041或更高）即可，如果低于此版本可使用 Windows 10 易升工具手动升级。下载 Windows 10 易升工具：

```
https://www.microsoft.com/zh-cn/software-download/windows10
```

#### 2. 如果之前没有用过 WSL，那么首先需要为Linux启用Windows子系统:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

#### 3. 安装 WSL 2 之前，必须启用“虚拟机平台”可选功能

`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
重新启动计算机以完成WSL安装并更新到WSL 2。

#### 4. 下载Linux内核更新程序包

[下载地址](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fwsl%2Finstall-win10)
[https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi](https://links.jianshu.com/go?to=https%3A%2F%2Fwslstorestorage.blob.core.windows.net%2Fwslblob%2Fwsl_update_x64.msi)

#### 5、安装 Linux 分发版本

打开微软应用商店，搜索 Ubuntu，在列表中选择最新的长期支持版本 20.04 LTS 并安装。



![img](https://upload-images.jianshu.io/upload_images/9766611-8198b6228065a78c.png?imageMogr2/auto-orient/strip|imageView2/2/w/740/format/webp)

image.png

#### 6. 使用任一终端，输入以下命令查看 WSL 版本，确保 WSL 的版本为 2.0：



```ruby
$ wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-20.04    Stopped         2
```

##### 7. 如果显示当前不是 WSL 2 版本，可以通过以下命令设置 WSL 的默认版本：



```bash
wsl --set-version Ubuntu-20.04 2
```

##### 8. 如果安装有问题的话，勾选此选项：

![img](https://upload-images.jianshu.io/upload_images/9766611-53d3b718ba477c6a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1133/format/webp)

image.png

### 9. 进入wsl2终端:

打开任一命令行工具，输入 `wsl`

![img](https://upload-images.jianshu.io/upload_images/9766611-f05b7f66e7232609.png?imageMogr2/auto-orient/strip|imageView2/2/w/976/format/webp)

image.png



### 关于使用WSL2出现“参考的对象类型不支持尝试的操作”的解决方法。

[https://zhuanlan.zhihu.com/p/151392411](https://links.jianshu.com/go?to=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F151392411)
下载此软件：
链接: [https://pan.baidu.com/s/12_cAA9L0wNCqxpquuWjNeQ](https://links.jianshu.com/go?to=https%3A%2F%2Fpan.baidu.com%2Fs%2F12_cAA9L0wNCqxpquuWjNeQ) 提取码: pir4
管理员身份运行CMD输入:
`NoLsp.exe C:\windows\system32\wsl.exe`
执行成功会显示 success!

#### 解决无法安装sshpass的问题：

首选运行命令,更新清单：：

```csharp
sudo apt-get update 
```

然后检查包是否可用:

```undefined
 apt-cache search sshpass 
```

然后就可以安装了

```undefined
sudo apt install sshpass
```

#### 编写sh脚本,用sshpass 进行ssh自动登录操作：

需要先手动用命令进行ssh登录，这样本地会有一个ssh登录缓存，然后才能运行sh脚本

1. 本地ssh登录,输入密码

```css
ssh root@xxx.xxx.xx.xx
password:

exit
```

1. sshpass 脚本操作：

```tsx
export SSHPASS='xxxxxxxx'
cd /dir/
sshpass -e rsync -z -r   root@xxx.xxx.xx.xx:/dir/
```

###### 解决Linux下编译.sh文件报错 “[: XXXX: unexpected operator”

直接在cmd，git bash下执行sh脚本没问题，而再wsl下执行报上面的错误
原因是Ubuntu默认的sh是连接到dash的,而dash跟bash是不兼容的；
解决：wsl下执行命令sudo dpkg-reconfigure dash，选择no,意思就是不默认使用dash命令行


####
135.252.30.200 builder1， builder1

rnxt 5g-alpine

docker run -ti --entrypoint=sh 8f23d0491f08 --name=vonrx

## CentOS 7通过yum安装Docker和docker-compose
- [CentOS 7通过yum安装Docker和docker-compose](https://blog.csdn.net/hbtj_1216/article/details/104159936)



## docker desktop 登录到宿主host, 修改配置问题导致的无法启动

```text
docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
```

最后登陆到宿主host就能操作所有容器文件了:

```text
chroot /host
```

例如你要操作某一个容器的文件，例如es01，可以通过命令查看es01容器的：

```text
docker inspect es01
```

## CentOS7.6安装docker-compose

```

Centos7.6安装docker-compose
官网地址:https://docs.docker.com/compose/install/

运行此命令以下载Docker Compose的当前稳定版本

curl -L "https://get.daocloud.io/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

对二进制文件应用可执行权限：

chmod +x /usr/local/bin/docker-compose
```

## 设置docker开机自启动 docker compose设置容器自启动
```
Docker启动命令

systemctl start docker
Docker开机自启动

systemctl enable docker
Docker设置容器为自启动

--restart=always
Docker修改容器状态为自启动

docker update --restart=always 容器ID(或者容器名)
docker compose设置容器自启动
```
## docker desktop 启动卡主
管理员权限
.\DockerCli.exe -SwitchDaemon

原因：
-PauseHDL /pause/events server not replying: Get "http://ipc/pause/events": open \\.\pipe\dockerProcd: The system cannot find the file specified.

if not works, open dev-sidecar for proxy the network

### cpu-rt-runtime
Docker容器在默认运行的时候由于安全原因，限制了一些功能，如果需要开启需在运行时进行配置。

要使用实时调度需要 --cpu-rt-runtime 来运行容器。

sched_setscheduler()函数要求sys_nice能力，Docker容器在运行的时候默认是不开启的。

所以需要在运行的时候运行：docker run -ti --cpu-rt-runtime=95000 --ulimit rtprio=99 --cap-add=sys_nice ubuntu

在使用上述命令是可能会遇到错误cpu-rt-runtime写入失败，原因是cgroup parent 的cpu-rt-runtime比你要修改的小。

运行 docker run -it --privileged --pid=host ubuntu nsenter -t 1 -m

cd /sys/fs/cgroup/cpu/docker 

查看 cpu.rt_runtime_us是多少

cat cpu.rt_runtime_us

将它修改为950000
```
## 如何给运行中的docker容器增加映射端口
```
方式二: 命令行操作
#1、查看容器的信息
docker ps -a
 
#2、查看容器的端口映射情况，在容器外执行：
docker port 容器ID 或者 docker port 容器名称
 
#3、查找要修改容器的全ID
docker inspect 容器ID |grep Id
 
#4、进到/var/lib/docker/containers 目录下找到与全 Id 相同的目录，修改 其中的hostconfig.json 和 config.v2.json文件：
 
#注意：若该容器还在运行中，需要先停掉
docker stop 容器ID 或者 docker stop 容器名称
 
#再停掉docker服务
systemctl stop docker
#可能会提示错误 Warning: Stopping docker.service, but it can still be activated by:
  docker.socket 不要管他 这是docker在关闭状态下被访问自动唤醒机制，很人性化，即这时再执行任意docker命令会直接启动
 
#5、修改hostconfig.json如下
#	格式如："{容器内部端口}/tcp":[{"HostIp":"","HostPort":"映射的宿主机端口"}]
"PortBindings":{"22/tcp":[{"HostIp":"","HostPort":"3316"}],"80/tcp":[{"HostIp":"","HostPort":"801"}]}
 
#6、修改config.v2.json在ExposedPorts中加上要暴露的端口
#	两个地方
```
"ExposedPorts":{"3306/tcp":{},"80/tcp":{}}"
"Ports":{"3306/tcp":[{"HostIp":"0.0.0.0","HostPort":"33061"}],"80/tcp":[{"HostIp":"","HostPort":"801"}]}"
最后改完之后，重启docker服务就行了

systemctl restart docker
————————————————
版权声明：本文为CSDN博主「new_PHP大神」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/my476530/article/details/125658365
```


# 这个在qd014Jenkins上可以windows本地用mysql -uroot -padmin -hxxx.xxx.xxx.xxx连接， go项目也能连接
docker run --name=mysql -it -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin --privileged=true -d registry.access.redhat.com/rhscl/mysql-57-rhel7
docker run --name=mysql -it -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin --privileged=true -d mariadb:latest

# 这个能跑起来
docker run -p 3306:3306 --name mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-v /var/run/mysqld:/var/run/mysqld \
-e MYSQL_ROOT_PASSWORD=admin \
-d mariadb:latest

docker run -p 3306:3306 --name mysql \
-v /home/jerry_zhang/usr/local/docker/mysql/conf:/etc/mysql \
-v /home/jerry_zhang/usr/local/docker/mysql/logs:/var/log/mysql \
-v /home/jerry_zhang/usr/local/docker/mysql/data:/var/lib/mysql \
-v /home/jerry_zhang/var/run/mysqld:/var/run/mysqld \
-e MYSQL_ROOT_PASSWORD=admin \
-d mariadb:latest


## 如何将所有操作系统/架构的Docker映像拉/推到Dockerhub？
```
# Create a tag for any digests you are interested in
docker pull --platform=amd64 mariadb:10.5.15
docker tag 2c7d1ac0ae1b hb.radionxt.com/thirdparty/mariadb:10.5.15-amd64

docker pull --platform=arm64 mariadb:10.5.15
docker tag eb7ed9035465 hb.radionxt.com/thirdparty/mariadb:10.5.15-arm64

docker manifest inspect
# Push to your registry
docker push hb.radionxt.com/thirdparty/mariadb:10.5.15-amd64
docker push hb.radionxt.com/thirdparty/mariadb:10.5.15-arm64
# Create and push the manifest
docker manifest create hb.radionxt.com/thirdparty/mariadb:10.5.15 --amend hb.radionxt.com/thirdparty/mariadb:10.5.15-arm64 --amend hb.radionxt.com/thirdparty/mariadb:10.5.15-amd64
docker manifest push hb.radionxt.com/thirdparty/mariadb:10.5.15
```
