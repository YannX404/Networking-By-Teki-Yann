Il existe 3 approches principales pour concevoir la couche de distribution dans un campus : 

- `Design traditionnel`

- `Design Simplifié`

- `Dsign SD-ACCESS`

## I. DESIGN TRADITIONNEL DE LA COUCHE DE DISTRIBUTION

![image](https://github.com/user-attachments/assets/ef861b87-370f-47e4-9c5e-04d19416efd1)

Ce modèle traditionnel sépare la couche d'accès de la couche de distribution. Pour la redondance, on utilise souvent 2 switches indépendants dans la distribution. 

Pour éviter les boucles, **il faut restreindre un VLAN à un seul switch d'accès (et son ou sa paire  d'uplink de distribution**. Cela donne une topologie sans boucle (`Loop-fee`) car **STP** n’a pas besoin de bloquer de port sur ce VLAN, car il n’y a pas de boucle potentielle à gérer

On utilise des protocoles FHRP pour offrir une passerelle IP redondante aux VLANs.

Dans le cas où un VLAN existe sur plusieurs switches, le STP peut bloquer certains liens pour éviter les boucles, ce qui limite la bande passante et peut allonger le temps de convergence en cas de panne.

![image](https://github.com/user-attachments/assets/99597966-195e-493e-a3c2-82d080d1ca11)

## II. DESIGN SIMPLIFIE DE LA COUCHE DE DISTRIBUTION

Au lieu d'avoir des switches séparés, on les regroupe ( par **stacking** ou **VSS** ) pour qu'ils fontionnent comme **un seul switch logique**

![image](https://github.com/user-attachments/assets/1c5e8bad-ef44-4b37-857a-45b1e383d91d)

Grâce à cela, tous les liens depuis la couche d'accès vers la distribution sont actifs (grâce à Etherchannel), ce qui élimine les problèmes de blocage STP.

Nous avons plus besoin de protocoles de redondance de passerelle (les FHRP) parce que la passerelle par défaut est sur un seul point logique.

Logiquement, le réseause comporte comme un hub central rélié à plusieurs spokes, ce qui simplifie le routage.

## III. DESIGN SD-ACCESS DE LA COUCHE DE DISTRIBUTION

Ici, tu ajoutes une couche `FABRIC` sur un réseau déjà en layer 3, et tout se gère de façon automatisée grâce à SDA (SD-Access). Tu as un réseau physique (`UNDERLAY`) sur lequeltu fais tourner des réseaux virtuels (`OVERLAY`) pour connecter les dispositifs autrement.
