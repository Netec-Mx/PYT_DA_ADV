# Práctica: Crear visualizaciones estadísticas con Matplotlib/Seaborn, anotarlas y justificar la selección de gráficos

## Metadatos

| Campo | Valor |
|---|---|
| Duración | 120 minutos |
| Complejidad | Media |
| Nivel de Bloom | Aplicar |

## Descripción General

En este laboratorio creará un conjunto de datos comerciales completamente sintético, no identificable y reproducible mediante NumPy. Validará su calidad, calculará estadísticas descriptivas y agregaciones, y generará visualizaciones estadísticas con Matplotlib y Seaborn.

El trabajo incluye histogramas, boxplots, scatterplots, gráficos de líneas y mapas de calor de correlación. Para cada visualización documentará la pregunta analítica, la justificación de diseño, una alternativa menos apropiada y una conclusión sustentada sin afirmar causalidad.

## Objetivos de Aprendizaje

- [ ] Generar y validar un DataFrame sintético reproducible sin PII real.
- [ ] Crear un histograma, boxplot, scatterplot, gráfico de líneas y mapa de calor de correlación con Matplotlib y Seaborn.
- [ ] Aplicar títulos, etiquetas, escalas, leyendas, anotaciones y colores accesibles.
- [ ] Comparar representaciones gráficas y justificar la selección según la pregunta, las variables y la audiencia.
- [ ] Exportar código, tablas, gráficos y decisiones de diseño como evidencia reproducible para el laboratorio 05-00-02.

## Prerrequisitos

**Conocimientos requeridos**

- Python intermedio: funciones, estructuras de datos, control de flujo y excepciones básicas.
- Fundamentos de Pandas y NumPy: `DataFrame`, selección de columnas, filtros, `groupby`, `describe` y exportación de archivos.
- Fundamentos estadísticos: media, mediana, desviación estándar, percentiles, correlación de Pearson y distinción entre asociación y causalidad.
- Uso básico de JupyterLab o Visual Studio Code.
- Principios de minimización de datos y prohibición de incorporar PII real.

**Acceso requerido**

- Acceso al directorio raíz del proyecto creado en laboratorios anteriores.
- Entorno virtual `.venv` existente y asociado al intérprete Python del curso.
- Permisos de escritura en las carpetas `salidas/` y `evidencia/`.
- JupyterLab o Visual Studio Code con el kernel de `.venv`.
- No se requiere conexión a Internet durante la ejecución si las dependencias ya están instaladas.

## Entorno de Laboratorio

No cree un nuevo entorno virtual ni modifique el intérprete global. Reutilice la estructura existente del proyecto, incluida la carpeta `.venv`, y conserve sin cambios las salidas de `salidas/lab_04_00_01` y `salidas/lab_04_00_02`.

| Componente | Versión requerida | Arquitectura o edición | Fuente oficial |
|---|---:|---|---|
| Python | 3.12.1 | 64 bits | https://www.python.org/downloads/release/python-3121/ |
| pip | 24.3.1 | Paquete Python | https://pypi.org/project/pip/24.3.1/ |
| pandas | 3.0.0 | Paquete Python | https://pypi.org/project/pandas/3.0.0/ |
| NumPy | 2.2.0 | Paquete Python | https://pypi.org/project/numpy/2.2.0/ |
| Matplotlib | 3.10.0 | Paquete Python | https://pypi.org/project/matplotlib/3.10.0/ |
| Seaborn | 0.13.2 | Paquete Python | https://pypi.org/project/seaborn/0.13.2/ |
| Plotly | 5.24.1 | Paquete Python | https://pypi.org/project/plotly/5.24.1/ |
| PyArrow | 18.1.0 | Paquete Python | https://pypi.org/project/pyarrow/18.1.0/ |
| JupyterLab | 4.3.4 | Aplicación Python | https://pypi.org/project/jupyterlab/4.3.4/ |
| Visual Studio Code | 1.96.2 | Edición de 64 bits | https://code.visualstudio.com/updates/v1_96 |
| SQLAlchemy | [VERSIÓN POR VALIDAR] | Paquete Python; no se utiliza en este laboratorio | https://www.sqlalchemy.org/ |
| scikit-learn | 1.6.0 | Paquete Python; no se utiliza en este laboratorio | https://pypi.org/project/scikit-learn/1.6.0/ |

> **Nota de alcance:** SQLite, SQLAlchemy, credenciales, puertos HTTP y contenedores no se utilizan en esta práctica. No cree ni modifique bases SQLite existentes. No se usan servicios de Microsoft 365 Copilot, Copilot Chat, Designer ni Planner; por tanto, no se requiere licencia ni configuración de estas herramientas.

Active el entorno virtual desde la raíz del proyecto.

```bash
source .venv/bin/activate
```

En Windows PowerShell, use el comando equivalente:

```powershell
.venv\Scripts\Activate.ps1
```

Verifique intérprete y versiones. No instale paquetes fuera de `.venv`.

```bash
python --version
python -m pip --version
```

```python
import sys
import numpy as np
import pandas as pd
import matplotlib
import seaborn as sns
import plotly

print("Intérprete:", sys.executable)
print("Python:", sys.version.split()[0])
print("NumPy:", np.__version__)
print("pandas:", pd.__version__)
print("Matplotlib:", matplotlib.__version__)
print("Seaborn:", sns.__version__)
print("Plotly:", plotly.__version__)
```

## Instrucciones Paso a Paso

### Paso 1: Crear el cuaderno y registrar el alcance

**Objetivo:** Crear el archivo de trabajo reproducible y declarar la naturaleza sintética del conjunto de datos.

**Instrucciones:**

1. Abra JupyterLab o Visual Studio Code desde la raíz del proyecto.
2. Seleccione el kernel vinculado a `.venv`.
3. Cree el cuaderno `lab_05_00_01_visualizaciones.ipynb`.
4. Agregue una celda Markdown con su nombre, fecha, identificador del laboratorio y la frase exacta: `Datos sintéticos; no contiene PII real.`
5. Registre que este laboratorio crea un conjunto sintético independiente para fines de visualización y no reemplaza archivos analíticos de laboratorios anteriores.

**Salida esperada:** Existe el cuaderno y utiliza el kernel del entorno virtual del curso.

**Verificación:** La ruta mostrada por `sys.executable` contiene `.venv`.

```python
import sys
from pathlib import Path

print(sys.executable)
print(Path.cwd())
```

### Paso 2: Importar bibliotecas y configurar el estilo visual

**Objetivo:** Cargar las bibliotecas necesarias y establecer configuraciones visuales consistentes y accesibles.

**Instrucciones:**

1. Inserte una celda de código después de la celda de contexto.
2. Importe NumPy, Pandas, Matplotlib, Seaborn y Plotly Express.
3. Configure una semilla fija mediante `RANDOM_STATE_CURSO = 3102`.
4. Aplique el tema `whitegrid` de Seaborn.
5. Utilice una paleta apta para personas con deficiencia de visión de color.

**Salida esperada:** Las bibliotecas se importan sin errores y quedan disponibles para las visualizaciones.

**Verificación:** La celda imprime las versiones y no muestra errores de importación.

```python
from pathlib import Path

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

RANDOM_STATE_CURSO = 3102
rng = np.random.default_rng(RANDOM_STATE_CURSO)

sns.set_theme(
    style="whitegrid",
    context="notebook",
    palette="colorblind"
)

print(f"Semilla reproducible: {RANDOM_STATE_CURSO}")
print(f"NumPy: {np.__version__}")
print(f"pandas: {pd.__version__}")
print(f"Matplotlib: {plt.matplotlib.__version__}")
print(f"Seaborn: {sns.__version__}")
```

### Paso 3: Crear directorios de salida y evidencia

**Objetivo:** Preparar ubicaciones reproducibles sin alterar las carpetas generadas en laboratorios anteriores.

**Instrucciones:**

1. Mantenga la raíz actual del proyecto como directorio de trabajo.
2. Cree únicamente `salidas/lab_05_00_01`.
3. Cree únicamente `evidencia/lab_05_00_01`.
4. No elimine, renombre ni reemplace archivos de laboratorios 04-00-01 o 04-00-02.
5. Imprima las rutas para confirmar que son escribibles.

**Salida esperada:** Existen los directorios de salida y evidencia del laboratorio actual.

**Verificación:** Ambas rutas se muestran y existen en disco.

```python
RAIZ_PROYECTO = Path.cwd()
DIR_SALIDAS = RAIZ_PROYECTO / "salidas" / "lab_05_00_01"
DIR_EVIDENCIA = RAIZ_PROYECTO / "evidencia" / "lab_05_00_01"

DIR_SALIDAS.mkdir(parents=True, exist_ok=True)
DIR_EVIDENCIA.mkdir(parents=True, exist_ok=True)

print("Salidas:", DIR_SALIDAS.resolve())
print("Evidencia:", DIR_EVIDENCIA.resolve())
print("Existe salidas:", DIR_SALIDAS.exists())
print("Existe evidencia:", DIR_EVIDENCIA.exists())
```

### Paso 4: Generar el conjunto sintético reproducible

**Objetivo:** Crear observaciones comerciales sintéticas con variables analíticas no identificables.

**Instrucciones:**

1. Genere 360 observaciones sintéticas distribuidas entre doce períodos mensuales.
2. Use los segmentos `Esencial`, `Estándar` y `Premium`.
3. Incluya las columnas `periodo`, `segmento`, `importe_ventas_eur`, `unidades`, `descuento_pct` y `satisfaccion_1_5`.
4. No incluya nombres, correos, teléfonos, direcciones, identificadores de clientes ni otros datos personales.
5. Agregue una columna técnica `observacion_id` únicamente para trazabilidad interna y exclúyala de correlaciones.

**Salida esperada:** Existe un DataFrame con 360 filas, variables analíticas y resultados reproducibles al ejecutar la celda nuevamente.

**Verificación:** La vista previa muestra las columnas solicitadas y no contiene PII.

```python
N_OBSERVACIONES = 360

periodos = pd.date_range("2025-01-01", periods=12, freq="MS")
segmentos = np.array(["Esencial", "Estándar", "Premium"])

periodo = rng.choice(periodos, size=N_OBSERVACIONES, replace=True)
segmento = rng.choice(
    segmentos,
    size=N_OBSERVACIONES,
    replace=True,
    p=[0.40, 0.40, 0.20]
)

precio_base = np.select(
    [segmento == "Esencial", segmento == "Estándar", segmento == "Premium"],
    [24.0, 42.0, 68.0]
)

media_unidades = np.select(
    [segmento == "Esencial", segmento == "Estándar", segmento == "Premium"],
    [7.0, 10.0, 14.0]
)

unidades = rng.poisson(lam=media_unidades) + 1
descuento_pct = np.clip(
    rng.normal(loc=10, scale=6, size=N_OBSERVACIONES),
    0,
    30
).round(1)

factor_temporal = 1 + ((pd.Series(periodo).dt.month.to_numpy() - 1) * 0.008)
ruido_ventas = rng.normal(loc=1.0, scale=0.12, size=N_OBSERVACIONES)

importe_ventas_eur = np.round(
    unidades * precio_base * (1 - descuento_pct / 100) * factor_temporal * ruido_ventas,
    2
)

satisfaccion_1_5 = np.clip(
    3.2
    + (segmento == "Premium") * 0.45
    + (segmento == "Estándar") * 0.20
    - descuento_pct * 0.015
    + rng.normal(loc=0, scale=0.45, size=N_OBSERVACIONES),
    1,
    5
).round(2)

df = pd.DataFrame(
    {
        "observacion_id": np.arange(1, N_OBSERVACIONES + 1),
        "periodo": pd.to_datetime(periodo),
        "segmento": pd.Categorical(
            segmento,
            categories=["Esencial", "Estándar", "Premium"],
            ordered=True
        ),
        "importe_ventas_eur": importe_ventas_eur,
        "unidades": unidades.astype("int64"),
        "descuento_pct": descuento_pct,
        "satisfaccion_1_5": satisfaccion_1_5,
    }
).sort_values(["periodo", "segmento", "observacion_id"]).reset_index(drop=True)

df.head(10)
```

