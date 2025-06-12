---
layout: post
title:  "some tips of logrotate"
date:   2025-06-11 22:00:00 +0800
categories: jekyll update
---

## Get interface index

```bash

Get-NetIPInterface | Sort-Object InterfaceIndex

ifIndex InterfaceAlias                  AddressFamily NlMtu(Bytes) InterfaceMetric Dhcp     ConnectionState PolicyStore
------- --------------                  ------------- ------------ --------------- ----     --------------- -----------
1       Loopback Pseudo-Interface 1     IPv6            4294967295              75 Disabled Connected       ActiveStore
1       Loopback Pseudo-Interface 1     IPv4            4294967295              75 Disabled Connected       ActiveStore
6       蓝牙网络连接                    IPv6                  1500              65 Disabled Disconnected    ActiveStore
6       蓝牙网络连接                    IPv4                  1500              65 Enabled  Disconnected    ActiveStore
13      以太网                          IPv6                  1280              25 Enabled  Connected       ActiveStore
13      以太网                          IPv4                  1280              25 Enabled  Connected       ActiveStore
15      LetsTAP                         IPv4                  1500               1 Disabled Connected       ActiveStore
16      WLAN                            IPv4                  1500              30 Enabled  Connected       ActiveStore
17      本地连接* 2                     IPv6                  1500              25 Disabled Disconnected    ActiveStore
17      本地连接* 2                     IPv4                  1500              25 Disabled Disconnected    ActiveStore
24      本地连接* 1                     IPv6                  1500              25 Disabled Disconnected    ActiveStore
24      本地连接* 1                     IPv4                  1500              25 Enabled  Disconnected    ActiveStore
54      vEthernet (Default Switch)      IPv6                  1500            5000 Enabled  Connected       ActiveStore
54      vEthernet (Default Switch)      IPv4                  1500            5000 Disabled Connected       ActiveStore
```

## Get Ip configuration

```bash
 
Get-NetIPConfiguration -InterfaceIndex 13

InterfaceAlias       : 以太网
InterfaceIndex       : 13
InterfaceDescription : SecTap Adapter
NetProfile.Name      : 未识别的网络
IPv4Address          : 192.163.77.191
IPv6DefaultGateway   :
IPv4DefaultGateway   :
DNSServer            : 26.26.26.53


```

## Add route

```bash

route -p add 172.17.249.150 mask 255.255.255.255 172.17.249.150 if 13

```

## add dnsserver

```bash
Set-DnsClientServerAddress -InterfaceAlias "SecTap Adapter" -ServerAddresses ("10.54.12.130")
Set-DnsClientServerAddress -InterfaceIndex 14 -ServerAddresses ("10.54.12.130")

PS C:\Users\Administrator> get-netipconfiguration

InterfaceAlias       : 以太网
InterfaceIndex       : 14
InterfaceDescription : SecTap Adapter
NetProfile.Name      : 未识别的网络
IPv4Address          : 192.163.90.243
IPv6DefaultGateway   :
IPv4DefaultGateway   :
DNSServer            : 10.54.12.130


InterfaceAlias       : WLAN
InterfaceIndex       : 17
InterfaceDescription : Intel(R) Wi-Fi 6 AX200 160MHz
NetProfile.Name      : TP-LINK_5B4B
IPv4Address          : 192.168.0.101
IPv4DefaultGateway   : 192.168.0.1
DNSServer            : 26.26.26.53

InterfaceAlias       : 蓝牙网络连接
InterfaceIndex       : 6
InterfaceDescription : Bluetooth Device (Personal Area Network)
NetAdapter.Status    : Disconnected

```