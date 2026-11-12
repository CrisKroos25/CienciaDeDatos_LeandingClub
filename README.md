# Análisis y predicción del incumplimiento de préstamos — LendingClub

Proyecto académico de ciencia de datos que estudia los factores asociados al incumplimiento de préstamos de **LendingClub** y compara modelos de clasificación para estimar el riesgo crediticio. El flujo combina **Python**, **R**, informes reproducibles con **Quarto** y un panel interactivo en **Power BI**.

> **Alcance:** el análisis se limita a préstamos con resultado conocido (`Fully Paid` o `Charged Off`). Las asociaciones observadas no demuestran causalidad y el rendimiento medido sobre estos préstamos no garantiza el mismo desempeño en préstamos nuevos o todavía activos.

## Objetivos

- Comprender y depurar los datos de préstamos.
- Explorar relaciones entre características financieras, categorías de riesgo e incumplimiento.
- Entrenar y comparar modelos de clasificación sin utilizar información del conjunto de prueba durante el entrenamiento.
- Evaluar los modelos y comunicar sus resultados mediante informes y visualizaciones.

## Datos y metodología

El conjunto de datos original contiene aproximadamente **2,260,668 registros** y **145 columnas**. Para el análisis se seleccionan los **1,303,607 préstamos** cuyo estado final es `Fully Paid` o `Charged Off`.

La variable objetivo `incumplimiento` se codifica como:

| Valor | Estado original | Interpretación |
|---|---|---|
| `0` | `Fully Paid` | Préstamo pagado |
| `1` | `Charged Off` | Préstamo castigado / incumplido |

Se utiliza una separación **cronológica** con fecha de corte **2016-09-01**:

| Conjunto | Registros | Uso |
|---|---:|---|
| Entrenamiento/desarrollo | 1,026,281 | Exploración, preparación y entrenamiento |
| Prueba | 277,326 | Evaluación final sobre datos separados |

El conjunto de desarrollo presenta aproximadamente **19.59 %** de incumplimiento. Estas cifras describen la versión de datos utilizada en el proyecto; deben comprobarse al reproducirlo.

## Estructura del repositorio

```text
lendingclub-data-science/
├── .gitignore
├── _quarto.yml
├── lendingclub-data-science.Rproj
├── lendingclub-data-science.qmd
├── requirements.txt
├── README.md
├── data/
│   ├── README.md                 # Fuente e instrucciones de obtención (recomendado)
│   ├── raw/                      # Datos originales; no versionar archivos grandes
│   └── processed/                # Archivos generados durante la limpieza
├── reports/
│   ├── 01_comprension.qmd
│   ├── 02_limpieza_python.qmd
│   ├── 03_analisis_exploratorio.qmd
│   ├── 04_modelado_predictivo.qmd
│   ├── 05_evaluacion_modelos.qmd
│   └── 06_conclusiones.qmd
├── outputs/                      # Tablas, métricas, modelos y resultados generados
├── _output/                      # HTML renderizados y sus recursos
└── powerBi/                      # Archivo Power BI (.pbix), si se publica
```

La estructura representa las rutas utilizadas en el proyecto; algunas carpetas se generan al ejecutar los informes. **No se incluyen por defecto los archivos de datos de gran tamaño.**

## Requisitos