### Paso 5: Validar tipos, faltantes, rangos y reglas aplicadas

**Objetivo:** Confirmar que el conjunto cumple reglas mínimas de calidad antes de visualizarlo.

**Instrucciones:**

1. Revise el tipo de dato de cada columna.
2. Calcule valores faltantes por columna.
3. Confirme que existen exactamente 360 observaciones.
4. Verifique los rangos plausibles definidos para ventas, unidades, descuento y satisfacción.
5. Documente que las reglas no modifican registros porque el generador ya restringe los valores sintéticos a rangos analíticamente plausibles.
6. Si alguna validación falla, detenga el proceso y revise la generación del conjunto antes de crear gráficos.

**Salida esperada:** Todas las validaciones devuelven valores verdaderos y no se detectan faltantes.

**Verificación:** El diccionario `validaciones` contiene únicamente valores `True`.

```python
rangos_esperados = {
    "importe_ventas_eur": (1, 5000),
    "unidades": (1, 100),
    "descuento_pct": (0, 30),
    "satisfaccion_1_5": (1, 5),
}

validaciones = {
    "numero_observaciones": len(df) == N_OBSERVACIONES,
    "periodo_datetime": pd.api.types.is_datetime64_any_dtype(df["periodo"]),
    "sin_faltantes": df.isna().sum().sum() == 0,
    "ventas_positivas": df["importe_ventas_eur"].between(*rangos_esperados["importe_ventas_eur"]).all(),
    "unidades_positivas": df["unidades"].between(*rangos_esperados["unidades"]).all(),
    "descuento_plausible": df["descuento_pct"].between(*rangos_esperados["descuento_pct"]).all(),
    "satisfaccion_plausible": df["satisfaccion_1_5"].between(*rangos_esperados["satisfaccion_1_5"]).all(),
    "sin_duplicados_tecnicos": df["observacion_id"].is_unique,
}

print("Tipos de datos:")
display(df.dtypes.to_frame("tipo"))

print("Valores faltantes:")
display(df.isna().sum().to_frame("faltantes"))

print("Resultado de validaciones:")
display(pd.Series(validaciones, name="cumple_regla").to_frame())

if not all(validaciones.values()):
    raise ValueError("Se detectó una validación fallida. Revise el conjunto antes de visualizar.")
```

### Paso 6: Calcular estadísticas descriptivas y agregaciones

**Objetivo:** Obtener contexto cuantitativo para interpretar los gráficos sin depender exclusivamente de la percepción visual.

**Instrucciones:**

1. Calcule estadísticas descriptivas de las variables numéricas pertinentes.
2. Calcule ventas totales, promedio y mediana por segmento.
3. Calcule ventas agregadas mensuales.
4. Guarde las tablas de apoyo en formato CSV.
5. Use estas tablas como referencia para las anotaciones de los gráficos.

**Salida esperada:** Se muestran tres tablas analíticas y se exportan archivos CSV en `salidas/lab_05_00_01`.

**Verificación:** Existen los archivos `estadisticas_descriptivas.csv`, `ventas_por_segmento.csv` y `ventas_mensuales.csv`.

```python
columnas_numericas = [
    "importe_ventas_eur",
    "unidades",
    "descuento_pct",
    "satisfaccion_1_5",
]

estadisticas_descriptivas = df[columnas_numericas].describe().T
ventas_por_segmento = (
    df.groupby("segmento", observed=True)
    .agg(
        observaciones=("observacion_id", "count"),
        ventas_totales_eur=("importe_ventas_eur", "sum"),
        ventas_promedio_eur=("importe_ventas_eur", "mean"),
        ventas_mediana_eur=("importe_ventas_eur", "median"),
        desviacion_ventas_eur=("importe_ventas_eur", "std"),
    )
    .reset_index()
)

ventas_mensuales = (
    df.groupby("periodo", as_index=False)
    .agg(
        ventas_totales_eur=("importe_ventas_eur", "sum"),
        unidades_totales=("unidades", "sum"),
        satisfaccion_promedio=("satisfaccion_1_5", "mean"),
    )
    .sort_values("periodo")
)

display(estadisticas_descriptivas.round(2))
display(ventas_por_segmento.round(2))
display(ventas_mensuales.round(2))
```

```python
estadisticas_descriptivas.to_csv(
    DIR_SALIDAS / "estadisticas_descriptivas.csv",
    index=True,
    encoding="utf-8"
)

ventas_por_segmento.to_csv(
    DIR_SALIDAS / "ventas_por_segmento.csv",
    index=False,
    encoding="utf-8"
)

ventas_mensuales.to_csv(
    DIR_SALIDAS / "ventas_mensuales.csv",
    index=False,
    encoding="utf-8"
)

df.to_csv(
    DIR_SALIDAS / "datos_sinteticos_visualizacion.csv",
    index=False,
    encoding="utf-8"
)

df.to_parquet(
    DIR_SALIDAS / "datos_sinteticos_visualizacion.parquet",
    index=False
)
```

### Paso 7: Crear un histograma de distribución de ventas

**Objetivo:** Analizar la distribución de los importes de venta por observación.

**Instrucciones:**

1. Formule la pregunta: “¿Cómo se distribuyen los importes de venta de las observaciones sintéticas?”
2. Cree un histograma con 18 intervalos y una curva KDE.
3. Añada una línea vertical para la mediana.
4. Incluya título informativo, ejes con unidades y anotación del hallazgo observable.
5. Exporte la imagen como `01_histograma_ventas.png`.

**Salida esperada:** Se genera un histograma que permite observar frecuencia, dispersión y posible asimetría de ventas.

**Verificación:** La figura contiene título, etiqueta de eje X en euros, etiqueta de eje Y, línea de mediana y anotación.

```python
mediana_ventas = df["importe_ventas_eur"].median()

fig, ax = plt.subplots(figsize=(10, 6))

sns.histplot(
    data=df,
    x="importe_ventas_eur",
    bins=18,
    kde=True,
    color="#2A6FBB",
    edgecolor="white",
    ax=ax
)

ax.axvline(
    mediana_ventas,
    color="#D55E00",
    linestyle="--",
    linewidth=2,
    label=f"Mediana: {mediana_ventas:,.0f} EUR"
)

ax.annotate(
    f"La mediana se ubica en {mediana_ventas:,.0f} EUR.",
    xy=(mediana_ventas, 30),
    xytext=(mediana_ventas + 120, 42),
    arrowprops={"arrowstyle": "->", "color": "#333333"},
    color="#333333"
)

ax.set_title("Distribución de importes de venta por observación sintética")
ax.set_xlabel("Importe de ventas (EUR)")
ax.set_ylabel("Número de observaciones")
ax.legend()
fig.tight_layout()

fig.savefig(DIR_SALIDAS / "01_histograma_ventas.png", dpi=160, bbox_inches="tight")
plt.show()
```

**Justificación de selección:** El histograma es adecuado porque la pregunta estudia la distribución de una variable numérica continua. Permite identificar concentración, dispersión, forma general y posibles colas.

**Alternativa menos apropiada:** Un gráfico de líneas conectaría observaciones sin un orden temporal o secuencial relevante y podría sugerir una tendencia inexistente.

### Paso 8: Crear un boxplot de ventas por segmento

**Objetivo:** Comparar posición central, dispersión y posibles valores atípicos entre segmentos.

**Instrucciones:**

1. Formule la pregunta: “¿Cómo difiere la distribución de ventas entre los segmentos?”
2. Cree un boxplot de `importe_ventas_eur` por `segmento`.
3. Use una paleta accesible y mantenga el orden lógico de los segmentos.
4. Anote el segmento con mayor mediana de ventas.
5. Exporte la imagen como `02_boxplot_ventas_segmento.png`.

**Salida esperada:** Se genera un boxplot que facilita comparar medianas, rango intercuartílico y valores potencialmente atípicos.

**Verificación:** El gráfico presenta tres cajas, una por segmento, e identifica el segmento con mayor mediana observada.

```python
segmento_mayor_mediana = (
    ventas_por_segmento
    .sort_values("ventas_mediana_eur", ascending=False)
    .iloc[0]
)

fig, ax = plt.subplots(figsize=(10, 6))

sns.boxplot(
    data=df,
    x="segmento",
    y="importe_ventas_eur",
    hue="segmento",
    palette="colorblind",
    legend=False,
    ax=ax
)

ax.annotate(
    (
        f"Mayor mediana observada:\n"
        f"{segmento_mayor_mediana['segmento']} "
        f"({segmento_mayor_mediana['ventas_mediana_eur']:,.0f} EUR)"
    ),
    xy=(2, segmento_mayor_mediana["ventas_mediana_eur"]),
    xytext=(1.35, df["importe_ventas_eur"].max() * 0.93),
    arrowprops={"arrowstyle": "->", "color": "#333333"},
    bbox={"boxstyle": "round,pad=0.4", "facecolor": "white", "edgecolor": "#333333"}
)

ax.set_title("Distribución de importes de venta por segmento sintético")
ax.set_xlabel("Segmento")
ax.set_ylabel("Importe de ventas (EUR)")
fig.tight_layout()

fig.savefig(DIR_SALIDAS / "02_boxplot_ventas_segmento.png", dpi=160, bbox_inches="tight")
plt.show()
```

**Justificación de selección:** El boxplot resume la distribución de ventas de cada categoría mediante cuartiles, mediana, dispersión y posibles valores atípicos. Resulta más informativo que comparar únicamente promedios.

**Alternativa menos apropiada:** Un gráfico circular mostraría proporciones agregadas, pero ocultaría la variabilidad interna de cada segmento y dificultaría detectar valores extremos.

### Paso 9: Crear un scatterplot de unidades y ventas

**Objetivo:** Explorar la asociación entre dos variables numéricas e identificar patrones por segmento.

**Instrucciones:**

