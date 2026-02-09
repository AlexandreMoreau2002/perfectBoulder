# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## AVANT TOUTE CHOSE

**Lis ces 3 fichiers dans cet ordre avant de faire quoi que ce soit :**

1. `docs/README.md` - Point d'entree unique (ou aller selon le besoin)
2. `docs/WORKFLOW.md` - Etape en cours + decisions tranchees
3. `docs/prd.md` - Ce qu'on construit (scope MVP)

**En cas de contradiction** entre un doc et le PRD (`docs/prd.md`), le PRD fait foi.
**En cas de doute** sur quoi faire ensuite, consulte `docs/WORKFLOW.md`.

---

## Project Overview

**perfectBoulder** is a monorepo containing multiple services for a climbing/bouldering application.

### Repository Structure

```
.
├── backend/          # FastAPI + PostgreSQL + GraphQL API
├── frontend/         # React Native mobile app (à venir)
├── nginx/            # Reverse proxy configuration (à venir)
├── docs/             # Product & technical documentation
└── .agent/           # AI agent contracts and rules
    ├── agent.md            # General rules
    ├── backend.md          # Backend-specific rules
    ├── frontend.md         # Frontend-specific rules
    ├── infra.md            # Infrastructure rules
    └── doc-agent.md        # Documentation agent contract
```

### Technology Stack

- **Backend**: FastAPI, Strawberry GraphQL, PostgreSQL, Docker
- **Frontend**: React Native (à venir)
- **Infrastructure**: Docker Compose, Nginx

---

## General Rules (All Services)

These rules apply to the entire monorepo (from `.agent/agent.md`):

### Non-Negotiables
- **Ask before assuming**: If requirements are ambiguous, ask 1-3 targeted questions before coding
- **No speculative features**: Only implement what's explicitly requested + strictly necessary (tests/README)
- **Don't change existing conventions** (structure, naming, style) without explicit reason and approval
- **Never add dependencies** (npm, pip, docker images, GitHub actions) without explicit instruction
- **Avoid large refactors**: Minimal, localized, justified changes only

### Code Quality & Readability
- **One responsibility per file** (SRP)
- **No dead code**, unused imports, or obvious duplication (DRY)
- **Simple over clever** (KISS)
- **Explicit naming**: No vague variable/function names
- **Import ordering**: "Escalier" style - imports sorted by length (short → long), grouped clearly at top of file

#### Import Ordering (Escalier Pattern)

**CRITICAL RULE: Sort imports by TOTAL LINE LENGTH (shortest → longest), then group by category.**

Count the full import statement length including `import` keyword, not just the module name.

**React Native / TypeScript Frontend**:
```typescript
import I18n from '@/utils/i18n';
import { User } from '@/types/models';
import { View, Text } from 'react-native';
import { useAuth } from '@/hooks/useAuth';
import { colors } from '@/constants/colors';
import { Button } from '@/components/Button';
import { sessionService } from '@/services/sessionService';
import { useSafeAreaInsets } from 'react-native-safe-area-context';
```

**How it works** (by line length):
- Line 1: `import I18n from '@/utils/i18n';` (45 chars) ← shortest
- Line 2: `import { View, Text } from 'react-native';` (51 chars)
- Line 3: `import { Button } from '@/components/Button';` (54 chars)
- ...
- Line N: longest line (80+ chars)

**Groups** (keep groups together, but sort within each):
1. React + React Native + external libs (shortest to longest)
2. Local imports: @/components, @/hooks, @/services, @/utils, @/constants, @/types (shortest to longest)

**FastAPI / Python Backend**:
```python
from typing import Optional
from sqlalchemy.orm import Session
from fastapi import APIRouter, Depends
from app.domain.entities import Boulder
from app.infra.database import get_session
from app.application.services import BoulderService
from app.adapters.rest.schemas import BoulderSchema
```

**How it works** (by line length):
- `from typing import Optional` (shortest)
- `from sqlalchemy.orm import Session`
- `from fastapi import APIRouter, Depends`
- `from app.domain.entities import Boulder`
- ...
- `from app.infra.database import get_session` (longest)

**Groups** (keep groups together, then sort by length):
1. Standard library (short)
2. External packages (increasing length)
3. Local imports: app.domain, app.application, app.adapters, app.infra (shortest to longest)

### Security & Secrets
- Never log secrets (tokens, passwords, keys)
- Never commit `.env` files - provide `.env.example`/`.env.template` + documentation
- Validate, escape, and limit user inputs (uploads, payloads, size)

