# 🔄 Flux de Travail Tdarr

Ce document détaille le pipeline de traitement vidéo automatisé géré par Tdarr. L'objectif est de standardiser tous les médias pour assurer une compatibilité maximale (Direct Play) et réduire l'espace de stockage.

## 🎬 Pipeline de Transcodage

Le flux de travail suit une logique conditionnelle stricte :

1.  **Analyse (Health Check)** :
    *   Chaque nouveau fichier est analysé pour vérifier son codec vidéo.
    *   **Cible** : HEVC (H.265) dans un conteneur MKV.

2.  **Transcodage (Si nécessaire)** :
    *   Si le fichier n'est pas en HEVC, Tdarr lance une tâche de transcodage.
    *   **Outil** : FFmpeg est utilisé avec l'accélération matérielle (si disponible sur le nœud) ou logicielle (libx265).

3.  **Remplacement** :
    *   Une fois le transcodage réussi, le fichier original est remplacé par la version optimisée.

---

## 📊 Diagramme de Flux

```mermaid
---
config:
  layout: elk
  theme: neutral
---
flowchart TB
 subgraph FFMPEG_STACK["Construction & Exécution FFmpeg"]
        CMD_BEG("fa:fa-play-circle Begin Command")
        CMD_ENC("fa:fa-film Set Video Encoder<br>(hevc_videotoolbox / libx265)")
        CMD_CONT("fa:fa-box-open Set Container<br>(.mkv)")
        CMD_EXEC("fa:fa-bolt Execute Transcode")
  end
 subgraph TDARR_FLOW["Flux Tdarr : Plugin Logic"]
        IN("fa:fa-file-video Input File")
        CHK{"fa:fa-magnifying-glass Check Video Codec"}
        OUT("fa:fa-circle-check Replace Original File")
        FFMPEG_STACK
  end
    IN --> CHK
    CHK -- Codec OK (Direct) --> OUT
    CHK -- Codec Incorrect --> CMD_BEG
    CMD_BEG --> CMD_ENC
    CMD_ENC --> CMD_CONT
    CMD_CONT --> CMD_EXEC
    CMD_EXEC --> OUT
    style CMD_BEG fill:stroke:#ce93d8,color:#263238
    style CMD_ENC fill:stroke:#ce93d8,color:#263238
    style CMD_CONT fill:stroke:#ce93d8,color:#263238
    style CMD_EXEC fill:stroke:#ce93d8,color:#263238
    style IN fill:#e3f2fd,stroke:#64b5f6,stroke-width:2px,color:#263238
    style CHK fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#263238
    style OUT fill:#e3f2fd,stroke:#64b5f6,stroke-width:2px,color:#263238
    style FFMPEG_STACK fill:#f3e5f5,stroke:#ce93d8,stroke-width:2px,color:#263238,stroke-dasharray: 5 5
    style TDARR_FLOW fill:transparent,stroke:none,stroke-width:1px,color:#263238
```