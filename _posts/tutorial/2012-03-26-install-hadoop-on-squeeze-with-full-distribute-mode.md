---
layout: post
title: 在 Debian 6.0 (Squeeze) 上完全分布式安装 Hadoop 集群系统
desc: 找遍网络文档, 很少有全部的完全使用分布式安装 Hadoop 集群系统的文章. 网络上能找到的文章大部分是基于简单的伪分布式安装的文档. 今天要在生产环境中建立集群系统, 把过程写下来分享一下.
tags: [Debian, Hadoop, HBase]
category: tutorial
---

背景
----

找遍网络文档, 很少有全部的完全使用分布式安装 Hadoop 集群系统的文章. 网络上能找到的文章大
部分是基于简单的伪分布式安装的文档. 今天要在生产环境中建立集群系统, 把过程写下来分享一下.

### Hadoop 分发版本 ###

目前流行的 Hadoop 分发版本主要有两个:

- Apache Distribution  
    自从 Hadoop 加入了 Apache Foundation 以后, Apache 的分发版本就成了 Hadoop 官方的分发
    版本. 虽然 Apache 分发是官方版本, 由于很多基于 Hadoop 的软件都在 Apache Foundation 旗
    下进行独立分发, 而很多软件之间相互依赖特定的版本, 所以在生产系统上安装一个稳定运行的
    版本集合并不容易.

- Cloudera (CDH) 分发  
    Cloudera 公司主是一家提供 Hadoop 以及基于 Hadoop 软件的服务商. Cloudera 针对 Hadoop 的
    官方版本做了很多集成以及补丁修复工作. 并且一个发布下的所有基于 Hadoop 的软件之间的版本
    依赖已经经过了大量测试, 可以说是一个比较省心的版本. 而且 Cloudera 针对不同的 Linux 发
    行版进行了打包工作,比如: Ubuntu, Debian, Redhat, Fedora, SUSE 等, 可以非常容易得安装.
    到本文发布时间为止 (2012 年 3 月 26), CDH 的分发版本是 CDH3 Update 3 (CHD3u3). CDH4 的
    测试版本已经是 beta1 了. 虽然 CHD4 对比 CDH3 有了很大的性能提高, 但是在生产环境上, 还
    是使用稳定版本比较好.  
    而且 Cloudera 的 Flume 软件现在还不支持 CDH4, 所以 CHD3 成了必然的选择. 要知道, 人最难
    做的就是选择啊. :)

### Linux 发行版 ###

相对于 Unbuntu, Redhat, SUSE 等发行版来说, 我更钟爱 [Debian][], 所以大部分服务器我都是在
Debian 上安装的. 截至到本文发布时间为止 (2012 年 3 月 26), Debian 的版本是 6.0.4, 版本代号
Squeeze.

### CDH3 安装方式 ###

虽然 CDH3 针对不同的 Linux 发行版提供了已经打好的包, 包括 Debian 的 APT 包管理器, 由于我开
发软件使用了 Buildout 自动部署方式, 所以, 使用 APT 安装方式与我们的项目部署方式还不是特别
一致. 所以我选择了 CHD3 的 tar.gz 包的最原始的安装方式, 其实这种方式和 Apache 的分发包没有
太大区别, 不过对于安装 HBase, Flume 等软件来说, 就不用为特定版本的依赖操心了.

### Squeeze 安装 ###

集群中的每台机器先安装好基本的 Debian 系统, 需要说明的是, Hadoop 最
好还是使用 **x86\_64** 的系统.

安装过程
--------

### 安装的软件 ###

在集群中我们使用了以下的软件:

- Hadoop 相关
    - Zookeeper
    - Hadoop
    - HBase
    - Hive

