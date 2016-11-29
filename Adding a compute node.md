需要控制节点有额外的网卡连接private网络，例如：

The controller node: (All functions include: identity, compute, network, dashboard etc)

* Hostname: controller0
* Public ip: 10.172.15.55
* Private ip: 192.168.100.10 为了和计算节点通信


计算节点可以不用连接外网，内网的IP需要与控制节点的内网IP在一个网段即可，在安装前需要保证两个IP能相互ping通

The compute node: (Only compute)

* Hostname: localhost.localdomain (default hostname)
* Public ip: (not set)
* Private ip: 192.168.100.20 (connect with controller)

10.156.64.70

Expanding your single-node OpenStack cloud to include a second compute node requires a second network adapter: in order for our pair of nodes to share the same private network, we must replace the "lo" interface we used for the private network with a real nic.

###Backup the answer file
­ 
    [root@controller ~]# cp /root/answers.txt /root/answers.txt.backup

###Edit the answer file 

First, you must edit the "answer file" generated during the initial packstack setup. You'll find it in the directory from which you ran packstack.

NOTE: by default $youranswerfile is called packstack-answer-$date-$time.txt

    $EDITOR $youranswerfile

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Adding-a-compute-node-1.png)

Replace $EDITOR with your preferred editor.
###Adjust network card names
Change both CONFIG_NOVA_COMPUTE_PRIVIF and CONFIG_NOVA_NETWORK_PRIVIF from lo to eth1 or whatever name your network card uses.

Your second NIC may have a different name. You can find the names of your devices by running:
    ifconfig | grep '^\S'
###Change IP addresses

Change the value for CONFIG_COMPUTE_HOSTS from the value of your first host IP address to The value of your second host IP address. 

Ensure that the key CONFIG_NETWORK_HOSTSexists and is set to the IP address of your first host.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Adding-a-compute-node-2.png)

###Skip installing on an already existing servers

In case you do not want to run the installation over again on the already configured servers, add the following parameter to the answerfile:
  EXCLUDE_SERVERS=<serverIP>,<serverIP>,…
  
![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Adding-a-compute-node-3.png)

###Re-run packstack with the new values
Run packstack again, specifying your modified answer file:

NOTE: by default $youranswerfile is called packstack-answer-$date-$time.txt
    sudo packstack --answer-file=$youranswerfile

Packstack will prompt you for the root password for each of your nodes.
