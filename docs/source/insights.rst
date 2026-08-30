Ize Insights
============

Ize Insights es el componente de Ize destinado al análisis y
visualización de los datos generados durante las sesiones de Blender.

La versión documentada es **Ize Insights 1.2.0** y requiere
**Blender 4.0 o posterior**.

El complemento se encuentra en:

``3D Viewport > Sidebar > Ize Insights``

Ize Insights permite trabajar tanto con datos de interacción procedentes
de Ize Logger como con métricas geométricas de modelos 3D.


Interfaz principal
------------------

La interfaz se divide en cuatro áreas principales:

``Data``
   Importación y organización de archivos CSV y cálculo de métricas
   de las sesiones registradas.

``Models``
   Cálculo e importación de métricas geométricas de modelos.

``Tables``
   Creación de tablas tridimensionales dentro de la escena de Blender.

``Graphs``
   Generación y configuración de diferentes visualizaciones 3D.

.. TODO: Añadir captura general de Ize Insights.


Data
----

La pestaña **Data** es normalmente el punto de partida del análisis.

Permite seleccionar una carpeta o archivo CSV procedente de Ize Logger,
detectar los archivos disponibles y seleccionar qué registros se desean
analizar.

También permite combinar varios archivos mediante grupos.

Flujo básico:

#. Abre la pestaña **Data**.
#. Selecciona una carpeta o archivo CSV.
#. Pulsa la opción para actualizar o detectar los CSV disponibles.
#. Selecciona uno o varios archivos.
#. Opcionalmente crea grupos de datos.
#. Pulsa **Calculate Data Metric Stats** para calcular las métricas.

.. TODO: Añadir captura de Data > Data import.


Grupos de datos
~~~~~~~~~~~~~~~

Varios archivos CSV pueden agruparse para ser tratados como una fuente
de datos conjunta.

Ize Insights conserva los archivos originales y utiliza el grupo como
una fuente virtual para determinados análisis y visualizaciones.


Métricas de interacción
-----------------------

Ize Insights calcula diferentes métricas a partir de los registros
generados por Ize Logger.

Las métricas principales son:

.. list-table::
   :header-rows: 1
   :widths: 12 35 35

   * - Código
     - Métrica
     - Tipo
   * - A1
     - Duración de la sesión
     - Actividad temporal
   * - A2
     - Pausas
     - Actividad temporal
   * - A4
     - Velocidad de navegación
     - Vista y navegación
   * - A5
     - Distancia al objetivo
     - Vista y navegación
   * - A6
     - Picos de movimiento
     - Vista y navegación
   * - A17
     - Vista sin oclusión
     - Vista y navegación
   * - A19
     - Vista ortográfica
     - Vista y navegación
   * - A12
     - Cambios de vértices
     - Evolución del modelo
   * - A13
     - Incidencias de topología
     - Evolución del modelo
   * - A16
     - Cambios de objetos
     - Evolución del modelo
   * - A14
     - Modo de trabajo
     - Herramientas y acciones
   * - A10
     - Acciones de reutilización
     - Herramientas y acciones
   * - A11
     - Uso de modificadores
     - Herramientas y acciones
   * - A15
     - Edición UV
     - Herramientas y acciones
   * - A18
     - Acciones de deshacer
     - Herramientas y acciones


Models
------

La pestaña **Models** permite analizar características geométricas
de los modelos.

El usuario puede seleccionar un objeto de tipo malla y calcular
sus métricas directamente a partir de la escena.

También puede seleccionarse un modelo de referencia para determinadas
comparaciones.

Ize Insights puede además importar los archivos de métricas de malla
generados previamente por Ize Logger.

Por ejemplo:

``modelo_mesh_metrics.csv``

Las observaciones importadas y las métricas calculadas directamente
en Blender pueden utilizarse posteriormente en tablas y gráficos.


Métricas de modelo
~~~~~~~~~~~~~~~~~~

Entre las métricas geométricas utilizadas por Ize Insights se encuentran:

