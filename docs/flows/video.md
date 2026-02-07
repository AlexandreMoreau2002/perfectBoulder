# Gestion Vidéo - UX & Fonctionnalités

## 🎯 Vision Produit

La vidéo dans Perfect Boulder n'est pas un "nice-to-have", c'est **une fonctionnalité centrale** qui capture :
- Le mouvement et la méthode (pas juste le bloc)
- La progression technique (style, fluidité)
- Les souvenirs vivants (vs photo statique)

**Principe clé** : Simplicité d'usage > Richesse des options

---

## 📱 Scénarios d'Usage

### Scénario 1 : Post-Séance Rapide (Usage Principal)
**Contexte** : Alex vient de finir un bloc, veut capturer rapidement
**Parcours** :
1. Ouvre l'app
2. Crée une croix
3. **Enregistre 10-20 secondes de vidéo** depuis l'app
4. Sauvegarde (pas de montage)
5. Quitte la salle

**Temps cible** : < 30 secondes par croix
**Priorité** : MVP Must Have

---

### Scénario 2 : Upload Vidéo Existante
**Contexte** : Marie a filmé ses essais pendant la séance, veut garder la meilleure tentative
**Parcours** :
1. Crée une croix
2. Sélectionne vidéo de la galerie
3. (Optionnel) Crop la vidéo (début/fin)
4. Sauvegarde

**Temps cible** : < 1 minute
**Priorité** : MVP Must Have

---

### Scénario 3 : Montage Externe (Power Users)
**Contexte** : Thomas (ouvreur) veut montrer son bloc avec un montage pro
**Parcours** :
1. Fait son montage sur CapCut/InShot
2. Exporte la vidéo
3. Upload dans Perfect Boulder (vidéo déjà éditée)
4. Publie

**Temps cible** : Upload uniquement (montage externe)
**Priorité** : v1.0 Should Have

---

## 🎬 Fonctionnalités par Version

### MVP (Must Have) - Capture & Upload Simple

**Capture vidéo native** :
- [x] Enregistrement depuis l'app (caméra native)
- [x] Durée max : 60 secondes (configurable)
- [x] Qualité : HD (720p) par défaut
- [x] Orientation : Portrait prioritaire
- [x] Preview immédiat après capture
- [x] Refaire si nécessaire

**Upload depuis galerie** :
- [x] Sélection vidéo depuis galerie téléphone
- [x] Limite taille : 50MB (free) / 200MB (premium)
- [x] Formats supportés : MP4, MOV
- [x] Affichage durée/taille avant upload
- [x] Barre de progression upload

**Lecture & Affichage** :
- [x] Lecteur vidéo natif (pas de player custom au MVP)
- [x] Contrôles : Play/Pause, timeline, volume
- [x] Fullscreen
- [x] Thumbnail auto-généré (première frame)

**États & Feedback** :
- [x] Loading pendant upload
- [x] Erreur si fichier trop gros/format incorrect
- [x] Confirmation upload réussi
- [x] Retry en cas d'échec réseau

---

### v1.0 (Should Have) - Édition Basique

