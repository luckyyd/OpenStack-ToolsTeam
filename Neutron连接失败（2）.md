我们在Icehouse版本创建虚拟机会遇到错误：无法连接到Neutron.的报错，但是虚拟机还可以创建成功，这个是一个已知的bug，可以通过修改源码解决。

**注意：还有一种情况，就是你的Neutron真的无法连接，要查看服务和监听端口是否正常！**

Yum安装的文件在这里：

    [root@test-node1 ~]# vim /usr/share/openstack-dashboard/openstack_dashboard/api/neutron.py
    
源码安装的在这里：

    vim /usr/lib/python2.6/site-packages/openstack_dashboard/api/neutron.py
    
在class FloatingIpManager类里少了is_supported的方法，这个是一个bug，可以通过手动修改解决。

    def is_simple_associate_supported(self):
         # NOTE: There are two reason that simple association support
         # needs more considerations. (1) Neutron does not support the
         # default floating IP pool at the moment. It can be avoided
         # in case where only one floating IP pool exists.
         # (2) Neutron floating IP is associated with each VIF and
         # we need to check whether such VIF is only one for an instance
         # to enable simple association support.
         return False
    #在这个类的最下面，增加下面的方法，注意缩进。
    def is_supported(self):
         network_config = getattr(settings, 'OPENSTACK_NEUTRON_NETWORK', {})
         return network_config.get('enable_router', True)

修改完毕后，需要重启apache才可以生效：

    [root@test-node1 ~]# /etc/init.d/httpd restart
