# 🐉 Kali Linux — Notes Cybersécurité & Pentest

> **Auteur : Chasseur De Script Kiddies**

> **⚠️ Prérequis : avoir lu les notes « Linux — Bases & Administration »**
>
> Ce document part du principe que les notions fondamentales de Linux sont déjà acquises : navigation dans le système, fichiers, permissions, utilisateurs, processus, services, Bash, SSH, gestion des paquets, etc.
>
> Si certaines notions Linux de base ne sont pas comprises, il est recommandé de consulter le document **Linux — Bases & Administration** avant de poursuivre.

---

## 📖 Présentation

Ce document regroupe mes **notes personnelles**, mes **recherches**, mes **expérimentations** ainsi que différentes notions étudiées dans le domaine de la **cybersécurité** et du **pentest**.

Une partie de ces connaissances provient de mes **études**, notamment de mon parcours orienté informatique, réseaux et cybersécurité.

D'autres notions sont issues de mon **apprentissage autodidacte**, à travers des documentations techniques, des recherches, des laboratoires, des CTF et des expérimentations personnelles.

L'objectif de ce document est de centraliser progressivement les **outils, techniques, concepts et méthodes de cybersécurité** que j'étudie avec **Kali Linux**.

> ⚠️ **Usage éducatif uniquement**
>
> Ces notes sont destinées à l'apprentissage, aux études, aux travaux pratiques, aux CTF et aux environnements de laboratoire autorisés.
>
> Toute utilisation contre un système tiers nécessite une **autorisation explicite**.

---

# 📚 Sommaire

## 🕵️ Reconnaissance & Énumération

