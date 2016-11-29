Installation via Mirantis Fuel

Running Mirantis OpenStack on VirtualBox

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-1.png)

部署硬件：windows 10 (8GB RAMI3，CPU必须支持虚拟化)+ virtualbox 5.0

虚机下载：https://www.virtualbox.org/wiki/Downloads

部署工具：Mirantis Fuel 7.0，openstack juno

镜像地址：https://software.mirantis.com/

部署网络： 

1、10.20.0.0：这是master的专有网络，我们访问web的时候就是走这个网络；

2、172.16.0.0：这是公共网络，也是浮动IP网段，给虚机提供外网

3、192.168.4.0：这是openstack的管理、存储和虚机内部网络

先打开virtualbox，点击“新建”来新建一个虚机

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-2.png)

我们命名为“Master”，类型为"linux"，版本选择“redhat 64 bit”，

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-3.png)

由于安装过程资源占用较大，先给它分配4G内存，安装完成后可调小

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-4.png)

新建一个硬盘

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-5.png)

默认VDI硬盘即可

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-6.png)

由于是在本机，为了节省空间，选择“动态分配”，实际部署环境如果空间足够，选择“固定大小”，速度会快一点。

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-7.png)

选择硬盘的存放位置，磁盘最好大于40G

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-8.png)

虚机创建完毕，我们来设置下虚机，选中“Master”虚机，然后单击上方的“设置

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-9.png)

选择“存储”，装配mirantisopenstack 6.0的镜像，下载地址见开始说明

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-10.png)

第一块网卡选择#2，第二块网卡选择#3，第三块网卡选择#4，每个虚机都是如此，方便管理，由于openstack的虚机需要PXE启动，所以网卡一定要选择“fast III”，由于网络支持VLAN，所以混杂模式要选择“全部允许”

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-11.png)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-12.png)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-13.png)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-14.png)

以上虚机的界面名称来自以下，先选择virtualbox的“管理”，选择“全局设定”，单击“网络”来对网络进行设定，我们单击仅主机网络，然后添加三张网卡如下，IP地址设置如下，禁用DHCP，各个网络的作业前面已经说明了。

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-15.jpg)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-16.jpg)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-17.jpg)

好了，选定“Master”主机，右键，启动。如下开始安装Fuel master

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-18.png)

如下开始自动安装，centos系统已经精简，安装包只有305个，这个过程很快

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-19.jpg)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-20.jpg)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Mirantis-fuel-21.jpg)

终于等到这个完结的画面：系统用户名密码是root，r00tme；Fuel的web地址是10.20.0.2::8443，用户名密码是admin，admin。

由于这个地址是默认的，所以你应该知道我们为什么设置10.20.0.0这个网段了，因为我们实体机和虚机需要通信，另外确认本地没有代理，确认防火墙关闭，以顺利登陆Fuel界面


问题：安装完Fuel Master之后，按提示访问http://10.20.0.2:8000

没反应

解决方案：

http://blog.sina.com.cn/s/blog_7643a1bf0102vjw0.html

结果：尝试但问题未解决

网上提供了一些视频教程，准备去研究一下



