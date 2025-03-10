## **Guía del Proyecto: Sistema de Alertas Tempranas para Inundaciones**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
El 60% de Bogotá está en riesgo de inundaciones, con eventos como la tormenta de abril/2024 causando daños por **$380 mil millones**[5][8]. Barrios como Bosa y Kennedy sufren anegaciones recurrentes debido a lluvias intensas y mala infraestructura de drenaje. Este proyecto busca crear un sistema que cruce datos pluviométricos, topografía y registros históricos para emitir alertas preventivas.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar bases de datos con series temporales.  
2. Implementar consultas espaciales básicas.  
3. Automatizar alertas mediante triggers.  
4. Colaborar usando Git con flujo definido.  

---

### **Objetivo General**  
Implementar una base de datos PostgreSQL que integre datos hidrometeorológicos y geográficos para predecir zonas de riesgo de inundación, generando alertas automatizadas.

---

### **Objetivos Técnicos**  

1. **Modelado Temporal-Espacial**  
   - Crear diagrama ER que incluya:  
     - Entidades: `sensores`, `alertas`, `zonas_riesgo`.  
     - Relaciones: Mediciones horarias vs. capacidad de drenaje por zona.  

2. **Georreferenciación Básica**  
   - Almacenar polígonos de zonas de riesgo con PostGIS (`POLYGON`).  
   - Consulta que identifique sensores dentro de zonas de alto riesgo.  

3. **Series Temporales**  
   - Usar TimescaleDB para almacenar mediciones pluviométricas en hypertables.  
   - Crear vista materializada con promedios diarios de lluvia.  

4. **Automatización Esencial**  
   - Trigger que inserte en `alertas` cuando lluvia supere 50mm/hora.  
   - Procedimiento para generar reportes semanales en PDF (usando PL/Python).  

5. **Optimización**  
   - Particionar tabla de mediciones por cuenca hidrográfica.  
   - Implementar índice GiST para consultas espaciales frecuentes.  

6. **Integración de Datos**  
   - Importar dataset CSV con 200+ registros de lluvia usando Python.  
   - Cargar zonas de riesgo desde GeoJSON simplificado.  

7. **Colaboración**  
   - Usar Git para:  
     - Mantener branch `alertas` para desarrollo de triggers.  
     - Realizar **8+ commits** con mensajes como "fix: adjust flood threshold in trigger".  

8. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts para generar consultas espaciales o triggers.  
     - Modificaciones hechas al código generado (ej: ajustar umbrales).  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tabla `zonas_riesgo` con geometría y capacidad de drenaje.  
   - Hipertable TimescaleDB funcional.  
2. **Script Python** para carga de datos pluviométricos.  
3. **Tres Consultas Clave**:  
   - Sensores activos en zonas de riesgo durante lluvias intensas.  
   - Promedio de precipitación por cuenca en últimos 7 días.  
   - Histórico de alertas activadas por localidad.  
4. **Repositorio GitHub** con:  
   - README.md explicando estructura de alertas.  
   - Evidencia de uso de IA en carpeta `ia_audit`.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin relaciones temporales   | ER integra series temporales y geometrías | ER incluye umbrales dinámicos por zona |  
| **Geospacial (25%)**  | Coordenadas básicas            | Consultas con `ST_Contains`       | Uso de raster para modelos de elevación |  
| **Automatización (20%)**| Trigger no funcional          | Alertas básicas por lluvia        | Alertas multicriterio (lluvia + topografía) |  
| **Optimización (15%)**| Índices básicos                | Particionamiento + GiST           | Uso de BRIN para datos temporales |  
| **IA Ética (10%)**    | Código sin documentar          | 2 casos con mejoras moderadas     | Auditoría detallada de riesgos en código IA |  

---

### **🆘 Recursos de Apoyo**  
1. [Introducción a TimescaleDB](https://docs.timescale.com/tutorials/latest/getting-started/)  
2. [Geodatos de cuencas - IDEAM](https://www.ideam.gov.co/)  
3. [Ejemplos de ST_Contains](https://postgis.net/docs/ST_Contains.html)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **equilibrar precisión científica con practicidad**. No se esperan modelos hidrológicos complejos, sino un MVP que demuestre flujo básico de alertas. Se valorará claridad en umbrales y documentación de supuestos técnicos.  

**¡Ayuda a Bogotá a anticiparse a las inundaciones!** 🌧️🚨  
