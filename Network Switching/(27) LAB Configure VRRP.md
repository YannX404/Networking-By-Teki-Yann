## I. CONFIGURER VRRP SUR LES ROUTEURS

![image](https://github.com/user-attachments/assets/75266ba6-0f4a-4168-ace6-b9fd6e684cdd)

Le maître est le seul périphérique à envoyer des annonces (analogues aux messages Hello HSRP). Ces annonces sont envoyées à l'adresse multicast `224.0.0.18`, avec le numéro de protocole 112. L'intervalle d'annonce par défaut est de 1 seconde. Le holdtime par défaut est de 3 secondes. En comparaison, le délai de Hello par défaut est de 3 secondes et le holdtime de 10 secondes.

### 1. Etape 1

Configurez Ethernet 0/1 sur R1 avec l'adresse IP 192.168.1.3 et l'adresse IP virtuelle VRRP 192.168.1.1.

![image](https://github.com/user-attachments/assets/05c47813-37b0-4a2c-84ca-0a21686d34fe)
![image](https://github.com/user-attachments/assets/f3ccd36d-23c6-42dc-a019-26d9e4003321)

Comme HSRP, VRRP utilise le concept d'adresse IP virtuelle pour fournir aux appareils des utilisateurs finaux une connectivité redondante de premier saut. L'adresse IP virtuelle est configurée à l'aide de la commande `vrrp group_number ip virtual_ip` de configuration d'interface.

`Vous pouvez utiliser l'une des adresses IP réelles des routeurs physiques comme adresse IP virtuelle. Dans cet exemple, vous pourriez utiliser 192.168.1.3 comme adresse IP virtuelle.`

Observez le message de la console et notez que, comme il n’y a pas d’autre routeur VRRP actif dans le domaine de diffusion, R1 passe à l’état Master.

### 2. Etape 2

Configurez Ethernet 0/1 sur R2 avec l'adresse IP 192.168.1.2 et l'adresse IP virtuelle VRRP 192.168.1.1.

![image](https://github.com/user-attachments/assets/8a483814-e175-4605-ab28-9a454de8619f)

Vous verrez les messages suivants : 
`*Jul 24 20:32:19.192: %VRRP-6-STATECHANGE: Et0/1 Grp 1 state Init -> Backup`
`*Jul 24 20:32:19.197: %VRRP-6-STATECHANGE: Et0/1 Grp 1 state Init -> Backup`

Observez le message de la console et remarquez que R1 est déjà actif et que le maître R2 reste dans l'état « Backup ».

`Avec HSRP, vous pouvez omettre le numéro de groupe lors de la configuration ; la valeur par défaut sera alors le groupe 0. Avec VRRP, cette valeur par défaut n'existe pas. Vous devez spécifier un numéro de groupe, compris entre 1 et 255.`

### 3. Etape 3

Configurez Ethernet 0/1 sur R2 avec une priorité VRRP de 110.

![image](https://github.com/user-attachments/assets/7973b75f-0961-43a4-b859-cf6db8f3c844)

Vous verrez sur R2 le messages : `*Jul 24 20:35:50.804: %VRRP-6-STATECHANGE: Et0/1 Grp 1 state Backup -> Master`

Et sur R1 le message : `*Jul 24 20:35:50.804: %VRRP-6-STATECHANGE: Et0/1 Grp 1 state Master -> Backup`

Dans les CLI des routeurs, vous avez observé des messages de console indiquant que R2 est désormais passé à l'état maître et R1 à l'état de sauvegarde.

Une priorité plus élevée est configurée sur un périphérique qui doit être le maître du groupe VRRP. Dans cet exemple, vous avez configuré R2 avec une priorité de 110. R1 conserve la priorité par défaut de 100.

`Cependant, si vous utilisez l'une des adresses IP du routeur comme adresse IP virtuelle, les priorités sont ignorées pour l'élection du maître. Le routeur dont l'adresse IP correspond à l'adresse IP virtuelle deviendra maître.`

`VRRP a la préemption activée par défaut, tandis que HSRP a la préemption désactivée par défaut.`

### 4. Etape 4

Sur les appareils compatibles VRRP, vérifiez l’état VRRP.

![image](https://github.com/user-attachments/assets/5b382627-b3dd-482c-9e85-262d0948eee4)

Pour vérifier l'état VRRP, utilisez la commande `show vrrp`. En ajoutant le mot-clé `brief`, vous obtiendrez une vue plus condensée.

## II. CONFIGURER L'AUTHENTIFICATION VRRP

La norme VRRP utilisée pour spécifier l'authentification en clair et MD5 a été révoquée ultérieurement. Cependant, les périphériques Cisco IOS prennent toujours en charge les mécanismes d'authentification.

VRRP utilise l'authentification en texte clair et MD5 avec RFC 2338.

`Les RFC 3768 et RFC 5798 suppriment la prise en charge de l'authentification pour VRRP.`

Le logiciel Cisco IOS prend toujours en charge les mécanismes d’authentification RFC 2338.

Selon la RFC 5798, l'expérience opérationnelle et des analyses plus poussées ont montré que l'authentification VRRP n'offrait pas une sécurité suffisante pour surmonter la vulnérabilité des secrets mal configurés, entraînant l'élection de plusieurs maîtres. De par la nature du protocole VRRP, même si les messages VRRP sont protégés par chiffrement, cela n'empêche pas les nœuds hostiles de se comporter comme s'ils étaient le maître VRRP, créant ainsi plusieurs maîtres. L'authentification des messages VRRP aurait pu empêcher un nœud hostile de provoquer le basculement de tous les routeurs en état de secours. Cependant, la présence de plusieurs maîtres peut entraîner autant de perturbations que l'absence de routeurs, ce que l'authentification ne peut empêcher. De plus, même si un nœud hostile ne pouvait pas perturber VRRP, il pourrait perturber ARP et produire le même effet que le basculement de tous les routeurs en état de secours.

Indépendamment de tout type d'authentification, VRRP inclut un mécanisme (définition d'une durée de vie [TTL] de 255, vérification à réception) qui protège contre l'injection de paquets VRRP depuis un autre réseau distant. Ce paramètre limite la vulnérabilité aux attaques locales.

Avec les périphériques Cisco IOS, l'authentification VRRP par défaut est en texte brut. L'authentification MD5 peut être configurée en spécifiant une key string ou, comme avec HSRP, en faisant référence à un Key-chain.

### 5. Etape 5

Configurez l’authentification MD5 pour VRRP sur l’interface Ethernet 0/1 de R1.

![image](https://github.com/user-attachments/assets/78823ab5-4479-4781-a71f-6a6114195e6e)

Vous devez avoir un message comme celui-ci : `%VRRP-4-BADAUTHTYPE: Bad authentication from 192.168.1.2, group 1, type 0, expected 254.`

Dans la sortie CLI de R1, notez le message « bad authentication ». R1 est actuellement configuré avec l'authentification MD5, tandis que R2 n'a pas d'authentification VRRP configurée. Par conséquent, les routeurs ne se considèrent pas comme membres du même groupe. Si vous vérifiez l'état VRRP sur les deux périphériques, vous constaterez qu'ils se considèrent tous deux comme maîtres du groupe VRRP 1.

### 6. Etape 6

Configurez l’authentification MD5 pour VRRP sur l’interface Ethernet 0/1 de R2.

![image](https://github.com/user-attachments/assets/3714b86d-f7d4-43b2-9214-b17c730710cf)

Notez que maintenant que vous avez configuré les authentifications MD5 VRRP correspondantes, vous obtenez un message dans la sortie CLI de R1 indiquant que R1 passe à l'état de Backup : `%VRRP-6-STATECHANGE: Et0/1 Grp 1 state Master -> Backup`

### 7. Etape 7

Vérifiez les états VRRP et la méthode d’authentification de R1 et R2.

![image](https://github.com/user-attachments/assets/dbdd47d8-48ee-418b-b541-a9c68f88a487)