- Python 3 y `pip`.
- R y RStudio (RStudio es opcional si se trabaja desde terminal).
- Quarto CLI.
- Bibliotecas Python declaradas en `requirements.txt` (entre ellas, según el análisis, `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `pyarrow` y `joblib`).
- Paquetes R utilizados por los fragmentos de código R, como `readr` y otros que aparezcan en los `.qmd`.
- Power BI Desktop **solo** para abrir o modificar el panel `.pbix`.

> Para una reproducción estricta, conviene fijar las versiones exactas de Python, R y paquetes, así como registrar el sistema operativo y las semillas aleatorias. Si `requirements.txt` no contiene versiones fijadas, los resultados pueden variar entre instalaciones.

## Reproducción paso a paso
### 0. Descargar el Dataset en el siguiente link
https://drive.google.com/file/d/11ikQ0UTjBROZ0spzhmpxjREERp-UhSD-/view?usp=sharing

### 1. Clonar el repositorio

```bash
git clone <URL-DEL-REPOSITORIO>
cd lendingclub-data-science
```

Sustituir `<URL-DEL-REPOSITORIO>` por la dirección real del proyecto.

### 2. Crear el entorno de Python

**Windows (PowerShell):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

**Linux/macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Instalar por separado **Quarto**, **R** y los paquetes R requeridos. Verificar que el motor de ejecución de Quarto use el entorno Python creado (o el kernel configurado para el proyecto).

### 3. Obtener los datos

1. Descargar el conjunto de datos LendingClub desde la **fuente específica documentada para esta versión del proyecto**.
2. Comprobar sus condiciones de uso y redistribución.
3. Colocar el CSV original en:

```text
data/raw/loan.csv
```

El archivo debe conservar los nombres de columnas esperados por los informes. **Pendiente antes de publicar:** agregar en `data/README.md` la URL exacta de descarga, versión del dataset, fecha de acceso y, de ser posible, checksum SHA-256. Sin esa referencia no puede garantizarse una reproducción exacta.

### 4. Ejecutar los seis informes en orden

Los informes contienen rutas relativas como `../data/raw/loan.csv` y `../outputs/models/`, por lo que **sus bloques de código deben ejecutarse con `reports/` como directorio de trabajo**. Revisar `_quarto.yml` y la configuración de ejecución antes de renderizar.

Desde la raíz del repositorio, si la configuración de Quarto conserva ese directorio de ejecución:

```bash
quarto render reports/01_comprension.qmd --to html
quarto render reports/02_limpieza_python.qmd --to html
quarto render reports/03_analisis_exploratorio.qmd --to html
quarto render reports/04_modelado_predictivo.qmd --to html
quarto render reports/05_evaluacion_modelos.qmd --to html
quarto render reports/06_conclusiones.qmd --to html
```

| Orden | Informe | Función |
|---:|---|---|
| 1 | `01_comprension.qmd` | Examina el dataset y sus variables |
| 2 | `02_limpieza_python.qmd` | Limpia, transforma y separa los datos; genera archivos en `data/processed/` |
| 3 | `03_analisis_exploratorio.qmd` | Análisis univariado, bivariado, temporal y estadístico |
| 4 | `04_modelado_predictivo.qmd` | Entrena y compara modelos; genera resultados en `outputs/models/` |
| 5 | `05_evaluacion_modelos.qmd` | Evalúa los modelos con el conjunto de prueba y guarda métricas |
| 6 | `06_conclusiones.qmd` | Resume los hallazgos a partir de los resultados anteriores |

**Importante:** no ejecutar directamente las fases 3–6 antes de la limpieza y el modelado, ya que consumen archivos generados en etapas anteriores. El entrenamiento puede requerir tiempo y memoria considerables.

### 5. Verificar resultados

Comprobar al menos:

- Que la fase de limpieza genere los archivos de `data/processed/` (incluidos `train_prepared.parquet`, `train_raw_split.parquet`, `test_raw_split.parquet` y `metadata.json`, según la versión de los informes).
- Que el modelado produzca `outputs/models/comparacion_validacion.csv` y los artefactos requeridos por la evaluación.
- Que la evaluación produzca `outputs/tables/metricas_prueba.csv` y `outputs/tables/calibracion_prueba.csv`.
- Que los seis HTML se generen sin errores y contengan las figuras y tablas esperadas.
- Que los tamaños de entrenamiento/prueba y la distribución de la variable objetivo coincidan con los valores de referencia, si se usa la misma versión de datos y código.

**Nota:** las cifras de referencia no sustituyen la validación de resultados. Los informes y sus salidas deben revisarse después de ejecutarlos en el entorno local.

## Dashboard Power BI

El panel de `powerBi/` complementa los informes con indicadores y visualizaciones interactivas:

- Total de préstamos, cantidad de incumplimientos y tasa de incumplimiento.
- Segmentación por calificación crediticia (`grade`).
- Matriz de incumplimiento por calificación y plazo.
- Distribución de préstamos pagados e incumplidos.
- Dispersión de monto frente a tasa de interés.
- Visual de **Influenciadores clave**, que destaca asociaciones estadísticas, **no causas demostradas**.

El archivo `.pbix` puede contener una copia de los datos importados. Antes de publicarlo, comprobar su tamaño y los permisos de redistribución del dataset.

## Interpretación y limitaciones

- Se estudian únicamente préstamos con estado final conocido; la selección puede introducir sesgos respecto de la cartera completa.
- La separación temporal reduce ciertos riesgos de evaluación optimista, pero no elimina por sí sola todas las formas de fuga de información.
- Las variables usadas para predecir deben estar disponibles en el momento en que se pretende realizar la predicción; cualquier variable conocida solo después del desembolso debe excluirse de un modelo de riesgo de originación.
- Los patrones del análisis exploratorio y los influenciadores de Power BI describen **asociaciones**, no relaciones causales.
- Los resultados dependen de la versión del dataset, los paquetes, las decisiones de limpieza y el procedimiento de validación.

## Archivos publicados y política de datos

Se recomienda versionar los `.qmd`, archivos de configuración, dependencias, documentación y resultados pequeños. Evitar subir datos originales o procesados grandes, entornos virtuales, credenciales y archivos temporales. Si se publican HTML en `_output/`, incluir los recursos que necesitan para mostrarse correctamente.

## Estado de reproducibilidad

El repositorio proporciona el **flujo documentado de ejecución**. Para declarar reproducción completa y exacta es necesario verificar: fuente/versionado de los datos, instalación de dependencias R y Python, configuración de Quarto, generación de todos los artefactos intermedios y comparación de métricas finales.

## Uso académico

Proyecto desarrollado con fines educativos y de análisis estadístico. No constituye una herramienta validada para decisiones crediticias reales.
