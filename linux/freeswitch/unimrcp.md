### freeswitch+kamailio+unimrcp
```
  安装freeswitch和unimrcp
  1.准备
  2.安装顺序：
  安装freeswitch：
  安装unimrcp
  3.开始配置 freeswitch 和unimrcp
  启动freeswitch
  配置freeswitch
  unimrcpserver-mrcp2 对应的是一个配置：
  配置freeswitch对应的 unimrcp
  启动unimrcp
  用sip客户端拨打
  4.配置负载均衡kamailio
  1.日志设置：
  2.负载均衡mrcp的配置
  3.freeswitch端口修改
  4.图结构
  5.sip的h5的客户端的配置wss
  需求
  问题：
  需要修改的内容：
  freeswitch基本命令：
  生成wss的key
  配置freeswitch
  配置nginx
  其他参考
  6.FAQ
  问题1：freeswitch内网无声音的问题（外网ip导致的问题）
  问题2：407等认证问题
  问题3： 480
  问题4：没有声音的问题
  安装freeswitch和unimrcp
  1.准备
  freeswitch-1.8.7.tar.gz
  下载
  https://files.freeswitch.org/releases/freeswitch/freeswitch-1.8.7.tar.gz

  unimrcp-deps-1.3.0
  下载：https://www.unimrcp.org/project/component-view/dependencies/unimrcp-deps-1-3-0-tar-gz

  unimrcp.tar.gz

  git clone git@github.com:unispeech/unimrcp.git 
  目前测过的 commit版本：

  commit 7c8703d6fe4d9816bf635b2848e3657606729fe3
  Merge: 373d886 ebeaff6
  Author: Arsen Chaloyan <achaloyan@gmail.com>
  Date:   Sat Apr 18 19:02:17 2020 -0700
      Merge pull request #269 from ladenedge/master
      Supply OPTIONS request when responding in offline mode. Fixes #268.
  libsndfile-1.0.28.tar.gz

  wget http://www.mega-nerd.com/libsndfile/files/libsndfile-1.0.28.tar.gz
  ivr-welcome.wav
  这个必须有 ，否则websocket的音频传不出来，是freeswitch需要的音频流的文件

  sofia-sip.tar.gz

  git clone https://github.com/freeswitch/sofia-sip.git
  2.安装顺序：
  1.安装freeswitch：
  libsndfile-1.0.28.tar.gz
  freeswitch-1.8.7.tar.gz

  2.安装unimrcp：

  unimrcp-deps-1.3.0.tar.gz
  sofia-sip.tar.gz
  unimrcp.tar.gz

  安装freeswitch：
  yum install sqlite sqlite-devel -y
  yum install yasm -y
  yum install lua-devel -y
   yum install autoconf automake -y
   yum install libtool -y
  yum install openssl-devel -y
  yum install speex-devel -y
  yum install ldns-devel ldns -y 
  yum install libedit-devel -y
  yum install libtiff-devel -y
  yum install curl-devel -y
  ############# libsndfile-1.0.28.tar.gz #############

  ./configure 

  make

  make install

  cp /usr/local/lib/pkgconfig/sndfile.pc /usr/lib64/pkgconfig


  wget http://www.ijg.org/files/jpegsrc.v6a.tar.gz
  tar zxvf jpegsrc.v8a.tar.gz
  cd jpeg-8a
  ./configure --enable-shared
  make && make install`

  #然后 重新 configure FreeSWITCH , 再 make

  #如果还是报这个错误,就修改这两行,在 Makefile 末尾:

  #vim src/mod/formats/mod_sndfile/Makefile

  修改这两行

  #install: install-am
  #all: install
  freeswitch-1.8.7.tar.gz  
  ./devel-bootstrap.sh
  vim  modules.conf
  #codecs/mod_opus
  #applications/mod_signalwire
  打开： 这个先不打开

  #asr_tts/mod_unimrcp
  ./configure  --disable-signalwire --prefix=/usr/local/freeswitch
  make 
  make install  
  cd /root/freeswitch-1.8.7/libs/unimrcp
  autoreconf -fiv
  cd /root/freeswitch-1.8.7/src/mod/asr_tts/mod_unimrcp
  make
  make install 
  检查

  /usr/local/freeswitch/lib/freeswitch/mod

  是否有mod_unimrcp.la mod_unimrcp.so

  sofia-sip.tar.gz

  ./bootstrap.sh
  ./configure --prefix=/usr/local/sofia-sip
  make
  make install 
  export PKG_CONFIG_PATH=/usr/local/sofia-sip/lib/pkgconfig:${PKG_CONFIG_PATH}
  ldconfig
  unimrcp-deps-1.3.0.tar.gz

  这个如果装，可能会出现 错误：…/…/platforms/libunimrcp-client/.libs/libunimrcpclient.so: undefined reference to `apr_pool_mutex_set’

  ./build-dep-libs.sh

  安装unimrcp
  export UNIMRCP_APR_INCLUDES=" -I/root/unimrcp-deps-1.3.0/libs/apr/include "
  export UNIMRCP_APR_LIBS=" -L/root/unimrcp-deps-1.3.0/libs/apr/.libs/ -lapr-1 "
  ./bootstrap
  ./configure --prefix=/usr/local/unimrcp --with-sofia-sip=/usr/local/sofia-sip
  make
  make install 
  3.开始配置 freeswitch 和unimrcp
  unimrcp基本不用改

  观察一下端口号：

  启动freeswitch
  /usr/local/unimrcp/bin/unimrcpserver -o 3 -d

  netstat -nltp|grep unimrcp

  8060
  配置freeswitch
  触发 unimrcp的lua脚本

  /usr/local/freeswitch/share/freeswitch/scripts/names.lua

  --打印日志

  session:consoleLog("info","hao--------------进入欢迎的语音菜单");

  --要执行answer才能给对方播放语音菜单

  session:answer();

  --设置这一行才会在lua执行完毕以后不自动挂断

  session:setAutoHangup(false)

  --在死循环里面必定要判断当前会话还有没有效

  while(session:ready()==true) do

          --播放语音，告诉对方，每个拨号的选项

          session:consoleLog("info","hao--------------循环里面");

          session:streamFile("/usr/share/freeswitch/ivr-welcome.wav");

          --这里获取对端输入的dtmf信息，也就算按下的是多少

          local digit = session:getDigits(2, "#", 1000);

           --下面对数字逐一判断 选择执行

          if(digit == "1") then

               session:consoleLog("info","hao------>>>>>1>>>begin");

               session:set_tts_parms("unimrcp", "xiaofang");

               session:speak("我是个帅哥")

               session:consoleLog("info","hao------>>>>>1>>>end");

          end

          if(digit == "0") then

                  --若是匹配按下的是0，进入call center，call center是一个APP，默认没用call center模块，须要在源码自行安装而且 须要load mod_callcenter加载

                  session:consoleLog("info","hao------>>>>>>>>进入callcenter");

                  session:execute("callcenter","necoagent");

          end

  end
  修改配置：

  /usr/local/freeswitch/etc/freeswitch

  /usr/local/freeswitch/etc/freeswitch/autoload_configs/modules.conf.xml

  检查是否启用了

  /usr/local/freeswitch/etc/freeswitch/autoload_configs/unimrcp.conf.xml

  <configuration name="unimrcp.conf" description="UniMRCP Client">

    <settings>





      <param name="default-tts-profile" value="unimrcpserver-mrcp2"/>





      <param name="default-asr-profile" value="unimrcpserver-mrcp2"/>



      <param name="log-level" value="DEBUG"/>



      <param name="enable-profile-events" value="false"/>

      <param name="max-connection-count" value="100"/>

      <param name="offer-new-connection" value="1"/>

      <param name="request-timeout" value="3000"/>

    settings>

    <profiles>

      <X-PRE-PROCESS cmd="include" data="../mrcp_profiles/*.xml"/>

    profiles>

  configuration>
  unimrcpserver-mrcp2 对应的是一个配置：
  新增：

  /usr/local/freeswitch/etc/freeswitch/mrcp_profiles/unimrcpserver-mrcp-v2.xml

  <include>



    <profile name="unimrcpserver-mrcp2" version="2">

      <param name="server-ip" value="10.48.172.157"/>

      <param name="server-port" value="8060"/>

      <param name="resource-location" value=""/>

      <param name="speechsynth" value="speechsynthesizer"/>

      <param name="speechrecog" value="speechrecognizer"/>



      <param name="rtp-ip" value="auto"/>

      <param name="rtp-port-min" value="4000"/>

      <param name="rtp-port-max" value="5000"/>







      <param name="codecs" value="PCMU PCMA L16/96/8000"/>



      <synthparams>

      synthparams>



      <recogparams>



      recogparams>

    profile>

  注意改一下本机ip

  配置freeswitch对应的 unimrcp
  unimrcpserver-mrcp2 这里对应的就是 autoload_configs/unimrcp.conf.xml 中的配置default-tts-profile

  修改触发ivr按键 拨号号码：

  /usr/local/freeswitch/etc/freeswitch/dialplan/default.xml

  新增

      <extension name="unimrcp">

       <condition field="destination_number" expression="^5001$">

          <action application="answer"/>

          <action application="lua" data="names.lua"/>

       condition>

      extension>
  mkdir -p /usr/share/freeswitch/

  cp ivr-welcome.wav /usr/share/freeswitch/

  yum install -y git alsa-lib-devel autoconf automake bison broadvoice-devel bzip2 curl-devel libdb4-devel e2fsprogs-devel erlang flite-devel g722_1-devel gcc-c  ++ gdbm-devel gnutls-devel ilbc2-devel ldns-devel libcodec2-devel libcurl-devel libedit-devel libidn-devel  libmemcached-devel libogg-devel libsilk-devel   libsndfile-devel libtheora-devel libtiff-devel libtool libuuid-devel libvorbis-devel libxml2-devel lua-devel lzo-devel  ncurses-devel net-snmp-devel  openssl-devel opus-devel pcre-devel perl perl-ExtUtils-Embed pkgconfig portaudio-devel postgresql-devel python-devel python-devel soundtouch-devel speex-devel   sqlite-devel unbound-devel unixODBC-devel wget which yasm zlib-devel libshout-devel libmpg123-devel lame-devel rpm-build libX11-devel libyuv-devel
  启动freeswitch：

  /usr/local/freeswitch/bin/freeswitch -nonat -ncwait

  如果

  Cannot Initialize [[error near line 6896]: unexpected closing tag ]

  检查配置文件是否出错

  启动unimrcp
  /usr/local/unimrcp/bin/unimrcpserver -o 3 -d
  观察两个log

  一个是unimrcp的：

  /usr/local/unimrcp/log

  tail -f unimrcpserver_current.log

  /usr/local/freeswitch/var/log/freeswitch

  tail -f freeswitch.log
  用sip客户端拨打
  输入ip

  账号1001

  密码默认的1234

  5001

  按键 1 触发tts的unimrcp 参考names.lua脚本

  按键0 退出挂断
  如果
  freeswitch报错：
  Invalid TTS module unimrcp
  检查modules.conf.xml 是否
  加载
  4.配置负载均衡kamailio
  1.日志设置：
  https://blog.csdn.net/feiying5829/article/details/82666377

  etc/kamailio/kamailio.cfg

  log_facility=LOG_LOCAL0

  vi /etc/rsyslog.conf

  local0.* -/var/log/kamailio.log

  systemctl stop   rsyslog.service    关闭日志服务

  systemctl start   rsyslog.service     开启日志服务
  如果配置不成功

  默认日志还是去/var/log/message里面去看

  2.负载均衡mrcp的配置
  kamailio.cfg

  代码块

  #add by hao begin
  loadmodule "dispatcher.so"
  #modparam("tm", "reparse_invite", 0)
  modparam("dispatcher", "list_file", "/usr/local/kamailio/etc/kamailio/dispatcher.list")
  route {
      if(method=="INVITE"){
      # dst_select( "GROUP", "HASH METHOD")
        ds_select_dst("1","4");
        #sl_send_reply("100","Trying");
        forward();#uri:host, uri:port);
        exit();
      }
  }
  #modparam("dispatcher", "force_dst", 1)
  ###add by hao end
  需要处理 INVITE 的第一个包，否则不对

  dispatcher.list

  1 sip:10.48.127.22:8060
  1 sip:10.48.172.157:8060
  22和157配置了两个unimrcp的服务 ，端口都是8086

  kamctlrc 配置数据库的一些信息

  SIP_DOMAIN=haoning.org
  DBENGINE=MYSQL
  DBHOST=localhost
  DBNAME=kamailio
  DBRWUSER="root"
  DBRWPW="haoning"
  3.freeswitch端口修改
  由于kamailio用的是5060

  freeswitch用的也是5060

  把freeswitch的端口改成5062 （5061也已经被freeswitch占用了）

  netstat -nltpu |grep freeswitch

  freeswitch/etc/freeswitch/vars.xml

  <X-PRE-PROCESS cmd="set" data="internal_sip_port=5062"/>
  4.图结构
  freeswitch ----kamailio----->两个unimrcp

  5.sip的h5的客户端的配置wss
  需求
  建立一个浏览器打电话的功能：可以任何设备都可以随时测试我们的tts等功能

  h5------nginx---->freeswitch---->unimrcp+tts

  问题：
  浏览器使用音频功能需要https
  https的websocket需要wss
  https和wss都需要证书
  前提条件
  freeswitch体统了websocket
  ws的端口是5066
  wss的端口是7443
  nginx搭建的web代码使用的端口是80 ，https的端口是默认的443

  需要修改的内容：
  1.生成证书 和key给nginx使用 和给freeswitch使用
  2.配置freeswitch的证书wss.pem
  3.配置nginx的证书crt 和 key

  freeswitch基本命令：
  查看变量

  fs_cli
  eval $${certs_dir}
  /usr/local/freeswitch/etc/freeswitch/tls
  eval $${conf_dir}
  /usr/local/freeswitch/etc/freeswitch
  fs_cli -x 'sofia status profile internal' | grep WSS-BIND-URL
  生成wss的key
  参考https://freeswitch.org/confluence/display/FREESWITCH/WebRTC#WebRTC-InstallCertificates

  key的生成 ，服务器的ip是：10.189.160.70 ， 如果是域名，就把key更换成域名

  mkdir /usr/local/freeswitch/certs

  cd /usr/local/freeswitch/certs

  wget http://files.freeswitch.org/downloads/ssl.ca-0.1.tar.gz

  tar zxfv ssl.ca-0.1.tar.gz

  cd ssl.ca-0.1/

  perl -i -pe 's/md5/sha256/g' *.sh

  perl -i -pe 's/1024/4096/g' *.sh

  ./new-root-ca.sh

  输入密码 三次

  Country Name (2 letter code) [MY]:CN

  State or Province Name (full name) [Perak]:HN

  Locality Name (eg, city) [Sitiawan]:CS

  Organization Name (eg, company) [My Directory Sdn Bhd]:my_ca

  Organizational Unit Name (eg, section) [Certification Services Division]:machu

  Common Name (eg, MD Root CA) []:10.189.160.70

  Email Address []:killinux@163.com

  ./new-server-cert.sh 10.189.160.70

  Country Name (2 letter code) [MY]:CN

  State or Province Name (full name) [Perak]:HN

  Locality Name (eg, city) [Sitiawan]:CS

  Organization Name (eg, company) [My Directory Sdn Bhd]:my_server

  Organizational Unit Name (eg, section) [Secure Web Server]:manong

  Common Name (eg, www.domain.com) []:10.189.160.70

  Email Address []:killinux@163.com

  ./sign-server-cert.sh 10.189.160.70

  一路y

  cat 10.189.160.70.crt 10.189.160.70.key > /usr/local/freeswitch/certs/wss.pem
  配置freeswitch
  vim  /usr/local/freeswitch/conf/sip_profiles/internal.xml

  #Set these params and save the file:

  <param name="tls-cert-dir" value="/usr/local/freeswitch/certs"/>

  <param name="wss-binding" value=":7443"/>

  fs_cli -x 'sofia status profile internal' | grep WSS-BIND-URL
  配置nginx
  nginx配置：主要是https

      map $http_upgrade $connection_upgrade {
          default upgrade;
            ''      close;
      }
      server {
           listen       80;
           listen       443 ssl;
           root         /usr/local/nginx/html;
           proxy_read_timeout 3600;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $connection_upgrade;
           ssl_certificate      /usr/local/freeswitch/certs/ssl.ca-0.1/10.189.160.70.crt;
           ssl_certificate_key  /usr/local/freeswitch/certs/ssl.ca-0.1/10.189.160.70.key;
           location / {
               if ($scheme = 'http') {
                   set $ws_port 5066;
               }
               if ($scheme = 'https') {
                   set $ws_port 7443;
               }
               if ($http_upgrade = 'websocket') {
                   proxy_pass    $scheme://$server_addr:$ws_port;
               }
           }
      }
  ###################

     map $http_upgrade $connection_upgrade {
          default upgrade;
            ''      close;
      }
      server {
           listen       80;
           listen       443 ssl;
           root         /usr/local/nginx/html;
           proxy_read_timeout 3600;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection $connection_upgrade;
           ssl_certificate      /usr/local/freeswitch/certs/ssl.ca-0.1/10.189.160.70.crt;
           ssl_certificate_key  /usr/local/freeswitch/certs/ssl.ca-0.1/10.189.160.70.key;
           location / {
               root   html;
               index  index.html index.htm;
           }
      }
  其他参考
  生成证书：
  https://blog.csdn.net/Rookie_Manito/article/details/112765729
  配置freeswitch：
  https://blog.csdn.net/Rookie_Manito/article/details/112771183
  wss.pem的位置
  https://blog.csdn.net/weixin_42275389/article/details/89183536/

  客户端ios也可以直接用 Adore SIP Client

  6.FAQ
  问题1：freeswitch内网无声音的问题（外网ip导致的问题）
  如果报 MEDIA_TIMEOUT 等错误，并不是播放的基础库没装，可能是网络问题

  配置里的ip有两个，一个是内部的，一个是外部的ext-rtp-ip，外网进入的ip，这个如果是局域网内访问，非外网访问，可能会存在端口不通。

  参考：

  https://blog.csdn.net/sinat_33384251/article/details/95059763

  填坑指南，可能存在ipv6的问题，把配置文件备份。https://www.cnblogs.com/lmsthoughts/p/9322816.html

  常用命令：https://www.jianshu.com/p/2ffc55c8da83 http://diseng.github.io/assets/sip-test-guide/appendix/appendix-three.html

  查看sofia模块状态：sofia status
  查看freeswitch状态：status
  查看通话命令： show calls
  查看channel命令： show channels
  打开log命令：console loglevel 7
  关闭log命令：console loglevel 0
  重新加载xml： reloadxml
  开启全局信令追踪：sofia global siptrace on
  关闭全局信令追踪：sofia global siptrace off
  发起一个通话：originate 呼叫字符串 &fsApp
  挂断所有通话：hupall
  退出fs_cli：/bye
  显示在线用户:show registration
  开启sip消息显示：sofia global siptrace on
  关闭sip消息显示：sofia global siptrace off
  fs_cli中呼叫指定号码，并且回传语音：originate user/1000 &echo
  启动 freeswitch ：/usr/local/freeswitch/bin/freeswitch -nonat -ncwait

  客户端登录：/usr/local/freeswitch/bin/fs_cli
  查看内网的配置：

  sofia status profile internal
  Ext-RTP-IP Ext-SIP-IP 指向的是外网的美团云的ip ，这个ip的 5080和5060是不通的

  去配置文件搜索

  grep -nR ext-rtp-ip *

  找到 四个配置文件里面有

  因为是内网使用，我们只改internel

  $${external_rtp_ip} 这个变量是取外网地址，我们改成相同的内网地址就可以了

  sip_profiles/internal.xml

      <param name="ext-rtp-ip" value="10.189.160.70"/>

      <param name="ext-sip-ip" value="10.189.160.70"/>
  如果sipp不通也可能是这个问题

  问题2：407等认证问题
  grep -nR accept-blind-auth *
  sip_profiles/internal.xml:249:

   <param name="accept-blind-auth" value="true"/>
  这个注释打开

  问题3： 480
  dialplan/public.xml

    <extension name="public_extensions">
       <condition field="destination_number" expression="^(10[01][0-9])$">
     <action application="transfer" data="$1 XML default"/>
       condition>
     extension>
  改成

  10改成50

    <extension name="public_extensions">
       <condition field="destination_number" expression="^(50[01][0-9])$">
     <action application="transfer" data="$1 XML default"/>
       condition>
     extension>
  问题4：没有声音的问题
  如果没有声音可能是nat的问题

  https://blog.csdn.net/daitu3201/article/details/80096630?utm_medium=distribute.pc_relevant.none-task-blog-2defaultbaidujs_baidulandingword~default-0.control& spm=1001.2101.3001.4242

  修改ext-rtp-ip和ext-sip-ip为freeswitch公网地址

  <param name="ext-rtp-ip" value="10.48.127.22"/>![](2022-10-05-23-33-05.png)
  <param name="ext-sip-ip" value="10.48.127.22"/>
  参考freeswitch内网无声音的问题（外网ip导致的问题

```