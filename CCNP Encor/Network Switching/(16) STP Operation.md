## I. LES BPDUs (BRIDGE PROTOCOL DATA UNITS)

![image](https://github.com/user-attachments/assets/2bffc443-c22e-4ebf-9d34-14aaad45fff8)


Les BPDUs sont des trames spéciales échangées entre switches pour transmettre des informations STP, indispensables pour : 

- L'élection du root bridge (pont racine)

- La détection des boucles et la gestion des changements de topologie

Une trame BPDU comporte les champs suivants : 

-`Protocol ID`: identifie le protocole STP.

- `Version`:  identifie la version actuelle du protocole.

- `Message type`:  identifie le type de BPDU : `configuration BPDU` ou `Topologiy Change Notification BPDU (TCN)`.

- `Flags`: utilisés en réponse à un BPDU TCN.

- `Root bridge ID`: identifie l'ID du pont racine.

- `Root path cost`: identifie le coût cumulatif pour atteindre le root bridge.

- `Senderbridge ID`:  identifie l'ID du commutateur qui envoie le BPDU.

- `Port ID`:  identifie le port du switch qui envoie le BPDU.

- `Message age`: indique l’âge du BPDU actuel.

- `Maximum age`:  indique la valeur du délai d'expiration.

- `Hello time`: identifie l'intervalle de temps entre la génération des BPDU de configuration par la racine.

- `Forward delay`: définit le temps pendant lequel un port de commutateur doit attendre dans l’état `Listening` et `learning`.

**`Par défaut, les BPDUs sont envoyés toutes les 2 secondes.`**

Nous avons 2 types de BPDUs : 

![image](https://github.com/user-attachments/assets/30763869-c3af-45dc-9583-f7eca78571c1)

- `Configuration BPDU` : Ces BPDUs sont utilisés pour construire et maintenir l'arborscence (spanning tree). Ils contiennent des informations telles que l'identifiant du pont racine, le coût du chemin vers le pont racine, l'identifiant du pont émetteur et l'identifiant du port émetteur.​ En gros ils sont utilisés pour les fonctionnement normal.

- `Topology Change Notification (TCN)` : Ces BPDUs sont utilisés pour notifier un changement de topologie, tel que l'activation ou la désactivation d'un port, ou l'ajout ou la suppression d'un commutateur dans le réseau. Émission : Lorsqu'un commutateur détecte un changement de topologie, il envoie un TCN BPDU vers le pont racine pour l'informer du changement. Le pont racine répond ensuite en envoyant des Configuration BPDUs avec le bit de changement de topologie (TC) activé pour informer tous les autres commutateurs du réseau.​

## II. L'ELECTION DU ROOT BRIDGE

![image](https://github.com/user-attachments/assets/cb36db84-ef41-4360-b9b5-a4a01034234e)

![image](https://github.com/user-attachments/assets/d50e2de0-9ae8-451e-9ad2-47823397c4eb)

Chaque switch possède un `BRIDGE ID` constitué de : 

- **`Priority`** : de `0 à 61 440`, par défaut `32 768`, modifiable par incréments de `4 096`.

- **`MAC ADDRESS`** : Unique pour chaque switch

NB :**` Initialement, chaque switch pense être le Root Bridge`**. Ils envoient des BPDUs présentant leur propre Bridge ID.

Lorsqu'un switch reçoit un BPDU, il compare le Bridge ID reçu avec ce qu'il a. Si le BPDU contient un Bridge ID inférieur (ce qui est donc meilleur car `C'est le Bridge ID le plus petit qu'on prend`), il abandonne l'idée d'être le Root Bridge.

Tous finissent par s'accorder sur un switch comme Root Bridge (`Celui avec le Bridge ID le plus bas` comme on l'a dit )

Si un switch avec un Bridge ID plus bas que celui du Root Bridge apparaît, l'élection se refait et un nouveau Root Bridge peut être élu.


## III. DETERMINATION DES PORTS ET DES CHEMINS

Une fois le Root Bridge élu, chaque switch non-racine doit déterminer : 

- **`Le port racine (Root Port)`** : Sur un switch non racine, c'est le port `ayant le chemin de coût le plus bas vers le root bridge`. Le coût est calculé en fonction de la bande passant de chaque lien. Plus la bande passante est élevée, plus le coût est faible.

![image](https://github.com/user-attachments/assets/d22c05db-9d6a-4f9f-97ef-b52cabaf090c)

**En cas d'égalité, on regarde le `Port ID` du port émetteur**. `Port ID = Port Priority (par défaut 128) + Port Number`

- **`Les Designated Ports`** : Sur chaque segment, un seul port est élu pour transmettre le trafic vers le Root Bridge.

`Sur le Root Bridge, tous les ports sont par défaut des ports Désignés.`

Sur les autres switches, le port `ayant le coût le plus faible pour atteindre le Root Bridge est élu Designated Port pour le segment`.

Si plusieurs ports ont le même coût, le switch `compare ensuite les Bridge ID et enfin les Ports ID pour trancher`.

-**`Les ports non-désignés`** : Tous les ports qui ne sont ni root ni un port designated deviennent des ports non-designés et se mettent en `blocking state` pour éviter la formation de boucles.


## IV. LES ETATS DES PORTS

Pour participer au processus STP, chaque port passe par plusieurs états, qui déterminent s'il peut transmettre ou pas : 

![image](https://github.com/user-attachments/assets/2ac343bc-90ff-4b39-a273-41e2049a4b57)


