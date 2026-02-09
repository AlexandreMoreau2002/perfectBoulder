# Data Model - Perfect Boulder MVP

**Version** : 1.0
**Date** : 2026-02-09

---

## Schema ERD (Mermaid)

```mermaid
erDiagram
    users ||--o{ videos : posts
    users ||--o{ sessions : creates
    users ||--o{ croix : logs
    users ||--o{ exercises : does
    users ||--o{ likes : gives

    videos ||--o{ likes : receives
    videos }o--|| sessions : "links to (optional)"
    videos }o--|| croix : "links to (optional)"
    videos }o--|| exercises : "links to (optional)"

    sessions }o--|| gyms : "at (optional)"
    sessions ||--o{ croix : contains
    sessions ||--o{ exercises : contains

    users {
        uuid id PK
        string email UK
        string password
        string pseudo UK
        string avatar_url
        text bio
        timestamp created_at
    }

    videos {
        uuid id PK
        uuid user_id FK
        string video_url
        text caption
        string color
        string grade
        enum status
        uuid session_id FK_NULL
        uuid croix_id FK_NULL
        uuid exercise_id FK_NULL
        timestamp created_at
    }

    likes {
        uuid id PK
        uuid video_id FK
        uuid user_id FK
        timestamp created_at
    }

    sessions {
        uuid id PK
        uuid user_id FK
        uuid gym_id FK_NULL
        date date
        int feeling
        text notes
        timestamp created_at
    }

    croix {
        uuid id PK
        uuid user_id FK
        uuid session_id FK_NULL
        string photo_url
        string color
        string grade
        enum status
        text notes
        timestamp created_at
    }

    gyms {
        uuid id PK
        string name
        text location
        timestamp created_at
    }

    exercises {
        uuid id PK
        uuid user_id FK
        uuid session_id FK_NULL
        string type
        int reps
        timestamp created_at
    }
```

## Schema ERD (ASCII)

```
┌─────────────┐
│    users    │
├─────────────┤
│ id          │──┐
│ email       │  │
│ password    │  │
│ pseudo      │  │
│ avatar_url  │  │
│ bio         │  │
│ created_at  │  │
└─────────────┘  │
                 │
       ┌─────────┴─────────┬──────────────┬─────────────┐
       │                   │              │             │
       ▼                   ▼              ▼             ▼
┌─────────────┐     ┌─────────────┐  ┌──────────┐  ┌──────────┐
│   videos    │     │  sessions   │  │  croix   │  │exercises │
├─────────────┤     ├─────────────┤  ├──────────┤  ├──────────┤
│ id          │     │ id          │  │ id       │  │ id       │
│ user_id     │──┐  │ user_id     │  │ user_id  │  │ user_id  │
│ video_url   │  │  │ gym_id      │  │session_id│  │session_id│
│ caption     │  │  │ date        │  │photo_url │  │type      │
│ color       │  │  │ feeling     │  │color     │  │reps      │
│ grade       │  │  │ notes       │  │grade     │  │...       │
│ status      │  │  │ created_at  │  │status    │  └──────────┘
│ session_id? │──┼──┘              │  │notes     │
│ croix_id?   │──┼─────────────────┼──┘          │
│ exercise_id?│──┼─────────────────┼─────────────┘
│ created_at  │  │                 │
└─────────────┘  │                 │
                 │                 │
       ┌─────────┘                 │
       ▼                           │
┌─────────────┐                    │
│    likes    │                    │
├─────────────┤                    │
│ id          │                    │
│ video_id    │                    │
│ user_id     │                    │
│ created_at  │                    │
└─────────────┘                    │
                                   │
                          ┌────────┘
                          ▼
                    ┌──────────┐
                    │   gyms   │
                    ├──────────┤
                    │ id       │
                    │ name     │
                    │ location │
                    └──────────┘
```

---

## Tables

### users

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| email | VARCHAR(255) | UNIQUE, NOT NULL |
| password | VARCHAR(255) | NOT NULL (hashed) |
| pseudo | VARCHAR(50) | UNIQUE, NOT NULL |
| avatar_url | TEXT | NULL |
| bio | TEXT | NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |

---

### videos (feed social)

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| user_id | UUID | FK users(id), NOT NULL |
| video_url | TEXT | NOT NULL |
| caption | TEXT | NULL |
| color | VARCHAR(20) | NOT NULL |
| grade | VARCHAR(10) | NOT NULL |
| status | ENUM('flash','top','project') | NOT NULL |
| session_id | UUID | FK sessions(id), NULL |
| croix_id | UUID | FK croix(id), NULL |
| exercise_id | UUID | FK exercises(id), NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |

**Note :** session_id, croix_id, exercise_id sont optionnels. Une video peut exister seule (post social pur).

---

### likes

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| video_id | UUID | FK videos(id), NOT NULL |
| user_id | UUID | FK users(id), NOT NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| UNIQUE(video_id, user_id) | | Pas de double-like |

---

### sessions (logbook)

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| user_id | UUID | FK users(id), NOT NULL |
| gym_id | UUID | FK gyms(id), NULL |
| date | DATE | NOT NULL |
| feeling | INT | NULL (1-5) |
| notes | TEXT | NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |

---

### croix (logbook)

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| user_id | UUID | FK users(id), NOT NULL |
| session_id | UUID | FK sessions(id), NULL |
| photo_url | TEXT | NOT NULL |
| color | VARCHAR(20) | NOT NULL |
| grade | VARCHAR(10) | NOT NULL |
| status | ENUM('flash','top','project') | NOT NULL |
| notes | TEXT | NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |

---

### gyms

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| name | VARCHAR(255) | NOT NULL |
| location | TEXT | NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |

---

### exercises (futur, hors MVP)

| Champ | Type | Contrainte |
|-------|------|------------|
| id | UUID | PK |
| user_id | UUID | FK users(id), NOT NULL |
| session_id | UUID | FK sessions(id), NULL |
| type | VARCHAR(50) | NOT NULL |
| reps | INT | NULL |
| ... | | |

---

## Regles metier

1. **Une video peut pointer vers** session, croix, ou exercise (optionnel)
2. **Session/Croix/Exercise ne pointent PAS** vers video
3. **Un user ne peut liker qu'une seule fois** une video (UNIQUE constraint)
4. **Les croix sont privees** (pas de feed, juste dans le logbook)
5. **Les videos sont publiques** (visibles dans le feed)

---

## Index (performance)

```sql
CREATE INDEX idx_videos_user_id ON videos(user_id);
CREATE INDEX idx_videos_created_at ON videos(created_at DESC);
CREATE INDEX idx_likes_video_id ON likes(video_id);
CREATE INDEX idx_likes_user_id ON likes(user_id);
CREATE INDEX idx_sessions_user_id ON sessions(user_id);
CREATE INDEX idx_croix_user_id ON croix(user_id);
CREATE INDEX idx_croix_session_id ON croix(session_id);
```

---

## Migrations (ordre de creation)

1. users
2. gyms
3. sessions
4. croix
5. exercises
6. videos
7. likes
