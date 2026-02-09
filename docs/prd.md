# PRD - Perfect Boulder MVP

**Version** : 1.0
**Date** : 2026-02-09
**Source de verite** : Ce document + `docs/WORKFLOW.md`

---

## Vision

App mobile de grimpe. Feed video TikTok/Instagram-style + logbook personnel structure.
Le feed est social (videos publiques). Le logbook est prive (photos, notes, stats).

---

## Scope MVP

### IN

**Home (Feed Video)**
- Feed vertical fullscreen, scroll snap, autoplay muted, loop
- Contenu = videos uniquement (pas de photos dans le feed)
- Ordre chronologique (plus recent en haut)
- Infos affichees sur chaque post :
  - Avatar + pseudo (top-left, tappable → profil)
  - Bloc info : couleur + grade + status (flash/top/projet)
  - Caption (texte libre, 1-3 lignes, tronque avec "...")
- Interaction : Like (coeur + compteur)
- Etats : loading, empty ("Aucune video"), error ("Reessayer")

**Search**
- Barre de recherche pour trouver des profils (par pseudo)
- Resultats : liste de users avec avatar + pseudo
- Tap → profil public du user

**Stats**
- Placeholder "Bientot disponible" (image ou animation)

**Profile**
- Infos : avatar, pseudo, bio
- Section 1 : Grille de mes videos (contenu social)
- Section 2 : Mon logbook (sessions, croix, photos - contenu metier)
- Bouton Create (poster une video) accessible depuis cette page

**Navigation**
- Bottom bar 4 tabs : Home / Search / Stats / Profile
- Bouton Create sur les pages (pas en tab)

**Auth**
- Signup email + password
- Login / Logout
- JWT

### OUT (post-MVP)

- Photos dans le feed
- Comments, share, save/playlists
- Tabs filtres (Suivis / Decouverte / Ma Salle)
- Feed algorithmique
- Grille explore / videos populaires
- Stats reelles
- Outdoor
- Gamification, badges, streaks
- Notifications push
- Dark mode
- Support multi-langues (i18n infrastructure deja en place, mais langue = FR uniquement au MVP)
- Analyse IA
- Stories 24h
- Messages / DM
- Follow system (on peut voir les profils mais pas follow au MVP)

---

## Architecture

```
Feed (Social)          Logbook (Metier)
─────────────          ────────────────
Videos publiques       Photos privees
Like                   Sessions + Croix
Decouverte             Progression
```

Ces deux mondes coexistent mais sont separes dans l'UI (feed vs profile/logbook).

---

## Donnees par post video

```typescript
interface FeedPost {
  id: string
  authorId: string
  authorName: string
  authorAvatar: string
  videoUrl: string
  color: string
  grade: string
  status: 'flash' | 'top' | 'project'
  caption: string
  likeCount: number
  isLikedByMe: boolean
  createdAt: string
}
```

---

## Pages

| Page | Tab | Contenu |
|------|-----|---------|
| Home | Home | Feed video fullscreen |
| Search | Search | Barre recherche + resultats profils |
| Stats | Stats | Placeholder |
| Profile | Profile | Info + grille videos + logbook |

---

## Architecture technique

**Backend :**
- FastAPI + Strawberry GraphQL + PostgreSQL
- Architecture hexagonale (domain → application → adapters → infra)
- 7 queries + 7 mutations GraphQL
- Cloudflare R2 pour stockage video (egress gratuit)

**Donnees :**
- 7 tables : users, videos, likes, sessions, croix, gyms, exercises
- Videos peuvent pointer vers session/croix (optionnel)
- Sessions/croix ne pointent PAS vers videos

**Specs detaillees :**
- Data model : `docs/specs/data-model.md`
- Archi backend : `backend/docs/architecture.md`
- Archi frontend : `mobile/docs/home-page.md`

---

En cas de contradiction avec un autre doc, ce PRD fait foi.
