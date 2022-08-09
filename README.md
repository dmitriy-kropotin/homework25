init
сети на inetRouter
```
[vagrant@inetRouter ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.255.0/30 dev eth1 proto kernel scope link src 192.168.255.1 metric 101
```
Сеть - 192.168.255.1/30

Сети на centralRouter
```
[vagrant@centralRouter ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.0.0/28 dev eth2 proto kernel scope link src 192.168.0.1 metric 102
192.168.0.32/28 dev eth3 proto kernel scope link src 192.168.0.33 metric 103
192.168.0.64/26 dev eth4 proto kernel scope link src 192.168.0.65 metric 104
192.168.255.0/30 dev eth1 proto kernel scope link src 192.168.255.2 metric 101
```

Сети на centralServer

```
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.0.0/28 dev eth1 proto kernel scope link src 192.168.0.2 metric 101
```

Занятые диапазоны               Network         Broadcast     
192.168.255.1 - 192.168.255.2   192.168.255.0   192.168.255.3

192.168.0.1 - 192.168.0.14      192.168.0.0     192.168.0.15