### Definition of Done (Mandatory)

Every interaction must conclude with:
1. **Test**: Verify code works (manual or automated)
2. **Update docs**: If behavior changes, update relevant documentation
3. **Create context**: If needed, create/update docs to help next AI (CONTEXT.md or `.agent/` notes)

### Pre-Submission Code Review Checklist (ALWAYS DO THIS)

**BEFORE submitting ANY code changes, verify:**

#### Imports (Escalier Pattern)
- [ ] All imports sorted by **TOTAL LINE LENGTH** (shortest → longest)
- [ ] External libs grouped first, local imports (`@/`) second
- [ ] No relative paths like `./` or `../` (use `@/` aliases)
- [ ] Semicolons at end of each import line

#### TypeScript & Types
- [ ] `npx tsc --noEmit` runs WITHOUT errors
- [ ] No `any` types (use `unknown` + narrowing if needed)
- [ ] All function parameters typed
- [ ] All component props have interfaces

#### Code Quality
- [ ] No unused imports or variables
- [ ] No `console.log` left in code (use `debugLog` if needed)
- [ ] No commented-out code blocks
- [ ] File follows SRP (one responsibility)
- [ ] **If modifying package.json**: Always run `npm ci` after to sync node_modules

#### File-Specific Checks
- **TypeScript/TSX files**: Check `tsconfig.json` is valid, no errors in IDE
- **React Native**: Check for `react-native-safe-area-context` import if using `SafeAreaView`
- **Config files**: Validate JSON/JS syntax

#### Before Submitting
- [ ] Run linter: `npx eslint src` (if configured)
- [ ] Run formatter: `npx prettier --check src` (if configured)
- [ ] **Test on iOS simulator** (`npm run ios`)
- [ ] **Test on Android emulator** (`npm run android`)
- [ ] **Check console for errors** (Xcode logs on iOS, logcat on Android)
- [ ] **Verify Context Providers** (SafeAreaProvider, etc. at top level App component, NOT inside nested components)
- [ ] Read CLAUDE.md rules again to ensure compliance

### Testing Strategy (All Services)