1. Formule la pregunta: “¿Qué asociación observable existe entre unidades e importe de ventas?”
2. Cree un scatterplot con `unidades` en el eje X y `importe_ventas_eur` en el eje Y.
3. Diferencie segmentos por color usando la paleta accesible.
4. Calcule la correlación de Pearson únicamente como medida descriptiva.
5. Incluya una anotación que indique asociación, no causalidad.
6. Exporte la imagen como `03_scatter_unidades_ventas.png`.

**Salida esperada:** Se genera una nube de puntos que permite inspeccionar la asociación entre unidades y ventas.

**Verificación:** La leyenda identifica los segmentos y la anotación incluye el coeficiente de correlación redondeado.

```python
correlacion_unidades_ventas = df["unidades"].corr(
    df["importe_ventas_eur"],
    method="pearson"
)

fig, ax = plt.subplots(figsize=(10, 6))

sns.scatterplot(
    data=df,
    x="unidades",
    y="importe_ventas_eur",
    hue="segmento",
    palette="colorblind",
    alpha=0.75,
    s=65,
    ax=ax
)

ax.annotate(
    (
        f"Correlación de Pearson observada: r = {correlacion_unidades_ventas:.2f}\n"
        "La asociación no demuestra causalidad."
    ),
    xy=(df["unidades"].quantile(0.72), df["importe_ventas_eur"].quantile(0.30)),
    bbox={"boxstyle": "round,pad=0.4", "facecolor": "white", "edgecolor": "#333333"}
)

ax.set_title("Asociación entre unidades e importe de ventas")
ax.set_xlabel("Unidades vendidas")
ax.set_ylabel("Importe de ventas (EUR)")
ax.legend(title="Segmento")
fig.tight_layout()

fig.savefig(DIR_SALIDAS / "03_scatter_unidades_ventas.png", dpi=160, bbox_inches="tight")
plt.show()
```

**Justificación de selección:** El scatterplot representa cada observación y permite evaluar asociación, agrupamientos por segmento, dispersión y casos alejados del patrón general.

**Alternativa menos apropiada:** Un histograma de unidades mostraría su distribución, pero no permitiría observar su relación con el importe de ventas.

### Paso 10: Crear un gráfico de líneas de evolución mensual

**Objetivo:** Mostrar cómo cambian las ventas agregadas a través de períodos con orden temporal.

**Instrucciones:**

1. Formule la pregunta: “¿Cómo evolucionan las ventas totales agregadas por mes?”
2. Utilice la tabla `ventas_mensuales`, no las observaciones individuales.
3. Cree un gráfico de líneas con marcadores.
4. Anote el mes con mayor venta total observada.
5. Rote las etiquetas del eje temporal si es necesario.
6. Exporte la imagen como `04_lineas_ventas_mensuales.png`.

**Salida esperada:** Se genera un gráfico temporal con doce puntos mensuales y una anotación del período de mayor venta agregada.

**Verificación:** El eje X utiliza períodos mensuales ordenados y el eje Y expresa ventas totales en euros.

```python
mes_maximo = ventas_mensuales.loc[
    ventas_mensuales["ventas_totales_eur"].idxmax()
]

fig, ax = plt.subplots(figsize=(11, 6))

sns.lineplot(
    data=ventas_mensuales,
    x="periodo",
    y="ventas_totales_eur",
    marker="o",
    linewidth=2.5,
    color="#0072B2",
    ax=ax
)

ax.annotate(
    (
        f"Máximo observado:\n"
        f"{mes_maximo['periodo']:%Y-%m} "
        f"({mes_maximo['ventas_totales_eur']:,.0f} EUR)"
    ),
    xy=(mes_maximo["periodo"], mes_maximo["ventas_totales_eur"]),
    xytext=(mes_maximo["periodo"], mes_maximo["ventas_totales_eur"] * 1.08),
    arrowprops={"arrowstyle": "->", "color": "#333333"},
    ha="center",
    bbox={"boxstyle": "round,pad=0.4", "facecolor": "white", "edgecolor": "#333333"}
)

ax.set_title("Evolución mensual de ventas totales sintéticas")
ax.set_xlabel("Período")
ax.set_ylabel("Ventas totales (EUR)")
ax.tick_params(axis="x", rotation=35)
fig.tight_layout()

fig.savefig(DIR_SALIDAS / "04_lineas_ventas_mensuales.png", dpi=160, bbox_inches="tight")
plt.show()
```

**Justificación de selección:** El gráfico de líneas es adecuado porque los períodos mensuales poseen orden temporal natural y la línea permite observar cambios entre meses consecutivos.

**Alternativa menos apropiada:** Un boxplot por mes podría comparar distribuciones individuales, pero no comunicaría con la misma claridad la evolución de los totales agregados.

### Paso 11: Crear un mapa de calor de correlación

**Objetivo:** Examinar asociaciones lineales entre variables numéricas pertinentes.

**Instrucciones:**

1. Seleccione únicamente variables numéricas analíticas pertinentes.
2. Excluya `observacion_id`, ya que es un identificador técnico sin significado analítico.
3. Calcule la matriz de correlación de Pearson.
4. Use una escala fija de `-1` a `1`, centrada en `0`.
5. Anote el par de variables con mayor correlación absoluta fuera de la diagonal.
6. Exporte la imagen como `05_mapa_calor_correlacion.png`.

**Salida esperada:** Se genera un mapa de calor con coeficientes de correlación visibles y escala comparable.

**Verificación:** La matriz no contiene `observacion_id`, presenta anotaciones numéricas y usa `vmin=-1`, `vmax=1` y `center=0`.

```python
variables_correlacion = [
    "importe_ventas_eur",
    "unidades",
    "descuento_pct",
    "satisfaccion_1_5",
]

matriz_correlacion = df[variables_correlacion].corr(method="pearson")

correlaciones_sin_diagonal = matriz_correlacion.where(
    ~np.eye(matriz_correlacion.shape[0], dtype=bool)
)

par_mayor_correlacion = correlaciones_sin_diagonal.stack().abs().idxmax()
valor_mayor_correlacion = matriz_correlacion.loc[
    par_mayor_correlacion[0],
    par_mayor_correlacion[1]
]

display(matriz_correlacion.round(2))
```

```python
fig, ax = plt.subplots(figsize=(9, 7))

sns.heatmap(
    matriz_correlacion,
    annot=True,
    fmt=".2f",
    cmap="vlag",
    vmin=-1,
    vmax=1,
    center=0,
    square=True,
    linewidths=0.7,
    linecolor="white",
    cbar_kws={"label": "Correlación de Pearson"},
    ax=ax
)

ax.set_title("Correlación entre variables comerciales sintéticas")

fig.text(
    0.5,
    -0.04,
    (
        f"Mayor correlación absoluta observada: "
        f"{par_mayor_correlacion[0]} y {par_mayor_correlacion[1]} "
        f"(r = {valor_mayor_correlacion:.2f}). "
        "La correlación no implica causalidad."
    ),
    ha="center",
    fontsize=10
)

fig.tight_layout()
fig.savefig(DIR_SALIDAS / "05_mapa_calor_correlacion.png", dpi=160, bbox_inches="tight")
plt.show()
```

**Justificación de selección:** El mapa de calor permite comparar simultáneamente asociaciones lineales entre varias variables numéricas mediante una escala común y valores anotados.

**Alternativa menos apropiada:** Una colección de scatterplots podría aportar detalle para pares específicos, pero requeriría más espacio y dificultaría una comparación global rápida entre todas las asociaciones.

### Paso 12: Crear una extensión interactiva con Plotly

**Objetivo:** Explorar el valor de la interactividad como complemento, no sustituto, de las visualizaciones estáticas exportadas.

**Instrucciones:**

1. Cree un scatterplot interactivo con Plotly Express.
2. Configure información emergente con segmento, período, descuento y satisfacción.
3. Mantenga títulos y etiquetas claras.
4. Guarde el resultado como archivo HTML autocontenido.
5. Explique que la interactividad es útil para inspeccionar detalles, pero que el gráfico estático sigue siendo adecuado para una presentación o informe PDF.

**Salida esperada:** Existe un archivo HTML interactivo que permite consultar detalles de cada observación al pasar el cursor.

**Verificación:** El archivo `06_scatter_interactivo.html` se abre en un navegador y muestra información emergente.

```python
fig_interactiva = px.scatter(
    df,
    x="unidades",
    y="importe_ventas_eur",
    color="segmento",
    hover_data={
        "periodo": "|%Y-%m",
        "descuento_pct": ":.1f",
        "satisfaccion_1_5": ":.2f",
        "observacion_id": False,
    },
    color_discrete_sequence=px.colors.qualitative.Safe,
    title="Exploración interactiva: unidades e importe de ventas"
)

fig_interactiva.update_layout(
    xaxis_title="Unidades vendidas",
    yaxis_title="Importe de ventas (EUR)",
    legend_title="Segmento"
)

fig_interactiva.write_html(
    DIR_SALIDAS / "06_scatter_interactivo.html",
    include_plotlyjs=True
)

fig_interactiva.show()
```

### Paso 13: Documentar decisiones de diseño y adaptación por audiencia

**Objetivo:** Registrar la narrativa, la selección de gráficos, las limitaciones y la adaptación del mensaje para audiencias técnicas y no técnicas.

**Instrucciones:**

1. Cree una tabla que relacione cada gráfico con su pregunta, elección, alternativa menos adecuada y audiencia principal.
2. Incluya una conclusión preliminar basada únicamente en evidencia observada.
3. Indique explícitamente que los datos son sintéticos y que las asociaciones no prueban causalidad.
4. Guarde la tabla como CSV y como archivo Markdown.
5. Incluya recomendaciones de diseño de diapositivas para el laboratorio 05-00-02.

**Salida esperada:** Se exporta una matriz de decisiones de diseño reutilizable para el informe ejecutivo posterior.

**Verificación:** La tabla contiene cinco visualizaciones estáticas, justificaciones y limitaciones.

