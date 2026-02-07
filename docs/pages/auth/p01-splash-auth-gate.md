# P01 - Splash / Auth Gate

## Objectif
Déterminer en quelques secondes si l'utilisateur doit aller vers `Login` ou vers `Home`, sans friction.

## Type De Page
- Système / routage
- Non scrollable
- Non accessible depuis le menu principal

## Entrées / Sorties Navigation
- Entrée: lancement app, retour foreground si session expirée
- Sorties:
  - `docs/pages/home/p04-home-video-feed.md` si session valide
  - `docs/pages/auth/p02-login.md` si non connecté

## Structure UI (MVP)
1. Logo / nom de l'app centré
2. Loader principal
3. Message discret d'état (ex: "Connexion...")
4. CTA secondaire `Réessayer` si erreur réseau prolongée

## Règles Fonctionnelles (MVP)
1. Lire token local sécurisé
2. Si absent: redirection login immédiate
3. Si présent: tentative refresh session
4. Si refresh OK: redirection home
5. Si refresh KO: purge tokens + redirection login

## États A Maquetter
- Chargement normal (1-2s)
- Chargement long (>3s) avec message
- Erreur réseau (retry)
- Session invalide (transition douce vers login)

## Données / API
- Storage local: `accessToken`, `refreshToken`, `userSnapshot`
- API: `POST /auth/refresh` (ou équivalent)

## Tracking Minimal
- `auth_gate_opened`
- `auth_gate_refresh_success`
- `auth_gate_refresh_failed`
- `auth_gate_redirect_login`

## Évolutions (v1+)
- Deep links (ouvrir directement une session/croix)
- Maintenance mode global
- Forced update check
- Remote config bootstrap

## Checklist Validation Maquette
- [ ] L'état "chargement" est clair
- [ ] La transition vers login/home est instantanée
- [ ] Le cas offline ne bloque pas l'utilisateur indéfiniment
- [ ] Le rendu reste propre sur petit écran
