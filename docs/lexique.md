# Lexique Détaillé - Perfect Boulder

**Version**: 2026-02-07

## Objectif

Spécifier précisément ce que représente chaque terme UI/produit en termes de données et de structure technique.
Ce lexique est la **source de vérité** pour la modélisation en base de données et l'API.

---

## Pages et Leurs Contenus

### Feed / Accueil

**Terme UI** : « Feed »
**Contient** : Liste de `Post` publiés par les utilisateurs suivis + feed local + algorithme
**Filtrages** :
- **Suivis** : affiche les posts (visibility ≥ followers) des users suivis, triés par `created_at` DESC
- **Local** : affiche les posts dans un rayon géographique ou salle, triés par `created_at` DESC
- **Pour toi** : algorithme (future), affiche posts recommandés

**Structure BD** :
```
posts (table)
├── id (UUID PK)
├── author_id (UUID FK → users)
├── content (text)
├── visibility (enum: public, followers, private)
├── media_ids (UUID[] FK → media)
├── created_at
├── updated_at
```

**API Response** :
```json
{
  "posts": [
    {
      "id": "uuid",
      "author": { "id", "name", "avatar" },
      "content": "J'ai validé...",
      "visibility": "public",
      "media": [{ "id", "type", "url" }],
      "createdAt": "2026-02-07T10:00:00Z",
      "likeCount": 12,
      "commentCount": 3
    }
  ]
}
```

---

### Post / Contenu Social

**Terme UI** : « Post »
**Définition** : Contenu publié par un user (photo/vidéo + texte), indépendant d'une session
**C'est** : Un message social avec attachements médias
**Ce n'est PAS** : Une croix/ascent

**Propriétés** :
- `visibility` : public | followers | private
- `media` : 1+ images/vidéos (ou texte seul)
- `caption` : texte libre
- Reactions : likes + commentaires
- Attributable à un `author_id` (user)

**Structure BD** :
```
posts (table)
├── id (UUID PK)
├── author_id (UUID FK → users)
├── caption (text)
├── visibility (enum)
├── created_at
└── updated_at

media (table)
├── id (UUID PK)
├── post_id (UUID FK → posts)
├── type (enum: photo, video)
├── url (text)
└── order (int)
```

---

### Session / Séance de Grimpe

**Terme UI** : « Session »
**Définition** : Séance personnelle de grimpe = date + salle/spot + feeling
**Contient** : 0+ croix (ascents), photos/vidéos, notes personnelles
**Visibilité** : Privée par défaut, peut être partagée partiellement

**Propriétés** :
- `gym_id` ou `crag_id` (où la session a eu lieu)
- `date` : quand
- `feeling` : évaluation personnelle (0-5 ou qualitative)
- `notes` : texte libre
- `media` : photos/vidéos perso de la session
- `ascents` : liste des croix validées lors de cette session

**Structure BD** :
```
sessions (table)
├── id (UUID PK)
├── user_id (UUID FK → users)
├── gym_id ou crag_id (optional FK)
├── session_date (date)
├── feeling (enum ou int: 1-5)
├── notes (text)
├── created_at
└── updated_at

session_media (table)
├── id (UUID PK)
├── session_id (UUID FK → sessions)
├── type (enum: photo, video)
├── url (text)
└── order (int)
```

---

### Croix / Ascent (Logbook)

**Terme UI** : « Croix »
**Nom technique** : `Ascent`
**Définition** : Validation d'un bloc par l'utilisateur (enregistrement dans son logbook)
**Porte** : status (flash, top, project), date, notes perso
**Lié à** : une session (optionnel), un bloc (BoulderProblem futur)

**Propriétés** :
- `status` : flash | top | project
- `date` : quand la croix a été validée
- `session_id` (optionnel) : si validée dans une session
- `notes` : commentaire personnel
- `boulder_problem_id` (futur) : ref au bloc

**Structure BD** :
```
ascents (table)
├── id (UUID PK)
├── user_id (UUID FK → users)
├── boulder_problem_id (UUID FK → boulder_problems, futur)
├── session_id (UUID FK → sessions, optional)
├── status (enum: flash, top, project)
├── ascent_date (date)
├── notes (text)
├── created_at
└── updated_at
```

