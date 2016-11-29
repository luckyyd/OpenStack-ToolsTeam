首先查看nova-novncproxy的状态：

1. /etc/init.d/openstack-nova-novncproxy status
2. openstack-nova-novncproxy dead but pid file exists


然后debug输出看看：

     [root@controller ~]# /usr/bin/nova-novncproxy --debug
    	Traceback (most recent call last):
    	    File "/usr/bin/nova-novncproxy", line 10, in <module>
    	    sys.exit(main())
    	    File "/usr/lib/python2.6/site-packages/nova/cmd/novncproxy.py", line 87, in main
    	    wrap_cmd=None)
    	  File "/usr/lib/python2.6/site-packages/nova/console/websocketproxy.py", line 47, in __init__
    	    ssl_target=None, *args, **kwargs)
    	  File "/usr/lib/python2.6/site-packages/websockify/websocketproxy.py", line 231, in __init__
    	    websocket.WebSocketServer.__init__(self, RequestHandlerClass, *args, **kwargs)
    	TypeError: __init__() got an unexpected keyword argument 'no_parent'


错误原因：

websockify版本太低，升级websockify版本即可。


解决措施：

1.	pip install websockify==0.5.1
