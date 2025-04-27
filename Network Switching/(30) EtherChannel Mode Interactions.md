## I. INTRODUCTUON A ETHERCHANNEL

Etherchannel, c'est une méthode qui permet de regrouper plusieurs liens physiques entre 2 équipements (Switches ou routeurs) pour fournir un seul canal logique. Cela offre : 

- Augmentation de la bande passante (Les débits se cumulent)

- Redondance (Si un lien tombe en panne, les autres continuent de fonctionner)

- Simplification de la gestion (le lien agrégé apparaît comme une seule interface)

Pour établir un Etherchannel, on peut utiliser 3 mécanismes : 

  - LACP (Link Aggregation Protocol)
  - PAgP (Port Aggregation Protocol)
  - Configuration statique (mode 'on')

![image](https://github.com/user-attachments/assets/97279fda-d885-4e84-b366-2785c52d7a77)


## II. LACP

LACP est protocole standard `IEEE 802.3ad` ce qui signifie qu'il fonctionne aussi bien sur des équipements de différents vendeurs. Les ports compatibles LACP s'échangent des paquets LACP pour négocier automatiquement la formation du canal.

LACP vérifie que chaque port a des configurations compatibles.

Une fois que le groupe est formé, jusqu'à 16 liens sur certaines plate-formes peuvent être configurés, mais généralement, `8 sont actifs en même temps`.

Les liens non acifs sont en `standby` et se mettent en service si l'un des liens actifs tombe en panne.

Mode de fonctionnement de LACP : 

- `Active` : Le port envoie des paquets LACP sans attendre.

- `Passive` : Le port n'envoie des paquets LACP que s'il détecte que son voisin est en mode active (en gros, il repon aux demandes)


## III. PAgP

PAgP est `protocole propriétaire de Cisco`. Comme LACP, PAgP négocie la formation d'un canal en echangeant des paquets entre les ports.

Les ports doivent être configurés avec des paramètres identiques (Vitesses, duplex, VLANs, etc...) pour se regrouper.

Une fois agrégés, l'ensemble du groupe apparaît dans le réseau comme une seule interface logique

Modes de fonctionnement de PAgP : 

- `Desirable` : Le port envoie des messages PAgP de manière proactive (il démarre la négociation sans attendre)

- `Auto` : Le port attend les messages de négociation venant d'un port configuré en désirable

## IV. CONFIGURATION STATIQUE

En mode statique, on configure manuellement les ports pour les inclures dans un Etherchannel, sans négociation.

Aucun échange de paquets de contrôle n'a lieu

Les ports sont immédiatement agrégés, même si le port du côté apposé n'est pas configuré pour l'agrégation.

L'avantage est qu'il n'y a pas de délai dû aux négociations.

Mais, il n'y a aucune vérification automatique de la cohérence de la configuration. Risque d'erreurs de configuration entre les 2 extrémités qui peuvent provoquer des problèmes de connectivité.
