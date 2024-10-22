# Termux


参考[Termux 高级终端安装使用配置教程](https://www.sqlsec.com/2018/05/termux.html)整理

##  termux简介
&gt; Termux是一个Android终端仿真器和Linux环境应用程序，直接工作，无需根目录或设置。一个最小的基本系统被自动安装-额外的软件包可以使用APT软件包管理器来使用。不需要root，运行于内部存储。

* [termux官网](https://termux.com/)
* [termux wiki](https://wiki.termux.com/wiki/Main_Page)
* [github地址](https://github.com/termux/termux-app)

## termux 安装
* 下载安装包并安装

  Googleplay 上的版本已不再更新(见[说明](https://wiki.termux.com/wiki/Termux_Google_Play)),官方推荐到 **f-droid** [下载](https://f-droid.org/packages/com.termux/)
* 第一次启动Termux的时候需要从[服务器](http://termux.net/bootstrap/)加载数据,如出现提示:
  &gt; Ubable to install Termux was unable to install the bootstrap packages.Check your ntwork connection and try again.
 
  需要科学上网。

## termux 基本操作
* 调整屏幕字体大小，双指捏合或展开
* 长按屏幕，会出现菜单项，
  * copy 
  * paste
  * more
    * Select URL      # 提取屏幕上的链接
    * Share transcipt # 分享命令
    * Reset           # 重置
    * Kill process    # 结束当前进程
    * Style           # 风格配色
    * Keep screen on  # 屏幕常亮
    * help            # 帮助
    * setting         # 设置
					
* 屏幕左上角从左向右滑动，调出菜单，可以新建、切换会话。长按 keyboard 可以打开/关闭输入法工具条（扩展按键）  
* 常用快捷键 
    * termux 中手机**音量减**键相当于**Ctrl**键，快捷键组合如下
      ```   
      Ctrl &#43; A -&gt; 将光标移动到行首
      Ctrl &#43; C -&gt; 中止当前进程
      Ctrl &#43; D -&gt; 注销终端会话			
      Ctrl &#43; E -&gt; 将光标移动到行尾
      Ctrl &#43; K -&gt; 从光标删除到行尾
      Ctrl &#43; U -&gt; 从光标删除到行首
      Ctrl &#43; L -&gt; 清除终端
      Ctrl &#43; Z -&gt; 挂起（发送SIGTSTP到）当前进程
      Ctrl &#43; alt &#43; C -&gt; 打开新会话
      ```
						
    * **音量加**键的组合
      ```
      音量加 &#43; E -&gt; Esc键
      音量加 &#43; T -&gt; Tab键
      音量加 &#43; 1 -&gt; F1（音量增加 &#43; 2 → F2…以此类推）
      音量加 &#43; 0 -&gt; F10
      音量加 &#43; B -&gt; Alt &#43; B，使用readline时返回一个单词
      音量加 &#43; F -&gt; Alt &#43; F，使用readline时转发一个单词
      音量加 &#43; X -&gt; Alt&#43;X
      音量加 &#43; W -&gt; 向上箭头键
      音量加 &#43; A -&gt; 向左箭头键
      音量加 &#43; S -&gt; 向下箭头键
      音量加 &#43; D -&gt; 向右箭头键
      音量加 &#43; L -&gt; | （管道字符）
      音量加 &#43; H -&gt; 〜（波浪号字符）
      音量加 &#43; U -&gt; _ (下划线字符)
      音量加 &#43; P -&gt; 上一页
      音量加 &#43; N -&gt; 下一页
      音量加 &#43; . -&gt; Ctrl &#43; \（SIGQUIT）
      音量加 &#43; V -&gt; 显示音量控制
      音量加 &#43; Q -&gt; 切换显示的功能键视
      音量加 &#43; K -&gt; 切换显示的功能键视图
      ```
## termux 包管理
termux 支持`pkg`和`apt`，`pkg`兼容`apt`

```
pkg search &lt;query&gt;              # 搜索包
pkg install &lt;package&gt;           # 安装包
pkg uninstall &lt;package&gt;         # 卸载包
pkg reinstall &lt;package&gt;         # 重新安装包
pkg update                      # 更新源
pkg upgrade                     # 升级软件包
pkg list-all                    # 列出可供安装所有包
pkg list-installed              # 列出已经安装的包
pkg show &lt;package&gt;              # 显示某个包的详细信息
pkg files &lt;package&gt;             # 显示某个包的相关文件夹路径	
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
    sed -i &#39;s@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@&#39; $PREFIX/etc/apt/sources.list
    sed -i &#39;s@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@&#39; $PREFIX/etc/apt/sources.list.d/game.list
    sed -i &#39;s@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@&#39; $PREFIX/etc/apt/sources.list.d/science.list
    apt update &amp;&amp; apt upgrade
    ```

    见[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)

* 安装oh-my-zsh

  使用 zsh 替代 bash ，并安装 oh-my-zsh 。项目地址: https://github.com/Cabbagec/termux-ohmyzsh/

  使用脚本
  ```
  sh -c &#34;$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)&#34;
  ```
  弹框确认，允许访问文件。不小心拒绝，使用`termux-setup-storage` 命令可以重新获取访问权限。

  脚本执行后提示设置色彩样式和字体样式
  &gt; Enter a number, leave blank to not to change: 14 Enter a number, leave blank to not to change: 4

  可以用`chcolor`和`chfont`命令再次修改色彩和字体

* 定制扩展按键

  通过 `~/.termux/termux.properties` 定制扩展按键，加入以下内容:
  ```
  extra-keys=[[&#39;ESC&#39;,&#39;|&#39;,&#39;/&#39;,&#39;-&#39;,&#39;HOME&#39;,&#39;UP&#39;,&#39;END&#39;,&#39;PGUP&#39;],[&#39;TAB&#39;,&#39;CTRL&#39;,&#39;ALT&#39;,&#39;DEL&#39;,&#39;LEFT&#39;,&#39;DOWN&#39;,&#39;RIGHT&#39;,&#39;PGDN&#39;]]
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
  &#34;解决中文乱码
  set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
  set enc=utf8
  set fencs=utf8,gbk,gb2312,gb18030
  &#34;显示行号
  set nu
  &#34;颜色主题
  colorscheme desert
  &#34;打开语法高亮
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

    `cat id_rsa.pub &gt; authorized_keys`
 
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
  `echo &#34;xxxxx&#34; |curl -F-=\&lt;- qrenco.de`  # 生成二维码
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
> URL: https://lukeio.github.io/posts/termux/  

