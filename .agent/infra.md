# Contrat agent (infra / déploiement)

## Principes CI/CD (issus du projet)
- Pipelines séparés `develop` vs `main` (dev/prod).
- Déploiement par **artefact** (archive) plutôt que build sur serveur.
- Avant déploiement : tests obligatoires (au minimum unit tests + coverage quand dispo).
- Sur serveur : backup avant remplacement (ex. build précédent, `.env`), puis déploiement atomique.

## Variables & secrets
- Toute config runtime passe par variables d’environnement (GitHub env vars + `.env` côté serveur/compose).
- Ne jamais transférer/committer `.env` depuis le repo : générer/injecter via CI.
- Conserver des fichiers `.env.example` / `.env.template` + documentation des variables.

## Nginx (reverse proxy)
- Reverse proxy (Nginx ou équivalent) ; DEV et PROD strictement séparés : domaines, ports, chemins distincts.
- `/api/` :
  - proxy vers upstream dédié, `rewrite` pour retirer le préfixe,
  - timeouts/proxy headers standardisés,
  - CORS explicite + gestion OPTIONS.
- `/uploads/` :
  - `alias` vers dossier uploads backend,
  - cache + blocage exécution scripts.
- Sécurité :
  - headers (XFO, nosniff, referrer policy, etc.),
  - rate limiting (API + login endpoints),
  - bot protection de base,
  - blocage dotfiles et fichiers de config.
- Validation : test de config en CI (sandbox) + sur serveur avant reload.
- Déploiement : script avec garde‑fous (refus mélange dev/prod) + backup + reload gracieux.
