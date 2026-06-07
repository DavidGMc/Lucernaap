[README (1).md](https://github.com/user-attachments/files/28686764/README.1.md)
# Lucernaap
Sistema de gestion integral para congregaciones.
# 🕯️ Lucernapp

<div align="center">

![Lucernapp Banner](https://img.shields.io/badge/Lucernapp-Sistema%20de%20Gesti%C3%B3n%20de%20Congregaci%C3%B3n-1e40af?style=for-the-badge&logo=android&logoColor=white)

**Sistema de gestión integral para congregaciones religiosas**

*Asignaciones · Territorios · Informes de Servicio · Predicación Pública · Panel Web Admin*

---

[![Android](https://img.shields.io/badge/Android-8.0%2B-3DDC84?style=flat-square&logo=android&logoColor=white)](https://android.com)
[![Kotlin](https://img.shields.io/badge/Kotlin-2.1.0-7F52FF?style=flat-square&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![KMP](https://img.shields.io/badge/Kotlin-Multiplatform-7F52FF?style=flat-square&logo=kotlin&logoColor=white)](https://kotlinlang.org/docs/multiplatform.html)
[![Compose](https://img.shields.io/badge/Jetpack%20Compose-1.7%2B-4285F4?style=flat-square&logo=jetpackcompose&logoColor=white)](https://developer.android.com/jetpack/compose)
[![Firebase](https://img.shields.io/badge/Firebase-33.7.0-FFCA28?style=flat-square&logo=firebase&logoColor=black)](https://firebase.google.com)
[![Next.js](https://img.shields.io/badge/Next.js-15-000000?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org)
[![License](https://img.shields.io/badge/License-MIT-059669?style=flat-square)](LICENSE)

</div>

---

## 📖 Tabla de Contenidos

- [¿Qué es Lucernapp?](#-qué-es-lucernapp)
- [Características principales](#-características-principales)
- [Comparativa con NW Publisher](#-comparativa-con-nw-publisher)
- [Arquitectura del sistema](#-arquitectura-del-sistema)
- [Stack tecnológico](#-stack-tecnológico)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Modelo de datos Firebase](#-modelo-de-datos-firebase)
- [Pantallas principales](#-pantallas-principales)
- [Panel Web Admin](#-panel-web-admin)
- [Roadmap](#-roadmap)
- [Configuración del proyecto](#-configuración-del-proyecto)
- [Contribuir](#-contribuir)
- [Licencia](#-licencia)

---

## 🕯️ ¿Qué es Lucernapp?

**Lucernapp** *(del latín lucerna — lámpara, luz)* es una aplicación Android nativa para la **gestión integral de congregaciones religiosas**. Permite a publicadores, ancianos y secretarios gestionar todo su trabajo desde una sola app — sin depender de software externo de pago.

> 💡 Competidor directo de **NW Publisher** (1.7M descargas, ⭐ 4.84) — con funcionalidades que este no tiene: mapa de territorios, intercambio de asignaciones, panel web para ancianos y reporte completo del secretario.

### ¿Por qué Lucernapp?

```
El problema actual:
  NW Publisher (app) + NW Scheduler (software de pago) + Excel para el secretario
  
La solución:
  Lucernapp → todo en una sola app + panel web, sin costos adicionales
```

---

## ✨ Características Principales

### 📋 Gestión de Asignaciones
- CRUD completo de asignaciones de reuniones (entresemana y fin de semana)
- Confirmación de preparación por el publicador
- **Sistema de intercambio**: solicitar swap con otro publicador, aprobación del anciano
- Notificaciones push 48h y 2h antes de cada asignación
- Soporte para todas las partes: estudiante, discurso, oración, presidente, lector

### 🗺️ Territorios con Mapa
- Mapa interactivo Google Maps con **polígonos** de colores por estado
- Estados del ciclo de vida: `AVAILABLE → ASSIGNED → IN_PROGRESS → COMPLETED`
- Registro de **Do Not Calls (DNC)** con acceso restringido
- Historial completo por territorio y alertas de vencimiento automáticas
- Tipos: Residencial, Comercial, Telefónico, Cartas, Digital

### ⏱️ Informes de Servicio
- **Timer inteligente** con Foreground Service — sigue corriendo al cerrar la app
- Acumulación automática de minutos al informe del mes
- Gráfica de barras con historial de 12 meses
- Barra de progreso para pioneros (30h / 50h)
- Panel del secretario: compilar estadísticas, exportar **PDF S-21 oficial**

### 👥 Predicación Pública
- Reserva de turnos con visualización de compañeros
- **Lista de espera automática**: cuando alguien cancela, notificación push al siguiente
- GPS del punto de predicación en Google Maps
- Regla de cancelación mínimo 24h antes

### 📢 Anuncios y Eventos
- Prioridades: alta (push inmediato), normal, baja
- Adjuntos: imágenes y PDFs
- Calendario de eventos con tipos: asamblea, visita CO, discurso especial, memorial
- Registro de lecturas (quién leyó el anuncio)

### 🔔 Deberes del Salón
- Sonido, Micrófonos, Limpieza, Jardín, Mesa de atención, Zoom
- Tipos personalizables por congregación
- Confirmación del hermano asignado

---

## 📊 Comparativa con NW Publisher

| Característica | NW Publisher | Lucernapp |
|---|:---:|:---:|
| Software externo requerido | ❌ NW Scheduler (pago) | ✅ 100% autónomo |
| Territorios con mapa | ❌ Lista básica | ✅ Google Maps + polígonos |
| Intercambio de asignaciones | ❌ No disponible | ✅ Con aprobación |
| Panel secretario completo | ⚠️ Parcial | ✅ + exportar PDF S-21 |
| Panel web para ancianos | ❌ No | ✅ Next.js |
| Grupos de servicio | ❌ No | ✅ Conductores + listas |
| Do Not Calls con privacidad | ❌ No | ✅ Acceso restringido |
| Modo offline | ⚠️ Limitado | ✅ Offline-first con Room |
| Costo operativo | 💰 NW Scheduler | 🆓 $0/mes (Firebase Spark) |

---

## 🏗️ Arquitectura del Sistema

```
┌─────────────────────────────────────────────────────────┐
│                    FIREBASE (Backend)                    │
│   Firestore  │  Auth  │  Storage  │  FCM  │  Functions  │
└──────────────────────────┬──────────────────────────────┘
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
  │ Android App  │ │  Web Panel   │ │  iOS (Fase 4)│
  │  Kotlin KMP  │ │  Next.js 15  │ │  KMP target  │
  │              │ │  TypeScript  │ │  (preparado) │
  │ commonMain   │ │  shadcn/ui   │ │              │
  │ androidMain  │ │  Tailwind    │ │              │
  └──────┬───────┘ └──────────────┘ └──────────────┘
         │
         ▼
  ┌──────────────┐
  │ Room SQLite  │  ← Cache local offline-first
  └──────────────┘
```

### Principios arquitecturales

- **MVVM + Clean Architecture**: ViewModel → UseCase → Repository → DataSource
- **Offline-First**: Room emite datos inmediatamente, Firestore actualiza en background
- **Multi-tenancy**: cada congregación completamente aislada bajo `congregationData/{congId}/`
- **Unidirectional Data Flow**: eventos UI → ViewModel → StateFlow → UI
- **KMP preparado**: `commonMain` desde el día 1, iOS se activa con un target

---

## 🛠️ Stack Tecnológico

### App Android (KMP)

| Capa | Tecnología | Versión | Propósito |
|------|-----------|---------|-----------|
| **UI** | Jetpack Compose + Material3 | 1.7+ | UI declarativa, Dynamic Colors |
| **Navegación** | Navigation Compose | 2.8+ | Type-safe con `@Serializable` |
| **DI** | Koin | 4.0.0 | Inyección de dependencias KMP |
| **Base de datos** | SQLDelight | 2.0.2 | Cache local KMP (reemplaza Room) |
| **Networking** | Ktor Client | 3.1.0 | HTTP client KMP |
| **Firebase** | GitLive Firebase KMP | 2.1.0 | Wrapper KMP del SDK oficial |
| **Preferencias** | Multiplatform-Settings | 1.2.0 | Key-value KMP |
| **Imágenes** | Coil 3 | 3.0.4 | Image loading KMP |
| **Mapas** | Maps Compose | 4.4.1 | Google Maps en Compose |
| **Gráficas** | Vico Charts | 2.x | Barras y líneas en Compose |
| **Serialización** | kotlinx.serialization | 1.7.3 | JSON Kotlin-first |
| **Corrutinas** | kotlinx.coroutines | 1.9.0 | Async + Flow |

### Panel Web Admin

| Tecnología | Versión | Propósito |
|-----------|---------|-----------|
| **Next.js 15** (App Router) | 15.x | Framework React con SSR |
| **TypeScript** | 5.x | Tipado fuerte |
| **shadcn/ui + Tailwind CSS** | latest | Componentes + estilos |
| **Firebase JS SDK** | 10.x | Mismo proyecto Firebase |
| **TanStack Table** | 8.x | Tablas con filtros y exportación |
| **Recharts** | 2.x | Estadísticas del secretario |
| **React Hook Form + Zod** | latest | Formularios con validación |
| **@react-pdf/renderer** | 3.x | Exportar S-21 PDF |

---

## 📁 Estructura del Proyecto

```
lucernapp/
├── composeApp/                          # Módulo KMP principal
│   └── src/
│       ├── commonMain/kotlin/com/lucernapp/
│       │   ├── data/
│       │   │   ├── model/               # DTOs @Serializable
│       │   │   ├── remote/              # Firebase DataSources
│       │   │   ├── repository/          # Implementaciones
│       │   │   └── local/               # SQLDelight queries
│       │   ├── domain/
│       │   │   ├── model/               # Entidades del dominio
│       │   │   ├── repository/          # Interfaces (contratos)
│       │   │   └── usecase/             # Lógica de negocio
│       │   ├── presentation/
│       │   │   ├── navigation/          # NavGraph + Routes
│       │   │   ├── screens/             # Composables de pantallas
│       │   │   ├── components/          # Composables reutilizables
│       │   │   └── theme/               # Material3 + Colors
│       │   └── di/                      # Koin modules
│       │
│       ├── androidMain/kotlin/          # Código específico Android
│       │   ├── DatabaseDriverFactory.android.kt
│       │   ├── NotificationService.android.kt
│       │   └── MainActivity.kt
│       │
│       └── iosMain/kotlin/              # iOS — preparado, activar en Fase 4
│           └── DatabaseDriverFactory.ios.kt
│
├── sqldelight/                          # Esquemas SQL compartidos
│   └── com/lucernapp/
│       ├── Assignment.sq
│       ├── Report.sq
│       ├── Territory.sq
│       └── SyncQueue.sq
│
└── webPanel/                            # Panel admin Next.js
    ├── app/
    │   ├── (auth)/login/
    │   └── (dashboard)/
    │       ├── schedule/                # Editor programa reuniones
    │       ├── reports/                 # Panel secretario
    │       ├── territories/             # Mapa + gestión
    │       ├── users/                   # Gestión usuarios
    │       └── statistics/              # Gráficas y estadísticas
    ├── lib/
    │   ├── firebase.ts
    │   └── repositories/
    └── types/
```

---

## 🗄️ Modelo de Datos Firebase

Firestore usa un modelo **multi-tenant** — cada congregación tiene sus datos completamente aislados:

```
firestore-root/
│
├── congregations/{congId}/          # UUID v4 al crear la congregación
│   ├── info                         # nombre, ciudad, idioma, circuito
│   ├── settings                     # pinHash, horarios, configuración
│   └── inviteCodes/{codeId}         # códigos PIN de invitación (72h)
│
├── users/{userId}/                  # Firebase Auth UID
│   ├── profile                      # displayName, email, photo, género
│   ├── membership                   # congregationId, role, isActive, fcmToken
│   ├── settings                     # notificaciones, darkMode, idioma
│   └── delegate                     # delegado para gestionar asignaciones
│
└── congregationData/{congId}/       # Mismo congId — datos operativos
    ├── assignments/                 # Asignaciones de reuniones
    ├── duties/                      # Deberes del salón
    ├── territories/                 # Territorios + polígonos mapa
    ├── doNotCalls/                  # 🔒 Solo asignado + ancianos
    ├── fieldReports/                # Informes mensuales {userId_YYYY-MM}
    ├── witnessing/                  # Turnos predicación pública
    ├── announcements/               # Anuncios + adjuntos
    ├── events/                      # Calendario de eventos
    ├── talks/                       # Programa discursos fin de semana
    ├── groups/                      # Grupos de servicio del campo
    ├── awayPeriods/                 # Períodos de ausencia
    ├── literature/                  # Solicitudes de publicaciones
    └── secretaryData/               # 🔒 Solo secretario/admin {YYYY-MM}
```

### Roles de usuario

| Rol | Permisos |
|-----|----------|
| `publisher` | Ver asignaciones, registrar informes, trabajar territorios asignados |
| `servant` | + Crear asignaciones y deberes, publicar anuncios |
| `elder` | + Gestionar territorios, ver todos los informes, aprobar intercambios |
| `secretary` | + Compilar estadísticas mensuales, exportar S-21 PDF |
| `coordinator` | + Coordinación general de ancianos |
| `admin` | Control total: gestión de usuarios, configuración, generar PINs |

---

## 📱 Pantallas Principales

<table>
<tr>
<td align="center"><b>🏠 Home Dashboard</b><br/>Asignación próxima + progreso informe + anuncios</td>
<td align="center"><b>📋 Asignaciones</b><br/>Lista + calendario mensual + confirmación</td>
<td align="center"><b>⏱️ Timer Servicio</b><br/>Foreground Service + barra pioneros</td>
</tr>
<tr>
<td align="center"><b>🗺️ Territorios Mapa</b><br/>Google Maps + polígonos por estado</td>
<td align="center"><b>👥 Predicación</b><br/>Reservas + lista de espera + GPS</td>
<td align="center"><b>⚙️ Admin Panel</b><br/>Usuarios + PINs + configuración</td>
</tr>
</table>

**Navegación principal** — Bottom Navigation Bar con 5 tabs:
`Inicio` · `Asignaciones` · `Informes` · `Territorios` · `Predicación`

---

## 🖥️ Panel Web Admin

El panel web está diseñado para lo que es incómodo en móvil — los ancianos y secretarios lo usan desde computadora:

| Módulo | Funcionalidad | Rol |
|--------|--------------|-----|
| **Programa reuniones** | Editor drag-and-drop del mes, exportar PDF para cartelera | Elder, Admin |
| **Informes** | Tabla filtrable, irregulares, compilar, exportar S-21 | Secretary, Admin |
| **Territorios** | Mapa completo, asignar, ver DNC, alertas vencimiento | Elder, Admin |
| **Usuarios** | Cambiar roles, grupos de servicio, ausencias | Admin |
| **Estadísticas** | Gráficas mensuales/anuales, tendencias, exportar Excel | Elder, Secretary |
| **Configuración** | Datos cong., horarios, tipos deberes, generar PINs | Admin |

> El panel web usa el **mismo proyecto Firebase** que la app — mismos usuarios, mismos datos, sin sincronización adicional.

---

## 🗺️ Roadmap

```
Fase 1 — MVP Android (Meses 1-4)
├── ✅ Autenticación + onboarding con PIN
├── ✅ Asignaciones CRUD + notificaciones
├── ✅ Deberes + confirmación
├── ✅ Informes + timer básico
├── ✅ Anuncios y eventos
└── ✅ SQLDelight offline cache

Fase 2 — Features Core (Meses 5-8)
├── 🗺️ Territorios + mapa + DNC + alertas
├── 🔄 Intercambio de asignaciones
├── 👥 Predicación pública + lista de espera
├── ⏱️ Timer avanzado + widget Android
├── 👨‍👩‍👧 Grupos de servicio del campo
└── 🔁 WorkManager sync queue robusta

Fase 3 — Web Panel Admin (Meses 7-9)
├── 📅 Editor programa de reuniones
├── 📊 Panel secretario + S-21 PDF
├── 🗺️ Mapa territorios web
└── 📈 Estadísticas con Recharts

Fase 4 — iOS (Mes 10+)
└── 📱 Activar KMP iOS target (ya preparado)
    └── iosX64, iosArm64, iosSimulatorArm64
```

---

## ⚙️ Configuración del Proyecto

### Prerrequisitos

- **Android Studio** Hedgehog o superior
- **JDK 17**
- **Cuenta Firebase** (plan Spark gratuito es suficiente)
- **Node.js 20+** (solo para el panel web)

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/lucernapp.git
cd lucernapp
```

### 2. Configurar Firebase

1. Crear proyecto en [Firebase Console](https://console.firebase.google.com)
2. Habilitar: **Firestore**, **Authentication** (Email + Google), **Storage**, **FCM**
3. Descargar `google-services.json` → colocar en `composeApp/`
4. Agregar `google-services.json` a `.gitignore`

```bash
# .gitignore
composeApp/google-services.json
webPanel/.env.local
local.properties
```

### 3. Configurar Google Maps

```properties
# local.properties
MAPS_API_KEY=tu_api_key_aqui
```

### 4. Build Android

```bash
./gradlew :composeApp:assembleDebug
```

### 5. Panel Web Admin

```bash
cd webPanel
cp .env.example .env.local
# Editar .env.local con tus credenciales de Firebase
npm install
npm run dev
```

### 6. Aplicar reglas de Firestore

```bash
# Desde la raíz del proyecto
firebase deploy --only firestore:rules
firebase deploy --only firestore:indexes
```

### Variables de entorno Web Panel

```env
# webPanel/.env.local
NEXT_PUBLIC_FIREBASE_API_KEY=...
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=...
NEXT_PUBLIC_FIREBASE_PROJECT_ID=...
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=...
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=...
NEXT_PUBLIC_FIREBASE_APP_ID=...
```

---

## 💰 Costos

| Servicio | Límite Gratuito | Estimado 1 congregación (100 usuarios) |
|---------|----------------|----------------------------------------|
| Firestore reads | 50,000/día | ~5,000-10,000/día ✅ |
| Firestore writes | 20,000/día | ~500-2,000/día ✅ |
| Firebase Auth | Ilimitado | Gratuito siempre ✅ |
| Storage | 5 GB total | ~500 MB/año ✅ |
| FCM Notificaciones | Ilimitado | Gratuito siempre ✅ |
| Firebase Hosting | 10 GB/mes | Panel web gratis ✅ |
| Google Play Store | $25 único | Registro para siempre ✅ |

> 💡 **Costo operativo para una congregación típica: $0/mes**

---

## 🔒 Seguridad

- **Reglas Firestore** en servidor — no se pueden saltear desde la app
- **Multi-tenancy estricto** — ningún usuario accede a datos de otra congregación
- **PINs con SHA-256** — el PIN original nunca se guarda en texto plano
- **Roles granulares** — cada operación verifica el rol del usuario
- **DNC con acceso restringido** — solo publicador asignado + ancianos/admin
- **TLS/HTTPS** — todos los datos cifrados en tránsito y en reposo

---

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor:

1. Fork el repositorio
2. Crea tu rama: `git checkout -b feature/nueva-funcionalidad`
3. Commit tus cambios: `git commit -m 'feat: agregar nueva funcionalidad'`
4. Push: `git push origin feature/nueva-funcionalidad`
5. Abre un Pull Request

### Convención de commits

```
feat:     nueva funcionalidad
fix:      corrección de bug
docs:     cambios en documentación
style:    formato, espacios (sin cambio de lógica)
refactor: refactorización de código
test:     agregar o modificar tests
chore:    cambios en build, dependencias
```

---

## 📄 Licencia

```
MIT License — Copyright (c) 2026 Lucernapp

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software to deal in the Software without restriction.
```

Ver [LICENSE](LICENSE) para más detalles.

---

<div align="center">

**Lucernapp** — Hecho con ❤️ para congregaciones

[![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://android.com)
[![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com)

*"La lámpara de tu cuerpo es el ojo"*

</div>
