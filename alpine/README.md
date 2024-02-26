

# setup rnxt ims build environment

- docker pull alpine

- docker run -it --restart=always --privileged --network=host --name alpine -v /etc/resolv.conf:/etc/resolv.conf alpine:latest

- docker exec -it alpine bash

- apk add --no-cache openssh-server openssh-client

- ssh-keygen -t rsa -C "xxxx@126.com"

- cd ~;mkdir src;cd src;git clone git@codeup.aliyun.com:radionxt/ims/rnxt-docker.git

- git checkout simple_n5

- mkdir /usr/src;cd /usr/src;git clone git@codeup.aliyun.com:radionxt/ims/rnxt.git vonrptt

- mv rnxt kamailio

- adduser -g root build

- apk add gcc g++ make cmake gfortran libffi-dev openssl-dev libtool abuild

- apk --update --no-cache add bash docker

- apk add openrc --no-cache

- rc-update add docker

- touch /run/openrc/softlevel

- /etc/init.d/docker restart

- cd ~/src/rnxt-docker/kamailio/

- ./build

# build ims

- 确保执行以下操作是在已经安装docker service的linux系统。

- 确保一下执行操作不在docker container内部，build过程会在新的docker container中进行，此条款可避免docker内部启docker导致网络无法访问的问题。

- git clone git@codeup.aliyun.com:radionxt/ims/rnxt-docker.git

- cd rnxt-docker

- git checkout simple_n5

- cd rnxt-docker/kamailio/

- git clone git@codeup.aliyun.com:radionxt/ims/rnxt.git vonrptt

- ./build

- after build success, run ./pack

# build ims - hss

接上一个，

-  cd ../hss/; git clone git@codeup.aliyun.com:radionxt/ims/FHoSS.git

- ./build

- ./pack

# build ims - dns

- cd ../bind

- ./pcak