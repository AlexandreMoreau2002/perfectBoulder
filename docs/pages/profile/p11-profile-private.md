# P11 - Profil (Privé)

## Objectif
Donner à l'utilisateur sa vue personnelle: identité, stats de base, accès paramètres, et gestion de ses contenus.

## Type De Page
- Main tab
- Vue personnelle (non publique)

## Entrées / Sorties Navigation
- Entrée: onglet `Profil`
- Sorties:
  - `docs/pages/profile/p12-settings.md`
  - Édition profil inline/modal
  - Logout -> `docs/pages/auth/p02-login.md`

## Structure UI (MVP)
1. Header profil: avatar, nom, email
2. Bloc stats rapides:
  - nombre total de sessions
  - nombre total de croix
  - nombre total de posts
  - dernière activité
3. Section `Mes contenus`:
  - onglet `Mes blocs`
  - onglet `Mes posts`
  - actions `ouvrir`, `modifier`, `supprimer`
4. Actions rapides:
  - Modifier nom
  - Modifier photo
  - Ouvrir paramètres
5. CTA secondaire `Se déconnecter`

## Règles Fonctionnelles (MVP)
- Afficher les infos du user connecté uniquement
- Permettre update nom/photo
- Rafraîchir stats au retour sur écran
- Logout fiable (purge token + redirection login)

## États A Maquetter
- Profil chargé
- Chargement initial
- Erreur récupération profil
- Empty state stats (nouveau user)

## Données / API
- API: `GET /me`, `PATCH /me`
- Données affichées: `name`, `email`, `avatarUrl`, `sessionsCount`, `croixCount`

## Tracking Minimal
- `profile_viewed`
- `profile_edit_name`
- `profile_edit_photo`
- `profile_logout`

## Évolutions (v1+)
- Profil public partageable
- Badge niveau / achievements
- Grille médias personnelle
- Liens vers profil social/crew

## Checklist Validation Maquette
- [ ] Lecture ultra rapide de l'identité et des stats
- [ ] Actions principales visibles sans scroll profond
- [ ] Séparation claire entre data perso et navigation
