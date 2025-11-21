# 🗺️ Parcours Utilisateur (User Journey)

Ce document illustre l'expérience d'un utilisateur type sur le serveur, mettant en avant les interactions émotionnelles et les étapes clés de l'utilisation des services.

## 🍿 Scénario : Soirée Cinéma

```mermaid
---
config:
  layout: elk
  theme: neutral
---
journey
    title Soirée Cinéma : De la recherche au visionnage
    section Recherche
      Ouvre Overseerr: 5: Utilisateur
      Cherche "Inception": 4: Utilisateur
      Ne trouve pas le film: 2: Utilisateur
    section Requête
      Clique sur "Demander": 5: Utilisateur
      Reçoit notif "Approuvé": 5: Utilisateur, Système
      Reçoit notif "Disponible": 5: Utilisateur, Système
    section Visionnage
      Ouvre Plex: 5: Utilisateur
      Lance le film: 5: Utilisateur
      Qualité 4K HDR (Direct Play): 5: Utilisateur, Système
      Sous-titres synchros: 4: Utilisateur
```

### Analyse du Parcours

*   **Points de friction potentiels** : Si le film n'est pas trouvé immédiatement ou si le téléchargement est long (note de 2 lors de la recherche infructueuse).
*   **Points forts** : L'automatisation des notifications et la qualité du streaming (Direct Play) garantissent une satisfaction élevée (note de 5) une fois le contenu disponible.
