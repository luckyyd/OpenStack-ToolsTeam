nova提供了novncproxy代理支持用户通过vnc来访问虚拟机，用户可以通过websocket、java客户端或者spicehtml5来访问。通过websket访问虚拟机的功能已经集成到horizon中，而通过java客户端则需要先安装相应的软件。为了方便用户访问虚拟机，nova通过有一个proxy来实现，proxy通常同nova-api一起部署。

vnc访问的实现方法如下，首先是启动一个虚拟机时启用vnc，这可以通过给kvm加上vnc参数即可。这样，kvm就会启动一个vncserver监听虚拟机。通过websocket来访问虚拟时，其步骤如下：

1.通过nova-api获取访问url，url的格式是： http://ip:port/?token=xxx ，该地址实际上就是vnc proxy的地址。

2.浏览器连接到vnc proxy

3.vnc proxy连接到nova-consoleauth来验证token，并将token映射到虚拟机所在的宿主机的ip地址和某个端口，该端口就是虚拟机启动时所监听的端口。

4.vnc proxy与虚拟机所在的宿主机的vncserver建立连接，并开始代理，直到浏览器session结束。

在nova.conf中，计算节点可以指定vncserver的监听地址及vnc proxy应该通过那个地址连接到vncserver，该选项就是vncserver_proxyclient_address。vnc proxy充当了公网和计算节点之间的桥梁，此外还需要对vnc协议进行封装。

vnc proxy配置方法
通常情况下，为了提供完整的vnc功能，需要部署三个服务：

• nova-consoleauth: 提供token验证，维护token与ip地址、端口号的映射。

• nova-novncproxy: 支持基于浏览器的vnc 客户端，通常与nova-api部署在一起。

• nova-xvpvncproxy: 支持基于java的vnc客户端，，通常与nova-api部署在一起。

此外还需要对计算节点进行设当的配置。具体如下：

• vnc_enabled=True   启用虚拟机的vnc功能。

• vncserver_listen=0.0.0.0   默认是127.0.0.1，即只可以从本机进行访问，通常情况下是配置为管理网的IP地址。设置为0.0.0.0主要是考虑到动态迁移时，目的宿主机没有相应的IP地址，动态迁移会失败。

• vncserver_proxyclient_address  该地址指明vnc proxy应该通过那个IP地址来连接vncserver，通常是管理网IP地址。

• novncproxy_base_url=http://$SERVICE_HOST:6080/vnc_auto.html  指定浏览器client应该连接的地址。$SERVICE_HOST通常是一个公网IP地址。

• xvpvncproxy_base_url=http://$SERVICE_HOST:6081/console  指定java client应该连接的地址。$SERVICE_HOST通常是一个公网IP地址。

vnc proxy的配置则相对简单，只需要设置其监听的主机和端口即可。具体如下：

• novncproxy_host=$SERVICE_HOST  通常为一个公网IP。

• novncproxy_host=6080

• xvpvncproxy_host=$SERVICE_HOST 通常为一个公网IP。

• xvpvncproxy_port=6081
