# kamailio笔记


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

## kamailio reference

- [学习资料之Kaimailio and rtpengine安装使用](https://blog.csdn.net/weixin_41486034/article/details/106249598)
- [ubuntu上kamailio+rtpproxy+mediaproxy环境搭建](https://www.jianshu.com/p/9e2ffbf853fc)
- [kamailio docs](http://kamailio.org/docs/db-tables/kamailio-db-5.5.x.html)
- [kamailio modules](https://www.kamailio.org/docs/modules/stable/)
- [imsi和手机号码的关系](https://blog.csdn.net/qq276726581/article/details/53057078)
- [kamailio debug日志设置](https://blog.csdn.net/zhangtuo/article/details/81937791)

+ 以前装好的kamailio sip服务器经常在启动的时候经常遇到这个错误：
  ERROR: PID file /var/run/kamailio/kamailio.pid does not exist -- Kamailio start failed
  经过自己尝试发现可能引起的原因是因为在使用kamctl start之前使用了kamailio 导致启动了一些进程。
  使用PS 命令查看kamialio相关进程：

```
ps axw | /usr/bin/egrep kamailio

ps -ef|grep kamailio|grep -v grep|cut -c 9-15|xargs kill -9
```

## kamailio简介

Kamailio is an Open Source, GPL2, SIP Server Routing Platform. It is written in C for Linux/Unix plaforms and focuses on performance, flexibility and security.

+ Home page with new project name: http://www.kamailio.org

+ Home page with old project name: http://www.openser-project.org

+ SourceForge.net Project page: http://sourceforge.net/projects/openser/

+ Features
  + SIP proxy/registrar/redirect server (RFC3261, RFC3263)
  + UDP/TCP/TLS/SCTP support
  + Transactional stateful proxy
  + Modular architecture
  + Programmable configuration file
  + ENUM support
  + Call Processing Language (CPL)
  + Gateway to sms or xmpp
  + Authentication, authorization and accounting via Radius or database
  + NAT traversal system
  + Least cost routing
  + Load balancing
  + Carrier routing
  + Multiple database backends: MySQL, Postgres, Oracle, BDB or flat files
  + SIMPLE Presence Server (IETF SIMPLE extensions - rich presence)
  + Dialog Info Presence - SLA/BLA
  + XCAP and RLS
  + Presence User Agent
  + Dialog Stateful Proxy
  + Instant Messaging
  + Offline message storage
  + Instant messaging conferencing
  + SNMP support
  + Perl Programming Interface
  + Java SIP Servlet Application server
  + Over 80 modules (extensions)

+ Documentation
  + Main Documentation Page - http://www.kamailio.org/docs/
  + Dokuwiki Page - http://www.kamailio.org/dokuwiki/

### centos 7.6 安装kamailio n5记录

```shell
git remote set-url origin git@git.samxtec.com:bbluesnow/kamaililo.git

git remote set-url origin git@gitee.com:rnxt/vonrptt.git

https://gitee.com/rnxt/vonrptt

```
```shell
yum list java

yum install -y wget

yum install -y java-1.8.0-openjdk-devel.x86_64

yum groupinstall -y "Development Tools"

yum install -y json-c json-c-devel

yum install -y gcc

yum install -y bison

yum install -y flex

yum install -y MariaDB-devel MariaDB-client MariaDB-shared MariaDB-server

yum install -y libxml2-devel

yum install -y libmnl-devel

yum install -y libcurl libcurl-devel

make include_modules="db_mysql xmlrpc cdp cdp_avp ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf presence http_async_client" cfg

make all

make install
```
---

```shell
vi /usr/local/etc/kamailio/kamctlrc
change below value:
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL
```

then, 
`kamdbctl create`

`$ kamdbctl create`
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;

```
$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
No Subscribers are added yet
```

The kamdbctl will add two users in MySQL user tables:

- ***kamailio***   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- ***kamailioro*** - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database

## VoLTE Setup with Kamailio IMS and Open5GS

+ [VoLTE Setup with Kamailio IMS and Open5GS](https://open5gs.org/open5gs/docs/tutorial/02-VoLTE-setup/)

### 1. Setup description

+ MCC: 001, MNC: 01
+ Single OpenStack VM with Kamailio IMS and Open5GS (Internal IP 10.4.128.21 and Floating IP 172.24.15.30)
+ 4G Casa Smallcell
+ Sysmocom USIM - sysmoUSIM-SJS1
+ Oneplus 5 as UE

### 2. OpenStack is not required for VoLTE setup

```
If you are using one machine without OpenStack, you can start from step 3.

### 1. Start from Bionic Ubuntu cloud image

### 2. Use the following Cloud init config while spawning an instance
```
#cloud-config
disable_root: 0
ssh_pwauth: True
users:
  - name: root
chpasswd:
    list: |
    root:admin
    expire: False
runcmd:
  - sed -i -e '/^#PermitRootLogin/s/^.*$/PermitRootLogin yes/' /etc/ssh/sshd_config
  - systemctl restart sshd

This removes all existing cloud users and allows only root user and sets a password

### 3. Install following packages

+ ubuntu/Debian

  ```shell
  apt update && apt upgrade -y && apt install -y mysql-server tcpdump screen ntp ntpdate git-core dkms gcc flex bison libmysqlclient-dev make \
  libssl-dev libcurl4-openssl-dev libxml2-dev libpcre3-dev bash-completion g++ autoconf rtpproxy libmnl-dev libsctp-dev ipsec-tools libradcli-dev \
  libradcli4
  ```

+ CentOS

```

```

### 4. Clone Kamailio repository and checkout 5.3 version of repository

```
$ mkdir -p /usr/local/src/
$ cd /usr/local/src/
$ git clone https://github.com/herlesupreeth/kamailio
$ cd kamailio
$ git checkout -b 5.3 origin/5.3
```

### 5. Generate build config files

```
$ cd /usr/local/src/kamailio
$ make cfg
```

#### 6. Enable MySQL module and all required IMS modules.

Edit modules.lst file present at /usr/local/src/kamailio/src

The contents of `modules.lst` should be as follows:

```
# this file is autogenerated by make modules-cfg

# the list of sub-directories with modules
modules_dirs:=modules

# the list of module groups to compile
cfg_group_include=

# the list of extra modules to compile
include_modules= cdp cdp_avp db_mysql dialplan ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_ocs ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf outbound presence presence_conference presence_dialoginfo presence_mwi presence_profile presence_reginfo presence_xml pua pua_bla pua_dialoginfo pua_reginfo pua_rpc pua_usrloc pua_xmpp sctp tls utils xcap_client xcap_server xmlops xmlrpc

# the list of static modules
static_modules=

# the list of modules to skip from compile list
skip_modules=

# the list of modules to exclude from compile list
exclude_modules= acc_json acc_radius app_java app_lua app_lua_sr app_mono app_perl app_python app_python3 app_ruby auth_ephemeral auth_identity auth_radius cnxcc cplc crypto db2_ldap db_berkeley db_cassandra db_mongodb db_oracle db_perlvdb db_postgres db_redis db_sqlite db_unixodbc dnssec erlang evapi geoip geoip2 gzcompress h350 http_async_client http_client jansson janssonrpcc json jsonrpcc kafka kazoo lcr ldap log_systemd lost memcached misc_radius ndb_cassandra ndb_mongodb ndb_redis nsq osp peering phonenum pua_json rabbitmq regex rls rtp_media_server snmpstats systemdops topos_redis uuid websocket xhttp_pi xmpp $(skip_modules)

modules_all= $(filter-out modules/CVS,$(wildcard modules/*))
modules_noinc= $(filter-out $(addprefix modules/, $(exclude_modules) $(static_modules)), $(modules_all)) 
modules= $(filter-out $(modules_noinc), $(addprefix modules/, $(include_modules) )) $(modules_noinc) 
modules_configured:=1
```

### 7. Compile and install Kamailio
```
$ cd /usr/local/src/kamailio
$ export RADCLI=1
$ make Q=0 all | tee make_all.txt
$ make install | tee make_install.txt
$ ldconfig
```
The binaries and executable scripts are installed in: /usr/local/sbin
```
kamailio - Kamailio SIP server
kamdbctl - script to create and manage the Databases
kamctl - script to manage and control Kamailio SIP server
kamcmd - CLI - command line tool to interface with Kamailio SIP server
```
To be able to use the binaries from command line, make sure that /usr/local/sbin is set in PATH environment variable. You can check that with echo $PATH. If not and you are using bash, open /root/.bash_profile and at the end add:

```
  PATH=$PATH:/usr/local/sbin
  export PATH
```
Kamailio modules are installed at: <font color="Hotpink">/usr/local/lib64/kamailio/modules</font>

The documentation and readme files are installed at: <font color="Hotpink">/usr/local/share/doc/kamailio</font>

The configuration files are installed at: <font color="Hotpink">/usr/local/etc/kamailio</font>

In case you set the PREFIX variable in <font color="Hotpink">make cfg</font> command, then replace <font color="Hotpink">/usr/local</font> in all paths above with the value of PREFIX in order to locate the files installed.

### 8. Populate MySQL database using kamctlrc command

Edit SIP_DOMAIN and DBENGINE in the <font color="Hotpink">/usr/local/etc/kamailio/kamctlrc</font> configuration file (Used by kamctl and kamdbctl tools).

Set the SIP_DOMAIN to your SIP service domain (or IP address if you don’t have a DNS hostname associated with your SIP service). Set the DBENGINE to be MYSQL and adjust other setting as you want. Finally, uncomment both SIP_DOMAIN and DBENGINE.

In example above, the following values are set for SIP_DOMAIN and DBENGINE

```
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL
```
You can change other values in kamctlrc file. Once you are done updating kamctlrc file, run the script to create the database used by Kamailio:
```
$ kamdbctl create
```
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;
```
$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
```
No Subscribers are added yet

The kamdbctl will add two users in MySQL user tables:
```
- kamailio   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- kamailioro - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database
```
### 9. 安装rtp proxy
1. 安装依赖包:

```
   yum groupinstall "Development Tools"
   yum install glibc-static libstdc++-static
```

   gcc编译器大于4.9，如gcc5.4版本的安装： 升级gcc, gcc-v可以查看当前版本

   + [Centos7升级gcc版本方法之一使用scl软件集](https://blog.csdn.net/helpmsg/article/details/105876127)
   + [CentOS 7下升级gcc版本](https://blog.csdn.net/ncdx111/article/details/106047228)

   ```
   yum install centos-release-scl scl-utils-build
   yum list all --enablerepo='centos-sclo-rh' | grep devtoolset-.*
   可以根据grep出来的结果选择安装最高版本，其中下面命令中devtoolset-4 代表版本gcc 4.x
   yum install centos-release-scl -y
   yum install devtoolset-4-toolchain -y
   scl enable devtoolset-4 bash
   ```

2. 安装rtpproxy app:

- 可以参照这个连接进行安装详细访问[Install RTPProxy from source on Ubuntu 20.04/18.04/16.04](https://computingforgeeks.com/how-to-install-rtpproxy-from-source-on-ubuntu-linux/)

```
git clone -b master https://github.com/sippy/rtpproxy.git

cd rtpproxy

git -c rtpproxy submodule update --init --recursive

./configure

make 

make install

启动rtpproxy:

rtpproxy -l 182.XX.10.17 -s udp:127.0.0.1 7078 -F 
```

### 9. Edit /etc/default/rtpproxy file as follows:
```
# Defaults for rtpproxy

# The control socket.
#CONTROL_SOCK="unix:/var/run/rtpproxy/rtpproxy.sock"
# To listen on an UDP socket, uncomment this line:
#CONTROL_SOCK=udp:127.0.0.1:22222
CONTROL_SOCK=udp:127.0.0.1:7722

# Additional options that are passed to the daemon.
EXTRA_OPTS="-l 192.168.2.240 -d DBUG:LOG_LOCAL0"
```
here, <font color="Hotpink">-l <PUBLIC_IP></font>

Then run,
```
$ systemctl restart rtpproxy
```
### 10. Edit configuration file to fit your requirements for the VoIP platform:

You have to edit the /usr/local/etc/kamailio/kamailio.cfg configuration file.

Follow the instruction in the comments to enable usage of MySQL. Basically you have to add several lines at the top of config file, like:
```
#!define WITH_MYSQL
#!define WITH_AUTH
#!define WITH_USRLOCDB
#!define WITH_NAT

(uncomment this line)
auto_aliases=no

(uncomment this line and enter the DNS domain created above)
alias="ims.mnc000.mcc460.3gppnetwork.org"

(uncomment this line, 192.168.2.240 is the internal IP and 172.24.15.30 is the Public/Floating IP)
listen=udp:192.168.2.240:5060 advertise 172.24.15.30:5060
listen=tcp:192.168.2.240:5060 advertise 172.24.15.30:5060

(Further down, we will need to modify the rtpproxy_sock value to match the CONTROL_SOCK option we set for RTPProxy in /etc/default/rtpproxy)
modparam("rtpproxy", "rtpproxy_sock", "udp:127.0.0.1:7722")
```
If you changed the password for the ‘kamailio’ user of MySQL, you have to update the value for ‘DBURL’ parameters.

### 11. The init.d script
The init.d script can be used to start/stop the Kamailio server in a nicer way. A sample of init.d script for Kamailio is provided at:
```
/usr/local/src/kamailio/pkg/kamailio/deb/debian/kamailio.init
```
Just copy the init file into the /etc/init.d/kamailio. Then change the permissions:
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.init /etc/init.d/kamailio
$ chmod 755 /etc/init.d/kamailio
```
Then edit the /etc/init.d/kamailio file updating the $DAEMON and $CFGFILE values:
```
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin
DAEMON=/usr/local/sbin/kamailio
CFGFILE=/usr/local/etc/kamailio/kamailio.cfg
```
You need to setup a configuration file in the /etc/default/ directory. This file can be found at:

```
/usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default
```
You need to rename the /etc/default/kamailio file to ‘kamailio’ after you’ve copied it. Then edit this file and set RUN_KAMAILIO=yes. Edit the other options as per your setup.
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default /etc/default/kamailio
$ systemctl daemon-reload
```
Create the directory for pid file:
```
$ mkdir -p /var/run/kamailio
```
Default setting is to run Kamailio as user kamailio and group kamailio. For that you need to create the user and set ownership
```
$ adduser --quiet --system --group --disabled-password \
        --shell /bin/false --gecos "Kamailio" \
        --home /var/run/kamailio kamailio
$ chown kamailio:kamailio /var/run/kamailio
```
Then you can start Kamailio using the following commands:
```
$ systemctl start kamailio.service
```
check running processes with: ps axw egrep kamailio

### 12. A quick check for the basic working of SIP server can be done as follows:
Create new subscriber accounts. A new account can be added using kamctl tool via kamctl add <username> <password> (When asked for entering MySQL password for user kamailio@localhost: type kamailiorw, as provided in kamailio.cfg)
```
$ kamctl add test testpasswd
$ kamctl add test2 testpasswd
Setting on OnePlus phones
```
Connect to a network through which SIP server is reachable (either Wi-Fi or LTE)
Goto phone dialer and select the Settings in the menu on top right corner
Then select Call settings
Configure SIP accounts in phones as added above using kamctl:
In Phone 1:
```
Username: test
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
In Phone 2:
```
Username: test2
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test2
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
+ + Set “Receive incoming calls” option to enabled state in both phones
+ + Set “Use SIP calling” to “For all calls”
+ + Add a new contact as follows:

In Phone 1:

Select more option
```
Name: SIP Contact test2 (Any arbitary name)
SIP: test2@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
In Phone 2:

Select more option
```
Name: SIP Contact test (Any arbitary name)
SIP: test@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
Now try calling from either phone
```
Upon completion of this test, set “Receive incoming calls” option to disabled state and set “Use SIP calling” to “Only for SIP calls”
```
### 13. Create new mysql database for pcscf, scscf and icscf, populate databases and grant permissions to respective users identified by a password
```
$ mysql
<mysql> CREATE DATABASE  `pcscf`;
<mysql> CREATE DATABASE  `scscf`;
<mysql> CREATE DATABASE  `icscf`;
```
In all of the below steps, when prompted for mysql root user password, leave it blank i.e. Press Enter
```
$ cd /usr/local/src/kamailio/utils/kamctl/mysql
$ mysql -u root -p pcscf < standard-create.sql
$ mysql -u root -p pcscf < presence-create.sql
$ mysql -u root -p pcscf < ims_usrloc_pcscf-create.sql
$ mysql -u root -p pcscf < ims_dialog-create.sql

$ mysql -u root -p scscf < standard-create.sql
$ mysql -u root -p scscf < presence-create.sql
$ mysql -u root -p scscf < ims_usrloc_scscf-create.sql
$ mysql -u root -p scscf < ims_dialog-create.sql
$ mysql -u root -p scscf < ims_charging-create.sql

$ cd /usr/local/src/kamailio/misc/examples/ims/icscf
$ mysql -u root -p icscf < icscf.sql
```

Verify that following tables are present in respective databases by logging into mysql
```
+-----------------+
| Tables_in_pcscf |
+-----------------+
| active_watchers |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| location        |
| presentity      |
| pua             |
| version         |
| watchers        |
| xcap            |
+-----------------+

+-----------------+
| Tables_in_scscf |
+-----------------+
| active_watchers |
| contact         |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| impu            |
| impu_contact    |
| impu_subscriber |
| presentity      |
| pua             |
| ro_session      |
| subscriber      |
| version         |
| watchers        |
| xcap            |
+-----------------+

+---------------------+
| Tables_in_icscf     |
+---------------------+
| nds_trusted_domains |
| s_cscf              |
| s_cscf_capabilities |
+---------------------+

<mysql> grant delete,insert,select,update on pcscf.* to pcscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on scscf.* to scscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to icscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to provisioning@localhost identified by 'provi';
<mysql> GRANT ALL PRIVILEGES ON pcscf.* TO 'pcscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON scscf.* TO 'scscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'icscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'provisioning'@'%' identified by 'provi';
<mysql> FLUSH PRIVILEGES;
```
Then,
```
$ mysql
<mysql> use icscf;
<mysql> INSERT INTO `nds_trusted_domains` VALUES (1,'ims.mnc000.mcc460.3gppnetwork.org');
<mysql> INSERT INTO `s_cscf` VALUES (1,'First and only S-CSCF','sip:scscf.ims.mnc000.mcc460.3gppnetwork.org:6060');
<mysql> INSERT INTO `s_cscf_capabilities` VALUES (1,1,0),(2,1,1);
```
### 14. Copy pcscf, icscf and scscf configuration files to /etc folder and edit accordingly
一定要以这几个文件为base做改动，不要用源代码中misc目录中的文件
```
$ cd ~ && git clone https://github.com/herlesupreeth/Kamailio_IMS_Config
$ cd Kamailio_IMS_Config
$ cp -r kamailio_icscf /etc
$ cp -r kamailio_pcscf /etc
$ cp -r kamailio_scscf /etc
```
### 15. Setup the DNS for resolving IMS and EPC components names
```
$ apt install -y bind9
```
Use the below example DNS Zone file to create a DNS Zone file into the bind folder and edit /etc/named/named.conf.local and /etc/named/named.conf.options accordingly:
```
$ cd /etc/bind
```
In the below example: Kamailio IMS & DNS server running at 192.168.2.240/172.24.15.30 (Floating IP) and PCRF also at 192.168.2.240/172.24.15.30 (Floating IP)
```
$ cat ims.mnc000.mcc460.3gppnetwork.org
```
```
$ORIGIN ims.mnc000.mcc460.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        ns
ns                      1D IN A         192.168.2.240

pcscf                   1D IN A         192.168.2.240
_sip._udp.pcscf         1D SRV 0 0 5060 pcscf
_sip._tcp.pcscf         1D SRV 0 0 5060 pcscf

icscf                   1D IN A         192.168.2.240
_sip._udp               1D SRV 0 0 4060 icscf
_sip._tcp               1D SRV 0 0 4060 icscf

scscf                   1D IN A         192.168.2.240
_sip._udp.scscf         1D SRV 0 0 6060 scscf
_sip._tcp.scscf         1D SRV 0 0 6060 scscf

hss                     1D IN A         192.168.2.240
Create another DNS zone for resolving pcrf domain as follows:
```
```
$ cat epc.mnc001.mcc001.3gppnetwork.org
```
```
$ORIGIN epc.mnc001.mcc001.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        epcns
epcns                   1D IN A         192.168.2.240

pcf                    1D IN A         127.0.0.1
```
Edit /etc/named/named.conf.local file as follows:
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/named/zones.rfc1918";

zone "ims.mnc000.mcc460.3gppnetwork.org" {
        type master;
        file "/etc/named/ims.mnc000.mcc460.3gppnetwork.org";
};

zone "epc.mnc001.mcc001.3gppnetwork.org" {
        type master;
        file "/etc/named/epc.mnc001.mcc001.3gppnetwork.org";
};
```
Edit /etc/named/named.conf.options file as follows:
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
    
        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.
    
        //forwarders {
    	// Put here the IP address of other DNS server which could be used if name cannot be resolved with DNS server running in this machine (Optional)
    	//10.4.128.2;
        //};
    
        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation no;
        allow-query { any; };
    
        auth-nxdomain no;    # conform to RFC1035
        //listen-on-v6 { any; };
};
```
```
cp /etc/named/epc.mnc000.mcc460.3gppnetwork.org /var/named/
cp /etc/named/ims.mnc000.mcc460.3gppnetwork.org /var/named/
```

/etc/bind
[root@localhost bind]# ls
bind  epc.mnc000.mcc460.3gppnetwork.org  ims.mnc000.mcc460.3gppnetwork.org  named.conf.local  named.conf.options  pcf.mnc000.mcc460.3gppnetwork.org
[root@localhost bind]#

[root@localhost named]# ls
chroot      data     dyndb-ldap                         ims.mnc000.mcc460.3gppnetwork.org  named.empty      named.loopback
chroot_sdb  dynamic  epc.mnc000.mcc460.3gppnetwork.org  named.ca                           named.localhost  slaves
[root@localhost named]#

```
$ systemctl restart named
```
Then, test DNS resolution by adding following entries on top of all other entries in /etc/resolv.conf (make sure it persist across reboots)
```
search ims.mnc000.mcc460.3gppnetwork.org
nameserver 192.168.40.53
```
安装dig
```
yum -y install bind-utils
```
Finally, ping to ensure

```
$ ping pcscf
PING pcscf.ims.mnc000.mcc460.3gppnetwork.org (192.168.2.240) 56(84) bytes of data.
64 bytes from localhost (192.168.2.240): icmp_seq=1 ttl=64 time=0.017 ms
64 bytes from localhost (192.168.2.240): icmp_seq=2 ttl=64 time=0.041 ms
```
To make changes in /etc/resolv.conf be persistent across reboot edit the /etc/netplan/50-cloud-init.yaml file as follows:
```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        ens3:
            dhcp4: true
            match:
                macaddress: fa:16:3e:99:f5:67
            set-name: ens3
            nameservers:
                search: [ims.mnc000.mcc460.3gppnetwork.org,epc.mnc001.mcc001.3gppnetwork.org]
                addresses:
                      - 192.168.2.240
    version: 2
```
```
$ netplan apply
$ ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
$ systemctl restart systemd-resolved.service
```

### 16. Install RTPEngine
Check for dependencies, install dependencies and build .deb packages

```
$ export DEB_BUILD_PROFILES="pkg.ngcp-rtpengine.nobcg729"
$ apt install dpkg-dev
$ git clone https://github.com/sipwise/rtpengine
$ cd rtpengine && git checkout mr7.4.1
$ dpkg-checkbuilddeps
```

The above command checks for dependencies and give you a list of dependencies which are missing in the system. The below list is the result of this command

```
$ apt install debhelper default-libmysqlclient-dev gperf iptables-dev libavcodec-dev libavfilter-dev libavformat-dev libavutil-dev libbencode-perl libcrypt-openssl-rsa-perl libcrypt-rijndael-perl libdigest-crc-perl libdigest-hmac-perl libevent-dev libhiredis-dev libio-multiplex-perl libio-socket-inet6-perl libiptc-dev libjson-glib-dev libnet-interface-perl libpcap0.8-dev libsocket6-perl libspandsp-dev libswresample-dev libsystemd-dev libxmlrpc-core-c3-dev markdown dkms module-assistant keyutils libnfsidmap2 libtirpc1 nfs-common rpcbind
After installing dependencies run the below command again and verify that no dependencies are left out
```

```
$ dpkg-checkbuilddeps
```

This should just return back to shell with no output if all depedencies are met

```
$ dpkg-buildpackage -uc -us
$ cd ..
$ dpkg -i *.deb
$ cp /etc/rtpengine/rtpengine.sample.conf /etc/rtpengine/rtpengine.conf
```

Edit this file as follows under [rtpengine]:

```
interface = 192.168.2.240
```

Port on which rtpengine binds i.e. listen_ng parameter is udp port 2223. This should be updated in kamailio_pcscf.cfg file at modparam(rtpengine …)

rtpproxy params

```
modparam("rtpengine", "rtpengine_sock", "1 == udp:localhost:2223")
Edit /etc/default/ngcp-rtpengine-daemon and /etc/default/ngcp-rtpengine-recording-daemon as follows in respective files:

RUN_RTPENGINE=yes
RUN_RTPENGINE_RECORDING=yes
$ cp /etc/rtpengine/rtpengine-recording.sample.conf /etc/rtpengine/rtpengine-recording.conf
$ mkdir /var/spool/rtpengine
$ systemctl restart ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service
$ systemctl enable ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service

$ systemctl stop rtpproxy
$ systemctl disable rtpproxy
$ systemctl mask rtpproxy
Second instance of RTPENGINE can be run as follows (Optional)

$ iptables -I rtpengine -p udp -j RTPENGINE --id 1
$ ip6tables -I INPUT -p udp -j RTPENGINE --id 1
$ echo 'del 1' > /proc/rtpengine/control
$ /usr/sbin/rtpengine --table=1 --interface=192.168.2.240 --listen-ng=127.0.0.1:2224 --tos=184 --pidfile=ngcp-rtpengine-daemon2.pid --no-fallback --foreground
```

### 17. Running I-CSCF, P-CSCF and S-CSCF as separate process
First, stop the default kamailio SIP server
```
$ systemctl stop kamailio
$ systemctl disable kamailio
$ systemctl mask kamailio
```
Run all the process as root and NOT sudo

```
$ mkdir -p /var/run/kamailio_pcscf
$ kamailio -f /etc/kamailio_pcscf/kamailio_pcscf.cfg -P /kamailio_pcscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_scscf
$ kamailio -f /etc/kamailio_scscf/kamailio_scscf.cfg -P /kamailio_scscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_icscf
$ kamailio -f /etc/kamailio_icscf/kamailio_icscf.cfg -P /kamailio_icscf.pid -DD -E -e
```

### 18. Install Open5GS in the same machine as Kamailio IMS - Install Open5GS from source

Please refer to instructions at https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/

If you are using OpenStack, installing Open5GS and Kamailio IMS on the same machine is very important because the Framed-IP-Address in the AAR request via Rx interface takes received IP address and port in ims_qos module, hence, if the Open5GS is on a separate VM/machine, the IP and port received in received_ip and received_port values seen by Kamailio IMS will be the NATed IP of the Open5GS machine resulting in failing of AAR request.

Modify below mentioned parts of configuration files in addition to Configure Open5GS section. For reference, look at the configuration files at https://github.com/herlesupreeth/Open5gs_Config. These configuration only holds for open5gs tag v1.3.0, please tweak configuration files based on the open5gs tag you use.

Change realm of components to epc.mnc001.mcc001.3gppnetwork.org
Define IP pools for APNs used i.e one for default APN and another for IMS apn
Define P-CSCF address in the pgw configuration
Define a ConnectPeer for pcscf.ims.mnc000.mcc460.3gppnetwork.org with its IP and port in PCRF freediameter configuration
Setup IP tables for the UE pools defined and create appropriate tun interfaces
Below startup script can be used for setting up interfaces:

```
#!/bin/bash

sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl -w net.ipv6.conf.all.forwarding=1

ip tuntap add name ogstun mode tun
ip addr add 192.168.100.1/24 dev ogstun
ip addr add fd84:6aea:c36e:2b69::/48 dev ogstun
ip link set ogstun mtu 1400
ip link set ogstun up
iptables -t nat -A POSTROUTING -s 192.168.100.0/24 ! -o ogstun -j MASQUERADE
ip6tables -t nat -A POSTROUTING -s fd84:6aea:c36e:2b69::/48 ! -o ogstun -j MASQUERADE
iptables -I INPUT -i ogstun -j ACCEPT
ip6tables -I INPUT -i ogstun -j ACCEPT

ip tuntap add name ogstun2 mode tun
ip addr add 192.168.101.1/24 dev ogstun2
ip addr add fd1f:76f3:da9b:0101::/48 dev ogstun2
ip link set ogstun2 mtu 1400
ip link set ogstun2 up
iptables -I INPUT -i ogstun2 -j ACCEPT
ip6tables -I INPUT -i ogstun2 -j ACCEPT
Add users with following APN settings in Open5GS:

APN Configuration:
---------------------------------------------------------------------------------------------------------------------
| APN      | Type | QCI | ARP | Capability | Vulnerablility | MBR DL/UL(Kbps)     | GBR DL/UL(Kbps) | PGW IP        |
---------------------------------------------------------------------------------------------------------------------
| internet | IPv4 | 9   | 8   | Disabled   | Disabled       | unlimited/unlimited |                 |               |
---------------------------------------------------------------------------------------------------------------------
| ims      | IPv4 | 5   | 1   | Disabled   | Disabled       | 3850/1530           |                 |               |
|          |      | 1   | 2   | Enabled    | Enabled        | 128/128             | 128/128         |               |
|          |      | 2   | 4   | Enabled    | Enabled        | 128/128             | 128/128         |               |
---------------------------------------------------------------------------------------------------------------------
Finally, make sure of the following in Open5GS

PCO options which indicate the address of the Proxy-CSCF
Need to indicate support for Voice-over-Packet-Switched (VoPS) in NAS message to UE from EPC
### 19. Setup FoHSS in order to talk with I-CSCF and S-CSCF
Requirements for FoHSS: Install Java JDK and ant

Download Oracle Java 7 JDK from following link using a browser:

https://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html
$ mkdir -p  /usr/lib/jvm/
$ tar -zxf jdk-7u79-linux-x64.tar.gz -C /usr/lib/jvm/
$ update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_79/bin/java 100
$ update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_79/bin/javac 100
Verify that java has been successfully configured by running:

$ update-alternatives --display java
java - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/java
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/java
  link java is /usr/bin/java
/usr/lib/jvm/jdk1.7.0_79/bin/java - priority 100

$ update-alternatives --display javac
javac - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link javac is /usr/bin/javac
/usr/lib/jvm/jdk1.7.0_79/bin/javac - priority 100

$ update-alternatives --config java
(select java jdk1.7.0_79)
$ update-alternatives --config javac
Check java version

$ java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
Install Ant

$ cd ~
$ wget http://archive.apache.org/dist/ant/binaries/apache-ant-1.9.14-bin.tar.gz
$ tar xvfvz apache-ant-1.9.14-bin.tar.gz
$ mv apache-ant-1.9.14 /usr/local/
$ sh -c 'echo ANT_HOME=/usr/local/  >> /etc/environment'
$ ln -s /usr/local/apache-ant-1.9.14/bin/ant /usr/bin/ant
Verfiy ant version as follows:

$ ant -version
Apache Ant(TM) version 1.9.14 compiled on March 12 2019
Create working directories for OpenIMSCore:

$ mkdir /opt/OpenIMSCore
$ cd /opt/OpenIMSCore
Download:

$ git clone https://github.com/herlesupreeth/FHoSS
Compile:
```
```
$ cd FHoSS
$ export JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79";export CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
$ ant compile deploy | tee ant_compile_deploy.txt
```

```

```
Create configurator.sh using below script to change domain names and IP address in all configuration files

```
$ cd deploy
$ vim configurator.sh
```

```

#!/bin/bash

# Initialization & global vars
# if you execute this script for the second time
# you should change these variables to the latest
# domain name and ip address
DDOMAIN="open-ims\.test"
DSDOMAIN="open-ims\\\.test"
DEFAULTIP="127\.0\.0\.1"
CONFFILES=`ls *.cfg *.xml *.sql *.properties 2>/dev/null`

# Interaction
printf "Domain Name:"
read domainname 
printf "IP Adress:"
read ip_address

# input domain is to be slashed for cfg regexes 
slasheddomain=`echo $domainname | sed 's/\./\\\\\\\\\./g'`

  if [ $# != 0 ] 
  then 
  printf "changing: "
      for j in $* 
      do
    sed -i -e "s/$DDOMAIN/$domainname/g" $j
    sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $j
    sed -i -e "s/$DEFAULTIP/$ip_address/g" $j
    printf "$j " 
      done
  echo 
  else 
  printf "File to change [\"all\" for everything, \"exit\" to quit]:"
  # loop
      while read filename ;
      do
        if [ "$filename" = "exit" ] 
        then 
        printf "exitting...\n"
        break ;
    
      elif [ "$filename" = "all" ]
      then    
          printf "changing: "
         for i in $CONFFILES 
         do
        sed -i -e "s/$DDOMAIN/$domainname/g" $i
        sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $i
        sed -i -e "s/$DEFAULTIP/$ip_address/g" $i
        
        printf "$i " 
         done 
         echo 
         break;
    
        elif [ -w $filename ] 
        then
            printf "changing $filename \n"
            sed -i -e "s/$DDOMAIN/$domainname/g" $filename
            sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $filename
            sed -i -e "s/$DEFAULTIP/$ip_address/g" $filename
    
          else 
          printf "cannot access file $filename. skipping... \n" 
        fi
        printf "File to Change:"
      done 
  fi
```

```
$ chmod +x configurator.sh
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ grep -r "open-ims"
(Change realm name in the below file from open-ims.test to ims.mnc000.mcc460.3gppnetwork.org)
$ vim webapps/hss.web.console/WEB-INF/web.xml
$ vim hibernate.properties
```
And, change the following line:
```
hibernate.connection.url=jdbc:mysql://127.0.0.1:3306/hss_db
$ cp configurator.sh ../scripts/
$ cd ../scripts
$ grep -r "open-ims"
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ cp configurator.sh ../config/
$ cd ../config
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ cd ../src-web
$ vim WEB-INF/web.xml
```
And, change open-ims.test to ims.mnc000.mcc460.3gppnetwork.org

Prepare mysql database:
```
$ mysql
<mysql> drop database hss_db;
<mysql> create database hss_db;
<mysql> quit
```
Import database located at /opt/OpenIMSCore into hss_db
```
$ cd /opt/OpenIMSCore
$ mysql -u root -p hss_db < FHoSS/scripts/hss_db.sql
$ mysql -u root -p hss_db < FHoSS/scripts/userdata.sql
```
Check grants for mysql access rights at first time installation:
```
$ mysql
# See last line in hss_db.sql:
<mysql> grant delete,insert,select,update on hss_db.* to hss@localhost identified by 'hss';
<mysql> grant delete,insert,select,update on hss_db.* to hss@'%' identified by 'hss';
```
Check database if domain names are o.k. in various entries and privileges
```
$ mysql -u hss -p
<mysql> show databases;
<mysql> use hss_db;
<mysql> select * from impu;
```
Prepare script-file, start HSS

Copy startup.sh to hss.sh in root directory
```
$ cp /opt/OpenIMSCore/FHoSS/deploy/startup.sh /root/hss.sh
```
And, add the following to hss.sh <font color="Hotpink">before echo Building Classpath</font>
```
cd /opt/OpenIMSCore/FHoSS/deploy
JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79"
CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
```
Start HSS using hss.sh

```
$ /root/hss.sh
```
Access the web-interface of HSS: <font color="Hotpink">http://<IMS_VM_FLOATING_IP>:8080/hss.web.console/</font>

For example, http://192.168.2.258:8080/hss.web.console/
```
user:      hssAdmin
password:  hss
```

Then, edit the /etc/hosts file as follows:

In the below example. epc-ims is the hostname of the machine
```
root@epc-ims:~# cat /etc/hosts
127.0.0.1	localhost
127.0.0.1	epc-ims
```
### 20. Add IMS subscription use in FoHSS as follows from the Web GUI
Assuming IMSI of the user as 001010123456791 and MSISDN is 0198765432100

Login to the HSS web console.
Navigate to the User Identities page	
Create the IMSU 
Click IMS Subscription / Create
Enter:
Name = 001010123456791
Capabilities Set = cap_set1
Preferred S-CSCF = scsf1
Click Save

Create the IMPI and Associate the IMPI to the IMSU
Click Create & Bind new IMPI
Enter:
Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Secret Key = 8baf473f2f8fd09487cccbd7097c6862 (Ki value as in Open5GS HSS database)
Authentication Schemes - All
Default = Digest-AKAv1-MD5
AMF = 8000 (As in Open5GS HSS database)
OP = 11111111111111111111111111111111 (As in Open5GS HSS database)
SQN = 000000021090 (SQN value as in Open5GS HSS database)
Click Save

Create and Associate IMPI to IMPU
Click Create & Bind new IMPU
Enter:
Identity = sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Barring = Yes
Service Profile = default_sp
Charging-Info Set = default_charging_set
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Now, goto Public User Identity and create further IMPUs as following

1. tel:0198765432100

Public User Identity -IMPU-
Identity = tel:0198765432100
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add

2. sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org

Public User Identity -IMPU-
Identity = sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add

And, finally add these IMPUs as implicit set of IMSI derived IMPU in HSS i.e sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org as follows:

1. Goto to IMPU sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
2. In "Add IMPU(s) to Implicit-Set" section give IMPU Identity created above to be added to this IMPU

### 21. APN settings
Clear all previous APN settings

Then, create APN as follows:

First create internet APN, APN name: internet, APN type: default –> Save APN
Then, create ims APN, APN name: ims, APN type: ims –> Save APN

### 22. eNB settings

Must have in the eNB:

Support for QoS
Support for Dedicated radio bearer creation
Make sure to check the DRB configuration with respect to QCI of APN accordingly (QCI 5 for ims)
On the eNB machine have the following static routes (since internal IP of the VM is advertised in S1AP messages and UE wont find the core in Uplink)

```
ip r add 192.168.2.240/32 via 172.24.15.30
```

### 23. USIM and UE settings

Make sure to disable SQN check in Sysmocom SIM cards using sysmo-usim-tool tool https://github.com/herlesupreeth/sysmo-usim-tool
Tested with OnePlus 5 with following methods (Official Google method is the recommended method to prevent damage to phone)
(Official Google method) - Please follow the instructions in the following link @herlesupreeth/CoIMS_Wiki to force enable VoLTE using Carrier Privileges
(Risky method) With modfication to enable force IMS registration is a must or else UE will not even attempt to connect to P-CSCF. Need to apply the fix back after each update. https://forum.xda-developers.com/oneplus-5t/how-to/guide-volte-vowifi-german-carriers-t3817542

### 24. Start IMS components and FoHSS followed by Open5GS and eNB, then try connecting the phones

### 25. Test voice call

Assuming IMSI of the user1 as 001010123456791 and MSISDN is 0198765432100 and IMSI of the user2 as 001010123456792 and MSISDN is 0298765432100. Try calling user2 from user1 by dialing its MSISDN ie. 0298765432100

You can see the sample traffic. – [volte.pcapng].

### 26. For debugging

Debug using wireshark at Open5GS machine and following wireshark display filter

```
s1ap || gtpv2 || pfcp || diameter || diameter.3gpp || sip
```

Also,

Debugging Diameter messages between PCRF and P-CSCF in Wireshark if the TCP/SCTP port other than 3868

Open Wireshark –> Preferences –> Protocols –> Diameter –> Change to whatever ports are being used

Appendix
Open5GS
Sukchan Lee
acetcom@gmail.com
open5gs
Open5GS is a C-language implementation of 5G Core and EPC, i.e. the core network of NR/LTE network (Release-16)

## CentOS安装rtpengine
```
#!/bin/bash
# Install the packages required to compile RTP engine

set -e

yum install -y iptables-devel kernel-devel kernel-headers xmlrpc-c-devel

yum install -y "kernel-devel-uname-r == $(uname -r)"

## if not exist, could run yum install kernel and then reboot

yum install -y glib glib-devel gcc zlib zlib-devel openssl openssl-devel pcre pcre-devel libcurl libcurl-devel xmlrpc-c xmlrpc-c-devel

yum install -y libevent-devel glib2-devel json-glib-devel gperf libpcap-devel git hiredis hiredis-devel redis perl-IPC-Cmd

yum install MariaDB-devel MariaDB-client MariaDB-shared MariaDB-server

yum install -y spandsp-devel spandsp

yum install -y epel-release

yum install -y libwebsockets libwebsockets-devel

rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro

# rpm -Fvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm

yum -y install https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm

yum repolist

yum -y install ffmpeg ffmpeg-devel

### Get The Latest Release Of RTPEngine Source From RTPEngine’s GitHub Repository

cd /root/src
if [ -d rtpengine ]; then
    cd rtpengine
    make clean
    git pull
else
    git clone https://github.com/sipwise/rtpengine.git
fi

### Compile and install the daemon

cd /root/src/rtpengine/daemon/

export exec_prefix=/usr

make

cp rtpengine /usr/sbin/rtpengine

### Compile and install iptables extension

cd /root/src/rtpengine/iptables-extension

make all

cp libxt_RTPENGINE.so /usr/lib64/xtables/.

### Compile and install the kernel module xt_RTPENGINE

cd /root/src/rtpengine/kernel-module

make

### determine kernel release

uname -a

kernel_ver=`uname -r`

cp xt_RTPENGINE.ko /lib/modules/${kernel_ver}/extra/xt_RTPENGINE.ko

depmod -a

### load module at boot time

### Add the following lines to /etc/modules-load.d/rtpengine.conf (add the following lines)

CONF_FILE=/etc/modules-load.d/rtpengine.conf
echo '# load xt_RTPENGINE module' > ${CONF_FILE}
echo 'xt_RTPENGINE' >> ${CONF_FILE}

### load the module and check

modprobe xt_RTPENGINE
lsmod | grep xt_RTPENGINE

### check files

ls -l /proc/rtpengine/control
ls -l /proc/rtpengine/list

TableID=0

### add forwarding table with an ID=$TableID. $TableID is a number between 0-63

echo 'add 0' > /proc/rtpengine/control

### Adding iptables rules to forward the incoming packets to xt_RTPENGINE module

iptables -I INPUT -p udp -j RTPENGINE --id $TableID

ip6tables -I INPUT -p udp -j RTPENGINE --id $TableID

```
## Kamailio服务器安装配置

我们使用Kamailio主要用在SIP dispatcher server，即SIP redirect server
安装及配置手册如下

### 一．安装

#### 1．依赖包：

```
libmysqlclient & libz (zlib) ：mysql DB support (the db_mysql module) Shared libraries

```shell
MySQL-shared-5.1.32-0.glibc23.i386.rpm
MySQL-shared-5.1.32-0.glibc23.i386.rpm
MySQL-devel-community-5.1.32-0.rhel5.i386.rpm
MySQL-devel-community-5.1.32-0.rhel5.i386.rpm

libxml2：cpl-c (Call Processing Language) or the presence modules (presence and pua*)
libperl：perl scripting from you config file (perl module)
```



#### 2．源代码安装

```
make，make modules，make install
make all，make install
```

#### 3．启动：kamctl start

#### 4．重启：kamctl restart

#### 5．监控服务状态：kamctl moni

#### 6．MySQL配置：

1）安装：

```
edit Makefile.var files to include the MySQL module
vim Makefile.vars
Uncomment the next line in the file:
MODS_MYSQL=on
cp /usr/local/lib/mysql/libmysqlclient.so.16 /usr/lib

Edit now /usr/local/etc/kamailio/kamctlrc and add:
DBENGINE=MYSQL
SIP_DOMAIN=pryko.com
```

6.1 创建数据库：kamdbctl create
6.2管理员登录：user 'admin' with password ' openserrw '
6.3 添加用户：kamctl add <name> <password> <email>
6.4 默认值：database url, users and passwords

  - DEFAULT_DB_URL="mysql://opensips:opensipsrw@localhost/opensips"
  - r/w user: openser; passwd: openserrw
  - r/o user: openserro; passwd: openserro

二．配置
1．配置文件 kamailio.cfg

```
/usr/local/etc/kamailio/kamailio.cfg
```

2．配置文件 kamctlrc

```
/usr/local/etc/kamailio/kamctlrc
```

三．脚本
参考文档：
Kamailio Wiki
http://www.kamailio.com/dokuwiki
Cookbooks and Reference
http://www.kamailio.com/dokuwiki/doku.php/core-cookbook:1.5.x
Kamalio 1.5.x Module Functions Index
http://www.kamailio.com/dokuwiki/doku.php/modules:1.5.x:index-functions

四．负载均衡Load Balancing
参考：http://www.kamailio.org/dokuwiki/doku.php/asterisk:load-balancing-and-ha
4.1配置文件 kamailio.cfg

```
loadmodule("dispatcher.so")
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
modparam("dispatcher", "force_dst", 1)
```

4.2 ---dispatcher.list----文件

# group sip addresses of your * units

1 sip:221.5.152.171:5060
1 sip:221.5.152.170:5060
4.3 kamctl命令：kamctl dispatcher show
-- command 'dispatcher' - manage dispatcher

  * Examples:  dispatcher addgw 1 sip:1.2.3.1:5050 1 'outbound gateway'
  * dispatcher addgw 2 sip:1.2.3.4:5050 3 ''
  * dispatcher rmgw 4
    dispatcher show ..................... show dispatcher gateways
    dispatcher reload ................... reload dispatcher gateways
    dispatcher dump ..................... show in memory dispatcher gateways
    dispatcher addgw <setid> <destination> <flags> <description>
    .......................... add gateway
    dispatcher rmgw <id> ................ delete gateway

查看载入的配置：kamctl dispatcher dump
修改后重新载入配置：kamctl dispatcher reload

 

如需使用，需安装MySQL-client-community-5.1.32-0.rhel5.i386.rpm
否则报错：ERROR: This command requires a database engine - none was loaded

五．与Asterisk对接负载均衡
注意事项：sip.conf
注释如下行
;canreinvite=no ; Asterisk by default tries to redirect

Asterisk#1  10.10.10.56
配置sip.conf
[5000]
type=friend
;username=5000
secret=5000_phone2
callerid=5000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000@10.10.10.57,60)
exten => 5000,1,Dial(SIP/5000,60)

Asterisk#2  10.10.10.57
配置sip.conf
[6000]
type=friend
;username=6000
secret=6000_phone2
callerid=6000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000,60)
exten => 5000,1,Dial(SIP/5000@10.10.10.136,60)

Kamailio 10.10.10.136
配置kamailio.cfg
…
loadmodule "dispatcher.so"
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
…
route{
if ( !mf_process_maxfwd_header("10") )
{
  sl_send_reply("483","To Many Hops");
  drop();
};
ds_select_dst("1", "0");
forward();
}
配置dispatcher.list

# line format

# setit(integer) destination(sip uri) flags (integer, optional)

1 sip:10.10.10.56:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56

六．按号码段重定向网关
配置kamailio.cfg
使用正则表达式
route{
        if (!mf_process_maxfwd_header("10")) {
                sl_send_reply("483","Too Many Hops");
                exit;
        }
if (uri=~"^sip:5[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("1", "0");
  forward();
  exit;
  }
}
if (uri=~"^sip:8[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("2", "0");
  forward();
  exit;
  }
}
sl_send_reply("404","Not here");
exit;
}
配置dispatcher.list

# line format

# setit(integer) destination(sip uri) flags (integer, optional)

1 sip:10.10.10.56:5060 #1
2 sip:10.10.10.54:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56
从6000呼叫8002，会呼叫10.10.10.136上的8002，10.136重定向到10.54


Kamailio 3.3.0 是一个主要的版本，包含众多新特性，包括通用数据库集群，可选的 Cassandra 后端，SIP Outbound and GRUU, MSRP relaying, detection of broken active calls 以及一个嵌入式的 C# 解释器。

```
## 安装RTPProxy
```

安装依赖包:

gcc编译器大于4.9，如gcc5.4版本的安装：

yum install centos-release-scl -y
yum install devtoolset-4-toolchain -y
scl enable devtoolset-4 bash
安装rtpproxy app:

git clone -b master https://github.com/sippy/rtpproxy.git
cd rtpproxy
git -c rtpproxy submodule update --init --recursive
./configure
make
make install
启动rtpproxy:

rtpproxy -l 182.XX.10.17 -s udp:127.0.0.1 7078 -F

## 其他reference
- [最新完整关于SIP开源项目以及其衍生开源项目汇总说明](www.ctiforum.com/news/guandian/589369.html)


## kamailio

- [OpenSER(OpenSIPS/Kamailio) 和FreeSWITCH间的区别](qiusuoge.com/17742.html)
- [学习资料之Kaimailio and rtpengine安装使用](https://blog.csdn.net/weixin_41486034/article/details/106249598)

```
以前装好的kamailio sip服务器经常在启动的时候经常遇到这个错误：
ERROR: PID file /var/run/kamailio/kamailio.pid does not exist -- Kamailio start failed
经过自己尝试发现可能引起的原因是因为在使用kamctl start之前使用了kamailio 导致启动了一些进程。
使用PS 命令查看kamialio相关进程：ps axw | /usr/bin/egrep kamailio
ps -ef|grep kamailio|grep -v grep|cut -c 9-15|xargs kill -9
```

- [ubuntu上kamailio+rtpproxy+mediaproxy环境搭建](https://www.jianshu.com/p/9e2ffbf853fc)

- [kamailio docs](http://kamailio.org/docs/db-tables/kamailio-db-5.5.x.html)
- [kamailio modules](https://www.kamailio.org/docs/modules/stable/)
- [imsi和手机号码的关系](https://blog.csdn.net/qq276726581/article/details/53057078)
- [kamailio debug日志设置](https://blog.csdn.net/zhangtuo/article/details/81937791)
- [VoLTE Setup with Kamailio IMS and Open5GS](https://open5gs.org/open5gs/docs/tutorial/02-VoLTE-setup/)

## centos 7.6 安装kamailio n5记录

```
git remote set-url origin git@git.samxtec.com:bbluesnow/kamaililo.git

git remote set-url origin git@gitee.com:rnxt/vonrptt.git

https://gitee.com/rnxt/vonrptt
```
```
yum list java

yum install -y wget

yum install -y java-1.8.0-openjdk-devel.x86_64

yum groupinstall -y "Development Tools"

yum install -y json-c json-c-devel

yum install -y gcc

yum install -y bison

yum install -y flex

yum install -y MariaDB-devel MariaDB-client MariaDB-shared MariaDB-server

yum install -y libxml2-devel

yum install -y libmnl-devel

yum install -y libcurl libcurl-devel

make include_modules="db_mysql xmlrpc cdp cdp_avp ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf presence http_async_client" cfg

make all

make install

vi /usr/local/etc/kamailio/kamctlrc
change below value:
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL

then, 
kamdbctl create

$ kamdbctl create
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;

$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
No Subscribers are added yet

The kamdbctl will add two users in MySQL user tables:

- kamailio   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- kamailioro - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database

```

### Kamailio服务器安装配置
```
Kamailio是一个开源的SIP服务器，原名OpenSER 

Kamailio is an Open Source, GPL2, SIP Server Routing Platform. It is written in C for Linux/Unix plaforms and focuses on performance, flexibility and security.

Home page with new project name: http://www.kamailio.org
Home page with old project name: http://www.openser-project.org
SourceForge.net Project page: http://sourceforge.net/projects/openser/
Features

SIP proxy/registrar/redirect server (RFC3261, RFC3263)
UDP/TCP/TLS/SCTP support
Transactional stateful proxy
Modular architecture
Programmable configuration file
ENUM support
Call Processing Language (CPL)
Gateway to sms or xmpp
Authentication, authorization and accounting via Radius or database
NAT traversal system
Least cost routing
Load balancing
Carrier routing
Multiple database backends: MySQL, Postgres, Oracle, BDB or flat files
SIMPLE Presence Server (IETF SIMPLE extensions - rich presence)
Dialog Info Presence - SLA/BLA
XCAP and RLS
Presence User Agent
Dialog Stateful Proxy
Instant Messaging
Offline message storage
Instant messaging conferencing
SNMP support
Perl Programming Interface
Java SIP Servlet Application server
Over 80 modules (extensions)
Documentation

Main Documentation Page - http://www.kamailio.org/docs/
Dokuwiki Page - http://www.kamailio.org/dokuwiki/
我们使用Kamailio主要用在SIP dispatcher server，即SIP redirect server
```

#### 安装及配置手册如下

#### 一. 安装
- 1．依赖包：
```
libmysqlclient & libz (zlib) ：mysql DB support (the db_mysql module) Shared libraries
```

```shell
MySQL-shared-5.1.32-0.glibc23.i386.rpm
                        MySQL-shared-5.1.32-0.glibc23.i386.rpm
    
                        MySQL-devel-community-5.1.32-0.rhel5.i386.rpm

MySQL-devel-community-5.1.32-0.rhel5.i386.rpm
```

```
libxml2：cpl-c (Call Processing Language) or the presence modules (presence and pua*)
libperl：perl scripting from you config file (perl module)
```

- 2．源代码安装
```
make，make modules，make install
或者make all，make install
```
- 3．启动：kamctl start
- 4．重启：kamctl restart
- 5．监控服务状态：kamctl moni
- 6．MySQL配置：
- - 1）安装：
```
edit Makefile.var files to include the MySQL module
vim Makefile.vars
Uncomment the next line in the file:
MODS_MYSQL=on
cp /usr/local/lib/mysql/libmysqlclient.so.16 /usr/lib

Edit now /usr/local/etc/kamailio/kamctlrc and add:
DBENGINE=MYSQL
SIP_DOMAIN=pryko.com
6.1 创建数据库：kamdbctl create
6.2管理员登录：user 'admin' with password ' openserrw '
6.3 添加用户：kamctl add <name> <password> <email>
6.4 默认值：database url, users and passwords
  - DEFAULT_DB_URL="mysql://opensips:opensipsrw@localhost/opensips"
  - r/w user: openser; passwd: openserrw
  - r/o user: openserro; passwd: openserro
```
- - 二．配置

- - - 1．配置文件 kamailio.cfg
```
/usr/local/etc/kamailio/kamailio.cfg
```
- - - 2．配置文件 kamctlrc
```
/usr/local/etc/kamailio/kamctlrc
```

参考文档：
- [Kamailio Wiki](http://www.kamailio.com/dokuwiki)
- [Cookbooks and Reference](http://www.kamailio.com/dokuwiki/doku.php/core-cookbook:1.5.x)
- [Kamalio 1.5.x Module Functions Index](http://www.kamailio.com/dokuwiki/doku.php/modules:1.5.x:index-functions)

四．负载均衡Load Balancing

- 参考：http://www.kamailio.org/dokuwiki/doku.php/asterisk:load-balancing-and-ha

- 4.1配置文件 kamailio.cfg
loadmodule("dispatcher.so")
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
modparam("dispatcher", "force_dst", 1)
- 4.2 ---dispatcher.list----文件
```
# group sip addresses of your * units
1 sip:221.5.152.171:5060
1 sip:221.5.152.170:5060
```
- - 4.3 kamctl命令：kamctl dispatcher show
```
-- command 'dispatcher' - manage dispatcher
  * Examples:  dispatcher addgw 1 sip:1.2.3.1:5050 1 'outbound gateway'
  *            dispatcher addgw 2 sip:1.2.3.4:5050 3 ''
  *            dispatcher rmgw 4
dispatcher show ..................... show dispatcher gateways
dispatcher reload ................... reload dispatcher gateways
dispatcher dump ..................... show in memory dispatcher gateways
dispatcher addgw <setid> <destination> <flags> <description>
            .......................... add gateway
dispatcher rmgw <id> ................ delete gateway
```
- 查看载入的配置：
```kamctl dispatcher dump```
- 修改后重新载入配置：```kamctl dispatcher reload```

- 如需使用，需安装MySQL-client-community-5.1.32-0.rhel5.i386.rpm, 否则报错：ERROR: This command requires a database engine - none was loaded

五．与Asterisk对接负载均衡
```
注意事项：sip.conf
注释如下行
;canreinvite=no ; Asterisk by default tries to redirect

Asterisk#1  10.10.10.56
配置sip.conf
[5000]
type=friend
;username=5000
secret=5000_phone2
callerid=5000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000@10.10.10.57,60)
exten => 5000,1,Dial(SIP/5000,60)

Asterisk#2  10.10.10.57
配置sip.conf
[6000]
type=friend
;username=6000
secret=6000_phone2
callerid=6000
qualify=yes ; Qualify peer is no more than 2000 ms away
nat=no ; This phone is natted
host=dynamic ; This device registers with us
;canreinvite=no ; Asterisk by default tries to redirect
配置extension.conf
[default]
exten => 6000,1,Dial(SIP/6000,60)
exten => 5000,1,Dial(SIP/5000@10.10.10.136,60)

Kamailio 10.10.10.136
配置kamailio.cfg
…
loadmodule "dispatcher.so"
modparam("dispatcher", "list_file", "/usr/local/etc/kamailio/dispatcher.list")
…
route{
if ( !mf_process_maxfwd_header("10") )
{
  sl_send_reply("483","To Many Hops");
  drop();
};
ds_select_dst("1", "0");
forward();
}
配置dispatcher.list
# line format
# setit(integer) destination(sip uri) flags (integer, optional)
1 sip:10.10.10.56:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56
```
六．按号码段重定向网关
```
配置kamailio.cfg
使用正则表达式
route{
        if (!mf_process_maxfwd_header("10")) {
                sl_send_reply("483","Too Many Hops");
                exit;
        }
if (uri=~"^sip:5[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("1", "0");
  forward();
  exit;
  }
}
if (uri=~"^sip:8[0-9]+@10.10.10.136$") {
  if (is_method("INVITE")) {
  ds_select_dst("2", "0");
  forward();
  exit;
  }
}
sl_send_reply("404","Not here");
exit;
}
配置dispatcher.list
# line format
# setit(integer) destination(sip uri) flags (integer, optional)
1 sip:10.10.10.56:5060 #1
2 sip:10.10.10.54:5060

测试：
登录10.10.10.57上的6000，登录10.10.10.56上的5000
从6000呼叫5000，会呼叫10.10.10.136上的5000，10.136重定向到10.56
从6000呼叫8002，会呼叫10.10.10.136上的8002，10.136重定向到10.54


Kamailio 3.3.0 是一个主要的版本，包含众多新特性，包括通用数据库集群，可选的 Cassandra 后端，SIP Outbound and GRUU, MSRP relaying, detection of broken active calls 以及一个嵌入式的 C# 解释器。
```
## 安装RTPProxy
```
安装依赖包:

gcc编译器大于4.9，如gcc5.4版本的安装：

yum install centos-release-scl -y
yum install devtoolset-4-toolchain -y
scl enable devtoolset-4 bash
安装rtpproxy app:

git clone -b master https://github.com/sippy/rtpproxy.git
cd rtpproxy
git -c rtpproxy submodule update --init --recursive
./configure
make
make install
启动rtpproxy:

rtpproxy -l 182.XX.10.17 -s udp:127.0.0.1 7078 -F 
```

## VoLTE Setup with Kamailio IMS and Open5GS

Setup description:

+ MCC: 001, MNC: 01
+ Single OpenStack VM with Kamailio IMS and Open5GS (Internal IP 10.4.128.21 and Floating IP 172.24.15.30)
+ 4G Casa Smallcell
+ Sysmocom USIM - sysmoUSIM-SJS1
+ Oneplus 5 as UE
```
OpenStack is not required for VoLTE setup. If you are using one machine without OpenStack, you can start from step 3.
```

### 1. Start from Bionic Ubuntu cloud image

### 2. Use the following Cloud init config while spawning an instance
```
#cloud-config
disable_root: 0
ssh_pwauth: True
users:
  - name: root
chpasswd:
    list: |
    root:admin
    expire: False
runcmd:
  - sed -i -e '/^#PermitRootLogin/s/^.*$/PermitRootLogin yes/' /etc/ssh/sshd_config
  - systemctl restart sshd
```
```
This removes all existing cloud users and allows only root user and sets a password
```
### 3. Install following packages
+ ubuntu/Debian
```
$ apt update && apt upgrade -y && apt install -y mysql-server tcpdump screen ntp ntpdate git-core dkms gcc flex bison libmysqlclient-dev make \
libssl-dev libcurl4-openssl-dev libxml2-dev libpcre3-dev bash-completion g++ autoconf rtpproxy libmnl-dev libsctp-dev ipsec-tools libradcli-dev \
libradcli4
```
+ CentOS

### 4. Clone Kamailio repository and checkout 5.3 version of repository
```
$ mkdir -p /usr/local/src/
$ cd /usr/local/src/
$ git clone https://github.com/herlesupreeth/kamailio
$ cd kamailio
$ git checkout -b 5.3 origin/5.3
```
### 5. Generate build config files
```
$ cd /usr/local/src/kamailio
$ make cfg
```
### 6. Enable MySQL module and all required IMS modules.
```
Edit modules.lst file present at /usr/local/src/kamailio/src
```
The contents of modules.lst should be as follows:
```
# this file is autogenerated by make modules-cfg

# the list of sub-directories with modules
modules_dirs:=modules

# the list of module groups to compile
cfg_group_include=

# the list of extra modules to compile
include_modules= cdp cdp_avp db_mysql dialplan ims_auth ims_charging ims_dialog ims_diameter_server ims_icscf ims_ipsec_pcscf ims_isc ims_ocs ims_qos ims_registrar_pcscf ims_registrar_scscf ims_usrloc_pcscf ims_usrloc_scscf outbound presence presence_conference presence_dialoginfo presence_mwi presence_profile presence_reginfo presence_xml pua pua_bla pua_dialoginfo pua_reginfo pua_rpc pua_usrloc pua_xmpp sctp tls utils xcap_client xcap_server xmlops xmlrpc

# the list of static modules
static_modules=

# the list of modules to skip from compile list
skip_modules=

# the list of modules to exclude from compile list
exclude_modules= acc_json acc_radius app_java app_lua app_lua_sr app_mono app_perl app_python app_python3 app_ruby auth_ephemeral auth_identity auth_radius cnxcc cplc crypto db2_ldap db_berkeley db_cassandra db_mongodb db_oracle db_perlvdb db_postgres db_redis db_sqlite db_unixodbc dnssec erlang evapi geoip geoip2 gzcompress h350 http_async_client http_client jansson janssonrpcc json jsonrpcc kafka kazoo lcr ldap log_systemd lost memcached misc_radius ndb_cassandra ndb_mongodb ndb_redis nsq osp peering phonenum pua_json rabbitmq regex rls rtp_media_server snmpstats systemdops topos_redis uuid websocket xhttp_pi xmpp $(skip_modules)

modules_all= $(filter-out modules/CVS,$(wildcard modules/*))
modules_noinc= $(filter-out $(addprefix modules/, $(exclude_modules) $(static_modules)), $(modules_all)) 
modules= $(filter-out $(modules_noinc), $(addprefix modules/, $(include_modules) )) $(modules_noinc) 
modules_configured:=1
```

### 7. Compile and install Kamailio
```
$ cd /usr/local/src/kamailio
$ export RADCLI=1
$ make Q=0 all | tee make_all.txt
$ make install | tee make_install.txt
$ ldconfig
```
The binaries and executable scripts are installed in: /usr/local/sbin
```
kamailio - Kamailio SIP server
kamdbctl - script to create and manage the Databases
kamctl - script to manage and control Kamailio SIP server
kamcmd - CLI - command line tool to interface with Kamailio SIP server
```
To be able to use the binaries from command line, make sure that /usr/local/sbin is set in PATH environment variable. You can check that with echo $PATH. If not and you are using bash, open /root/.bash_profile and at the end add:

```
  PATH=$PATH:/usr/local/sbin
  export PATH
```
Kamailio modules are installed at: <font color="Hotpink">/usr/local/lib64/kamailio/modules</font>

The documentation and readme files are installed at: <font color="Hotpink">/usr/local/share/doc/kamailio</font>

The configuration files are installed at: <font color="Hotpink">/usr/local/etc/kamailio</font>

In case you set the PREFIX variable in <font color="Hotpink">make cfg</font> command, then replace <font color="Hotpink">/usr/local</font> in all paths above with the value of PREFIX in order to locate the files installed.

### 8. Populate MySQL database using kamctlrc command

Edit SIP_DOMAIN and DBENGINE in the <font color="Hotpink">/usr/local/etc/kamailio/kamctlrc</font> configuration file (Used by kamctl and kamdbctl tools).

Set the SIP_DOMAIN to your SIP service domain (or IP address if you don’t have a DNS hostname associated with your SIP service). Set the DBENGINE to be MYSQL and adjust other setting as you want. Finally, uncomment both SIP_DOMAIN and DBENGINE.

In example above, the following values are set for SIP_DOMAIN and DBENGINE

```
SIP_DOMAIN=ims.mnc000.mcc460.3gppnetwork.org
DBENGINE=MYSQL
```
You can change other values in kamctlrc file. Once you are done updating kamctlrc file, run the script to create the database used by Kamailio:
```
$ kamdbctl create
```
When prompted for mysql root user password enter the root password if its is set or else leave it blank i.e. Press Enter

check database manually;
```
$ mysql
<mysql> show databases;
<mysql> use kamailio;
<mysql> show tables;
<mysql> select * from subscriber;
```
No Subscribers are added yet

The kamdbctl will add two users in MySQL user tables:
```
- kamailio   - (with default password 'kamailiorw') - user which has full access rights to 'kamailio' database
- kamailioro - (with default password 'kamailioro') - user which has read-only access rights to 'kamailio' database
```
### 9. 安装rtp proxy
1. 安装依赖包:

```
   yum groupinstall "Development Tools"
   yum install glibc-static libstdc++-static
   ```

   gcc编译器大于4.9，如gcc5.4版本的安装： 升级gcc, gcc-v可以查看当前版本

   + [Centos7升级gcc版本方法之一使用scl软件集](https://blog.csdn.net/helpmsg/article/details/105876127)
   + [CentOS 7下升级gcc版本](https://blog.csdn.net/ncdx111/article/details/106047228)

   ```
   yum install centos-release-scl scl-utils-build
   yum list all --enablerepo='centos-sclo-rh' | grep devtoolset-.*
   可以根据grep出来的结果选择安装最高版本，其中下面命令中devtoolset-4 代表版本gcc 4.x
   yum install centos-release-scl -y
   yum install devtoolset-4-toolchain -y
   scl enable devtoolset-4 bash
   ```

2. 安装rtpproxy app:

- 可以参照这个连接进行安装详细访问[Install RTPProxy from source on Ubuntu 20.04/18.04/16.04](https://computingforgeeks.com/how-to-install-rtpproxy-from-source-on-ubuntu-linux/)

```
git clone -b master https://github.com/sippy/rtpproxy.git

cd rtpproxy

git -c rtpproxy submodule update --init --recursive

./configure

make 

make install

启动rtpproxy:

rtpproxy -l 182.XX.10.17 -s udp:127.0.0.1 7078 -F 
```

### 9. Edit /etc/default/rtpproxy file as follows

```
# Defaults for rtpproxy

# The control socket.
#CONTROL_SOCK="unix:/var/run/rtpproxy/rtpproxy.sock"
# To listen on an UDP socket, uncomment this line:
#CONTROL_SOCK=udp:127.0.0.1:22222
CONTROL_SOCK=udp:127.0.0.1:7722

# Additional options that are passed to the daemon.
EXTRA_OPTS="-l 192.168.2.240 -d DBUG:LOG_LOCAL0"
```
here, <font color="Hotpink">-l <PUBLIC_IP></font>

Then run,
```
$ systemctl restart rtpproxy
```
### 10. Edit configuration file to fit your requirements for the VoIP platform:

You have to edit the /usr/local/etc/kamailio/kamailio.cfg configuration file.

Follow the instruction in the comments to enable usage of MySQL. Basically you have to add several lines at the top of config file, like:
```
#!define WITH_MYSQL
#!define WITH_AUTH
#!define WITH_USRLOCDB
#!define WITH_NAT

(uncomment this line)
auto_aliases=no

(uncomment this line and enter the DNS domain created above)
alias="ims.mnc000.mcc460.3gppnetwork.org"

(uncomment this line, 192.168.2.240 is the internal IP and 172.24.15.30 is the Public/Floating IP)
listen=udp:192.168.2.240:5060 advertise 172.24.15.30:5060
listen=tcp:192.168.2.240:5060 advertise 172.24.15.30:5060

(Further down, we will need to modify the rtpproxy_sock value to match the CONTROL_SOCK option we set for RTPProxy in /etc/default/rtpproxy)
modparam("rtpproxy", "rtpproxy_sock", "udp:127.0.0.1:7722")
```
If you changed the password for the ‘kamailio’ user of MySQL, you have to update the value for ‘DBURL’ parameters.

### 11. The init.d script

The init.d script can be used to start/stop the Kamailio server in a nicer way. A sample of init.d script for Kamailio is provided at:
```
/usr/local/src/kamailio/pkg/kamailio/deb/debian/kamailio.init
```
Just copy the init file into the /etc/init.d/kamailio. Then change the permissions:
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.init /etc/init.d/kamailio
$ chmod 755 /etc/init.d/kamailio
```
Then edit the /etc/init.d/kamailio file updating the $DAEMON and $CFGFILE values:
```
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin
DAEMON=/usr/local/sbin/kamailio
CFGFILE=/usr/local/etc/kamailio/kamailio.cfg
```
You need to setup a configuration file in the /etc/default/ directory. This file can be found at:

```
/usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default
```
You need to rename the /etc/default/kamailio file to ‘kamailio’ after you’ve copied it. Then edit this file and set RUN_KAMAILIO=yes. Edit the other options as per your setup.
```
$ cp /usr/local/src/kamailio/pkg/kamailio/deb/bionic/kamailio.default /etc/default/kamailio
$ systemctl daemon-reload
```
Create the directory for pid file:
```
$ mkdir -p /var/run/kamailio
```
Default setting is to run Kamailio as user kamailio and group kamailio. For that you need to create the user and set ownership
```
$ adduser --quiet --system --group --disabled-password \
        --shell /bin/false --gecos "Kamailio" \
        --home /var/run/kamailio kamailio
$ chown kamailio:kamailio /var/run/kamailio
```
Then you can start Kamailio using the following commands:
```
$ systemctl start kamailio.service
```
check running processes with: ps axw egrep kamailio

### 12. A quick check for the basic working of SIP server can be done as follows

Create new subscriber accounts. A new account can be added using kamctl tool via kamctl add <username> <password> (When asked for entering MySQL password for user kamailio@localhost: type kamailiorw, as provided in kamailio.cfg)
```
$ kamctl add test testpasswd
$ kamctl add test2 testpasswd
Setting on OnePlus phones
```
Connect to a network through which SIP server is reachable (either Wi-Fi or LTE)
Goto phone dialer and select the Settings in the menu on top right corner
Then select Call settings
Configure SIP accounts in phones as added above using kamctl:
In Phone 1:
```
Username: test
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
In Phone 2:
```
Username: test2
Password: testpasswd
Server: ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Optional Settings:
	Authentication username: test2
	Outbound proxy address: 172.24.15.30 (Floating IP of VM in case of OpenStack or else no need to fill in case of physical machine)
	Transport type: UDP
```
+ + Set “Receive incoming calls” option to enabled state in both phones
+ + Set “Use SIP calling” to “For all calls”
+ + Add a new contact as follows:

In Phone 1:

Select more option
```
Name: SIP Contact test2 (Any arbitary name)
SIP: test2@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
In Phone 2:

Select more option
```
Name: SIP Contact test (Any arbitary name)
SIP: test@ims.mnc000.mcc460.3gppnetwork.org (Created DNS Domain Name or IP to which IMS components are bound to, visible interface IP address)
Save and exit
```
Now try calling from either phone
```
Upon completion of this test, set “Receive incoming calls” option to disabled state and set “Use SIP calling” to “Only for SIP calls”
```
### 13. Create new mysql database for pcscf, scscf and icscf, populate databases and grant permissions to respective users identified by a password
```
$ mysql
<mysql> CREATE DATABASE  `pcscf`;
<mysql> CREATE DATABASE  `scscf`;
<mysql> CREATE DATABASE  `icscf`;
```
In all of the below steps, when prompted for mysql root user password, leave it blank i.e. Press Enter
```
$ cd /usr/local/src/kamailio/utils/kamctl/mysql
$ mysql -u root -p pcscf < standard-create.sql
$ mysql -u root -p pcscf < presence-create.sql
$ mysql -u root -p pcscf < ims_usrloc_pcscf-create.sql
$ mysql -u root -p pcscf < ims_dialog-create.sql

$ mysql -u root -p scscf < standard-create.sql
$ mysql -u root -p scscf < presence-create.sql
$ mysql -u root -p scscf < ims_usrloc_scscf-create.sql
$ mysql -u root -p scscf < ims_dialog-create.sql
$ mysql -u root -p scscf < ims_charging-create.sql

$ cd /usr/local/src/kamailio/misc/examples/ims/icscf
$ mysql -u root -p icscf < icscf.sql
```

Verify that following tables are present in respective databases by logging into mysql
```
+-----------------+
| Tables_in_pcscf |
+-----------------+
| active_watchers |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| location        |
| presentity      |
| pua             |
| version         |
| watchers        |
| xcap            |
+-----------------+

+-----------------+
| Tables_in_scscf |
+-----------------+
| active_watchers |
| contact         |
| dialog_in       |
| dialog_out      |
| dialog_vars     |
| impu            |
| impu_contact    |
| impu_subscriber |
| presentity      |
| pua             |
| ro_session      |
| subscriber      |
| version         |
| watchers        |
| xcap            |
+-----------------+

+---------------------+
| Tables_in_icscf     |
+---------------------+
| nds_trusted_domains |
| s_cscf              |
| s_cscf_capabilities |
+---------------------+

<mysql> grant delete,insert,select,update on pcscf.* to pcscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on scscf.* to scscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to icscf@localhost identified by 'heslo';
<mysql> grant delete,insert,select,update on icscf.* to provisioning@localhost identified by 'provi';
<mysql> GRANT ALL PRIVILEGES ON pcscf.* TO 'pcscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON scscf.* TO 'scscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'icscf'@'%' identified by 'heslo';
<mysql> GRANT ALL PRIVILEGES ON icscf.* TO 'provisioning'@'%' identified by 'provi';
<mysql> FLUSH PRIVILEGES;
```
Then,
```
$ mysql
<mysql> use icscf;
<mysql> INSERT INTO `nds_trusted_domains` VALUES (1,'ims.mnc000.mcc460.3gppnetwork.org');
<mysql> INSERT INTO `s_cscf` VALUES (1,'First and only S-CSCF','sip:scscf.ims.mnc000.mcc460.3gppnetwork.org:6060');
<mysql> INSERT INTO `s_cscf_capabilities` VALUES (1,1,0),(2,1,1);
```
### 14. Copy pcscf, icscf and scscf configuration files to /etc folder and edit accordingly
一定要以这几个文件为base做改动，不要用源代码中misc目录中的文件
```
$ cd ~ && git clone https://github.com/herlesupreeth/Kamailio_IMS_Config
$ cd Kamailio_IMS_Config
$ cp -r kamailio_icscf /etc
$ cp -r kamailio_pcscf /etc
$ cp -r kamailio_scscf /etc
```
### 15. Setup the DNS for resolving IMS and EPC components names
```
$ apt install -y bind9
```
Use the below example DNS Zone file to create a DNS Zone file into the bind folder and edit /etc/named/named.conf.local and /etc/named/named.conf.options accordingly:
```
$ cd /etc/bind
```
In the below example: Kamailio IMS & DNS server running at 192.168.2.240/172.24.15.30 (Floating IP) and PCRF also at 192.168.2.240/172.24.15.30 (Floating IP)
```
$ cat ims.mnc000.mcc460.3gppnetwork.org
```
```
$ORIGIN ims.mnc000.mcc460.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        ns
ns                      1D IN A         192.168.2.240

pcscf                   1D IN A         192.168.2.240
_sip._udp.pcscf         1D SRV 0 0 5060 pcscf
_sip._tcp.pcscf         1D SRV 0 0 5060 pcscf

icscf                   1D IN A         192.168.2.240
_sip._udp               1D SRV 0 0 4060 icscf
_sip._tcp               1D SRV 0 0 4060 icscf

scscf                   1D IN A         192.168.2.240
_sip._udp.scscf         1D SRV 0 0 6060 scscf
_sip._tcp.scscf         1D SRV 0 0 6060 scscf

hss                     1D IN A         192.168.2.240
Create another DNS zone for resolving pcrf domain as follows:
```
```
$ cat epc.mnc001.mcc001.3gppnetwork.org
```
```
$ORIGIN epc.mnc001.mcc001.3gppnetwork.org.
$TTL 1W
@                       1D IN SOA       localhost. root.localhost. (
                                        1               ; serial
                                        3H              ; refresh
                                        15M             ; retry
                                        1W              ; expiry
                                        1D )            ; minimum

                        1D IN NS        epcns
epcns                   1D IN A         192.168.2.240

pcf                    1D IN A         127.0.0.1
```
Edit /etc/named/named.conf.local file as follows:
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/named/zones.rfc1918";

zone "ims.mnc000.mcc460.3gppnetwork.org" {
        type master;
        file "/etc/named/ims.mnc000.mcc460.3gppnetwork.org";
};

zone "epc.mnc001.mcc001.3gppnetwork.org" {
        type master;
        file "/etc/named/epc.mnc001.mcc001.3gppnetwork.org";
};
```
Edit /etc/named/named.conf.options file as follows:
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
    
        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.
    
        //forwarders {
    	// Put here the IP address of other DNS server which could be used if name cannot be resolved with DNS server running in this machine (Optional)
    	//10.4.128.2;
        //};
    
        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation no;
        allow-query { any; };
    
        auth-nxdomain no;    # conform to RFC1035
        //listen-on-v6 { any; };
};
```
```
cp /etc/named/epc.mnc000.mcc460.3gppnetwork.org /var/named/
cp /etc/named/ims.mnc000.mcc460.3gppnetwork.org /var/named/
```
```
/etc/bind
[root@localhost bind]# ls
bind  epc.mnc000.mcc460.3gppnetwork.org  ims.mnc000.mcc460.3gppnetwork.org  named.conf.local  named.conf.options  pcf.mnc000.mcc460.3gppnetwork.org
[root@localhost bind]#

[root@localhost named]# ls
chroot      data     dyndb-ldap                         ims.mnc000.mcc460.3gppnetwork.org  named.empty      named.loopback
chroot_sdb  dynamic  epc.mnc000.mcc460.3gppnetwork.org  named.ca                           named.localhost  slaves
[root@localhost named]#
$ systemctl restart named
```
Then, test DNS resolution by adding following entries on top of all other entries in /etc/resolv.conf (make sure it persist across reboots)
```
search ims.mnc000.mcc460.3gppnetwork.org
nameserver 192.168.40.53
```
安装dig
```
yum -y install bind-utils
```
Finally, ping to ensure

```
$ ping pcscf
PING pcscf.ims.mnc000.mcc460.3gppnetwork.org (192.168.2.240) 56(84) bytes of data.
64 bytes from localhost (192.168.2.240): icmp_seq=1 ttl=64 time=0.017 ms
64 bytes from localhost (192.168.2.240): icmp_seq=2 ttl=64 time=0.041 ms
```
To make changes in /etc/resolv.conf be persistent across reboot edit the /etc/netplan/50-cloud-init.yaml file as follows:
```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        ens3:
            dhcp4: true
            match:
                macaddress: fa:16:3e:99:f5:67
            set-name: ens3
            nameservers:
                search: [ims.mnc000.mcc460.3gppnetwork.org,epc.mnc001.mcc001.3gppnetwork.org]
                addresses:
                      - 192.168.2.240
    version: 2
$ netplan apply
$ ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
$ systemctl restart systemd-resolved.service
```
16. Install RTPEngine
```
Check for dependencies, install dependencies and build .deb packages

$ export DEB_BUILD_PROFILES="pkg.ngcp-rtpengine.nobcg729"
$ apt install dpkg-dev
$ git clone https://github.com/sipwise/rtpengine
$ cd rtpengine && git checkout mr7.4.1
$ dpkg-checkbuilddeps
The above command checks for dependencies and give you a list of dependencies which are missing in the system. The below list is the result of this command

$ apt install debhelper default-libmysqlclient-dev gperf iptables-dev libavcodec-dev libavfilter-dev libavformat-dev libavutil-dev libbencode-perl libcrypt-openssl-rsa-perl libcrypt-rijndael-perl libdigest-crc-perl libdigest-hmac-perl libevent-dev libhiredis-dev libio-multiplex-perl libio-socket-inet6-perl libiptc-dev libjson-glib-dev libnet-interface-perl libpcap0.8-dev libsocket6-perl libspandsp-dev libswresample-dev libsystemd-dev libxmlrpc-core-c3-dev markdown dkms module-assistant keyutils libnfsidmap2 libtirpc1 nfs-common rpcbind
After installing dependencies run the below command again and verify that no dependencies are left out

$ dpkg-checkbuilddeps
This should just return back to shell with no output if all depedencies are met

$ dpkg-buildpackage -uc -us
$ cd ..
$ dpkg -i *.deb
$ cp /etc/rtpengine/rtpengine.sample.conf /etc/rtpengine/rtpengine.conf
Edit this file as follows under [rtpengine]:

interface = 192.168.2.240
Port on which rtpengine binds i.e. listen_ng parameter is udp port 2223. This should be updated in kamailio_pcscf.cfg file at modparam(rtpengine …)

# ----- rtpproxy params -----
modparam("rtpengine", "rtpengine_sock", "1 == udp:localhost:2223")
Edit /etc/default/ngcp-rtpengine-daemon and /etc/default/ngcp-rtpengine-recording-daemon as follows in respective files:

RUN_RTPENGINE=yes
RUN_RTPENGINE_RECORDING=yes
$ cp /etc/rtpengine/rtpengine-recording.sample.conf /etc/rtpengine/rtpengine-recording.conf
$ mkdir /var/spool/rtpengine
$ systemctl restart ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service
$ systemctl enable ngcp-rtpengine-daemon.service ngcp-rtpengine-recording-daemon.service ngcp-rtpengine-recording-nfs-mount.service

$ systemctl stop rtpproxy
$ systemctl disable rtpproxy
$ systemctl mask rtpproxy
Second instance of RTPENGINE can be run as follows (Optional)

$ iptables -I rtpengine -p udp -j RTPENGINE --id 1
$ ip6tables -I INPUT -p udp -j RTPENGINE --id 1
$ echo 'del 1' > /proc/rtpengine/control
$ /usr/sbin/rtpengine --table=1 --interface=192.168.2.240 --listen-ng=127.0.0.1:2224 --tos=184 --pidfile=ngcp-rtpengine-daemon2.pid --no-fallback --foreground
```
17. Running I-CSCF, P-CSCF and S-CSCF as separate process

First, stop the default kamailio SIP server
```
$ systemctl stop kamailio
$ systemctl disable kamailio
$ systemctl mask kamailio
Run all the process as root and NOT sudo

$ mkdir -p /var/run/kamailio_pcscf
$ kamailio -f /etc/kamailio_pcscf/kamailio_pcscf.cfg -P /kamailio_pcscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_scscf
$ kamailio -f /etc/kamailio_scscf/kamailio_scscf.cfg -P /kamailio_scscf.pid -DD -E -e
$ mkdir -p /var/run/kamailio_icscf
$ kamailio -f /etc/kamailio_icscf/kamailio_icscf.cfg -P /kamailio_icscf.pid -DD -E -e
```
18. Install Open5GS in the same machine as Kamailio IMS - Install Open5GS from source
- Please refer to instructions at https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/

If you are using OpenStack, installing Open5GS and Kamailio IMS on the same machine is very important because the Framed-IP-Address in the AAR request via Rx interface takes received IP address and port in ims_qos module, hence, if the Open5GS is on a separate VM/machine, the IP and port received in received_ip and received_port values seen by Kamailio IMS will be the NATed IP of the Open5GS machine resulting in failing of AAR request.

Modify below mentioned parts of configuration files in addition to Configure Open5GS section. For reference, look at the configuration files at https://github.com/herlesupreeth/Open5gs_Config. These configuration only holds for open5gs tag v1.3.0, please tweak configuration files based on the open5gs tag you use.

Change realm of components to epc.mnc001.mcc001.3gppnetwork.org
Define IP pools for APNs used i.e one for default APN and another for IMS apn
Define P-CSCF address in the pgw configuration
Define a ConnectPeer for pcscf.ims.mnc000.mcc460.3gppnetwork.org with its IP and port in PCRF freediameter configuration
Setup IP tables for the UE pools defined and create appropriate tun interfaces
Below startup script can be used for setting up interfaces:
```
#!/bin/bash

sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl -w net.ipv6.conf.all.forwarding=1

ip tuntap add name ogstun mode tun
ip addr add 192.168.100.1/24 dev ogstun
ip addr add fd84:6aea:c36e:2b69::/48 dev ogstun
ip link set ogstun mtu 1400
ip link set ogstun up
iptables -t nat -A POSTROUTING -s 192.168.100.0/24 ! -o ogstun -j MASQUERADE
ip6tables -t nat -A POSTROUTING -s fd84:6aea:c36e:2b69::/48 ! -o ogstun -j MASQUERADE
iptables -I INPUT -i ogstun -j ACCEPT
ip6tables -I INPUT -i ogstun -j ACCEPT

ip tuntap add name ogstun2 mode tun
ip addr add 192.168.101.1/24 dev ogstun2
ip addr add fd1f:76f3:da9b:0101::/48 dev ogstun2
ip link set ogstun2 mtu 1400
ip link set ogstun2 up
iptables -I INPUT -i ogstun2 -j ACCEPT
ip6tables -I INPUT -i ogstun2 -j ACCEPT
```
Add users with following APN settings in Open5GS:
```
APN Configuration:
---------------------------------------------------------------------------------------------------------------------
| APN      | Type | QCI | ARP | Capability | Vulnerablility | MBR DL/UL(Kbps)     | GBR DL/UL(Kbps) | PGW IP        |
---------------------------------------------------------------------------------------------------------------------
| internet | IPv4 | 9   | 8   | Disabled   | Disabled       | unlimited/unlimited |                 |               |
---------------------------------------------------------------------------------------------------------------------
| ims      | IPv4 | 5   | 1   | Disabled   | Disabled       | 3850/1530           |                 |               |
|          |      | 1   | 2   | Enabled    | Enabled        | 128/128             | 128/128         |               |
|          |      | 2   | 4   | Enabled    | Enabled        | 128/128             | 128/128         |               |
---------------------------------------------------------------------------------------------------------------------
```
Finally, make sure of the following in Open5GS

PCO options which indicate the address of the Proxy-CSCF
Need to indicate support for Voice-over-Packet-Switched (VoPS) in NAS message to UE from EPC

19. Setup FoHSS in order to talk with I-CSCF and S-CSCF

Requirements for FoHSS: Install Java JDK and ant

Download Oracle Java 7 JDK from following link using a browser:
```
https://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html
$ mkdir -p  /usr/lib/jvm/
$ tar -zxf jdk-7u79-linux-x64.tar.gz -C /usr/lib/jvm/
$ update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_79/bin/java 100
$ update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_79/bin/javac 100
Verify that java has been successfully configured by running:

$ update-alternatives --display java
java - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/java
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/java
  link java is /usr/bin/java
/usr/lib/jvm/jdk1.7.0_79/bin/java - priority 100

$ update-alternatives --display javac
javac - auto mode
  link best version is /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link currently points to /usr/lib/jvm/jdk1.7.0_79/bin/javac
  link javac is /usr/bin/javac
/usr/lib/jvm/jdk1.7.0_79/bin/javac - priority 100

$ update-alternatives --config java
(select java jdk1.7.0_79)
$ update-alternatives --config javac
Check java version

$ java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
Install Ant

$ cd ~
$ wget http://archive.apache.org/dist/ant/binaries/apache-ant-1.9.14-bin.tar.gz
$ tar xvfvz apache-ant-1.9.14-bin.tar.gz
$ mv apache-ant-1.9.14 /usr/local/
$ sh -c 'echo ANT_HOME=/usr/local/  >> /etc/environment'
$ ln -s /usr/local/apache-ant-1.9.14/bin/ant /usr/bin/ant
Verfiy ant version as follows:

$ ant -version
Apache Ant(TM) version 1.9.14 compiled on March 12 2019
Create working directories for OpenIMSCore:

$ mkdir /opt/OpenIMSCore
$ cd /opt/OpenIMSCore
Download:

$ git clone https://github.com/herlesupreeth/FHoSS
Compile:

$ cd FHoSS
$ export JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79";export CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
$ ant compile deploy | tee ant_compile_deploy.txt
```
Create configurator.sh using below script to change domain names and IP address in all configuration files
```
$ cd deploy
$ vim configurator.sh
```
```
#!/bin/bash

# Initialization & global vars
# if you execute this script for the second time
# you should change these variables to the latest
# domain name and ip address
DDOMAIN="open-ims\.test"
DSDOMAIN="open-ims\\\.test"
DEFAULTIP="127\.0\.0\.1"
CONFFILES=`ls *.cfg *.xml *.sql *.properties 2>/dev/null`

# Interaction
printf "Domain Name:"
read domainname 
printf "IP Adress:"
read ip_address

# input domain is to be slashed for cfg regexes 
slasheddomain=`echo $domainname | sed 's/\./\\\\\\\\\./g'`

  if [ $# != 0 ] 
  then 
  printf "changing: "
      for j in $* 
      do
    sed -i -e "s/$DDOMAIN/$domainname/g" $j
    sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $j
    sed -i -e "s/$DEFAULTIP/$ip_address/g" $j
    printf "$j " 
      done
  echo 
  else 
  printf "File to change [\"all\" for everything, \"exit\" to quit]:"
  # loop
      while read filename ;
      do
        if [ "$filename" = "exit" ] 
        then 
        printf "exitting...\n"
        break ;
    
      elif [ "$filename" = "all" ]
      then    
          printf "changing: "
         for i in $CONFFILES 
         do
        sed -i -e "s/$DDOMAIN/$domainname/g" $i
        sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $i
        sed -i -e "s/$DEFAULTIP/$ip_address/g" $i
        
        printf "$i " 
         done 
         echo 
         break;
    
        elif [ -w $filename ] 
        then
            printf "changing $filename \n"
            sed -i -e "s/$DDOMAIN/$domainname/g" $filename
            sed -i -e "s/$DSDOMAIN/$slasheddomain/g" $filename
            sed -i -e "s/$DEFAULTIP/$ip_address/g" $filename
    
          else 
          printf "cannot access file $filename. skipping... \n" 
        fi
        printf "File to Change:"
      done 
  fi
```
```
$ chmod +x configurator.sh
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ grep -r "open-ims"
(Change realm name in the below file from open-ims.test to ims.mnc000.mcc460.3gppnetwork.org)
$ vim webapps/hss.web.console/WEB-INF/web.xml
$ vim hibernate.properties
```
And, change the following line:
```
hibernate.connection.url=jdbc:mysql://127.0.0.1:3306/hss_db
$ cp configurator.sh ../scripts/
$ cd ../scripts
$ grep -r "open-ims"
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ cp configurator.sh ../config/
$ cd ../config
$ ./configurator.sh 
Domain Name:ims.mnc000.mcc460.3gppnetwork.org
IP Adress:192.168.2.240

$ cd ../src-web
$ vim WEB-INF/web.xml
```
And, change open-ims.test to ims.mnc000.mcc460.3gppnetwork.org

Prepare mysql database:
```
$ mysql
<mysql> drop database hss_db;
<mysql> create database hss_db;
<mysql> quit
```
Import database located at /opt/OpenIMSCore into hss_db
```
$ cd /opt/OpenIMSCore
$ mysql -u root -p hss_db < FHoSS/scripts/hss_db.sql
$ mysql -u root -p hss_db < FHoSS/scripts/userdata.sql
```
Check grants for mysql access rights at first time installation:
```
$ mysql
# See last line in hss_db.sql:
<mysql> grant delete,insert,select,update on hss_db.* to hss@localhost identified by 'hss';
<mysql> grant delete,insert,select,update on hss_db.* to hss@'%' identified by 'hss';
```
Check database if domain names are o.k. in various entries and privileges
```
$ mysql -u hss -p
<mysql> show databases;
<mysql> use hss_db;
<mysql> select * from impu;
```
Prepare script-file, start HSS

Copy startup.sh to hss.sh in root directory
```
$ cp /opt/OpenIMSCore/FHoSS/deploy/startup.sh /root/hss.sh
```
And, add the following to hss.sh <font color="Hotpink">before echo Building Classpath</font>
```
cd /opt/OpenIMSCore/FHoSS/deploy
JAVA_HOME="/usr/lib/jvm/jdk1.7.0_79"
CLASSPATH="/usr/lib/jvm/jdk1.7.0_79/jre/lib/"
```
Start HSS using hss.sh

```
$ /root/hss.sh
```
Access the web-interface of HSS: <font color="Hotpink">http://<IMS_VM_FLOATING_IP>:8080/hss.web.console/</font>

For example, http://192.168.2.240:8080/hss.web.console/
```
user:      hssAdmin
password:  hss
```

Then, edit the /etc/hosts file as follows:

In the below example. epc-ims is the hostname of the machine
```
root@epc-ims:~# cat /etc/hosts
127.0.0.1	localhost
127.0.0.1	epc-ims
```
20. Add IMS subscription use in FoHSS as follows from the Web GUI

Assuming IMSI of the user as 001010123456791 and MSISDN is 0198765432100
```
Login to the HSS web console.
Navigate to the User Identities page	
Create the IMSU 
Click IMS Subscription / Create
Enter:
Name = 001010123456791
Capabilities Set = cap_set1
Preferred S-CSCF = scsf1
Click Save
```
Create the IMPI and Associate the IMPI to the IMSU

Click Create & Bind new IMPI

Enter:
```
Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Secret Key = 8baf473f2f8fd09487cccbd7097c6862 (Ki value as in Open5GS HSS database)
Authentication Schemes - All
Default = Digest-AKAv1-MD5
AMF = 8000 (As in Open5GS HSS database)
OP = 11111111111111111111111111111111 (As in Open5GS HSS database)
SQN = 000000021090 (SQN value as in Open5GS HSS database)
```
Click Save

Create and Associate IMPI to IMPU

Click Create & Bind new IMPU

Enter:
```
Identity = sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Barring = Yes
Service Profile = default_sp
Charging-Info Set = default_charging_set
IMPU Type = Public_User_Identity
```
Click Save

Add Visited Network to IMPU
Enter:
```
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
```
Click Add

Now, goto Public User Identity and create further IMPUs as following

1. tel:0198765432100
```
Public User Identity -IMPU-
Identity = tel:0198765432100
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add
```
2. sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org
```
Public User Identity -IMPU-
Identity = sip:0198765432100@ims.mnc000.mcc460.3gppnetwork.org
Service Profile = default_sp
Charging-Info Set = default_charging_set
Can Register = Yes
IMPU Type = Public_User_Identity
Click Save

Add Visited Network to IMPU
Enter:
Visited Network = ims.mnc000.mcc460.3gppnetwork.org
Click Add

Associate IMPI(s) to IMPU
IMPI Identity = 001010123456791@ims.mnc000.mcc460.3gppnetwork.org
Click Add
```
And, finally add these IMPUs as implicit set of IMSI derived IMPU in HSS i.e sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org as follows:
```
1. Goto to IMPU sip:001010123456791@ims.mnc000.mcc460.3gppnetwork.org
2. In "Add IMPU(s) to Implicit-Set" section give IMPU Identity created above to be added to this IMPU
```
21. APN settings

Clear all previous APN settings

Then, create APN as follows:

First create internet APN, APN name: internet, APN type: default –> Save APN

Then, create ims APN, APN name: ims, APN type: ims –> Save APN

22. eNB settings

Must have in the eNB:
```
Support for QoS
Support for Dedicated radio bearer creation
Make sure to check the DRB configuration with respect to QCI of APN accordingly (QCI 5 for ims)
On the eNB machine have the following static routes (since internal IP of the VM is advertised in S1AP messages and UE wont find the core in Uplink)

$ ip r add 192.168.2.240/32 via 172.24.15.30
```
23. USIM and UE settings
```
Make sure to disable SQN check in Sysmocom SIM cards using sysmo-usim-tool tool https://github.com/herlesupreeth/sysmo-usim-tool
Tested with OnePlus 5 with following methods (Official Google method is the recommended method to prevent damage to phone)
(Official Google method) - Please follow the instructions in the following link @herlesupreeth/CoIMS_Wiki to force enable VoLTE using Carrier Privileges
(Risky method) With modfication to enable force IMS registration is a must or else UE will not even attempt to connect to P-CSCF. Need to apply the fix back after each update. https://forum.xda-developers.com/oneplus-5t/how-to/guide-volte-vowifi-german-carriers-t3817542
```
24. Start IMS components and FoHSS followed by Open5GS and eNB, then try connecting the phones

25. Test voice call
```
Assuming IMSI of the user1 as 001010123456791 and MSISDN is 0198765432100 and IMSI of the user2 as 001010123456792 and MSISDN is 0298765432100. Try calling user2 from user1 by dialing its MSISDN ie. 0298765432100

You can see the sample traffic. – [volte.pcapng].
```
26. For debugging
```
Debug using wireshark at Open5GS machine and following wireshark display filter

s1ap || gtpv2 || pfcp || diameter || diameter.3gpp || sip
Also,

Debugging Diameter messages between PCRF and P-CSCF in Wireshark if the TCP/SCTP port other than 3868

Open Wireshark –> Preferences –> Protocols –> Diameter –> Change to whatever ports are being used

Appendix
Open5GS
Sukchan Lee
acetcom@gmail.com
open5gs
Open5GS is a C-language implementation of 5G Core and EPC, i.e. the core network of NR/LTE network (Release-16)
```

## 安装rtpengine

```
#!/bin/bash
# Install the packages required to compile RTP engine
set -e
yum install iptables-devel kernel-devel kernel-headers xmlrpc-c-devel
yum install "kernel-devel-uname-r == $(uname -r)"
yum install glib glib-devel gcc zlib zlib-devel openssl openssl-devel pcre pcre-devel libcurl libcurl-devel xmlrpc-c xmlrpc-c-devel
yum install libevent-devel glib2-devel json-glib-devel gperf libpcap-devel git hiredis hiredis-devel redis perl-IPC-Cmd
# MariaDB ver 10+
yum install MariaDB-devel MariaDB-client MariaDB-shared MariaDB-server
# Spandsp
yum install spandsp-devel spandsp
# epel
yum install epel-release
# libwebsockets
yum install libwebsockets libwebsockets-devel
# ffmpeg
rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Fvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
# check for Nux desktop repo by yum repolist
yum -y install ffmpeg ffmpeg-devel
# Get The Latest Release Of RTPEngine Source From RTPEngine’s GitHub Repository
cd /usr/local/src
if [ -d rtpengine ]; then
    cd rtpengine
    make clean
    git pull
else
    git clone https://github.com/sipwise/rtpengine.git
fi
# Compile and install the daemon
cd /usr/local/src/rtpengine/daemon/
make
cp rtpengine /usr/sbin/rtpengine
# Compile and install iptables extension
cd /usr/local/src/rtpengine/iptables-extension
make all
cp libxt_RTPENGINE.so /usr/lib64/xtables/.
# Compile and install the kernel module xt_RTPENGINE
cd /usr/local/src/rtpengine/kernel-module
make
# determine kernel release
uname -a
kernel_ver=`uname -r`
cp xt_RTPENGINE.ko /lib/modules/${kernel_ver}/extra/xt_RTPENGINE.ko
depmod -a
# load module at boot time
# Add the following lines to /etc/modules-load.d/rtpengine.conf (add the following lines)
CONF_FILE=/etc/modules-load.d/rtpengine.conf
echo '# load xt_RTPENGINE module' > ${CONF_FILE}
echo 'xt_RTPENGINE' >> ${CONF_FILE}
# load the module and check
modprobe xt_RTPENGINE
lsmod | grep xt_RTPENGINE
# check files
ls -l /proc/rtpengine/control
ls -l /proc/rtpengine/list

TableID=0
# add forwarding table with an ID=$TableID. $TableID is a number between 0-63
echo 'add 0' > /proc/rtpengine/control
# Adding iptables rules to forward the incoming packets to xt_RTPENGINE module
iptables -I INPUT -p udp -j RTPENGINE --id $TableID
ip6tables -I INPUT -p udp -j RTPENGINE --id $TableID
```