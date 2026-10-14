# Nota técnica de versión: BugTrack IO v0.1.0

**Tipo de versión:** inicial controlada (fase de diseño y especificación).
**Fecha de corte:** octubre de 2026.
**Responsable:** Cristofer Alexander Castillo.

## 1. Contenido de la versión

Esta versión contiene la base técnica del sistema:

- Requisitos funcionales (RF01 a RF05) y no funcionales (REQ-01 a REQ-05), con criterios de aceptación.
- Historias de usuario HU-01 a HU-05 y trazabilidad hacia Issues de GitHub.
- Modelos UML: casos de uso, clases, secuencia y actividades.
- Arquitectura cliente-servidor con capas en el backend y separación de frontend.
- Tablero Kanban en GitHub Projects con tareas, prioridades y riesgos.
- Documentación principal en el README y registro de cambios.

Esta versión no contiene una aplicación ejecutable completa.

## 2. Cambios controlados

Los cambios se registran en `CHANGELOG.md` y se vinculan a Issues y a la documentación:

1. Especificación de requisitos (Semana 3).
2. Modelado UML (Semana 4).
3. Definición de la arquitectura inicial (Semana 5).
4. Organización del tablero Kanban (Semana 6).
5. Plan de comunicación y validación con interesados (Semana 7).

## 3. Estado de avance

| Área | Estado |
|------|--------|
| Requisitos y modelos | Finalizado |
| Arquitectura | Definida, pendiente de validación docente |
| Diseño de base de datos | En progreso |
| Interfaz de gestión de incidencias | En progreso |
| Registro de incidencias | En progreso |
| Actualización de estados | Por hacer |
| Pruebas funcionales | Por hacer |
| Documentación técnica final | Backlog |

## 4. Aspectos pendientes

- **Validación de la arquitectura:** la solicitud enviada al docente facilitador sigue sin respuesta registrada.
- **Base de datos:** entidades, relaciones e índices aún no implementados en PostgreSQL.
- **Seguridad:** la autenticación JWT (REQ-05) está especificada, pero su implementación aún no se ha verificado.
- **Rendimiento:** el cumplimiento de REQ-04 (menos de 2.5 s) no ha sido medido.


## 5. Riesgos abiertos

- Retraso en las funcionalidades principales (probabilidad media, impacto alto).
- Inconsistencias entre los modelos UML y la implementación (probabilidad media, impacto alto).
- Problemas en la estructura o integración de la base de datos (probabilidad media, impacto alto).
- Falta de pruebas antes de la entrega (probabilidad media, impacto medio).

## 6. Próximo paso técnico

Cerrar el diseño de la base de datos: validar las entidades (usuario, incidencia, proyecto, comentario, notificación, archivo), sus relaciones y los índices necesarios para REQ-04. Una vez validado, implementar la persistencia y el registro de incidencias. Este paso reduce el riesgo de inconsistencias entre los modelos y la implementación.
