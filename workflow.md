# 🔄 Flux de Travail

[↑ Retour au sommaire](./README.md)

Ce document offre une vue d'ensemble de l'automatisation globale du serveur. Il connecte les différents services pour former un écosystème cohérent et autonome, orchestrant les flux de données et les processus.

> 💡 **Conseil** : Ce diagramme est idéal pour comprendre comment les services interagissent entre eux. Pour des détails spécifiques, consultez les diagrammes dédiés.

## ⚙️ Orchestration des Flux

L'automatisation est divisée en quatre domaines principaux :

1.  **Automation Média (Acquisition)** :
    *   De la requête de l'utilisateur (Overseerr) au téléchargement (Client Torrent) et à l'organisation des fichiers (*Arrs).
    *   Inclut le post-traitement via Tdarr pour l'optimisation.

2.  **Intelligence Artificielle (IA)** :
    *   Intégration de modèles de langage locaux (Ollama) accessibles via une interface web (Open WebUI).

3.  **Maintenance & Système** :
    *   Mises à jour automatiques des conteneurs (Watchtower), sauvegardes et tâches planifiées (Nextcloud Cron).

4.  **Observabilité (Monitoring)** :
    *   Collecte de métriques en temps réel (Prometheus) et visualisation (Grafana) pour surveiller la santé du serveur.

---

## 📊 Diagramme de Flux

```mermaid
---
config:
  layout: elk
  theme: neutral
---
flowchart TB
 subgraph WORKFLOW_AUTOMATION["Flux de Travail Média (Acquisition & Traitement)"]
        OS("Overseerr (Requête User)")
        PR("Prowlarr (Indexeur)")
        FS("FlareSolverr (Contournement Anti-DDoS)")
        RD("Radarr")
        SN("Sonarr")
        BZ("Bazarr (Sous-titres)")
        QB["Qbittorrent-VPN"]
        TD_S("fa:fa-server Tdarr Server (Orchestration)")
        TD_NI("fa:fa-microchip Node Interne")
        TD_NE("fa:fa-laptop-code MacBook Pro M4 (Node Externe)")
  end
 subgraph WORKFLOW_AI["Flux de Travail IA & LLM"]
        OWUI("fa:fa-desktop Open WebUI (Interface)")
        OL("fa:fa-brain Ollama (Moteur LLM)")
  end
 subgraph WORKFLOW_CONTENT["Flux de Travail Contenu & Publication"]
        GH("fa:fa-github GitHub (Source)")
        P("Portfolio (Publication)")
  end
 subgraph WORKFLOW_MAINTENANCE["Flux de Travail Système & Gestion"]
        WT("fa:fa-hourglass-half Watchtower (Update)")
        NCC("Nextcloud Cron")
        SC("Scrutiny (S.M.A.R.T. Health)")
        TT("Tautulli (Analyse Plex)")
        UK("fa:fa-heart-pulse Uptime Kuma (Uptime)")
        KP("fa:fa-database Kopia (Sauvegardes)")
        NC("fa:fa-cloud Nextcloud")
        PL("fa:fa-film Plex")
  end
 subgraph WORKFLOW_OBSERVABILITY["Flux de Travail Monitoring"]
        PROM("Prometheus (Collecteur)")
        CADV("cAdvisor (Métriques Conteneurs)")
        NE("Node Exporter (Métriques Hôte)")
        GL("Glances (Système)")
        DZ("Dozzle (Logs)")
        GF("fa:fa-chart-bar Grafana (Visualisation)")
        DSP("Docker Socket Proxy (Accès Sécurisé)")
  end
 subgraph WORKFLOW_SECURITY["Flux de Travail Sécurité"]
        AUTH("fa:fa-shield-halved Authelia (SSO/2FA)")
        USERS("fa:fa-users Utilisateurs")
  end
    OS -- Commande Nouvelle Tâche --> RD & SN
    PR -- Requête Index --> FS
    RD -- Tâche de Téléchargement --> QB
    SN -- Tâche de Téléchargement --> QB
    QB -- Média Brut --> TD_S
    TD_S -- Dispatch Job --> TD_NI & TD_NE
    TD_NI -- Média Optimisé --> PL
    TD_NE -- Média Optimisé --> PL
    BZ -- Synchronise Metadata --> RD & SN
    OWUI -- Requête Chat (API) --> OL
    GH -- Déclenche CI/CD --> P
    WT -- Déclenche Mise à Jour --> WORKFLOW_AUTOMATION & WORKFLOW_MAINTENANCE & WORKFLOW_OBSERVABILITY & WORKFLOW_AI & P
    NC -- Déclenche Tâches Régulières --> NCC
    PL -- Alimente Données --> TT
    SC -- Collecte État --> TT
    UK -- Surveille --> WORKFLOW_AUTOMATION & WORKFLOW_MAINTENANCE & WORKFLOW_OBSERVABILITY
    KP -- Sauvegarde --> NC & WORKFLOW_AUTOMATION
    DSP -- Expose Métriques --> CADV
    PROM -- Scrape (Collecte Active) --> CADV & NE & SC & GL
    GF -- Requête Visualisation --> PROM
    DZ -- Affiche Logs --> WORKFLOW_AUTOMATION & WORKFLOW_MAINTENANCE
    USERS -- Authentification --> AUTH
    AUTH -- Autorise Accès --> WORKFLOW_AUTOMATION & WORKFLOW_MAINTENANCE & WORKFLOW_OBSERVABILITY
    style TD_NE fill:#fff7ed,stroke:#f97316,stroke-dasharray: 5 5
    style WORKFLOW_AUTOMATION fill:#e3f2fd,stroke:#64b5f6,stroke-width:1px,color:#263238
    style WORKFLOW_MAINTENANCE fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#263238
    style WORKFLOW_OBSERVABILITY fill:#e8f5e9,stroke:#81c784,stroke-width:2px,color:#263238
    style WORKFLOW_AI fill:#f3e5f5,stroke:#ce93d8,stroke-width:1px,color:#263238
    style WORKFLOW_CONTENT fill:#fce4ec,stroke:#f48fb1,stroke-width:1px,color:#263238
    style WORKFLOW_SECURITY fill:#fef3c7,stroke:#f59e0b,stroke-width:2px,color:#263238
```

