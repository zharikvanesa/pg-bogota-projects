## **Guía del Proyecto: Sistema de Correlación Climática-Salud Respiratoria**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
En Bogotá, los habitantes de estratos 1-2 tienen **40% más hospitalizaciones por IRA** que estratos altos, vinculado a la exposición a PM2.5 y condiciones húmedas en zonas marginales[1][18]. Actualmente, los datos de salud y clima están fragmentados, dificultando políticas preventivas. Este proyecto creará una base de datos para identificar patrones de riesgo y optimizar recursos médicos.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Integrar datasets heterogéneos (climáticos y médicos).  
2. Implementar consultas espaciales básicas.  
3. Garantizar privacidad de datos sensibles.  
4. Colaborar usando control de versiones.

---

### **Objetivo General**  
Diseñar una base de datos PostgreSQL que relacione variables climáticas (calidad del aire, humedad) con registros hospitalarios, permitiendo identificar zonas prioritarias para intervención.

---

### **Objetivos Técnicos**  

1. **Modelado con Datos Temporales**  
   - Diagrama ER en 3FN que integre:  
     - `hospitalizaciones` (fecha, diagnóstico, edad).  
     - `clima` (PM2.5, humedad, ubicación).  
     - Relación muchos-a-muchos entre zonas y hospitales.  

2. **Análisis Espacial Básico**  
   - Almacenar ubicaciones de hospitales como `POINT` en PostGIS.  
   - Consulta que cuente hospitalizaciones dentro de 2km de estaciones con PM2.5 > 25 µg/m³.  

3. **Seguridad Esencial**  
   - Enmascarar nombres de pacientes usando `pgcrypto` (ej: `encode(sha256(...))`).  
   - Vista materializada anonimizada para análisis públicos.  

4. **Automatización Básica**  
   - Trigger que actualice un indicador de riesgo al insertar nuevos datos climáticos.  
   - Función que calcule el promedio móvil de PM2.5 por zona.  

5. **Optimización**  
   - Particionar tabla de hospitalizaciones por trimestre.  
   - Índice GIST para consultas espaciales frecuentes.  

6. **Integración de Datos**  
   - Importar dataset CSV de 500+ hospitalizaciones usando Python.  
   - Cargar coordenadas de 10 hospitales desde GeoJSON.  

7. **Colaboración Controlada**  
   - Usar Git para:  
     - Dos branches (`clima` y `salud`).  
     - **8+ commits** con mensajes como "feat: add hospital geolocation".  

8. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts para generar consultas de correlación.  
     - Modificaciones al código generado (ej: corregir JOINs incorrectos).  

---

### **Entregables Mínimos**  
1. **Esquema SQL** con:  
   - 4 tablas principales + 2 estructuras de seguridad (ej: vista anonimizada).  
   - Un trigger funcional y un índice espacial.  
2. **Script Python** para carga y enmascaramiento de datos.  
3. **Tres Consultas Clave**:  
   - Correlación semanal PM2.5 vs hospitalizaciones.  
   - Zonas con >10 hospitalizaciones y PM2.5 alto.  
   - Hospitales con mayor carga en épocas lluviosas.  
4. **Repositorio GitHub** con:  
   - README.md explicando relaciones entre tablas.  
   - Carpeta `ia` con ejemplos de código generado/adaptado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente (0-4)**         | **Satisfactorio (5-7)**           | **Excelente (8-10)**              |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin normalizar o relaciones incorrectas | ER en 3FN con claves foráneas | ER incluye tablas puente para zonas-hospitales |  
| **Espacial (25%)**    | Solo coordenadas sin PostGIS   | 2+ consultas con operadores espaciales | Uso de JOIN espacial para correlacionar datos |  
| **Seguridad (20%)**   | Datos sensibles visibles       | Enmascaramiento básico con hash   | Vista anonimizada + RLS por roles |  
| **Optimización (15%)**| Índices básicos                | Particionamiento + GIST          | Uso de EXPLAIN para optimizar consultas |  
| **IA Ética (10%)**    | Código no documentado          | 2 prompts con adaptaciones menores| Auditoría crítica de código IA    |  

---

### **🆘 Recursos de Apoyo**  
1. [Geolocalización con PostGIS](https://postgis.net/workshops/postgis-intro/).  
2. [Datos de Calidad del Aire Bogotá](https://datosabiertos.bogota.gov.co/dataset/calidad-del-aire).  
3. [Tutorial pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html).  

---

**⚠️ Nota Pedagógica:**  
Priorizar **claridad sobre complejidad**: no se esperan modelos predictivos avanzados, sino un diseño robusto que permita escalar. Se valorará especialmente la capacidad de cruzar variables aparentemente inconexas (ej: humedad + PM2.5).  

**¡Conviértete en arquitecto de datos para la salud pública!** 🏥🌦️  
