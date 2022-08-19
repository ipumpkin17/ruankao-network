# ACL 配置

## 基本 ACL 配置 2000-2999

```shell
[Router] time-range workday 8:30 to 18:00 working-day
[Router] acl 2000
[Router-acl-basic-2000] rule 5 permit source 192.168.1.10 0 time-range workday
[Router-acl-basic-2000] quit
```

## 高级 ACL 配置 3000-3999

```shell
[Router] acl 3000
[Router-acl-adv-3000] rule 5 permit ip source 192.168.2.0 0.0.0.255 destination 192.168.3.100 0
[Router-acl-adv-3000] rule 10 deny ip source 192.168.1.0 0.0.0.255 destination 192.168.3.100 0
[Router-acl-adv-3000] rule 15 deny ip source any destination 192.168.3.100 0
[Router-acl-adv-3000] quit
```

## 应用场景

### 禁止 telnet 登录

```shell
[Router] user-interface vty 0 4
[Router-ui-vty0-4] acl 2000 inbound
```

### 禁止访问

```shell
[Router] interface GigabitEthernet 3/0/0
[Router-GigabitEthernet3/0/0] traffic-filter outbound acl 2000
```
