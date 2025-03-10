## **Guía del Proyecto: Sistema Inteligente de Recolección de Residuos**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
Bogotá genera **6,200 toneladas diarias de residuos**, pero el 38% de las rutas de recolección son ineficientes, aumentando emisiones y costos[5][23]. Camiones repiten trayectos, zonas como Suba reportan demoras de 12+ horas, y el sistema carece de adaptación dinámica a eventos como cierres viales. Este proyecto busca optimizar rutas usando datos geoespaciales para reducir tiempos y huella de carbono.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar sistemas de movilidad con datos espaciales.  
2. Automatizar procesos mediante triggers y procedimientos.  
3. Optimizar consultas para grandes volúmenes de datos.  
4. Trabajar con control de versiones en equipo.

---

### **Objetivo General**  
Implementar una base de datos PostgreSQL que permita planificar y ajustar dinámicamente rutas de recolección de basuras, integrando datos geográficos, de tráfico y rendimiento vehicular.

---

### **Objetivos Técnicos**  

1. **Modelado de Redes**  
   - Crear diagrama ER con:  
     - Entidades: `rutas`, `puntos_recoleccion`, `vehiculos`.  
     - Relaciones: Capacidad de vehículos por tipo de residuo (doméstico, reciclaje).  

2. **Georreferenciación Básica**  
   - Almacenar coordenadas de puntos de recolección usando PostGIS (`LINESTRING` para rutas).  
   - Consulta que calcule la ruta más corta entre dos puntos usando pgRouting.  

3. **Automatización Esencial**  
   - Procedimiento almacenado que recalcule rutas diarias basado en alertas de tráfico.  
   - Trigger que actualice el historial de mantenimiento al cambiar estado de un vehículo.  

4. **Optimización Accesible**  
   - Particionar tabla de registros de recolección por mes.  
   - Implementar índice BRIN para consultas por rangos de tiempo.  

5. **Integración de Datos**  
   - Importar dataset CSV con 50+ puntos de recolección usando Python.  
   - Cargar red vial básica desde Shapefile simplificado.  

6. **Colaboración Controlada**  
   - Usar Git para:  
     - Mantener dos branches (`rutas` y `main`).  
     - Realizar **10+ commits** con mensajes como "feat: add traffic-aware routing function".  

7. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts usados para generar consultas de pgRouting.  
     - Adaptaciones realizadas al código generado (ej: ajustar algoritmos de Dijkstra).  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tablas geoespaciales con relaciones Many-to-Many.  
   - Un procedimiento almacenado funcional.  
2. **Script Python** para carga de datos de tráfico/ubicaciones.  
3. **Tres Consultas Clave**:  
   - Ruta óptima evitando vías congestionadas.  
   - Cálculo de distancia total recorrida por vehículo en una semana.  
   - Identificación de puntos con mayor frecuencia de recolección.  
4. **Repositorio GitHub** con:  
   - README.md explicando modelo de datos.  
   - Carpeta `ia` con ejemplos de código generado y modificado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin normalizar o con relaciones incorrectas | ER en 3FN con cardinalidades claras | ER incluye subtipos de vehículos/residuos |  
| **Routing (25%)**     | Consultas básicas sin pgRouting | Uso de Dijkstra para 2+ rutas     | Algoritmo A* con ponderación por tráfico |  
| **Optimización (20%)**| Índices básicos B-tree         | BRIN + Particionamiento           | Uso de EXPLAIN para optimizar JOINs |  
| **Colaboración (15%)**| Menos de 5 commits             | 10+ commits semánticos            | Uso de issues y PRs documentados  |  
| **IA Ética (10%)**    | Código copiado sin adaptar     | 2 prompts documentados con mejoras| Auditoría de errores en código IA |  

---

### **🆘 Recursos de Apoyo**  
1. [Introducción a pgRouting](https://workshop.pgrouting.org/)  
2. [Datos abiertos de rutas - UAESP](https://www.uaesp.gov.co/)  
3. [Ejemplos de BRIN en PostgreSQL](https://www.postgresql.org/docs/current/brin-intro.html)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **equilibrar complejidad geográfica con usabilidad**. No se esperan redes viales completas, sino modelos simplificados que demuestren comprensión de grafos. Se valorará la creatividad en restricciones de routing (ej: evitar colegios en hora de entrada).  

**¡Convierte a Bogotá en ejemplo de gestión inteligente de residos!** 🚛🗺️  
