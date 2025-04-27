Détails des fonctionnalités

- `RPR (Routing Processor Redundancy)`

Le superviseur de secours n’a chargé que le système d’exploitation minimal.

En cas de panne du module actif, il doit recharger tous les modules de la baie et initialiser toutes les fonctions.

- `RPR+ (Routing Processor Redundancy Plus)`

Le superviseur de secours a déjà chargé l’OS et la partie routage, mais pas encore les autres services.

À la panne, il termine son initialisation locale sans toucher aux autres modules du châssis, ce qui conserve l’état des ports.

- `SSO (Stateful Switchover)`

Le superviseur de secours est en permanence à jour :

Configurations de démarrage et d’exécution synchronisées,

Tables de commutation (CAM/MAC) et tables de routage (RIB/FIB) répliquées.

À la panne, il prend le rôle actif en moins d’une seconde, sans interruption de trafic.


## Option supplémentaire : NSF/SSO

`NSF (Nonstop Forwarding)` s’ajoute à SSO pour accélérer la reconstruction de la table de routage (RIB) après basculement.

Les modules de commutation conservent la FIB et continuent de forwarder les paquets pendant que le nouveau superviseur reconstruit ses routes.

Sur Catalyst 9400, NSF/SSO permet un basculement en ≤ 150 ms et une interruption de trafic < 200 ms.
