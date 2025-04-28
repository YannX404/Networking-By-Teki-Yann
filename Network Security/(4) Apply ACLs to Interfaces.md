Une fois que vous avez configuré une ACL, elle ne prend effet que lorsque vous l'associez à une interface à l'aide de la commande : `ip access-group`.

![image](https://github.com/user-attachments/assets/491f3516-45f9-4e76-8a07-db8f59783578)

Direction de l'ACL : 

![image](https://github.com/user-attachments/assets/ae036986-2be9-4a94-928b-516dfc424b32)

- `Entrante (inbound)` : L'ACL est appliquée avant le routage. Les paquets sont filtrés dès leur arrivée sur l'interface. Cela permet de bloquer du trafic non désiré dès le début, économisant ainsi les ressources en évitant des recherches de routage inutiles.

![image](https://github.com/user-attachments/assets/16b63d18-3f7d-4047-ad03-39be757aa541)

- `Sortante (outbound)` : L'ACL est appliquée après le routage, juste avant que les paquets ne quittent l'interface. Ici, les paquets ont déjà été routés, et l'ACL contrôle leur sortie.

Une seule ACL par protocole, par direction et par interface est autorisée.

NB : `Pour les ACLs Etendues, il est préférable de les appliquer en entrée sur l'interface la plus proche de la source.`
