archlinux 安装过程所得

﻿fcitx fcitx-table-extra tint2 openbox xorg-server xorg-xinit alsa-utils ntfs-3g wifi-menu netctl dialog feh volumeicon rox slim man base base-devel openssh git rsync pciutils usbutils aria2
 
fcitx问题
安装fcitx,fcitx-table-extra可得到wubi，不过无法打出中文，纠结很久最后在～/.bash_profile里加入一行：

export XMODIFIERS="@im=fcitx"

slim+openbox问题

安装了slim和openbox，但是无法用startx启动，wiki上说编辑~/.xinitrc，可是没有找到，在/etc/skel中也没有，怀疑wiki已过时，后来通过

systemctl start slim

来成功启动登录界面，不过无法进入，最后在~/.bash_profile里加入下面一行：

exec openbox-session

不过会出现该用户无法在终端登录的现象，无奈，最后尝试自己建立~/.xinitrc，并加入上面一行，成功！！


终端

想直接安装gnome-terminal，可是安装好启动不了，它依赖gtk3.0了，还是先安装个最原始的吧xterm，没终端不可活！

thunar图标

安装了文件管理器可是里面没有图标，于是安装了gnome的图标gnome-icon-theme

alsa-utils

安装了后没声音，一直没搞懂，最近是安装了gnome后好了

pacman -S gnome

pulseaudio

安装了这个后就可以有声音，不过openbox开机并没有启动，所以会出现上面这样的现象，运行

start-pulseaudio-x11

就可以启动了，加入开机运行脚本就OK，在此推荐一个声音管理软件---pavucontrol

ntfs-3g

安装此软件，即可对ntfs文件系统进行访问

关于网络

8.1 netctl    /console tools: netctl,wifi-menu

8.2 networkmanager        /console tools: nmcli

8.3 wicd        /console tools: wicd-curses

wifi-menu开机自启动

systemctl enable netctl-auto@wlp4s0-tux.service

wlp4s0-tux为配置文件，因人而异

evince中文显示问题

安装popper-data

关于时间问题

ln -sf /usr/share/zoneinfo/Etc/Greenwin /etc/localtime

图形与字符的切换

systemctl isolate runlevel3.target  OR multi-user.target

systemctl isolate runlevel5.target  OR graphical.target

以上的只是临时的，下次开机时不影响，想下次开机生效，如下：

systemctl set-default(-f) multi-user.target OR graphical.target

samba的配置

配置文件在/etc/samba/smb.conf，要自己建立，有模板

启动服务 systemctl start smbd

相应的服务一定要启动:nmbd

share的安全级别会被忽略，要建一个用户，设一个密码来访问:smbpasswd -a username

要想任何用户来访问，方法如下：

global中添加security=user;map to guest=bad user;guest account=nobody,share中添加guest ok=yes

htop

top的另一个版本，简洁，且带有颜色    

ifconfig command is in the package "net-tools"
install ansys for linux

1)15.0以前的版本都试了，没成功，不知是什么原因，个人感觉是licensing的问题

2)15.0的版本无论在WIN还是LINUX上，都成功安装，只安装products，不安装LM，然 

  后直接把ansyslmd.ini和license.dat复制到ansys_inc/shared_files/licensing下

  修改ansyslmd.ini中相应的路径,OK,please enjoy!

无线网卡被禁用，不知为何弄好了，熟悉了一些网络命令，如下：

iw wlan0 link   查看是否连接

iw dev          查看端口号

iw link set wlan0 up    启用网卡

1.grub> loopback loop (hd0,1)/ubuntu.iso
利用grub2的回放设备，挂iso,这样可以使你不用把casper文件夹提取出来，就能从iso中启动。

2.grub> set root=(loop)

这是设置grub的根目录。

3.grub> linux /casper/vmlinuz boot=casper iso-scan/filename=/ubuntu.iso

这是让grub挂内核。并传递参数boot=casper 给initramfs

4.grub> initrd /casper/initrd.lz

设置系统引导

5.grub> boot

开始启动引导

wicd的使用

wicd安装后只有命令行，要安装wicd-gtk才可以有图形界面，启动图形界面wicd-client和wicd-gtk是一样的，不过先要运行wicd，否则会出现端口错误

systemctl enable wicd 实现开机自启动

