## **Guía del Proyecto: Sistema de Monitoreo Hídrico para Bogotá**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
En 2024, Bogotá enfrentó su **peor crisis hídrica en 40 años**, con el sistema Chingaza al 34% de capacidad y racionamientos afectando a 9 millones de personas[8][23]. Actualmente, la EAAB monitorea niveles manualmente, sin integración con datos de consumo por localidad. Este proyecto busca crear una base de datos predictiva para optimizar la distribución y detectar fugas.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar bases de datos con series temporales.  
2. Implementar alertas automatizadas básicas.  
3. Trabajar con datos geohidrológicos.  
4. Colaborar usando control de versiones.

---

### **Objetivo General**  
Desarrollar una base de datos PostgreSQL que integre mediciones de embalses, consumo por zona y red de acueducto, permitiendo predecir déficits hídricos y priorizar intervenciones.

---

### **Objetivos Técnicos**  

1. **Modelado de Series Temporales**  
   - Diagrama ER que incluya:  
     - Entidades: `embalses`, `sensores`, `zonas_consumo`.  
     - Relaciones: Capacidad histórica vs consumo promedio por UPZ.  

2. **HiperTablas Básicas**  
   - Usar TimescaleDB para almacenar mediciones horarias de niveles.  
   - Consulta que calcule promedio móvil de 7 días por embalse.  

3. **Geolocalización Esencial**  
   - Almacenar red de acueducto con PostGIS (`LINESTRING`).  
   - Consulta que identifique tuberías cercanas a zonas de alto consumo.  

4. **Automatización Básica**  
   - Trigger que active alerta cuando nivel de embalse baje del 20%.  
   - Vista materializada con consumo diario por localidad.  

5. **Optimización**  
   - Particionar tabla de mediciones por cuenca hidrográfica.  
   - Índice BRIN para consultas por rango temporal.  

6. **Integración de Datos**  
   - Importar dataset CSV con 1000+ registros de consumo usando Python.  
   - Cargar coordenadas de 5 embalses desde GeoJSON simplificado.  

7. **Colaboración**  
   - Usar Git para:  
     - Mantener dos branches (`sensors` y `main`).  
     - Realizar **8+ commits** (ej: "fix: adjust reservoir capacity calculation").  

8. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts para generar consultas temporales/espaciales.  
     - Modificaciones a código generado por IA.  

---

### **Entregables Mínimos**  
1. **Esquema SQL** con:  
   - Tabla de series temporales en TimescaleDB.  
   - Trigger funcional para alertas.  
   - Dos consultas espaciales.  
2. **Script Python** para carga de datos de sensores.  
3. **Tres Consultas Clave**:  
   - Consumo semanal por zona vs capacidad almacenada.  
   - Tuberías en riesgo por proximidad a zonas de bajo nivel.  
   - Detección de anomalías en flujo (ej: valores ±3σ del promedio).  
4. **Repositorio GitHub** con:  
   - README.md explicando modelo de datos.  
   - Carpeta `ia_usage` con ejemplos de código generado/modificado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin tipos temporales        | ER con tablas para series temporales | ER incluye relaciones de presión en tuberías |  
| **Series Temporales (25%)** | Uso básico de TIMESTAMP     | Hipertable en TimescaleDB         | Consultas con funciones de ventana |  
| **Georreferenciación (20%)** | Coordenadas simples        | Consultas de proximidad funcionales | Análisis de redes con PostGIS     |  
| **Automatización (15%)** | Alertas no funcionales      | Trigger actualiza tabla de eventos | Integra notificaciones por email  |  
| **Colaboración (10%)** | Commits sin mensajes claros   | 8+ commits semánticos             | Uso de tags y releases en GitHub  |  

---

### **🆘 Recursos de Apoyo**  
1. [Guía TimescaleDB para Iniciantes](https://docs.timescale.com/tutorials/latest/quick-start/)  
2. [Datos Abiertos EAAB](https://www.acueducto.com.co/)  
3. [Ejemplos PostGIS para Redes](https://postgis.net/workshops/postgis-intro/network.html)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **equilibrar complejidad hidrológica con usabilidad**. No se esperan modelos de predicción avanzados, sino un sistema básico que demuestre integración entre datos temporales y espaciales. Se valorará documentación clara de supuestos (ej: "Consideramos pérdidas del 8% en red").  

**¡Sé parte de la solución para garantizar el agua en Bogotá!** 💧📉  
