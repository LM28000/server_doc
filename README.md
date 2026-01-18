# 📘 Documentation Serveur Hôte

Bienvenue dans la documentation technique du serveur domestique **Antigravity**. Ce dépôt contient les détails de configuration, les procédures de maintenance et l'architecture complète des services.

## 📑 Table des Matières

- [Vue d'ensemble](#-vue-densemble)
- [Structure de la documentation](#-structure-de-la-documentation)
- [Diagrammes](#-diagrammes)
- [Guide de démarrage rapide](#-guide-de-démarrage-rapide)
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

| Service | Port | URL d'accès |
|---------|------|-------------|
| Plex | 32400 | Via reverse proxy |
| Overseerr | 5000 | Via reverse proxy |
| Grafana | 3000 | Via reverse proxy |
| Nextcloud | 8081 | Via reverse proxy |

---

## 📞 Support et Contribution

Pour toute question ou amélioration, veuillez consulter les diagrammes appropriés ou ouvrir une issue.

- 📖 [Glossaire technique](./GLOSSARY.md) - Définitions et références
- 📝 [Guide de contribution](./CONTRIBUTING.md) - Comment améliorer cette documentation

---

*Dernière mise à jour : 18/01/2025*