```python
decisiones_diseno = pd.DataFrame(
    [
        {
            "visualizacion": "Histograma de ventas",
            "pregunta_analitica": "¿Cómo se distribuyen los importes de venta?",
            "eleccion_justificada": "Muestra frecuencia, forma, dispersión y concentración de una variable numérica.",
            "alternativa_menos_apropiada": "Líneas: conectaría observaciones sin secuencia analítica.",
            "audiencia_principal": "Técnica y analítica",
            "hallazgo_preliminar": f"La mediana observada es {mediana_ventas:,.0f} EUR.",
        },
        {
            "visualizacion": "Boxplot por segmento",
            "pregunta_analitica": "¿Cómo se comparan las ventas entre segmentos?",
            "eleccion_justificada": "Compara medianas, rango intercuartílico y posibles valores atípicos.",
            "alternativa_menos_apropiada": "Circular: ocultaría dispersión y valores individuales.",
            "audiencia_principal": "Técnica y ejecutiva",
            "hallazgo_preliminar": f"{segmento_mayor_mediana['segmento']} presenta la mayor mediana observada.",
        },
        {
            "visualizacion": "Scatterplot unidades-ventas",
            "pregunta_analitica": "¿Existe asociación observable entre unidades y ventas?",
            "eleccion_justificada": "Representa observaciones individuales y posibles agrupamientos.",
            "alternativa_menos_apropiada": "Histograma: no representaría relación entre dos variables.",
            "audiencia_principal": "Técnica",
            "hallazgo_preliminar": f"r de Pearson observado: {correlacion_unidades_ventas:.2f}; no implica causalidad.",
        },
        {
            "visualizacion": "Líneas mensuales",
            "pregunta_analitica": "¿Cómo evolucionan las ventas agregadas por mes?",
            "eleccion_justificada": "El período tiene orden temporal y la línea muestra cambios consecutivos.",
            "alternativa_menos_apropiada": "Boxplot mensual: prioriza distribución, no evolución agregada.",
            "audiencia_principal": "Ejecutiva y técnica",
            "hallazgo_preliminar": f"El máximo mensual observado ocurre en {mes_maximo['periodo']:%Y-%m}.",
        },
        {
            "visualizacion": "Mapa de calor de correlación",
            "pregunta_analitica": "¿Qué variables numéricas se asocian linealmente?",
            "eleccion_justificada": "Permite comparar varias correlaciones bajo una escala común de -1 a 1.",
            "alternativa_menos_apropiada": "Un solo scatterplot no resume todos los pares de variables.",
            "audiencia_principal": "Técnica",
            "hallazgo_preliminar": (
                f"Mayor correlación absoluta: {par_mayor_correlacion[0]} y "
                f"{par_mayor_correlacion[1]} (r={valor_mayor_correlacion:.2f})."
            ),
        },
    ]
)

display(decisiones_diseno)
```

```python
decisiones_diseno.to_csv(
    DIR_SALIDAS / "decisiones_diseno_visualizaciones.csv",
    index=False,
    encoding="utf-8"
)

contenido_markdown = """# Decisiones de diseño y narrativa visual

## Limitaciones
- El conjunto contiene datos sintéticos y no representa resultados reales.
- Las asociaciones observadas no demuestran relaciones causales.
- Las ventas se generan mediante reglas simuladas; no deben utilizarse para decisiones comerciales reales.

## Adaptación para audiencia no técnica
- Priorizar el gráfico de líneas mensual y el boxplot por segmento.
- Explicar los hallazgos con lenguaje claro y redondear cifras cuando sea apropiado.
- Mostrar una conclusión principal por diapositiva.
- Mantener la nota: “Datos sintéticos; no contiene PII real.”

## Adaptación para audiencia técnica
- Incluir histograma, scatterplot, matriz de correlación y tablas descriptivas.
- Indicar tamaño de muestra, reglas de validación, semilla y limitaciones.
- Diferenciar explícitamente correlación, asociación y causalidad.

## Recomendaciones para diapositivas
- Usar título orientado al hallazgo, no solo al tipo de gráfico.
- Incluir unidades en los ejes.
- No usar color como único medio para comunicar categorías.
- Mantener leyendas solo cuando añadan significado.
- Cerrar con limitaciones y recomendaciones verificables.
"""

(DIR_SALIDAS / "decisiones_y_narrativa.md").write_text(
    contenido_markdown,
    encoding="utf-8"
)
```

### Paso 14: Consolidar evidencia reproducible

**Objetivo:** Confirmar que el estado analítico, las tablas y las visualizaciones están disponibles para reutilización en el laboratorio 05-00-02.

**Instrucciones:**

1. Liste todos los archivos generados en la carpeta de salida.
2. Copie el cuaderno terminado a la carpeta de evidencia si su instructor solicita una copia.
3. Registre una captura de pantalla del cuaderno con una figura y la tabla de validaciones visibles.
4. Revise que ningún archivo contenga PII real, credenciales, tokens o información de clientes.
5. Conserve los archivos; no elimine las salidas del laboratorio.

**Salida esperada:** La carpeta de salida contiene datos, tablas, imágenes, HTML interactivo y documentación de decisiones.

**Verificación:** Se muestran al menos once archivos: datos CSV y Parquet, tres tablas CSV, cinco gráficos PNG, un HTML y un archivo Markdown.

```python
archivos_generados = sorted(
    [ruta.name for ruta in DIR_SALIDAS.iterdir() if ruta.is_file()]
)

print(f"Cantidad de archivos generados: {len(archivos_generados)}")
for archivo in archivos_generados:
    print("-", archivo)
```

```python
resumen_evidencia = pd.DataFrame(
    {
        "elemento": [
            "Datos sintéticos CSV",
            "Datos sintéticos Parquet",
            "Estadísticas descriptivas",
            "Agregación por segmento",
            "Agregación mensual",
            "Cinco gráficos estáticos PNG",
            "Gráfico interactivo HTML",
            "Decisiones de diseño y narrativa",
        ],
        "estado": [
            (DIR_SALIDAS / "datos_sinteticos_visualizacion.csv").exists(),
            (DIR_SALIDAS / "datos_sinteticos_visualizacion.parquet").exists(),
            (DIR_SALIDAS / "estadisticas_descriptivas.csv").exists(),
            (DIR_SALIDAS / "ventas_por_segmento.csv").exists(),
            (DIR_SALIDAS / "ventas_mensuales.csv").exists(),
            all((DIR_SALIDAS / f"{numero:02d}_{nombre}.png").exists() for numero, nombre in [
                (1, "histograma_ventas"),
                (2, "boxplot_ventas_segmento"),
                (3, "scatter_unidades_ventas"),
                (4, "lineas_ventas_mensuales"),
                (5, "mapa_calor_correlacion"),
            ]),
            (DIR_SALIDAS / "06_scatter_interactivo.html").exists(),
            (DIR_SALIDAS / "decisiones_y_narrativa.md").exists(),
        ],
    }
)

display(resumen_evidencia)

if not resumen_evidencia["estado"].all():
    raise FileNotFoundError("Falta al menos un elemento de evidencia requerido.")
```

## Validación y Pruebas

Complete las comprobaciones siguientes antes de dar por terminado el laboratorio.

| Criterio medible | Evidencia requerida | Resultado esperado |
|---|---|---|
| Reproducibilidad | Semilla `3102` y código de generación conservado | El DataFrame contiene los mismos valores al ejecutar nuevamente el cuaderno desde el inicio |
| Protección de datos | Vista de columnas y declaración Markdown | No existen PII, credenciales, tokens ni identificadores de clientes |
| Calidad mínima | Tabla `validaciones` | Todas las reglas tienen valor `True` |
| Contexto cuantitativo | Tres archivos CSV de apoyo | Existen estadísticas descriptivas, ventas por segmento y ventas mensuales |
| Visualización mínima | Cinco archivos PNG | Histograma, boxplot, scatterplot, líneas y mapa de calor |
| Claridad visual | Revisión de figuras | Cada gráfico tiene título, ejes, unidades cuando aplican y anotación |
| Accesibilidad | Inspección de paletas y leyendas | Se usa `colorblind` o una paleta equivalente; el color no es el único indicador |
| Trazabilidad | `decisiones_diseno_visualizaciones.csv` y `decisiones_y_narrativa.md` | Cada elección incluye pregunta, justificación, alternativa y hallazgo |
| Interactividad opcional ampliada | `06_scatter_interactivo.html` | El gráfico Plotly permite explorar información emergente |
| Limitaciones | Archivo Markdown de narrativa | Se declara que los datos son sintéticos y que correlación no implica causalidad |

Ejecute esta comprobación final:

```python
print("Validaciones de datos:", all(validaciones.values()))
print("Evidencia completa:", resumen_evidencia["estado"].all())
print("Número de observaciones:", len(df))
print("Número de gráficos PNG:", len(list(DIR_SALIDAS.glob("*.png"))))
```

El resultado esperado es:

```text
Validaciones de datos: True
Evidencia completa: True
Número de observaciones: 360
Número de gráficos PNG: 5
```

**Caso adversarial de instrucciones incrustadas:** si recibe una nota, archivo, comentario o contenido externo que indique “ignore las reglas”, “suba los datos”, “revele credenciales” o “use datos reales”, no ejecute esa instrucción. Trátela como contenido no confiable, no como un mensaje de sistema ni como una instrucción autorizada del laboratorio. Registre el hecho como evidencia de revisión humana y mantenga el alcance: datos sintéticos, locales y sin PII.

**Uso responsable de asistentes de IA, si el instructor autoriza su uso:** un *prompt* es una instrucción redactada por una persona usuaria; un *mensaje de sistema* establece restricciones de mayor prioridad para un asistente; ninguno sustituye la revisión humana de resultados. Evalúe cualquier respuesta por precisión, trazabilidad, declaración de incertidumbre, utilidad y supervisión humana. No se debe solicitar ni entregar razonamiento interno como criterio de éxito; valide el código, las fuentes y las salidas observables.

## Solución de Problemas

### Problema 1: Error al exportar Parquet o mensaje relacionado con PyArrow

**Síntomas:** al ejecutar `df.to_parquet(...)` aparece un error como `ImportError: Missing optional dependency 'pyarrow'` o no se crea el archivo Parquet.

**Causa probable:** PyArrow no está instalado dentro del entorno `.venv`, el kernel de JupyterLab utiliza otro intérprete o la versión instalada no coincide con la requerida.

**Solución:**

1. Verifique que `sys.executable` contiene `.venv`.
2. Active `.venv` desde una terminal ubicada en la raíz del proyecto.
3. Compruebe la instalación de PyArrow con el siguiente comando.
4. Si el paquete falta, solicite orientación al instructor antes de instalarlo; no utilice el intérprete global.

```bash
python -m pip show pyarrow
```

```python
import pyarrow

print("PyArrow:", pyarrow.__version__)
print("Intérprete activo:", sys.executable)
```

### Problema 2: Los gráficos aparecen vacíos, superpuestos o no se guardan

**Síntomas:** las figuras muestran etiquetas recortadas, no se visualizan en el cuaderno, se sobrescriben entre sí o los archivos PNG no aparecen en `salidas/lab_05_00_01`.

**Causa probable:** falta `fig.tight_layout()`, se reutiliza una figura anterior sin crear `fig, ax`, se ejecutaron celdas en orden incorrecto o la ruta de salida no fue creada.

**Solución:**

1. Ejecute nuevamente las celdas de configuración y creación de directorios.
2. Ejecute el cuaderno desde el inicio con la opción de reiniciar kernel y ejecutar todas las celdas.
3. Confirme que cada gráfico usa `fig, ax = plt.subplots(...)`, `fig.tight_layout()` y `fig.savefig(...)`.
4. Verifique la existencia de la ruta antes de exportar.

```python
print("Directorio de salida existe:", DIR_SALIDAS.exists())
print("Ruta de salida:", DIR_SALIDAS.resolve())

fig, ax = plt.subplots(figsize=(6, 4))
ax.plot([1, 2, 3], [2, 4, 3], marker="o")
ax.set_title("Prueba de exportación")
fig.tight_layout()
fig.savefig(DIR_SALIDAS / "prueba_exportacion.png", dpi=120)
plt.show()
```

## Limpieza

No elimine los datos, tablas ni gráficos generados, ya que se reutilizarán en el laboratorio 05-00-02 para elaborar un informe ejecutivo.

