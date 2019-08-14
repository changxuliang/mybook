# 第3节：Zabbix监控mysql配置

#  1  zabbix-server端配置

给mysql的主机增加一个模板Template DB MySQL。

![1565143083410](http://localhost:4000/Chapter5/img/1565143083410.png)

#  2  mysql监控账号创建

如果数据库不属于运维管理，则需要联系数据库管理员帮忙创建zabbix账号。

mysql需要创建一个用户，用来给zabbix-agent登录mysql和获取数据（注意这里只能本地localhost访问，并且只能访问usage权限）。

创建命令如下（请注意，我们用了统一的密码，方便以后统一管理配置文件）：

```
grant usage on  *.*  to 'zabbix'@'localhost' identified by '6toEk3iA94l6j1Yo' with grant option;

flush privileges;
```

#  3  zabbix-agent配置

这里需要注意，配置不当会造成zabbix-server端，一直无法远程获取数据（在这里耽搁了好久，现在把梳理简化后的正确配置过程整理如下，请严格安装以下步骤执行）。

##  3.1  创建文件 /etc/zabbix/.my.cnf

注意：这个是新文件，与mysql原本配置文件不同，为了防止密码出现在zabbix-agent脚本中，所以单独配置，专门提供给zabbix脚本中处理mysql的部分的。

```
[client]

user=zabbix

host=localhost

password=6toEk3iA94l6j1Yo
```

本地测试，执行命令：

```
HOME=/etc/zabbix/ mysqladmin ping | grep -c alive

>1
```

看看是否能返回正确的数据。

##  3.2  修改userparameter_mysql.conf

修改/etc/zabbix/zabbix_agentd.d/userparameter_mysql.conf配置文件，

将所有：HOME=/var/lib/zabbix  替换为：HOME=/etc/zabbix

此修改表示会使用3.1中的配置文件。

##  3.3  重启zabbix-agent服务

在zabbix-server上测试：

```
zabbix_get -s 192.168.80.218 -p 10050 -k mysql.ping

>1
```

看看是否能拿到正常数据。

如果一切正确，则过一会可以在zabbix-web上看见mysql的相关graph:

![1565146468782](http://localhost:4000/Chapter5/img/1565146468782.png)

![1565146927006](http://localhost:4000/Chapter5/img/1565146927006.png)