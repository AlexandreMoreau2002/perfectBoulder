# 📚 Perfect Boulder - Documentation

Documentation complète du projet Perfect Boulder, organisée par thématique.

---

## 🗂️ Structure de la Documentation

```
docs/
├── README.md                    # ← Vous êtes ici
├── features/                    # Spécifications des features
├── pages/                       # Wireframes & specs des écrans
├── flows/                       # Parcours utilisateurs (UX flows)
├── technical/                   # Choix techniques & architecture
├── design/                      # Charte graphique, composants UI
├── architecture/                # Diagrammes système, modèles de données
└── decisions/                   # ADR (Architecture Decision Records)
```

---

## 📋 Index des Documents

### 🧭 Vocabulaire Commun

**Emplacement** : `/docs/`

| Document | Description | Statut |
|----------|-------------|--------|
| `lexique.md` | Lexique canonique produit + termes techniques | ✅ Complet |

---

### 🎯 Features (Spécifications Produit)

**Emplacement** : `/docs/features/`

| Document | Description | Statut |
|----------|-------------|--------|
| `mvp-scope.md` | Liste complète des features MVP | ✅ Complet |
| _À créer_ | Roadmap v1.0 → v2.0 | 🔴 Todo |
| _À créer_ | Priorisation (MoSCoW) | 🔴 Todo |

**Création prévue** :
- `features/mvp-scope.md` : Périmètre MVP complet
- `features/croix-tracking.md` : Feature "Enregistrer une croix"
- `features/social-posts.md` : Feature "Partager un moment"
- `features/historique.md` : Feature "Consulter l'historique"
- `features/premium.md` : Features premium (v2.0+)

---

### 📱 Pages (Wireframes & Specs Écrans)

**Emplacement** : `/docs/pages/`

| Document | Description | Statut |
|----------|-------------|--------|
| _À créer_ | Home (feed principal) | 🔴 Todo |
| _À créer_ | Ajouter contenu (stepper) | 🔴 Todo |
| _À créer_ | Historique (liste sessions) | 🔴 Todo |
| _À créer_ | Stats (tableau de bord) | 🔴 Todo |
| _À créer_ | Profil utilisateur | 🔴 Todo |

**Création prévue** :
- `pages/home-feed.md` : Écran d'accueil + feed
- `pages/add-content.md` : Stepper création croix/post
- `pages/session-detail.md` : Détail d'une session
- `pages/croix-detail.md` : Détail d'une croix (fullscreen média)
- `pages/stats-dashboard.md` : Statistiques et progression
- `pages/profile.md` : Profil public/privé

---

### 🔄 Flows (Parcours Utilisateurs)

**Emplacement** : `/docs/flows/`

| Document | Description | Statut |
|----------|-------------|--------|
| `video.md` | Gestion vidéo (capture, upload, lecture) | ✅ Complet |
| _À créer_ | Onboarding nouveau user | 🔴 Todo |
| _À créer_ | Post-séance (ajout rapide croix) | 🔴 Todo |
| _À créer_ | Consultation pré-séance | 🔴 Todo |
| _À créer_ | Partage contenu social | 🔴 Todo |

**Création prévue** :
- `flows/onboarding.md` : Première utilisation (signup → première croix)
- `flows/post-session-quick.md` : Ajout rapide après séance
- `flows/pre-session-review.md` : Consultation historique avant grimpe
- `flows/social-sharing.md` : Partage profil/session/croix
- `flows/conversion-social-to-tracking.md` : Convertir post → croix

---

### 🏗️ Technical (Choix Techniques)

**Emplacement** : `/docs/technical/`

| Document | Description | Statut |
|----------|-------------|--------|
| _À créer_ | Stack technique complète | 🔴 Todo |
| _À créer_ | Gestion médias (S3, thumbnails) | 🔴 Todo |
| _À créer_ | Upload background & retry | 🔴 Todo |
| _À créer_ | Auth & sécurité | 🔴 Todo |

