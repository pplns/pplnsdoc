fail2ban的安装说明
===

****
***Author***: Neal Zhang

***Email***: master@pplns.com

****
目录
----
*   [概述] (#概述)
*   [资源获取] (#资源获取)
*   [安装条件] (#安装条件)
    *   [必需条件] (#必需条件)
    *   [可选条件] (#可选条件)
*   [安装步骤] (#安装步骤)
    *   [centos6.5] (#centos6.5)
    *   [ubuntu14.04] (#ubuntu14.04)
*   [配置说明] (#配置说明)
*   [注意事项] (#注意事项)

****
概述
----

fail2ban是一款实用软件，可以监视你的系统日志，然后匹配日志的错误信息（正则式匹配）执行相应的屏蔽动作。

****
资源获取
----

网站：www.fail2ban.org

github：https://github.com/fail2ban/fail2ban

****
安装条件
----

必须条件：

Python2 >= 2.6 or Python >= 3.2 or PyPy

可选条件：

pyinotify >= 0.8.3

Linux >= 2.6.13

gamin >= 0.0.21

systemd >= 204

dnspython

****
安装步骤
----

* 下载：

   git clone https://github.com/fail2ban/fail2ban.git

* 安装：

```
cd fail2ban

python setup.py install
```

* 相关命令：

fail2ban-client (start|stop|restart|status)

* centos6.5环境:

设置服务：

cp fail2ban/files/redhat-initd /etc/init.d/fail2ban

设置开机自启动：

sudo chkconfig --list fail2ban

chkconfig --add fail2ban

chkconfig fail2ban on

sudo chkconfig --list fail2ban

* ubuntu 14.04环境:

设置服务：

cp fail2ban/files/debian-initd /etc/init.d/fail2ban

chmod 755 /etc/init.d/fail2ban

设置开机自启动：

update-rc.d fail2ban defaults

* 服务命令：

service fail2ban (start|stop|restart|status)

****
配置说明
----

* 目录简介

1./etc/fail2ban/ 为配置文件目录;

2./etc/init.d/fail2ban 为启动服务

3./etc/fail2ban/fileter.d  预定义的日志过滤器目录

4./etc/fail2ban/action.d  预定义的禁止操作目录

* 主要文件

1.fail2ban.conf为fail2ban的主配置文件;

2.jail.conf为定义的需要处理的的动作是否生效，指定需要分析的logpath等。

* fail2ban.conf 

```
[Definition]
loglevel = INFO #默认日志的级别
logtarget = /var/log/fail2ban.log #日志的位置
socket = /var/run/fail2ban/fail2ban.sock #socket的位置(默认即可)
```

* jail.conf

全局配置：

```
[DEFAULT]
#全局变量
ignoreip = 127.0.0.1       #忽略IP,在这个清单里的IP不会被屏蔽
bantime = 600      #屏蔽时间，单位秒，该bantime将被[ssh-iptables]中bantime覆盖；
findtime = 600      #发现时间，在此期间内重试超过规定次数，会激活fail2ban
maxretry = 3      #默认尝试次数
backend = auto      #日志修改检测机制
```

ssh监狱：

```
[sshd]
enabled = true
filter = sshd
port = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s

```

centos系统下apache监狱:

```
# 检测密码认证失败
[apache]
enabled =true
port = http,https
filter = apache-auth
logpath =/var/log/httpd/*error_log
maxretry = 6
# 检测抓取邮件地址的爬虫
[apache-badbots]
enabled = true
port = http,https
filter = apache-badbots
logpath = /var/log/httpd/*access_log
bantime = 172800
maxretry = 1
# 检测漏洞和 PHP 脆弱性扫描
[apache-noscript]
enabled = true
port = http,https
filter = apache-noscript
logpath = /var/log/httpd/*error_log
maxretry = 6
# 检测 Apache 溢出攻击
[apache-overflows]
enabled = true
port = http,https
filter = apache-overflows
logpath = /var/log/httpd/*error_log
maxretry = 2
# 检测在服务器寻找主目录的尝试
[apache-nohome]
enabled = true
port = http,https
filter = apache-nohome
logpath = /var/log/httpd/*error_log
maxretry = 2
# 检测执行不存在的脚本的企图
# 这些都是流行的网站服务程序
# 如：webmail， phpMyAdmin，WordPress
port = http,https
filter = apache-botsearch
logpath = /var/log/httpd/*error_log
maxretry = 2
```

ubuntu系统下apache监狱:

```
# 检测密码认证失败
[apache]
enabled =true
port = http,https
filter = apache-auth
logpath =/var/log/apache*/*error.log
maxretry = 6
# 检测漏洞和 PHP 脆弱性扫描
[apache-noscript]
enabled = true
port = http,https
filter = apache-noscript
logpath = /var/log/apache*/*error.log
maxretry =6
# 检测 Apache 溢出攻击
[apache-overflows]
enabled =true
port = http,https
filter = apache-overflows
logpath =/var/log/apache*/*error.log
maxretry = 2
# 检测在服务器寻找主目录的尝试
[apache-nohome]
enabled = true
port = http,https
filter = apache-nohome
logpath = /var/log/apache*/*error.log
maxretry =2
```

****
注意事项
----

从fail2ban 0.10.0版本之后，开始支持匹配IPv6地址






