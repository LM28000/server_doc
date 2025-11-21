# 🎬 Séquence de Requête Média

Ce document détaille les interactions précises entre les services lorsqu'un utilisateur demande un nouveau contenu. Contrairement au diagramme de flux global, ce diagramme de séquence met l'accent sur l'ordre chronologique et les messages échangés.

## ⏱️ Cycle de Vie d'une Requête

Le diagramme ci-dessous illustre le parcours d'une demande de film, de l'interface utilisateur jusqu'au lancement du téléchargement.

```mermaid
---
config:
  layout: elk
  theme: neutral
---
sequenceDiagram
    autonumber
    actor User as 👤 Utilisateur
    participant OS as Overseerr
    participant RD as Radarr
    participant PR as Prowlarr
    participant IDX as Indexers (Trackers)
    participant QB as Qbittorrent

    Note over User, OS: Phase 1 : Demande & Approbation
    User->>OS: Requête du film "Inception"
    OS->>OS: Vérification des droits / Quotas
    OS-->>User: Demande en attente d'approbation
    OS->>OS: Auto-Approbation (si Admin/Configuré)
    OS->>RD: Ajout du film (Profil Qualité: Ultra-HD)
    
    Note over RD, IDX: Phase 2 : Recherche & Grab
    RD->>PR: Recherche des releases disponibles
    PR->>IDX: Requête API (Search Query)
    IDX-->>PR: Liste des résultats (Torrents)
    PR-->>RD: Retourne les résultats standardisés
    
    RD->>RD: Analyse & Filtrage (Custom Formats, Score)
    RD->>RD: Sélection de la meilleure release
    
    Note over RD, QB: Phase 3 : Téléchargement
    RD->>QB: Envoi du fichier .torrent / Magnet
    QB-->>RD: Confirmation (Hash du torrent)
    QB->>QB: Démarrage du téléchargement
    
    loop Monitoring
        RD->>QB: Vérification de la progression
        QB-->>RD: Statut (Downloading, Seeding, Completed)
    end
```

### Points Clés
*   **Prowlarr comme Proxy** : Radarr ne parle jamais directement aux sites de torrents, tout passe par Prowlarr qui standardise les réponses.
*   **Décision Locale** : C'est Radarr qui contient toute la logique de décision (quel fichier prendre ?), Prowlarr ne fait que fournir les options.
*   **Boucle de Monitoring** : Une fois le téléchargement lancé, Radarr surveille activement Qbittorrent pour savoir quand importer le fichier final.