**Création prévue** :
- `technical/stack-overview.md` : Vue d'ensemble backend/mobile
- `technical/media-storage.md` : Stockage S3, compression, thumbnails
- `technical/upload-strategy.md` : Background upload, retry, multipart
- `technical/auth-flow.md` : JWT, refresh tokens, permissions
- `technical/api-design.md` : REST vs GraphQL, conventions
- `technical/testing-strategy.md` : Unit, E2E, coverage

---

### 🎨 Design (Charte Graphique)

**Emplacement** : `/docs/design/`

| Document | Description | Statut |
|----------|-------------|--------|
| _À créer_ | Design system (couleurs, typo) | 🔴 Todo |
| _À créer_ | Composants UI (library) | 🔴 Todo |
| _À créer_ | Iconographie | 🔴 Todo |
| _À créer_ | Motion design (animations) | 🔴 Todo |

**Création prévue** :
- `design/design-system.md` : Couleurs, espacements, typographie
- `design/components.md` : Boutons, inputs, cards, modals
- `design/icons.md` : Jeu d'icônes (custom ou lib)
- `design/animations.md` : Transitions, gestures, micro-interactions
- `design/brand-identity.md` : Logo, ton, personnalité de marque

**Choix éditoriaux** (à définir) :
- Palette de couleurs (primaire, secondaire, accents)
- Style visuel (minimaliste, coloré, pro, fun)
- Ton de communication (technique, inspirant, communautaire)

---

### 🏛️ Architecture (Système & Données)

**Emplacement** : `/docs/architecture/`

| Document | Description | Statut |
|----------|-------------|--------|
| _À créer_ | Diagramme architecture globale | 🔴 Todo |
| `architecture/data-model.md` | Modèle de données v0 (entités + relations) | ✅ Initialisé |
| _À créer_ | Architecture backend (hexagonale) | 🔴 Todo |
| _À créer_ | Architecture mobile (layered) | 🔴 Todo |

**Création prévue** :
- `architecture/system-overview.md` : Vue d'ensemble (backend, mobile, infra)
- `architecture/data-model.md` : ERD (User, Session, Croix, Média)
- `architecture/backend-hexagonal.md` : Ports & adapters, dépendances
- `architecture/mobile-layered.md` : Screens → Services → Repositories
- `architecture/deployment.md` : CI/CD, environnements (dev/prod)

---

### 📜 Decisions (ADR - Architecture Decision Records)

**Emplacement** : `/docs/decisions/`

Les ADR documentent **pourquoi** une décision technique/produit a été prise, avec contexte et alternatives.

| Document | Décision | Statut |
|----------|----------|--------|
| _À créer_ | Pourquoi FastAPI (vs Django/Flask) | 🔴 Todo |
| _À créer_ | Pourquoi React Native (vs Flutter) | 🔴 Todo |
| _À créer_ | Pourquoi PostgreSQL (vs MongoDB) | 🔴 Todo |
| _À créer_ | Pourquoi S3 (vs DB pour médias) | 🔴 Todo |
| _À créer_ | Pourquoi Hexagonal Architecture | 🔴 Todo |

**Format ADR** :
```markdown
# ADR-XXX: [Titre de la décision]

**Date** : YYYY-MM-DD
**Statut** : Accepté / Supplanté / Deprecated

## Contexte

[Quel est le problème/besoin ?]

## Décision

[Quelle solution a été choisie ?]

## Alternatives Considérées

1. **Option A** : [Description] → Raison rejet
2. **Option B** : [Description] → Raison rejet

## Conséquences

**Positives** :
- ...

**Négatives** :
- ...

## Références

- [Lien vers doc externe]
```

**Création prévue** :
- `decisions/001-backend-fastapi.md`
- `decisions/002-frontend-react-native.md`
- `decisions/003-database-postgresql.md`
- `decisions/004-storage-s3.md`
- `decisions/005-architecture-hexagonal.md`
- `decisions/006-two-flow-social-vs-tracking.md` ← **Important !**

