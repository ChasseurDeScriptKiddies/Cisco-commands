# 🐧 Linux — Administration & Bases

> **Auteur : Chasseur De Script Kiddies**
>
> Ce document regroupe mes **notes personnelles** concernant Linux, l'administration système, les commandes essentielles, les services, le stockage, les utilisateurs, les permissions et différentes notions liées aux systèmes GNU/Linux.
>
> Une partie de ces connaissances provient de mes **études**, notamment de mon parcours orienté informatique, réseaux et cybersécurité. D'autres notions ont été acquises de manière **autodidacte**, à travers mes recherches, lectures de documentations techniques, expérimentations et environnements de pratique.
>
> Ce document a été créé afin de centraliser progressivement les connaissances fondamentales nécessaires à l'utilisation et à l'administration d'un système Linux.
>
> Il est volontairement évolutif et pourra être complété au fur et à mesure de mon apprentissage.

---

# 📚 Sommaire

## 🐧 Commandes Linux

* [Navigation](#-navigation)
* [Fichiers et répertoires](#-fichiers-et-répertoires)
* [Recherche](#-recherche)
* [Informations système](#-informations-système)

## 👤 Gestion des utilisateurs

* [Utilisateurs](#-utilisateurs)

## 👥 Gestion des groupes

* [Groupes](#-groupes)

## 🔐 Permissions

* [Comprendre les permissions](#-comprendre-les-permissions)
* [Commandes](#-commandes)

## ⚙️ Gestion des processus

* [Processus](#-processus)

## 🔧 Services

* [Gestion des services](#-gestion-des-services)
* [Systemd](#-systemd)

## 🔑 SSH

* [Connexion SSH](#-connexion-ssh)
* [Clés SSH](#-clés-ssh)

## 📦 Gestion des paquets

* [APT](#-apt)

## 🐚 Bash

* [Variables](#-variables)
* [Conditions](#-conditions)
* [Boucles](#-boucles)
* [Redirections](#-redirections)
* [Pipes](#-pipes)

## ⏰ Cron

* [Crontab](#-crontab)

## 💾 Gestion des disques

* [Commandes](#-commandes-1)

## 📀 LVM

* [Concept](#-concept)

## 💿 RAID

* [Niveaux RAID](#-niveaux-raid)

## 🗂️ Samba

* [SMB/CIFS](#-smbcifs)

## 📁 NFS

* [NFS](#-nfs)

## 🌐 Réseau

* [IPv4](#-ipv4)
* [Plages privées](#-plages-ipv4-privées)
* [Loopback](#-loopback)
* [Sous-réseaux](#-sous-réseaux-ipv4)
* [Commandes réseau](#-commandes-réseau)

## 📡 Cisco IOS

* [Commandes Cisco](#-commandes-cisco)

---

# 🐧 Administration Linux

Linux est un système d'exploitation de type Unix utilisé aussi bien sur des ordinateurs personnels que sur des serveurs, des systèmes embarqués et de nombreuses infrastructures informatiques.

Il existe de nombreuses distributions Linux, notamment :

* Debian
* Ubuntu
* Fedora
* Arch Linux
* Rocky Linux
* AlmaLinux
* openSUSE
* Kali Linux

Les commandes et concepts présentés dans ce document sont principalement communs aux systèmes GNU/Linux.

---

# 💻 Commandes Linux

## 📂 Navigation

| Commande     | Description                                                              |
| ------------ | ------------------------------------------------------------------------ |
| `pwd`        | Affiche le répertoire courant.                                           |
| `ls`         | Liste le contenu du répertoire.                                          |
| `ls -la`     | Affiche les fichiers, y compris les fichiers cachés, avec leurs détails. |
| `cd /chemin` | Change de répertoire.                                                    |
| `cd ..`      | Remonte d'un niveau.                                                     |
| `cd ~`       | Retourne dans le répertoire personnel.                                   |
| `cd -`       | Retourne au répertoire précédent.                                        |

---

## 📁 Fichiers et répertoires

| Commande                    | Description                            |
| --------------------------- | -------------------------------------- |
| `touch fichier`             | Crée un fichier vide.                  |
| `mkdir dossier`             | Crée un répertoire.                    |
| `mkdir -p chemin/dossier`   | Crée une arborescence complète.        |
| `cp source destination`     | Copie un fichier ou un répertoire.     |
| `cp -r dossier destination` | Copie récursivement un répertoire.     |
| `mv source destination`     | Déplace ou renomme un fichier.         |
| `rm fichier`                | Supprime un fichier.                   |
| `rm -r dossier`             | Supprime un répertoire et son contenu. |
| `cat fichier`               | Affiche le contenu d'un fichier.       |
| `less fichier`              | Consulte un fichier page par page.     |
| `head fichier`              | Affiche le début d'un fichier.         |
| `tail fichier`              | Affiche la fin d'un fichier.           |
| `file fichier`              | Identifie le type d'un fichier.        |

> ⚠️ `rm` supprime directement les fichiers sans passer par une corbeille dans la plupart des environnements en ligne de commande. Utiliser les options récursives avec prudence.

---

## 🔎 Recherche

| Commande                       | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| `find /chemin -name "fichier"` | Recherche un fichier par son nom.                            |
| `grep "texte" fichier`         | Recherche du texte dans un fichier.                          |
| `grep -R "texte" dossier`      | Effectue une recherche récursive.                            |
| `which commande`               | Recherche l'emplacement d'une commande.                      |
| `whereis commande`             | Recherche les fichiers associés à une commande.              |
| `locate fichier`               | Recherche rapidement un fichier à partir d'une base indexée. |

---

## 🖥️ Informations système

| Commande         | Description                                            |
| ---------------- | ------------------------------------------------------ |
| `uname -a`       | Affiche les informations du noyau.                     |
| `hostname`       | Affiche le nom de la machine.                          |
| `whoami`         | Affiche l'utilisateur courant.                         |
| `id`             | Affiche l'identifiant et les groupes de l'utilisateur. |
| `uptime`         | Affiche depuis combien de temps le système fonctionne. |
| `date`           | Affiche la date et l'heure.                            |
| `df -h`          | Affiche l'espace disque disponible.                    |
| `du -sh dossier` | Affiche la taille d'un dossier.                        |
| `free -h`        | Affiche l'utilisation de la mémoire.                   |
| `lscpu`          | Affiche les informations du processeur.                |
| `lsmem`          | Affiche les informations concernant la mémoire.        |

---

# 👤 Gestion des utilisateurs

Linux permet de gérer plusieurs utilisateurs sur une même machine.

Chaque utilisateur possède notamment :

* un nom ;
* un UID ;
* un groupe principal ;
* éventuellement plusieurs groupes secondaires ;
* un répertoire personnel ;
* un shell ;
* des permissions.

## Utilisateurs

| Commande                       | Description                                                                   |
| ------------------------------ | ----------------------------------------------------------------------------- |
| `whoami`                       | Affiche l'utilisateur courant.                                                |
| `id utilisateur`               | Affiche les informations d'un utilisateur.                                    |
| `who`                          | Affiche les utilisateurs actuellement connectés.                              |
| `w`                            | Affiche les utilisateurs connectés et leur activité.                          |
| `sudo useradd utilisateur`     | Crée un utilisateur.                                                          |
| `sudo adduser utilisateur`     | Crée un utilisateur avec un assistant interactif sur certaines distributions. |
| `sudo passwd utilisateur`      | Définit ou modifie son mot de passe.                                          |
| `sudo usermod ... utilisateur` | Modifie les propriétés d'un utilisateur.                                      |
| `sudo userdel utilisateur`     | Supprime un utilisateur.                                                      |

Informations concernant les comptes :

```bash
cat /etc/passwd
```

> `/etc/passwd` contient des informations générales sur les comptes utilisateurs. Les mots de passe ne sont normalement pas stockés directement dans ce fichier.

---

# 👥 Gestion des groupes

Les groupes permettent notamment d'attribuer des permissions communes à plusieurs utilisateurs.

## Groupes

| Commande                              | Description                                    |
| ------------------------------------- | ---------------------------------------------- |
| `groups`                              | Affiche les groupes de l'utilisateur courant.  |
| `id utilisateur`                      | Affiche les groupes associés à un utilisateur. |
| `sudo groupadd groupe`                | Crée un groupe.                                |
| `sudo groupdel groupe`                | Supprime un groupe.                            |
| `sudo usermod -aG groupe utilisateur` | Ajoute un utilisateur à un groupe.             |
| `getent group`                        | Affiche les groupes connus du système.         |

---

# 🔐 Permissions

Linux utilise principalement trois permissions :

```text
r = lecture
w = écriture
x = exécution
```

Exemple :

```text
-rwxr-xr--
```

Les permissions sont généralement présentées pour :

1. le propriétaire ;
2. le groupe ;
3. les autres utilisateurs.

| Symbole | Signification      |
| ------- | ------------------ |
| `r`     | Lecture            |
| `w`     | Écriture           |
| `x`     | Exécution          |
| `-`     | Permission absente |

---

## 🔢 Notation octale

Les permissions peuvent être représentées numériquement.

| Permission | Valeur |
| ---------- | -----: |
| `---`      |    `0` |
| `--x`      |    `1` |
| `-w-`      |    `2` |
| `-wx`      |    `3` |
| `r--`      |    `4` |
| `r-x`      |    `5` |
| `rw-`      |    `6` |
| `rwx`      |    `7` |

Exemple :

```bash
chmod 755 fichier
```

Correspond à :

```text
Propriétaire : rwx
Groupe       : r-x
Autres       : r-x
```

---

## 🔧 Commandes

| Commande                           | Description                                      |
| ---------------------------------- | ------------------------------------------------ |
| `ls -l`                            | Affiche les permissions.                         |
| `chmod 755 fichier`                | Modifie les permissions avec la notation octale. |
| `chmod +x fichier`                 | Ajoute le droit d'exécution.                     |
| `chmod -x fichier`                 | Retire le droit d'exécution.                     |
| `chown utilisateur fichier`        | Change le propriétaire.                          |
| `chown utilisateur:groupe fichier` | Change le propriétaire et le groupe.             |
| `chgrp groupe fichier`             | Change le groupe propriétaire.                   |

---

# ⚙️ Gestion des processus

Un processus est une instance d'un programme en cours d'exécution.

## 🔎 Processus

| Commande          | Description                                 |
| ----------------- | ------------------------------------------- |
| `ps aux`          | Liste les processus.                        |
| `top`             | Affiche les processus en temps réel.        |
| `htop`            | Alternative interactive à `top`.            |
| `pgrep processus` | Recherche un processus par son nom.         |
| `pidof programme` | Recherche le PID d'un programme.            |
| `kill PID`        | Envoie un signal à un processus.            |
| `pkill nom`       | Cible les processus correspondant à un nom. |

Exemple :

```bash
ps aux
```

Un processus possède notamment un **PID** permettant de l'identifier.

---

# 🔧 Gestion des services

Les distributions Linux modernes utilisent généralement `systemd` comme système d'initialisation et gestionnaire de services.

| Commande                    | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| `systemctl status service`  | Affiche l'état d'un service.                            |
| `systemctl start service`   | Démarre un service.                                     |
| `systemctl stop service`    | Arrête un service.                                      |
| `systemctl restart service` | Redémarre un service.                                   |
| `systemctl reload service`  | Recharge sa configuration lorsque le service le permet. |
| `systemctl enable service`  | Active le démarrage automatique.                        |
| `systemctl disable service` | Désactive le démarrage automatique.                     |

---

# 🛠️ Systemd

`systemd` est utilisé pour l'initialisation du système et la gestion de nombreux services.

| Commande                    | Description                                          |
| --------------------------- | ---------------------------------------------------- |
| `systemctl`                 | Interface principale de gestion de systemd.          |
| `systemctl list-units`      | Liste les unités actives.                            |
| `systemctl list-unit-files` | Liste les unités installées.                         |
| `systemctl get-default`     | Affiche la cible de démarrage par défaut.            |
| `journalctl`                | Consulte les journaux système.                       |
| `journalctl -u service`     | Affiche les journaux d'un service.                   |
| `journalctl -b`             | Affiche les journaux du démarrage courant.           |
| `journalctl -f`             | Suit les nouveaux messages de journal en temps réel. |

---

# 🔑 SSH

SSH permet d'établir une connexion distante sécurisée vers une machine.

## 💻 Connexion SSH

```bash
ssh utilisateur@serveur
```

Utiliser un port différent :

```bash
ssh -p 2222 utilisateur@serveur
```

Copier un fichier vers un serveur :

```bash
scp fichier utilisateur@serveur:/chemin/
```

Copier un fichier depuis un serveur :

```bash
scp utilisateur@serveur:/chemin/fichier .
```

---

## 🔍 Vérification

Vérifier l'état du serveur SSH :

```bash
systemctl status ssh
```

Sur certaines distributions, le service peut être nommé différemment.

---

## 🔐 Clés SSH

Générer une paire de clés :

```bash
ssh-keygen
```

La paire contient généralement :

* une clé privée ;
* une clé publique.

La **clé privée doit rester secrète**.

---

# 📦 Gestion des paquets

Les distributions Linux utilisent différents gestionnaires de paquets.

Exemples :

| Distribution    | Gestionnaire courant |
| --------------- | -------------------- |
| Debian / Ubuntu | `apt`                |
| Fedora          | `dnf`                |
| Arch Linux      | `pacman`             |
| openSUSE        | `zypper`             |

---

## 📦 APT

Sur les distributions basées sur Debian :

| Commande                  | Description                                                                       |
| ------------------------- | --------------------------------------------------------------------------------- |
| `sudo apt update`         | Met à jour la liste des paquets.                                                  |
| `sudo apt upgrade`        | Met à niveau les paquets installés.                                               |
| `sudo apt install paquet` | Installe un paquet.                                                               |
| `sudo apt remove paquet`  | Supprime un paquet.                                                               |
| `sudo apt purge paquet`   | Supprime un paquet et ses fichiers de configuration associés lorsque disponibles. |
| `sudo apt search paquet`  | Recherche un paquet.                                                              |
| `apt show paquet`         | Affiche les informations d'un paquet.                                             |
| `apt list --installed`    | Liste les paquets installés.                                                      |

---

# 🐚 Bash

**Bash** est un shell très répandu dans les environnements GNU/Linux.

Il permet notamment :

* d'exécuter des commandes ;
* de manipuler des fichiers ;
* de créer des scripts ;
* d'utiliser des variables ;
* de créer des conditions ;
* d'effectuer des boucles ;
* de rediriger les entrées et sorties.

---

## Variables

```bash
nom="Linux"

echo "$nom"
```

---

## Conditions

```bash
if [ "$USER" = "root" ]; then
    echo "Utilisateur root"
else
    echo "Utilisateur standard"
fi
```

---

## Boucle

```bash
for fichier in *.txt; do
    echo "$fichier"
done
```

---

## Redirections

Redirection standard :

```bash
commande > sortie.txt
```

Ajouter à un fichier :

```bash
commande >> sortie.txt
```

Rediriger les erreurs :

```bash
commande 2> erreurs.txt
```

Rediriger la sortie standard et les erreurs :

```bash
commande > sortie.txt 2>&1
```

---

## Pipes

Les pipes permettent d'envoyer la sortie d'une commande vers l'entrée d'une autre.

```bash
commande1 | commande2
```

Exemple :

```bash
ps aux | grep firefox
```

---

# ⏰ Cron

Cron permet d'exécuter automatiquement des tâches selon une planification.

## 📋 Crontab

Afficher la crontab :

```bash
crontab -l
```

Modifier la crontab :

```bash
crontab -e
```

Exemple :

```text
0 2 * * * /chemin/script.sh
```

Cette entrée exécute le script tous les jours à **02:00**.

Structure :

```text
minute heure jour_du_mois mois jour_de_la_semaine commande
```

---

# 💾 Gestion des disques

Linux permet d'identifier, partitionner, monter et gérer différents périphériques de stockage.

## Commandes

| Commande                              | Description                                        |
| ------------------------------------- | -------------------------------------------------- |
| `lsblk`                               | Affiche les périphériques de stockage.             |
| `blkid`                               | Affiche les UUID et types de systèmes de fichiers. |
| `df -h`                               | Affiche l'utilisation des systèmes de fichiers.    |
| `du -sh dossier`                      | Affiche la taille d'un dossier.                    |
| `mount`                               | Affiche les systèmes de fichiers montés.           |
| `mount périphérique point_de_montage` | Monte un système de fichiers.                      |
| `umount /mnt/test`                    | Démonte un système de fichiers.                    |
| `fdisk -l`                            | Affiche les disques et partitions.                 |

---

# 📀 LVM

**LVM — Logical Volume Manager** permet de gérer des volumes logiques indépendamment de la structure classique des partitions.

Organisation simplifiée :

```text
Physical Volume (PV)
        ↓
Volume Group (VG)
        ↓
Logical Volume (LV)
```

## Commandes

| Commande    | Description                     |
| ----------- | ------------------------------- |
| `pvs`       | Affiche les volumes physiques.  |
| `vgs`       | Affiche les groupes de volumes. |
| `lvs`       | Affiche les volumes logiques.   |
| `pvdisplay` | Affiche les détails des PV.     |
| `vgdisplay` | Affiche les détails des VG.     |
| `lvdisplay` | Affiche les détails des LV.     |

---

# 💿 RAID

RAID permet de combiner plusieurs disques afin d'améliorer certaines caractéristiques comme les performances ou la tolérance aux pannes.

| Niveau  | Principe                       |
| ------- | ------------------------------ |
| RAID 0  | Agrégation, aucune redondance. |
| RAID 1  | Miroir des données.            |
| RAID 5  | Parité répartie.               |
| RAID 6  | Double parité.                 |
| RAID 10 | Miroir + agrégation.           |

> ⚠️ **RAID n'est pas un système de sauvegarde.**

Une suppression accidentelle ou certaines corruptions peuvent être répliquées sur plusieurs disques.

---

# 🗂️ Samba

**Samba** permet notamment de fournir des partages **SMB/CIFS** compatibles avec Windows.

## Vérification du service

```bash
systemctl status smbd
```

## Liste des partages

```bash
smbclient -L //192.168.1.10
```

Samba est notamment utilisé pour :

* partager des fichiers ;
* fournir des ressources accessibles depuis Windows ;
* intégrer certains environnements Linux dans des réseaux utilisant SMB.

---

# 📁 NFS

**NFS — Network File System** permet de partager des systèmes de fichiers sur un réseau.

## Commandes

| Commande                         | Description              |
| -------------------------------- | ------------------------ |
| `showmount -e serveur`           | Affiche les exports NFS. |
| `mount serveur:/export /mnt/nfs` | Monte un export NFS.     |
| `umount /mnt/nfs`                | Démonte le partage.      |

---

# 🌐 Réseau

Linux fournit de nombreux outils permettant d'observer et de configurer les interfaces réseau.

Commandes courantes :

```bash
ip addr
```

Affiche les adresses des interfaces.

```bash
ip route
```

Affiche la table de routage.

```bash
ip link
```

Affiche les interfaces réseau.

Tester une connectivité :

```bash
ping 192.168.1.1
```

Résolution DNS :

```bash
nslookup example.com
```

ou :

```bash
dig example.com
```

---

# 🌍 IPv4

Une adresse IPv4 est composée de **32 bits**, généralement représentés sous la forme de quatre nombres décimaux séparés par des points.

Exemple :

```text
192.168.1.10
```

---

## 📚 Classes IPv4

Les classes d'adresses sont aujourd'hui principalement historiques. L'adressage moderne utilise surtout **CIDR**.

| Classe   | Plage                         |
| -------- | ----------------------------- |
| Classe A | `0.0.0.0 – 127.255.255.255`   |
| Classe B | `128.0.0.0 – 191.255.255.255` |
| Classe C | `192.0.0.0 – 223.255.255.255` |
| Classe D | `224.0.0.0 – 239.255.255.255` |
| Classe E | `240.0.0.0 – 255.255.255.255` |

---

# 🔒 Plages IPv4 privées

Les principales plages privées définies pour les réseaux internes sont :

| Plage                           | CIDR  |
| ------------------------------- | ----- |
| `10.0.0.0 – 10.255.255.255`     | `/8`  |
| `172.16.0.0 – 172.31.255.255`   | `/12` |
| `192.168.0.0 – 192.168.255.255` | `/16` |

---

# 🔁 Loopback

La plage IPv4 de loopback est :

```text
127.0.0.0/8
```

L'adresse la plus couramment utilisée est :

```text
127.0.0.1
```

Elle permet à une machine de communiquer avec elle-même via son interface loopback.

---

# 📝 Sous-réseaux IPv4

| CIDR  | Masque décimal    | Hôtes utilisables* |
| ----- | ----------------- | -----------------: |
| `/30` | `255.255.255.252` |                  2 |
| `/29` | `255.255.255.248` |                  6 |
| `/28` | `255.255.255.240` |                 14 |
| `/27` | `255.255.255.224` |                 30 |
| `/26` | `255.255.255.192` |                 62 |
| `/25` | `255.255.255.128` |                126 |
| `/24` | `255.255.255.0`   |                254 |
| `/23` | `255.255.254.0`   |                510 |
| `/22` | `255.255.252.0`   |               1022 |
| `/21` | `255.255.248.0`   |               2046 |
| `/20` | `255.255.240.0`   |               4094 |
| `/19` | `255.255.224.0`   |               8190 |
| `/18` | `255.255.192.0`   |              16382 |
| `/17` | `255.255.128.0`   |              32766 |
| `/16` | `255.255.0.0`     |              65534 |
| `/15` | `255.254.0.0`     |             131070 |
| `/14` | `255.252.0.0`     |             262142 |
| `/13` | `255.248.0.0`     |             524286 |
| `/12` | `255.240.0.0`     |            1048574 |
| `/11` | `255.224.0.0`     |            2097150 |
| `/10` | `255.192.0.0`     |            4194302 |
| `/9`  | `255.128.0.0`     |            8388606 |
| `/8`  | `255.0.0.0`       |           16777214 |

> *Valeurs classiques pour les sous-réseaux IPv4, en excluant l'adresse réseau et l'adresse de broadcast. Certains préfixes, notamment `/31`, ont un comportement particulier.

---

# 📡 Commandes réseau

Quelques commandes importantes à connaître :

| Commande           | Utilisation                                      |
| ------------------ | ------------------------------------------------ |
| `ip addr`          | Afficher les adresses IP des interfaces.         |
| `ip link`          | Afficher les interfaces réseau.                  |
| `ip route`         | Afficher la table de routage.                    |
| `ping hôte`        | Tester la connectivité avec un hôte.             |
| `traceroute hôte`  | Observer le chemin réseau vers un hôte.          |
| `ss`               | Afficher les sockets et connexions réseau.       |
| `hostname -I`      | Afficher les adresses IP locales.                |
| `dig domaine`      | Interroger le DNS.                               |
| `nslookup domaine` | Effectuer une résolution DNS.                    |
| `curl URL`         | Effectuer une requête vers une ressource réseau. |

---

# 💻 Cisco IOS

Quelques commandes Cisco utiles pour mes notes réseau.

Ces commandes sont exécutées dans l'interface **Cisco IOS** et non directement dans un terminal Linux.

| Commande                             | Description                           |
| ------------------------------------ | ------------------------------------- |
| `enable`                             | Passe en mode privilégié.             |
| `conf t`                             | Passe en mode configuration globale.  |
| `show running-config`                | Affiche la configuration active.      |
| `show startup-config`                | Affiche la configuration sauvegardée. |
| `show ip interface`                  | Affiche les interfaces IP.            |
| `show ip route`                      | Affiche la table de routage.          |
| `show access-lists`                  | Affiche les ACL.                      |
| `show version`                       | Affiche la version IOS.               |
| `terminal length 0`                  | Désactive la pagination.              |
| `copy running-config startup-config` | Sauvegarde la configuration.          |

---

# 📌 Évolution de mes notes

Ce document est **volontairement évolutif**.

Il sera complété progressivement avec :

* de nouvelles commandes ;
* de nouveaux concepts Linux ;
* de nouvelles notions réseau ;
* de nouvelles méthodes d'administration ;
* des exemples pratiques ;
* des notes issues de mes études ;
* des résultats issus de mes expérimentations.

De nouvelles sections pourront notamment être ajoutées autour de :

* 🐧 Administration Linux avancée
* 🔐 Sécurité Linux
* 🗄️ Administration serveur
* 🌐 Réseaux
* 🐚 Bash avancé
* 🐍 Python sous Linux
* 🗃️ Bases de données
* 🐳 Conteneurs
* ☁️ Administration cloud
* 🛠️ Automatisation

---

# ⚠️ Note importante pour le document Kali Linux

Ce document constitue le **socle Linux** nécessaire pour comprendre la suite.

Le futur document **🐉 Kali Linux — Notes Cybersécurité & Pentest** sera consacré aux outils et aux techniques de cybersécurité.

> **📚 Prérequis — Kali Linux**
>
> Avant de commencer le document Kali Linux, il est recommandé d'avoir **lu et compris ce document Linux — Administration & Bases**.
>
> Les commandes Linux fondamentales, la gestion des fichiers, les permissions, les utilisateurs, Bash, les processus, les services, le réseau et les autres notions de base ne seront pas systématiquement réexpliqués dans le document Kali Linux.
>
> L'objectif est de pouvoir se concentrer directement sur les **outils, concepts et méthodes de cybersécurité**.

---

# 🛡️ Usage éducatif

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

🐧 **Linux — Administration & Bases**

<br>

🛡️ **Chasseur De Script Kiddies**

<br><br>

📚 **Apprendre. Comprendre. Administrer.**

</p>