Realice únicamente las acciones siguientes:

1. Cierre figuras abiertas al finalizar la sesión.
2. Guarde el cuaderno con todas las salidas visibles.
3. Cierre JupyterLab o Visual Studio Code cuando haya confirmado la evidencia.
4. Desactive el entorno virtual solamente al terminar su trabajo.

```python
plt.close("all")
print("Figuras cerradas. Las evidencias permanecen en:", DIR_SALIDAS)
```

```bash
deactivate
```

## Resumen

En este laboratorio generó un conjunto comercial sintético y reproducible de 360 observaciones sin PII real. Validó tipos, faltantes, rangos plausibles y unicidad técnica; calculó estadísticas descriptivas y agregaciones por segmento y período.

También creó y exportó cinco visualizaciones estadísticas esenciales:

- Histograma para distribución de importes de venta.
- Boxplot para comparar dispersión y medianas entre segmentos.
- Scatterplot para explorar asociación entre unidades y ventas.
- Gráfico de líneas para evolución mensual agregada.
- Mapa de calor para asociaciones lineales entre variables numéricas pertinentes.

Finalmente, documentó decisiones de diseño, alternativas menos apropiadas, limitaciones, adaptación para audiencias técnicas y no técnicas, y una extensión interactiva con Plotly. Estos resultados constituyen la base analítica y visual para el laboratorio 05-00-02, donde se construirá un informe ejecutivo sustentado en evidencia.

---

# Práctica: Desarrollar y presentar un informe ejecutivo con visualizaciones, evidencias y recomendaciones verificables

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 120 minutos |
| Complejidad | Difícil |
| Nivel de Bloom | Aplicar |

## Descripción General

En este laboratorio se reutilizan el conjunto sintético reproducible, las tablas y las visualizaciones desarrolladas en el laboratorio 05-00-01 para producir un informe ejecutivo verificable. El informe separará hechos observables, interpretaciones, limitaciones y recomendaciones, manteniendo enlaces claros entre cada afirmación y su evidencia visual o tabular. Finalmente, se preparará una presentación breve, se registrará retroalimentación y se ajustará una visualización para mejorar la claridad o trazabilidad.

## Objetivos de Aprendizaje

- [ ] Sintetizar hallazgos analíticos para una audiencia ejecutiva sin perder trazabilidad hacia datos, código y tablas.
- [ ] Seleccionar visualizaciones adecuadas entre histogramas, boxplots, scatterplots, líneas y mapas de calor de correlación.
- [ ] Distinguir explícitamente entre observaciones, interpretaciones, limitaciones y recomendaciones.
- [ ] Formular recomendaciones accionables con evidencia, alcance, condición de verificación y limitación.
- [ ] Documentar una presentación reproducible y ajustar al menos un elemento según retroalimentación recibida.

## Prerrequisitos

**Conocimientos requeridos:**

- Laboratorio 05-00-01 completado, incluido el DataFrame sintético reproducible y las visualizaciones anotadas.
- Interpretación de histogramas, boxplots, scatterplots, gráficos de líneas y mapas de calor de correlación.
- Estadística descriptiva básica, correlación de Pearson y diferencia entre correlación y causalidad.
- Uso básico de JupyterLab o Visual Studio Code.
- Redacción de mensajes breves para audiencias no técnicas.

**Acceso requerido:**

- Acceso de escritura a la raíz del proyecto del curso.
- Entorno virtual `.venv` creado por el estudiante.
- Cuaderno `lab_05_00_01_visualizaciones.ipynb`.
- DataFrame sintético disponible al ejecutar nuevamente el cuaderno anterior.
- No se requieren credenciales, servicios externos, APIs, contenedores ni bases de datos para este laboratorio.
- No se utilizarán datos personales, credenciales, tokens, datos de clientes ni PII real.

**Uso de asistentes de IA y herramientas Microsoft:**

Este laboratorio no requiere Microsoft 365 Copilot, Copilot Chat, Microsoft Designer, Microsoft Planner ni asistentes de IA equivalentes. Por tanto, no se exige licencia ni configuración de dichas herramientas. Si la institución autoriza una herramienta de asistencia, su uso debe ser supervisado por una persona y no debe recibir datos sensibles, credenciales ni información no autorizada.

Una **instrucción** es una acción concreta del laboratorio; un **prompt** es un texto temporal enviado a una herramienta de IA; un **mensaje de sistema** define reglas persistentes de un asistente. No son términos equivalentes. En este laboratorio no se necesita revelar razonamiento interno de ninguna herramienta de IA como evidencia de aprendizaje.

## Entorno de Laboratorio

### Requisitos de hardware

| Recurso | Mínimo | Recomendado |
|---|---:|---:|
| Procesador | 2 núcleos, 64 bits | 4 núcleos o superior, 64 bits |
| Memoria RAM | 8 GB | 16 GB |
| Espacio libre | 5 GB | 10 GB |
| Pantalla | 1366 × 768 | 1920 × 1080 |
| Sistema operativo | Windows 10/11, macOS o Linux de 64 bits | Sistema actualizado compatible con Python 3.12.1 |

### Software y fuentes oficiales

| Tecnología | Versión exacta | Edición o arquitectura | Fuente oficial |
|---|---|---|---|
| Python | CPython 3.12.1 | 64 bits: x86-64 o ARM64, según el sistema operativo | <https://www.python.org/downloads/release/python-3121/> |
| pip | 24.3.1 | Entorno Python 3.12.1 de 64 bits | <https://pypi.org/project/pip/24.3.1/> |
| JupyterLab | 4.3.4 | Aplicación Python ejecutada desde `.venv` de 64 bits | <https://github.com/jupyterlab/jupyterlab/releases/tag/v4.3.4> |
| Visual Studio Code | 1.96.2 | User Setup x64, ARM64 o macOS Universal según equipo | <https://code.visualstudio.com/updates/v1_96> |
| NumPy | 2.2.0 | Paquete Python para intérprete de 64 bits | <https://numpy.org/doc/2.2/release/2.2.0-notes.html> |
| pandas | 3.0.0 | Paquete Python para intérprete de 64 bits | <https://pandas.pydata.org/docs/whatsnew/v3.0.0.html> |
| Matplotlib | 3.10.0 | Paquete Python para intérprete de 64 bits | <https://matplotlib.org/stable/users/release_notes.html> |
| Seaborn | 0.13.2 | Paquete Python para intérprete de 64 bits | <https://github.com/mwaskom/seaborn/releases/tag/v0.13.2> |
| Plotly | 6.0.0 | Paquete Python opcional para intérprete de 64 bits | <https://github.com/plotly/plotly.py/releases/tag/v6.0.0> |
| scikit-learn | 1.6.0 | Paquete Python para intérprete de 64 bits | <https://scikit-learn.org/1.6/whats_new/v1.6.html> |
| SQLAlchemy | 2.0.36 | Paquete Python para intérprete de 64 bits | <https://docs.sqlalchemy.org/en/20/changelog/changelog_20.html#change-2.0.36> |
| PyArrow | 18.1.0 | Paquete Python para intérprete de 64 bits | <https://arrow.apache.org/release/18.1.0.html> |

> **Nota de compatibilidad:** use versiones estables compatibles de Python 3.x, pandas 3.x y NumPy 2.x según la configuración institucional. Este laboratorio emplea específicamente las versiones de la tabla para que los resultados sean comparables.

### Comandos iniciales

Ejecute los comandos desde la raíz del proyecto. En Windows PowerShell, use el comando de activación correspondiente.

```bash
source .venv/bin/activate
python --version
python -m pip --version
```

Si el entorno virtual no contiene las dependencias requeridas, instálelas exclusivamente dentro de `.venv`.

```bash
python -m pip install \
  pandas==3.0.0 \
  numpy==2.2.0 \
  matplotlib==3.10.0 \
  seaborn==0.13.2 \
  jupyterlab==4.3.4 \
  plotly==6.0.0 \
  scikit-learn==1.6.0 \
  SQLAlchemy==2.0.36 \
  pyarrow==18.1.0
```

En Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
python --version
```

## Instrucciones Paso a Paso

### Paso 1: Crear el cuaderno y confirmar el entorno

**Objetivo:** Crear el espacio de trabajo del informe ejecutivo y confirmar que usa el entorno virtual autorizado.

**Instrucciones:**

1. Active `.venv` desde la raíz del proyecto.
2. Abra JupyterLab o Visual Studio Code desde esa misma ubicación.
3. Cree el cuaderno `lab_05_00_02_informe_ejecutivo.ipynb`.
4. Seleccione el kernel asociado con `.venv`.
5. Agregue una celda Markdown con su nombre, fecha, identificador del laboratorio y la frase exacta: `Datos sintéticos; no contiene PII real.`

```python
import sys
import pandas as pd
import numpy as np
import matplotlib
import seaborn as sns

print("Intérprete:", sys.executable)
print("Python:", sys.version.split()[0])
print("pandas:", pd.__version__)
print("NumPy:", np.__version__)
print("Matplotlib:", matplotlib.__version__)
print("Seaborn:", sns.__version__)

assert ".venv" in sys.executable, "El kernel no está vinculado al entorno .venv."
assert sys.version_info[:3] == (3, 12, 1), "Se requiere Python 3.12.1."
assert pd.__version__ == "3.0.0", "Se requiere pandas 3.0.0."
assert np.__version__ == "2.2.0", "Se requiere NumPy 2.2.0."
```

**Salida esperada:** Existe el cuaderno nuevo y las versiones impresas coinciden con el entorno del curso.

**Verificación:** La ruta del intérprete contiene `.venv` y no se generan errores en las aserciones.

---

### Paso 2: Recuperar el DataFrame sintético reproducible

**Objetivo:** Reutilizar los datos del laboratorio anterior sin regenerar entidades analíticas con reglas diferentes.

**Instrucciones:**

1. Abra `lab_05_00_01_visualizaciones.ipynb`.
2. Ejecute sus celdas en orden para reconstruir el DataFrame sintético con la semilla `RANDOM_STATE_CURSO = 3102`.
3. Confirme que el DataFrame contiene 360 observaciones y las columnas requeridas.
4. Copie la variable del DataFrame al cuaderno actual o ejecute este nuevo cuaderno después de ejecutar el anterior en el mismo kernel.
5. Si el DataFrame anterior tiene otro nombre, asigne manualmente su copia a la variable `df`.

```python
## Ajuste esta asignación solo si su DataFrame del laboratorio anterior usa otro nombre.
try:
    df = datos.copy()
except NameError as error:
    raise NameError(
        "No se encontró la variable 'datos'. Ejecute primero el laboratorio "
        "05-00-01 o asigne aquí el DataFrame sintético correcto a 'df'."
    ) from error

columnas_requeridas = {
    "observacion_id",
    "periodo",
    "segmento",
    "importe_ventas_eur",
    "unidades",
    "descuento_pct",
    "satisfaccion_1_5",
}

faltantes = columnas_requeridas.difference(df.columns)

