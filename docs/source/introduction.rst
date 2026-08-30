Introducción
============

¿Qué es Ize Cube?
-----------------

Ize Cube es un conjunto de complementos para Blender diseñado para
registrar y analizar la interacción del usuario durante el uso
de la aplicación.

El sistema está compuesto por dos componentes principales:

Ize Logger
~~~~~~~~~~

Registra eventos producidos durante una sesión de trabajo en Blender.

Existen dos variantes:

* Ize Logger Manual
* Ize Logger Consent

Ize Insights
~~~~~~~~~~~~

Permite importar los datos registrados por Ize Logger y realizar
su análisis mediante métricas, tablas, gráficos y visualizaciones.

Arquitectura general
--------------------

El flujo básico del sistema es:

.. code-block:: text

   Blender
      |
      v
   Ize Logger
      |
      v
   Registro de eventos
      |
      v
   Archivo CSV
      |
      v
   Ize Insights
      |
      +--> Métricas
      +--> Gráficos
      +--> Tablas
      +--> Visualizaciones