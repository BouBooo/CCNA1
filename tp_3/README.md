# TP 3 - Utilisation de matériel Cisco


 1. GNS3 :
    - Virtualiser des routeurs

2. Routeurs
    - Gestion du routage entre les différents réseaux
    - Permettre un accès à internet
    - Les "routeurs" seront des Cisco 3640
3. Switches
    - Gestion des VLANs
    - Permet aux clients d'accéder au réseau
    - Les "switches" seront des iOU Cisco


## Sommaire

 ### [I.Manipulation de switches et de VLAN](#1)</a>

 ### [II. Manipulation simple de routeurs](#2)</a>

 ### [III. Mise en place d'OSPF](#3)</a>

 ### [IV. Lab Final](#4)</a>



## <a name="1">Manipulation de switches et de VLAN</a>

```bash
client1           SW1                  SW 2
+----+         +-------+            +-------+
|    +---------+       +------------+       |
+----+         +---+---+            +---+---+
                   |                    |
                   |                    |
                   |                    |
                   |                    |
                +--+-+               +--+-+
                |    |               |    |
                +----+               +----+
               client2               client3

```

###### client1.lab1.tp3 peut joindre client3.lab1.tp3 :

```bash
[root@client1 ~]# ping -c 4 10.1.1.5
PING 10.1.1.5 (10.1.1.5) 56(84) bytes of data.
64 bytes from 10.1.1.5: icmp_seq=1 ttl=64 time=0.281 ms
64 bytes from 10.1.1.5: icmp_seq=2 ttl=64 time=0.549 ms
64 bytes from 10.1.1.5: icmp_seq=3 ttl=64 time=0.554 ms
64 bytes from 10.1.1.5: icmp_seq=4 ttl=64 time=0.483 ms

--- 10.1.1.5 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 0.281/0.466/0.554/0.113 ms
[root@client1 ~]#

```

##### Création du VLAN10
```bash
SW1#conf t
SW1(config)#vlan 10
SW1(config-vlan)#name VLAN10
SW1(config-vlan)#exit
SW1(config)#interface Ethernet 0/0
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 10
```

##### Création du VLAN20
```bash
SW1(config)#vlan 20
SW1(config-vlan)#name VLAN20
SW1(config-vlan)#exit
SW1(config)#interface Ethernet 0/1
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 20
```

##### Création du trunk:
```bash
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport trunk encapsulation dot1q
IOU1(config-if)#switchport mode trunk
```

##### On s’occupe du VLAN 10 de SW2 :
```bash
IOU1#conf t
IOU1(config)#vlan 10
IOU1(config-vlan)#name VLAN10
IOU1(config)#interface Ethernet 0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
```

##### Création du trunk:
```bash
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport trunk encapsulation dot1q
IOU1(config-if)#switchport mode trunk
```

##### Tests ping:
```bash
[root@client1 ~]$ ping 10.1.1.3
PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=2.50 ms
64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=1.45 ms
64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=1.45 ms
^C
--- 10.1.1.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 0.380/0.407/0.437 ms

[root@client1 ~]$ ping 10.1.1.2
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
From 10.1.1.1 icmp_seq=1 Destination Host Unreachable
From 10.1.1.1 icmp_seq=2 Destination Host Unreachable
From 10.1.1.1 icmp_seq=3 Destination Host Unreachable
From 10.1.1.1 icmp_seq=4 Destination Host Unreachable
^C
--- 10.1.1.2 ping statistics ---
5 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3012ms
pipe 4
```

> Comme prévu le client2 est injoignable.



## <a name="2"> Manipulation simple de routeurs</a>

```bash
                           10.2.12.0/30

                  router1                router2
client1          +------+               +------+
+----+.10        |      |.1           .2|      |.254     .10+----+
|    +-----------+      +---------------+      +------------+    |
+----+           +------+               +------+            +----+
                     |.254                                  server1
                     |
                     |
                     |
   10.2.1.0/24       |                         10.2.2.0/24
                     |.11
                  +----+
                  |    |
                  +----+
```


### Les clients et serveurs peuvent joindre leurs gateways respectives :

#### Ping client2 vers gateway:

```bash
[root@client2 ~]$ ping 10.2.1.254
PING 10.2.1.254 (10.2.1.254) 56(84) bytes of data.
64 bytes from 10.2.1.254: icmp_seq=1 ttl=255 time=1.75 ms
64 bytes from 10.2.1.254: icmp_seq=2 ttl=255 time=1.80 ms
64 bytes from 10.2.1.254: icmp_seq=3 ttl=255 time=1.75 ms
64 bytes from 10.2.1.254: icmp_seq=4 ttl=255 time=1.65 ms

^C
--- 10.2.1.254 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 4020ms
rtt min/avg/max/mdev = 1.405/1.407/2.380/1.302 ms

Ping client2 vers gateway:

[root@server1 ~]$ ping 10.2.2.254
PING 10.2.2.254 (10.2.2.254) 56(84) bytes of data.
64 bytes from 10.2.2.254: icmp_seq=1 ttl=255 time=2.60 ms
64 bytes from 10.2.2.254: icmp_seq=2 ttl=255 time=1.95 ms
64 bytes from 10.2.2.254: icmp_seq=3 ttl=255 time=1.85 ms
^C
--- 10.2.2.254 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 3012ms
rtt min/avg/max/mdev = 1.405/1.407/2.380/1.302 ms
```

