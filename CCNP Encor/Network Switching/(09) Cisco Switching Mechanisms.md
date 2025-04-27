Les routeurs Cisco utilisent des **`mécanismes de commutation`**. Il y'en a **3**.

## I. PROCESS SWITCHING

![image](https://github.com/user-attachments/assets/a3fe9cea-5643-4d98-bc7b-f31b09fa23bc)

Pour chaque trame, le routeur enlève l'en-tête de couche 2 (désencapsulation) pour obtenir le paquet, il fait une recherche dans sa table de routage pour trouver l'adresse IP de destination et reconstruit l'en-tête de couche 2 avec le CRC recalculé. Tout ça est fait par le logiciel exécuté sur le CPU.

Cette manière de fonctionner est très **lente et gourmande en CPU**. Chaque paquet est traité individuellement par le CPU, ce qui n'est pas adapté pour un trfaic intense.

## II. FAST SWITCHING (ROUTE CACHING)

![image](https://github.com/user-attachments/assets/59f419c7-5b38-4f07-8a85-2b88747bfc85)

Le premier paquet d'un flux est traité par le CPU (comme dans process switching). Le routeur crée ensuite une `entrée` dans `un cache Fast Switching`. Les paquets qui viendront après le premiers paquet et correspondant à ce flux seront traités par le matériel (hardware) en utilisant cette entrée. **`Le hardware fait référence au processeur d’interface ou à l’ASIC (Application‑Specific Integrated Circuit) dédié au plan de données, situé sur les line cards (modules d’interface) du routeur`**

Ce mécanisme est plus rapide que le process switching. Seul le premier paquet est traité en profondeur par le CPU. En plus, il utilise un cache Fast Switching pour éviter des recherches répétée, ce qui réduit la charge du CPU pour les `flux connus`.

## III. CISCO EXPRESS FORWARDING (CEF)

![image](https://github.com/user-attachments/assets/8ee5b7ef-9db9-477f-a924-75194178cf9f)

### `C'est le mécanisme par défaut sur beaucoup de routeur Cisco.`

Le routeur construit des tables matérielles (`Forwarding Information Base (FIB) et Adjacency table`) en se basant sur les informations du CPU (`table de routage et ARP`).

Tous les paquets, même le premier sont transférés par le matériel, ce qui est super rapide.

Ce mécanisme de commutation est le plus rapide des 3.

## IV. COMMENT CA MARCHE EN PARTIQUE (CAS DE EIGRP)

![image](https://github.com/user-attachments/assets/e3806f63-3ff4-4682-a7f2-e43f649109c9)

Pour bien comprendre la différence entre process switching et Fast Switching, imaginons un scénario avec EIGRP.

  #### 1. Réception d'un message UPDATE EIGRP

Le routeur met à jour sa table de routage avec le nouveau chemin.

  #### 2. Premier paquet vers une destination EIGRP

Le routeur ne trouve pas encore l'entrée dans le cache Fast Switching. Donc il utilise process switching. Le CPU fait une recherche complète dans la table de routage. Puis, il crée une entrée dans le cache Fast Switching.

Pour les paquet suivants, ils sont traités directement par le matériel, en utilisant un en-tête prégénéré.

`NB : Sur le CPU, il y'a un code qui tourne dessus. Donc quand on parle de logiciel, c'est généralement de ce code qu'on parle. Comparé au matériel, le CPU est moins rapide pour traiter des milliers de paquets à la fois.`

