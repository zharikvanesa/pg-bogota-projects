## **Guía del Proyecto: Sistema de Monitoreo de Calidad del Aire con Georreferenciación**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

### **Contexto**

Bogotá enfrenta una crisis crónica de calidad del aire, con niveles de PM2.5 que superan **2.3 veces** (35 µg/m³ vs 15 µg/m³) las recomendaciones de la OMS[2][19]. En 2024 se registraron 6,500 muertes prematuras relacionadas con la contaminación, concentradas en localidades del sur como Kennedy y Ciudad Bolívar[1][5]. Aunque la ciudad cuenta con 14 estaciones de monitoreo, los datos se almacenan en silos y no se correlacionan con indicadores de salud pública, lo que limita la acción preventiva. Este proyecto tiene como fin crear una base de datos integrada que permita tomar decisiones basadas en evidencia georreferenciada.

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Diseñar bases de datos que integren datos espaciales.  
2. Automatizar alertas básicas utilizando SQL.  
3. Trabajar colaborativamente mediante Git.  
4. Emplear herramientas de IA de forma ética en el desarrollo.

### **Objetivo General**  
Diseñar e implementar una base de datos en PostgreSQL que integre datos georreferenciados de calidad del aire y registros de salud pública de Bogotá, permitiendo identificar patrones espaciales y temporales de riesgo para la toma de decisiones preventivas.

### **Objetivos Técnicos**

1. **Modelado Conceptual**  
   - Crear un diagrama Entidad-Relación (ER) normalizado hasta la 3FN que integre:  
     - Estaciones de monitoreo con coordenadas geográficas.  
     - Mediciones horarias de PM2.5 y NO₂.  
     - Hospitalizaciones por enfermedades respiratorias.

2. **Georreferenciación Básica**  
   - Implementar al menos dos tablas que utilicen tipos de datos espaciales de PostGIS (`GEOMETRY` o `GEOGRAPHY`).  
   - Realizar una consulta que calcule la distancia entre tres estaciones de monitoreo y hospitales clave.

3. **Automatización con Triggers**  
   - Crear un trigger en PL/pgSQL que registre en una tabla `alertas` cuando los niveles de PM2.5 superen los 25 µg/m³ durante más de dos horas consecutivas.

4. **Optimización Accesible**  
   - Aplicar particionamiento por rango de fechas en la tabla `mediciones`.  
   - Implementar al menos dos tipos de índices (B-tree para fechas y GiST para datos espaciales).

5. **Integración de Datos Reales**  
   - Importar un dataset en formato CSV de calidad del aire utilizando Python y Pandas.  
   - Cargar las coordenadas de cinco estaciones a partir de un archivo GeoJSON o un Shapefile simplificado.

6. **Colaboración Controlada**  
   - Usar Git para:  
     - Crear dos branches (por ejemplo, `desarrollo` y `main`).  
     - Realizar **10+ commits** con mensajes significativos (ejemplo: "feat: add trigger for PM2.5 alerts").

7. **Análisis Básico**  
   - Generar una vista materializada que muestre el promedio semanal de PM2.5 por localidad.  
   - Crear una consulta que relacione tres días de alta contaminación con hospitalizaciones (usando JOIN y cláusula WHERE).

8. **Uso Ético de IA**  
   - Documentar en un informe:  
     - Dos prompts utilizados en ChatGPT/Copilot para generar código SQL.  
     - Las modificaciones realizadas al código generado para corregir errores o mejorar su funcionalidad.

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tres tablas principales y dos tablas de soporte (por ejemplo, `alertas` y `localidades`).  
   - Un trigger funcional.
2. **Script en Python** que cargue datos desde un archivo CSV a PostgreSQL.  
3. **Cinco consultas demostrativas** (dos espaciales, dos analíticas y una de optimización).  
4. **Repositorio GitHub** con:  
   - Una historia de commits coherente.  
   - Un archivo README.md que contenga la documentación mínima (modelo ER, descripción de tablas e instrucciones de uso).

#### **Estructura Sugerida del Repositorio GitHub**  
```
data/
├── grupo_00/
│   ├── README.md
│   ├── database/
│   │   ├── ddl/
│   │   │   └── tables.sql
│   │   ├── dml/
│   │   │   └── inserts.sql
│   │   ├── functions/
│   │   │   └── calcular_co2.sql
│   │   └── triggers/
│   ├── processed/         # Scripts de limpieza y datos procesados
│   ├── raw/               # Datos crudos (CSV, Shapefiles)
│   └── etl/               # Scripts en Python/Shell
├── grupo_01/
```

### **Rúbrica Simplificada**  
| **Criterio**          | **Insuficiente**           | **Satisfactorio**                   |  
|-----------------------|----------------------------|-------------------------------------|  
| **Modelado**          | Diagrama ER sin normalizar | ER en 3FN con relaciones claras     |  
| **Georreferenciación**| Uso exclusivo de coordenadas| Dos o más consultas espaciales útiles|  
| **Colaboración**      | Commits sin estructura     | 10+ commits con mensajes significativos  |  
| **Uso Ético de IA**   | Uso de código sin modificar| Dos casos documentados con mejoras  |

---

### **🆘 Recursos de Apoyo**  
1. [PostGIS para Principiantes](https://postgis.net/workshops/postgis-intro/)  
2. [Ejemplos de Triggers en PostgreSQL](https://www.postgresqltutorial.com/postgresql-triggers/)

---

**¿Listo para hacer que Bogotá respire mejor?** 🌬️ ¡Comienza con el modelado básico y escala progresivamente!

---

**Nota Pedagógica:**  
El enfoque debe estar en el **aprendizaje incremental**. No se esperan consultas espaciales excesivamente complejas ni particionamientos multidimensionales avanzados. Se valorará principalmente la claridad del modelo y la calidad de la documentación.
