# Hackerlab2019 – Code de Houégbadja

* **Category:** Crypto
* **Points:** 100

## Challenge

> Les cyber-amazones connaissaient parfaitement le code de Houégbadja. C'est un code basé sur 41 articles formant la 
>constitution du Danhomey. Pour le déchiffrer les cyber-amazones ramenaient ce code dans une base de 10 articles, 
>souvent bien moins difficile à garder. Pour valider le flag, précède-le de CTF_
> the challenge.

## Solution
Il s’agissait d’un code écrit en base 41 qu’il fallait ramener en base 10.
Le code en base 41 est : 6CL5 avec C=12 et L=21.
Avec python en faisant le calcul suivant: 
`print(6*41**3 + 12*41**2 + 21*41**1 +5)`
, on obtient le résultat suivant : 434564. Le flag est donc: 
```
CTF_434564
```