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

Существующие сети: 

```
Network             Диапазоны                          Broadcast     
192.168.255.0/30    192.168.255.1 - 192.168.255.2      192.168.255.3 inetrouter-centralrouter

192.168.0.0/28      192.168.0.1 - 192.168.0.14         192.168.0.15 centralrouter-centralserver

192.168.0.32/28     192.168.0.33-192.168.0.46          192.168.0.47    

192.168.0.64/26     192.168.0.65-192.168.0.126         192.168.0.127
```

Для связки роутеров выберем доступные сети 
centralRouter- office1Router -
```
Network             Диапазон                      Broadcast
192.168.255.4/30    192.168.255.5-192.168.255.6   192.168.255.7

```
centralRouter- office2Router -
```
Network             Диапазон                      Broadcast
192.168.255.8/30    192.168.255.9-192.168.255.10   192.168.255.11

```

Добавляю в Vagrantfile два роутера office1router и office2router
сети на office1router

```
[vagrant@office1router ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.2.0/26 dev eth2 proto kernel scope link src 192.168.2.1 metric 102
192.168.2.64/26 dev eth3 proto kernel scope link src 192.168.2.65 metric 103
192.168.2.128/26 dev eth4 proto kernel scope link src 192.168.2.129 metric 104
192.168.2.192/26 dev eth5 proto kernel scope link src 192.168.2.193 metric 105
192.168.255.4/30 dev eth1 proto kernel scope link src 192.168.255.6 metric 101
```

сети на office2router

```
[vagrant@office2router ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.1.0/25 dev eth2 proto kernel scope link src 192.168.1.1 metric 102
192.168.1.128/26 dev eth3 proto kernel scope link src 192.168.1.129 metric 103
192.168.1.192/26 dev eth4 proto kernel scope link src 192.168.1.193 metric 104
192.168.255.8/30 dev eth1 proto kernel scope link src 192.168.255.10 metric 101
```

Добавляю в Vagrantfile два сервера office1Server и office2Server


