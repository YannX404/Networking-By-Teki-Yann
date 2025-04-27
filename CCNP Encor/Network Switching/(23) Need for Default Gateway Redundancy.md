## I. POURQUOI LA REDONDANCE DE LA PASSERELLE PAR DEFAUT EST NECESSAIRE ?

![image](https://github.com/user-attachments/assets/6b936957-43f0-4842-a7da-42640b9ae8c3)

Chaque hôte (Ordinateur, imprimante, serveurs, etc...) utilise une passerelle par défaut pour envoyer les paquets destinés à des réseaux situés en dehors de leur sous-réseau local. Généralement, cette passerelle est configurée `statiquement` ou apprise via `DHCP`.

La plupart des hôtes ne reçoivent pas d'informations de routage dynamique. Ils ne connaissent qu'une seule adresse IP de passerelle par défaut. Si cette passerelle tombe en pane, l'hôte est `isolé`.

Même si, en arrière plan, il existe plusieurs routeurs redondants (par exemple, Router A et Router B), l'hôte ne sait pas automatiquement qu'il doit utiliser une autre adresse en cas de défaillance du routeur configuré.

Même si le réseau interne (entre les routeurs) se reconstruit et converge rapidement pour rediriger le trafic, les hôtes eux-mêmes ne reçoivent pas les mises à jour dynamiques. Cela signifie qu'une panne de la passerelle par défaut entraîne une perte de connectivité pour tous les appareils configurés avec cette seule adresse.

Pour garantir une `haute disponibilité`, il est essentiel d'implémenter des mécanismes de redondance pour la passerelle par défaut. Cela permet de s'assurer que si un routeur ou un switch de couche 3 défaillant était celui configuré en tant que passerelle, un autre appareil redondant prendra automatiquement le relais pour maintenir la connectivité des hôtes.
