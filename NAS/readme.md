# debian2nas

## linux

[菜鸟教程](https://www.runoob.com/linux/linux-tutorial.html)

```sh
lsb_release -a //查看虚拟机版本
uname -a //查看虚拟机位数，但是没有下一个命令直观
getconf LONG_BIT//查看long的位数，可以直接反映出虚拟机位数
```

- 查看用户信息
- hostname
- passwd
- who
- whoami
- uptime
- who -b
- last
- sudo lastb

```sh
id：为指定的用户名打印用户和组信息。
getent：从 Name Service Switch 库中获取条目。
/etc/passwd： 文件包含每个用户的详细信息，每个用户详情是一行，包含 7 个字段。
finger：用户信息查询程序
lslogins：显示系统中已有用户的信息
compgen：是 bash 内置命令，它将显示用户的所有可用命令。
```

```sh
lsusb
blkid 命令：定位或打印块设备的属性。
lsblk 命令：列出所有可用的或指定的块设备的信息。
hwinfo 命令：硬件信息工具，是另外一个很好的实用工具，用于查询系统中已存在硬件。
udevadm 命令：udev 管理工具
tune2fs 命令：调整 ext2/ext3/ext4 文件系统上的可调文件系统参数。
dumpe2fs 命令：查询 ext2/ext3/ext4 文件系统的信息。
使用 by-uuid 路径：该目录下包含有 UUID 和实际的块设备文件，UUID 与实际的块设备文件链接在一起
df
```

```sh
# 查询本机外网IPv4地址
curl 4.ipw.cn

# 查询本机外网IPv6地址
curl 6.ipw.cn

# 测试网络是IPv4还是IPv6访问优先(访问IPv4/IPv6双栈站点，如果返回IPv6地址，则IPv6访问优先)
curl test.ipw.cn

ifconfig

iwinfo
```

```sh
# email
date | s-nail -s data xxx@xxx.com
```

### 挂载异常

```sh
sudo dmesg | grep usb
umount -a
fusermount -u path
mount -a

```

### tmux

### 使用远程桌面

```sh
#通过windows自带的远程桌面软件，可以远程访问树莓派桌面应用
sudo apt-get install xrdp
```

### Fail2Ban

Fail2Ban是一款入侵防御软件，可以保护服务器免受暴力攻击。 它是用 Python 编程语言编写的。 Fail2Ban 基于auth 日志文件工作，默认情况下它会扫描所有 auth 日志文件，如 /var/log/auth.log、/var/log/apache/access.log 等，并禁止带有恶意标志的IP，比如密码失败太多，寻找漏洞等等标志。通常，Fail2Ban 用于更新防火墙规则，用于在指定的时间内拒绝 IP 地址。 它也会发送邮件通知。 Fail2Ban 为各种服务提供了许多过滤器，如 ssh、apache、nginx、squid、named、mysql、nagios 等。Fail2Ban 能够降低错误认证尝试的速度，但是它不能消除弱认证带来的风险。 这只是服务器防止暴力攻击的安全手段之一。

[如何使用 fail2ban 防御 SSH 服务器的暴力破解攻击](https://cloud.tencent.com/developer/article/1511129)

配置jail.local 文件:创建一个新文件jail.local，它将覆盖jail.conf中的任何同类设置.同时将监视/var/log/auth.log，

```sh
sudo vim /etc/fail2ban/jail.local
sudo systemctl start fail2ban
sudo systemctl status fail2ban        # 查看fail2ban 是否正常启动
sudo systemctl enable fail2ban

# 日常查看 lastb 命令:
# 如何查看指定日期的 ssh 失败登陆列表:
# 查看 2022 年 10 月 16 日至 10 月 18 日的失败 ssh 登陆记录
lastb -s 2022-10-16 -t 2022-10-18

# 可以使用以下命令取消 ban 测试IP地址：
sudo fail2ban-client set sshd unbanip IP_ADDRESS
# IP_ADDRESS是禁止的IP地址
# 现在你又可以开心的使用SSH重新登录那个 fail2ban 的服务器了.​​​
```

```conf
[sshd]
enabled = true # 启用规则
filter = sshd # 使用fail2ban sshd过滤器，
action = iptables[name=SSH, port=ssh, protocol=tcp]
backend = systemd
logpath = /var/log/fail2ban.log # this logpath is the standard logpath for CentOS/RHEL - based operating systems
findtime = 8467200 #; 14 days
bantime = 604800 # ; 1 week
maxretry = 3  # 将最大重试次数设置为3, 超过就 ban 它的 ip
```

## NAS

### [宝塔面板](https://www.bt.cn/)

```sh
bt
```

### omv/openmediavault

[开源NAS系统之OpenMediaVault(OMV) 6快速上手](https://blog.xiaoz.org/archives/16499)
账号密码：admin/openmediavault

- omv6替换国内清华镜像源及extras安装教程

```sh
sudo omv-env set OMV_APT_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public"
sudo omv-env set OMV_APT_ALT_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages"
sudo omv-env set OMV_APT_KERNEL_BACKPORTS_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/debian"
sudo omv-env set OMV_APT_SECURITY_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/debian-security"
sudo omv-env set OMV_EXTRAS_APT_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/openmediavault-plugin-developers"
sudo omv-env set OMV_DOCKER_APT_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/debian"
sudo omv-env set OMV_PROXMOX_APT_REPOSITORY_URL "https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian"
sudo omv-salt stage run all
```

完成后安装extras的命令

```sh
sudo wget https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/openmediavault-plugin-developers/pool/main/o/openmediavault-omvextrasorg/openmediavault-omvextrasorg_6.3.1_all.deb
sudo dpkg -i openmediavault-omvextrasorg_6.3.1_all.deb
```

omv-firstaid是系统内置的修复工具，虽然修复功能有限但是向导式修复过程还是很简单，紧急情况下在终端输入“omv-firstaid”通常可以修复以下几项故障：

IP配置重设，有时网络配置设置失误不能联机后可以用此工具修复；
WEBGUI端口重设；
WEBGUI管理员密码重置；
OMV配置恢复，可以恢复最近一次配置文件备份；

- 安装Docker：

[OMV 6.4 安装Docker](https://zhuanlan.zhihu.com/p/634569985)

OMV 6本身是不支持Docker和虚拟机的，需要通过一个插件omv-extras-plugins来支持和安装。OMV 6安装omv-extras-plugins需要使用root用户在命令行执行：

```sh
#安装omv-extras-plugins
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash
```

然后再次回到OMV 6的WEB管理界面刷新就可以看到Docker和Portainer的选项，根据提示操作完成安装即可。

注意：此时在docker这里会显示状态，是否是Installed and Running如果不是或者显示未安装，请重新在omv-extras中取消勾选docker-repo后保存，然后重新勾选进行保存后，回到这里，就会显示正常。

可以选择docker compose或docker二选一，我比较推荐docker compose安装，维护简单，且不用打一长串的参数命令行

- 安装KVM：

安装好了omv-extras-plugins我们就可以在插件中心搜索kvm进行虚拟机插件的安装，插件全称是openmediavault-kvm

openmediavault-kvm安装完毕后，服务里面会多出一个KVM的选项，就是用来安装和管理虚拟机的。

- sharerootfs

## 内网穿透

### **ipv6**

```sh
# email
curl 6.ipw.cn | s-nail -s data msgsvr@163.com
```

#### s-nail

```sh
sudo nano /etc/s-nail.rc

# add
set from="msgsvr@163.com"   #用来发送邮件的邮箱
set smtp="smtp.163.com"
set smtp-auth-user="msgsvr@163.com"
set smtp-auth-password="..."  #邮箱授权码
set smtp-auth=login
```

### *ipv4 端口转发*

### 花生壳/oray

- 下载： <https://hsk.oray.com/download/>
- [Linux版使用教程](https://service.oray.com/question/11630.html)
- [花生壳5.0 for Linux使用教程](https://blog.csdn.net/ganky/article/details/114762461)
- phddns_5.2.0_i386.deb
- phddns回车，可以看到扩展的功能
- 浏览器访问<http://b.oray.com> ，输入花生壳Linux 5.0在安装时产生SN码与默认登录密码admin登录。

### [WireGuard](https://github.com/WireGuard)

WireGuard 的安装条件非常苛刻，对内核版本要求极高，不仅如此，在不同的系统中，内核，内核源码包，内核头文件必须存在且这三者版本要一致，Red Hat、CentOS、Fedora 等系统的内核，内核源码包，内核头文件包名分别为 kernel、kernel-devel、kernel-headers；Debian、Ubuntu 等系统的内核，内核源码包，内核头文件包名分别为 kernel、linux-headers。果这三者任一条件不满足的话，则不管是从代码编译安装还是从 repository 直接安装，也只是安装了 wireguard-tools 而已。而 WireGuard 真正工作的部分，是 wireguard-dkms，也就是动态内核模块支持(DKMS)，是它将 WireGuard 编译到系统内核中。

目前 WireGuard 已经被合并到 Linux 5.6 内核中了，如果你的内核版本 >= 5.6，就可以用上原生的 WireGuard 了，只需要安装 wireguard-tools 即可，内核版本<5.6，可能需要首先更新内核，否则可能会报错：Unable to access interface: Protocol not supported

WireGuard 优点：

- 配置精简，可直接使用默认值
- 只需最少的密钥管理工作，每个主机只需要 1 个公钥和 1 个私钥。
- 就像普通的以太网接口一样，以 Linux 内核模块的形式运行，资源占用小。
- 能够将部分流量或所有流量通过 VPN 传送到局域网内的任意主机。
- 能够在网络故障恢复之后自动重连，戳到了其他 VPN 的痛处。
- 比目前主流的 VPN 协议，连接速度要更快，延迟更低。
- 使用了更先进的加密技术，具有前向加密和抗降级攻击的能力。
- 支持任何类型的二层网络通信，例如 ARP、DHCP 和 ICMP，而不仅仅是 TCP/HTTP。
- 可以运行在主机中为容器之间提供通信，也可以运行在容器中为主机之间提供通信。

WireGuard 不能做的事：

- 类似 gossip 协议实现网络自愈。
- 通过信令服务器突破双重 NAT。
- 通过中央服务器自动分配和撤销密钥。
- 发送原始的二层以太网帧。

当然，你可以使用 WireGuard 作为底层协议来实现自己想要的功能，从而弥补上述这些缺憾。

最后，在 WireGuard 中的所有数据报文，都采用 UDP 的方式发送。

- [被Linux创始人称做艺术品的组网神器——WireGuard](https://zhuanlan.zhihu.com/p/447375895)
- [通过WireGuard搭建隧道实现内网穿透](https://www.jianshu.com/p/02ee6aa6241d)
  - [官方安装手册](https://www.wireguard.com/install/)
  - [docker安装](https://hub.docker.com/r/linuxserver/wireguard)
- [Wireguard配置文件详解](https://zhuanlan.zhihu.com/p/566892816)
- [使用 WireGuard 搭建 VPN 访问家庭内网](https://dev.admirable.pro/using-wireguard/)
- [How to setup a VPN server using WireGuard (with NAT and IPv6)](https://stanislas.blog/2019/01/how-to-setup-vpn-server-wireguard-nat-ipv6/)
- [通过WireGuard搭建隧道实现内网穿透](https://blog.csdn.net/qq_26212181/article/details/128479836)
- [WireGuard 教程：WireGuard 的搭建使用与配置详解](https://icloudnative.io/posts/wireguard-docs-practice/#-address)
- [WireGuard 搭建方法与使用教程](https://blog.starryvoid.com/archives/337.html)

一键安装：WireGuard VPN installer for [Linux servers](https://github.com/angristan/wireguard-install)：

#### 全互联模式（full mesh）

全互联模式其实就是一种网络连接形式，即所有结点之间都直接连接，不会通过第三方节点中转流量。和前面提到的点对多点架构其实是一个意思。

在 WireGuard 的世界里没有 Server 和 Client 之分，所有的节点都是 Peer。大家使用 WireGuard 的常规做法是找一个节点作为中转节点，也就是 VPN 网关，然后所有的节点都和这个网关进行连接，所有节点之间都通过这个网关来进行通信。这种架构中，为了方便理解，我们可以把网关看成 Server，其他的节点看成 Client，但实际上是不区分 Server 和 Client 的。

#### [more](https://yaoqs.github.io/)

### **[NetBird](https://netbird.io/)**

<https://github.com/netbirdio/netbird>:Connect your devices into a single secure private WireGuard®-based mesh network with SSO/MFA and simple access controls.

- NetBird combines a configuration-free peer-to-peer private network and a centralized access control system in a single platform, making it easy to create secure private networks for your organization or home.
- Connect. NetBird creates a WireGuard-based overlay network that automatically connects your machines over an encrypted tunnel, leaving behind the hassle of opening ports, complex firewall rules, VPN gateways, and so forth.
- Secure. NetBird enables secure remote access by applying granular access policies, while allowing you to manage them intuitively from a single place. Works universally on any infrastructure.

NetBird 是一个建立在WireGuard之上的开源网络管理平台，它允许计算机、设备和服务器通过快速加密隧道直接连接，无需配置或中央VPN服务器。它使专用网络变得安全，并创建了一个专用网络，在没有手动配置和专家的情况下应用安全实践。NetBird网络普遍适用于云、本地、边缘和容器环境，省去了打开端口、复杂防火墙规则和VPN网关的麻烦。

NetBird 没有集中式 VPN 服务器，您的计算机、设备、机器和服务器直接通过快速加密隧道相互连接。NetBird只需点击几下即可连接在任何地方运行的机器。使用NetBird部署安全的点对点VPN只需不到5分钟。

它与 Tailscale 很像，但是区别也比较明显。Tailscale 是在用户态实现了 WireGuard 协议，无法使用 WireGuard 原生的命令行工具来进行管理。而 NetBird 直接使用了内核态的 WireGuard，可以使用命令行工具 wg 来查看和管理。

[一款超牛逼的组网神器！吊打市面其它工具~](https://cloud.tencent.com/developer/article/2353479)

### [Netmaker](https://www.netmaker.io/)

[Netmaker](https://github.com/gravitl/netmaker) Netmaker makes networks with WireGuard. Netmaker automates fast, secure, and distributed virtual networks.是一个用来配置 WireGuard 全互联模式的可视化工具，它的功能非常强大，不仅支持 UDP 打洞、NAT 穿透、多租户，还可以使用 Kubernetes 配置清单来部署，客户端几乎适配了所有平台，包括 Linux, Mac 和 Windows，还可以通过 WireGuard 原生客户端连接 iPhone 和 Android。Netmaker 使用的是 C/S 架构，即客户端/服务器架构。Netmaker Server 包含两个核心组件：用来管理网络的可视化界面，以及与客户端通信的 gRPC Server。你也可以可以选择部署DNS服务器（CoreDNS）来管理私有DNS。客户端（netclient）是一个二进制文件，可以在绝大多数 Linux 客户端以及 macOS 和 Windows 客户端运行，它的功能就是自动管理 WireGuard，动态更新 Peer 的配置。

Netmaker 还有一个重要的术语叫签到，客户端会通过定时任务来不断向 Netmaker Server 签到，以动态更新自身的状态和 Peer 的配置，它会从 Netmaker Server 检索 Peer 列表，然后与所有的 Peer 建立点对点连接，即全互联模式。所有的 Peer 通过互联最终呈现出来的网络拓扑结构就类似于本地子网或 VPC。

Netmaker 支持多种部署方式，包括二进制部署和容器化部署，容器化部署还支持 docker-compose 和 Kubernetes。

Netmaker 允许创建任意数量的私有网络，可以设置任意地址范围。你只需要给这个网络起个名字，设置一个地址范围，并选择想要启用的选项。

- 目前总共包含三个可选项：
  - Dual Stack : 双栈，即开启 IPv6。
  - Local Only : 各个 Peer 之间只会通过内网地址来互联，即 Endpoint 皆为内网地址。适用于数据中心、VPC 或家庭/办公网络的内部。
  - Hole Punching : 动态发现和配置 Endpoint 和端口，帮助 Peer 轻松穿透 NAT 进行 UDP 打洞。

- 管理员拥有对网络的最高控制器，例如，更改私有网络的网段，Peer 便会自动更新自身的 IP。
- 如果发现网络被入侵，也可以让网络中的所有节点刷新公钥。

Node 表示节点，通常是运行 Linux 的服务器，安装了 netclient 和 WireGuard。这个节点会通过 WireGuard 私有网络和其他所有节点相连。一但节点被添加到私有网络中，Netmaker 管理员就可以操控该节点的配置.管理员也可以将该节点从私有网络中完全删除，让其无法连接其他所有 Peer 节点。

Node 还有两个比较重要的功能，就是将自身设置为 Ingress Gateway（入口网关）或者 Egress Gateway（出口网关）。Ingress Gateway 允许外部客户端的流量进入内部网络，Egress Gateway 允许将内部网络的流量转发到外部指定的 IP 范围。这两项功能对全互联模式进行了扩展，比如手机客户端就可以通过 Ingress Gateway 接入进来。

一个节点想要加入到私有网络，需要获取访问秘钥进行授权，当然你也可以选择手动批准。
一个访问秘钥可以被多个节点重复使用，你只需修改 Number 数量就可以实现这个目的。

Reference:

- [WireGuard 全互联模式终极指南（上）！](https://cloud.tencent.com/developer/article/1893909)

### ddns

#### ddns-go

动态DNS解析，支持WEB界面设置
因为一般家庭或企业用户所获得的的广域网ip并非固定，而是会时常变化，一旦变化，我们的域名针对ip的A类解析记录就会失效，因此我们需要DDNS(动态域名解析服务)，在ip变动时自动更改我们的域名解析记录值。

ddns-go项目地址：
GitHub地址：<https://github.com/jeessy2/ddns-go>
Gitee地址：<https://gitee.com/OtherCopy/ddns-go>

自动获得你的公网 IPv4 或 IPv6 地址，并解析到对应的域名服务。

[Docker部署ddns-go，动态域名解析公网IPv6地址](https://blog.csdn.net/qq_51173321/article/details/128975377)

### wireguard：一个开源的VPN软件，如果你不想将OMV服务映射到公网，这是一个必不可少的软件，可以通过这个软件连接到你的内网

## docker

### [Docker基础入门：镜像、容器导入导出与私有仓库搭建](https://blog.csdn.net/qq_41840843/article/details/132331501)

### 安装

```sh
# 卸载老旧的版本（若未安装过可省略此步）：
$ sudo apt-get remove docker docker-engine docker.io

# 安装最新的docker：
$ curl -fsSL get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh

# shell会提示你输入sudo的密码，然后开始执行最新的docker过程 或者
$ curl -sSL https://get.docker.com/ | sh

# 确认Docker成功最新的docker：
$ sudo docker run hello-world
```

更换docker国内源
单纯替换国内源，但是在创建添加 /etc/docker/daemon.json 后出现下图无法启动的问题。

原因：/etc/docker/daemon.json 文件内容格式不正确

解决方法：修改文件内容并重启docker服务

1）添加或修改文件daemon.json

nano /etc/docker/daemon.json

2）在daemon.json中添加国内镜像源

```json
{
"registry-mirrors" : [

"https://mirror.ccs.tencentyun.com",

"http://registry.docker-cn.com",

"http://docker.mirrors.ustc.edu.cn",

"http://hub-mirror.c.163.com"
],
"insecure-registries" : [

"http://registry.docker-cn.com",

"http://docker.mirrors.ustc.edu.cn"
],
"debug" : true,
"experimental" : true
}
```

===================镜像说明=========================

"<https://mirror.ccs.tencentyun.com>" //腾讯

"<http://registry.docker-cn.com>" //docker官方提供的中国镜像

"<http://docker.mirrors.ustc.edu.cn>" //中国科学技术大学

"<http://hub-mirror.c.163.com>" //网易

3）重启服务

```sh
[root@localhost]# systemctl daemon-reload
[root@localhost]# systemctl restart docker
#或者输入：
systemctl restart docker.service
```

### [Portainer 安装与使用](https://zhuanlan.zhihu.com/p/383491674)

Portainer 是一个用于管理容器化应用程序的开源工具。它在数据中心和边缘与Kubernetes、Docker、Docker Swarm、Azure ACI一起使用。Portainer 消除了与编排器相关的复杂性，因此任何人都可以管理容器。它可用于部署和管理应用程序、观察容器的行为并提供广泛部署容器所需的安全性和治理。Portainer CE（开源）受到全球超过 500,000 名用户的信赖。Portainer Business建立在开源基础之上，使组织能够大规模运行容器化应用程序，而无需雇用新人员或重新培训现有团队。

**no matching manifest for linux/386 in the manifest list entries**

#### Portainer 搭建与使用（docker）

1. 安装
1.1 创建 portainer 工作目录

```sh
// 创建文件夹
mkdir portainer

// 进入工作目录
cd ./portainer
```

1.2 pull 官方容器镜像

```sh
// 拉取镜像
docker pull portainer/portainer

// 检查是否更新成功（成功如下图）

docker images | grep portainer

```

1.3 创建docker-compose.yml 配置如下：
touch docker-compose.yml
docker-compose.yml 配置文件

```yaml
version: "3.6"
services:
    portainer-mian:
        container_name: portainer
        image: portainer/portainer:latest
        restart: always
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock:rw
            - ./ortainer_data:/data:rw
        networks:
            - traefik
        labels:
            - "traefik.enable=true"
            - "traefik.docker.network=traefik"
            - "traefik.http.routers.portainer_halobug.entrypoints=https"
            - "traefik.http.routers.portainer_halobug.rule=Host(`portainer.halobug.cn`)"
            - "traefik.http.routers.portainer_halobug.tls=true"
            - "traefik.http.services.portainer_halobug-backend.loadbalancer.server.scheme=http"
            - "traefik.http.services.portainer_halobug-backend.loadbalancer.server.port=9000"
        logging:
            driver: "json-file"
            options:
                max-size: "10m"
networks:
    traefik:
        external: true
```

1.4 启动服务

```sh
// 本机测试绑定 hosts
127.0.0.1 portainer.cn

//启动成功如下图
docker-compose down && docker-compose up -d
```

1.5 访问域名portainer.cn

#### [Docker可视化工具——Portainer全解](https://zhuanlan.zhihu.com/p/403285855)

Portainer是一个可视化的Docker操作界面，提供状态显示面板、应用模板快速部署、容器镜像网络数据卷的基本操作（包括上传下载镜像，创建容器等操作）、事件日志显示、容器控制台操作、Swarm集群和服务等集中管理和操作、登录用户管理和控制等功能。功能十分全面，基本能满足中小型单位对容器管理的全部需求。

- 登录 Portainer

如果你部署了包含 Portainer 的Docker环境，请直接登录使用。否则，请先安装 Portainer：

# 通过命令安装 Portainer

```sh
docker volume create portainer_data
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
cd /usr/libexec/docker/
sudo ln -s docker-runc-current docker-runc
```

通过本地浏览器访问：<http://服务器公网IP:9000，> 直接进入 Portainer 界面

进入Portainer后台管理界面，点击Local项目就可以开始使用Portainer

- 部署MySQL容器
- 部署WordPress容器
- 部署Nginx容器
- 部署 FileBrowser 容器
- 设置 Nginx 配置文件实现端口转发
  - 打开第二步中创建的 File Browser 网站（[公网ip:端口]），账号密码为 admin/admin，登录到 File Browser;
  - 进入conf.d目录，双击default.conf文件，将原来的配置删除，修改为如下图所示内容：注意：请将 server_name 改为自己的域名，proxy_pass 改为自己网站容器的 IP:端口号

```conf
upstream wordpress {
       server 159.138.6.145:32773;
   }

   server {
       listen 80;
       server_name  test.example.top; #绑定域名

       location / {
       proxy_pass http://wordpress;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header Via "nginx";
       }
   }
```

- 进阶实战：Portainer 设置 HTTPS
  - 在浏览器打开 File Browser ，新建一个名为 cert 文件夹，将证书上传至 cert；
  - 修改 Nginx 的配置文件,注意将 IP 和域名改成自己的服务器 IP 和域名；
  - 请将证书路径改为自己的证书所在路径，并将证书名改为自己的证书名。

```conf
upstream portainer {
server 159.138.6.145:9000;
}

server {
    listen 80;
    listen 443 ssl;
    server_name  test.websoft9.top;

    ssl_certificate /etc/nginx/cert/cert-1540972394298_test.websoft9.top.crt;
    ssl_certificate_key /etc/nginx/cert/cert-1540972394298_test.websoft9.top.key;

    location / {
    proxy_pass http://portainer;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Via "nginx";
    }
}
```

- 如果想要达到访问 http 自动跳转到 https 的效果，请将配置改成如下所示：

```conf
 upstream portainer {
 server 159.138.6.145:9000;
 }

 server {
     listen 80;
     listen 443 ssl;
     server_name  test.websoft9.top;

 ssl_certificate /etc/nginx/cert/cert-1540972394298_test.websoft9.top.crt;
 ssl_certificate_key /etc/nginx/cert/cert-1540972394298_test.websoft9.top.key;

 if ($scheme != "https") {
 return 301 https://$host$request_uri;
 }

 location / {
 proxy_pass http://portainer;
 proxy_set_header Host $host;
 proxy_set_header X-Real-IP $remote_addr;
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_set_header Via "nginx";
 }
 }
```

- Portainer 备份
到 Portainer 的容器列表里面查看 portainer 的 volume 对应的服务器目录，在/var/lib/docker/volumes下可找到 volume 对应的目录名，将其备份即可。

- Portainer 升级
只需运行 docker pull portainer就可以将 Portainer 升级到最新版本。

- Portaniner 绑定域名
域名绑定可在 [配置Nginx实现端口转发]章节中将 server_name 改成自己的域名即可。
- 不知道容器镜像所需的端口怎么办？
建议开启【Publish all exposed network ports...】 以保证容器中的服务可以自动匹配服务器端口被外界访问。如果不开启，需自行到[DockerHub ]网站查看端口。

- 容器的端口与服务器的端口有什么区别？
容器端口需要通过服务器端口做映射，才可以被互联网用户访问。

### compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

#### [菜鸟驿站](https://www.runoob.com/docker/docker-compose.html)

#### [全网最详细的Docker-Compose详细教程](https://juejin.cn/post/7042663735156015140)

compose、machine 和 swarm 是docker 原生提供的三大编排工具。简称docker三剑客。

Docker Compose能够在 Docker 节点上，以单引擎模式(Single-Engine Mode)进行多容器应用的部 署和管理。多数的现代应用通过多个更小的微服务互相协同来组成一个完整可用的应用。

部署和管理繁多的服务是困难的。而这正是 Docker Compose 要解决的问题。Docker Compose 并不 是通过脚本和各种冗长的 docker 命令来将应用组件组织起来，而是通过一个声明式的配置文件描述整 个应用，从而使用一条命令完成部署。应用部署成功后，还可以通过一系列简单的命令实现对其完整声 明周期的管理。甚至，配置文件还可以置于版本控制系统中进行存储和管理。

- docker compose安装

```sh
# 下载 https://github.com/docker/compose
mv /data/docker-compose-Linux-x86_64 /usr/local/bin/docker-compose

cp /data/docker-compose-Linux-x86_64 /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

#开发环境可以授予最高权限

chmod 777 /usr/local/bin/docker-compose

docker-compose -v

# 使用pip安装
sudo pip install docker-compose

# 卸载
rm -rf /usr/local/bin/docker-compose
reboot
```

- yml配置文件及常用指令
Docker Compose 使用 YAML 文件来定义多服务的应用。YAML 是 JSON 的一个子集，因此也可以使用JSON。Docker Compose 默认使用文件名 docker-compose.yml。当然，也可以使用 -f 参数指定具体文件。

Docker Compose 的 YAML 文件包含 4 个一级 key:version、services、networks、volumes

version 是必须指定的，而且总是位于文件的第一行。它定义了 Compose 文件格式(主要是 API)的版本。注意，version 并非定义 Docker Compose 或 Docker 引擎的版本号。

services 用于定义不同的应用服务。上边的例子定义了两个服务:一个名为 lagou-mysql数据库服 务以及一个名为lagou-eureka的微服。Docker Compose 会将每个服务部署在各自的容器中。

networks 用于指引 Docker 创建新的网络。默认情况下，Docker Compose 会创建 bridge 网络。 这是一种单主机网络，只能够实现同一主机上容器的连接。当然，也可以使用 driver 属性来指定不 同的网络类型。

volumes 用于指引 Docker 来创建新的卷。

demo

```yaml
version: '3'
services:
  mysql:
    build:
      context: ./mysql
    environment:
      MYSQL_ROOT_PASSWORD: admin
    restart: always
    container_name: mysql
    volumes:
    - /data/edu-bom/mysql/test:/var/lib/mysql
    image: mysql/mysql:5.7
    ports:
      - 3306:3306
    networks:
      net:
  eureka:
    build:
      context: ./edu-eureka-boot
    restart: always
    ports:
      - 8761:8761
    container_name: edu-eureka-boot
    hostname: edu-eureka-boot
    image: edu/edu-eureka-boot:1.0
    depends_on:
      - mysql
    networks:
      net:
networks:
    net:
volumes:
    vol:
```

- docker compose常用命令

```sh
# 启动服务
docker-compose up -d

# 停止服务
docker-compose down

# 列出所有运行容器
docker-compose ps

# 查看服务日志
docker-compose logs

# 构建或
docker-compose build者重新构建服务

# 启动服务
docker-compose start

# 停止已运行的服务
docker-compose stop

# 重启服务
docker-compose restart
```

#### [Docker-Compose 基础与实战，看这一篇就够啦](https://zhuanlan.zhihu.com/p/107981897)

Compose 项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。使用前面介绍的Dockerfile我们很容易定义一个单独的应用容器。能够管理一组相关联的的应用容器.

- Compose有2个重要的概念：
项目（Project）：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。
服务（Service）：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。

- docker compose 重要命令

```sh
# 命令选项
-f, --file FILE 指定使用的 Compose 模板文件，默认为 docker-compose.yml，可以多次指定。
-p, --project-name NAME 指定项目名称，默认将使用所在目录名称作为项目名。
–x-networking 使用 Docker 的可拔插网络后端特性
–x-network-driver DRIVER 指定网络后端的驱动，默认为 bridge
–verbose 输出更多调试信息。
-v, --version 打印版本并退出。
```

- 常用&重要命令
  - config验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因。如：docker-compose -f skywalking.yml config此命令不会执行真正的操作，而是显示 docker-compose 程序解析到的配置文件内容
  - images列出 Compose 文件中包含的镜像。如docker-compose -f skywalking.yml images
  - ps列出项目中目前的所有容器。如docker-compose -f skywalking.yml ps
  - build构建（重新构建）项目中的服务容器。如：docker-compose -f skywalking.yml build，一般搭配自定义镜像，比如编写的Dockfile，功能类似于docker build .
  - up该命令十分强大（重点掌握），它将尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作。如docker-compose -f skywalking.yml up。默认情况，docker-compose up 启动的容器都在前台，控制台将会同时打印所有容器的输出信息，可以很方便进行调试。如果使用docker-compose up -d将会在后台启动并运行所有的容器。一般推荐生产环境下使用该选项。默认情况，如果服务容器已经存在，docker-compose up 将会尝试停止容器，然后重新创建（保持使用 volumes-from 挂载的卷），以保证新启动的服务匹配 docker-compose.yml 文件的最新内容。如果用户不希望容器被停止并重新创建，可以使用 docker-compose up --no-recreate。这样将只会启动处于停止状态的容器，而忽略已经运行的服务。如果用户只想重新部署某个服务，可以使用 docker-compose up --no-deps -d <SERVICE_NAME> 来重新创建服务并后台停止旧服务，启动新服务，并不会影响到其所依赖的服务。此命令有如下选项：①：-d 在后台运行服务容器。②：--no-color 不使用颜色来区分不同的服务的控制台输出。③：--no-deps 不启动服务所链接的容器。④：--force-recreate 强制重新创建容器，不能与 --no-recreate 同时使用。⑤：--no-recreate 如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。⑥：--no-build 不自动构建缺失的服务镜像。⑦：-t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。
  - down此命令停止用up命令所启动的容器并移除网络，如docker-compose -f skywalking.yml down
  - stop格式为 docker-compose stop [options] [SERVICE...]停止已经处于运行状态的容器，但不删除它。通过 docker-compose start 可以再次启动这些容器，如果不指定service则默认停止所有的容器。如docker-compose -f skywalking.yml stop elasticsearch选项：-t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。
  - start启动已经存在的服务容器。用法跟上面的stop刚好相反,如docker-compose -f skywalking.yml start elasticsearch
  - restart重启项目中的服务。用法跟上面的stop,start一样
  - logs格式为docker-compose logs [options] [SERVICE...]查看服务容器的输出。默认情况下，docker-compose 将对不同的服务输出使用不同的颜色来区分。可以通过 --no-color 来关闭颜色。该命令在调试问题的时候十分有用。如docker-compose -f skywalking.yml logs 查看整体的日志，docker-compose -f skywalking.yml logs elasticsearch 查看单独容器的日志

- docker compose 模板文件
模板文件是使用 Compose 的核心，涉及到的指令关键字也比较多。默认的模板文件名称为 docker-compose.yml，格式为 YAML 格式。

注意每个服务都必须通过 image 指令指定镜像或 build 指令（需要 Dockerfile）等来自动构建生成镜像。如果使用 build 指令，在 Dockerfile 中设置的选项(例如：CMD, EXPOSE, VOLUME, ENV 等) 将会自动被获取，无需在 docker-compose.yml 中重复设置。

```yaml
images指定为镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉取这个镜像。
image: apache/skywalking-oap-server:6.5.0
image: apache/skywalking-ui:6.5.0
ports暴露端口信息。使用宿主端口：容器端口 (HOST:CONTAINER) 格式，或者仅仅指定容器的端口（宿主将会随机选择端口）都可以，端口字符串都使用引号包括起来的字符串格式。
ports:
    - "3000"
    - "8080:8080"- "127.0.0.1:8001:8001"
volumes数据卷所挂载路径设置。可以设置为宿主机路径(HOST:CONTAINER)或者数据卷名称(VOLUME:CONTAINER)，并且可以设置访问模式 （HOST:CONTAINER:ro）。
volumes:
      - /app/skywalking/elasticsearch/data:/usr/share/elasticsearch/data:rw
      - conf/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml
version: "3"
services:
  my_src:
    image: mysql:8.0
    volumes:
      - mysql_data:/var/lib/mysql
volumes:
  mysql_data:
ulimits指定容器的 ulimits 限制值。例如，指定最大进程数为 65535，指定文件句柄数为 20000（软限制，应用可以随时修改，不能超过硬限制） 和 40000（系统硬限制，只能 root 用户提高）。
ulimits:
   nproc: 65535
   nofile:
     soft: 20000
     hard: 40000
depends_on解决容器的依赖、启动先后的问题。以下例子中会先启动 redis mysql 再启动 web
version: '3'
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: mysql
environment设置环境变量。你可以使用数组或字典两种格式。
environment:
      SW_STORAGE: elasticsearch
      SW_STORAGE_ES_CLUSTER_NODES: elasticsearch:9200

environment:
      - SW_STORAGE= elasticsearch
      - SW_STORAGE_ES_CLUSTER_NODES=elasticsearch:9200
restart指定容器退出后的重启策略为始终重启。该命令对保持服务始终运行十分有效，在生产环境中推荐配置为 always 或者 unless-stopped。
restart: always
```

#### [docker-compose教程（安装，使用, 快速入门）](https://blog.csdn.net/pushiqiang/article/details/78682323)

### [本地系统（Linux）打包成docker镜像](https://blog.csdn.net/weixin_42763067/article/details/113663060)

1. tar

```sh
# 一、 安装工具tar
yum install -y tar
# 二、将本机系统打包成tar文件
tar --numeric-owner --exclude=/proc --exclude=/sys -cvf Linux-base.tar /
# 三、将Linux-base.tar导入
docker import Linux-base.tar  <docker中images的名字>
# 四、查看镜像
docker images
# 四、运行该系统
docker run -it <docker中images的名字> /bin/bash
```

2. dockerfile

你将你的装机过程书写成dockerfile，就制作好了镜像。你按照dockerfile来装机，就等于把镜像复刻到宿主机上了。这里的一切，都因为，dockerfile本质上就是个装机脚本。

3. dd

玩pve的应该很熟悉，很多人会在lxc里面跑一个完整系统。

如果你只是想迁移备份导入导出系统，直接dd /tar就完了。如果你还想能运行，那么dd到qcow2，开kvm，简单又省事。

4. [Convert Any Server to a Docker Container (Updated)](https://zwischenzugs.com/2016/04/04/convert-any-server-to-a-docker-container-updated/)

ShutIt script (as root):

There are therefore 3 main steps to getting into your container:

– Install ShutIt on the server

```sh
sudo su -
pip install shutit
```

The pre-requisites are python-pip, git and docker. The exact names of these in your package manager may vary slightly (eg docker-io or docker.io) depending on your distro.

You may need to make sure the docker server is running too, eg with ‘systemctl start docker’ or ‘service docker start’.

– Run the ‘copyserver’ ShutIt script

Check out the copyserver script:

```sh
git clone https://github.com/ianmiell/shutit_copyserver.git

cd shutit_copyserver/bin
./copy_server.sh

```

There is a prompt to ask what docker base image you want to use. Make sure you use one as close to the original server as possible.

– Run your copyserver Docker image as a container

Run the built image:

```sh
docker run -ti [username]/copyserver /bin/bash
```

## 存储器/文件系统

### ntfs

### SMB

### NFS

### FTP/SFTP/TFTP

### filebrowser：使用Golang开发的文件管理器，支持WEB管理文件和文件分享

- openmediavault-filebrowser

## 内网应用仪表盘

### [Flare](https://github.com/soulteary/docker-flare)

Lightweight, high performance and fast self-hosted navigation pages,[从零开始搭建个人书签导航应用：Flare](https://zhuanlan.zhihu.com/p/471484010)

### Heimdall

[Heimdall](https://heimdall.site/) 是一款网络书签仪表盘，它内置了超过 300 款网络服务图标，以及接通了部分服务的 API，可以实现一个非常漂亮的网络书签、内网门户页面.当然它本质上还是一个书签服务，支持自定义 URL 和图标的。并且 Heimdall 支持多用户、标签分类、自带了 242 个应用信息，以及 67 个可以显示更多信息的增强型应用。

#### [安装方式](https://www.appinn.com/heimdall/)

Docker 命令行

```sh
docker run --name=heimdall -d -v /home/heimdall:/config -e PGID=1000 -e PUID=1000 -p 8080:80 -p 8443:443 linuxserver/heimdall
# 其中 -v 是挂载配置文件夹，-e 是设置运行用户权限，一般情况下默认即可，-p 是映射端口，可自定义。
```

然后就能直接在内网通过 ip:8443 访问了。当然也可以放在公网上使用，毕竟支持多用户，

### [OneNav](https://www.onenav.top/)

OneNav是一款开源免费的书签（导航）管理程序，由xiaoz使用使用PHP + SQLite 3开发，界面简洁，安装简单，使用方便。OneNav可帮助你你将浏览器书签集中式管理，解决跨设备、跨平台、跨浏览器之间同步和访问困难问题，做到一处部署，随处访问。

<https://doc.xiaoz.org/books/onenav/page/a1d0c>

#### 常规安装

<https://github.com/helloxz/onenav/releases>

#### 宝塔面板安装

OneNav已上架宝塔商店，通过宝塔后台 - 软件商店 - 一键部署 - 搜索onenav，并点击一键部署。

#### Docker安装

```sh
# 80：第1个80端口为访问端口，可自行修改，第2个80为容器内部端口，请不要修改
# /data/onenav：本机挂载目录，用于持久存储Onenav数据
docker run -itd --name="onenav" -p 80:80 \
    -v /data/onenav:/data/wwwroot/default/data \
    helloz/onenav
```

#### docker-compose安装

您也可以选择docker-compose进行安装，将下面的文件保存为docker-compose.yaml

```yaml
version: '3'
services:
  onenav:
    container_name: onenav
    ports:
      - "3080:80"
    volumes:
      - './data:/data/wwwroot/default/data'
    image: 'helloz/onenav'
    restart:
      always
```

然后运行命令docker-compose up -d进行启动。

#### 安全设置

如果您使用的Nginx，请务必将以下规则添加到站点配置中，否则数据库可能被下载（非常危险）：

```conf
#安全设置
location ~* ^/(class|controller|db|data|functions|templates)/.*.(db3|php|php5|sql)$ {
    return 403;
}
location ~* ^/(data)/.*.(html)$ {
        deny all;
}
location /db {
        deny all;
}

#伪静态
rewrite ^/click/(.*) /index.php?c=click&id=$1 break;
rewrite ^/api/(.*)?(.*) /index.php?c=api&method=$1&$2 break;
rewrite /login /index.php?c=login break;
```

如果使用得Apache则无需设置，已内置.htaccess进行屏蔽。
如果您使用的Docker安装也无需此配置，默认已内置规则
如果使宝塔一键安装用户，不需要此设置

#### Nginx反向代理

如果您使用的Docker部署，需要通过域名进行访问，您可以通过下面2个方法完成：

直接将域名解析到您服务器IP，然后访问<http://域名:端口进行访问>
如果您不希望带上容器端口，也可以通过Nginx反向代理绑定域名进行访问
Nginx反向代理配置参考：

```conf
server {
    listen 80;

 #改成你自己的域名
    server_name demo.onenav.top;

    #指定网站日志路径
    #access_log /data/logs/demo.onenav.top_nginx.log xlog;
    charset utf-8,gbk;

 location / {
        proxy_connect_timeout 10;
     # Default is HTTP/1, keepalive is only enabled in HTTP/1.1
     proxy_http_version 1.1;
     proxy_set_header Connection "";
     #chunked_transfer_encoding off;
        proxy_set_header Host $host;
     #将3080改成你容器的端口
        proxy_pass http://127.0.0.1:3080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## 网盘

### [alist](https://alist.nn.ci/)

- <https://github.com/alist-org/alist>
- <https://github.com/Xmarmalade/alisthelper>

#### [AList 白嫖网盘空间神器 - 免费开源挂载百度/阿里/OneDrive等云盘到本地工具](https://www.iplaysoft.com/alist.html)

AList 是一款免费开源支持多存储的自建网盘程序 (文件列表程序)，可以轻松在 VPS 服务器、NAS、普通电脑 Win、Mac、Linux 上部署。它除了能作为一款自建网盘 (将文件保存在设备硬盘上) 外，最大的特色就是支持「挂载各大主流网盘」，免费将它们的空间“据为己用”！AList 提供了网页版界面能让你集中管理各大网盘的文件，支持文件上传下载、文件管理、预览图片、查看文档、在线播放音乐、视频等等。AList 网页版可以支持 PC 以及手机浏览器。而更加有用的是，AList 还支持对外提供 WebDAV 服务！也就是能将网盘转换成 WebDAV 协议，这样我们就可以用各种各样的软件来访问它们了 (比如使用播放器直接播放网盘里的视频)。通过 WebDAV 协议，你还能使用 nPlayer、Infuse、Fileball、Kodi、MX Player、KMPlayer、NOVA、VLC、PotPlayer 等播放器直接连接播放了！而且如果借助「RaiDrive」等工具，还能把 Alist 的 WebDAV 服务直接挂载变成一个虚拟的本地“硬盘”来使用 (如上图)，可以用于备份、传照片、保存文档、播放原画质视频等等，用途更加广泛。

```sh
./alist storage list

./alist storage disable /233
```

```ini
# /usr/lib/systemd/system/alist.service
# 其中 path_alist 为 AList 所在的路径

[Unit]
Description=alist
After=network.target

[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=path_alist/alist server
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```sh
# 重载配置
systemctl daemon-reload
# 启动:
systemctl start alist
# 关闭:
systemctl stop alist
# 配置开机自启:
systemctl enable alist
# 取消开机自启:
systemctl disable alist
# 状态:
systemctl status alist
# 重启:
systemctl restart alist
```

AList 支持后端挂载的存储服务：

- 本地硬盘存储
- SMB 共享 、WebDAV、FTP / SFTP
- 对象存储 (S3 协议通用)
- 百度网盘、阿里云盘、OneDrive (SharePoint)
- 迅雷云盘、PikPak、天翼云盘、移动云盘、115 网盘
- 腾讯微云、夸克网盘、Dropbox、GoogleDrive
- Yandex.Disk、MEGA、Seafile、Cloudreve
- ...

Alist 部署安装教程：

- 选择一：Alist Helper 桌面版 (适合新手 / 个人电脑使用)
如果你打算在电脑上本机运行 AList，又或者不太熟悉命令行的操作，那么拥有 UI 图形界面的桌面版 AList 相比命令行将会更加容易上手！其中，有付费的官方版 Alist Desktop (定价 50 元)，以及第三方免费开源的 Alist Helper 可以选择。
免费的 Alist Helper 几乎拥有官方 Alist Desktop 的全部功能，同时可以支持 Windows 和 macOS 系统，它旨在简化 Alist 的使用，可以通过图形界面快速管理 Alist，让您更轻松地开启、关闭 Alist 程序。

Alist Helper 桌面版的功能：

- 开源免费、无广告
- 跨平台支持 Win 和 Mac
- 自动启动 Alist
- 最小化至系统托盘
- 开机自启和开机静默启动
- 能够快速查看alist的版本和管理员信息
- 可调整的 alist 启动参数。可根据自己的特定需求和偏好来自定义启动参数。
AList 的桌面版拥有漂亮直观的 UI 图形界面，使用上非常简单！对新手，或不喜欢命令行操作的朋友更为友好，上手使用几乎没有任何难度和门槛。

- 选择二：Docker 部署 Alist 教程 (适合安装到服务器 / Nas 等长期运行)
如果你想要在 Linux 系统上部署 Alist，比如各种 VPS 服务器、NAS、树莓派或普通电脑，打算长期运行，都推荐使用 Docker 进行安装部署。下面是一个在 Debian / Ubuntu / CentOS 上安装 Docker 的教程：通过 Docker 部署 Alist 命令：

```sh
docker run -d --restart=always -v /your/host/dir:/opt/alist/data -p 5244:5244 -e PUID=0 -e PGID=0 -e UMASK=022 --name="alist" xhofe/alist:latest
# -v 是挂载数据目录，其中的 /your/host/dir 是你主机上的目录，自行修改
# -p 是绑定端口号，启动后可通过「http://主机IP:5244」进行访问

# 如需手动设置密码，则可以执行下面的命令：
docker exec -it alist ./alist admin set 你的密码

```

待 Alist 服务成功启动之后，我们就能通过浏览器访问它了：

本机访问：<http://127.0.0.1:5244>
局域网访问：<http://局域网IP:5244>
公网访问：<http://服务器公网IP:5244> (如部署在云服务器上可直接互联网访问)，如果是家庭宽带无公网 IP 的，那么需要额外配置「内网穿透」才可以实现。
将网盘空间挂载到本地 (建立 WebDAV)

Alist 作为一个网盘文件管理器，它后端支持挂载的存储服务非常非常多！包括但不限于本地硬盘存储、SMB 共享、FTP / SFTP、WebDAV、各大云服务的对象存储等，同时还可以支持各种网盘：百度网盘、阿里云盘、OneDrive (SharePoint)、迅雷云盘、天翼云盘、移动云盘、腾讯微云、PikPak、夸克网盘、Dropbox、Seafile 等等，非常丰富。

在 Alist 的管理界面中的「存储」页面，我们可以随意添加它所支持的网盘。具体每一个网盘的添方法都有所区别，比如需要扫码登录账号获取 token 或 cookie 等，具体就需要大家「参考官网的文档」来使用了

AList 的 WebDAV 服务：
AList 运行后就会开启 WebDAV 服务，供其他程序连接，下面是其连接信息，大家可以参考：

URL <http://主机IP地址:端口号/dav/>
路径 /dav
协议 http
端口号 与网页端一致
WebDAV用户名 与网页端用户名一致
WebDAV密码 与网页端密码一致

将AList 变成本地硬盘盘符
比如有了这些信息，你就可以使用 RaiDrive 将 Alist 的 WebDAV 挂载成本地硬盘了。

AList 功能特性：
部署方便，开箱即用；黑暗模式、国际化多语言支持
文件预览（PDF、markdown、代码、纯文本等...）；支持 README.md 预览渲染
画廊模式下的图像预览
视频和音频预览，支持歌词和字幕
Office 文档预览（docx、pptx、xlsx、...）
文件永久链接复制和直接文件下载
支持密码保护和身份验证
提供 WebDav 服务
Docker 部署、Cloudflare workers 中转
文件/文件夹打包下载
网页上传(可以允许访客上传)，删除，新建文件夹，重命名，移动，复制
离线下载；跨存储复制文件；单线程下载/串流的多线程下载加速

AList 不仅是一款开源实用的自建网盘程序，通过它你还可以自由添加任意多的网盘作为背后存储，从而实现“多网盘聚合管理”！同时还能将网盘空间转换成 WebDAV 服务“据为己用”。

无论是作为网络影视资源库，或是文档备份的空间，它都能让你更好地利用网盘的容量，节省本地磁盘空间。从而也能省下一大笔购买 NAS 、硬盘的开支。不得不说，AList 绝对是一个非常实用的开源项目，如果你有一点动手能力，绝对值得部署一个。

### 百度网盘

[omv 家用 nas 搭建[3]， 百度云网盘部署](https://zhuanlan.zhihu.com/p/363608459)

```sh
docker pull johngong/baidunetdisk
docker create  \
    --name=baidunetdisk  \
    -p 5800:5800  \
    -p 5900:5900  \
    -v /配置文件位置:/config  \
    -v /下载位置:/config/baidunetdiskdownload  \
    --restart unless-stopped  \
    johngong/baidunetdisk:latest
```

docker-compose:

```yml
---
version: "2.1"
services:
  baiduNetdisk:
    image: johngong/baidunetdisk:latest
    container_name: baiduNetdisk
    environment:
      - TZ=Asia/Shanghai
      - VNC_PASSWORD=123456
    volumes:
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/software/baiduNetdisk/config:/config 
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/pt/baidupcs/downloads:/config/baidunetdiskdownload 
    ports:
      - 5900:5900
      - 6080:5800
    restart: unless-stopped
```

## 远程下载

### xunlei

#### [在Docker环境中实现NAS版迅雷安装指南](https://blog.xiaoz.org/archives/19632)

```sh
docker run -d \
 --name=xunlei \
 --hostname=mynas \
 --net=host \
 -v /mnt/sdb1/xunlei:/xunlei/data \
 -v /mnt/sdb1/downloads:/xunlei/downloads \
 --restart=unless-stopped \
 --privileged \
 cnk3x/xunlei:latest

hostname：主机名称（设备名称）
/mnt/sdb1/xunlei：为迅雷配置保存路径，请根据自身情况修改
/mnt/sdb1/downloads：迅雷下载文件保存路径，请根据自身情况修改
latest：版本号，截至目前，Docker镜像最新的版本号为3.7.1，如果您使用latest安装发现不是最新版本，建议手动修改版本号
```

使用docker-compose安装

```yaml
services:
  xunlei:
    image: cnk3x/xunlei:latest
    privileged: true
    container_name: xunlei
    hostname: mynas
    network_mode: host
    volumes:
      - /mnt/sdb1/xunlei:/xunlei/data
      - /mnt/sdb1/downloads:/xunlei/downloads
    restart: unless-stopped
```

参数含义同上，安装完毕后访问端口为<http://IP:2345，也可以增加XL_WEB_PORT变量来指定端口，具体见：https://hub.docker.com/r/cnk3x/xunlei>

如果您使用的host网络模式，还需要在防火墙上放行2345端口，命令为：

```sh
# 如果是firewalld
firewall-cmd --zone=public --add-port=2345/tcp --permanent
firewall-cmd --reload
# 如果是ufw
ufw allow 2345
```

### Aria2 一键安装管理脚本 增强版

多线程下载工具，支持http/ftp/BT等协议

- <https://github.com/P3TERX/aria2.sh>
- 下载脚本

```sh
wget -N https://github.com/P3TERX/aria2.sh && chmod +x aria2.sh
```

- 运行脚本

```sh
./aria2.sh
```

- <https://ariang.js.org/>

### transmission

BT下载工具，用来保种不错，占用很低

- 403: Forbidden

```sh
sudo find / -name transmission-daemon
cd ~/.config/transmission-daemon
sudo nano settings.json
sudo service transmission-daemon restart
sudo transmission-daemon -d
```

[修改transmission配置，实现远程访问transmission](https://zhuanlan.zhihu.com/p/86949334)

```json
"rpc-authentication-required": true,
"rpc-bind-address": "0.0.0.0",
"rpc-enabled": true,
"rpc-host-whitelist": "",
"rpc-host-whitelist-enabled": true,
"rpc-password": "123456",
"rpc-port": 9091,
"rpc-url": "/transmission/",
"rpc-username": "admin",
"rpc-whitelist": "",
"rpc-whitelist-enabled": false,
```

- ip:9091

### [qbitttorrent](https://github.com/qbittorrent/qBittorrent)

BT下载工具，抢上传比较厉害，一般PT用得比较多

[omv 家用 nas 搭建[2]， qbitttorrent 部署](https://zhuanlan.zhihu.com/p/363378341)

```sh
docker pull linuxserver/qbittorrent
```

docker-compose:

```yml
---
version: "2.1"
services:
  qbittorrent:
    image: ghcr.io/linuxserver/qbittorrent
    container_name: qbittorrent
    environment:
      - PUID=998
      - PGID=100
      - TZ=Asia/Shanghai
      - WEBUI_PORT=9001
    volumes:
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/software/qbittorrent/config:/config
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/pt/qbittorrent/downloads:/downloads
    ports:
      - 6881:6881
      - 6881:6881/udp
      - 9001:9001
    restart: unless-stopped
```

### qbittorrent-nox

qbittorrent webui版 [在 Ubuntu 服务器上安装 qBittorrent-nox](https://aimerneige.com/zh/post/linux/install-qbittorrent-nox-on-ubuntu-server/)

- 导入 qBittorrent-nox 稳定版本的源
  - ```sudo add-apt-repository ppa:qbittorrent-team/qbittorrent-stable -y```
- 安装 qBittorrent-nox
  - ```sudo apt install qbittorrent-nox -y```
- 创建 service 文件
  - ```sudo nano /etc/systemd/system/qbittorrent-nox.service```

```service
[Unit]
Description=qBittorrent Command Line Client
After=network.target

[Service]
Type=forking
User=root

ExecStart=/usr/bin/qbittorrent-nox -d --webui-port=8080
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

- 重载 systemctl
  - ```sudo systemctl daemon-reload```
- 启动 qBittorrent-nox
  - ```sudo systemctl start qbittorrent-nox```
- 开启开机自启动 qBittorrent-nox
  - ```sudo systemctl enable qbittorrent-nox```
- 检查 qBittorrent-nox 是否启动
  - ```systemctl status qbittorrent-nox```
- 登录到 qBittorrent-nox
  - username:admin
  - password:adminadmin
- 如何删除 qBittorrent-nox

```sh
# Remove qBittorrent Stable
sudo add-apt-repository --remove ppa:qbittorrent-team/qbittorrent-stable
# Remove qBittorrent Unstable (Nightly)
sudo add-apt-repository --remove ppa:qbittorrent-team/qbittorrent-unstable -y
# Remove qBittorrent
sudo apt autoremove qbittorrent-nox
```

- WebUI HTTPS configuration
  - Linux WebUI setting up HTTPS with self signed SSL certificates
    - Create neccesary folders:
      - ```mkdir ~/.config/qBittorrent/ssl```
      - ```cd ~/.config/qBittorrent/ssl```
    - Now we generate the key and certificate pair:
      - ```openssl req -new -x509 -nodes -out server.crt -keyout server.key```
    - You should now have two files in your ssl folder:server.crt  server.key
    - Tools -> Options... -> WebUI
    - Enable HTTPS and optionally change the port to your liking. Then, according to your version:
      - 4.2.0 and newer: copy the path of the key and certificate files into the respective fields of the WebUI (for example, /home/qbtuser/.config/qBittorrent/ssl/server.key and /home/qbtuser/.config/qBittorrent/ssl/server.crt)
      - older versions: copy and paste the key and certificate's contents into the respective fields of the webui. You can use cat in your terminal to view the contents of the files: cat server.key
      - Copy the contents of the entire file (including -----BEGIN PRIVATE KEY----- and -----END PRIVATE KEY-----) into the 'key' field of the WebUI and proceed to do the same with the certificate by issuing: cat server.crt

## 播放服务

### jellyfin

[omv 家用 nas 搭建[4]， jellyfin 部署](https://zhuanlan.zhihu.com/p/363652899)

jellyfin 作为播放系统，除了海报墙等基础功能外，还自带免费的硬件解码以及转码功能，对于远程播放且带宽受限的用户来说，是一个比较经济的解决方案。除此之外，jellyfin 源码也在 github 上开源了，这相比闭源系统来说，使用更加放心。

```sh
docker pull linuxserver/jellyfin
```

docker-compose:

```yml
---
version: "2.1"
services:
  jellyfin:
    image: ghcr.io/linuxserver/jellyfin
    container_name: jellyfin
    environment:
      - PUID=998
      - PGID=100
      - TZ=Asia/Shanghai
    volumes:
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/software/jellyfin/config:/config # 配置
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/pt/qbittorrent/downloads/Movies:/data/Movies # 电影
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/pt/qbittorrent/downloads/Musics:/data/Musics # 音乐
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/pt/qbittorrent/downloads/Pictures:/data/Pictures # 图片

    ports:
      - 9003:8096 # web ui
      - 8920:8920 # optional
      - 7359:7359/udp # optional
      - 1900:1900/udp # optional
    devices:
      - /dev/dri:/dev/dri # Intel 集显驱动
    restart: unless-stopped
```

## 同步

### urbackup

[omv 家用 nas 搭建[5]， urbackup 部署](https://zhuanlan.zhihu.com/p/363746660)

nas 中一个重要功能就是同步不同设备的数据。同步大概可分为定时同步和增量同步，定时同步一般是设定时间间隔，以每个时间间隔作为增量进行同步更新。而增量同步则是每当待同步的文件夹更新时，服务端主动拉取信息进行同步。

C/S 架构需要远程部署服务端，在本地部署客户端，并且服务端与客户端成功建立连接进行配对。当客户端在事件触发后有同步需求时，将会往服务端发送同步信息，进而开始同步进程。

urbackup 架构也属于 C/S 架构，需要在本地下载同步客户端，远程部署服务端，在客户端设置按照文件夹或是按照磁盘为单位，选择增量同步方式或是定时同步方式进行同步。

```sh
docker pull uroni/urbackup-server
```

docker-compose:

```yml
version: '2'

services:
  urbackup:
    image: uroni/urbackup-server:latest
    container_name: urbackup
    restart: unless-stopped
    environment:
      - PUID=998 # Enter the UID of the user who should own the files here
      - PGID=100  # Enter the GID of the user who should own the files here
      - TZ=Asia/Shanghai # Enter your timezone
    volumes:
      - /srv/dev-disk-by-uuid-760db5aa-db10-48eb-bc2e-06fcf98b2c8f/software/urbackup:/var/urbackup
      - /srv/dev-disk-by-uuid-c9ed017d-390d-4243-8ffd-5089e6ac1d91/backup:/backups
    network_mode: "host"
```

## 开发环境

### 版本控制

#### [gitea](https://about.gitea.com/)

Private, Fast, Reliable DevOps Platform

[文档](https://docs.gitea.com/zh-cn/)

- 准备工作
- [下载](https://dl.gitea.com/)

### gcc

### 文字转语音服务

- [三种方法在树莓派上实现文字转语音服务](https://shumeipai.nxez.com/2013/10/05/three-methods-developed-in-text-to-voice-services.html)
- [RPi Text to Speech (Speech Synthesis)](https://elinux.org/RPi_Text_to_Speech_(Speech_Synthesis))
