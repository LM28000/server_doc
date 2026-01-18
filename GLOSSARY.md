# 📖 Glossaire Technique

[↑ Retour au sommaire](./README.md)

Ce document définit les termes techniques, acronymes et concepts utilisés dans la documentation du serveur.

---

## 🔤 Acronymes & Technologies

### Infrastructure

| Terme | Définition | Documentation |
|-------|------------|---------------|
| **DMZ** | Zone Démilitarisée - Réseau isolé pour services exposés | [networkflow.md](./networkflow.md) |
| **IPMI** | Intelligent Platform Management Interface - Gestion hors-bande | [01_infrastructure.md](./01_infrastructure.md) |
| **iRMC** | Integrated Remote Management Controller (Fujitsu) | [01_infrastructure.md](./01_infrastructure.md) |
| **MergerFS** | Système de fichiers d'union pour pooler plusieurs disques | [dataflow.md](./dataflow.md) |
| **SnapRAID** | Solution de parité pour protection contre pannes disques | [dataflow.md](./dataflow.md) |
| **VPN** | Virtual Private Network - Tunnel chiffré pour Qbittorrent | [03_maintenance_drp.md](./03_maintenance_drp.md) |

### Stockage

| Terme | Définition | Usage |
|-------|------------|-------|
| **Hot Storage** | Stockage ultra-rapide (NVMe) | OS, DB, Configs |
| **Warm Storage** | Stockage rapide (SSD) | Cache, Downloads |
| **Cold Storage** | Stockage de masse (HDD) | Médias, Archives |
| **Tiering** | Stratégie de stockage hiérarchisé par performance | [dataflow.md](./dataflow.md) |

### Média

| Terme | Définition | Documentation |
|-------|------------|---------------|
| **Arrs** | Famille de logiciels : Radarr, Sonarr, Prowlarr, Bazarr | [02_applications.md](./02_applications.md) |
| **Direct Play** | Lecture sans transcodage (qualité originale) | [tdarrflow.md](./tdarrflow.md) |
| **HEVC / H.265** | Codec vidéo moderne, 40-60% plus efficace que H.264 | [tdarrflow.md](./tdarrflow.md) |
| **Indexeur** | Site de torrents référencé dans Prowlarr | [02_applications.md](./02_applications.md) |
| **Custom Format** | Profil de qualité personnalisé dans Radarr/Sonarr | [media_request_sequence.md](./media_request_sequence.md) |

### Monitoring

| Terme | Définition | Documentation |
|-------|------------|---------------|
| **cAdvisor** | Container Advisor - Métriques conteneurs Docker | [02_applications.md](./02_applications.md) |
| **Node Exporter** | Collecteur de métriques système Linux | [02_applications.md](./02_applications.md) |
| **Prometheus** | Base de données time-series pour métriques | [02_applications.md](./02_applications.md) |
| **Scrape** | Collecte active de métriques par Prometheus | [workflow.md](./workflow.md) |
| **S.M.A.R.T.** | Self-Monitoring Analysis and Reporting Technology | [02_applications.md](./02_applications.md) |

### IA & LLM

| Terme | Définition | Documentation |
|-------|------------|---------------|
| **LLM** | Large Language Model (modèle de langage) | [02_applications.md](./02_applications.md) |
| **Ollama** | Moteur d'inférence local pour LLM | [02_applications.md](./02_applications.md) |
| **Open WebUI** | Interface chat pour modèles Ollama | [02_applications.md](./02_applications.md) |
| **Inférence** | Génération de réponses par le modèle IA | [workflow.md](./workflow.md) |

---

## 🎯 Concepts Clés

### Architecture Réseau

#### Segmentation en Zones

```
Internet 
  ↓
Reverse Proxy (Nginx)
  ↓
┌─────────────────────┐
│  DMZ (Exposé)       │ ← Services accessibles publiquement
└─────────────────────┘
          ↓ (Unidirectionnel)
┌─────────────────────┐
│  Internal (Backend) │ ← Services critiques isolés
└─────────────────────┘
          ↑
┌─────────────────────┐
│  Host (Serveur)     │ ← OS, Docker Engine
└─────────────────────┘
```

**Principe** : Si la DMZ est compromise, les services backend restent protégés.

### Cycle de Vie du Média

```
Requête → Téléchargement → Traitement → Disponible
  (30s)      (10min-4h)      (20min-8h)    (∞)
```

