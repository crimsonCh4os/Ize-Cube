# Ize — Herramientas para monitorización y análisis de procesos de modelado 3D en Blender

Suite de add-ons para Blender orientada al **registro, análisis y visualización de procesos de modelado 3D**.

El proyecto se divide en dos componentes principales:

- **Ize Logger**: registra actividad y métricas de una sesión de modelado en Blender y genera datos estructurados en CSV.
- **Ize Insights**: carga los CSV generados por Ize Logger, calcula métricas, permite comparar sesiones o grupos y genera tablas y visualizaciones 3D dentro de Blender.

Repositorio: https://github.com/crimsonCh4os/Add_On_Blender_2026

---

## Componentes

### Ize Logger

Actualmente se distribuyen dos variantes:

- `Ize_Logger_Manual.zip`: el registro se controla manualmente desde la interfaz.
- `Ize_Logger_Consent.zip`: incorpora el flujo de consentimiento antes de registrar datos.

Ambas variantes comparten la misma base de código y el mismo formato CSV. **No deben instalarse simultáneamente**, ya que ambas utilizan el mismo paquete interno `Ize_Logger`.

Versión actual de los paquetes de referencia: **1.3.0**.

### Ize Insights

`Ize_Insights.zip` proporciona las herramientas de análisis, comparación y visualización de los datos registrados.

Incluye, entre otras funciones:

- Carga de CSV individuales.
- Creación de grupos a partir de múltiples sesiones.
- Compatibilidad con CSV actuales y formatos heredados reconocibles por su estructura.
- Cálculo de métricas de interacción y de modelado.
- Cálculo de métricas geométricas del modelo.
- Tablas paginadas y comparativas.
- Gráficos de interacción / timeline.
- Forest plot comparativo.
- Radar comparativo.
- Comparación entre sesiones, grupos y modelos.
- Interfaz en inglés y español.

Versión actual de los paquetes de referencia: **1.2.0**.

También existe una variante específica para **Python 3.13**:

- `Ize_Insights_Python_3_13.zip`: edición para **Windows x64 + CPython 3.13**. Está pensada para versiones de Blender que incorporen Python 3.13. En una instalación nueva solicita instalar las dependencias y las descarga desde PyPI como wheels binarios compatibles.

---

## Compatibilidad

- **Blender:** 4.0 o superior para la rama estándar, validando siempre la versión concreta utilizada.
- **Python estándar:** la versión incluida con el Blender para el que se haya preparado el paquete.
- **Python 3.13:** disponible mediante `Ize_Insights_Python_3_13.zip`, preparada para **Windows x64 + CPython 3.13**.
- **Sistemas operativos:** la variante Python 3.13 distribuida actualmente está orientada a Windows x64. Para otras plataformas, las dependencias deben disponer de wheels compatibles.

La validación final debe realizarse siempre en la versión concreta de Blender utilizada, especialmente después de actualizar Blender o sus dependencias de Python.

---

## Instalación

### Ize Logger

1. Abre Blender.
2. Ve a **Edit > Preferences > Add-ons** o **Get Extensions**, según la versión de Blender.
3. Selecciona **Install from Disk**.
4. Instala **una sola** de estas variantes:
   - `Ize_Logger_Manual.zip`
   - `Ize_Logger_Consent.zip`
5. Activa el complemento.
6. Abre la barra lateral de la Vista 3D con `N`.
7. Accede a la pestaña **Ize Logger**.

Si cambias de variante Manual a Consent, o viceversa, es recomendable desactivar/eliminar primero la variante anterior y reiniciar Blender antes de instalar la nueva.

### Ize Insights

#### Versión estándar

1. Abre Blender.
2. Ve a **Edit > Preferences > Add-ons** o **Get Extensions**.
3. Selecciona **Install from Disk**.
4. Instala `Ize_Insights.zip`.
5. Activa **Ize Insights**.
6. En una instalación nueva, abre el panel de Ize Insights y pulsa **Instalar dependencias**.
7. Espera a que finalice la instalación.
8. Reinicia Blender.
9. Vuelve a activar Ize Insights si fuese necesario.

#### Versión para Python 3.13

1. Comprueba que la versión de Blender utilizada ejecuta **Python 3.13**.
2. Instala `Ize_Insights_Python_3_13.zip` mediante **Install from Disk**.
3. Activa **Ize Insights**.
4. Pulsa **Instalar dependencias** cuando aparezca el aviso.
5. Mantén conexión a Internet durante este paso: el instalador obtiene desde PyPI únicamente wheels binarios compatibles con CPython 3.13.
6. Reinicia Blender cuando finalice la instalación.

No instales simultáneamente `Ize_Insights.zip` e `Ize_Insights_Python_3_13.zip`: son dos variantes del mismo complemento.

### Dependencias de Ize Insights

Ize Insights utiliza principalmente:

- NumPy
- Matplotlib
- Pillow
- y sus dependencias asociadas

En la **versión estándar**, el ZIP distribuye los archivos `.whl` necesarios dentro de `Ize_Insights/wheels/`.

