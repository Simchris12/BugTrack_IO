# BugTrack_IO
Plataforma web enfocada en el crowd-testing y la gestión integral de errores de software. Este sistema permite a los evaluadores reportar fallos técnicos y a los desarrolladores realizar un seguimiento del ciclo de vida de cada error, estructurando la solución bajo una arquitectura MVC robusta


# BugTrack IO

Plataforma web para crowd-testing y gestión del ciclo de vida de errores de software.

Proyecto de la asignatura Ingeniería de Software I. Autor: Cristofer Alexander Castillo.

## Objetivo

Ofrecer una plataforma web centralizada donde los evaluadores (QA) reporten errores con evidencias multimedia, los desarrolladores den seguimiento a su estado y los administradores gestionen las incidencias y sus asignaciones.

## Problema que resuelve

En los ciclos de desarrollo existe una desconexión entre quienes encuentran los fallos y quienes los corrigen. Sin un medio estandarizado se pierde información, se retrasan las correcciones y no hay trazabilidad sobre el estado de cada error. BugTrack IO centraliza el reporte, la priorización y el seguimiento.

## Usuarios principales

- **QA / Tester:** registra y consulta incidencias, adjunta evidencias y comenta.
- **Desarrollador:** consulta los bugs asignados, actualiza su estado y comenta.
- **Administrador:** gestiona incidencias y asignaciones.

## Funcionalidades principales

Requisitos funcionales definidos (ver `docs/requisitos.md`):

| ID | Requisito |
|----|-----------|
| RF01 | Registrar nuevas incidencias o bugs. |
| RF02 | Consultar y visualizar las incidencias registradas. |
| RF03 | Asignar una incidencia a un desarrollador. |
| RF04 | Actualizar el estado de una incidencia (Pendiente, En revisión, Solucionado). |
| RF05 | Agregar comentarios a una incidencia. |

Requisitos clave de calidad:

- **REQ-01:** adjuntar archivos JPG, PNG o MP4 con un máximo de 15 MB por reporte.
- **REQ-02:** severidad obligatoria (Baja, Media, Alta, Crítica) al crear un bug.
- **REQ-03:** el cambio de estado se refleja en el panel del evaluador que creó el reporte.
- **REQ-04:** la API responde en menos de 2.5 segundos al enviar formularios con adjuntos (percentil 95).
- **REQ-05:** autenticación mediante JSON Web Tokens; sin token válido la API responde 403 Forbidden.

## Arquitectura seleccionada

Estilo **cliente-servidor** con separación de frontend y backend. El backend sigue una **arquitectura por capas con patrón MVC** (controladores, servicios, repositorios y entidades).

Principio de diseño principal: **Responsabilidad Única (SRP)**, complementado con DIP (inyección de dependencias) para facilitar pruebas con mocks.

Limitación conocida: mayor número de componentes y capas, lo que aumenta la complejidad inicial del desarrollo. Se considera aceptable porque facilita mantenimiento, pruebas y crecimiento futuro.

## Organización frontend y backend

### Responsabilidades

| Componente | Responsabilidad | Se relaciona con |
|------------|-----------------|------------------|
| **Frontend** (React) | Interfaz para autenticarse, registrar y consultar incidencias, y ver paneles según el rol. Valida datos del lado del cliente (por ejemplo, tamaño y tipo de archivo) antes de enviarlos. | Backend, mediante la API REST. |
| **Controllers** (`@RestController`) | Recibir peticiones HTTP, validar entradas y delegar en los servicios. No contienen lógica de negocio. | Frontend (entrada) y Services (salida). |
| **Services** (`@Service`) | Reglas de negocio: validación de severidad, asignación de incidencias, cambios de estado y gestión de archivos. | Controllers y Repositories. |
| **Repositories** (JPA) | Acceso y persistencia de datos. | Services y PostgreSQL. |
| **Seguridad** (JWT, Spring Security) | Generar y validar tokens, aplicar roles y permisos (`@PreAuthorize`). | Filtro HTTP y Controllers. |
| **Base de datos** (PostgreSQL) | Almacenar usuarios, proyectos, incidencias, comentarios, notificaciones y archivos. | Repositories. |

### Comunicación

1. El usuario interactúa con el frontend.
2. El frontend envía una petición HTTP a la API REST, incluyendo el token JWT en la cabecera `Authorization`.
3. El filtro de seguridad valida el token. Si no es válido, responde 403.
4. El controlador valida la entrada y llama al servicio correspondiente.
5. El servicio aplica las reglas de negocio y solicita datos al repositorio.
6. El repositorio consulta o escribe en PostgreSQL y devuelve la información en formato DTO.
7. La respuesta viaja de vuelta en JSON hasta el frontend, que actualiza la vista.

