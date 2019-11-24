# Hackerlab2019 – Que faire ?

* **Category:** Web
* **Points:** 100

## Challenge

> Pour défendre notre cyber-contrée, les cyber-amazones devaient choisir la bonne méthode.
>http://hackerlab.bj:8001/web01/web01_sqfrs35_quefaire.php

## Solution
Le lien de l’épreuve nous conduisait sur une page vide. Même en inspectant la page il n’y avait rien. Nous avons donc eu l’idée de changer le Header la page qui était envoyer avec la méthode _GET_ par _POST_ et en ensuite par _OPTIONS_ en utilisant `CURL`.
```bash
lick@host:~$ curl -X POST http://hackerlab.bj:8001/web01/web01_sqfrs35_quefaire.php

lick@host:~$ curl -X OPTIONS http://hackerlab.bj:8001/web01/web01_sqfrs35_quefaire.php
CTF_34GFGLFRFE343432323R2
lick@host:~$
```

La commande `curl -X OPTIONS http://hackerlab.bj:8001/web01/web01_sqfrs35_quefaire
.php` avec <b>-X</b> pour préciser la méthode, nous avons a permi à obtenir le flag qui était : ```CTF_34GFGLFRFE343432323R2```.
