Sql Server en practica
---
## 📘 CURSO 1: SQL Server – Nivel Básico
**Duración total:** 32 horas  
**Horas clase:** 24 horas  
**Horas asesoria:** 8 horas  
**Horarios:** sábados 3 horas (8 semanas), asesorías 1 hora (entre semana), con fecha y hora a convenir en la sesión sabatina previa.  
**Objetivo:** Instalar, configurar y administrar bases de datos simples, además de escribir y ejecutar consultas T-SQL fundamentales.    
**Requisitos previos:** Conocimientos básicos de informática y lógica relacional.

| Tema | Horas | Enfoque | Descripción |
|------|-------|---------|-------------|
| 1. Arquitectura y Ediciones de SQL Server | 2 | Admin | Motor de base de datos, servicios, SSMS, ediciones (Express/Standard/Enterprise) y límites |
| 2. Instalación y Configuración Inicial | 2 | Admin | Proceso de instalación, autenticación (Windows/SQL), configuración de red, instancias nombradas vs. predeterminadas |
| 3. Fundamentos de Modelado Relacional | 2 | Admin/Prog | Tablas, tipos de datos, claves primarias/foráneas, normalización 1NF-3NF, restricciones básicas |
| 4. Programación T-SQL Esencial | 4 | Prog | `SELECT`, `WHERE`, `ORDER BY`, operadores lógicos, funciones escalares, `INSERT/UPDATE/DELETE`, manejo de nulos |
| 5. Objetos de Base de Datos Básicos | 3 | Prog/Admin | Vistas simples, índices básicos, `DEFAULT`, `CHECK`, `UNIQUE`, reglas de integridad |
| 6. Seguridad Básica y Control de Acceso | 2 | Admin | Logins, usuarios de BD, roles fijos (`db_datareader`, `db_datawriter`), `GRANT/REVOKE/DENY` |
| 7. Copias de Seguridad y Restauración | 3 | Admin | `BACKUP DATABASE/LOG`, diferenciales, restauración simple, verificación con `RESTORE VERIFYONLY` |
| 8. Transacciones y Control de Flujo | 3 | Prog | `BEGIN TRAN`, `COMMIT`, `ROLLBACK`, `IF/ELSE`, `WHILE`, variables locales, `TRY...CATCH` básico |
| 9. Taller Integrador y Evaluación | 3 | Mix | Diseño de BD simple, programación de scripts administrativos, restauración de backup, examen práctico |

---
## 📗 CURSO 2: SQL Server – Nivel Intermedio
**Duración total:** 64 horas  
**Horas clase:** 48 horas  
**Horas asesoria:** 16 horas   
**Horarios:** sábados 3 horas (12 semanas), asesorías 1 hora (entre semana), con fecha y hora a convenir en la sesión sabatina previa.  
**Objetivo:** Dominar consultas complejas, programación modular, indexación operativa y administración proactiva.   
**Requisitos previos:** Nivel básico o experiencia equivalente administrando/consultando SQL Server.  

| Tema | Horas | Enfoque | Descripción |
|------|-------|---------|-------------|
| 1. Consultas Avanzadas y Funciones de Ventana | 15 | Prog | `JOIN` complejos, CTEs recursivas y no recursivas, subconsultas correlacionadas, `ROW_NUMBER`, `RANK`, `LAG/LEAD`, `PIVOT/UNPIVOT` |
| 2. Programación Modular: SPs y Funciones | 15 | Prog | Stored Procedures (parámetros `OUTPUT`, `RECOMPILE`, validación), UDFs escalares y de tabla, reutilización y rendimiento |
| 3. Triggers y Gestión de Eventos | 5 | Prog | `AFTER` vs `INSTEAD OF`, auditoría básica con triggers, limitaciones, cuándo evitarlos y alternativas modernas |
| 4. Transacciones, Concurrencia y Bloqueos | 5 | Admin/Prog | Niveles de aislamiento, `READ COMMITTED SNAPSHOT`, locks, deadlocks, `NOLOCK`/hints, detección con `sp_who2` y DMVs |
| 5. Indexación y Optimización Básica | 5 | Admin/Prog | Estructura B-Tree, clustered vs nonclustered, estadísticas, fragmentación, planes de ejecución básicos, tuning guiado por índices |
| 6. Seguridad Operativa y Cumplimiento | 4 | Admin | Roles personalizados, esquemas, enmascaramiento de datos, auditoría con `SERVER AUDIT`, cifrado básico (TDE conceptual) |
| 7. Estrategias de Backup, Restore y Mantenimiento | 4 | Admin | RPO/RTO, `CHECKDB`, planes de mantenimiento automatizados, limpieza de logs y archivos |
| 8. Monitoreo y Diagnóstico Operativo | 4 | Admin | DMVs/DLFs, Extended Events básicos, alertas, registro de errores, métricas de CPU/IO/memoria, dashboards iniciales |
| 9. Automatización con SQL Server Agent | 3 | Admin | Jobs, steps, schedules, notificaciones por correo, logging de ejecuciones, administración de tareas recurrentes |
| 10. Proyecto Integrador y Evaluación | 4 | Mix | Caso empresarial: diseño, programación de SPs/triggers, indexación, backup/restore, monitoreo y presentación técnica |

---
## 📙 CURSO 3: SQL Server – Nivel Avanzado
**Duración total:** 64 horas  
**Horas clase:** 48 horas  
**Horas asesoria:** 16 horas  
**Horarios:** sábados 3 horas (12 semanas), asesorías 1 hora (entre semana), con fecha y hora a convenir en la sesión sabatina previa.  
**Objetivo:** Diseñar soluciones con optimización a nivel enterprise, automatización y gobernanza moderna.  
**Requisitos previos:** Nivel intermedio + experiencia real en entornos productivos o entornos de práctica intensiva.

| Tema | Horas | Enfoque | Descripción |
|------|-------|---------|-------------|
| 1. Optimización Avanzada de Consultas | 5 | Admin/Prog | Planes de ejecución avanzados, wait stats, parameter sniffing, query hints, reoptimización, Query Store (forzar/eliminar planes), baselines |
| 2. Estrategias de Indexación y Particionamiento | 10 | Admin | Índices filtrados, columnstore, índices espaciales/JSON, mantenimiento inteligente, particionamiento de tablas e índices, archiving |
| 3. T-SQL Avanzado y Programación Moderna | 30 | Prog | SQL dinámico seguro (`sp_executesql`), manejo avanzado de errores, transacciones distribuidas, CLR (conceptos), JSON/XML nativo, temp tables vs table variables |
| 4. Seguridad Empresarial y Hardening | 5 | Admin | Row-Level Security, Dynamic Data Masking avanzado, Always Encrypted, auditoría granular, integración con AD/Azure AD, políticas de cumplimiento (GDPR/LFPDPPP) |
| 5. Automatización para Bases de Datos | 5 | Admin | PowerShell + `dbatools`|
| 7. Monitoreo Proactivo y Capacity Planning | 5 | Admin | Extended Events avanzados, análisis de Query Store, creación de baselines, Resource Governor, integración con Grafana/Prometheus, alertas predictivas |
| 9. Arquitectura de Datos y ETL/ELT Básico | 3 | Prog/Admin | Introducción a Data warehousing, staging patterns, SSIS introductorio, calidad de datos y metadatos básicos |
| 10. Proyecto Final | 3 | Mix | Resolución de deadlocks masivos, optimización de cargas ETL pesadas, diseño de arquitectura HA/DR, revisión por pares |
