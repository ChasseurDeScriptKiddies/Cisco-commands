# 📄 CCNA Router Command Cheat Sheet

*Une référence claire et organisée des commandes essentielles pour l’apprentissage du CCNA (Routing and Switching Essentials).*

**Auteur : Chasseur De Script Kiddies**

---

## 📑 Table des matières

1. [Cisco IOS — Commandes générales](#cisco-ios--commandes-générales)
2. [Commandes `show` essentielles](#commandes-show-essentielles)
3. [Filtrer les sorties des commandes `show`](#filtrer-les-sorties-des-commandes-show)
4. [Routage statique](#routage-statique)
5. [Routage dynamique : OSPF](#routage-dynamique-ospf)
6. [Routage inter-VLAN (Router-on-a-Stick)](#routage-inter-vlan-router-on-a-stick)
7. [Listes de contrôle d'accès (ACL)](#listes-de-contrôle-daccès-acl)
8. [DHCPv4](#dhcpv4)
9. [DHCPv6](#dhcpv6)
10. [NAT (Network Address Translation)](#nat-network-address-translation)
11. [HSRP (Hot Standby Router Protocol)](#hsrp-hot-standby-router-protocol)
12. [Annexes](#annexes)

    * [Valeurs d'Administrative Distance (AD)](#valeurs-dadministrative-distance-ad)
    * [Classes d'adresses IPv4](#classes-dadresses-ipv4)
    * [Plages d'adresses IPv4 privées](#plages-dadresses-ipv4-privées-rfc-1918)
13. [Contenu hérité — CCNA v6](#contenu-hérité--ccna-v6-examen-200-125)

    * [RIPv1](#configuration-ripv1)
    * [RIPv2](#configuration-ripv2)
    * [EIGRP](#configuration-eigrp)
    * [Vérification EIGRP](#vérification-eigrp)
    * [Réglages fins EIGRP](#réglages-fins-eigrp)
    * [EIGRP pour IPv6](#routage-dynamique--eigrp-pour-ipv6)

---

## 💻 Cisco IOS — Commandes générales

Quelques commandes Cisco IOS utiles pour les notes réseau et la configuration courante des routeurs.

| Commande                             | Description                           |
| ------------------------------------ | ------------------------------------- |
| `enable`                             | Passe en mode privilégié.             |
| `conf t`                             | Passe en mode configuration globale.  |
| `show running-config`                | Affiche la configuration active.      |
| `show startup-config`                | Affiche la configuration sauvegardée. |
| `show ip interface`                  | Affiche les interfaces IP.            |
| `show ip route`                      | Affiche la table de routage.          |
| `show access-lists`                  | Affiche les ACL.                      |
| `show version`                       | Affiche la version d'IOS.             |
| `terminal length 0`                  | Désactive la pagination.              |
| `copy running-config startup-config` | Sauvegarde la configuration.          |

---

## Commandes `show` essentielles

*Ces commandes s’exécutent en mode* **Privileged EXEC** (`R1#`).

Pour les exécuter depuis le mode **Global Configuration** (`R1(config)#`), ajoutez `do` devant la commande.

**Exemple :**

```text
R1(config)# do show ip interface brief
```

| Commande                  | Description                                        |
| ------------------------- | -------------------------------------------------- |
| `R1# show running-config` | Affiche la configuration active en RAM             |
| `R1# show startup-config` | Affiche la configuration sauvegardée dans la NVRAM |
| `R1# show history`        | Affiche l’historique des commandes saisies         |

---

## Filtrer les sorties des commandes `show`

Pour filtrer les sorties longues, utilisez le **pipe** (`|`) suivi d’un paramètre de filtrage.

| Paramètre        | Effet                                                              |
| ---------------- | ------------------------------------------------------------------ |
| `section [expr]` | Affiche uniquement la section correspondant à l’expression         |
| `include [expr]` | Inclut uniquement les lignes contenant l’expression                |
| `exclude [expr]` | Exclut les lignes contenant l’expression                           |
| `begin [expr]`   | Affiche toutes les lignes à partir de celle contenant l’expression |

**Exemple :**

```text
R1# show running-config | include line con
```

> 💡 **Astuce :** Par défaut, l’affichage est limité à 24 lignes. Pour modifier ce nombre :
>
> `R1# terminal length [nombre-de-lignes]`
>
> ⚠️ **Non supporté dans Cisco Packet Tracer** (testé sur la version 7.2.2).

---

## Routage statique

Les routes statiques ont une **distance administrative (AD) de 1**.

Toutes les routes statiques se configurent en **mode Global Configuration** :

```text
R1(config)# ip route [adresse-réseau] [masque-sous-réseau] [next-hop-ip|interface-sortie] [AD]
```

### Types de routes statiques

| Type                     | Syntaxe                                              | Utilisation      |                                                           |
| ------------------------ | ---------------------------------------------------- | ---------------- | --------------------------------------------------------- |
| **Route standard**       | `ip route [réseau] [masque] [next-hop-ip             | interface]`      | Route vers un réseau spécifique                           |
| **Route par défaut**     | `ip route 0.0.0.0 0.0.0.0 [next-hop-ip               | interface]`      | Route utilisée lorsqu’aucune correspondance n’est trouvée |
| **Route flottante**      | `ip route [réseau] [masque] [next-hop-ip             | interface] [AD]` | Route de secours avec une AD élevée                       |
| **Route récapitulative** | `ip route [réseau-récap] [masque-récap] [next-hop-ip | interface]`      | Regroupe plusieurs réseaux en une seule route             |

---

## Routage dynamique : OSPF

### Configuration d’OSPF

| Commande                                                   | Description                          |
| ---------------------------------------------------------- | ------------------------------------ |
| `R1(config)# router ospf [PID]`                            | Crée un processus OSPF               |
| `R1(config-router)# router-id [a.b.c.d]`                   | Définit manuellement l’ID du routeur |
| `R1(config-router)# network [réseau] [wildcard] area [ID]` | Annonce un réseau dans une zone OSPF |

> 💡 **Astuce :** Pour lister les réseaux directement connectés :
>
> `R1(config)# do show ip route con`

> 🏆 **Bonne pratique :** Définir manuellement le `router-id`.

### Vérification d’OSPF

| Commande                           | Description                           |
| ---------------------------------- | ------------------------------------- |
| `R1# show ip protocols`            | Affiche les paramètres clés d’OSPF    |
| `R1# show ip route`                | Affiche la table de routage           |
| `R1# show ip route ospf`           | Affiche les routes apprises par OSPF  |
| `R1# show ip ospf neighbor`        | Liste les voisins OSPF                |
| `R1# show ip ospf`                 | Affiche les détails du processus OSPF |
| `R1# show ip ospf interface brief` | Résumé des interfaces OSPF            |
| `R1# show ip ospf interface`       | Détails des interfaces OSPF           |

> 💡 **Rappel :** L’AD par défaut d’OSPF est **110**.

---

## Routage inter-VLAN (Router-on-a-Stick)

Pour activer le routage inter-VLAN, configurez des **sous-interfaces** sur une interface physique.

| Commande                                          | Description                       |
| ------------------------------------------------- | --------------------------------- |
| `R1(config)# interface g0/0.[ID-VLAN]`            | Crée une sous-interface           |
| `R1(config-subif)# encapsulation dot1q [ID-VLAN]` | Associe la sous-interface au VLAN |
| `R1(config-subif)# ip address [IP] [masque]`      | Attribue une adresse IP           |
| `R1(config-subif)# interface g0/0`                | Retourne à l’interface physique   |
| `R1(config-if)# no shutdown`                      | Active l’interface physique       |

---

## Listes de contrôle d'accès (ACL)

### Standard ou étendue ?

* **ACL standard** : placée près de la **destination**, filtre principalement l’adresse source.
* **ACL étendue** : placée près de la **source**, filtre notamment source, destination et protocole.

> 💡 **Mémo :**
>
> **Standard → Destination**
>
> **Étendue → Source**

### ACL standard numérotée

| Commande                                     | Description                 |                     |
| -------------------------------------------- | --------------------------- | ------------------- |
| `R1(config)# access-list [numéro] [permit    | deny] [adresse] [wildcard]` | Crée une entrée ACL |
| `R1(config)# interface [interface]`          | Sélectionne l’interface     |                     |
| `R1(config-if)# ip access-group [numéro] [in | out]`                       | Applique l’ACL      |
| `R1(config)# no access-list [numéro]`        | Supprime l’ACL              |                     |

### ACL standard nommée

```text
R1(config)# ip access-list standard [nom]
```

---

## DHCPv4

### Serveur DHCPv4

| Commande                                             | Description                 |
| ---------------------------------------------------- | --------------------------- |
| `R1(config)# ip dhcp excluded-address [début] [fin]` | Exclut une plage d’adresses |
| `R1(config)# ip dhcp pool [nom]`                     | Crée un pool DHCP           |
| `R1(dhcp-config)# network [réseau] [masque]`         | Définit le réseau           |
| `R1(dhcp-config)# default-router [IP]`               | Définit la passerelle       |
| `R1(dhcp-config)# lease [jours] [heures] [minutes]`  | Définit la durée du bail    |
| `R1(dhcp-config)# dns-server [IP1] [IP2]`            | Définit les serveurs DNS    |
| `R1(dhcp-config)# domain-name [nom]`                 | Définit le nom de domaine   |

### Relais DHCP

```text
R1(config-if)# ip helper-address [IP-DHCP]
```

---

## DHCPv6

*Section à compléter selon les besoins du projet.*

---

## NAT (Network Address Translation)

### NAT statique

```text
R1(config)# ip nat inside source static [IP-locale] [IP-globale]
```

### Interfaces NAT

| Commande                        | Description                 |
| ------------------------------- | --------------------------- |
| `R1(config-if)# ip nat inside`  | Définit l’interface interne |
| `R1(config-if)# ip nat outside` | Définit l’interface externe |

### NAT dynamique

| Commande                                                       | Description                       |
| -------------------------------------------------------------- | --------------------------------- |
| `R1(config)# ip nat pool [nom] [début] [fin] netmask [masque]` | Crée un pool d’adresses publiques |
| `R1(config)# access-list [numéro] permit [réseau] [wildcard]`  | Définit le trafic autorisé        |
| `R1(config)# ip nat inside source list [numéro] pool [nom]`    | Associe l’ACL au pool NAT         |

> ⚠️ **Important :** Les interfaces doivent être configurées avec `inside` et `outside`.

---

## HSRP (Hot Standby Router Protocol)

| Commande                                                | Description                    |
| ------------------------------------------------------- | ------------------------------ |
| `R1(config-if)# standby version 2`                      | Définit la version HSRP        |
| `R1(config-if)# standby [groupe] ip [IP-virtuelle]`     | Définit l’adresse IP virtuelle |
| `R1(config-if)# standby [groupe] priority [0-255]`      | Définit la priorité            |
| `R1(config-if)# standby [groupe] preempt`               | Active la préemption           |
| `R1(config-if)# standby [groupe] timers [hello] [hold]` | Définit les temporisateurs     |

> ⚠️ Utilisez la même version d’HSRP sur les équipements concernés.

---

# Annexes

## Valeurs d’Administrative Distance (AD)

| Code | Type                        | Valeur AD |
| ---- | --------------------------- | --------- |
| `C`  | Réseau directement connecté | `0`       |
| `S`  | Route statique              | `1`       |
| `D`  | EIGRP interne               | `90`      |
| `O`  | OSPF                        | `110`     |
| `R`  | RIP                         | `120`     |

---

## Classes d’adresses IPv4

| Classe | Plage du 1er octet | Utilisation     |
| ------ | ------------------ | --------------- |
| A      | `1 - 127`          | Réseaux étendus |
| B      | `128 - 191`        | Réseaux moyens  |
| C      | `192 - 223`        | Réseaux locaux  |
| D      | `224 - 239`        | Multicast       |
| E      | `240 - 255`        | Réservé         |

---

## Plages d’adresses IPv4 privées (RFC 1918)

| Classe | CIDR             | Plage                             |
| ------ | ---------------- | --------------------------------- |
| A      | `10.0.0.0/8`     | `10.0.0.0` à `10.255.255.255`     |
| B      | `172.16.0.0/12`  | `172.16.0.0` à `172.31.255.255`   |
| C      | `192.168.0.0/16` | `192.168.0.0` à `192.168.255.255` |

---

# Contenu hérité — CCNA v6 (examen 200-125)

> ⚠️ Ces sujets proviennent d’anciennes versions du CCNA.

## Routage dynamique : RIP

### Configuration RIPv1

| Commande                                           | Description                                    |
| -------------------------------------------------- | ---------------------------------------------- |
| `R1(config)# router rip`                           | Active RIP                                     |
| `R1(config-router)# network [réseau]`              | Annonce un réseau                              |
| `R1(config-router)# passive-interface [interface]` | Empêche les mises à jour RIP sur une interface |

### Configuration RIPv2

| Commande                             | Description                       |
| ------------------------------------ | --------------------------------- |
| `R1(config-router)# version 2`       | Active RIPv2                      |
| `R1(config-router)# no auto-summary` | Désactive la synthèse automatique |

---

## Routage dynamique : EIGRP

### Configuration EIGRP

| Commande                                         | Description             |
| ------------------------------------------------ | ----------------------- |
| `R1(config)# router eigrp [AS]`                  | Active EIGRP            |
| `R1(config-router)# eigrp router-id [a.b.c.d]`   | Définit l’ID du routeur |
| `R1(config-router)# network [réseau] [wildcard]` | Annonce un réseau       |

> 💡 Pour lister les réseaux directement connectés :
>
> `R1(config)# do show ip route con`

### Vérification EIGRP

| Commande                      | Description                  |
| ----------------------------- | ---------------------------- |
| `R1# show ip protocols`       | Affiche les paramètres EIGRP |
| `R1# show ip eigrp neighbors` | Liste les voisins EIGRP      |
| `R1# show ip route eigrp`     | Affiche les routes EIGRP     |

> 💡 **Rappel :** L’AD par défaut d’EIGRP est **90**.

### Réglages fins EIGRP

| Commande                               | Description                                           |
| -------------------------------------- | ----------------------------------------------------- |
| `R1(config-router)# variance [valeur]` | Active l’équilibrage de charge inégal                 |
| `R1(config-router)# auto-summary`      | Active la synthèse automatique                        |
| `R1(config-if)# bandwidth [kbit]`      | Modifie la bande passante utilisée dans les métriques |

---

## Routage dynamique : EIGRP pour IPv6

| Commande                                    | Description                    |
| ------------------------------------------- | ------------------------------ |
| `R1(config)# ipv6 unicast-routing`          | Active le routage IPv6         |
| `R1(config)# ipv6 router eigrp [AS]`        | Active EIGRP pour IPv6         |
| `R1(config-rtr)# eigrp router-id [a.b.c.d]` | Définit l’ID du routeur        |
| `R1(config-if)# ipv6 eigrp [AS]`            | Active EIGRP sur une interface |

---

**📌 Réalisé par Chasseur De Script Kiddies**
