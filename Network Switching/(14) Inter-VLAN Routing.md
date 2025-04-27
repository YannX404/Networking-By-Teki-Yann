## I. POURQUOI L'INTER-VLAN ROUTING EST NECESSAIRE ?

![image](https://github.com/user-attachments/assets/5dbec317-2922-48d4-8483-9b86578a87e2)

Chaque VLAN représente ce qu'on appelle un **`domaine de diffusion unique`**. Par défaut, les appareils dans un VLAN ne peuvent pas communiquer avec ceux d'un autre VLAN car ils sont isolés.

Pour que les appareils de différents VLANs puissent échanger des données, ils faut faire du `routage`. Un dispositif de couche 3 comme un routeur ou un switch de couche 3 doit être utilisé pour transférer le trafic d'un VLAN vers un autre.

## II. LES OPTIONS POUR L'INTER-VLAN ROUTING

  #### 1. Option 1 : Routeur avec une interface physique par vlan

![image](https://github.com/user-attachments/assets/ca3c93a2-3499-4a1c-8d4e-eeeebb4156d3)

On connecte chaque VLAN à une interface physique distincte sur le routeur. Cela est simple à comprendre et à mettre en oeuvre pour un petit nombre de VLANs. Mais, dès que tu as beaucoup de VLANs, tu peux manquer d'interfaces physiques.

  #### 2. Option 2 : ROAS (ROUTER ON A STICK)

![image](https://github.com/user-attachments/assets/ef20f6e8-de93-478f-80b9-14f0297a7e93)

Un seul lien physique entre le routeur et le switch est configuré en mode trunk. Le routeur utilise ce qu'on appelle des **`sous-interfaces (subinterfaces)`** pour distinguer le trafic de chaque VLAN.

Chaque sous-interface est configurée avec un identifiant VLAN et une adresse IP correspondant au sous réseau du VLAN.

`Cela peut devenir un goulot d'étranglement en cas de trafic important, puisque tout le trafic passe par une seule interface physique`

  #### 3. Option 3 : Switch de couche 3

![image](https://github.com/user-attachments/assets/081a4a52-5bb7-4ec9-a973-da0a2743a956)

Un switch de couche 3 combine les fonctions de communication et de routage. Il peut acheminer le trafic entre VLANs en interne sans passer par un routeur externe. Cela est plus scalable et performant pour des environnement avec beaucoup de trafic inter-VLAN.
