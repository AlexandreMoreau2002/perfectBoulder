# Feature: Exercices de Renforcement

**Statut** : 🟡 En cours de spécification

---

## 🎯 Objectif

Permettre aux utilisateurs d'enregistrer des exercices de renforcement (muscu, suspension, poutre, etc.) lors de leurs séances, et afficher les statistiques agrégées pour établir le profil de performance et l'évolution de l'utilisateur.

---

## 📋 Description

En complément des croix (blocs escaladés), les utilisateurs peuvent ajouter à chaque session :
- **Exercices de muscu** (pompes, tractions, squats, etc.)
- **Exercices de suspension** (pull-ups, L-sits, hangs)
- **Exercices de poutre** (équilibre, déplacement)
- **Autres exercices de renforcement** (étirements, cardio, etc.)

Chaque exercice enregistre :
- **Nom** (ex: "Tractions")
- **Sets/Reps** (ex: 3 sets de 10 reps) ou durée (ex: 45 sec de hang)
- **Note** (poids, difficulté, RPE 1-10)
- **Mémoriser** (marquer comme "exercice standard" pour futures séances)

**Utilité** :
- Tracker la progression (max reps, poids, durée)
- Voir les patterns de renforcement (quels exercices aide à progresser)
- Identifier les faiblesses (peu d'exercice X = moins d'aisance sur Y)
- Profiler l'utilisateur (grimpeur "fort" vs "technique")

---

## 📊 Page Statistiques Améliorée

La page **Stats** affiche maintenant :

### Section "Force & Progression"

```
┌─────────────────────────────────┐
│ Tractions (Max reps)            │
│ Progression: 8 → 12 reps (+50%) │
│ [████████████░░░░░░] 12/15      │
├─────────────────────────────────┤
│ Hang (Max durée)                │
│ Progression: 30s → 65s (+116%)  │
│ [████████░░░░░░░░░░] 65s/120s   │
├─────────────────────────────────┤
│ Pompes (Max reps)               │
│ [████████░░░░░░░░░░] 25/50      │
└─────────────────────────────────┘
```

### Métriques par Exercice

**Pour chaque exercice enregistré** :
- **Max historique** (reps, poids, durée)
- **Tendance** (↗ croissant, ↘ décroissant, → stable)
- **Fréquence** (nombre de fois/semaine, mois)
- **Évolution 30j** (gain % depuis 30 jours)

### Profil d'Utilisateur

Basé sur les stats d'exercices :
- **Force générale** (score basé sur max reps/poids des exos)
- **Endurance** (hang time, durée des sets)
- **Type de grimpeur** (force, technique, équilibre) ← dérivé des stats

---

## 🎮 Workflow d'Ajout

Lors de la création d'une session :

```
1. Sélectionner activité (Grimpe / Renforcement / Autre)
2. Si "Grimpe" → croix (existant)
3. Si "Renforcement" → nouveau workflow :

   ┌─────────────────────────┐
   │ Ajouter un exercice     │
   ├─────────────────────────┤
   │ [Sélectionner ex.]      │
   │ (Tractions, Pompes...)  │
   │                         │
   │ Sets/Reps: [  3 ] x [10] │
   │ Note (RPE): [ 8/10 ]    │
   │                         │
   │ [ Mémoriser ]           │
   │ [ Ajouter ]             │
   └─────────────────────────┘

   (Répéter jusqu'à fini)

4. Enregistrer session
```

---

## 💾 Modèle de Données

### Entité `Exercise` (nouvelle)

```typescript
interface Exercise {
  id: string;
  name: string;                    // "Tractions", "Pompes", etc.
  category: 'strength' | 'suspension' | 'endurance' | 'balance' | 'other';
  description?: string;
  defaultUnit?: 'reps' | 'weight' | 'duration' | 'rpe';  // 10 reps, 20kg, 45s, RPE 8
}
```

### Entité `SessionExercise` (nouvelle)

```typescript
interface SessionExercise {
  id: string;
  sessionId: string;
  exerciseId: string;
  exerciseName: string;             // Snapshot du nom

  // Enregistrement
  sets?: number;                    // Nombre de sets (pour reps/weight)
  reps?: number;                    // Reps par set
  weight?: number;                  // Poids (kg)
  duration?: string;                // Durée (ex: "45s", "1m30s")
  rpe?: number;                     // Rate of Perceived Exertion 1-10
  notes?: string;                   // Notes libres ("Pas assez dormi", etc.)

  // Meta
  createdAt: Date;
  isFavorite?: boolean;             // Marquer comme favori pour retrouver facilement
}
```

### Entité `Session` (modifiée)

```typescript
interface Session {
  // ... existing fields
  exercises: SessionExercise[];     // Nouveau : list d'exercices de renforcement
  sessionType: 'climbing' | 'strength' | 'mixed' | 'other';  // Nouveau
}
```

---

## 📈 Stats API

### Nouvel endpoint

```
GET /api/stats/exercises

Réponse:
{
  "exercises": [
    {
      "exerciseId": "ex-1",
      "name": "Tractions",
      "category": "strength",
      "maxReps": 12,
      "maxWeight": 0,
      "maxDuration": null,
      "trend": "↗",  // ↗ | → | ↘
      "frequency": {
        "count": 15,
        "period": "30d"  // Nombre de fois en 30j
      },
      "evolution30d": {
        "maxRepsPrev": 8,
        "maxRepsCurr": 12,
        "percentGain": 50  // +50%
      },
      "lastSession": "2026-02-05T10:30:00Z"
    },
    ...
  ],
  "profileScore": {
    "strengthLevel": 72,      // 0-100
    "enduranceLevel": 58,
    "grimpeurType": "strength"  // "strength", "technique", "balanced"
  }
}
```

---

## 🎯 Priorisation

- **Version** : v1.5+ (après MVP de croix)
- **Priorité** : Should Have
- **Effort** : M (moyen - nouvelle entité + stats)
- **Impact** : High (profiling utilisateur crucial)

---

## ✅ Critères d'Acceptance

- [ ] Utilisateur peut ajouter exercice à une session
- [ ] Exercices sauvegardés et affichés dans détail session
- [ ] Page Stats affiche max reps/poids/durée par exercice
- [ ] Évolution 30j calculée et affichée
- [ ] Profil utilisateur (force/endurance/type) généré
- [ ] Exercices favoris mémorisés pour réutilisation facile

---

## 🔗 Références Futures

- Lien vers `pages/stats-dashboard.md` (à créer)
- Lien vers `pages/session-detail.md` (à créer)
- ADR : Pourquoi deux entités (Exercise + SessionExercise)
