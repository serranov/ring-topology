**Before testing this scenario, take a look at the other one called *Stretched L2 using EVPN-VXLAN*. All the files that do not appear within the current directory are inherited from that scenario. Before applying any file from the current scenario, delete any file from the previous one that shares the same folder/app**

This scenario relies on the default `.yaml` file plus some small changes to create a different subnet per host that will be interconnected.

The following commands have been executed after deploying the CLAB topology using the original file, but the same comands can be added at the end of each `exec` part per server (the ones to be included are the ones that start with `sudo`. Within the `.yml` file, type them without `sudo`).

## Changes on host1

```bash
[*]─[host1]─[~]
└──> ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0@if1123: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether ae:34:dc:8c:e5:e5 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.120.120.101/24 brd 172.120.120.255 scope global eth0
       valid_lft forever preferred_lft forever
3: bond0: <BROADCAST,MULTICAST,MASTER,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether aa:c1:ab:57:01:9d brd ff:ff:ff:ff:ff:ff
    inet 192.168.3.10/24 scope global bond0
       valid_lft forever preferred_lft forever
    inet6 fe80::a8c1:abff:fe57:19d/64 scope link 
       valid_lft forever preferred_lft forever
1129: eth1@if1128: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc noqueue master bond0 state UP group default 
    link/ether aa:c1:ab:57:01:9d brd ff:ff:ff:ff:ff:ff link-netnsid 1
1131: eth2@if1130: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc noqueue master bond0 state UP group default 
    link/ether aa:c1:ab:57:01:9d brd ff:ff:ff:ff:ff:ff link-netnsid 2

[*]─[host1]─[~]
└──> sudo ip r add 192.168.4.0/24 via 192.168.3.254 dev bond0

[*]─[host1]─[~]
└──> ip r
default via 172.120.120.1 dev eth0 
172.120.120.0/24 dev eth0 proto kernel scope link src 172.120.120.101 
192.168.3.0/24 dev bond0 proto kernel scope link src 192.168.3.10 
192.168.4.0/24 via 192.168.3.254 dev bond0 
```

## Changes on host2


```bash
[*]─[host2]─[~]
└──> sudo ip addr del 192.168.3.20/24 dev bond0

[*]─[host2]─[~]
└──> sudo ip addr add 192.168.4.20/24 dev bond0

[*]─[host2]─[~]
└──> ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0@if1108: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 2a:02:f0:8f:a0:8e brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.120.120.102/24 brd 172.120.120.255 scope global eth0
       valid_lft forever preferred_lft forever
3: bond0: <BROADCAST,MULTICAST,MASTER,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether aa:c1:ab:d9:80:6a brd ff:ff:ff:ff:ff:ff
    inet 192.168.4.20/24 scope global bond0
       valid_lft forever preferred_lft forever
    inet6 fe80::a8c1:abff:fed9:806a/64 scope link 
       valid_lft forever preferred_lft forever
1116: eth1@if1117: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc noqueue master bond0 state UP group default 
    link/ether aa:c1:ab:d9:80:6a brd ff:ff:ff:ff:ff:ff link-netnsid 1
1121: eth2@if1122: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc noqueue master bond0 state UP group default 
    link/ether aa:c1:ab:d9:80:6a brd ff:ff:ff:ff:ff:ff link-netnsid 2

[*]─[host2]─[~]
└──> sudo ip r add 192.168.3.0/24 via 192.168.4.254 dev bond0

[*]─[host2]─[~]
└──> ip r
default via 172.120.120.1 dev eth0 
172.120.120.0/24 dev eth0 proto kernel scope link src 172.120.120.102 
192.168.3.0/24 via 192.168.4.254 dev bond0 
192.168.4.0/24 dev bond0 proto kernel scope link src 192.168.4.20 
```
