# 📄 CCNA Router Command Cheat Sheet
*Une référence claire et organisée des commandes essentielles pour le CCNA (Routing and Switching Essentials).*
**Auteur : Chasseur De Script Kiddies**

---

## Table des matières
1. [Commandes `show` essentielles](#commandes-show-essentielles)
2. [Filtrer les sorties des commandes `show`](#filtrer-les-sorties-des-commandes-show)
3. [Routage statique](#routage-statique)
4. [Routage dynamique : OSPF](#routage-dynamique-ospf)
5. [Routage inter-VLAN (Router-on-a-Stick)](#routage-inter-vlan-router-on-a-stick)
6. [Listes de contrôle d'accès (ACL)](#listes-de-contrôle-daccès-acl)
7. [DHCPv4](#dhcpv4)
8. [DHCPv6](#dhcpv6)
9. [NAT (Network Address Translation)](#nat)
10. [HSRP (Hot Standby Router Protocol)](#hsrp)
11. [Annexes](#annexes)
    - [Valeurs d/Admin Distance (AD)](#valeurs-dadministrative-distance)
    - [Classes d’adresses IPv4](#classes-dadresses-ipv4)
    - [Plages d’adresses IPv4 privées](#plages-dadresses-ipv4-privées)
12. [Contenu hérité (CCNA v6, examen 200-125)](#contenu-hérité-ccna-v6-examen-200-125)

---

## Commandes `show` essentielles
*Ces commandes s’exécutent en mode **Privileged EXEC** (`R1#`).*
Pour les exécuter depuis le mode **Global Configuration** (`R1(config)#`), ajoutez `do` devant la commande.
**Exemple** : `R1(config)# do show ip interface brief`

| Commande | Description |
|----------|-------------|
| `R1# show running-config` | Affiche la configuration active en RAM |
| `R1# show startup-config` | Affiche la configuration sauvegardée dans la NVRAM |
| `R1# show history` | Affiche l’historique des commandes saisies |

---

## Filtrer les sorties des commandes `show`
Pour filtrer les sorties longues (ex: `show running-config`), utilisez le **pipe** (`|`) suivi d’un paramètre de filtrage.

| Paramètre | Effet |
|-----------|-------|
| `section [expr]` | Affiche uniquement la section correspondant à l’expression |
| `include [expr]` | Inclut uniquement les lignes contenant l’expression |
| `exclude [expr]` | Exclut les lignes contenant l’expression |
| `begin [expr]` | Affiche toutes les lignes **à partir** de celle contenant l’expression |

**Exemple** :
`R1# show running-config | include line con`

> 💡 **Astuce** : Par défaut, l’affichage est limité à 24 lignes. Pour modifier ce nombre :
> `R1# terminal length [nombre-de-lignes]`
> ⚠️ **Non supporté** dans Cisco Packet Tracer (testé sur la version 7.2.2).

---

## Routage statique
Les routes statiques ont une **distance administrative (AD) de 1**, ce qui les rend très fiables après les routes directement connectées.

Toutes les routes statiques se configurent en **mode Global Configuration** avec la commande :
`R1(config)# ip route [adresse-réseau] [masque-sous-réseau] [next-hop-ip|interface-sortie] [AD]`

### Types de routes statiques

| Type | Syntaxe | Utilisation |
|------|---------|-------------|
| **Route standard** | `ip route [réseau] [masque] [next-hop-ip|interface]` | Route basique vers un réseau spécifique |
| **Route par défaut** | `ip route 0.0.0.0 0.0.0.0 [next-hop-ip|interface]` | Route utilisée lorsque aucune autre correspondance n’est trouvée |
| **Route flottante** | `ip route [réseau] [masque] [next-hop-ip|interface] [AD]` | Route de secours avec une AD élevée (ex: 200) |
| **Route récapitulative** | `ip route [réseau-récap] [masque-récap] [next-hop-ip|interface]` | Regroupe plusieurs réseaux en une seule route (ex: `192.168.0.0 255.255.0.0`) |

---

## Routage dynamique : OSPF

### Configuration d’OSPF

| Commande | Description |
|----------|-------------|
| `R1(config)# router ospf [PID]` | Crée un processus OSPF avec un **Process ID** (1 à 65535) |
| `R1(config-router)# router-id [a.b.c.d]` | Définit manuellement l’**ID du routeur** (format IPv4) |
| `R1(config-router)# network [réseau] [wildcard] area [ID]` | Annonce un réseau directement connecté dans une **zone OSPF** |

> 💡 **Astuce** : Pour lister les réseaux directement connectés :
> `R1(config)# do show ip route con`

> 🏆 **Meilleure pratique** : Toujours définir manuellement l’**ID du routeur** pour éviter les conflits dans une zone.

---
### Vérification d’OSPF

| Commande | Description |
|----------|-------------|
| `R1# show ip protocols` | Affiche les paramètres clés d’OSPF (PID, ID, voisins, AD) |
| `R1# show ip route` | Affiche la table de routage (utilisez `show ip route ospf` pour filtrer) |
| `R1# show ip ospf neighbor` | Liste les **voisins OSPF** du routeur |
| `R1# show ip ospf` | Affiche les détails du processus OSPF (PID, ID, dernière exécution du SPF) |
| `R1# show ip ospf interface brief` | Résumé des interfaces OSPF activées |
| `R1# show ip ospf interface` | Détails des interfaces OSPF |

> 💡 **Rappel** : L’AD par défaut d’OSPF est **110**.

---
## Routage inter-VLAN (Router-on-a-Stick)
Pour activer le routage inter-VLAN, configurez des **sous-interfaces** (une par VLAN) sur une seule interface physique.

| Commande | Description |
|----------|-------------|
| `R1(config)# interface g0/0.[ID-VLAN]` | Crée une sous-interface pour le VLAN spécifié |
| `R1(config-subif)# encapsulation dot1q [ID-VLAN]` | Associe la sous-interface au VLAN (utilisez `native` pour le VLAN natif) |
| `R1(config-subif)# ip address [IP] [masque]` | Attribue une adresse IP à la sous-interface |
| `R1(config-subif)# interface g0/0` | Retourne à l’interface physique pour l’activer |
| `R1(config-if)# no shutdown` | Active l’interface physique (et toutes ses sous-interfaces) |

---
## Listes de contrôle d’accès (ACL)

### Standard ou étendue ?
- **ACL standard** : Placée **près de la destination** (filtre par adresse source)
- **ACL étendue** : Placée **près de la source** (filtre par adresse source **et** destination)

> 💡 **Astuce mnémotechnique** :
> - **Standard → Destination** (S → D)
> - **Étendue → Source** (E → S)

> 💡 **Rappel** : Une seule ACL par interface/protocole/direction (inbound/outbound)

---
### Configuration d’une ACL standard numérotée

| Commande | Description |
|----------|-------------|
| `R1(config)# access-list [numéro] [permit|deny] [adresse] [wildcard]` | Crée une entrée dans l’ACL |
| `R1(config)# interface [interface]` | Sélectionne l’interface pour appliquer l’ACL |
| `R1(config-if)# ip access-group [numéro] [in|out]` | Active l’ACL sur l’interface |
| `R1(config)# no access-list [numéro]` | ⚠️ **Supprime l’ACL** (toutes les entrées sont effacées) |

> 🏆 **Meilleure pratique** : **Toujours sauvegarder** vos ACL avant toute modification

---
### Configuration d’une ACL standard nommée

| Commande | Description |
|----------|-------------|
| `R1(config)# ip access-list standard [nom]` | Crée une ACL nommée (passe en mode configuration d’ACL) |

---
## DHCPv4
### Configurer un routeur Cisco comme serveur DHCPv4

| Commande | Description |
|----------|-------------|
| `R1(config)# ip dhcp excluded-address [début] [fin]` | Exclut une plage d’adresses (non attribuables) |
| `R1(config)# ip dhcp pool [nom]` | Crée un pool d’adresses DHCP |
| `R1(dhcp-config)# network [réseau] [masque]` | Définit le réseau et le masque pour le pool |
| `R1(dhcp-config)# default-router [IP]` | Définit la passerelle par défaut pour les hôtes |
| `R1(dhcp-config)# lease [jours] [heures] [minutes]` | Définit la durée de location des adresses |
| `R1(dhcp-config)# dns-server [IP1] [IP2]` | Définit les serveurs DNS (jusqu’à 8) |
| `R1(dhcp-config)# domain-name [nom]` | Définit le nom de domaine |

### Relayer les requêtes DHCP (si le serveur est distant)

| Commande | Description |
|----------|-------------|
| `R1(config-if)# ip helper-address [IP-DHCP]` | Relaye les requêtes DHCP vers un serveur distant |

---
## DHCPv6
*(Section à compléter selon vos besoins spécifiques)*

---
## NAT (Network Address Translation)
### NAT statique

| Commande | Description |
|----------|-------------|
| `R1(config)# ip nat inside source static [IP-locale] [IP-globale]` | Mappe une adresse locale à une adresse globale |

### Définir les interfaces interne/externes

| Commande | Description |
|----------|-------------|
| `R1(config-if)# ip nat inside` | Marque une interface comme **interne** (réseau local) |
| `R1(config-if)# ip nat outside` | Marque une interface comme **externe** (réseau public) |

### NAT dynamique

| Commande | Description |
|----------|-------------|
| `R1(config)# ip nat pool [nom] [début] [fin] netmask [masque]` | Crée un pool d’adresses publiques |
| `R1(config)# access-list [numéro] permit [réseau] [wildcard]` | Définit les adresses locales autorisées à utiliser le NAT |
| `R1(config)# ip nat inside source list [numéro] pool [nom]` | Lie le pool d’adresses au trafic local |

> ⚠️ **Ne pas oublier** de marquer les interfaces comme `inside`/`outside` !

---
## HSRP (Hot Standby Router Protocol)

| Commande | Description |
|----------|-------------|
| `R1(config-if)# standby version 2` | Spécifie la version 2 d’HSRP (supporte IPv6) |
| `R1(config-if)# standby [groupe] ip [IP-virtuelle]` | Définit l’adresse IP virtuelle du groupe |
| `R1(config-if)# standby [groupe] priority [0-255]` | Définit la priorité pour élire le routeur **actif** |
| `R1(config-if)# standby [groupe] preempt` | Active la **préemption** (le routeur avec la priorité la plus élevée devient actif) |
| `R1(config-if)# standby [groupe] timers [hello] [hold]` | Définit les temporisateurs **hello** et **hold** |

> ⚠️ **Important** : Utilisez **la même version d’HSRP** dans tout votre réseau pour éviter les conflits

---
## Annexes

### Valeurs d’Administrative Distance (AD)

| Code | Type | Valeur AD |
|------|------|-----------|
| `C` | Réseau directement connecté | `0` |
| `S` | Route statique | `1` |
| `D` | EIGRP interne | `90` |
| `O` | OSPF | `110` |
| `R` | RIP | `120` |

---
### Classes d’adresses IPv4

| Classe | Plage (1er octet) | Utilisation |
|--------|-------------------|-------------|
| A | `1 - 127` | Réseaux étendus (ex: `10.0.0.0/8`) |
| B | `128 - 191` | Réseaux moyens (ex: `172.16.0.0/12`) |
| C | `192 - 223` | Réseaux locaux (ex: `192.168.0.0/16`) |
| D | `224 - 239` | **Multicast** |
| E | `240 - 255` | Réservé (recherche) |

---
### Plages d’adresses IPv4 privées (RFC 1918)

| Classe | CIDR | Plage |
|--------|------|-------|
| A | `10.0.0.0/8` | `10.0.0.0` à `10.255.255.255` |
| B | `172.16.0.0/12` | `172.16.0.0` à `172.31.255.255` |
| C | `192.168.0.0/16` | `192.168.0.0` à `192.168.255.255` |

---
## Contenu hérité (CCNA v6, examen 200-125)
⚠️ **Attention** : Ces sujets ne sont plus au programme du **CCNA 200-301 (version 7)**.

---
### Routage dynamique : RIP
#### Configuration RIPv1

| Commande | Description |
|----------|-------------|
| `R1(config)# router rip` | Active le protocole RIP |
| `R1(config-router)# network [réseau]` | Annonce un réseau (sans masque de sous-réseau) |
| `R1(config-router)# passive-interface [interface]` | Empêche l’envoi de mises à jour RIP sur une interface |

> 💡 **Rappel** : RIPv1 ne supporte pas le **VLSM** (les réseaux doivent être contigus)

---
#### Configuration RIPv2

| Commande | Description |
|----------|-------------|
| `R1(config-router)# version 2` | Passe en version 2 (supporte le VLSM et les masques de sous-réseau) |
| `R1(config-router)# no auto-summary` | Désactive la **synthèse automatique** des réseaux |

---
### Routage dynamique : EIGRP
#### Configuration EIGRP

| Commande | Description |
|----------|-------------|
| `R1(config)# router eigrp [AS]` | Active EIGRP avec un **numéro de système autonome (AS)** |
| `R1(config-router)# eigrp router-id [a.b.c.d]` | Définit manuellement l’**ID du routeur** |
| `R1(config-router)# network [réseau] [wildcard]` | Annonce un réseau directement connecté |

> 💡 **Astuce** : Pour lister les réseaux directement connectés :
> `R1(config)# do show ip route con`

---
#### Vérification EIGRP

| Commande | Description |
|----------|-------------|
| `R1# show ip protocols` | Affiche les paramètres EIGRP (AS, ID, voisins, AD) |
| `R1# show ip eigrp neighbors` | Liste les **voisins EIGRP** |
| `R1# show ip route eigrp` | Affiche uniquement les routes apprises par EIGRP |

> 💡 **Rappel** : L’AD par défaut d’EIGRP est **90** (5 pour les routes récapitulatives, 170 pour les routes externes)

---
#### Réglages fins EIGRP

| Commande | Description |
|----------|-------------|
| `R1(config-router)# variance [valeur]` | Active l’**équilibrage de charge inégal** (valeur : 1 à 128) |
| `R1(config-router)# auto-summary` | Active la **synthèse automatique** des réseaux |
| `R1(config-if)# bandwidth [kbit]` | Modifie la **bande passante** (affecte les métriques EIGRP) |

---
### Routage dynamique : EIGRP pour IPv6

| Commande | Description |
|----------|-------------|
| `R1(config)# ipv6 unicast-routing` | Active le routage IPv6 |
| `R1(config)# ipv6 router eigrp [AS]` | Active EIGRP pour IPv6 |
| `R1(config-rtr)# eigrp router-id [a.b.c.d]` | Définit l’ID du routeur pour EIGRP IPv6 |
| `R1(config-if)# ipv6 eigrp [AS]` | Active EIGRP sur une interface IPv6 |

---
**📌 Réalisé par Chasseur De Script Kiddies**
*Document optimisé pour la clarté et la praticité.*