---

### Bloc / Boulder Problem

**Terme UI** : « Bloc »
**Nom technique** : `BoulderProblem` (futur)
**Définition** : Problème de grimpe spécifique (nom, localisation, difficulté)
**Référencé par** : croix, sessions, projets

**Propriétés** :
- `name` : nom du bloc
- `grade` : difficulté (Fontainebleau: 3+, 4, 4+, 5, 5+, 6a, 6a+, 6b, 6b+, 6c, 6c+, 7a, ...)
- `gym_id` ou `crag_id` : où le bloc se trouve
- `wall_section` (optional) : zone/mur spécifique
- `color` (optional) : couleur du bloc (si marqué)
- `description` : détails techniques

**Structure BD** (future) :
```
boulder_problems (table)
├── id (UUID PK)
├── name (text)
├── grade (enum ou text)
├── gym_id ou crag_id (UUID FK)
├── wall_section (text, optional)
├── color (text, optional)
├── description (text)
├── created_at
└── updated_at
```

---

### Salle / Gym

**Terme UI** : « Salle »
**Nom technique** : `Gym`
**Définition** : Lieu de grimpe indoor où se trouvent des blocs
**Contient** : murs, blocs, sections

**Propriétés** :
- `name` : nom de la salle
- `location` : adresse / coords GPS
- `description` : infos pratiques

**Structure BD** :
```
gyms (table)
├── id (UUID PK)
├── name (text)
├── location (text)
├── latitude (float)
├── longitude (float)
└── description (text)
```

---

### Spot / Crag

**Terme UI** : « Spot »
**Nom technique** : `Crag` (future)
**Définition** : Lieu de grimpe outdoor où se trouvent des blocs
**Contient** : zones, blocs naturels

**Propriétés** :
- `name` : nom du spot
- `location` : région / coords GPS
- `description` : accès, conditions

---

### Projet

**Terme UI** : « Projet »
**Définition** : Bloc non encore validé, suivi par l'utilisateur
**Différence avec Croix** : status = `project` dans ascents, pas flashé/topé

**Structure BD** : Même table `ascents`, mais `status = 'project'`

---

### Profil Privé

**Terme UI** : « Profil privé » ou « Mon profil »
**Définition** : Profil personnel modifiable, visible seulement à l'utilisateur
**Contient** : données personnelles éditables (bio, avatar, etc.)

**Propriétés** :
- `name` : nom complet
- `email` : email (privé)
- `avatar` : photo de profil
- `bio` : courte description
- `location` : localisation (optionnel)

**Structure BD** :
```
users (table)
├── id (UUID PK)
├── name (text)
├── email (text, unique)
├── avatar_url (text, optional)
├── bio (text, optional)
├── location (text, optional)
└── ...
```

---

### Profil Public

**Terme UI** : « Profil public » (quand on visite un autre user)
**Définition** : Vue publique d'un profil utilisateur
**Affiche** :
- Name, avatar, bio, location
- Statistiques : nombre de croix, niveau moyen, ascensions récentes
- Posts publics de cet user
- Bouton « Suivre »

**Structure BD** :
```
SELECT u.id, u.name, u.avatar_url, u.bio, u.location,
       COUNT(a.id) as croix_count,
       AVG(a.grade_numeric) as avg_grade
FROM users u
LEFT JOIN ascents a ON u.id = a.user_id
WHERE u.id = ?
GROUP BY u.id
```

---

### Like

**Terme UI** : « Like »
**Définition** : Réaction positive sur un post
**Cible** : un post spécifique

**Structure BD** :
```
post_likes (table)
├── id (UUID PK)
├── post_id (UUID FK → posts)
├── user_id (UUID FK → users)
├── created_at
└── UNIQUE(post_id, user_id)
```

---

### Commentaire

**Terme UI** : « Commentaire »
**Définition** : Message texte lié à un post
**Propriétés** :
- `author_id` : qui a commenté
- `post_id` : sur quel post
- `content` : texte du commentaire
- `created_at` : quand