### Estructura de carpetas propuesta

```
bugtrack-io/
├── backend/                  # Spring Boot (Java)
│   └── src/main/java/.../
│       ├── controller/       # Capa de presentación de la API
│       ├── service/          # Lógica de negocio
│       ├── repository/       # Acceso a datos (JPA)
│       ├── entity/           # Entidades de la base de datos
│       ├── dto/              # Objetos de transferencia
│       └── security/         # JWT y configuración de seguridad
├── frontend/                 # React (JavaScript)
│   └── src/
│       ├── pages/            # Login, dashboards por rol
│       ├── components/       # Formularios y elementos reutilizables
│       ├── store/            # Estado global de autenticación
│       └── hooks/            # Llamadas a la API con token
└── docs/                     # Requisitos, modelos UML y decisiones
```

## Herramientas utilizadas

- **Backend:** Java, Spring Boot, Spring Security, JWT, JPA.
- **Frontend:** React, JavaScript.
- **Base de datos:** PostgreSQL.
- **Modelado:** Visual Paradigm Online (casos de uso, clases, secuencia, actividades, arquitectura).
- **Gestión y control de versiones:** GitHub, GitHub Issues y GitHub Projects (tablero Kanban).
- **Metodología:** Kanban.

## Documentación de modelado

- Diagrama de casos de uso: https://online.visual-paradigm.com/share.jsp?id=343730343734342d32
- Diagrama de clases: https://online.visual-paradigm.com/share.jsp?id=343730343734342d33
- Diagrama de secuencia: https://online.visual-paradigm.com/share.jsp?id=343730343734342d34
- Diagrama de actividades: https://online.visual-paradigm.com/share.jsp?id=343730343734342d35
- Tablero del proyecto: https://github.com/users/Simchris12/projects/1

## Estado actual

Fase inicial de desarrollo.

- **Completado:** requisitos funcionales y no funcionales, historias de usuario, diagramas UML, definición de la arquitectura, tablero Kanban y plan de comunicación con interesados.
- **En progreso:** diseño de la estructura de la base de datos, interfaz de gestión de incidencias y módulo de registro de incidencias.
- **Pendiente:** actualización de estados, pruebas funcionales, documentación técnica final y ejecución del proyecto con instrucciones de instalación.

Consulta la versión y los cambios controlados en `CHANGELOG.md` y `NOTA_TECNICA_VERSION.md`.

📋 Requisitos Previos

Para levantar el entorno de desarrollo local, asegúrate de tener instalado:

Git

Docker Desktop o Docker Engine

Node.js (v18 o superior)

Java JDK (v17 o superior)

⚙️ Variables de Entorno (.env)

Antes de iniciar, crea un archivo .env en el directorio raíz del backend y del frontend con las siguientes configuraciones básicas:

Backend (/backend/.env):

DB_HOST=localhost
DB_PORT=5432
DB_NAME=bugtrack_db
DB_USER=root
DB_PASSWORD=secret
JWT_SECRET=ingsoftware_uapa_secret_key_2026
MAX_FILE_SIZE=15MB

Frontend (/frontend/.env.local):

NEXT_PUBLIC_API_URL=http://localhost:8080/api/v1

🚀 Instalación y Ejecución

Sigue estos pasos para ejecutar el proyecto en tu entorno local:

1. Clonar el repositorio

git clone: 


2. Levantar la Infraestructura

Base de datos y servicios auxiliares:

docker-compose up -d

3. Ejecutar el Backend (Spring Boot)

cd backend
./mvnw spring-boot:run

4. Ejecutar el Frontend (Next.js)

cd frontend
npm install
npm run dev

La plataforma estará disponible en http://localhost:3000 y la API en http://localhost:8080.

📁 Estructura del Proyecto (Backend MVC)

backend/src/main/java/com/bugtrackio/
 ┣ 📂 config       # Configuraciones de seguridad (JWT, CORS)
 ┣ 📂 controllers  # Endpoints de la API REST (Controladores)
 ┣ 📂 models       # Entidades de la Base de Datos (Modelos)
 ┣ 📂 repositories # Interfaces para acceso a datos (Spring Data JPA)
 ┣ 📂 services     # Lógica de negocio principal
 ┗ 📜 BugTrackApplication.java

🔒 Autenticación y Seguridad

El acceso a los endpoints protegidos se realiza mediante JSON Web Tokens (JWT).

Para probar la API, envía el token en los encabezados HTTP:

Authorization: Bearer <tu_token_aqui>

👨‍💻 Autores

Cristofer Alexander Castillo Henríquez - Análisis, Arquitectura y Desarrollo - Universidad Abierta para Adultos (UAPA) - Asignatura: Ingeniería de Software I.
