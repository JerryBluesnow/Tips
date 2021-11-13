#  Tips for windows

## support both LAN and WIFI 
route add -p  223.*.*.* mask 255.255.255.255 192.168.43.1

## windows下ssh server搭建方法
  [reference](https://blog.csdn.net/luhuaxing/article/details/104074680)
  https://www.cnblogs.com/feipeng8848/p/8568018.html
  https://jingyan.baidu.com/article/a3aad71a64fb38f1fa009671.html



## [Hyper-V 虚拟机无法上网的解决方法](https://www.cnblogs.com/lymblog/p/7269677.html)

Windows 8中内置的Hyper-V管理器可以说给许多人带来了惊喜！在Hyper-V管理器强大的同时，也同样面临着设置中一些不可避免的麻烦。有人说，Hyper-V[虚拟机](http://www.cr173.com/s/VmareWorkstation/)联网麻烦，其实，只要掌握了技巧，也只是举手之劳。

| 相关链接                                                     | 版本说明       | 下载地址                                     |
| ------------------------------------------------------------ | -------------- | -------------------------------------------- |
| [VMware Workstation](http://www.cr173.com/soft/68480.html)   | 官方中文正式版 | [查看](http://www.cr173.com/soft/68480.html) |
| [Mac超强虚拟机](http://www.cr173.com/soft/49936.html)        | VMware Fusion  | [查看](http://www.cr173.com/soft/49936.html) |
| [virtualbox虚拟机](http://www.cr173.com/soft/2626.html)      | 多语中文版     | [查看](http://www.cr173.com/soft/2626.html)  |
| [VirtualBox虚拟机MAC版](http://www.cr173.com/soft/49921.html) | 最新版         | [查看](http://www.cr173.com/soft/49921.html) |
| [virtualBox汉化补丁包](http://www.cr173.com/soft/31504.html) | 简体中文语言包 | [查看](http://www.cr173.com/soft/31504.html) |
| [VMware workstation MAC补丁](http://www.cr173.com/soft/88164.html) | 绿色版         | [查看](http://www.cr173.com/soft/88164.html) |

任何一台计算机，如果不能与网络连通，可以说已经失去了大部分的功能，Windows 8尤是如此，[虚拟机](http://www.cr173.com/k/VM/)亦是如此。

 

Hyper-V教程相关资料

- [创建虚拟机](http://www.cr173.com/html/18675_1.html)
- [安装Ubuntu](http://www.cr173.com/html/18467_1.html)
- [转换成vmware](http://www.cr173.com/html/21227_1.html)
- [设置联网](http://www.cr173.com/html/18692_1.html)
- [远程部署](http://www.cr173.com/html/23028_1.html)
- [主机网络上网](http://www.cr173.com/html/49190_1.html)

 

Hyper-V并不能对物理机的网卡进行识别，所以需要借助虚拟网卡通过物理机的网络共享实现网络链接。

在关闭Hyper-V虚拟机的情况下，**选择Hyper-V管理界面中的“虚拟交换机管理器”。**

![img](http://www.cr173.com/up/2012-12/2012122022255958561.jpg)

在弹出的对话框中**“新建虚拟网络交换机**”，选择“内部”，点击“创建虚拟交换机”。

![img](http://www.cr173.com/up/2012-12/2012122022254269408.jpg)

为虚拟交换机命名后点击“应用”。稍事等待后即可在左侧看到新添加的虚拟交换机。

![img](http://www.cr173.com/up/2012-12/2012122022254381416.jpg)

此时，在控制面板-网络和Internet-网络和共享中心中，**可以看到如下未识别的链接：**

![img](http://www.cr173.com/up/2012-12/2012122022254384088.jpg)

在“更改适配器设置”下面也可以见到如下设备：

![img](http://www.cr173.com/up/2012-12/2012122022254344565.jpg)

这就是刚刚创建出来的虚拟交换机。

虽然有了虚拟设备，但是此时虚拟机仍不能正常链接。

**在网络与共享中心下面点击现有的Internet链接：**

![img](http://www.cr173.com/up/2012-12/2012122022254453901.jpg)

在弹出的对话框中选择“属性”：

![img](http://www.cr173.com/up/2012-12/2012122022254425858.jpg)

切换到“共享”标签下，勾选“允许其他网络用户通过此计算机的Internet连接来连接”并在下方“家庭网络连接”中选择刚刚创建的虚拟交换机——vEthernet (Hyper-V Switch)，点击“确定”。

![img](http://www.cr173.com/up/2012-12/2012122022254464854.jpg)

此时，在管理员模式运行的命令提示符（在屏幕左下角右键，选择“命令提示符 管理员”）中输入“route print”后会在IPv4路由表中找到关于192.168.137.1的信息：

![img](http://www.cr173.com/up/2012-12/2012122022254436811.jpg)

接下来，进入到Hyper-V虚拟机设置界面，在“硬件”下的“网络适配器”中，设置“虚拟交换机”**为刚刚设置好的Hyper-V Switch虚拟交换机，点击“确定”。**

![img](http://www.cr173.com/up/2012-12/2012122022254461918.jpg)

![img](http://www.cr173.com/up/2012-12/2012122022254428267.jpg)

此时再重新启动Hyper-V虚拟机，在对应的网络连接下面的TCP/IP协议中设置为“自动获取IP地址”和“自动获取DNS服务器”，则可进行网络连接。

![img](http://www.cr173.com/up/2012-12/2012122022254531994.jpg)

若使用手动设置，则设置IP地址为“192.167.137.X”，X为2~255任意数字，子网掩码为“255.255.255.0”，默认网关为 “192.168.137.1”，DNS服务器设置为“192.168.137.1”。注意此处的网关与DNS服务器为微软默认，没有需要请勿更改。

![img](http://www.cr173.com/up/2012-12/2012122022254540275.jpg)

**确定之后会发现虚拟机已经可以进行网络连接了！**

![img](http://www.cr173.com/up/2012-12/2012122022254548556.jpg)

若在此后更换了物理机的网络连接，需要重新设置共享，共享方式不变。

此外，如果发现虚拟机中的链接变为“未识别的网络连接”，在网络图标上带有黄色的叹号，可以按照前文在命令提示符中查看路由表是否正常，若不包含192.168.137.1内容（如下图）则说明网络共享不正常，**可以先禁用网络共享再按照前文重新开启即可**。

![img](http://www.cr173.com/up/2012-12/2012122022254599032.jpg)

除了采用共享式的内部网络连接，还可以使用外部网络连接，但是此时物理机若只含有一个网卡设备，则物理机网络连接将会断开。（一块网卡在同一时间只支持一条网络连接，虚拟机也相当于一台计算机，故不能两者同时使用。）

# Win10内置虚拟机Hyper-V如何联网 Hyper-V显示连接错误是无效操作的解决方法

2019-09-23 11:54 教程之家 媛媛 

字号：**T**|**T**

<iframe width="800" frameborder="0" height="140" scrolling="no" src="https://pos.baidu.com/s?wid=800&amp;hei=140&amp;di=u6574772&amp;s1=657518486&amp;s2=1342998651&amp;ltu=https%3A%2F%2Fwww.jiaochengzhijia.com%2Fwin10%2F7345.html&amp;tr=1639034589&amp;mt=45d3e789dcb61930&amp;dc=3&amp;ti=Win10%E5%86%85%E7%BD%AE%E8%99%9A%E6%8B%9F%E6%9C%BAHyper-V%E5%A6%82%E4%BD%95%E8%81%94%E7%BD%91%20Hyper-V%E6%98%BE%E7%A4%BA%E8%BF%9E%E6%8E%A5%E9%94%99%E8%AF%AF%E6%98%AF%E6%97%A0%E6%95%88%E6%93%8D%E4%BD%9C%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%20-%20Win10%20-%20%E6%95%99%E7%A8%8B%E4%B9%8B%E5%AE%B6&amp;ps=223x327&amp;drs=1&amp;pcs=1855x969&amp;pss=1855x4369&amp;cfv=0&amp;cpl=5&amp;chi=1&amp;cce=true&amp;cec=UTF-8&amp;tlm=1639034588&amp;psr=1920x1080&amp;par=1920x1040&amp;pis=-1x-1&amp;ccd=24&amp;cja=false&amp;cmi=2&amp;col=en-US&amp;cdo=-1&amp;tcn=1639034589&amp;dtm=HTML_POST&amp;tpr=1639034588987&amp;ari=2&amp;ant=0&amp;psi=e03f4eed12e5a2f3&amp;exps=110257,110009,111000,110011&amp;prot=2&amp;dis=0&amp;dai=1&amp;dri=0&amp;ltr=https%3A%2F%2Fwww.baidu.com%2Flink%3Furl%3D6xfzy87p_1xizplnNh31aJPLp1zgzdR06I-AeWQUsdCSoC2OtV1zvubzafBFuw4-V3HyqBimyWA_vnG7Aabeba%26wd%3D%26eqid%3D8f0ccf2d000308640000000561b1ac02"></iframe>

 　Win10系统内置虚拟机，在软件开启/关闭上的设置里可以直接开启虚拟机Hyper-V。在Hyper-V里，你可以独立下载、安装、运行某些程序，不对Win10系统产生垃圾、病毒等影响。不过，很多用户使用Hyper-V时，第一个问题是如何联网。虚拟机如何联网呢？有时候打开Hyper-V，却提示连接出错，操作失败，这是什么原因？如何解决？下面来看看Win10内置虚拟机Hyper-V如何联网以及打不开的解决方法。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115528_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115528_1.jpg)

## Hyper-V无法联网的解决方法

　　1、打开Win10下虚拟机 Hyper-v管理器，检查安装好的虚拟机的设置 例如我的是win7系统

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115537_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115537_1.jpg)

　　2、打开设置后点击左边的网络适配器，然后在右边虚拟交换机选择新建虚交换机然后确定。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115543_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115543_1.jpg)

　　3、返回Hyper-v管理器中点击虚拟交换机管理器

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115549_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115549_1.jpg)

　　4、创建虚拟交换机 ，如果想虚拟机联网，需要选择外部 只想虚拟机与本机交互使用可以选择内部（可以通过网上邻居来相互传送文件）

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115555_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115555_1.jpg)

　　5、这里就是外部做介绍

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115601_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115601_1.jpg)

　　6、点确定后会在[电脑](https://www.jiaochengzhijia.com/digital/taishi/)主系统中生成一个

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115608_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115608_1.jpg)

　　7、此时在vEthernet 点右键选属性，选择 interntet协议版本4（tcp/ipv4）

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115614_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115614_1.jpg)

　　8、Ip 可以根据情况选指定或自动获取。而另外一个 以太网 适配器就不用管就行 ，正常情况下电脑就能连上网了，如果不能需要在虚拟交换机管理器中把刚建的新建虚拟机管理器删除，重新建立就行。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115626_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115626_1.jpg)

　　9、这时可以进入我们的虚拟机系统进行如下设置Ip 可以根据情况选指定或自动获取

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115643_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115643_1.jpg)

　　10、此时虚拟机就能联上网了，同时也能和本地局域网电脑相联。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115650_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115650_1.jpg)

　　注意事项

　　假入没有连接成功，就把创建的新建虚拟机管理器删除，重新按按步骤456再做一次。

## Hyper-V虚拟机无法打开显示连接出错怎么办

　　微软也有一个本身自带的虚拟机叫做Hyper-V，但是并不好用。如题，打开Hyper-V，提示：尝试连接“XXX”时出错。请检查……此服务器。计算机“XXX”上的操作失败：无效类。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115709_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115709_1.jpg)

　　1、之所以说“无效类”，是因为Hyper-V平台没有打开。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115702_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115702_1.jpg)

　　2、同时按下win+X键。找到程序和功能。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115716_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115716_1.jpg)

　　3、启用和关闭windows功能。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115723_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115723_1.jpg)

　　4、勾选Hyper-V平台。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115753_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115753_1.jpg)

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115759_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115759_1.jpg)

　　5、如果你发现，没有这个选项（如图）

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115807_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115807_1.jpg)

　　6、那很可能是因为你的系统不是64位的，注意32位系统是无法在本地运行Hyper-V的！此时建议改用VMware Workstation。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115822_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115822_1.jpg)

　　7、如果发现是灰色的，无法勾选，很可能是因为BOIS中虚拟技术VT没有打开。

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115829_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115829_1.jpg)

## Hyper-V无法使用鼠标怎么办

　　hyper-v 虚拟机建立完成后，链接虚拟机，发现无法捕捉鼠标（不能使用鼠标操作）

　　1、打开虚拟机，直接连接虚拟机

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115841_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115841_1.jpg)

　　2、弹出安装系统时的安装关盘

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115847_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115847_1.jpg)

　　3、弹出后，虚拟光盘的挂载状态

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115853_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115853_1.jpg)

　　4、在菜单栏的操作下面，选择 插入集成服务安装盘

[![img](https://img.jiaochengzhijia.com/uploads/190923/4_115903_1.jpg)](https://img.jiaochengzhijia.com/uploads/190923/4_115903_1.jpg)

　　5、重启虚拟机！OK，鼠标可以使用

　　注意事项：如果重启之后还不能使用鼠标，是因为没有虚拟光驱没有自动运行，请手动执行一次。

　　以上就是Win10系统内置虚拟机Hyper-V联网和无法打开解决方法的详细教程。Hyper-V是虚拟机，是一种虚拟机软件，依靠硬件技术实现虚拟技术，你的Win10设备是64位，还得开启虚拟化技术，也就是说，你的CPU硬件支持虚拟技术VT，但是默认关闭，你得在BIOS这个主板芯片程序里开启，这是前提。然后就可以在[win10](http://www.jiaochengzhijia.com/win10/)中随心所欲使用虚拟机。关注教程之家，解锁更多系统教程。



# 宿主机上新建虚拟机_Hyper-V安装Windows 10虚拟机

https://blog.csdn.net/weixin_35589827/article/details/112487233



# WIN10 Hyper-V 新建虚拟机 步骤以及一些有坑的地方说明

https://blog.csdn.net/zfr_xuexiao66/article/details/80525970

## windows预览体验计划 0x0错误解决方法
都是网上整合的方法，能用就行 作者：小黑-xiaohei https://www.bilibili.com/read/cv11947720 出处：bilibili
下载一个注册表文件 直接安装

https://xiaoheiswz001.lanzoui.com/ievBmquyflg


然后打开windows预览计划，如果闪退

windows+R 输入regedit定位到

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsSelfHost

1.选中Account项，然后在右侧安全窗格中删除除“默认”外的所有键值

2.删除红框中的子项


现在再打开预览计划应该就正常了 https://www.bilibili.com/read/cv11947720 出处：bilibili

## 磁盘清理
- [Dism++，也许是最强的实用工具](https://www.chuyu.me/zh-Hans/index.html)
