# 🛡️ 03 - Maintenance, Sécurité & DRP

## 🔒 Sécurité & Mises à jour

### Mises à jour Automatiques (Watchtower)
* **Outil :** Watchtower
* **Fréquence :** Toutes les 6 heures (`21600` secondes).
* **Exclusions :** Watchtower, Tdarr (Server), et conteneurs avec label `com.centurylinklabs.watchtower.enable=false`.

### Pare-feu & Accès
* **Niveau 1 (Edge) :** Routeur/Firewall gère l'entrée initiale.
* **Niveau 2 (Proxy) :** Nginx filtre et dispatch vers le `dmz_net`.
* **Segmentation :** Les services sensibles (Bases de données, *Arrs) restent isolés dans `internal_net` sans exposition directe.

---

## 💾 Stratégie de Sauvegarde

*Note : SnapRAID n'est PAS une sauvegarde, c'est une protection contre la panne d'un disque.*

### Politique de Sauvegarde (Règle du 3-2-1 recommandée)

| Type de Donnée | Méthode | Fréquence | Rétention | Destination |
| :--- | :--- | :--- | :--- | :--- |
| **Configurations Docker** | Sauvegarde des volumes (ex: `plex_config`) | Quotidienne (Nuit) | 30 Jours | [À COMPLÉTER - ex: NAS Externe / Cloud] |
| **Bases de Données** | Dump SQL (Nextcloud, *Arrs) | Quotidienne | 30 Jours | [À COMPLÉTER] |
| **Données Critiques** | Documents Nextcloud / Photos | Hebdomadaire | 6 Mois | [À COMPLÉTER - ex: Cloud Chiffré] |
| **Médias (Films/Séries)** | SnapRAID (Parité) | Sync scriptée | N/A | Disques Locaux (Pool) |

### Outil recommandé
* Utilisation de **[À COMPLÉTER - ex: Kopia / BorgBackup / Restic]** pour chiffrer et envoyer les sauvegardes vers un stockage distant.

---

## 🚨 Plan de Reprise après Sinistre (DRP)

### En cas de crash complet de l'OS (Disque M.2 HS)
1.  **Matériel :** Remplacer le SSD M.2.
2.  **OS :** Réinstaller l'OS Hôte (voir fichier `01_infrastructure.md`).
3.  **Réseau :** Restaurer l'IP statique.
4.  **Disques :** Réinstaller MergerFS et remonter le pool de disques (les données sur les HDD sont intactes).
5.  **Docker :**
    * Réinstaller Docker & Docker Compose.
    * Récupérer les fichiers `docker-compose.yml` (depuis Git).
    * **Restauration :** Restaurer les volumes persistants (configs) depuis la dernière sauvegarde.
    * Lancer `docker-compose up -d`.
