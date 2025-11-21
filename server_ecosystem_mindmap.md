# 🧠 Carte Mentale de l'Écosystème (Mindmap)

Ce document offre une vue d'ensemble hiérarchique et visuelle de tous les composants du serveur. C'est le point d'entrée idéal pour comprendre "qui fait quoi" en un coup d'œil.

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
      Portainer
        (Gestion Docker)
    Monitoring
      Grafana
        (Dashboards)
      Prometheus
        (Métriques)
      Scrutiny
        (Santé Disques)
      Tautulli
        (Stats Plex)
    IA & LLM
      Ollama
        (Moteur)
      Open WebUI
        (Interface)
```

### Organisation

L'écosystème est centré autour de 5 piliers majeurs :
*   **Média** : Le cœur du divertissement.
*   **Téléchargement** : L'acquisition de données.
*   **Système** : La gestion et la pérennité de l'infrastructure.
*   **Monitoring** : La surveillance de la santé du serveur.
*   **IA** : Les capacités d'intelligence artificielle locales.
