## I. CREATION D'UN VLAN

![image](https://github.com/user-attachments/assets/73eed366-93a8-4007-8afe-b171b44d0113)

Pour créer un VLAN sur un Cisco Catalyst, tu vas en mode de configuration globale et utilise la commande **`vlan`** avec **l'ID** souhaité, puis tu lui attribues un nom. Par exemple : 

![image](https://github.com/user-attachments/assets/1cb1daa5-2227-4c04-a7ec-eff03eb0c924)

NB : Les VLANs crées sont enregistrés dans un fichier appelé **`vlan.dat`** qui se trouve dans une mémoire qui est le **`flash`** du switch.

## II. AFFECTATION D'UN PORT A UN VLAN 

Ensuite, tu dois assigner un port à un VLAN :

![image](https://github.com/user-attachments/assets/61293928-4ab0-4ff4-ac94-416ed98a8dcf)

## III. VERIFICATION DE LA CONFIGURATION

NB : `Par défaut, tous les ports appartiennent au VLAN 1.`

Pour vérifier que tout est bien configuré, tu peux utiliser quelques commandes : 

![image](https://github.com/user-attachments/assets/97cb322e-557d-4653-ac80-2f88f13aaf0b)

![image](https://github.com/user-attachments/assets/41d7d07b-5411-4083-ac96-7d8f1de5fe2f)

![image](https://github.com/user-attachments/assets/5e6cbe06-6e0d-49c6-8032-08f865fd8b73)

