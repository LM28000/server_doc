```mermaid
---
config:
  layout: elk
  theme: neutral
---
flowchart TD
 subgraph DATA_SOURCES["Sources de Données Externes"]
        INET("fa:fa-globe Internet")
        GH("fa:fa-github GitHub (Code Source)")
  end
 subgraph FAST_STORAGE["Stockage Persistant Rapide (M.2)"]
        M2["fa:fa-microchip M.2 SSD (OS/Configs)"]
        DE["fa:fa-docker Docker Engine"]
  end
 subgraph DATA_INGESTION["Acquisition & Cache"]
        QB["fa:fa-download Qbittorrent-VPN"]
        SATA["fa:fa-compact-disc SATA SSD (Cache Téléchargement)"]
  end
 subgraph SLOW_STORAGE["Pool de Stockage (Redondé)"]
        HDDS["fa:fa-hdd 8x HDD (Données brutes)"]
        MFS("MergerFS Pool (Volume Logique)")
        SRD("fa:fa-shield-halved SnapRAID (Parité)")
  end
 subgraph DATA_ACCESS["Services Accédant aux Médias"]
        PL("fa:fa-film Plex")
        NC("fa:fa-cloud Nextcloud")
        RD("Radarr")
        SN("Sonarr")
        BZ("Bazarr")
  end
    INET -- Téléchargement --> QB
    GH -- Clone/Synchro --> M2
    QB -. Fichier Brute .-> SATA
    SATA -. Déplacement .-> MFS
    HDDS -- Agrégation --> MFS
    HDDS -- Parité --> SRD
    DE -- Stockage Configurations --> M2
    MFS -- Montage Read/Write --> DATA_ACCESS
    style DATA_ACCESS fill:#e0f2fe,stroke:#0284c7
    style FAST_STORAGE fill:#fef9c3,stroke:#ca8a04
    style SLOW_STORAGE fill:#ccfbf1,stroke:#0d9488
    style DATA_INGESTION fill:#f3e8ff,stroke:#9333ea
