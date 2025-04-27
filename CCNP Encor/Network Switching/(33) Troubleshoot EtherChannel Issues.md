## I. PROBLEMES COURANTS D'ETHERCHANNEL

![image](https://github.com/user-attachments/assets/97578988-b01b-48eb-8e8f-8d28afee3f6b)

Pour qu'un Etherchannel se forme corretement, toutes les interfaces physiques qui le composent doivent avoir une configuration `identiques`. Parmi les paramètres critiques : 

- Vitesse et Duplex : Les interfaces doivent fonctionner à la même vitesse et en `full duplex`.

- Mode de Trunkin et VLAN : Si les port sont configurés en trunk, ils doivent avoir :

  - Le même mode (trunk ou access)
  - La même encapsulation (802.1Q)
  - Le même VLAN Natif (pour les trunks)
  - La même liste de VLAN autorisés
 
Pour les ports en accès, ils doivent être affectés au même VLAN.

Tous les ports d'un Etherchannel doivent opérer sur la même couche. Par exemple, pour un Etherchannel de couche 3, il faut désactiver le mode `switchport (<<no switchport>>)` sur toutes les interfaces.

Les interfaces doivent être configurées avec le même protocole pour la formation de l'Etherchannel, c'et à dire : 

- LACP : Les 2 côtés doivent être en mode active ou passive

- PAgp : Les ports doivent être en mode auto ou desirable

- Statique (mode on) : Les 2 côtés doivent être configurés en mode 'on'. Sinon, si un côté est configuré en 'on' et l'autre ne l'est pas, les ports seront traités individuellement. Cela peut provoquer une situation où le switch non configuré pour l'agrégatio, voit les liens comme indépendants, entraînant des liens comme indépendants, entraînant ainsi des boucles.

## II. OUTILS ET COMMANDES POUR LE DEPANNAGE

  #### 1. Vérifier l'état de l'Etherchannel

`show etherchannel summary`

Les liens opérationnel sont marqués par `p` et les liens suspendu sont marqués par `s`.

  #### 2. Détails des interfaces

Pour examiner en profondeur un groupe etherchannel, utilise : `show etherchannel group <number> detail`.

  #### 3. Pour les paramètres de configuration physique

Pour les paramètres vitesse, duplex et trunking, utilise : 

`show interfaces <interface>`
`show running-config interface <interface>`
`show <interface> switchport`

  #### 4. Vérifier la charge et la répartition

Pour vérifier quel algorithme de répartition est utilisé et les statistiques de trafic, utilise :

`show etherchannel load-balance`
`sjoz etherchannel trafic`


## III. ETHERCHANNEL GUARD

L'Etherchannel Guard est `activé par défaut` et sert à détecter les incohérences dans les paramètres de configuration entre les 2 côtés du lien.

Qi le switch reçoit des BPDUs provenant de différentes addresses MAC sur un même bundle (Etherchannel), il en déduit que le voisin ne regroupe pas correctement ses liens.

Dans ce cas, le port est mis dans un état `err-disable`.

Pour vérifier que l'etherchannel Guard est activé, utilise : 

![image](https://github.com/user-attachments/assets/ef5dc87e-e158-4d10-be84-82cead7e388d)


 
