# CCNA1

## TP 1 - Remise dans le bain

##### /30 ne marchait pas sur vm ware et donc pour ce TP ce sera /29

Combien y a-t-il d'adresses disponibles dans un /24 ?
254
Combien y a-t-il d'adresses disponibles dans un /30 ?
2

Il permet d’avoir 4 adresses dont deux utilisables, soit-on peu faire un réseau de deux machines


### Table ARP
ip neigh show :
On affiche les voisins à notre ip

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_1.PNG?raw=true "Title")



Vider la table ARP :
sudo ip neigh flush all
ip neigh show

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_2.PNG?raw=true "Title")


On re ping l’hôte:

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_3.PNG?raw=true "Title")

On peut voir que le 192.168.180.2 est réapparut qui est l’ip de mon adaptater vmWare.


### Capture réseau

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_4.PNG?raw=true "Title")


## II. Communication simple entre deux machines

Paquet ping2 :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_5.PNG?raw=true "test ping")

Netcat:

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_6.PNG?raw=true "Netcat")

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_7.PNG?raw=true "ss")


On apllique le filtre udp sur wireshark :

![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_8.PNG?raw=true "udp filter")


On peut même récupérer les messages transmis et on voit des paquet UDP en plus.


### Tcp :
3-way handshake TCP :


![Alt text](https://github.com/BouBooo/CCNA1/blob/master/img/ccna_9.PNG?raw=true "handshake")

