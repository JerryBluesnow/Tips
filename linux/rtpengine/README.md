 
RTPEngine install on Centos 7
October 24, 2020  3 min read

```
https://github.com/sipwise/rtpengine/wiki/Install-RTPEngine-on-Centos-7
https://blog.kolmisoft.com/rtpengine-install-on-centos-7/
```

Complete guide on how to install RTPEngine on Centos 7.

As the original guide is a little bit outdated, here is the step-by-step complete guide to getting working RTPEngine installation on Centos 7 server.

Update the system and install the newest kernel
yum -y update
yum -y install kernel-devel
yum -y update kernel
reboot

Install iptables
systemctl stop firewalld
systemctl disable firewalld
yum -y install iptables-services iptables-devel
systemctl enable iptables.service
systemctl start iptables.service
iptables -F
service iptables save

Check if iptables are active and running

systemctl status iptables.service

Install the packages required to compile RTPEngine
yum -y install epel-release
yum -y install glib glib-devel gcc zlib zlib-devel openssl openssl-devel pcre pcre-devel libcurl libcurl-devel xmlrpc-c xmlrpc-c-devel wget
yum -y install libevent-devel glib2-devel json-c-devel json-glib json-glib-devel gperf libpcap-devel git perl-IPC-Cmd libiptcdata-devel libiptcdata-devel hiredis hiredis-devel redis iptables-devel libwebsockets-devel
yum -y install spandsp spandsp-devel

FFmpeg packets
rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
yum -y install http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
yum repolist

# RPM fussion repo for ffmpeg build dependencies
yum -y localinstall --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm

# We need opus 1.3 to work with new ffmpeg
yum -y localinstall --nogpgcheck http://repo.okay.com.mx/centos/8/x86_64/release/opus-1.3-0.4.beta.el8.x86_64.rpm

mkdir /usr/src/ffmpeg_rpms
cd /usr/src/ffmpeg_rpms

wget https://download1.rpmfusion.org/free/el/updates/7/x86_64/f/ffmpeg-devel-3.4.9-1.el7.x86_64.rpm
wget https://download1.rpmfusion.org/free/el/updates/7/x86_64/l/libavdevice-3.4.9-1.el7.x86_64.rpm
wget https://download1.rpmfusion.org/free/el/updates/7/x86_64/f/ffmpeg-3.4.9-1.el7.x86_64.rpm
wget https://forensics.cert.org/centos/cert/7/x86_64/ffmpeg-libs-2.6.8-3.el7.nux.x86_64.rpm

yum -y localinstall *.rpm

yum -y install bcg729 bcg729-devel

MariaDB
As MariaDB is a default substitute for MySQL on Centos 7, we will use it, instead of the real MySQL. If you need MySQL – install MySQL packets instead or Maria

cd /usr/src/
wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
chmod +x mariadb_repo_setup
./mariadb_repo_setup
yum -y install MariaDB-devel MariaDB-client MariaDB-shared

Get RTPEngine source from GitHub repository:
cd /usr/src
git clone https://github.com/sipwise/rtpengine.git rtpengine
cd rtpengine

Compile and install the daemon
cd /usr/src/rtpengine/daemon/
make
cp -fr rtpengine /usr/sbin/rtpengine
cp -fr rtpengine /usr/local/bin/rtpengine

Compile and install iptables extension
cd /usr/src/rtpengine/iptables-extension
make all
cp -fr libxt_RTPENGINE.so /usr/lib64/xtables/.

Compile and install the kernel module xt_RTPENGINE
cd /usr/src/rtpengine/kernel-module
make
cp -fr xt_RTPENGINE.ko /lib/modules/`uname -r`/extra/xt_RTPENGINE.ko
depmod -a
modprobe -v xt_RTPENGINE

Check if the kernel module loaded properly:

lsmod | grep xt_RTPENGINE

Make to load the module at the boot time:

echo "# load xt_RTPENGINE module"  >> /etc/modules-load.d/rtpengine.conf
echo "xt_RTPENGINE" >> /etc/modules-load.d/rtpengine.conf

Check if RTPEngine is accessible:

ls -l /proc/rtpengine/control | grep root

Should see:

–w–w—-. 1 root root 0 Oct 16 10:32 /proc/rtpengine/control

ls -l /proc/rtpengine/list

