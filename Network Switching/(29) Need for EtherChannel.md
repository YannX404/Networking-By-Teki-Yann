## I. POURQUOI ETHERCHANNEL ?

![image](https://github.com/user-attachments/assets/6e9bea0c-4d1a-4f64-946c-15cca9ff600b)

Plutôt que d'acheter des liens plus rapides et plus chers, tu peux utiliser plusieurs ports existants pour créer un lien logique qui `cumule` leur capacité.

`Le trafic est reparti entre toutes les liaisons physiques`, ce qui permet d'optimiser l'utilisation de la bande passante.

Si l'un des liens physiques tombe en panne, le lien logique (`Etherchannel`) reste fonctionnel, tant qu'au moins un lien est actif. Il n'y aura pas de recalcul STP à chaque défaillance mineure.


## II. LES CARACTERISTIQUES D'ETHERCHANNEL 

- Aggregation logique : plusieurs ports physiques, `entre 2 et 8, voir 16 sur certains modèles` sont régroupés en un seul lien logique.

- Un seul lien du point de vue de STP : STP voit l'Etherchannel comme une seule connexion, donc il n'y a pas de risque de boucles sur chaque port individuel.

- Configuration simplifiée ; Tu configures l'Etherchannel sur un seul port logique, ce qui assure la cohérence et facilite la gestion.

- Compatibilité : `Tu ne peux pas mélanger différents types de ports` (par exemple, FastEthernet et GigabitEthernet) dans le même groupe.
