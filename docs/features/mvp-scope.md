# Feature: MVP Scope - Perfect Boulder

**Status**: 🟡 En cours
**Version**: MVP (v0.1)
**Last Updated**: 2026-02-07

---

## 🎯 Objectif

Définir le périmètre **minimum viable** pour valider que Perfect Boulder répond au besoin principal : **permettre à un grimpeur de garder un souvenir visuel structuré de ses séances**.

**Critère de succès MVP** :
> "Je peux utiliser l'app systématiquement après chaque séance pendant 1 mois, sans friction."

---

## 👥 Personas Concernés

**Persona Principal** : Alex (grimpeur régulier en salle)
- Grimpe 2-3x/semaine
- Veut suivre sa progression
- Frustré de voir les blocs disparaître

**Utilisation MVP** : Usage personnel uniquement (pas de beta testeurs)

---

## 📋 Features Must Have (MVP)

### 1. Authentication & Profil

#### 1.1 Créer un Compte
**En tant que** nouveau utilisateur
**Je veux** créer un compte
**Afin de** sauvegarder mes croix de manière durable

**Acceptance Criteria** :
- [ ] Signup avec email + password
- [ ] Validation email (format)
- [ ] Password min 8 caractères
- [ ] Confirmation password
- [ ] Message d'erreur clair si email déjà utilisé
- [ ] Redirect vers écran Home après signup

**Priorité** : P0 (bloquant)
**Effort** : S (1-2 jours)

---

#### 1.2 Se Connecter
**En tant que** utilisateur existant
**Je veux** me connecter
**Afin de** accéder à mon historique

**Acceptance Criteria** :
- [ ] Login avec email + password
- [ ] JWT token généré et stocké
- [ ] Message d'erreur si credentials invalides
- [ ] Redirect vers Home après login
- [ ] Token refresh automatique

**Priorité** : P0 (bloquant)
**Effort** : S (1-2 jours)

---

#### 1.3 Profil Basique
**En tant que** utilisateur
**Je veux** voir mon profil
**Afin de** vérifier mes infos et settings

**Acceptance Criteria** :
- [ ] Écran profil avec nom, email, photo
- [ ] Possibilité de modifier nom
- [ ] Possibilité de changer photo (upload)
- [ ] Logout fonctionnel

**Priorité** : P1 (important)
**Effort** : S (1 jour)

---

### 2. Session Management

#### 2.1 Créer une Session
**En tant que** grimpeur
**Je veux** créer une session après ma séance
**Afin de** regrouper toutes mes croix du jour