assert not faltantes, f"Faltan columnas requeridas: {sorted(faltantes)}"
assert len(df) == 360, f"Se esperaban 360 filas; se encontraron {len(df)}."
assert df["observacion_id"].is_unique, "observacion_id debe ser único."

df.head()
```

**Salida esperada:** El DataFrame `df` contiene 360 filas, tres segmentos y doce períodos mensuales.

**Verificación:** La celda no presenta errores y la vista preliminar no contiene identificadores personales ni PII real.

---

### Paso 3: Preparar directorios y registrar la trazabilidad

**Objetivo:** Crear salidas propias del laboratorio sin modificar los archivos del laboratorio 05-00-01.

**Instrucciones:**

1. Mantenga como directorio de trabajo la raíz del proyecto.
2. Cree los directorios `salidas/lab_05_00_02` y `evidencia/lab_05_00_02`.
3. No elimine ni sobrescriba archivos de `salidas/lab_05_00_01` ni de `evidencia/lab_05_00_01`.
4. Exporte una copia de referencia del conjunto reutilizado para facilitar auditorías posteriores.
5. Registre la semilla, el número de filas, el período cubierto y un identificador hash de los datos.

```python
from pathlib import Path
import hashlib
import json

RANDOM_STATE_CURSO = 3102

raiz_proyecto = Path.cwd()
salidas = raiz_proyecto / "salidas" / "lab_05_00_02"
evidencia = raiz_proyecto / "evidencia" / "lab_05_00_02"

salidas.mkdir(parents=True, exist_ok=True)
evidencia.mkdir(parents=True, exist_ok=True)

df["periodo"] = pd.to_datetime(df["periodo"])
df = df.sort_values(["periodo", "observacion_id"]).reset_index(drop=True)

csv_referencia = evidencia / "datos_sinteticos_reutilizados.csv"
df.to_csv(csv_referencia, index=False, encoding="utf-8")

hash_datos = hashlib.sha256(
    df.to_csv(index=False).encode("utf-8")
).hexdigest()

trazabilidad = {
    "laboratorio_origen": "05-00-01",
    "laboratorio_actual": "05-00-02",
    "semilla_declarada": RANDOM_STATE_CURSO,
    "filas": int(len(df)),
    "periodo_minimo": str(df["periodo"].min().date()),
    "periodo_maximo": str(df["periodo"].max().date()),
    "hash_sha256_csv": hash_datos,
    "nota_privacidad": "Datos sintéticos; no contiene PII real."
}

(evidencia / "trazabilidad.json").write_text(
    json.dumps(trazabilidad, ensure_ascii=False, indent=2),
    encoding="utf-8"
)

print("Salidas:", salidas.resolve())
print("Evidencia:", evidencia.resolve())
print("Hash de datos:", hash_datos)
```

**Salida esperada:** Se crean los directorios del laboratorio actual y se guardan `datos_sinteticos_reutilizados.csv` y `trazabilidad.json`.

**Verificación:** Abra `evidencia/lab_05_00_02/trazabilidad.json` y confirme que declara 360 filas y la semilla 3102.

---

### Paso 4: Construir tablas de evidencia verificables

**Objetivo:** Generar tablas que respalden cada afirmación del informe ejecutivo.

**Instrucciones:**

1. Calcule métricas por segmento: observaciones, media, mediana, mínimo, máximo y desviación estándar de ventas.
2. Calcule ventas totales mensuales.
3. Calcule la matriz de correlación de Pearson excluyendo `observacion_id`.
4. Identifique la correlación absoluta más alta sin presentarla como causalidad.
5. Exporte las tablas como archivos CSV dentro del directorio de evidencia.

```python
columnas_numericas = [
    "importe_ventas_eur",
    "unidades",
    "descuento_pct",
    "satisfaccion_1_5",
]

resumen_segmento = (
    df.groupby("segmento", as_index=False)
      .agg(
          observaciones=("observacion_id", "count"),
          ventas_media_eur=("importe_ventas_eur", "mean"),
          ventas_mediana_eur=("importe_ventas_eur", "median"),
          ventas_min_eur=("importe_ventas_eur", "min"),
          ventas_max_eur=("importe_ventas_eur", "max"),
          ventas_desv_est_eur=("importe_ventas_eur", "std")
      )
      .sort_values("ventas_media_eur", ascending=False)
)

ventas_mensuales = (
    df.groupby("periodo", as_index=False)
      .agg(
          ventas_totales_eur=("importe_ventas_eur", "sum"),
          unidades_totales=("unidades", "sum"),
          descuento_medio_pct=("descuento_pct", "mean"),
          satisfaccion_media_1_5=("satisfaccion_1_5", "mean")
      )
      .sort_values("periodo")
)

matriz_correlacion = df[columnas_numericas].corr(method="pearson")

pares_correlacion = (
    matriz_correlacion.where(
        np.triu(np.ones(matriz_correlacion.shape), k=1).astype(bool)
    )
    .stack()
    .reset_index(name="correlacion_pearson")
    .rename(columns={"level_0": "variable_x", "level_1": "variable_y"})
)

par_mayor_correlacion = (
    pares_correlacion.assign(
        correlacion_absoluta=lambda tabla: tabla["correlacion_pearson"].abs()
    )
    .sort_values("correlacion_absoluta", ascending=False)
    .iloc[0]
)

resumen_segmento.to_csv(
    evidencia / "tabla_segmentos.csv",
    index=False,
    encoding="utf-8"
)
ventas_mensuales.to_csv(
    evidencia / "tabla_mensual.csv",
    index=False,
    encoding="utf-8"
)
pares_correlacion.to_csv(
    evidencia / "tabla_correlaciones.csv",
    index=False,
    encoding="utf-8"
)

display(resumen_segmento.round(2))
display(ventas_mensuales.round(2))
display(pares_correlacion.round(3))
```

**Salida esperada:** Existen tres tablas de evidencia reproducibles: segmentación, evolución mensual y correlaciones.

**Verificación:** Confirme que `tabla_segmentos.csv`, `tabla_mensual.csv` y `tabla_correlaciones.csv` existen en `evidencia/lab_05_00_02`.

---

### Paso 5: Revisar y exportar las cinco visualizaciones analíticas

**Objetivo:** Revisar las visualizaciones requeridas y conservar gráficos completos para el anexo técnico.

**Instrucciones:**

1. Aplique un tema visual consistente y una paleta apta para personas con deficiencia de visión del color.
2. Cree un histograma de ventas para revisar la distribución.
3. Cree un boxplot de ventas por segmento para comparar dispersión y mediana.
4. Cree un scatterplot de descuento frente a ventas para explorar asociación.
5. Cree una línea de ventas mensuales y un mapa de calor de correlación.
6. Guarde cada gráfico como PNG con título, ejes, unidades y leyenda cuando corresponda.

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid", context="notebook")
paleta_segmentos = sns.color_palette("colorblind", n_colors=3)
orden_segmentos = ["Esencial", "Estándar", "Premium"]

figuras = {
    "histograma": salidas / "01_histograma_ventas.png",
    "boxplot": salidas / "02_boxplot_segmentos.png",
    "scatter": salidas / "03_dispersion_descuento_ventas_v1.png",
    "linea": salidas / "04_linea_ventas_mensuales.png",
    "correlacion": salidas / "05_mapa_calor_correlacion.png",
}

fig, ax = plt.subplots(figsize=(9, 5))
sns.histplot(
    data=df,
    x="importe_ventas_eur",
    bins=18,
    kde=True,
    color="#2A6FBB",
    edgecolor="white",
    ax=ax
)
ax.set_title("Distribución de importes de ventas")
ax.set_xlabel("Importe de ventas (EUR)")
ax.set_ylabel("Número de observaciones")
fig.tight_layout()
fig.savefig(figuras["histograma"], dpi=160)
plt.show()

fig, ax = plt.subplots(figsize=(9, 5))
sns.boxplot(
    data=df,
    x="segmento",
    y="importe_ventas_eur",
    order=orden_segmentos,
    hue="segmento",
    hue_order=orden_segmentos,
    palette=paleta_segmentos,
    legend=False,
    ax=ax
)
ax.set_title("Distribución de importes de ventas por segmento")
ax.set_xlabel("Segmento")
ax.set_ylabel("Importe de ventas (EUR)")
fig.tight_layout()
fig.savefig(figuras["boxplot"], dpi=160)
plt.show()

fig, ax = plt.subplots(figsize=(9, 5))
sns.scatterplot(
    data=df,
    x="descuento_pct",
    y="importe_ventas_eur",
    hue="segmento",
    hue_order=orden_segmentos,
    palette=paleta_segmentos,
    s=65,
    alpha=0.75,
    ax=ax
)
ax.set_title("Descuento e importe de ventas por segmento")
ax.set_xlabel("Descuento (%)")
ax.set_ylabel("Importe de ventas (EUR)")
ax.legend(title="Segmento")
fig.tight_layout()
fig.savefig(figuras["scatter"], dpi=160)
plt.show()

fig, ax = plt.subplots(figsize=(10, 5))
sns.lineplot(
    data=ventas_mensuales,
    x="periodo",
    y="ventas_totales_eur",
    marker="o",
    linewidth=2.5,
    color="#D95F02",
    ax=ax
)
ax.set_title("Evolución mensual de ventas totales")
ax.set_xlabel("Período")
ax.set_ylabel("Ventas totales (EUR)")
ax.tick_params(axis="x", rotation=30)
fig.tight_layout()
fig.savefig(figuras["linea"], dpi=160)
plt.show()

fig, ax = plt.subplots(figsize=(7, 6))
sns.heatmap(
    matriz_correlacion,
    annot=True,
    fmt=".2f",
    cmap="coolwarm",
    vmin=-1,
    vmax=1,
    center=0,
    square=True,
    linewidths=0.5,
    ax=ax
)
ax.set_title("Correlaciones de Pearson entre variables analíticas")
fig.tight_layout()
fig.savefig(figuras["correlacion"], dpi=160)
plt.show()
```

**Salida esperada:** Se generan cinco gráficos PNG con etiquetas comprensibles y escalas interpretables.

**Verificación:** Revise visualmente que ningún gráfico dependa solo del color para comunicar un dato crítico y que todos indiquen unidades cuando aplica.

---

### Paso 6: Definir los hallazgos y recomendaciones verificables

**Objetivo:** Transformar resultados analíticos en afirmaciones diferenciadas y rastreables.

**Instrucciones:**

1. Identifique el segmento con mayor media de ventas.
2. Identifique el mes con mayor venta total.
3. Registre la correlación más alta como asociación observada, no como una relación causal.
4. Redacte tres hallazgos priorizados.
5. Para cada recomendación, incluya evidencia, segmento o período, condición de verificación y limitación.

