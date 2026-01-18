# 📝 Historique des Modifications

Toutes les modifications notables de la documentation sont documentées dans ce fichier.

Le format est basé sur [Keep a Changelog](https://keepachangelog.com/fr/1.0.0/).

---

## [2.0.0] - 18/01/2025

### ✨ Ajouté

#### Navigation & Structure
- 🔗 Liens de navigation entre tous les documents (retour sommaire, liens contextuels)
- 📑 Tables des matières pour les documents principaux
- 📖 **Nouveau fichier** : [GLOSSARY.md](./GLOSSARY.md) - Glossaire technique complet
- 📝 **Nouveau fichier** : [CONTRIBUTING.md](./CONTRIBUTING.md) - Guide de contribution
- 📋 **Nouveau fichier** : CHANGELOG.md (ce fichier)

#### Contenu Enrichi
- 🎯 Section "Guide de démarrage rapide" dans le README
- 📊 Statistiques et métriques détaillées dans tous les diagrammes
- 🔧 Sections de dépannage dans les documents techniques
- 💡 Blocs d'information (conseils, avertissements) partout
- 🔗 Sections "Voir Aussi" avec liens contextuels

#### Documentation Technique
- ⚙️ Configuration détaillée Tdarr avec exemples de plugins
- 🔐 Règles de sécurité réseau détaillées (DMZ, Internal, Host)
- 💾 Explication complète de la stratégie de stockage (Tiering)
- 📈 Statistiques de gain d'espace HEVC vs H.264
- 🎯 Métriques UX et objectifs de performance

### 📝 Amélioré

#### README.md
- Ajout de la table des matières complète
- Section "Vue d'ensemble" enrichie avec détails système
- Organisation des diagrammes par catégorie (vues d'ensemble, architecture, workflows)
- Guide de démarrage avec services principaux et ports

#### 01_infrastructure.md
- Table des matières et navigation
- Section stockage avec explication Hot/Warm/Cold Storage
- Contexte de sécurité pour les interfaces réseau
- Détails sur MergerFS et SnapRAID

#### 02_applications.md
- Table des matières et navigation complète
- Colonne "Réseau" ajoutée aux tableaux de services
- Section Tdarr enrichie avec détails techniques
- Ajout de Scrutiny dans le monitoring
- Notes de sécurité pour chaque catégorie

#### 03_maintenance_drp.md
- Table des matières détaillée
- Classification des sauvegardes par priorité (CRITIQUE, HAUTE, MOYENNE)
- **Nouvelle section** : Procédures de restauration (complète et partielle)
- **Nouvelle section** : Maintenance planifiée (hebdo, mensuelle, trimestrielle)
- Détails techniques VPN avec commandes de test
- RTO/RPO définis (2-4h / 24h)

#### Diagrammes

**workflow.md**
- Section "Détails des flux" avec durées et services
- Explication de chaque flux (Média, IA, Maintenance, Monitoring)
- Liens vers documentation détaillée

**dataflow.md**
- **Nouvelle section** : Avantages de l'architecture Tiering
- Tableau comparatif performances/coûts par couche
- Exemple de flux de données typique avec durées
- Configuration technique MergerFS et SnapRAID

**networkflow.md**
- **Nouvelle section** : Règles de sécurité réseau (tableau détaillé)
- Isolation par zone avec restrictions
- Best practices appliquées (3 niveaux)
- Configuration Docker Networks en YAML

**tdarrflow.md**
- **Nouvelle section** : Configuration Tdarr (plugin et paramètres)
- Architecture distribuée (tableau comparatif serveur vs M4)
- **Nouvelle section** : Statistiques (gains d'espace, qualité)
- **Nouvelle section** : Dépannage (3 cas courants)

**media_request_sequence.md**
- **Nouvelle section** : Détails des étapes (durées, critères)
- Explication de la sélection et du filtrage
- **Nouvelle section** : Notifications (4 points de contact)

**media_lifecycle_state.md**
- **Nouvelle section** : Détails par état (localisation, durée, gestion)
- Sous-étapes du Processing expliquées
- Tableau des durées moyennes (min/max/moyenne)
- **Nouvelle section** : Dépannage (2 cas courants avec commandes)

**user_journey.md**
- **Nouvelle section** : Analyse détaillée (points forts/améliorations)
- **Nouveaux diagrammes** : Admin monitoring, Utilisateur IA
- **Nouvelle section** : Objectifs UX avec métriques

**server_ecosystem_mindmap.md**
- **Nouvelle section** : Statistiques de l'écosystème
- Tableau ressources consommées
- **Nouvelle section** : Navigation recommandée
- **Nouvelle section** : Liens externes (doc officielle, communautés)

### 🔧 Corrigé
- Dates de mise à jour harmonisées à 18/01/2025
- Correction de "Debian 13.1" en "Debian GNU/Linux 13 (Trixie)"
- Uniformisation des termes (voir Glossaire)
- Correction des chemins de fichiers dans les exemples

### 🎨 Style
- Emojis cohérents selon les catégories
- Formatage Markdown standardisé
- Blocs de code avec syntaxe appropriée
- Tableaux formatés uniformément

---

## [1.0.0] - 20/11/2025

### Ajouté
- Documentation initiale complète
- 8 fichiers de diagrammes Mermaid
- 3 documents principaux (Infrastructure, Applications, Maintenance)
- README avec structure de base

---

## Types de Changements

- `✨ Ajouté` : Nouvelles fonctionnalités ou sections
- `📝 Amélioré` : Améliorations de contenu existant
- `🔧 Corrigé` : Corrections de bugs ou erreurs
- `🗑️ Supprimé` : Fonctionnalités ou sections retirées
- `🔒 Sécurité` : Modifications liées à la sécurité
- `🎨 Style` : Changements de formatage uniquement

---

*Ce changelog est maintenu manuellement. Pensez à le mettre à jour lors de chaque modification significative de la documentation.*
