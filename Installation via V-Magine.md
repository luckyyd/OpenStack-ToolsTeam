V-magine is an OpenStack deployment solution using your favorite virtualization platforms.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/V-Magine-1.png)

V-Magine是安装在Windows上的OpenStack解决方案，可以结合Windows Hyper-V 使用；

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/V-Magine-2.png)

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/V-Magine-3.png)

网上提供视频教程：
https://www.youtube.com/watch?v=TsESYPz3uGo

安装过程遇到error：

    10.170.129.3_nagios.pp:                           [ ERROR ]                                                                             
    Applying Puppet manifests                         [ ERROR ]                                                                             

    ERROR : Error appeared during Puppet run: 10.170.129.3_nagios.pp                                                                        
    Error: Could not start Service[nagios]: Execution of '/usr/bin/systemctl start nagios' returned 1: Job for nagios.service failed. See 's
    ystemctl status nagios.service' and 'journalctl -xn' for details.                                                                       
    You will find full trace in log /var/tmp/packstack/20151210-083022-DSzOTa/manifests/10.170.129.3_nagios.pp.log                          
    Please check log file /var/tmp/packstack/20151210-083022-DSzOTa/openstack-setup.log for more information                                
Additional information:                                                                                                                 
    * Warning: NetworkManager is active on 10.170.129.3, 10.172.14.39. OpenStack networking currently does not work on systems that have the Network Manager service enabled.                                                                                                      
    * File /root/keystonerc_admin has been created

未解决，无相关解决方案