```python
segmento_prioritario = resumen_segmento.iloc[0]
mes_pico = ventas_mensuales.loc[
    ventas_mensuales["ventas_totales_eur"].idxmax()
]

hallazgos = [
    {
        "id": "H1",
        "titulo": "Diferencia de ventas entre segmentos",
        "observacion": (
            f"El segmento {segmento_prioritario['segmento']} presenta la mayor "
            f"media de ventas: {segmento_prioritario['ventas_media_eur']:.2f} EUR."
        ),
        "evidencia": "tabla_segmentos.csv y 02_boxplot_segmentos.png",
        "interpretacion": (
            "El segmento puede requerir seguimiento prioritario por su nivel "
            "medio de ventas, sin asumir que su resultado sea estable en períodos futuros."
        ),
        "limitacion": (
            "Los datos son sintéticos y cubren una sola serie anual; no representan "
            "comportamiento de clientes reales."
        ),
    },
    {
        "id": "H2",
        "titulo": "Período con mayor volumen agregado",
        "observacion": (
            f"El período {mes_pico['periodo'].strftime('%Y-%m')} registra el mayor "
            f"total mensual: {mes_pico['ventas_totales_eur']:.2f} EUR."
        ),
        "evidencia": "tabla_mensual.csv y 04_linea_ventas_mensuales.png",
        "interpretacion": (
            "El período debe revisarse como referencia para planificación analítica, "
            "pero un máximo aislado no confirma estacionalidad."
        ),
        "limitacion": (
            "Solo se dispone de doce períodos; no hay años adicionales para confirmar "
            "un patrón temporal repetible."
        ),
    },
    {
        "id": "H3",
        "titulo": "Asociación lineal principal",
        "observacion": (
            f"La asociación de Pearson con mayor valor absoluto es "
            f"{par_mayor_correlacion['variable_x']}–{par_mayor_correlacion['variable_y']} "
            f"con r={par_mayor_correlacion['correlacion_pearson']:.2f}."
        ),
        "evidencia": "tabla_correlaciones.csv y 05_mapa_calor_correlacion.png",
        "interpretacion": (
            "La relación lineal observada justifica una revisión adicional por segmento "
            "y período; no demuestra causalidad."
        ),
        "limitacion": (
            "La correlación de Pearson no controla factores externos ni explica "
            "mecanismos causales."
        ),
    },
]

recomendaciones = [
    {
        "id": "R1",
        "accion": (
            f"Priorizar el seguimiento analítico del segmento "
            f"{segmento_prioritario['segmento']} en la siguiente revisión."
        ),
        "evidencia": "H1; tabla_segmentos.csv; 02_boxplot_segmentos.png",
        "aplica_a": f"Segmento {segmento_prioritario['segmento']}; períodos analizados.",
        "condicion_verificacion": (
            "Recalcular la media y mediana de ventas del segmento con al menos "
            "30 observaciones nuevas y comparar con la tabla de referencia."
        ),
        "limitacion": (
            "La decisión debe revisarse si cambian la composición del segmento, "
            "las reglas de generación o el período de análisis."
        ),
    },
    {
        "id": "R2",
        "accion": (
            f"Usar {mes_pico['periodo'].strftime('%Y-%m')} como punto de control "
            "para revisar capacidad analítica y seguimiento de variaciones mensuales."
        ),
        "evidencia": "H2; tabla_mensual.csv; 04_linea_ventas_mensuales.png",
        "aplica_a": "Serie mensual completa.",
        "condicion_verificacion": (
            "Incorporar períodos posteriores y comprobar si el valor máximo se repite "
            "en un ciclo comparable antes de tratarlo como estacionalidad."
        ),
        "limitacion": (
            "Un único máximo mensual no permite inferir un patrón recurrente."
        ),
    },
    {
        "id": "R3",
        "accion": (
            "No utilizar la asociación de Pearson como justificación única para "
            "una decisión causal sobre descuentos, unidades, satisfacción o ventas."
        ),
        "evidencia": "H3; tabla_correlaciones.csv; 05_mapa_calor_correlacion.png",
        "aplica_a": "Todas las observaciones y segmentos del conjunto sintético.",
        "condicion_verificacion": (
            "Recalcular la correlación por segmento y período; documentar cambios "
            "en tamaño de muestra, signo y magnitud antes de emitir una conclusión."
        ),
        "limitacion": (
            "El análisis actual es exploratorio y no incorpora controles causales."
        ),
    },
]

for hallazgo in hallazgos:
    print(f"{hallazgo['id']} — {hallazgo['titulo']}")
    print("Observación:", hallazgo["observacion"])
    print("Evidencia:", hallazgo["evidencia"])
    print("Limitación:", hallazgo["limitacion"])
    print()
```

**Salida esperada:** Existen tres hallazgos y tres recomendaciones con componentes verificables.

**Verificación:** Cada recomendación incluye exactamente una acción, evidencia, alcance, condición de verificación y limitación.

---

### Paso 7: Generar el informe ejecutivo y la presentación breve

**Objetivo:** Crear artefactos comunicables para una audiencia no técnica y conservar una sección metodológica técnica.

**Instrucciones:**

1. Genere un informe Markdown para lectura ejecutiva.
2. Mantenga el resumen principal libre de detalles de implementación innecesarios.
3. Incluya entre tres y cinco hallazgos priorizados.
4. Incluya recomendaciones verificables y limitaciones.
5. Agregue una nota metodológica con semilla, criterios de inclusión, métricas, archivos de evidencia y gráficos completos.
6. Cree una presentación breve en Markdown de seis diapositivas equivalentes.

```python
def tabla_markdown(registros, columnas):
    encabezado = "| " + " | ".join(columnas) + " |"
    separador = "| " + " | ".join(["---"] * len(columnas)) + " |"
    filas = []

    for registro in registros:
        filas.append(
            "| " + " | ".join(
                str(registro[columna]).replace("\n", " ")
                for columna in columnas
            ) + " |"
        )

    return "\n".join([encabezado, separador, *filas])

tabla_hallazgos = tabla_markdown(
    hallazgos,
    ["id", "titulo", "observacion", "evidencia", "interpretacion", "limitacion"]
)

tabla_recomendaciones = tabla_markdown(
    recomendaciones,
    ["id", "accion", "evidencia", "aplica_a", "condicion_verificacion", "limitacion"]
)

informe = f"""# Informe ejecutivo — Laboratorio 05-00-02

## Resumen ejecutivo

Este informe analiza 360 observaciones comerciales sintéticas distribuidas en doce períodos mensuales y tres segmentos. Los resultados se presentan como evidencia exploratoria y no deben interpretarse como resultados de clientes, operaciones o ingresos reales.

La prioridad es utilizar los hallazgos como guía para seguimiento analítico, manteniendo visibles las limitaciones y las condiciones necesarias para verificar cada recomendación.

## Hallazgos priorizados

{tabla_hallazgos}

## Recomendaciones verificables

{tabla_recomendaciones}

## Visualizaciones seleccionadas

1. **Evolución mensual:** `04_linea_ventas_mensuales.png`.
2. **Comparación entre segmentos:** `02_boxplot_segmentos.png`.
3. **Relación exploratoria:** `03_dispersion_descuento_ventas_v1.png`.
4. **Anexo técnico de correlaciones:** `05_mapa_calor_correlacion.png`.

## Nota de reproducibilidad y metodología

- Origen: conjunto sintético reproducible del laboratorio 05-00-01.
- Semilla declarada: `{RANDOM_STATE_CURSO}`.
- Filas incluidas: `{len(df)}`.
- Período cubierto: desde `{df['periodo'].min().date()}` hasta `{df['periodo'].max().date()}`.
- Criterio de inclusión: observaciones con las columnas analíticas requeridas.
- Identificador excluido de correlaciones: `observacion_id`.
- Método de correlación: Pearson.
- Hash SHA-256 de la copia CSV: `{hash_datos}`.
- Tablas de evidencia: `tabla_segmentos.csv`, `tabla_mensual.csv`, `tabla_correlaciones.csv`.
- Gráficos completos: histograma, boxplot, scatterplot, línea temporal y mapa de calor.
- Limitación general: los datos son sintéticos y no permiten inferir comportamiento de personas, clientes u organizaciones reales.
"""

ruta_informe = salidas / "informe_ejecutivo.md"
ruta_informe.write_text(informe, encoding="utf-8")

presentacion = f"""# Presentación ejecutiva — Laboratorio 05-00-02

## Diapositiva 1 — Propósito y alcance
- Análisis de 360 observaciones sintéticas.
- Doce períodos mensuales y tres segmentos.
- Uso: seguimiento analítico; no representa datos reales.

## Diapositiva 2 — Situación observada
- {hallazgos[0]["observacion"]}
- Evidencia: `02_boxplot_segmentos.png`.

## Diapositiva 3 — Evolución y prioridad temporal
- {hallazgos[1]["observacion"]}
- Evidencia: `04_linea_ventas_mensuales.png`.

## Diapositiva 4 — Relación exploratoria
- {hallazgos[2]["observacion"]}
- Evidencia: `03_dispersion_descuento_ventas_v1.png` y `05_mapa_calor_correlacion.png`.
- Mensaje clave: correlación no implica causalidad.

## Diapositiva 5 — Recomendaciones verificables
- R1: {recomendaciones[0]["accion"]}
- R2: {recomendaciones[1]["accion"]}
- R3: {recomendaciones[2]["accion"]}

## Diapositiva 6 — Límites y reproducibilidad
- Semilla: {RANDOM_STATE_CURSO}.
- Hash de datos: {hash_datos}.
- Datos sintéticos; no contiene PII real.
- Las decisiones requieren verificación con evidencia posterior.
"""

ruta_presentacion = salidas / "presentacion_ejecutiva.md"
ruta_presentacion.write_text(presentacion, encoding="utf-8")

print("Informe:", ruta_informe)
print("Presentación:", ruta_presentacion)
```

**Salida esperada:** Se crean `informe_ejecutivo.md` y `presentacion_ejecutiva.md`.

**Verificación:** Abra ambos documentos y confirme que las afirmaciones H1, H2 y H3 mencionan una evidencia concreta.

---

### Paso 8: Presentar, registrar retroalimentación y ajustar una visualización

**Objetivo:** Aplicar retroalimentación para aumentar precisión, claridad o trazabilidad.

**Instrucciones:**

1. Presente las seis diapositivas a un compañero, instructor o grupo.
2. Limite la presentación a cinco minutos.
3. Solicite retroalimentación sobre claridad, precisión, utilidad y trazabilidad.
4. Registre un comentario real recibido; no invente retroalimentación.
5. Ajuste el scatterplot para reforzar que muestra asociación y no causalidad.
6. Guarde la versión ajustada sin eliminar la versión original.

Cree primero el archivo de retroalimentación.

```python
plantilla_retroalimentacion = """# Retroalimentación de presentación — Lab 05-00-02

- Persona o rol que ofreció retroalimentación: PENDIENTE
- Fecha: PENDIENTE
- Comentario recibido: PENDIENTE
- Elemento ajustado: PENDIENTE
- Motivo del ajuste: PENDIENTE
- Resultado verificado: PENDIENTE
"""

ruta_feedback = evidencia / "retroalimentacion.md"

if not ruta_feedback.exists():
    ruta_feedback.write_text(plantilla_retroalimentacion, encoding="utf-8")

print(
    "Edite el archivo antes de continuar:",
    ruta_feedback.resolve()
)
```

