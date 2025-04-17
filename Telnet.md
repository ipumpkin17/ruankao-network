# Telnet 配置

## 需求

1. 通过 telnet 远程登录。
2. 采用本地 aaa 认证，用户名为 admin123，密码为 admin123。
3. 限制 192.168.1.254 地址的 telnet 登录。

## 配置

### 创建 VLAN

> 配置 Telnet 远程登录，设置 VLAN，配置 IP 地址和 ACL，限制指定主机登录。

```shell
<Huawei> system-view # 进入系统视图
[Huawei] vlan 100 # 创建 VLAN 100
[Huawei-vlan100] quit
[Huawei] interface GigabitEthernet 0/0/1 # 进入接口
[Huawei-GigabitEthernet0/0/1] port lint-type access # 设置为 access 口
[Huawei-GigabitEthernet0/0/1] port default vlan 100 # 加入 VLAN 100
[Huawei-GigabitEthernet0/0/1] quit
[Huawei] interface Vlanif 100 # 创建并进入 Vlanif 100
[Huawei-Vlanif100] ip address 192.168.1.254 24 # 配置接口 IP
[Huawei-Vlanif100] quit
```

### 开启设备 Telnet 服务

> 启用 Telnet 服务，配置 VTY 口，设置认证方式。

```shell
[Huawei] telnet server enable # 启用 Telnet 服务
[Huawei] user-interface vty 0 4 # 进入 VTY 0-4，开启用户登录接口 0-4
[Huawei-ui-vty0-4] protocol inbound telnet # 通过允许 Telnet 协议登录
[Huawei-ui-vty0-4] quit
```

#### 1. aaa 认证

> 配置 AAA 本地认证，创建本地用户。

```shell
[Huawei] user-interface vty 0 4 # 进入 VTY 0-4
[Huawei-ui-vty0-4] authentication-mode aaa # 设置认证方式为 AAA
[Huawei-ui-vty0-4] quit
[Huawei] aaa # 进入 AAA 配置
[Huawei-aaa] local-user admin123 password admin123 # 创建本地用户 admin123
[Huawei-aaa] local-user admin123 service-type telnet # 允许 telnet 登录
[Huawei-aaa] local-user admin123 privilege level 15 # 设置最高权限
[Huawei-aaa] quit
```

#### 2. password 认证

> 配置密码认证方式（可选）。

```shell
[Huawei] user-interface vty 0 4 # 进入 VTY 0-4
[Huawei-ui-vty0-4] authentication-mode password # 设置为密码认证
[Huawei-ui-vty0-4] Please configure the login password (maximum length 16): admin123 # 设置登录密码
```

### ACL 限制用户登录

> 配置 ACL，限制指定主机登录。

```shell
[Huawei] acl 2000 # 创建 ACL 2000
[Huawei-acl-basic-2000] rule 5 deny source 192.168.1.254 0.0.0.0 # 拒绝此主机登录（使用反掩码）
[Huawei-acl-basic-2000] quit
[Huawei] user-interface vty 0 4 # 进入 VTY 0-4
[Huawei-ui-vty0-4] acl 2000 inbound # 入方向应用 ACL 2000
[Huawei-ui-vty0-4] quit
```
