# ssh学习笔记

## ssh服务端的安装

Redhat、CentOS等使用RPM包发行版

```bash
# 安装服务端
yum install openssh-server
# 安装客户端
yum install openssh-client
# 同时安装服务端、客户端
yum install openssh-server openssh-client
```

Debian、Ubuntu等使用RPM包发行版

```bash
# 安装服务端
apt-get install openssh-server
# 安装客户端
apt-get install openssh-client
# 同时安装服务端、客户端
apt-get install openssh-server openssh-client
```



## ssh的认证方式与访问策略

### 认证方式

1. ssh基于口令认证方式
   命令格式：

   ```bash
   ssh options username@host 'command'
   ```

2. ssh基于密钥认证方式

   1. 生成密钥与公钥文件
      ssh-keygen命令可以生存公钥与密钥
   2. 将公钥文件加入主机的认证文件中
      cat id_ras.pub >> ~/.ssh/authorized_keys

   **注意：.ssh目录权限为700， authorized_keys文件为600**

### 限制访问方式

1. 基于用户限制访问
   在/etc/ssh/sshd_config总添加：`Denyusers		指定用户名`

2. 基于ip地址限制访问
    1）iptables防火墙策略（针对端口）

   ```
   # 允许IP为192.168.108.1访问22端口
   iptables -A INPUT -p tcp --dport 22 -s 192.168.108.1/32 -j ACCEPT
   # 阻止所有ip访问22端口
   iptables -A INPUT -p tcp --dport 22 -j DROP
   ```

   2）TCP Wrapps（推荐这种）

   在/etc/hosts.allow文件添加如下，允许指定ip机器或网段访问某个服务。

   `sshd:192.168.108.1/255.255.255.255`

   在/etc/hosts.deny文件添加如下，拒绝指定ip机器或网段访问某个服务。

   `sshd:ALL 或 sshd:All EXCEPT 192.169.108.1`

   **注意：TCP Wrapps只支持长格式掩码，不能用192.168.0.0/24

## ssh运维常用参数

#### ssh构建跳板隧道

hostA可以随意访问，hostB只允许hostA访问，用ssh构建从本地访问hostB通道，命令格式如下：

```bash
ssh -t hostA ssh hostB
```

示例：

```bash
ssh -t rootA@192.168.1.101 ssh rootB@192.168.1.100
```

#### ssh调试参数

调试模式：

```bash
ssh -v username@host
```

-v以类似log的形式返回debug信息，方便在连接ssh出错时，快速定位问题。

#### 构建Socket5代理

实战实例：

hostA可访问www.website.com, hostB无法直接访问该站点。现要求hostB可以访问该站点。

> 本机执行命令：ssh username@hostA -D 2015
> 注意，以上2015是自定义的，可随意。成功建立ssh隧道后，在浏览器中（IE不支持Socket5代理，推荐使用Firefox）设置Socket5代理，填写指定的2015端口，即可通过hostA网络获取本地受限的网络资源

## windows ssh客户端

一般使用putty、secureCRT、Xshell、winSCP