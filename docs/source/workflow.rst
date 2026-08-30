Flujo de trabajo
================

El flujo de trabajo de Ize se divide en dos fases principales.

Fase 1: registro
----------------

Ize Logger se ejecuta durante una sesión de Blender.

.. code-block:: text

   Usuario
      |
      v
   Blender
      |
      v
   Ize Logger
      |
      v
   Eventos
      |
      v
   CSV

Fase 2: análisis
----------------

Los registros obtenidos pueden abrirse posteriormente con
Ize Insights.

.. code-block:: text

   CSV
    |
    v
   Ize Insights
    |
    +------> Métricas
    |
    +------> Gráficos
    |
    +------> Visualización