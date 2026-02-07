# Debugging et Logging Mobile - Perfect Boulder

**Status**: ✅ Complete
**Last Updated**: 2026-02-07
**Audience**: Développeurs frontend React Native

---

## Objectif

Spécifier un système de debugging et logging **centralisé** et **facile à contrôler** via des **IF conditions** pour développer rapidement avec visibilité totale sur les appels API, changements d'état, et rendu des composants.

---

## Architecture

### Configuration DEBUG Centralisée

Le fichier `utils/debug.ts` exporte un objet `DEBUG` avec des flags pour chaque type de log :

```typescript
export const DEBUG = {
  // Global debug toggle
  ENABLED: __DEV__ || process.env.DEBUG === 'true',

  // Log détaillé des calls API
  API: __DEV__ || process.env.DEBUG_API === 'true',

  // Log des changements de state / context
  STATE: __DEV__ || process.env.DEBUG_STATE === 'true',

  // Log du rendu des composants (expensive - disabled by default)
  RENDER: process.env.DEBUG_RENDER === 'true',

  // Log des erreurs détaillées
  ERRORS: __DEV__ || process.env.DEBUG_ERRORS === 'true',

  // Log des navigations
  NAVIGATION: __DEV__ || process.env.DEBUG_NAV === 'true',

  // Mock API responses (dev rapide sans backend)
  MOCK_API: process.env.MOCK_API === 'true',

  // Simulate network delays (ms)
  NETWORK_DELAY: process.env.NETWORK_DELAY ? parseInt(process.env.NETWORK_DELAY, 10) : 0,

  // Simulate API errors
  SIMULATE_ERRORS: process.env.SIMULATE_ERRORS === 'true',
};
```

**Chaque flag contrôle un aspect spécifique du debug**.

---

## Setup Initial

### Pour un nouveau dev

1. **Cloner le repo**
   ```bash
   git clone ...
   cd mobile
   ```

2. **Installer dépendances**
   ```bash
   npm install
   ```

3. **Créer `.env` depuis template**
   ```bash
   cp .env.example .env
   ```

4. **Éditer `.env`** selon tes besoins (API URL, debug flags, etc.)
   ```bash
   # Par défaut : DEBUG_API=true pour logs API
   nano .env
   ```

5. **Lancer l'app**
   ```bash
   npm start
   ```

**Note** : Ne JAMAIS commiter `.env` - seulement `.env.example` !

---

## Utilisation

### Pattern 1 : IF DEBUG dans Composants

```typescript
// App.tsx
import { DEBUG, withDebug, debugLog } from '@/utils/debug';

export default function App() {
  // Exécute UNIQUEMENT si DEBUG.ENABLED = true
  withDebug.if(() => {
    debugLog.render('App');
    console.log('Debug mode actif');
  });

  return (
    <View>
      {/* Affiche badge debug si activé */}
      {DEBUG.ENABLED && <Text style={styles.debugTag}>🐛 DEBUG</Text>}
    </View>
  );
}
```

### Pattern 2 : IF DEBUG dans Services

```typescript
// repositories/sessionRepository.ts
import { DEBUG, debugLog } from '@/utils/debug';

async getSession(id: string): Promise<SessionDTO> {
  const startTime = performance.now();

  // Log API call SEULEMENT si DEBUG.API = true
  debugLog.api('GET', `/sessions/${id}`);

  try {
    const response = await apiClient.get<SessionDTO>(`/sessions/${id}`);
    const duration = performance.now() - startTime;

    // Log réponse avec timing
    debugLog.apiResponse('GET', `/sessions/${id}`, response.data, duration);
    return response.data;
  } catch (error) {
    const duration = performance.now() - startTime;
    debugLog.apiError('GET', `/sessions/${id}`, error, duration);
    throw error;
  }
}
```

### Pattern 3 : IF DEBUG pour State Changes

```typescript
// contexts/AuthContext.tsx
import { DEBUG, debugLog } from '@/utils/debug';

const [user, setUser] = useState<User | null>(null);

const login = async (email: string, password: string) => {
  const prevUser = user;

  try {
    const newUser = await authService.login(email, password);
    setUser(newUser);

    // Log state change SEULEMENT si DEBUG.STATE = true
    debugLog.stateChange('AuthContext.user', prevUser, newUser);
  } catch (error) {
    debugLog.error('AuthContext.login', error);
  }
};
```

### Pattern 4 : Conditional Rendering Debug

