# Data Model - V0 (Alignement Backend / Frontend)

**Version**: 2026-02-07
**But**: définir une base d'entités stable pour la DB et les types TS frontend.

## 1. Principes D'Architecture Données

1. **Source de vérité**: backend + DB.
2. **Frontend sans modèle métier dupliqué**: types TS alignés sur les DTO API.
3. **Conventions stables**:
   - DB: `snake_case`
   - API JSON: `camelCase`
   - TS: interfaces/types en `PascalCase`
4. **Entités transverses communes**: `id`, `created_at`, `updated_at`.

---

## 2. Contextes Métier

1. **Auth**: utilisateur + session de connexion
2. **Social**: posts, feed, interactions
3. **Logbook**: sessions de grimpe + croix
4. **Media**: assets photo/vidéo
5. **Profile**: vue et gestion de ses contenus

---

## 3. Entités Canoniques (V0)

## 3.1 User
- **Table DB**: `users`
- **Type TS**: `User`
- **Rôle**: identité et propriétaire des contenus
- **Champs V0**:
  - `id` UUID PK
  - `email` unique
  - `display_name`
  - `avatar_url` nullable
  - `bio` nullable
  - `home_gym_id` nullable FK -> gyms.id
  - `created_at`, `updated_at`

## 3.2 Auth Session / Refresh Token
- **Table DB**: `auth_sessions`
- **Type TS**: `AuthSession` (rarement exposé côté app)
- **Rôle**: gestion sessions sécurisées
- **Champs V0**:
  - `id`, `user_id`
  - `refresh_token_hash`
  - `expires_at`, `revoked_at`
  - `created_at`, `updated_at`

## 3.3 Gym
- **Table DB**: `gyms`
- **Type TS**: `Gym`
- **Rôle**: localisation indoor
- **Champs V0**:
  - `id`
  - `name`
  - `city`
  - `country_code`
  - `lat`, `lng` nullable
  - `created_at`, `updated_at`

## 3.4 Session (logbook)
- **Table DB**: `sessions`
- **Type TS**: `Session`
- **Rôle**: séance personnelle regroupant des croix
- **Champs V0**:
  - `id`, `user_id`, `gym_id` nullable
  - `session_date`
  - `feeling` nullable
  - `notes` nullable
  - `state` (`active`|`closed`|`archived`)
  - `created_at`, `updated_at`

## 3.5 Ascent (UI: Croix)
- **Table DB**: `ascents`
- **Type TS**: `Ascent`
- **Rôle**: bloc validé ou en projet dans une session
- **Champs V0**:
  - `id`, `user_id`, `session_id`
  - `gym_id` nullable
  - `color` nullable
  - `grade` nullable
  - `status` (`flash`|`top`|`project`)
  - `attempt_count` nullable
  - `feeling` nullable
  - `comment` nullable
  - `created_at`, `updated_at`

## 3.6 Post (social)
- **Table DB**: `posts`
- **Type TS**: `Post`
- **Rôle**: contenu publié dans le feed social
- **Champs V0**:
  - `id`, `author_id`
  - `caption` nullable
  - `visibility` (`public`|`followers`|`private`)
  - `gym_id` nullable
  - `primary_media_id` nullable
  - `created_at`, `updated_at`

## 3.7 Media Asset
- **Table DB**: `media_assets`
- **Type TS**: `MediaAsset`
- **Rôle**: photo/vidéo attachable à `Post` ou `Ascent`
- **Champs V0**:
  - `id`, `owner_id`
  - `type` (`photo`|`video`)
  - `storage_key`
  - `url`
  - `thumbnail_url` nullable
  - `duration_ms` nullable
  - `width`, `height` nullable
  - `size_bytes`
  - `created_at`, `updated_at`

## 3.8 Liens Media
- **Tables DB**:
  - `post_media` (post_id, media_id, sort_order)
  - `ascent_media` (ascent_id, media_id, sort_order)
- **Rôle**: éviter de figer le modèle à 1 média unique

## 3.9 Interactions Sociales (v1)
- **Tables DB**:
  - `post_likes` (post_id, user_id)
  - `post_comments` (id, post_id, author_id, body)
  - `follows` (follower_id, followed_id)

---

## 4. Relations Principales

1. `users` 1--N `sessions`
2. `sessions` 1--N `ascents`
3. `users` 1--N `posts`
4. `posts` N--N `media_assets` via `post_media`
5. `ascents` N--N `media_assets` via `ascent_media`
6. `gyms` 1--N `sessions` / `posts` / `ascents`

---

## 5. Contrat Backend -> Frontend (TS)

## Règle
- Le frontend consomme des DTO API typés.
- Pas de couche "model" frontend supplémentaire hors besoin de présentation.

## Recommandation de fichiers TS
1. `src/types/entities.ts` -> entités partagées (`User`, `Post`, `Session`, `Ascent`, `MediaAsset`)
2. `src/types/api.ts` -> payloads API (`CreatePostInput`, `UpdateAscentInput`, etc.)
3. `src/types/view.ts` -> types strictement UI (cards/feed grouping)

## Mapping minimal
- DB `created_at` -> API `createdAt` -> TS `createdAt: string`
- DB `author_id` -> API `authorId` -> TS `authorId: string`

---

## 6. Décisions V0 Importantes

1. Séparer `Post` (social) et `Ascent` (logbook)
2. Garder `Session` comme agrégat perso
3. `Add Content` est un flow d'entrée unique, pas une entité
4. Les écrans d'édition/suppression de contenus personnels vivent dans la famille `Profile`

---

## 7. Questions Ouvertes (A Trancher)

1. Un `Post` peut-il être lié directement à une `Ascent` ? (recommandé: oui, via `ascent_id` nullable dans `posts`)
2. Est-ce qu'une `Ascent` sans `Session` est autorisée ? (recommandé: non)
3. Niveaux/cotations: enum interne ou table de référence ?
4. Stratégie soft delete pour posts/ascents/media ?
