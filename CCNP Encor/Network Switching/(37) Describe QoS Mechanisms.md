## I. Classification et marquage

  #### 1. Classifier

Inspecte chaque paquet pour décider à quel type de trafic il appartient (voix, vidéo, data, etc.). Se fait idéalement à l’entrée du réseau (au plus près du bord de confiance).

  #### 2. Marker

Écrit un « label » dans l’en-tête du paquet (DSCP, CoS, 802.1p…). Préserve la décision du classifier, pour que tous les équipements suivants sachent comment traiter ce paquet sans devoir reclassifier.

## II. Policing, Shaping et re-marquage

  #### 1. Policing

Vérifie le débit d’un flux par rapport à un contrat (quota). Si le flux dépasse sa limite, on jette ou on abaisse le marquage des paquets en excès. Pas d’attente : la décision est instantanée, idéal au point d’entrée.

  #### 2. Shaping

Lisse le trafic en retardant les paquets pour que le débit global n’excède jamais la limite. Stocke les paquets excédentaires dans un tampon et les envoie dès que possible. Utile sur les liens vers un fournisseur pour respecter les SLA.

  #### 3. Re-marquage

Change la valeur DSCP/CoS d’un paquet trop gourmand pour le placer dans une classe moins prioritaire.

## III. Gestion de la congestion et Scheduling

  #### 1. Files d’attente (Queuing)

Sépare les paquets en files selon leur classe (voix, vidéo, data…). Les flux sensibles attendent dans des files à haute priorité.

  #### 2. Scheduling

Le scheduling, c'est le processus qui décide quel paquet de quelle file doit être envoyé en premier.

`Strict Priority` : on vide d’abord la file la plus prioritaire, risque de « famine » pour les autres.

`Round-Robin` : on envoie un paquet de chaque file à tour de rôle, garantit un service minimum pour toutes.

`Weighted Fair (WFQ)` : Sur Cisco IOS, Weighted Fair Queuing (WFQ) attribue à chaque flux un poids égal (chaque flux est “équally weighted”), donc chaque file reçoit la même part” de la bande passante disponible (débit du lien ÷ nombre de flux). Formellement, si la capacité du lien est R et qu’il y a N flux, chacun reçoit `R ÷ N` tant que tous les poids sont identiques (cas par défaut). Poids plus élevé → part plus grande selon la formule pondérée. 

  #### 3. CBWFQ (Class-Based WFQ)

Assure un quota minimum de bande passante à chaque classe. Pas de garantie de latence, plutôt conçu pour le trafic data.

  #### 4. LLQ (Low-Latency Queuing)

CBWFQ + une file à priorité stricte en tête (pour voix/vidéo). Combine garanties de bande passante et faible latence pour les flux temps réel.


## IV. Évitement de la congestion (Congestion Avoidance)

  #### 1. Tail Drop

Quand les files sont pleines, on jette simplement les derniers paquets arrivés. Peut provoquer une « `synchronisation TCP` » qui réduit l’efficacité.

  #### 2. RED / WRED

- RED `Random Early Detection` : on jette aléatoirement des paquets avant que la file soit pleine pour éviter le « bouchon » d’un coup.

- WRED `Weighted RED` prend en compte la priorité des classes de trafic : les paquets critiques sont moins susceptibles d’être jetés.
