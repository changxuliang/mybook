# 第2节：四维图新案例

#  客户虚拟机无法访问VPC网络分配的公网IP

【故障描述】

  客户虚拟机无法访问VPC网络分配的公网IP，可以访问114.114.114.114。

【故障处理】

  (1)   出公网的接口路由表里面少了一条路由：          

```
throw 192.168.0.0/16  table Table_eth2
```

  (2)   在VR上操作如下：

```
ip route add throw 192.168.0.0/16 table Table_eth1

ip route add throw 192.168.0.0/16 table Table_eth2
```

  (3)   查看路由：

```
ip route list table Table_eth2
```

正常情况配置如下：

```
default via 203.34.4.254 dev eth2  proto static

throw 192.168.0.0/16

203.34.4.0/24 dev eth2  proto static  scope link
```

