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
> URL: https://lukeio.github.io/posts/slugorcontentbasename/  

: https://github.com/Cabbagec/termux-ohmyzsh/

  使用脚本
  ```
  sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
  ```
  弹框确认，允许访问文件。不小心拒绝，使用`termux-setup-storage` 命令可以重新获取访问权限。

  脚本执行后提示设置色彩样式和字体样式
  > Enter a number, leave blank to not to change: 14 Enter a number, leave blank to not to change: 4

  可以用`chcolor`和`chfont`命令再次修改色彩和字体

* 定制扩展按键

  通过 `~/.termux/termux.properties` 定制扩展按键，加入以下内容:
  ```
  extra-keys=[['ESC','|','/','-','HOME','UP','END','PGUP'],['TAB','CTRL','ALT','DEL','LEFT','DOWN','RIGHT','PGDN']]
  ```
  按键定义
  ```
  |CTRL|CTRL键|ALT|ALT键|FN|功能键|
  |ESC|退出键|TAB|表格键|HOME|原位键|
  |END|结尾键|PGUP|上翻页键|PGDN|下翻页键|
  |INS|插入键|DEL|删除键|BKSP|退格键|
  |UP |方向键上|LEFT|方向键左|RIGHT|方向键右|
  |DOWN|方向键下|ENTER|回车键|BACKSLASH|反斜杠\|
  |QUOTE|双引号键|APOSTROPHE|单引号键|F1~F12|F1-F12按键|
  ```
  重启 termux 生效
* 安装autosuggestions插件

  自动提示接下来可能要输入的命令，提高效率。下载:

  `git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions`

  修改配置 `~/.zshrc` 中的选项

  `plugins=(git zsh-autosuggestions)`
 
  输入`zsh`使配置生效

* 修改启动问候语

  `vim $PREFIX/etc/motd`

* vim 设置优化
  在 `~/.vimrc` 文件中修改
  ```
  "解决中文乱码
  set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
  set enc=utf8
  set fencs=utf8,gbk,gb2312,gb18030
  "显示行号
  set nu
  "颜色主题
  colorscheme desert
  "打开语法高亮
  syntax on
  ```

## termux 常用软件
* Openssh  `pkg install openssh`

  * 连接其它服务器
    ```
    ssh -p 22 user@hostname_or_ip      # ssh -p 端口号 用户名@主机名或者IP
    ssh -i id_rsa user@hostname_or_ip  # ssh -i 私钥 用户名@主机名或者IP
    ```
  * 开启 termux 的 sshd 服务
    ```
    sshd        #开启服务
    pkill sshd  #关闭服务
    ```
    Termux 下的 SSH 默认端口为8022，配置文件位于 `$PREFIX/etc/ssh/sshd_config`
    ```
    PrintMotd yes
    PasswordAuthentication yes
    Subsystem sftp /data/data/com.termux/files/usr/libexec/sftp-server
    ```
    用`paswd`命令修改用户密码后就可以使用密码登录SSH
  * 通过密钥对连接

    生成密钥对
 
    `ssh-keygen`

    在 ~/.ssh 目录下生成 3 个文件 `id_rsa`(私钥)、`id_rsa.pub`(公钥)、`known_hosts`，将公钥内容复制到 `.ssh/` 目录下的 `authorized_keys `中

    `cat id_rsa.pub > authorized_keys`
 
    修改ssh配置

    `vim $PREFIX/etc/ssh/sshd_config`

    `PasswordAuthentication no`

    重启ssh
    ```
    pkill sshd
    sshd
    ```

* 娱乐一下
  ``` 
  cmatrix      # 《黑客帝国》的代码雨视觉特效
  cowsay       # 用 ASCII 字符描绘牛，羊和许多其他动物
  figlet       # 创建 ASCII logo
  hollywood    # 终端中伪造黑客屏幕，假装是黑客
  nyancat      # 彩虹猫
  sl           # ls输错，火车来了
  toilet       # 用字母拼写出更大字母的工具
  screenfetch/neofetch       # 显示系统信息
  `echo "xxxxx" |curl -F-=\<- qrenco.de`  # 生成二维码
  ```

## termux 其它应用
* 搭建 hexo 
  ```
  pkg install nodejs-lts
  npm install hexo-cli -g
  hexo init myblog_hexo
  cd myblog_hexo
  npm install hexo-theme-fluid
  cp node_modules/hexo-theme-fluid/_config.yml _config.fluid.yml
  hexo g
  hexo s					
  ```
  默认访问地址 http://localhost:4000

* 搭建hugo
  ```
  pkg install hugo -y
  hugo new site myblog_hugo
  cd myblog_hugo
  git clone https://github.com/olOwOlo/hugo-theme-even themes/even
  cp themes/even/exampleSite/config.toml ./
  hugo server -D
  ```
  默认访问地址 http://localhost:1313

* 安装 linux 发行版
  使用官方工具[proot-distro](https://github.com/termux/proot-distro)安装 linux 
  ```
  pkg install proot-distro
  ```




---

> 作者: LukeIO  
> URL: https://lukeio.github.io/posts/slugorcontentbasename/  

