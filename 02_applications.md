# 💾 02 - Applications & Services

## ☁️ Cloud Personnel & Productivité
Services exposés principalement via `dmz_net`.

| Service | Image | Ports Exposés | Volumes Clés | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Nextcloud** | `nextcloud:latest` | `8081:80` | `nextcloud_data`, `nextcloud_apps` | Dépend de MariaDB. |
| **Nextcloud Cron**| `nextcloud:latest` | - | (Identiques à App) | Container sidecar pour exécuter les tâches de fond (cron.php). |
| **Roundcube** | `roundcubemail` | `8085:80` | `roundcube_config` | Webmail. |
| **Actual Budget**| `actual-server` | `5006:5006` | `actual_budget_data` | Gestion financière. |
| **Vaultwarden** | `vaultwarden/server`| `8084:80`, `3012` | `vaultwarden_data` | Gestionnaire de mots de passe. |

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
Tdarr est configuré pour optimiser la médiathèque (HEVC/H265).
* **Serveur :** `tdarr` (Ports 8265 Web / 8266 Server).
* **Nœud Interne :** Activé (`internalNode=true`).
* **Nœud Externe :** MacBook Pro M4 connecté via LAN.

---

## 🧠 Intelligence Artificielle (IA)
Stack dédiée à l'inférence locale de LLM.

| Service | Image | Ports | Volumes | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Ollama** | `ollama/ollama` | `11434` | `ollama_data` | Moteur d'inférence. API Backend. |
| **Open WebUI**| `open-webui` | `3001` | `open_webui_data` | Interface Chat (style ChatGPT). Connecté à Ollama via `http://ollama:11434`. |

---

## 📊 Monitoring & Maintenance

| Service | Rôle | Ports | Notes |
| :--- | :--- | :--- | :--- |
| **Prometheus** | Collecte | `9090` | Scrape Node Exporter & cAdvisor. |
| **Grafana** | Visu | `3000` | Dashboards unifiés. Admin via env vars. |
| **cAdvisor** | Docker Stats | `8098` | Métriques CPU/RAM par conteneur. |
| **Node Exp.** | Host Stats | `9100` | Métriques système hôte. |
| **Socket Proxy**| Sécurité | `2375` | Expose `docker.sock` en lecture seule pour le monitoring. |
| **Watchtower** | Updates | - | Vérification toutes les 6h (`21600s`). Exclusions via labels. |
