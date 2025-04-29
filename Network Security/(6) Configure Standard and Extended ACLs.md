![image](https://github.com/user-attachments/assets/972529a0-c88c-4ad2-a97d-48e41dc17954)

NB : `Il est toujours conseillé de tester la connectivité et les services réseau avant d'appliquer le filtrage ACL.` Cela garantit que le réseau est pleinement fonctionnel et que la perte de connectivité ou de fonctionnalité est due aux ACL appliquées et non à un problème réseau préexistant.

#### 1. Etape 1

À l’aide de Telnet, effectuez les tests suivants :

PC1 peut utiliser avec succès Telnet pour se connecter à R1 en utilisant le port 23.

PC1 peut utiliser avec succès Telnet pour se connecter à SRV1 en utilisant les ports 80 et 443.

PC1 peut utiliser avec succès Telnet pour se connecter à SRV2 en utilisant les ports 80 et 443.

PC2 peut utiliser avec succès Telnet pour se connecter à SRV1 en utilisant les ports 80 et 443.

Note : Lorsque vous utilisez Telnet pour vous connecter au port 80 ou 443, vous devrez peut-être appuyer sur Entrée pour que le système réponde une fois la session établie.

![image](https://github.com/user-attachments/assets/35f9ef4e-55e5-4f0a-96dc-fa0bacd62220)

![image](https://github.com/user-attachments/assets/f8c9a5c2-03d0-460f-b65e-a4bbb83fa388)

![image](https://github.com/user-attachments/assets/ca4074ec-f78e-45b7-86ae-731197ad4d12)

Notez l'utilisation des noms de domaine des appareils ( srv1 , srv2 , r1 ) lors des tests. Rappelons que SRV2 est un serveur DNS qui répond aux requêtes qu'il reçoit.


#### 2. Etape 2

Tous les tests sont réussis. Telnet est utilisé pour vérifier le fonctionnement des ports 80 et 443 sur SRV1 et SRV2. Une fois la session Telnet ouverte, vous pouvez utiliser n'importe quelle touche de caractère suivie de la touche Entrée pour fermer la connexion.


#### 3. Etape 3

Les listes de contrôle d'accès standard permettent de filtrer le trafic uniquement en fonction de l'adresse IP source. Une bonne pratique courante consiste à configurer une liste de contrôle d'accès standard aussi proche que possible de la destination. Dans cette étape, vous configurez une liste de contrôle d'accès standard numérotée sur R1. Cette liste est conçue pour bloquer le trafic de l'hôte 10.10.1.10 vers le réseau 10.10.2.0/24 sur R1. Cette liste de contrôle d'accès sera appliquée en sortie sur l'interface Ethernet 0/1 de R1.

![image](https://github.com/user-attachments/assets/c46a307d-ff3f-4446-8cd7-93b652107ee2)

Rappelez vous que l'ACL standard ne se base que sur l'adresse IP source !

Cette liste de contrôle d'accès (ACL) a pour but d'empêcher l'hôte PC1 d'accéder au réseau 10.10.2.0/24, tout en autorisant tout autre trafic. N'oubliez pas que chaque ACL possède une instruction `implicite deny any` qui bloque tout trafic ne correspondant pas à une instruction de l'ACL. C'est pourquoi cette instruction `permit any` a été ajoutée à la fin de l'ACL.

L'ACL est appliquée dans le sens sortant sur l'interface Ethernet 0/1 car il s'agit de l'interface la plus proche de la destination que vous essayez de bloquer.


#### 4. Etape 4

Testez la liste de contrôle d'accès (ACL) en envoyant un ping de PC1 à SRV1. Comme l'ACL est conçue pour bloquer le trafic provenant des adresses sources de l'hôte 10.10.1.10, PC1 ne devrait pas pouvoir envoyer de ping à SRV1. Effectuez un test similaire depuis PC2 pour vous assurer que les autres périphériques ont toujours accès à SRV1. Vérifiez la liste de contrôle d'accès à l'aide de la show access-listcommande suivante.

![image](https://github.com/user-attachments/assets/740cfbcd-7c45-4d3b-8dc5-62a209f403e1)

Tout d'abord, notez que les requêtes DNS envoyées à SRV2 sont toujours fonctionnelles. SRV2 a résolu le nom de domaine « srv1 » et a renvoyé l'adresse 10.10.2.20 à PC1. PC1 envoie ensuite cinq messages de requête d'écho à SRV1, mais R1 abandonne les paquets et renvoie des messages ICMP de destination inaccessible (`Unreachable (U)`) à PC1 en raison de l'instruction de refus dans la liste de contrôle d'accès.

Sur PC2, on a : 

![image](https://github.com/user-attachments/assets/d3c8cf91-e474-4867-bac8-09569c106471)

La connectivité à SRV1 à partir d'autres sources fonctionne toujours en raison de la déclaration `permit any` dans l'ACL.

Sur R1 : 

![image](https://github.com/user-attachments/assets/1bff3f21-def9-4e26-94bc-d7a5882b2d30)

Vous devriez voir des correspondances pour les deux entrées de contrôle d'accès (ACE) car les paquets de PC1 ont été refusés et les paquets de PC2 ont été autorisés via l'ACL.


#### 5. Etape 5 

En tant qu'administrateur réseau, il est important de disposer d'un accès à distance à vos routeurs, commutateurs et pare-feu. Cet accès ne doit pas être accessible aux autres utilisateurs du réseau. Par conséquent, vous allez configurer et appliquer une liste de contrôle d'accès standard nommée qui autorise PC1 à accéder aux lignes vty sur R1, mais refuse explicitement toutes les autres adresses IP sources. Utilisez ce mot-clé `log` pour générer un message syslog sur la console lorsqu'une tentative de connexion est bloquée.

![image](https://github.com/user-attachments/assets/dd5b1b59-ee39-49f8-8172-d4dfa538bb1e)

Une liste de contrôle d'accès standard nommée, appelée VTY-ACCESS, est configurée avec une instruction `permit` pour l'adresse IP du PC1 et une instruction explicite `deny any` permettant d'appliquer le mot-clé `log`. Lors des tests, le premier paquet d'un flux déclenche un message Syslog. Les listes de contrôle d'accès avec journalisation fournissent un aperçu du trafic traversant le réseau ou abandonné par les périphériques réseau. Malheureusement, la journalisation des listes de contrôle d'accès peut être gourmande en ressources processeur et impacter négativement d'autres fonctions du périphérique réseau. Deux facteurs principaux contribuent à l'augmentation de la charge processeur due à la journalisation des listes de contrôle d'accès : la commutation de processus des paquets correspondant aux entrées de contrôle d'accès (ACE) avec journalisation, et la génération et la transmission des messages de journal. L'utilisation de l'option de journalisation sur un réseau de production doit être prudente.

L'ACL est ensuite appliquée à toutes les lignes vty du routeur dans le sens entrant , car les tentatives de connexion seront considérées comme entrantes du point de vue du routeur.


#### 6. Etape 6

Testez la liste de contrôle d'accès (ACL) en lançant une session Telnet de PC1 vers R1. Répétez le test de PC2 vers R1. Sur R1, vérifiez les messages syslog et la correspondance des ACL.

Vous verrez le message Syslog sur R1 : `*Jul 19 04:13:17.927: %SEC-6-IPACCESSLOGNP: list VTY-ACCESS denied 0 203.0.113.40 -> 0.0.0.0, 1 packet`

![image](https://github.com/user-attachments/assets/8f774b35-97c4-4992-b30f-c4e9c45667b3)

La connexion Telnet de PC1 à R1 réussit, mais pas celle de PC2. En raison du mot-clé `log`, un message syslog est déclenché par l'instruction explicite `deny any` à la fin de l'ACL. Ce message confirme la source de la tentative de connexion échouée (PC2). La commande `show access-list VTY-ACCESS` indique des correspondances pour les deux ACE, car les paquets ont été autorisés depuis PC1 et refusés depuis PC2.

Si une granularité plus poussée est requise, vous pouvez utiliser une liste de contrôle d'accès étendue. Ces listes permettent de filtrer le trafic en fonction de critères autres que l'adresse source. Elles peuvent filtrer le trafic en fonction du protocole, des adresses IP source et de destination, ainsi que des numéros de port source et de destination.


#### 7. Etape 7

Dans cette étape, vous allez configurer une ACL étendue nommée sur R1 qui agira comme un filtre de trafic pour les demandes ou les réponses provenant de l'espace d'adressage IP public, affiché à droite dans la carte topologique, et qui sont destinées à l'espace d'adressage IP privé à gauche de la carte topologique.

La liste d'accès sera appliquée sur R1 à l'interface Ethernet 0/3 dans le sens entrant avec les objectifs suivants à l'esprit :

Autoriser l'accès HTTP et HTTPS à SRV1 pour tous les appareils publics

Autoriser les réponses ping au réseau 10.10.1.0/24 à partir de n'importe quel appareil public

Autoriser les réponses DNS au réseau 10.10.1.0/24 depuis SRV2

Autoriser les réponses HTTP et HTTPS au réseau 10.10.1.0/24 à partir de n'importe quel appareil public

Refuser explicitement tout autre trafic

Sur R1, entrez les commandes suivantes : 


`R1(config)# ip access-list extended TRAFFIC-FILTER`

`R1(config-ext-nacl)# permit tcp any host 10.10.2.20 eq www`

`R1(config-ext-nacl)# permit tcp any host 10.10.2.20 eq 443`

`R1(config-ext-nacl)# permit icmp any 10.10.1.0 0.0.0.255 echo-reply`

`R1(config-ext-nacl)# permit udp host 203.0.113.30 eq domain 10.10.1.0 0.0.0.255`

`R1(config-ext-nacl)# permit tcp any eq www 10.10.1.0 0.0.0.255 established`

`R1(config-ext-nacl)# permit tcp any eq 443 10.10.1.0 0.0.0.255 established `

`R1(config-ext-nacl)# deny ip any any`

`R1(config-ext-nacl)# exit`

`R1(config)# interface Ethernet 0/3`

`R1(config-if)# ip access-group TRAFFIC-FILTER in`


Une ACL étendue nommée appelée TRAFFIC-FILTER est configurée avec les ACE suivants :

- Autoriser l'accès HTTP de tout appareil public à SRV1

- Autoriser l'accès HTTPS de tout appareil public à SRV1

- Autoriser les réponses ping ICMP au réseau PC1 à partir de n'importe quelle source publique

- Autoriser les réponses DNS au réseau PC1 à partir du serveur DNS SRV2

- Autoriser les réponses HTTP au réseau PC1 à partir de n'importe quelle source publique

- Autoriser les réponses HTTPS au réseau PC1 à partir de n'importe quelle source publique

- Refuser tout autre trafic


Notez l'utilisation de ce paramètre `established` pour deux des ACE. Ce paramètre `established`autorise uniquement les réponses au trafic provenant du réseau 10.10.1.0/24 à revenir vers ce réseau depuis un serveur public HTTP ou HTTPS. Une correspondance est établie si le segment TCP renvoyé possède les bits ACK ou de réinitialisation (RST) activés, ce qui indique que le paquet appartient à une connexion préétablie. Sans ce paramètre `established` dans l'instruction ACL, les clients pourraient envoyer du trafic à un serveur web, mais pas recevoir le trafic renvoyé par ce serveur.

Le mot-clé established dans une entrée de liste de contrôle d'accès (ACL) Cisco sert uniquement à autoriser le trafic retournant vers votre réseau interne, en se basant sur les indicateurs TCP de la réponse. Voici l’idée en trois points :

Détection des paquets « retour »

Lorsqu’un client (ex. 10.10.1.5) initie une connexion TCP vers un serveur public HTTP (port 80), il envoie d’abord un paquet avec le drapeau SYN.

Le serveur répond avec un paquet ayant les drapeaux SYN+ACK (pour confirmer la connexion) puis le client renvoie un ACK final (§ trois-voies TCP).

established regarde si le paquet entrant possède le bit ACK ou RST activé : c’est la preuve que c’est bien une réponse à une connexion déjà initiée. 
 
Pourquoi on en a besoin :

Sans le mot-clé established, une ACL ne distinguerait pas un SYN normal (nouvelle connexion) d’un paquet ACK (réponse).

Résultat : vous pourriez envoyer vos requêtes HTTP, mais vos réponses (SYN+ACK puis ACK) seraient bloquées car elles ne correspondent pas à une règle permit tcp … established.

En gros , Une ACL Cisco est `stateless`, c’est-à-dire qu’elle examine chaque paquet indépendamment sans mémoriser l’état de la connexion .

`permit tcp any host 10.10.2.20 eq 443` autorise bien les paquets entrant dont le port de destination est 443 (le SYN initial) mais bloque les paquets de réponse (source 443, destination port éphémère) car ils ne correspondent plus au critère eq 443. Sans une règle dédiée ou sans le mot-clé `established`, les réponses (avec bits ACK/RST) sont rejetées par le « implicit deny » en fin de liste

Vous pouvez effectivement autoriser le trafic de retour HTTPS en ajoutant simplement la règle inverse permit tcp any eq 443 10.10.1.0 0.0.0.255, qui matche le trafic dont le port source est 443 vers votre réseau 10.10.1.0/24 
Cisco. Cette configuration permet bien aux réponses du serveur HTTPS d’atteindre vos hôtes internes, mais elle reste stateless : elle n’inspecte pas les indicateurs TCP (SYN, ACK, RST) et peut donc laisser passer des connexions non sollicitées provenant de n’importe quel hôte utilisant le port source 443. En comparaison, le mot-clé established restreint les paquets entrants à ceux portant les bits ACK ou RST, prouvant qu’ils font partie d’une session déjà initiée, tout en simplifiant la liste sans créer de deuxième règle inverse 

Donc c'est mieux d'utiliser le mot clé established ainsi on est sûr que la réponse autorisée vient bien pour une connexion qui a déjà été initiée.

#### 8. Etape 8 

Modifiez la liste de contrôle d'accès (ACL) « TRAFFIC-FILTER » sur R1 pour autoriser l'accès SSH de PC2 à SRV1. Insérez cette nouvelle ACE à la ligne 65 de la liste de contrôle d'accès.


`R1(config)# ip access-list extended TRAFFIC-FILTER`
`R1(config-ext-nacl)# 65 permit tcp host 203.0.113.40 host 10.10.2.20 eq 22`

Les numéros de séquence permettent de supprimer ou d'insérer une instruction ACL. Cette commande `ip access-list extended name` permet d'accéder au mode de configuration ACL nommée. Si l'ACL est numérotée et non nommée, le numéro d'ACL est utilisé pour le paramètre nom. Les ACE peuvent être insérées ou supprimées à l'aide du mot-clé `no`. Par exemple, en mode de configuration ACL nommée, la commande `no 40` supprime la ligne 40 de l'ACL.