- **AAA pattern**: Arrange / Act / Assert with explicit assertions
- **Mock external dependencies**: Network, DB, mailer, time
- **Clean state**: Clear mocks/reset between tests
- **No fragile tests**: Avoid uncontrolled timers, unnecessary snapshots
- **Coverage threshold**: ~80% global (ignore only what's justified)
- **Stable CI runner**: Manage parallelism carefully

---

## Backend Service (`backend/`)

FastAPI-based API with PostgreSQL and GraphQL support following hexagonal architecture.

### Commands

All commands executed from `backend/` directory:

**Docker Environment (Makefile)**:
- `make start`: Start services in detached mode
- `make start-build`: Rebuild and start services
- `make stop`: Stop and remove containers
- `make reset`: Rebuild and restart everything
- `make reset-volumes`: Nuclear reset (destroys all volumes)
- `make code`: Open bash shell in backend container
- `make log`: Follow backend logs (last 50 lines)
- `make lint`: Run Ruff linting inside container

**Database Access**:
```bash
docker compose exec db psql -U perfectboulder -d perfectboulder
```

**Code Quality**:
```bash
# Linting
docker compose exec backend python -m ruff check app

# Formatting
docker compose exec backend python -m ruff format app
```

### Architecture

**Pattern: Hexagonal Architecture (Ports & Adapters)**

This backend follows hexagonal architecture to isolate business logic from technical concerns and make the system testable and adaptable.

```
backend/app/
├── domain/          # Core: Pure business entities (User, Boulder, etc.)
├── application/     # Core: Use cases / business services
├── adapters/        # Ports: External interfaces (inbound)
│   ├── rest/       # REST/HTTP transport (FastAPI routes)
│   └── graphql/    # GraphQL transport (Strawberry resolvers)
├── infra/          # Ports: Infrastructure adapters (outbound)
│   ├── database/   # Database access (SQLAlchemy, repositories)
│   └── probes/     # Health checks, monitoring
├── config.py       # Settings management (pydantic-settings)
└── main.py         # FastAPI app factory
```

**Dependency Flow** (Dependency Rule):
```
Adapters (REST/GraphQL) → Application Services → Domain Entities
       ↓
Infrastructure (DB, external APIs)
```

**Responsibilities by Layer**:

1. **domain/** - Pure business logic, NO framework dependencies
   - Entities (User, Boulder, Route, etc.)
   - Value objects (Email, Coordinates, Grade)
   - Domain exceptions (InvalidGradeError, etc.)
   - NO imports from FastAPI, SQLAlchemy, or any framework

2. **application/** - Business orchestration
   - Use cases / business services (CreateBoulderService, AuthService)
   - DTOs / request-response models (CreateBoulderRequest, BoulderResponse)
   - Application-level exceptions (BoulderNotFoundError)
   - Depends ONLY on `domain/` (not on adapters or infra)

3. **adapters/** - External entry points (inbound ports)
   - **rest/**: FastAPI routes, HTTP status codes, request/response serialization
   - **graphql/**: Strawberry schema, resolvers, GraphQL types
   - Validate input, call application services, format output
   - NO business logic here (delegate to application layer)

4. **infra/** - External dependencies (outbound ports)
   - **database/**: SQLAlchemy models, repositories, DB connection
   - **probes/**: Health checks, readiness checks
   - Implementations of interfaces defined in application layer (if any)

**Key Architectural Rules**:

- **Inward dependency**: Outer layers depend on inner layers, NEVER the reverse
  - `adapters/` and `infra/` can import from `application/` and `domain/`
  - `application/` can import from `domain/` only
  - `domain/` imports nothing from the project (pure Python)

- **No business logic in adapters**: Routes and resolvers orchestrate only
  - Validate HTTP/GraphQL input
  - Call application service
  - Format and return response
  - NO calculations, NO business rules, NO database calls

- **Application services are framework-agnostic**: Can be tested without FastAPI
  - Accept plain Python objects (dataclasses, Pydantic models)
  - Return plain Python objects
  - NO FastAPI Request/Response objects in application layer

- **Settings**: Centralized via `app/config.py` using `pydantic-settings`
  - Supports `DATABASE_URL` OR discrete vars (`DB_HOST`, `DB_USER`, etc.)
  - Settings cached via `@lru_cache` in `get_settings()`
  - Injected as dependency where needed (not imported globally)

- **Database check on startup**: `ensure_database_ready()` runs in FastAPI startup event

**API Endpoints** (when running `make start`):
- Health check: http://localhost:8000/health
- GraphQL: http://localhost:8000/graphql
- API docs: http://localhost:8000/docs

### Backend-Specific Rules

**API Conventions** (from `.agent/backend.md`):
- Explicit HTTP status codes (400/401/403/404/500)
- Structured JSON responses: `{"error": bool, "message": str, "data": optional}`
- Protected write routes require JWT + role checks (admin)

**Database**:
- Verify DB connection at boot (fail explicitly if unavailable)
- Support `DATABASE_URL` OR discrete variables
- Migrations run at container startup (entrypoint) or via dedicated command

**Security Practices**:
- Auth tokens: Read `Authorization: Bearer <token>`, reject cleanly if absent/invalid
- Uploads: Sanitize filenames (unicode → ascii), size limits (e.g., 5MB), allowlist mime types
- Don't expose sensitive files via static routes; only serve `/uploads`

**Configuration** (see `backend/.env.example`):

| Variable       | Default          | Description                      |
|----------------|------------------|----------------------------------|
| `APP_NAME`     | `Backend API`    | Application name                 |
| `DATABASE_URL` | _empty_          | Full PostgreSQL URL (priority)   |
| `DB_HOST`      | `db`             | Database host                    |
| `DB_PORT`      | `5432`           | Database port                    |
| `DB_USER`      | `perfectboulder` | Database user                    |
| `DB_PASSWORD`  | `perfectboulder` | Database password                |
| `DB_NAME`      | `perfectboulder` | Database name                    |

---

## Mobile Service (`mobile/`)

React Native mobile application using Expo.

**Stack**: React Native 0.81.5, Expo SDK ~54, TypeScript (strict mode)

### Commands

All commands executed from `mobile/` directory:

**Development**:
- `npm start`: Start Expo dev server
- `npm run ios`: Run on iOS simulator
- `npm run android`: Run on Android emulator
- `npm run web`: Run on web browser

**Code Quality**:
- `npx tsc --noEmit`: TypeScript type checking
- `npx eslint .`: Lint code (when configured)
- `npx expo-doctor`: Check project health

**Build** (requires EAS):
- `eas build --platform ios`: Build for iOS
- `eas build --platform android`: Build for Android

### Architecture

**Pattern: Clean Layered Architecture with Repository Pattern**

The mobile app follows a layered architecture separating UI from business logic and data access, ensuring testability and maintainability.

```
mobile/src/
├── screens/          # UI Layer: Screen components (BoulderListScreen, etc.)
├── components/       # UI Layer: Reusable UI components (Button, Card, etc.)
├── navigation/       # UI Layer: Navigation config (React Navigation)
├── services/         # Business Logic Layer: Domain logic & orchestration
├── repositories/     # Data Access Layer: API communication (BoulderRepository)
├── contexts/         # State Management: Global state (AuthContext, ThemeContext)
├── hooks/            # Reusable Logic: Custom hooks (useAuth, useBoulders)
├── constants/        # Configuration: Theme (colors, spacing, typography)
└── types/            # TypeScript: Type definitions & interfaces
```

**Dependency Flow**:
```
Screens/Components → Services → Repositories → HTTP Client
       ↓                ↓
    Hooks           Contexts
```

**Responsibilities by Layer**:

1. **screens/** - Screen-level UI components
   - One screen = one navigation route (BoulderListScreen, LoginScreen)
   - Compose smaller components from `components/`
   - Use hooks and contexts for state and logic
   - NO direct API calls (use repositories via services/hooks)
   - NO business logic (delegate to services)

2. **components/** - Reusable UI building blocks
   - Pure presentational components (Button, Input, Card, LoadingSpinner)
   - Receive props, render UI, emit events
   - NO direct state management (use props/callbacks)
   - NO API calls, NO business logic

3. **navigation/** - Navigation configuration
   - React Navigation setup (Stack, Tab, Drawer navigators)
   - Type-safe navigation params (using TypeScript)
   - Route definitions and screen mappings

4. **services/** - Business logic & orchestration
   - Domain logic that doesn't belong in UI (validation, calculations, orchestration)
   - Coordinate multiple repositories if needed
   - Transform repository data for UI consumption
   - Example: `BoulderService.createBoulder()` validates input, calls repository, handles errors

5. **repositories/** - Data access abstraction
   - ONE repository per resource (BoulderRepository, UserRepository, AuthRepository)
   - Encapsulate ALL API calls for that resource
   - Use centralized HTTP client (NO direct fetch/axios calls)
   - Transform API responses to domain models/types
   - Handle HTTP errors and return normalized results

6. **contexts/** - Global state management
   - React Context API for app-wide state (AuthContext, ThemeContext, LoadingContext)
   - Provide state + actions to component tree
   - Examples: current user, auth token, theme mode, global loading state

7. **hooks/** - Reusable logic
   - Custom hooks encapsulating common patterns (useAuth, useBoulders, useForm)
   - Combine contexts, state, and side effects
   - Example: `useBoulders()` fetches boulders, manages loading/error state, returns data

8. **constants/** - Configuration & theme
   - Theme constants: colors, spacing, typography, dimensions
   - NO magic values in components (always reference constants)
   - Example: `Colors.primary`, `Spacing.medium`, `Typography.heading1`

9. **types/** - TypeScript definitions
   - Type definitions for domain models (Boulder, User, Route)
   - API request/response types
   - Navigation param types
   - Shared interfaces and enums

**Key Architectural Rules**:

- **Centralized HTTP client**: ALL network calls go through ONE HTTP client instance
  - Client configured in one place (base URL, headers, interceptors)
  - Repositories use this client (NO scattered fetch/axios imports)
  - Client handles auth token injection, error normalization, loading state

- **Repository Pattern**: One resource = one repository
  - `BoulderRepository` handles ALL boulder-related API calls
  - `UserRepository` handles ALL user-related API calls
  - NO API calls outside repositories (screens/components must use repositories)

- **Separation of concerns**:
  - UI components: Render and events ONLY
  - Services: Business logic and orchestration
  - Repositories: Data access and API communication
  - Hooks: Reusable stateful logic
  - Contexts: Global state

- **Type safety**: Use TypeScript strictly
  - All components, services, repositories fully typed
  - Navigation params typed (React Navigation TypeScript support)
  - NO `any` types (use `unknown` if truly needed, then narrow)

- **Theming**: NO hardcoded values in UI code
  - Colors: `Colors.primary` not `"#007AFF"`
  - Spacing: `Spacing.medium` not `16`
  - Typography: `Typography.heading1` not `fontSize: 24`

- **Global loading state**: Driven by HTTP client
  - HTTP client updates LoadingContext on request start/end
  - UI shows global loader based on context
  - NO scattered loading states (centralized pattern)

### Mobile-Specific Rules (from `.agent/frontend.md`)

**Architecture**:
- **Network access ONLY via centralized HTTP client** (no scattered fetch/axios calls)
- Data access via repository/adapter layer (one resource = one module)
- Global loading state driven by HTTP client + UI (Loader) via global store/context
- Centralized navigation with type-safe params

**UI/UX Conventions**:
- Reuse theme constants (colors, spacing, typography) - no magic values
- Forms in edit mode: File/image fields must be **optional** (no forced re-upload)
- Consistent feedback: Success/error displayed uniformly (same toasts/patterns)
- Consistency: Input sizes, button placement, labels aligned across screens
- Test on both iOS and Android for UI changes
- Use SafeAreaView for device compatibility

**HTTP/Errors**:
- `baseURL` from environment variable (no hardcoding)
- Normalize errors to UI-consumable format (user message, network cases)
- Don't duplicate auth/header logic - centralize in client/repo

**Configuration** (see `mobile/.env.example`):

| Variable              | Description              | Default                |
|-----------------------|--------------------------|------------------------|
| `EXPO_PUBLIC_API_URL` | Backend API URL          | `http://localhost:8000`|
| `EXPO_PUBLIC_ENV`     | Environment (dev/prod)   | `development`          |

---

## Infrastructure & Deployment

### CI/CD Principles (from `.agent/infra.md`)

- **Separate dev/prod pipelines** (`develop` vs `main` branches)
- **Artifact-based deployment** (archive), not build-on-server
- **Tests required before deployment** (minimum: unit tests + coverage)
- **Backup before deployment** (previous build, `.env`), then atomic deployment

### Environment Variables & Secrets

- All runtime config via environment variables (GitHub env vars + `.env` on server/compose)
- **Never transfer/commit `.env`** from repo - generate/inject via CI
- Maintain `.env.example`/`.env.template` + variable documentation

### Nginx (when added)

- Reverse proxy with strict DEV/PROD separation (distinct domains, ports, paths)
- `/api/`: Proxy to upstream, rewrite to remove prefix, standard timeouts/headers, explicit CORS + OPTIONS handling
- `/uploads/`: Alias to backend uploads folder, caching + script execution blocking
- Security: Headers (XFO, nosniff, referrer policy), rate limiting (API + login), bot protection, block dotfiles/config files
- Validation: Config test in CI (sandbox) + on server before reload
- Deployment: Script with safeguards (refuse dev/prod mixing) + backup + graceful reload

---

## Documentation (`/docs`)

The `/docs` folder contains all product, technical, and architectural documentation.

### Structure

```
docs/
├── README.md           # Documentation index
├── features/           # Feature specifications
├── flows/              # UX flows and user journeys
│   └── video.md       # Video capture/upload/playback
├── pages/              # Page/screen wireframes
├── technical/          # Technical specifications
├── design/             # Design system & UI components
├── architecture/       # System architecture & data models
└── decisions/          # ADR (Architecture Decision Records)
```

### Documentation Agent

A specialized agent is available for documentation tasks. See `.agent/doc-agent.md` for the complete contract.

**Commands** (when using doc agent context):
- `/doc create <type> <name>` - Create new documentation
- `/doc update <file>` - Update existing documentation
- `/doc review` - Review all docs for completeness
- `/doc index` - Regenerate README index
- `/doc search <term>` - Search across all docs

**Usage**:
```
# Load doc agent context
cat .agent/doc-agent.md

# Then request documentation work
"Create feature documentation for croix tracking"
"Update the video flow with new wireframes"
"Review all MVP documentation"
```

**When to use Doc Agent**:
- Creating feature specs or user stories
- Documenting UX flows or user journeys
- Writing ADRs (Architecture Decision Records)
- Maintaining design system documentation
- Generating architecture diagrams or data models

**Integration with BMAD**:
- Scrum Master (`/sm`) creates user stories → Doc agent creates feature docs
- Architect designs system → Doc agent creates architecture docs
- Developer implements → Doc agent documents technical choices

### Documentation Standards

**Naming**:
- Use kebab-case: `croix-tracking.md`
- Be descriptive: `post-session-flow.md`, not `flow1.md`
- Use prefixes for ADRs: `001-backend-fastapi.md`

**Structure**:
- Clear title (# Title)
- Status emoji: 🔴 Todo, 🟡 In Progress, ✅ Complete
- Consistent heading levels
- Visual aids (diagrams, wireframes, tables)

**Maintenance**:
- Update docs BEFORE coding (design-first)
- Cross-reference related docs
- Keep `/docs/README.md` index updated
- Add "Last updated" timestamps

See `/docs/README.md` for complete documentation guidelines and templates.
