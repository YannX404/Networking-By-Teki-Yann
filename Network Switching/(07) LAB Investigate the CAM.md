![image](https://github.com/user-attachments/assets/081ffeb2-0b52-48d1-9ca7-a92b763fcf10)


## I. Etape 1

De PC1, générer du traffic vers tous les appareils du sous-réseau.

Se connecter à PC1 et émettre un ping de diffusion jusqu'au 10.1.1.255. Configurer un nombre de répétitions de 10 et une taille de datagramme de 1500.

NB : **Dans l'environnement Cisco IOL, les PC sont simulés à l'aide de routeurs.**

![image](https://github.com/user-attachments/assets/7a7fdf2a-cc33-449c-8a65-7c37edcde120)


## II. Etape 2

Accédez à Switch1 et examinez sa table CAM.

Utilisez la commande : **`show mac address-table`**.

![image](https://github.com/user-attachments/assets/332911ce-770f-4acf-a3be-8655db1fca08)

Si PC1 envoie un paquet à PC2, Switch1 le reçoit sur Ethernet 0/1. Switch1 analyse la trame et constate que l'adresse MAC de destination est celle de PC2. Switch1 effectue ensuite une recherche et trouve l'adresse MAC de PC2 mappée sur Ethernet 0/2. Enfin, Switch1 transmet le message.

## III. Etape 3

Les commutateurs connectés à de nombreux appareils peuvent avoir des tables CAM très longues. Dans ce cas, vous pouvez utiliser le filtrage.

Sur Switch1, filtrez les adresses MAC que le commutateur a apprises via Ethernet 1/1.

Utilisez la commande **`show mac address-table interface Ethernet 1/1`**.

![image](https://github.com/user-attachments/assets/70443838-d486-4451-b832-9872a1ff075f)

Vous pouvez ajouter le mot-clé **`address`** pour spécifier une seule adresse MAC. Si vous souhaitez afficher uniquement les adresses MAC des appareils d'un VLAN spécifique, ajoutez le mot-clé **`vlan`**. 

Exemple : **`show mac address-table vlan 10`** ou **`show mac address-table address 0011.2233.4455`**

## IV. Etape 4

Comment est-il possible pour Switch1 de voir deux adresses MAC via le port Eth1/1 ?

**Switch1 voit deux adresses MAC via Ethernet 1/1 car ce port se connecte à un autre commutateur**. Donc on a l'adresse MAC du port du switch auquel Switch1 est connecté et l'adresse MAC d'un autre périphérique, peut-être un PC.

Pour vérifier cela utilisons la commande : **`show cdp neighbors`**

![image](https://github.com/user-attachments/assets/62fb89e1-e939-4213-8e87-b65933e18b4e)

Vous pouvez vérifier que le Switch 1 se connecte à un autre Switch en examinant ses voisins utilisant le protocole ** `Cisco Discovery Protocol (CDP)`**. le protocole Cisco Discovery Protocol permet de partager des informations sur les équipements Cisco directement connectés.

## V. TEMPS DE VIEILLISSEMENT CAM

Les tables CAM peuvent accueillir de nombreuses entrées pour le transfert de trames. Cependant, l'espace disponible pour chaque adresse sur un grand réseau est insuffisant. C'est pourquoi les adresses qui n'ont pas été utilisées depuis longtemps (entrées obsolètes) sont **périmées**. Ce délai est également appelé « **`aging time`** ».

Étudiez le temps de vieillissement à l'aide de la commande **`show mac address-table aging-time`**.

![image](https://github.com/user-attachments/assets/9016222f-5a53-4aea-a9f3-1c425e067c9d)

Le délai d'expiration par défaut des entrées de la table ARP (Address Resolution Protocol) est de **`4 heures`**. Sur les réseaux dont l'hôte génère peu de trafic pendant de longues périodes, les entrées de la table CAM peuvent expirer toutes les **`5 minutes`**. Dans ces rares cas, il peut être nécessaire d'augmenter le délai d'expiration CAM pour réduire le nombre d'inondations.

Les entrées de la table CAM ne peuvent pas être résumées comme dans le routage IP. Avec 1 000 appareils sur le réseau, il y a 1 000 adresses par table CAM et par commutateur. **`Lorsque la table CAM est pleine, le commutateur agit comme un concentrateur en transférant toutes les nouvelles trames, comme les diffusions.`** 

La solution consiste à implémenter un routage dans le réseau pour limiter l'inondation MAC.

Le paramètre par défaut du temps de vieillissement de la table CAM peut être modifié à l'aide de la commande suivante : **`mac address-table aging-time seconds`**. Modifiez le temps de vieillissement sur Switch1 à 600 secondes :

![image](https://github.com/user-attachments/assets/afaa9dcf-f5a7-4c87-a0d5-33655968ca20)

Après avoir modifié le temps de vieillissement, vérifiez le changement à l'aide de la commande **`show mac address-table aging-time`** :

![image](https://github.com/user-attachments/assets/962f4e41-1744-4d96-b71c-df74804d1040)












