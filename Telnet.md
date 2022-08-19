# Telnet 配置

## 需求

1. 通过 telnet 远程登录。
2. 采用本地 aaa 认证，用户名为 admin123，密码为 admin123。
3. 限制 192.168.1.254 地址的 telnet 登录。

## 配置

### 创建 VLAN

```shell
<Huawei> system-view
[Huawei] vlan 100
[Huawei-vlan100] quit
[Huawei] interface GigabitEthernet 0/0/1
[Huawei-GigabitEthernet0/0/1] port lint-type access
[Huawei-GigabitEthernet0/0/1] port default vlan 100
[Huawei-GigabitEthernet0/0/1] quit
[Huawei] interface Vlanif 100
[Huawei-Vlanif100] ip address 192.168.1.254 24
[Huawei-Vlanif100] quit
```

### 开启设备 Telnet 服务

```shell
[Huawei] telnet server enable
[Huawei] user-interface vty 0 4 // 开启用户登录接口 0-4
[Huawei-ui-vty0-4] protocol inbound telnet // 通过 telnet 协议登录
[Huawei-ui-vty0-4] quit
```

#### 1. aaa 认证

```shell
[Huawei] user-interface vty 0 4
[Huawei-ui-vty0-4] authentication-mode aaa
[Huawei-ui-vty0-4] quit
[Huawei] aaa
[Huawei-aaa] local-user admin123 password admin123
[Huawei-aaa] local-user admin123 service-type telnet
[Huawei-aaa] local-user admin123 privilege level 15
[Huawei-aaa] quit
```

#### 2. password 认证

```shell
[Huawei] user-interface vty 0 4
[Huawei-ui-vty0-4] authentication-mode password
[Huawei-ui-vty0-4] Please configure the login password (maximum length 16): admin123
```

### ACL 限制用户登录

```shell
[Huawei] acl 2000
[Huawei-acl-basic-2000] rule 5 deny source 192.168.1.254 0.0.0.0 // 拒绝此主机，使用反掩码
[Huawei-acl-basic-2000] quit
[Huawei] user-interface vty 0 4
[Huawei-ui-vty0-4] acl 2000 inbound // 在 inbound 方向应用 acl 2000
```
