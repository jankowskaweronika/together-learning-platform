# Platforma Kursowa - Specyfikacja Techniczna MVP

## Spis treści

1. [Opis projektu](#opis-projektu)
2. [Funkcjonalności MVP](#funkcjonalności-mvp)
3. [Stack Technologiczny](#stack-technologiczny)
4. [Architektura](#architektura)
5. [Struktura Projektu](#struktura-projektu)
6. [Schemat Bazy Danych](#schemat-bazy-danych)
7. [Hosting & Infrastruktura](#hosting--infrastruktura)
8. [Koszty](#koszty)
9. [Plan Wdrożenia](#plan-wdrożenia)

---

## Opis projektu

Platforma e-learningowa umożliwiająca tworzenie i konsumpcję kursów online z funkcją budowania społeczności. System składa się z panelu administracyjnego, panelu instruktora oraz interfejsu użytkownika.

### Główne cele:

- Umożliwienie tworzenia i zarządzania kursami online
- Śledzenie postępów uczniów
- Budowanie społeczności wokół kursów
- Skalowalność - przygotowanie do multi-tenant (wielu instruktorów)

---

## Funkcjonalności MVP

### 1. System użytkowników

- ✅ Rejestracja i logowanie
- ✅ Profile użytkowników (avatar, bio, statystyki)
- ✅ Role: Admin, Instruktor, User
- ✅ Zarządzanie kontem

### 2. Kursy

- ✅ Przeglądanie dostępnych kursów
- ✅ Struktura: Kurs → Rozdziały → Lekcje
- ✅ Zapisy na kursy (enrollment)
- ✅ Status kursu: draft/published/archived
- ✅ Zarządzanie kursami (tworzenie, edycja, usuwanie)

### 3. Lekcje

- ✅ Wideo w lekcjach
- ✅ Treść tekstowa/markdown
- ✅ Notatki prywatne (widoczne tylko dla użytkownika)
- ✅ Komentarze publiczne (widoczne dla społeczności)
- ✅ Możliwość ukończenia/pominięcia lekcji

**Zasady ukończenia lekcji (MVP):**

- Lekcja posiada status `draft` lub `published`; tylko opublikowane pojawiają się w kursie.
- Ukończenie jest możliwe po obejrzeniu całego wideo lub oznaczeniu manualnym.
- Pominięcie lekcji nie podnosi progresu, ale pozwala przejść do kolejnej; UI wyraźnie oznacza pominięte pozycje.
- Postęp kursu wyliczany jest jako `ukończone lekcje / liczba lekcji publikowanych`; pominięte liczą się jako nieukończone.

### 4. Tracker postępów

- ✅ Pasek postępu kursu (np. 45% ukończone)
- ✅ Historia ukończonych lekcji
- ✅ Dashboard użytkownika z przeglądem wszystkich kursów
- ✅ Lista "Moje kursy" vs "Dostępne kursy"

### 5. Wyszukiwarka i filtrowanie

- ✅ Szukanie kursów po nazwie/kategorii
- ✅ Filtrowanie kursów
- ✅ Sortowanie (popularność, data, ocena)

### 6. Oceny i recenzje

- ✅ Ocena kursu (1-5 gwiazdek)
- ✅ Recenzja tekstowa
- ✅ Wyświetlanie średniej oceny

### 7. Społeczność

- ✅ Tworzenie postów/dyskusji
- ✅ Komentarze do postów
- ✅ Upvotes/likes (opcjonalnie)
- ✅ Przeglądanie aktywności społeczności

**Zakres społeczności (MVP):**

- Globalny feed (chronologiczny) z możliwością filtrowania po kursie.
- Post może być przypięty do konkretnego kursu lub mieć charakter ogólny; w modelu `Post` dodajemy opcjonalne `courseId`.
- Moderacja: flagowanie treści przez użytkowników, soft-delete przez admina/instruktora.
- W roadmapie: reakcje, tagi i kanały dedykowane kursom.

### 8. Powiadomienia

- ✅ Nowa odpowiedź na komentarz
- ✅ Nowa lekcja w zapisanym kursie
- ✅ Real-time notifications (Socket.io)

### 9. Panel Admina/Instruktora

- ✅ Zarządzanie użytkownikami
- ✅ Tworzenie i edycja kursów
- ✅ Statystyki (liczba użytkowników, ukończenia)
- ✅ Moderacja komentarzy

### Priorytety MVP vs kolejne iteracje

| Obszar        | MVP (obowiązkowe)                                                          | Po MVP                                                  |
| ------------- | -------------------------------------------------------------------------- | ------------------------------------------------------- |
| Kursy         | Lista kursów, enrollment, status ukończenia, podstawowe statystyki         | Płatności, wieloautorzy, certyfikaty                    |
| Lekcje        | Wideo, treść tekstowa, notatki prywatne, komentarze publiczne, ukończ/omiń | Quizy, blokady progresu, live sesje                     |
| Społeczność   | Globalny feed postów + komentarze, podstawowa moderacja                    | Upvote, tagi, kanały per kurs, gamifikacja              |
| Powiadomienia | E-mail/in-app dla kluczowych zdarzeń                                       | Real-time Socket.io, granularne preferencje             |
| Admin panel   | CRUD kursów i lekcji, moderacja treści                                     | Zaawansowane analityki, raporty, zespoły instruktorskie |

---

## Role i User Stories MVP

### Użytkownik (Learner)

- Jako zalogowany użytkownik chcę zapisać się na kurs i zobaczyć go w zakładce „Moje kursy”.
- Jako użytkownik chcę oznaczać lekcje jako ukończone lub pominięte, aby śledzić progres.
- Jako użytkownik chcę dodawać notatki prywatne do lekcji i wracać do nich później.
- Jako użytkownik chcę komentować lekcje oraz odpowiadać na komentarze innych.
- Jako użytkownik chcę śledzić globalny feed społeczności i brać udział w dyskusjach.

### Instruktor

- Jako instruktor chcę tworzyć kursy, rozdziały i lekcje w panelu, pozostawiając je w statusie draft do czasu publikacji.
- Jako instruktor chcę podejrzeć statystyki kursu (enrollment, ukończenia) w panelu.
- Jako instruktor chcę moderować komentarze i posty, aby utrzymać jakość treści.

### Admin

- Jako admin chcę zarządzać użytkownikami (blokowanie, zmiana ról) i przypisywać role instruktora.
- Jako admin chcę mieć pełny widok aktywności społeczności oraz możliwość ukrycia/oznaczenia treści.
- Jako admin chcę mieć wgląd w logi publikacji kursów i lekcji.

---

## Stack Technologiczny

### Frontend

- **Next.js 14** (App Router) + **TypeScript**
  - SSR/SSG dla lepszego SEO
  - Server Components
  - Image Optimization
- **MUI (Material-UI v5)** - komponenty UI
- **Redux Toolkit + RTK Query** - globalny stan i cache zapytań
- **React Hook Form** + **Zod** - formularze i walidacja
- **Axios** - HTTP client
- **next-auth** - autentykacja po stronie klienta

### Backend

- **Node.js** + **Express** + **TypeScript**
- **PostgreSQL** - baza danych relacyjna
- **Prisma** - ORM (Type-safe, migracje)
- **JWT** - tokeny dostępu i refresh
- **bcrypt** - hashowanie haseł
- **Socket.io** - powiadomienia real-time
- **Multer** + **Supabase Storage SDK** - upload plików (REST)
- **Express Validator** - walidacja requestów
- **Redis** (opcjonalnie) - cache, sessions, rate limiting

### Video Hosting

- **YouTube (unlisted)** - darmowy hosting wideo (tymczasowo)

### Dodatkowe narzędzia

- **ESLint** + **Prettier** - formatowanie kodu
- **Jest** + **React Testing Library** - testy
- **GitHub Actions** - CI/CD
- **Sentry** - error tracking (opcjonalnie)

---

## Architektura

```
┌─────────────────────────────────────────┐
│  Frontend: Next.js 14 (App Router)      │
│  Hosted: Vercel                         │
│  ├─ Server Components (SEO)             │
│  ├─ Client Components (interaktywność)  │
│  ├─ MUI Components                      │
│  ├─ Redux Toolkit + RTK Query (data)    │
│  ├─ Redux Toolkit (global state)        │
│  └─ NextAuth (session handling)         │
└──────────────┬──────────────────────────┘
               │
               │ REST API + WebSocket
               │
┌──────────────▼──────────────────────────┐
│  Backend API: Node.js + Express         │
│  Hosted: Railway/Render                 │
│  ├─ REST endpoints (/api/v1/...)        │
│  ├─ JWT auth middleware                 │
│  ├─ Prisma ORM                          │
│  ├─ Socket.io server                    │
│  ├─ File upload handling                │
│  └─ Business logic                      │
└──────────────┬──────────────────────────┘
               │
               ├──────────► PostgreSQL (Neon/Railway)
               │            - Dane użytkowników
               │            - Kursy, lekcje
               │            - Postępy, komentarze
               │
               ├──────────► Redis (opcjonalnie)
               │            - Cache
               │            - Rate limiting
               │
               ├──────────► Supabase Storage
               │            - Avatary użytkowników
               │            - Pliki do pobrania
               │
               └──────────► YouTube Data API
                            - Upload/zarządzanie wideo
                            - Streaming (embed)
```

### Przepływ danych:

1. **Autentykacja:**
   - User loguje się → Backend zwraca JWT (access + refresh token)
   - Frontend przechowuje tokeny (httpOnly cookies)
   - Każdy request zawiera access token w headerze

2. **Pobieranie kursów:**
   - Next.js SSR → Prefetch danych na serwerze
   - Client hydration → RTK Query cache w Reduxie
   - Optymistyczne updaty UI

3. **Real-time powiadomienia:**
   - Socket.io connection przy logowaniu
   - Backend emituje events (new_comment, new_lesson)
   - Frontend słucha i aktualizuje UI

---

## Struktura Projektu

Kod utrzymujemy w dwóch repozytoriach: `together-learning-platform` (Next.js) oraz `together-learning-platform-backend` (Express + Prisma). Poniższy schemat pokazuje logiczny podział katalogów wewnątrz każdego z nich.

```
/platform-kursowa
│
├── /frontend                      # Next.js Application
│   ├── /src
│   │   ├── /app                   # App Router (Next.js 14)
│   │   │   ├── /(auth)           # Auth group
│   │   │   │   ├── /login
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── /register
│   │   │   │   │   └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   │
│   │   │   ├── /(dashboard)      # Protected routes
│   │   │   │   ├── /courses
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── /my-courses
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── /community
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── /profile
│   │   │   │   │   └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   │
│   │   │   ├── /course/[courseId]
│   │   │   │   ├── page.tsx
│   │   │   │   └── /chapter/[chapterId]/lesson/[lessonId]
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── /admin            # Admin panel
│   │   │   │   ├── /courses
│   │   │   │   ├── /users
│   │   │   │   └── /analytics
│   │   │   │
│   │   │   ├── layout.tsx        # Root layout
│   │   │   ├── page.tsx          # Landing page
│   │   │   └── globals.css
│   │   │
│   │   ├── /components
│   │   │   ├── /ui               # MUI wrappers/custom components
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Card.tsx
│   │   │   │   └── Modal.tsx
│   │   │   ├── /layout
│   │   │   │   ├── Navbar.tsx
│   │   │   │   ├── Sidebar.tsx
│   │   │   │   └── Footer.tsx
│   │   │   ├── /courses
│   │   │   │   ├── CourseCard.tsx
│   │   │   │   ├── CourseList.tsx
│   │   │   │   └── CourseProgress.tsx
│   │   │   ├── /lessons
│   │   │   │   ├── VideoPlayer.tsx
│   │   │   │   ├── LessonContent.tsx
│   │   │   │   ├── Notes.tsx
│   │   │   │   └── Comments.tsx
│   │   │   ├── /community
│   │   │   │   ├── PostCard.tsx
│   │   │   │   ├── PostList.tsx
│   │   │   │   └── CreatePost.tsx
│   │   │   └── /admin
│   │   │       ├── CourseForm.tsx
│   │   │       └── UserManagement.tsx
│   │   │
│   │   ├── /lib
│   │   │   ├── api.ts            # Axios instance + interceptors
│   │   │   ├── auth.ts           # NextAuth configuration
│   │   │   ├── store.ts          # Redux store + RTK Query setup
│   │   │   └── socket.ts         # Socket.io client
│   │   │
│   │   ├── /hooks
│   │   │   ├── useAuth.ts
│   │   │   ├── useCourses.ts
│   │   │   ├── useLessons.ts
│   │   │   └── useNotifications.ts
│   │   │
│   │   ├── /store                # Redux slices
│   │   │   ├── authSlice.ts
│   │   │   ├── uiSlice.ts
│   │   │   └── notificationSlice.ts
│   │   │
│   │   ├── /types
│   │   │   ├── user.ts
│   │   │   ├── course.ts
│   │   │   ├── lesson.ts
│   │   │   └── api.ts
│   │   │
│   │   └── /utils
│   │       ├── formatters.ts
│   │       └── validators.ts
│   │
│   ├── /public
│   │   ├── /images
│   │   └── /icons
│   │
│   ├── .env.local
│   ├── next.config.js
│   ├── package.json
│   ├── tsconfig.json
│   └── README.md
│
└── /backend                       # Node.js + Express API
    ├── /src
    │   ├── /controllers
    │   │   ├── authController.ts
    │   │   ├── userController.ts
    │   │   ├── courseController.ts
    │   │   ├── chapterController.ts
    │   │   ├── lessonController.ts
    │   │   ├── enrollmentController.ts
    │   │   ├── progressController.ts
    │   │   ├── commentController.ts
    │   │   ├── reviewController.ts
    │   │   └── communityController.ts
    │   │
    │   ├── /routes
    │   │   ├── auth.routes.ts
    │   │   ├── user.routes.ts
    │   │   ├── course.routes.ts
    │   │   ├── lesson.routes.ts
    │   │   ├── enrollment.routes.ts
    │   │   ├── progress.routes.ts
    │   │   ├── comment.routes.ts
    │   │   ├── review.routes.ts
    │   │   ├── community.routes.ts
    │   │   └── index.ts
    │   │
    │   ├── /middleware
    │   │   ├── auth.middleware.ts      # JWT verification
    │   │   ├── role.middleware.ts      # Role-based access
    │   │   ├── validation.middleware.ts
    │   │   ├── errorHandler.ts
    │   │   ├── rateLimiter.ts
    │   │   └── upload.middleware.ts    # Multer config
    │   │
    │   ├── /services
    │   │   ├── authService.ts
    │   │   ├── userService.ts
    │   │   ├── courseService.ts
    │   │   ├── lessonService.ts
    │   │   ├── enrollmentService.ts
    │   │   ├── progressService.ts
    │   │   ├── commentService.ts
    │   │   ├── reviewService.ts
    │   │   ├── communityService.ts
    │   │   ├── emailService.ts
    │   │   ├── storageService.ts       # Supabase uploads
    │   │   └── notificationService.ts
    │   │
    │   ├── /prisma
    │   │   ├── schema.prisma
    │   │   ├── seed.ts
    │   │   └── /migrations
    │   │
    │   ├── /utils
    │   │   ├── jwt.ts
    │   │   ├── hash.ts
    │   │   ├── validators.ts
    │   │   └── logger.ts
    │   │
    │   ├── /config
    │   │   ├── database.ts
    │   │   ├── aws.ts
    │   │   ├── redis.ts
    │   │   └── env.ts
    │   │
    │   ├── /types
    │   │   ├── express.d.ts
    │   │   └── models.ts
    │   │
    │   ├── /socket
    │   │   ├── index.ts
    │   │   └── handlers.ts
    │   │
    │   ├── app.ts                # Express app setup
    │   └── server.ts             # Server entry point
    │
    ├── .env
    ├── .env.example
    ├── package.json
    ├── tsconfig.json
    └── README.md
```

---

## Schemat Bazy Danych

### Prisma Schema

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// ============================================
// USER MANAGEMENT
// ============================================

model User {
  id            String    @id @default(uuid())
  email         String    @unique
  password      String
  firstName     String?
  lastName      String?
  avatar        String?
  bio           String?   @db.Text
  role          Role      @default(USER)
  isActive      Boolean   @default(true)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  // Relations
  courses       Course[]  @relation("CourseAuthor")
  enrollments   Enrollment[]
  progress      UserProgress[]
  comments      Comment[]
  notes         Note[]
  reviews       Review[]
  posts         Post[]
  replies       Reply[]
  notifications Notification[]

  @@index([email])
  @@map("users")
}

enum Role {
  USER
  INSTRUCTOR
  ADMIN
}

// ============================================
// COURSE STRUCTURE
// ============================================

model Course {
  id            String        @id @default(uuid())
  title         String
  description   String        @db.Text
  thumbnail     String?
  status        CourseStatus  @default(DRAFT)
  price         Float         @default(0)
  level         CourseLevel?
  duration      Int?          // w minutach
  authorId      String
  author        User          @relation("CourseAuthor", fields: [authorId], references: [id])
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
  publishedAt   DateTime?

  // Relations
  chapters      Chapter[]
  enrollments   Enrollment[]
  reviews       Review[]
  categories    CourseCategory[]

  @@index([authorId])
  @@index([status])
  @@map("courses")
}

enum CourseStatus {
  DRAFT
  PUBLISHED
  ARCHIVED
}

enum CourseLevel {
  BEGINNER
  INTERMEDIATE
  ADVANCED
}

model Category {
  id          String   @id @default(uuid())
  name        String   @unique
  slug        String   @unique
  description String?

  courses     CourseCategory[]

  @@map("categories")
}

model CourseCategory {
  courseId    String
  categoryId  String

  course      Course   @relation(fields: [courseId], references: [id], onDelete: Cascade)
  category    Category @relation(fields: [categoryId], references: [id], onDelete: Cascade)

  @@id([courseId, categoryId])
  @@map("course_categories")
}

model Chapter {
  id          String    @id @default(uuid())
  title       String
  description String?   @db.Text
  order       Int
  courseId    String
  course      Course    @relation(fields: [courseId], references: [id], onDelete: Cascade)
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt

  // Relations
  lessons     Lesson[]

  @@index([courseId])
  @@map("chapters")
}

model Lesson {
  id            String    @id @default(uuid())
  title         String
  content       String?   @db.Text
  videoUrl      String?
  videoDuration Int?      // w sekundach
  order         Int
  isFree        Boolean   @default(false)
  chapterId     String
  chapter       Chapter   @relation(fields: [chapterId], references: [id], onDelete: Cascade)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  // Relations
  progress      UserProgress[]
  comments      Comment[]
  notes         Note[]

  @@index([chapterId])
  @@map("lessons")
}

// ============================================
// USER PROGRESS & ENROLLMENT
// ============================================

model Enrollment {
  id            String    @id @default(uuid())
  userId        String
  courseId      String
  enrolledAt    DateTime  @default(now())
  completedAt   DateTime?

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  course        Course    @relation(fields: [courseId], references: [id], onDelete: Cascade)

  @@unique([userId, courseId])
  @@index([userId])
  @@index([courseId])
  @@map("enrollments")
}

model UserProgress {
  id            String    @id @default(uuid())
  userId        String
  lessonId      String
  completed     Boolean   @default(false)
  completedAt   DateTime?
  lastWatchedAt DateTime  @default(now())
  watchTime     Int       @default(0) // w sekundach

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  lesson        Lesson    @relation(fields: [lessonId], references: [id], onDelete: Cascade)

  @@unique([userId, lessonId])
  @@index([userId])
  @@index([lessonId])
  @@map("user_progress")
}

// ============================================
// NOTES & COMMENTS
// ============================================

model Note {
  id            String    @id @default(uuid())
  content       String    @db.Text
  timestamp     Int?      // timestamp w wideo (sekundy)
  userId        String
  lessonId      String
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  lesson        Lesson    @relation(fields: [lessonId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@index([lessonId])
  @@map("notes")
}

model Comment {
  id            String    @id @default(uuid())
  content       String    @db.Text
  userId        String
  lessonId      String
  parentId      String?   // dla odpowiedzi na komentarze
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  lesson        Lesson    @relation(fields: [lessonId], references: [id], onDelete: Cascade)
  parent        Comment?  @relation("CommentReplies", fields: [parentId], references: [id])
  replies       Comment[] @relation("CommentReplies")

  @@index([userId])
  @@index([lessonId])
  @@index([parentId])
  @@map("comments")
}

// ============================================
// REVIEWS
// ============================================

model Review {
  id            String    @id @default(uuid())
  rating        Int       // 1-5
  comment       String?   @db.Text
  userId        String
  courseId      String
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  course        Course    @relation(fields: [courseId], references: [id], onDelete: Cascade)

  @@unique([userId, courseId])
  @@index([courseId])
  @@map("reviews")
}

// ============================================
// COMMUNITY
// ============================================

model Post {
  id            String    @id @default(uuid())
  title         String
  content       String    @db.Text
  authorId      String
  isPinned      Boolean   @default(false)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  author        User      @relation(fields: [authorId], references: [id], onDelete: Cascade)
  replies       Reply[]

  @@index([authorId])
  @@map("posts")
}

model Reply {
  id            String    @id @default(uuid())
  content       String    @db.Text
  postId        String
  authorId      String
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  post          Post      @relation(fields: [postId], references: [id], onDelete: Cascade)
  author        User      @relation(fields: [authorId], references: [id], onDelete: Cascade)

  @@index([postId])
  @@index([authorId])
  @@map("replies")
}

// ============================================
// NOTIFICATIONS
// ============================================

model Notification {
  id            String    @id @default(uuid())
  type          NotificationType
  title         String
  message       String
  userId        String
  isRead        Boolean   @default(false)
  relatedId     String?   // ID powiązanego obiektu (comment, post, etc)
  createdAt     DateTime  @default(now())

  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@index([isRead])
  @@map("notifications")
}

enum NotificationType {
  NEW_COMMENT
  NEW_REPLY
  NEW_LESSON
  COURSE_COMPLETED
  NEW_POST
}
```

### Relacje między tabelami:

```
User (1) ──────> (N) Course (jako autor)
User (1) ──────> (N) Enrollment (zapisy na kursy)
User (1) ──────> (N) UserProgress (postępy)
User (1) ──────> (N) Comment, Note, Review, Post

Course (1) ────> (N) Chapter
Chapter (1) ───> (N) Lesson
Course (1) ────> (N) Enrollment
Course (1) ────> (N) Review

Lesson (1) ────> (N) UserProgress
Lesson (1) ────> (N) Comment
Lesson (1) ────> (N) Note

Post (1) ──────> (N) Reply
```

---

## Hosting & Infrastruktura

### Frontend (Next.js)

**Vercel Hobby (free)** - Polecane

- ✅ Bezproblemowa integracja z Next.js
- ✅ Automatyczny deploy z Git
- ✅ Edge Functions
- ✅ Image Optimization
- ✅ Analytics wbudowane
- 💸 $0 przy założeniach MVP

**Alternatywy (również free tier):**

- Netlify Free
- Cloudflare Pages

### Backend (Node.js + Express)

**Render Free Tier** - Polecane ⭐

- ✅ Darmowe dyno (sleep po 15 min idle, akceptowalne w MVP)
- ✅ Auto-deploy z GitHub
- ✅ Obsługa cron/background jobs (limitowana)
- ✅ SSL + custom domains
- 💸 $0 (wymaga miesięcznego odświeżenia)

**Alternatywy (free):**

- Railway Free (200 godzin/mies.)
- Fly.io Free Allowance

### Database

**Neon Free Tier** - Polecane dla MVP ⭐

- ✅ Serverless PostgreSQL z 0.5GB storage
- ✅ Branching (dev/staging/prod)
- ✅ Connection pooling (PgBouncer)
- 💸 $0 w limicie free

**Alternatywy:**

- Supabase Postgres Free (0.5GB)
- Railway Postgres Free

### Storage (Pliki, Avatary)

**Supabase Storage Free**

- ✅ 1 GB storage + 2 GB transfer
- ✅ REST API + podpisane URL-e
- ✅ CDN przez Supabase Edge
- 💸 $0 na start

**Alternatywy (free):**

- Cloudinary Free (assets + transforms)
- Firebase Storage (Spark plan)

### Video Hosting

**YouTube (unlisted) – free**

- ✅ Darmowy upload + hosting
- ✅ Automatyczne transkodowanie i napisy
- ✅ Player osadzany w lekcjach
- ⚠️ Widoczny branding + słabsza kontrola dostępu (akceptowalne w MVP)

**Alternatywy (płatne, na później):**

- Vimeo Plus / Pro
- Cloudflare Stream

### Cache (Opcjonalnie)

**Redis** - Upstash Free

- 💸 10,000 requests/day (wystarczające na MVP)

---

## Wymagania niefunkcjonalne

- **Dostępność:** 99% miesięcznie (maks. ~7h niedostępności); health-check na backendzie.
- **Wydajność:** TTFB < 300ms dla głównej listy kursów; bundle < 250kB JS na landing po tree-shakingu.
- **Skalowalność:** Prisma migracje automatycznie uruchamiane w CI + backup bazy 1x dziennie (Neon branch snapshot).
- **Monitoring:** Alerty Sentry dla error rate > 1% oraz logowanie akcji admina (moderacja, publikacja).
- **Observability:** Podstawowe metryki (CPU, RAM) z Railway + dashboard w Grafana/Logtail (opcjonalnie).
- **Dane:** Zgodność z RODO – endpointy eksportu i usuwania konta (MVP: ręczna obsługa, udokumentowany proces).

---

## Koszty

### MVP - Miesięczne koszty (szacunkowe)

| Usługa            | Plan (Free)      | Koszt       |
| ----------------- | ---------------- | ----------- |
| Vercel            | Hobby            | $0          |
| Render            | Free Web Service | $0          |
| Neon Database     | Free tier        | $0          |
| Supabase Storage  | Free tier        | $0          |
| YouTube (hosting) | Unlisted         | $0          |
| Upstash Redis     | Free plan        | $0          |
| **TOTAL**         |                  | **$0/mies** |

### Po przekroczeniu limitów (plan na przyszłość)

Na potrzeby MVP zakładamy pozostanie w darmowych progach. Po zwiększeniu ruchu należy dobrać płatne plany (np. Vercel Pro, Render Plus, Vimeo/Cloudflare Stream). Koszty zostaną oszacowane po przekroczeniu limitów free.

---

## Plan Wdrożenia

### Faza 1: Setup projektu (Tydzień 1)

- [ ] Setup dwóch repozytoriów: `together-learning-platform` (Next.js) i `together-learning-platform-backend` (Express/Prisma)
- [ ] Konfiguracja Next.js + TypeScript
- [ ] Konfiguracja Express + TypeScript
- [ ] Setup Prisma + PostgreSQL
- [ ] Podstawowa struktura folderów
- [ ] ESLint + Prettier
- [ ] Git hooks (Husky)

### Faza 2: Autentykacja (Tydzień 2)

- [ ] Model User w Prisma
- [ ] Rejestracja (backend endpoint)
- [ ] Logowanie + JWT
- [ ] Middleware auth
- [ ] Strony login/register (frontend)
- [ ] NextAuth integration
- [ ] Protected routes

### Faza 3: Kursy - podstawy (Tydzień 3)

- [ ] Modele: Course, Chapter, Lesson
- [ ] CRUD API dla kursów (admin)
- [ ] Strona listy kursów
- [ ] Strona szczegółów kursu
- [ ] Upload thumbnails (Supabase Storage)
- [ ] Integracja z YouTube API (upload + osadzanie)

### Faza 4: System lekcji (Tydzień 4)

- [ ] Strona lekcji z video playerem
- [ ] Model UserProgress
- [ ] Śledzenie postępów (ukończone/nieukończone)
- [ ] Notatki prywatne
- [ ] Komentarze publiczne
- [ ] Nawigacja między lekcjami

### Faza 5: Enrollment & Dashboard (Tydzień 5)

- [ ] Model Enrollment
- [ ] Zapis na kurs
- [ ] Dashboard użytkownika
- [ ] "Moje kursy" + pasek postępu
- [ ] Statystyki (ukończone lekcje, czas nauki)

### Faza 6: Społeczność (Tydzień 6)

- [ ] Modele: Post, Reply
- [ ] CRUD API dla postów
- [ ] Strona społeczności
- [ ] Tworzenie postów
- [ ] Komentowanie
- [ ] Moderacja (admin)

### Faza 7: Oceny i powiadomienia (Tydzień 7)

- [ ] Model Review
- [ ] Wystawianie ocen
- [ ] Wyświetlanie średniej oceny
- [ ] Socket.io setup
- [ ] Real-time notifications
- [ ] Lista powiadomień

### Faza 8: Panel Admina (Tydzień 8)

- [ ] Dashboard z statystykami
- [ ] Zarządzanie użytkownikami
- [ ] Zarządzanie kursami
- [ ] Moderacja komentarzy
- [ ] Analytics (wykresy)

### Faza 9: Wyszukiwarka i optymalizacja (Tydzień 9)

- [ ] Wyszukiwarka kursów
- [ ] Filtrowanie (kategorie, poziom, ocena)
- [ ] Sortowanie
- [ ] SEO optimization (meta tags, sitemap)
- [ ] Image optimization
- [ ] Performance audit

### Faza 10: Testing & Deploy (Tydzień 10)

- [ ] Unit testy (backend)
- [ ] Integration testy
- [ ] E2E testy (Playwright/Cypress)
- [ ] Setup CI/CD (GitHub Actions)
- [ ] Deploy staging
- [ ] User testing
- [ ] Bug fixes
- [ ] Deploy production

---

## Testy i QA

- **Pokrycie:** minimum 70% linii w usługach backendowych (auth, enrollment, progress) oraz testy komponentów krytycznych (course list, lesson player).
- **Scenariusze krytyczne:** rejestracja/login, zapis na kurs, ukończenie lekcji, dodanie komentarza/posta, moderacja admina.
- **E2E:** Playwright – ścieżka ucznia (enroll → lesson → community) i instruktora (create course → publish). Uruchamiane w CI przed merge do main.
- **Manual QA:** checklista regresji dla publikacji kursu, panelu admina i feedu społeczności; raport w Notion.
- **Observability QA:** test alertów Sentry i health checków po każdym deployu.

---

## API Endpoints (Przykłady)

### Auth

```
POST   /api/v1/auth/register
POST   /api/v1/auth/login
POST   /api/v1/auth/refresh
POST   /api/v1/auth/logout
GET    /api/v1/auth/me
```

### Users

```
GET    /api/v1/users/:id
PUT    /api/v1/users/:id
DELETE /api/v1/users/:id
POST   /api/v1/users/:id/avatar
```

### Courses

```
GET    /api/v1/courses                  # Lista wszystkich
GET    /api/v1/courses/:id              # Szczegóły
POST   /api/v1/courses                  # Tworzenie (Instruktor/Admin)
PUT    /api/v1/courses/:id              # Edycja
DELETE /api/v1/courses/:id              # Usunięcie
GET    /api/v1/courses/:id/chapters     # Rozdziały kursu
POST   /api/v1/courses/:id/enroll       # Zapis na kurs
GET    /api/v1/courses/:id/progress     # Postęp użytkownika
```

### Chapters

```
POST   /api/v1/chapters
PUT    /api/v1/chapters/:id
DELETE /api/v1/chapters/:id
GET    /api/v1/chapters/:id/lessons
```

### Lessons

```
GET    /api/v1/lessons/:id
POST   /api/v1/lessons
PUT    /api/v1/lessons/:id
DELETE /api/v1/lessons/:id
POST   /api/v1/lessons/:id/complete     # Ukończenie lekcji
GET    /api/v1/lessons/:id/comments     # Komentarze
POST   /api/v1/lessons/:id/comments     # Dodaj komentarz
```

### Progress

```
GET    /api/v1/progress/courses/:courseId
POST   /api/v1/progress/lessons/:lessonId
```

### Notes

```
GET    /api/v1/notes/lessons/:lessonId
POST   /api/v1/notes
PUT    /api/v1/notes/:id
DELETE /api/v1/notes/:id
```

### Reviews

```
GET    /api/v1/reviews/courses/:courseId
POST   /api/v1/reviews
PUT    /api/v1/reviews/:id
DELETE /api/v1/reviews/:id
```

### Community

```
GET    /api/v1/posts
GET    /api/v1/posts/:id
POST   /api/v1/posts
PUT    /api/v1/posts/:id
DELETE /api/v1/posts/:id
GET    /api/v1/posts/:id/replies
POST   /api/v1/posts/:id/replies
```

### Notifications

```
GET    /api/v1/notifications
PUT    /api/v1/notifications/:id/read
DELETE /api/v1/notifications/:id
```

---

## Zmienne środowiskowe

### Frontend (.env.local)

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1
NEXT_PUBLIC_SOCKET_URL=http://localhost:5000
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-secret-key
```

### Backend (.env)

```env
# Server
PORT=5000
NODE_ENV=development

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/platform_kursowa

# JWT
JWT_SECRET=your-jwt-secret
JWT_REFRESH_SECRET=your-refresh-secret
JWT_EXPIRE=15m
JWT_REFRESH_EXPIRE=7d

# Supabase
SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=
SUPABASE_ANON_KEY=
SUPABASE_STORAGE_BUCKET=course-files

# YouTube API
YOUTUBE_CLIENT_ID=
YOUTUBE_CLIENT_SECRET=
YOUTUBE_REFRESH_TOKEN=

# Redis (optional)
REDIS_URL=

# Email (optional)
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASS=
```

---

## Bezpieczeństwo

### Backend

- ✅ Hashowanie haseł (bcrypt)
- ✅ JWT z refresh tokens
- ✅ Rate limiting (express-rate-limit)
- ✅ CORS configuration
- ✅ Helmet.js (security headers)
- ✅ Input validation (express-validator)
- ✅ SQL injection protection (Prisma)
- ✅ XSS protection
- ✅ Audit log dla akcji admina/instruktora (moderacja, publikacja kursów)
- 🔜 2FA dla adminów (TOTP) – po stabilizacji MVP

### Frontend

- ✅ HttpOnly cookies dla tokenów
- ✅ CSRF protection
- ✅ Input sanitization
- ✅ Protected routes
- ✅ Role-based access control
- ✅ Szyfrowanie notatek po stronie serwera + limit rozmiaru
- 🔜 Preferencje prywatności i eksport danych użytkownika (RODO)

---

## Monitoring & Analytics

- **Sentry** - error tracking
- **Vercel Analytics** - web vitals
- **Posthog** / **Plausible** - privacy-focused analytics
- **LogRocket** - session replay (opcjonalnie)

---

**Wersja:** 1.0  
**Data:** Listopad 2024
