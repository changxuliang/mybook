# 第1节：Zabbix-server安装配置

#  1  安装

##   1.1  zabbix-server获取

因为server是单独的服务器，操作系统可选，故server选择了较新版本4.2.5，centos7 x86_64的版本。

下载地址：

http://repo.zabbix.com/zabbix/4.2/rhel/7/x86_64/ 

我们需要的安装包包括：

zabbix-server-mysql-4.2.5-1.el7.x86_64.rpm

zabbix-web-4.2.5-1.el7.noarch.rpm

zabbix-web-mysql-4.2.5-1.el7.noarch.rpm 

安装过程中遇到的依赖库：

Fping

OpenIPMI-libs

libiodbc

##  1.2  安装zabbix-server

rpm -ivh zabbix-server-mysql-4.2.5-1.el7.x86_64.rpm

rpm –ivh zabbix-web-4.2.5-1.el7.noarch.rpm

rpm –ivh zabbix-web-mysql-4.2.5-1.el7.noarch.rpm

(web和web-mysql相互依赖，需要同时安装)

##  1.3  安装mysql

Centos7下默认提供mariadb，与mysql一致，我们使用mariadb。

yum install mariadb-server mariadb mariadb-libs

#  2  修改配置

##  2.1  配置mysql

因为存储监控数据，我们给mysql的数据库存储位置做修改，指向一个单独的大空间。

修改/etc/my.cnf：

datadir=/var/lib/mysql #修改当前路径为新的 

启动数据库

```
systemctl start mariadb
systemctl enable mariadb
```

 创建zabbix的数据库：

```
mysql
```

`create database zabbix character set utf8 collate utf8_bin`;

`grant all privileges on zabbix.* to zabbix@localhost identified by '6toEk3iA94l6j1Yo';`

`grant all privileges on zabbix.* to zabbix@'%' identified by "6toEk3iA94l6j1Yo";`

`flush privileges;`

`quit;`

 解压sql脚本并导入：

`cp /usr/share/doc/zabbix-server-mysql-3.2.11/create.sql.gz .`

```
gunzip create.sql.gz
```

`mysql -uzabbix -pPassword zabbix <create.sql`

##  2.2  配置zabbix-server

在/etc/zabbix/zabbix_server.conf修改

`DBPassword=Password`

启动zabbix-server和httpd服务

```
systemctl start zabbix-server
systemctl enable zabbix-server
systemctl start httpd
```

注意如果启动zabbix-server失败，错误如下：

cannot start alert manager service: Cannot bind socket to "/var/run/zabbix/zabbix_server_alerter.sock": [13] Permission denied.

错误原因为selinux的权限限制问题，禁用selinux并重启系统即可。

##  2.3  修改web配置

修改时区

```
vim /etc/php.ini
date.timezone = Asia/Shanghai
 
vim  /etc/httpd/conf.d/zabbix.conf
php_value date.timezone Asia/Shanghai 
```

`service httpd restart`

##  2.4  zabbix-web界面配置