Voir [media_lifecycle_state.md](./media_lifecycle_state.md) pour les détails.

### Stratégie 3-2-1 (Sauvegardes)

- **3** copies des données
- **2** supports différents (SSD + Cloud)
- **1** copie hors site

Voir [03_maintenance_drp.md](./03_maintenance_drp.md)

---

## 🔢 Ports & Protocoles

### Ports Réseau Principaux

| Service | Port(s) | Protocole | Accès |
|---------|---------|-----------|-------|
| **Plex** | 32400 | HTTP/HTTPS | DMZ → Internet |
| **Overseerr** | 5000 | HTTP | DMZ → Internet |
| **Nextcloud** | 8081 | HTTP | DMZ → Internet |
| **Grafana** | 3000 | HTTP | DMZ → Internet |
| **Prometheus** | 9090 | HTTP | Internal uniquement |
| **Radarr** | 7878 | HTTP | Internal uniquement |
| **Sonarr** | 8989 | HTTP | Internal uniquement |
| **Qbittorrent** | 8080 | HTTP | Internal uniquement |
| **Ollama** | 11434 | HTTP | Internal uniquement |
| **SSH** | 22 | SSH | LAN uniquement |
| **iRMC** | 443 | HTTPS | LAN uniquement |

### Protocoles Réseau

| Protocole | Usage | Sécurité |
|-----------|-------|----------|
| **HTTP** | Trafic interne non chiffré | ✅ OK (réseau privé) |
| **HTTPS** | Trafic externe chiffré | ✅ Obligatoire (TLS/SSL) |
| **SSH** | Administration serveur | ✅ Clés uniquement |
| **OpenVPN** | Tunnel VPN Qbittorrent | ✅ Chiffré |

---

## 📂 Chemins Système

### Volumes Docker

```bash
/var/lib/docker/volumes/
├── nextcloud_data/          # Fichiers utilisateurs Nextcloud
├── nextcloud_db_data/       # Base MariaDB
├── vaultwarden_data/        # Coffre-fort mots de passe ⚠️ CRITIQUE
├── plex_config/             # Métadonnées & bibliothèque
├── radarr_config/           # Configuration Radarr
├── sonarr_config/           # Configuration Sonarr
└── grafana_data/            # Dashboards Grafana
```

### Stockage Média

```bash
/mnt/
├── cache/                   # Warm Storage (SSD)
│   └── downloads/
│       ├── incomplete/      # Téléchargements en cours
│       └── complete/        # Téléchargements terminés
│
└── storage/                 # Cold Storage (Pool MergerFS)
    └── media/
        ├── movies/          # Films (Radarr)
        └── tv/              # Séries (Sonarr)
```

---

## 🔗 Références Externes

### Documentation Officielle

- **Docker** : https://docs.docker.com/
- **Plex** : https://support.plex.tv/
- **MergerFS** : https://github.com/trapexit/mergerfs
- **SnapRAID** : https://www.snapraid.it/
- **Prometheus** : https://prometheus.io/docs/
- **Grafana** : https://grafana.com/docs/

### Guides Communautaires

- **TRaSH Guides** : https://trash-guides.info/ (Best practices *Arrs)
- **r/selfhosted** : https://reddit.com/r/selfhosted
- **LinuxServer.io** : https://docs.linuxserver.io/

---

## 📚 Index par Thème

### 🎬 Je veux comprendre... le Média

1. [Comment demander un film/série](./user_journey.md)
2. [Comment se passe le téléchargement](./media_request_sequence.md)
3. [Les étapes de traitement](./media_lifecycle_state.md)
4. [Comment fonctionne Tdarr](./tdarrflow.md)

### 🔧 Je veux comprendre... l'Infrastructure

1. [Le matériel du serveur](./01_infrastructure.md)
2. [Le réseau et la sécurité](./networkflow.md)
3. [Le stockage hiérarchisé](./dataflow.md)

### 📊 Je veux comprendre... le Monitoring

1. [Les applications installées](./02_applications.md)
2. [Comment tout fonctionne ensemble](./workflow.md)
3. [La maintenance et sauvegardes](./03_maintenance_drp.md)

### 🗺️ Je veux une vue d'ensemble

1. [Carte mentale complète](./server_ecosystem_mindmap.md) ⭐ **Recommandé**
2. [Workflow global](./workflow.md)

---

*Dernière mise à jour : 18/01/2025*