* número de polígonos;
* número de componentes de malla;
* polígonos por componente;
* ángulo medio;
* polígonos no quad;
* porcentaje de polígonos no quad;
* vértices duplicados;
* porcentaje de vértices duplicados;
* vértices non-manifold;
* porcentaje de vértices non-manifold;
* normales invertidas;
* porcentaje de normales invertidas;
* área UV;
* número de islas UV;
* estiramiento UV;
* densidad de texel;
* transformaciones;
* posición;
* similitud respecto a un modelo de referencia.


Asociación entre datos y modelos
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ize Insights permite asociar un archivo de datos registrado con una
observación de modelo.

Esta relación permite estudiar conjuntamente el comportamiento
registrado durante una sesión y las características del modelo
producido.

Estas asociaciones se utilizan especialmente en determinados gráficos
comparativos y de correlación.


Tables
------

La pestaña **Tables** permite generar tablas tridimensionales dentro
de la escena de Blender.

La fuente de la tabla puede ser:

* métricas de los datos registrados;
* métricas de los modelos.

Para los datos registrados pueden utilizarse diferentes estadísticas,
como media, mediana, desviación estándar, mínimo, máximo, suma,
número de observaciones o último valor.

El usuario también puede configurar el número de filas y columnas
mostradas y navegar entre las diferentes páginas de la tabla.

Las tablas generadas pueden eliminarse sin afectar a los datos
originales.


Graphs
------

Ize Insights dispone actualmente de cuatro tipos principales
de visualización.

Timeline
~~~~~~~~

Representa la evolución de las métricas a lo largo de las muestras
registradas.

Permite inspeccionar tendencias, pausas, cambios y picos de actividad.

Puede mostrar las métricas en bandas independientes o superpuestas.


Forest Plot
~~~~~~~~~~~

Permite comparar valores centrales y dispersión entre diferentes
archivos, grupos u observaciones.

Está orientado a comparaciones entre varias sesiones o modelos.


Radar Plot
~~~~~~~~~~

Permite comparar perfiles multidimensionales utilizando métricas
de datos, métricas de modelos o una combinación de ambas.

Los valores se normalizan por métrica.

El gráfico requiere al menos tres métricas seleccionadas.


Correlation Heatmap
~~~~~~~~~~~~~~~~~~~

Genera mapas de calor basados en correlación de Pearson.

Puede utilizar tres modos:

``Data ↔ Model``
   Relaciona métricas de comportamiento con métricas geométricas.

``Data ↔ Data``
   Estudia relaciones entre métricas de comportamiento.

``Model ↔ Model``
   Estudia relaciones entre métricas geométricas.

Para este tipo de análisis es necesario disponer de varias observaciones.
El complemento exige un mínimo de tres en los análisis de correlación
correspondientes.


Dependencias
------------

Ize Insights utiliza bibliotecas externas para realizar determinados
cálculos y representaciones.

Entre ellas se encuentran **NumPy** y **Matplotlib**.

El complemento incluye archivos ``.whl`` con las dependencias necesarias
y dispone de un sistema para instalarlas dentro de una carpeta privada
del propio complemento.

Si las dependencias no están disponibles, Ize Insights muestra una
interfaz específica que permite instalarlas.

Después de instalar las dependencias puede ser necesario reiniciar Blender.


Compatibilidad con Ize Logger
-----------------------------

Ize Insights reconoce diferentes generaciones de archivos CSV de
Ize Logger.

Los archivos actuales utilizan el esquema moderno, pero existe código
de compatibilidad para interpretar registros históricos y determinadas
exportaciones sin ``UserID``.

Los datos se normalizan internamente antes de realizar los análisis.


Flujo de trabajo
----------------

El flujo habitual entre los dos complementos es:

.. code-block:: text

   Blender
      |
      v
   Ize Logger
      |
      +----------------------+
      |                      |
      v                      v
   *_data.csv         *_mesh_metrics.csv
      |                      |
      +----------+-----------+
                 |
                 v
            Ize Insights
                 |
        +--------+--------+
        |        |        |
        v        v        v
     Métricas  Tablas   Gráficos