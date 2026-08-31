# Contribuir a Ize — Blender 2026

Gracias por colaborar en **Ize Logger** e **Ize Insights**, la suite de herramientas para registrar, analizar y visualizar procesos de modelado 3D en Blender.

Repositorio oficial: <https://github.com/crimsonCh4os/Add_On_Blender_2026>

## 1. Componentes del proyecto

El proyecto se distribuye actualmente en tres paquetes instalables:

- `Ize_Insights.zip` — análisis de CSV, cálculo de métricas, tablas y visualizaciones 3D.
- `Ize_Logger_Consent.zip` — registrador con flujo de consentimiento.
- `Ize_Logger_Manual.zip` — registrador con inicio manual y sin flujo de consentimiento.

Las dos variantes de Ize Logger comparten intencionadamente el mismo paquete Python `Ize_Logger`. **No deben instalarse o activarse simultáneamente en una misma sesión de Blender.**

Versiones de referencia de los paquetes actuales:

- Ize Insights: `1.2.0`.
- Ize Logger Consent / Manual: `1.3.0`.
- Blender mínimo declarado por los add-ons: `4.0`.

## 2. Estructura del repositorio

La rama `main` contiene, entre otros elementos:

```text
Add_On_Blender_2026/
├── assets/
├── datos_analisis/
├── docs/
├── images/
├── CONTRIBUTING.md
├── Ize_Insights.zip
├── Ize_Logger_Consent.zip
├── Ize_Logger_Manual.zip
├── LICENSE
├── README.md
└── requirements.txt
```

Los ZIP instalables deben mantener una estructura compatible con Blender:

```text
Ize_Insights.zip
└── Ize_Insights/
    ├── __init__.py
    ├── ...
    └── wheels/

Ize_Logger_*.zip
└── Ize_Logger/
    ├── __init__.py
    ├── logger_core.py
    ├── texts.py
    ├── config.py
    └── csv_schema.py
```

No añadas una carpeta intermedia adicional al empaquetar un add-on.

## 3. Preparación para contribuir

1. Haz un fork del repositorio o crea una rama a partir de `main` actualizado.
2. Instala una versión de Blender compatible con el cambio que vas a realizar.
3. Trabaja sobre una copia extraída del paquete correspondiente.
4. Antes de modificar código, comprueba que la versión de partida se registra y funciona en Blender.
5. Mantén los cambios limitados al objetivo de la rama o pull request.

Ejemplos de nombres de rama:

```text
feature/nueva-metrica
fix/radar-legend
fix/logger-edit-mode
refactor/translations
release/1.3.1
```

## 4. Estilo de código

- Sigue PEP 8 cuando sea compatible con las convenciones de Blender.
- Usa nombres descriptivos y evita abreviaturas innecesarias.
- Añade docstrings a funciones cuya intención no sea evidente.
- Mantén separada la lógica de cálculo de la lógica de interfaz y de `bpy` siempre que sea posible.
- Evita duplicar lógica entre paneles, operadores y servicios de gráficos.
- Gestiona los errores recuperables sin cerrar Blender.
- No ocultes excepciones importantes con `except Exception: pass` salvo que exista una justificación clara y documentada.
- Mantén correcto el ciclo `register()` / `unregister()` y elimina las propiedades registradas dinámicamente cuando corresponda.

## 5. Ize Logger: reglas específicas

### 5.1 Mantener las dos variantes sincronizadas

`Ize_Logger_Consent` e `Ize_Logger_Manual` comparten prácticamente todo el código. La diferencia funcional principal está controlada por:

```python
ENABLE_CONSENT_FLOW = True   # Consent
ENABLE_CONSENT_FLOW = False  # Manual
```

Si modificas `logger_core.py`, `texts.py`, `csv_schema.py` o cualquier comportamiento compartido, aplica y valida el cambio en **ambos paquetes**.

### 5.2 Rendimiento y estabilidad

El logger se ejecuta durante una sesión interactiva de Blender y no debe interferir con la edición.

- Evita trabajo pesado dentro de handlers de `depsgraph`.
- No escribas una fila CSV por cada actualización del depsgraph durante una ráfaga de cambios.
- Evita recorrer geometría completa continuamente.
- No realices operaciones bloqueantes en callbacks de alta frecuencia.
- En Edit Mode, cualquier lectura geométrica debe diseñarse para no competir con una actualización activa de Blender.
- Si una lectura no puede hacerse de forma segura, es preferible omitir esa muestra concreta que provocar inestabilidad.
- No debe existir ningún escenario normal de edición que cierre Blender por culpa del logger.