-r–r–r–. 1 root root 0 Oct 16 10:32 /proc/rtpengine/list

Install configuration file
Get your external IP (with ‘ip addr’ for example)

In the following command change 111.111.111.111 to your external IP

echo "OPTIONS=\"-i 111.111.111.111 -n 127.0.0.1:2223 -m 10000 -M 60000 -L 4 --log-facility=local1 --table=8 --delete-delay=0 --timeout=60 --silent-timeout=600 --final-timeout=7200 –offer-timeout=60 --num-threads=12 --tos=184 –no-fallback\"" > /etc/sysconfig/rtpengine

Configure RTPEngine log
echo "local1.*      -/var/log/rtpengine/rtpengine.log;clean_template" >> /etc/rsyslog.conf
mkdir -p /var/log/rtpengine
touch /var/log/rtpengine/rtpengine.log

Configure logrotate
echo "/var/log/rtpengine/rtpengine.log {
daily
rotate 4
missingok
dateext
copytruncate
compress
}" > /etc/logrotate.d/rtpengine

Disable logging to /var/log/messages
perl -pi.bak -e 's#(\s+)\/var\/log\/messages#;local1.none$1/var/log/messages#' /etc/rsyslog.conf
systemctl restart rsyslog

Add packet forwarding
echo 'add 8' > /proc/rtpengine/control

Add iptables rules to forward the incoming packets to xt_RTPENGINE module
iptables -I INPUT -p udp -j RTPENGINE --id 8
ip6tables -I INPUT -p udp -j RTPENGINE --id 8
service iptables save
service ip6tables save

Configure RTPEngine service
echo "[Unit]
Description=Kernel based rtp proxy
After=syslog.target
After=network-online.target
After=iptables.service
Requires=network-online.target

[Service]
Type=forking
PIDFile=/var/run/rtpengine.pid
EnvironmentFile=-/etc/sysconfig/rtpengine
ExecStart=/usr/local/bin/rtpengine -p /var/run/rtpengine.pid \$OPTIONS

Restart=on-abort

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/rtpengine.service

Enable and start RTPEengine
systemctl enable rtpengine.service
mkdir -p /var/spool/rtpengine
systemctl start rtpengine

Check status
systemctl status rtpengine.service

Check if g729 installed properly
rtpengine --codecs | grep "G729"

Some notes about systemd/journald
Centos 7 comes with such ugly abomination as systemd/journald which is hardcoded into the system and can’t be turned off. The result is HUGE CPU usage when doing such a simple task as writing logs.

It is very important to run RTPEngine in the low log-level. We put log-level 4 (-L 4 in /etc/sysconfig/rtpengine), which equals WARNING level. Using levels 5-7 is strongly not advised. With high call load, RTPEngine produces a lot of NOTICE/INFO/DEBUG messages and journald process will eat your CPU in no time.

On our servers, we do the following steps to minimize the impact of this monstrosity:

In /etc/rsyslog.conf add after ‘$ModLoad imuxsock’ and ‘$ModLoad imjournal’:

$SystemLogRateLimitInterval 0
$SystemLogRateLimitBurst    0
$IMUXSockRateLimitInterval 0
$IMJournalRatelimitInterval 0

In /etc/systemd/journald.conf add/change current ones to:

RateLimitInterval=0
RateLimitBurst=0
Storage=volatile
Compress=no
RateLimitInterval=0
MaxRetentionSec=5s

Execute:
mv /var/log/journal /var/log/journal-disabled
systemctl restart systemd-journald
systemctl restart rsyslog

If anybody can share how to completely turn journald off, please contact us. We will be eternally grateful.

Log Levels of RTPEngine
“EMERG”,

0

“ALERT”,

1

“CRIT”,

2

“ERR”,

3

“WARNING”,

4

“NOTICE”,

5

“INFO”,

6

“DEBUG”

7

Cover from Unsplash
General Guide
« 2020 September Update
2020 October Update »
2022 February Update

Feb 1, 2022  1 min read
Freeswitch installation on Debian 11

Jan 19, 2022  2 min read
2022 January Update

Jan 3, 2022  1 min read
Leave a Reply
Write a response...

Name

E-mail address

Website Link

 Save my name, email, and website in this browser for the next time I comment.

8  −  
  =  2	

© Copyright Kolmisoft
