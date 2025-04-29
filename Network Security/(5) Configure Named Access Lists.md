Les ACLs nommées offrent une approche plus lisible et plus modulaire pour configurer des ACLs. Plutôt que d'utiliser un numéro, vous définissez un nom qui décrit clairement l'intention de l'ACL.

## I. CREATION D'UNE ACL NOMMEE STANDARD

Pour configurer une ACL nommées standard, vous entrez en mode de configuration de l'ACL : 

![image](https://github.com/user-attachments/assets/dcbc5f6d-fe65-4876-9370-2ef19bec68b0)

Une fois en `mode ACL`, vous pouvez ajouter des entrées comme vous pouvez le voir sur l'image précédente.

Pour insérer une règle avant une entrée existante, vous pouvez spécifier un numéro de séquence : 

![image](https://github.com/user-attachments/assets/800dc79a-d86d-430c-8e03-f8bc7ff97fe2)

Ici, la règle <<deny>> sera évaluée avant la règle 10 (Attribuée par défaut à la première règle ajoutée)

Par défaut, si aucun numéro de séquence n'est précisé, les numéros commencent à 10 et s'incrémentent de 10 à chaque nouvelle entrée.

Vous pouvez également utiliser la commande `access-list resequence` pour réordonner les séquences sans recharger l'appareil.


## II. APPLICATION DE L'ACL SUR UNE INTERFACE

Après avoir configuré l'ACL, vous devez l'appliquer sur une interface pour qu'elle prenne effet. La commande est la même que pour les ACLs numérotées :

![image](https://github.com/user-attachments/assets/ac3a6249-8e95-403d-9c81-d5b9952a5824)

