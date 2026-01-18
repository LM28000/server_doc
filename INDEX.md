# 🗂️ Index Complet de la Documentation

Navigation rapide vers tous les documents du serveur Antigravity.

---

## 📚 Documents Principaux

### 🏠 Point d'Entrée
- **[README.md](./README.md)** - Vue d'ensemble et navigation principale ⭐ **Commencer ici**

### 📖 Documentation de Référence
1. **[01_infrastructure.md](./01_infrastructure.md)** - Infrastructure physique et système
   - Matériel (Fujitsu TX1330 M4)
   - Stockage (M.2, SSD, 8x HDD)
   - Réseau (IPs, interfaces, bridges Docker)

2. **[02_applications.md](./02_applications.md)** - Applications et services
   - Cloud & Productivité (Nextcloud, Vaultwarden, etc.)
   - Média Center (*Arrs, Plex, Tdarr)
   - IA (Ollama, Open WebUI)
   - Monitoring (Prometheus, Grafana)

3. **[03_maintenance_drp.md](./03_maintenance_drp.md)** - Maintenance et DRP
   - Stratégie de sauvegarde (3-2-1)
   - Procédures de restauration
   - Sécurité réseau (VPN, Socket Proxy)
   - Maintenance planifiée

---

## 📊 Diagrammes et Visualisations

### 🗺️ Vue d'Ensemble
- **[server_ecosystem_mindmap.md](./server_ecosystem_mindmap.md)** - Carte mentale complète ⭐ **Vue globale**
  - 5 piliers : Média, Téléchargement, Système, Monitoring, IA
  - 25+ services organisés

### 🔄 Workflows et Architecture
- **[workflow.md](./workflow.md)** - Flux de travail global
  - Automation Média
  - Flux IA
  - Maintenance
  - Observabilité

- **[networkflow.md](./networkflow.md)** - Architecture réseau
  - Segmentation (DMZ, Internal, Host)
  - Règles de sécurité
  - Configuration Docker Networks

- **[dataflow.md](./dataflow.md)** - Flux de données et stockage
  - Stratégie Tiering (Hot/Warm/Cold)
  - MergerFS + SnapRAID
  - Comparatif performances/coûts

### 🎬 Workflows Média Détaillés
- **[media_request_sequence.md](./media_request_sequence.md)** - Séquence chronologique
  - De la requête au téléchargement (15 étapes)
  - Interactions Overseerr → Radarr → Prowlarr → Qbittorrent

- **[media_lifecycle_state.md](./media_lifecycle_state.md)** - Cycle de vie du fichier
  - États : Downloading → Processing → Available
  - Durées moyennes et transitions

- **[tdarrflow.md](./tdarrflow.md)** - Pipeline de transcodage
  - Configuration Tdarr (HEVC/H.265)
  - Nodes distribués (Serveur + MacBook M4)
  - Statistiques de gain d'espace

### 👤 Expérience Utilisateur
- **[user_journey.md](./user_journey.md)** - Parcours utilisateur
  - Scénario : Soirée Cinéma
  - Analyse satisfaction (points forts/friction)
  - Objectifs UX

---

## 🔧 Documentation Technique

### 📖 Références
- **[GLOSSARY.md](./GLOSSARY.md)** - Glossaire technique ⭐ **Définitions**
  - Acronymes (DMZ, IPMI, HEVC, etc.)
  - Concepts clés
  - Ports & Protocoles
  - Chemins système
  - Index thématique

### 📝 Méta-Documentation
- **[CONTRIBUTING.md](./CONTRIBUTING.md)** - Guide de contribution
  - Principes directeurs
  - Structure des documents
  - Conventions visuelles (emojis, blocs)
  - Diagrammes Mermaid
  - Processus de mise à jour

- **[CHANGELOG.md](./CHANGELOG.md)** - Historique des modifications
  - Version 2.0.0 (18/01/2025) - Refonte complète
  - Version 1.0.0 (20/11/2025) - Version initiale

---

## 📂 Fichiers de Configuration

