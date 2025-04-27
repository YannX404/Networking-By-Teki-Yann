## I. ETAPES POUR CONFIGURER UN ETHERCHANNEL

![image](https://github.com/user-attachments/assets/331fdb82-3765-4a9d-95d6-ef940d439759)

  #### 1. Identifier les ports à utiliser

Choisis les interfaces physiques qui seront regroupées sur chaque switch. `Pour un load-balancing optimal, privilégiez un nombre de ports à une puissance de 2 (2, 4, 8, etc...)`.

  #### 2. Configurz le channel group sur chaque interface

Tu vas entrer en configuration d'interface sur chaque port et associer ces ports à un numéro de groupe (Channel-group)

  #### 3. Configurer l'interface Port-Channel

Une fois le channel group configuré, une interface logique, appelée `port-channel` est créée. Les paramètres appliquées à l'interface port-channel (comme les VLANs, mode Trunk/Access, etc...) s'appliqueront à l'ensemble du lien agrégé (tous les ports qui y sont)

Pour qu'un Etherchannel se forme correctement, toutes les interfaces physiques du groupe doivent avoir exactement la même configuration : 

- Vitesse et Duplex indentiques

- Mode de port (access ou trunk). Si c'est un trunk, les interfaces doivent avoir le même VLAN natif et la même plage de VLAN autorisés. Pour un port d'accès, chaque port doit être affecté au même VLAN.