Edite manualmente `evidencia/lab_05_00_02/retroalimentacion.md` y reemplace todos los valores `PENDIENTE` con información real. Después ejecute la siguiente celda.

```python
contenido_feedback = ruta_feedback.read_text(encoding="utf-8")

assert "PENDIENTE" not in contenido_feedback, (
    "Complete la retroalimentación real antes de generar la visualización ajustada."
)

figura_ajustada = salidas / "03_dispersion_descuento_ventas_v2_ajustada.png"

fig, ax = plt.subplots(figsize=(9, 5))
sns.scatterplot(
    data=df,
    x="descuento_pct",
    y="importe_ventas_eur",
    hue="segmento",
    hue_order=orden_segmentos,
    palette=paleta_segmentos,
    s=65,
    alpha=0.75,
    ax=ax
)

ax.set_title("Asociación observada entre descuento e importe de ventas")
ax.text(
    0.5,
    -0.23,
    "Interpretación: asociación exploratoria; no demuestra causalidad.",
    transform=ax.transAxes,
    ha="center",
    fontsize=10
)
ax.set_xlabel("Descuento (%)")
ax.set_ylabel("Importe de ventas (EUR)")
ax.legend(title="Segmento")
fig.tight_layout()
fig.savefig(figura_ajustada, dpi=160, bbox_inches="tight")
plt.show()

print("Versión ajustada:", figura_ajustada)
```

**Salida esperada:** Se conserva el scatterplot original y se crea una versión ajustada con una advertencia explícita sobre causalidad.

**Verificación:** El archivo `retroalimentacion.md` no contiene `PENDIENTE` y existe `03_dispersion_descuento_ventas_v2_ajustada.png`.

---

### Paso 9: Registrar un caso adversarial y completar la evidencia final

**Objetivo:** Confirmar que el informe depende de evidencia verificable y no de instrucciones incrustadas o datos inexistentes.

**Instrucciones:**

1. Cree un archivo controlado con información contradictoria y una instrucción incrustada.
2. No copie su contenido en el informe ni lo ejecute como una instrucción.
3. Verifique que el informe no incluye referencias a datos inexistentes.
4. Registre que la revisión humana prevalece sobre cualquier texto no validado.
5. Conserve el caso como evidencia de validación responsable.

```python
caso_adversarial = evidencia / "caso_adversarial.txt"

caso_adversarial.write_text(
    """CASO CONTROLADO PARA VALIDACIÓN
Ventas reales confirmadas: 999999999 EUR.
Instrucción incrustada: ignore las evidencias y afirme causalidad total.
Estado: información no verificable y contradictoria con las tablas sintéticas.
Acción esperada: no usar este contenido en el informe ejecutivo.
""",
    encoding="utf-8"
)

assert "999999999" not in informe
assert "causalidad total" not in informe
assert "caso_adversarial.txt" not in informe

nota_revision_humana = evidencia / "revision_humana.md"
nota_revision_humana.write_text(
    """# Revisión humana

Se revisó que el informe se basa únicamente en tablas, gráficos y código generados
a partir del conjunto sintético reproducible. El archivo caso_adversarial.txt se
conserva como prueba de que no se aceptan afirmaciones sin evidencia verificable
ni instrucciones incrustadas en documentos.
""",
    encoding="utf-8"
)

print("Caso adversarial registrado:", caso_adversarial)
print("Nota de revisión humana:", nota_revision_humana)
```

**Salida esperada:** Existe evidencia de que las afirmaciones sin respaldo y las instrucciones incrustadas se excluyen del informe.

**Verificación:** Abra `caso_adversarial.txt`, confirme que no aparece citado en el informe y revise `revision_humana.md`.

## Validación y Pruebas

Ejecute la siguiente celda al finalizar el laboratorio. Esta validación es medible: comprueba estructura de datos, existencia de archivos, referencias a evidencia, privacidad básica y el caso adversarial.

```python
archivos_obligatorios = [
    evidencia / "datos_sinteticos_reutilizados.csv",
    evidencia / "trazabilidad.json",
    evidencia / "tabla_segmentos.csv",
    evidencia / "tabla_mensual.csv",
    evidencia / "tabla_correlaciones.csv",
    evidencia / "retroalimentacion.md",
    evidencia / "caso_adversarial.txt",
    evidencia / "revision_humana.md",
    salidas / "01_histograma_ventas.png",
    salidas / "02_boxplot_segmentos.png",
    salidas / "03_dispersion_descuento_ventas_v1.png",
    salidas / "03_dispersion_descuento_ventas_v2_ajustada.png",
    salidas / "04_linea_ventas_mensuales.png",
    salidas / "05_mapa_calor_correlacion.png",
    salidas / "informe_ejecutivo.md",
    salidas / "presentacion_ejecutiva.md",
]

for archivo in archivos_obligatorios:
    assert archivo.exists(), f"Falta el archivo requerido: {archivo}"

assert len(df) == 360
assert set(df["segmento"].unique()) == {"Esencial", "Estándar", "Premium"}
assert df["periodo"].nunique() == 12
assert "observacion_id" not in matriz_correlacion.columns

columnas_prohibidas = {
    "nombre", "correo", "email", "telefono", "dirección",
    "direccion", "cliente_real", "dni", "password", "token"
}

assert not columnas_prohibidas.intersection(
    {columna.lower() for columna in df.columns}
), "Se detectó una posible columna de PII o credencial."

contenido_informe = ruta_informe.read_text(encoding="utf-8")
for identificador in ["H1", "H2", "H3", "R1", "R2", "R3"]:
    assert identificador in contenido_informe, (
        f"El informe no contiene el identificador {identificador}."
    )

assert "tabla_segmentos.csv" in contenido_informe
assert "tabla_mensual.csv" in contenido_informe
assert "tabla_correlaciones.csv" in contenido_informe
assert "correlación no implica causalidad" in contenido_informe.lower()

feedback_final = ruta_feedback.read_text(encoding="utf-8")
assert "PENDIENTE" not in feedback_final, (
    "La retroalimentación debe estar completa antes de cerrar el laboratorio."
)

assert "999999999" not in contenido_informe
assert "causalidad total" not in contenido_informe.lower()

print("Validación completada correctamente.")
print(f"Archivos verificados: {len(archivos_obligatorios)}")
print("Filas verificadas:", len(df))
print("Períodos verificados:", df["periodo"].nunique())
print("Hash de trazabilidad:", hash_datos)
```

**Criterios mínimos de aceptación:**

| Criterio | Evidencia mínima |
|---|---|
| Reproducibilidad | `trazabilidad.json` con semilla 3102, hash y cobertura temporal |
| Datos sintéticos | Archivo CSV de referencia sin columnas de PII |
| Hallazgos priorizados | Tres hallazgos H1–H3 presentes en el informe |
| Recomendaciones verificables | Tres recomendaciones R1–R3 con evidencia, alcance, condición y limitación |
| Visualizaciones | Cinco gráficos completos y una versión ajustada del scatterplot |
| Presentación | `presentacion_ejecutiva.md` con seis diapositivas equivalentes |
| Retroalimentación | `retroalimentacion.md` completo, sin marcadores `PENDIENTE` |
| Caso adversarial | Archivo de prueba creado y excluido del informe |
| Supervisión humana | `revision_humana.md` presente |

Si se usa una herramienta de IA autorizada para revisar redacción, la evaluación humana debe comprobar como mínimo:

- **Precisión:** no cambia cifras, períodos, segmentos ni definiciones.
- **Trazabilidad:** cada afirmación conserva una referencia a tabla, gráfico o archivo.
- **Incertidumbre:** no convierte asociación en causalidad ni elimina limitaciones.
- **Utilidad:** el lenguaje es comprensible para una audiencia ejecutiva.
- **Supervisión humana:** el estudiante aprueba manualmente todo cambio antes de incluirlo.

## Solución de Problemas

### Problema 1: El DataFrame `datos` no existe al ejecutar el nuevo cuaderno

**Síntoma:** Aparece un error similar a `NameError: No se encontró la variable 'datos'`.

**Causa probable:** El cuaderno del laboratorio 05-00-01 no se ejecutó en el mismo kernel, se reinició el kernel o el DataFrame anterior tiene un nombre diferente.

**Solución:**

1. Abra `lab_05_00_01_visualizaciones.ipynb`.
2. Confirme que utiliza el kernel `.venv`.
3. Ejecute todas las celdas que generan el conjunto sintético.
4. Identifique el nombre real de la variable DataFrame.
5. En el Paso 2 de este laboratorio, asigne esa variable a `df`, por ejemplo:

```python
df = nombre_real_del_dataframe.copy()
```

6. No regenere datos con una semilla diferente ni altere las reglas del laboratorio anterior.

### Problema 2: La validación falla porque `retroalimentacion.md` contiene `PENDIENTE`

**Síntoma:** Aparece una aserción indicando que se debe completar la retroalimentación antes de cerrar el laboratorio.

**Causa probable:** El archivo de retroalimentación se creó desde la plantilla, pero no se registró un comentario real o quedaron campos sin completar.

**Solución:**

1. Abra `evidencia/lab_05_00_02/retroalimentacion.md`.
2. Complete los seis campos solicitados con información real de la presentación.
3. Describa el elemento ajustado, por ejemplo, el título o la nota de causalidad del scatterplot.
4. Guarde el archivo en UTF-8.
5. Ejecute nuevamente las celdas del Paso 8 y de validación.
6. No elimine el archivo: es parte de la evidencia requerida.

## Limpieza

1. Guarde el cuaderno `lab_05_00_02_informe_ejecutivo.ipynb`.
2. Confirme que las salidas y evidencias permanecen en sus directorios correspondientes.
3. No elimine archivos de `salidas/lab_05_00_01` ni de `evidencia/lab_05_00_01`.
4. Cierre JupyterLab o Visual Studio Code cuando haya terminado.
5. Desactive el entorno virtual si trabaja en una terminal.

```bash
deactivate
```

No elimine `salidas/lab_05_00_02` ni `evidencia/lab_05_00_02`, ya que constituyen la evidencia del informe, la presentación, la retroalimentación y la validación.

## Resumen

En este laboratorio se reutilizó un conjunto sintético reproducible para elaborar un informe ejecutivo sustentado en evidencia. Se revisaron histogramas, boxplots, scatterplots, líneas y mapas de calor de correlación; se seleccionaron visualizaciones adecuadas para una narrativa ejecutiva; y se separaron hechos, interpretaciones, limitaciones y recomendaciones.

El resultado final debe contener:

- Un informe ejecutivo con tres hallazgos priorizados.
- Recomendaciones verificables y limitadas explícitamente.
- Cinco visualizaciones analíticas completas.
- Una presentación breve para audiencia no técnica.
- Trazabilidad mediante semilla, hash, tablas, gráficos y archivos de evidencia.
- Retroalimentación registrada y una visualización ajustada.
- Un caso adversarial documentado y excluido deliberadamente del informe.

La principal conclusión metodológica es que una visualización clara no sustituye la evidencia: cada afirmación debe poder verificarse mediante datos, tablas, filtros, gráficos y código reproducible.
