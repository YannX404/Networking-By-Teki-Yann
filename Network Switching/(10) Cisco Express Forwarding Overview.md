CEF (CISCO EXPRESS FORWARDINGà, c'est le système qui permet de transférer les paquets super rapidement en utilisant des `tables pré-construites`. CEF se base sur 2 tables clées : 

- `FIB (Forwarding Information Base)` : Elle contient toutes les informations de routage dérivées de la table de routage. Elle est organisée pour des recherches ultr-rapides dans la TCAM.

NB : La FIB est stockée dans la TCAM sur le matériel (ASIC).

- `Adjacency Table` : Elle stocke les adresses MAC et les informations de réécriture d'en-tête pour le prochaint saut. Elle est construite à partir de la table ARP et permet de savoir comment formater les trames en sortie.

## I. SEPARATION DU PLAN DE CONTROLE ET DE DONNEES

  ### 1. Plan de contrôle (Logiciel)

C'est le cerveau qui construit et met à jour les tables FIB et Adjacency à partir des protocoles de toutage (OSPF, EIGRP, BGP, etc...). Il choisit les meilleures routes grâce à des critères comme la Distance Administrative.

  ### 2. Plan de données (Matériel)

Le muscle qui utilise ces tables (FIB et Adjacency) pour transférer les paquets ultr-rapidement via le microprocesseur (ASIC). 

Grâce à cette séparation, le CPU n'est pas surchargé et le transfert se fait en quelques fractions de seconde.

## II. MODES DE FONCTIONNEMENT DE CEF

  ### `1. Le mode centralisé CEF`

Les tables FIB et Adjacency sont stockées sur le **`route processor (RP) qui constitue le plan de contrôle et est géré par le CPU`**. Dans ce mode, c'est le RP fait lui même le transfert des paquets en utilisant ces tables.

### `1. Le mode Distribué CEF`

Chaque **line card** d'un switch ou routeur en chassis a sa propre copie des tables (FIB et Adjacency). Les line cards font le forwarding directement déchargeant ainsi le RP.

Lorsqu'un paquet arrive sur une line card (**`Rx Line card`**), le sytème consulte la FIB stocké dans la TCAM (**qui permet des recherches rapides**) sur son microprocesseur (ASIC) pour connaître le prochain saut. Si l'information est dans la Adjacency table, le paquet reçoit déjà l'en-tête de couche correct.

Le paquet est envoyé via le **`switching fabric (C'est comme le réseau interne à l'intérieur du châssis qui relie les Line Cards entre elles)`** vers la line card de sortie (**`Tx Line card`**), puis sur le réseau. Ce processus se fait entièrement en hardware, ce qui permet une transmission ultra rapide.




NB : Une line card (ou carte de ligne) est un composant matériel modulaire que l'on insère dans un châssis de switch ou de routeur modulaire. Elle fournit des interfaces réseau physiques (comme des ports Ethernet, SFP, QSFP, etc.) permettant la connexion aux périphériques du réseau : 

![image](https://github.com/user-attachments/assets/2a3e2f4e-8d56-438f-aef3-4f554bddea42)
