# 💾 02 - Applications & Services (Docker)

## 🔄 Flux de Travail (Workflows)

Le serveur est organisé par finalité métier :

* **🎬 Média (Acquisition & Traitement) :** Automatisation complète via la suite *arr (Radarr, Sonarr) couplée à Overseerr pour les demandes et Plex pour la diffusion. Tdarr optimise les fichiers.
* **🧠 IA & LLM :** Hébergement local de modèles (Ollama) avec interface web (Open WebUI).
* **☁️ Cloud & Contenu :** Nextcloud pour les données perso, Vaultwarden pour les mots de passe.
* **📊 Observabilité :** Stack Prometheus + Grafana pour surveiller la santé du système.

---

## 🐳 Architecture Docker

### Réseaux Docker
* **`dmz_net`** : Contient les services exposés au reverse proxy (Plex, Nextcloud, Overseerr).
* **`internal_net`** : Contient les services backend, bases de données et services d'acquisition.
* **Communication** : Le Reverse Proxy dirige le trafic vers le `dmz_net`.

### Liste des Services Critiques

| Service | Rôle | Réseau(x) | Ports (Hôte:Conteneur) | Image |
| :--- | :--- | :--- | :--- | :--- |
| **Plex** | Serveur Média | `dmz`, `internal` | `32400:32400` | `lscr.io/linuxserver/plex` |
| **Nextcloud** | Cloud Perso | `dmz`, `internal` | `8081:80` | `nextcloud` |
| **Vaultwarden** | Mots de passe | `dmz` | `3012`, `8084` | `vaultwarden/server` |
| **Overseerr** | Requêtes Média | `dmz`, `internal` | `5000:5055` | `sctx/overseerr` |
| **Qbittorrent-VPN** | Téléchargement | `internal` | `8080:8080` | `binhex/arch-qbittorrentvpn` |
| **Radarr** | Gestion Films | `internal` | `7878:7878` | `linuxserver/radarr` |
| **Sonarr** | Gestion Séries | `internal` | `8989:8989` | `linuxserver/sonarr` |
| **Tdarr Server** | Transcodage | `internal` | `8265`, `8266` | `haveagitgat/tdarr` |
| **Ollama** | Moteur IA | N/A (Local) | `11434:11434` | `ollama/ollama` |
| **Open WebUI** | Interface IA | N/A (Local) | `3001:8080` | `open-webui/open-webui` |
| **Prometheus** | Métriques | `internal` | `9090:9090` | `prom/prometheus` |
| **Grafana** | Dashboards | `internal` | `3000:3000` | `grafana/grafana` |

---

## 🔗 Dépendances & Intégrations Spécifiques

### Nœud de Transcodage Externe
* **Machine :** MacBook Pro M4
* **Rôle :** Nœud Tdarr Externe (Worker)
* **Fonctionnement :** Le serveur Tdarr délègue les tâches lourdes de transcodage au MacBook via le réseau LAN pour profiter de l'encodage hardware Apple Silicon.

### Gestion des Utilisateurs (PUID/PGID)
La majorité des conteneurs utilisent des variables d'environnement pour gérer les permissions sur les fichiers partagés :
* `PUID` : [À COMPLÉTER]
* `PGID` : [À COMPLÉTER]
