## I. QU'EST CE QUE FHRP ?

FHRP, ça veut dire `First Hop Redundancy Protocol`. L'idée, c'est de faire en sorte que les hôtes sur ton réseau aient toujours une passerelle par défaut disponible même si un des routeurs qui gère cette passerelle tombe en panne.

![image](https://github.com/user-attachments/assets/516d1747-4667-4668-b1a3-1bed197ecfe3)

Pour ça, on `regroupe 2 ou plusieurs routeurs` pour qu'ils se comportent comme `un seul <<routeur virtuel>>` : 

- IP et MAC Virtuels :  Ils partagent une adresse IP et une adresse MAC uniques que les hôtes utilisent comme passerelle par défaut.

- Fonctionnement : Par exemple, si ton PC a pour passerelle <192.168.1.1> (adresse virtuelle), quand il envoie un paquet, il resout l'adresse MAC de cette adresse IP. Il reçoit alors l'adresse MAC virtuelle.


## II. POURQUOI C'EST UTILE ?

Même si le routeur `actif` tombe en panne, le protocole fait en sorte qu'un routeur en `standby` prenne le relais rapidement. Le basculement se fait sans que les hôtes aient à changer quoi que ce soit dans leur configuration. Les hôtes continuent de voir la même IP et MAC de passerelle, donc ils n'ont pas besoin de connaître les détails de la redondance derrière.


## III. LES PRINCIPAUX FHRP CISCO ET NON-CISCO

- `HSRP (Hot Standby Router Protocol)` : C'est le protocole propriétaire de Cisco. Un seul routeur est actif et les autres attendent en standby. Quand l'actif tombe en panne, un standby prend le relais.

- `VRRP ( Virtual Router Redundancy Protocol)` : C'est un standard ouvert. Même principe, un routeur est actif et les autres sont en standby. Permet même de rejouter plus de routeurs pour une redondance accrue.

- `GLBP (Gateway Load Balancing Protocol)` : Ici, contrairement aux 2 précédents, plusieurs routeurs peuvent être actifs en même temps et répartir la charge du trafic sortant. Ca offre un load balancing tout en assurant la redondance. 
