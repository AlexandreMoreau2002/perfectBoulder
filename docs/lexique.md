# Lexique Produit Et Technique - Perfect Boulder

**Version**: 2026-02-07

## Objectif
Harmoniser le vocabulaire produit, backend et frontend pour éviter les ambiguïtés de conception et d'implémentation.

## Règles D'Usage
1. En UI, utiliser des termes compréhensibles grimpeur.
2. En backend/base, utiliser des noms stables et explicites.
3. En frontend, utiliser des types TS alignés avec les DTO API (pas de double modèle métier).
4. Un terme produit = une seule signification.

---

## Glossaire Canonique

| Terme UI | Définition produit | Nom technique recommandé | À éviter |
|---|---|---|---|
| Feed | Flux de vidéos publiées par les autres utilisateurs | `Post` | "Home sessions" |
| Post | Contenu social publié (photo/vidéo + caption) | `Post` | Mélanger avec `Croix` |
| Session | Séance personnelle de grimpe (date, salle, ressenti) | `Session` | "Sortie" |
| Croix | Bloc validé par l'utilisateur dans son logbook | `Ascent` (tech) / `Croix` (UI) | "Post" |
| Bloc | Problème de grimpe tenté/réussi | `BoulderProblem` (future) | "Voie" (indoor bloc) |
| Salle | Lieu de grimpe indoor | `Gym` | "Spot" |
| Spot | Lieu outdoor | `Crag` / `Spot` (future) | "Salle" |
| Projet | Bloc non encore validé, suivi dans le temps | `Project` (future) | "Croix" |
| Mes contenus | Espace profil où je gère mes sessions/croix/posts | `ProfileContent` (UI state) | Gérer depuis Home |
| Profil privé | Mon profil perso (editable) | `UserProfilePrivate` | Confondre avec profil public |
| Profil public | Profil visible par les autres | `UserProfilePublic` | Mélanger avec settings |
| Like | Réaction positive sur un post | `PostLike` | "Vote" |
| Commentaire | Message lié à un post | `PostComment` | "Note" |
| Suivre | Relation social entre utilisateurs | `FollowRelation` | "Ami" |
| Suivis | Feed des comptes suivis | `FeedFollowed` | |
| Local | Feed géographique/salle locale | `FeedLocal` | |
| Pour toi | Feed algorithmique personnalisé | `FeedForYou` | |
| Ajouter contenu | Point d'entrée unique de création | `AddContentFlow` | "Ajouter session" seulement |

---

## Langue Et Nommage

## UI
- Français clair orienté usage grimpeur.
- Exemples: `Mes blocs`, `Ajouter contenu`, `Salle`, `Ressenti`.

## Backend / DB
- Tables SQL: `snake_case` pluriel.
- Colonnes SQL: `snake_case`.
- Clés primaires: `id` (UUID).

## API / Frontend TS
- JSON API: `camelCase`.
- Types TS: noms d'entités singulier PascalCase (`Post`, `Session`, `Ascent`).
- Éviter les transformations multiples inutiles côté frontend.

---

## Dictionnaire Des États Métier (v0)

- `post.visibility`: `public` | `followers` | `private`
- `ascent.status`: `flash` | `top` | `project`
- `media.type`: `photo` | `video`
- `session.state`: `active` | `closed` | `archived`

---

## Règle De Décision En Cas De Conflit

1. Ce lexique fait foi pour les termes.
2. Le modèle de données (`docs/architecture/data-model.md`) fait foi pour les entités.
3. En cas d'écart: corriger d'abord le lexique, puis les docs/pages/features.
