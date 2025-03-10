**Listado de proyectos grupales con problemáticas locales de la ciudad de Bogotá** 
**Nivel:** Avanzado

---

### **1. Proyecto: Sistema de Monitoreo de Calidad del Aire con Georreferenciación**  
**Problema:** PM2.5 en Bogotá supera 2.7x los límites de la OMS, afectando zonas vulnerables[2][19].  
**Objetivo BD:**  
- Almacenar datos históricos de contaminación por localidad.  
- Relacionar niveles de PM2.5 con hospitalizaciones respiratorias.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS**: Georreferenciar estaciones de monitoreo y calcular áreas de impacto.  
   ```sql  
   -- Ej: Zonas dentro de 1km de estaciones con alerta roja
   SELECT * FROM zonas_salud 
   WHERE ST_DWithin(geometria, (SELECT ubicacion FROM estaciones WHERE id = 'E12'), 1000);
   ```  
2. **Window Functions**: Identificar tendencias temporales (ej: contaminación por hora en Kennedy).  
3. **Particionamiento**: Dividir tabla de mediciones por año y localidad.  
4. **Triggers**: Alertar cuando nuevos datos superen umbrales críticos.  

**Dataset Sugerido:**  
- [Datos abiertos calidad del aire Bogotá](https://datosabiertos.bogota.gov.co/dataset/calidad-del-aire)  
- [Registros hospitalarios Secretaría de Salud](https://saludata.saludcapital.gov.co/)

---

### **2. Proyecto: Plataforma de Gestión de Asentamientos Informales**  
**Problema:** 34% de habitantes en viviendas informales sin servicios básicos[1][10].  
**Objetivo BD:**  
- Mapear asentamientos y priorizar intervenciones.  
- Gestionar subsidios cruzando datos de estratificación y migración.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **JSONB**: Almacenar estructuras variables (materiales de vivienda, acceso a servicios).  
   ```sql  
   -- Ej: Buscar viviendas con riesgo de inundación
   SELECT * FROM asentamientos 
   WHERE atributos->>'riesgo' = 'inundación' 
     AND atributos->>'material_techo' = 'lamina';
   ```  
2. **Full-Text Search**: Analizar informes sociales en texto libre (ej: "faltan alcantarillado").  
3. **Row-Level Security**: Restringir acceso a datos sensibles por rol (ingenieros vs trabajadores sociales).  
4. **pgcrypto**: Encriptar datos personales de migrantes venezolanos.  

**Dataset Sugerido:**  
- [Censo de asentamientos informales 2023](https://www.catastrobogota.gov.co/)  
- [Registro Único de Migrantes](https://www.migracioncolombia.gov.co/)

---

### **3. Proyecto: Optimización de Rutas para el Sistema de Basuras**  
**Problema:** 6,200 toneladas diarias de residuos con rutas ineficientes que aumentan emisiones[5][23].  
**Objetivo BD:**  
- Reducir tiempos de recolección mediante planificación dinámica.  
- Calcular huella de carbono por ruta.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PgRouting**: Calcular rutas óptimas para camiones (evitar congestiones en hora pico).  
   ```sql  
   -- Encontrar ruta más corta evitando vías con tráfico > 50%
   SELECT * FROM pgr_dijkstra('
       SELECT id, source, target, cost 
       FROM vias 
       WHERE congestion < 0.5', 125, 789);
   ```  
2. **Procedimientos Almacenados**: Recalcular rutas diarias basado en alertas de tráfico.  
3. **Índices BRIN**: Para consultas temporales en horarios de recolección.  
4. **Foreign Data Wrappers**: Integrar datos de Waze/Google Maps en tiempo real.  

**Dataset Sugerido:**  
- [Geodatabase de rutas de basuras UAESP](https://www.uaesp.gov.co/)  
- [Datos históricos de tráfico](https://www.movilidadbogota.gov.co/)

---

### **4. Proyecto: Sistema de Alertas Tempranas para Inundaciones**  
**Problema:** Tormentas como la de 2024 causan $380M en daños[5].  
**Objetivo BD:**  
- Predecir inundaciones usando datos pluviométricos y topografía.  
- Coordinar respuestas con entidades de emergencia.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS Raster**: Analizar modelos de elevación digital (DEM) de quebradas.  
2. **TimescaleDB (Extensión)**: Almacenar series temporales de lluvias en tiempo real.  
   ```sql  
   -- Crear hypertable para datos de sensores
   SELECT create_hypertable('datos_lluvia', 'fecha_hora');
   ```  
3. **Triggers + NOTIFY**: Enviar alertas por Websockets cuando se active umbral crítico.  
4. **Materialized Views**: Reportes consolidados para Defensa Civil.  

**Dataset Sugerido:**  
- [Sensores pluviométricos IDEAM](https://www.ideam.gov.co/)  
- [Mapas de riesgo por localidad](https://www.idiger.gov.co/)

---

### **5. Proyecto: Plataforma de Seguridad Vial para Motociclistas**  
**Problema:** Motos implicadas en 61% de accidentes, con 200% de crecimiento[3][7].  
**Objetivo BD:**  
- Identificar puntos críticos de accidentes.  
- Vincular infracciones a planes de seguros.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS + pgRouting**: Mapear "puntos calientes" y rutas alternas seguras.  
2. **Índices GIN**: Búsqueda por patrones de placa (ej: motos con multas recurrentes).  
3. **Logical Replication**: Sincronizar datos con cámaras de vigilancia en tiempo real.  
4. **PL/Python**: Analizar fotos de cámaras (usando OpenCV) para detectar cascos.  

**Dataset Sugerido:**  
- [Registro Nacional de Accidentes](https://www.ansv.gov.co/)  
- [Multas Secretaría de Movilidad](https://www.movilidadbogota.gov.co/)

---
Aquí tienes **5 nuevos proyectos grupales** enfocados en problemáticas específicas de Bogotá, integrando funcionalidades avanzadas de PostgreSQL y datasets locales:

---

### **6. Proyecto: Plataforma de Gestión de Agua para Crisis Hídricas**  
**Problema:** Racionamiento de agua afectando a 9 millones de habitantes en 2024[8][23].  
**Objetivo BD:**  
- Monitorear niveles de reservorios y distribución por localidades.  
- Detectar fugas en tiempo real usando datos de sensores.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **TimescaleDB (Hypertables)**: Almacenar series temporales de niveles de agua en Chingaza.  
   ```sql  
   CREATE TABLE mediciones_agua (
       tiempo TIMESTAMPTZ NOT NULL,
       nivel FLOAT,
       ubicacion_id INT
   );
   SELECT create_hypertable('mediciones_agua', 'tiempo');
   ```  
2. **PostGIS Network Topology**: Modelar red de tuberías y simular flujos.  
3. **Triggers + pg_notify**: Alertar cuando fugas superen el 15% en una zona.  
4. **Particionamiento por Rango**: Dividir datos históricos por cuencas hidrográficas.  

**Dataset Sugerido:**  
- [Datos de niveles de embalses - EAAB](https://www.acueducto.com.co/)  
- [Mapas de red de acueducto](https://geoportal.dadep.gov.co/)  

---

### **7. Proyecto: Sistema de Predicción de Deslizamientos en Cerros Orientales**  
**Problema:** Deforestación en Andes aumenta riesgo de deslizamientos un 18% anual[6][21].  
**Objetivo BD:**  
- Cruzar datos pluviométricos, uso de suelo y alertas tempranas.  
- Priorizar reubicaciones en zonas de alto riesgo.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS Raster + 3D**: Analizar modelos de pendientes con QGIS integration.  
2. **PL/R**: Ejecutar modelos predictivos de machine learning (ej: Random Forest).  
3. **BRIN Indexes**: Consultar rápidamente datos satelitales históricos.  
4. **Foreign Data Wrappers**: Integrar datos de Copernicus EU sobre humedad de suelos.  

**Ejemplo de Consulta:**  
```sql  
SELECT zona_id, 
       ST_3DArea(geometria) AS area_riesgo,
       prediccion_deslizamiento(lluvia, pendiente) AS riesgo
FROM zonas_criticas
WHERE riesgo > 0.8;
```  

**Dataset Sugerido:**  
- [Datos LiDAR de cerros - IGAC](https://www.igac.gov.co/)  
- [Alertas tempranas del SIAT](https://www.siat.gov.co/)  

---

### **8. Proyecto: Optimización de Mercados Campesinos con Comercio Justo**  
**Problema:** 42% de productores rurales no acceden a canales directos de venta[10][20].  
**Objetivo BD:**  
- Conectar productores de Sumapaz con mercados urbanos.  
- Reducir desperdicios alimenticios mediante demanda predictiva.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **JSON Schema Validation**: Estandarizar datos de productos agroecológicos.  
   ```sql  
   ALTER TABLE productos 
   ADD CONSTRAINT valida_origen 
   CHECK (atributos @? '$.certificaciones.organico' == true);
   ```  
2. **Graph Databases (Apache Age)**: Modelar cadenas de suministro eficientes.  
3. **Materialized Views**: Reportes diarios de precios en Corabastos.  
4. **Full-Text Search en Español**: Buscar productos por denominación de origen.  

**Dataset Sugerido:**  
- [Censo Agropecuario - DANE](https://www.dane.gov.co/)  
- [Precios mayoristas - Corabastos](https://www.corabastos.com.co/)  

---

### **9. Proyecto: Plataforma de Salud Pública para Enfermedades Respiratorias**  
**Problema:** 40% más hospitalizaciones respiratorias en estratos bajos[1][18].  
**Objetivo BD:**  
- Relacionar calidad del aire con registros clínicos.  
- Optimizar ubicación de centros de atención.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS Voronoi Diagrams**: Determinar áreas de cobertura óptima para clínicas.  
2. **Row-Level Security**: Proteger historias médicas según roles (médicos vs investigadores).  
3. **Window Functions**: Identificar tendencias epidemiológicas por UPZ.  
4. **pg_stat_statements**: Monitorear consultas frecuentes para cache estratégico.  

**Ejemplo de Análisis:**  
```sql  
WITH datos_salud AS (
    SELECT localidad, 
           COUNT(*) FILTER (WHERE enfermedad = 'IRA') AS casos_ira,
           AVG(pm25) AS contaminacion_promedio
    FROM hospitalizaciones
    JOIN calidad_aire USING (fecha, zona)
    GROUP BY ROLLUP(localidad)
)
SELECT * FROM datos_salud 
WHERE contaminacion_promedio > 25;
```  

**Dataset Sugerido:**  
- [Registros hospitalarios - SaluData](https://saludata.saludcapital.gov.co/)  
- [Datos demográficos - Bogotá Cómo Vamos](https://bogotacomovamos.org/)  

---

### **10. Proyecto: Sistema de Gestión de Espacio Público para Vendedores Informales**  
**Problema:** 68% de conflicto en espacio público por comercio informal[9][15].  
**Objetivo BD:**  
- Asignar zonas temporales de venta mediante subastas dinámicas.  
- Medir impacto en movilidad peatonal.  

**Funcionalidades PostgreSQL recomendadas:**  
1. **PostGIS Network Analysis**: Calcular flujos peatonales y optimizar ubicaciones.  
2. **Procedimientos Almacenados con PL/pgSQL**: Automatizar subastas horarias.  
3. **Transactional Outbox Pattern**: Sincronizar datos con app móvil en tiempo real.  
4. **Indexes on Expressions**: Buscar vendedores por antigüedad y tipo de mercancía.  

**Ejemplo de Subasta:**  
```sql  
CREATE OR REPLACE FUNCTION asignar_zona_subasta() 
RETURNS TRIGGER AS $$
BEGIN
    UPDATE zonas_venta 
    SET asignada_a = NEW.vendedor_id,
        precio = NEW.oferta * 1.1  -- Margen dinámico
    WHERE zona_id = NEW.zona_id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```  

**Dataset Sugerido:**  
- [Censo de vendedores informales - Secretaría de Gobierno](https://www.gobiernobogota.gov.co/)  
- [Datos de movilidad peatonal - SCRD](https://www.culturarecreacionydeporte.gov.co/)  

---

### **Recursos Clave**  
1. [PostgreSQL para Ciudades Inteligentes](https://postgis.net/workshops/postgis-intro/)  
2. [Geodatos Abiertos Bogotá](https://geoportal.dadep.gov.co/)  
3. [Guía de Análisis Espacial con PostGIS](https://postgis.net/docs/manual-3.4/)  
