

## EPS HSS、HLR、IMS HSS有什么区别？分别用于什么场景

    HLR、EPS—HSS、IMS—HSS在网络中的功能类似，均用来存储用户的鉴权数据，业务签约数据，并维护移动用户的位置信息，为呼叫等业务提供路由。

    （1）HLR（Home  Location Register，归属位置寄存器）用于存储用户的2G/3G签约信息和动态位置信息；EPS—HSS（Evolved  Packet System—Home  Subscriber Server，EPS归属签约用户服务器）是HLR的演进，包含HLR的功能，同时存储用户的4G签约信息和动态位置信息；

    （2）IMS—HSS（IP  Multimedia Subsystem—Home  Subscriber Server，IMS归属签约用户服务器）位于IMS网络，用于用户IMS数据存储、认证、鉴权和寻址。

    （3）IMS—HSS由HLR演变而来，除了原来HLR功能外，还存储IMS业务相关的数据，如用户的业务签约信息和业务触发信息等。
 
     三者位于不同的网络场景，为网络提供如下功能：

    （1）HLR位于2G/3G网络，通过C/D接口与MSC相连，Gr/Gc接口与SGSN/GGSN相连，采用MAP协议；

    （2）EPS—HSS归于4G网络，通过S6a/S6d接口与MME/SGSN相连，采用DIAMETER协议；

    （3）IMS—HSS用户IMS网络，通过Cx/Dx接口与CSCF相连，采用DIAMETER协议。