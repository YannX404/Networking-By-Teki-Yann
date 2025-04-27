## I. HSRP INTERFACE FEATURES

Même si R1 et R2 sont configurés pour faire du HSRP, il peut arrivér qu'un routeur (disons R2) devienne actif, mais qu'un autre problème, comme la défaillance d'un lien non HSRP (par exemple un uplink) survienne.

![image](https://github.com/user-attachments/assets/5d577f07-cfb3-436a-97e0-89a81beb49d6)

Sans interface `tracking`, R2 resterait actif même si son uplink est en panne, ce qui peut forcer le trafic à emprunter un chemin sous optimal (par exemple, passer par R2 et revenir à R1 pour sortir).

Avec l'interface tracking, tu peux configurer R2 pour surveiller une interface (ou un objet) spécifique.

Si cette interface tombe en panne, la priorité de R2 est réduite d'une valeur configurable (`par défaut c'est 10`).

Si la priorité chute suffisamment, R1, qui a une priorité plus élevée prendra le relais grâce à la préemption.

![image](https://github.com/user-attachments/assets/5c91dd25-d8f6-42b4-bac2-475f189c9b6c)

Ici, si l'interface Ethernet 0/0 de R2 tombe, sa priorité diminue de 20, ce qui peut déclancher un basculement si R1 a une priorité supérieure.


## II. HSRP MULTIGROUP (MHSRP) POUR LE LOAD-SHARING


Par défaut, un seul routeur devient actif pour un groupe HSRP donné. Dans un réseau avec plusieurs VLANs sur un même switch, ça peut conduire à ce que le même routeur prenne tout le trafic, saturant un seul lien.

Tu peux configurer plusieurs groupes HSRP sur la même interface, chacun associé à un VLAN différent. Par exemple, sur VLAN 10, le switch 1 peut être actif et sur VLAN 20, switche 2 peut être actif. Ainsi, tu partages le trafic entre plusieurs routeurs, optimisant l'utilisation des liens vers le coeur du réseau.

![image](https://github.com/user-attachments/assets/a85019bf-7518-4972-af39-55285c519edf)

![image](https://github.com/user-attachments/assets/0fcc6809-df48-4b54-b36c-0cb1e5841f7e)



## III. HSRP AUTHENTICATION

La sécurité est importante. Sans authentification, un routeur malveillant pourrait envoyer de faux messages HSRP, prendre le rôle actif, et pretuber le trafic.

Nous avons `2 types d'authentification` : 

- `Plaintext` : Simple, mais la clé est envoyée en clair ( limité à 8 caractères ) -> Peu sécurisée. La syntaxe est : `Switch(config-if)# standby group authentication string`

- `MD5` : Préférée. La clé est utilisée pour générer un **hash** que les routeurs comparent, sans échanger la clé en clair. La syntaxe est : `Switch(config-if)# standby group authentication md5 key-string [0 | 7] string`

Pour MD5, vous pouvez aussi utiliser `key chain` : 

![image](https://github.com/user-attachments/assets/9ea74c7c-9a75-4f22-bfc1-07798f06ad14)


## IV. HSRP VERSIONS

  ### 1. HSRPv1

- Conçu pour IPV4

- Groupes de 0 à 255

- Vitual MAC : 0000.0c07.acXX

- Multicast pour les messages Hello : `224.0.0.2`

  ### 2. HSRPv2

- Compatible avec IPV4 et IPV6

- Groupe de 0 à 4095

- Virtual MAC : 0000.0c9f.fXXX

- Multicast pour Hello : `224.0.0.102`

- Doit être configuré sur tous les routeurs du groupe sinon ça ne fonctionne pas.
