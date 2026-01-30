# 🧠 Carte Mentale de l'Écosystème (Mindmap)

[↑ Retour au sommaire](./README.md)

Ce document offre une **vue d'ensemble hiérarchique et visuelle** de tous les composants du serveur. C'est le **point d'entrée idéal** pour comprendre "qui fait quoi" en un coup d'œil.

> 💡 **Recommandation** : Commencez ici si vous découvrez le serveur, puis consultez les diagrammes spécifiques pour approfondir.

## 🗺️ Vue d'Ensemble

```mermaid
---
config:
  layout: elk
  theme: neutral
---
mindmap
  root((Serveur
  Antigravity))
    Média
      Plex
        (Streaming)
      Overseerr
        (Requêtes)
      Arrs Stack
        Radarr (Films)
        Sonarr (Séries)
        Bazarr (Subs)
        Prowlarr (Indexers)
      Tdarr
        (Transcodage)
    Téléchargement
      Qbittorrent
        (Client VPN)
      FlareSolverr
        (Anti-DDoS)
    Système & Maintenance
      Watchtower
        (Mises à jour)
      Nextcloud
        (Cloud & Backup)
      Redis
        (Cache)
    Monitoring
      Grafana
        (Dashboards)
      Prometheus
        (Métriques)
      Uptime Kuma
        (Uptime)
      Glances
        (Système)
      Dozzle
        (Logs)
      Scrutiny
        (Santé Disques)
      Tautulli
        (Stats Plex)
    IA & LLM
      Ollama
        (Moteur)
      Open WebUI
        (Interface)
    Sécurité
      Authelia
        (SSO/2FA)
      Headscale
        (VPN Mesh)
      Kopia
        (Sauvegardes)
      Vaultwarden
        (Mots de passe)
    Outils
      Dashboard
        (Accueil)
      IT-Tools
        (Boîte à outils)
      MediaWiki
        (Wiki)
      TTYD
        (Terminal Web)
```

### Organisation

L'écosystème est centré autour de 5 piliers majeurs :
*   **Média** : Le cœur du divertissement.
*   **Téléchargement** : L'acquisition de données.
*   **Système** : La gestion et la pérennité de l'infrastructure.
*   **Monitoring** : La surveillance de la santé du serveur.
*   **IA** : Les capacités d'intelligence artificielle locales.

---

## 📊 Statistiques de l'Écosystème

### Services par Catégorie

| Catégorie | Nombre de Services | Services Critiques |
|-----------|-------------------|--------------------|
| **Média** | 8 | Plex, Radarr, Sonarr |
| **Téléchargement** | 2 | Qbittorrent |
| **Système** | 4 | Nextcloud, Redis, Watchtower |
| **Monitoring** | 8 | Prometheus, Grafana, Uptime Kuma |
| **IA** | 2 | Ollama, Open WebUI |
| **Sécurité** | 5 | Authelia, Headscale, Headscale UI, Kopia, Vaultwarden |
| **Cloud & Productivité** | 5 | Nextcloud, Roundcube, Actual Budget, MediaWiki |
| **Outils** | 5 | Dashboard, IT-Tools, Dozzle, Glances, TTYD |

**Total** : 32 conteneurs actifs

### Ressources Consommées

| Ressource | Utilisation Moyenne | Pic |
|-----------|---------------------|-----|
| **CPU** | 15-20% | 80% (transcodage) |
| **RAM** | 8 Go | 14 Go |
| **Stockage** | 7 To / 32 To | - |
| **Réseau** | 50 Mbps | 200 Mbps (DL) |

---

## 🔗 Navigation Recommandée

### 🎯 Pour Débuter

1. **Vue d'ensemble** : Vous êtes ici ✅
2. **Infrastructure** : [Matériel et réseau](./01_infrastructure.md)
3. **Applications** : [Liste des services](./02_applications.md)
4. **Workflow** : [Comment tout fonctionne ensemble](./workflow.md)

### 🔍 Pour Approfondir

**Architecture** :
- [Segmentation réseau](./networkflow.md)
- [Stratégie de stockage](./dataflow.md)

**Média** :
- [Séquence de requête](./media_request_sequence.md)
- [Cycle de vie](./media_lifecycle_state.md)
- [Transcodage Tdarr](./tdarrflow.md)

**Expérience** :
- [Parcours utilisateur](./user_journey.md)

**Maintenance** :
- [Procédures & DRP](./03_maintenance_drp.md)

---

## 🔗 Liens Externes

### Documentation Officielle

- [Docker Documentation](https://docs.docker.com/)
- [Plex Support](https://support.plex.tv/)
- [TRaSH Guides](https://trash-guides.info/) - Best practices *Arrs
- [MergerFS GitHub](https://github.com/trapexit/mergerfs)
- [SnapRAID Manual](https://www.snapraid.it/manual)

### Communautés

- [r/selfhosted](https://reddit.com/r/selfhosted)
- [r/PleX](https://reddit.com/r/PleX)
- [r/DataHoarder](https://reddit.com/r/DataHoarder)
