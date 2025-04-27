**STP (Spanning Tree Protocol)** est un protocole conçu pour éviter les boucles dans les réseaux rédondants tout en maintenant une redondance physique.

## I. POURQUOI STP EST NECESSAIRE ?

Dans un réseau rédondant, les trames (broadcast, multicast ou même certans unicast) peuvent se répéter `indéfiniment` en circulant dans le réseau, ce qui peut causer : 

- `Broadcast storms` : Tous les switches inondent les trames de broadcast sur tous leurs ports (sauf celui sur lequel il a reçu la trame) et celles-ci tournent en boucle.

- `Transmission multiple de trames` : Un même paquet peut arriver en plusieurs exemplaires à destination, ce qui pertube les protocoles qui attendent une seule copie.

- `Instabilité de la base de données MAC` : Un même MAC peut être appris sur plusieurs ports à cause des boucles, ce qui peut saturer le switch et pertuber le transfert des données. La table CAM sera donc constamment mise à jour c'est ce qu'on appelle `le flapping`.


L'objectif de STP est donc de permettre la redondance physique (pour la tolérance aux pannes) sans créer de boucles qui pertubent le réseau.


## II. COMMENT STP FONCTIONNE ?

![image](https://github.com/user-attachments/assets/7028173d-6ba2-447f-9302-dbd02fada8c6)

STP crée une `arborescence (Spanning Tree)` qui couvre tous les switches du réseau. Pour ce faire, il réalise **3 étapes majeures** : 

  ### 1. Election du Root Bridge

Un seul switch est élu comme Racine (Root). **C'est le point de référence pour tout le réseau.** Tous les ports dur Root Bridge sont en mode `forwarding`

  ### 2. Sélection du Root Port sur les Non-root Bridge

Chaque switch non racine choisit un port qui offre `le chemin le moins couteux vers le root bridge. Le root port est le seul à transmettre les données vers le root bridge (pont racine)`

  ### 3. Choix du Designated Port sur chaque segment

Sur chaque segment de réseau, un seul port est désigné pour transférer le trafic vers le root bridge. Ce port est appelé **Designated port**, et est choisi sur le switch qui offre `le chemin le plus court vers le root.`


Tous les autres ports qui ne sont ni root ni designated deviennent des port `non désignés` et se placent en `blocking state.` Ces ports ne transmettent pas les trames, évitant ainsi la formations de boucles.

Si un lien actif (port en `forwarding state`) tombe en panne, STP reconfigure automatique l'arborescence (Spanning Tree) pour activer un port bloqué précédemment, rétablissant la connectivité.

## III. ROLES DES PORTS DANS STP

![image](https://github.com/user-attachments/assets/b3891393-f404-4020-9029-9c12c43fdfb4)



