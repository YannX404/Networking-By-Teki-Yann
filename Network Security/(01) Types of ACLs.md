Les ACL se divisent en **2 grandes catégories** :

- `ACL Standard`
- `ACL Etendue`

## I. ACL STANDARD

Les ACL Standard se contentent de `vérifier l'adresse source d'un paquet IP`. Elles déterminent si l'accès doit être autorisé ou refusé pour l'ensemble du trafic provenant d'un réseau, d'un sous-réseau ou d'un hôte précis.

La plage des ACL Standard pour IPV4 est de `1 à 99` et `1300 à 1999`

## II. ACL ETENDUE

Les ACL Etendues vont plus loin que les ACL Standard en vérifiant à la fois `l'adresse source et l'adresse de destination`. Elles permettent également de filtrer selon des critères supplémentaires, tels que : 

- Des protocoles spécifiques (TCP, UDP, ICMP, etc...)

- Des numéros de port source et destination

La plage des ACL Etendue pour IPV4 est de `100 à 199` et `2000 à 2699`.

Il existe 2 façons d'identifier et de nommer une ACL : 

- `ACL Numérotée` : L'ACL est identifiée par un numéro. La plage de numéros utilisée indique si l'ACL est standard ou étandue

- `ACL Nommée` : L'ACL est identifiée par un nom descriptif. Cette méthode permet de créer des règles dont l'intention est plus facilement compréhensible et gérer plusieurs ACLs de manière plus intuitive. Les ACL nommées peuvent être configurées en tant que standard ou étendues.

`Sur une interface donnée, vous ne pouvez appliquer qu'une seule ACL par protocole et par direction (Entrante ou Sortante)` 
