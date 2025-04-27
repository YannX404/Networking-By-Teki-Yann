## I. OBJECTIFS ET AVANTAGES DE MST

MST a été conçu pour réduire le nobre d'instances de spanning tree dans un réseau. Plutôt que de créer une instance de spanning tree pour chaque VLAN (comme avec PVST+ pouvant aller jusqu'à 4094), MSTP permet de regrouper plusieurs VLANs dans une même instance, ce qui réduit la charge CPU et mémoire sur les switches.

En regroupant les VLANs dans moins d'instances, la topologie spanning tree est simplifiée. Le fait d'avoir moins d'instance à gérer signifie une convergence plus rapide.

`MSTP est compatible avec les autres variantes de STP.`

MST permet d'avoir plusieurs chemins de transit actifs pour différents groupes de VLANs, facilitant ainsi la répartition de charge et améliorant la tolérance aux pannes.


## II. FONCTIONNEMENT DE MST

Plutôt que de créer une instance de spanning tree par VLAN, MST permet de **`mapper`** plusieurs VLANs à une seule instance de spanning tree.

Par exemple, si vous souhaitez que les VLANs 1 - 500 empruntent un chemin et les VLANs 501 - 1000 un autre, au lieu de gérer 1000 instances avec PVST+, vous pouvez créer seulement 2 instances MST.

Pour que MST fonctionne, tous les switches d'une même `région MST` doivent avoir `la même configuration MST (les mêmes mappages de VLANs vers instances)`.

`Une région MST = un groupe de switches configurés avec exactement les mêmes paramètres MSTP` :

- `même nom de région`,

- `même révision`,

- `même mappage VLAN ↔ instance`.

Les switches avec une configuration différentes ou les switches qui fonctionnent en mode legacy (802.1D) seront considérés comme appartenant à une autre région distincte.

IST ( Interne Spanning Tree) représente une instance spéciale (`habituellement l'instance 0`) qui est toujours active sur tous les ports qu'ils soient en mode trunk ou access. Les informations MST sont véhiculées via une seule BPDU qui est **`l'IST BPDU`**, qui contient les données pour toutes les instances internes.

`Le rôle de l'instance IST est de transporter l'information MST entre tous les switches d'une même région.`

Au lieu d'envoyer un BPDU par instance MST, tous les détails des instances internes sont inclus dans un seul BPDU, l'IST BPDU. Ce BPDU contient toutes les informations nécessaires pour toutes les instances MST configurées sur le switch.


## III. PRATIQUES RECOMMANDEES ET POINT DE VIGILANCES.

Il est recommandé d'utiliser des liens trunk entre les switches dans une régions MST afin que tous les VLANs puissent circuler correctement.

Le principe, c'est que dans MST, tous les liens au sein d'une région MST doivent pouvoir transporter les mêmes VLANs pour que la topologie soit cohérente. Lorsque tu fais du `pruning manuel`, tu supprimes certains VLANs de ce lien. Par exemple, si tu prunes VLAN 10 sur un lien, ce lien ne transportera plus le trafic associé à VLAN 10

`Il est conseillé de ne pas utiliser MST sur des ports configurés en mode access entre switches.`

`La configuration MST sur un switch d'une région est la même sur tous les autres switches de cette région. Donc MST attend que tous les VLANs Mappés soient les disponibles. D'où le fait d'éviter le pruning.`

Chaque switch diffuse un BPDU IST vers tous les autres switches de la région.
