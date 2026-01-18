# 📝 Guide de Contribution à la Documentation

[↑ Retour au sommaire](./README.md)

Ce document explique comment maintenir et améliorer la documentation du serveur Antigravity.

---

## 🎯 Principes Directeurs

### 1. Clarté avant tout

- Utiliser un langage simple et direct
- Préférer les exemples concrets aux explications abstraites
- Ajouter des captures d'écran quand pertinent

### 2. Maintenir la cohérence

- Utiliser les mêmes termes pour les mêmes concepts (voir [Glossaire](./GLOSSARY.md))
- Respecter la structure de navigation (liens entre pages)
- Maintenir le même niveau de détail dans chaque document

### 3. Penser à l'utilisateur

- Chaque document doit pouvoir être lu indépendamment
- Fournir des liens de navigation en haut et en bas
- Ajouter des sections "Voir Aussi" pour approfondir

---

## 📋 Structure des Documents

### Template de Base

```markdown
# 🔷 Titre du Document

[↑ Retour au sommaire](./README.md) | [Lien contextuel →](./autre-doc.md)

Introduction courte expliquant le sujet du document.

> 💡 **Note importante** : Contexte ou recommandation

## 📑 Table des Matières (si > 200 lignes)

- [Section 1](#section-1)
- [Section 2](#section-2)

---

## Section 1

Contenu...

## Section 2

Contenu...

---

## 🔗 Voir Aussi

- [Document connexe 1](./lien1.md)
- [Document connexe 2](./lien2.md)

---

*Dernière mise à jour : JJ/MM/AAAA*
```

### Niveaux de Titres

- **H1 (`#`)** : Titre principal du document (1 seul par fichier)
- **H2 (`##`)** : Sections principales
- **H3 (`###`)** : Sous-sections
- **H4 (`####`)** : Détails spécifiques (rare)

---

## 🎨 Conventions Visuelles

### Emojis par Catégorie

| Catégorie | Emojis Recommandés | Usage |
|-----------|-------------------|--------|
| **Infrastructure** | 🖥️ 💾 🌐 📦 | Matériel, stockage, réseau |
| **Sécurité** | 🔒 🛡️ 🔐 🔑 | Sécurité, authentification |
| **Média** | 🎬 🎥 📺 🍿 | Contenus, streaming |
| **Monitoring** | 📊 📈 📉 🔍 | Métriques, analyse |
| **IA** | 🤖 🧠 💬 | Intelligence artificielle |
| **Maintenance** | 🔧 🛠️ ⚙️ | Configuration, maintenance |
| **Documentation** | 📖 📝 📚 📑 | Guides, références |
| **Navigation** | ↑ ↓ ← → 🔗 | Liens de navigation |
| **Status** | ✅ ⚠️ ❌ 🔴 🟡 🟢 | États, priorités |

### Blocs Informatifs

#### Astuce / Conseil
```markdown
> 💡 **Conseil** : Utilisez cette approche pour...
```

#### Avertissement
```markdown
> ⚠️ **Attention** : Cette opération est dangereuse si...
```

#### Information Importante
```markdown
> 📝 **Note** : Cette configuration nécessite...
```

#### Sécurité Critique
```markdown
> 🔒 **Sécurité** : Ne jamais exposer ce service directement...
```

---

## 📊 Diagrammes Mermaid

### Bonnes Pratiques

1. **Configuration** : Toujours inclure le bloc de configuration
   ```markdown
   ```mermaid
   ---
   config:
     layout: elk
     theme: neutral
   ---
   ```

2. **Styles** : Utiliser des couleurs cohérentes
   - Bleu (`#e3f2fd`) : Média / Data flow
   - Orange (`#fff3e0`) : Système / Maintenance
   - Vert (`#e8f5e9`) : Monitoring
   - Violet (`#f3e5f5`) : IA / Processing

3. **Lisibilité** : Ne pas surcharger, préférer plusieurs petits diagrammes

### Types de Diagrammes par Usage

| Type | Usage | Exemple |
|------|-------|---------|
| **Flowchart** | Processus, décisions | [workflow.md](./workflow.md) |
| **Sequence** | Interactions chronologiques | [media_request_sequence.md](./media_request_sequence.md) |
| **State** | États d'un objet | [media_lifecycle_state.md](./media_lifecycle_state.md) |
| **Journey** | Expérience utilisateur | [user_journey.md](./user_journey.md) |
| **Mindmap** | Vue hiérarchique | [server_ecosystem_mindmap.md](./server_ecosystem_mindmap.md) |

---

## ✍️ Processus de Mise à Jour

### Quand mettre à jour la documentation ?

#### ✅ Obligatoire

