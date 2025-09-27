# Markdown

**Markdown**基本格式

# **标题**
```
# 一级标题
## 二级标题
### 三级标题
```
# 一级标题
## 二级标题
### 三级标题

# **列表项**
```
* 第一项
- 第二项
* 第三项
  * abcdef
    * opqrst
    * uvwxyz
  * abcdef
    * opqrst
    * uvwxyz
- 第四项
  - abcdef
    - opqrst
    - uvwxyz
  - abcdef
    - opqrst
    - uvwxyz
1. 第一项
2. 第二项
3. 第三项
```
* 第一项
- 第二项
* 第三项
  * abcdef
    * opqrst
    * uvwxyz
  * abcdef
    * opqrst
    * uvwxyz
- 第四项
  - abcdef
    - opqrst
    - uvwxyz
  - abcdef
    - opqrst
    - uvwxyz
1. 第一项
2. 第二项
3. 第三项

# **任务列表**
```
- [ ] 项目一
- [x] 项目二
```
- [ ] 项目一
- [x] 项目二

# **段落**
  段落之间用空行分割

  换行  一行结束时输入两个空格

# **内容对齐**
```
<p align="left">左对齐</p>
<p align="center">居中</p>
<p align="right">右对齐</p>
```
<p align="left">左对齐</p>
<p align="center">居中</p>
<p align="right">右对齐</p>

# **分隔线**

```
---
___
```
---
___
# **脚注**
```
这里有脚注[^1]
这里有脚注[^note]
这里有脚注[^注释]
[^1]:脚注1的内容
[^note]:脚注2的内容
[^注释]:脚注3的内容
```
这里有脚注[^1]
这里有脚注[^note]
这里有脚注[^注释]
[^1]:脚注1的内容
[^note]:脚注2的内容
[^注释]:脚注3的内容

# **文本格式**
```
*斜体*
_斜体_
**粗体**
__粗体__
***粗斜体***
___粗斜体___
~~删除线~~
==高亮==
<font color="blue">字体颜色</font>
```
*斜体*

_斜体_

**粗体**

__粗体__

***粗斜体***

___粗斜体___

~~删除线~~

==高亮==

<font color="blue">字体颜色</font>

# **引用**
```
> Markdown 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
>> 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。
```
> Markdown 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
>> 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，许多网站都广泛使用Markdown来撰写帮助文档或是用于论坛上发表消息。

## **定位标记**
```
[转到列表项](#列表项)
[转到三级标题](#三级标题)
```
[转到列表项](#列表项)

[转到三级标题](#三级标题)

# **插入链接**
```
[Google](https://www.google.com)
```
[Google](https://www.google.com)

# **插入图片**
```
![图片名称](图片链接或路径)
<img src="图片链接或路径" width="300" alt="图片名称"></img>
```
![图片名称](图片链接或路径)
<img src="图片链接或路径" width="300" alt="图片名称"></img>

# **插入代码**
```
`int  a b c`
```
`int  a b c`

````
```
int  a  b  c
a=b+c
```
````
```
int  a  b  c
a=b+c
```

# **插入数学式**
```
$x^2+y^3=z$
```
$x^2+y^3=z$

```
$$
y=x^2+2a+b/2
$$
```
$$
y=x^2+2a+b/2
$$
# **插入表格**
```
| HEADER | HEADER | HEADER |
|:----:|:----:|:----:|
|      |      |      |
|      |      |      |
|      |      |      |
```
| HEADER | HEADER | HEADER |
|:----:|:----:|:----:|
|      |      |      |
|      |      |      |
|      |      |      |




---

> 作者: LukeIO  
> URL: https://lukeio.github.io/posts/slugorcontentbasename/  

�
pkg show <package>              # 显示某个包的详细信息
pkg files <package>             # 显示某个包的相关文件夹路径	
```

安装几个基础应用

```
pkg update
pkg install vim curl wget git tree -y
```

`pkg` 命令每次安装的时候自动执行 `apt update` 命令，建议使用。如有 deb 包，还可以使用`dpkg`安装
						
```
dpkg -i ./package.deb        # 安装 deb 包
dpkg --remove [package name] # 卸载软件包
dpkg -l                      # 查看已安装的包
man dpkg                     # 查看详细文档	
```

## termux 文件目录
```
echo $HOME
/data/data/com.termux/files/home
echo $PREFIX
/data/data/com.termux/files/usr
echo $TMPDIR
/data/data/com.termux/files/usr/tmp/		
```

`export`命令显示更多信息

## termux 优化设置
* 更换国内源
  * 用 `termux-change-repo` 命令替换
  * 输入以下命令替换
    ```
    sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
    sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
    sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
    apt update && apt upgrade
    ```

    见[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)

* 安装oh-my-zsh

  使用 zsh 替代 bash ，并安装 oh-my-zsh 。项目地址: https://github.com/Cabbagec/termux-ohmyzsh/

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

