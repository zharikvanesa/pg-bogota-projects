## **Guía del Proyecto: Conexión Campo-Ciudad para Mercados Agroecológicos**  
**Nivel:** 3er semestre de Ingeniería de Sistemas  
**Conocimientos Previos:** Conocimientos básicos de SQL, nociones iniciales de Python y modelado entidad-relación  

---

### **Contexto**  
El 42% de los pequeños productores de Sumapaz y Usme no acceden a mercados formales, desperdiciando 18% de sus cosechas[10][20]. Simultáneamente, Bogotá tiene 1.2M de hogares que pagan sobreprecios por alimentos en cadenas comerciales. Este proyecto busca crear un sistema que conecte directamente a campesinos con consumidores urbanos, garantizando precios justos y reduciendo desperdicios.

---

### **🎯 Objetivos de Aprendizaje**  
Al finalizar este proyecto, podrás:  
1. Modelar bases de datos para comercio electrónico básico.  
2. Implementar búsquedas avanzadas en productos agrícolas.  
3. Gestionar datos semi-estructurados con JSONB.  
4. Colaborar usando Git en un contexto realista.

---

### **Objetivo General**  
Implementar una base de datos PostgreSQL que gestione catálogos de productos agrícolas, órdenes de compra y logística de entrega, optimizando la cadena de suministro campo-ciudad.

---

### **Objetivos Técnicos**  

1. **Modelado Flexible**  
   - Diagrama ER con:  
     - Entidades: `productores`, `productos`, `pedidos`.  
     - Uso de JSONB para atributos variables (certificaciones, temporadas de cosecha).  

2. **Búsquedas Avanzadas**  
   - Implementar Full-Text Search en español para descripciones de productos.  
   - Crear vista materializada con disponibilidad estacional (ej: "frutas de junio").  

3. **Geolocalización Básica**  
   - Almacenar ubicación de fincas con PostGIS (`POINT`).  
   - Consulta que calcule distancia entre finca y punto de entrega más cercano.  

4. **Seguridad Esencial**  
   - Encriptar datos de contacto de productores usando `pgcrypto`.  
   - Implementar función que genere códigos de seguimiento únicos para pedidos.  

5. **Automatización**  
   - Trigger que actualice stock tras cada pedido.  
   - Procedimiento para generar reportes semanales de ventas por categoría.  

6. **Integración de Datos**  
   - Importar dataset CSV con 50+ productos usando Python/Pandas.  
   - Cargar calendario de ferias locales desde archivo JSON.  

7. **Colaboración**  
   - Usar Git con:  
     - 2 branches (`catalogo` y `logistica`).  
     - 8+ commits con convención semántica (ej: "feat: add seasonal availability view").  

8. **Uso Ético de IA**  
   - Documentar:  
     - 2 prompts usados para generar consultas espaciales.  
     - Adaptaciones a código generado (ej: ajustar radios de búsqueda).  

---

### **Entregables Mínimos**  
1. **Esquema SQL** que incluya:  
   - 3 tablas principales + 2 tablas de soporte (ej: `certificaciones`).  
   - Índice GIN para Full-Text Search.  
2. **Script Python** para carga de productos con validación JSON.  
3. **Cuatro Consultas Clave**:  
   - Búsqueda de productos por tipo y radio de 20km.  
   - Cálculo de disponibilidad mensual por productor.  
   - Listado de pedidos pendientes con días en tránsito.  
   - Optimización de JOIN entre productos y pedidos.  
4. **Repositorio GitHub** con:  
   - README.md explicando modelo y políticas de seguridad.  
   - Carpeta `auditoria_IA` con ejemplos de código modificado.  

---

### **Rúbrica desde el Modelado**  
| **Criterio**          | **Insuficiente**               | **Satisfactorio**                 | **Excelente**                     |  
|-----------------------|--------------------------------|-----------------------------------|-----------------------------------|  
| **Modelado (30%)**    | ER sin uso de JSONB           | ER con 3 tablas y 1 campo JSONB   | ER normalizado con subtipos para certificaciones |  
| **Búsquedas (25%)**   | LIKE básico en nombres        | Full-Text Search con stemmer español | Búsqueda facetada (tipo + temporada + ubicación) |  
| **Geolocalización (20%)** | Coordenadas sin PostGIS     | 2 consultas espaciales básicas    | Optimización con índices SP-GiST  |  
| **Seguridad (15%)**   | Datos sensibles en texto plano| Cifrado de 2 campos con pgcrypto  | RLS por rol (productor vs cliente)|  
| **Colaboración (10%)**| Menos de 5 commits            | 8+ commits semánticos             | Uso de tags y releases en GitHub  |  

---

### **🆘 Recursos de Apoyo**  
1. [Validación JSONB en PostgreSQL](https://www.postgresql.org/docs/current/datatype-json.html)  
2. [Geolocalización con PostGIS](https://postgis.net/workshops/postgis-intro/geography.html)  
3. [Datos abiertos agrícolas - UPRA](https://www.upra.gov.co/)  

---

**⚠️ Nota Pedagógica:**  
El enfoque debe estar en **equilibrar flexibilidad y estructura**. Se permite denormalización controlada en JSONB para atributos variables, pero manteniendo núcleo relacional. Se valorará documentación que justifique decisiones técnicas entre normalización vs performance.  

**¡Conectemos el sudor del campo con la mesa bogotana!** 🌱🏙️  
