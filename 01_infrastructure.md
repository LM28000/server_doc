# 🖥️ 01 - Infrastructure Physique & Système

[← Retour au sommaire](./README.md) | [Applications →](./02_applications.md)

## 📑 Table des Matières

- [Matériel (Hardware)](#-matériel-hardware)
- [Système d'Exploitation](#️-système-dexploitation-hôte)
- [Réseau & Connectivité](#-réseau--connectivité)

---

## 📦 Matériel (Hardware)

| Catégorie | Détails | Notes |
| :--- | :--- | :--- |
| **Modèle** | **Fujitsu PRIMERGY TX1330 M4** | Serveur Tour |
| **CPU** | Intel Xeon E-2136 @ 3.30GHz | 6 Cœurs / 12 Threads |
| **RAM** | 16 Go DDR4 UDIMM | ECC (Probablement, vu le serveur) |
| **Alimentation** | 450 Watt | Protégé par **Onduleur** |
| **Gestion** | **iRMC S5** | Interface de gestion hors bande (IPMI) |

### 💾 Stockage Physique

| Rôle | Type | Description |
| :--- | :--- | :--- |
| **Système (OS/DB)** | M.2 NVMe SSD | Stockage rapide pour l'OS, configurations Docker et bases de données (Hot Storage). |
| **Cache** | SATA SSD | Cache d'écriture et zone de téléchargement temporaire (Warm Storage). |
| **Stockage de Masse** | 8x HDD SATA | Pool de données principales pour les médias (Cold Storage). |
| **Architecture** | **MergerFS** + **SnapRAID** | MergerFS unifie les disques en un volume logique, SnapRAID assure la parité (1-2 disques). |

> 💡 **Note** : Cette architecture de stockage hiérarchisé (tiering) optimise les performances tout en maintenant un coût par To raisonnable. Voir le [diagramme de flux de données](./dataflow.md) pour plus de détails.

---

## ⚙️ Système d'Exploitation (Hôte)

* **Hostname :** `debian`
* **OS & Version :** `Debian GNU/Linux 13 (trixie)`
* **Branche :** Testing

---

## 🌐 Réseau & Connectivité

### Configuration IP (Interfaces Physiques)

| Interface | IP (v4) | MAC Address | État / Note |
| :--- | :--- | :--- | :--- |
| **eno1** (LAN) | `192.168.0.75/24` | `4c:52:62:46:7f:ee` |  |
| **eno2** | - | `4c:52:62:46:7f:ef` |  |
| **IPMI (iRMC)** | `192.168.0.215` | `4c:52:62:46:7f:ef` | Port de gestion dédié |
| **Gateway** | `192.168.0.1` | - |  |
| **DNS** | `8.8.8.8` | - |  |

### Interfaces Virtuelles & Docker (Bridges)

Le serveur utilise de nombreux sous-réseaux internes pour l'isolation des conteneurs, conférant une sécurité accrue.

| Interface Pont | Sous-réseau (Interne) | Correspondance Probable |
| :--- | :--- | :--- |
| **docker0** | `172.17.0.1/16` | Bridge Docker par défaut (non utilisé) |
| **br-265b6517ea68**| `172.18.0.1/16` | Réseau `internal_net` (Backend) |
| **br-de45d84601ec**| `172.19.0.1/16` | Réseau `dmz_net` (Services exposés) |
| **br-00f608581f6e**| `172.21.0.1/16` | Réseau Custom |
| **br-264f5b3fd647**| `172.23.0.1/16` | Réseau Custom |

> 🔐 **Sécurité** : La segmentation réseau isole les services critiques des services exposés. Consultez le [diagramme de flux réseau](./networkflow.md) pour comprendre l'architecture complète.

### Topologie Simplifiée
```mermaid
flowchart LR
    INET((Internet)) --> BOX[Routeur / Box]
    BOX -- 192.168.0.x --> ENO1[Interface eno1<br>192.168.0.75]
    
    subgraph SERVER ["Serveur (Debian)"]
        ENO1 --> DOCKER[Docker Engine]
        DOCKER --> BR1[Bridge dmz_net<br>172.18.x.x]
        DOCKER --> BR2[Bridge internal_net<br>172.19.x.x]
    end