Como prueba de estrés manual, conviene comprobar operaciones que modifican muchos elementos de una malla en poco tiempo, por ejemplo editar todos los vértices de una Suzanne y aplicar una transformación aleatoria.

### 5.3 Modificadores

El conteo de modificadores debe representar modificadores relevantes para el análisis. Actualmente **Smooth by Angle** se excluye del conteo porque puede introducir falsos positivos al representar una operación de shading más que una decisión de modelado equivalente a otros modificadores.

Si se cambia esta lógica, documenta la razón y comprueba tanto el conteo total como la detección de cambios de modificadores.

### 5.4 Privacidad y consentimiento

No elimines ni debilites las protecciones de privacidad existentes.

- No registres nombres de usuario del sistema ni rutas locales como datos de sesión.
- Mantén la exportación anónima cuando esté disponible.
- Los CSV y `.blend` usados como ejemplos deben estar anonimizados.
- Los cambios del flujo de consentimiento deben validarse específicamente en `Ize_Logger_Consent`.

## 6. Ize Insights: reglas específicas

### 6.1 Dependencias

Ize Insights necesita actualmente:

```text
numpy==1.26.4
matplotlib==3.9.4
```

La distribución incluye los **wheels offline** necesarios en `Ize_Insights/wheels/`.

El flujo esperado es:

1. El usuario instala el ZIP de Ize Insights.
2. La instalación nueva muestra **Instalar dependencias**.
3. El usuario instala las dependencias desde los wheels incluidos.
4. Las librerías extraídas se crean localmente para esa instalación.
5. Las dependencias desplegadas no forman parte del ZIP distribuido ni del repositorio.

El archivo distribuido `_install_state.txt` debe conservar el estado esperado para una instalación nueva.

### 6.2 No incluir binarios desplegados

No deben incluirse dentro del ZIP final ni en commits:

- `site-packages/` generado por la instalación.
- `.pyd` extraídos.
- `.dll` extraídas.
- `.so` o `.dylib` extraídos.
- copias desplegadas de NumPy, Matplotlib, Pillow u otras dependencias.

Sí pueden incluirse los `.whl` que forman parte del instalador offline del proyecto.

Los wheels distribuidos actualmente están preparados para el entorno soportado por ese instalador. Si se cambia la versión de Python, Blender, arquitectura o sistema operativo soportado, deben regenerarse y probarse los wheels correspondientes.

### 6.3 Traducciones

La interfaz debe mantenerse en **inglés y español**.

Al añadir o cambiar elementos visibles revisa:

- títulos de panel;
- botones;
- tooltips;
- mensajes de error y confirmación;
- títulos y ayudas contextuales;
- tablas;
- controles de paginación;
- leyendas y explicaciones de gráficos;
- nombres y descripciones de métricas.

Centraliza los textos reutilizables en `texts.py` y la semántica de métricas en `metric_semantics.py` cuando corresponda. Evita introducir nuevas cadenas estáticas EN/ES dispersas por servicios de renderizado si pueden resolverse desde estas fuentes comunes.

### 6.4 Semántica de métricas

Un cambio visual no debe alterar silenciosamente el significado estadístico de una métrica.

- Distingue entre totales, medias, porcentajes, tasas, cuartiles y extremos.
- Los grupos representan varias sesiones y no deben tratarse automáticamente como una única sesión concatenada.
- Si se cambia una agregación, explica en el PR por qué la nueva operación es estadísticamente adecuada.
- Las unidades mostradas deben coincidir con los valores que realmente se representan.

Para las métricas de vista de los **gráficos**, la normalización por tamaño local se limita a:

- velocidad de navegación;
- distancia al objetivo;
- picos de movimiento.

No extiendas esa normalización a otras métricas sin una decisión explícita y documentada.

### 6.5 Gráficos

Al modificar Interaction/Timeline, Forest o Radar:

- conserva nombres de archivos/grupos identificables en las leyendas;
- evita etiquetas genéricas como `Obs. 1` cuando se conoce el nombre de la fuente;
- las explicaciones deben indicar qué significa un valor alto cuando la interpretación no sea evidente;
- las unidades de la etiqueta deben corresponder con la transformación aplicada al dato;
- comprueba inglés y español;
- verifica el resultado con uno y con varios CSV;
- verifica también los grupos de CSV.

## 7. Compatibilidad del CSV

Los cambios del esquema CSV deben preservar la lectura de datos anteriores siempre que sea viable.

Si añades, eliminas o cambias el significado de una columna:

