# Workflow de Pilotage - Perfect Boulder

**Source de verite unique. En cas de doute, revenir ici.**

---

## Etat actuel

| Etape | Status | Livrable |
|-------|--------|----------|
| 1. Brainstorming Home | `DONE` | Decisions tranchees (ci-dessous) |
| 2. PRD MVP | `DONE` | `docs/prd.md` |
| 3. Spec Front-End Home | `DONE` | `docs/specs/home-page.md` |
| 4. Implementation | `A FAIRE` | Code qui tourne |

---

## Regles du systeme

1. **1 source de verite par sujet** - Pas 3 docs qui disent des choses differentes
2. **Decider avant de documenter** - On tranche, puis on ecrit
3. **Concis > Verbeux** - Si ca tient pas en 1 page, c'est trop
4. **MVP = le minimum qui marche** - Pas de features "au cas ou"

---

## Decisions (Etape 1 - DONE)

Tranchees le 2026-02-09. Ces decisions remplacent tout ce qui se contredit dans les anciens docs.

### Architecture produit

```
DECISION-001: La Home est un feed video TikTok-style (fullscreen, scroll vertical, autoplay)
DECISION-002: Le feed Home = VIDEO uniquement. Pas de photos dans le feed.
DECISION-003: Separation stricte contenu social vs contenu metier :
              - Social = Videos (feed, public, decouverte, attractif)
              - Metier = Photos + notes + stats (logbook, prive, utilitaire)
```

### Interactions MVP

```
DECISION-004: Interactions MVP = Like uniquement
              - Like : coeur + compteur
              - PAS de save, PAS de comments, PAS de share au MVP
```

### Navigation MVP

```
DECISION-005: Bottom bar = 4 tabs :
              1. Home (feed video)
              2. Search/Explore (decouverte)
              3. Stats (placeholder "Bientot disponible")
              4. Profile (mon profil)
DECISION-006: Le bouton Create (poster video) est accessible depuis les pages, PAS en tab dedie
```

---

## Etape 2 : PRD MVP (DONE)

**Objectif** : 1 seul document qui definit le scope MVP.

- Remplace `mvp-scope.md`, `HOME_FEED_MVP.md`, `home-feed.md`
- Maximum 2 pages
- Base sur les decisions ci-dessus
- Liste exhaustive de ce qui est IN et OUT

### Output attendu

`docs/prd.md` - Le PRD unique du MVP

---

## Etape 3 : Spec Front-End Home (DONE)

**Objectif** : 1 spec technique de la page Home, prete a coder.

- Layout exact (wireframe ASCII)
- Composants et leur comportement
- Structure des donnees (types TS)
- Etats : loading, empty, error, normal
- Interactions : scroll, tap, navigation

### Output attendu

`docs/specs/home-page.md` - La spec unique de la Home

---

## Etape 4 : Implementation (CURRENT)

**Objectif** : Coder la Home.

- React Native + Expo
- Navigation fonctionnelle (4 tabs)
- Feed video avec scroll snap
- Like + Save fonctionnels
- Testable sur simulateur

### Output attendu

Code fonctionnel dans `mobile/`

---

## Fichiers obsoletes (a archiver apres etape 2)

Ces fichiers seront remplaces par le PRD unique :
- `docs/mvp/HOME_FEED_MVP.md`
- `docs/features/home-feed.md`
- `docs/features/mvp-scope.md`

**Ne pas les supprimer avant que le PRD soit valide.**

---

## Comment utiliser ce workflow

1. Ouvre ce fichier au debut de chaque session
2. Regarde le status de chaque etape
3. Travaille sur l'etape marquee `CURRENT`
4. Quand une etape est finie, passe son status a `DONE` et la suivante a `CURRENT`
5. Si c'est le chaos, reviens ici

---

*Derniere MAJ : 2026-02-09*
