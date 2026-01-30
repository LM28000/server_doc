# 💾 02 - Applications & Services

[← Infrastructure](./01_infrastructure.md) | [↑ Retour au sommaire](./README.md) | [Maintenance →](./03_maintenance_drp.md)

## 📑 Table des Matières

- [Cloud Personnel & Productivité](#️-cloud-personnel--productivité)
- [Média Center & Automatisation](#-média-center--automatisation)
- [Intelligence Artificielle (IA)](#-intelligence-artificielle-ia)
- [Monitoring & Maintenance](#-monitoring--maintenance)

---

## ☁️ Cloud Personnel & Productivité

Services exposés principalement via le réseau `dmz_net` pour un accès sécurisé depuis l'extérieur.

| Service | Image | Ports Exposés | Volumes Clés | Réseau | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Nextcloud** | `nextcloud:latest` | `8081:80` | `nextcloud_data`, `nextcloud_apps` | DMZ + Internal | Dépend de MariaDB et Redis. |
| **Redis** | `redis:alpine` | `6379:6379` | - | DMZ + Internal | Cache pour Nextcloud (performances). |
| **Nextcloud Cron**| `nextcloud:latest` | - | (Identiques à App) | DMZ + Internal | Container sidecar pour exécuter les tâches de fond (cron.php). |
| **Roundcube** | `roundcubemail` | `8085:80` | `roundcube_config` | DMZ + Internal | Webmail. |
| **Actual Budget**| `actual-server` | `5006:5006` | `actual_budget_data` | DMZ | Gestion financière. |
| **Vaultwarden** | `vaultwarden/server`| `8084:80`, `3012` | `vaultwarden_data` | DMZ | Gestionnaire de mots de passe (critique ⚠️). |
| **MediaWiki** | `mediawiki:1.39` | `8083:80` | `mediawiki_data`, `mediawiki_db` | Externe | Wiki personnel avec base MariaDB. |

> 🔐 **Sécurité** : Tous ces services sont accessibles via un reverse proxy externe (Nginx) qui gère le TLS/SSL.

---

## 🎬 Média Center & Automatisation
Stack backend isolée principalement sur `internal_net`, sauf pour la diffusion (Plex) et la requête (Overseerr).

| Service | Rôle | Ports | Réseau | Détails |
| :--- | :--- | :--- | :--- | :--- |
| **Plex** | Diffusion | `32400` | DMZ + Internal | Transcodage via `/transcode` sur cache. |
| **Overseerr** | Requêtes | `5000` | DMZ + Internal | Interface unifiée pour Radarr/Sonarr. |
| **Qbittorrent**| Download | `8080` | Internal | **VPN Actif** (OpenVPN). Interface WebUI. |
| **Prowlarr** | Indexeur | `9696` | Internal | Gère les trackers pour les *arrs. |
| **FlareSolverr**| Proxy | `8191` | Internal | Contournement Cloudflare pour Prowlarr. |
| **Radarr** | Films | `7878` | Internal | Gestion bibliothèque Films. |
| **Sonarr** | Séries | `8989` | Internal | Gestion bibliothèque Séries. |
| **Bazarr** | Sous-titres | `6767` | Internal | Téléchargement auto des sous-titres. |
| **Tautulli** | Stats | `8181` | Internal | Statistiques de lecture Plex. |

### ⚡ Transcodage Distribué (Tdarr)

Tdarr est configuré pour optimiser automatiquement la médiathèque vers le codec HEVC/H.265, réduisant l'espace disque tout en maintenant la qualité.

* **Serveur** : `tdarr` (Ports 8265 Web / 8266 Server)
* **Nœud Interne** : Activé (`internalNode=true`) - Transcodage CPU sur le serveur
* **Nœud Externe** : MacBook Pro M4 connecté via LAN - Accélération matérielle VideoToolbox (M4)

> 🎬 **Gain moyen** : 40-60% de réduction d'espace avec HEVC vs H.264. Consultez le [workflow Tdarr détaillé](./tdarrflow.md).

---

## 🧠 Intelligence Artificielle (IA)
Stack dédiée à l'inférence locale de LLM.

| Service | Image | Ports | Volumes | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Ollama** | `ollama/ollama` | `11434` | `ollama_data` | Moteur d'inférence. API Backend. |
| **Open WebUI**| `open-webui` | `3001` | `open_webui_data` | Interface Chat (style ChatGPT). Connecté à Ollama via `http://ollama:11434`. |

---

## � Sécurité & Authentification

| Service | Rôle | Ports | Réseau | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Authelia** | SSO | `9091` | DMZ | Authentification unifiée (SSO) pour tous les services. 2FA support. |
| **Headscale** | VPN Maillé | `9999` | DMZ | Serveur VPN mesh open-source (alternative Tailscale). Accès sécurisé aux services. |
| **Headscale UI** | Interface Web | `9092` | DMZ | Interface de gestion web pour Headscale. |
| **Kopia** | Sauvegardes | `8200` | DMZ | Sauvegardes incrémentales chiffrées. Accès `/docker` en lecture seule. |

> 🔑 **Authelia** : Fournit une authentification centralisée avec support 2FA (TOTP) pour tous les services exposés.  
> 🌐 **Headscale** : VPN mesh auto-hébergé pour accès sécurisé distant à tous les services du homelab.  
> 💾 **Kopia** : Solution de sauvegarde moderne avec déduplication, chiffrement et support multi-cloud.

---

## �📊 Monitoring & Maintenance

| Service | Rôle | Ports | Réseau | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Prometheus** | Collecte | `9090` | Internal | Scrape Node Exporter, cAdvisor. Retention 15j. |
| **Grafana** | Visu | `3000` | Internal | Dashboards unifiés. Auth via env vars. |
| **cAdvisor** | Docker Stats | `8098` | Internal | Métriques CPU/RAM/Réseau par conteneur. |
| **Node Exp.** | Host Stats | `9100` | Internal | Métriques système hôte (CPU, RAM, Disque, Réseau). |
| **Glances** | System Monitor | `61208` | DMZ | Monitoring système en temps réel (alternative à htop). |
| **Uptime Kuma** | Uptime Monitoring | `3002` | DMZ | Surveillance de disponibilité des services. Notifications. |
| **Dozzle** | Docker Logs | `8088` | DMZ | Visualisateur de logs Docker en temps réel. |
| **Tautulli** | Plex Analytics | `8181` | Internal | Statistiques détaillées de lecture Plex. |
| **Socket Proxy**| Sécurité | `2375` | Internal | Expose `docker.sock` en lecture seule pour le monitoring. |
| **Watchtower** | Updates | - | Internal | Vérification toutes les 6h (`21600s`). Exclusions via labels. |
| **Scrutiny** | Disques | `8080` | Internal | Monitoring S.M.A.R.T des disques (santé, prévention pannes). |

> 📊 **Dashboards disponibles** : Système, Docker, Réseau, Stockage, Plex.  
> 👁️ **Uptime Kuma** : Surveille tous les services critiques avec alertes email/Discord/Telegram.

---

## �️ Outils & Utilitaires

| Service | Rôle | Ports | Réseau | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Dashboard** | Homelab Dashboard | `8089` | DMZ | Page d'accueil personnalisée avec liens vers tous les services. |
| **IT-Tools** | Boîte à outils | `8090` | DMZ | Collection d'outils IT (encodage, crypto, réseau, etc.). |
| **TTYD** | Terminal Web | `7681` | DMZ + Internal | Terminal SSH accessible via navigateur. Auth: admin/admin. |
| **Qui** | Autobrr UI | `7476` | DMZ | Interface web pour autobrr (automatisation torrents). |

> 🏛️ **Dashboard** : Point d'entrée central pour accéder rapidement à tous les services du homelab.  
> 🛠️ **IT-Tools** : Plus de 80 outils pour développeurs et sysadmins (JSON formatter, UUID generator, etc.).

---

## �🔗 Liens Rapides

- [Infrastructure matérielle](./01_infrastructure.md)
- [Procédures de maintenance](./03_maintenance_drp.md)
- [Architecture réseau](./networkflow.md)
- [Workflow complet](./workflow.md)
