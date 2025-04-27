![image](https://github.com/user-attachments/assets/d6ada505-99f2-46a8-82cb-9c7dd3b11712)

### 1. Etape 1

Émettez la commande `show spanning-tree` sur les trois commutateurs (SW1, SW2 et SW3) et identifiez quel commutateur est le pont racine : 

![image](https://github.com/user-attachments/assets/c2f84b5f-2b9c-4cfd-abb3-718f4db690d4)
![image](https://github.com/user-attachments/assets/f334bf75-3c2d-40f8-88f0-dd6926afdbf3)
![image](https://github.com/user-attachments/assets/bf815718-91c5-4053-bce7-8e24b2536ccb)

SW1 est le pont racine. Les trois commutateurs ont la même priorité ; `celui dont l'adresse MAC est la plus basse est` donc choisi comme pont racine.

### 2. Etape 2

Utilisez la commande `show spanning-tree` sur les trois commutateurs (SW1, SW2 et SW3) et examinez les rôles des ports : 

![image](https://github.com/user-attachments/assets/4209ce18-24a3-4a32-802b-907b68baaca4)
![image](https://github.com/user-attachments/assets/581705a7-9d90-4ce7-bfcc-ef273260d218)
![image](https://github.com/user-attachments/assets/b5027f37-fd43-4f8a-9093-657a6853ffc1)

Comme vous pouvez le voir, étant donné que SW1 est le pont racine, ses deux ports connectés sont dans un état désigné (transfert).

Comme SW2 et SW3 ne constituent pas le pont racine, un seul port doit être sélectionné comme racine sur chacun de ces deux commutateurs. Le port racine est celui dont le coût pour le pont racine est le plus faible. Comme SW2 a un BID (Bridge ID) inférieur (donc meilleur) à celui de SW3, tous ses ports sont définis comme désignés. Les autres ports de SW3 ne sont pas désignés.

Le protocole propriétaire Cisco PVST+ utilise le terme **`« alternatif »`** pour les ports non désignés. `Le port bloqué est également appelé port alternatif avec RSTP`. Il s'agit d'un chemin alternatif vers la racine, moins souhaitable. Le port est un port **`discarding`**, où les trames entrantes sont abandonnées.

![image](https://github.com/user-attachments/assets/6b5d8155-5478-4aff-8250-080f10468932)

## Modification de la priorité STP

`Il est préférable que le réseau ne choisisse pas lui-même le pont racine`. Si tous les commutateurs ont des priorités STP par défaut, celui dont l'adresse MAC est la plus basse deviendra le pont racine. Le commutateur le plus ancien aura l'adresse MAC la plus basse, car les adresses MAC les plus basses ont été attribuées en usine en premier. `Pour définir manuellement le pont racine, vous pouvez modifier la priorité du commutateur.`

Dans l'exemple de topologie, il est préférable que le commutateur de couche d'accès SW3 ne devienne pas le pont racine. Si SW3 était le pont racine, la liaison entre les commutateurs de couche de distribution serait bloquée. Le trafic entre SW1 et SW2 devrait alors transiter par SW3, ce qui n'est pas optimal. `Il est donc préférable que les commutateurs de distribution ou Core deviennent le pont racine.`

Il existe deux manières de modifier la priorité du commutateur :

- Définition de la valeur exacte . La valeur doit être comprise entre 0 et 65 535, par incréments de 4 096.

Syntaxe : `spanning-tree vlan vlan-id priority bridge-priority`

- Définition du pont racine principal avec une macro. Utilisez la commande `spanning-tree vlan vlan-id root primary`.

Lors de la modification de la priorité, vous devez définir les valeurs correctes. `La priorité peut être comprise entre 0 et 61 440, par incréments de 4 096.` **La valeur par défaut est 32 768**. Avec PVST, `extended system ID` est utilisé pour calculer la priorité du pont. La priorité passe de 16 à 4 bits, car 12 bits représentent l'ID VLAN. Avec une priorité de 4 bits, il existe 16 combinaisons au total. Par exemple, une valeur d'incrément de 15 donne une valeur de priorité de 61 440 (4 096 x 15). Vous ne pouvez pas modifier la priorité de 32 768 à 32 789. Vous devez ajouter un multiple de 4 096, de sorte que la valeur de priorité suivante possible soit 36 864.

<<NOTE : Si je créé un VLAN avec un id 3 et que la priorité STP est 32 768 en PVST+ on aurait un priorité de 32 768 + 3.>>

La meilleure solution consiste à utiliser la commande suivante :  `spanning-tree vlan vlan-id root {primary | secondary}` une macro qui abaisse la priorité du commutateur pour qu'il devienne le pont racine.

Pour configurer le commutateur afin qu'il devienne le pont racine d'un VLAN spécifique, utilisez le mot-clé `primary`. Utilisez `secondary` le pour configurer un pont racine secondaire. `En cas de défaillance du pont racine principal, il est déconseillé de faire du commutateur de couche d'accès le plus lent et le plus ancien le pont racine.`

Si la priorité racine actuelle est supérieure à 24 576, le commutateur local la définit sur 24 576. Si la priorité du pont racine est inférieure à 24 576, le commutateur local la définit sur 4 096, soit une valeur inférieure à la valeur du pont racine actuel.

Si vous émettez la commande `show running-configuration`, vous verrez la priorité du commutateur sous la forme d'un nombre, et non du mot-clé primary ou secondary.

NB : `Si la priorité du pont racine est définie sur 0, la configuration d'un autre commutateur avec cette root primarycommande sera sans effet. La commande échouera, car elle ne peut pas définir une priorité de commutateur local pour 4096 inférieure à celle du pont racine.`

### 3. Etape 3

Configurez et vérifiez SW2 comme pont racine pour le VLAN 1.

![image](https://github.com/user-attachments/assets/a8ec33b7-9b37-4ba4-b8c7-eb2447a83bb2)

Vérifiez que SW2 est désormais le pont racine pour le VLAN 1 : 

![image](https://github.com/user-attachments/assets/200bf1ac-f9ca-49e9-a1a5-12a1825d74b2)

Étant donné que SW2 est le pont racine, tous ses ports seront dans l'état désigné ou en cours de transfert. Les rôles de port SW1 et SW3 ont changé en fonction du changement du pont racine.

### 4. Etape 4

Utilisez la show spanning-treecommande sur SW1 et SW3 pour observer les rôles de port modifiés.

![image](https://github.com/user-attachments/assets/3b29880f-c1b7-4a66-899d-158226fdba09)

![image](https://github.com/user-attachments/assets/b3a5c52d-b016-48ca-ac01-0fba7b9e6ded)


## Manipulation du chemin STP

Pour déterminer le rôle du port, la valeur du coût est utilisée. Si tous les ports ont le même coût, l'ID de port de l'expéditeur tranche. Pour contrôler la sélection du port actif, vous pouvez modifier le coût de l'interface ou l'ID de port de l'interface de l'expéditeur.

![image](https://github.com/user-attachments/assets/4ae5116d-1b23-4b22-a382-457348619d72)

Vous pouvez modifier le coût du port à l'aide de la commande `spanning-tree vlan vlan-list cost cost-value`. La valeur peut être comprise entre 1 et 65 535.

Le Port_ID se compose de la priorité et du numéro du port. Le numéro de port étant fixe, vous pouvez modifier le Port_ID en configurant la priorité du port, car celle-ci dépend uniquement de son emplacement matériel.

Vous pouvez également modifier la priorité du port à l'aide de la commande `spanning-tree vlan vlan-list port-priority port-priority`. La valeur de priorité du port peut être comprise entre 0 et 255 ; `la valeur par défaut est 128. Une priorité de port inférieure indique un chemin privilégié vers le pont racine.`

Dans l'exemple donné, les interfaces Ethernet 0/1 et Ethernet 0/2 sur SW3 ont le même coût STP (100). Ethernet 0/1 sur SW3 est un port de transfert, car son Port_ID d'expéditeur (128,2 sur SW2) est inférieur à celui de l'expéditeur connecté à l'interface Ethernet 0/2 de SW3 (128,4 sur SW2). Une façon de faire de l'Ethernet 0/2 de SW3 un port de transfert est de réduire le coût de port de l'Ethernet 0/2 de SW3 à une valeur inférieure à 100. Une autre façon de faire de l'Ethernet 0/2 de SW3 un port de transfert est de réduire sa priorité de port d'expéditeur. Dans ce cas, vous devez modifier la priorité de port d'Ethernet 0/3 sur SW2 à une valeur inférieure à 128.

NB : `Donc ici, le port expéditeur => port du switch qui envoie les BPDUs sur ce trançon.`

### 5. Etape 5

Configurez Ethernet 0/2 sur SW3 comme port racine en modifiant son coût.

Actuellement, Ethernet 0/1 est le port racine de SW3. Ethernet 0/1 sur SW3 est en transfert car son Port_ID d'expéditeur (128,2 sur SW2) est inférieur à celui de l'expéditeur connecté à l'interface Ethernet 0/2 de SW3 (128,4 sur SW2). Puisque vous allez modifier le coût de l'interface Ethernet 0/2 de SW3, la priorité du port de l'interface expéditeur ne sera plus respectée. STP vérifie la priorité du port expéditeur uniquement lorsque les coûts sont égaux.

![image](https://github.com/user-attachments/assets/26ce1bab-23eb-4adc-afda-2239a6ed4cba)

### 6. Etape 6

À l’aide de la commande `show spanning-tree`, examinez les rôles des ports STP sur SW1 et SW3.

L'interface Ethernet 0/2 étant désormais moins coûteuse, elle est désignée comme port racine. STP reconsidère un nouveau chemin, de nouveaux rôles de port sont donc attribués aux ports SW1 et SW3. SW2 étant le pont racine, il disposera de tous les ports désignés ou redirigés.

![image](https://github.com/user-attachments/assets/c25e5aba-dad3-4fce-9130-737cbe8bc23b)

![image](https://github.com/user-attachments/assets/4f9713b6-3bd4-4ac9-bf20-115d48f8d21a)

### 7. Etape 7

Activez le débogage des événements de topologie STP sur SW3.

Vous avez effectué ce débogage pour observer la convergence STP, c'est-à-dire le temps nécessaire à STP pour établir un nouveau chemin après une panne de liaison en temps réel.

![image](https://github.com/user-attachments/assets/cdb1eadb-7939-47fc-8053-6d33a56acd10)

### 8. Etape 8

Arrêtez la liaison montante de transfert sur SW3, Ethernet 0/2, et observez combien de temps il faut à STP pour remarquer l'échec et effectuer la liaison de transfert redondante.

![image](https://github.com/user-attachments/assets/079d53e1-1059-4da6-8993-842b2e0afa04)


## STP Timers

STP utilise trois Timers différents pour garantir une convergence sans boucle appropriée :

- `Hello time` : le temps entre chaque BPDU envoyé sur un port. Par défaut, `il est de 2 secondes`.

- `forward delay` : temps passé en mode Listening et Learning. Par défaut, `il est de 15 secondes`.

- `maximum age` : contrôle la durée maximale qui s'écoule avant qu'un port de pont enregistre ses informations de configuration BPDU ; `égale à 20 secondes`, par défaut

`La transition entre les états du port prend de 30 à 50 secondes, selon le changement de topologie.`

Vous pouvez ajuster les timers STP. Vous pouvez régler le Hello time de `1 à 10 secondes`, le forward delay de `4 à 30 secondes` et le maximum age de `6 à 40 secondes`. `Cependant, les valeurs des temporisateurs ne doivent jamais être modifiées sans réfléchir. Lors de la modification des temporisateurs, appliquez-les uniquement au pont racine. Ce dernier propagera ensuite les valeurs des temporisateurs aux autres commutateurs.`

`Normalement, les temporisateurs STP ne sont pas modifiés. Il est préférable d'utiliser RSTP.`

### 9. Etape 9

Réactivez l'interface Ethernet 0/2 sur SW3.

Vous l'avez réactivé pour observer comment la topologie change après le retour d'une interface défaillante.

![image](https://github.com/user-attachments/assets/e2f3baa7-ed9f-4501-877c-ed80919ab87b)

### 10. Etape 10

Sur SW1 et SW3, utilisez la show spanning-treecommande pour observer comment les rôles de port STP sont redéfinis après la remise en service d'une interface défaillante.

![image](https://github.com/user-attachments/assets/71f04bbe-4e56-47d8-96e2-f20c8a0c7ca0)

Une fois que vous avez rétabli Ethernet 0/2 sur le SW3, il faudra environ 30 secondes à STP pour transférer ou bloquer les ports.


## Configurer RSTP

Les états des ports RSTP correspondent aux trois opérations de base d'un port de commutateur : `discarding, learning, and forwarding`. Il n'y a pas d'état Listening comme avec STP. Les états Listening et de blocage STP sont remplacés par l'état de Discarding.

Dans une topologie stable, RSTP garantit que chaque port racine et port désigné transitent vers le transfert, tandis que tous les `ports alternatifs` et `ports de Backup` sont toujours dans l'état de Discarding.

![image](https://github.com/user-attachments/assets/aabbacda-ad64-4c89-891b-5ff3fd9d4fa2)

Les rôles des ports RSTP, le rôle non désigné correspond aux rôles `alternatif` et `Backup`.

Comparé à STP, RSTP possède que 3 états : 

- `Discarding` : Cet état est observé aussi bien dans une topologie active stable que lors de la synchronisation et des modifications de topologie. L'état de Discarding empêche la transmission des trames de données, « interrompant » ainsi la continuité d'une boucle de couche 2.

- `Learning` : Cet état est observé aussi bien dans une topologie active stable que lors de la synchronisation et des modifications de topologie. L'état Learning accepte les trames de données pour remplir la table MAC afin de limiter l'inondation de trames unicast inconnues.

- `Forwarding` : Cet état n'est observé que dans les topologies actives stables. Les ports de commutation de transfert déterminent la topologie. Suite à un changement de topologie ou pendant la synchronisation, le transfert des trames de données n'intervient qu'après un processus de proposition et d'accord.

### 11. Etape 11

Activez RSTP sur tous les commutateurs (SW1, SW2 et SW3).

![image](https://github.com/user-attachments/assets/b5c20f62-80b4-4518-8ca1-ef618ba9a77a)

`Si tous les commutateurs du réseau, sauf un, utilisent RSTP, les interfaces reliées aux commutateurs STP LEGACY basculeront automatiquement vers le protocole STP non rapide`. Si vous utilisez des commutateurs Cisco, elles basculeront vers PVST+. Vous pouvez vérifier si RSTP est configuré sur tous les commutateurs en observant le temps de convergence.

### 12. Etape 12

Vérifiez que RSTP est activé sur les trois commutateurs (SW1, SW2 et SW3).

![image](https://github.com/user-attachments/assets/6af5ab05-8631-48e8-a012-f5ea6dea65b5)

### 13. Etape 13

Vérifiez les états de l’interface sur les trois commutateurs (SW1, SW2 et SW3).

![image](https://github.com/user-attachments/assets/7646953b-76bc-4ba7-a121-956c4f4908ef)


## RSTP Topology Changes

Dans l’ancien STP, tout changement d’état de port (montée en Forwarding ou descente en Blocking, link down, link up…) générait un Topology Change Notification (TCN).

En RSTP, on ne génère une notification (TCN) que quand un port non-edge (c’est-à-dire un port susceptible de participer à la topologie STP) passe à l’état Forwarding.

Une perte de connectivité n'est pas considérée comme un changement de topologie, contrairement au protocole STP. Quand un lien tombe, le port va en Discarding (équivalent de Blocking/Listening regroupés), mais ce n’est pas une transition vers Forwarding. RSTP ne déclenche donc pas de TCN sur un link-down, pour éviter de purger inutilement les tables MAC sur tout le domaine.

![image](https://github.com/user-attachments/assets/81b3f2c3-c3a5-4c3e-9bfd-743421adbb06)

`Changement de topologie = un port non périphérique change son état en état de transfert`

`Avec RSTP, tous les commutateurs peuvent envoyer des BPDU, pas seulement le pont racine.` Contrairement à STP où lorsqu'un switch détecte un changeent, il informe d'abord le root bridge qui lui envoie un TCA au switch qui a détecté et c'est uniquement le root bridge qui envoie les BPDUs avec le bit TC définis.

Dans la figure, SW4 envoie des BPDU depuis tous ses ports après avoir détecté une défaillance de liaison. SW2 envoie ensuite le BPDU à tous ses voisins, sauf celui qui l'a reçu de SW4, et ainsi de suite.

`Lorsqu'un commutateur reçoit une BPDU avec le bit TC activé d'un voisin, il efface les adresses MAC apprises sur tous ses ports, à l'exception du port qui reçoit le changement de topologie`. Le commutateur envoie également des BPDU avec le bit TC activé sur tous les ports désignés et le port racine.

`RSTP n'utilise plus le BPDU de notification de changement de topologie spécifique (TCN BPDU) à moins qu'un pont hérité ne doive être notifié.`

Pourquoi RSTP ne considère-t-il pas une défaillance de liaison comme une modification de topologie ? La perte de connectivité ne crée pas de nouveaux chemins dans la topologie. Si un commutateur perd la liaison avec un commutateur en aval, ce dernier dispose ou non d'un chemin alternatif vers le pont racine. Si le commutateur en aval ne dispose pas de chemin alternatif, aucune action n'est entreprise pour améliorer la convergence. Si le commutateur en aval dispose d'un chemin alternatif, il le débloque et génère ainsi ses propres BPDU avec le bit TC activé.

Comme avec STP, les ports compatibles PortFast (Qui est uniquement connecté à un utilisateur final, donc ne participe pas au processus STP) ne créent pas de modifications de topologie, ce qui réduit le nombre de messages de modification de topologie. Ils n'ont pas d'adresses MAC associées qui sont vidées lors de la réception d'un message de modification de topologie.

### 14. Etape 14

Arrêtez l'interface Ethernet 0/2 sur SW3 et observez le temps de convergence de RSTP.

Pour observer le recalcul de l'état du port, vous devez déclencher une modification de topologie. L'une des options consiste à arrêter l'interface.

Combien de temps faudra-t-il à Spanning Tree pour converger maintenant que vous avez activé la version rapide ?

![image](https://github.com/user-attachments/assets/8d796709-b346-46fc-8292-da13909f0c3c)

Le temps de convergence du protocole RSTP est bien plus court que celui du protocole STP. La convergence s'effectue à la vitesse de transmission des BPDU. Cette convergence peut durer moins d'une seconde.

### 15. Etape 15

Désactivez tout débogage sur le commutateur SW3.

![image](https://github.com/user-attachments/assets/3e02ab70-2091-4d92-84c3-c7327fa21bfa)

### 16. Etape 16

Vérifiez les états de l’interface sur les trois commutateurs (SW1, SW2 et SW3).

![image](https://github.com/user-attachments/assets/2e56cc83-b55f-4948-b0d3-1140b48f9a22)

L'interface E0/2 sur SW3 n'est plus répertoriée car elle n'est plus activée.

## Types de liens RSTP

![image](https://github.com/user-attachments/assets/99ba53b7-7bf0-4eed-9ec3-a53807570258)

Le type de lien est déterminé automatiquement mais peut être écrasé par une configuration de port explicite.

**`Un Port Edge`** est un port de commutateur qui n'est jamais destiné à être connecté à un autre commutateur. Les ports edge, équivalents aux liaisons point à point, sont candidats à une transition rapide vers un état de transfert. Avant que le paramètre de type de liaison puisse être pris en compte pour une transition rapide du port, RSTP doit déterminer le rôle du port.

`L'implémentation Cisco maintient que le mot-clé PortFast doit être utilisé pour la configuration du port edge, ce qui simplifie la transition vers RSTP.`

Les ports racine n'utilisent pas le paramètre de type de lien. Ils peuvent effectuer une transition rapide vers l'état de transfert lorsqu'ils sont synchronisés.

Les ports désignés exploitent au mieux le paramètre de type de lien. La transition rapide vers l'état de transfert du port désigné ne se produit que si le paramètre de type de lien indique **`une liaison point à point`**.

Nous avons le type de **`lien Shared`** qui est un port fonctionnant en mode semi-duplex. On suppose que le port est connecté à un média partagé où plusieurs commutateurs peuvent se trouver.

### 17. Etape 17

Configurez manuellement la liaison entre SW1 et SW3 en tant que liaison point à point.

![image](https://github.com/user-attachments/assets/0dd6779d-b39d-487c-985b-e21bfe75d272)

### 18. Etape 18

À l’aide de la commande show spanning-tree, vérifiez le type de lien pour le lien entre SW1 et SW3.

![image](https://github.com/user-attachments/assets/ed015083-624d-403a-bbb9-182aa13f78f3)