* [Nmap](#-nmap)
* [Énumération SMB](#-énumération-smb)
* [Découverte des hôtes](#-découverte-des-hôtes)
* [Fingerprinting](#-fingerprinting)
* [SNMP](#-snmp)
* [DNS](#-dns)
* [DNSRecon](#-dnsrecon)
* [HTTP / HTTPS](#-http--https)

## 🌐 Analyse réseau

* [Serveur Web Python](#-serveur-web-python)
* [Montage de partages](#-montage-de-partages)
* [Inspection des paquets](#-inspection-des-paquets)
* [TTL Fingerprinting](#-ttl-fingerprinting)

## 👤 Utilisateurs & Identifiants

* [Énumération des utilisateurs](#-énumération-des-utilisateurs)
* [Wordlists](#-wordlists)
* [Hachage](#-hachage)
* [Cryptographie](#-cryptographie)

## 🌍 Sécurité Web

* [Énumération Web](#-énumération-web)
* [SQLMap](#-sqlmap)
* [Fuzzing](#-fuzzing)

## 🔎 Recherche de vulnérabilités

* [Recherche d'exploits](#-recherche-dexploits)
* [Compilation C](#-compilation-c)
* [SUID](#-suid)
* [TTY / Shells interactifs](#-tty--shells-interactifs)

## 🧰 Metasploit

* [Metasploit](#-metasploit)
* [Meterpreter](#-meterpreter)

## 🌐 Réseau

* [IPv4](#-ipv4)
* [Sous-réseaux IPv4](#-sous-réseaux-ipv4)

## ⌨️ Références

* [Table ASCII](#-table-ascii)
* [Cisco IOS](#-cisco-ios)

---

# 🕵️ Reconnaissance & Énumération

## 🌐 Nmap

Nmap est un outil permettant notamment d'effectuer de la **découverte réseau**, de l'énumération de services et de l'audit de systèmes dans des environnements autorisés.

| Commande                       | Description                                       |
| ------------------------------ | ------------------------------------------------- |
| `nmap target`                  | Effectue un scan de base.                         |
| `nmap -v target`               | Affiche davantage d'informations pendant le scan. |
| `sudo nmap -sn 192.168.1.0/24` | Recherche les hôtes actifs.                       |
| `nmap -sV target`              | Tente d'identifier les versions des services.     |
| `nmap -O target`               | Tente d'identifier le système d'exploitation.     |
| `nmap -A target`               | Active plusieurs fonctions de détection.          |
| `ls /usr/share/nmap/scripts/`  | Liste les scripts NSE disponibles.                |

> ⚠️ Les scans doivent être effectués uniquement sur des systèmes pour lesquels on dispose d'une autorisation.

---

## 📂 Énumération SMB

SMB est notamment utilisé pour les partages de fichiers et certains services Windows.

| Commande                  | Description                                          |
| ------------------------- | ---------------------------------------------------- |
| `nbtscan 192.168.1.0/24`  | Recherche des informations NetBIOS sur le réseau.    |
| `enum4linux -a target-ip` | Effectue différentes opérations d'énumération SMB.   |
| `smbclient -L target-ip`  | Recherche les partages SMB accessibles.              |
| `smbmap -H target-ip`     | Affiche certaines informations sur les partages SMB. |
| `smbstatus`               | Affiche les connexions Samba locales.                |

---

## 🌐 Découverte des hôtes

| Commande                                   | Description                                       |
| ------------------------------------------ | ------------------------------------------------- |
| `netdiscover -r 192.168.1.0/24`            | Découverte d'hôtes avec ARP.                      |
| `arp-scan --interface=eth0 192.168.1.0/24` | Recherche des hôtes sur un réseau local avec ARP. |
| `fping -g 192.168.1.0/24`                  | Vérifie plusieurs hôtes avec ICMP.                |

---

## 🕵️ Fingerprinting

Le fingerprinting consiste à recueillir des caractéristiques permettant d'identifier un système, un service ou une technologie.

| Commande                     | Description                                           |
| ---------------------------- | ----------------------------------------------------- |
| `nc -v 192.168.1.1 25`       | Permet d'observer une éventuelle bannière de service. |
| `curl -I http://192.168.1.1` | Récupère les en-têtes HTTP.                           |
| `nmap -O 192.168.1.1`        | Tente d'identifier l'OS.                              |
| `whatweb http://192.168.1.1` | Identifie certaines technologies Web.                 |

---

## 📡 SNMP

SNMP peut exposer différentes informations lorsqu'il est mal configuré.

| Commande                                             | Description                                 |
| ---------------------------------------------------- | ------------------------------------------- |
| `snmpcheck -t 192.168.1.X -c public`                 | Effectue une énumération SNMP.              |
| `snmpwalk -c public -v1 192.168.1.X 1`               | Parcourt les informations SNMP accessibles. |
| `onesixtyone -c names -i hosts`                      | Recherche des services SNMP.                |
| `snmpbulkwalk -v2c -c public -Cn0 -Cr10 192.168.1.X` | Parcourt SNMP par lots.                     |

> ⚠️ À utiliser uniquement dans un environnement autorisé.

---

## 🌐 DNS

Quelques outils couramment utilisés pour analyser le DNS :

```bash
nslookup example.local
```

```bash
dig example.local
```

```bash
host example.local
```

---

## 📡 DNSRecon

DNSRecon permet d'effectuer différentes opérations de collecte d'informations DNS.

```bash
dnsrecon -d TARGET -t std
```

> Les domaines analysés doivent être autorisés.

---

## 🌍 HTTP / HTTPS

| Commande                                   | Description                                                   |
| ------------------------------------------ | ------------------------------------------------------------- |
| `curl -I http://192.168.1.1`               | Affiche les en-têtes HTTP.                                    |
| `whatweb http://192.168.1.1`               | Identifie certaines technologies Web.                         |
| `nikto -h 192.168.1.1`                     | Recherche certaines configurations ou vulnérabilités connues. |
| `nmap -p80 --script http-enum 192.168.1.1` | Effectue une énumération HTTP avec Nmap.                      |

---

# 🌐 Analyse réseau

## 🐍 Serveur Web Python

Python permet de lancer rapidement un petit serveur HTTP pour des environnements de laboratoire.

```bash
python3 -m http.server 8000
```

Pour limiter l'écoute à la machine locale :

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

---

## 🗄️ Montage de partages

### NFS

```bash
mount 192.168.1.1:/vol/share /mnt/nfs
```

### SMB / CIFS

```bash
mount -t cifs //192.168.1.X/share-name /mnt/cifs
```

---

## 📦 Inspection des paquets

### Tcpdump

```bash
tcpdump -i eth0
```

Capture d'un trafic HTTP :

```bash
tcpdump -i eth0 'tcp port 80'
```

Enregistrer une capture :

```bash
tcpdump -i eth0 -w output.pcap
```

### Wireshark

```bash
wireshark -k -i <interface>
```

### TShark

```bash
tshark -i eth0
```

> ⚠️ Les captures réseau doivent être réalisées uniquement sur des réseaux ou interfaces que l'on est autorisé à analyser.

---

## 📡 TTL Fingerprinting

Le TTL (**Time To Live**) peut fournir un indice concernant le système ayant initialement généré un paquet.

| Système / équipement                | TTL initial couramment observé |
| ----------------------------------- | -----------------------------: |
| Windows                             |                            128 |
| Linux / Unix                        |                             64 |
| Solaris                             |                            255 |
| Cisco / certains équipements réseau |                            255 |

> ⚠️ Le TTL n'est qu'un **indice**. Il ne permet pas d'identifier avec certitude un système d'exploitation.

---

# 👤 Utilisateurs & Identifiants

## 👤 Énumération des utilisateurs

Certains services peuvent exposer des informations concernant les comptes lorsqu'ils sont mal configurés.

Outils étudiés :

* `enum4linux`
* `rpcclient`
* `snmpwalk`
* Impacket

> ⚠️ Toute collecte d'informations concernant des comptes doit rester limitée aux environnements autorisés.

---

## 🔑 Wordlists

Kali Linux fournit différentes listes dans :

```text
/usr/share/wordlists/
```

Une collection très utilisée dans les environnements de cybersécurité :

```text
SecLists
```

Les wordlists peuvent notamment être utilisées dans des exercices de découverte de mots de passe, de fuzzing ou d'énumération.

---

## 🔐 Hachage

Un hash est le résultat d'une fonction de hachage appliquée à une donnée.

| Algorithme | Taille de sortie |
| ---------- | ---------------: |
| MD5        |        16 octets |
| SHA-1      |        20 octets |
| SHA-256    |        32 octets |
| SHA-512    |        64 octets |

> MD5 et SHA-1 ne sont plus recommandés pour de nombreux usages de sécurité en raison de faiblesses cryptographiques connues.

---

## 🔒 Cryptographie

Notions étudiées :

* Hachage
* Chiffrement symétrique
* Chiffrement asymétrique
* Clés publiques
* Clés privées
* Certificats
* TLS
* Signatures numériques

---

# 🌍 Sécurité Web

## 🔎 Énumération Web

Outils étudiés dans les environnements autorisés :

| Outil    | Utilisation                     |
| -------- | ------------------------------- |
| Nikto    | Analyse Web                     |
| Gobuster | Découverte de ressources        |
| Wfuzz    | Fuzzing                         |
| WPScan   | Analyse WordPress               |
| WhatWeb  | Identification des technologies |
| cURL     | Requêtes HTTP                   |

---

## 🐞 SQLMap

SQLMap permet d'étudier les injections SQL dans des applications prévues pour les tests.

Exemple de laboratoire :

```bash
sqlmap -u "http://lab.local/item?id=1"
```

> ⚠️ SQLMap doit uniquement être utilisé sur une application de test ou un système explicitement autorisé.

---

## 🔎 Fuzzing

Le fuzzing consiste à envoyer différentes entrées à une application afin d'observer son comportement.

Outils étudiés :

* Gobuster
* Wfuzz
* FFUF

Exemple de laboratoire avec une application locale :

```bash
ffuf -u http://lab.local/FUZZ -w wordlist.txt
```

---

# 🔎 Recherche de vulnérabilités

## 📚 Searchsploit

Searchsploit permet de rechercher localement des références issues d'Exploit-DB.

```bash
searchsploit windows
```

Recherche ciblée :

```bash
searchsploit apache
```

---

## 🔧 Compilation C

### Identifier l'environnement

| En-tête        | Système généralement associé |
| -------------- | ---------------------------- |
| `windows.h`    | Windows                      |
| `winsock2.h`   | Windows                      |
| `process.h`    | Windows                      |
| `arpa/inet.h`  | Linux / Unix                 |
| `netinet/in.h` | Linux / Unix                 |
| `sys/types.h`  | Linux / Unix                 |
| `unistd.h`     | Linux / Unix                 |

### GCC

```bash
gcc -o programme programme.c
```

Avec avertissements :

```bash
gcc -Wall -Wextra programme.c -o programme
```

Compilation 32 bits :

```bash
gcc -m32 programme.c -o programme
```

> Les exemples de code offensif doivent rester limités à des environnements de laboratoire contrôlés.

---

## 🔑 SUID

Le bit **SUID** permet à certains programmes de s'exécuter avec les privilèges du propriétaire du fichier.

Recherche de fichiers SUID :

```bash
find / -perm -4000 -type f 2>/dev/null
```

> ⚠️ Les binaires SUID mal configurés peuvent représenter un risque d'élévation de privilèges.

---

## 🐚 TTY / Shells interactifs

Les TTY permettent notamment de travailler avec des terminaux interactifs dans certains environnements Linux.

Exemple utilisé dans certains laboratoires :

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

Autres environnements pouvant être étudiés :

* Python
* Perl
* Ruby
* Lua
* Vi
* awk
* socat

> Ces techniques doivent rester limitées aux environnements de formation et de laboratoire.

---

# 🧰 Metasploit

Metasploit est un framework destiné à la recherche de vulnérabilités et aux tests de sécurité.

Lancement :

```bash
msfconsole
```

Principales catégories :

| Type        | Description                                              |
| ----------- | -------------------------------------------------------- |
| `exploit`   | Modules permettant de tester certaines vulnérabilités.   |
| `auxiliary` | Modules auxiliaires, scanners et outils complémentaires. |
| `post`      | Modules utilisés après certaines phases de test.         |
| `payload`   | Charges utiles associées à certains modules.             |
| `encoder`   | Encodeurs disponibles dans certains scénarios.           |

> ⚠️ Metasploit doit être utilisé dans un environnement de laboratoire ou sur une cible explicitement autorisée.

---

# 💻 Meterpreter

Meterpreter fournit un environnement interactif associé à certaines simulations Metasploit.

Commandes générales :

```text
help
info
getuid
sysinfo
ps
pwd
```

> ⚠️ Les fonctionnalités permettant d'accéder à des données sensibles ou privées doivent rester limitées aux environnements de laboratoire autorisés.

---

# 🌐 Réseau & IPv4

## 📡 IPv4

### Plages par classes

Les classes d'adresses IPv4 sont historiques. L'adressage moderne utilise principalement **CIDR**.

| Classe   | Plage                         |
| -------- | ----------------------------- |
| Classe A | `0.0.0.0 – 127.255.255.255`   |
| Classe B | `128.0.0.0 – 191.255.255.255` |
| Classe C | `192.0.0.0 – 223.255.255.255` |
| Classe D | `224.0.0.0 – 239.255.255.255` |
| Classe E | `240.0.0.0 – 255.255.255.255` |

---

## 🔒 Plages IPv4 privées

| Plage                           | CIDR  |
| ------------------------------- | ----- |
| `10.0.0.0 – 10.255.255.255`     | `/8`  |
| `172.16.0.0 – 172.31.255.255`   | `/12` |
| `192.168.0.0 – 192.168.255.255` | `/16` |

### Loopback

```text
127.0.0.0/8
```

---

# 📝 Sous-réseaux IPv4

| CIDR  | Masque décimal    | Hôtes utilisables |
| ----- | ----------------- | ----------------: |
| `/31` | `255.255.255.254` |                2* |
| `/30` | `255.255.255.252` |                 2 |
| `/29` | `255.255.255.248` |                 6 |
| `/28` | `255.255.255.240` |                14 |
| `/27` | `255.255.255.224` |                30 |
| `/26` | `255.255.255.192` |                62 |
| `/25` | `255.255.255.128` |               126 |
| `/24` | `255.255.255.0`   |               254 |
| `/23` | `255.255.254.0`   |               510 |
| `/22` | `255.255.252.0`   |              1022 |
| `/21` | `255.255.248.0`   |              2046 |
| `/20` | `255.255.240.0`   |              4094 |
| `/19` | `255.255.224.0`   |              8190 |
| `/18` | `255.255.192.0`   |             16382 |
| `/17` | `255.255.128.0`   |             32766 |
| `/16` | `255.255.0.0`     |             65534 |
| `/15` | `255.254.0.0`     |            131070 |
| `/14` | `255.252.0.0`     |            262142 |
| `/13` | `255.248.0.0`     |            524286 |
| `/12` | `255.240.0.0`     |           1048574 |
| `/11` | `255.224.0.0`     |           2097150 |
| `/10` | `255.192.0.0`     |           4194302 |
| `/9`  | `255.128.0.0`     |           8388606 |
| `/8`  | `255.0.0.0`       |          16777214 |

* `/31` possède un comportement particulier et est principalement utilisé pour certains liens point-à-point.

---

# ⌨️ Table ASCII

| Hex   | Caractère | Hex   | Caractère | Hex   | Caractère | Hex   | Caractère |
| ----- | --------- | ----- | --------- | ----- | --------- | ----- | --------- |
| `x00` | NUL       | `x08` | BS        | `x09` | TAB       | `x0A` | LF        |
| `x0D` | CR        | `x1B` | ESC       | `x20` | SPC       | `x21` | `!`       |
| `x22` | `"`       | `x23` | `#`       | `x24` | `$`       | `x25` | `%`       |
| `x26` | `&`       | `x27` | `'`       | `x28` | `(`       | `x29` | `)`       |
| `x2A` | `*`       | `x2B` | `+`       | `x2C` | `,`       | `x2D` | `-`       |
| `x2E` | `.`       | `x2F` | `/`       | `x30` | `0`       | `x31` | `1`       |
| `x32` | `2`       | `x33` | `3`       | `x34` | `4`       | `x35` | `5`       |
| `x36` | `6`       | `x37` | `7`       | `x38` | `8`       | `x39` | `9`       |
| `x3A` | `:`       | `x3B` | `;`       | `x3C` | `<`       | `x3D` | `=`       |
| `x3E` | `>`       | `x3F` | `?`       | `x40` | `@`       | `x41` | `A`       |
| `x42` | `B`       | `x43` | `C`       | `x44` | `D`       | `x45` | `E`       |
| `x46` | `F`       | `x47` | `G`       | `x48` | `H`       | `x49` | `I`       |
| `x4A` | `J`       | `x4B` | `K`       | `x4C` | `L`       | `x4D` | `M`       |
| `x4E` | `N`       | `x4F` | `O`       | `x50` | `P`       | `x51` | `Q`       |
| `x52` | `R`       | `x53` | `S`       | `x54` | `T`       | `x55` | `U`       |
| `x56` | `V`       | `x57` | `W`       | `x58` | `X`       | `x59` | `Y`       |
| `x5A` | `Z`       | `x5B` | `[`       | `x5C` | `\`       | `x5D` | `]`       |
| `x5E` | `^`       | `x5F` | `_`       | `x60` | `` ` ``   | `x61` | `a`       |
| `x62` | `b`       | `x63` | `c`       | `x64` | `d`       | `x65` | `e`       |
| `x66` | `f`       | `x67` | `g`       | `x68` | `h`       | `x69` | `i`       |
| `x6A` | `j`       | `x6B` | `k`       | `x6C` | `l`       | `x6D` | `m`       |
| `x6E` | `n`       | `x6F` | `o`       | `x70` | `p`       | `x71` | `q`       |
| `x72` | `r`       | `x73` | `s`       | `x74` | `t`       | `x75` | `u`       |
| `x76` | `v`       | `x77` | `w`       | `x78` | `x`       | `x79` | `y`       |
| `x7A` | `z`       |       |           |       |           |       |           |

---

# 📌 Évolution de mes notes

Ce document est **volontairement évolutif**.

Il sera complété progressivement avec de nouveaux outils, de nouvelles techniques, de nouvelles notions et de nouveaux résultats issus de mes recherches et expérimentations.

De nouvelles sections pourront notamment être ajoutées autour de :

* 🛡️ Sécurité Linux
* 🪟 Sécurité Windows
* 🌐 Sécurité Web
* 🏢 Active Directory
* 🔎 Analyse de vulnérabilités
* 📡 Analyse réseau
* 🧪 Laboratoires de cybersécurité
* 🛠️ Nouveaux outils Kali Linux
* 📦 Exploitation et post-exploitation en laboratoire
* 🔬 Analyse forensique
* 🧬 Reverse engineering
* 🔐 Sécurité cryptographique

Cette liste n'est pas définitive.

---

# ⚠️ Usage éducatif

Ces notes constituent un **travail personnel** créé et maintenu par :

> **🛡️ Chasseur De Script Kiddies**

Elles sont destinées à :

* 📚 l'apprentissage ;
* 🎓 mes études ;
* 🧪 mes expérimentations ;
* 🛠️ mes travaux pratiques ;
* 🏁 les CTF ;
* 🔬 les environnements de laboratoire autorisés.

Toute utilisation sur un système, un réseau ou une application ne m'appartenant pas doit être réalisée uniquement avec une **autorisation explicite**.

---

<p align="center">

🐉 **Kali Linux — Notes personnelles**

<br>

🛡️ **Chasseur De Script Kiddies**

<br><br>

📚 **Apprendre. Comprendre. Expérimenter.**

</p>
