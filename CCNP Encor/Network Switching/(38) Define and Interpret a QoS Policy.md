## I. Définir une politique QoS globale (MQC)

  #### 1. class-map

Cette commande crée une classe de trafic et spécifie les critères de correspondance (match) pour y associer les paquets 
Cisco.

`Router(config)# class-map match-any VOICE-CLASS
Router(config-cmap)# match dscp ef`

  #### 2. policy-map

Cette commande crée une politique de trafic (policy map) qui regroupe une ou plusieurs class-map et définit les actions QoS à appliquer (priorité, bande passante garantie, police, etc.).

`Router(config)# policy-map QOS-POLICY
Router(config-pmap)# class VOICE-CLASS
Router(config-pmap-c)# priority percent 30`

  #### 3. service-policy
  
Cette commande attache la policy-map à une interface en entrée ou en sortie, rendant la QoS active sur ce lien 
Cisco.

`Router(config)# interface Serial0/0/0
Router(config-if)# service-policy output QOS-POLICY`

## II. Méthodes de mise en œuvre d’une politique QoS

  #### 1. Legacy CLI

La configuration se fait entièrement à la main sur chaque interface et chaque classe, sans modularité. C’est lent et sujet aux erreurs.

  #### 2. Modular QoS CLI (MQC)
  
MQC offre une structure modulaire : class-map → policy-map → service-policy, réutilisable sur plusieurs interfaces sans duplication.

  #### 3. Cisco AutoQoS
  
AutoQoS génère automatiquement les class-maps, policy-maps et service-policies recommandés pour la voix et la vidéo, basés sur les bonnes pratiques Cisco. Idéal pour déployer rapidement une QoS cohérente sur de nombreux équipements Cisco.

`Switch(config-if)# auto qos voip cisco-phone`

  #### 4. SDM QoS Wizard
  
L’assistant SDM (Security Device Manager) propose une interface graphique pour sélectionner les interfaces WAN et générer visuellement une politique QoS (class-map, policy-map, service-policy) Cisco.

## III. Interpréter une politique QoS

  #### 1. Identifier les classes

Chaque class-map correspond à un usage : voix (EF), signalisation (CS3), data interactive (AF21), best-effort (BE). 
Cisco

  #### 2. Analyser les actions

priority pour la voix (faible latence),

bandwidth ou police pour limiter ou garantir un débit,

queue-limit pour contrôler la taille des files.

  #### 3. Vérifier l’attachement

Confirmer que le service-policy est bien appliqué à l’interface concernée (input/output).

Utiliser `show policy-map interface <if>` pour inspecter les compteurs de chaque classe.

En pratique, pour un lien WAN à faible bande passante :

**class-map pour VoIP → match dscp ef**

**policy-map → priority percent 30 (garantit 30 % du lien)**

**service-policy output sur Serial0/0/0**

Cette configuration assure que le trafic voix bénéficie toujours de la priorité et de la bande passante nécessaire, tandis que les autres flux sont mis en file ou éventuellement policed en cas de congestion.