En la **versión Python 3.13**, el ZIP no incluye wheels predescargados. Al pulsar **Instalar dependencias**, el Python de Blender ejecuta `pip` con `--only-binary=:all:` y `--no-cache-dir` para obtener desde PyPI paquetes binarios compatibles con `cp313`, sin compilar desde código fuente. Esta variante requiere Internet durante la instalación inicial.

Las librerías **no se distribuyen preextraídas** dentro de `site-packages`. Se instalan para esa instalación concreta del complemento.

No se recomienda copiar manualmente carpetas `site-packages`, `.pyd`, `.dll` u otros binarios entre instalaciones de Blender.

---

## Uso básico de Ize Logger

Ize Logger registra información de la sesión mientras el usuario trabaja en Blender.

Según la variante instalada, el registro puede iniciarse manualmente o estar condicionado al consentimiento del usuario.

Entre los datos registrados se encuentran, según el contexto:

- tiempo de sesión;
- modo de trabajo;
- actividad de navegación;
- distancia y velocidad de vista;
- picos de movimiento;
- uso de Object Mode y Edit Mode;
- actividad sobre geometría;
- cambios de modificadores;
- información de topología;
- estados de visualización y oclusión;
- operaciones UV;
- métricas necesarias para el análisis posterior en Ize Insights.

### Edit Mode y estabilidad

El logger está diseñado para seguir recogiendo información en **Edit Mode** sin recorrer de forma agresiva el BMesh vivo durante ráfagas de actualización.

Las operaciones que modifican gran cantidad de geometría —por ejemplo, aplicar transformaciones aleatorias sobre todos los vértices de una Suzanne— deben considerarse pruebas de estrés relevantes. El logger debe priorizar la estabilidad de Blender y limitar el coste del muestreo durante estas ráfagas.

### Smooth by Angle

El efecto/modificador **Smooth by Angle** se excluye del conteo de modificadores de Ize Logger para evitar falsos positivos, ya que se considera un efecto de shading más que un modificador de modelado convencional para los objetivos del análisis.

---

## Consentimiento y privacidad

La variante **Ize Logger Consent** incorpora un flujo explícito de consentimiento antes del registro.

El proyecto está diseñado para trabajar con identificadores seudónimos y evitar incluir información personal innecesaria en los CSV.

Antes de distribuir datos de ejemplo o datos de participantes, comprueba que no contienen:

- nombres personales;
- rutas locales;
- nombres de usuario del sistema;
- identificadores personales innecesarios;
- cualquier información que permita relacionar una sesión con una persona concreta sin una justificación explícita.

---

## Formato CSV

Ize Logger genera CSV estructurados para su posterior análisis en Ize Insights.

Los esquemas actuales pueden incluir campos de control como:

```text
SchemaVersion
LoggerVersion
SessionID
UserID
```

Ize Insights también mantiene compatibilidad con CSV heredados cuando su estructura permite reconocerlos correctamente, incluso si no incluyen todos los campos modernos de identificación.

La especificación detallada se encuentra en:

- `docs/CSV_SCHEMA.md`

---

## Uso básico de Ize Insights

1. Abre la pestaña **Ize Insights** en la barra lateral de la Vista 3D.
2. Selecciona el tipo de datos que quieres analizar.
3. Añade uno o varios CSV.
4. Opcionalmente, crea grupos para comparar varias sesiones como una unidad de análisis.
5. Ejecuta **Calculate Data / Calcular datos** para obtener métricas de sesión.
6. Para análisis geométrico, selecciona los modelos necesarios y ejecuta **Calculate model metrics / Calcular métricas del modelo**.
7. Consulta las tablas o genera los gráficos disponibles.

### Agregación de grupos

Un grupo representa una colección de sesiones, no una sesión gigante concatenada.

Por tanto, las métricas de grupo se agregan de acuerdo con su significado. Por ejemplo:

- la duración se calcula como **media de la duración de las sesiones**;
- las medias por sesión deben dar el mismo peso a cada CSV;
- los conteos comparativos se expresan como medias por sesión cuando corresponde;
- máximos y mínimos conservan su significado cuando representan extremos reales.

---

## Métricas de vista y unidades locales

Para facilitar la comparación entre usuarios y escenas de diferente escala, los **gráficos** normalizan únicamente estas métricas de vista respecto al tamaño local calculado para la escena:

- **Velocidad de navegación** → tamaño local/min.
- **Distancia al objetivo** → múltiplos del tamaño local.
- **Picos de movimiento** → calculados a partir de la velocidad ya normalizada.

Esta normalización se aplica en el contexto gráfico para estas tres métricas. No implica convertir de forma general todas las métricas físicas almacenadas en los datos o mostradas en tablas.

---

## Visualizaciones de Ize Insights

### Interaction / Timeline

Permite observar la evolución temporal de las métricas seleccionadas y comparar varias sesiones.