title Windows 7 ISO
find --set-root /boot/iso/ylmf_ghostwin7sp1_yn2013_x86.iso
map /boot/iso/ylmf_ghostwin7sp1_yn2013_x86.iso (0xff)
map --hook
root (0xff)
chainloader (0xff)
mount /dev/sdax /xx -o rw,uid=0(root),gid=0(root),utf8
grub4dos引导windows

直接解压ISO，然后用grub4dos来引导，命令chainloader /BOOTMGR

或者直接解压出ghost文件，手动还原也可以！！

scp的命令使用需要ssh，openssh-client ,openssh-server

mplayer截屏

mplayer -vf screenshot filename 按s就可截屏

或在 ～/.mplayer/config中添加vf=screenshot

mount -o iosharset=utf8 /dev/sdbx /mnt 解决中文显示问题
单个文件最大下载速度： aria2c --max-download-limit=300K

整体下载最大速度：aria2c --max-overall-download-limit=300k

aria2c --header=“Cookie:cookie名称=cookie内容“

vim语法高亮 <command> :syntax enable
添加中文社区源

[archlinuxcn]
Include = /etc/pacman.d/mirrorlist-cn
mirrorlist-cn内容如下：
Server = http://mirrors.ustc.edu.cn/archlinuxcn/$arch
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
Server = http://mirrors.hustunique.com/archlinuxcn/$arch
HoldPkg = package …

IgnorePkg = package …
IgnoreGroup = group …
mencoder处理视频

mencoder 1.mp4 2.mp4 -oac pcm -ovc copy  -o ok.mp4          //合并MP4视频
mencoder -ss 1:00  -endpos 3:00 -oac pcm -ovc copy orgin.mp4 -o 1.mp4        //分割视频    
xorg-xinit

安装startx 命令
mencoder -oac mp3lame -ovc copy -of rawaudio 视频文件 -o 音频文件

Mod4 + Enter            打开一个终端
Mod4 + r                运行命令
Mod4 + Shift + c        关掉当前窗口
Mod4 + m                最大化当前窗口
Mod4 + Control + r      重启awesome
Mod4 + Shift + q        退出awesome
Mod4 + j                切换到下一个窗口
Mod4 + k                切换到前一个窗口
Mod4 + Left             查看前一个tag
Mod4 + Right            查看后一个tag
Mod4 + 1-9              切换到tag 1-9
Mod4 + Control + j      切换到下一个屏幕
Mod4 + Control + k      切换到前一个屏幕
Mod4 + Shift + j        当前窗口和前一个窗口互换位置
Mod4 + Shift + k        当前窗口和后一个窗口互换位置
Mod4 + h                把主区域(master width)的宽度增大5%
Mod4 + l                把主区域(master width)的宽度减少5%
Mod4 + Shift + h        增加主区域窗口的数量
Mod4 + Shift + l        减少主区域窗口的数量
Mod4 + Control + h      增加非主区域窗口的数量
Mod4 + Control + l      减少非主区域窗口的数量
Mod4 + space            把当前tag更换为下一种布局
Mod4 + Shift + space    把当前tag更换为前一种布局
Mod4 + Control + space  切换当前窗口是否为浮动的
Mod4 + Shift + i        显示当前窗口的class和instance。这在写脚本的时候尤其有用
Mod4 + Shift + r        重绘当前窗口
Mod4 + t                标记窗口（可标记多个）
Mod4 + Shift + F1~F9    把标记的窗口移动到第一~第九桌面上
Ctrl + Mod4 + 1~9       把当前桌面和1~9桌面同时显示
Mod4 + 1~9              恢复
Mod4 + Esc              快速切换到上一个桌面
32. OpenVPN Setup

作为用户，需要从服务商那里获得几个证书：user certificate, ca certificate,privae key.

还有几个可有可无的:    tls-auth,extra certs

然后再写一个openVPN的conf如下：

/etc/openvpn/client.conf
remote elmer.acmecorp.org 1194
.
.
user nobody
group nobody
ca /etc/openvpn/ca.crt
cert /etc/openvpn/bugs.crt
key /etc/openvpn/bugs.key
.
.
tls-auth /etc/openvpn/ta.key 1

Then, run command "openvpn configure_file".

OK! Enjoy it.



33. shadowsocks proxy

    First, install shadowsocks and start it

    Second, install autoproxy for firefox(author:hrimfaxi) or install proxy switchysharp for chrome

    then, 订阅GFWlist,编辑代理，添加SS，端口与Shadowsocks的代理端口一致(1080一般), 选择默认代理为SS,模式为自动代理.



