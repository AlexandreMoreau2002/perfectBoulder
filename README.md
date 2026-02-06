# perfectBoulder

Une application de bouldering/escalade construite en monorepo avec une architecture hexagonale et clean layers.

## Structure du Projet

```
.
├── backend/          # API FastAPI + PostgreSQL + GraphQL
├── mobile/           # App React Native (Expo)
├── nginx/            # Configuration reverse proxy (à venir)
└── .agent/           # Contrats AI et règles de développement
```

## Stack Technologique

- **Backend**: FastAPI, Strawberry GraphQL, PostgreSQL, Docker
- **Mobile**: React Native 0.81.5, Expo SDK ~54, TypeScript
- **Infrastructure**: Docker Compose, Nginx

---

## 🤖 BMAD - Workflow Agile avec IA

Ce projet utilise **BMAD** (Business-Minded Agile Development) pour orchestrer le développement avec des agents IA spécialisés.

### Qu'est-ce que BMAD ?

BMAD est un framework qui structure la collaboration entre humains et agents IA en rôles spécialisés :

- **Scrum Master (Bob)** `/sm` - Crée les stories détaillées pour les développeurs
- **Développeur** `/dev` - Implémente les stories
- **QA** `/qa` - Teste et valide
- **Architecte** `/architect` - Conçoit les solutions complexes
- **Analyste** `/analyst` - Recherche et investigation
- Et d'autres rôles spécialisés...

Chaque rôle a ses responsabilités précises, ses commandes et ses workflows.

### Workflow BMAD sur ce Projet

```
1. SCRUM MASTER (/sm)
   ↓
   Crée une story détaillée du PRD

2. DÉVELOPPEUR (/dev)
   ↓
   Implémente la story

3. QA (/qa)
   ↓
   Teste et valide

4. REVISION → MERGE
```

---

## 🚀 Guide Rapide - Comment Utiliser BMAD

### 1. Démarrer une Session Scrum Master

```bash
/sm
```

Le Scrum Master (Bob) affiche ses commandes disponibles.

### 2. Créer une Nouvelle Story

```bash
*draft
```

Cela lance le workflow `create-next-story` qui :
- Vous pose des questions sur la feature à implémenter
- Extrait les infos du PRD et de l'architecture
- Génère une story **détaillée et actionnelle**

**Résultat** : Une story prête à être assignée au développeur.

### 3. Valider la Story (Checklist)

```bash
*story-checklist
```

Valide que la story respecte les critères de qualité :
- Acceptance criteria clairs
- Lien au PRD
- Architecture alignée
- Estimable et faisable

### 4. Passer la Story au Développeur

```bash
/dev
```

Le développeur prend la story et l'implémente.

### 5. Tester avec QA

```bash
/qa
```

QA teste l'implémentation et valide.

### 6. Corriger la Trajectoire (Si Nécessaire)

```bash
*correct-course
```

Le Scrum Master ajuste le scope ou les priorités si besoin.

### 7. Quitter le Mode Scrum Master

```bash
*exit
```

---

## 📋 Commandes Scrum Master (`/sm`)

| Commande | Description |
|----------|-------------|
| `*help` | Affiche la liste des commandes |
| `*draft` | Crée une nouvelle story détaillée |
| `*story-checklist` | Valide une story |
| `*correct-course` | Corrige la trajectoire du projet |
| `*exit` | Quitte le mode Scrum Master |

---

## 📋 Commandes Développeur (`/dev`)

Le développeur implémente les stories avec ses propres commandes (code, tests, commits).

---

## 📋 Commandes QA (`/qa`)

QA valide les implémentations avec ses propres workflows de test.

---

## 🎯 Principes Clés

✅ **Une story = Une feature claire et testable**

✅ **Les infos viennent du PRD et de l'architecture**

✅ **Pas d'ambiguïtés - chaque story est actionnelle**

✅ **Workflow structuré : SM → Dev → QA → Merge**

✅ **Agile avec IA : Efficace, traçable, itératif**

---

## 🛠️ Setup Initial

### Backend

```bash
cd backend
make start          # Démarrer les services
make code           # Shell dans le container
```

### Mobile

```bash
cd mobile
npm start           # Démarrer Expo dev server
npm run ios         # iOS simulator
npm run android     # Android emulator
```

---

## 📖 Documentation

Voir aussi :
- `CLAUDE.md` - Guide détaillé pour Claude Code
- `.agent/` - Contrats et règles de développement
- `backend/` - Architecture hexagonale du backend
- `mobile/` - Architecture layered de l'app mobile

---

## 🤝 Workflow Collaboratif

1. **PM/PO** définit les features dans le PRD
2. **Scrum Master** crée les stories (`*draft`)
3. **Développeur** implémente (`/dev`)
4. **QA** teste (`/qa`)
5. **Architecte** intervient si besoin (`/architect`)
6. **Review & Merge** en main branch

---

## 💡 Astuces

- Utilisez `*help` dans n'importe quel rôle pour voir les commandes
- Les stories bien préparées = implémentation rapide et sans surprises
- La checklist story-draft est votre guarantee de qualité
- Restez en mode approprié : SM pour les stories, Dev pour l'implémentation, QA pour la validation

---

Bon développement! 🚀
