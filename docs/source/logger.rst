Ize Logger
==========

Ize Logger es el complemento encargado de registrar determinados eventos
del flujo de trabajo realizado por el usuario durante una sesión de Blender.

La versión documentada de Ize Logger es la **1.3.0** y requiere
**Blender 4.0 o posterior**.

El complemento se encuentra en:

``3D Viewport > Sidebar > Ize Logger``

Ize Logger está disponible en dos variantes:

* **Ize Logger Manual**
* **Ize Logger Consent**


Interfaz
--------

El panel principal de Ize Logger permite seleccionar el idioma,
consultar el estado del registro, iniciar o detener una sesión,
exportar los datos y gestionar determinadas opciones relacionadas
con privacidad.

El panel contiene las siguientes secciones:

``Estado del registro``
   Indica si el logger está detenido o en ejecución.

``Iniciar registro / Detener registro``
   Inicia o finaliza la captura de eventos.

``Exportar a CSV``
   Exporta los datos registrados a un archivo CSV.

``Exportar CSV sin UserID``
   Genera una copia del registro sin la columna ``UserID``.

``Exportar métricas de malla``
   Calcula métricas geométricas de la escena y las exporta en formato CSV.

``Privacidad``
   Permite regenerar el identificador de usuario y eliminar los
   datos almacenados por el complemento.

.. TODO: Añadir una captura del panel principal de Ize Logger.


Ize Logger Manual
-----------------

La variante **Manual** no inicia el registro automáticamente.

El usuario debe pulsar explícitamente **Iniciar registro** para comenzar
una sesión.

Cuando el registro se detiene, los datos se incorporan al archivo
``.blend`` como un bloque de texto interno.

La variante Manual también conserva el registro al guardar el archivo
de Blender.

Además, al guardar la escena puede calcular un conjunto de métricas
de la geometría de la escena e incorporarlo al archivo ``.blend``.

Esta variante no muestra un diálogo de consentimiento antes de comenzar
el registro.


Ize Logger Consent
------------------

La variante **Consent** incorpora un flujo de consentimiento previo
a la recogida de datos.

El registro no puede comenzar hasta que el usuario haya aceptado
el consentimiento.

Al pulsar **Iniciar registro**, si todavía no se ha concedido
consentimiento, se muestra el diálogo correspondiente.

En esta variante el consentimiento se mantiene únicamente durante
la sesión actual de Blender.

El ``UserID`` también se mantiene en memoria durante la sesión y no
se guarda automáticamente dentro del archivo ``.blend``.

A diferencia de la versión Manual, la versión Consent no incrusta
automáticamente el registro ni las métricas de malla al guardar
el archivo.

Los datos permanecen temporales hasta que el usuario realiza una
exportación explícita.

.. note::

   La variante Consent está diseñada para evitar que el simple hecho
   de guardar un archivo ``.blend`` provoque la persistencia automática
   de los datos registrados.


Registro de una sesión
----------------------

El flujo básico de trabajo es:

#. Abre Blender.
#. Abre el panel ``Ize Logger``.
#. Comprueba qué variante del complemento estás utilizando.
#. Pulsa **Iniciar registro**.
#. En la variante Consent, acepta el consentimiento cuando sea solicitado.
#. Trabaja normalmente en Blender.
#. Pulsa **Detener registro** al finalizar.
#. Exporta los datos si necesitas analizarlos externamente.


¿Qué información registra?
---------------------------

Ize Logger crea registros únicamente cuando detecta cambios compatibles
con el sistema de monitorización.

Entre la información registrada se encuentra:

* identificador de versión del esquema;
* versión de Ize Logger;
* identificador de sesión;
* identificador de usuario;
* marca temporal;
* posición de la vista 3D;
* posición y radio del objeto activo;
* cambios de transformación;
* cambios en vértices;
* cambios en polígonos n-gon;
* cambios en triángulos;
* cambios relacionados con normales;
* cambios de geometría non-manifold;
* estado de Object Mode y Edit Mode;
* cambios de modo;
* operaciones de despliegue UV;
* creación y eliminación de objetos;
* creación y eliminación de modificadores;
* operaciones de duplicación;
* instancias;
* operaciones de deshacer;
* estado de oclusión;
* estado de vista ortográfica.

Ize Logger no exporta las coordenadas UV individuales. El registro UV
se limita a detectar determinadas operaciones de despliegue o
``unwrap`` completadas.


Formato CSV
-----------

La versión actual utiliza el esquema CSV versión ``4``.

Las columnas principales son:

.. code-block:: text

   SchemaVersion
   LoggerVersion
   SessionID
   UserID
   TimeStamp

   UserX
   UserY
   UserZ

   ObjX
   ObjY
   ObjZ
   ObjRadius
   ObjectTransform

   VertexDelta
   NgonDelta
   TriDelta
   NormalDelta
   NonManifoldDelta

   ObjModeState
   EditModeState
   ModeChanged

   UVDeploy

   ObjectDelta
   ObjectCreated
   ObjectDeleted

   ModifierDelta
   ModifierCreated
   ModifierDeleted

   Duplicate
   Instance
   Undo

   Occlusion
   Orthographic


Exportación de datos
--------------------

Para poder exportar los CSV, el archivo de Blender debe haberse
guardado previamente.

Si el archivo de Blender se llama:

``modelo.blend``

la exportación normal genera:

``modelo_data.csv``

La exportación sin ``UserID`` genera:

``modelo_data_anon.csv``

y la exportación de métricas de malla genera:

``modelo_mesh_metrics.csv``

Los archivos se guardan en la misma carpeta que el archivo ``.blend``.


Métricas de malla
-----------------

Ize Logger también puede calcular un conjunto de métricas sobre la
geometría de la escena.

Para este cálculo se genera temporalmente una geometría combinada
a partir de los objetos compatibles de la escena.

Las cámaras y las luces se excluyen de este cálculo.

Entre las métricas disponibles se encuentran:

* área ocupada en UV;
* número de islas UV;
* estiramiento UV;
* densidad de texel;
* normales invertidas;
* transformaciones aplicadas;
* posición respecto al origen;
* porcentaje de polígonos que no son quads;
* vértices duplicados;
* vértices non-manifold;
* número de caras;
* número de componentes;
* caras por componente;
* ángulo medio entre caras.

La geometría temporal utilizada para calcular estas métricas se elimina
después del cálculo y no debe modificar la escena original.


Privacidad
----------

Ize Logger utiliza un ``UserID`` generado aleatoriamente.

La opción **Regenerar UserID** permite reemplazarlo por un identificador
nuevo.

La opción **Exportar CSV sin UserID** elimina la columna ``UserID`` del
archivo CSV exportado.

.. important::

   La eliminación de ``UserID`` afecta únicamente a dicha columna.
   La interpretación de si un archivo puede considerarse anónimo debe
   realizarse teniendo en cuenta el conjunto completo de datos registrados.