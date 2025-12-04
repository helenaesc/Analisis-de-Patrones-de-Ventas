# Análisis de Patrones de Ventas

Proyecto de analítica de datos enfocado en identificar patrones y ciclicidades de ventas por región, categoría y producto, con el fin de optimizar inventarios, precios y decisiones comerciales.

---

## Objetivos

Analizar patrones y ciclicidades de ventas para: 

- Detectar tendencias y estacionalidad por **zona, sector y categoría**.
- Optimizar la **rotación de productos** y la **asignación de recursos** (inventario, fuerza de ventas).
- Anticipar la **demanda a corto y mediano plazo** mediante modelos estadísticos.
- Evaluar y proponer **estrategias de precios** basadas en elasticidad.
- Construir un **dashboard ejecutivo** con KPIs clave (ventas, margen, ticket promedio, etc.).

---

## Datos

- **Fuente:** Dataset de Kaggle 
  <[Datos usados](https://www.kaggle.com/datasets/dataregina/datasets-para-proyecto-bi?select=ventas.csv)>
- **Nivel de detalle:** Transacciones de venta por producto, fecha, región, categoría, precio y cantidad.

---

## Enfoque de negocio

El análisis se orienta a decisiones en tres frentes: 

1. **Abasto y surtido:**  
   - Identificar productos y categorías con mayor relevancia por región.
2. **Eficiencia operativa:**  
   - Reducir sobre-stock y quiebre de inventario.
3. **Planificación comercial:**  
   - Ajustar precios y promociones en función de la elasticidad y del desempeño por producto/cluster.

En el EDA se priorizó el análisis de **regiones**, **productos** e **ingresos totales**, incluyendo comparaciones de ingreso total, ticket promedio y presencia regional.

---

## Metodología

### 1. Recolección y diccionario de datos

- Descarga del dataset desde Kaggle.
- Construcción de un **diccionario de datos** con:
  - Nombre de variables.
  - Tipo (`int`, `float`, `string`, `date`, etc.).
  - Llaves y relaciones entre tablas.
  - Nivel de granularidad (transacción, producto, región, etc.).

### 2. Calidad, limpieza y preparación

- Detección y tratamiento de:
  - Valores nulos.
  - Duplicados.
  - Outliers en precio, cantidad e ingreso.
- Transformaciones principales:
  - Conversión de fechas a tipo `datetime`.
  - Creación de variables derivadas:
    - **Ingreso** = precio × cantidad.
    - **Mes**, **año**, **día de semana**, flag de **fin de semana**.
    - Variables agregadas por producto, región y categoría.

### 3. Análisis exploratorio (EDA)

- Distribuciones de:
  - Ingreso por región, categoría y producto.
  - Ticket promedio y volumen de ventas.
- Comparación de **ingreso total por región** y relación entre **ingreso vs ticket promedio**.
- Análisis de:
  - Top/bottom productos por región (Top 5 y Bottom 5).
  - Comportamiento temporal de las ventas (cuando aplica).

### 4. Segmentación y clustering de productos

- Construcción de un dataset agregado a nivel **producto** con:
  - Ingreso promedio por producto.
  - Ticket promedio.
  - Presencia regional (# de regiones donde se vende).
  - Participación de Buenos Aires y fines de semana.
  - Distribución por categoría.
- Aplicación de un algoritmo de **clustering** (ej. K-means) para agrupar productos en 3 clusters:
  - **Cluster 0 – Productos de alto valor**
    - Ingreso promedio alto por producto (~4,336).
    - Ticket promedio elevado (~54.4).
    - Amplia presencia regional (~6 regiones).
  - **Cluster 1 – Productos base**
    - Alto volumen, ingresos medios.
    - Productos esenciales presentes en la mayoría de las regiones.
  - **Cluster 2 – Productos de ocasión**
    - Ingresos más bajos por producto.
    - Consumo ligado a fines de semana/ocasiones específicas.


### 5. Modelado y elasticidad precio-demanda

- Estimación de **elasticidad precio-demanda por categoría**
- Clasificación de categorías en:
  - **Elasticidad negativa:** sube precio, baja demanda (sensibles).
  - **Elasticidad neutra:** cambios de precio casi no afectan la demanda.
  - **Elasticidad positiva (o anómala):** interpretar con cuidado dependiendo del contexto.
- Simulación de **escenarios de precios**:
  - Escenarios con incrementos/decrementos de precio por categoría.
  - Estimación del **cambio en ingresos por región** bajo cada escenario.

### 6. Visualización y dashboard

- Construcción de un **dashboard** en Power BI
  - KPIs de ventas, ticket promedio y margen.
  - Filtros por región y categoría.
  - Visualizaciones de:
    - Top/Bottom productos.
    - Resultados de clusters.
    - Impacto en ingresos de los escenarios de precios.

---

## Estructura del repositorio

.
├── notebook data/         # Datos crudos de Kaggle
├── dashboard data/        # Datos limpios y transformados para el dashboard
├── notebook/              # Jupyter Notebook (EDA, clustering, elasticidad, etc.)
├── dashboard/             # Archivos .pbix
├── reports/               # Presentación PDF y reporte técnico
└── README.md


## Uso

- Notebook
1. Clonar el repositorio
2. Abrir el Jupyter notebook en VS Code
3. Ir a la carpeta de notebook/
4. Ejecutar el código
5. Explorar cada sección

- Dashboard
1. Descargar el dashboard
2. Descargar los datos de la carpeta dashboard data/
3. Abrir el archivo .pbix en Power BI
4. Seleccionar la tab de inicio e ir a Consultas
5. Dar click en transformar datos
6. Dar click en cada una de las consultas
7. Ir a la sección de Pasos Aplicados y seleccionar origen
8. Modificar la ruta dependiendo del folder en donde se descargaron los datos
9. Seleccionar cerrar y aplicar
10. Explorar las páginas del dashboard


## Principales resultados

1. **Heterogeneidad por región y producto**  
   Las regiones y los productos muestran comportamientos de venta muy distintos.  
   Cada variable (tickets, ingresos, método de pago, etc.) aporta información no redundante sobre el cliente.

2. **Top y bottom productos por región**  
   Se identifican los **Top 5** y **Bottom 5** productos en cada región, lo que ayuda a tomar decisiones sobre:
   - Productos a impulsar.
   - Productos candidatos a descontinuar o reemplazar.

3. **Clusters de productos**  
   A partir del clustering se obtienen grupos de productos con patrones similares, por ejemplo:
   - **Cluster 0 – Productos de alto valor:** alto ingreso y ticket promedio; prioridad para disponibilidad y margen.
   - **Cluster 1 – Productos base:** alto volumen, sostienen el grueso de las ventas; adecuados para promociones.
   - **Cluster 2 – Productos de ocasión:** ingresos más bajos, consumo ligado a fines de semana o fechas específicas.

4. **Elasticidad precio–demanda**  
   - Algunas categorías presentan **elasticidad negativa fuerte** (subir precio reduce significativamente la demanda).
   - Otras son casi **inelásticas**, permitiendo ajustes de precio con poco impacto en volumen.

5. **Escenarios de ingresos**  
   Se evalúan distintos escenarios de cambios de precio por categoría/región y su impacto en:
   - Ingresos totales.
   - Distribución de ingresos entre regiones y clusters de productos.

---

## Recomendaciones de negocio

- **Estrategia por región**
  - Definir el rol de cada región (crecimiento, rentabilidad, defensa de mercado).
  - Reforzar categorías fuertes en cada zona y revisar aquellas con bajo desempeño.

- **Gestión de portafolio de productos**
  - **Productos de alto valor:** asegurar disponibilidad, evitar quiebres de inventario, revisar márgenes.
  - **Productos base:** utilizarlos como ancla en promociones y programas de fidelización.
  - **Productos de ocasión:** enfocarlos en campañas específicas (fines de semana, temporadas, fechas clave).
  - Revisar los **Bottom 5** productos por región para:
    - Ajustar precios.
    - Rediseñar promociones.
    - Considerar su sustitución o descontinuación.

- **Precios y elasticidad**
  - Incrementar precios de forma controlada en categorías con demanda inelástica.
  - Diseñar estrategias de descuento o bundles en categorías muy elásticas para aumentar volumen.
  - Monitorear periódicamente la respuesta de la demanda ante cambios de precio.

- **Seguimiento y monitoreo**
  - Mantener un dashboard actualizado con KPIs por región, categoría y cluster.
  - Revisar mensualmente los cambios en patrón de ventas y ajustar la estrategia comercial.

---

## Autores

Proyecto realizado por:

- **Helena Eridani Escandón López** – A01659511  
- **Franck Rodolfo Méndez Saint Louis** – A01659339  
- **Armando Atanasio Navarrete Yépez** – A01658529  
- **Fátima Quiroz Romero** – A01660757  

