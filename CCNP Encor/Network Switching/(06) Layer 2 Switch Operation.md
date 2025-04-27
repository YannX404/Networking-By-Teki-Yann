## I. DU HUB AU SWITCH

Avant, on utilisait des hubs. Dans un hub, tous les appareils partagent la même bande passante et fonctionnent en mode **`half-duplex`** c'est à dire envoyer ou recevoir mais pas les 2 en même temps. Si 2 appareils envoient des données en même temps, ça crée ce qu'on appelle **`Une collision`** et tout le monde doit attendre avant de pouvoir renvoyer des données.

Un switch, c'est comme un pont à plusieurs ports. Chaque port est "**isolé**", ce qui signifie qu'il n'y a pas de collisions entre les ports, et tu peux avoir le **`Full-Duplex`** c'est à dire envoyer et récevoir en même temps.

## II. LA TABLE CAM (CONTENT ADDRESSABLE MEMORY)

![image](https://github.com/user-attachments/assets/2dc74669-58d2-4b86-bfaa-9a8c5eae3e57)

Le switch utilise une **`table CAM`** pour enregistrer les adresses MAC des appareils. Quand il reçoit une trame, il regarde **l'adresse MAC source** pour l'ajouter dans la table CAM avec **le port sur lequel il reçu cette trame**. 

Si l'adresse MAC de destination est connue, le switch envoie la trame uniquement sur le port associé à cette adresse MAC de destination. Si elle n'est pas connue, le protocole **`ARP`** rentre en jeu pour resoudre l'adresse MAC de l'adresse IP source contenue dans le paquet. Donc on aura un **`flooding`** sur tous les port du VLAN (`sauf sur le port sur lequel la trame a été reçu`) pour trouver l'adresse MAC du destinataire.


## III. TRAITEMENT DES TRAMES ET FILES D'ATTENTES

  ![image](https://github.com/user-attachments/assets/ce97e2f1-6771-4938-81f5-7f3b4696db6d)
  

  #### `1. Ingress Queue`

Chaque port reçoit des trames dans une file d'attente (ingress queue). ces files d'attente peuvent avoir différentes **`priorités`** pour traiter d'abord les trames les plus importantes.

Pour chaque trame, le switch se demande : 

- Où la transmettre ?

- Est-ce qu'il doit la transmettre ?

- Comment la transmettre ?(avec quelle priorité par exemple)

#### `2. Egress Queue`

Une fois les décisions prises, la trame est placée dans une file d'attente de sortie (Egress Queue) sur le port adéquat, toujours en tenant compte de la priorité, donc de la QoS (Quality of Service).


## IV. CAM VS TCAM

La table **CAM** est utilisée pour les décisions de `couche 2` (très rapide). C'est là que le switch stocke les adresses MAC pour savoir sur quel port envoyer les trames.

La table **`TCAM (Ternary Content Adressable Memory)`** est une mémoire spécialisée utilisée dans les équipements réseau comme les switches de couche 3 et les routeurs, c'est à dire les équipement de `couche 3`. Cette mémoire est essentielle pour accélérer les décisions de commutation et de routage, améliorant ainsi les performances globales du réseau. Comparée à la table CAM qui est nécessaire principalement pour la table d'adresses MACs (commutation), la table TCAM est utilisée pour des fonctions plus avancées comme les ACLs, la QoS, le routage, etc...
