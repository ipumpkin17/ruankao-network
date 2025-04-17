# DHCP

## 配置

### DHCP 接口地址池配置

> 配置 DHCP 接口地址池，实现接口独立分配 IP 地址。

```shell
[Router] dhcp enable # 开启 DHCP 功能
[Router] interface GigabitEthernet 0/0/1
[Router-GigabitEthernet0/0/1] ip address 192.168.101.1 24 # 配置接口 IP
[Router-GigabitEthernet0/0/1] dhcp select interface # 选择接口地址池分配
[Router-GigabitEthernet0/0/1] dhcp server dns-list 8.8.8.8 # 指定 DNS
[Router-GigabitEthernet0/0/1] dhcp server excluded-ip-address 192.168.101.101 192.168.101.253 # 排除部分 IP
[Router-GigabitEthernet0/0/1] dhcp server lease day 10 # 设置租期
[Router-GigabitEthernet0/0/1] quit
```

### DHCP 全局地址池配置

> 配置 DHCP 全局地址池，实现集中统一分配。

```shell
[Router] dhcp enable # 开启 DHCP 功能
[Router] ip pool 102 # 创建地址池 102
[Router-ip-pool-102] network 192.168.102.0 mask 255.255.255.0 # 设置地址池网段
[Router-ip-pool-102] gateway-list 192.168.102.254 # 指定网关
[Router-ip-pool-102] dns-list 8.8.8.8 # 指定 DNS
[Router-ip-pool-102] excluded-ip-address 192.168.102.101 192.168.102.253 # 排除部分 IP
[Router-ip-pool-102] lease 10 # 设置租期
[Router-ip-pool-102] quit
[Router] interface GigabitEthernet 0/0/2
[Router-GigabitEthernet0/0/2] dhcp select global # 选择全局地址池分配
[Router-GigabitEthernet0/0/2] quit
```
