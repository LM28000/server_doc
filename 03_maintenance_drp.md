# 🛡️ 03 - Maintenance & DRP

[← Applications](./02_applications.md) | [↑ Retour au sommaire](./README.md)

## 📑 Table des Matières

- [Sauvegardes Critiques](#-sauvegardes-critiques)
- [Sécurité Réseau](#-sécurité-réseau)
- [Procédures de Restauration](#-procédures-de-restauration)
- [Maintenance Planifiée](#-maintenance-planifiée)

---

## 💾 Sauvegardes Critiques

La stratégie de sauvegarde suit le principe 3-2-1 : 3 copies, 2 supports différents, 1 hors site. Elle cible en priorité les **Volumes Nommés Docker** qui contiennent les configurations et bases de données.

### � Outil de Sauvegarde : Kopia

**Kopia** est déployé pour gérer toutes les sauvegardes :
- **Interface Web** : Port 8200
- **Type** : Sauvegardes incrémentales avec déduplication
- **Chiffrement** : AES-256
- **Destinations** : Cloud (Backblaze B2, Wasabi, S3) ou NAS local
- **Accès** : `/docker` monté en lecture seule

> 💾 **Avantage** : Déduplication au niveau des blocs = économies d'espace massives (~70% pour les sauvegardes multiples).

### �🔴 Priorité CRITIQUE (Sauvegarde quotidienne)

Perte de données = Impact majeur sur le service.

* `/docker/services/vaultwarden/` - Base de mots de passe **⚠️ CRITIQUE**
* `/docker/services/nextcloud/db/` - Base MariaDB Nextcloud
* `/docker/services/actual_budget/` - Données financières

### 🟡 Priorité HAUTE (Sauvegarde hebdomadaire)

Gain de temps significatif à la restauration.

* `/docker/services/nextcloud/app/` - Fichiers config Nextcloud
* `/docker/services/plex/config/` - Bibliothèque, métadonnées, vues
* `/docker/services/grafana/` - Dashboards personnalisés
* `/docker/services/open-webui/` - Historique des conversations IA
* `/docker/services/authelia/` - Configuration SSO et utilisateurs
* `/docker/services/headscale/config/` - Configuration VPN mesh et clés
* `/docker/services/headscale/data/` - Base de données Headscale
* `/docker/services/uptime-kuma/` - Configuration monitoring uptime
* `/docker/services/mediawiki/data/` - Contenu du wiki
* `/docker/services/mediawiki/db/` - Base de données MediaWiki

### 🟢 Priorité MOYENNE (Sauvegarde mensuelle)

Configurables mais fastidieux à recréer.

* `/docker/services/radarr/`, `/docker/services/sonarr/`, `/docker/services/prowlarr/`
* `/docker/services/prometheus/` - Historique des métriques (15 jours de rétention)

> 📝 **Outil recommandé** : Kopia pour les sauvegardes incrémentales chiffrées vers le cloud (Backblaze B2, Wasabi).

### Procédure de mise à jour (Watchtower)

Les conteneurs sont mis à jour automatiquement toutes les 6h, sauf exceptions.

**Commande manuelle** (si besoin) :
```bash
cd /path/to/docker-compose
docker-compose pull
docker-compose up -d
```

**Exclusions actuelles** : Tdarr, Watchtower (auto-géré).

**Logs de mise à jour** :
```bash
docker logs watchtower -f
```

---

## 🔄 Procédures de Restauration

### Restauration Complète (Disaster Recovery)

1. **Réinstaller l'OS** (Debian 13 Trixie)
2. **Restaurer les fichiers Docker Compose** (stacks/)
3. **Créer les volumes Docker**
   ```bash
   docker volume create nextcloud_data
   docker volume create vaultwarden_data
   # etc...
   ```
4. **Restaurer les données depuis la sauvegarde**
   ```bash
   kopia restore <snapshot-id> /var/lib/docker/volumes/
   ```
5. **Redémarrer les stacks**
   ```bash
   docker-compose up -d
   ```

### Restauration Partielle (Un seul service)

1. **Arrêter le conteneur concerné**
   ```bash
   docker-compose stop <service>
   ```
2. **Restaurer uniquement le volume concerné**
3. **Redémarrer le service**
   ```bash
   docker-compose up -d <service>
   ```

> ⏱️ **RTO** (Recovery Time Objective) : ~2-4h pour une restauration complète  
> 💾 **RPO** (Recovery Point Objective) : 24h maximum (sauvegardes quotidiennes)

---

## 🗓️ Maintenance Planifiée

### Tâches Hebdomadaires

- [ ] Vérifier les logs Watchtower
- [ ] Contrôler l'espace disque disponible
- [ ] Vérifier Grafana pour anomalies
- [ ] Vérifier Uptime Kuma (disponibilité des services)
- [ ] Tester les sauvegardes Kopia (restauration d'un volume test)
- [ ] Consulter Dozzle pour erreurs dans les logs

### Tâches Mensuelles

- [ ] Exécuter SnapRAID Sync (parité)
- [ ] Vérifier les données S.M.A.R.T (Scrutiny)
- [ ] Mettre à jour l'OS hôte (`apt update && apt upgrade`)
- [ ] Auditer les logs de sécurité (accès SSH, tentatives d'intrusion)

### Tâches Trimestrielles

- [ ] Tester le plan de reprise (DRP)
- [ ] Vérifier les certificats SSL/TLS
- [ ] Nettoyer les images Docker inutilisées (`docker system prune`)
- [ ] Auditer les accès utilisateurs (Nextcloud, Vaultwarden)

## 🔒 Sécurité Réseau

### � Authentification Centralisée (Authelia)

**Authelia** fournit une couche d'authentification SSO (Single Sign-On) pour tous les services exposés.

* **Image** : `authelia/authelia:latest`
* **Port** : 9091
* **Fonctionnalités** :
  - Authentification à deux facteurs (2FA/TOTP)
  - Support LDAP/fichiers utilisateurs
  - Règles d'accès granulaires
  - Intégration avec reverse proxy (Nginx/Traefik)

**Configuration** :
```yaml
# Fichier de configuration : /docker/services/authelia/configuration.yml
default_redirection_url: https://home.du-cray.eu

access_control:
  rules:
    - domain: "*.du-cray.eu"
      policy: two_factor
```

> 🔐 **Best Practice** : Tous les services sensibles (Grafana, Portainer, etc.) doivent passer par Authelia.

### �🔐 VPN (Qbittorrent)

Le conteneur `qbittorrent` agit comme une passerelle sécurisée pour tout le trafic P2P.

* **Image** : `binhex/arch-qbittorrentvpn`
* **Protocole** : OpenVPN (configuration custom)
* **Fournisseur VPN** : GhostVPN
* **Killswitch** : Actif - Si le tunnel VPN tombe, l'accès internet du conteneur est coupé
* **Vérification IP** : Accessible via WebUI (8080) > Paramètres > Connexion

**Test de sécurité** :
```bash
# Vérifier l'IP publique du conteneur
docker exec qbittorrent curl ifconfig.me
# Doit correspondre à l'IP du VPN, PAS votre IP FAI
```

### 🔒 Socket Proxy

L'accès au démon Docker est protégé par `tecnativa/docker-socket-proxy`.

* **Autorisé** : GET (Info containers, images, volumes)
* **Bloqué** : POST, DELETE (Impossible de créer/tuer des conteneurs via l'API exposée)
* **Usage** : Utilisé par cAdvisor et Grafana pour récupérer les métriques

> 🛡️ **Best Practice** : Ne JAMAIS exposer `/var/run/docker.sock` directement aux conteneurs.

### 🌐 Segmentation Réseau

Consultez le [diagramme de flux réseau](./networkflow.md) pour comprendre l'isolation complète entre :
- **DMZ** (services exposés)
- **Internal Net** (services backend critiques)
- **Host Network** (accès direct uniquement pour iRMC et monitoring)
