## MAPA DE RUTAS BACKEND - APLICACIÓN SOCIAL DE LECTORES

### Descripción General

Este documento define todas las rutas de la API REST del backend, organizadas por módulos funcionales. La API sigue los principios RESTful con autenticación JWT y códigos de respuesta HTTP estándar.

**Base URL:** `https://api.booksocial.com/v1`

**Autenticación:** Bearer Token (JWT) en header `Authorization: Bearer <token>`

---

## 📋 ÍNDICE DE MÓDULOS

1. [Autenticación y Autorización](#1-autenticación-y-autorización)
2. [Gestión de Usuarios](#2-gestión-de-usuarios)
3. [Catálogo de Libros](#3-catálogo-de-libros)
4. [Sistema de Reseñas](#4-sistema-de-reseñas)
5. [Clubes de Lectura](#5-clubes-de-lectura)
6. [Notificaciones](#6-notificaciones)
7. [Administración](#7-administración)
8. [Recursos Auxiliares](#8-recursos-auxiliares)

---

## 1. AUTENTICACIÓN Y AUTORIZACIÓN

### **Prefix:** `/auth`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `POST` | `/register` | Registro de nuevo usuario | ❌ | - |
| `POST` | `/login` | Inicio de sesión | ❌ | - |
| `POST` | `/refresh` | Renovar token de acceso | ❌ | - |
| `POST` | `/logout` | Cerrar sesión | ✅ | ALL |
| `POST` | `/forgot-password` | Solicitar recuperación de contraseña | ❌ | - |
| `POST` | `/reset-password` | Restablecer contraseña | ❌ | - |
| `GET` | `/verify-email/{token}` | Verificar email de usuario | ❌ | - |
| `POST` | `/change-password` | Cambiar contraseña actual | ✅ | ALL |
| `POST` | `/validate-token` | Validar token actual | ✅ | ALL |

#### **Ejemplos de Request/Response:**

**POST /auth/register**
```json
// Request Body
{
  "username": "reader123",
  "email": "reader@example.com",
  "password": "SecurePass123!",
  "firstName": "Juan",
  "lastName": "Pérez",
  "birthDate": "1990-05-15"
}

// Response 201 Created
{
  "message": "Usuario registrado exitosamente",
  "user": {
    "userId": 123,
    "username": "reader123",
    "email": "reader@example.com",
    "firstName": "Juan",
    "lastName": "Pérez",
    "emailVerified": false
  }
}
```

**POST /auth/login**
```json
// Request Body
{
  "usernameOrEmail": "reader123",
  "password": "SecurePass123!"
}

// Response 200 OK
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "def50200abc123...",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "user": {
    "userId": 123,
    "username": "reader123",
    "role": "USER"
  }
}
```

---

## 2. GESTIÓN DE USUARIOS

### **Prefix:** `/users`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `GET` | `/profile` | Obtener perfil del usuario actual | ✅ | ALL |
| `PUT` | `/profile` | Actualizar perfil del usuario actual | ✅ | ALL |
| `GET` | `/profile/{userId}` | Obtener perfil de otro usuario | ✅ | ALL |
| `POST` | `/follow/{userId}` | Seguir a un usuario | ✅ | ALL |
| `DELETE` | `/follow/{userId}` | Dejar de seguir a un usuario | ✅ | ALL |
| `GET` | `/followers` | Obtener lista de seguidores | ✅ | ALL |
| `GET` | `/following` | Obtener lista de seguidos | ✅ | ALL |
| `GET` | `/search` | Buscar usuarios | ✅ | ALL |
| `GET` | `/stats` | Estadísticas del usuario actual | ✅ | ALL |
| `PUT` | `/privacy` | Actualizar configuración de privacidad | ✅ | ALL |
| `DELETE` | `/account` | Desactivar cuenta | ✅ | ALL |

#### **Ejemplos de Request/Response:**

**GET /users/profile**
```json
// Response 200 OK
{
  "userId": 123,
  "username": "reader123",
  "email": "reader@example.com",
  "firstName": "Juan",
  "lastName": "Pérez",
  "bio": "Amante de la ciencia ficción y fantasía",
  "profilePicture": "https://cdn.booksocial.com/profiles/123.jpg",
  "location": "Madrid, España",
  "privacyLevel": "PUBLIC",
  "followersCount": 45,
  "followingCount": 32,
  "reviewsCount": 28,
  "clubsCount": 3,
  "createdAt": "2024-01-15T10:30:00Z"
}
```

**GET /users/search?q=juan&page=0&size=10**
```json
// Response 200 OK
{
  "content": [
    {
      "userId": 123,
      "username": "reader123",
      "firstName": "Juan",
      "lastName": "Pérez",
      "profilePicture": "https://cdn.booksocial.com/profiles/123.jpg",
      "followersCount": 45,
      "isFollowing": false
    }
  ],
  "totalElements": 1,
  "totalPages": 1,
  "size": 10,
  "number": 0
}
```

---

## 3. CATÁLOGO DE LIBROS

### **Prefix:** `/books`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `POST` | `/` | Crear nuevo libro | ✅ | USER+ |
| `GET` | `/` | Obtener lista de libros con filtros | ✅ | ALL |
| `GET` | `/{bookId}` | Obtener detalles de un libro | ✅ | ALL |
| `PUT` | `/{bookId}` | Actualizar información de libro | ✅ | MODERATOR+ |
| `DELETE` | `/{bookId}` | Eliminar libro | ✅ | ADMIN |
| `GET` | `/search` | Búsqueda avanzada de libros | ✅ | ALL |
| `GET` | `/recommendations` | Obtener recomendaciones personalizadas | ✅ | ALL |
| `GET` | `/trending` | Obtener libros en tendencia | ✅ | ALL |
| `GET` | `/recent` | Obtener libros recién agregados | ✅ | ALL |
| `GET` | `/top-rated` | Obtener libros mejor calificados | ✅ | ALL |
| `GET` | `/genres` | Obtener lista de géneros | ✅ | ALL |
| `GET` | `/authors` | Obtener lista de autores | ✅ | ALL |
| `GET` | `/publishers` | Obtener lista de editoriales | ✅ | ALL |

#### **Ejemplos de Request/Response:**

**POST /books**
```json
// Request Body
{
  "isbn": "978-84-376-0494-7",
  "title": "Cien años de soledad",
  "subtitle": "",
  "description": "La obra maestra de Gabriel García Márquez...",
  "coverImage": "https://cdn.booksocial.com/covers/cien-anos.jpg",
  "publicationDate": "1967-06-05",
  "pageCount": 432,
  "language": "es",
  "publisherId": 15,
  "authorIds": [25],
  "genreIds": [1, 8]
}

// Response 201 Created
{
  "bookId": 456,
  "isbn": "978-84-376-0494-7",
  "title": "Cien años de soledad",
  "averageRating": 0.0,
  "totalRatings": 0,
  "status": "ACTIVE",
  "createdAt": "2025-08-23T23:03:53Z"
}
```

---

## 4. SISTEMA DE RESEÑAS

### **Prefix:** `/reviews`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `POST` | `/` | Crear nueva reseña | ✅ | ALL |
| `GET` | `/` | Obtener reseñas con filtros | ✅ | ALL |
| `GET` | `/{reviewId}` | Obtener detalles de una reseña | ✅ | ALL |
| `PUT` | `/{reviewId}` | Actualizar reseña propia | ✅ | ALL |
| `DELETE` | `/{reviewId}` | Eliminar reseña | ✅ | ALL |
| `POST` | `/{reviewId}/like` | Dar like a una reseña | ✅ | ALL |
| `DELETE` | `/{reviewId}/like` | Quitar like de una reseña | ✅ | ALL |
| `GET` | `/{reviewId}/comments` | Obtener comentarios de una reseña | ✅ | ALL |
| `POST` | `/{reviewId}/comments` | Comentar en una reseña | ✅ | ALL |
| `PUT` | `/comments/{commentId}` | Actualizar comentario propio | ✅ | ALL |
| `DELETE` | `/comments/{commentId}` | Eliminar comentario | ✅ | ALL |
| `GET` | `/book/{bookId}` | Obtener reseñas de un libro | ✅ | ALL |
| `GET` | `/user/{userId}` | Obtener reseñas de un usuario | ✅ | ALL |
| `PUT` | `/{reviewId}/moderate` | Moderar reseña | ✅ | MODERATOR+ |

---

## 5. CLUBES DE LECTURA

### **Prefix:** `/clubs`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `POST` | `/` | Crear nuevo club | ✅ | ALL |
| `GET` | `/` | Obtener lista de clubes | ✅ | ALL |
| `GET` | `/{clubId}` | Obtener detalles de un club | ✅ | ALL |
| `PUT` | `/{clubId}` | Actualizar información del club | ✅ | ADMIN_CLUB |
| `DELETE` | `/{clubId}` | Eliminar club | ✅ | ADMIN_CLUB |
| `POST` | `/{clubId}/join` | Unirse a un club | ✅ | ALL |
| `DELETE` | `/{clubId}/leave` | Abandonar un club | ✅ | ALL |
| `GET` | `/{clubId}/members` | Obtener miembros del club | ✅ | MEMBER+ |
| `PUT` | `/{clubId}/members/{userId}/role` | Cambiar rol de miembro | ✅ | MODERATOR_CLUB+ |
| `DELETE` | `/{clubId}/members/{userId}` | Expulsar miembro | ✅ | MODERATOR_CLUB+ |
| `GET` | `/{clubId}/readings` | Obtener lecturas del club | ✅ | MEMBER+ |
| `POST` | `/{clubId}/readings` | Crear nueva lectura grupal | ✅ | MODERATOR_CLUB+ |
| `PUT` | `/readings/{readingId}` | Actualizar lectura | ✅ | MODERATOR_CLUB+ |
| `POST` | `/readings/{readingId}/join` | Unirse a una lectura | ✅ | MEMBER+ |
| `PUT` | `/readings/{readingId}/progress` | Actualizar progreso de lectura | ✅ | MEMBER+ |
| `GET` | `/my-clubs` | Obtener clubes del usuario actual | ✅ | ALL |
| `GET` | `/search` | Buscar clubes | ✅ | ALL |

---

## 6. NOTIFICACIONES

### **Prefix:** `/notifications`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `GET` | `/` | Obtener notificaciones del usuario | ✅ | ALL |
| `PUT` | `/{notificationId}/read` | Marcar notificación como leída | ✅ | ALL |
| `PUT` | `/mark-all-read` | Marcar todas como leídas | ✅ | ALL |
| `DELETE` | `/{notificationId}` | Eliminar notificación | ✅ | ALL |
| `GET` | `/unread-count` | Obtener cantidad de no leídas | ✅ | ALL |
| `PUT` | `/settings` | Actualizar preferencias de notificaciones | ✅ | ALL |
| `GET` | `/settings` | Obtener preferencias de notificaciones | ✅ | ALL |

---

## 7. ADMINISTRACIÓN

### **Prefix:** `/admin`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `GET` | `/users` | Obtener lista de usuarios | ✅ | ADMIN |
| `PUT` | `/users/{userId}/role` | Cambiar rol de usuario | ✅ | ADMIN |
| `PUT` | `/users/{userId}/status` | Activar/desactivar usuario | ✅ | ADMIN |
| `GET` | `/reports/users` | Reportes de usuarios | ✅ | ADMIN |
| `GET` | `/reports/books` | Reportes de libros | ✅ | ADMIN |
| `GET` | `/reports/reviews` | Reportes de reseñas | ✅ | ADMIN |
| `GET` | `/reports/clubs` | Reportes de clubes | ✅ | ADMIN |
| `GET` | `/moderation/reviews` | Cola de moderación de reseñas | ✅ | MODERATOR+ |
| `GET` | `/moderation/comments` | Cola de moderación de comentarios | ✅ | MODERATOR+ |
| `PUT` | `/moderation/reviews/{reviewId}` | Moderar reseña | ✅ | MODERATOR+ |
| `PUT` | `/moderation/comments/{commentId}` | Moderar comentario | ✅ | MODERATOR+ |

---

## 8. RECURSOS AUXILIARES

### **Prefix:** `/utils`

| Método | Ruta | Descripción | Auth | Roles |
|--------|------|-------------|------|-------|
| `POST` | `/upload/image` | Subir imagen | ✅ | ALL |
| `GET` | `/health` | Estado de salud de la API | ❌ | - |
| `GET` | `/version` | Versión de la API | ❌ | - |
| `GET` | `/countries` | Lista de países | ✅ | ALL |
| `GET` | `/languages` | Lista de idiomas | ✅ | ALL |

---

## 📊 CÓDIGOS DE RESPUESTA HTTP

| Código | Descripción | Uso |
|--------|-------------|-----|
| `200` | OK | Operación exitosa |
| `201` | Created | Recurso creado exitosamente |
| `204` | No Content | Operación exitosa sin contenido |
| `400` | Bad Request | Error en la solicitud |
| `401` | Unauthorized | No autenticado |
| `403` | Forbidden | Sin permisos |
| `404` | Not Found | Recurso no encontrado |
| `409` | Conflict | Conflicto (ej: email duplicado) |
| `422` | Unprocessable Entity | Error de validación |
| `429` | Too Many Requests | Límite de rate limiting |
| `500` | Internal Server Error | Error interno del servidor |

---

## 🔐 SISTEMA DE AUTORIZACIÓN

### Roles del Sistema:
- **USER**: Usuario básico registrado
- **MODERATOR**: Moderador de contenido
- **ADMIN**: Administrador del sistema

### Roles de Club:
- **MEMBER**: Miembro básico del club
- **MODERATOR_CLUB**: Moderador del club
- **ADMIN_CLUB**: Administrador del club

### Headers Requeridos:
```
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

### Rate Limiting:
- **Usuarios autenticados**: 1000 requests/hora
- **Usuarios no autenticados**: 100 requests/hora
- **Operaciones de escritura**: 200 requests/hora