- Ajout/suppression d'un service
- Changement d'architecture réseau
- Modification des chemins de stockage
- Mise à jour majeure d'OS ou de version Docker
- Changement de procédure de sauvegarde

#### 📝 Recommandé

- Optimisation de configuration
- Ajout de règles de sécurité
- Découverte de best practices
- Ajout d'exemples ou de cas d'usage

#### ❌ Non nécessaire

- Changement de ports mineurs
- Mise à jour de conteneurs (version identique)
- Modifications cosmétiques

### Checklist de Mise à Jour

```markdown
- [ ] Modifier le document principal concerné
- [ ] Mettre à jour les liens de navigation si nécessaire
- [ ] Vérifier la cohérence avec le glossaire
- [ ] Mettre à jour la date "Dernière mise à jour"
- [ ] Vérifier les diagrammes Mermaid (si modifiés)
- [ ] Tester les liens internes
```

---

## 🔍 Revue de Documentation

### Auto-Vérification

Avant de valider une modification, vérifier :

1. **Orthographe et grammaire** : Relecture attentive
2. **Liens** : Tous les liens fonctionnent
3. **Diagrammes** : S'affichent correctement
4. **Navigation** : Liens de retour présents
5. **Cohérence** : Terminologie identique au glossaire
6. **Complétude** : Section "Voir Aussi" pertinente

### Questions à se Poser

- ✅ Un nouvel arrivant peut-il comprendre ce document ?
- ✅ Les informations sont-elles à jour ?
- ✅ Les exemples sont-ils pertinents ?
- ✅ La structure est-elle logique ?
- ✅ Les liens de navigation facilitent-ils la découverte ?

---

## 📚 Ressources pour Rédacteurs

### Markdown

- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Flavored Markdown](https://github.github.com/gfm/)

### Mermaid

- [Documentation officielle](https://mermaid.js.org/)
- [Live Editor](https://mermaid.live/)
- [Exemples](https://mermaid.js.org/ecosystem/integrations.html)

### Style d'écriture

- [Microsoft Writing Style Guide](https://learn.microsoft.com/en-us/style-guide/welcome/)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)

---

## 🎯 Exemples de Bonne Documentation

### ✅ Bon Exemple

```markdown
## Configuration du VPN

Le conteneur Qbittorrent utilise OpenVPN pour sécuriser le trafic P2P.

**Étapes de configuration** :

1. Créer un fichier `openvpn.ovpn` avec les identifiants du fournisseur
2. Monter le fichier dans le conteneur : `/config/openvpn/`
3. Vérifier la connexion : `docker exec qbittorrent curl ifconfig.me`

> ⚠️ **Killswitch** : Si le VPN se déconnecte, le trafic est automatiquement coupé.

**Voir aussi** : [Sécurité réseau](./03_maintenance_drp.md#vpn-qbittorrent)
```

### ❌ Mauvais Exemple

```markdown
## VPN

Il faut configurer le VPN. Mettre le fichier dans le dossier config.
Ça marche avec OpenVPN.
```

**Problèmes** :
- Pas assez de détails
- Pas d'exemples concrets
- Pas de liens de référence
- Pas d'avertissements importants

---

## 🔗 Index des Documents

### Documents Principaux

1. [README.md](./README.md) - Point d'entrée
2. [01_infrastructure.md](./01_infrastructure.md) - Infrastructure physique
3. [02_applications.md](./02_applications.md) - Services et applications
4. [03_maintenance_drp.md](./03_maintenance_drp.md) - Maintenance et DRP
5. [GLOSSARY.md](./GLOSSARY.md) - Glossaire technique

### Diagrammes

1. [server_ecosystem_mindmap.md](./server_ecosystem_mindmap.md) - Vue d'ensemble
2. [workflow.md](./workflow.md) - Workflow global
3. [networkflow.md](./networkflow.md) - Architecture réseau
4. [dataflow.md](./dataflow.md) - Flux de données
5. [media_request_sequence.md](./media_request_sequence.md) - Séquence média
6. [media_lifecycle_state.md](./media_lifecycle_state.md) - Cycle de vie
7. [tdarrflow.md](./tdarrflow.md) - Pipeline Tdarr
8. [user_journey.md](./user_journey.md) - Parcours utilisateur

### Méta-Documentation

1. [CONTRIBUTING.md](./CONTRIBUTING.md) - Ce document
2. [GLOSSARY.md](./GLOSSARY.md) - Définitions et références

---

## 📞 Contact

Pour toute question ou suggestion d'amélioration de la documentation :

- Ouvrir une issue sur le dépôt
- Proposer une pull request avec les modifications
- Contacter l'administrateur du serveur

---

*Dernière mise à jour : 18/01/2026*
