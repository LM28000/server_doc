# 🛡️ 03 - Maintenance & DRP

## 💾 Sauvegardes Critiques

La stratégie de sauvegarde doit cibler en priorité les **Volumes Nommés Docker** qui contiennent les configurations et bases de données.

### Liste des Volumes à Sauvegarder
L'infrastructure repose sur les volumes suivants, à extraire via un outil de backup (ex: Kopia, Borg) :

1.  **Bases de données & Configs Vitales :**
    * `nextcloud_db_data` (Base MariaDB Nextcloud)
    * `nextcloud_data` (Fichiers config Nextcloud)
    * `vaultwarden_data` (Base de mots de passe - **CRITIQUE**)
    * `actual_budget_data` (Données financières)

2.  **Configurations Applicatives (Gain de temps à la restauration) :**
    * `plex_config` (Bibliothèque, métadonnées)
    * `radarr_config`, `sonarr_config`, `prowlarr_config`
    * `grafana_data` (Dashboards)
    * `open_webui_data` (Historique des conversations IA)

### Procédure de mise à jour (Watchtower)
Les conteneurs sont mis à jour automatiquement sauf exceptions.
* **Commande manuelle (si besoin) :** `docker-compose pull && docker-compose up -d`
* **Exclusions actuelles :** Tdarr, Watchtower (auto-géré).

## 🔒 Sécurité Réseau

### VPN (Qbittorrent)
Le conteneur `qbittorrent` agit comme une passerelle sécurisée.
* **Image :** `binhex/arch-qbittorrentvpn`
* **Protocole :** OpenVPN
* **Comportement :** Si le tunnel VPN tombe, l'accès internet du conteneur est coupé (Killswitch).

### Socket Proxy
L'accès au démon Docker est protégé par `tecnativa/docker-socket-proxy`.
* **Autorisé :** GET (Info containers, images).
* **Bloqué :** POST, DELETE (Impossible de créer/tuer des conteneurs via l'API exposée).
