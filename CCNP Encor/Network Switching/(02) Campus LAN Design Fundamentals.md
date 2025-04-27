## I. LES BASES D'UN CAMPUS LAN

  ### 1. C'est quoi un CAMPUS LAN ?

Un réseau `Campus LAN` (Campus Local Area Network) est une infrastructure réseau interconnectant plusieurs LAN au sein d’un bâtiment ou d’un groupe de bâtiments géographiquement proches, afin de relier des équipements tels que PC, imprimantes, serveurs et points d’accès sans fil de manière optimisée et centralisée. 

  ### 2. Le but ?
  
Le but de ce LAN est de permettre à tous ces appareils de communiquer, de partager des ressources et d'accéder à Internet ou au WAN via un réseau central.

## II. MODELE DE CONCEPTION HIERARCHIQUE

  ### 1. Réseau Plat (Flat Network)

![image](https://github.com/user-attachments/assets/1a02a908-0339-42f7-9073-23417e2086e2)

Un réseau Flat ou Plat en français est un réseau dans lequel tous les appareils sont connectés dans un seul grand réseaux **sans sous-réseaux**.

  ### 2. Quel est le problème ?

Quand il y a peu d'appareils, ça va, mais avec des centaines d'appareils, le trafic de diffusion fini pas saturer le réseau et ralentir tout le monde.

  ### 3. Segmentation avec des dispositifs de couche 3

Utiliser des routeurs ou des switches de couche 3 pour diviser le réseau en sous-réseaux plus petits. En faisant cela, les broadcast (diffusions) restent limités à un sous-réseau, donc moins de saturation et de ralentissements.

## III. LES 3 COUCHES DU MODELE HIERARCHIQUE

![image](https://github.com/user-attachments/assets/5ceaba7a-8b0b-49b0-9699-cdfcf6e9d91d)

  
  `### 1. La couche d'accès (Access Layer)`

C'est sur cette couche que se branchent les utilisateurs finaux.

  `### 2. La couche de distribution (Distribution Layer)`

Elle agrège les connexions provenant de plusieurs switches d'accès

  `### 3. La couche coeur (Core Layer)`

C'est le coeur. Elle se doit d'être ultra-rapide. Elle s'occupe de transporter le trafic agrégé de toutes les autres couches. Comparée aux autres couches, elle doit être extrêmement disponible, rapide et capable de s'adapter aux changement pour maintenir le réseau en continu.


## IV. ARCHITECTURES CAMPUS : LAYER 2 VS LAYER 3

  `### 1. Design Layer 2 pur`

![image](https://github.com/user-attachments/assets/bce93df3-a967-4ef8-ab92-04e8494d4953)

Cette architecture est plus simple et moins cher à mettre en place. Les VLANs se limitent à la couche de distribution. **Le protocole STP bloque la moitié des `uplinks` pour éviter les boucles, ce qui limite la bande passante**.

`### 2. Design Layer 3 (Routage au niveau de l'accès`

![image](https://github.com/user-attachments/assets/f709d647-cb54-475b-b3a4-99efbc659b12)

Ici, chaque switch d'accès peut gérer ses propre VLANs. Les liens entre les couches d'accès et de distribution sont `routés`, ce qui permet de mieux contrôler et séparer le trafic. La couche 2 s'arrête donc au niveau de la couche d'accès et non de distribution.
