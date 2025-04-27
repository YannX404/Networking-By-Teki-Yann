## I. APPROCHE MODULAIRE</h1>

![ENCOR_2-0-0_Cisco_Enterprise_Architecture_Model_001-1](https://github.com/user-attachments/assets/57076d4f-e494-415d-8411-d0dc2fa619a0)

CISCO divise le réseau en ce qu'on appelle des '**modules**'. En faisant cela, si une chose déconne dans un module, cela n'affectera pas totalement tout le réseau et aussi, tu pourras faire des changements sans vraiment créer de nouveaux problèmes.

Nous avons **4 modules** principaux qui sont :

1. #### `Le module Entreprise Campus`

C'est tout simplement le réseau interne de l'entreprise. Ce module est organisé en 3 niveaux :

- `Access` : Où se branchent les utilisateurs et les appareils.

- `Distribution` : Qui s'occupe de gérer le trafic entre les accès.

- `Coeur` : Qui représente le centre du réseau. A l'intérieur, il y a aussi un sous-module qui est le `'data-center'` avec **une arichtecture spine-leaf** pour héberger les serveurs et gérer la surveillance et la maintenance.

2. #### `Le module Enterprise Edge`

C'est **la porte de sortie ou d'entrée du réseau interne de l'entreprise vers l'extérieur**. Il gère l'accès à Internet, le VPN et la connexion WAN(avec des technologies comme le MPLS, par exemple)

3. #### `Le module Service Provider Edge` 

Ce module relie ton site principale à d'autres sites distants via un FAI. 

4. #### `Le module Remote Locations`

Ici, on parle des sites éloignés. L'idée, c'est d'avoir une connectivité fiable même à distance.


## II. CARACTERISTIQUES DU RESEAU

Pour être fiable et évolutif, le réseau doit avoir certaines qualités. Ces qualités sont : 

- Le `Self-healing` : c'est à dire le réseau doit petre capable de se réparer automatiquement en cas de panne, pour rester toujours actif(opérationnel)

- Le `Self-defending` : Le réseau doit pouvoir se protéger lui-même contre les attaques et les intrusions

- Le `Self-optimizing` : Il doit pouvoir s'ajuster en fonction des changements. On parle là d'adaptation, sans que tu aies à intervenir constamment.

- Le `Self-aware` : Il doit être capable de surveiller ce qui se passe.
