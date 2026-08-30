Privacidad y consentimiento
===========================

Ize Logger registra información relacionada con la interacción
del usuario durante una sesión de trabajo en Blender.

El tratamiento de estos registros depende de la variante
de Ize Logger utilizada.

.. important::

   Esta sección describe el comportamiento técnico del complemento.
   No constituye por sí misma una política de privacidad ni determina
   el cumplimiento de requisitos legales o éticos de una investigación.


Identificadores
---------------

Ize Logger utiliza dos identificadores principales:

``SessionID``
   Identifica una sesión de ejecución del logger.

``UserID``
   Es un identificador aleatorio basado en UUID.

Estos identificadores permiten distinguir sesiones y registros
sin utilizar directamente el nombre del usuario.


Ize Logger Manual
-----------------

En la variante **Ize Logger Manual**, el ``UserID`` se almacena
dentro del archivo de Blender mediante un bloque de texto interno.

Si el archivo se vuelve a abrir, ese identificador puede conservarse.

El usuario dispone de una opción para regenerar el ``UserID``.

Al regenerarlo se crea un identificador aleatorio nuevo.


Ize Logger Consent
------------------

En la variante **Ize Logger Consent**, el identificador del usuario
se mantiene únicamente durante la ejecución actual de Blender.

El complemento evita almacenar automáticamente ese identificador
dentro del archivo ``.blend``.

Esta variante incorpora además un flujo de consentimiento antes
de iniciar el registro.

El consentimiento se conserva únicamente durante la sesión actual
de Blender.

Al cerrar y volver a abrir Blender será necesario realizar de nuevo
el flujo de consentimiento cuando corresponda.


Consentimiento
--------------

Ize Logger Consent no inicia el registro hasta que se ha concedido
el consentimiento requerido por el complemento.

El objetivo de este flujo es diferenciar explícitamente:

* la instalación o activación del complemento;
* la aceptación del consentimiento;
* el comienzo efectivo de la recogida de datos.

La mera instalación del complemento no debe interpretarse como
el inicio automático de una sesión de registro.


Datos registrados
-----------------

Ize Logger puede registrar información relacionada con:

* posición del punto de vista en el espacio 3D;
* posición aproximada del objeto activo;
* transformaciones del objeto;
* modificaciones de geometría;
* cambios en el número de vértices;
* cambios relacionados con triángulos y n-gons;
* incidencias relacionadas con normales;
* geometría non-manifold;
* cambios entre Object Mode y Edit Mode;
* operaciones determinadas de UV unwrap;
* creación y eliminación de objetos;
* creación y eliminación de modificadores;
* operaciones de duplicado e instancia;
* acciones Undo;
* estado de X-Ray o Wireframe;
* uso de vista ortográfica.

Ize Logger no registra las coordenadas UV individuales.

El registro de UV se utiliza para indicar determinadas operaciones
completadas de despliegue o ``unwrap``.


Exportación sin UserID
----------------------

Ize Logger permite generar una versión del CSV sin la columna
``UserID``.

El archivo exportado utiliza un nombre similar a:

``modelo_data_anon.csv``

Esta operación elimina únicamente la columna ``UserID``.

.. warning::

   La eliminación del ``UserID`` no implica automáticamente que
   el conjunto de datos sea anónimo.

   Otras variables, combinaciones de variables o información externa
   podrían permitir distinguir sesiones o participantes dependiendo
   del contexto del estudio.


Eliminación de datos
--------------------

Ize Logger proporciona una opción para eliminar los datos registrados
por el complemento.

Esta operación elimina el registro interno correspondiente y el
archivo temporal utilizado durante la sesión.

Los archivos CSV que ya hayan sido exportados al sistema de archivos
no deben considerarse eliminados automáticamente.

Si se desea eliminar completamente una exportación previa, también
debe eliminarse manualmente el archivo correspondiente.


Persistencia de datos
---------------------

El comportamiento es diferente entre las dos variantes.

Ize Logger Manual
~~~~~~~~~~~~~~~~~

La variante Manual puede almacenar información dentro del archivo
``.blend`` mediante bloques de texto internos.

Esto permite recuperar el registro posteriormente.

Ize Logger Consent
~~~~~~~~~~~~~~~~~~

La variante Consent está diseñada para reducir la persistencia
automática de información.

El identificador y el consentimiento se mantienen en memoria
durante la sesión actual y no se crean automáticamente como datos
persistentes del archivo ``.blend``.


Recomendaciones para investigación
----------------------------------

Si Ize se utiliza en un estudio con participantes, se recomienda
documentar previamente:

* qué información será registrada;
* para qué se utilizará;
* durante cuánto tiempo se conservará;
* quién tendrá acceso a ella;
* cómo se identificarán las sesiones;
* cómo podrán eliminarse los datos;
* qué procedimiento de consentimiento se utilizará;
* qué versión de Ize Logger se utilizó.

También es recomendable conservar junto al conjunto de datos la
versión del logger y la versión del esquema CSV.