1. actualiza la versión del esquema cuando corresponda;
2. actualiza `csv_schema.py` en Logger e Insights;
3. comprueba CSV actuales y legacy;
4. evita depender de `UserID` para reconocer un CSV válido si el archivo puede estar anonimizado;
5. documenta la migración o compatibilidad en el pull request.

No incluyas en el repositorio CSV con información identificable.

## 8. Archivos que no deben enviarse

No incluyas en commits, pull requests o ZIP de distribución:

```text
__pycache__/
*.pyc
*.pyo
*.log
*.tmp
*.bak
site-packages/
.venv/
venv/
```

Tampoco incluyas:

- binarios desplegados de dependencias;
- archivos de configuración personales del IDE;
- rutas absolutas locales;
- datos personales;
- archivos `.blend` de participantes sin anonimizar;
- resultados temporales de pruebas.

## 9. Validación antes de un pull request

### 9.1 Validación sintáctica

Sobre el código extraído puedes ejecutar:

```bash
python -m compileall Ize_Insights
python -m compileall Ize_Logger
```

Elimina después cualquier `__pycache__` generado antes de empaquetar.

### 9.2 Validación manual en Blender

Los cambios relacionados con `bpy`, paneles, operadores, handlers, timers, depsgraph o geometría deben probarse dentro de Blender.

Comprueba como mínimo lo que corresponda al cambio:

- instalación limpia del ZIP;
- activación y desactivación del add-on;
- `register()` / `unregister()` sin errores;
- cambio entre inglés y español;
- guardar y volver a abrir `.blend` cuando afecte al logger;
- Object Mode y Edit Mode;
- operaciones intensivas sobre geometría para cambios del logger;
- un CSV y varios CSV para Insights;
- grupos de CSV;
- tablas y paginación;
- G1 / Timeline, G2 / Forest y G3 / Radar cuando se modifique código gráfico;
- instalación de dependencias desde una instalación nueva de Ize Insights.

### 9.3 Comprobación del ZIP

Antes de publicar un paquete:

- verifica que la carpeta raíz sea `Ize_Insights/` o `Ize_Logger/`;
- verifica que no exista una carpeta intermedia accidental;
- comprueba que no haya `__pycache__`, `.pyc` o binarios desplegados;
- en Insights, comprueba que `wheels/` y `requirements.txt` estén presentes;
- en Insights, comprueba que no se distribuya un `site-packages/` ya instalado;
- instala el ZIP resultante en una instalación limpia de Blender.

## 10. Commits

Usa commits pequeños y descriptivos. Ejemplos:

```text
Fix radar metric labels in Spanish
Exclude Smooth by Angle from modifier count
Normalize navigation graph metrics by local scene size
Prevent heavy geometry sampling in depsgraph bursts
Fix CSV legacy schema detection
```

Evita mezclar en un mismo commit:

- refactorizaciones grandes;
- cambios de formato;
- nuevas métricas;
- correcciones no relacionadas.

## 11. Pull requests

Todo pull request debería indicar:

- problema o necesidad que resuelve;
- componente afectado (`Ize Insights`, `Logger Consent`, `Logger Manual`);
- archivos principales modificados;
- cambios de comportamiento;
- cambios de esquema CSV, si existen;
- cambios de unidades o semántica estadística, si existen;
- pruebas manuales realizadas;
- versión de Blender utilizada;
- sistema operativo utilizado;
- riesgos conocidos o aspectos pendientes.

Para correcciones visuales, adjunta capturas antes/después cuando sea útil.

Para cierres o problemas de estabilidad de Blender, describe los pasos exactos para reproducirlos y la prueba de estrés utilizada para validar la corrección.

## 12. Checklist rápida del PR

Antes de solicitar revisión:

- [ ] El cambio está limitado al objetivo del PR.
- [ ] El código compila sin errores.
- [ ] El add-on se registra y desregistra correctamente.
- [ ] Se ha probado dentro de Blender.
- [ ] Inglés y español están revisados si hay cambios visibles.
- [ ] Consent y Manual están sincronizados si el cambio afecta al Logger compartido.
- [ ] Los cambios de CSV mantienen compatibilidad o están documentados.
- [ ] Las unidades mostradas coinciden con los datos calculados.
- [ ] No hay `__pycache__`, `.pyc`, `site-packages` ni binarios desplegados.
- [ ] Los ZIP instalables tienen la estructura correcta.
- [ ] No se han incluido datos personales o rutas locales.
- [ ] README/docs se han actualizado si cambia el comportamiento público.

## 13. Licencia

Al contribuir aceptas que tus cambios se distribuyan bajo la licencia del proyecto, **GNU General Public License v3.0 or later (`GPL-3.0-or-later`)**.
