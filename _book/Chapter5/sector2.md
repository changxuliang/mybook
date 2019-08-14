# 第2节：Grafana+Zabbix安装配置

#  1  安装

##  1.1  grafana获取

官网下载地址：

https://grafana.com/grafana/download 

我们下载的是CentOS最新版本（6.2.5-1）

https://dl.grafana.com/oss/release/grafana-6.2.5-1.x86_64.rpm 

当前系统环境配置:

CentOS 7.6 x86_64 

CentOS7上禁用防火墙的方式：

```
systemctl stop firewalld.service

systemctl disable firewalld.service
```

##  1.2   安装grafana

```
rpm -ivh grafana-6.2.5-1.x86_64.rpm
```

##  1.3  安装zabbix插件

安装zabbix插件：

```
grafana-cli plugins install alexanderzobnin-zabbix-app
```

 官网位置：https://grafana.com/grafana/plugins/alexanderzobnin-zabbix-app

 或者下载后解压到/var/lib/grafana/plugins

下载地址：https://grafana.com/api/plugins/alexanderzobnin-zabbix-app/versions/3.10.3/download

#  2  配置

配置文件位于：/etc/grafana/grafana.ini

详细的参数说明：https://grafana.com/docs/installation/configuration/

默认3000端口，如果要用80，需要提供给权限，或者通过端口转发：

```
a、$ sudo setcap 'cap_net_bind_service=+ep' /usr/sbin/grafana-server

b、$ sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
```

防火墙策略上添加一条允许3000端口的策略：

```
[root@localhost grafana]# firewall-cmd --add-port=3000/tcp --zone=public --permanent

Success

[root@localhost grafana]# firewall-cmd --reload

success

[root@localhost grafana]# firewall-cmd --list-ports

3000/tcp
```

我们需要配置的参数至少包含：

```
protocol = http

http_addr =

http_port = 3000

enforce_domain = false
```

#  3  运行

##  3.1  启动

启动：

```
systemctl stop grafana-server
```

停止：

```
systemctl start grafana-server
```

开机启动： 

```
chkconfig grafana-server on
```

##  3.2  登陆

浏览器进入：

http://ip:3000

 用户名密码：admin/admin

初次登录需要重置admin密码。（admin/7IyQh45Lt26oArsd）

##  3.3  启用zabbix插件

进入后会看见zabbix插件，需要enable。

![1565074890471](http://localhost:4000/Chapter5/img/1565074890471.png)

![1565074941477](http://localhost:4000/Chapter5/img/1565074941477.png)

##  3.4  为zabbix创建org和用户

###  3.4.1  创建org

![1565075389028](http://localhost:4000/Chapter5/img/1565075389028.png)

###  3.4.2  创建管理员账户

用来管理grafana模板，具有管理员权限，可以编辑

zabbix_admin@test.com/5DFshJDYN1Cu1ijg 

将zabbix_admin邀请添加到zabbix的org中，并设置为Admin角色。

![1565075580032](http://localhost:4000/Chapter5/img/1565075580032.png)

正常情况下zabbix_admin用户邮箱会收到邀请，接受后会加入当前org。

如果只是创建账户用的非可用邮箱，可以关闭Send Invite email。点击invite后会看到peding invites，找到新加的zabbix_admin用户，并点击copy invite，会拿到一个url，浏览器输入后，点击登录，即可完成邀请。

![1565075680846](http://localhost:4000/Chapter5/img/1565075680846.png)

###  3.4.3  创建普通账户

用来查看监控内容，具有只读权限，不可以编辑模板

zabbix_view@test.com/eYEd1388p5XPTAO1

 将zabbix_admin邀请添加到zabbix的org中，并设置为View角色。添加方式与3.4.2一致。

![1565075823678](http://localhost:4000/Chapter5/img/1565075823678.png)

##  3.5  添加zabbix的数据源

退出当前admin登录，以zabbix_admin@test.com账户登录，注意：登录org时候，需要以邮箱登录，不能以用户名登录。

url:zabbix-server的API地址，例如: http://113.141.64.15:10080/zabbix/api_jsonrpc.php

API:Username:zabbix管理界面登录的用户名(Admin)

API:Password:zabbix管理界面登录的用户密码(zW3*vtdppw)

![1565076429628](http://localhost:4000/Chapter5/img/1565076429628.png)

配置好后，点击save and test，如果配置正确，则会提示绿色的测试通过，然后就可以添加模板了。

![1565076531726](http://localhost:4000/Chapter5/img/1565076531726.png)

##  3.6  添加zabbix的监控模板

选择创建一个新模板，并选择Add Query。

![1565077920760](http://localhost:4000/Chapter5/img/1565077920760.png)

![1565077965614](http://localhost:4000/Chapter5/img/1565077965614.png)

我们的query类型，选择zabbix，然后下面的group，host，application，item在点击时候，会看到grafana已经帮我们从zabbix中自动列出了值，我们只要选择对应的选项即可。点击完成后，会看到已经生成了一个曲线图。

![1565078113820](http://localhost:4000/Chapter5/img/1565078113820.png)

在这里配置好各项数据后，点击保存，即可看到展示。

![1565078177746](http://localhost:4000/Chapter5/img/1565078177746.png)