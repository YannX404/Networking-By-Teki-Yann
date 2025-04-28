## I. ACL IPV4 NUMEROTEES STANDARD

![image](https://github.com/user-attachments/assets/5ed2e4d3-3c6f-4738-bb0d-88a35d35e5f3)

Les ACLs Standard filtrent les paquets uniquement en se basant sur `l'adresse source`. Cela signifie qu'elles `permettent` ou `refusent` l'accès pour l'ensemble du protocole TCP/IP à partir d'un réseau, sous-réseau ou hôte précis.

La commande utilisée est : `RouterX(config)# access-list 1 permit 172.16.0.0 0.0.255.255`

Cette commande crée une ACL Standard car le numéro 1 est compris dans la plage 1 à 99 et permet tout trafic dont l'adresse source commence par 172.16.

La commande de vérification est la suivante : 

![image](https://github.com/user-attachments/assets/4631f72f-95f0-40fe-be8d-4a0fa243ecd6)

Si vous voulez supprimer une ACL Numérotée Standard, vous utilisez la commande : 

![image](https://github.com/user-attachments/assets/771e8b44-9b09-4f46-b8ec-7b1c9bfc78cf)

NB : Avec les ACLs Numérotées, vous ne pouvez pas supprimer une seule entrée (ACE) à l'aide de la commande précédente. `Celle-ci supprime toute l'ACL.` La méthode traditionnelle pour modifier une entrée consiste à copier l'ACL dans un éditeur, apporter les modifications, supprimer l'ACL existante et reconfigurer la nouvelle version.

## II. ACL IPV4 NUMEROTEES ETENDUES

Les ACLs Etendues offrent un filtrage plus précis. Elles ne se limitent pas à vérifier l'adresse source, mais elles examinent aussi : 

- l'adresse de destination
- Les protocoles utilisés (TCP, UDP, ICMP, etc...)
- Les numéros de ports source et de destination

Exemple 1 : `RouterX(config)# access-list 101 deny tcp 172.16.4.0 0.0.0.255 172.16.3.0 0.0.0.255 eq 21`

L'ACL refuse le trafic FTP du sous-réseau 172.16.4.0/24 vers le sous-réseau 172.16.3.0/24.

Exemple 2 : `RouterX(config)# access-list 101 permit ip any any`

L'ACL autorise tout autre trafic.


La configuration ACL étendue utilise des nombres de 100 à 199, ou de 2000 à 2699 (101 a été utilisé dans l'exemple).

Vous pouvez configurer des listes de contrôle d'accès IPv4 étendues numérotées sur un routeur Cisco en mode de configuration globale. Cette commande : `access-list` crée une entrée dans une liste de filtres IPv4 étendue.

Pour les listes d'accès étendues, vous pouvez spécifier le nom ou le numéro du protocole. Les mots-clés les plus couramment utilisés sont ip, tcp, udpet icmp.

Lorsque TCP ou UDP est spécifié comme protocole, il est possible d'inclure `lt(inférieur à)`, `gt(supérieur à)`, `eq(égal)`, `neq(différent de)` et `range(plage inclusive)` pour faire correspondre des applications réseau spécifiques par nom (par exemple, www, FTP, SSH) ou par numéro (par exemple, 80, 21, 22).

![image](https://github.com/user-attachments/assets/7c5c97a4-521e-4492-b78d-dea6e37ca430)

La sortie de la commande `show access-list 101` affiche uniquement les ACL étendues nouvellement ajoutées qui ont été configurées sur RouterX.

Notez que le numéro de port 21 initialement configuré a été automatiquement remplacé par le mot-clé FTP.

Comme c'était le cas avec la liste d'accès standard, l'utilisation de la commande : `no access-list 101` supprime l'intégralité de l'ACL étendue 101 du routeur.







