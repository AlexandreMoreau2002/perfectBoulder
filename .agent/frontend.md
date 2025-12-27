# Contrat agent (frontend)

## Stack & contraintes
- Application web (SPA ou MPA) avec un routeur côté client si applicable.
- Styles : CSS/SCSS (ou équivalent) avec design system léger (variables + mixins/utilitaires).
- HTTP : un client HTTP centralisé + interceptors/middlewares (ou équivalent).
- Tests : un setup global unique (polyfills/mocks) et une stratégie de coverage stable.
- Imports “en escalier” (ordonnés, en haut de fichier).

## Architecture (à respecter)
- Routing centralisé (un point de vérité) + layout global + gardes d’accès si besoin.
- Accès réseau **uniquement** via un client HTTP unique (pas d’appels HTTP dispersés).
- Accès data via une couche “repositories/adapters” (une ressource = un module).
- État “loading” global piloté par le client HTTP (début/fin) + UI (Loader) via un store/context global.
- Mocks de dev pilotés par variable d’environnement (ex. `APP_ENV=dev`), sans impacter la prod.
- Workflow styles en dev : watcher/compilation styles + serveur dev lancés ensemble.

## Règles UI/UX (issues du projet)
- Réutiliser en priorité variables/mixins SCSS existants (pas de valeurs magiques).
- Formulaires : en mode édition, les champs fichier/image doivent être **optionnels** (pas de re-upload forcé).
- Feedback cohérent : succès/erreur affichés de manière uniforme (mêmes composants/toasts/patterns).
- Cohérence : tailles d’inputs, placement des boutons, et libellés alignés entre pages.

## HTTP / erreurs
- La `baseURL` vient d’une variable d’environnement (pas de hardcode).
- Normaliser les erreurs à un format consommable par l’UI (message utilisateur, cas réseau).
- Ne pas dupliquer la logique de headers/auth dans 20 fichiers : factoriser (client/repo).

## Tests (front)
- Centraliser mocks/polyfills et filtres de warnings dans un setup global (et réutiliser).
- Isoler les tests : pas de dépendance au réseau ; mock du client HTTP ; mock composants externes bruyants.
- Coverage : garder une stratégie stable (exclusions justifiées : bootstrap, exports, setup).
