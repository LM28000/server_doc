'''mermaid
---
config:
  layout: elk
  theme: neutral
---
flowchart TD
 subgraph WORKFLOW_AUTOMATION["Flux de Travail Média"]
    OS("Overseerr (Requête User)")
    PR("Prowlarr (Indexeur)")
    FS("FlareSolverr (Contournement Anti-DDoS)")
    RD("Radarr")
    SN("Sonarr")
    BZ("Bazarr (Sous-titres)")
    QB["Qbittorrent-VPN"]
  end
 subgraph WORKFLOW_AI["Flux de Travail IA & LLM"]
    OWUI("fa:fa-desktop Open WebUI (Interface)")
    OL("fa:fa-brain Ollama (Moteur LLM)")
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

    %% 1. Workflow Média
    OS -- Commande Nouvelle Tâche --> RD & SN
    PR -- Requête Index --> FS
    RD -- Tâche de Téléchargement --> QB
    SN -- Tâche de Téléchargement --> QB
    BZ -- Synchronise Metadata --> RD & SN
    
    %% 2. Workflow IA
    OWUI -- Requête Chat (API) --> OL
    
    %% 3. Workflow d'Infrastructure
    WT -- Déclenche Mise à Jour --> WORKFLOW_AUTOMATION & WORKFLOW_MAINTENANCE & WORKFLOW_OBSERVABILITY & WORKFLOW_AI
    NC -- Déclenche Tâches Régulières --> NCC
    PL -- Alimente Données --> TT
    SC -- Collecte État --> TT
    
    %% 4. Workflow de Monitoring (Boucle)
    DSP -- Expose Métriques --> CADV
    PROM -- Scrape (Collecte Active) --> CADV & NE
    PROM -- Scrape (Collecte Active) --> SC
    GF -- Requête Visualisation --> PROM
    
    %% Styles Harmonisé (Basé sur le set "Clean & Pro")
    style WORKFLOW_AUTOMATION fill:#e3f2fd,stroke:#64b5f6,stroke-width:1px,color:#263238
    style WORKFLOW_AI fill:#f3e5f5,stroke:#ce93d8,stroke-width:1px,color:#263238
    style WORKFLOW_MAINTENANCE fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#263238
    style WORKFLOW_OBSERVABILITY fill:#e8f5e9,stroke:#81c784,stroke-width:2px,color:#263238