---

## 🚀 Documents Prioritaires (À Créer Maintenant)

### Phase 1 : MVP Scope (Cette Semaine)

1. **`features/mvp-scope.md`** ✨
   - Liste complète features Must Have
   - User stories principales
   - Critères d'acceptance

2. **`flows/post-session-quick.md`** ✨
   - Parcours post-séance (30 sec par croix)
   - Wireframes stepper (Média → Infos → Commentaire)
   - Cas limites

3. **`architecture/data-model.md`** ✨
   - ERD (User, Session, Croix, Média)
   - Relations entre entités
   - Champs obligatoires vs optionnels

4. **`technical/media-storage.md`** ✨
   - Upload vidéo (multipart, compression)
   - Génération thumbnails (ffmpeg)
   - Stockage S3 (dev: MinIO, prod: Scaleway)

5. **`decisions/006-two-flow-social-vs-tracking.md`** ✨
   - Pourquoi deux parcours distincts
   - Alternatives (toggle, auto-detect, deux points d'entrée)
   - Choix final : Menu FAB

---

## 📝 Template de Documentation

### Pour une Feature

```markdown
# Feature: [Nom de la Feature]

## 🎯 Objectif

[Quel problème cette feature résout ?]

## 👥 Personas Concernés

- Persona 1 (usage principal)
- Persona 2 (usage secondaire)

## 📋 User Stories

**En tant que** [persona]
**Je veux** [action]
**Afin de** [bénéfice]

**Acceptance Criteria** :
- [ ] Critère 1
- [ ] Critère 2

## 🎨 Wireframes / Mockups

[Insérer images ou liens Figma]

## 🔧 Spécifications Techniques

**Backend** :
- Endpoints API
- Modèles de données

**Mobile** :
- Écrans concernés
- Services/Repositories

## 🚦 Priorisation

- **Version** : MVP / v1.0 / v1.5 / v2.0
- **Priorité** : Must / Should / Could / Won't
- **Effort** : S / M / L / XL
- **Impact** : High / Medium / Low

## 📊 Métriques de Succès

[Comment mesurer si la feature fonctionne ?]

## 🔗 Références

- Lien vers flow UX
- Lien vers wireframes
- Lien vers ADR
```

---

## 🔄 Maintenance de la Doc

### Règles

1. **Toujours à jour** : MAJ docs AVANT de coder
2. **ADR pour décisions importantes** : Documenter le "pourquoi"
3. **Wireframes > long texte** : Privilégier visuels
4. **Liens entre docs** : Cross-références systématiques
5. **Versionning** : Git pour tracer évolutions

### Workflow

```
1. Brainstorm feature
   ↓
2. Créer doc feature
   ↓
3. Créer flow UX si besoin
   ↓
4. Valider avec user (toi)
   ↓
5. Créer ADR si décision tech
   ↓
6. Implémenter
   ↓
7. MAJ doc si comportement change
```

---

## 🤝 Contribution

### Pour Ajouter une Doc

1. Identifier le bon dossier (`features/`, `flows/`, etc.)
2. Utiliser template approprié
3. Nommer clairement (`croix-tracking.md`, pas `feature1.md`)
4. Ajouter entrée dans ce README
5. Commit avec message clair

### Conventions de Nommage

- **Fichiers** : kebab-case (`croix-tracking.md`)
- **Titres** : Titre Case (`Feature: Croix Tracking`)
- **Statuts** : Émoji (🔴 Todo, 🟡 En cours, ✅ Complet)

---

## 📞 Contact & Questions

Pour toute question sur la doc :
- Créer une issue GitHub
- Ping dans le README du repo principal
- Demander à Claude Code (`/claude`)

---

**Dernière mise à jour** : 2026-02-07
**Mainteneur** : Alex (@alex)
