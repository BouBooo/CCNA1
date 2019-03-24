# TP 2 - Routage statique et services d'infra

Ping de server 1 a client 1 :

```bash

[toor@server1 network-scripts]$ ping -c 4  10.2.1.254
PING 10.2.1.254 (10.2.1.254) 56(84) bytes of data.
64 bytes from 10.2.1.254: icmp_seq=1 ttl=63 time=0.947 ms
64 bytes from 10.2.1.254: icmp_seq=2 ttl=63 time=0.803 ms
64 bytes from 10.2.1.254: icmp_seq=3 ttl=63 time=1.05 ms
64 bytes from 10.2.1.254: icmp_seq=4 ttl=63 time=1.01 ms

--- 10.2.1.254 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 0.803/0.954/1.057/0.103 ms

```



Ping de Client1 à server1 :

```bash
[toor@client1 network-scripts]$ ping -c 4  10.2.2.10
PING 10.2.2.10 (10.2.2.10) 56(84) bytes of data.
64 bytes from 10.2.2.10: icmp_seq=1 ttl=62 time=1.08 ms
64 bytes from 10.2.2.10: icmp_seq=2 ttl=62 time=1.49 ms
64 bytes from 10.2.2.10: icmp_seq=3 ttl=62 time=1.39 ms
64 bytes from 10.2.2.10: icmp_seq=4 ttl=62 time=1.35 ms

--- 10.2.2.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 1.088/1.333/1.495/0.152 ms
```

Client1 qui a bien internet :
```bash
sudo ip route add default via 10.2.1.254 dev ens37
```

Pour client 1 : default via 10.2.1.254 dev ens37
```bash
sudo sysctl -w net.ipv4.conf.all.forwarding=1
```


![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_1.PNG?raw=true "")

##### Serveur DHCP :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_2.PNG?raw=true "")



##### Server ntp :
###### Chronyc sources

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_3.PNG?raw=true "")


##### Chronyc tracking :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_4.PNG?raw=true "")

```bash
systemctl status chronyd
```

> ![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_5.PNG?raw=true "")



#### Sur client 1:
```bash
[toor@client1 ~]$ chronyc sources
240 Number of sources = 1
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^? router1                       2   6     1     2    +73us[  +73us] +/-   23ms
```

##### Serveur Nginx status :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_6.PNG?raw=true "")


##### ```bash sudo ss -altnp4 ```


![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_7.PNG?raw=true "")



##### On curl l ip du server :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/tp_2/img/ccna2_8.PNG?raw=true "")


