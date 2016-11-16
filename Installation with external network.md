版本：OpenStack Liberty (latest version)

These instructions have been tested on Centos 7.

Reminder: If you deploy an all-in-one installation. Remember to keep Centos root folder "/" disk space large enough to load and spawn Windows Server Image.

###Step 1: Software repositories

Update your current packages:

` sudo yum update -y `

Setup the RDO repositories:

`sudo yum install -y https://rdoproject.org/repos/rdo-release.rpm`

###Step 2: Install Packstack Installer

`sudo yum install -y openstack-packstack`

###Step 3: Run Packstack to install OpenStack
Packstack takes the work out of manually setting up OpenStack. For a single node OpenStack deployment, run the following command.

`packstack --allinone --provision-demo=n`

###Step 4: CentOS (Host) Network Configuration

After completion, given a single machine with a current IP of 10.172.15.98/23 via DHCP with gateway of 10.172.14.1:

Make /etc/sysconfig/network-scripts/ifcfg-br-ex resemble: (note this file will exist, and IPADDR/NETMASK will be populated with _br_ex at the end, remove that part, and fill all the missing fields)

If the file is not listed in network-scripts. Create the file ifcfg-br-ex.

`DEVICE=br-ex`

`DEVICETYPE=ovs`

`TYPE=OVSBridge`

`BOOTPROTO=static`

`IPADDR=10.172.15.98  # Old eth0 IP since we want the network restart to not kill the connection, otherwise pick something outside your dhcp range`
`NETMASK=255.255.254.0  # your netmask`

`GATEWAY=10.172.14.1  # your gateway`

`DNS1=10.172.11.45     # your nameserver`

`ONBOOT=yes`

This file will configure the network parameters we probably had into our eth0 interface but, over br-ex.

Make /etc/sysconfig/network-scripts/ifcfg-eth0 resemble (no BOOTPROTO!):

Note: if on Centos7, the file could be /etc/sysconfig/network-scripts/enp2s0 or  /etc/sysconfig/network-scripts/em1

`DEVICE=eth0`

`HWADDR=52:54:00:92:05:AE # your hwaddr (find via ip link show eth0)`

`TYPE=OVSPort`

`DEVICETYPE=ovs`

`OVS_BRIDGE=br-ex`

`ONBOOT=yes`

This means, we will bring up the interface, and plug it into br-ex OVS bridge as a port, providing the uplink connectivity.

Modify the following config parameter:

Note: The Official guide in this part is wrong. Please follow configuration in this doc.

`openstack-config --set /etc/neutron/plugins/ml2/openvswitch_agent.ini ovs bridge_mappings extnet:br-ex`

This will define a logical name for our external physical L2 segment, as "extnet", this will be referenced as a provider network when we create the external networks.

This one will overcome a packstack deployment bug where only vxlan is made available.

`openstack-config --set /etc/neutron/plugin.ini ml2 type_drivers vxlan,flat,vlan`

Restart the network service

`reboot`

NOTE: It is important to do the network restart before setting up the router gateway below, because a network restart takes destroys and recreates br-ex which causes the router's interface in the qrouter-* netns to be deleted, and it won't be recreated without clearing and re-setting the gateway.


###Step 5: Openstack Network Configuration

`# . keystonerc_amidn`

`# neutron net-create external_network --provider:network_type flat --provider:physical_network extnet  --router:external --shared`

Please note: "extnet" is the L2 segment we defined in the bridge_mappings above (plugin.ini file, ml2 section).

You need to recreate the public subnet with an allocation range outside of your external DHCP range and set the gateway to the default gateway of the external network.

Please note: 192.168.122.1/24 is the router and CIDR we defined in /etc/sysconfig/network-scripts/ifcfg-br-ex for external connectivity.

`# neutron subnet-create --name public_subnet --enable_dhcp=False --allocation-pool=start=10.172.15.190,end=10.172.15.200 --gateway=10.172.14.1 external_network 10.172.15.0/23`

`# neutron router-create router1`

`# neutron router-gateway-set router1 external_network`

Now create a private network and subnet, since that provisioning has been disabled:

`# neutron net-create private_network`

`# neutron subnet-create --name private_subnet private_network 192.168.100.0/24`

And connect this private network to the public network via the router, which will provide the floating IP addresses.

`# neutron router-interface-add router1 private_subnet`


The Router connects external network and internal network:

![network](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Network.png)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Router_details.png)

The Neutron network topology is shown as follows:

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Topology.png)

###Step 6: Load Windows Image and Run!

Download a windows server image from the link: [cloudbase](https://cloudbase.it/windows-cloud-images/)

In OpenStack dashboard, load the windows server image (qcow2 format) and create instance.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Instance.png)

Finally, for your user, you need to create a network and connect that network through a router to your shared and external network. Since you don't created a user yet

`keystone tenant-create --name internal --description "internal tenant" --enabled true`

`keystone user-create --name internal --tenant internal --pass "foo" --email bar@corp.com --enabled true`

Easiest way to the network and to launch instances is via horizon, which was set up by packstack.

You should now be able to follow the steps at running an instance with Neutron to launch an instance with external network access as admin, if you want other tenants you may need to create them manually.



