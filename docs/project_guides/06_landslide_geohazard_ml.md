## **Guía del Proyecto: Monitoreo de Riesgo en Cerros Orientales**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
Los Cerros Orientales de Bogotá presentan **78 zonas de alto riesgo** por deslizamientos, con un incremento del 18% anual debido a deforestación y lluvias extremas[6][21]. Actualmente, el IDIGER utiliza sistemas desconectados para monitoreo pluviométrico y estabilidad de suelos. Este proyecto busca crear una base de datos integrada que permita predecir eventos usando variables geológicas y meteorológicas.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar datos geo-temporales en 3D.  
2. Implementar consultas espaciales básicas.  
3. Automatizar alertas simples.  
4. Colaborar usando control de versiones.

---

### **Objetivo General**  
Desarrollar una base de datos PostgreSQL que integre datos topográficos, mediciones de lluvia y registros históricos de deslizamientos para generar alertas preventivas en zonas críticas.

---

### **Objetivos Técnicos**  

1. **Modelado 3D Básico**  
   - Diagrama ER que incluya:  
     - Entidades: `zonas_riesgo`, `sensores`, `registros_lluvia`.  
     - Uso de PostGIS para almacenar geometrías 3D (`POLYHEDRALSURFACEZ`).  

2. **Análisis Espacial**  
   - Consulta que calcule pendientes mayores a 30° usando PostGIS 3D.  
   - Implementar buffer de 500m alrededor de zonas habitadas.  

3. **Automatización Básica**  
   - Trigger que registre alertas cuando lluvia acumulada en 24h > 80mm.  
   - Vista materializada con zonas en riesgo por temporada.  

4. **Manejo de Series Temporales**  
   - Particionar tabla de lecturas de sensores por mes.  
   - Índice BRIN para consultas por rango de fechas.  

5. **Integración de Datos**  
   - Importar dataset CSV con 200+ registros históricos usando Python.  
   - Cargar modelo digital de elevación (DEM) simplificado desde GeoJSON.  

6. **Colaboración**  
   - Usar Git para:  
     - Mantener dos branches (`dev` y `main`).  
     - Realizar **10+ commits** (ej: "feat: add 3D slope calculation").  

7. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts para generar consultas 3D.  
     - Correcciones a código espacial generado por IA.  

---

### **Entregables Mínimos**  
1. **Esquema SQL** con:  
   - Tabla de zonas de riesgo con geometría 3D.  
   - Trigger funcional para alertas por lluvia.  
   - Dos índices espaciales (GiST).  
2. **Script Python** para carga de datos topográficos.  
3. **Tres Consultas Clave**:  
   - Zonas con pendiente >30° y lluvia acumulada >50mm.  
   - Distancia entre asentamientos informales y zonas de riesgo.  
   - Tendencia mensual de precipitaciones en áreas críticas.  
4. **Repositorio GitHub** con:  
   - README.md explicando modelo de datos.  
   - Carpeta `auditoria_IA` con ejemplos de código generado/modificado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin tipos geométricos 3D    | ER con geometrías PostGIS básicas | ER incluye relaciones de capas geológicas |  
| **Análisis 3D (25%)** | Consultas planas (2D)          | Cálculo básico de pendientes 3D   | Integra curvas de nivel y uso de suelo |  
| **Automatización (20%)** | Alertas no relacionadas con datos | Trigger funcional con umbrales   | Sistema de priorización de alertas |  
| **Optimización (15%)** | Índices básicos               | BRIN + Particionamiento temporal | Uso de CLUSTER en datos espaciales |  
| **Colaboración (10%)** | Menos de 5 commits            | 10+ commits semánticos           | Uso de milestones en GitHub       |  

---

### **🆘 Recursos de Apoyo**  
1. [PostGIS 3D Tutorial](https://postgis.net/docs/using_postgis_dbmanagement.html#PolyhedralSurfaceZ)  
2. [Datos LiDAR Abiertos - IGAC](https://www.igac.gov.co/)  
3. [Registros Históricos de Lluvias - IDEAM](https://www.ideam.gov.co/)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **modelos simplificados pero funcionales**, no en precisión geológica absoluta. Se valorará la identificación clara de supuestos (ej: "Consideramos suelo homogéneo") y documentación de limitaciones. Priorizar comprensión espacial sobre complejidad matemática.  

**¡Convierte datos en prevención de tragedias!** ⚠️🌧️  