- 集群依赖的其他软件
    - dnsmasq  
        由于 HBase 配置 HDFS 路径时无法使用 ip 地址访问, 所以必须要使用机器名, 然而使用机器的
        Hostname 必然要同步集群中所有机器的 hosts 文件, 而这个文件的修改必须要 root 权限. 在同
        步上不方便. 而且每次 hosts 修改都要同步到集群中的所有机器上, 为了方便管理集群, 在集群
        内部建立了自己使用的 DNS 服务器, dnsmasq 是一个比较好的选择. 安装指令:
            #apt-get install dnsmasq
    - openssh-server  
        管理集群内的机器需要, 服务端口号使用默认的 **22**, 不要修改成其他的端口. 因为 Hadoop 的
        管理指令自动连接 **22** 端口, 如果修改了这个端口号, 必须要修改 Hadoop 的管理脚本. 另外,
        为了避免在使用 Hadoop 管理脚本时连接其他机器时的登陆密码输入, 需要使用 *ssh-keygen* 命
        令建立统一的 SSH 密钥对.
            #apt-get install openssh-server openssh-client
    - ntpdate  
        对于集群来说, 各个节点的时间必须保持一致. 很多情况下, 集群中的各个节点在统一的 VLAN 中,
        可能无法访问外网, 所以有一个时间同步服务器是必要的.
            #apt-get install ntpdate
    - rsync  
        Hadoop 管理脚本需要使用 rsync 命令来同步集群的配置, 这个软件是必须的.
            #apt-get install rsync

- 其他辅助性的软件
    - screen
    - vmstat
    - ifstat
    - lsof

### 各节点系统配置

#### 用户

各节点的管理用户统一命名. 可以起一个比较贴切的名字. 比如 `imkerberos`

#### 性能调整

- `/etc/security/limits.conf` 或者 `/etc/security/limits.d/`  
    主要调整系统资源限制选项, 包括
    - 进程数限制
    - 打开文件数量

        <pre>
        # file: /etc/security/limits.d/mycluster.conf
        # 首先修改用户 imkerberos 的硬限制, 再改软限制
        imkerberos hard nproc unlimited # 无限制
        imkerberos soft nproc unlimited # 无限制
        imkerberos hard nofiles 65536
        imkerberos soft nofiles 65536
        </pre>

- `/etc/sysctl.conf` 或者 `/etc/sysctl.conf.d`  
    主要调整网络相关的参数选项, 包括
    - 本地端口范围
    - TCP TIME\_WAIT 等

        <pre>
        # file: /etc/sysctl.conf.d/mycluster.conf
        # 缺省是 40000 65000, 扩大本地可用端口号, 注意其他服务器不要监听在这些端口号上
        net.ipv4.local_port_range = 2048 65535
        net.ipv4.tcp_max_tw_buckets = 524288
        net.ipv4.tcp_max_syn_backlog = 8192
        net.ipv4.netfilter.ip_conntrack_max = 524288
        net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 180
        net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 5
        </pre>

- `/etc/defaults/*`  
    `/etc/security/limits.conf` 系统资源限制只是在用户登陆以后才会生效, 其机制是通过
    `PAM` 插件实现的, 如果需要修改某些守护进程的资源限制, 需要修改 `/etc/defaults/`
    目录下相应服务的配置文件, 例如: `/etc/defaults/nginx`

        ulimit -Hn 65536 
        ulimit -Hs 65536
        ulimit -Hu unlimited
        ulimit -Su unlimited

### 节点部署

生产环境中的机器共 8 台, 主要角色如下划分:

- Hadoop Jobtracker, Hadoop NameNode 与 HBase Master 2 台, 一台作为主节点, 另外一台作为备份节点.
- Hadoop Tasktracker, Hadoop DataNode 与 HBase RegionServer 5 台
- 应用服务器 1 台

具体的分配表如下

|组件名称 | 服务器1 | 服务器2 | 服务器3  | 服务器4 | 服务器5 | 服务器6 | 服务器7 | 服务器8|
|:------- |:--------|:--------|:---------|:--------|:--------|:--------|:--------|:-------|
|Jobtrac r| OK      |   OK    |   OK     |    OK   | OK      | OK      | OK      | OK     |

[Debian]: http://www.debian.org "Debian"
