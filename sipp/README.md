
## 安装
在Debian/ Ubuntu Linux系统中，我们可以使用以下命令来安装ncurses:
sudo apt-get install libncurses5-dev libncursesw5-dev -y
而我使用的是 CentOS版本的Linux，其解决办法有点不一样，具体是，在RHEL / Fedora / CentOS Linux中，我们使用以下命令：
yum install ncurses-devel ncurses -y

copy sipp-3.3.990.built.tar.gz

tar -xvf sipp-3.3.990.built.tar.gz

cd sipp-3.3.990

make

make install

脚本暂时使用 docker container vonrconftest:/root/src/sipp/

## TODO::