## I. VUE D'ENSEMBLE DE L'ARCHITECTURE MULTICOUCHE TRADITIONNELLE

![image](https://github.com/user-attachments/assets/bb9ef0dd-bd19-4a00-ad3c-1db536e4979b)

L'idée, c'est d'assigner à chaque partie du réseau un rôle précis pour faciliter l'implémentation, la gestion et le dépannage.

Même si on parle souvent de 3 niveaux ou couches (Access, Distribution, Core), ça peut varier. Dans un petit campus, tu peux n'avoir que 2 niveaux (La couche Core et Distribution sont fusionnées ), et dans un grand campus, tu peux même avoir 4 niveaux ou plus.

L'essentiel, c'est que chaque niveau doit avoir des fonctions bien définies, peu importe comment le câblage est organisé.

## II. LES DIFFERENTES COUCHES ET LEURS RÔLES

  `### 1. Access Layer`

![image](https://github.com/user-attachments/assets/84efa6a9-a305-44e8-abe1-8238241777c2)

C'est là que se branchent tous les appareils finaux (PCs, Imprimantes, caméras, APs, etc...)

  `### 2. Distribution Layer`

![image](https://github.com/user-attachments/assets/edaff406-b096-4f5e-8a34-e4f59e633c30)

Cette couche fait le lien entre l'accès et le coeur (Core). Elle regroupe les connexions de plusieurs zones d'accès et isole les problèmes. ces fonctions sont les suivantes :

- Agrégation des connexions pour mieux gérer le trafic
  
- Mise en place des politiques (QoS, sécurité, etc...) pour filtrer et diriger le trafic
  
- Utilisations des mécanismes comme les FHRP pour offrir une redondance de passerelle par défaut, assurant ainsi une haute disponibilité
  
- Réalisation de routage Inter-VLAN et synthétiqsation des routes pour alléger le trafic vers le Core. **Le route summarization (ou agrégation de routes)** consiste à regrouper plusieurs sous‑réseaux contigus en une seule route de synthèse afin de n’en annoncer qu’une seule au routeur voisin. Appliquée à la couche de distribution, elle permet d’alléger significativement le nombre de routes que le cœur de réseau doit traiter et d’optimiser le trafic de mise à jour.

  `### 3. Core Layer (Backbone)`

![image](https://github.com/user-attachments/assets/ee96f17e-2970-44f4-84c8-0005bece4005)

Le Core, c'est le pilier central du campus, qui relie tous les autres niveaux et assur la communication globale. Cette couche doit être ultr-fiable et disponible en permanence. Il se contente de transférer les paquets très rapidements sans s'encombrer de traitements complexes ou de connexions directes aux users. Cette couche permet des mises à jour ou des changements sans interruption de service.

## III. QUAND ET POURQUOI UN COUCHE CORE (BACKBONE) ?

Dans un seul bâtiment ou des bâtiments adjacents, on peut parfois se passer d'un Core séparé et utiliser une topologie `Full Mesh` entre les switches des Distribution. Mais, plus il y a de bâtiments ou de modules, plus le nombre de connexions requises explose : 

![image](https://github.com/user-attachments/assets/6a0a506e-bf22-414a-84de-cb0037e5fdac)

NB : La formule pour calculer ce nombre de lien est `B X ( N - 2 )` avec B : Nombre de Building ou de modules, N : Nombre de switches de distribution.

Pour palier à ce problème, avoir un Core dédié permet de simplifier le design, de réduire la complexité du câblage et de faciliter les évolutions (ajouts, changements, etc...)

![image](https://github.com/user-attachments/assets/1cf45ef5-47dd-4eea-838c-3b87c94650fe)


