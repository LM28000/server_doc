# 🖥️ 01 - Infrastructure Physique & Système

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
| **Système (OS/DB)** | M.2 SSD | Stockage rapide pour l'OS, configurations et bases de données. |
| **Cache** | SATA SSD | Cache d'écriture et téléchargement temporaire. |
| **Stockage de Masse** | 8x HDD | Pool de données principales. |
| **Architecture** | **MergerFS** + **SnapRAID** | MergerFS unifie les disques, SnapRAID assure la parité. |

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
| **IPMI (iRMC)** | `4c:52:62:46:7f:ef` | `192.168.0.215` | Port de gestion dédié |
| **Gateway** | `192.168.0.1` | - |  |
| **DNS** | `8.8.8.8` | - |  |

### Interfaces Virtuelles & Docker (Bridges)
Le serveur utilise de nombreux sous-réseaux internes pour l'isolation des conteneurs.

| Interface Pont | Sous-réseau (Interne) | Correspondance Probable |
| :--- | :--- | :--- |
| **docker0** | `172.17.0.1/16` | Bridge Docker par défaut |
| **br-265b6517ea68**| `172.18.0.1/16` | `internal_net` |
| **br-de45d84601ec**| `172.19.0.1/16` | `dmz_net` |
| **br-00f608581f6e**| `172.21.0.1/16` | Réseau Custom |
| **br-264f5b3fd647**| `172.23.0.1/16` | Réseau Custom |

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
