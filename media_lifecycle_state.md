# 🧬 Cycle de Vie du Média (State Diagram)

Ce document illustre les différents états par lesquels passe un fichier vidéo au sein du serveur, depuis son téléchargement initial jusqu'à son archivage final.

## 🔄 États du Fichier

```mermaid
---
config:
  theme: neutral
---
stateDiagram-v2
    [*] --> Downloading : Ajout Torrent
    
    state "📥 Téléchargement" as Downloading {
        Incomplete --> Complete : 100%
    }

    Downloading --> Processing : Déplacement (Sonarr/Radarr)

    state "⚙️ Traitement & Optimisation" as Processing {
        state "Analyse Tdarr" as TdarrCheck
        state "Transcodage (GPU)" as Transcode
        state "Vérification Santé" as HealthCheck
        
        [*] --> TdarrCheck
        TdarrCheck --> HealthCheck : Codec OK (HEVC)
        TdarrCheck --> Transcode : Codec KO (H264/Other)
        Transcode --> HealthCheck : Nouveau Fichier
    }

    Processing --> Available : Import Final

    state "✅ Disponible (Plex)" as Available {
        New --> Watched : Vu par l'utilisateur
        Watched --> Archived : Rétention (Cold Storage)
    }

    Available --> [*] : Suppression Manuelle
```

### Explication des États

1.  **Downloading** : Le fichier est physiquement sur le cache SSD (Warm Storage), géré par le client Torrent.
2.  **Processing** : Phase critique où le fichier est renommé, déplacé, et potentiellement transcodé par Tdarr pour respecter les standards (HEVC).
3.  **Available** : Le fichier est propre, optimisé et indexé par Plex. Il réside sur le stockage de masse (Cold Storage).
