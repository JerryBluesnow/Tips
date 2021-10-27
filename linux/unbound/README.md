## [Wiki] https://wiki.archlinux.org/index.php/Unbound_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

## there is no systemctl command in ubuntu

    apt-get install 

## systemctl 命令详解及使用教程

    https://linux265.com/news/3385.html
    https://linux.cn/article-5926-1.html
    https://linux265.com/course/3763.html

## Unbound服务的安装与运行管理

一、[Unbound服务的安装与运行管理](https://www.cnblogs.com/rusking/p/7591938.html)
- 1．获取Unbound软件包
    RHEL7.x自带了Bind和Unbound两种DNS服务包,Unbound是红帽公司推荐使用的DNS服务器。目前,虽然Bind在全球拥有最多的用户,但这个老牌产品是针对简单网络设计的,随着网络的迅速发展,Bind系统已经越来越不适应在如今复杂的大规模网络环境下提供DNS服务了。Unbound是FreeBSD(类Unix)操作系统下的默认DNS服务器软件,它是一个功能强大、安全性高、跨平台(类Unix、Linux、Windows)、易于配置,以及支持验证、递归(转发)、缓存等功能的DNS服务软件,其主要安装文件有:

    (1) unbound-1.4.20-28.el7.x86_64.rpm:DNS的主程序包。

    (2) unbound-libs-1.4.20-28.el7.x86_64:进行域名解析必备的库文件。

- 2．检查是否已安装Unbound软件包
    [root@dyzx ~]# rpm -qa unbound*
    unbound-libs-1.4.20-28.el7.x86_64

- 3． 安装Unbound软件包
    [root@dyzx ~]# mount /dev/cdrom /mnt

    [root@dyzx ~]# rpm -ivh /mnt/Packages/unbound-1.4.20-28.el7.x86_64.rpm

- 4．Unbound服务的运行管理
- - (1)Unbound服务的启动、停止、重启、重新加载和状态查询

    systemctl start|stop|restart|reload|status unbound.service

- - (2) Unbound服务在系统开机时自动启动或不启动

    systemctl enable|disable unbound.service

- - (3)检查Unbound进程

    ps -ef | grep unbound

- - (4)查看Unbound服务启动后侦听的端口

    ss -tunap | grep unbound

- 5. 使用Unbound软件部署DNS服务器时,相关的配置文件及目录。
      位置及名称

    作用

    /etc/unbound/unbound.conf

    主(全局)配置文件

    /etc/unbound/local.d/

    子配置文件所在目录。其中的*.conf文件用于定义正向解析记录和反向解析记录以及设置转发

    /etc/hosts

    用于指定IP地址与主机名的映射关系

    /etc/resolv.conf

    为Linux客户端指定DNS服务器的IP地址的配置文件

    /etc/nsswitch.conf

    /etc/nsswitch.conf文件的第39行“hosts: files dns”规定了一台主机解析的顺序,首先找的是本地文件/    etc/hosts,然后再是DNS。

- 二、授权DNS服务器的配置
- - 1、案例需求
    为学校搭建一台授权DNS服务器,该服务器能访问互联网中其他DNS服务器,能解析校园网内搭建的所有服务器的域  名,并通过配置转发地址使校园网内的用户使用域名访问校园网内外的服务器,网络连接方式及配置参数如图所  示：

    image

    服务器

    完全合格域名

    IP地址

    授权DNS服务器

    dns1.dyzx.edu

    192.168.8.1

    纯缓存DNS服务器

    dns2.dyzx.edu

    192.168.8.2

    Web服务器

    www.dyzx.edu

    192.168.8.3

    FTP服务器

    ftp.dyzx.edu

    192.168.8.3

    邮件服务器

    mail.dyzx.edu

    192.168.8.4

- - 2、授权DNS服务器的配置
- - - 步骤1:以root用户身份登录RHEL7系统→配置DNS服务器网卡的IP地址为192.168.8.1/24、主机名为dns1.dyzx.edu。

- - - 步骤2:安装.Unbound软件包→启动和开机自动启动Unbound服务。

    [root@dns1 ~]# mount /dev/cdrom /mnt

    [root@dns1 ~]# rpm -ivh /mnt/Packages/unbound-1.4.20-28.el7.x86_64.rpm

    [root@dns1 ~]# systemctl start unbound

    [root@dns1 ~]# systemctl enable unbound

- - - 步骤3:使用vim编辑配置文件unbound.conf,对服务器全局参数进行配置

    [root@dns1~]# vim /etc/unbound/unbound.conf

    //配置区域的全局参数:

    interface: 192.168.8.1 //38行:设置监听的网络接口(默认监听localhost网络接口)

    access-control: 192.168.8.0/24 allow //176行:允许allow或拒绝refuse给哪些地址提供解析服务

    username: "” //211行:改成空字符串,表示任何用户均可访问

    domain-insecure: "dyzx.edu" //372行:跳过验证域“dyzx.edu”,以避免信任链验证失败

    include: /etc/unbound/local.d/*.conf //472行:将指定的其他可能的配置文件包含进当前文件

- - - 步骤4:配置正向解析记录和反向解析记录。可以在全局配置文件中配置,也可以在/etc/unbound/local.d目录中定义一个以.conf结尾的文件中(如/etc/unbound/local.d/domain.conf)配置(以全局配置文件中的第454行～第470行为格式模板)。在此,继续在全局配置文件中配置正向和反向解析记录。

    [root@dns1~]# vim /etc/unbound/unbound.conf

    local-zone: "dyzx.edu." static //455行:设置解析的区域名

    //添加以下7行local-data,以定义正向解析记录

    local-data: "dyzx.edu. 86400 IN SOA dns1.dyzx.edu. root.dyzx.edu 1 1D 1H 1W 1H"

    local-data: "dns1.dyzx.edu. IN A 192.168.8.1"

    local-data: "dns2.dyzx.edu. IN A 192.168.8.2"

    local-data: "www.dyzx.edu. IN A 192.168.8.3"

    local-data: "ftp.dyzx.edu. IN CNAME www.dyzx.edu."

    local-data: "mail.dyzx.edu. IN A 192.168.8.4"

    local-data: "dyzx.edu. IN MX 5 mail.dyzx.edu."

    //添加以下5行local-data-ptr,以定义反向解析记录

    local-data-ptr: "192.168.8.1 dns1.dyzx.edu"

    local-data-ptr: "192.168.8.2 dns2.dyzx.edu"

    local-data-ptr: "192.168.8.3 www.dyzx.edu"

    local-data-ptr: "192.168.8.3 ftp.dyzx.edu"

- - - 步骤5:配置转发。任何一台DNS服务器能直接提供的解析记录都是有限的,当用户请求的解析记录超出了某台DNS服务器所能解析的范围时,就需要在该DNS服务器上设置转发功能,以便把超范围的用户解析请求转发给其他DNS服务器代为解析。若要将本DNS服务器的解析请求转发给由ISP提供的IP地址为8.8.8.8的公共DNS服务器,则只要在unbound.conf作以下修改便可。

    [root@dns1 ~]# vim /etc/unbound/unbound.conf
    
    forward-zone: //547行:定义转发forward
    
    name: "." //转发所有的查询
    
    forward-addr: 8.8.8.8 //将解析请求转发到指定IP地址的DNS服务器

- - - 步骤6:检测配置文件是否有语法错误,没有错误后,重启服务:

    [root@ dns1 ~]# unbound-checkconf

    unbound-checkconf: no errors in /etc/unbound/unbound.conf

    [root@ dns1 ~]# systemctl restart unbound

    步骤7:在服务器端的防火墙中开放DNS服务。

    [root@ dns1 ~]# firewall-cmd --permanent --add-service=dns //设置防火墙开放DNS服务

    [root@ dns1 ~]# firewall-cmd –reload

- - - 步骤8:Linux客户端测试。在客户端修改/etc/resolv.conf文件,将DNS服务器的IP地址指向上述所配置的授权DNS服务器的IP地址→使用nslookup命令验证DNS查询结果。

    [root@client ~]# vim /etc/resolv.conf

    nameserver 192.168.8.1

    [root@client ~]# nslookup

    > www.dyzx.edu //验证正向解析记录

    Server: 192.168.8.1

    Address: 192.168.8.1#53

    Name: www.dyzx.edu

    Address: 192.168.8.3

    > 192.168.8.1 //验证反向解析记录

    Server: 192.168.8.1

    Address: 192.168.8.1#53

    1.1.168.192.in-addr.arpa name = dns1.dyzx.edu.

    > set type=cname //验证别名记录的解析结果

    > ftp.dyzx.edu

    Server: 192.168.8.1

    Address: 192.168.8.1#53

    ftp.dyzx.edu canonical name = www.dyzx.edu

测试2：

    > set type=mx //验证MX记录的解析结果
    
    > dyzx.edu
    
    Server: 192.168.8.1
    
    Address: 192.168.8.1#53
    
    dyzx.edu mail exchanger = 5 mail.dyzx.edu.
    
    > www.baidu.com //验证转发功能的解析结果
    
    Server: 192.168.8.1
    
    Address: 192.168.8.1#53
    
    Non-authoritative answer:
    
    www.baidu.com canonical name = www.a.shifen.com.
    
    Name: www.a.shifen.com
    
    Address: 58.217.200.112
    
    Name: www.a.shifen.com
    
    Address: 58.217.200.113
    
    > exit //退出nslookup命令,结束测试

- - 三、纯缓存DNS服务器的配置
    为了提高校园网内域名解析的效率,减少校园网出口流量,现搭建一台纯缓存DNS服务器,配置参数如下图所示,其中递归查询转发到校园网内地址为192.168.8.1的授权DNS服务器。

- - - 步骤1:以root用户身份登录RHEL7系统→配置DNS服务器网卡的IP地址为192.168.8.2/24、主机名为dns2.dyzx.edu。

- - - 步骤2:安装.Unbound软件包→启动和开机自动启动。

    [root@ dns2 ~]# systemctl start unbound

    [root@ dns2 ~]# systemctl enable unbound

- - - 步骤3:使用vim编辑全局配置文件unbound.conf。

    [root@dns2~]# vim /etc/unbound/unbound.conf

    //配置区域的全局参数:

    interface: 192.168.8.2 //38行:设置DNS服务监听所有网络接口

    msg-cache-size: 8m //108行:缓存大小

    access-control: 0.0.0.0/0 allow //177行:允许所有地址访问,refuse表示拒绝;allow表示允许

    username: "" //211行:改成空字符串,表示任何用户均可访问

    domain-insecure: "dyzx.edu” //372行:跳过验证域"dyzx.edu",以避免信任链验证失败

    forward-zone: //547行:除掉行首"#"号,配置转发

    name: ".” //548行:除掉行首"#"号,并将"example.com"改为"."

    forward-addr: 192.168.8.1 //549行:将所有解析请求转发给192.168.8.1的授权DNS服务器

- - - 步骤4:检测配置文件是否有语法错误,确认无误后重新加载unbound服务:

    [root@ dns2 ~]# unbound-checkconf

    unbound-checkconf: no errors in /etc/unbound/unbound.conf

    [root@ dns2~]# unbound-control reload

- - - 步骤5:配置防火墙允许DNS流量。

    [root@ dns2 ~]# firewall-cmd --permanent --add-service=dns //设置防火墙开放DNS服务

    [root@ dns2 ~]# firewall-cmd --reload

- - - 步骤6:验证纯缓存DNS服务器。将客户端的DNS服务器的IP地址设为纯缓存DNS服务器的IP地址,然后使用nslookup命令测试正向解析和反向解析的效果。

*******VICTORY LOVES PREPARATION*******


root@bbb76c123a97:/usr/local/unbound/etc/unbound# service unbound status
 * unbound is running