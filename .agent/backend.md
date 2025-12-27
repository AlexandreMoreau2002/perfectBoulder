# Contrat agent (backend)

## Stack & contraintes
- API backend (HTTP) avec séparation routes/handlers.
- Base de données relationnelle ou équivalent + couche d’accès aux données (ORM/Query builder ou équivalent).
- Uploads : filtrage, limites, sanitation des noms, stockage persistant.
- Emails/notifications : transport configurable par environnement + file asynchrone (dispatcher/worker).
- Conteneurisation : composition multi-services possible, avec nom d’instance paramétrable si besoin.

## Architecture (à respecter)
- Séparer `routes` → `controllers` → `services` → `models`/`utils`.
- Pas de logique métier dans les routes : elles orchestrent et délèguent.
- Middlewares dédiés : auth JWT, rôle, upload, gestion d’erreur globale (dernier middleware).
- Chargement env : un point d’entrée unique (ex. `config/loadEnv`) et idempotent.

## API / conventions
- Statuts HTTP explicites (400/401/403/404/500).
- Réponses JSON structurées et stables : `error` (bool), `message` (string), `data` (optionnel).
- Routes d’écriture protégées par JWT + contrôle de rôle (admin).

## Sécurité (pratiques du projet)
- Auth token : lire `Authorization: Bearer <token>` ; refuser proprement si absent/invalide.
- Uploads :
  - Sanitize du nom de fichier (unicode → ascii, caractères sûrs, suffixe conservé).
  - Limite de taille (ex. 5MB) + allowlist mime (`image/jpeg`, `image/png`, `image/jpg`).
- Ne pas exposer de fichiers sensibles via statique ; servir uniquement `/uploads`.

## Base de données
- Démarrage : vérifier la connexion DB au boot (échouer explicitement si KO).
- Config : supporter `DATABASE_URL` sinon variables “discrètes” (`DB_HOST`, `DB_USER`, etc.).
- Migrations : exécutées au démarrage conteneur (entrypoint) et/ou via commande dédiée.

## Docker / exploitation
- Privilégier des commandes “safe” via un `Makefile`/scripts (start/stop/rebuild/log/shell).
- Éviter les commandes destructives en dev/prod (suppression volumes/données) sauf demande explicite.
- Uploads persistés via volume monté ; DB persistée via volume dédié.

## Emails
- Transport auto :
  - si variables SMTP présentes → SMTP “prod”
  - sinon → fallback “dev”
- Envoi asynchrone : enqueue → worker retry (backoff) ; ne pas bloquer l’API.
- Templates : échapper le HTML sur les champs utilisateurs.
