# N1 Openwrt设置


# 固体介绍 
Phicomm N1的 Openwrt 固件，见作者的[发布页](https://www.right.com.cn/forum/thread-4076037-1-1.html)，以下设置基于60+o版本。

# 网络

## 接口》LAN 口

* 地址      192.168.1.2

* 子网掩码  255.255.255.0

* 网关      192.168.1.1

* 广播      192.168.1.255

* 自定义 DNS
  ```
  x.x.x.x
  223.5.5.5
  119.29.29.29
  ```

* 物理设置，取消桥接，选中 eth0          

* DHCP  勾选 强制 （禁用主路由 dhcp）

* 禁用所有 ipv6 选项

* 禁用无线

## 防火墙》自定义规则，添加

`iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE`

* 若有桥接，添加

`iptables -t nat -I POSTROUTING -o  br-lan  -j MASQUERADE`

* 重启防火墙

## Turbo ACC    
打开 BBR 选项

# 网络存储

## NFS

添加共享目录 `/mnt/sda1`   `192.168.1.0/24`   `rw,sync,no_root_squash,insecure,no_subtree_check`

## 硬盘休眠
sda1 3 分钟

## 网络共享
共享名  `N1_share`  目录 `/mnt/sda1/`  允许匿名
编辑配置模板，`aio read size = 0`（去掉注释，提高读取速度）

## qBittorrent 
* 下载文件存放目录 `/mnt/sda1/N1_Download/qBittorrent/`

* WebUI 监听端口 8091

* 下载设置>临时路径 `/mnt/sda1/N1_Download/qBittorrent/tmp/`

*  磁盘缓存 128

*  设置 bt-tracker  见[bt-tracker源](https://github.com/ngosang/trackerslist)

## FTP 
本地用户>根目录 ` /mnt/sda1`

## Aria2 
* 文件和位置>下载目录 `/mnt/sda1/N1_Download/Aria2/`
* 磁盘缓存 128M

## miniDLNA 
* 接口，选中 eth0
* 媒体目录  `/mnt/sda1` 

# 服务

## AdGuard Home
通过 docker 实现，见下文

## ServerChan  
* `SCUxxxxxxxxxxxx`

* 设备名称 N1 路由器 

## Docker CE 
* 修改配置文件`/etc/docker/daemon.json`

```
{
"bip": "172.31.0.1/24",
"data-root": "/mnt/mmcblk2p4/docker/",
"log-level": "warn",
"log-driver": "json-file",
"log-opts": {
"max-size": "10m",
"max-file": "5"
},
"registry-mirrors": [
"https://dockerhub.azk8s.cn","https://docker.mirrors.ustc.edu.cn/","https://hub-mirror.c.163.com","https://registry.docker-cn.com"
]
}
```

### 安装 portainer
```     
docker pull portainer/portainer:linux-arm64
docker volume create portainer_data
docker run -d \
--name=Portainer \
--restart always \
-e TZ=Asia/Shanghai \
-p 9999:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer:linux-arm64 
```

### 安装荒野无灯 filebrowser
```
docker pull 80x86/filebrowser:arm64
mkdir /var/lib/filebrowser
docker run -it -d \
--restart always \
--name filebrowser \
--net=host \
-e SSL=on -e PUID=1000 -e PGID=100 -e WEB_PORT=8091 \
-v /var/lib/filebrowser:/config \
-v /mnt/sda1:/myfiles \
--mount type=tmpfs,destination=/tmp \
80x86/filebrowser:arm64
```

### 安装荒野无灯 qbittorrent
```
docker pull 80x86/qbittorrent:4.2.5-focal-20200423-arm64-nova
mkdir -p /var/lib/qbittorrent/config
mkdir -p /var/lib/qbittorrent/data
docker run -d --name qbittorrent \
-e PUID=1000 \
-e PGID=100 \
-e WEB_PORT=8099 \
-e BT_PORT=18099 \
-e TZ=Asia/Shanghai \
--restart=always \
-p 8099:8099 -p 18099:18099/tcp -p 18099:18099/udp \
-v /var/lib/qbittorrent/config/:/config \
-v /var/lib/qbittorrent/data/:/data \
-v /mnt/sda1/N1_Download/qBittorrent/:/downloads \
80x86/qbittorrent:4.2.5-focal-20200423-arm64-nova
```

### 安装 adguardhome
```
docker pull adguard/adguardhome:latest
mkdir -p /mnt/mmcblk2p4/adguardhome/confdir
mkdir -p /mnt/mmcblk2p4/adguardhome/workdir
docker run --name adguardhome \
-e TZ=Asia/Shanghai \
-v /mnt/mmcblk2p4/adguardhome/workdir:/opt/adguardhome/work \
-v /mnt/mmcblk2p4/adguardhome/confdir:/opt/adguardhome/conf \
--restart always \
--net host \
-d adguard/adguardhome:latest
```

* 网页管理端口 8090  监听端口 8053

* 重定向为 dnsmasq 上游服务器,打开 openwrt 的"网络“->"DHCP/DNS"， "DNS 转发"设为  `127.0.0.1#8053`

* 网络》防火墙》自定义规则，修改为

  ```
  iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 8053
  iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 8053
  ```

* adguardhome 设置：
  上游 DNS 服务器 Bootstrap DNS 服务器 
  ```
  x.x.x.x
  223.5.5.5
  119.29.29.29
  ```

  并行请求勾选，速度限制改为 0，禁用 ipv6

  过滤规则
  ```
  https://anti-ad.net/easylist.txt    #anti ad
  https://github.com/521xueweihan/GitHub520    #github hosts
  ```

  AdGuardHome 配置文件目录:  `/mnt/mmcblk2p4/adguardhome/confdir/AdGuardHome.yaml`
  部分内容:
  ```
  rewrites:
  - domain: api.themoviedb.org
  answer: 52.85.79.89
  - domain: api.themoviedb.org
  answer: 13.226.238.76
  - domain: api.themoviedb.org
  answer: 13.225.103.110
  - domain: api.themoviedb.org
  answer: 13.35.7.102
  - domain: api.themoviedb.org
  answer: 13.224.161.90
  ```
  ```
  user_rules:
  - '@@||dig.bdurl.net^$important'
  - '@@||passport.youdao.com^$important'
  - '@@||is.snssdk.com^$important'
  - '@@||sf3-ttcdn-tos.pstatp.com^$important'
  - '@@||p.bokecc.com^$important'
  ```


---

> 作者: LukeIO  
> URL: https://lukeio.github.io/posts/n1-openwrt%E8%AE%BE%E7%BD%AE/  

