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

сеть на office1Server

```
[vagrant@office1Server ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
192.168.2.192/26 dev eth1 proto kernel scope link src 192.168.2.194 metric 101
```

сеть на office2Server

```
[vagrant@office2Server ~]$ ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 101
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 101
192.168.1.192/26 dev eth1 proto kernel scope link src 192.168.1.194 metric 100
```

```
[vagrant@office1Server ~]$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  gateway (192.168.2.193)  0.345 ms  0.310 ms  0.285 ms
 2  192.168.255.5 (192.168.255.5)  0.582 ms  0.561 ms  0.518 ms
 3  192.168.255.1 (192.168.255.1)  0.899 ms  0.841 ms  0.781 ms
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  msk-google-as15169-ix.megafon.ru (62.89.200.25)  11.431 ms  11.133 ms  11.101 ms
 9  108.170.250.66 (108.170.250.66)  11.787 ms 108.170.250.146 (108.170.250.146)  11.854 ms *
10  72.14.234.54 (72.14.234.54)  46.698 ms 142.251.237.154 (142.251.237.154)  37.407 ms 72.14.234.54 (72.14.234.54)  45.937 ms
11  172.253.65.82 (172.253.65.82)  21.606 ms 108.170.235.64 (108.170.235.64)  23.005 ms 142.251.238.66 (142.251.238.66)  22.305 ms
12  108.170.233.163 (108.170.233.163)  24.411 ms 216.239.63.129 (216.239.63.129)  24.849 ms 142.250.56.217 (142.250.56.217)  25.377 ms
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  dns.google (8.8.8.8)  22.050 ms  22.015 ms *

```

```
[vagrant@office2Server ~]$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  gateway (192.168.1.193)  0.373 ms  0.345 ms  0.324 ms
 2  192.168.255.9 (192.168.255.9)  0.542 ms  0.472 ms  0.451 ms
 3  192.168.255.1 (192.168.255.1)  0.837 ms  0.799 ms  0.758 ms
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  msk-google-as15169-ix.megafon.ru (62.89.200.25)  13.828 ms  10.549 ms  10.535 ms
 9  108.170.250.99 (108.170.250.99)  11.427 ms * *
10  142.250.239.64 (142.250.239.64)  21.782 ms 142.250.238.138 (142.250.238.138)  21.741 ms *
11  108.170.232.251 (108.170.232.251)  22.596 ms 142.251.238.68 (142.251.238.68)  21.775 ms 72.14.238.168 (72.14.238.168)  22.674 ms
12  216.239.40.61 (216.239.40.61)  25.506 ms 142.250.56.129 (142.250.56.129)  23.240 ms 172.253.51.247 (172.253.51.247)  23.409 ms
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  dns.google (8.8.8.8)  22.236 ms *  22.179 ms
```
