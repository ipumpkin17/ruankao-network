# ACL 配置

## 基本 ACL 配置 2000-2999

> 配置基本 ACL，实现基于源地址和时间段的访问控制。

```shell
[Router] time-range workday 8:30 to 18:00 working-day # 创建名为 workday 的时间段，工作日 8:30-18:00
[Router] acl 2000 # 创建编号为 2000 的基本 ACL
[Router-acl-basic-2000] rule 5 permit source 192.168.1.10 0 time-range workday # 允许 192.168.1.10 在工作日访问
[Router-acl-basic-2000] quit
```

## 高级 ACL 配置 3000-3999

> 配置高级 ACL，实现基于源/目的地址的精细化访问控制。

```shell
[Router] acl 3000 # 创建编号为 3000 的高级 ACL
[Router-acl-adv-3000] rule 5 permit ip source 192.168.2.0 0.0.0.255 destination 192.168.3.100 0 # 允许 192.168.2.0/24 访问 192.168.3.100
[Router-acl-adv-3000] rule 10 deny ip source 192.168.1.0 0.0.0.255 destination 192.168.3.100 0 # 拒绝 192.168.1.0/24 访问 192.168.3.100
[Router-acl-adv-3000] rule 15 deny ip source any destination 192.168.3.100 0 # 拒绝任意源访问 192.168.3.100
[Router-acl-adv-3000] quit
```

## 应用场景

> ACL 常用于限制远程管理、禁止特定主机或网段的访问等实际网络安全场景。

### 禁止 telnet 登录

```shell
[Router] user-interface vty 0 4 # 进入 vty 0-4 虚拟终端配置
[Router-ui-vty0-4] acl 2000 inbound # 入方向应用 2000 号 ACL，限制 telnet 登录
```

### 禁止访问

```shell
[Router] interface GigabitEthernet 3/0/0
[Router-GigabitEthernet3/0/0] traffic-filter outbound acl 2000 # 出方向应用 2000 号 ACL，限制访问
```
