**Licitaciones públicas y mercados financieros**

**Autor:** Julián Cámara
**Asignatura:** Visualización de Datos
**Proyecto:** Práctica final – Parte I y Parte II
**Dataset principal:** Licitaciones publicadas por la Junta de Andalucía en 2023
**Visualización final:** Storytelling interactivo sobre gasto público, concentración empresarial y posible relación con mercados bursátiles.

**1. Descripción del proyecto**

Este repositorio contiene el trabajo desarrollado para la práctica final de la asignatura de Visualización de Datos.

El objetivo del proyecto es analizar las licitaciones públicas publicadas por la Junta de Andalucía en 2023 y construir una narrativa visual que permita observar cómo se distribuye el gasto público, qué empresas concentran mayor volumen de adjudicación y qué parte de ese gasto está vinculada a empresas o grupos cotizados en bolsa.

El proyecto combina datos abiertos de contratación pública con datos bursátiles, incorporando un caso exploratorio centrado en Endesa.

El propósito del análisis no es demostrar una relación causal directa entre adjudicaciones públicas y movimientos bursátiles, sino explorar si determinadas licitaciones pueden actuar como eventos temporales de interés para un análisis financiero posterior.

**2. Pregunta principal**

¿Puede la contratación pública aportar señales relevantes para analizar empresas cotizadas o grupos empresariales vinculados a los mercados financieros?

A partir de esta pregunta general, el proyecto responde a varias preguntas específicas:

¿Cómo evoluciona el gasto público publicado o adjudicado durante el periodo analizado?
¿Qué empresas concentran mayor volumen económico de adjudicación?
¿Qué parte del gasto llega a empresas vinculadas a grupos cotizados?
¿Puede observarse algún comportamiento bursátil relevante alrededor de un evento de licitación?

**3. Motivación del proyecto**

La elección del tema responde a un doble interés:

Interés social y público, ya que las licitaciones permiten analizar cómo se distribuye el gasto de una administración pública.
Interés financiero, ya que algunas adjudicatarias pertenecen a grupos empresariales cotizados, lo que permite conectar la contratación pública con el análisis de mercados.

La práctica busca ir más allá de un ranking descriptivo de empresas adjudicatarias, construyendo una narrativa que relacione gasto público, concentración empresarial y posibles señales de interés para inversores.

**4. Fuente de datos**

La fuente principal del proyecto es el conjunto de datos de licitaciones públicas de la Junta de Andalucía, obtenido a través de datos.gob.es.

El dataset original se encontraba en formato XML y contenía información administrativa sobre:

Datos generales del expediente.
Órgano de contratación.
Empresa adjudicataria.
NIF de la empresa.
Importe de adjudicación.
Fechas de publicación y adjudicación.
Información sobre el resultado del procedimiento.
Publicaciones oficiales.

Además, para el análisis bursátil se utilizaron datos históricos de mercado descargados mediante la librería yfinance, principalmente para el caso de Endesa.

**5. Proceso de preparación de datos**

El proceso de preparación de datos se estructuró como una fase ETL: extracción, transformación y carga.

**5.1 Extracción**

Se procesaron múltiples archivos XML correspondientes a expedientes de licitación. A partir de estos archivos se extrajeron los campos necesarios para el análisis:

Título de la licitación.
Empresa adjudicataria.
NIF.
Importe.
Fecha de publicación.
Fecha de adjudicación.
Enlace o identificador del expediente, cuando estaba disponible.

**5.2 Transformación y limpieza**

Durante la limpieza se realizaron varias operaciones:

Conversión de importes a formato numérico.
Normalización de nombres de empresas.
Revisión de variantes de una misma empresa usando el NIF como criterio auxiliar.
Eliminación de registros no interpretables como “Ver resolución adjunta”.
Revisión y eliminación de duplicados derivados de publicaciones o actualizaciones repetidas.
Creación de variables temporales como mes, año o trimestre.
Clasificación manual de empresas según su relación con mercados bursátiles.

Esta fase fue especialmente importante porque algunos registros administrativos podían distorsionar la lectura visual si se cargaban directamente en Flourish.

**6. Datasets derivados**

El proyecto no carga directamente el XML original en la herramienta de visualización. En su lugar, se generaron datasets específicos para cada gráfico.

licitaciones_corregidas_deduplicadas.csv

Dataset base limpio y deduplicado. Contiene los registros de licitaciones depurados, con empresas normalizadas e importes en formato numérico.

visualizacion_01_gasto_publicado_2023.csv

Dataset agregado por mes para mostrar la evolución temporal del gasto público.

Campos principales:

Mes
Importe_Total
Numero_Contratos
Importe_Medio
Importe_Total_Millones
visualizacion_02_ranking_empresas_top25_corregido.csv

Dataset agregado por empresa para crear el ranking de adjudicatarias con mayor volumen económico.

Campos principales:

Empresa_Canonica
Importe_Total
Importe_Total_Millones
Numero_Contratos
Importe_Medio
NIFs
visualizacion_03_cotizadas_vs_no_cotizadas.csv

Dataset enriquecido con una clasificación manual de las empresas según su relación con mercados bursátiles.

