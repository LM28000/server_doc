# 📋 Liste Complète des Services

Liste exhaustive de tous les conteneurs Docker déployés sur le serveur Antigravity.

**Dernière mise à jour** : 18/01/2026  
**Total de services** : 37 conteneurs actifs

---

## 🎬 Média Center (8 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Plex** | `linuxserver/plex` | 32400 | Host | Serveur de streaming média |
| **Overseerr** | `sctx/overseerr` | 5000 | DMZ + Internal | Gestion des requêtes média |
| **Radarr** | `linuxserver/radarr` | 7878 | Internal | Gestion films |
| **Sonarr** | `linuxserver/sonarr` | 8989 | Internal | Gestion séries |
| **Bazarr** | `linuxserver/bazarr` | 6767 | Internal | Gestion sous-titres |
| **Prowlarr** | `linuxserver/prowlarr` | 9696 | Internal | Gestionnaire d'indexeurs |
| **Tdarr** | `haveagitgat/tdarr` | 8265, 8266 | Internal | Transcodage vidéo HEVC |
| **Tautulli** | `linuxserver/tautulli` | 8181 | Internal | Statistiques Plex |

---

## 📥 Téléchargement (2 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Qbittorrent** | `binhex/arch-qbittorrentvpn` | 8080 | Internal + DMZ | Client torrent avec VPN |
| **FlareSolverr** | `flaresolverr/flaresolverr` | 8191 | Internal + DMZ | Contournement Cloudflare |

---

## ☁️ Cloud & Productivité (6 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Nextcloud** | `nextcloud:latest` | 8081 | DMZ + Internal | Cloud personnel |
| **Redis** | `redis:alpine` | 6379 | DMZ + Internal | Cache pour Nextcloud |
| **Nextcloud Cron** | `nextcloud:latest` | - | DMZ + Internal | Tâches planifiées Nextcloud |
| **Nextcloud DB** | `mariadb:10.6` | - | Internal | Base de données Nextcloud |
| **Roundcube** | `roundcube/roundcubemail` | 8085 | DMZ + Internal | Webmail |
| **Actual Budget** | `actualbudget/actual-server` | 5006 | DMZ | Gestion financière |

---

## 📚 Documentation & Wiki (2 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **MediaWiki** | `mediawiki:1.39` | 8083 | Externe | Wiki personnel |
| **MediaWiki DB** | `mariadb:10.6` | - | Externe | Base de données MediaWiki |

---

## 🔐 Sécurité & Authentification (3 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Authelia** | `authelia/authelia` | 9091 | DMZ | SSO avec 2FA |
| **Vaultwarden** | `vaultwarden/server` | 8084, 3012 | DMZ | Gestionnaire de mots de passe |
| **Kopia** | `kopia/kopia` | 8200 | DMZ | Sauvegardes chiffrées |

---

## 📊 Monitoring & Observabilité (8 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Prometheus** | `prom/prometheus` | 9090 | Internal | Collecte de métriques |
| **Grafana** | `grafana/grafana` | 3000 | Internal | Visualisation dashboards |
| **cAdvisor** | `gcr.io/cadvisor/cadvisor` | 8098 | Internal | Métriques conteneurs |
| **Node Exporter** | `prom/node-exporter` | 9100 | Internal | Métriques système hôte |
| **Uptime Kuma** | `louislam/uptime-kuma` | 3002 | DMZ | Monitoring uptime |
| **Glances** | `nicolargo/glances` | 61208 | DMZ | Monitoring système temps réel |
| **Dozzle** | `amir20/dozzle` | 8088 | DMZ | Logs Docker temps réel |
| **Scrutiny** | `analogj/scrutiny` | 8080 | Internal | Santé disques S.M.A.R.T. |

---

## 🛠️ Outils & Utilitaires (5 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Dashboard** | `nginx:alpine` | 8089 | DMZ | Page d'accueil homelab |
| **IT-Tools** | `corentinth/it-tools` | 8090 | DMZ | 80+ outils pour développeurs |
| **TTYD** | `tsl0922/ttyd` | 7681 | DMZ + Internal | Terminal SSH web |
| **Qui** | `autobrr/qui` | 7476 | DMZ | Interface autobrr |
| **Watchtower** | `containrrr/watchtower` | - | Internal | Mises à jour automatiques |

---

## 🤖 Intelligence Artificielle (2 services)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Ollama** | `ollama/ollama` | 11434 | Internal | Moteur d'inférence LLM |
| **Open WebUI** | `ghcr.io/open-webui` | 3001 | DMZ + Internal | Interface chat IA |

---

## 🔧 Infrastructure (1 service)

| Service | Image | Port(s) | Réseau | Description |
|---------|-------|---------|--------|-------------|
| **Docker Socket Proxy** | `tecnativa/docker-socket-proxy` | 2375 | Internal | Accès sécurisé Docker API |

---

## 📊 Statistiques par Catégorie

```
┌──────────────────────┬──────────┬──────────────┐
│ Catégorie            │ Services │ % du total   │
├──────────────────────┼──────────┼──────────────┤
│ Média                │    8     │    21.6%     │
│ Monitoring           │    8     │    21.6%     │
│ Cloud & Productivité │    6     │    16.2%     │
│ Outils               │    5     │    13.5%     │
│ Sécurité             │    3     │     8.1%     │
│ Téléchargement       │    2     │     5.4%     │
│ IA                   │    2     │     5.4%     │
│ Wiki                 │    2     │     5.4%     │
│ Infrastructure       │    1     │     2.7%     │
├──────────────────────┼──────────┼──────────────┤
│ TOTAL                │   37     │   100.0%     │
└──────────────────────┴──────────┴──────────────┘
```

---

## 🌐 Répartition par Réseau

### DMZ (Services accessibles)
- Dashboard, Authelia, Plex, Overseerr, Nextcloud
- Vaultwarden, Grafana, Uptime Kuma, Kopia
- MediaWiki, IT-Tools, Dozzle, Glances, TTYD
- Roundcube, Actual Budget, Open WebUI
- **Total** : 17 services exposés

### Internal (Backend)
- Bases de données (MariaDB x2)
- Stack *Arrs (Radarr, Sonarr, Bazarr, Prowlarr)
- Qbittorrent, Tdarr, Redis, Ollama
- Prometheus, cAdvisor, Node Exporter, Scrutiny
- Docker Socket Proxy, Watchtower
- **Total** : 15 services internes

### DMZ + Internal (Hybride)
- Nextcloud, Nextcloud Cron, Overseerr
- FlareSolverr, TTYD, Open WebUI
- **Total** : 5 services hybrides

---

## 💾 Volumes Critiques à Sauvegarder

### 🔴 Priorité CRITIQUE
- `vaultwarden_data`
- `nextcloud_db_data`
- `actual_budget_data`

### 🟡 Priorité HAUTE
- `nextcloud_data`
- `plex_config`
- `grafana_data`
- `open_webui_data`
- `authelia_config`
- `uptime_kuma_data`
- `mediawiki_data`
- `mediawiki_db`

### 🟢 Priorité MOYENNE
- `radarr_config`
- `sonarr_config`
- `prowlarr_config`
- `bazarr_config`
- `prometheus_data`

---

## 🔗 Liens Rapides

- [Documentation complète](./README.md)
- [Applications détaillées](./02_applications.md)
- [Architecture réseau](./networkflow.md)
- [Glossaire technique](./GLOSSARY.md)

---

*Ce document est généré à partir des fichiers de configuration Docker Compose.*
