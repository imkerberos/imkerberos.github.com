---
layout: post
title: uwsgi listen queue 队列溢出的问题
tags: [Debian, Python]
catgory: tips
---

今天使用 uWSGI 启动 Python Web应用程序, 使用如下指令:

{: title = "uWSGI 启动 Python应用"}

{: .lang-sh}
    #usr/local/bin/uwsgi --paste config:/home/kerberos/testapp/production.ini --socket :50%(process_num)02d -H /home/kerberos/sandbox --workers 4 --master --reload-mercy 64 --max-requests 8192 --listen 2048 --socket-timeout 30 --disable-logging 

但是当查看 uwsgi 的日志时发现错误:

{: title = "uWSGI 错误日志"}

*** uWSGI listen queue of socket 4 full !!! (129/128) ***

但是明明指定了 `--listen 2048`, 看来是没有生效. 最后查看系统配置:

{: title = "查看系统选项"}

{: .lang-sh}
    kerberos@avm01:~$ sudo sysctl -a | grep net.core.somaxconn
    net.core.somaxconn = 128
    error: permission denied on key 'net.ipv4.route.flush'
    error: permission denied on key 'net.ipv6.route.flush'

看来是系统 net.core.somaxconn 缺省的 128 限制导致 uWSGI 的 `--listen 2048` 参数没有生效生效. 还好, 编辑 `/etc/sysctl.d/sysctl_kerberos_webapp.conf` :

{: title = "sysctl_kerberos.conf"}

    net.core.somaxconn = 2048

执行命令

{: title = "sysctl 生效"}

    kerberos@avm01:~$ sudo sysctl -p /etc/sysctl.d/sysctl_adview.conf 
    net.core.somaxconn = 2048
    kerberos@avm01:~$ sudo sysctl -a | grep net.core.somaxconn 
    net.core.somaxconn = 2048
    error: permission denied on key 'net.ipv4.route.flush'
    error: permission denied on key 'net.ipv6.route.flush'

已经生效, 并且在 Debian 上开机会自动生效. 