Categorías principales:

Grupo cotizado / filial.
Cotizada directa.
Entidad pública.
No cotizada / UTE.

Campos principales:

Empresa_Canonica
Tipo_Empresa
Ticker
Mercado
Comentario
Importe_Total_Millones
visualizacion_04_endesa_evento_bursatil.csv

Dataset bursátil utilizado para el caso Endesa.

Campos principales:

Fecha
Close
Close_Base100
Volume
Volume_Millones
Evento
Importe_Evento_Millones
Titulo_Evento

**7. Visualizaciones del story**

La visualización final se organiza como una historia visual en varias escenas.

**7.1. Evolución temporal del gasto público**

Muestra el importe mensual publicado o adjudicado durante el periodo analizado. Permite observar si el gasto se distribuye de forma homogénea o si existen picos temporales.

**7.2. Ranking de empresas adjudicatarias**

Muestra qué empresas concentran mayor volumen económico. Esta visualización permite detectar la concentración del gasto público en un número reducido de entidades.

**7.3. Empresas vinculadas a bolsa frente a no cotizadas**

Utiliza un treemap para clasificar las principales adjudicatarias según su relación con los mercados financieros. El tamaño representa el importe adjudicado y la categoría indica si la empresa pertenece a un grupo cotizado, cotiza directamente, es entidad pública o no cotiza.

**7.4. Caso Endesa: evento de licitación y evolución bursátil**

Muestra la evolución del precio de cierre de Endesa durante 2023, indexado a base 100, y sitúa sobre la serie temporal el evento de publicación del principal contrato identificado en el dataset.

**8. Herramientas utilizadas**
Python: extracción, limpieza y transformación de datos.
Pandas: manipulación de datasets.
ElementTree: procesamiento de archivos XML.
yfinance: descarga de datos bursátiles.
Flourish: creación del story y visualizaciones interactivas.
GitHub: publicación del proyecto, documentación y archivos reproducibles.
9. Estructura del repositorio
.
├── README.md
├── data/
│   ├── raw/
│   │   └── a01002820-licitaciones-publicadas-2023.xml
│   └── processed/
│       ├── licitaciones_corregidas_deduplicadas.csv
│       ├── visualizacion_01_gasto_publicado_2023.csv
│       ├── visualizacion_02_ranking_empresas_top25_corregido.csv
│       ├── visualizacion_03_cotizadas_vs_no_cotizadas.csv
│       └── visualizacion_04_endesa_evento_bursatil.csv
├── notebooks/
│   ├── Codigo_procesamiento_dataset_licitaciones.ipynb
│   └── Descarga_metricas_Endesa_2023_Colab_YahooFinance.ipynb
├── reports/
│   ├── parte_1_seleccion_dataset.pdf
│   └── parte_2_memoria_visualizacion.pdf
├── story/
│   └── enlace_flourish.txt
├── video/
│   └── enlace_video.txt
└── LICENSE
**10. Enlace a la visualización**

La visualización interactiva final está disponible en:


**11. Vídeo explicativo**

El vídeo explicativo del proyecto está disponible en:

**12. Reproducibilidad**

Para reproducir el proyecto se pueden seguir estos pasos:

Descargar o clonar este repositorio.
Revisar los notebooks incluidos en la carpeta notebooks/.
Ejecutar el notebook de procesamiento de licitaciones para generar el dataset limpio.
Ejecutar el notebook de descarga bursátil para obtener los datos de Endesa.
Cargar los archivos CSV procesados en Flourish.
Reconstruir las visualizaciones siguiendo la estructura del story.

**13. Limitaciones**

Este proyecto debe interpretarse como un análisis exploratorio.

Las principales limitaciones son:

Una adjudicación pública no implica necesariamente un movimiento bursátil.
Las empresas cotizadas pueden verse afectadas por muchos otros factores: resultados financieros, tipos de interés, mercado general, dividendos o noticias sectoriales.
Algunas adjudicatarias no cotizan directamente, sino que son filiales de grupos cotizados.
La clasificación de empresas cotizadas se ha realizado manualmente y puede requerir revisión adicional.
Algunos expedientes administrativos contienen fechas anteriores o actualizaciones posteriores, por lo que se debe diferenciar entre fecha de adjudicación y fecha de publicación.
El caso Endesa se utiliza como ejemplo exploratorio y no como prueba estadística de causalidad.

**14. Conclusión**

El proyecto muestra cómo los datos abiertos de contratación pública pueden transformarse en una herramienta visual para analizar gasto público, concentración empresarial y posibles conexiones con los mercados financieros.

La principal aportación del trabajo es combinar datos administrativos con datos bursátiles, construyendo una narrativa visual que va desde la transparencia pública hasta el análisis financiero exploratorio.

**15. Licencia**

Este repositorio se publica con fines académicos.

Se recomienda utilizar una licencia abierta, por ejemplo:

MIT License para el código.
Creative Commons Attribution 4.0 para documentación, gráficos y textos.

**16. Autor**

Julián Cámara
Práctica final de Visualización de Datos
Máster Universitario en Ciencia de Datos
