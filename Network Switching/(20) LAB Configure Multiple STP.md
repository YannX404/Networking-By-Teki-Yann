## I. REGIONS MST

![image](https://github.com/user-attachments/assets/8d5ab77b-736a-47c3-b7b3-a7fdd84f9cfe)

La configuration MST est présente sur chaque commutateur :

- Nom

- Numéro de révision

- Table d'association VLAN

![image](https://github.com/user-attachments/assets/2fe832de-1bb2-4b1d-96ae-6acaaf91156e)

MSTP diffère des autres implémentations de Spanning Tree par le fait qu'il combine certains VLAN, mais pas nécessairement tous, en instances logiques de Spanning Tree. Cette différence pose le problème de l'association des VLAN à chaque instance. Plus précisément, il s'agit d'étiqueter les BPDU afin que les équipements récepteurs puissent identifier les instances et les VLAN auxquels elles s'appliquent.

Ce problème n'est pas pertinent dans le cas de la norme 802.1D, où toutes les instances sont mappées à un CST d'instance unique et commun. Dans l'implémentation PVST+, les différents VLAN transportent les BPDU de leurs instances respectives (un BPDU par VLAN), en fonction des informations de marquage VLAN.

Pour fournir cette affectation logique de VLAN aux arbres de recouvrement, chaque commutateur exécutant MSTP dans le réseau dispose d'une configuration MSTP unique composée de trois attributs :

- `Un nom de configuration alphanumérique` (32 octets)

- `Un numéro de révision de configuration` (2 octets)

- `Une table de 4 096 éléments qui associe chacun des 4 096 VLAN potentiels pris en charge sur le châssis à une instance donnée`

Pour garantir un mappage VLAN-instance cohérent, le protocole doit pouvoir identifier précisément les limites des régions. À cette fin, les caractéristiques de chaque région sont incluses dans les BPDU. Le mappage VLAN-instance exact n'est pas propagé dans les BPDU, car les commutateurs doivent uniquement savoir s'ils se trouvent dans la même région qu'un voisin.

Par conséquent, seul un `digest (résumé) de la table de mappage VLAN-instance` est envoyé, accompagné `du numéro de révision` et `du nom`. Après réception d'une BPDU, un commutateur extrait ce digest (une valeur numérique dérivée de la table de mappage VLAN-instance via une fonction mathématique) et le compare à son propre digest calculé. Si les digest diffèrent, le mappage doit être différent, de sorte que le port sur lequel la BPDU a été reçue se trouve à la limite d'une région.

En termes génériques, un port se trouve à la limite d'une région si le pont désigné sur son segment se trouve dans une région différente ou s'il reçoit des BPDU 802.1D hérités.

Le numéro de révision de configuration permet de suivre les modifications apportées à une région MST. Il n'augmente pas automatiquement à chaque modification de la configuration MST. `Chaque modification doit être augmentée d'un numéro de révision (donc manuellement)`.

  ### 1. Etape 1

À l’aide de la commande `show spanning-tree summary`, examinez les instances de l’arbre de recouvrement sur SW3.

![image](https://github.com/user-attachments/assets/5244b006-aa12-4bef-88e9-51b99408ac5e)

Une instance STP est créée pour chaque VLAN avec PVST+. Dans ce laboratoire, cinq VLAN se traduisent par cinq instances STP. Si vous examinez SW1 et SW2, vous découvrirez qu'ils ont tous deux le même nombre d'instances STP en cours d'exécution que SW3.

  ### 2. Etape 2

Configurez les trois commutateurs (SW1, SW2 et SW3) pour qu'ils fassent partie de la même région MST, appelée « CCNP », et pour qu'ils aient la même révision, « 1 ».

![image](https://github.com/user-attachments/assets/e42576de-6e1d-4f56-9e6f-c12dbb42d85e)

  ### 3. Etape 3

Sur les trois commutateurs (SW1, SW2 et SW3), mappez les VLAN 2 et 3 à l'instance MST 1. Mappez les VLAN 4 et 5 à l'instance MST 2.

![image](https://github.com/user-attachments/assets/14a7ec47-1678-4c52-a117-ac9095ec33e5)

À ce stade, MST est configuré avec trois instances. Les VLAN 2 et 3 appartiennent à l'instance 1. Les VLAN 4 et 5 appartiennent à l'instance 2. `Tous les autres VLAN compris entre 1 et 4094 qui ne sont pas dans les instances 1 ou 2 appartiennent à l'instance 0`.

La commande `end` ou `exit` appliquera la configuration. Pour annuler la modification, utilisez le mot-clé `abort`.

  ### 4. Etape 4

Configurez SW1 comme pont racine principal pour l’instance MST 1 et comme racine secondaire pour l’instance 2.

![image](https://github.com/user-attachments/assets/087249c8-e449-4dee-b1e1-10e55f7e08d0)

Alternativement, vous pouvez modifier la priorité du pont du commutateur directement en utilisant la commande `spanning-tree mst instance-id priority priority`.

  ### 5. Etape 5

Configurez SW2 comme pont racine secondaire pour l’instance MST 1 et comme racine principale pour l’instance 2.

![image](https://github.com/user-attachments/assets/2a7a0fdd-3b6f-4913-8be4-b79aaa540b5b)

  ### 6. Etape 6

Changez le mode STP en MST sur les trois commutateurs (SW1, SW2 et SW3).

![image](https://github.com/user-attachments/assets/7fdad3e3-2023-4a05-883b-d1cb35b5f993)

`Il est déconseillé de modifier le mode STP en MST avant d'effectuer les mappages VLAN-instance. Toute modification du mappage entraînera un recalcul de l'arborescence STP.`

Un commutateur ne peut pas exécuter MST et PVST+ simultanément. Si vous exécutez la commande `show spanning-tree` sur l'un des trois commutateurs, vous remarquerez que « MSTP » est désormais le protocole activé.

  ### 7. Etape 7

Encore une fois, examinez les instances de l'arbre de recouvrement sur SW3.

![image](https://github.com/user-attachments/assets/9b353bcb-1c40-4859-ad73-390baef5ea40)

  ### 8. Etape 8

Utilisez la commande `show spanning-tree mst configuration` pour étudier la configuration MST sur SW3.

![image](https://github.com/user-attachments/assets/db899b2b-526e-416d-bf6e-14e9e36efcec)

Les VLAN 2 et 3 sont mappés à l'instance MST 1. Les VLAN 4 et 5 sont mappés à l'instance MST 2. Tous les autres VLAN sont mappés à l'instance MST 0 ou à l'IST.

Pour vérifier la configuration MST actuellement appliquée, utilisez `show current` en mode de configuration MST. Pour vérifier la configuration MST en attente, utilisez `show pending` en mode de configuration MST. Lorsque vous saisissez `exit` ou `end`, la configuration en attente devient actuelle. 

  ### 9. Etape 9

Vérifiez le digest du message MST sur les trois commutateurs.

![image](https://github.com/user-attachments/assets/57189e3d-24e6-4d54-9093-5f521ab81652)

La configuration MST étant identique sur les trois commutateurs d'une région, le résumé correspond. Une incohérence dans le résumé indiquerait une incohérence entre les listes de VLAN des commutateurs. Notez que le résumé peut être différent dans votre cas. Il est important que le résumé soit identique sur les trois commutateurs.

Le « Pre-std Digest » fait référence à l'implémentation pré-standard de MST par Cisco. Cisco a développé une version propriétaire de MST, appelée `MISTP`, dont les principes étaient similaires à ceux de MST.

  ### 10. Etape 10

Sur SW3, vérifiez les mappages des instances MST 1 et MST 2 et la convergence de la couche 2.

![image](https://github.com/user-attachments/assets/0d219f7d-6a50-4ffc-81fb-5c3f2ad7d2d7)

Les instances MST 1 et 2 présentent deux topologies de couche 2 distinctes. L'instance 1 utilise la liaison montante vers SW1 comme lien actif et bloque la liaison montante vers SW2. L'instance 2 utilise la liaison montante vers SW2 comme lien actif et bloque la liaison montante vers SW1.

![image](https://github.com/user-attachments/assets/7da6a88b-3f5f-413b-8e93-bbbcb8ca24b1)

## II. CONFIGURATION DE LA PRIORITE DU PORT

La priorité du port fonctionne de la même manière qu'avec les autres STP, sauf qu'avec MST, **les priorités du port sont configurées par instance**.

Comme tout autre STP, si une boucle se produit, MST peut utiliser le Port_ID de l'expéditeur pour sélectionner l'interface de transfert.

Définissez la priorité du port MST pour une instance MST donnée.

![image](https://github.com/user-attachments/assets/5baa9a13-98ed-4c21-85c2-374ca94d530d)

Vérifiez les paramètres Port_ID qui sont envoyés.

![image](https://github.com/user-attachments/assets/69cddb8c-3ccc-49a2-8ea1-66ef69f99572)

Une fois le pont racine déterminé, un périphérique MST non racine utilise cette séquence pour choisir le meilleur chemin vers le pont racine :

- `Coût du chemin racine le plus bas`

- `BID expéditeur le plus bas`

- `Port_ID expéditeur le plus bas`

Vous pouvez attribuer des valeurs de priorité d'expéditeur plus élevées (valeurs numériques plus faibles) aux interfaces à sélectionner en premier, et des valeurs de priorité d'expéditeur plus faibles (valeurs numériques plus élevées) à sélectionner en dernier. Si toutes les interfaces d'expéditeur ont la même valeur de priorité, MST place l'interface avec le Port_ID d'expéditeur le plus bas en état de transfert et bloque les autres interfaces.

Pour modifier la priorité du port STP d'une interface, entrez en mode de configuration d'interface et utilisez la commande `spanning-tree mst instance port-priority priority`.

Pour la variable d'instance , vous pouvez spécifier une instance unique, une plage d'instances séparées par un tiret ou une série d'instances séparées par une virgule. La plage est comprise entre 0 et 4 094. `Pour la variable de priorité , la plage est comprise entre 0 et 240, par incréments de 16. La valeur par défaut est 128`. `Plus le nombre est bas, plus la priorité est élevée`. Pour rétablir les paramètres par défaut de l'interface, utilisez la commande `no spanning-tree mst instance port-priority` de configuration d'interface.

Pour vérifier les paramètres de priorité des ports, utilisez `show spanning-tree mst interface interface` ou `show spanning-tree mst instance`. Cependant, les informations ne s'affichent que pour les ports en état de liaison opérationnelle. Sinon, vous pouvez utiliser la commande `show running-config` pour confirmer la configuration.

  ### 11. Etape 11

Sur SW3, définissez la priorité du port MST pour l'instance MST 1 sur 64 sur l'interface Ethernet 0/2.

![image](https://github.com/user-attachments/assets/4cee77d6-cc44-4947-9887-795f1f8bb105)

  ### 12. Etape 12

Sur SW3, vérifiez la priorité du port pour toutes les interfaces dans MST 1.

![image](https://github.com/user-attachments/assets/b3a38697-374e-4047-bf5d-348a94190e01)


## III. CONFIGURATION DU COUT DU CHEMIN MST

Le coût du chemin fonctionne de la même manière qu'avec les autres STP, mais avec MST, les coûts de port sont configurés par instance.

Comme pour tout autre protocole STP, la valeur par défaut du coût du chemin MST est dérivée de la vitesse du média d'une interface. En cas de boucle, MST utilise ce coût pour sélectionner l'interface de transfert.

Définissez le coût MST de l'interface sur 1 000 000.

![image](https://github.com/user-attachments/assets/2da86da0-fd11-4250-b45e-35baabac9118)

![image](https://github.com/user-attachments/assets/f6aebdde-0589-44f7-89f1-a7b9fb2f9ac6)

MST, comme tout autre STP, utilise une séquence de quatre critères pour choisir le meilleur chemin :

- BID le plus bas (pour sélectionner un commutateur racine)

- Coût de chemin racine le plus bas (sur tous les commutateurs non racine)

- BID d'expéditeur le plus bas (sur tous les commutateurs non root)

- Port_ID d'expéditeur le plus bas (sur tous les commutateurs non root)

Vous pouvez attribuer des valeurs de coût faible aux interfaces à sélectionner en premier et des valeurs de coût élevé à sélectionner en dernier. Si toutes les interfaces ont le même coût, MST place l'interface avec le Port_ID d'expéditeur le plus bas en état de transfert et bloque les autres interfaces.

Pour modifier le coût STP d'une interface, accédez au mode de configuration de cette interface et utilisez la commande `spanning-tree mst instance cost cost`. Pour la variable d'instance , vous pouvez spécifier une instance unique, une plage d'instances séparées par un tiret ou une série d'instances séparées par une virgule. La plage est comprise entre 0 et 4 094. Pour la variable de coût , la plage est comprise entre 1 et 200 000 000 ; la valeur par défaut est généralement dérivée de la vitesse du support de l'interface.

  ### 13. Etape 13

Sur SW3, définissez le coût du chemin MST pour l'instance MST 1 sur 1 000 000 sur l'interface Ethernet 0/2.

![image](https://github.com/user-attachments/assets/4b376b2f-6d4f-4c56-ad3c-bf38fb305756)

  ### 13. Etape 13
  
Sur SW3, vérifiez le coût du chemin pour toutes les interfaces dans MST 1.

![image](https://github.com/user-attachments/assets/c7ddc9cd-4739-44ae-9f41-3475bcb6e753)





