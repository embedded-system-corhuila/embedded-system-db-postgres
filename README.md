# embedded-system-db-postgres

> PostgreSQL relational database for structured operational data and traceability in the Embedded Hardware-Software System.  
> *Base de datos relacional PostgreSQL para datos operativos estructurados y trazabilidad del Sistema Embebido Hardware-Software.*

---

## Language / Idioma

- [English Documentation](#en--english)
- [Documentación en Español](#es--español)

---

## EN | English

### Overview

This repository manages the relational database component of the system. It is powered by PostgreSQL and serves as the single source of truth for structured operational data, device registry, and end-to-end data traceability. All data stored here is subject to ACID compliance guarantees.

### Role in the Ecosystem

PostgreSQL is strictly reserved for relational, structured, and operationally critical data. It is consumed directly by the Core API (`embedded-system-api-core`) for read and write operations. It does not store analytics workloads, time-series bulk data, or machine learning model outputs; those are handled by MongoDB (`embedded-system-db-mongo`).

Primary responsibilities:

- Device registry and state management.
- Operational configuration data.
- User management and access control.
- Structured traceability logs (data flow origin, processing stage, and destination).
- Critical telemetry records requiring referential integrity.

### Technology Stack

| Component | Technology |
|:---|:---|
| Database Engine | PostgreSQL 16+ |
| Schema Migrations | Flyway / Alembic |
| Deployment | Docker / Docker Compose |

### Prerequisites

- Docker 24+ and Docker Compose v2+
- Open network port: `5432`

### Project Structure

```
embedded-system-db-postgres/
├── migrations/            # Versioned schema migration scripts
├── seeds/                 # Initial data scripts for development
├── scripts/               # Utility scripts (backup, restore, health check)
├── docker-compose.yml
└── README.md
```

### Environment Variables

| Variable | Description | Default |
|:---|:---|:---|
| `POSTGRES_DB` | Database name | `embedded_db` |
| `POSTGRES_USER` | Database user | `embedded_user` |
| `POSTGRES_PASSWORD` | Database password | *(required)* |
| `POSTGRES_PORT` | Exposed port | `5432` |

### Setup and Deployment

```bash
# Start the database
docker compose up -d

# View database logs
docker compose logs -f db

# Connect to the database via psql
docker compose exec db psql -U embedded_user -d embedded_db

# Stop the database
docker compose down
```

### Best Practices

- All schema changes must be applied through versioned migration scripts; direct DDL modifications to a running database are not permitted.
- Large binary objects (BLOBs) and unstructured analytics data must not be stored in this database; use MongoDB for that purpose.
- Database credentials must never be committed to version control; use environment files or a secrets manager.

---

## ES | Español

### Descripción General

Este repositorio gestiona el componente de base de datos relacional del sistema. Funciona sobre PostgreSQL y actúa como la única fuente de verdad para los datos operativos estructurados, el registro de dispositivos y la trazabilidad de extremo a extremo. Todos los datos aquí almacenados están sujetos a las garantías de cumplimiento ACID.

### Rol en el Ecosistema

PostgreSQL está estrictamente reservado para datos relacionales, estructurados y operativamente críticos. Es consumido directamente por la API Core (`embedded-system-api-core`) para operaciones de lectura y escritura. No almacena cargas de trabajo analíticas, datos en serie de tiempo masivos ni salidas de modelos de aprendizaje automático; esos son responsabilidad de MongoDB (`embedded-system-db-mongo`).

Responsabilidades principales:

- Registro de dispositivos y gestión de su estado.
- Datos de configuración operativa.
- Gestión de usuarios y control de acceso.
- Registros de trazabilidad estructurada (origen del flujo de datos, etapa de procesamiento y destino).
- Registros de telemetría crítica que requieren integridad referencial.

### Stack Tecnológico

| Componente | Tecnología |
|:---|:---|
| Motor de Base de Datos | PostgreSQL 16+ |
| Migraciones de Esquema | Flyway / Alembic |
| Despliegue | Docker / Docker Compose |

### Prerequisitos

- Docker 24+ y Docker Compose v2+
- Puerto de red abierto: `5432`

### Estructura del Proyecto

```
embedded-system-db-postgres/
├── migrations/            # Scripts de migración de esquema versionados
├── seeds/                 # Scripts de datos iniciales para desarrollo
├── scripts/               # Scripts de utilidad (backup, restore, health check)
├── docker-compose.yml
└── README.md
```

### Variables de Entorno

| Variable | Descripción | Valor por defecto |
|:---|:---|:---|
| `POSTGRES_DB` | Nombre de la base de datos | `embedded_db` |
| `POSTGRES_USER` | Usuario de la base de datos | `embedded_user` |
| `POSTGRES_PASSWORD` | Contraseña de la base de datos | *(requerido)* |
| `POSTGRES_PORT` | Puerto expuesto | `5432` |

### Configuración y Despliegue

```bash
# Iniciar la base de datos
docker compose up -d

# Ver logs de la base de datos
docker compose logs -f db

# Conectar a la base de datos vía psql
docker compose exec db psql -U embedded_user -d embedded_db

# Detener la base de datos
docker compose down
```

### Buenas Prácticas

- Todos los cambios de esquema deben aplicarse mediante scripts de migración versionados; las modificaciones DDL directas sobre una base de datos en ejecución no están permitidas.
- Los objetos binarios grandes (BLOBs) y los datos analíticos no estructurados no deben almacenarse en esta base de datos; utilizar MongoDB para ese propósito.
- Las credenciales de la base de datos nunca deben ser versionadas en el repositorio; utilizar archivos de entorno o un gestor de secretos.
