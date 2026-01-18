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
| **Nextcloud** | `nextcloud:latest` | `8081:80` | `nextcloud_data`, `nextcloud_apps` | DMZ | Dépend de MariaDB. |
| **Nextcloud Cron**| `nextcloud:latest` | - | (Identiques à App) | DMZ | Container sidecar pour exécuter les tâches de fond (cron.php). |
| **Roundcube** | `roundcubemail` | `8085:80` | `roundcube_config` | DMZ | Webmail. |
| **Actual Budget**| `actual-server` | `5006:5006` | `actual_budget_data` | DMZ | Gestion financière. |
| **Vaultwarden** | `vaultwarden/server`| `8084:80`, `3012` | `vaultwarden_data` | DMZ | Gestionnaire de mots de passe (critique ⚠️). |

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

## 📊 Monitoring & Maintenance

| Service | Rôle | Ports | Réseau | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Prometheus** | Collecte | `9090` | Internal | Scrape Node Exporter, cAdvisor. Retention 15j. |
| **Grafana** | Visu | `3000` | DMZ | Dashboards unifiés. Auth via env vars. |
| **cAdvisor** | Docker Stats | `8098` | Internal | Métriques CPU/RAM/Réseau par conteneur. |
| **Node Exp.** | Host Stats | `9100` | Internal | Métriques système hôte (CPU, RAM, Disque, Réseau). |
| **Socket Proxy**| Sécurité | `2375` | Internal | Expose `docker.sock` en lecture seule pour le monitoring. |
| **Watchtower** | Updates | - | Internal | Vérification toutes les 6h (`21600s`). Exclusions via labels. |
| **Scrutiny** | Disques | `8080` | Internal | Monitoring S.M.A.R.T des disques (santé, prévention pannes). |

> 📊 **Dashboards disponibles** : Système, Docker, Réseau, Stockage, Plex.

---

## 🔗 Liens Rapides

- [Infrastructure matérielle](./01_infrastructure.md)
- [Procédures de maintenance](./03_maintenance_drp.md)
- [Architecture réseau](./networkflow.md)
- [Workflow complet](./workflow.md)
