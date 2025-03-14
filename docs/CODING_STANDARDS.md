# Estándares de Codificación 🌍🗄️

## Tabla de contenido

1. [Convenciones de Nombrado Mixtas](#1-convenciones-de-nombrado-mixtas)
    <br> 1.1. [Principios Básicos](#11-principios-básicos)
    <br> 1.2. [Reglas Explícitas](#12-reglas-explícitas)
    <br> 1.3. [Comentarios en Código](#13-comentarios-en-código)
    <br> 1.4. [Ejemplos de Código](#15-ejemplos-de-código)
    <br> 1.5. [Glosario Bilingüe](#16-glosario-bilingüe)
    <br> 1.6. [Justificación de la Estrategia](#17-justificación-de-la-estrategia)
2. [Control de Versiones con Git](#2-control-de-versiones-con-git)
    <br> 2.1 [Estructura de Commits (Conventional Commits)](#21-estructura-de-commits-conventional-commits)
    <br> 2.2 [Estrategia de Branching](#22-estrategia-de-branching)
    <br> 2.3 [Relación Issues-Commit](#23-relación-issues-commit)
    <br> 2.4 [Lineamientos para Pull Requests](#24-lineamientos-para-pull-requests)
3. [Estructura de Proyecto Recomendada](#3-estructura-de-proyecto-recomendada)
    <br> 3.1 [Explicación por Sección](#31-explicación-por-sección)
    <br> 3.2 [Notas Clave para Estudiantes](#32-notas-clave-para-estudiantes)
    <br> 3.3 [Reglas para Nombrado de Archivos](#33-reglas-para-nombrado-de-archivos)   

---

## 1. Convenciones de Nombrado Mixtas

Este documento establece las normas de codificación para proyectos de bases de datos urbanas en Bogotá. La estrategia de codificación combina términicos técnicos en inglés con dominio en español para facilitar la colaboración global y local.

### 1.1 Principios Básicos

El siguiente código muestra cómo se pueden combinar términos técnicos en inglés con dominio en español. Las líneas de código están comentadas para explicar el razonamiento detrás de cada elección de idioma.

```sql
-- Estructuras técnicas en inglés + Dominio en español
CREATE TABLE air_quality_measurements (  -- Inglés técnico
    measurement_id SERIAL PRIMARY KEY,
    localidad VARCHAR(50) NOT NULL CHECK (localidad IN ('Usaquén', 'Kennedy')),  -- Término local
    pm25 NUMERIC(5,2),  -- Sigla técnica internacional
    geom GEOMETRY(Point, 4686)  -- Tipo PostGIS en inglés
);
```

### 1.2 Reglas Explícitas

La siguiente tabla resume las reglas de codificación para diferentes componentes de bases de datos. Se recomienda seguir estas reglas para mantener la coherencia en el código.

| **Componente**       | **Idioma** | **Ejemplo**                | **Excepción**                     |
|-----------------------|------------|----------------------------|------------------------------------|
| Nombres de tablas     | Inglés     | `transport_routes`         | Datasets oficiales: `SISBEN`      |
| Columnas técnicas     | Inglés     | `geom`, `created_at`       | Campos legales: `codigo_divipola` |
| Dominio bogotano      | Español    | `estrato_socioeconomico`   |                                    |
| Funciones PostGIS     | Inglés     | `ST_Buffer`, `ST_Transform`|                                    |
| Restricciones         | Inglés     | `CHECK (pm25 > 0)`         |                                    |

### 1.3 Comentarios en Código

Los comentarios en el código deben estar en español para facilitar la comprensión de los desarrolladores locales. Se recomienda incluir una descripción breve y clara de cada componente. Puede utilizar COMMENT ON en PostgreSQL para documentar tablas y columnas.

```sql
/* 
TABLA: water_management_systems
Propósito: Almacena sistemas de acueducto veredal en Bogotá
Creado por: Equipo HidroBogotá
Última actualización: 2024-03-20 (Agregado campo caudal)
*/
CREATE TABLE water_management_systems (
    system_id UUID PRIMARY KEY,
    nombre_vereda VARCHAR(80) NOT NULL,  -- Nombre oficial según alcaldía
    caudal_promedio NUMERIC(10,2),  -- m³/s
    geom GEOMETRY(Polygon, 4686)
);

COMMENT ON COLUMN water_management_systems.nombre_vereda IS 
    'Nombre oficial según resolución 1234 de 2020 de la Alcaldía';
```

### 1.4 Ejemplos de código

A continuación, se presentan ejemplos de código que combinan términos técnicos en inglés con dominio en español. Estos ejemplos ilustran cómo se pueden aplicar las convenciones de codificación en diferentes contextos.

#### 1.4.1 Ejemplo Consulta con PostGIS

El siguiente ejemplo muestra una consulta espacial que calcula el área de los sistemas de acueducto veredal en Bogotá. La consulta utiliza funciones PostGIS en inglés y nombres de columnas en español.

```sql
-- Muestra contaminación por UPZ con geometrías
WITH upz_contaminacion AS (
    SELECT
        u.nombre_upz,
        AVG(m.pm25) AS promedio_pm25,
        u.geom
    FROM 
        air_quality_measurements m
        JOIN upz_boundaries u  -- Capa límites UPZ en inglés
            ON ST_Within(m.geom, u.geom)
    WHERE
        m.fecha BETWEEN '2023-01-01' AND '2023-12-31'
    GROUP BY 
        u.nombre_upz, u.geom
)
SELECT 
    nombre_upz AS "Unidad de Planeamiento Zonal",
    ROUND(promedio_pm25::numeric, 2) AS "PM2.5 Promedio (μg/m³)",
    ST_Area(geom) AS area_m2  -- Función PostGIS en inglés
FROM upz_contaminacion
ORDER BY promedio_pm25 DESC;
```

#### 1.4.2 Ejemplo Función con Validación

El siguiente ejemplo muestra una función PL/pgSQL que valida rutas de transporte público en Bogotá. La función combina términos técnicos en inglés con mensajes de error en español para comunicar claramente las restricciones.

```sql
/**
FUNCIÓN: validate_transport_route
Propósito: Valida rutas de transporte contra restricciones distritales
Parámetros:
  - ruta_geom: Geometría LineString de la ruta
  - tipo_servicio: 'SITP' o 'TransMilenio' (español para dominio específico)
*/
CREATE OR REPLACE FUNCTION validate_transport_route(
    ruta_geom GEOMETRY,
    tipo_servicio VARCHAR(20)
) RETURNS BOOLEAN AS $$
BEGIN
    -- Restricción 1: No cruzar zonas protegidas
    IF EXISTS (
        SELECT 1 
        FROM protected_zones pz
        WHERE ST_Intersects(ruta_geom, pz.geom)
    ) THEN
        RAISE EXCEPTION 'Ruta intersecta zona protegida';
    END IF;

    -- Restricción 2: Longitud máxima según servicio
    CASE tipo_servicio
        WHEN 'SITP' THEN
            IF ST_Length(ruta_geom) > 25000 THEN  -- 25 km
                RETURN FALSE;
            END IF;
        WHEN 'TransMilenio' THEN
            IF ST_Length(ruta_geom) > 15000 THEN  -- 15 km
                RETURN FALSE;
            END IF;
        ELSE
            RAISE EXCEPTION 'Tipo de servicio no válido: %', tipo_servicio;
    END CASE;

    RETURN TRUE;
END;
$$ LANGUAGE plpgsql;
```

#### 1.4.3 Ejemplo Reglas de Validación

El siguiente ejemplo muestra cómo se pueden definir reglas de validación en una tabla de bases de datos. Las reglas de validación utilizan términos técnicos en inglés para las restricciones y mensajes en español para la comunicación con los usuarios.

```sql
-- Mix de idiomas técnicos y mensajes en español
ALTER TABLE public_transport_routes
    ADD CONSTRAINT chk_valid_vehicle_type 
    CHECK (vehicle_type IN ('Articulado', 'Alimentador', 'Dual'));

CREATE INDEX idx_rutas_transporte_geom 
    ON public_transport_routes USING GIST (geom);

COMMENT ON INDEX idx_rutas_transporte_geom IS 
    'Índice espacial para optimizar consultas de rutas';
```

### 1.5 Glosario Bilingüe

El siguiente glosario bilingüe proporciona una lista de términos técnicos en inglés y sus equivalentes en español. Estos términos se pueden utilizar en la documentación y el código para mantener la coherencia y facilitar la comprensión de los desarrolladores.

| **Término Técnico (Inglés)** | **Equivalente Contextual (Español)** |  
|------------------------------|--------------------------------------|  
| `buffer`                     | zona_amortiguacion                  |  
| `trigger`                    | disparador                          |  
| `index`                      | indice                              |  
| `routing`                    | enrutamiento                        |  
| `constraint`                 | restriccion                         |  
| `schema`                     | esquema                             |
| `query`                      | consulta                            |
| `join`                       | unir/unión                                |
| `view`                       | vista                               |

### 1.6 Justificación de la Estrategia

La estrategia de codificación mixta se basa en la necesidad de combinar estándares internacionales con requisitos locales específicos. Algunas de las razones clave para esta estrategia son:

1. **Colaboración Global:**  
   - Compatibilidad con herramientas (GitHub, PostGIS, Metabase)
   - Integración con estándares internacionales

2. **Contexto Local:**  
   - Mantiene significado en dominios específicos bogotanos
   - Alinea con datasets oficiales (Ej: `codigo_upz`)

3. **Eficiencia:**  
   - Reduce traducciones forzadas (Ej: `ST_Transform > ST_Transformar`)
   - Mejora legibilidad para equipos multilingües

---

## 2. Control de Versiones con Git

El control de versiones es esencial para la colaboración efectiva en proyectos de bases de datos urbanas. La siguiente sección describe las convenciones y prácticas recomendadas para el uso de Git en proyectos técnicos.

### 2.1 Estructura de Commits (Conventional Commits)

Los mensajes de commit deben seguir el formato de [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). Este formato permite una fácil clasificación y seguimiento de los cambios realizados en el proyecto.

```bash
# Formato:
<tipo>[alcance(opcional)]: <descripción>

# Ejemplos en español con tipos técnicos en inglés:
git commit -m "feat(spatial): add PostGIS routing for garbage collection"
git commit -m "fix(auth): corregir validación de roles en función de usuario"
git commit -m "docs(quality): actualizar protocolo de medición PM2.5"

# Tipos permitidos:
| Tipo         | Escenario                                  |
|--------------|--------------------------------------------|
| feat         | Nueva funcionalidad                       |
| fix          | Corrección de errores                     |
| refactor     | Cambios que no corrigen errores ni añaden features |
| docs         | Actualización de documentación            |
| test         | Adición o modificación de pruebas         |
| db           | Cambios en estructura de BD (migraciones) |
| style        | Cambios de formato (espacios, comas, etc) |
| chore        | Tareas de mantenimiento general           |
```

### 2.2 Estrategia de Branching

Se recomienda seguir un flujo de trabajo basado en GitFlow para la gestión de ramas en el proyecto. Este flujo incluye ramas principales para la integración continua y ramas de características para el desarrollo de nuevas funcionalidades.

```markdown
# Flujo principal:
main        → Versión estable (protegida)
develop     → Integración continua
feature/*   → Nuevas funcionalidades
hotfix/*    → Correcciones críticas

# Ejemplo para nueva feature:
1. git checkout -b feature/spatial-indexes develop
2. Realizar cambios + commits semánticos
3. git push origin feature/spatial-indexes
4. Crear Pull Request hacia develop
```

### 2.3 Relación Issues-Commit

Es importante vincular los commits a las issues correspondientes para mantener un seguimiento claro de los cambios realizados en el proyecto. Esto facilita la revisión de código y la resolución de problemas.

```markdown
# Vincular commits a issues:
git commit -m "feat: add CO2 calculation model refs #45, #78"

# Cierre automático de issues:
git commit -m "fix: resolve invalid geom validation closes #123"

# Sintaxis soportada:
- closes #123
- fixes #45, #78
- resolves #100
```

### 2.4 Lineamientos para Pull Requests

Los Pull Requests deben incluir una descripción detallada de los cambios realizados, así como una lista de verificación de requisitos de aprobación. Esto facilita la revisión y aprobación por parte de los compañeros de equipo.

1. **Revisión de Código:**
   ```markdown
   ### Cambios Propuestos
   - [x] Añadido índice espacial para consultas de calidad del aire
   - [x] Corregido cálculo de rutas óptimas

   ### Pruebas Realizadas
   - Ejecutado EXPLAIN ANALYZE en consultas principales
   - Validación con dataset de 10k registros

   ### Issues Relacionados
   Resuelve #45, #78
   ```

2. **Requisitos de Aprobación:**
   - ✅ Al menos 2 aprobaciones de compañeros
   - ✅ Todos los tests en GitHub Actions pasan
   - ✅ Documentación actualizada

---

## 3. Estructura de Proyecto Recomendada

La estructura de un proyecto de bases de datos urbanas en Bogotá debe seguir una organización clara y coherente para facilitar la colaboración y el mantenimiento. La siguiente estructura de proyecto se basa en las mejores prácticas de la industria.

```bash
Proyecto_1_Calidad_Aire/            # Nombre del proyecto (ej: 1_Calidad_Aire)
│
├── data/
│   ├── raw/                        # Datos originales (CSV, GeoJSON, Shapefiles)
│   │   ├── estaciones_ideam.csv    # Datos crudos de 14 estaciones (ID, nombre, lat, lon)
│   │   └── hospitalizaciones.json  # Registros de salud 2023-2024
│   │
│   └── processed/                  # Datos transformados (listos para carga)
│       ├── estaciones_normalizadas.csv  # Sin duplicados, coordenadas en WGS84
│       └── aire_hospital.sql        # Datos unificados para carga masiva
│
├── database/
│   ├── ddl/                        # Definición de estructura
│   │   ├── 01_tables.sql           # CREATE TABLE estaciones, mediciones...
│   │   ├── 02_indexes.sql          # Índices y optimizaciones -> CREATE INDEX idx_pm25 ON mediciones...
│   │   └── 03_security.sql         # RLS y pgcrypto -> ALTER TABLE... ENABLE ROW LEVEL SECURITY
│   │
│   ├── dml/                        # Datos iniciales
│   │   └── initial_data.sql        # Inserciones básicas
│   │
│   ├── functions/                  # Lógica de negocio
│   │   ├── calcular_promedio.sql
│   │   ├── generar_alerta.sql
│   │   └── calcular_ica.sql        # Función que calcula Índice de Calidad del Aire
│   │
│   └── triggers/                   # Automatizaciones
│       ├── alerta_contaminacion.sql
│       └── alerta_pm25.sql         # Trigger que inserta en tabla alertas
│
├── etl/                            # Scripts de transformación
│   ├── data_cleaning.py           # Limpieza con Pandas, Ej: Elimina registros con PM2.5 < 0
│   ├── load_to_postgres.py        # Carga a la BD, Ej: Usa psycopg2 para cargar CSV a PostgreSQL
│   └── requirements.txt           # Dependencias Python (si aplica): pandas==2.0.3, sqlalchemy==2.0.0
│
├── docs/                           # Documentación
│   ├── er_diagram.md              # Modelo Entidad-Relación mermaid
│   ├── decisions.md               # Justificación técnica, Ej: # "¿Por qué elegimos PostGIS sobre MongoDB?
│   ├── legal/                     # Documentos legales
│   │   ├── privacy_policy.md      # Política de privacidad
│   │   └── consent.md             # Consentimiento de datos
│   │
│   └── ia_audit/                  # Uso de IA
│       ├── prompts_usados.md      # Historial de prompts, Ej: "Genera función SQL para promedio semanal PM2.5"
│       └── codigo_modificado/     # Modificaciones al código generado por ChatGPT
│
├── tests/                         # Pruebas básicas
│   ├── test_queries.sql           # SELECT * FROM alertas WHERE...
│   └── test_triggers.sql          # Simula inserción que debe activar alerta
├── README.md                      # Instrucciones generales
```

### 3.1. **Explicación por Sección**  
1. **`data/`**  
   - **Raw:** Datos originales *inalterados* (preservar fuentes).  
     *Ej: CSV descargado del [Sistema de Monitoreo de Bogotá](https://datosabiertos.bogota.gov.co/)*.  
   - **Processed:** Datos limpios y estructurados para la BD.  
     *Ej: CSV con coordenadas convertidas a WGS84 usando Python*.

2. **`database/`**  
   - **DDL:** Scripts SQL ordenados secuencialmente:  
     ```sql
     -- 01_tables.sql
     CREATE TABLE estaciones (
         id SERIAL PRIMARY KEY,
         nombre VARCHAR(100),
         ubicacion GEOGRAPHY(POINT)
     );
     ```
   - **Functions/Triggers:** Lógica reusable y automatizaciones.  

3. **`etl/`**  
   - Scripts en Python para ETL (Extraer, Transformar, Cargar).  
     *Ej: `data_cleaning.py` filtra registros inválidos de PM2.5*.

4. **`docs/`**  
   - **IA Audit:** Registro ético del uso de IA.  
     *Ej: Captura de prompt usado en ChatGPT y cómo se mejoró su solución*.

5. **`tests/`**  
   - Validaciones básicas:  
     ```sql
     -- test_triggers.sql
     INSERT INTO mediciones (pm25, estacion_id) VALUES (35, 1);  -- Debe generar alerta
     ```

### 3.2. **Notas Clave para Estudiantes**  
1. **Versionado Incremental:**  
   - Numerar scripts SQL (`01_`, `02_`) para controlar orden de ejecución.  
2. **Git Diario:**  
   - Hacer commit al final de cada sesión de trabajo con mensajes descriptivos:  
     *"feat: add trigger for weekly averages calculation"*.  
3. **Testing Básico:**  
   - Antes de hacer merge a `main`, ejecutar `test_queries.sql` para verificar datos.  
4. **Consideraciones Legales:**  
   - Si usan datos reales de pacientes, agregar anonimización en `data_cleaning.py`.  

### 3.3. **Reglas para Nombrado de Archivos**

Los archivos en el proyecto deben seguir una convención de nombres clara y coherente para facilitar la identificación y el mantenimiento. Se recomienda utilizar un formato descriptivo y consistente para todos los archivos.

1. **SQL**:  
   - `[orden]_[tipo]_[descripción].sql`  
   - Ej: `02_dml_insert_hospitalizaciones.sql`  

2. **Python**:  
   - `[proceso]_[fuente]_[destino].py`  
   - Ej: `transform_calidad_aire_postgres.py`  

3. **Geodatos**:  
   - `[tipo]_[localidad]_[año].[ext]`  
   - Ej: `poligonos_ciudad_bolivar_2024.geojson`

