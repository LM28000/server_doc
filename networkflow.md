# 🌐 Flux de Réseau

[↑ Retour au sommaire](./README.md) | [Infrastructure →](./01_infrastructure.md)

Ce document illustre la **segmentation réseau** du serveur, conçue pour isoler les services exposés des services critiques internes selon le principe de **défense en profondeur** (Defense in Depth).

> 🛡️ **Sécurité** : En cas de compromission d'un service exposé (DMZ), l'accès aux services backend critiques reste protégé par l'isolation réseau.

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
        DASH("Dashboard")
        AUTH("Authelia (SSO)")
        HS("Headscale (VPN)")
        HSUI("Headscale UI")
        RC("Roundcube")
        AB("Actual Budget")
        PL("Plex")
        OS("Overseerr")
        VW("Vaultwarden")
        NC("Nextcloud")
        P("Portfolio")
        GF("Grafana")
        UK("Uptime Kuma")
        DZ("Dozzle")
        GL("Glances")
        IT("IT-Tools")
        MW("MediaWiki")
        KP("Kopia")
        TTYD("TTYD")
        DSP("Docker Socket Proxy")
        OWUI("Open WebUI")
  end
 subgraph INTERNAL_NET["internal_net (Services Backend)"]
    direction TB
        DB_AI("Bases de Données & IA")
        MEDIA_STACK("Stack Média (*Arrs)")
        MONITORING("Stack Monitoring")
        REDIS("Redis (Cache)")
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
    NGINX["Nginx Proxy Manager<br/>*.du-cray.eu"] --> DMZ_NET
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

---

## 🔐 Règles de Sécurité Réseau

### Flux Autorisés

| Source | Destination | Ports | Protocole | Usage |
|--------|-------------|-------|-----------|-------|
| **Internet** | DMZ | 80, 443 | HTTPS | Accès web via reverse proxy |
| **DMZ** | Internal Net | Variés | TCP | API backends, DB |
| **Internal Net** | DMZ | - | - | **❌ Bloqué** (unidirectionnel) |
| **LAN Local** | iRMC | 443 | HTTPS | Gestion hors-bande |
| **LAN Local** | Host SSH | 22 | SSH | Administration |

### Isolation par Zone

#### 🟢 DMZ (Zone Exposée)

**Services** : Plex, Overseerr, Nextcloud, Grafana, Vaultwarden, Open WebUI, Dashboard, Authelia, Headscale, Headscale UI, Kopia, MediaWiki, Uptime Kuma, Dozzle, Glances, IT-Tools, TTYD  
**Accès** : Internet → Reverse Proxy → DMZ  
**Restriction** : Pas d'accès direct aux services backend

#### 🟡 Internal Net (Backend)

**Services** : Bases de données, *Arrs, Qbittorrent, Prometheus, Ollama, Redis  
**Accès** : Uniquement depuis DMZ ou Host  
**Restriction** : Aucun accès depuis Internet

#### 🟠 Host Network

**Services** : Docker Engine, SSH, iRMC  
**Accès** : LAN local uniquement  
**Restriction** : Pas d'exposition Internet directe

---

## 🔒 Best Practices Appliquées

### 1️⃣ Principe du Moindre Privilège

Chaque service n'a accès qu'aux ressources strictement nécessaires.

### 2️⃣ Défense en Profondeur

Plusieurs couches de sécurité :
- Firewall externe (Box/Routeur)
- Reverse Proxy (Nginx avec TLS)
- Segmentation réseau Docker
- VPN pour le trafic P2P
- Socket Proxy pour Docker API

### 3️⃣ Monitoring Continu

Grafana surveille les connexions anormales et les pics de trafic.

---

## 🔧 Configuration Docker Networks

```yaml
networks:
  dmz_net:
    driver: bridge
    ipam:
      config:
        - subnet: 172.19.0.0/16
  
  internal_net:
    driver: bridge
    internal: true  # Pas d'accès Internet direct
    ipam:
      config:
        - subnet: 172.18.0.0/16
```

---

## 🔗 Voir Aussi

- [Infrastructure matérielle](./01_infrastructure.md) - Détails interfaces réseau
- [Sécurité & DRP](./03_maintenance_drp.md) - VPN, Socket Proxy
- [Applications](./02_applications.md) - Attribution des réseaux par service