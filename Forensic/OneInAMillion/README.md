# Hackerlab2019 – One in a million ?

* **Categorie:** Forensic
* **Points:** 100

## Challenge
>Télécharge le fichier et trouve la flag.<br>
[file](file)

## Solution
Après avoir téléchargé le fichier <b>file</b> comme précisé dans l'énoncé du challenge, nous avons découvert que ce dernier est un fichier compressé grâce à la commande `file`
```bash
lick@host:~/Documents/Hackerlab2019/Forensic/OneInAMillion/$ file file
file: XZ compressed data
```
Nous avons donc procéder à la décompression du fichier <b>file</b> qui contenait un fichier `chal.img`.

```bash
lick@host:~/Documents/Hackerlab2019/Forensic/OneInAMillion/file$ file chal.img
chal.img: DOS/MBR boot sector, code offset 0x3c+2, OEM-ID "mkfs.fat", sectors/cluster 4, reserved sectors 4, root entries 512, sectors 10240 (volumes <=32 MB), Media descriptor 0xf8, sectors/FAT 8, sectors/track 62, heads 248, hidden sectors 2048, serial number 0x4e883265, label: "CHAL       ", FAT (12 bit)
lick@host:~/Documents/Hackerlab2019/Forensic/OneInAMillion/file$
```

Le premier réflexe qui nous ait venu est d'afficher les strings contenu dans le fichier `chal.img`. Au premier essai, nous avons retrouvé beaucoup d'information. Nous avions donc penser à effectuer un grep avec le mot clé `CTF`.
```bash
$ strings chal.img | grep CTF
CTF_FOuineURDeCLes
```

Bingo...........nous voici en face du flag `CTF_FOuineURDeCLes`
