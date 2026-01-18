# 🔄 Flux de Données & Stratégie de Stockage

[↑ Retour au sommaire](./README.md) | [Infrastructure →](./01_infrastructure.md)

Ce document détaille le cycle de vie de la donnée au sein du serveur, de son acquisition à son archivage. L'architecture repose sur une stratégie de **stockage hiérarchisé (Tiering)** pour optimiser les performances, la durabilité et le coût.

> 🎯 **Objectif** : Maximiser les performances pour les opérations fréquentes (Hot) tout en minimisant les coûts pour le stockage de masse (Cold).

## 💾 Stratégie de Stockage (Tiering)

Le serveur utilise trois types de stockage distincts, chacun adapté à un usage spécifique :

1.  **Hot Storage (M.2 NVMe)** :
    *   **Usage** : Système d'exploitation (Debian), configurations Docker, et bases de données (MariaDB, SQLite).
    *   **Avantage** : Latence ultra-faible et débits élevés pour la réactivité du système et des applications.

2.  **Warm Storage (SATA SSD)** :
    *   **Usage** : Cache d'écriture temporaire et zone de téléchargement.
    *   **Rôle** : Les fichiers sont téléchargés et traités ici avant d'être déplacés vers le stockage de masse. Cela évite de fragmenter les disques durs et permet des décompressions rapides.

3.  **Cold Storage (HDD Pool)** :
    *   **Usage** : Stockage de masse pour les médias (Films, Séries) et les archives.
    *   **Technologie** : **MergerFS** unifie les disques en un seul volume logique, tandis que **SnapRAID** assure la parité (protection contre la panne d'un disque) sans les contraintes d'un RAID classique.

---

## 📊 Diagramme de Flux

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
```

---

## 📈 Avantages de l'Architecture Tiering

### 🚀 Performances

| Couche | Latence | Débit | Usage |
|--------|---------|--------|-------|
| **Hot (M.2)** | < 1ms | ~3500 MB/s | Système, DB, Configs |
| **Warm (SATA SSD)** | ~5ms | ~550 MB/s | Téléchargements, Cache |
| **Cold (HDD)** | ~15ms | ~150 MB/s | Médias, Archives |

### 💰 Coûts

- **M.2 NVMe** : ~0.10€/Go (256 Go = 25€)
- **SATA SSD** : ~0.08€/Go (500 Go = 40€)
- **HDD** : ~0.02€/Go (8x 4To = 640€)

**Total stockage** : ~32 To pour ~700€ vs 3200€ en full SSD

### 🔄 Flux de Données Typique

```
1. Téléchargement → Warm (SATA SSD)
   Durée : 10min - 2h selon taille

2. Traitement Tdarr → Warm (SATA SSD)
   Durée : 20min - 8h selon complexité

3. Déplacement final → Cold (HDD Pool)
   Durée : 5-30min selon taille

4. Accès lecture → Cold (HDD Pool)
   Direct Play : Pas de transcodage nécessaire
```

---

## ⚙️ Configuration Technique

### MergerFS

```bash
# Configuration typique
mergerfs -o defaults,allow_other,use_ino,category.create=mfs \
  /mnt/disk1:/mnt/disk2:/mnt/disk3:/mnt/disk4 \
  /mnt/disk5:/mnt/disk6:/mnt/disk7:/mnt/disk8 \
  /mnt/storage
```

**Politique de création** : `mfs` (most free space) - Les nouveaux fichiers sont écrits sur le disque avec le plus d'espace libre.

### SnapRAID

```bash
# Synchronisation hebdomadaire
snaprraid sync

# Vérification mensuelle
snapraid scrub -p 10  # Vérifie 10% des données
```

**Disques de parité** : 2 disques (permet de survivre à 2 pannes simultanées)

---

## 🔗 Voir Aussi

- [Infrastructure matérielle détaillée](./01_infrastructure.md)
- [Procédures de sauvegarde](./03_maintenance_drp.md)
- [Architecture réseau](./networkflow.md)
