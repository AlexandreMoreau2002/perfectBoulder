# P12 - Paramètres

## Objectif
Centraliser la gestion du compte, de la sécurité et des préférences.

## Type De Page
- Page utilitaire
- Structurée en sections

## Entrées / Sorties Navigation
- Entrée: `docs/pages/profile/p11-profile-private.md`
- Sorties:
  - Retour profil
  - Logout -> `docs/pages/auth/p02-login.md`

## Structure UI (MVP)
1. Section `Compte`
  - Nom
  - Email (lecture seule MVP)
  - Photo de profil
2. Section `Session`
  - Se déconnecter
3. Section `Support`
  - Aide / contact (placeholder)
  - Version app

## Règles Fonctionnelles (MVP)
- Édition du nom et photo possible
- Email non modifiable au MVP
- Logout toujours accessible
- Confirmation logout recommandée

## États A Maquetter
- Vue normale
- Édition en cours
- Sauvegarde loading
- Erreur sauvegarde

## Données / API
- API: `GET /me`, `PATCH /me`, `POST /auth/logout` (ou purge locale)

## Tracking Minimal
- `settings_viewed`
- `settings_update_profile`
- `settings_logout`

## Évolutions (v1+)
- Notifications push (granulaire)
- Privacy controls (profil privé/public, visibilité stats)
- Langue
- Theme dark/light
- Export données
- Suppression compte
- Changement mot de passe

## Checklist Validation Maquette
- [ ] Les réglages critiques sont en haut
- [ ] Le logout est évident mais non accidentel
- [ ] Les futures sections peuvent s'ajouter sans casser la structure
