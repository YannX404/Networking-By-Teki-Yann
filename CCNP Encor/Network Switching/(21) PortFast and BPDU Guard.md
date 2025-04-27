![image](https://github.com/user-attachments/assets/57adc7fe-97b3-4899-805d-6c1ceab7791b)## I. CONTEXTE ET ROLE DU STP

Le protocole STP est conçu pour éviter les boucles de commutation dans un réseau Ethernet en assurant une topologie sans boucle. Chaque fois qu'un port d'un switch est activé, il traverse plusieurs états pour s'assurer que l'activation ne crée pas de boucle.

![image](https://github.com/user-attachments/assets/6a4b8311-377c-45f6-a528-b058f60102ba)

- `Blocking State` - Jusqu'à 20 secondes : Le port ne transmet pas de trames et se contente de recevoir des BPDUs pour déterminer l'état du réseau.

- `Listening` - 15 secondes : Le port écoute les BPDUs qu'il reçoit pour obtenir des informations sur la topologie et ne transmet pas encore de données.

- `Learning` - 15 secondes : Le port commence à apprendre les adresses MAC, mais il ne transmet toujours pas de trames.

- `Forwarding`  : Le port est pleinement opérationnel, transmettant et recevant des trames tout en continuant de surveiller les BPDUs pour détecter d'éventuelles modifications de la topologie.

Ce processus complet peut introduire des délais lors de la connexion d'appareils qui n'ont pas besoin de la protection complète offerte par STP, comme les PCs, les imprimantes ou les serveurs.


## II. FONCTIONNEMENT PORTFAST


![image](https://github.com/user-attachments/assets/b4a7d5fa-eae4-46b0-9c87-f28d666f19c0)

PORTFAST est une amélioration de Cisco pour STP, spécialement conçue pour les `ports d'accès` qui se connectent directement à des dispositifs finaux (PCs, serveurs, etc...) et non à d'autres switches.

Lorsqu'un port est configuré avec PortFast, il contourne les états Listening et Learning de STP et passe directement à l'état forwarding dès son activation

Comme ces ports sont connectés à des appareils finaux qui ne génèrent pas de BPDUs, il n'y a pas de risque de boucle lors de la transition rapide.

Ceci est particulièrement utile pour éviter les délais (ex : DHCP) pour les utilisateurs ou les serveurs qui ont besoins d'une connexion réseau immédiate.

## III. FONCTIONNALITE BPDU GUARD

BPDU Guard est une mésure de sécurité complémentaire utilisée en conjonction avec PortFast. Elle permet de protéger le réseau contre une mauvaise configuration ou une connexion accidentelle d'un autre switch sur un port supposé être connecté à un appareil final.

Normalement, un port d'accès configuré avec PortFast ne devrait pas recevoir de BPDU, car les appareils finaux ne les envoient pas.

Si un BPDU est détecté sur un port où PortFast est activé, BPDU Guard `désactive immédiatement le port` en mettant le port dans un état `errdisable`. Ceci évite que le port ne devienne la source d'une boucle de commutation.

En désactivant le port dès qu'un BPDU est reçu, BPDU Guard garantit que la topologie STP ne sera pas altéré par des dispositifs non autorisés, comme un switch mal connecté par inadvertance.


## IV. CONFIGURATION DE PORTFAST ET BPDU GUARD

  #### 1. Configuration sur une interface spécifique

![image](https://github.com/user-attachments/assets/21e220da-19fe-46d7-a2c5-0f696b592309)

  #### 2. Configuration sur une interface globale

![image](https://github.com/user-attachments/assets/678d93d0-d2c7-4d13-85da-fff93b8828c0)

`NB : En activant BPDU Guard globalement, il sera activé que sur tous les ports avec PortFast.`

  #### 3. Vérifier les configurations PortFast et BPDU Guard

![image](https://github.com/user-attachments/assets/d2965679-6e1e-4b7c-afb5-9cf156980dc4)

Pour vérifier que PortFast est activé globalement : 

![image](https://github.com/user-attachments/assets/38fec7f8-a23f-4734-9706-a4c0005355eb)

Pour vérifier que BPDU Guard est activé sur une interface : 

![image](https://github.com/user-attachments/assets/5ef90806-cff5-4ee2-af44-c228c687e25d)

Pour vérifier que BPDU Guard est activé globalement : 

![image](https://github.com/user-attachments/assets/4773e390-cac7-44b0-af36-30f27d805e01)


NB: Pour définir un port comme port d'accès, vous pouvez également utiliser la commande suivante : `switchport host` cette commande définit le port comme port de commutation, définit le protocole Spanning Tree PortFast et désactive le channeling de port (etherchannel).

L'objectif de PortFast étant de minimiser le temps d'attente des ports d'accès connectés aux équipements utilisateur et aux serveurs pour la convergence du Spanning Tree, il est conseillé de l'utiliser uniquement sur les ports d'accès. Si vous activez PortFast sur un port connecté à un autre commutateur, vous risquez de créer une boucle Spanning Tree. N'oubliez pas que le filtre BPDU est disponible, mais déconseillé.

**`BPDU filter n'est pas recommandé car il masque les BPDUs`**