**Crop vidéo (trim)** :
- [ ] Sélectionner début/fin (timeline slider)
- [ ] Preview du segment sélectionné
- [ ] Max : Extraire 60 secondes d'une vidéo longue
- [ ] Sauvegarde du crop (pas de modification de l'original)

**Qualité & Compression** :
- [ ] Choix qualité : Auto / HD / Standard
- [ ] Compression côté client avant upload (réduire bande passante)
- [ ] Indicateur taille finale après compression

**Métadonnées vidéo** :
- [ ] Détection durée automatique
- [ ] Détection résolution
- [ ] Affichage infos dans l'historique

---

### v1.5 (Could Have) - Édition Avancée

**Musique** :
- [ ] Ajouter musique de fond (bibliothèque libre de droits)
- [ ] Régler volume musique vs son original
- [ ] Preview avec musique

**Effets basiques** :
- [ ] Filtres simples (contraste, saturation)
- [ ] Ralenti/accéléré (0.5x, 1x, 2x)
- [ ] Rotation vidéo

**Multi-angles** (si plusieurs essais) :
- [ ] Attacher plusieurs vidéos à une croix
- [ ] Galerie vidéo (swipe entre les essais)

---

### v2.0+ (Won't Have for now) - Features Premium

**Analyse IA** :
- [ ] Détection automatique du début/fin du mouvement
- [ ] Tracking du grimpeur (suivi automatique)
- [ ] Analyse de posture (suggestions)

**Montage auto** :
- [ ] Génération auto d'une vidéo "récap séance"
- [ ] Transitions entre blocs
- [ ] Overlay infos (cotation, date)

**Social avancé** :
- [ ] Réactions en temps réel (comme/commentaires)
- [ ] Split-screen (comparer deux essais)

---

## 💾 Contraintes Techniques

### Stockage

**Backend** :
- Vidéos stockées sur S3 compatible (AWS, Scaleway, MinIO dev)
- DB stocke uniquement : URL, durée, résolution, taille, thumbnail URL
- Génération thumbnail côté backend (ffmpeg)

**Mobile** :
- Upload en arrière-plan (iOS/Android background upload)
- Retry automatique si échec réseau
- Cache local des vidéos récemment consultées

**Limites par tier** :
| Tier    | Taille max/vidéo | Stockage total | Qualité max |
|---------|------------------|----------------|-------------|
| Free    | 50MB             | 5GB            | HD (720p)   |
| Premium | 200MB            | Illimité       | Full HD     |

---

### Performance

**Upload** :
- Compression côté client avant upload (réduire de ~50%)
- Upload multipart pour gros fichiers (chunks de 5MB)
- Affichage progression en temps réel
- Temps estimé : ~10-20 secondes pour 50MB (4G)

**Lecture** :
- Streaming adaptatif (HLS si possible en v2.0)
- Cache vidéo locale (éviter re-téléchargement)
- Préchargement thumbnail pour historique

**Formats** :
- Input : MP4, MOV, AVI (convertis en MP4)
- Output : MP4 (H.264) pour compatibilité universelle
- Thumbnail : JPEG (auto-généré, 1ère frame ou frame à 2 secondes)

---

## 🎨 UX/UI Considérations

### Flow d'Ajout Vidéo (MVP)

```
Écran "Ajouter Croix"
    |
    v
[ Photo ] [ Vidéo ] <-- Toggle boutons
    |
    v (si Vidéo sélectionnée)
    |
+-------------------+
| [ 🎥 Enregistrer ]|  <-- Ouvre caméra native
| [  📁 Galerie   ] |  <-- Ouvre picker vidéo
+-------------------+
    |
    v (après sélection)
    |
+-------------------+
| Preview vidéo     |
| [▶ Lecture]       |
| Durée: 15s        |
| Taille: 12MB      |
|                   |
| [ ❌ Annuler ]    |
| [ ✅ Confirmer ]  |
+-------------------+
    |
    v (si confirmer)
    |
Upload en cours... [████████--] 80%
    |
    v
Vidéo ajoutée ✅
```

---

### Flow avec Crop (v1.0)

```
Après sélection vidéo :
    |
    v
+-------------------+
| Preview vidéo     |
| [▶ Lecture]       |
|                   |
| Timeline:         |
| |----[====]---| |  <-- Sliders début/fin
| 00:05 → 00:18    |  <-- Segment sélectionné
|                   |
| [ ✂️ Crop ]       |
| [ ✅ Confirmer ]  |
+-------------------+
```

---

### Affichage Historique

**Vue Liste (Sessions)** :
```
+-------------------+
| Session 06/02     |
| 📍 Sharma Climbing|
|                   |
| [🎬 Thumbnail]    |  <-- Vidéo (icône play)
| Bloc rouge V4     |
|                   |
| [📸 Thumbnail]    |  <-- Photo (pas d'icône)
| Bloc jaune V3     |
+-------------------+
```

**Vue Détail Croix** :
- Si vidéo : Lecteur plein écran avec contrôles
- Si photo : Image plein écran avec zoom
- Toggle photo/vidéo si les deux présents

---

## 🚨 Cas Limites & Gestion d'Erreurs

### Échecs Upload

**Réseau instable** :
- Retry automatique (3 tentatives)
- Upload en arrière-plan (continue même si app fermée)
- Notification quand upload terminé

**Fichier trop gros** :
- Message clair : "Vidéo trop grande (120MB), max 50MB en version gratuite"
- Proposition : "Passer à Premium pour 200MB" OU "Réduire qualité/durée"

**Format non supporté** :
- Détection avant upload
- Message : "Format non supporté (AVI). Formats acceptés : MP4, MOV"
- Proposition : "Convertir avec une app externe puis ré-essayer"

---

### Espace de Stockage

**Utilisateur atteint limite (Free)** :
- Message : "Stockage plein (5GB/5GB)"
- Options :
  1. Supprimer anciennes vidéos
  2. Passer à Premium (stockage illimité)
  3. Télécharger et supprimer

**Backend stockage plein** :
- Dégradation gracieuse : Photos toujours acceptées
- Notification admin
- Message utilisateur : "Service temporairement indisponible"

---

### Lecture Vidéo

**Connexion lente** :
- Affichage thumbnail immédiatement
- Loading spinner pendant chargement vidéo
- Possibilité d'annuler et revenir

**Vidéo corrompue** :
- Détection à l'upload (validation format)
- Si détecté après : Message "Vidéo indisponible", possibilité de re-uploader

---

## 🔐 Sécurité & Validation

### Upload

- **Validation côté client** :
  - Extension fichier (.mp4, .mov)
  - Taille max (50MB/200MB selon tier)
  - Durée max (60s pour MVP)

- **Validation côté backend** :
  - Vérification MIME type (pas juste extension)
  - Scan antivirus (si v2.0+)
  - Limite rate : 10 uploads/heure par user (anti-spam)

### Stockage

- **URLs signées** (S3 presigned URLs) :
  - Durée validité : 1 heure
  - Impossible de deviner URL d'autres vidéos
  - Régénération automatique à chaque requête

- **Visibilité** :
  - Privé : Uniquement l'utilisateur
  - Partageable : Via lien (URL unique non listée)
  - Public : Visible dans feed (v1.5+)

---

## 📊 Métriques & Analytics (v2.0+)

**Usage vidéo** :
- % de croix avec vidéo vs photo seule
- Durée moyenne des vidéos uploadées
- Taux d'abandon upload (échecs)
- Temps moyen upload

**Engagement** :
- Nombre de lectures (vue complète vs partielle)
- Vidéos les plus regardées
- Taux de partage (vidéo vs photo)

**Performance** :
- Temps upload moyen par taille
- Taux d'échec upload
- Bande passante consommée

---

## 🎯 Décisions de Conception

### Pourquoi limiter à 60 secondes (MVP) ?

✅ **Pour** :
- Simplifie l'implémentation (pas de gestion fichiers énormes)
- Réduit coûts stockage/bande passante
- Force l'utilisateur à capturer l'essentiel (pas de vidéos 10min)
- Cohérent avec usage "post-séance rapide"

❌ **Contre** :
- Limite pour projets longs (plusieurs essais)
- Frustrant pour ouvreurs qui veulent montrer un bloc complet

**Décision** : 60s au MVP, extensible en Premium (3-5 minutes)

---

### Pourquoi pas de montage avancé (MVP) ?

✅ **Pour** :
- Complexité technique élevée (traitement vidéo sur mobile)
- Déjà couvert par apps externes (CapCut, InShot)
- Risque de sur-engineering (pas besoin d'être TikTok)
- Focus sur le core : Garder souvenirs, pas créer contenu viral

❌ **Contre** :
- Friction si utilisateur doit switcher d'app
- Moins "wow effect" au lancement

**Décision** : Montage minimal (crop uniquement) en v1.0, montage externe encouragé

---

### Pourquoi pas de player custom (MVP) ?

✅ **Pour** :
- Player natif (iOS AVPlayer, Android ExoPlayer) fiable et performant
- Évite bugs/incompatibilités (formats, codecs)
- Moins de maintenance

❌ **Contre** :
- Moins de contrôle sur UI/UX
- Pas de features custom (annotations, slow-motion live)

**Décision** : Player natif au MVP, custom player en v1.5 si besoin réel

---

## 🎬 Roadmap Vidéo

### MVP (3-4 mois)
- [x] Capture vidéo native (60s max)
- [x] Upload galerie (50MB max)
- [x] Lecteur natif
- [x] Thumbnail auto

### v1.0 (2-3 mois après MVP)
- [ ] Crop vidéo (trim début/fin)
- [ ] Compression intelligente
- [ ] Upload arrière-plan
- [ ] Retry automatique

### v1.5 (3-4 mois après v1.0)
- [ ] Musique de fond
- [ ] Filtres basiques
- [ ] Multi-angles
- [ ] Ralenti/accéléré

### v2.0+ (6+ mois après v1.5)
- [ ] Analyse IA (détection mouvement)
- [ ] Montage auto récap séance
- [ ] Streaming adaptatif (HLS)
- [ ] Split-screen comparaison

---

## 🔧 Stack Technique (Implémentation)

### Backend

**Upload & Stockage** :
```python
# backend/app/application/services/video_service.py
class VideoService:
    async def upload_video(
        self,
        file: UploadFile,
        user_id: str,
        croix_id: str
    ) -> VideoResponse:
        # 1. Validation (taille, format, durée)
        # 2. Compression/transcoding si besoin
        # 3. Upload vers S3
        # 4. Génération thumbnail (ffmpeg)
        # 5. Sauvegarde métadonnées en DB
        pass
```

**Génération Thumbnail** :
```bash
# Utilise ffmpeg dans container backend
ffmpeg -i video.mp4 -ss 00:00:02 -vframes 1 -q:v 2 thumbnail.jpg
```

**Stockage** :
- MinIO (dev local)
- AWS S3 / Scaleway Object Storage (prod)

---

### Mobile

**Capture Vidéo** :
```typescript
// mobile/src/services/VideoService.ts
import * as ImagePicker from 'expo-image-picker';
import * as Camera from 'expo-camera';

export class VideoService {
  async recordVideo(): Promise<VideoAsset> {
    // 1. Demander permissions caméra
    // 2. Ouvrir caméra native
    // 3. Enregistrer max 60s
    // 4. Retourner URI locale
  }

  async pickFromGallery(): Promise<VideoAsset> {
    // 1. Ouvrir galerie
    // 2. Filtrer type: video
    // 3. Vérifier taille/durée
    // 4. Retourner URI locale
  }
}
```

**Upload** :
```typescript
// mobile/src/repositories/VideoRepository.ts
export class VideoRepository {
  async uploadVideo(
    uri: string,
    croixId: string,
    onProgress: (progress: number) => void
  ): Promise<VideoUploadResponse> {
    // 1. Créer FormData
    // 2. Upload multipart avec progression
    // 3. Retry si échec
    // 4. Retourner URL vidéo
  }
}
```

---

## 🚦 Prochaines Actions

### Immédiat (MVP)
1. [ ] Valider les flows UX (capture, upload, lecture)
2. [ ] Définir les limites techniques exactes (taille, durée, formats)
3. [ ] Implémenter upload backend (S3 + métadonnées)
4. [ ] Implémenter capture/upload mobile

### Court Terme (v1.0)
1. [ ] Ajouter crop vidéo (trim)
2. [ ] Implémenter compression côté client
3. [ ] Upload arrière-plan (iOS/Android)

### Moyen Terme (v1.5+)
1. [ ] Musique de fond (bibliothèque libre droits)
2. [ ] Effets basiques (filtres, ralenti)

---

## 🎯 Proposition : Flow Adaptatif selon l'Intention

Basé sur la maquette du stepper (Média → Informations → Commentaire), voici **deux parcours** distincts :

### Parcours A : Post Social (Rapide - 30 secondes)
```
┌─────────────────────────┐
│ 1. Média                │
│  [Importer vidéo/photo] │
└────────────┬────────────┘
             │
             v
┌─────────────────────────┐
│ 2. Informations         │ <-- SIMPLIFIÉ
│  ☐ C'est un post social│     (toggle)
│    (pas de tracking)    │
│                         │
│  [@] Taguer amis        │
│  [#] Hashtags           │
└────────────┬────────────┘
             │
             v
┌─────────────────────────┐
│ 3. Commentaire          │
│  "Check ce bloc 🔥"     │
└────────────┬────────────┘
             │
             v
          Publier
```

**Pour qui** : Utilisateur qui veut juste partager, comme Instagram
**Objectif** : Friction minimale, engagement social

---

### Parcours B : Croix Trackée (Détaillé - 2-3 minutes)

```
┌─────────────────────────┐
│ 1. Média                │
│  [Importer vidéo/photo] │
└────────────┬────────────┘
             │
             v
┌─────────────────────────┐
│ 2. Informations         │ <-- COMPLET
│  ☑ Enregistrer comme    │     (toggle activé)
│    croix (tracking)     │
│                         │
│ Type:                   │
│  ( ) Salle  ( ) Outdoor │
│                         │
│ --- Si Salle ---        │
│ Salle: [Sharma Climbing]│
│ Couleur: [🔴 Rouge]     │
│ Statut:                 │
│  ( ) Flash              │
│  (•) Essais (3 essais)  │
│  ( ) Projet             │
│                         │
│ --- Si Outdoor ---      │
│ Site: [Fontainebleau]   │
│ Voie: [La Marie-Rose]   │
│ Cotation: [7a]          │
│ Partenaires: [@Marie]   │
│                         │
│ Ressenti:               │
│  😊 😐 😓 😡           │
└────────────┬────────────┘
             │
             v
┌─────────────────────────┐
│ 3. Commentaire          │
│  "Enfin réussi après    │
│   une semaine! La clé   │
│   c'est le talon..."    │
└────────────┬────────────┘
             │
             v
      Enregistrer Croix
```

**Pour qui** : Grimpeur qui veut tracker sa progression
**Objectif** : Capturer métadonnées utiles (couleur, cotation, ressenti)

---

## 🎨 Options UX pour Différencier les Deux Parcours

### Option 1 : Toggle en Haut de l'Étape 2 (Recommandé MVP)

```
┌──────────────────────────────────────────┐
│  2. Informations                    [←]  │
├──────────────────────────────────────────┤
│                                          │
│  Enregistrer comme croix  [ OFF ] ← Toggle│
│                                          │
│  ┌────────────────────────────────────┐ │
│  │ Si OFF = Post social simple        │ │
│  │ • Pas de tracking                  │ │
│  │ • Juste description + hashtags     │ │
│  └────────────────────────────────────┘ │
│                                          │
│  📝 Description (optionnel)              │
│  [________________________]              │
│                                          │
│  #️⃣ Hashtags                            │
│  [________________________]              │
│                                          │
│  [@] Taguer des amis                     │
│  [________________________]              │
│                                          │
│              [Suivant]                   │
└──────────────────────────────────────────┘
```

**Avantages** :
- ✅ Intention claire dès le début
- ✅ Parcours court si toggle désactivé
- ✅ Utilisateur comprend la différence

**Inconvénients** :
- ❌ Risque de confusion (toggle pas vu)

---

### Option 2 : Détection Automatique + Suggestion (Plus Smart)

```
┌──────────────────────────────────────────┐
│  2. Informations                    [←]  │
├──────────────────────────────────────────┤
│                                          │
│  💡 On a détecté un bloc en salle!       │
│                                          │
│  Voulez-vous enregistrer cette croix     │
│  pour suivre votre progression?          │
│                                          │
│  [📊 Oui, tracker]  [🎬 Non, post social]│
│                                          │
└──────────────────────────────────────────┘
```

**Si "Oui, tracker"** → Formulaire complet
**Si "Non, post social"** → Description simple + publier

**Avantages** :
- ✅ Guide l'utilisateur vers le tracking
- ✅ Éduque sur la valeur ajoutée
- ✅ Pas de friction pour posts sociaux

**Inconvénients** :
- ❌ Requiert IA (détection bloc) → v2.0
- ❌ Peut irriter si mauvaise détection

---

### Option 3 : Deux Points d'Entrée Distincts (Le Plus Clair)

Refonte du point d'entrée (avant l'étape 1) :

```
┌──────────────────────────────────────────┐
│  Ajouter du contenu                 [←]  │
├──────────────────────────────────────────┤
│                                          │
│  Que voulez-vous faire?                  │
│                                          │
│  ┌────────────────────────────────────┐ │
│  │  📊 Enregistrer une croix          │ │
│  │                                    │ │
│  │  Suivez votre progression avec     │ │
│  │  toutes les infos (lieu, couleur,  │ │
│  │  cotation, ressenti)               │ │
│  │                                    │ │
│  │           [Commencer]              │ │
│  └────────────────────────────────────┘ │
│                                          │
│  ┌────────────────────────────────────┐ │
│  │  🎬 Partager un moment             │ │
│  │                                    │ │
│  │  Publiez simplement une photo/     │ │
│  │  vidéo avec description            │ │
│  │                                    │ │
│  │           [Commencer]              │ │
│  └────────────────────────────────────┘ │
│                                          │
└──────────────────────────────────────────┘
```

**Avantages** :
- ✅ **Clarté maximale** : Deux intentions distinctes
- ✅ Pas de confusion possible
- ✅ Onboarding évident pour nouveaux users

**Inconvénients** :
- ❌ Un écran de plus dans le flow
- ❌ Peut ralentir l'usage rapide post-séance

---

### 💡 Recommandation Finale : Hybride

**Menu contextuel (FAB) + Option 1 (Toggle)**

#### Navigation Principale (Bottom Tab)

```
┌────────────────────────────────────────┐
│                                        │
│         [Contenu de l'app]             │
│                                        │
└────────────────────────────────────────┘
  🏠    📸    [+]    📊    👤
 Home  Feed  Ajouter Stats Profil
            ↓
     ┌──────────┐
     │ Menu Fab │
     ├──────────┤
     │ 📊 Croix │  → Flow détaillé (tracking)
     │ 🎬 Post  │  → Flow rapide (social)
     └──────────┘
```

**Avantage** :
- Rapide d'accès (FAB central)
- Distinction claire dès le départ
- Pas d'écran supplémentaire (menu contextuel)
- L'utilisateur choisit avant de démarrer

---

## 📋 Formulaires Détaillés par Type de Croix

### Croix en Salle (Étape 2 - Informations)

```
┌──────────────────────────────────────────┐
│  2. Informations                         │
├──────────────────────────────────────────┤
│                                          │
│  📍 Salle *                              │
│  [Sharma Climbing        ▼]              │
│  ou [+ Ajouter une salle]                │
│                                          │
│  🎨 Couleur du bloc *                    │
│  [🔴][🟠][🟡][🟢][🔵][⚫][⚪]            │
│                                          │
│  🎯 Statut                               │
│  ( ) ⚡ Flash (1ère tentative)          │
│  (•) 🔄 À vue (2-5 essais)              │
│  ( ) 💪 Projet (6+ essais)              │
│                                          │
│  Si "Projet":                            │
│  Nombre d'essais: [3]                    │
│                                          │
│  😊 Ressenti                             │
│  😡────😐────😊────🤩                   │
│        ↑ (slider)                        │
│                                          │
│  📅 Date (optionnel)                     │
│  [Aujourd'hui      ▼] (défaut: auto)    │
│                                          │
│  ⏱️ Durée (optionnel)                   │
│  [2h30]                                  │
│                                          │
│              [Suivant]                   │
└──────────────────────────────────────────┘
```

**Champs obligatoires (*)** :
- Salle : Pour filtrer/retrouver
- Couleur : Identifiant principal du bloc en salle

**Champs optionnels** :
- Statut (défaut : "À vue")
- Nombre d'essais (si projet)
- Ressenti (slider émotions)
- Date (défaut : aujourd'hui)
- Durée séance

---

### Croix Outdoor (Étape 2 - Informations)

```
┌──────────────────────────────────────────┐
│  2. Informations                         │
├──────────────────────────────────────────┤
│                                          │
│  🏔️ Site *                               │
│  [Fontainebleau      ▼]                  │
│  ou [+ Ajouter un site]                  │
│                                          │
│  🧗 Nom de la voie/bloc                  │
│  [La Marie-Rose]                         │
│                                          │
│  📊 Cotation *                           │
│  [7a               ▼]                    │
│  (Liste : 4a→9c, V0→V17)                 │
│                                          │
│  🎯 Statut                               │
│  (•) ✅ Réussi                           │
│  ( ) 🔄 Projet                           │
│                                          │
│  👥 Partenaires (optionnel)              │
│  [@Marie] [@Thomas]                      │
│                                          │
│  😊 Ressenti                             │
│  😡────😐────😊────🤩                   │
│        ↑                                 │
│                                          │
│  📅 Date (optionnel)                     │
│  [06/02/2026       ▼]                    │
│                                          │
│              [Suivant]                   │
└──────────────────────────────────────────┘
```

**Champs obligatoires (*)** :
- Site : Localisation géographique
- Cotation : Niveau de difficulté

**Champs optionnels** :
- Nom de la voie (utile mais pas bloquant)
- Statut (défaut : "Réussi")
- Partenaires (tags sociaux)
- Ressenti
- Date (défaut : aujourd'hui)

---

## 🚨 Cas Limites UX

### 1. Utilisateur commence "Post social" puis change d'avis

**Scénario** : User uploade vidéo → mode social → réalise qu'il veut tracker
**Solution** :
- Bouton "Convertir en croix trackée" en haut du formulaire
- Conserve média + description déjà saisie
- Ajoute champs métadonnées
- Message : "Remplissez les infos pour suivre cette croix"

---

### 2. Utilisateur oublie de tracker pendant la séance

**Scénario** : User poste 5 vidéos "social" → veut les convertir en croix rétroactivement
**Solution** (v1.0) :
- Fonction "Éditer" sur posts existants
- Option "Convertir en croix trackée"
- Remplir métadonnées rétroactivement
- Date ajustable (si post ancien)

---

### 3. Utilisateur veut tracker SANS vidéo (juste photo)

**Scénario** : Pas de vidéo disponible, mais veut quand même tracker
**Solution** :
- Photo = média valide pour croix trackée
- Formulaire identique (salle/outdoor)
- Vidéo optionnelle (pas obligatoire)
- Message : "Ajoutez au moins une photo ou vidéo"

---

### 4. Utilisateur hésite entre Salle/Outdoor

**Scénario** : Bloc en salle mais dehors (mur extérieur)
**Solution** :
- Type "Salle" = Infrastructure permanente avec couleurs
- Type "Outdoor" = Site naturel avec cotations fixes
- Tooltip explicatif : "Salle = blocs éphémères par couleur"

---

## 📊 Priorisation Features

### MVP (Must Have)
- [x] Deux parcours : Social vs Tracking
- [x] Menu FAB (Croix / Post)
- [x] Formulaire salle : Lieu + Couleur + Statut
- [x] Formulaire outdoor : Site + Cotation
- [x] Média (photo OU vidéo)
- [x] Étape 3 : Commentaire (description texte)

### v1.0 (Should Have)
- [ ] Ressenti (slider émotions)
- [ ] Nombre d'essais (si projet)
- [ ] Date custom (si pas aujourd'hui)
- [ ] Partenaires (tags utilisateurs)
- [ ] Conversion post social → croix

### v1.5 (Could Have)
- [ ] Durée séance (tracking temps)
- [ ] Auto-suggestion salle/site (fréquents)
- [ ] Historique des essais (plusieurs tentatives sur un projet)

### v2.0 (Premium)
- [ ] Détection auto type bloc (IA)
- [ ] Suggestion cotation (IA analyse vidéo)
- [ ] Import automatique depuis Kilter Board
- [ ] Analyse posture/mouvement

---

## 📝 Notes & Questions Ouvertes

**Question 1** : Faut-il permettre photo ET vidéo sur une même croix ?
- **Oui** : L'utilisateur peut vouloir les deux (photo du problème + vidéo de la solution)
- **Non** : Simplifie l'UX (un média = un type)
- **Décision** : OUI, mais vidéo prioritaire (affichée en premier si les deux présents)

**Question 2** : Que faire si l'utilisateur oublie de filmer ?
- Accepter ajout vidéo après coup (édition croix)
- Suggestion automatique "Ajouter une vidéo ?" si croix sans vidéo

**Question 3** : Analyse IA activée par défaut ?
- **Non** pour posts "social" (flag `is_social_only: bool`)
- **Oui** pour croix "performance" (analyse progression)
- Réglage dans profil : "Activer analyse auto"

**Question 4** : Quel modèle de donnée backend ?
```python
class Croix(BaseModel):
    id: UUID
    user_id: UUID
    session_id: UUID  # Optionnel (peut être None si post social)

    # Type
    type: Literal["salle", "outdoor", "social"]  # Discriminant

    # Médias
    photo_url: Optional[str]
    video_url: Optional[str]
    thumbnail_url: Optional[str]

    # Métadonnées communes
    description: Optional[str]
    created_at: datetime

    # Métadonnées tracking (None si social)
    lieu: Optional[str]  # Nom salle ou site
    statut: Optional[Literal["flash", "essais", "projet"]]
    ressenti: Optional[int]  # 1-5

    # Spécifique salle
    couleur: Optional[str]  # Si type="salle"
    nb_essais: Optional[int]

    # Spécifique outdoor
    voie_nom: Optional[str]  # Si type="outdoor"
    cotation: Optional[str]
    partenaires: List[UUID]  # Tags

    # Social
    hashtags: List[str]
    is_social_only: bool  # True si pas de tracking
    visibility: Literal["private", "friends", "public"]
```