---

## 📝 Détails des Flux

### 🎬 Flux Média (Automation)

**Déclencheur** : Requête utilisateur via Overseerr  
**Durée moyenne** : 30 min - 4h  
**Services impliqués** : Overseerr → Radarr/Sonarr → Prowlarr → Qbittorrent → Tdarr → Plex

**Étapes clés** :
1. L'utilisateur cherche un contenu sur Overseerr
2. Overseerr notifie Radarr/Sonarr de la demande
3. Prowlarr recherche sur les indexeurs (via FlareSolverr si nécessaire)
4. Radarr/Sonarr sélectionne la meilleure release et envoie à Qbittorrent
5. Une fois téléchargé, le fichier est déplacé et traité par Tdarr
6. Le média optimisé est indexé par Plex
7. Bazarr télécharge les sous-titres automatiquement

### 🧠 Flux IA

**Déclencheur** : Requête utilisateur via Open WebUI  
**Latence** : < 2s (selon le modèle)  
**Services impliqués** : Open WebUI → Ollama

**Modèles disponibles** : Llama 3, Mistral, CodeLlama, etc.

### 🔧 Flux Maintenance

**Fréquence** : Continue (Watchtower : toutes les 6h)  
**Supervision** : Prometheus + Grafana  
**Services impliqués** : Watchtower, Scrutiny, Tautulli, Nextcloud Cron

### 📊 Flux Monitoring

**Architecture** : Pull-based (Prometheus scrape)  
**Rétention** : 15 jours  
**Services impliqués** : Prometheus ← cAdvisor / Node Exporter / Scrutiny  
**Visualisation** : Grafana avec dashboards pré-configurés

---

## 🔗 Voir Aussi

- **Architecture détaillée** :
  - [Séquence de requête média](./media_request_sequence.md)
  - [Cycle de vie du média](./media_lifecycle_state.md)
  - [Pipeline Tdarr](./tdarrflow.md)

- **Infrastructure** :
  - [Flux réseau](./networkflow.md)
  - [Flux de données](./dataflow.md)
  - [Matériel](./01_infrastructure.md)

- **Gestion** :
  - [Applications](./02_applications.md)
  - [Maintenance](./03_maintenance_drp.md)