# 📘 Documentation Serveur Hôte

Bienvenue dans la documentation technique du serveur domestique **Antigravity**. Ce dépôt contient les détails de configuration, les procédures de maintenance et l'architecture complète des services.

## 📑 Table des Matières

- [Vue d'ensemble](#-vue-densemble)
- [Structure de la documentation](#-structure-de-la-documentation)
- [Diagrammes](#-diagrammes)
- [Guide de démarrage rapide](#-guide-de-démarrage-rapide)
- [Liste complète des services](./SERVICES.md) 📋
- [Index complet](./INDEX.md) 🗂️
- [Glossaire technique](./GLOSSARY.md) 📖

## 📌 Vue d'ensemble

* **Hostname :** `debian`
* **Rôle :** Hôte Docker pour Média, IA, Domotique et Monitoring
* **OS :** Debian GNU/Linux 13 (Trixie)
* **Architecture :** Intel Xeon E-2136 (6C/12T) + 16 Go RAM
* **Technologie principale :** Docker & Docker Compose
* **Stockage :** Pool MergerFS (8x HDD) + Parité SnapRAID

## 📂 Structure de la documentation

1.  [**Infrastructure Physique & Système**](./01_infrastructure.md)
    * Détails Hardware, Réseau (IPs, MAC), OS Hôte.
2.  [**Applications & Services**](./02_applications.md)
    * Architecture Docker, Flux de travail (Média, IA), Ports.
3.  [**Maintenance & DRP**](./03_maintenance_drp.md)
    * Mises à jour, Sauvegardes, Restauration.

## 📊 Diagrammes

### Vues d'Ensemble

* 🗺️ [**Carte Mentale de l'Écosystème**](./server_ecosystem_mindmap.md) - Vue hiérarchique complète de tous les composants *(Recommandé pour débuter)*
* 🔄 [**Flux de Travail**](./workflow.md) - Vue d'ensemble de l'automatisation du serveur et orchestration des services

### Architecture

* 🌐 [**Flux de Réseau**](./networkflow.md) - Segmentation réseau, DMZ et zones de sécurité
* 💾 [**Flux de Données & Stratégie de Stockage**](./dataflow.md) - Cycle de vie de la donnée et architecture de stockage hiérarchisé (tiering)

### Workflows Métier

* 🎬 [**Séquence de Requête Média**](./media_request_sequence.md) - Interactions chronologiques détaillées lors d'une demande de contenu
* 🧬 [**Cycle de Vie du Média**](./media_lifecycle_state.md) - États d'un fichier vidéo du téléchargement à l'archivage
* 🔄 [**Flux de Travail Tdarr**](./tdarrflow.md) - Pipeline de transcodage vidéo automatisé (HEVC/H.265)
* 🗺️ [**Parcours Utilisateur**](./user_journey.md) - Expérience utilisateur et interactions émotionnelles

## 🚀 Guide de démarrage rapide

### Pour les nouveaux arrivants

1. Consultez d'abord la [Carte Mentale](./server_ecosystem_mindmap.md) pour avoir une vue d'ensemble
2. Lisez l'[Infrastructure](./01_infrastructure.md) pour comprendre la base matérielle
3. Parcourez les [Applications](./02_applications.md) pour découvrir les services disponibles
4. Référez-vous à la [Maintenance & DRP](./03_maintenance_drp.md) pour les procédures de sauvegarde

### Pour les administrateurs

- **Accès système** : SSH sur `192.168.0.75`
- **Gestion hors-bande** : iRMC sur `192.168.0.215`
- **Monitoring** : Grafana (port 3000)
- **Gestion Docker** : Portainer ou CLI directe

### Services principaux

| Service | Port | URL d'accès | Catégorie |
|---------|------|-------------|-----------|
| **Dashboard** | 8089 | Via reverse proxy | 🏠 Accueil |
| **Authelia** | 9091 | Via reverse proxy | 🔐 SSO |
| **Headscale** | 9999 | Via reverse proxy | 🌐 VPN |
| **Headscale UI** | 9092 | Via reverse proxy | 🌐 VPN |
| **Plex** | 32400 | Via reverse proxy | 🎬 Média |
| **Overseerr** | 5000 | Via reverse proxy | 🎬 Média |
| **Nextcloud** | 8081 | Via reverse proxy | ☁️ Cloud |
| **Vaultwarden** | 8084 | Via reverse proxy | 🔐 Sécurité |
| **Grafana** | 3000 | Via reverse proxy | 📊 Monitoring |
| **Uptime Kuma** | 3002 | Via reverse proxy | 📊 Monitoring |
| **Kopia** | 8200 | Via reverse proxy | 💾 Sauvegardes |
| **MediaWiki** | 8083 | Via reverse proxy | 📚 Wiki |
| **IT-Tools** | 8090 | Via reverse proxy | 🛠️ Outils |

> 💡 **Point d'entrée recommandé** : Dashboard (port 8089) qui centralise l'accès à tous les services.

---

## 📞 Support et Contribution

Pour toute question ou amélioration, veuillez consulter les diagrammes appropriés ou ouvrir une issue.

- 📖 [Glossaire technique](./GLOSSARY.md) - Définitions et références
- 📝 [Guide de contribution](./CONTRIBUTING.md) - Comment améliorer cette documentation

---

*Dernière mise à jour : 18/01/2026*
