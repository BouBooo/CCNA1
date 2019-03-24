# TP 3 - Utilisation de matériel Cisco


 1. GNS3 :
    1. Virtualiser des routeurs

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


## <a name="3">Mise en place d'OSPF</a>



## <a name="4">IV. Lab Final</a>


