# 🛠️ Configuration Initiale Cisco

> **Auteur :** Chasseur De Script Kiddies
> **Sujet :** Configuration Cisco de base

---

## 📋 Table des Matières

1. [Accès au Mode Privilégié](#accès-au-mode-privilégié)
2. [Configuration Console](#configuration-console)
3. [Script de Configuration Rapide](#script-de-configuration-rapide)
4. [Astuces et Bonnes Pratiques](#astuces-et-bonnes-pratiques)

---

## 🚀 Accès au Mode Privilégié

| Commande                      | Description                         |
| ----------------------------- | ----------------------------------- |
| `CDevice> enable`             | Accède au mode privilégié (EXEC)    |
| `CDevice# configure terminal` | Passe en mode Configuration globale |

---

## 🖥️ Configuration Console

| Commande                                    | Description                                       |
| ------------------------------------------- | ------------------------------------------------- |
| `CDevice(config)# line console 0`           | Entre dans le mode de configuration de la console |
| `CDevice(config-line)# logging synchronous` | Synchronise l'affichage des messages système      |
| `CDevice(config-line)# history size 50`     | Définit l'historique à 50 commandes               |
| `CDevice(config-line)# exec-timeout 25 0`   | Déconnexion après 25 minutes d'inactivité         |

---

## 📋 Script de Configuration Rapide

```bash
ena

conf t

no ip domain-lookup

cdp run

lldp run

line con 0

logging syn

his size 50

exec-timeout 25 0

end

copy run start
```

### Commandes clés

| Commande              | Description                               |
| --------------------- | ----------------------------------------- |
| `no ip domain-lookup` | Désactive la résolution DNS               |
| `cdp run`             | Active Cisco Discovery Protocol           |
| `lldp run`            | Active Link Layer Discovery Protocol      |
| `copy run start`      | Sauvegarde la configuration dans la NVRAM |

---

## 💡 Astuces et Bonnes Pratiques

* **Dépannage :** Utiliser `show running-config` pour vérifier la configuration.
* **Sécurité :** Définir un mot de passe privilégié avec `enable secret [mot-de-passe]`.
* **Documentation :** Ajouter une description aux interfaces avec `description [texte]`.
* **Sauvegarde :** Exporter régulièrement la configuration avec `copy run tftp`.

---

> **Usage :** Guide éducatif à adapter selon l'environnement réseau.
