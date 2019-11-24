# Hackerlab2019 – Choosy Web Server ?

* **Category:** Web
* **Points:** 150

## Challenge

> C'est l'histoire d'un serveur web qui choisit ses agents minutieusement.
>http://hackerlab.bj:8001/web07/

## Solution
Le challenge pour cette épreuve était de trouver le `User-agent` nécessaire pour afficher le contenu de la page. Étant donné qu’il existe plusieurs User-agent, on ne pouvait pas les tester chacun.
>Un *user agent* ou *agent utilisateur* est une application cliente utilisée avec un protocole réseau particulier ; l'expression est plus généralement employée comme référence pour celles qui accèdent au World Wide Web.
      <u>source</u> : [wikipedia](https://fr.wikipedia.org/wiki/User_agent)

Ainsi, avec la commande `ua-tester` sous Kali Linux, nous avions pu connaitre le User-agent spécifique à ce serveur.

Pour faire, nous avons utilisé la commande:
`ua-tester -u http://hackerlab.bj:8001/web07/`.
```bash
$ ua-tester -u http://hackerlab.bj:8001/web07/


         _/    _/  _/_/_/_/       _/_/_/_/ _/_/_/_/ _/_/_/_/ _/_/_/_/ _/_/_/_/ _/_/_/_/
        _/    _/  _/    _/          _/    _/       _/          _/    _/       _/    _/
       _/    _/  _/_/_/_/  _/_/_/  _/    _/_/_/   _/_/_/_/    _/    _/_/_/   _/_/_/_
      _/    _/  _/    _/          _/    _/             _/    _/    _/       _/    _/
     _/_/_/_/  _/    _/          _/    _/_/_/_/ _/_/_/_/    _/    _/_/_/_/ _/      _/ [v1.06]

                                                                 _/ User-Agent Tester ↵
                                                                  _/ AKA: Purple Pimp ↵
                                                                    _/ ChrisJohnRiley ↵
                                                                       _/ blog.c22.cc ↵

 [>] Performing initial request and confirming stability
 [>] Using User-Agent string Mozilla/5.0

    [ ] URL (ENTERED): http://hackerlab.bj:8001/web07/
    [ ] Response Code: 200 OK
    [ ] Date: Sun, 24 Nov 2019 05:10:17 GMT
    [ ] Server: Apache/2.4.38 (Debian)
    [ ] X-Powered-By: PHP/7.1.31
    [ ] Content-Length: 10
    [ ] Connection: close
    [ ] Content-Type: text/html; charset=UTF-8
    [ ] Data (MD5): cc94fe735fbc20b892e660254a4d985b

 [1] Pass
 [2] Pass
 [3] Pass

 [>] URL appears stable. Beginning test

 [>] Using DEFAULT User-Agent Strings


 [>] Output: [+] Added Headers, [-] Removed Headers, [!] Altered Headers, [ ] No Change


 [>] User-Agent String : Lynx (textmode)


    [!] Content-Length: 21


 [>] That's all folks... Fo' Shizzle!

```
A la fin de l’exécution de la commande, on vu que le User-agent était **Lynx**.


Pour pouvoir changer le User-agent et afficher le contenue de la page web, nous avons utiliser la commande CURL de la manière suivante:
`curl -A 'Lynx' http://hackerlab.bj:8001/web07/`, le -A pour spécifier le User-agent qu’on voulait utiliser.
```bash
$ curl -A 'Lynx' http://hackerlab.bj:8001/web07/
CTF_LynxIsLightAsHell
```

Le flag pour ce challenge était: ```CTF_LynxIsLightAsHell```

Enjoy -:)
