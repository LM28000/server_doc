# 📘 Documentation Serveur Hôte

## 📌 Vue d'ensemble

Ce dépôt documente l'infrastructure du serveur domestique, hébergeant des services de média, de cloud personnel, d'intelligence artificielle et de domotique.

* **Architecture :** Docker Compose (Multi-stack)
* **Sécurité :** Segmentation réseau (`dmz_net` vs `internal_net`)
* **Stockage :** Stratégie hiérarchisée (M.2 > SSD Cache > HDD Pool)

## 🔄 Flux de Travail (Workflows)

Le serveur orchestre plusieurs processus complexes, notamment l'acquisition de médias et l'inférence IA locale.

```mermaid
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
        NC("fa:fa-cloud Nextcloud")
        PL("fa:fa-film Plex")
  end
 subgraph WORKFLOW_OBSERVABILITY["Flux de Travail Monitoring"]
        PROM("Prometheus (Collecteur)")
        CADV("cAdvisor (Métriques Conteneurs)")
        NE("Node Exporter (Métriques Hôte)")
        GF("fa:fa-chart-bar Grafana (Visualisation)")
        DSP("Docker Socket Proxy (Accès Sécurisé)")
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
    DSP -- Expose Métriques --> CADV
    PROM -- Scrape (Collecte Active) --> CADV & NE & SC
    GF -- Requête Visualisation --> PROM
    style TD_NE fill:#fff7ed,stroke:#f97316,stroke-dasharray: 5 5
    style WORKFLOW_AUTOMATION fill:#e3f2fd,stroke:#64b5f6,stroke-width:1px,color:#263238
    style WORKFLOW_MAINTENANCE fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#263238
    style WORKFLOW_OBSERVABILITY fill:#e8f5e9,stroke:#81c784,stroke-width:2px,color:#263238
    style WORKFLOW_AI fill:#f3e5f5,stroke:#ce93d8,stroke-width:1px,color:#263238
    style WORKFLOW_CONTENT fill:#fce4ec,stroke:#f48fb1,stroke-width:1px,color:#263238
