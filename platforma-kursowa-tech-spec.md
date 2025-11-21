# Platforma Kursowa – Tech Spec (skrót)

## 1. Cel dokumentu

Zebrać tylko decyzje techniczne i zasady architektoniczne umożliwiające budowę MVP kursowej platformy w sposób niezależny od konkretnych dostawców oraz gotowy na podmiany komponentów (frontend/backend/infrastruktura).

## 2. Zasady architektoniczne

- **Hexagonal Architecture (Ports & Adapters)**
  - Domenowe use-case’y w warstwie aplikacyjnej (`core`) nie znają frameworków ani baz danych.
  - Zewnętrzne integracje (DB, storage, email, video, auth) implementują porty jako adaptery, co pozwala je podmieniać.
- **DDD-lite moduły domenowe**: `users`, `courses`, `lessons`, `community`, `progress`, `notifications`.
- **Komunikacja**: HTTP/REST + WebSocket event bus (Socket.io) za pośrednictwem adapterów.
- **Konfiguracja przez ENV**: Każdy adapter wstrzykuje swoje zależności poprzez interfejsy i zmienne środowiskowe.

## 3. Frontend

- **Framework**: React 18 + TypeScript, Next.js App Router (CSR + SSR).
  - Next.js widziany jako adapter UI; logika domenowa w hookach/services niezależnych od frameworka.
  - React Query (TanStack) izolowane w module `infrastructure/ui-data`.
- **UI System**: MUI v5 + własna warstwa `ui-kit`.
- **Storybook**:
  - Wymagany katalog `src/stories` + `*.stories.tsx` obok komponentów.
  - Automatyczne snapshoty + kontrakty wizualne, integracja z Chromatic (opcjonalnie).
  - Każdy komponent eksportowany z `ui-kit` musi mieć story oraz test wizualny.
- **Zasada hexagonalna w FE**:
  - `core` (typy, serwisy domenowe) → `application` (hooks, state machines) → `infrastructure` (API klient, socket, storage).
  - UI komponenty konsumują tylko `application` łagodząc zależność od backendu.

## 4. Backend

- **Runtime**: Node.js + TypeScript.
- **Warstwy**:
  1. `domain`: agnostyczne modele, serwisy, polityki.
  2. `application`: use-case’y (command/query), porty dla repozytoriów, notyfikacji, plików.
  3. `infrastructure`: Express adapter dla HTTP, Prisma repo adapter, Supabase storage adapter, YouTube adapter, Redis adapter.
  4. `interface`: REST/WS kontrolery mapujące DTO ↔ domena.
- **Ports** (przykłady): `CourseRepository`, `LessonContentService`, `VideoProvider`, `NotificationGateway`, `AuthService`.
- **Adapters**:
  - `PrismaCourseRepository` (Postgres/Neon)
  - `SupabaseFileStorageAdapter`
  - `YouTubeVideoAdapter` (upload + metadata)
  - `SocketIoNotificationGateway`

## 5. Testy i jakość

- **Unit tests** na warstwie domain/application (Jest).
- **Contract tests** dla adapterów (np. `CourseRepository.contract.test.ts`).
- **Storybook visual regression** + Playwright E2E dla ścieżek: enrollment, lesson complete, community post.

## 6. Dev Experience

- **Repozytoria**:
  - Utrzymujemy dwa niezależne repozytoria: `together-learning-platform` (Next.js App Router + MUI + Storybook) oraz `together-learning-platform-backend` (Express + Prisma + Socket.io).
  - Każde repo zachowuje hexagonalny podział (`domain/application/infrastructure/interface`), osobne wersjonowanie i pipeline CI dopasowany do stacku.
- **Współdzielone artefakty**:
  - Backend publikuje kontrakt OpenAPI (`/contracts/platforma.json`) oraz paczkę typów (`@platforma/api-types` w prywatnym registry). Frontend konsumuje je jako zależności z automatycznym sprawdzaniem zgodności w CI.
  - Design tokens i komponenty UI dostarczane są przez Storybook w repo frontu; w razie potrzeby eksportujemy `@platforma/ui-kit` jako osobną paczkę.
- **Husky hooks**: lint-staged (ESLint, Stylelint) + testy krytyczne w każdym repo.
- **CI (GitHub Actions)**: `together-learning-platform` – lint + unit + Storybook build + E2E smoke; `together-learning-platform-backend` – lint + unit + API E2E smoke + publikacja kontraktów.

## 7. Infrastrukturę można podmienić

- Storage: port `FileStoragePort` → dziś Supabase, jutro S3/R2.
- Video: `VideoProviderPort` → YouTube adapter vs Vimeo adapter.
- Auth: `AuthProviderPort` → Bcrypt/JWT vs zewnętrzny IdP (Clerk/Auth0).
- Deployment: front (Vercel) i backend (Render) jako adaptery; można przenieść na DO/Fly/Heroku przy zachowaniu kontenerów.
- Konfiguracja budowana w `.env` per środowisko, loader w `packages/config`.

## 8. Storybook & Design System Checklist

- Storybook uruchamiany z `pnpm storybook`.
- Każdy komponent UI ma:
  - plik `.stories.tsx`,
  - test wizualny (Chromatic/Locally),
  - doc block opisujący propsy i powiązane use-case’y.
- Theming i tokeny (`theme/tokens.ts`) współdzielone między aplikacją i Storybookiem.

## 9. Roadmapa techniczna (skrócona)

1. Utworzenie repozytoriów `together-learning-platform` i `together-learning-platform-backend` + bazowych pakietów (`ui-kit`, kontrakty API).
2. Implementacja portów domenowych + mock adapters (w pamięci).
3. Budowa Storybooka i podstaw UI (Autoryzacja, Course Card, Lesson Player).
4. Adaptery produkcyjne (Prisma, Supabase, YouTube).
5. Testy kontraktowe + E2E i pipeline CI.
6. Hardening (observability, rate limits, caching).

## 10. Kluczowe decyzje niezależności

- Biznesowa logika i kontrakty wersjonowane w `packages/core`.
- Każdy adapter implementuje wspólny interfejs i jest rejestrowany w kontenerze DI (np. Awilix).
- Frameworki (Next.js, Express) mogą zostać wymienione (np. na Remix / Fastify) bez ruszania `domain` i `application`.
- Frontend komunikuje się wyłącznie przez REST/WS kontrakty opisane w OpenAPI, co ułatwia przyszłe klienckie aplikacje (mobile).
