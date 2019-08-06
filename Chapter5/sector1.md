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

```
datadir=/var/lib/mysql #修改当前路径为新的
```

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

` cp /usr/share/doc/zabbix-server-mysql-3.2.11/create.sql.gz .`

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

浏览器打开：http://192.168.80.216/zabbix

![Welcome](http://localhost:4000/Chapter5/img/1564995590703.png)

![Check of pre-requires](http://localhost:4000/Chapter5/img/1564995640474.png)

配置数据库连接信息：

![Configure DB connection](http://localhost:4000/Chapter5/img/1564995925191.png)

设置zabbix-server信息:

![Zabbix server details](http://localhost:4000/Chapter5/img/1564996062716.png)

输入用户名密码登录：Admin/zabbix，成功后记得修改密码：

![login](http://localhost:4000/Chapter5/img/1564996149999.png)

##  2.5  创建zabbix_view账号给grafana使用

zabbix_view/4Qk6PM5U3vB4fYk3

#  3  zabbix-agent安装

##  3.1  版本选择

为了能兼容大多数系统，我们选择了zabbix-agent-3.2.9-1。

##  3.2  安装

`rpm –ivh zabbix-agent-3.2.9-1.*.rpm`

##  3.3  配置

修改文件：/etc/zabbix/zabbix_agentd.conf

我们一般修改以下几项即可：

```
SourceIP=192.168.80.217   # agent端的IP，也可以默认不指定

Server=192.168.80.216     # zabbix-server地址

ServerActive=192.168.80.216 # zabbix-server地址

Hostname=ms_01         # 主机名称
```

##  3.4  启动agent

```
systemctl start zabbix-agent

systemctl enable zabbix-agent
```

