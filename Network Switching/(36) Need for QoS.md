## I. POURQUOI METTRE EN PLACE LE QoS ?

  #### 1. Gérer la saturation

Sur les liens, la bande passante peut se remplir. Sans QoS, tous les paquets sont traités de la même façon. Mais avec le QoS, on retarde ou on supprime d’abord les flux peu importants pour faire passer les flux critiques.

  #### 2. Prioriser l’expérience utilisateur

Certaines données (voix, visioconf) sont sensibles à la latence et aux pertes. D’autres données (transfert de fichiers en batch) tolèrent les délais. `Le QoS garantit que les sessions sensibles bénéficient toujours d’assez de ressources.`

## II. LES GRANDES FAMILLES D’OUTILS QoS

![image](https://github.com/user-attachments/assets/bc5d0c3c-7365-4b1f-9af3-080d22a76edd)

  #### 1. Classification et marquage

On analyse le trafic dès l’entrée du réseau pour déterminer sa classe (voix, vidéo, data…). On ajoute un marquage (DSCP, CoS) aux paquets pour indiquer le traitement à appliquer. `On évite de reclasser trop souvent (Imaginez que pour chaque paquet que chaque équipement reçoit, il doit refaire ce processus) : le marquage reste jusqu’à la sortie du réseau.`. 

  #### 2. Policing, shaping et remarquage

- `Policing` : si un flux dépasse son quota, on jette immédiatement les paquets excédentaires.

- `Shaping` : on met les paquets en file d’attente pour lisser leur débit et respecter le profil et éviter une congestion.

- `Remarking` : on change le marquage des paquets trop gourmands pour leur donner une priorité plus basse.

  #### 3. Gestion de la congestion (scheduling)

Quand plusieurs classes arrivent en même temps, on les met en files d’attente séparées. Les files de haute priorité sont servies en premier. Si les tampons sont pleins, on supprime d’abord les paquets des classes moins sensibles.

  #### 4. Outils spécifiques aux liens

Certains liens (par exemple des liaisons WAN) peuvent ajouter des mécanismes comme la fragmentation ou le compresseur QoS pour optimiser l’usage limité.

## III. LE CONCEPT DE BORDURE DE CONFIANCE (TRUST BOUNDARY)

![image](https://github.com/user-attachments/assets/e711f799-594f-45a2-aaa7-7cada44e9e05)

  #### 1. Domaine non fiable (Untrusted Domain)

Ce sont les appareils hors de votre contrôle (PC utilisateurs, imprimantes…). Les marquages qu’ils émettent ne sont pas considérés.

  #### 2. Bordure de confiance (Trust Boundary)

Point où l’on vérifie, supprime ou remplace le marquage. Exemple : port d’accès d’un téléphone IP ou liaison avec un fournisseur WAN.

  #### 3. Domaine fiable (Trust Domain)

Ensemble des équipements gérés par l’administrateur (switches, routeurs), où l’on respecte les marquages validés à la bordure de confiance et où s’appliquent les règles QoS internes (filtrage, mise en forme, ordonnancement). 
