---
layout: post
title: 新arch安装小计
tags: linux
category: 学习
---



不用arch已经有一段时间啦，看完孙同学的帖子，又勾起了我的。。
小记一下这次安装,主要是遇到的问题

安装基本系统

这次arch的安装方式和以前大不一样，还是看wiki。因为正在使用ubuntu，/home 的挂载分区准备留着 

* 首先在win上下载了一个linuxliveusb-creator 的小软件做了一个 arch的usb安装u盘（有点惭愧啊，用瘟到死啦，历史因素，当还是小白时就用这个了），wiki上提到还可以在linux上用dd直接将镜像写到优盘。 64位arch的安装（个人喜好），这次就没有原来的setup工具啦，看wiki的流程，需要手动对硬盘进行分区，用fdisk或者cfdisk，cfdisk 稍微直观一点，因为我没有想要改变分区，所以直接对硬盘进行格式化，我习惯linux分区为四个，/boot ,/ ,/home ,swap。所以我只对boot和根需要的分区格式化,详细请看 [archwiki](https://wiki.archlinux.org/index.php/Installation_Guide).

* 安装基本系统一定要注意各分区在/mnt下的挂载, pacstrap 安装完基本系统后 chroot到 /mnt下目的系统时,就是需要修改一些配置文件(除了下面其他的没有修改,应为不太懂), 我修改了:

    locale.conf

    	LANG="en_US.UTF-8"
    
    locale.gen  将其中 zh_cn 开头的行解注释 , 还有 en_US.UTF-8 解注释, 是本地支持所选的字符集好要执行命令:

    	locale-gen

    设定时区:
    
    	ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    
    mkinitcpio -p linux  生成内核镜像
    wiki中前几步中已经把 grub 安装到系统里了, 配置完以上以后, 就要配置grub了, 首先将grub的引导
    安装到硬盘, 然后生成 grub.cfg
    
    	grub-install   /dev/sda
    	grub-makconfig  > /boot/grub/grub.cfg

    exit 退出目的系统, unmount 后 重启
    
* 接下来要配置x 先执行 [archwiki](https://wiki.archlinux.org/index.php/Beginners%27_Guide) 上的几步,测试完x 之后安装桌面环境, 我这几天一直在用kde 所以来个kde, 我先安装了 kdebase一路默认安完然后又安装 dbus, 接着应该用 systemctl enbale dus   让他开机启动, 但报错啦, 有人说使用systemd就默认开启啦, 后来正式确实是, 我的命令如下, 开始前先对 mirrlist 修改, 我将163的放在最前面,然后执行 dhcpd 获取ip:

	    pacman -S  kdebase
   		pacman -S  kde-l10n-zh_cn
    	pacman -S  wqy-zenhei
    	systemctl enable kdm

* 接下来添加一个的用户, 重启就能进kde啦
* 装完系统后就来配置我的开发环境, ruby on rails, 当然是 [ruby-chian-wiki](http://ruby-china.org/wiki/install_ruby_guide)所不同的是 依赖包,  先不用装 ,先装好rvm ,然后  rvm requirement  查看提示安装依赖包. 安装 mysql后配置root密码,使用命令: 

		mysql-secure-installation .

* zsh 和 oh-my-zsh 的主题  [参考](https://github.com/robbyrussell/oh-my-zsh) github上的安装指南
* vim 并安装bundule [参考](https://github.com/gmarik/vundle)
* 播放器 mplayer 还有 音乐播放器cmus , 我的第二编辑器 sublime(这个要先安装yaourt [参考](https://wiki.archlinux.org/index.php/Yaourt))gtalk  kde下用 kdenetwork-kopete, 还有新发现的一个好用的终端模拟器  yakuake, chromium 浏览器
* 中文输入 fcitx 并  yaourt 了 googlepinyin 输入法 ,fcitx需要简单配置, 编辑 ~/.xprofile 添加:

    	export XMODIFIERS=@im=fcitx
    	export GTK_IM_MODULE=fcitx
    	export QT_IM_MODULE=fcitx

* 还有就是装完后状态栏没有声音选项, 然后又装了  kde-multimedia.现在就是这些
