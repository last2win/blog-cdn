---
layout: post
title: "Linux/ubuntu 安装 redis 4.0报错解决：redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file or directory"
categories: [行走的问题解决机]
description: "Linux/ubuntu 安装 redis 4.0报错解决：redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file"
keywords: Linux, redis
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

晚上在我的ubuntu 18.04的服务器上安装redis时报错如下：

```sh
Job for redis-server.service failed because a timeout was exceeded.
See "systemctl status redis-server.service" and "journalctl -xe" for details.
invoke-rc.d: initscript redis-server, action "start" failed.
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; disabled; vendor preset: enabled)
   Active: activating (auto-restart) (Result: timeout) since Wed 2020-01-22 19:09:23 CST; 14ms ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 15450 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)

Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: Failed to start Advanced key-value store.
Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: redis-server.service: Service hold-off time over, scheduling restart.
Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: redis-server.service: Scheduled restart job, restart counter is at 1.
Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: Stopped Advanced key-value store.
Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: Starting Advanced key-value store...
Jan 22 19:09:23 VM-0-15-ubuntu systemd[1]: redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file or directory
dpkg: error processing package redis-server (--configure):
 installed redis-server package post-installation script subprocess returned error exit status 1
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for systemd (237-3ubuntu10.33) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Processing triggers for ureadahead (0.100.0-21) ...
Errors were encountered while processing:
 redis-server
E: Sub-process /usr/bin/dpkg returned an error code (1)

```
```sh
-> # service redis-server status 
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; disabled; vendor preset: enabled)
   Active: activating (start) since Thu 2020-01-23 08:12:09 CST; 55s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 22669 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
    Tasks: 0 (limit: 2126)
   CGroup: /system.slice/redis-server.service

Jan 23 08:12:09 VM-0-15-ubuntu systemd[1]: redis-server.service: Service hold-off time over, scheduling restart.
Jan 23 08:12:09 VM-0-15-ubuntu systemd[1]: redis-server.service: Scheduled restart job, restart counter is at 520.
Jan 23 08:12:09 VM-0-15-ubuntu systemd[1]: Stopped Advanced key-value store.
Jan 23 08:12:09 VM-0-15-ubuntu systemd[1]: Starting Advanced key-value store...
Jan 23 08:12:09 VM-0-15-ubuntu systemd[1]: redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file
```

网上有很多人遇到了一样的问题：[rhel6 - Redis Daemon not creating a PID file - Stack Overflow](https://stackoverflow.com/questions/25515166/redis-daemon-not-creating-a-pid-file/53161016)

解决问题的第一步是查看日志：
```sh
-># cat /var/log/redis/redis-server.log
21692:M 23 Jan 08:07:37.871 # Creating Server TCP listening socket ::1:6379: bind: Cannot assign requested address
21973:C 23 Jan 08:09:08.372 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
21973:C 23 Jan 08:09:08.374 # Redis version=4.0.9, bits=64, commit=00000000, modified=0, pid=21973, just started
21973:C 23 Jan 08:09:08.374 # Configuration loaded
21987:M 23 Jan 08:09:08.376 # Creating Server TCP listening socket ::1:6379: bind: Cannot assign requested address
22264:C 23 Jan 08:10:38.872 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
22264:C 23 Jan 08:10:38.872 # Redis version=4.0.9, bits=64, commit=00000000, modified=0, pid=22264, just started
22264:C 23 Jan 08:10:38.872 # Configuration loaded
22278:M 23 Jan 08:10:38.877 # Creating Server TCP listening socket ::1:6379: bind: Cannot assign requested address
22669:C 23 Jan 08:12:09.378 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
22669:C 23 Jan 08:12:09.379 # Redis version=4.0.9, bits=64, commit=00000000, modified=0, pid=22669, just started
22669:C 23 Jan 08:12:09.379 # Configuration loaded
22688:M 23 Jan 08:12:09.383 # Creating Server TCP listening socket ::1:6379: bind: Cannot assign requested address
23016:C 23 Jan 08:13:39.867 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
23016:C 23 Jan 08:13:39.867 # Redis version=4.0.9, bits=64, commit=00000000, modified=0, pid=23016, just started
23016:C 23 Jan 08:13:39.867 # Configuration loaded
23031:M 23 Jan 08:13:39.871 # Creating Server TCP listening socket ::1:6379: bind: Cannot assign requested address
23294:C 23 Jan 08:15:10.394 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
23294:C 23 Jan 08:15:10.394 # Redis version=4.0.9, bits=64, commit=00000000, modified=0, pid=23294, just started
23294:C 23 Jan 08:15:10.394 # Configuration loaded
```

先尝试修改绑定IP：
```sh
/etc/redis/redis.conf
```
把`bind 127.0.0.1 ::1`改为`bind 127.0.0.1`

可以正常启动了：
```sh
-> # service redis-server status        
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; disabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-01-23 08:16:37 CST; 2min 58s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 23552 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 23569 (redis-server)
    Tasks: 4 (limit: 2126)
   CGroup: /system.slice/redis-server.service
           └─23569 /usr/bin/redis-server 127.0.0.1:6379

Jan 23 08:16:37 VM-0-15-ubuntu systemd[1]: Starting Advanced key-value store...
Jan 23 08:16:37 VM-0-15-ubuntu systemd[1]: redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file
Jan 23 08:16:37 VM-0-15-ubuntu systemd[1]: Started Advanced key-value store.
```
但还是有报错`redis-server.service: Can't open PID file /var/run/redis/redis-server.pid (yet?) after start: No such file`


解决方法如下：
```sh
vi /usr/lib/systemd/system/redis.service #centos 7
nano /etc/systemd/system/redis.service  #debian/ubuntu
```
在`[Service]`下新增一行`ExecStartPost=/bin/sh -c "echo $MAINPID > /var/run/redis/redis.pid"`
```sh
[Service]
Type=forking
ExecStart=/usr/bin/redis-server /etc/redis/redis.conf
ExecStop=/bin/kill -s TERM $MAINPID
ExecStartPost=/bin/sh -c "echo $MAINPID > /var/run/redis/redis.pid"
```
随后重启服务：
```sh
sudo systemctl daemon-reload
sudo systemctl enable redis-server
sudo systemctl restart redis.service
```
报错消失：
```sh
-> # service redis-server status 
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-01-23 09:03:12 CST; 4s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 14002 ExecStop=/bin/kill -s TERM $MAINPID (code=exited, status=0/SUCCESS)
  Process: 14024 ExecStartPost=/bin/sh -c echo $MAINPID > /var/run/redis/redis.pid (code=exited, status=0/SUCCESS)
  Process: 14006 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 14023 (redis-server)
    Tasks: 4 (limit: 1108)
   CGroup: /system.slice/redis-server.service
           └─14023 /usr/bin/redis-server *:34343

Jan 23 09:03:12 VM-0-17-ubuntu systemd[1]: Stopped Advanced key-value store.
Jan 23 09:03:12 VM-0-17-ubuntu systemd[1]: Starting Advanced key-value store...
Jan 23 09:03:12 VM-0-17-ubuntu systemd[1]: Started Advanced key-value store.
```

redis已经可以正常运行：
```sh
-> # redis-cli 
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set test "test redis"
OK
127.0.0.1:6379> get test
"test redis"
```