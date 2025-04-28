![image](https://github.com/user-attachments/assets/630f9112-624f-4c3c-adcd-d2e9fa70452c)

Pour restreindre l'accès Internet pour PC2, vous devez empêcher que les paquets émis par PC2 n'atteignet l'interface connectée à Internet. Cela se fait généralement en configurant une ACL qui bloque le trafic provenant de PC2, en fonction de son adresse IP source.

L'ACL va filtrer le trafic en vérifiant l'adresse IP source. Vous devez donc connaître l'adresse IP attribuée à PC2.

Si PC2 a l'adresse 192.168.1.2, l'ACL devra spécifier cette adresse.

Vous pouvez utiliser une ACL standard, qui se base uniquement sur l'adresse source : 

`Router(config)# access-list 10 deny 192.168.1.2 0.0.0.0`
`Router(config)# access-list 10 permit any`

Il existe 2 options quant au placement de l'ACL : 

- `En entrée (inbound)` : L'ACL est appliquée dès que les paquets entrent dans le routeur depuis le LAN. Cela bloque le trafic de PC2 avant qu'il ne soit routé vers l'interface Internet, réduisant ainsi la charge du routeur.

- `En Sortie (Outbound)` ; L'ACL est appliquée juste avant que les paquets ne quittent le routeur vers Internet. Cela permet de filtrer le trafic après le processis de routage, mais avant la transmission vers l'extérieur.


Note : `ACL Standard : Comme elles ne filtrent que l'adresse source, il est souvent conseillé de les appliquer près de la destination (Dans notre cas, sur l'interface connectée à Internet) pour éviter d'impacter d'autres flux de trafic sur le LAN.`
