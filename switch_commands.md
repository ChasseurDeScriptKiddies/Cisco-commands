# 🛠️ CCNA Switch Command Cheat-Sheet

> **Auteur :** Chasseur De Script Kiddies  
> **Certification :** CCNA | **Topic :** Switch Configuration

---

## 📋 Table des Matières

1. [Modes de Configuration](#modes-de-configuration)
2. [Commandes Show Essentielles](#commandes-show-essentielles)
3. [Filtrage des Commandes Show](#filtrage-des-commandes-show)
4. [Gestion Multiple d'Interfaces](#gestion-multiple-dinterfaces)
5. [VLANs](#vlans)
6. [Configuration SSH](#configuration-ssh)
7. [Sécurité des Ports](#sécurité-des-ports)
8. [VTP (VLAN Trunking Protocol)](#vtp-vlan-trunking-protocol)
9. [STP (Spanning Tree Protocol)](#stp-spanning-tree-protocol)
10. [EtherChannel](#etherchannel)

---

## 🔰 Modes de Configuration

| Mode | Description | Commande pour passer au mode suivant |
|------|-------------|--------------------------------------|
| `S1>` | EXEC mode | Taper `enable` |
| `S1(config)#` | Global Configuration mode | `configure terminal` |

```bash
# Raccourcis utiles
en, ena                  # enable
conf t, config term      # configure terminal
```

---

## 🔍 Commandes Show Essentielles

```bash
# Syntaxe : utiliser "do" depuis n'importe quel mode
S1(config)# do show <commande>
```

| Commande | Description |
|----------|-------------|
| `show running-config` | Affiche la configuration actuelle |
| `show history` | Affiche l'historique des commandes |
| `show mac address-table` | Affiche la table MAC |
| `show vlan` | Affiche les VLANs configurés |
| `show interface vlan [id]` | Affiche les détails d'une interface VLAN |
| `show interfaces trunk` | Affiche les trunks configurés |

---

## 🔎 Filtrage des Commandes Show

| Paramètre | Effet |
|-----------|-------|
| `\| include <mot>` | Affiche les lignes contenant le mot |
| `\| exclude <mot>` | Exclut les lignes contenant le mot |
| `\| begin <mot>` | Commence à partir de la ligne contenant le mot |

**Exemple :**
```bash
S1# show running-config | include line con
```

---

## 🌐 Gestion Multiple d'Interfaces

### Types d'Interfaces

| Type | Raccourcis possibles |
|------|---------------------|
| `FastEthernet` | `f`, `fa`, `fast` |
| `GigabitEthernet` | `g`, `gi`, `gig` |
| `TenGigabitEthernet` | `t`, `te`, `ten` |

### Exemples de Plages d'Interfaces

```bash
# Plage simple
S1(config)# interface range fastEthernet 0/1-12

# Plage multiple
S1(config)# interface range fastEthernet 0/1-12, 15-24, gigabitEthernet 0/1-2
```

---

## 🏷️ VLANs

### Configuration des VLANs

```bash
# Créer et nommer un VLAN
S1(config)# vlan [vlan-id]
S1(config-vlan)# name [nom-du-vlan]
S1(config-vlan)# exit

# Assigner un port en mode access
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

> ⚠️ **Important :** Retirer le cable ou désactiver l'interface ne supprime PAS l'appartenance au VLAN !

### Configuration des Trunks IEEE 802.1q

```bash
S1(config)# interface [int-id]
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan [liste-vlans]
```

### Dynamic Trunking Protocol (DTP)

| Mode DTP | Comportement |
|----------|--------------|
| `switchport mode access` | Désactive DTP, port en mode static access |
| `switchport mode trunk` | Active DTP, port en mode trunk permanent |
| `switchport mode dynamic auto` | Trunk si l'autre côté le demande |
| `switchport mode dynamic desirable` | Tente de négocier un trunk |

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

# Vérifier les trunks
S1# show interfaces trunk

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

| Violation Mode | Comportement |
|----------------|--------------|
| `protect` | Drop les trames non autorisées (sans notification) |
| `restrict` | Drop les trames + incrementer le compteur de violations |
| `shutdown` | Désactive l'interface (mode par défaut) |

### Modes d'Apprentissage des Adresses MAC

```bash
# Apprentissage dynamique (par défaut)
S1(config-if)# switchport port-security mac-address sticky

# Spécifier une adresse MAC statique
S1(config-if)# switchport port-security mac-address 00:00:00:00:00:00
```

### Réactiver une Interface en Err-Disabled

```bash
# Méthode 1 : Désactiver et réactiver
S1(config)# interface [int-id]
S1(config-if)# shutdown
S1(config-if)# no shutdown

# Méthode 2 : Auto-recovery (si configuré)
S1(config)# errdisable recovery cause psecure-violation
S1(config)# errdisable recovery interval [secondes]
```

---

## 🔄 VTP (VLAN Trunking Protocol)

### Modes VTP

| Mode | Description |
|------|-------------|
| `server` | Par défaut. Crée, modifie et supprime des VLANs |
| `client` | Ne peut pas créer/modifier/supprimer des VLANs |
| `transparent` | Transmet les annonces VTP sans les appliquer |

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
# Méthode 1 : Priorité Bridge (par pas de 4096)
S1(config)# spanning-tree vlan [vlan-id] priority [priorite]

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

> ⚡ **PortFast :** Pour les ports connectes aux hotes (pas de COMPUTER). Désactive le calcul STP sur le port.

> 🛡️ **BPDU Guard :** Désactive le port si un BPDU est reçu (protection contre des switches non autorisés).

### Vérification PortFast/BPDU Guard

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

```bash
# Méthode 1 : LACP (Industry Standard)
S1(config)# interface range [int-range]
S1(config-if-range)# channel-group [numero] mode [active|passive]

# Méthode 2 : PAgP (Cisco Proprietary)
S1(config)# interface range [int-range]
S1(config-if-range)# channel-group [numero] mode [desirable|auto]

# Méthode 3 : Mode ON (Static)
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

| Mode | Protocole | Description |
|------|-----------|-------------|
| `active` | LACP | Active LACP inconditionnellement |
| `passive` | LACP | Passive (attend les requêtes LACP) |
| `desirable` | PAgP | Tente de négocier PAgP |
| `auto` | PAgP | Attend les requêtes PAgP |
| `on` | Static | Active EtherChannel sans protocole |

### Vérification EtherChannel

```bash
S1# show etherchannel summary
S1# show etherchannel port-channel
S1# show interfaces etherchannel
```

---

> 📚 **Source :** Cheat-sheet CCNA Switch Configuration  
> ✍️ **Auteur :** Chasseur De Script Kiddies