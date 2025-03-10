## **Guía del Proyecto: Sistema de Gestión de Seguridad para Motociclistas**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
El **61% de los accidentes en Bogotá** involucran motocicletas, con un aumento del 200% en registros por aplicaciones de delivery[3][7]. Zonas como Kennedy y Engativá presentan "puntos calientes" donde ocurren el 45% de estos siniestros, relacionados con exceso de velocidad y falta de uso de cascos. Este proyecto busca crear una base de datos geoespacial para identificar zonas de riesgo y vincular infracciones con planes de seguros.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Diseñar bases de datos con componentes espaciales y de seguridad.  
2. Implementar automatizaciones básicas con triggers.  
3. Optimizar consultas para análisis geoespacial.  
4. Gestionar colaboración mediante control de versiones.  

---

### **Objetivo General**  
Implementar una base de datos PostgreSQL que permita:  
- Identificar zonas de alto riesgo mediante análisis espacial.  
- Relacionar infracciones de tránsito con costos de seguros.  
- Generar alertas preventivas para motociclistas.  

---

### **Objetivos Técnicos**  

1. **Modelado de Datos**  
   - Diagrama ER con entidades: `accidentes`, `motociclistas`, `infracciones`.  
   - Uso de JSONB para atributos variables (ej: detalles del accidente, multas).  

2. **Georreferenciación Básica**  
   - Almacenar coordenadas de accidentes usando PostGIS (`POINT`).  
   - Consulta que identifique 5 "puntos calientes" con clusterización espacial.  

3. **Automatización Esencial**  
   - Trigger que actualice el costo del seguro al registrar 3+ infracciones.  
   - Vista materializada con estadísticas mensuales por localidad.  

4. **Seguridad de Datos**  
   - Encriptar números de licencia con `pgcrypto`.  
   - Política RLS para restringir acceso a datos sensibles (rol `aseguradora` vs `policia`).  

5. **Integración de Datos**  
   - Importar dataset CSV con 200+ registros de accidentes usando Python.  
   - Cargar ubicación de cámaras de vigilancia desde GeoJSON.  

6. **Colaboración Controlada**  
   - Usar Git para:  
     - Mantener dos branches (`dev` y `main`).  
     - Realizar **8+ commits** con convención semántica (ej: "fix: adjust accidentes PK").  

7. **Uso Ético de IA**  
   - Documentar en informe:  
     - Dos prompts para generar consultas geoespaciales.  
     - Modificaciones a código generado por IA (ej: ajustar radio de búsqueda).  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - Tabla `accidentes` con campo espacial y 3 índices (GIST, BRIN, B-tree).  
   - Función para calcular densidad de accidentes por km².  
2. **Script Python** para carga y encriptación de datos.  
3. **Cuatro Consultas Clave**:  
   - Zonas con +10 accidentes en últimos 6 meses (buffer 500m).  
   - Motociclistas con 3+ infracciones y su costo de seguro actualizado.  
   - Relación entre tipo de infracción y gravedad de accidentes (JSONB).  
   - Ruta más segura entre dos puntos evitando "zonas calientes".  
4. **Repositorio GitHub** con:  
   - README.md explicando políticas de seguridad.  
   - Carpeta `ia_usage` con capturas de diálogos con IA.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | Entidades faltantes o sin PK   | ER completo con relaciones N:M    | Subtipos para infracciones (ej: graves/leves) |  
| **Georreferenciación (25%)** | Coordenadas sin PostGIS       | 2+ consultas espaciales útiles    | Clusterización dinámica con ST_ClusterDBSCAN |  
| **Automatización (20%)**| Triggers no funcionales        | Trigger actualiza 1 campo         | Procedimiento que recalcula primas periódicamente |  
| **Seguridad (15%)**   | Datos sensibles visibles       | pgcrypto básico + 1 política RLS  | Encriptación AES-256 + roles detallados |  
| **Colaboración (10%)**| Commits sin mensajes claros    | 8+ commits semánticos             | Uso de tags/releases en GitHub    |  

---

### **🆘 Recursos de Apoyo**  
1. [PostGIS para Análisis de Densidad](https://postgis.net/docs/ST_ClusterDBSCAN.html)  
2. [Datos Abiertos de Accidentes - Secretaría de Movilidad](https://www.movilidadbogota.gov.co/)  
3. [Guía de pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html)  

---

**⚠️ Nota Pedagógica:**  
Priorizar **soluciones prácticas sobre complejidad técnica**. Por ejemplo: un modelo básico de cálculo de primas (aumento del 20% por 3 infracciones) es aceptable. ¡Creatividad en las propuestas de seguridad vial será valorada!  

**¡Contribuye a salvar vidas en las calles de Bogotá!** 🏍️🛑  