Cuando se representan varias fuentes, las leyendas deben identificar las series por el **nombre real del CSV o del grupo**, evitando etiquetas genéricas como `Observation 1`, `Observation 2`, etc.

### Forest plot

Permite comparar métricas normalizadas entre sesiones o grupos.

El gráfico utiliza puntuaciones comparativas para facilitar la lectura entre variables con escalas diferentes. Se evita mostrar información estadística avanzada que no sea necesaria para la interpretación principal del gráfico.

### Radar

El radar permite comparar varias métricas de Data Metrics y Model Metrics en una representación conjunta.

Recomendaciones de uso:

- seleccionar al menos tres métricas para que la forma del radar sea interpretable;
- interpretar cada eje según la descripción incluida junto al gráfico;
- en **Modo de trabajo**, la escala debe leerse como:
  - `0 = Otro`
  - `1 = Objeto`
  - `2 = Edición`
  - un valor mayor indica mayor predominio de Edit Mode;
- velocidad, distancia y picos utilizan las unidades locales descritas anteriormente.

---

## Traducciones

La interfaz se mantiene en inglés y español.

Los textos generales de interfaz se centralizan principalmente en:

- `Ize_Insights/texts.py`
- `Ize_Logger/texts.py`

La semántica de las métricas de Ize Insights se centraliza principalmente en:

- `Ize_Insights/metric_semantics.py`

Los textos específicos de ciertas visualizaciones pueden construirse dinámicamente en:

- `Ize_Insights/ui_graph_service.py`

Al añadir nuevos textos visibles, tooltips, ayudas contextuales o etiquetas gráficas, deben añadirse las versiones EN/ES correspondientes y evitar cadenas de interfaz duplicadas innecesariamente en distintos archivos.

---

## Estructura del repositorio

La raíz del repositorio contiene actualmente los paquetes instalables y la documentación principal:

```text
Add_On_Blender_2026/
├── assets/
├── datos_analisis/
├── docs/
├── images/
├── Ize_Insights.zip
├── Ize_Insights_Python_3_13.zip
├── Ize_Logger_Consent.zip
├── Ize_Logger_Manual.zip
├── CONTRIBUTING.md
├── LICENSE
├── README.md
├── requirements.txt
└── ...
```

Los ZIP instalables deben contener directamente el paquete del complemento correspondiente:

```text
Ize_Insights.zip
└── Ize_Insights/
    ├── __init__.py
    ├── ...
    └── wheels/

Ize_Insights_Python_3_13.zip
└── Ize_Insights/
    ├── __init__.py
    ├── dependencies.py
    └── ...

Ize_Logger_Manual.zip
└── Ize_Logger/
    ├── __init__.py
    ├── logger_core.py
    └── ...
```

No deben distribuirse `__pycache__`, `.pyc`, archivos temporales ni dependencias ya desplegadas dentro del ZIP de Ize Insights. La variante Python 3.13 tampoco debe incluir wheels `cp311` ni binarios de otra versión de Python.

---

## Desarrollo y contribuciones

Consulta [`CONTRIBUTING.md`](CONTRIBUTING.md) antes de realizar cambios.

Al modificar los complementos, presta especial atención a:

- mantener sincronizadas las variantes Manual y Consent de Ize Logger;
- no bloquear ni cerrar Blender durante operaciones intensivas en Edit Mode;
- conservar compatibilidad con los CSV existentes;
- mantener traducciones EN/ES;
- no cambiar la semántica de una métrica sin actualizar tablas, gráficos y documentación;
- validar la agregación de grupos;
- comprobar los tres gráficos principales de Ize Insights;
- probar instalación limpia de dependencias;
- limpiar el ZIP final de cachés y binarios desplegados.

---

## Validación manual recomendada

Antes de publicar una nueva versión:

1. Instala cada ZIP desde cero en Blender.
2. Comprueba que Ize Insights solicita la instalación de dependencias en una instalación nueva.
3. Instala las dependencias desde el propio complemento y reinicia Blender.
4. Para `Ize_Insights_Python_3_13.zip`, realiza la prueba en un Blender con Python 3.13 y confirma que se descargan wheels binarios compatibles con `cp313`.
5. Verifica que Ize Logger registra en Object Mode y Edit Mode.
6. Realiza una prueba de estrés editando simultáneamente muchos vértices.
7. Exporta un CSV desde cada variante del logger.
8. Carga los CSV individualmente en Insights.
9. Crea un grupo de varios CSV y ejecuta Calculate Data.
10. Comprueba que la duración del grupo representa la media de sesiones.
11. Genera Interaction/Timeline, Forest y Radar.
12. Comprueba nombres de leyenda, traducciones y unidades.
13. Valida las métricas de modelo con una escena de prueba conocida.
14. Revisa la consola de Blender para detectar warnings o tracebacks.

---

## Licencia

Este proyecto se distribuye bajo la licencia **GNU General Public License v3.0 or later (`GPL-3.0-or-later`)**.

Consulta el archivo [`LICENSE`](LICENSE) incluido en el repositorio.