```typescript
// Affiche infos debug seulement en mode debug
{DEBUG.ENABLED && (
  <View style={styles.debugPanel}>
    <Text>API enabled: {DEBUG.API}</Text>
    <Text>State enabled: {DEBUG.STATE}</Text>
    <Text>Locale: {I18n.locale}</Text>
  </View>
)}
```

---

## Logging Structuré

### debugLog API

Tous les logs retournent des **logs formatés avec couleurs** dans la console :

#### `debugLog.api(method, url, data?)`

```typescript
debugLog.api('POST', '/posts', { content: 'Hello' });
// [API] POST /posts
// payload: { content: 'Hello' }
```

#### `debugLog.apiResponse(method, url, response, duration)`

```typescript
debugLog.apiResponse('GET', '/sessions/123', { id: '123', ... }, 245);
// [API RESPONSE] GET /sessions/123 (245ms)
// response: { id: '123', ... }
```

#### `debugLog.apiError(method, url, error, duration)`

```typescript
debugLog.apiError('POST', '/posts', error, 150);
// [API ERROR] POST /posts (150ms)
// error: Error: Network timeout
```

#### `debugLog.stateChange(name, prevState, newState)`

```typescript
debugLog.stateChange('posts', prevPosts, newPosts);
// [STATE] posts
// prev: [...]
// new: [...]
```

#### `debugLog.render(componentName, props?)`

```typescript
debugLog.render('PostCard', { postId: '123' });
// [RENDER] PostCard
// props: { postId: '123' }
```

#### `debugLog.navigation(action, params?)`

```typescript
debugLog.navigation('navigate', { screen: 'ProfileScreen', id: 'user-123' });
// [NAV] navigate
// params: { screen: 'ProfileScreen', id: 'user-123' }
```

#### `debugLog.error(context, error)`

```typescript
debugLog.error('SessionService.getSession', error);
// [ERROR] SessionService.getSession
// error: Error: Session not found
// stack: at SessionService.getSession (...)
```

---

## Configuration via .env

Tous les flags se configurent via le fichier `.env` (local, ne pas commiter).

### Fichier `.env.example`

Template versionné pour nouveaux devs :

```env
# ============================================
# Perfect Boulder Mobile - Environment Variables
# ============================================

# API Configuration
EXPO_PUBLIC_API_URL=http://localhost:8000

# Debug & Logging
DEBUG=false
DEBUG_API=false
DEBUG_STATE=false
DEBUG_RENDER=false
DEBUG_ERRORS=true
DEBUG_NAV=false

# API Simulation
NETWORK_DELAY=0
SIMULATE_ERRORS=false
MOCK_API=false

# Environment
EXPO_PUBLIC_ENV=development
```

### Fichier `.env` (Local)

Copier `.env.example` en `.env` et adapter :

```env
# Mode développement avec API logs
DEBUG=false
DEBUG_API=true
DEBUG_STATE=false
DEBUG_RENDER=false
DEBUG_ERRORS=true
DEBUG_NAV=false

NETWORK_DELAY=0
SIMULATE_ERRORS=false
MOCK_API=false

EXPO_PUBLIC_API_URL=http://localhost:8000
EXPO_PUBLIC_ENV=development
```

**Ne PAS commiter `.env`** - il est dans `.gitignore`

### Modification des Flags

**Pour déboguer rapidement** :
1. Éditer `.env`
2. Change les flags selon tes besoins :
   ```env
   DEBUG_API=true      # Active les logs API
   NETWORK_DELAY=500   # Simule 500ms de latence
   SIMULATE_ERRORS=true # Simule erreurs API
   ```
3. Sauvegarder et relancer : `npm start`
4. Les changes sont appliqués au reload

**Pour overrider CLI** (temporaire, ne change pas `.env`) :
```bash
DEBUG_API=true npm start
DEBUG_STATE=true npm start
NETWORK_DELAY=2000 npm start
```

### .gitignore

`.env` est déjà dans `.gitignore` :
```
# local env files
.env
.env.local
.env*.local
```

**Seul `.env.example` est commité** (pour servir de template).

---

## Cas d'Usage

### Use Case 1 : Debugger un appel API qui fail

```bash
# Lancer avec logs API détaillés
DEBUG_API=true npm start

# Vérifier la requête et la réponse dans les logs
# [API] GET /sessions/123
# [API ERROR] GET /sessions/123 (245ms)
# error: Error: 404 Not Found
```

### Use Case 2 : Profiler les performances

```bash
# Simuler delay réseau pour tester UI en slow network
NETWORK_DELAY=2000 npm start

# Observer comment l'app se comporte avec latence
```

### Use Case 3 : Tracker les changements de state