**Structure BD** :
```
post_comments (table)
├── id (UUID PK)
├── post_id (UUID FK → posts)
├── author_id (UUID FK → users)
├── content (text)
├── created_at
└── updated_at
```

---

### Suivre / Follow

**Terme UI** : « Suivre »
**Définition** : Relation sociale : user A suit user B
**Effet** : Posts publics/followers de B apparaissent dans feed de A

**Structure BD** :
```
follow_relations (table)
├── id (UUID PK)
├── follower_id (UUID FK → users)
├── following_id (UUID FK → users)
├── created_at
└── UNIQUE(follower_id, following_id)
```

---

### Feeds Spécifiques

#### Suivis
**Affiche** : Posts (visibility ≥ followers) des users suivis, triés par date DESC

**Query** :
```sql
SELECT p.* FROM posts p
JOIN follow_relations fr ON p.author_id = fr.following_id
WHERE fr.follower_id = ?
  AND p.visibility IN ('public', 'followers')
ORDER BY p.created_at DESC
```

#### Local
**Affiche** : Posts dans un périmètre géographique (salle ou coords) ou posts liés à sessions locales

**Query** :
```sql
SELECT p.* FROM posts p
WHERE p.author_id IN (
  SELECT DISTINCT s.user_id FROM sessions s
  WHERE s.gym_id = ? OR st_distance(s.coordinates, ?) < 5000
)
  AND p.visibility = 'public'
ORDER BY p.created_at DESC
```

#### Pour toi
**Affiche** : Feed algorithmique (future) basé sur préférences/historique

---

## Nommage Convention

### Base de données

- **Tables** : `snake_case` pluriel (ex. `users`, `posts`, `ascents`, `sessions`, `gyms`)
- **Colonnes** : `snake_case` (ex. `user_id`, `created_at`, `session_date`)
- **PK** : `id` (UUID)
- **FK** : `{entity}_id` (ex. `user_id`, `post_id`)
- **Booléens** : `is_*` (ex. `is_active`)
- **Timestamps** : `created_at`, `updated_at`, `{action}_at` (ex. `deleted_at`, `ascent_date`)

### API / JSON

- **Keys** : `camelCase` (ex. `userId`, `createdAt`, `sessionDate`)
- **Énums** : `lowercase` ou `UPPERCASE` (ex. `"public"`, `"flash"`)

### Types TypeScript

- **Entités** : `PascalCase` singulier (ex. `Post`, `Session`, `Ascent`, `User`)
- **Énums** : `PascalCase` (ex. `PostVisibility`, `AscentStatus`)
- **Interfaces DTO** : `{Entity}DTO` ou `Create{Entity}Request` (ex. `PostDTO`, `CreatePostRequest`)

---

## États et Énumérations

### Post.visibility
- `public` : visible à tous
- `followers` : visible aux followers de l'auteur
- `private` : visible uniquement à l'auteur

### Ascent.status
- `flash` : validé sans chute (première tentative après observation)
- `top` : validé avec chutes (nombre de tentatives)
- `project` : non encore validé, suivi

### Media.type
- `photo` : image statique
- `video` : vidéo (MP4, etc.)

### Session.feeling (optionnel)
- `1` : très mauvais
- `2` : mauvais
- `3` : moyen
- `4` : bon
- `5` : très bon

---

## Règles de Cohérence

1. **Un post ≠ une croix** : un post est social (peut être texte seul), une croix est un enregistrement de validation
2. **Une session contient des croix** : les croix d'une session sont liées à `session_id`
3. **Visibility** : s'applique uniquement aux `posts`, pas aux sessions/croix (privées par défaut)
4. **Unicité des follows** : impossible de suivre 2 fois le même user (UNIQUE constraint)
5. **Unicité des likes** : impossible de liker 2 fois le même post (UNIQUE constraint)

---

## Source de Vérité

**Ce lexique est la source de vérité pour** :
- ✅ Noms de tables/colonnes en BD
- ✅ Noms de properties en API/JSON
- ✅ Noms de types TypeScript
- ✅ Contenu affiché en UI (français clair)

**En cas d'écart** : corriger ce lexique EN PREMIER, puis les docs/code.
