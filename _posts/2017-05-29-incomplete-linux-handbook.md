---
layout: post
title: An incomplete Linux handbook
subtitle: 一份不完整的Linux生存手册
image: /ico/icon-linux.png
#bigimg: /img/path.jpg
tags: [linux]
---

# This page is under construction...
# 该页面建设中...

[1. Linux installation 系统安装](#1)

+ [1.1 制作系统启动盘](#dd)
+ [1.2 GRUB2引导区设置](#lock-touchpad)
+ [1.3 系统分区自动挂载（fstab）](#lock-touchpad)
+ [1.4 使用内存加载临时目录/tmp](#lock-touchpad)
+ [1.5 挂载加密分区](#lock-touchpad)
+ [1.6 减少swap写入](#lock-touchpad)
+ [1.7 打开和关闭交换分区](#lock-touchpad)
+ [1.8 开机自动启动后台进程](#lock-touchpad)
+ [1.9 CentOS软件源](#lock-touchpad)

[2. Linux settings 系统设置](#1)

+ [2.1 显示所有开机自动启动项目](#lock-touchpad)
+ [2.2 锁定笔记本触控板](#lock-touchpad)
+ [2.3 ubuntu中合上屏幕后无操作](#lock-touchpad)
+ [2.4 查看系统信息](#lock-touchpad)
+ [2.5 自定义命令](#lock-touchpad)
+ [2.6 使用系统为父进程执行命令](#lock-touchpad)
+ [2.7 禁用CentOS7的IPv6和防火墙](#lock-touchpad)
+ [2.8 向sudo group中添加用户](#lock-touchpad)
+ [2.9 设置文件默认打开方式](#lock-touchpad)
+ [2.10 启用nautilus上一级目录的BackSpace快捷键](#lock-touchpad)
+ [2.11 删除ubuntu unity桌面lenses](#lock-touchpad)
+ [2.12 更改Gnome桌面默认文件夹地址](#lock-touchpad)
+ [2.13 gnome ibus输入法设置](#lock-touchpad)
+ [2.14 手动创建Gnome桌面快捷方式](#lock-touchpad)
+ [2.15 修复ibus候选框不跟踪光标](#lock-touchpad)
+ [2.16 修复gvfs-metadata大量消耗内存](#lock-touchpad)

[3. Linux commands & tools 命令与工具](#3)

+ [3.1 crontab计划任务](#crontab)
+ [3.2 rsync文件同步](#lock-touchpad)
+ [3.3 通过代理服务器使用yum和dnf](#lock-touchpad)
+ [3.4 maven local repository](#lock-touchpad)
+ [3.5 向Gnome桌面发送通知](#lock-touchpad)
+ [3.6 ssh免密码登录](#lock-touchpad)
+ [3.7 ssh反向隧道](#lock-touchpad)
+ [2.8 samba共享文件夹设置](#lock-touchpad)
+ [3.9 apache2和php5网站](#lock-touchpad)
+ [3.10 将视频转换为mp4格式](#lock-touchpad)

<h2 id='1'> 1. Linux installation 系统安装 </h2>

<h3 id='dd'> 1.1 制作系统启动盘 </h3>

使用dd将已下载好的linux系统镜像[os-image].iso写入到优盘上：

~~~
sudo dd if=[os-image].iso of=/dev/sdx bs=1M
~~~

优盘的设备文件为/dev/sdx，其中编号x可使用fdisk查询：

~~~
sudo fdisk -l
~~~

----------------------------------------------------------------

<h3 id='grub2'> GRUB2引导区设置 </h3>

GRUB2包含3部分文件：
- /etc/default/grub    grub的自定义文件
- /etc/grub.d/         各操作系统的启动脚本
- /boot/grub2/grub.cfg  最终生成的主配置文件(或/boot/grub/grub.cfg)

/etc/grub.d/目录中默认包含以下文件：
- 00_header
- 10_linux
- 20_memtest86+
- 30_os-prober
- 40_custom

GRUB2 的/usr/sbin/grub2/grub2-mkconfig会根据/etc/default/grub文件和/etc/grub.d/中的脚本文件生成/boot/grub2/grub.cfg(或/boot/grub/grub.cfg)

+ 为linux添加grub启动项
在/etc/grub.d/中添加启动脚本11_linux：

~~~
#!/bin/sh -e
echo "Adding my custom Linux to GRUB 2"
cat << EOF
menuentry "My custom Linux" {
set root=(hd0,5)
linux /boot/vmlinuz
initrd /boot/initrd.img
}
EOF
~~~

menuentry一行为显示在GRUB2启动界面上的名称，set一行用来标示启动的硬盘和分区，特别要注意与GRUB不同的是，在GRUB2中硬盘的分区是从1开始计数的，而非GRUB中的0。但硬盘编号依然是从0开始计数的。例如(hd0,5)标示第1块硬盘上的第5个分区。

+ 为windows添加grub启动项
在/etc/grub.d/中添加启动脚本12_windows：

~~~
#!/bin/sh -e
echo "Adding Windows 7 to GRUB 2 menu"
cat << EOF
menuentry "Windows 7" {
set root=(hd0,1)
chainloader (hd0,1)+1
}
EOF
~~~

+ 生成/boot/grub2/grub.cfg
为添加的启动脚本添加执行权限：
```shell
sudo chmod a+x /etc/grub.d/11_linux
sudo chmod a+x /etc/grub.d/12_windows
```

生成/boot/grub2/grub.cfg：
~~~
sudo /usr/sbin/grub2/grub2-mkconfig --output=/boot/grub2/grub.cfg
~~~
注意此处若不添加output参数则会将生成的grub.cfg输出在默认输出设备（即屏幕）上。

将grub设置写入硬盘分区：
~~~
sudo grub-install /dev/sda
~~~

完成。重新启动：
~~~
sudo reboot
~~~

----------------------------------------------------------------

<h2 id='2'> 2. Linux settings 系统设置 </h2>

<h3 id='lock-touchpad'> 2.2 锁定笔记本触控板 </h3>

在终端中输入：
	
~~~
sudo modprobe -r psmouse
~~~

即可锁定触控板。该命令仅在下次重启前有效。

----------------------------------------------------------------

<h2 id='3'> 3. Linux commands & tools 命令与工具 </h2>


<h3 id='crontab'> 3.1 crontab计划任务 </h3>

crontab可以定时、周期性地执行命令或脚本。
以“每周一凌晨3点执行备份文件的脚本upn-backup.sh”为例：
+ 首先，安装crontab软件：

~~~
sudo apt-get install corntab
~~~

+ 然后，创建crontab文件（~/crontab-file）：

~~~
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

0 3	* * 1	/home/uraplutonium/upn-backup.sh
~~~

如默认crontab文件的注释所说，每一行的前5个数字分别代表执行命令的分钟（0-59）、小时（0-23）、日期（1-31）、月份（1-12）和星期（1-7）。星号“*”代表不作限制。“0 3	* * 1”代表每个月、每个星期一的03点00分执行命令。

+ 最后，在终端中输入下列语句来加载crontab文件：

~~~
crontab ~/crontab-file
~~~

要查看是否设置成功，可以执行下列命令来查看当前所有的计划任务：

~~~
crontab -l
~~~

----------------------------------------------------------------