```bash
# Lancer avec logs d'état
DEBUG_STATE=true npm start

# Observer chaque changement de state/context
# [STATE] posts
# prev: [...]
# new: [...]
```

### Use Case 4 : Tester mocking API

```bash
# Lancer l'app sans backend réel
MOCK_API=true npm start

# Toutes les APIs retournent des données mockées
```

### Use Case 5 : Tester gestion d'erreurs

```bash
# Simuler erreurs API aléatoires
SIMULATE_ERRORS=true npm start

# 50% des requests failent - vérifier error handling
```

---

## Bonnes Pratiques

### ✅ Do's

- Utiliser `withDebug.if()` pour logique debug seulement
- Utiliser `debugLog.*` pour logs formatés
- Placer `debugLog.api()` AVANT l'appel API
- Placer `debugLog.apiResponse()` / `debugLog.apiError()` APRÈS
- Inclure **timing** pour perf checks (`performance.now()`)
- Toggler via **variables d'environnement** (jamais hardcoder)
- Nettoyer les `console.log` bruts avant merge

### ❌ Don'ts

- Ne pas laisser `console.log` nudz (utiliser `debugLog`)
- Ne pas activer `DEBUG.RENDER` en prod (expensive)
- Ne pas hardcoder `DEBUG.* = true` dans le code
- Ne pas oublier les cleanup dans les logs
- Ne pas logguer données sensibles (tokens, passwords)

---

## Intégration dans Projets Existants

### Étape 1 : Ajouter utils/debug.ts

Copier le contenu depuis `/mobile/utils/debug.ts` (créé)

### Étape 2 : Importer dans App.tsx

```typescript
import { DEBUG, withDebug, debugLog } from '@/utils/debug';

withDebug.if(() => console.log('DEBUG MODE'));
```

### Étape 3 : Utiliser dans Services

```typescript
import { debugLog } from '@/utils/debug';

debugLog.api('GET', '/posts');
// ... API call
debugLog.apiResponse('GET', '/posts', data, 200);
```

### Étape 4 : Lancer avec flags

```bash
DEBUG_API=true npm start
```

---

## Performance Considerations

### Coûts des Logs

| Log Type | Cost | Default |
|----------|------|---------|
| `API` | Low (string formatting) | Enabled in __DEV__ |
| `STATE` | Low (object diff) | Enabled in __DEV__ |
| `RENDER` | **HIGH** (every render) | **Disabled** |
| `ERRORS` | Low (only on error) | Enabled in __DEV__ |
| `NAVIGATION` | Low (per nav) | Enabled in __DEV__ |

**Note**: `DEBUG.RENDER` est **disabled by default** car cela crée BEAUCOUP de logs.

### Optimisation

```typescript
// ✅ GOOD: Check before expensive operations
if (DEBUG.RENDER) {
  console.log('expensive calculation:', complexObject);
}

// ❌ BAD: Calculs toujours exécutés même si log désactivé
console.log('expensive calculation:', complexObject);
```

---

## Dépannage

### Logs n'apparaissent pas

**Vérifier** :
1. Flag `DEBUG.*` est bien set à `true`
2. Vous êtes en mode `__DEV__` (ou `DEBUG=true`)
3. Code exécute bien le chemin (ajouter breakpoint)

### Trop de logs

**Solution** :
1. Désactiver `DEBUG.RENDER` (très verbose)
2. Activer seulement le flag pertinent (ex: `DEBUG_API=true`)
3. Utiliser grep pour filtrer : `npm start 2>&1 | grep "\[API\]"`

### Performance dégradée

**Causes** :
1. `DEBUG.RENDER` activé (expensive)
2. `NETWORK_DELAY` trop élevé
3. Trop de `SIMULATE_ERRORS` créant exceptions

**Solution** : Désactiver flags non-nécessaires

---

## Checklist Implementation

- [ ] `utils/debug.ts` créé avec tous les flags
- [ ] `debugLog` exporté avec toutes les méthodes
- [ ] `withDebug` utilitaire crée
- [ ] Logs API ajoutés dans repositories
- [ ] Logs state ajoutés dans contexts
- [ ] App.tsx affiche `DEBUG` badge si activé
- [ ] Docs actualisées dans `/mobile/docs/FRONTEND_RULES.md`
- [ ] Variables d'env testées
- [ ] Aucun `console.log` brut restant

---

## References

- **Mobile Rules**: `/mobile/docs/FRONTEND_RULES.md` → Section "Logging et Debugging"
- **Lexique**: `/docs/lexique.md`
- **CLAUDE.md**: `/CLAUDE.md` → Mobile section

---

**Last Updated**: 2026-02-07
**Version**: 1.0.0
