# Perfect Boulder - Index Des Pages (Structure Par Famille)

**Version**: 2026-02-07  
**Sources**: `docs/features/mvp-scope.md`, `docs/brainstorming-session-results.md`, `docs/flows/video.md`

---

## 1. Principe De Structuration

On structure les écrans par **famille de page principale** (pas juste par numéro):

1. `Auth`: accès et session utilisateur
2. `Home`: découverte sociale (feed vidéo des autres)
3. `Add Content`: création de contenu (session, croix, post)
4. `Profile`: identité + gestion de ses propres contenus
5. `Stats`: progression

Règle produit clé:
- **Home = contenu des autres (feed social)**
- **Profile = mes contenus + édition/suppression**

---

## 2. Arborescence Cible

```text
docs/pages/
├── index.md
├── auth/
│   ├── auth.md
│   ├── p01-splash-auth-gate.md
│   ├── p02-login.md
│   └── p03-signup.md
├── home/
│   ├── home.md
│   └── p04-home-video-feed.md
├── add-content/
│   ├── add-content.md
│   └── p05-add-content.md
├── profile/
│   ├── profile.md
│   ├── p11-profile-private.md
│   ├── p12-settings.md
│   └── content-management.md
└── stats/
    └── stats.md
```

---

## 3. Familles, Entités, Pages Principales

## `Auth`
- **Page principale**: `docs/pages/auth/p01-splash-auth-gate.md`
- **Entités dominantes**: `User`, `SessionToken`
- **Pages**:
  1. `docs/pages/auth/p01-splash-auth-gate.md`
  2. `docs/pages/auth/p02-login.md`
  3. `docs/pages/auth/p03-signup.md`

## `Home`
- **Page principale**: `docs/pages/home/p04-home-video-feed.md`
- **Entités dominantes**: `Post`, `Media`, `User`, `Gym`
- **Pages**:
  1. `docs/pages/home/p04-home-video-feed.md`
  2. `docs/pages/home/p13-home-social-feed.md` (future)

## `Add Content`
- **Page principale**: `docs/pages/add-content/p05-add-content.md`
- **Entités dominantes**: `Session`, `Croix`, `Post`, `Media`
- **Pages**:
  1. `docs/pages/add-content/p05-add-content.md`
  2. `docs/pages/add-content/p07-add-croix-stepper.md` (future)

## `Profile`
- **Page principale**: `docs/pages/profile/p11-profile-private.md`
- **Entités dominantes**: `User`, `Session`, `Croix`, `Post`, `Media`
- **Pages**:
  1. `docs/pages/profile/p11-profile-private.md`
  2. `docs/pages/profile/p12-settings.md`
  3. `docs/pages/profile/content-management.md`
  4. `docs/pages/profile/p06-session-detail.md` (future)
  5. `docs/pages/profile/p08-croix-detail.md` (future)
  6. `docs/pages/profile/p09-edit-croix.md` (future)

## `Stats`
- **Page principale**: `docs/pages/stats/p10-stats-placeholder.md` (à créer)
- **Entités dominantes**: `Session`, `Croix`, `Analytics`
- **Pages**:
  1. `docs/pages/stats/p10-stats-placeholder.md` (future)
  2. `docs/pages/stats/p23-ai-coach.md` (future)

---

## 4. Décisions De Navigation (MVP)

## Tabs principales
1. Home (feed vidéo social)
2. Ajouter (point d'entrée unique création)
3. Stats
4. Profil (compte + mes contenus)

## Dépendances fonctionnelles
1. `Auth` redirige vers `Home` après login/signup
2. `Add Content` crée du contenu puis renvoie vers `Profile` ou détail contenu
3. Toute **édition de contenu** est rattachée à la famille `Profile`
4. `Home` consomme le contenu des autres, pas l'historique perso

---

## 5. Priorités De Docs A Rédiger Ensuite

1. `docs/pages/home/p04-home-video-feed.md` (raffiner interactions feed)
2. `docs/pages/add-content/p05-add-content.md` (matrice des types de contenu)
3. `docs/pages/profile/p11-profile-private.md` (section Mes blocs / Mes posts)
4. `docs/pages/profile/p06-session-detail.md`
5. `docs/pages/profile/p08-croix-detail.md`
6. `docs/pages/profile/p09-edit-croix.md`
7. `docs/pages/stats/p10-stats-placeholder.md`

