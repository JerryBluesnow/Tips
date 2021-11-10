# Kamailio

## Kamailio 简介
```
Kamailio项目诞生于 2005年7月， 它是从德国FhG FOKUS研究所主导的SIP Express Router(SER)项目组分裂出来的。新项目建立的目标是创建一个开放的开发环境，以建立一个强大的可扩展的开源SIP服务器。最初，新项目命名为OpenSer，后来因为商标侵权问题，在2008年7月28号，重命名为Kamailio（另外一个分枝是OpenSips）。
 
      Kamailio的官方主页是http://www.kamailio.org 。1.5.x前的版本，其源码托管在sourceforge.net的SVN库上。从3.0.0版开始，源码托管在sip-router.org的GIT库上。

不同于PBX，KAMAILIO是个纯粹的SIP服务器，它可以作为PROXY、注册服务器、重定向服务器，也可作为简单的PRESENCE服务器，其本身并不处理RTP，可能通过RTPPROXY来处理RTP的NAT问题
```


## 视频通话 reference

- [搭建kamailio服务器实现公网视频通话](https://blog.csdn.net/CHNIM/article/details/100121942)

## Kamailio SIP 软交换中文知识分享平台

- [Kamailio SIP 软交换中文知识分享平台](www.kamailio.org.cn/doku.php?id=start)

- [rtpengine github](https://github.com/sipwise/rtpengine)

- [RTC hacker](https://blog.csdn.net/voipmaker)

- [基于声网的音视频SDK和FreeSWITCH开发WebRTC2SIP Gateway 方案和思路（一）](blog.itpub.net/69972526/viewspace-2701410/)

- [freeswitch之SIP动态注册及动态配置拨号方案](https://blog.csdn.net/weixin_45843878/article/details/105655400)

- [[转]WebRTC 音视频开发总结（十）](https://www.cnblogs.com/decwang/articles/4573296.html)

- [Android WebRTC 音视频开发总结（一）](https://www.cnblogs.com/lingyunhu/p/3578218.html)

- [Freeswitch系统分享](https://blog.csdn.net/weixin_45843878/category_9929765.html)

- [如何实现WebRTC协议与SIP协议互通](https://blog.csdn.net/weixin_45843878/article/details/107786455)

- [www.webrtc2sip.com](www.webrtc2sip.com)


## WebRTC与SIP互通

- [WebRtc与SIP](https://www.cnblogs.com/cnhk19/p/9475038.html)

```
要想让WebRTC与sip互通，要解决两个层面的问题：信令层和媒体层。
两个网络使用的信令机制不同，所以要进行信令的转换，才能完成媒体的协商，建立会话。媒体层要完成编码的转换，以及rtp/srtp转换等功能。这里主要说项信令层面的互通。

信令互通方案
目前sip和webrtc信令上互通有两种解决方案：

用JavaScript实现sip协议栈，webrtc应用程序基于这个协议栈开发。这样webrtc client发出的信令就是sip信令，但一般采用websocket为信令传输协议。这样的webrtc client就可以直接注册到支持ws的sip server上了。
jssip 、sipml5 都是这种解决方案。
通过转换网关实现协议的转换，从而互通。一个开源的网关项目就是 webrtc2sip。
webrtc2sip是一个功能很完善的网关，既实现了信令层，也实现了媒体层，编码转换功能很强大，也可以直接当做媒体网关，用于编解码，沟通两端的媒体。
```


## rtpengine build

- 1. git clone https://github.com/sipwise/rtpengine.git rtpengine
- 2. install necessary package
```
apt-get install gperf -y
apt-get install dkms -y
apt-get install module-assistant -y
apt-get install libbencode-perl -y
apt-get install libcrypt-rijndael-perl -y
apt-get install libdigest-hmac-perl -y
apt-get install libio-socket-inet6-perl -y
apt-get install libio-socket-ip-perl -y
apt-get install libsocket6-perl -y
apt-get install debhelper -y
apt-get install iptables-dev -y
apt-get install libcurl4-openssl-dev -y
apt-get install libpcre3-dev -y
apt-get install libxmlrpc-core-c3-dev -y
apt-get install markdown -y
apt-get install libglib2.0-dev -y
apt-get install libevent-dev -y
apt-get install libhiredis-dev -y
apt-get install libpcap0.8-dev -y
apt-get install libswresample-dev -y
apt-get install libavcodec-dev libavformat-dev libswscale-dev libavutil-dev  -y
apt-get install libspandsp-dev -y
apt-get install libavfilter-dev -y
apt-get install libjson-glib-1.0-0 libjson-glib-dev -y
apt-get install libmysql++-dev -y
apt-get install libwebsockets-dev -y
```

## kamailio build

git clone https://github.com/kamailio/kamailio.git kamailio

The main index for documentation is available at:

https://www.kamailio.org/w/documentation/
The online documentation for modules in the latest stable branch:

https://kamailio.org/docs/modules/stable/
The wiki collects a consistent number of tutorials, the indexes for variables, functions and parameters:

https://www.kamailio.org/wiki/

Step by step tutorials to install Kamailio from source code are available at:

https://www.kamailio.org/wiki/start#installation
Please read the INSTALL file from the source code for more information.

Repositories for Linux packages:

deb: https://www.kamailio.org/wiki/packages/debs
rpm: https://www.kamailio.org/wiki/packages/rpms

## mix audio
- [MultiStreamsMixer](https://www.webrtc-experiment.com/MultiStreamsMixer/)

- [Certbot-免费的HTTPS证书](https://zhuanlan.zhihu.com/p/80909555)

- [学习资料之Kaimailio and rtpengine安装使用](https://blog.csdn.net/weixin_41486034/article/details/106249598)

