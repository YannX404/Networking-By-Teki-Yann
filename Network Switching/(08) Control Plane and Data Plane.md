Imagine ton périphérique de couche 3 (Switch de couche 3 ou routeur), mais avec **2 cerveux** spécialisés.

![image](https://github.com/user-attachments/assets/e30095c7-fe38-47a8-988a-220e6009e4bb)

## I. CONTROL PLANE (PLAN DE CONTROLE)

Le plan de contrôle représente le cerveau de l'appareil. Il s'occupe de tout ce qui concerne `la gestion et l'échange des informations de routage`. Par exemple, il utilise des protocole comme OSPF, EIGRP, etc... pour connaître le meilleur chemin pour les données.

## II. DATA PLANE (PLAN DE DONNEES)

Le plan de données représente le muscle. Il prend les décisions du cerveau (le plan de contrôle) et s'occupe de `transférer les paquets à grande vitesse`. Chaque interface Ethernet a un **microprocesseur** et chacune gère le transfert réel des paquets.

## III. COMMENT LE PLAN DE CONTROLE ET DE DONNEE INTERAGISSENT ?

  #### 1. Du plan de contrôle au plan de données

Le plan de contrôle extrait les informations (routage, politiques de sécurité ou QoQ, etc...) et envoient aux modules d'interface. **Un module est une carte enfichable dans un châssis de routeur ou switch, apportant un ou plusieurs ports physiques**. Ces modules utilisent ces informations pour savoir où envoyer les paquets.

  #### 2. Du plan de données au plan de contrôle

Le plan de données collecte des informations comme les statistiques de trafic et les transmet au CPU (Processeur) pour permettre des ajustements et une meilleure gestion.
