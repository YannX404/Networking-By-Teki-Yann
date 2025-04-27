## I. Impact des applications sur le réseau

  #### 1.Applications batch
  
Les transferts de fichiers (FTP, TFTP) démarrent, puis roulent sans intervention : la vitesse dépend de la bande passante, mais même sur un lien lent, le téléchargement finit toujours par s’achever. La latence importe peu.

  #### 2. Applications interactives
  
Chaque action de l’utilisateur (requête en base, recherche d’inventaire) génère une attente de réponse. Si le réseau est lent, tout fonctionne toujours, mais l’expérience se dégrade (« clic… attente… résultat »).

  #### 3. Applications temps réel
  
Voix et vidéo exigent un débit stable et une latence très faible : tout délai trop important ou toute perte de paquet impacte directement la qualité (pas de retransmission automatique). On les place sous QoS pour leur garantir la priorité.

## II. Champs essentiels de l’en-tête IPv4

![image](https://github.com/user-attachments/assets/68f71b8f-f890-47f8-a2ae-77efed29b84c)

Le format IPv4 s’appuie sur un en-tête structuré en plusieurs champs :

Adresse source (32 bits) : IP de l’émetteur

Adresse destination (32 bits) : IP du destinataire

Les autres champs (Version, IHL, Type de service, Longueur totale, Fragmentation, TTL, Protocole, Checksum, Options, Padding) servent à gérer la découpe, la priorité et la validité du paquet en transit.

## III. Caractéristiques des trafics

Trois grands profils coexistent sur un réseau (Types de trafic) :

  #### 1. Données
  
Trafic « par à-coups », souvent non critique (emails, transferts). On peut supporter des pics ou des retards sans compromettre la transmission.

  #### 2. Voix
  
Flux continu et régulier. Très sensible à la `latence (C’est le délai total nécessaire pour qu’un paquet voyage du point A au point B.)` et à la `gigue (C’est la variation de ces temps de latence, c’est-à-dire la différence de délai entre paquets successifs. Si deux paquets arrivent successivement à 30 ms et 50 ms, la gigue instantanée est +20 ms. Une gigue élevée signifie que les paquets n’arrivent pas régulièrement)`, chaque paquet a une fenêtre de temps très stricte.

  #### 3. Vidéo

Streaming : tolère un léger buffering (jusqu’à quelques secondes) et quelques pertes (< 5 %).

Visioconférence : mêmes exigences que la voix + besoin de plus de bande passante.

En plus de ces trois catégories, n’oubliez pas `le trafic de contrôle du réseau` (protocoles de routage, SNMP, etc.), qu’il faut aussi inclure dans vos plans de QoS pour éviter qu’un pic de mise à jour n’interfère avec les flux utilisateurs.