**Acceptance Criteria** :
- [ ] Bouton "Nouvelle session" visible sur Home
- [ ] Formulaire : Date (défaut: aujourd'hui), Lieu, Ressenti (optionnel)
- [ ] Sauvegarde instantanée
- [ ] Redirect vers liste des croix de la session
- [ ] Session créée vide (sans croix au départ)

**Priorité** : P0 (bloquant)
**Effort** : M (2-3 jours)

---

#### 2.2 Voir Mes Sessions
**En tant que** grimpeur
**Je veux** voir la liste de mes sessions
**Afin de** retrouver mes séances passées

**Acceptance Criteria** :
- [ ] Écran Home affiche liste des sessions (plus récentes en premier)
- [ ] Chaque session affiche : Date, Lieu, Nombre de croix, Thumbnail première croix
- [ ] Tap sur session → Détail session
- [ ] Pull-to-refresh fonctionnel
- [ ] Message si aucune session ("Créez votre première session !")

**Priorité** : P0 (bloquant)
**Effort** : M (2-3 jours)

---

#### 2.3 Détail d'une Session
**En tant que** grimpeur
**Je veux** voir le détail d'une session
**Afin de** revoir mes croix de cette séance

**Acceptance Criteria** :
- [ ] Écran affiche : Date, Lieu, Ressenti, Liste des croix
- [ ] Chaque croix affiche : Photo/vidéo thumbnail, Couleur, Statut
- [ ] Tap sur croix → Détail croix (fullscreen)
- [ ] Bouton "Ajouter une croix" visible
- [ ] Possibilité de modifier infos session (date, lieu)
- [ ] Possibilité de supprimer session (avec confirmation)

**Priorité** : P0 (bloquant)
**Effort** : M (3 jours)

---

### 3. Croix en Salle (Indoor)

#### 3.1 Ajouter une Croix en Salle
**En tant que** grimpeur en salle
**Je veux** ajouter une croix avec photo
**Afin de** garder un souvenir du bloc avant qu'il disparaisse

**Acceptance Criteria** :
- [ ] Stepper 3 étapes : Média → Informations → Commentaire
- [ ] **Étape 1 - Média** :
  - [ ] Prendre photo (caméra native)
  - [ ] Ou sélectionner photo galerie
  - [ ] Preview photo avant valider
  - [ ] Bouton "Suivant" actif seulement si photo sélectionnée
- [ ] **Étape 2 - Informations** :
  - [ ] Sélection salle (dropdown + "Ajouter nouvelle")
  - [ ] Sélection couleur (palette 7 couleurs : 🔴🟠🟡🟢🔵⚫⚪)
  - [ ] Sélection statut : Flash / À vue / Projet (radio buttons)
  - [ ] Si "Projet" : Champ "Nombre d'essais" (number input)
  - [ ] Ressenti (slider 1-5 émotions : 😡😐😊🤩)
  - [ ] Tous champs obligatoires sauf ressenti
- [ ] **Étape 3 - Commentaire** :
  - [ ] Champ texte libre (optionnel, max 500 caractères)
  - [ ] Bouton "Enregistrer"
- [ ] Upload photo vers S3
- [ ] Sauvegarde métadonnées en DB
- [ ] Redirect vers Détail session après enregistrement
- [ ] Message de succès ("Croix ajoutée ✅")

**Priorité** : P0 (bloquant)
**Effort** : L (5-7 jours)

---

#### 3.2 Voir Détail d'une Croix
**En tant que** grimpeur
**Je veux** voir une croix en fullscreen
**Afin de** revoir le bloc en détail

**Acceptance Criteria** :
- [ ] Photo affichée fullscreen
- [ ] Possibilité de zoomer (pinch gesture)
- [ ] Affichage métadonnées : Salle, Couleur, Statut, Ressenti, Date
- [ ] Affichage commentaire si présent
- [ ] Bouton retour
- [ ] Swipe left/right pour naviguer entre croix de la session

**Priorité** : P0 (bloquant)
**Effort** : M (2-3 jours)

---

#### 3.3 Modifier une Croix
**En tant que** grimpeur
**Je veux** modifier une croix
**Afin de** corriger une erreur ou ajouter info oubliée

**Acceptance Criteria** :
- [ ] Bouton "Modifier" sur détail croix
- [ ] Formulaire pré-rempli avec données actuelles
- [ ] Possibilité de changer photo
- [ ] Possibilité de changer métadonnées (couleur, statut, etc.)
- [ ] Sauvegarde modifications
- [ ] Message de succès

**Priorité** : P1 (important)
**Effort** : M (2 jours)

---

#### 3.4 Supprimer une Croix
**En tant que** grimpeur
**Je veux** supprimer une croix
**Afin de** retirer une erreur de saisie

**Acceptance Criteria** :
- [ ] Bouton "Supprimer" sur détail croix
- [ ] Modal de confirmation ("Êtes-vous sûr ?")
- [ ] Suppression DB + suppression photo S3
- [ ] Redirect vers Détail session
- [ ] Message de succès

**Priorité** : P1 (important)
**Effort** : S (1 jour)

---

### 4. Upload & Stockage Photo

#### 4.1 Upload Photo vers S3
**En tant que** système
**Je veux** stocker les photos sur S3
**Afin de** éviter de surcharger la DB

**Acceptance Criteria** :
- [ ] Upload multipart pour photos > 5MB
- [ ] Compression intelligente (réduire ~50% sans perte visible)
- [ ] Génération thumbnail (150x150px)
- [ ] Nom de fichier unique (UUID)
- [ ] URL stockée en DB
- [ ] Retry automatique si échec (3 tentatives)
- [ ] Message d'erreur clair si échec définitif

**Priorité** : P0 (bloquant)
**Effort** : M (3-4 jours)

---

#### 4.2 Affichage Photo Optimisé
**En tant que** utilisateur
**Je veux** que les photos s'affichent rapidement
**Afin de** avoir une expérience fluide

**Acceptance Criteria** :
- [ ] Affichage thumbnail dans listes (pas fullsize)
- [ ] Lazy loading (charge images quand visible)
- [ ] Cache images localement (éviter re-téléchargement)
- [ ] Placeholder pendant chargement
- [ ] Message d'erreur si image corrompue

**Priorité** : P1 (important)
**Effort** : M (2 jours)

---

### 5. Navigation & UI Basique

#### 5.1 Bottom Navigation
**En tant que** utilisateur
**Je veux** naviguer facilement entre écrans
**Afin de** accéder rapidement aux fonctions principales

**Acceptance Criteria** :
- [ ] Bottom tab bar avec 4 onglets :
  - [ ] 🏠 Home (liste sessions)
  - [ ] ➕ Ajouter (création session/croix)
  - [ ] 📊 Stats (placeholder MVP - "Bientôt disponible")
  - [ ] 👤 Profil
- [ ] Onglet actif visuellement distinct
- [ ] Navigation fluide (pas de lag)

**Priorité** : P0 (bloquant)
**Effort** : S (1-2 jours)

---

#### 5.2 Loading States
**En tant que** utilisateur
**Je veux** voir un feedback pendant les chargements
**Afin de** savoir que l'app travaille

**Acceptance Criteria** :
- [ ] Spinner global pendant requêtes API
- [ ] Skeleton screens pour listes (sessions, croix)
- [ ] Boutons disabled pendant actions (upload, save)
- [ ] Message "Chargement..." si > 2 secondes

**Priorité** : P1 (important)
**Effort** : S (1 jour)

---

#### 5.3 Error Handling
**En tant que** utilisateur
**Je veux** comprendre les erreurs
**Afin de** savoir quoi faire

**Acceptance Criteria** :
- [ ] Toast/Snackbar pour erreurs (non bloquant)
- [ ] Messages clairs (pas de code erreur technique)
- [ ] Exemples :
  - "Impossible de charger les sessions. Vérifiez votre connexion."
  - "Photo trop grande (max 10MB)"
  - "Email déjà utilisé"
- [ ] Possibilité de retry pour erreurs réseau

**Priorité** : P1 (important)
**Effort** : M (2 jours)

---

## 🚫 Hors Scope MVP

Ces features ne sont **PAS** dans le MVP (v1.0 ou plus tard) :

### Social
- ❌ Feed public
- ❌ Suivre des utilisateurs
- ❌ Likes/Commentaires
- ❌ Partage profil/sessions

### Croix Outdoor
- ❌ Ajouter croix outdoor (site, cotation)
- ❌ Carte des sites

### Vidéo
- ❌ Upload vidéo
- ❌ Capture vidéo
- ❌ Lecteur vidéo

### Stats
- ❌ Statistiques avancées
- ❌ Graphiques de progression
- ❌ Analyse IA

### Filtres & Recherche
- ❌ Filtrer sessions (date, lieu)
- ❌ Recherche par couleur/statut
- ❌ Tags personnalisés

### Gamification
- ❌ Badges
- ❌ Streaks
- ❌ Objectifs

### Premium
- ❌ Abonnement
- ❌ Features payantes

---

## 📊 Métriques de Succès MVP

**Objectif** : Valider usage personnel sur 1 mois

**Métriques** :
- ✅ **Utilisation systématique** : J'ajoute mes croix après chaque séance (8+ sessions en 1 mois)
- ✅ **Consultation pré-séance** : Je consulte l'historique avant 50%+ des séances
- ✅ **Temps d'ajout** : < 3 minutes par séance (4-5 croix)
- ✅ **Fiabilité** : 0 bug bloquant, < 3 bugs mineurs
- ✅ **Performance** : Upload photo < 10 secondes (4G)

**Critère d'échec** (MVP à revoir) :
- ❌ J'oublie d'ajouter mes croix > 30% du temps
- ❌ Bugs récurrents empêchent usage normal
- ❌ Temps d'ajout > 5 minutes (trop lent)

---

## 🗓️ Roadmap MVP (10 Semaines)

### Sprint 1 (S1-S2) : Infrastructure & Auth
- [ ] Setup monorepo (backend + mobile)
- [ ] Auth : Signup/Login/JWT
- [ ] Database : Tables users

**Deliverable** : Je peux créer un compte et me connecter

---

### Sprint 2 (S3-S4) : Sessions
- [ ] API CRUD sessions
- [ ] Database : Table sessions
- [ ] Mobile : Écran "Nouvelle session"
- [ ] Mobile : Liste sessions
- [ ] Mobile : Détail session

**Deliverable** : Je peux créer et voir mes sessions

---

### Sprint 3 (S5-S6) : Croix + Upload Photo
- [ ] Storage S3 (MinIO dev)
- [ ] API Upload photo
- [ ] API CRUD croix
- [ ] Database : Table croix
- [ ] Mobile : Stepper "Ajouter croix"
- [ ] Mobile : Caméra + galerie picker
- [ ] Mobile : Upload photo

**Deliverable** : Je peux ajouter une croix avec photo

---

### Sprint 4 (S7-S8) : Historique & Navigation
- [ ] API Liste croix par session
- [ ] Mobile : Bottom navigation
- [ ] Mobile : Détail croix (fullscreen)
- [ ] Mobile : Modifier/Supprimer croix
- [ ] Mobile : Navigation fluide

**Deliverable** : Je peux consulter mon historique visuellement

---

### Sprint 5 (S9-S10) : Polish & Tests
- [ ] Loading states
- [ ] Error handling
- [ ] Cache images
- [ ] Compression photos
- [ ] Tests backend (unit)
- [ ] Tests mobile (E2E basiques)
- [ ] Bug fixes

**Deliverable** : MVP stable et utilisable

---

## 🔗 Références

- [Flow UX Post-Séance](/docs/flows/post-session-quick.md) - À créer
- [Wireframes Stepper](/docs/pages/add-content.md) - À créer
- [Architecture Data Model](/docs/architecture/data-model.md) - À créer
- [Technical: Upload Strategy](/docs/technical/upload-strategy.md) - À créer

---

## 📝 Notes

**Décisions Importantes** :
- MVP = Photo uniquement (vidéo en v1.0)
- MVP = Salle uniquement (outdoor en v1.0)
- MVP = Usage personnel (social en v1.5)
- MVP = Stats placeholder (implémentation v1.0)

**Questions Ouvertes** :
- [ ] Faut-il permettre plusieurs photos par croix au MVP ? (Décision : Non, 1 photo max)
- [ ] Faut-il sauvegarder durée séance ? (Décision : Non, pas au MVP)
- [ ] Faut-il permettre édition date session ? (Décision : Oui, si erreur)

---

**Last Updated**: 2026-02-07
**Author**: Alex + Claude (Documentation Agent)
