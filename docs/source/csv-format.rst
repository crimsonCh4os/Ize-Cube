Formato de datos CSV
====================

Ize Logger exporta los registros de interacción en formato CSV.

La versión actual utiliza:

``SchemaVersion = 4``

Cada fila representa un cambio o evento significativo detectado
durante la sesión.

El logger no genera necesariamente una fila en cada intervalo de
tiempo: se registra una nueva observación cuando se detecta un cambio
relevante.


Estructura general
------------------

El esquema actual contiene las siguientes columnas:

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


Identificación
--------------

.. list-table::
   :header-rows: 1
   :widths: 25 20 55

   * - Campo
     - Tipo
     - Descripción

   * - ``SchemaVersion``
     - Texto / número
     - Versión del esquema CSV utilizado por Ize Logger.

   * - ``LoggerVersion``
     - Texto
     - Versión de Ize Logger que generó el registro.

   * - ``SessionID``
     - UUID
     - Identificador de la sesión de registro.

   * - ``UserID``
     - UUID
     - Identificador aleatorio asociado al usuario o ejecución según
       la variante de Logger utilizada.

   * - ``TimeStamp``
     - Número
     - Marca temporal Unix correspondiente a la observación.


Posición de la vista
--------------------

.. list-table::
   :header-rows: 1
   :widths: 20 20 60

   * - Campo
     - Tipo
     - Descripción

   * - ``UserX``
     - Número
     - Coordenada X de la posición del punto de vista del viewport 3D.

   * - ``UserY``
     - Número
     - Coordenada Y de la posición del punto de vista del viewport 3D.

   * - ``UserZ``
     - Número
     - Coordenada Z de la posición del punto de vista del viewport 3D.


Objeto activo
-------------

La posición registrada para el objeto activo se calcula utilizando
el centro de su caja envolvente en coordenadas globales.

.. list-table::
   :header-rows: 1
   :widths: 24 18 58

   * - Campo
     - Tipo
     - Descripción

   * - ``ObjX``
     - Número
     - Coordenada X del centro del objeto activo.

   * - ``ObjY``
     - Número
     - Coordenada Y del centro del objeto activo.

   * - ``ObjZ``
     - Número
     - Coordenada Z del centro del objeto activo.

   * - ``ObjRadius``
     - Número
     - Radio aproximado calculado a partir de la caja envolvente
       del objeto activo.

   * - ``ObjectTransform``
     - 0 / 1
     - Indica si se ha detectado un cambio de transformación
       del objeto.


Cambios de geometría
--------------------

Las columnas terminadas en ``Delta`` expresan cambios respecto
al estado registrado anteriormente.

Un valor positivo representa normalmente un aumento y un valor
negativo una reducción.

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``VertexDelta``
     - Entero
     - Cambio en el número de vértices.

   * - ``NgonDelta``
     - Entero
     - Cambio en el número de polígonos n-gon detectados.

   * - ``TriDelta``
     - Entero
     - Cambio en el número de triángulos.

   * - ``NormalDelta``
     - Entero
     - Cambio relacionado con las incidencias de normales detectadas.

   * - ``NonManifoldDelta``
     - Entero
     - Cambio en el número de elementos non-manifold detectados.


Modo de trabajo
---------------

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``ObjModeState``
     - 0 / 1
     - Vale 1 cuando el estado registrado corresponde a Object Mode.

   * - ``EditModeState``
     - 0 / 1
     - Vale 1 cuando el objeto se encuentra en Edit Mode.

   * - ``ModeChanged``
     - 0 / 1
     - Indica que se ha detectado un cambio de modo.


UV
--

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``UVDeploy``
     - 0 / 1
     - Indica la detección de una operación completada de despliegue
       o ``unwrap`` UV.

.. note::

   Ize Logger no almacena las coordenadas UV individuales en esta
   columna.


Objetos
-------

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``ObjectDelta``
     - Entero
     - Balance neto en el número de objetos.

   * - ``ObjectCreated``
     - Entero
     - Número de objetos creados desde el registro anterior.

   * - ``ObjectDeleted``
     - Entero
     - Número de objetos eliminados desde el registro anterior.


Modificadores
-------------

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``ModifierDelta``
     - Entero
     - Balance neto de modificadores.

   * - ``ModifierCreated``
     - Entero
     - Número de modificadores creados.

   * - ``ModifierDeleted``
     - Entero
     - Número de modificadores eliminados.


Acciones
--------

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``Duplicate``
     - 0 / 1
     - Indica una operación de duplicación detectada.

   * - ``Instance``
     - 0 / 1
     - Indica una operación de instancia detectada.

   * - ``Undo``
     - Entero
     - Número de acciones Undo detectadas en la observación.


Visualización
-------------

.. list-table::
   :header-rows: 1
   :widths: 25 15 60

   * - Campo
     - Tipo
     - Descripción

   * - ``Occlusion``
     - 0 / 1
     - Indica si se encuentra activa una visualización tipo
       X-Ray o Wireframe.

   * - ``Orthographic``
     - 0 / 1
     - Indica si el viewport está utilizando proyección ortográfica.


Valores booleanos
-----------------

Numerosas columnas utilizan los valores:

``0``
   Estado inactivo o evento no detectado.

``1``
   Estado activo o evento detectado.

Algunas variables son estados que pueden mantenerse durante varias
observaciones, mientras que otras representan eventos puntuales.


Variables Delta
---------------

Las variables ``Delta`` deben interpretarse como cambios entre
observaciones.

Por ejemplo:

.. code-block:: text

   VertexDelta = 5

indica un aumento neto de cinco vértices.

Mientras que:

.. code-block:: text

   VertexDelta = -3

indica una reducción neta de tres vértices.


Compatibilidad
--------------

Ize Logger incluye mecanismos para convertir determinados formatos
CSV históricos al esquema actual.

Ize Insights también contempla la lectura de distintas generaciones
de registros.

Por este motivo es recomendable conservar siempre:

* ``SchemaVersion``;
* ``LoggerVersion``;
* ``SessionID``.

Estas columnas permiten identificar la procedencia de los registros
y facilitan su interpretación posterior.


CSV sin UserID
--------------

La opción de exportación sin identificador elimina:

``UserID``

pero mantiene las demás columnas.

Por tanto, el archivo conserva información como:

* ``SessionID``;
* marcas temporales;
* posición de la vista;
* datos del objeto;
* cambios de geometría;
* eventos de interacción.

La exportación sin ``UserID`` debe entenderse como una eliminación
del identificador explícito, no como una garantía automática
de anonimización completa.