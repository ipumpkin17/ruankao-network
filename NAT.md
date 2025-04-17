# NAT 配置

## 总结

1. 静态 NAT：将内网主机的私有地址一对一映射到公有地址。
2. 动态 NAT：将内网主机的私有地址转换为公网地址池里面的地址。
3. NAPT：也叫端口 NAT 或 PAT，从地址池中选择地址进行地址转换时不仅转换 IP 地址，同时也会对端口号进行转换。
4. Easy IP：特殊的 NAPT，Easy IP 没有地址池的概念，使用设备接口地址作为 NAT 转换的公有地址。
5. NAT Server：将内部服务器映射到公网，保障服务器安全。

## 配置

### 一、静态 NAT

```shell
[Router] interface GigabitEthernet 0/0/1
[Router-GigabitEthernet0/0/1] ip address 12.1.1.1 24
[Router-GigabitEthernet0/0/1] nat static enable // 开启静态 NAT 功能
[Router-GigabitEthernet0/0/1] nat static global 12.1.1.2 inside 192.168.1.2 // 一对一映射
[Router-GigabitEthernet0/0/1] nat static global 12.1.1.3 inside 192.168.1.3
[Router-GigabitEthernet0/0/1] quit
```

### 二、动态 NAT

```shell
[Router] nat address-group 1 2.2.2.100 2.2.2.200 // group 1
[Router] acl 2000
[Router-acl-basic-2000] rule 5 permit source 192.168.20.0 0.0.0.255
[Router-acl-basic-2000] quit
[Router] interface GigabitEthernet 0/0/2
[Router-GigabitEthernet0/0/2] nat outbound 2000 address-group 1 not-pat // 有 not-pat
[Router-GigabitEthernet0/0/2] quit
```

### 三、NAPT

```shell
[Router] nat address-group 2 3.3.3.80 3.3.3.83 // group 2
[Router] acl 2001
[Router-acl-basic-2001] rule 5 permit source 10.0.0.0 0.255.255.255
[Router-acl-basic-2001] quit
[Router] interface GigabitEthernet 0/0/3
[Router-GigabitEthernet0/0/3] nat outbound 2001 address-group 2 // 没有 not-pat
[Router-GigabitEthernet0/0/3] quit
```

### 四、Easy IP

```shell
[Router] acl 2002
[Router-acl-basic-2002] rule 5 permit source 192.168.1.1、0 0.255.255.255
[Router-acl-basic-2002] quit
[Router] interface GigabitEthernet 0/0/4
[Router-GigabitEthernet0/0/4] nat outbound 2002 // 没有地址池
[Router-GigabitEthernet0/0/4] quit
```

### 五、NAT Server

```shell
[Router] interface GigabitEthernet 0/0/5
[Router-GigabitEthernet0/0/5] ip address 12.1.1.1 24
[Router-GigabitEthernet0/0/5] nat server protocol tcp global 12.1.1.1 80 inside 192.168.1.10 8080
```
