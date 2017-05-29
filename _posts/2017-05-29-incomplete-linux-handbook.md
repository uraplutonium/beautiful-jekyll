---
layout: post
title: An incomplete Linux handbook
subtitle: 一个不完整的Linux生存手册
image: /ico/icon-linux.png
#bigimg: /img/path.jpg
tags: [linux]
---

# heading 1
## 锁定笔记本触控板
在终端中输入：
	
~~~
sudo modprobe -r psmouse
~~~

即可锁定触控板。该命令仅在下次重启前有效。

## crontab计划任务
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
