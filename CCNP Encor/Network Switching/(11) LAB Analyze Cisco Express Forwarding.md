![image](https://github.com/user-attachments/assets/e4eb5d38-81a3-4d7f-9645-7302511ac9fd)

Pour afficher la FIB, on utilise la commande **`show ip cef`** : 

![image](https://github.com/user-attachments/assets/84619181-d28c-4200-8ee3-a5b0f14922c6)

Pour ce qui est de la Adjacency table, on utilise la commande : **`show adjacency`** : 

![image](https://github.com/user-attachments/assets/fa0f5529-42a3-4d16-b773-23329c79ee2b)

Notez qu'il existe une entrée dans la table FIB pour chaque réseau connu du HQ ; autrement dit, pour chaque entrée de la table de routage, il existe déjà une entrée préconfigurée. HQ n'étant actuellement configuré avec aucun protocole de routage, seuls les réseaux locaux sont présents dans la table de routage. Le routeur HQ ne dispose d'aucune information sur le réseau distant 192.168.110.0/24.

Cependant, la Adjacency table ne contient aucune entrée. Elle est construite à partir de la table ARP. Comme vous n'avez pas encore généré de trafic, la table ARP ne contient aucune entrée ; elle est donc également vide.


Lancons le trafic sur le routeur HQ vers le routeur voisin BR1 à l'aide d'une commande ping et vérifions le contenu des tables Adjacency et FIB : 

![image](https://github.com/user-attachments/assets/738166e7-34ef-452e-b4e9-2bc04a6d6c32)

Notez que le premier paquet a été perdu. Cette perte est due au fait que le routeur HQ attendait une réponse ARP de BR1, nécessaire pour finaliser une nouvelle entrée ARP sur HQ.

![image](https://github.com/user-attachments/assets/68682901-3e45-478a-bba3-a5abc9b3e753)

![image](https://github.com/user-attachments/assets/231df9db-ef9c-4a24-9dec-bbacb52956ba)

Notez que la table Adjacency a changée lorsque le routeur HQ a pris connaissance du nouvel hôte final via ARP. Par conséquent, la nouvelle entrée est également insérée dans la table FIB.


Activons EIGRP sur le routeur HQ. BR1 est déjà configuré. Une fois la contiguïté de routage activée, vérifiez que les nouvelles routes ont été reçues.

![image](https://github.com/user-attachments/assets/a34da4b1-c768-459c-9871-2ff461d3b40d)

Comme vous pouvez le voir sur la dernière ligne de la sortie, l’adjacence EIGRP est activée.

Sur HQ, entrez les commandes suivantes :

![image](https://github.com/user-attachments/assets/e9294cab-8839-43f4-8b05-1cf4487b1a04)

Vous pouvez voir que HQ a appris une nouvelle route EIGRP vers le réseau 192.168.110.0/24, le LAN sur le routeur BR1.


Si nous vérifions à nouveau les tables FIB et d’adjacence : 

![image](https://github.com/user-attachments/assets/635f6128-cb5f-41ca-9436-0df7c6b54083)

Notez que la table FIB comporte une nouvelle entrée pour le réseau 192.168.110.0/24. Cette nouvelle entrée est due au changement de la table de routage suite à l'apprentissage d'une nouvelle route via EIGRP. En revanche, la table d'adjacence est restée inchangée, ce qui était prévisible puisque la table ARP n'a pas changé.


Vérifions que Cisco Express Forwarding est activé pour l'interface Ethernet 0/0 sur le routeur HQ : 

Vous pouvez utiliser la commande **`show ip interface interface`** pour vérifier l’état Cisco Express Forwarding de l’interface particulière : 

![image](https://github.com/user-attachments/assets/26762a85-ba12-4166-ba7f-3341c91fa228)

Notez la ligne « IP CEF switching is enabled » dans la sortie, indiquant que Cisco Express Forwarding est activé sur cette interface. Par défaut, Cisco Express Forwarding pour IPv4 est activé sur toutes les interfaces avec la commande globale **`ip cef`**. Il est conseillé d'utiliser Cisco Express Forwarding autant que possible. Vous devrez peut-être désactiver Cisco Express Forwarding en cas de problème et de résolution.

NB : En revanche, **`Cisco Express Forwarding pour IPv6 n'est pas activé par défaut. Cependant, il est activé automatiquement lorsque vous activez le routage monodiffusion IPv6 (IPv6 unicast routing) sur vos appareils. Pour utiliser Cisco Express Forwarding IPv6, il est indispensable d'activer Cisco Express Forwarding IPv4`**.


Désactivons Cisco Express Forwarding sur l'interface Ethernet 0/0 du routeur HQ et vérifions que Cisco Express Forwarding est désactivé sur l'interface : 

Vous pouvez utiliser la commande **`no ip route-cache cef`** au niveau de l'interface pour désactiver Cisco Express Forwarding sur une interface particulière.

![image](https://github.com/user-attachments/assets/805469dc-0d64-4cd2-9ace-58709f74e1a1)

Notez la ligne « IP CEF switching is disabled », qui confirme que Cisco Express Forwarding est désactivé sur l'interface.


Désactivons Cisco Express Forwarding globalement sur le routeur HQ et Vérifions que Cisco Express Forwarding est désactivé globalement : 

Vous pouvez utiliser la commande globale **`no ip cef`** pour désactiver Cisco Express Forwarding sur toutes les interfaces du routeur. Cette commande **`show ip`** cef vous permet de vérifier si Cisco Express Forwarding est activé globalement : 

![image](https://github.com/user-attachments/assets/2f6244a9-4690-47ae-bd48-6eecd49904d8)










