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

使用dd将已下载好的linux系统镜像[os-image].iso写入到优盘上。

~~~
sudo dd if=[os-image].iso of=/dev/sdx bs=1M
~~~

优盘的设备文件为/dev/sdx，其中编号x可使用fdisk查询：

~~~
sudo fdisk -l
~~~

---

<h2 id='2'> 2. Linux settings 系统设置 </h2>

<h3 id='lock-touchpad'> 2.2 锁定笔记本触控板 </h3>

在终端中输入：
	
~~~
sudo modprobe -r psmouse
~~~

即可锁定触控板。该命令仅在下次重启前有效。

---

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


