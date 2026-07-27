# Configuration Initiale Recommandée pour un Appareil Réseau

> **Auteur** : Chasseur De Script Kiddies  
> **Thème** : Configuration Cisco de base — Guide de démarrage rapide

---

## 🚀 Commandes de Configuration de Base

### Accès au Mode Privilégié

| Commande | Description |
|---|---|
| `CDevice>enable` | Accède au mode privilégié (EXEC) |
| `CDevice#configure terminal` | Passe en mode Configuration globale |

### Configuration Console

| Commande | Description |
|---|---|
| `CDevice(config)#line console 0` | Entre dans le mode de configuration de la console |
| `CDevice(config-line)#logging synchronous` | Active la synchronisation des logs |
| `CDevice(config-line)#history size 50` | Définit l'historique à 50 commandes |
| `CDevice(config-line)#exec-timeout 25 0` | Déconnexion après 25 min d'inactivité |

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

> **Description des commandes clés :**
> - `no ip domain-lookup` — Désactive la résolution DNS (évite les erreurs de frappe)
> - `cdp run` — Active Cisco Discovery Protocol
> - `lldp run` — Active Link Layer Discovery Protocol
> - `copy run start` — Sauvegarde la configuration en mémoire NVRAM

---

## 💡 Astuces & Bonnes Pratiques

- **Dépannage** : Après chaque configuration, utilisez `show running-config` pour vérifier vos changements.
- **Sécurité** : Toujours définir un mot de passe privilégié avec `enable secret [mot-de-passe]`.
- **Documentation** : Ajoutez des descriptions sur vos interfaces avec `description [texte]`.
- **Sauvegarde** : Pensez à exporter votre configuration régulièrement avec `copy run tftp`.

---

*Ce guide est destiné à un usage éducatif. Adaptez les paramètres selon votre environnement réseau.*
