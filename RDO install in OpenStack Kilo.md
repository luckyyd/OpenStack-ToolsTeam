问题：Mongodb 连接失败

解决方案如下：

    I have noticed several questions (ask.openstack.org,stackoverflow.com [ 1 ],[ 2 ])
    regarding mentioned ongoing issue with mongodb-server and nagios when
    installing RDO Kilo 2015.1.1 on CentOS 7.2 via packstack. At the moment
    I see a hack provided bellow which might be applied as pre-installation step
    or fix after initial packstack crash. Bug submitted to bugzilla.redhat.com

    Due to https://bugzilla.redhat.com/show_bug.cgi?id=1296844
    to avoid packstack crash during mongodb.pp run, perform as root

    1. Download rdo-release-kilo-1.noarch.rpm
    2. # rpm -iv   rdo-release-kilo-1.noarch.rpm
    3. # yum -y install openstack-packstack
    4. # yum -y install mongodb-server python-pymongo
    5. # cd /etc
    6. # rm -f mongodb.conf
    7. # touch -f mongod.conf
    8. # ln -s /etc/mongod.conf mongodb.conf

    ************************
    Verify output :-
    ************************
    [root@ip-192-169-142-57 etc]# ls -l mongo*

    lrwxrwxrwx. 1 root root   16 Jan  8 15:57 mongodb.conf -> /etc/mongod.conf
    -rw-r--r--. 1 root root 1943 Dec 26  2014 mongodb-shard.conf
    -rw-r--r--. 1 root root    0 Jan  8 15:56 mongod.conf

    9.  # cd
    10.# packstack --gen-answe-file answerAIO.txt

    ******************************* 
    11. Update answerAIO.txt
    *******************************

    CONFIG_KEYSTONE_SERVICE_NAME=httpd

    12. #  packstack --answer-file=./answerAIO.txt

    [root@ip-192-169-142-57 ~]# packstack --answer-file=./answerAIO.txt
    Welcome to the Packstack setup utility

    The installation log file is available at: /var/tmp/packstack/20160108-160051-vRoOZ_/openstack-setup.log

    Installing:
    Clean Up                                             [ DONE ]
    Discovering ip protocol version                      [ DONE ]
    Setting up ssh keys                                  [ DONE ]
    Preparing servers                                    [ DONE ]
	Pre installing Puppet and discovering hosts' details [ DONE ]
	Adding pre install manifest entries                  [ DONE ]
	Setting up CACERT                                    [ DONE ]
	Adding AMQP manifest entries                         [ DONE ]
	Adding MariaDB manifest entries                      [ DONE ]
	Fixing Keystone LDAP config parameters to be undef if empty[ DONE ]
	Adding Keystone manifest entries                     [ DONE ]
	Adding Glance Keystone manifest entries              [ DONE ]
	Adding Glance manifest entries                       [ DONE ]
	Adding Cinder Keystone manifest entries              [ DONE ]
	Checking if the Cinder server has a cinder-volumes vg[ DONE ]
	Adding Cinder manifest entries                       [ DONE ]
	Adding Nova API manifest entries                     [ DONE ]
	Adding Nova Keystone manifest entries                [ DONE ]
	Adding Nova Cert manifest entries                    [ DONE ]
	Adding Nova Conductor manifest entries               [ DONE ]
	Creating ssh keys for Nova migration                 [ DONE ]
	Gathering ssh host keys for Nova migration           [ DONE ]
	Adding Nova Compute manifest entries                 [ DONE ]
	Adding Nova Scheduler manifest entries               [ DONE ]
	Adding Nova VNC Proxy manifest entries               [ DONE ]
	Adding OpenStack Network-related Nova manifest entries[ DONE ]
	Adding Nova Common manifest entries                  [ DONE ]
	Adding Neutron FWaaS Agent manifest entries          [ DONE ]
	Adding Neutron LBaaS Agent manifest entries          [ DONE ]
	Adding Neutron API manifest entries                  [ DONE ]
	Adding Neutron Keystone manifest entries             [ DONE ]
	Adding Neutron L3 manifest entries                   [ DONE ]
	Adding Neutron L2 Agent manifest entries             [ DONE ]
	Adding Neutron DHCP Agent manifest entries           [ DONE ]
	Adding Neutron Metering Agent manifest entries       [ DONE ]
	Adding Neutron Metadata Agent manifest entries       [ DONE ]
	Checking if NetworkManager is enabled and running    [ DONE ]
	Adding OpenStack Client manifest entries             [ DONE ]
	Adding Horizon manifest entries                      [ DONE ]
	Adding Swift Keystone manifest entries               [ DONE ]
	Adding Swift builder manifest entries                [ DONE ]
	Adding Swift proxy manifest entries                  [ DONE ]
	Adding Swift storage manifest entries                [ DONE ]
	Adding Swift common manifest entries                 [ DONE ]
	Adding Provisioning Demo manifest entries            [ DONE ]
	Adding Provisioning Glance manifest entries          [ DONE ]
	Adding MongoDB manifest entries                      [ DONE ]
	Adding Redis manifest entries                        [ DONE ]
	Adding Ceilometer manifest entries                   [ DONE ]
	Adding Ceilometer Keystone manifest entries          [ DONE ]
	Adding Nagios server manifest entries                [ DONE ]
	Adding Nagios host manifest entries                  [ DONE ]
	Adding post install manifest entries                 [ DONE ]
	Copying Puppet modules and manifests                 [ DONE ]
	Applying 192.169.142.57_prescript.pp
	192.169.142.57_prescript.pp:                         [ DONE ]          
	Applying 192.169.142.57_amqp.pp
	Applying 192.169.142.57_mariadb.pp
	192.169.142.57_amqp.pp:                              [ DONE ]        
	192.169.142.57_mariadb.pp:                           [ DONE ]        
	Applying 192.169.142.57_keystone.pp
	Applying 192.169.142.57_glance.pp
	Applying 192.169.142.57_cinder.pp
	192.169.142.57_keystone.pp:                          [ DONE ]         
	192.169.142.57_cinder.pp:                            [ DONE ]         
	192.169.142.57_glance.pp:                            [ DONE ]         
	Applying 192.169.142.57_api_nova.pp
	192.169.142.57_api_nova.pp:                          [ DONE ]         
	Applying 192.169.142.57_nova.pp
	192.169.142.57_nova.pp:                              [ DONE ]     
	Applying 192.169.142.57_neutron.pp
	192.169.142.57_neutron.pp:                           [ DONE ]        
	Applying 192.169.142.57_osclient.pp
	Applying 192.169.142.57_horizon.pp
	192.169.142.57_osclient.pp:                          [ DONE ]         
	192.169.142.57_horizon.pp:                           [ DONE ]         
	Applying 192.169.142.57_ring_swift.pp
	192.169.142.57_ring_swift.pp:                        [ DONE ]           
	Applying 192.169.142.57_swift.pp
	Applying 192.169.142.57_provision_demo.pp
	Applying 192.169.142.57_provision_glance
	192.169.142.57_swift.pp:                             [ DONE ]               
	192.169.142.57_provision_demo.pp:                    [ DONE ]               
	192.169.142.57_provision_glance:                     [ DONE ]               
	Applying 192.169.142.57_mongodb.pp
	Applying 192.169.142.57_redis.pp
	192.169.142.57_mongodb.pp:                           [ DONE ]        
	192.169.142.57_redis.pp:                             [ DONE ]        
	Applying 192.169.142.57_ceilometer.pp
	192.169.142.57_ceilometer.pp:                        [ DONE ]           
	Applying 192.169.142.57_nagios.pp
	Applying 192.169.142.57_nagios_nrpe.pp
	192.169.142.57_nagios.pp:                            [ DONE ]            
	192.169.142.57_nagios_nrpe.pp:                       [ DONE ]            
	Applying 192.169.142.57_postscript.pp
	192.169.142.57_postscript.pp:                        [ DONE ]           
	Applying Puppet manifests                            [ DONE ]
	Finalizing                                           [ DONE ]
	
	 **** Installation completed successfully ******
	
	Additional information:
	 * Time synchronization installation was skipped. Please note that unsynchronized time on server instances might be problem for some OpenStack components.
	 * Warning: NetworkManager is active on 192.169.142.57. OpenStack networking currently does not work on systems that have the Network Manager service enabled.
	 * File /root/keystonerc_admin has been created on OpenStack client host 192.169.142.57. To use the command line tools you need to source the file.
	 * To access the OpenStack Dashboard browse to http://192.169.142.57/dashboard .
	Please, find your login credentials stored in the keystonerc_admin in your home directory.
	 * To use Nagios, browse to http://192.169.142.57/nagios username: nagiosadmin, password: 8596f9100d0d48d0
	 * The installation log file is available at: /var/tmp/packstack/20160108-160051-vRoOZ_/openstack-setup.log
	 * The generated manifests are available at: /var/tmp/packstack/20160108-160051-vRoOZ_/manifests
	[root@ip-192-169-142-57 ~]# vi answerAIO.txt
	You have mail in /var/spool/mail/root
	[root@ip-192-169-142-57 ~]# . keystonerc_admin
	
	[root@ip-192-169-142-57 ~(keystone_admin)]# nova-manage version
	2015.1.1-1.el7
	
	[root@ip-192-169-142-57 ~(keystone_admin)]# netstat -antp | grep mongod
	
	tcp        0      0 192.169.142.57:27017    0.0.0.0:*               LISTEN      6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56898    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56883    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56897    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56884    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56895    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56896    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56900    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56899    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56906    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56904    ESTABLISHED 6557/mongod         
	tcp        0      0 192.169.142.57:27017    192.169.142.57:56905    ESTABLISHED 6557/mongod
	     
	[root@ip-192-169-142-57 ~(keystone_admin)]# ps -ef | grep 6557
	mongodb   6557     1  0 16:22 ?        00:00:01 /usr/bin/mongod --quiet -f /etc/mongodb.conf run

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/RDO-install-in-OpenStack-Kilo-1.png)


    [root@ip-192-169-142-57 ~(keystone_admin)]# cat /etc/mongodb.conf | grep -v ^#|grep -v ^$
    logpath=/var/log/mongodb/mongodb.log
    logappend=true
    bind_ip = 192.169.142.57
    fork=true
    dbpath=/var/lib/mongodb
    pidfilepath=/var/run/mongodb/mongod.pid
    journal = true
    noauth=true
    smallfiles = true

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/RDO-install-in-OpenStack-Kilo-2.png)
