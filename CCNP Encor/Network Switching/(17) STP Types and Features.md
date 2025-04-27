## I. POURQUOI STP ET SES VARIANTES ?

STP a été créé pour éviter les boucles dans un réseau redondant. Les boucles peuvent provoquer des `broadcast storms, des transmissions multiples et rendre instable la table MAC des switches`. STP met en place une `arborescence` où un seul chemin actif existe entre 2 points, tout en permettant la redondance au cas où un lien tombe en panne.

Plusieurs variantes ont été développées pour répondre à différents besoins en termes de convergence et d'optimisation du trafic, tout en prennant en compte la charge sur le CPU et la mémoire.


## II. LES DIFFERENTES VARIANTES DE STP

  #### `1. IEEE 802.1D (STP)`

Un seul arbe spanning Tree (une seule arborescence) est créée pour l'ensemble du réseau, quel que soit le nombre de VLANs. La convergence est lente (Il faut attendre plusieurs secondes pour réagir aux changements).

Le fait d'avoir une seule instance simplifie la charge CPU et mémoire.

Tous les VLANs utilisent le même chemin, ce qui peut conduire à des flux de trafic sous-optimaux.

  #### `2. Common Spanning Tree (CST)`

CST est essentiellement 802.1D dans lequel on considère une instance unique pour tout le réseau.

  #### `3. PVST+ (Per VLAN Spanning Tree)-Cisco`

Pour chaque VLAN configuré, une instance (arborescence) distincte de spanning tree est créee. La convergence est lente, similaire à 802.1D, mais se fait par VLAN. Il permet d'avoir des Root Bridge (Pont Racine) différents pour chaque VLAN, ce qui optimise les chemins de trafic.

Cependant, il y a une utilisation plus élevée du CPU et de la mémoire, car chaque VLAN nécessite sa propre instance (donc sa propre arborescence) de STP.

NB : **`Donc en gros ce qu'il faut retenir, plus il y a d'instance plus cela impacte le CPU et de la mémoire.`**

  #### `4. RSTP(Rapid Spanning Tree Protocol)-IEEE 802.1W`

Une évolution de 802.1D pour améliorer la vitesse de convergence. La convergence est rapide.Mais le soucis est qu'ici, nous aussi une seule instance (arborescence) pour le réseau entier, donc toujours le même problème de chemins non optimaux pour les différents VLANs

  #### `5. Rapid PVST+ - Cisco`

Combine les avantage de RSTP (Convergence rapide) avec la gestion par VLAN comme PVST+. Don, on a une instance RSTP pour chaque VLAN. `C'est donc la variante la plus gourmande en ressources (CPU, mémoire).`

  #### `6. MSTP (Multiple Spanning Tree Protocol)-IEEE 802.1S`

MSTP permet de regrouper plusieurs VLANs dans une même instance de spanning tree. **`MSTP supporte jusqu'à 16 instances de Spanning Tree.`**. Il permet de combiner des VLANs qui partagent des caractéristiques de trafic similaires dans une même instance, ce qui `réduit la charge du CPU par rapport à RPVST+ qui créé une instance RSTP par VLAN.`

`MSTP offre une convergence similaire à RSTP, mais avec moins d'instance à gérer`

NB : **`Sur les switches Cisco Catalyst, PSVT+ est activé par défaut pour IPV4.`**
