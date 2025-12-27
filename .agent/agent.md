# Contrat agent (global)

Ce dossier `.agent/` est un **contrat d’exécution** : règles impératives, courtes, vérifiables.

## Non‑négociables
- Ne **suppose** jamais un besoin : si une exigence est ambiguë, pose 1–3 questions ciblées avant de coder.
- Ne crée pas de “features” spéculatives : fais uniquement ce qui est demandé + le strict nécessaire (tests/README) pour que ça tourne.
- Ne change pas de conventions existantes (structure, nommage, style) sans raison explicite et acceptée.
- N’ajoute **aucune dépendance** (npm, docker image, action GH) sans instruction explicite.
- Évite les refactors larges : changements **minimaux**, localisés, et justifiés.

## Qualité & lisibilité
- Un fichier = une responsabilité claire (SRP).
- Pas de code mort, pas d’imports inutilisés, pas de duplications évidentes (DRY).
- Préfère simple et explicite à “malin” (KISS).
- Nommage explicite : pas de variables/fonctions vagues.
- Imports “en escalier” : tout les imports doivent etre triés par longueur (forme d'escalier), groupes d’imports clairs, en haut de fichier, quand c'est possible.

## Sécurité & secrets
- Ne log jamais de secrets (tokens, mots de passe, clés).
- Ne commit jamais `.env` : fournir `.env.example` / `.env.template` + doc des variables.
- Entrées utilisateur : valider, échapper, et limiter (uploads, payloads, taille).

## Changements attendus
- Avant de modifier : lire le code adjacent + les docs locales (README/DEPLOYMENT/INFRA/etc.).
- Après modification : exécuter la vérification la plus proche (tests/lint/build) si disponible.
- Si tu touches une API/contrat : mets à jour la doc correspondante (docs + exemples).

## Definition of Done (Obligatoire)
Chaque interaction doit se conclure par :
1. **Tester** : Vérifier que le code fonctionne (test manuel ou automatisé).
2. **Mettre à jour la doc** : Si le comportement change, mettre à jour la documentation existante.
3. **Créer du contexte** : Si nécessaire, créer/mettre à jour une doc pour expliquer l'état actuel et aider la prochaine IA (ex: `CONTEXT.md` ou note dans `.agent/`).
