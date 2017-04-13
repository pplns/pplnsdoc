tomcat-postgresql架构项目部署文档
===

***
**Author**: Neal Zhang

**Email**: Loong@pplns.com

此文档用于说明tomcat-postgresql架构项目在ubuntu环境下的部署工作

***
目录
----
* [准备工作] (#准备工作)
* [java环境配置] (#java环境配置)
* [postgresql数据库安装与初始化] (#postgresql数据库安装与初始化)
* [Tomcat配置] (#Tomcat配置)
* [常用操作] (#常用操作)
* [异常处理]  (#异常处理)
* [补充---Centos 6.5环境下postgresql的安装] (#补充---Centos 6.5环境下postgresql的安装)

****
准备工作
----
    部署项目之前请确保你已经有以下文件：

    *   apache-tomcat-8.5.6.tar.gz
    *   pplns.war
    *   jdk-8u60-linux-x64.tar.gz
    *   postgresql-9.4.1211.jar
    *   pplns_init.sql
    *   postgresql-9.4.9-2-linux-x64.run
    下载链接：
    链接：http://pan.baidu.com/s/1c284jL2 密码：09wj
 
****
java环境配置
--------------
    将jdk-8u60-linux-x64.tar.gz解压到/opt目录下。

    编辑/etc/environment文件，加入：

    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/jdk1.8.0_60/bin"

    CLASS_PATH="/opt/jdk1.8.0_60/lib"

    JAVA_HOME="/opt/jdk1.8.0_60"

****
postgresql数据库安装与初始化
----------------------------------
postgresql安装

    apt-get install postgresql-9.4

进入psql命令行

    sudo su
    su - postgres
    psql

修改superuser用户postgres的密码

    alter user postgres  with password 'pplns';
   
创建新role命名为pplns,并赋予其登陆权限

    create role pplns password 'pplns' LOGIN;

修改用户pplns的密码

    alter user pplns with password 'pplns';

退出到root用户操作界面

    \q
    exit

创建数据库pplns并指定其owner为用户pplns

    sudo -u postgres createdb -O pplns  pplns;
   
再次进入psql命令行，并切换数据库

    sudo su
    su - postgres
    psql
    \c
    \c pplns

导入pplns_init.sql以进行系统数据初始化

    \i /home/pplns/pplns_init.sql

****
Tomcat配置
------
解压apache-tomcat-8.5.6.tar.gz至/opt目录下，并命名为tomcat8

    tar -zxvf apache-tomcat-8.5.6.tar.gz -C /opt/
    mv apache-tomcat-8.5.6 tomcat8
   
删除/opt/tomcat8/webapps目录下所有文件
   
    rm -rf /opt/tomcat8/webapps/*
   
注释或删除 tomcat-users.xml 所有用户权限，看上去如下

    # cat conf/tomcat-users.xml
    <?xml version='1.0' encoding='utf-8'?>
    <tomcat-users>
    </tomcat-users>

启动项目

    mv pplns.war /opt/tomcat8/webapps/
    cd /opt/tomcat8/webapps/
    ../bin/startup.sh
   
添加postgres jdbc连接包

    mv postgresql-9.4.1211.jar /opt/tomcat8/webapps/pplns/WEB-INF/lib/

重启

    ../bin/shutdown.sh
    ../bin/startup.sh

****
常用操作
------
启动tomcat

    /opt/tomcat8/bin/startup.sh

关闭tomcat

    /opt/tomcat8/bin/shutdown.sh

查询计算机IP

    ifconfig
   
访问系统web页面

    本机IP:8080/pplns

****
异常处理
------
如果apt-get install postgresql-9.4失败，则尝试以下解决方案

方案一

    新建pgdg.list文件
   
    touch /etc/apt/sources.list.d/pgdg.list
   
    根据ubuntu不同版本，在pgdg.list中加入不同的源信息
   
    Xenial Ubuntu 16.04:
    deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main

    Trusty Ubuntu 14.04:
    deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main

    Precise Ubuntu 12.04:
    deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main
   
    导入资源signing key，并更新package lists
   
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    sudo apt-get update

执行完毕方案一，再尝试apt-get install postgresql-9.4

方案二

    下载postgresql支持所有版本的安装器postgresql-9.4.9-2-linux-x64.run
   
    http://www.enterprisedb.com/products-services-training/pgdownload#linux
   
    安装过程请参考windows版本安装

****
补充---Centos 6.5环境下postgresql的安装
------
在线下载

    yum install postgresql-server
    
初始化

    service postgresql initdb
    
开启不同级别下的服务

    chkconfig postgresql on

其他

    其他步骤请参考以上关于Ubuntu环境下的操作
