![image](https://github.com/user-attachments/assets/aa6ae577-7194-4fc9-985c-b8c9970db3a6)

## I. COMPRENDRE HSRP

HSRP est un protocole FHRP qui permet un basculement transparent du périphérique IP de premier saut (passerelle par défaut). La plupart des hôtes IP ont l'adresse IP d'un seul routeur configuré comme passerelle par défaut. `Avec HSRP, c'est l'adresse IP virtuelle HSRP qui est configurée comme passerelle par défaut pour l'hôte, et non l'adresse IP du routeur.`

Les caractéristiques du HSRP sont les suivantes :

- HSRP définit un groupe de routeurs : un `actif` et un en `standby`.

- Les adresses IP et MAC virtuelles sont partagées entre les deux routeurs.

- Pour vérifier l’état HSRP, utilisez la commande `show standby`.

- HSRP est une propriété de Cisco. VRRP est une norme ouverte

![image](https://github.com/user-attachments/assets/b3490d91-3d0d-4cd9-a81f-6aa27de6e6dd)

HSRP définit un groupe de routeurs de standby, dont un est désigné comme routeur actif. HSRP assure la redondance des passerelles en partageant les adresses IP et MAC entre elles. Le protocole se compose d'adresses MAC et IP virtuelles partagées par deux routeurs appartenant au même groupe HSRP.

![image](https://github.com/user-attachments/assets/5f2c91f0-f07f-4836-a879-d05144719e0d)

- `Routeur actif` : Le routeur qui transmet actuellement les paquets pour le routeur virtuel

- `Routeur standby` : Le routeur standby principal

- `standby group` : L'ensemble des routeurs participant au HSRP qui émulent conjointement un routeur virtuel

La route active HSRP présente les caractéristiques suivantes :

- Répond aux requêtes ARP de la passerelle par défaut avec l'adresse MAC du routeur virtuel

- Suppose une transmission active des paquets pour le routeur virtuel

- Envoie des messages Hello

- Connaît l'adresse IP du routeur virtuel


La route standby HSRP présente les caractéristiques suivantes :

- Envoie des messages de Hello

- Écoute les messages Hello périodiques

- Suppose une transmission active des paquets s'il n'entend pas le routeur actif

Les hôtes du sous-réseau IP protégé par HSRP configurent leur passerelle par défaut avec l'adresse IP virtuelle du groupe HSRP. Les paquets reçus sur cette adresse IP virtuelle sont transmis au routeur actif.

La fonction du routeur standby HSRP est de surveiller l'état opérationnel du groupe HSRP et d'assumer rapidement la responsabilité de transfert de paquets si le routeur actif devient inopérant.

HSRP est un protocole propriétaire Cisco, tandis que VRRP est un protocole standard. Au-delà de cela, les différences entre HSRP et VRRP sont minimes.


## II. GROUPE HSRP

- Un groupe HSRP est un ensemble de périphériques HSRP qui émulent un routeur virtuel.

- Utilisez la commande suivante pour configurer le groupe HSRP :

- `Routeur( config-if)# standby group number ip ip-address`

- Par défaut, le numéro de groupe serait défini sur le **groupe 0**.

- `Avec la version 1 de HSRP, un numéro de groupe peut être n’importe quel entier compris entre 0 et 255`.

- Tous les routeurs d’un groupe HSRP échangent des messages Hello.

- Les rôles des routeurs du groupe HSRP sont élus en fonction de l'échange de messages Hello.

- Les routeurs actifs et standby HSRP envoient des messages Hello à l'adresse de multidiffusion `224.0.0.2`, port UDP 1985.

Infos : Qu’est-ce qu’un message `ICMP Redirect `? 

Fonctionnement :

Le PC envoie un paquet à sa passerelle par défaut (Router A).

Router A voit : « Hé, j’ai un meilleur chemin vers cette IP via Router B »

Router A renvoie au PC un message ICMP Redirect :

« Pour aller à 8.8.8.8, envoie tes paquets à 192.168.1.2 (Router B) plutôt qu’à moi. »

Le PC stocke cette information et, pour cette destination, n’enverra plus qu’à Router B.

HSRP (Hot Standby Router Protocol) fournit une adresse virtuelle (ex. 192.168.1.1) sur laquelle tous les PCs s’appuient comme passerelle.

Scénario :

Le PC envoie vers 192.168.1.1 → c’est Router A (actif HSRP) qui répond.

Router A envoie un ICMP Redirect comme ci-dessus, PC se met à parler directement à Router B (qui n’est pas dans le groupe HSRP).

Si Router B tombe en panne, le PC continue d’envoyer là-bas — et ne bascule pas automatiquement sur le routeur de secours HSRP (Router C).

Conséquence : le PC perd la connectivité, car il ne réutilise plus jamais 192.168.1.1 (HSRP) pour cette destination.

Désactiver les redirections ICMP sur les interfaces HSRP, par exemple sous IOS :

`interface GigabitEthernet0/1
  no ip redirects`

  ### 1. Etape 1

Configurez l'interface Ethernet 0/1 (orientée LAN) de R1 avec l'adresse IP 192.168.1.3/24 et une adresse IP de secours du groupe 1 HSRP de 192.168.1.1.

L'adresse IP 192.168.1.1 est l'adresse IP virtuelle HSRP qui est également configurée comme adresse IP de passerelle par défaut sur PC1 et PC2.

![image](https://github.com/user-attachments/assets/38c100ed-ae01-48c7-bb29-b713fe29b893)

  ### 2. Etape 2

Sur R2, configurez l'interface Ethernet 0/1 (orientée LAN) avec l'adresse IP 192.168.1.2/24 et une adresse IP de secours du groupe 1 HSRP de 192.168.1.1.

`R1 et R2 doivent tous deux avoir la même adresse IP standby HSRP configurée.`

  ### 3. Etape 3

Sur R1, vérifiez la table ARP.

![image](https://github.com/user-attachments/assets/6ed48fad-d611-47e5-a957-9f6ee820a4fa)

`L'adresse IP et l'adresse MAC correspondante du routeur virtuel sont conservées dans la table ARP du routeur actif dans un groupe HSRP.`

L'adresse MAC HSRP est au format suivant : `0000.0c07.acXX`, où `XX correspond au numéro de groupe HSRP converti de décimal en hexadécimal`. Les clients utilisent cette adresse MAC pour transférer des données.

![image](https://github.com/user-attachments/assets/c01a1397-6745-4bf5-b498-a77628f162a6)


## III. TRANSFERT VIA LE ROUTEUR ACTIF

![image](https://github.com/user-attachments/assets/eca02b06-3184-4773-8938-9e690c6c6a10)

Tous les routeurs d’un groupe HSRP ont des rôles spécifiques et interagissent de manière spécifique.

Le routeur virtuel est simplement une paire d'adresses IP et MAC configurée par les terminaux comme passerelle par défaut. Le routeur actif traite tous les paquets et trames envoyés à l'adresse du routeur virtuel. Le routeur virtuel ne traite aucune trame physique.

Au sein d'un groupe HSRP, un routeur est désigné comme routeur actif. Ce routeur répond au trafic du routeur virtuel. Si une station terminale envoie un paquet à l'adresse MAC du routeur virtuel, le routeur actif reçoit et traite ce paquet. Si une station terminale envoie une requête ARP avec l'adresse IP du routeur virtuel, le routeur actif répond avec cette même adresse MAC. Dans cet exemple, R1 assume le rôle actif et transmet toutes les trames adressées à l'adresse MAC connue 0000.0c07.acXX, où XX est l'identifiant du groupe HSRP. `Tandis qu'ARP et ping utilisent les adresses IP et MAC virtuelles HSRP, le routeur répond au traceroute avec sa propre adresse IP. Ceci est utile lors du dépannage pour déterminer quel routeur est utilisé pour le flux de trafic.`

**`Lors d'une transition de basculement, le routeur nouvellement actif enverra trois requêtes ARP gratuites afin que les périphériques de couche 2 puissent apprendre le nouveau port de l'adresse MAC virtuelle.`**


## IV. PRIOTITE HSRP

Lors de la configuration de la priorité HSRP, tenez compte des éléments suivants :

- `Le routeur actif est élu en fonction de la priorité HSRP`.

- `Utilisez une valeur comprise entre 0 et 255`.

- `La priorité par défaut est 100`.

![image](https://github.com/user-attachments/assets/1b107984-1c47-4705-a7f8-448ffa55db70)

La priorité HSRP est un paramètre qui permet de choisir le routeur actif parmi les périphériques HSRP d'un groupe. La priorité est une valeur comprise entre 0 et 255. La valeur par défaut est 100. `Le périphérique ayant la priorité la plus élevée devient actif.`

`Si les priorités des groupes HSRP sont identiques, l'appareil possédant l'adresse IP la plus élevée sera actif. Dans cet exemple, il s'agit de R1.`

Définir une priorité est judicieux pour des raisons déterministes. Vous souhaitez connaître le comportement de votre réseau dans des conditions normales. Savoir que R2 est la passerelle active pour les clients du VLAN 1 vous permet de rédiger une documentation pertinente.

Cependant, maintenant que vous avez modifié la priorité R2 à 110, `il ne deviendra pas automatiquement le routeur` actif, car `la préemption n'est pas activée par défaut`. La préemption est la capacité d'un périphérique compatible HSRP à déclencher le processus de réélection.

  ### 4. Etape 4

Sur R2, configurez le groupe HSRP 1 avec une priorité de 110.

![image](https://github.com/user-attachments/assets/7b761a2d-4326-4db3-9a31-ea240fa6d4f5)


## V. CONFIGURER LA PREEMPTION

La configuration suivante préempte l'élection HSRP si un périphérique avec une priorité plus élevée est mis en ligne.

`La préemption est désactivée par défaut.`

![image](https://github.com/user-attachments/assets/be35f596-036e-48d9-85c4-0922adb7117f)

  ### 5. Etape 5

Sur R1 et R2, configurez l'interface Ethernet 0/1 HSRP groupe 1 avec préemption.

![image](https://github.com/user-attachments/assets/397d1e88-0dd5-4950-a121-2e2b53591451)

Vous aurez le message suivant : `*Jul 24 22:25:16.790: %HSRP-5-STATECHANGE: Ethernet0/1 Grp 1 state Standby -> Active
`

## VI. CONFIGURER LES TIMERS HSRP

- Configurez les valeurs de temps de salutation et de temps de maintien en millisecondes : `Router (config-if)# standby [group-number] timers [msec] hellotime [msec] holdtime
`

- `La valeur holdtime doit être au moins trois fois supérieure au Hello timer`.

- La configuration du délai de basculement doit également prendre en compte le délai de convergence IGP.

- Configurez le temporisateur de délai de préemption de sorte que la préemption se produise après le redémarrage complet du périphérique HSRP et l'établissement d'une connectivité complète au réseau.

Si un routeur actif envoie un message Hello, les routeurs récepteurs considèrent ce message comme valide pendant une période d'attente. Cette durée doit être au moins trois fois supérieure à la durée du message Hello. Elle doit être supérieure à cette durée.

Vous pouvez ajuster les temporisateurs HSRP pour optimiser les performances de HSRP sur les périphériques de distribution, augmentant ainsi leur résilience et leur fiabilité dans le routage des paquets hors du VLAN local.

Par défaut, `Hello Timer du protocole HSRP est de 3 secondes` et `le Hooldtime de 10 secondes`. `Cela signifie que le temps de basculement peut atteindre 10 secondes` pour que les clients puissent communiquer avec la nouvelle passerelle par défaut. Cet intervalle peut parfois être excessif pour la prise en charge des applications. Les paramètres Hello Timer et du Holdtime sont configurables. Pour configurer le délai entre les messages d'attente et le délai avant que les autres routeurs du groupe ne déclarent le routeur actif ou de secours hors service, saisissez la commande suivante en mode de configuration d'interface :

![image](https://github.com/user-attachments/assets/9270237a-6475-4d17-a277-4b1750d32423)

**NB**  : La réduction des temporisateurs HSRP permet de détecter plus rapidement une défaillance du premier saut. Cependant, des temporisateurs HSRP plus courts impliquent davantage de paquets Hello HSRP, ce qui entraîne un trafic supplémentaire.

La préemption est une fonctionnalité importante du protocole HSRP. Elle permet au routeur principal de reprendre son rôle actif lorsqu'il est remis en ligne après une panne ou une maintenance. La préemption est le comportement recherché, car elle impose un chemin de routage prévisible pour le trafic VLAN en fonctionnement normal. Elle garantit également que le chemin de transfert de couche 3 d'un VLAN est parallèle à celui du protocole STP (Spanning Tree Protocol) de couche 2, dans la mesure du possible.

  ### 6. Etape 6

Sur R1 et R2, configurez le hello timer du groupe 1 Ethernet 0/1 HSRP sur 200 millisecondes, un hold time de 750 millisecondes et un délai de préemption minimum de 300 secondes.

![image](https://github.com/user-attachments/assets/0092f83a-8aae-4d6a-b238-04698b147465)

Le preempt delay sert à éviter que votre routeur ne prenne trop vite le rôle Active HSRP au démarrage (ou dès qu’il redevient opérationnel), avant que toute la connectivité au niveau des liens et de l’infrastructure (STP, négociation de trunk, adresses MAC, etc.) ne soit parfaitement rétablie.

Si votre routeur devient Active HSRP dès qu’il sort du démarrage, mais que ses trunks ne sont pas encore en Forwarding, il pourrait annoncer la passerelle virtuelle au réseau alors que certains switches ne savent pas encore y acheminer le trafic.

  ### 7. Etape 7

Sur R1 et R2, vérifiez l’état HSRP.

![image](https://github.com/user-attachments/assets/0bac8a76-8eec-4600-8072-1144c12d3cfd)
![image](https://github.com/user-attachments/assets/96ac5ba3-c15c-4eac-bb04-c09729cb52e8)

R1 est le routeur de secours et R2 est le routeur actif. Le résultat show standbypeut être condensé avec le brief mot-clé.


## VII. TRANSITION D'ETAT HSRP

Un routeur HSRP peut être dans l’un des cinq états suivants.

- `Initial` : L'état initial. Il s'agit également de l'état après un changement de configuration ou lors de la première activation d'une interface.

- `Listen` : Le routeur connaît l'adresse IP virtuelle et écoute les messages Hello des autres routeurs.

- `Speak` : Le routeur envoie des messages Hello périodiques et participe activement à l'élection du routeur actif ou de secours.

- `Standby` : Le routeur est candidat pour devenir le prochain routeur actif et envoie des messages Hello périodiques. Hormis les conditions transitoires, il y a au plus un routeur du groupe en état standby.

- `Active` : Le routeur transfère actuellement les paquets envoyés à l'adresse MAC virtuelle du groupe. Il envoie des messages Hello périodiques. Hormis les situations transitoires, il ne doit y avoir qu'un seul routeur actif dans le groupe.

Lorsqu'un routeur se trouve dans l'un de ces états, il exécute les actions requises par cet état. Tous les routeurs HSRP du groupe ne passent pas par tous les états. `Dans un groupe HSRP de trois routeurs ou plus, un routeur autre que le routeur standby ou le routeur actif reste en état Listen`. Autrement dit, quel que soit le nombre d'appareils participant au protocole HSRP, un seul peut être actif et un autre en standby. Tous les autres appareils sont en état d'écoute.

Tous les routeurs démarrent dans l'état initial. Cet état est l'état de départ et indique que HSRP n'est pas en cours d'exécution. Cet état est atteint suite à une modification de configuration, par exemple lorsque HSRP est désactivé sur une interface ou lorsqu'une interface compatible HSRP est activée pour la première fois, par exemple, lors de l'exécution de la commande no shutdown.

L'état d'écoute permet de déterminer si des routeurs actifs ou de secours sont déjà présents dans le groupe. En état de communication, les routeurs participent activement à l'élection du routeur actif, du routeur de secours, ou des deux.

Chaque routeur utilise trois timers pour les messages Hello HSRP. À l'expiration d'un timer, le routeur passe à un nouvel état HSRP.

Lorsque deux routeurs participent à un processus d'élection, une priorité peut être configurée pour déterminer lequel doit devenir actif. Sans configuration de priorité spécifique, chaque routeur a une priorité par défaut de 100, et celui dont l'adresse IP est la plus élevée est élu routeur actif.

Quelles que soient les priorités ou les adresses IP des autres routeurs, un routeur actif le reste par défaut. Une nouvelle sélection n'aura lieu que si le routeur actif est supprimé. Lorsque le routeur de secours est supprimé, une nouvelle sélection est effectuée pour le remplacer. Vous pouvez modifier ce comportement par défaut avec l'option de préemption.

  ### 8. Etape 8

Vérifiez la transition d'état HSRP. Démarrez le débogage des événements HSRP sur R1.

![image](https://github.com/user-attachments/assets/b1961784-9752-4fbb-a08b-8d482cad0489)

  ### 9. Etape 9

Vérifiez la transition d'état HSRP. Activez un ping continu de 1 000 000 de paquets du PC1 vers le serveur à l'adresse 192.168.4.22.

![image](https://github.com/user-attachments/assets/29dc7d62-75e4-4ad0-92b1-229ab0a71752)

Le trafic circule désormais de PC1 via ASW1, ASW2, R2 et R3 jusqu'au serveur, puis inversement. R2 est désormais le routeur actif.

  ### 10. Etape 10

Pour simuler la panne de R2, fermez son interface Ethernet 0/1. Quelques tentatives pourraient être perdues lors de votre ping continu de PC1 vers le serveur, mais le basculement de R2 vers R1 devrait être rapide et fluide pour PC1.

![image](https://github.com/user-attachments/assets/024fe9fb-a926-4655-b78f-a667e718bfd0)

  ### 11. Etape 11

Sur R1 et R2, examinez l'état du protocole HSRP. Observez le message de débogage sur l'interface de ligne de commande de R1. Vérifiez ensuite l'état du protocole HSRP sur R1.

Le message de débogage doit être similaire au suivant :  `R1(config-if)# *Jul 29 16:22:09.856: HSRP: Et0/1 IP Redundancy "hsrp-Et0/1-1" update, Active -> Active 
R1(config-if)#`

  ### 12. Etape 12

Sur R2, affichez l’interface Ethernet 0/1.

![image](https://github.com/user-attachments/assets/ff885ddc-58bd-4f96-b1e9-e07adc1597bc)

R2 deviendra-t-il le routeur actif ou de secours ?

  ### 13. Etape 13

Sur R1 et R2, examinez l’état HSRP.

![image](https://github.com/user-attachments/assets/1bcc6834-8f5c-463a-9d12-39cbf2e2fae2)

Une fois que R2 est à nouveau disponible et que le délai de préemption a expiré, il devient le routeur actif. R1 est à nouveau en standby car la préemption est activée. Ainsi, lorsqu'un périphérique HSRP de priorité supérieure est connecté, il devient le périphérique actif.

`NB : Vous devez attendre que le délai de préemption expire avant que R2 retrouve son statut de routeur actif.`

Avec la préemption désactivée, ce qui est le comportement par défaut, R2 ne retrouverait pas le statut actif après sa remise en ligne.

Si vous effectuez un traceroute de PC1 vers le serveur, vous constaterez que R2 répond avec son adresse d'interface réelle (192.168.1.2), et non avec l'adresse HSRP virtuelle (192.168.1.1). Cette information est utile pour vérifier par quel périphérique de premier saut transite le trafic.









