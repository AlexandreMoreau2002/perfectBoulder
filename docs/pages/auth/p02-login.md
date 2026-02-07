# P02 - Login

## Objectif
Permettre une connexion rapide, compréhensible, et robuste.

## Type De Page
- Auth publique
- Formulaire

## Entrées / Sorties Navigation
- Entrées: `docs/pages/auth/p01-splash-auth-gate.md`, logout depuis settings
- Sorties:
  - `docs/pages/home/p04-home-video-feed.md` après succès
  - `docs/pages/auth/p03-signup.md` via CTA "Créer un compte"

## Structure UI (MVP)
1. Header: titre + sous-titre
2. Champ `Email`
3. Champ `Mot de passe` + toggle afficher/masquer
4. Bouton primaire `Se connecter`
5. Lien secondaire `Créer un compte`
6. Zone d'erreur globale (inline)

## Règles Fonctionnelles (MVP)
- Email obligatoire, format valide
- Password obligatoire
- Bouton disabled tant que invalide
- Submit appelle login API
- Stockage tokens sécurisé si succès

## Messages D'erreur MVP
- "Email ou mot de passe incorrect"
- "Impossible de se connecter. Vérifiez votre connexion."
- "Trop de tentatives. Réessayez dans quelques minutes."

## États A Maquetter
- Form idle
- Form loading (bouton disabled + spinner)
- Erreur inline
- Retour succès (transition home)

## Données / API
- API: `POST /auth/login`
- Payload: `email`, `password`
- Réponse attendue: `accessToken`, `refreshToken`, `user`

## Tracking Minimal
- `login_viewed`
- `login_submitted`
- `login_success`
- `login_failed`

## Évolutions (v1+)
- Mot de passe oublié
- SSO Apple / Google
- Magic link
- 2FA

## Checklist Validation Maquette
- [ ] Priorité visuelle claire sur l'action de connexion
- [ ] Erreurs lisibles sans jargon
- [ ] Une main: champs + bouton facilement atteignables mobile
- [ ] Accessibilité: labels, contraste, focus
