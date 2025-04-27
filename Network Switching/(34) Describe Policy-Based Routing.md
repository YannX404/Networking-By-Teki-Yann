## I. QU'EST CE QUE LA POLOCY-BASED-ROUTING

![image](https://github.com/user-attachments/assets/4f9d4280-037a-4f17-bfa2-8f997def2eca)

La `PBR (Policy-Based Routing)` est une technique avancée qui permet de définir des politiques personnalisées pour acheminer le trafic réseau, indépendamment de la destination finale.

Alors que le routage IP traditionnel se base uniquement sur l'adresse de destination pour déterminer le chemin, la PBR vous offre la possibilité de forcer certains paquets à suivre des chemins spécifiques en fonction de critères définis par l'administrateur.

## II. CARACTERISTIQUES ET AVANTAGES DE LA PBR

Le routage classique utilise uniquement l'adresse de destination. PBR permet de définir des politiques basées sur d'autres critères, par exemple `l'adresse source`, `le protocole`, `ou même le port source/destination`.

La PBR permet de contourner les limitations du routage par défaut en adaptant l'acheminement selon des critères spécifiques (Utile par exemple, quand on veut faire passer certains flux par un fournisseurs d'accès spécifique).

La PBR s'applique sur les paquets `entrants ou ceux générés localement par le routeur.` Elle nécessite l'utilisation d'un `route map` pour définir les critères de correspondance (`match`) et les actions à effectuer (`set`).

En plus de l'achéminement par défaut, `la PBR peut être utilisée pour répartir la charge entre plusieurs chemins` en fonction des caractéristiques du trafic (`load-sharing`)


## III. COMMENT FONCTIONNE LA PBR ?

Une route map est configurée avec des commandes match pour identifier les paquets (par exemple, par adresse source) et des commandes set pour modifier le comportement de routage.

![image](https://github.com/user-attachments/assets/d202bb82-c4e8-4cbb-82b9-315eb90635f6)

Exemple : Pour diriger le trafic du réseau 192.168.1.0/24 vers un chemin alternatif, on peut écrire une route map qui match l'adresse source et set le prochain saut vers ISP1.

La route map est ensuite appliquée à une interface d'entrée (`inbound`) ou aux paquets `générés localement`.

Cela force le routeur à utiliser l'information de la route map pour prendre la décision d'acheminement, en contournant le routage basé uniquement sur la destination. `La PBR contourne donc la table de routage.`

