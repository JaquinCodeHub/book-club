## CASOS DE USO - APLICACIÓN SOCIAL DE LECTORES

### Descripción General

Este documento define los casos de uso principales del sistema, organizados por actores y funcionalidades. Cada caso incluye precondiciones, flujo principal, flujos alternativos y postcondiciones para garantizar una implementación completa y robusta.

**Fecha de Documento:** 2025-08-23

**Actores del Sistema:**
- **Usuario Anónimo**: Visitante sin cuenta
- **Usuario Registrado**: Usuario autenticado básico
- **Moderador**: Usuario con permisos de moderación
- **Administrador**: Usuario con permisos administrativos completos

---

## 📋 ÍNDICE DE CASOS DE USO

1. [Gestión de Usuarios](#1-gestión-de-usuarios)
2. [Autenticación y Autorización](#2-autenticación-y-autorización)
3. [Gestión de Libros](#3-gestión-de-libros)
4. [Sistema de Reseñas](#4-sistema-de-reseñas)
5. [Funcionalidades Sociales](#5-funcionalidades-sociales)
6. [Clubes de Lectura](#6-clubes-de-lectura)
7. [Sistema de Notificaciones](#7-sistema-de-notificaciones)
8. [Administración del Sistema](#8-administración-del-sistema)

---

## 1. GESTIÓN DE USUARIOS

### **CU-001: Registro de Usuario**

**Actor Principal:** Usuario Anónimo

**Objetivo:** Crear una nueva cuenta en la plataforma

**Precondiciones:**
- El usuario no tiene una cuenta existente
- El email no está registrado en el sistema

**Flujo Principal:**
1. El usuario accede a la página de registro
2. El sistema muestra el formulario de registro
3. El usuario completa los campos obligatorios:
   - Username (único)
   - Email (único)
   - Contraseña (mínimo 8 caracteres)
   - Nombre
   - Apellido
   - Fecha de nacimiento
4. El usuario acepta términos y condiciones
5. El usuario envía el formulario
6. El sistema valida los datos ingresados
7. El sistema crea la cuenta con estado "no verificado"
8. El sistema envía email de verificación
9. El sistema muestra mensaje de confirmación

**Flujos Alternativos:**
- **A1 - Email duplicado:** Sistema muestra error, usuario debe usar otro email
- **A2 - Username duplicado:** Sistema sugiere usernames disponibles
- **A3 - Contraseña débil:** Sistema muestra criterios de seguridad
- **A4 - Datos inválidos:** Sistema resalta campos con errores

**Postcondiciones:**
- Nueva cuenta creada en estado "no verificado"
- Email de verificación enviado
- Usuario redirigido a página de verificación

---

### **CU-002: Verificación de Email**

**Actor Principal:** Usuario Registrado (no verificado)

**Objetivo:** Verificar la dirección de email para activar la cuenta

**Precondiciones:**
- Usuario tiene cuenta registrada no verificada
- Email de verificación fue enviado

**Flujo Principal:**
1. El usuario recibe email con enlace de verificación
2. El usuario hace clic en el enlace
3. El sistema valida el token de verificación
4. El sistema marca el email como verificado
5. El sistema activa la cuenta del usuario
6. El sistema muestra mensaje de bienvenida
7. El usuario es redirigido al dashboard

**Flujos Alternativos:**
- **A1 - Token expirado:** Sistema permite reenviar email
- **A2 - Token inválido:** Sistema muestra error y opciones de ayuda
- **A3 - Cuenta ya verificada:** Sistema redirige directamente al login

**Postcondiciones:**
- Email verificado y cuenta activa
- Usuario puede acceder a todas las funcionalidades

---

### **CU-003: Actualización de Perfil**

**Actor Principal:** Usuario Registrado

**Objetivo:** Modificar información personal del perfil

**Precondiciones:**
- Usuario autenticado en el sistema
- Usuario accede a su perfil personal

**Flujo Principal:**
1. El usuario navega a configuración de perfil
2. El sistema muestra formulario con datos actuales
3. El usuario modifica los campos deseados:
   - Información básica (nombre, apellido, bio)
   - Foto de perfil
   - Ubicación
   - Configuración de privacidad
4. El usuario guarda los cambios
5. El sistema valida los nuevos datos
6. El sistema actualiza la información
7. El sistema muestra confirmación de cambios

**Flujos Alternativos:**
- **A1 - Imagen muy grande:** Sistema redimensiona automáticamente
- **A2 - Datos inválidos:** Sistema muestra errores específicos
- **A3 - Sin cambios:** Sistema informa que no hay modificaciones

**Postcondiciones:**
- Perfil actualizado con nueva información
- Cambios reflejados en toda la plataforma

---

## 2. AUTENTICACIÓN Y AUTORIZACIÓN

### **CU-004: Inicio de Sesión**

**Actor Principal:** Usuario Registrado

**Objetivo:** Acceder a la cuenta personal en la plataforma

**Precondiciones:**
- Usuario tiene cuenta verificada
- Usuario no está autenticado actualmente

**Flujo Principal:**
1. El usuario accede a la página de login
2. El sistema muestra formulario de autenticación
3. El usuario ingresa credenciales (email/username y contraseña)
4. El usuario envía el formulario
5. El sistema valida las credenciales
6. El sistema genera tokens JWT (access + refresh)
7. El sistema registra información de sesión
8. El usuario es redirigido al dashboard

**Flujos Alternativos:**
- **A1 - Credenciales incorrectas:** Sistema muestra error genérico
- **A2 - Cuenta desactivada:** Sistema informa estado y opciones
- **A3 - Email no verificado:** Sistema redirige a verificación
- **A4 - Múltiples intentos fallidos:** Sistema implementa delay progresivo

**Postcondiciones:**
- Usuario autenticado con sesión activa
- Tokens de acceso generados y almacenados
- Información de dispositivo registrada

---

### **CU-005: Recuperación de Contraseña**

**Actor Principal:** Usuario Registrado

**Objetivo:** Restablecer contraseña olvidada

**Precondiciones:**
- Usuario tiene cuenta registrada
- Usuario no recuerda su contraseña actual

**Flujo Principal:**
1. El usuario accede a "Olvidé mi contraseña"
2. El sistema solicita dirección de email
3. El usuario ingresa su email
4. El sistema valida que el email existe
5. El sistema genera token de recuperación
6. El sistema envía email con enlace de recuperación
7. El usuario hace clic en el enlace
8. El sistema valida el token
9. El usuario ingresa nueva contraseña
10. El sistema actualiza la contraseña
11. El sistema invalida todas las sesiones activas

**Flujos Alternativos:**
- **A1 - Email no registrado:** Sistema no revela información pero simula envío
- **A2 - Token expirado:** Sistema permite nueva solicitud
- **A3 - Nueva contraseña débil:** Sistema rechaza y muestra criterios

**Postcondiciones:**
- Contraseña actualizada exitosamente
- Todas las sesiones anteriores invalidadas
- Usuario debe autenticarse nuevamente

---

## 3. GESTIÓN DE LIBROS

### **CU-006: Agregar Nuevo Libro**

**Actor Principal:** Usuario Registrado

**Objetivo:** Incorporar un nuevo libro al catálogo de la plataforma

**Precondiciones:**
- Usuario autenticado con rol USER o superior
- El libro no existe previamente en el catálogo

**Flujo Principal:**
1. El usuario navega a "Agregar Libro"
2. El sistema muestra formulario de creación
3. El usuario completa información del libro:
   - ISBN (opcional)
   - Título y subtítulo
   - Descripción/sinopsis
   - Fecha de publicación
   - Número de páginas
   - Idioma
   - Editorial
   - Autores (buscar o crear nuevos)
   - Géneros
   - Imagen de portada
4. El usuario envía el formulario
5. El sistema valida los datos
6. El sistema verifica duplicados por ISBN/título
7. El sistema crea el registro del libro
8. El sistema asocia autores y géneros
9. El sistema confirma creación exitosa

**Flujos Alternativos:**
- **A1 - Libro duplicado:** Sistema sugiere libro existente
- **A2 - Autor no existe:** Sistema permite crear nuevo autor
- **A3 - Género no existe:** Sistema permite sugerir nuevo género
- **A4 - ISBN inválido:** Sistema valida formato y muestra error

**Postcondiciones:**
- Nuevo libro agregado al catálogo
- Libro disponible para reseñas y listas
- Usuario creditado como contribuidor

---

### **CU-007: Búsqueda de Libros**

**Actor Principal:** Usuario Registrado

**Objetivo:** Encontrar libros específicos usando criterios de búsqueda

**Precondiciones:**
- Usuario autenticado en la plataforma
- Existe al menos un libro en el catálogo

**Flujo Principal:**
1. El usuario accede a la función de búsqueda
2. El sistema muestra interfaz de búsqueda avanzada
3. El usuario define criterios de búsqueda:
   - Texto libre (título, autor, ISBN)
   - Género específico
   - Rango de años de publicación
   - Idioma
   - Calificación mínima
   - Rango de páginas
4. El usuario ejecuta la búsqueda
5. El sistema procesa los filtros
6. El sistema retorna resultados paginados
7. El usuario puede ordenar resultados por:
   - Relevancia
   - Calificación
   - Fecha de publicación
   - Popularidad

**Flujos Alternativos:**
- **A1 - Sin resultados:** Sistema sugiere búsquedas relacionadas
- **A2 - Demasiados resultados:** Sistema sugiere filtros adicionales
- **A3 - Búsqueda vacía:** Sistema muestra libros recomendados

**Postcondiciones:**
- Resultados de búsqueda mostrados
- Historial de búsqueda guardado para recomendaciones

---

## 4. SISTEMA DE RESEÑAS

### **CU-008: Escribir Reseña**

**Actor Principal:** Usuario Registrado

**Objetivo:** Publicar una reseña y calificación de un libro

**Precondiciones:**
- Usuario autenticado
- Libro existe en el catálogo
- Usuario no ha reseñado previamente este libro

**Flujo Principal:**
1. El usuario navega al detalle del libro
2. El usuario selecciona "Escribir Reseña"
3. El sistema muestra formulario de reseña
4. El usuario completa la reseña:
   - Calificación (1-5 estrellas) - obligatorio
   - Título de la reseña
   - Contenido detallado
   - Marcador de spoilers (opcional)
5. El usuario puede guardar como borrador o publicar
6. El sistema valida contenido
7. El sistema guarda la reseña
8. El sistema actualiza estadísticas del libro
9. El sistema notifica a seguidores del usuario

**Flujos Alternativos:**
- **A1 - Reseña existente:** Sistema ofrece editar reseña actual
- **A2 - Contenido inapropiado:** Sistema marca para moderación
- **A3 - Guardar como borrador:** Sistema guarda sin publicar

**Postcondiciones:**
- Nueva reseña publicada o guardada como borrador
- Estadísticas del libro actualizadas
- Notificaciones enviadas a usuarios relevantes

---

### **CU-009: Comentar en Reseña**

**Actor Principal:** Usuario Registrado

**Objetivo:** Participar en la discusión de una reseña específica

**Precondiciones:**
- Usuario autenticado
- Reseña existe y está publicada
- Usuario tiene permisos para comentar

**Flujo Principal:**
1. El usuario lee una reseña
2. El usuario selecciona "Comentar"
3. El sistema muestra formulario de comentario
4. El usuario escribe su comentario
5. El usuario envía el comentario
6. El sistema valida el contenido
7. El sistema publica el comentario
8. El sistema notifica al autor de la reseña
9. El sistema actualiza contador de comentarios

**Flujos Alternativos:**
- **A1 - Reseña bloqueada:** Sistema informa que no se permiten comentarios
- **A2 - Comentario inapropiado:** Sistema marca para moderación
- **A3 - Usuario bloqueado:** Sistema impide comentar

**Postcondiciones:**
- Comentario publicado en la reseña
- Autor de reseña notificado
- Actividad registrada en el feed social

---

## 5. FUNCIONALIDADES SOCIALES

### **CU-010: Seguir Usuario**

**Actor Principal:** Usuario Registrado

**Objetivo:** Seguir a otro usuario para recibir actualizaciones de su actividad

**Precondiciones:**
- Usuario autenticado
- Usuario objetivo existe y está activo
- Usuario no se está siguiendo a sí mismo
- Usuario no está bloqueado por el usuario objetivo

**Flujo Principal:**
1. El usuario visita el perfil de otro usuario
2. El usuario hace clic en "Seguir"
3. El sistema verifica permisos y restricciones
4. El sistema crea la relación de seguimiento
5. El sistema actualiza contadores
6. El sistema notifica al usuario seguido
7. El sistema incluye al usuario en el feed de actividades

**Flujos Alternativos:**
- **A1 - Perfil privado:** Sistema envía solicitud de seguimiento
- **A2 - Ya siguiendo:** Sistema ofrece opción de dejar de seguir
- **A3 - Usuario bloqueado:** Sistema informa de la restricción

**Postcondiciones:**
- Relación de seguimiento establecida
- Usuario aparece en feed de actividades
- Notificación enviada al usuario seguido

---

### **CU-011: Dar Like a Reseña**

**Actor Principal:** Usuario Registrado

**Objetivo:** Expresar aprobación hacia una reseña específica

**Precondiciones:**
- Usuario autenticado
- Reseña existe y está publicada
- Usuario no ha dado like previamente a esta reseña

**Flujo Principal:**
1. El usuario lee una reseña
2. El usuario hace clic en el botón "Like"
3. El sistema registra el like
4. El sistema actualiza contador de likes
5. El sistema actualiza la interfaz visualmente
6. El sistema notifica al autor de la reseña
7. El sistema registra la actividad en el feed

**Flujos Alternativos:**
- **A1 - Like existente:** Sistema remueve el like (toggle)
- **A2 - Reseña eliminada:** Sistema informa que no está disponible
- **A3 - Usuario bloqueado:** Sistema impide la interacción

**Postcondiciones:**
- Like registrado en la reseña
- Contador actualizado en tiempo real
- Autor de reseña notificado del like

---

## 6. CLUBES DE LECTURA

### **CU-012: Crear Club de Lectura**

**Actor Principal:** Usuario Registrado

**Objetivo:** Establecer un nuevo club de lectura temático

**Precondiciones:**
- Usuario autenticado
- Usuario no ha excedido límite de clubes creados

**Flujo Principal:**
1. El usuario navega a "Crear Club"
2. El sistema muestra formulario de creación
3. El usuario completa información del club:
   - Nombre del club (único)
   - Descripción y objetivos
   - Imagen del club
   - Tipo de privacidad (público/privado/solo invitación)
   - Número máximo de miembros
   - Reglas del club
4. El usuario envía el formulario
5. El sistema valida los datos
6. El sistema crea el club
7. El sistema asigna al usuario como administrador
8. El sistema envía confirmación

**Flujos Alternativos:**
- **A1 - Nombre duplicado:** Sistema sugiere alternativas
- **A2 - Límite de clubes:** Sistema informa restricción
- **A3 - Imagen muy grande:** Sistema redimensiona automáticamente

**Postcondiciones:**
- Nuevo club creado y activo
- Usuario es administrador del club
- Club aparece en búsquedas (si es público)

---

### **CU-013: Unirse a Club**

**Actor Principal:** Usuario Registrado

**Objetivo:** Convertirse en miembro de un club de lectura existente

**Precondiciones:**
- Usuario autenticado
- Club existe y está activo
- Club no ha alcanzado el límite de miembros
- Usuario no es miembro actual del club

**Flujo Principal:**
1. El usuario encuentra un club de interés
2. El usuario visualiza detalles del club
3. El usuario hace clic en "Unirse al Club"
4. El sistema verifica disponibilidad
5. Para clubs públicos: unión inmediata
6. Para clubs privados: solicitud de unión
7. El sistema actualiza información del club
8. El sistema notifica a administradores (si aplica)
9. El usuario obtiene acceso a contenido del club

**Flujos Alternativos:**
- **A1 - Club lleno:** Sistema ofrece unirse a lista de espera
- **A2 - Solicitud pendiente:** Sistema informa estado actual
- **A3 - Usuario bloqueado:** Sistema impide unión

**Postcondiciones:**
- Usuario es miembro del club (o solicitud enviada)
- Acceso a lecturas y discusiones del club
- Club aparece en "Mis Clubes"

---

### **CU-014: Crear Lectura Grupal**

**Actor Principal:** Usuario Registrado (Moderador/Admin de Club)

**Objetivo:** Organizar una nueva lectura grupal dentro del club

**Precondiciones:**
- Usuario es moderador o administrador del club
- Club está activo
- No existe lectura activa que se superponga en fechas

**Flujo Principal:**
1. El usuario navega a la gestión del club
2. El usuario selecciona "Nueva Lectura"
3. El sistema muestra formulario de lectura:
   - Selección de libro del catálogo
   - Fecha de inicio
   - Fecha de fin
   - Descripción de la lectura
   - Objetivos específicos
4. El usuario configura la lectura
5. El sistema valida fechas y disponibilidad
6. El sistema crea la lectura grupal
7. El sistema notifica a todos los miembros
8. El sistema habilita seguimiento de progreso

**Flujos Alternativos:**
- **A1 - Fechas inválidas:** Sistema sugiere fechas apropiadas
- **A2 - Libro no disponible:** Sistema permite agregar libro al catálogo
- **A3 - Lectura superpuesta:** Sistema advierte del conflicto

**Postcondiciones:**
- Nueva lectura grupal creada y programada
- Miembros notificados de la nueva lectura
- Sistema de progreso activado

---

## 7. SISTEMA DE NOTIFICACIONES

### **CU-015: Gestionar Notificaciones**

**Actor Principal:** Usuario Registrado

**Objetivo:** Recibir, revisar y gestionar notificaciones personales

**Precondiciones:**
- Usuario autenticado en la plataforma
- Sistema de notificaciones habilitado

**Flujo Principal:**
1. El sistema genera notificación por actividad relevante
2. El sistema determina preferencias del usuario
3. El sistema envía notificación según canal preferido:
   - In-app (plataforma)
   - Email
   - Push (móvil)
4. El usuario recibe y visualiza notificación
5. El usuario puede marcar como leída
6. El usuario puede tomar acción directa desde notificación
7. El sistema actualiza estado de notificación

**Flujos Alternativos:**
- **A1 - Notificaciones deshabilitadas:** Sistema respeta preferencias
- **A2 - Múltiples notificaciones:** Sistema agrupa por tipo
- **A3 - Acción no disponible:** Sistema redirige apropiadamente

**Postcondiciones:**
- Usuario informado de actividades relevantes
- Estado de notificaciones actualizado
- Posible navegación a contenido relacionado

---

## 8. ADMINISTRACIÓN DEL SISTEMA

### **CU-016: Moderar Contenido**

**Actor Principal:** Moderador

**Objetivo:** Revisar y moderar contenido reportado o marcado

**Precondiciones:**
- Usuario tiene rol de Moderador o superior
- Existe contenido pendiente de moderación

**Flujo Principal:**
1. El moderador accede a la cola de moderación
2. El sistema muestra elementos pendientes
3. El moderador revisa contenido reportado:
   - Reseñas inapropiadas
   - Comentarios ofensivos
   - Perfiles con contenido inadecuado
   - Clubs con actividad sospechosa
4. El moderador toma decisión:
   - Aprobar contenido
   - Rechazar y eliminar
   - Editar contenido
   - Advertir al usuario
   - Suspender temporalmente
5. El sistema ejecuta la acción
6. El sistema notifica al usuario afectado
7. El sistema registra la decisión de moderación

**Flujos Alternativos:**
- **A1 - Caso complejo:** Moderador escala a administrador
- **A2 - Acción en lote:** Sistema permite moderar múltiples elementos
- **A3 - Apelación:** Usuario puede solicitar revisión

**Postcondiciones:**
- Contenido moderado según decisión
- Usuario informado de la acción tomada
- Registro de moderación creado para auditoría

---

### **CU-017: Generar Reportes del Sistema**

**Actor Principal:** Administrador

**Objetivo:** Obtener métricas y reportes sobre el uso de la plataforma

**Precondiciones:**
- Usuario tiene rol de Administrador
- Datos suficientes para generar reportes

**Flujo Principal:**
1. El administrador accede al panel de reportes
2. El sistema muestra opciones de reportes disponibles
3. El administrador selecciona tipo de reporte:
   - Usuarios activos por período
   - Libros más populares
   - Clubes más activos
   - Actividad de moderación
   - Métricas de engagement
4. El administrador define parámetros:
   - Rango de fechas
   - Filtros específicos
   - Formato de exportación
5. El sistema genera el reporte
6. El sistema presenta resultados
7. El administrador puede exportar datos

**Flujos Alternativos:**
- **A1 - Reporte muy grande:** Sistema pagina resultados
- **A2 - Datos insuficientes:** Sistema informa limitaciones
- **A3 - Error en generación:** Sistema permite reintento

**Postcondiciones:**
- Reporte generado según especificaciones
- Datos disponibles para análisis
- Información exportada si se solicitó

---

## 📊 MÉTRICAS DE CASOS DE USO

### **Criterios de Aceptación Generales:**
- **Tiempo de respuesta:** < 3 segundos para operaciones normales
- **Disponibilidad:** 99.9% uptime
- **Seguridad:** Todas las operaciones auditadas
- **Usabilidad:** Máximo 3 clics para funciones principales
- **Accesibilidad:** Cumplimiento WCAG 2.1 AA

### **Validaciones Transversales:**
- **Autenticación:** Verificación en cada operación
- **Autorización:** Validación de permisos por rol
- **Sanitización:** Limpieza de inputs del usuario
- **Rate Limiting:** Prevención de abuso de APIs
- **Logging:** Registro de operaciones críticas