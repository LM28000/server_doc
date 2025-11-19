# 🖥️ 01 - Infrastructure Physique & Système

## 📦 Matériel (Hardware)

| Catégorie | Détails | Notes |
| :--- | :--- | :--- |
| **Modèle** | [À COMPLÉTER] | |
| **CPU** | [À COMPLÉTER] | |
| **RAM** | [À COMPLÉTER] | |
| **Alimentation** | [À COMPLÉTER] | Onduleur / UPS |
| **Gestion** | iRMC S5 | Interface de gestion hors bande (IPMI) |

### 💾 Stockage Physique

| Rôle | Type | Description |
| :--- | :--- | :--- |
| **Système (OS/DB)** | M.2 SSD | Stockage rapide pour l'OS, configurations et bases de données. |
| **Cache** | SATA SSD | Cache d'écriture et téléchargement temporaire. |
| **Stockage de Masse** | 8x HDD | Pool de données principales. |
| **Architecture** | **MergerFS** + **SnapRAID** | MergerFS unifie les disques, SnapRAID assure la parité. |

---

## ⚙️ Système d'Exploitation (Hôte)

* **Hostname :** `[À COMPLÉTER]`
* **OS & Version :** `[À COMPLÉTER]` (ex: Debian 12 Bookworm)
* **Noyau (Kernel) :** `[À COMPLÉTER]`
* **Utilisateur Root/Admin :** `[À COMPLÉTER]`

---

## 🌐 Réseau & Connectivité

### Configuration IP
| Interface | IP (v4/v6) | MAC Address | Switch/Port |
| :--- | :--- | :--- | :--- |
| **LAN Principal** | `[À COMPLÉTER]` | `[À COMPLÉTER]` | `[À COMPLÉTER]` |
| **IPMI (iRMC)** | `[À COMPLÉTER]` | `[À COMPLÉTER]` | - |
| **Gateway** | `[À COMPLÉTER]` | | |
| **DNS** | `[À COMPLÉTER]` | | |

### Architecture Réseau
L'hôte agit comme point central mais l'accès externe est filtré :
1.  **Internet** → Routeur/Firewall
2.  → **Reverse Proxy Nginx** (Serveur Externe)
3.  → **Hôte Docker** (via `dmz_net`)
