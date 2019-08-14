# 第1节：西安交警案例

## VR操作系统CRASH（西安交警案例）

【故障描述】

  VR操作系统crash，所有公网IP断网。

【相关信息】

| **类型**          | **值**                                           |
| ----------------- | ------------------------------------------------ |
| VPC   Name        | 0a34810a763a45ed9243b73d25e7eb2a                 |
| VPC   ID          | 56d433c7-ff24-45ec-b11f-9897795e7d2f             |
| VR                | r-19411-VM                                       |
| VR链接地址        | 10.134.16.14                                     |
| VPC   offering    | XAJJ_VPC_Offering_SourceNat_8C8G                 |
| Zabbix            | http://10.255.96.182/zabbix/charts.php?ddreset=1 |
| Zabbix   account  | yanfa                                            |
| Zabbix   password | !@#yanqwe                                        |

![xianjiaojing](http://localhost:4000/Chapter4/img/xianjiaojing.png)

【操作步骤】

  (1)  检查VR监控；

  (2)  登陆vCenter找到VR虚机，关机后clone，备以后检查；

  (3)  重启VPC网络，重建VR虚机；

  (4)  登陆VR   

```
ssh -p 3922 -o StrictHostKeyChecking=no -i  /var/cloudstack/management/.ssh/id_rsa root@10.134.16.14
```

  (5)  手动修改haproxy.cfg在global块内增加配置 

```
nbproc 4
```

​         重新载入haproxy 

```
service haproxy reload
```

  (6)   修改TC

​          查看配置公网地址IP eth1 ~ eth5

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN 

​    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

​    inet 127.0.0.1/8 scope host lo

​       valid_lft forever preferred_lft forever

​    inet6 ::1/128 scope host 

​       valid_lft forever preferred_lft forever

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 02:00:56:94:1d:a8 brd ff:ff:ff:ff:ff:ff

​    inet 10.134.16.14/21 brd 10.134.23.255 scope global eth0

​       valid_lft forever preferred_lft forever

​    inet6 fe80::56ff:fe94:1da8/64 scope link 

​       valid_lft forever preferred_lft forever

3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 00:00:00:00:0f:4b brd ff:ff:ff:ff:ff:ff

​    inet 113.141.67.112/22 brd 113.141.67.255 scope global eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.87/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.85/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.219/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.67.92/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.114/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.67.225/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.140/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.163/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.180/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.243/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.67.34/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.162/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.174/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.66.75/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.98/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.176/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.66.119/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.64/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.173/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.28/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.232/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.2/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.207/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.104/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.66.208/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.64.200/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet 113.141.65.199/22 brd 113.141.67.255 scope global secondary eth1

​       valid_lft forever preferred_lft forever

​    inet6 fe80::200:ff:fe00:f4b/64 scope link 

​       valid_lft forever preferred_lft forever

4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 06:60:1e:00:17:5a brd ff:ff:ff:ff:ff:ff

​    inet 113.141.72.35/24 brd 113.141.72.255 scope global eth2

​       valid_lft forever preferred_lft forever

​    inet 113.141.72.75/24 brd 113.141.72.255 scope global secondary eth2

​       valid_lft forever preferred_lft forever

​    inet 113.141.72.224/24 brd 113.141.72.255 scope global secondary eth2

​       valid_lft forever preferred_lft forever

​    inet 113.141.72.248/24 brd 113.141.72.255 scope global secondary eth2

​       valid_lft forever preferred_lft forever

​    inet6 fe80::460:1eff:fe00:175a/64 scope link 

​       valid_lft forever preferred_lft forever

5: eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 06:94:8c:00:26:40 brd ff:ff:ff:ff:ff:ff

​    inet 203.3.81.205/21 brd 203.3.87.255 scope global eth3

​       valid_lft forever preferred_lft forever

​    inet 203.3.87.25/21 brd 203.3.87.255 scope global secondary eth3

​       valid_lft forever preferred_lft forever

​    inet6 fe80::494:8cff:fe00:2640/64 scope link 

​       valid_lft forever preferred_lft forever

6: eth4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 06:97:44:00:15:b7 brd ff:ff:ff:ff:ff:ff

​    inet 113.141.70.177/24 brd 113.141.70.255 scope global eth4

​       valid_lft forever preferred_lft forever

​    inet 113.141.70.116/24 brd 113.141.70.255 scope global secondary eth4

​       valid_lft forever preferred_lft forever

​    inet6 fe80::497:44ff:fe00:15b7/64 scope link 

​       valid_lft forever preferred_lft forever

7: eth5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 06:43:f0:00:19:9d brd ff:ff:ff:ff:ff:ff

​    inet 113.141.73.223/24 brd 113.141.73.255 scope global eth5

​       valid_lft forever preferred_lft forever

​    inet 113.141.73.109/24 brd 113.141.73.255 scope global secondary eth5

​       valid_lft forever preferred_lft forever

​    inet6 fe80::443:f0ff:fe00:199d/64 scope link 

​       valid_lft forever preferred_lft forever

8: eth6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000

​    link/ether 02:00:37:57:00:5e brd ff:ff:ff:ff:ff:ff

​    inet 172.16.0.1/24 brd 172.16.0.255 scope global eth6

​       valid_lft forever preferred_lft forever

​    inet6 fe80::37ff:fe57:5e/64 scope link 

​       valid_lft forever preferred_lft forever
```

```
tc qdisc del dev eth1 root

tc qdisc del dev eth2 root

tc qdisc del dev eth3 root

tc qdisc del dev eth4 root

tc qdisc del dev eth5 root
```

  (7)   查看TC

```
tc -s -d qdisc show dev eth1

tc -s -d qdisc show dev eth2

tc -s -d qdisc show dev eth3

tc -s -d qdisc show dev eth4

tc -s -d qdisc show dev eth5
```

  (8)  对比配置

```
haproxy.cfg

ip addr

ip rule

iptables
```

