在Dashboard中完成安装时遇到：

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-1.jpg)

Error: Unable to connect to Neutron. 同样也不能分配Floating IP，不能进入虚拟机。

检查了nova和neutron的服务，和视频里一样都正常开启了。

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-2.jpg)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-3.jpg)

发现在/var/log/httpd里面看到了Dashboard的error

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-4.jpg)

这个error没有出错的原因，只是显示了我发送的请求。

我在dashboard的local setting里面把DEBUG=TRUE  不过并没有错误信息输出，也没有找到openstack-dashboard-error.log这个文件。

控制节点/var/log/neutron/server.log Neutron Server log 没有显示Error

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-5.jpg)

网络节点/var/log/neutron/openvswitch-agent.log

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Neutron-connection-fail-6.jpg)

/etc/log/horizon里面没有Dashboard的日志信息

排查问题：

controller的Neutron Server服务正常开启

    root@openstack:~# neutron agent-list
    
    +--------------------------------------+--------------------+-------+-------+----------------+
    | id                                   | agent_type         | host  | alive | admin_state_up |
    +--------------------------------------+--------------------+-------+-------+----------------+
    | 74b06b71-a3fa-4567-9395-be6b1e427d50 | DHCP agent         | fsd25 | :-)   | True           |
    | 77113f11-7eff-4987-b51e-de40238c25dc | Open vSwitch agent | fsd25 | :-)   | True           |
    | 7c8562c0-4b10-4c5b-8930-fdfac079defc | L3 agent           | fsd25 | :-)   | True           |
    | c6f75d78-feea-4545-803d-e077858aaa44 | Metadata agent     | fsd25 | :-)   | True           |
    +--------------------------------------+--------------------+-------+-------+----------------+

Neutron tcp服务建立，端口也在监听：

    root@openstack:~# netstat -lntp  | grep 9696
    tcp        0      0 0.0.0.0:9696            0.0.0.0:*               LISTEN      13301/python


解决方法：

是由于默认的dashboard版本问题，默认安装新的版本会导致Neutron连接失败

先卸载最新的dashboard版本2014.1.5，安装2014.1.3版本即可

yum install openstack-dashboard-2014.1.3-1.el6.noarch -y

重新进入dashboard，问题解决。


