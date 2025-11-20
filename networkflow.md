# 🌐 Flux de Réseau

Ce document illustre la segmentation réseau du serveur, conçue pour isoler les services exposés des services critiques internes.

## 🛡️ Segmentation Réseau

Le réseau est divisé en plusieurs zones de confiance :

1.  **DMZ (Zone Démilitarisée)** :
    *   **Rôle** : Héberge les services accessibles depuis l'extérieur (via Reverse Proxy).
    *   **Sécurité** : Isolée du réseau interne critique. Si un service ici est compromis, l'accès au reste du système reste limité.

2.  **Internal Net (Backend)** :
    *   **Rôle** : Héberge les bases de données, les services *Arrs (Radarr, Sonarr), et les outils de monitoring.
    *   **Sécurité** : Non accessible directement depuis internet. Seuls les services de la DMZ peuvent y accéder via des règles strictes.

3.  **Host & Local Network** :
    *   **Rôle** : Le serveur physique et le réseau domestique.
    *   **Accès** : Gestion via SSH ou IPMI (hors bande) pour l'administration système.

---

## 📊 Diagramme de Flux

```mermaid
---
config:
  layout: elk
  theme: neutral
---
flowchart TD
 subgraph RESEAU_EXTERNE["Internet & Accès"]
        INET("fa:fa-globe Internet")
        GH("fa:fa-github GitHub")
  end
 subgraph DMZ_NET["dmz_net (Services Exposés)"]
    direction LR
        RC("Roundcube")
        AB("Actual Budget")
        PL("Plex")
        OS("Overseerr")
        VW("Vaultwarden")
        NC("Nextcloud")
        P("Portfolio")
        GF("Grafana")
        DSP("Docker Socket Proxy")
        OWUI("Open WebUI")
  end
 subgraph INTERNAL_NET["internal_net (Services Backend)"]
    direction TB
        DB_AI("Bases de Données & IA")
        MEDIA_STACK("Stack Média (*Arrs)")
        MONITORING("Stack Monitoring")
        OPS("Services de Gestion (Hybrides)")
  end
 subgraph HOST["Serveur : Hôte Docker"]
        IRMC["fa:fa-network-wired iRMC S5 (IPMI)"]
        DE["fa:fa-docker Docker Engine"]
        DMZ_NET
        INTERNAL_NET
  end
 subgraph RESEAU_LOCAL["Réseau Local (Home)"]
    direction TB
        ROUTEUR["Routeur/Firewall"]
        NGINX["Reverse Proxy Nginx (Serveur Externe)"]
        HOST
  end
    INET -- HTTPS --> ROUTEUR
    GH -- Synchronisation --> P
    ROUTEUR -- HTTP/S --> NGINX
    ROUTEUR -- IPMI (Hors Bande) --> IRMC
    NGINX --> DMZ_NET
    DE -- Exécute --> DMZ_NET & INTERNAL_NET
    DMZ_NET --> INTERNAL_NET
    OWUI -- API --> DB_AI
    OS -- Commande --> MEDIA_STACK
    GF -- Requête --> MONITORING
    DE@{ shape: rounded}
    
    %% Nouveaux Styles pour un look plus clean et pro
    style RESEAU_EXTERNE fill:#e0f7fa,stroke:#4dd0e1,stroke-width:2px,color:#263238
    style RESEAU_LOCAL fill:#e8f5e9,stroke:#81c784,stroke-width:2px,color:#263238
    
    style HOST fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#263238
    
    style DMZ_NET fill:#e3f2fd,stroke:#64b5f6,stroke-width:1px,color:#263238
    style INTERNAL_NET fill:#f3e5f5,stroke:#ce93d8,stroke-width:1px,color:#263238

    style DB_AI fill:transparent,stroke:transparent
    style MEDIA_STACK fill:transparent,stroke:transparent
    style MONITORING fill:transparent,stroke:transparent
    style OPS fill:transparent,stroke:transparent
```