#### On add les route sur le client 2 les 2 routeurs et le serveur 1.
#### On test le ping :

```bash
On add les route sur le client 2 les 2 routeurs et le serveur 1.
On test le ping :
[root@server1 ~]$ ping 10.3.102.254
PING 10.3.102.254 (10.3.102.254) 56(84) bytes of data.
64 bytes from 10.3.102.254: icmp_seq=1 ttl=62 time=2.40 ms
64 bytes from 10.3.102.254: icmp_seq=2 ttl=62 time=1.80 ms
^C
--- 10.3.102.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 2830ms
rtt min/avg/max/mdev = 11.296/20.185 ms
```








## <a name="3">Mise en place d'OSPF</a>

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/lab3.png?raw=true "")


#### Ping de Client1 vers la gateway :

```bash
[root@client1 ~]$ ping 10.3.101.254
PING 10.3.101.254 (10.3.101.254) 56(84) bytes of data.
64 bytes from 10.3.101.254: icmp_seq=1 ttl=255 time=54.2 ms
64 bytes from 10.3.101.254: icmp_seq=2 ttl=255 time=22.4 ms
^C
--- 10.3.101.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 19.451/45.354/80.341 ms
```

#### Ping de serveur1 vers la gateway :
```bash
[root@serveur1 ~]$ ping 10.3.102.254
PING 10.3.102.254 (10.3.102.254) 56(84) bytes of data.
64 bytes from 10.3.102.254: icmp_seq=1 ttl=255 time=20.5 ms
64 bytes from 10.3.102.254: icmp_seq=2 ttl=255 time=18.3 ms
^C
--- 10.3.102.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 18.021/18.032/35.872 ms
```



## <a name="4">IV. Lab Final</a>

### Config de L’OSPF

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/conf.PNG?raw=true "")

#### On met en place le triangle d’or

##### Config R1:

```bash
config t
int loopback 0
ip address 1.1.1.1 255.255.255.255
no shut
ip ospf 1 area 0
description interface loopback
exit
end
```
```bash
config t
int f0/0
ip address 10.0.0.1 255.255.255.252
no shut
ip ospf 1 area 0
description R1 to R2
end
```

```bash
config t
int f1/0
ip address 10.0.0.5 255.255.255.252
no shut
ip ospf 1 area 0
description R1 to R3
end
```

##### Config R2:

```bash
config t
int f0/0
ip address 10.0.0.2 255.255.255.252
no shut
ip ospf 1 area 0
description R2 to R1
end
```

```bash

config t
int f1/0
ip address 10.0.0.9 255.255.255.252
no shut
ip ospf 1 area 0
end
```

```bash
config t
int loopback 0
ip address 2.2.2.2 255.255.255.255
no shut
ip ospf 1 area 0
router ospf 1
router-id 2.2.2.2
end
clear ip ospf process
```

##### Config R3:

```bash
config t
int loopback 0
ip address 3.3.3.3 255.255.255.255
no shut 
ip ospf 1 area 0
end
```

```bash
config t
int f1/0
ip address 10.0.0.10 255.255.255.252
no shut
ip ospf 1 area 0
description R3 to R2
end
```

```bash
config t
int f0/0
ip address 10.0.0.6 255.255.255.252
no shut
ip ospf 1 area 0
description R3 to R1
```


##### Config R3:

```bash
config t
int loopback 0
ip address 3.3.3.3 255.255.255.255
no shut 
ip ospf 1 area 0
end
```

```bash
config t
int f1/0
ip address 10.0.0.10 255.255.255.252
no shut
ip ospf 1 area 0
description R3 to R2
end
```

```bash

config t
int f0/0
ip address 10.0.0.6 255.255.255.252
no shut
ip ospf 1 area 0
description R3 to R1
```

#### On ajoute le dhcp sur r1

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/domain.PNG?raw=true "")

#### On tente d’ajouter le domaine crée en windows server 

#### On teste le ping vers google depuis r1

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/cmd_1.PNG?raw=true "")

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/cmd_2.PNG?raw=true "")

#### On configure le dhcp sur r2 :

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/cmd_3.PNG?raw=true "")

##### Config du switch 1 :

```bash
config t
int e0/0
switchport trunk encapsluation dot1q
switchport access vlan 10
```


#### L'ip a bien était attribué par le dhcp

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/cmd_3.PNG?raw=true "")

![Alt text](https://github.com/BouBooo/CCNA_B2/blob/master/tp_3/img/C&D.PNG?raw=true "")

