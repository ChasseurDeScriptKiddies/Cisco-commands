# 🛠️ CCNA Switch Command Cheat-Sheet

> **Auteur :** Chasseur De Script Kiddies
> **Sujet :** Switch Configuration

---

## 📋 Table des Matières

1. [Modes de Configuration](#modes-de-configuration)
2. [Commandes Cisco IOS de Base](#commandes-cisco-ios-de-base)
3. [Commandes Show Essentielles](#commandes-show-essentielles)
4. [Filtrage des Commandes Show](#filtrage-des-commandes-show)
5. [Gestion Multiple d'Interfaces](#gestion-multiple-dinterfaces)
6. [VLANs](#vlans)
7. [Configuration SSH](#configuration-ssh)
8. [Sécurité des Ports](#sécurité-des-ports)
9. [VTP (VLAN Trunking Protocol)](#vtp-vlan-trunking-protocol)
10. [STP (Spanning Tree Protocol)](#stp-spanning-tree-protocol)
11. [EtherChannel](#etherchannel)

---

## 🔰 Modes de Configuration

| Mode          | Description               | Commande pour passer au mode suivant |
| ------------- | ------------------------- | ------------------------------------ |
| `S1>`         | EXEC mode                 | Taper `enable`                       |
| `S1#`         | EXEC privilégié           | Taper `configure terminal`           |
| `S1(config)#` | Global Configuration mode | Taper une commande de configuration  |

```bash
# Passer en mode privilégié
S1> enable

# Passer en configuration globale
S1# configure terminal
```

### Raccourcis utiles

```bash
en, ena                  # enable

conf t, config term      # configure terminal

exit                     # Revenir au mode précédent

end                      # Revenir directement au mode privilégié

Ctrl+Z                   # Revenir directement au mode privilégié
```

---

## 💻 Commandes Cisco IOS de Base

### Commandes essentielles

| Commande                             | Description                                          |
| ------------------------------------ | ---------------------------------------------------- |
| `enable`                             | Passe en mode privilégié                             |
| `conf t`                             | Passe en mode configuration globale                  |
| `show running-config`                | Affiche la configuration active en RAM               |
| `show startup-config`                | Affiche la configuration sauvegardée en NVRAM        |
| `show ip interface`                  | Affiche les informations IP des interfaces           |
| `show version`                       | Affiche la version d'IOS et les informations système |
| `terminal length 0`                  | Désactive la pagination de l'affichage               |
| `copy running-config startup-config` | Sauvegarde la configuration                          |
| `reload`                             | Redémarre le switch                                  |

### Sauvegarder la configuration

```bash
# Sauvegarder la configuration active dans la NVRAM

S1# copy running-config startup-config
```

Raccourci :

```bash
S1# copy run start
```

### Désactiver la résolution DNS

```bash
S1(config)# no ip domain-lookup
```

> 💡 Utile pour éviter qu'une faute de frappe soit interprétée comme un nom DNS.

### CDP et LLDP

```bash
# Activer Cisco Discovery Protocol
S1(config)# cdp run

# Activer Link Layer Discovery Protocol
S1(config)# lldp run
```

Vérification :

```bash
S1# show cdp neighbors
S1# show lldp neighbors
```

---

## 🔍 Commandes Show Essentielles

Ces commandes s'exécutent généralement en mode **Privileged EXEC** (`S1#`).

Depuis le mode configuration globale, utilisez `do` :

```bash
S1(config)# do show <commande>
```

| Commande                   | Description                                        |
| -------------------------- | -------------------------------------------------- |
| `show running-config`      | Affiche la configuration actuelle                  |
| `show startup-config`      | Affiche la configuration sauvegardée               |
| `show history`             | Affiche l'historique des commandes                 |
| `show version`             | Affiche les informations système et la version IOS |
| `show ip interface`        | Affiche les informations IP des interfaces         |
| `show ip interface brief`  | Résumé des interfaces et de leur état              |
| `show interfaces`          | Affiche les détails des interfaces                 |
| `show mac address-table`   | Affiche la table MAC                               |
| `show vlan`                | Affiche les VLANs configurés                       |
| `show interface vlan [id]` | Affiche les détails d'une interface VLAN           |
| `show interfaces trunk`    | Affiche les trunks configurés                      |
| `show cdp neighbors`       | Affiche les voisins CDP                            |
| `show lldp neighbors`      | Affiche les voisins LLDP                           |

### Désactiver la pagination

```bash
S1# terminal length 0
```

> 💡 Pratique pour afficher une configuration complète sans devoir appuyer sur `Entrée` ou `Espace`.

---

## 🔎 Filtrage des Commandes Show

| Paramètre          | Effet                                          |
| ------------------ | ---------------------------------------------- |
| `\| include <mot>` | Affiche les lignes contenant le mot            |
| `\| exclude <mot>` | Exclut les lignes contenant le mot             |
| `\| begin <mot>`   | Commence à partir de la ligne contenant le mot |
| `\| section <mot>` | Affiche une section correspondant au mot       |

### Exemple

```bash
S1# show running-config | include line con
```

Autre exemple :

```bash
S1# show running-config | section interface
```

---

## 🌐 Gestion Multiple d'Interfaces

### Types d'Interfaces

| Type                 | Raccourcis possibles |
| -------------------- | -------------------- |
| `FastEthernet`       | `f`, `fa`, `fast`    |
| `GigabitEthernet`    | `g`, `gi`, `gig`     |
| `TenGigabitEthernet` | `t`, `te`, `ten`     |

### Exemples de Plages d'Interfaces

```bash
# Plage simple

S1(config)# interface range fastEthernet 0/1-12
```

```bash
# Plage multiple

S1(config)# interface range fastEthernet 0/1-12, 15-24, gigabitEthernet 0/1-2
```

### Description d'une Interface

```bash
S1(config)# interface gigabitEthernet 0/1

S1(config-if)# description <description>
```

---

## 🏷️ VLANs

### Configuration des VLANs

```bash
# Créer et nommer un VLAN

S1(config)# vlan [vlan-id]

S1(config-vlan)# name [nom-du-vlan]

S1(config-vlan)# exit
```

### Assigner un Port en Mode Access

```bash
S1(config)# interface [int-id]

S1(config-if)# switchport mode access

S1(config-if)# switchport access vlan [vlan-id]
```

### Suppression d'un VLAN

```bash
S1(config)# no vlan [vlan-id]
```

### Retirer des Interfaces d'un VLAN

```bash
S1(config)# interface [int-id]

S1(config-if)# no switchport access vlan [vlan-id]
```

> ⚠️ **Important :** Retirer le câble ou désactiver l'interface ne supprime PAS l'appartenance au VLAN.

### Configuration des Trunks IEEE 802.1Q

```bash
S1(config)# interface [int-id]

S1(config-if)# switchport mode trunk

S1(config-if)# switchport trunk allowed vlan [liste-vlans]
```

### Dynamic Trunking Protocol (DTP)

| Mode DTP                            | Comportement                                  |
| ----------------------------------- | --------------------------------------------- |
| `switchport mode access`            | Désactive DTP et force le port en mode access |
| `switchport mode trunk`             | Force le port en mode trunk                   |
| `switchport mode dynamic auto`      | Devient trunk si l'autre côté le demande      |
| `switchport mode dynamic desirable` | Tente activement de négocier un trunk         |

### Voice VLANs

```bash
S1(config)# interface [int-id]

S1(config-if)# switchport mode access

S1(config-if)# switchport access vlan [vlan-id]

S1(config-if)# switchport voice vlan [voice-vlan-id]
```

### Dépannage VLANs

```bash
# Vérifier les VLANs

S1# show vlan
```

```bash
# Vérifier les trunks

S1# show interfaces trunk
```

```bash
# Vérifier la table MAC

S1# show mac address-table
```

---

## 🔐 Configuration SSH

```bash
# Étape 1 : Définir le domain name

S1(config)# ip domain-name [domain-name]

# Étape 2 : Générer les clés RSA

S1(config)# crypto key generate rsa

# Étape 3 : Créer un utilisateur local

S1(config)# username [admin] secret [password]

# Étape 4 : Configurer les lignes VTY

S1(config)# line vty 0 15

S1(config-line)# transport input ssh

S1(config-line)# login local

S1(config-line)# exit

# Étape 5 : Activer SSH version 2

S1(config)# ip ssh version 2
```

### Modification de la Configuration SSH

```bash
# Définir le timeout SSH

S1(config)# ip ssh time-out [secondes]

# Définir le nombre de tentatives d'authentification

S1(config)# ip ssh authentication-retries [nombre]
```

### Vérification SSH

```bash
S1# show ip ssh

S1# show ssh
```

---

## 🛡️ Sécurité des Ports

### Activation de la Port Security

```bash
S1(config)# interface [int-id]

S1(config-if)# switchport mode access

S1(config-if)# switchport port-security

S1(config-if)# switchport port-security maximum [nombre-adresses]

S1(config-if)# switchport port-security violation [protect|restrict|shutdown]

S1(config-if)# switchport port-security mac-address [adresse-MAC]
```

### Modes de Violation

| Violation Mode | Comportement                                            |
| -------------- | ------------------------------------------------------- |
| `protect`      | Drop les trames non autorisées sans notification        |
| `restrict`     | Drop les trames et incrémente le compteur de violations |
| `shutdown`     | Désactive l'interface (mode par défaut)                 |

### Modes d'Apprentissage des Adresses MAC

```bash
# Apprentissage avec Sticky MAC

S1(config-if)# switchport port-security mac-address sticky
```

```bash
# Spécifier une adresse MAC statique

S1(config-if)# switchport port-security mac-address 0000.0000.0000
```

### Réactiver une Interface en Err-Disabled

```bash
# Méthode 1 : Désactiver et réactiver

S1(config)# interface [int-id]

S1(config-if)# shutdown

S1(config-if)# no shutdown
```

### Auto-Recovery

```bash
S1(config)# errdisable recovery cause psecure-violation

S1(config)# errdisable recovery interval [secondes]
```

---

## 🔄 VTP (VLAN Trunking Protocol)

### Modes VTP

| Mode          | Description                                        |
| ------------- | -------------------------------------------------- |
| `server`      | Peut créer, modifier et supprimer des VLANs        |
| `client`      | Ne peut pas créer, modifier ou supprimer des VLANs |
| `transparent` | Transmet les annonces VTP sans les appliquer       |

### Configuration VTP

```bash
S1(config)# vtp mode [server|client|transparent]

S1(config)# vtp domain [nom-du-domaine]

S1(config)# vtp password [password]

S1(config)# vtp version [1|2|3]
```

### Vérification VTP

```bash
S1# show vtp status

S1# show vtp password
```

---

## 🌳 STP (Spanning Tree Protocol)

### Configuration de la Priorité Bridge

```bash
# Méthode 1 : Priorité Bridge

S1(config)# spanning-tree vlan [vlan-id] priority [priorite]
```

```bash
# Méthode 2 : Devenir Root Bridge

S1(config)# spanning-tree vlan [vlan-id] root [primary|secondary]
```

### Vérification du Bridge ID

```bash
S1# show spanning-tree

S1# show spanning-tree vlan [vlan-id]
```

### PortFast et BPDU Guard

```bash
S1(config)# interface [int-id]

S1(config-if)# spanning-tree portfast

S1(config-if)# spanning-tree bpduguard enable
```

> ⚡ **PortFast :** À utiliser sur les ports connectés aux hôtes. Permet au port d'atteindre rapidement l'état Forwarding.

> 🛡️ **BPDU Guard :** Désactive le port si un BPDU est reçu, afin de protéger le réseau contre des switches non autorisés.

### Vérification PortFast / BPDU Guard

```bash
S1# show spanning-tree interface [int-id] detail
```

### Configuration Rapid PVST+

```bash
S1(config)# spanning-tree mode rapid-pvst
```

### Commandes de Vérification STP

```bash
S1# show spanning-tree

S1# show spanning-tree summary

S1# show spanning-tree interface [int-id]
```

---

## 🔗 EtherChannel

### Création d'un EtherChannel

#### Méthode 1 : LACP — Standard IEEE

```bash
S1(config)# interface range [int-range]

S1(config-if-range)# channel-group [numero] mode [active|passive]
```

#### Méthode 2 : PAgP — Propriétaire Cisco

```bash
S1(config)# interface range [int-range]

S1(config-if-range)# channel-group [numero] mode [desirable|auto]
```

#### Méthode 3 : Mode ON — Statique

```bash
S1(config)# interface range [int-range]

S1(config-if-range)# channel-group [numero] mode on
```

### Configuration de l'Interface PortChannel

```bash
S1(config)# interface port-channel [numero]

S1(config-if)# switchport mode trunk

S1(config-if)# switchport trunk allowed vlan [liste-vlans]
```

### Modes EtherChannel Disponibles

| Mode        | Protocole | Description                        |
| ----------- | --------- | ---------------------------------- |
| `active`    | LACP      | Active LACP                        |
| `passive`   | LACP      | Attend les requêtes LACP           |
| `desirable` | PAgP      | Tente de négocier PAgP             |
| `auto`      | PAgP      | Attend les requêtes PAgP           |
| `on`        | Static    | Active EtherChannel sans protocole |

### Vérification EtherChannel

```bash
S1# show etherchannel summary

S1# show etherchannel port-channel

S1# show interfaces etherchannel
```

---

> 📚 **Source :** Cheat-Sheet CCNA Switch Configuration
> ✍️ **Auteur :** Chasseur De Script Kiddies
