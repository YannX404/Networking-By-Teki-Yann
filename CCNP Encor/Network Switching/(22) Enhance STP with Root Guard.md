![image](https://github.com/user-attachments/assets/5400b7d5-8f18-4334-969b-6226e88caa8a)

Dans un réseau fonctionnant avec STP, la stabilité et la cohérence de la topologie reposent sur le bon positionnement du root bridge. Cependant, en l'absence de mesures de protection supplémentaires, un attaquant ou un périphérique mal configuré pourrait envoyer des BPDUs avec des valeurs Bridge ID inférieures, donc meilleurs que celle du Root Bridge actuel. Ainsi, ce dispositif malveillant ou non autorisé se ferait élire Root Bridge, modifiant la topologie pour intercepter ou pertuber le trafic. Ce type d'attaque met en péril le concept clé de la sécurité qui est `la triade CIA` (Confidentiality, Integrity, Availability)

## I. FONCTIONNEMENT DE ROOT GUARD

![image](https://github.com/user-attachments/assets/50a137a2-b270-4712-b4b7-25823fe5c84f)

Root Guard est spécifiquement conçu pour protéger la position du Root Bridge en limitant les ports qui peuvent participer à l'élection de ce dernier.

Lorsqu'un port est configuré avec Root Guard et reçoit un BPDU qui indique une proposition de devenir Root Bridge, le port entre dans un état `root-inconsistent`.

Dans cet état, le port ne relaie pas de trafic de données. Ce comportement ressemble à l'état Listening de STP, ce qui empêche toute modification de la topologie jusqu'à ce que le problème soit resolu.

Une fois que les BPDUs <<Offensants>> cessent, le port peut revenir à son état normal et reprendre le rôle d'interface de communication dans le réseau.


## II. MISE EN OEUVRE DE ROOT GUARD

`Root Guard doit être activé sur les ports qui se connectent à des switches ou à des dispositifs qui ne doivent pas être élus root bridge.` Par exemple, `dans une topologie hiérarchique, seuls les switches de niveau supérieurs devraient être candidats pour le root bridge, tandis que les ports vers les périphériques de niveaux inférieur devraient être protégés par Root Guard.`

Pour activer Root Guard sur une interface Cisco, on utilise la commande : `spanning-tree guard root` 


