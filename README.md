# Back-DB-Postgres CareConnect

## Descripción
El servicio `back-db-postgres` es la base de datos central del sistema CareConnect, desarrollado para el Hospital Universitario San Ignacio. Utiliza PostgreSQL para almacenar y gestionar datos de pacientes, enfermeras, actividades, insumos, visitas, barrios, localidades, y otros dominios relacionados con la gestión de visitas domiciliarias. Se integra con microservicios a través de una red Docker y soporta el portal web y la app móvil.

## Funcionalidades
- **Esquema**: Base de datos con tablas clave:
  - `paciente`: Almacena información de pacientes (ID, nombre, identificación, dirección, coordenadas).
  - `enfermera`: Registra datos de enfermeras (ID, identificación, rol, turno, coordenadas).
  - `actividad`: Gestiona actividades médicas (e.g., procedimientos, medicamentos).
  - `actividad_paciente_visita`: Asocia actividades a pacientes con detalles (dosis, frecuencia, fechas).
  - `instalacion_insumos_paciente` y `insumos_consumidos`: Controla inventario y consumo de insumos.
  - `visita`: Registra visitas (enfermera, actividad, fechas, horarios).
  - `barrio`, `localidad`, `tipo_identificacion`, `rol`, `turno`, `tipo_actividad`: Tablas de referencia.
- **Integridad**: Claves primarias, foráneas, y secuencias para garantizar consistencia.
- **Persistencia**: Volumen Docker para datos (`postgres_data`) y backups (`/backup`).

## Tecnologías
- **Base de Datos**: PostgreSQL 15
- **Orquestación**: Docker Compose (v3.8)
- **Red**: Docker network (`backend-net`)

## Requisitos
- Docker y Docker Compose
- Archivo `.env` con variables:
  ```
  POSTGRES_USER=your_user
  POSTGRES_PASSWORD=your_password
  POSTGRES_DB=neondb
  ```
- Puerto 5432 disponible

## Instalación
1. Clonar el repositorio:
   ```bash
   git clone https://github.com/careconnect/back-db-postgres.git
   cd back-db-postgres
   ```

2. Crear archivo `.env` con las credenciales de la base de datos.

3. Iniciar el servicio con Docker Compose:
   ```bash
   docker-compose up -d
   ```

4. Verificar conexión:
   ```bash
   docker exec -it base-postgres psql -U your_user -d neondb
   ```

   La base de datos estará disponible en `localhost:5432`.

## Uso
- **Conexión**: Usa las credenciales del archivo `.env` para conectar desde microservicios o herramientas como `psql` o pgAdmin.
- **Esquema**: La base de datos `neondb` se crea automáticamente con las tablas y relaciones definidas.
- **Backup**: Los backups se almacenan en el directorio `./backup` mapeado al contenedor.
- **Ejemplo de consulta**:
  ```sql
  SELECT p.nombre, a.name AS actividad
  FROM paciente p
  JOIN actividad_paciente_visita apv ON p.id = apv.paciente_id
  JOIN actividad a ON apv.actividad_id = a.id;
  ```

## Estructura de Datos
- **Tablas Principales**:
  - `paciente` (51 registros, secuencia hasta 51)
  - `enfermera` (45 registros, secuencia hasta 45)
  - `actividad` (39 registros, secuencia hasta 39)
  - `actividad_paciente_visita` (57 registros, secuencia hasta 57)
  - `visita` (237 registros, secuencia hasta 237)
  - `instalacion_insumos_paciente` (31 registros, secuencia hasta 31)
  - `insumos_consumidos` (35 registros, secuencia hasta 35)
  - `barrio` (850 registros, secuencia hasta 850)
- **Tablas de Referencia**:
  - `tipo_identificacion` (4 registros), `rol` (2 registros), `tipo_actividad` (2 registros), `turno` (3 registros)
  - `localidad` (códigos únicos, e.g., '01', '02')

## Autoría
- Juan David González
- Lina María Salamanca
- Laura Alexandra Rodríguez
- Axel Nicolás Caro

**Pontificia Universidad Javeriana**  
**Mayo 26, 2025**