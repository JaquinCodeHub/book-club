## MAPA DE RUTAS FRONTEND - APLICACIÓN SOCIAL DE LECTORES

### Descripción General

Este documento define la estructura de rutas del frontend de la aplicación, organizada por módulos funcionales. La aplicación utiliza React Router para navegación SPA (Single Page Application) con protección de rutas y navegación condicional según roles de usuario.

**Base URL:** `https://booksocial.com`

**Framework:** React + React Router v6

**Autenticación:** Context API + Protected Routes

---

## 📋 ÍNDICE DE MÓDULOS

1. [Rutas Públicas](#1-rutas-públicas)
2. [Autenticación](#2-autenticación)
3. [Dashboard Principal](#3-dashboard-principal)
4. [Perfil de Usuario](#4-perfil-de-usuario)
5. [Catálogo de Libros](#5-catálogo-de-libros)
6. [Sistema de Reseñas](#6-sistema-de-reseñas)
7. [Clubes de Lectura](#7-clubes-de-lectura)
8. [Configuraciones](#8-configuraciones)
9. [Administración](#9-administración)
10. [Rutas de Error](#10-rutas-de-error)

---

## 1. RUTAS PÚBLICAS

### **Sin autenticación requerida**

| Ruta | Componente | Descripción | Meta Tags |
|------|------------|-------------|-----------|
| `/` | `LandingPage` | Página de inicio pública | Home - BookSocial |
| `/about` | `AboutPage` | Acerca de la plataforma | Sobre Nosotros |
| `/privacy` | `PrivacyPage` | Política de privacidad | Política de Privacidad |
| `/terms` | `TermsPage` | Términos y condiciones | Términos de Uso |
| `/help` | `HelpPage` | Centro de ayuda | Centro de Ayuda |
| `/contact` | `ContactPage` | Página de contacto | Contacto |

#### **Estructura de Landing:**
```
/ (LandingPage)
├── Header Navigation
├── Hero Section
├── Features Overview
├── Popular Books Preview
├── Testimonials
├── CTA Registration
└── Footer
```

---

## 2. AUTENTICACIÓN

### **Rutas de acceso y registro**

| Ruta | Componente | Descripción | Redirección |
|------|------------|-------------|-------------|
| `/login` | `LoginPage` | Inicio de sesión | → `/dashboard` |
| `/register` | `RegisterPage` | Registro de usuario | → `/verify-email` |
| `/verify-email` | `EmailVerificationPage` | Verificación de email | → `/welcome` |
| `/welcome` | `WelcomePage` | Bienvenida post-registro | → `/dashboard` |
| `/forgot-password` | `ForgotPasswordPage` | Recuperación de contraseña | → `/login` |
| `/reset-password/:token` | `ResetPasswordPage` | Restablecer contraseña | → `/login` |
| `/logout` | `LogoutHandler` | Cerrar sesión | → `/` |

#### **Flujo de Autenticación:**
```
/register → /verify-email → /welcome → /dashboard
     ↓              ↓           ↓         ↓
[Form]    →    [Token]   →   [Setup]  → [Home]
```

---

## 3. DASHBOARD PRINCIPAL

### **Área principal autenticada**

| Ruta | Componente | Descripción | Sidebar |
|------|------------|-------------|---------|
| `/dashboard` | `DashboardPage` | Panel principal del usuario | ✅ |
| `/dashboard/feed` | `FeedPage` | Feed de actividades | ✅ |
| `/dashboard/recommendations` | `RecommendationsPage` | Recomendaciones personalizadas | ✅ |
| `/dashboard/trending` | `TrendingPage` | Contenido en tendencia | ✅ |
| `/dashboard/notifications` | `NotificationsPage` | Centro de notificaciones | ✅ |

#### **Layout del Dashboard:**
```
/dashboard
├── TopNavigation (search, notifications, profile)
├── Sidebar Navigation
│   ├── Feed
│   ├── Mi Biblioteca
│   ├── Mis Clubes
│   ├── Recomendaciones
│   └── Configuración
└── Main Content Area
    ├── Welcome Section
    ├── Recent Activity
    ├── Reading Progress
    ├── Popular Books
    └── Club Updates
```

---

## 4. PERFIL DE USUARIO

### **Gestión de perfiles**

| Ruta | Componente | Descripción | Permisos |
|------|------------|-------------|----------|
| `/profile` | `MyProfilePage` | Mi perfil (redirect a /profile/me) | Owner |
| `/profile/me` | `ProfilePage` | Mi perfil completo | Owner |
| `/profile/me/edit` | `EditProfilePage` | Editar mi perfil | Owner |
| `/profile/me/library` | `MyLibraryPage` | Mi biblioteca personal | Owner |
| `/profile/me/lists` | `ReadingListsPage` | Mis listas de lectura | Owner |
| `/profile/me/reviews` | `MyReviewsPage` | Mis reseñas | Owner |
| `/profile/me/followers` | `MyFollowersPage` | Mis seguidores | Owner |
| `/profile/me/following` | `MyFollowingPage` | A quién sigo | Owner |
| `/profile/:userId` | `UserProfilePage` | Perfil de otro usuario | Public/Private |
| `/profile/:userId/reviews` | `UserReviewsPage` | Reseñas de usuario | Public |
| `/profile/:userId/lists` | `UserListsPage` | Listas públicas de usuario | Public |

#### **Estructura de Mi Biblioteca:**
```
/profile/me/library
├── Reading Status Tabs
│   ├── Currently Reading
│   ├── Want to Read
│   ├── Read
│   └── Did Not Finish
├── Filter & Sort Options
├── View Options (Grid/List)
└── Book Cards/Rows
```

---

## 5. CATÁLOGO DE LIBROS

### **Exploración y gestión de libros**

| Ruta | Componente | Descripción | Filtros |
|------|------------|-------------|---------|
| `/books` | `BooksPage` | Catálogo principal de libros | ✅ |
| `/books/search` | `BookSearchPage` | Búsqueda avanzada | ✅ |
| `/books/genres` | `GenresPage` | Explorar por géneros | ✅ |
| `/books/genres/:genreId` | `GenreBooksPage` | Libros de un género | ✅ |
| `/books/authors` | `AuthorsPage` | Directorio de autores | ✅ |
| `/books/authors/:authorId` | `AuthorPage` | Perfil de autor y sus libros | ✅ |
| `/books/publishers` | `PublishersPage` | Directorio de editoriales | ✅ |
| `/books/publishers/:publisherId` | `PublisherPage` | Perfil de editorial | ✅ |
| `/books/new` | `NewBooksPage` | Libros recién agregados | ✅ |
| `/books/trending` | `TrendingBooksPage` | Libros en tendencia | ✅ |
| `/books/top-rated` | `TopRatedBooksPage` | Mejor calificados | ✅ |
| `/books/:bookId` | `BookDetailsPage` | Detalles completos del libro | - |
| `/books/:bookId/reviews` | `BookReviewsPage` | Reseñas del libro | ✅ |
| `/books/add` | `AddBookPage` | Agregar nuevo libro | USER+ |
| `/books/:bookId/edit` | `EditBookPage` | Editar libro | MODERATOR+ |

#### **Filtros de Búsqueda:**
```
/books/search?
├── q=título_autor_isbn
├── genre=género_id
├── author=autor_id
├── publisher=editorial_id
├── year_from=año_inicio
├── year_to=año_fin
├── rating_min=calificación_mínima
├── page_count_min=páginas_mínimas
├── page_count_max=páginas_máximas
├── language=idioma
├── sort=criterio_orden
└── page=número_página
```

---

## 6. SISTEMA DE RESEÑAS

### **Gestión de reseñas y comentarios**

| Ruta | Componente | Descripción | Permisos |
|------|------------|-------------|----------|
| `/reviews` | `ReviewsPage` | Feed global de reseñas | ALL |
| `/reviews/recent` | `RecentReviewsPage` | Reseñas recientes | ALL |
| `/reviews/popular` | `PopularReviewsPage` | Reseñas populares | ALL |
| `/reviews/following` | `FollowingReviewsPage` | Reseñas de seguidos | ALL |
| `/reviews/:reviewId` | `ReviewDetailsPage` | Detalles de reseña | ALL |
| `/reviews/:reviewId/edit` | `EditReviewPage` | Editar reseña | Owner |
| `/reviews/new/:bookId` | `WriteReviewPage` | Escribir nueva reseña | ALL |
| `/reviews/drafts` | `DraftReviewsPage` | Mis borradores | Owner |

#### **Estructura de Reseña:**
```
/reviews/:reviewId
├── Review Header
│   ├── User Info
│   ├── Book Info
│   ├── Rating Stars
│   └── Action Buttons
├── Review Content
│   ├── Title
│   ├── Content (with spoiler warning)
│   └── Tags
├── Engagement Section
│   ├── Like/Unlike Button
│   ├── Share Options
│   └── Report Button
└── Comments Section
    ├── Comment Form
    └── Comments List
```

---

## 7. CLUBES DE LECTURA

### **Comunidades y lecturas grupales**

| Ruta | Componente | Descripción | Permisos |
|------|------------|-------------|----------|
| `/clubs` | `ClubsPage` | Directorio de clubes | ALL |
| `/clubs/search` | `ClubSearchPage` | Búsqueda de clubes | ALL |
| `/clubs/my-clubs` | `MyClubsPage` | Mis clubes | ALL |
| `/clubs/create` | `CreateClubPage` | Crear nuevo club | ALL |
| `/clubs/:clubId` | `ClubDetailsPage` | Detalles del club | MEMBER+ |
| `/clubs/:clubId/edit` | `EditClubPage` | Editar club | ADMIN_CLUB |
| `/clubs/:clubId/members` | `ClubMembersPage` | Miembros del club | MEMBER+ |
| `/clubs/:clubId/readings` | `ClubReadingsPage` | Lecturas del club | MEMBER+ |
| `/clubs/:clubId/readings/create` | `CreateReadingPage` | Nueva lectura grupal | MODERATOR_CLUB+ |
| `/clubs/:clubId/readings/:readingId` | `ReadingDetailsPage` | Detalles de lectura | MEMBER+ |
| `/clubs/:clubId/discussions` | `ClubDiscussionsPage` | Discusiones del club | MEMBER+ |
| `/clubs/:clubId/join` | `JoinClubPage` | Solicitud de unión | ALL |

#### **Dashboard de Club:**
```
/clubs/:clubId
├── Club Header
│   ├── Club Info
│   ├── Member Count
│   └── Join/Leave Button
├── Navigation Tabs
│   ├── Overview
│   ├── Current Reading
│   ├── Reading History
│   ├── Members
│   └── Discussions
└── Content Area
    ├── Current Reading Progress
    ├── Recent Activity
    ├── Upcoming Readings
    └── Member Highlights
```

---

## 8. CONFIGURACIONES

### **Preferencias y configuración**

| Ruta | Componente | Descripción | Permisos |
|------|------------|-------------|----------|
| `/settings` | `SettingsPage` | Panel de configuración | Owner |
| `/settings/profile` | `ProfileSettingsPage` | Configuración de perfil | Owner |
| `/settings/privacy` | `PrivacySettingsPage` | Configuración de privacidad | Owner |
| `/settings/notifications` | `NotificationSettingsPage` | Preferencias de notificaciones | Owner |
| `/settings/account` | `AccountSettingsPage` | Configuración de cuenta | Owner |
| `/settings/security` | `SecuritySettingsPage` | Seguridad y contraseña | Owner |
| `/settings/preferences` | `PreferencesPage` | Preferencias de lectura | Owner |

#### **Configuraciones Disponibles:**
```
/settings
├── Profile Settings
│   ├── Basic Info
│   ├── Profile Picture
│   ├── Bio & Interests
│   └── Location
├── Privacy Settings
│   ├── Profile Visibility
│   ├── Reading Lists Visibility
│   ├── Activity Visibility
│   └── Contact Preferences
├── Notification Settings
│   ├── Email Notifications
│   ├── Push Notifications
│   ├── In-App Notifications
│   └── Frequency Settings
└── Account Settings
    ├── Change Password
    ├── Two-Factor Auth
    ├── Connected Accounts
    └── Delete Account
```

---

## 9. ADMINISTRACIÓN

### **Panel de administración**

| Ruta | Componente | Descripción | Permisos |
|------|------------|-------------|----------|
| `/admin` | `AdminDashboardPage` | Panel de administración | MODERATOR+ |
| `/admin/users` | `AdminUsersPage` | Gestión de usuarios | ADMIN |
| `/admin/users/:userId` | `AdminUserDetailsPage` | Detalles de usuario | ADMIN |
| `/admin/books` | `AdminBooksPage` | Gestión de libros | MODERATOR+ |
| `/admin/reviews` | `AdminReviewsPage` | Moderación de reseñas | MODERATOR+ |
| `/admin/reviews/:reviewId` | `AdminReviewDetailsPage` | Detalles de reseña | MODERATOR+ |
| `/admin/clubs` | `AdminClubsPage` | Gestión de clubes | MODERATOR+ |
| `/admin/reports` | `AdminReportsPage` | Reportes y analytics | ADMIN |
| `/admin/moderation` | `ModerationQueuePage` | Cola de moderación | MODERATOR+ |
| `/admin/settings` | `AdminSettingsPage` | Configuración del sistema | ADMIN |

---

## 10. RUTAS DE ERROR

### **Manejo de errores**

| Ruta | Componente | Descripción | Código |
|------|------------|-------------|--------|
| `/404` | `NotFoundPage` | Página no encontrada | 404 |
| `/403` | `ForbiddenPage` | Acceso denegado | 403 |
| `/500` | `ServerErrorPage` | Error del servidor | 500 |
| `/maintenance` | `MaintenancePage` | Modo mantenimiento | 503 |
| `*` | `NotFoundPage` | Catch-all para rutas inválidas | 404 |

---

## 🛡️ PROTECCIÓN DE RUTAS

### **Guards y Middlewares:**

```typescript
// Route Protection Types
enum RouteGuard {
  PUBLIC = 'public',           // Acceso libre
  AUTH_REQUIRED = 'auth',      // Login requerido  
  VERIFIED_EMAIL = 'verified', // Email verificado
  ROLE_USER = 'user',         // Rol USER+
  ROLE_MODERATOR = 'mod',     // Rol MODERATOR+
  ROLE_ADMIN = 'admin',       // Rol ADMIN
  CLUB_MEMBER = 'club_member', // Miembro del club
  CLUB_MODERATOR = 'club_mod', // Moderador del club
  CLUB_ADMIN = 'club_admin',   // Admin del club
  RESOURCE_OWNER = 'owner'     // Propietario del recurso
}
```

### **Estructura de Protección:**
```
App Router
├── Public Routes (/, /about, /login, etc.)
├── Auth Required Routes (/dashboard/*)
│   ├── Email Verified Routes (/profile/*)
│   ├── Role-Based Routes (/admin/*)
│   └── Resource Owner Routes (/profile/me/*)
└── Dynamic Permission Routes (/clubs/:id/*)
```

---

## 🧭 NAVEGACIÓN CONTEXTUAL

### **Breadcrumbs Examples:**
```
Home > Books > Science Fiction > Dune
Home > Profile > My Library > Currently Reading
Home > Clubs > My Clubs > Sci-Fi Readers > Current Reading
Home > Admin > Users > User Details
```

### **Tab Navigation:**
```
Profile Tabs: Overview | Reviews | Lists | Following | Followers
Book Tabs: Details | Reviews | Reading Lists | Discussions
Club Tabs: Overview | Current Reading | Members | Discussions | Settings
```

---

## 📱 RESPONSIVE BEHAVIOR

### **Mobile Navigation:**
- Hamburger menu para rutas principales
- Bottom tab bar para funciones frecuentes
- Drawer navigation para configuraciones
- Swipe gestures para navegación entre tabs

### **Tablet Navigation:**
- Sidebar colapsible
- Dual-pane layouts para listas/detalles
- Touch-optimized interactions

### **Desktop Navigation:**
- Full sidebar navigation
- Hover states y tooltips
- Keyboard shortcuts
- Multi-column layouts