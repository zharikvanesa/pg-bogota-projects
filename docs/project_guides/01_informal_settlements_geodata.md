## **Guía del Proyecto: Plataforma de Gestión de Asentamientos Informales**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  

Bogotá alberga más de **900 asentamientos informales** donde el 34% de sus habitantes carece de acceso formal a servicios básicos[1][10]. Estos territorios, concentrados en localidades como Ciudad Bolívar y Usme, presentan riesgos de inundaciones, deslizamientos y exclusión social. Actualmente, los censos se realizan manualmente con formularios en papel, lo que dificulta priorizar intervenciones. Este proyecto busca crear una base de datos geoespacial para gestionar mejoras habitacionales y asignación de subsidios.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Diseñar bases de datos con datos semiestructurados.  
2. Implementar búsquedas avanzadas en texto libre.  
3. Gestionar seguridad básica de datos sensibles.  
4. Colaborar usando control de versiones.

---

### **Objetivo General**  
Implementar una base de datos PostgreSQL que centralice información de asentamientos informales con capacidad para: mapear riesgos, gestionar subsidios cruzados, y generar reportes para tomadores de decisiones.

---

### **Objetivos Técnicos**  

1. **Modelado Flexible**  
   - Crear un diagrama ER que incluya:  
     - Entidades principales: `asentamientos`, `familias`, `visitas_tecnicas`.  
     - Uso de JSONB para atributos variables (materiales de construcción, servicios disponibles).  

2. **Búsquedas Semánticas**  
   - Implementar Full-Text Search en español sobre campos de texto libre (descripciones de riesgos, necesidades reportadas).  
   - Crear una columna `tsvector` con índice GIN para acelerar búsquedas.  

3. **Seguridad Básica**  
   - Encriptar datos sensibles de beneficiarios usando `pgcrypto` (ej: número de documento).  
   - Implementar una política de RLS (Row Level Security) para restringir acceso por rol (`inspector`, `administrador`).  

4. **Geolocalización Esencial**  
   - Almacenar coordenadas de asentamientos usando PostGIS (`POINT`).  
   - Realizar consulta que cuente asentamientos en radio de 5km desde estaciones de bomberos.  

5. **Automatización Básica**  
   - Crear trigger que actualice fecha de última inspección al agregar nueva visita técnica.  
   - Generar vista materializada con resumen de riesgos por localidad.  

6. **Integración de Datos**  
   - Importar dataset CSV de 100+ asentamientos usando Python/Pandas.  
   - Cargar polígonos de riesgo desde archivo GeoJSON simplificado.  

7. **Colaboración Controlada**  
   - Usar Git para:  
     - Mantener dos branches (`desarrollo` y `main`).  
     - Realizar **8+ commits** con mensajes descriptivos (ej: "feat: add encryption for ID numbers").  

8. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts usados para generar consultas espaciales o triggers.  
     - Adaptaciones realizadas al código generado por IA.  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tres tablas principales con campos JSONB.  
   - Un índice GIN y una política de RLS funcional.  
2. **Script Python** para carga de datos con cifrado de campos sensibles.  
3. **Cuatro Consultas Clave**:  
   - Búsqueda semántica en descripciones (ej: "inundación" AND "laminas").  
   - Cálculo de densidad poblacional por zona de riesgo.  
   - Listado de familias elegibles para subsidios (WHERE condiciones JSONB).  
   - Consulta espacial de proximidad a servicios de salud.  
4. **Repositorio GitHub** con:  
   - README.md explicando estructura de datos y políticas de seguridad.  
   - Carpeta `documentacion_IA` con capturas de prompts y modificaciones.  

---

### **Rúbrica Simplificada**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 |  
|-----------------------|--------------------------------|-----------------------------------|  
| **Modelado**          | JSONB mal implementado        | Uso efectivo de JSONB para atributos variables |  
| **Búsquedas**         | LIKE básico                   | Full-Text Search con índice GIN   |  
| **Seguridad**         | Datos sensibles en texto plano| Cifrado funcional con pgcrypto    |  
| **Colaboración**      | Commits sin mensajes claros   | 8+ commits con convención semántica |  

---

### **🆘 Recursos de Apoyo**  
1. [Tutorial JSONB en PostgreSQL](https://www.postgresqltutorial.com/postgresql-json/)  
2. [Cifrado básico con pgcrypto](https://www.cybertec-postgresql.com/en/postgresql-pgcrypto-encrypt-your-data/)  
3. [Shapefiles de Bogotá](https://geoportal.dadep.gov.co/)  

---

**⚠️ Nota Pedagógica:**  
El foco debe estar en el **diseño flexible** que permita adaptarse a la diversidad de asentamientos. No se espera perfecta normalización de campos JSONB, pero sí un modelo coherente que priorice escalabilidad sobre complejidad. Se valorará especialmente la documentación de decisiones técnicas.  

**¡Conviértete en agente de cambio para la Bogotá invisible!** 🏘️🔍  
