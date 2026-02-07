# P03 - Signup

## Objectif
Créer un compte en moins d'une minute avec le minimum d'effort.

## Type De Page
- Auth publique
- Formulaire

## Entrées / Sorties Navigation
- Entrée: `docs/pages/auth/p02-login.md`
- Sorties:
  - `docs/pages/home/p04-home-video-feed.md` après création réussie
  - `docs/pages/auth/p02-login.md` via lien "J'ai déjà un compte"

## Structure UI (MVP)
1. Header: titre + bénéfice
2. Champ `Email`
3. Champ `Mot de passe`
4. Champ `Confirmer mot de passe`
5. Bouton primaire `Créer mon compte`
6. Lien secondaire `Déjà inscrit ? Se connecter`
7. Zone d'erreurs inline

## Règles Fonctionnelles (MVP)
- Email valide obligatoire
- Mot de passe >= 8 caractères
- Confirmation identique
- Erreur claire si email déjà utilisé
- Connexion automatique après signup (ou login immédiat)

## États A Maquetter
- Idle
- Validation en temps réel
- Loading submit
- Erreur métier (email existant)
- Succès + transition

## Données / API
- API: `POST /auth/signup`
- Payload: `email`, `password`, `confirmPassword`
- Réponse: user créé + tokens (ou confirmation + login)

## Tracking Minimal
- `signup_viewed`
- `signup_submitted`
- `signup_success`
- `signup_failed`

## Évolutions (v1+)
- Capture nom/prénom onboarding
- Consentements RGPD explicites
- Capture salle habituelle initiale
- Préférences onboarding (niveau, objectifs)

## Checklist Validation Maquette
- [ ] Le formulaire est plus simple que le login (pas de friction inutile)
- [ ] Les règles de password sont visibles
- [ ] Les messages d'erreur guident l'action
- [ ] Transition post-signup claire
