Le plan de contrôle, c'est l'ensemble du matériel et des logiciels dans un équipement réseau qui est responsable des `fonctions de routage` et `de gestion du périphérique`. En d'autres termes, c'est la partie <<intélligente>> qui prend des décisions sur le chemin à suivre pour les paquets, met à jour à les tables de routage et gère divers protocoles de communication entre les équipements.

Le trafic du plan de contrôle est traité par `le processeur (CPU)` de l'équipement. Cela inclut les paquets destinés au routeur lui-même ou ceux qui proviennent du routeur.

Du point de vue du trafic IP, les paquets transitent dans le routeur et peuvent être classés en `4 groupes principaux` : 

- `Les paquets du Data plane` : Ce sont `les paquets générés par l'utilisateur (les hôtes finaux)` qui transitent dans le réseau. Ils sont transmis rapidement d'un équipement à un autre via des mécanismes comme CEF (Cisco Exoress Forwarding).

- `Les paquets du Control Plane` : Ce type de paquet est soit généré par le routeur lui-même, soit lui est destiné. Ils servent à établir et à maintenir le réseau. Ces paquets sont envoyés au CPU du routeur pour être traités par le routeur lui-même.

- `Les paquets du Management Plane` : Ils sont utilisés pour la gestion et la surveillance du réseau ou des équipements. Comme le Control Plane, ils sont destinés au routeur et sont traités par le CPU.

- `Les paquets du Service Plane` : Ce sont des paquets générés par les utilisateurs, comme les paquets du Data Plane, mais qui nécéssitent un traitement particulier au délà du simple transfert basé sur l'adresse IP de destination. (GRE, MPLS VPNs, SSL/IPsec, QoS)


 Du point de vue interne du routeur (c'est à dire dans le routeur), les paquets peuvent être classés en **3 catégories** : 

 - `Les paquets Transit` : Ils regroupent la majorité des paquets du data plane et certains paquets du service plane. Ces paquets suivent le chemin rapid (fast path) via des mécanismes comme CEF. Leur transfert se fait directement sans nécéssiter l'intervention du CPU pour chaque paquet.

 - `Les paquets Receive (ou punted)` : Ce sont les paquets destinés au routeur lui-même, incluant ceux du contrôle plane et du management plane. Ils ne suivent pas le chemin le plus rapide, ils sont <<punted>>, c'est à dire envoyés au CPU pour traitement approfondi.

 - `Les paquets Exception et Non-IP` :

   - `Exception IP` : Paquets IPV4 présentant des options d'en tête particulières, des paquets dont le TTL a expiré ou ceux dont la destination est inatteignable.
  
   - `Non-IP`: Paquets qui ne sont pas des paquets IP (par exemple, CDP)

   Ces paquets requièrent le traitement par le CPU du routeur via le RP (Route Processor)
