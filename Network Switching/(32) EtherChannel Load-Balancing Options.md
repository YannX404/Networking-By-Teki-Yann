Etherchannel regroupe plusieurs liens physiques pour former un seul canal logique. Pour répartir le trafic sur ces liens physiques, le switche utilise `un algorithme de hachage` qui, à partir d'un ou plusieurs éléments d'une trame, calcule une valeur binaire qui correspond à un `index` dans le groupe. Cet index détermine sur quel lien le paquet sera transmis. 

![image](https://github.com/user-attachments/assets/4ff151ec-97fc-4855-955e-1cafdf9812b2)

`La méthode de load-balancing est globale sur le switch`. Tu ne peux pas utiliser une méthode différentes pour un Etherchannel particulier; la configuration s'applique à tous les canaux sur le dispositif.

## I. LES DIFFERENTS CRITERES DE HACHAGES

Voici quelques-unes des options de base (qui varient selon le modèle de switch)

- dst-ip : Utilise l'adresse IP de destination. Les paquets allant vers une destinations passeront toujours par le même lien

- dst-mac : Utilise l'adresse MAC de destination. Les paquets destinés au même hôte vont sur le même lien

- src-ip : Utilise l'adresse IP source. Les paquets provenant de différents hôtes seront répartis, mais ceux du même hôtes iront toujours par le même lien

- src-mac : Utilise l'adresse MAC source pareil qu'avec l'IP, les trames d'un même hôte iront sur le même lien

- src-dst-ip : Combine l'adresse IP source et destination. Permet de différencier les flux selon les deux extrémités, ce qui est utile quand plusieurs flux convergent vers une même destination.

- src-dst-mac : Combine les adresses MAC source et destination

- src-port/dst-port/src-dst-port : Utilise les numéros de port TCP/UDP. Ces options sont disponibles sur certaines modèles (4500, 6500, 9K)

## II. COMMENT L'ALGORITHME DE HACHAGE FONCTIONNE ?

Si le switch utilise, par exemple le dst-ip, qui est un seul critère, il examine certains bits de l'adresse IP (souvent les bits de poids faible). Par exemple, dans un groupe de 4 liens, il utilisera **2 bits**( car 2 puissance 2 fait 4) pour choisir l'un des 4 liens.

Quand on utilise 2 adresses (par exemple, src-dst-ip) le switch effectue un `XOR (Dans le XOR, si les 2 bits sont identiques, on 0. C'est que quand les 2 bits sont différents qu'on a 1)`

Exemple : Imagine 2 hôtes avec ces IP en binaire : 

- Hôte A : (Source) : ....XXXX0
- Hôte B : (Destination) : ....XXXX1

Dans l'Etherchannel, nous supposons que nous avons 2 liens.

On a 2 adresses, donc on fait un XOR est le résultat du XOR ici donnera : ....XXXX1. On a pris le bits de poids le plus faible car 2 puissance 1 => 2. Donc les index des liens de l'Etherchannel seront : 0 ou 1. 

Dans le resultat du XOR, le dernier bit est 1 du coup, c'est le 2 ème lien qui est choisi pour ce flux.


Si un hôte est très actif par exemple, communuque principalement avec un autre hôte, dans le cas de src-dst-ip par exemple, tous leur trafic iront sur un seul lien. Mais cela signifie aussi que la charge sur ce lien ne sera pas repartie. Dans ce cas, utiliser un critère différent (par exemple, ajouter les numéros de port) pourrait aider à mieux répartir le trafic.

En effet, si vous ajoutez aussi les numéros de port, vous hachez (srcIP, dstIP, srcPort, dstPort). Que se passe-t-il ?

Une même connexion TCP a bien sûr toujours les mêmes ports tout au long de sa durée (ex. srcPort=49152, dstPort=80) :
→ tous les paquets de cette connexion continueront d’aller sur le même lien (hash constant).

Mais si le même hôte A ouvre plusieurs connexions parallèles vers B (web, sftp, etc.), chaque connexion utilisera un port source différent (49152, 49153, 49154…).
→ le hash(srcIP, dstIP, srcPort, dstPort) variera de connexion en connexion, et différentes connexions seront réparties sur différents liens du bundle.



Vous pouvez vérifier les options d'équilibrage de charge disponibles sur l'appareil à l'aide de la commande `port-channel load-balance ?` de configuration globale. (N'oubliez pas que le « ? » affiche toutes les options de cette commande.)

