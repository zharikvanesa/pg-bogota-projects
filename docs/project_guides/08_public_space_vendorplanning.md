## **Guía del Proyecto: Gestión Digital de Espacio Público para Comercio Informal**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
El 68% de los conflictos en espacio público de Bogotá se relacionan con el comercio informal[9][15], donde 127,000 vendedores carecen de asignación formal. En localidades como La Candelaria y Chapinero, esto genera congestión peatonal y tensiones con autoridades. Este proyecto busca crear un sistema para asignar zonas temporales mediante criterios técnicos y mejorar la convivencia urbana.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar sistemas con restricciones espacio-temporales.  
2. Implementar seguridad básica de datos sensibles.  
3. Automatizar procesos mediante triggers.  
4. Trabajar con datos geográficos en PostgreSQL.

---

### **Objetivo General**  
Desarrollar una base de datos que gestione la asignación dinámica de espacios públicos para vendedores informales, integrando georreferenciación, horarios y tipos de mercancía.

---

### **Objetivos Técnicos**  

1. **Modelado Flexible**  
   - Diagrama ER que incluya:  
     - Entidades: `vendedores`, `zonas_asignadas`, `tipos_mercancia`.  
     - Uso de JSONB para atributos variables (permisos temporales, requerimientos especiales).  

2. **Geolocalización Básica**  
   - Almacenar ubicación de zonas con PostGIS (`POLYGON` o `POINT`).  
   - Consulta que detecte asignaciones solapadas espacial y temporalmente.  

3. **Seguridad y Auditoría**  
   - Encriptar datos personales de vendedores usando `pgcrypto`.  
   - Trigger que registre cambios en tabla `asignaciones_histórico`.  

4. **Automatización Esencial**  
   - Procedimiento almacenado que asigne zonas basado en antigüedad y tipo de mercancía.  
   - Vista materializada con reporte diario de zonas disponibles.  

5. **Integración de Datos**  
   - Importar dataset CSV con 50+ vendedores usando Python.  
   - Cargar polígonos de zonas permitidas desde GeoJSON simplificado.  

6. **Colaboración Controlada**  
   - Usar Git para:  
     - Mantener branch `asignaciones` y `main`.  
     - Realizar **8+ commits** con mensajes como "feat: add conflict detection query".  

7. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts usados para generar consultas espaciales.  
     - Modificaciones a código generado para cumplir normativas locales.  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tabla `zonas` con geometría y horarios (JSONB).  
   - Trigger de auditoría funcional.  
2. **Script Python** para carga de datos con validación básica.  
3. **Cuatro Consultas Clave**:  
   - Detección de solapamientos espaciales/temporales.  
   - Listado de vendedores por tipo de mercancía y localidad.  
   - Cálculo de densidad de vendedores por m².  
   - Consulta de disponibilidad horaria por zona.  
4. **Repositorio GitHub** con:  
   - README.md explicando políticas de asignación.  
   - Archivo `ia_usage.md` con ejemplos de código generado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin tipos de mercancía      | ER con relaciones N-M y JSONB     | Modelo incluye restricciones jerárquicas (ej: zonas prioritarias) |  
| **Geolocalización (25%)** | Coordenadas básicas         | Consultas que detectan solapamientos | Uso de operadores espaciales avanzados (`ST_Overlaps`, `ST_Within`) |  
| **Seguridad (20%)**   | Datos sensibles en texto plano | Cifrado básico con pgcrypto       | RLS por roles (admin/vendedor)    |  
| **Automatización (15%)** | Triggers no funcionales      | Procedimiento de asignación semi-automático | Sistema prioriza vendedores con discapacidad |  
| **IA Ética (10%)**    | Código copiado sin adaptar     | 2 casos documentados con mejoras  | Validación humana de sugerencias IA |  

---

### **🆘 Recursos de Apoyo**  
1. [PostGIS para Gestión Urbana](https://postgis.net/workshops/postgis-intro/)  
2. [Manejo de JSONB en PostgreSQL](https://www.postgresqltutorial.com/postgresql-json/)  
3. [Datos de espacio público - SCRD](https://www.culturarecreacionydeporte.gov.co/)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **balancear flexibilidad y control**. No se espera un sistema de asignación en tiempo real, sino un modelo base que demuestre comprensión de restricciones espaciotemporales. Se valorará la inclusión de criterios sociales en el diseño (ej: priorización a madres cabeza de familia).  

**¡Convierte el caos en orden para una Bogotá más incluyente!** 🛒🗺️  
