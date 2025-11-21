# 📘 Documentation Serveur Hôte

Bienvenue dans la documentation technique du serveur domestique. Ce dépôt contient les détails de configuration, les procédures de maintenance et l'architecture des services.

## 📌 Vue d'ensemble

* **Rôle :** Hôte Docker pour Média, IA, Domotique et Monitoring.
* **OS :** Debian 13.1
* **Technologie principale :** Docker & Docker Compose
* **Stockage :** Pool MergerFS + Parité SnapRAID

## 📂 Structure de la documentation

1.  [**Infrastructure Physique & Système**](./01_infrastructure.md)
    * Détails Hardware, Réseau (IPs, MAC), OS Hôte.
2.  [**Applications & Services**](./02_applications.md)
    * Architecture Docker, Flux de travail (Média, IA), Ports.
3.  [**Maintenance & DRP**](./03_maintenance_drp.md)
    * Mises à jour, Sauvegardes, Restauration.

## 📊 Diagrammes

* [**Flux de Travail**](./workflow.md) - Vue d'ensemble de l'automatisation du serveur
* [**Flux de Données & Stratégie de Stockage**](./dataflow.md) - Cycle de vie de la donnée et architecture de stockage
* [**Flux de Réseau**](./networkflow.md) - Segmentation réseau et zones de sécurité
* [**Flux de Travail Tdarr**](./tdarrflow.md) - Pipeline de traitement vidéo automatisé
* [**Séquence de Requête Média**](./media_request_sequence.md) - Interactions chronologiques lors d'une demande de contenu
* [**Cycle de Vie du Média**](./media_lifecycle_state.md) - États d'un fichier vidéo du téléchargement à l'archivage
* [**Parcours Utilisateur**](./user_journey.md) - Expérience utilisateur et interactions
* [**Carte Mentale de l'Écosystème**](./server_ecosystem_mindmap.md) - Vue hiérarchique de tous les composants

---
*Dernière mise à jour : 20/11/2025*