### stacks/
Contient les fichiers de configuration Docker Compose :
- `authelia.yaml` - Authentification unifiée
- `kopia.yaml` - Sauvegardes
- `mediawiki.yaml` - Wiki
- `securedmz.yaml` - Services DMZ

---

## 🎯 Navigation par Objectif

### 🆕 Je découvre le serveur
1. ➡️ [README.md](./README.md) - Point d'entrée
2. ➡️ [server_ecosystem_mindmap.md](./server_ecosystem_mindmap.md) - Vue globale
3. ➡️ [workflow.md](./workflow.md) - Comment ça fonctionne
4. ➡️ [01_infrastructure.md](./01_infrastructure.md) - Le matériel

### 🎬 Je veux comprendre le Média
1. ➡️ [user_journey.md](./user_journey.md) - Expérience utilisateur
2. ➡️ [media_request_sequence.md](./media_request_sequence.md) - Étapes détaillées
3. ➡️ [media_lifecycle_state.md](./media_lifecycle_state.md) - États du fichier
4. ➡️ [tdarrflow.md](./tdarrflow.md) - Optimisation vidéo
5. ➡️ [02_applications.md](./02_applications.md#-média-center--automatisation) - Services média

### 🔧 Je veux administrer
1. ➡️ [01_infrastructure.md](./01_infrastructure.md) - Configuration système
2. ➡️ [02_applications.md](./02_applications.md) - Services installés
3. ➡️ [03_maintenance_drp.md](./03_maintenance_drp.md) - Maintenance et sauvegardes
4. ➡️ [networkflow.md](./networkflow.md) - Sécurité réseau
5. ➡️ [GLOSSARY.md](./GLOSSARY.md) - Référence technique

### 🔍 Je cherche une information spécifique
1. ➡️ [GLOSSARY.md](./GLOSSARY.md) - Définitions et index thématique
2. ➡️ Utiliser Ctrl+F dans les documents

### 📝 Je veux contribuer
1. ➡️ [CONTRIBUTING.md](./CONTRIBUTING.md) - Guide de contribution
2. ➡️ [CHANGELOG.md](./CHANGELOG.md) - Historique des modifications

---

## 📊 Statistiques de la Documentation

### Nombre de Fichiers
- **3** Documents principaux (Infrastructure, Applications, Maintenance)
- **8** Diagrammes (Workflows, Architecture, Expérience)
- **3** Documents de référence (Glossaire, Contribution, Changelog)
- **1** Index (ce fichier)
- **4** Fichiers de configuration (stacks/)

**Total** : 19 fichiers markdown

### Contenu Estimé
- **~15,000 mots** de documentation
- **12 diagrammes Mermaid** interactifs
- **50+ tableaux** de référence
- **100+ liens** de navigation

---

## 🔗 Liens Externes Utiles

### Documentation Officielle
- [Docker](https://docs.docker.com/)
- [Plex](https://support.plex.tv/)
- [TRaSH Guides](https://trash-guides.info/) - Best practices *Arrs
- [MergerFS](https://github.com/trapexit/mergerfs)
- [SnapRAID](https://www.snapraid.it/)

### Communautés
- [r/selfhosted](https://reddit.com/r/selfhosted)
- [r/PleX](https://reddit.com/r/PleX)
- [r/DataHoarder](https://reddit.com/r/DataHoarder)

---

## 📈 Évolution de la Documentation

### Version Actuelle : 2.0.0 (18/01/2025)
- ✅ Navigation inter-documents complète
- ✅ Tables des matières
- ✅ Glossaire technique
- ✅ Guide de contribution
- ✅ Sections enrichies (dépannage, statistiques)
- ✅ Diagrammes détaillés avec explications

### Prochaines Améliorations Prévues
- 🔄 Captures d'écran des interfaces
- 🔄 Tutoriels vidéo
- 🔄 FAQ (Questions fréquentes)
- 🔄 Procédures de mise à jour pas-à-pas
- 🔄 Scripts d'automatisation documentés

---

*Ce fichier est généré automatiquement. Dernière mise à jour : 18/01/2025*
