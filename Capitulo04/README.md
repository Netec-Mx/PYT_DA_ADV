# Práctica: Crear un pipeline con Pandas/NumPy que filtre con loc/iloc, limpie, combine, pivotee y agregue datos

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | **210 minutos** |
| Complejidad | Difícil |
| Nivel de Bloom | Aplicar |
| Certificación relacionada | Certified Associate Data Analyst with Python (PCAD-31-02), Python Institute |
| Modalidad | Individual, entorno local, datos sintéticos o anonimizados |
| Producto principal | DataFrame analítico validado, persistido en CSV y Parquet, con particiones de entrenamiento y prueba |

## Descripción General

En este laboratorio construirá un pipeline reproducible para inspeccionar, limpiar, transformar, combinar y resumir el conjunto sintético reutilizado del laboratorio 03-00-02. Aplicará selección con `.loc` e `.iloc`, tratamiento de valores nulos, normalización de tipos, integración de una tabla auxiliar, reestructuración con `melt`, `pivot` y `pivot_table`, además de agregaciones con `groupby` y `crosstab`.

Como extensión de preparación analítica, generará variables codificadas, una partición entrenamiento/prueba y modelos supervisados mínimos de regresión lineal y logística. El resultado quedará preparado para el modelado y evaluación más profundos del laboratorio 04-00-02.

## Objetivos de Aprendizaje

- [ ] Inspeccionar el estado inicial de un DataFrame: columnas, tipos, nulos, duplicados y valores fuera de rango.
- [ ] Aplicar `.loc` para filtros y asignaciones por etiquetas, y `.iloc` para selecciones por posición, justificando su diferencia.
- [ ] Limpiar y ordenar datos mediante `dropna`, `fillna`, `replace`, conversión de tipos y `sort_values`.
- [ ] Combinar tablas con `merge` y `join`, verificando cardinalidad, filas no emparejadas y conservación de totales.
- [ ] Usar NumPy para transformaciones vectorizadas, broadcasting, agregaciones y comparación con un enfoque iterativo.
- [ ] Persistir el conjunto analítico en CSV y Parquet, crear variables codificadas y generar particiones de entrenamiento/prueba reproducibles.

## Prerrequisitos

### Conocimientos

Debe dominar los siguientes contenidos antes de iniciar:

- Estructuras `Series` y `DataFrame` de Pandas.
- Indexación elemental de DataFrame y arrays.
- Arrays NumPy, operaciones vectorizadas, broadcasting y agregaciones.
- Operaciones SQL básicas, claves y relaciones practicadas en el laboratorio 02-00-03.
- Resultados, variables y observaciones de calidad documentadas en el laboratorio 03-00-02.
- Fundamentos de regresión lineal, regresión logística, accuracy, sobreajuste, subajuste y compromiso sesgo-varianza.

### Acceso y continuidad

1. Debe conservar sin cambios los datos, notebooks y salidas de laboratorios anteriores.
2. Debe disponer de un archivo CSV o Parquet reutilizable creado o identificado en el laboratorio 03-00-02.
3. Debe usar exclusivamente datos sintéticos o anonimizados. No incorpore PII real, credenciales, tokens ni datos de clientes.
4. Debe tener acceso de escritura al directorio raíz del proyecto.
5. Este laboratorio no usa Docker, contenedores, servicios remotos ni puertos HTTP.
6. No se requiere acceso a una base SQLite durante este laboratorio; conserve sin modificar la base local creada en el laboratorio 02-00-01.

> **Nota sobre instrucciones e IA:** este laboratorio contiene instrucciones técnicas persistentes dentro del cuaderno y de esta guía. Un `prompt` es una instrucción enviada temporalmente a un sistema de IA; no equivale a un mensaje de sistema ni convierte a una herramienta en un agente persistente. No se requiere IA generativa para completar la práctica. Si se consulta una herramienta de IA autorizada por el instructor, sus respuestas deben verificarse contra la documentación y los resultados ejecutados localmente.

## Entorno de Laboratorio

### Hardware mínimo

| Recurso | Requisito |
|---|---|
| Procesador | 64 bits, dos núcleos o superior |
| Memoria RAM | 8 GB mínimo; 16 GB recomendado |
| Disco libre | 10 GB recomendado |
| Pantalla | 1366x768 mínimo; 1920x1080 recomendado |
| Sistema operativo | Windows 10/11 de 64 bits, macOS de 64 bits o Linux de 64 bits compatible con Python |

### Software y versiones

| Tecnología | Versión exacta | Arquitectura o edición | Fuente oficial |
|---|---:|---|---|
| Python | 3.12.11 | CPython, 64 bits | https://www.python.org/downloads/release/python-31211/ |
| pip | 24.3.1 | Python package installer | https://pip.pypa.io/en/stable/news/ |
| Pandas | 3.0.0 | Paquete Python | https://pandas.pydata.org/docs/ |
| NumPy | 2.2.6 | Paquete Python | https://numpy.org/doc/2.2/ |
| PyArrow | 20.0.0 | Paquete Python | https://arrow.apache.org/docs/20.0/python/ |
| scikit-learn | 1.6.0 | Paquete Python | https://scikit-learn.org/1.6/ |
| JupyterLab | 4.4.3 | Aplicación web local | https://jupyterlab.readthedocs.io/en/4.4.x/ |
| Visual Studio Code | 1.102.0 | Edición de 64 bits | https://code.visualstudio.com/updates/v1_102 |

> Matplotlib, Seaborn, Plotly y SQLAlchemy no son necesarios para ejecutar las actividades principales de este laboratorio. Si ya existen en el entorno virtual del curso, no los desinstale ni modifique sus versiones.

### Preparación del entorno virtual

### Paso 1: Crear la estructura del laboratorio

**Objetivo:** Crear las carpetas y el cuaderno del laboratorio sin alterar artefactos de laboratorios anteriores.

**Tiempo estimado:** 10 minutos.

**Instrucciones:**

1. Abra una terminal en la raíz del proyecto del curso.
2. Active el entorno virtual `.venv` ya utilizado en el laboratorio 03-00-02.
3. Cree las carpetas de salida y evidencia del laboratorio.
4. Cree el cuaderno `notebooks/lab_04_00_01_pipeline_pandas_numpy.ipynb`.
5. No elimine ni regenere el conjunto reutilizado del laboratorio anterior.

**Windows PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
New-Item -ItemType Directory -Force -Path salidas\lab_04_00_01, evidencia\lab_04_00_01, notebooks
New-Item -ItemType File -Force -Path notebooks\lab_04_00_01_pipeline_pandas_numpy.ipynb
```

**macOS o Linux:**

```bash
source .venv/bin/activate
mkdir -p salidas/lab_04_00_01 evidencia/lab_04_00_01 notebooks
touch notebooks/lab_04_00_01_pipeline_pandas_numpy.ipynb
```

**Salida esperada:** Existen las carpetas `salidas/lab_04_00_01`, `evidencia/lab_04_00_01` y el cuaderno indicado.

**Verificación:**

```python
from pathlib import Path

for ruta in [
    Path("notebooks/lab_04_00_01_pipeline_pandas_numpy.ipynb"),
    Path("salidas/lab_04_00_01"),
    Path("evidencia/lab_04_00_01"),
]:
    print(ruta, "->", ruta.exists())
```

### Paso 2: Verificar intérprete y dependencias

**Objetivo:** Confirmar que el cuaderno usará el entorno virtual y las versiones requeridas.

**Tiempo estimado:** 12 minutos.

**Instrucciones:**

1. Abra el cuaderno en JupyterLab o Visual Studio Code.
2. Seleccione el kernel asociado a `.venv`.
3. Inserte una celda Markdown con su nombre, fecha, identificador del laboratorio y la frase exacta: `Datos sintéticos; no contiene PII real.`
4. Ejecute la siguiente celda de código.
5. Si alguna versión no coincide, solicite orientación al instructor antes de continuar.

```python
import sys
import pandas as pd
import numpy as np
import pyarrow as pa
import sklearn

print("Python:", sys.version)
print("Ejecutable:", sys.executable)
print("Pandas:", pd.__version__)
print("NumPy:", np.__version__)
print("PyArrow:", pa.__version__)
print("scikit-learn:", sklearn.__version__)

assert ".venv" in sys.executable, "El kernel no pertenece al entorno virtual .venv."
assert pd.__version__ == "3.0.0"
assert np.__version__ == "2.2.6"
assert pa.__version__ == "20.0.0"
assert sklearn.__version__ == "1.6.0"
```

**Salida esperada:** Se muestran Python 3.12.11, Pandas 3.0.0, NumPy 2.2.6, PyArrow 20.0.0 y scikit-learn 1.6.0.

**Verificación:** La ruta de `sys.executable` contiene `.venv` y no se produce ninguna excepción.

### Paso 3: Configurar rutas, semilla y fuentes de datos

**Objetivo:** Definir un contexto reproducible y localizar el archivo reutilizado de 03-00-02.

**Tiempo estimado:** 15 minutos.

**Instrucciones:**

1. Inserte una nueva celda de código.
2. Mantenga `RANDOM_STATE_CURSO = 3102`.
3. Ejecute la búsqueda de archivos CSV y Parquet fuera de `.venv`.
4. Identifique el archivo exacto reutilizado en el laboratorio anterior.
5. Asigne la ruta correcta a `RUTA_DATOS_ORIGEN`.
6. No cree un nuevo conjunto sintético para reemplazar el conjunto anterior.

```python
from pathlib import Path
import platform

RANDOM_STATE_CURSO = 3102

RAIZ_PROYECTO = Path.cwd()
RUTA_SALIDAS = RAIZ_PROYECTO / "salidas" / "lab_04_00_01"
RUTA_EVIDENCIA = RAIZ_PROYECTO / "evidencia" / "lab_04_00_01"

archivos_disponibles = sorted(
    ruta for ruta in RAIZ_PROYECTO.rglob("*")
    if ruta.suffix.lower() in {".csv", ".parquet"}
    and ".venv" not in ruta.parts
)

print("Sistema operativo:", platform.platform())
print("Semilla del curso:", RANDOM_STATE_CURSO)
print("Raíz del proyecto:", RAIZ_PROYECTO)
print("\nArchivos CSV/Parquet localizados:")
for archivo in archivos_disponibles:
    print("-", archivo.relative_to(RAIZ_PROYECTO))
```

**Salida esperada:** Se muestra una lista de archivos CSV o Parquet disponibles, incluidos los producidos o identificados en el laboratorio 03-00-02.

**Verificación:** Documente en una celda Markdown la ruta seleccionada y la razón de su elección.

### Paso 4: Cargar el conjunto reutilizado y registrar el estado inicial

**Objetivo:** Cargar el archivo fuente sin modificarlo y crear copias controladas en memoria.

**Tiempo estimado:** 15 minutos.

**Instrucciones:**

1. Reemplace la ruta de ejemplo por el archivo identificado en el paso anterior.
2. Ejecute la carga según la extensión del archivo.
3. Cree una copia profunda para preservar el estado original en memoria.
4. Registre dimensiones, columnas, tipos, nulos y duplicados.
5. Guarde la auditoría inicial como CSV.

```python
## Reemplace la ruta por el archivo exacto localizado en el paso anterior.
RUTA_DATOS_ORIGEN = Path("salidas/lab_03_00_02/datos_reutilizados.parquet")

if not RUTA_DATOS_ORIGEN.exists():
    raise FileNotFoundError(
        f"No existe {RUTA_DATOS_ORIGEN}. Corrija la ruta sin generar datos nuevos."
    )

if RUTA_DATOS_ORIGEN.suffix.lower() == ".csv":
    df_origen = pd.read_csv(RUTA_DATOS_ORIGEN)
elif RUTA_DATOS_ORIGEN.suffix.lower() == ".parquet":
    df_origen = pd.read_parquet(RUTA_DATOS_ORIGEN, engine="pyarrow")
else:
    raise ValueError("El archivo fuente debe tener extensión .csv o .parquet.")

df_trabajo = df_origen.copy(deep=True)

auditoria_inicial = pd.DataFrame({
    "columna": df_trabajo.columns,
    "tipo": df_trabajo.dtypes.astype(str).to_numpy(),
    "nulos": df_trabajo.isna().sum().to_numpy(),
    "porcentaje_nulos": (df_trabajo.isna().mean() * 100).round(2).to_numpy(),
    "valores_unicos": df_trabajo.nunique(dropna=False).to_numpy(),
})

print("Dimensiones iniciales:", df_trabajo.shape)
print("Filas duplicadas completas:", df_trabajo.duplicated().sum())
display(auditoria_inicial)

auditoria_inicial.to_csv(
    RUTA_SALIDAS / "auditoria_inicial.csv",
    index=False,
    encoding="utf-8"
)
```

**Salida esperada:** Se visualiza la auditoría de las columnas y se crea `salidas/lab_04_00_01/auditoria_inicial.csv`.

**Verificación:**

```python
assert df_origen.equals(df_trabajo)
assert (RUTA_SALIDAS / "auditoria_inicial.csv").exists()
```

### Paso 5: Normalizar nombres de columnas y validar variables mínimas

**Objetivo:** Preparar nombres consistentes sin perder trazabilidad respecto a las variables documentadas en 03-00-02.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Revise las columnas mostradas en la auditoría inicial.
2. Use el diccionario de equivalencias únicamente si sus nombres de columnas difieren de los esperados.
3. Mantenga las variables originales; solo normalice el nombre de trabajo.
4. Confirme que existe una variable de ventas y una variable de inversión publicitaria.
5. Registre los nombres originales y normalizados.

```python
def normalizar_nombre(nombre):
    reemplazos = {
        "á": "a", "é": "e", "í": "i", "ó": "o", "ú": "u",
        "ñ": "n", " ": "_", "-": "_", "/": "_"
    }
    resultado = str(nombre).strip().lower()
    for origen, destino in reemplazos.items():
        resultado = resultado.replace(origen, destino)
    return resultado

df = df_trabajo.copy(deep=True)
columnas_originales = df.columns.tolist()
df.columns = [normalizar_nombre(columna) for columna in df.columns]

equivalencias = {
    "venta": "ventas",
    "importe": "ventas",
    "ventas_totales": "ventas",
    "inversion": "inversion_publicitaria",
    "gasto_publicitario": "inversion_publicitaria",
    "publicidad": "inversion_publicitaria",
}

df = df.rename(columns={
    columna: equivalencias[columna]
    for columna in df.columns
    if columna in equivalencias
})

trazabilidad_columnas = pd.DataFrame({
    "columna_original": columnas_originales,
    "columna_normalizada": [normalizar_nombre(columna) for columna in columnas_originales],
})

columnas_requeridas = {"ventas", "inversion_publicitaria"}
faltantes = columnas_requeridas - set(df.columns)

if faltantes:
    raise KeyError(
        "Faltan variables requeridas: "
        f"{sorted(faltantes)}. Revise las equivalencias según 03-00-02."
    )

display(trazabilidad_columnas)
print("Columnas analíticas disponibles:", df.columns.tolist())
```

**Salida esperada:** El DataFrame contiene las columnas `ventas` e `inversion_publicitaria`.

**Verificación:**

```python
assert {"ventas", "inversion_publicitaria"}.issubset(df.columns)
trazabilidad_columnas.to_csv(
    RUTA_SALIDAS / "trazabilidad_columnas.csv",
    index=False,
    encoding="utf-8"
)
```

### Paso 6: Inspeccionar con `.loc` e `.iloc`

**Objetivo:** Aplicar selección por etiquetas y selección por posiciones, justificando la diferencia entre ambos mecanismos.

**Tiempo estimado:** 18 minutos.

**Instrucciones:**

1. Seleccione las primeras cinco observaciones y las columnas numéricas por posición con `.iloc`.
2. Seleccione registros con ventas superiores o iguales a la mediana mediante `.loc`.
3. Use `.loc` para crear una asignación explícita y segura.
4. Compare los resultados.
5. Añada una celda Markdown que explique que `.loc` opera con etiquetas y `.iloc` con posiciones enteras.

```python
columnas_numericas = df.select_dtypes(include="number").columns.tolist()

muestra_iloc = df.iloc[:5, :min(5, df.shape[1])]
display(muestra_iloc)

mediana_ventas = df["ventas"].median()

muestra_loc = df.loc[
    df["ventas"] >= mediana_ventas,
    ["ventas", "inversion_publicitaria"]
].head(5)

display(muestra_loc)

df.loc[df["ventas"] >= mediana_ventas, "nivel_ventas"] = "Alta"
df.loc[df["ventas"] < mediana_ventas, "nivel_ventas"] = "Baja"

print(df["nivel_ventas"].value_counts(dropna=False))
```

**Salida esperada:** Se muestran dos subconjuntos: uno seleccionado por posiciones y otro por condición etiquetada.

**Verificación:**

```python
assert df["nivel_ventas"].isin(["Alta", "Baja"]).all()
assert len(muestra_iloc) <= 5
```

### Paso 7: Detectar rangos inválidos, nulos y duplicados

**Objetivo:** Medir problemas de calidad antes de aplicar reglas de limpieza declaradas.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Convierta ventas e inversión publicitaria a valores numéricos.
2. Cuantifique conversiones fallidas, valores nulos, registros duplicados y valores no válidos.
3. Declare las reglas de limpieza en una tabla.
4. No modifique todavía `df_origen`.
5. Guarde el reporte de calidad inicial.

```python
df_calidad = df.copy(deep=True)

for columna in ["ventas", "inversion_publicitaria"]:
    df_calidad[columna] = pd.to_numeric(df_calidad[columna], errors="coerce")

reporte_calidad = pd.DataFrame({
    "indicador": [
        "filas_iniciales",
        "ventas_nulas",
        "inversion_nula",
        "ventas_no_positivas",
        "inversion_negativa",
        "filas_duplicadas",
    ],
    "valor": [
        len(df_calidad),
        int(df_calidad["ventas"].isna().sum()),
        int(df_calidad["inversion_publicitaria"].isna().sum()),
        int((df_calidad["ventas"] <= 0).sum()),
        int((df_calidad["inversion_publicitaria"] < 0).sum()),
        int(df_calidad.duplicated().sum()),
    ],
})

reglas_limpieza = pd.DataFrame({
    "situacion": [
        "Ventas nulas o no positivas",
        "Inversión nula o negativa",
        "Categorías vacías o marcadores textuales",
        "Filas duplicadas completas",
    ],
    "regla": [
        "Eliminar con dropna y filtro porque no permiten calcular métricas de ventas.",
        "Convertir valores negativos en nulos e imputar con mediana.",
        "Aplicar replace y fillna con la etiqueta 'Sin categoría'.",
        "Eliminar conservando la primera ocurrencia.",
    ],
})

display(reporte_calidad)
display(reglas_limpieza)

reporte_calidad.to_csv(
    RUTA_SALIDAS / "reporte_calidad_inicial.csv",
    index=False,
    encoding="utf-8"
)
```

**Salida esperada:** Se obtiene un reporte cuantitativo de problemas de calidad y una tabla de reglas declaradas.

**Verificación:**

```python
assert len(reporte_calidad) == 6
assert (RUTA_SALIDAS / "reporte_calidad_inicial.csv").exists()
```

### Paso 8: Limpiar, normalizar y ordenar los registros

**Objetivo:** Aplicar reglas trazables mediante `dropna`, `fillna`, `replace`, `drop_duplicates` y `sort_values`.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Cree una nueva copia para la limpieza.
2. Elimine filas sin ventas válidas.
3. Convierta inversiones negativas en valores faltantes.
4. Impute inversión faltante con la mediana de inversiones válidas.
5. Identifique una columna categórica existente, si está disponible.
6. Estandarice categorías vacías o marcadores textuales.
7. Elimine duplicados completos y ordene por ventas descendentes.
8. Registre la cantidad de filas antes y después.

```python
df_limpio = df_calidad.copy(deep=True)
filas_antes_limpieza = len(df_limpio)

df_limpio = df_limpio.dropna(subset=["ventas"])
df_limpio = df_limpio.loc[df_limpio["ventas"] > 0].copy()

df_limpio.loc[
    df_limpio["inversion_publicitaria"] < 0,
    "inversion_publicitaria"
] = np.nan

mediana_inversion = df_limpio["inversion_publicitaria"].median()

if pd.isna(mediana_inversion):
    raise ValueError(
        "No hay valores válidos de inversión para imputar. Revise el archivo fuente."
    )

df_limpio["inversion_publicitaria"] = (
    df_limpio["inversion_publicitaria"]
    .fillna(mediana_inversion)
)

candidatas_categoria = [
    columna for columna in df_limpio.select_dtypes(
        include=["object", "string", "category"]
    ).columns
    if columna not in {"nivel_ventas"}
]

if candidatas_categoria:
    columna_categoria_origen = candidatas_categoria[0]
    df_limpio["categoria_analitica"] = (
        df_limpio[columna_categoria_origen]
        .astype("string")
        .str.strip()
        .replace({"": pd.NA, "N/A": pd.NA, "NA": pd.NA, "null": pd.NA})
        .fillna("Sin categoría")
    )
else:
    columna_categoria_origen = None
    df_limpio["categoria_analitica"] = "Sin categoría"

df_limpio = (
    df_limpio
    .drop_duplicates()
    .sort_values(
        by=["ventas", "inversion_publicitaria"],
        ascending=[False, False]
    )
    .reset_index(drop=True)
)

resumen_limpieza = pd.DataFrame({
    "medida": [
        "filas_antes_limpieza",
        "filas_despues_limpieza",
        "filas_eliminadas",
        "mediana_inversion_imputada",
        "columna_categoria_origen",
    ],
    "valor": [
        filas_antes_limpieza,
        len(df_limpio),
        filas_antes_limpieza - len(df_limpio),
        mediana_inversion,
        columna_categoria_origen or "No disponible",
    ],
})

display(resumen_limpieza)
display(df_limpio.head())
```

**Salida esperada:** Existe un DataFrame limpio, ordenado y sin valores nulos en `ventas` ni `inversion_publicitaria`.

**Verificación:**

```python
assert df_limpio["ventas"].notna().all()
assert df_limpio["inversion_publicitaria"].notna().all()
assert (df_limpio["ventas"] > 0).all()
assert (df_limpio["inversion_publicitaria"] >= 0).all()
assert df_limpio.duplicated().sum() == 0
```

### Paso 9: Crear una dimensión auxiliar y combinarla con `merge` y `join`

**Objetivo:** Crear una tabla auxiliar en memoria a partir de rangos presentes en los datos y verificar la relación entre tablas.

**Tiempo estimado:** 16 minutos.

**Instrucciones:**

1. Calcule la mediana de inversión publicitaria.
2. Clasifique cada observación en los rangos iniciales `bajo` y `alto`.
3. Use `replace` para normalizar las etiquetas a `Baja inversión` y `Alta inversión`.
4. Cree una dimensión auxiliar con un factor numérico por rango.
5. Combine mediante `merge` con validación `many_to_one`.
6. Revise filas no emparejadas usando `indicator=True`.
7. Repita la integración con `join` y confirme que el factor obtenido coincide.

```python
umbral_inversion = df_limpio["inversion_publicitaria"].median()

df_limpio["tramo_inversion"] = np.where(
    df_limpio["inversion_publicitaria"] <= umbral_inversion,
    "bajo",
    "alto"
)

df_limpio["tramo_inversion"] = df_limpio["tramo_inversion"].replace({
    "bajo": "Baja inversión",
    "alto": "Alta inversión",
})

dimension_tramo = pd.DataFrame({
    "tramo_inversion": ["Baja inversión", "Alta inversión"],
    "factor_tramo": [0.90, 1.10],
    "descripcion_tramo": [
        "Inversión menor o igual a la mediana",
        "Inversión superior a la mediana",
    ],
})

filas_antes_merge = len(df_limpio)
ventas_antes_merge = df_limpio["ventas"].sum()

df_integrado = df_limpio.merge(
    dimension_tramo,
    on="tramo_inversion",
    how="left",
    validate="many_to_one",
    indicator=True
)

no_emparejadas = df_integrado.loc[
    df_integrado["_merge"] != "both"
].copy()

df_join = df_limpio.join(
    dimension_tramo.set_index("tramo_inversion"),
    on="tramo_inversion",
    how="left",
    validate="many_to_one",
    rsuffix="_join"
)

display(dimension_tramo)
display(no_emparejadas)
```

**Salida esperada:** Todas las filas quedan emparejadas con la tabla auxiliar y obtienen un `factor_tramo`.

**Verificación:**

```python
assert len(df_integrado) == filas_antes_merge
assert df_integrado["ventas"].sum() == ventas_antes_merge
assert no_emparejadas.empty
assert df_integrado["factor_tramo"].equals(df_join["factor_tramo"])

df_integrado = df_integrado.drop(columns="_merge")
```

### Paso 10: Aplicar vectorización y broadcasting con NumPy

**Objetivo:** Crear una transformación numérica vectorizada y compararla con un enfoque iterativo equivalente.

**Tiempo estimado:** 18 minutos.

**Instrucciones:**

1. Convierta las columnas necesarias en arrays NumPy.
2. Calcule un índice de eficiencia ajustado por tramo usando vectorización y broadcasting.
3. Implemente una versión iterativa equivalente con una lista Python.
4. Compare ambos resultados con `np.allclose`.
5. Calcule agregaciones NumPy para comprobar coherencia con Pandas.

```python
ventas_array = df_integrado["ventas"].to_numpy(dtype=float)
inversion_array = df_integrado["inversion_publicitaria"].to_numpy(dtype=float)
factor_array = df_integrado["factor_tramo"].to_numpy(dtype=float)

indice_eficiencia_vectorizado = (
    ventas_array / np.maximum(inversion_array, 1.0)
) * factor_array

df_integrado["indice_eficiencia"] = indice_eficiencia_vectorizado

indice_eficiencia_iterativo = []
for venta, inversion, factor in zip(
    ventas_array,
    inversion_array,
    factor_array
):
    denominador = max(inversion, 1.0)
    indice_eficiencia_iterativo.append((venta / denominador) * factor)

comparacion_vectorizacion = pd.DataFrame({
    "medida": [
        "media_numpy",
        "suma_numpy",
        "maximo_numpy",
        "coincide_vectorizado_iterativo",
    ],
    "valor": [
        np.mean(indice_eficiencia_vectorizado),
        np.sum(indice_eficiencia_vectorizado),
        np.max(indice_eficiencia_vectorizado),
        np.allclose(
            indice_eficiencia_vectorizado,
            np.array(indice_eficiencia_iterativo)
        ),
    ],
})

display(comparacion_vectorizacion)
```

**Salida esperada:** La transformación vectorizada y la iterativa producen resultados equivalentes.

**Verificación:**

```python
assert np.allclose(
    df_integrado["indice_eficiencia"].to_numpy(),
    np.array(indice_eficiencia_iterativo)
)
assert np.isfinite(df_integrado["indice_eficiencia"]).all()
```

### Paso 11: Reestructurar con `groupby`, `pivot`, `pivot_table`, `melt` y `crosstab`

**Objetivo:** Resumir y transformar los datos entre formatos largo y ancho, validando los totales.

**Tiempo estimado:** 22 minutos.

**Instrucciones:**

1. Genere un resumen de ventas por tramo de inversión y categoría analítica.
2. Construya una tabla con `pivot`.
3. Construya una tabla equivalente con `pivot_table`.
4. Convierta la tabla ancha nuevamente a formato largo mediante `melt`.
5. Genere una tabla cruzada de frecuencias.
6. Compare los totales antes y después de cada transformación.

```python
total_ventas_integrado = df_integrado["ventas"].sum()

resumen_grupos = (
    df_integrado
    .groupby(
        ["tramo_inversion", "categoria_analitica"],
        dropna=False
    )
    .agg(
        ventas_totales=("ventas", "sum"),
        operaciones=("ventas", "size"),
        venta_promedio=("ventas", "mean"),
        inversion_promedio=("inversion_publicitaria", "mean"),
        eficiencia_promedio=("indice_eficiencia", "mean"),
    )
    .reset_index()
)

tabla_pivot = resumen_grupos.pivot(
    index="tramo_inversion",
    columns="categoria_analitica",
    values="ventas_totales"
).fillna(0)

tabla_pivot_table = pd.pivot_table(
    df_integrado,
    index="tramo_inversion",
    columns="categoria_analitica",
    values="ventas",
    aggfunc="sum",
    fill_value=0,
    margins=True,
    margins_name="Total"
)

tabla_larga = (
    tabla_pivot
    .reset_index()
    .melt(
        id_vars="tramo_inversion",
        var_name="categoria_analitica",
        value_name="ventas_totales"
    )
)

tabla_cruzada = pd.crosstab(
    df_integrado["tramo_inversion"],
    df_integrado["nivel_ventas"],
    margins=True,
    margins_name="Total"
)

validacion_totales = pd.DataFrame({
    "origen": [
        "DataFrame integrado",
        "groupby",
        "pivot",
        "melt",
        "pivot_table sin fila/columna Total",
    ],
    "ventas_totales": [
        total_ventas_integrado,
        resumen_grupos["ventas_totales"].sum(),
        tabla_pivot.to_numpy().sum(),
        tabla_larga["ventas_totales"].sum(),
        tabla_pivot_table.drop(
            index="Total",
            columns="Total",
            errors="ignore"
        ).to_numpy().sum(),
    ],
})

display(resumen_grupos)
display(tabla_pivot)
display(tabla_larga.head())
display(tabla_cruzada)
display(validacion_totales)
```

**Salida esperada:** Se generan resúmenes consistentes en formato largo, ancho y cruzado.

**Verificación:**

```python
assert np.allclose(
    validacion_totales["ventas_totales"],
    total_ventas_integrado
)
assert tabla_cruzada.loc["Total", "Total"] == len(df_integrado)
```

### Paso 12: Preparar variables, codificar y particionar datos

**Objetivo:** Crear un conjunto analítico para entrenamiento y prueba sin modificar el archivo fuente.

**Tiempo estimado:** 12 minutos.

**Instrucciones:**

1. Seleccione variables numéricas y categóricas relevantes.
2. Codifique las variables categóricas con `pd.get_dummies`.
3. Cree una partición reproducible entrenamiento/prueba.
4. Calcule el umbral de ventas altas usando solo entrenamiento para evitar fuga de información.
5. Cree la variable objetivo binaria en ambos subconjuntos.

```python
from sklearn.model_selection import train_test_split

df_modelo = df_integrado.copy(deep=True)

indices_entrenamiento, indices_prueba = train_test_split(
    df_modelo.index,
    test_size=0.25,
    random_state=RANDOM_STATE_CURSO,
    shuffle=True
)

df_entrenamiento = df_modelo.loc[indices_entrenamiento].copy()
df_prueba = df_modelo.loc[indices_prueba].copy()

umbral_ventas_entrenamiento = df_entrenamiento["ventas"].median()

for subconjunto in [df_entrenamiento, df_prueba]:
    subconjunto["ventas_altas"] = (
        subconjunto["ventas"] >= umbral_ventas_entrenamiento
    ).astype(int)

variables_modelo = [
    "inversion_publicitaria",
    "factor_tramo",
    "tramo_inversion",
    "categoria_analitica",
]

X_entrenamiento = pd.get_dummies(
    df_entrenamiento[variables_modelo],
    drop_first=False,
    dtype=float
)

X_prueba = pd.get_dummies(
    df_prueba[variables_modelo],
    drop_first=False,
    dtype=float
)

X_entrenamiento, X_prueba = X_entrenamiento.align(
    X_prueba,
    join="left",
    axis=1,
    fill_value=0
)

y_reg_entrenamiento = df_entrenamiento["ventas"]
y_reg_prueba = df_prueba["ventas"]
y_clf_entrenamiento = df_entrenamiento["ventas_altas"]
y_clf_prueba = df_prueba["ventas_altas"]

print("Entrenamiento:", X_entrenamiento.shape)
print("Prueba:", X_prueba.shape)
print("Umbral de ventas altas:", umbral_ventas_entrenamiento)
```

**Salida esperada:** Existen conjuntos `X_entrenamiento`, `X_prueba`, objetivos de regresión y clasificación, y una partición reproducible.

**Verificación:**

```python
assert set(X_entrenamiento.columns) == set(X_prueba.columns)
assert len(df_entrenamiento) + len(df_prueba) == len(df_modelo)
assert set(y_clf_entrenamiento.unique()).issubset({0, 1})
```

### Paso 13: Entrenar y evaluar modelos supervisados mínimos

**Objetivo:** Aplicar regresión lineal y logística como comprobación de que el conjunto preparado puede ser modelado.

**Tiempo estimado:** 6 minutos.

**Instrucciones:**

1. Entrene una regresión lineal para predecir ventas.
2. Entrene una regresión logística para clasificar ventas altas.
3. Calcule métricas de entrenamiento y prueba.
4. Interprete diferencias grandes como una señal que requiere revisión, no como una conclusión definitiva sobre sobreajuste.
5. Guarde las métricas obtenidas.

```python
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.metrics import accuracy_score, r2_score

if y_clf_entrenamiento.nunique() < 2:
    raise ValueError(
        "La partición de entrenamiento contiene una sola clase. "
        "Revise el tamaño y distribución del conjunto."
    )

modelo_regresion = LinearRegression()
modelo_regresion.fit(X_entrenamiento, y_reg_entrenamiento)

modelo_logistico = LogisticRegression(
    random_state=RANDOM_STATE_CURSO,
    max_iter=1000
)
modelo_logistico.fit(X_entrenamiento, y_clf_entrenamiento)

pred_reg_entrenamiento = modelo_regresion.predict(X_entrenamiento)
pred_reg_prueba = modelo_regresion.predict(X_prueba)

pred_clf_entrenamiento = modelo_logistico.predict(X_entrenamiento)
pred_clf_prueba = modelo_logistico.predict(X_prueba)

metricas_modelos = pd.DataFrame({
    "metrica": [
        "r2_regresion_entrenamiento",
        "r2_regresion_prueba",
        "accuracy_logistica_entrenamiento",
        "accuracy_logistica_prueba",
    ],
    "valor": [
        r2_score(y_reg_entrenamiento, pred_reg_entrenamiento),
        r2_score(y_reg_prueba, pred_reg_prueba),
        accuracy_score(y_clf_entrenamiento, pred_clf_entrenamiento),
        accuracy_score(y_clf_prueba, pred_clf_prueba),
    ],
})

display(metricas_modelos)

metricas_modelos.to_csv(
    RUTA_SALIDAS / "metricas_modelos_base.csv",
    index=False,
    encoding="utf-8"
)
```

**Salida esperada:** Se generan métricas de regresión y clasificación para entrenamiento y prueba.

**Verificación:**

```python
assert metricas_modelos["valor"].notna().all()
assert (RUTA_SALIDAS / "metricas_modelos_base.csv").exists()
```

### Paso 14: Persistir resultados y registrar evidencia

**Objetivo:** Guardar el DataFrame analítico y las particiones en formatos reutilizables.

**Tiempo estimado:** 6 minutos.

**Instrucciones:**

1. Cree una variable de partición en el DataFrame final.
2. Guarde el conjunto analítico en CSV y Parquet.
3. Guarde las particiones entrenamiento y prueba en Parquet.
4. Exporte los principales resúmenes.
5. Registre una evidencia mínima de los archivos generados.

```python
df_analitico = df_modelo.copy(deep=True)
df_analitico["particion"] = np.where(
    df_analitico.index.isin(indices_entrenamiento),
    "entrenamiento",
    "prueba"
)

ruta_csv = RUTA_SALIDAS / "datos_analiticos_preparados.csv"
ruta_parquet = RUTA_SALIDAS / "datos_analiticos_preparados.parquet"

df_analitico.to_csv(ruta_csv, index=False, encoding="utf-8")
df_analitico.to_parquet(ruta_parquet, index=False, engine="pyarrow")

df_entrenamiento.to_parquet(
    RUTA_SALIDAS / "entrenamiento.parquet",
    index=False,
    engine="pyarrow"
)

df_prueba.to_parquet(
    RUTA_SALIDAS / "prueba.parquet",
    index=False,
    engine="pyarrow"
)

resumen_grupos.to_csv(
    RUTA_SALIDAS / "resumen_groupby.csv",
    index=False,
    encoding="utf-8"
)

tabla_cruzada.to_csv(
    RUTA_SALIDAS / "tabla_cruzada.csv",
    encoding="utf-8"
)

evidencia_archivos = pd.DataFrame({
    "archivo": [
        ruta_csv.name,
        ruta_parquet.name,
        "entrenamiento.parquet",
        "prueba.parquet",
        "resumen_groupby.csv",
        "tabla_cruzada.csv",
        "metricas_modelos_base.csv",
    ],
    "existe": [
        (RUTA_SALIDAS / ruta_csv.name).exists(),
        (RUTA_SALIDAS / ruta_parquet.name).exists(),
        (RUTA_SALIDAS / "entrenamiento.parquet").exists(),
        (RUTA_SALIDAS / "prueba.parquet").exists(),
        (RUTA_SALIDAS / "resumen_groupby.csv").exists(),
        (RUTA_SALIDAS / "tabla_cruzada.csv").exists(),
        (RUTA_SALIDAS / "metricas_modelos_base.csv").exists(),
    ],
})

display(evidencia_archivos)

evidencia_archivos.to_csv(
    RUTA_EVIDENCIA / "archivos_generados.csv",
    index=False,
    encoding="utf-8"
)
```

**Salida esperada:** Se generan archivos CSV y Parquet reutilizables para el laboratorio 04-00-02.

**Verificación:**

```python
assert evidencia_archivos["existe"].all()
assert pd.read_parquet(ruta_parquet, engine="pyarrow").shape == df_analitico.shape
```

## Validación y Pruebas

Ejecute esta celda final después de completar todos los pasos. La validación es medible: comprueba dimensiones, nulos, duplicados, conservación de totales, integridad de la partición y existencia de archivos.

```python
resultados_validacion = {
    "origen_no_modificado_en_memoria": df_origen.equals(df_trabajo),
    "ventas_sin_nulos": df_analitico["ventas"].notna().all(),
    "inversion_sin_nulos": df_analitico["inversion_publicitaria"].notna().all(),
    "ventas_positivas": (df_analitico["ventas"] > 0).all(),
    "inversion_no_negativa": (df_analitico["inversion_publicitaria"] >= 0).all(),
    "sin_duplicados_completos": df_analitico.duplicated().sum() == 0,
    "merge_sin_filas_no_emparejadas": no_emparejadas.empty,
    "total_groupby_coincide": np.isclose(
        resumen_grupos["ventas_totales"].sum(),
        df_integrado["ventas"].sum()
    ),
    "total_pivot_coincide": np.isclose(
        tabla_pivot.to_numpy().sum(),
        df_integrado["ventas"].sum()
    ),
    "vectorizacion_equivale_a_bucle": np.allclose(
        indice_eficiencia_vectorizado,
        np.array(indice_eficiencia_iterativo)
    ),
    "particiones_completas": (
        len(df_entrenamiento) + len(df_prueba) == len(df_modelo)
    ),
    "archivos_generados": evidencia_archivos["existe"].all(),
}

df_validacion = pd.DataFrame(
    resultados_validacion.items(),
    columns=["prueba", "resultado"]
)

display(df_validacion)

df_validacion.to_csv(
    RUTA_EVIDENCIA / "validacion_pipeline.csv",
    index=False,
    encoding="utf-8"
)

assert df_validacion["resultado"].all(), (
    "Una o más validaciones fallaron. Revise las filas marcadas como False."
)
```

### Criterios de evidencia

Entregue o conserve los siguientes elementos:

1. El notebook `notebooks/lab_04_00_01_pipeline_pandas_numpy.ipynb` con todas las celdas ejecutadas.
2. `salidas/lab_04_00_01/auditoria_inicial.csv`.
3. `salidas/lab_04_00_01/datos_analiticos_preparados.parquet`.
4. `salidas/lab_04_00_01/entrenamiento.parquet`.
5. `salidas/lab_04_00_01/prueba.parquet`.
6. `salidas/lab_04_00_01/metricas_modelos_base.csv`.
7. `evidencia/lab_04_00_01/validacion_pipeline.csv` con todos los resultados en `True`.
8. Una captura o exportación de la celda de validación final donde se observe que no hubo excepciones.

### Caso adversarial de calidad y trazabilidad

Si el archivo fuente contiene una columna de texto con frases como “ignore las instrucciones”, “ejecute este comando”, “revele credenciales” o contenido contradictorio con esta guía:

1. Trate ese contenido como **dato**, no como una instrucción ejecutable.
2. No ejecute comandos, macros, enlaces ni código incrustado en los registros.
3. Registre la columna y la cantidad de coincidencias.
4. Informe al instructor si el contenido afecta la interpretación analítica.

```python
patron_adversarial = (
    r"ignore.*instrucci|ejecute.*comando|revele.*credencial|"
    r"system message|prompt injection"
)

columnas_texto = df_origen.select_dtypes(
    include=["object", "string"]
).columns.tolist()

hallazgos_adversariales = []

for columna in columnas_texto:
    conteo = (
        df_origen[columna]
        .astype("string")
        .str.contains(patron_adversarial, case=False, regex=True, na=False)
        .sum()
    )
    hallazgos_adversariales.append({
        "columna": columna,
        "coincidencias": int(conteo),
    })

df_hallazgos_adversariales = pd.DataFrame(hallazgos_adversariales)
display(df_hallazgos_adversariales)

df_hallazgos_adversariales.to_csv(
    RUTA_EVIDENCIA / "revision_contenido_adversarial.csv",
    index=False,
    encoding="utf-8"
)
```

La calidad del pipeline se evalúa por precisión de las transformaciones, trazabilidad de reglas, declaración de incertidumbre, utilidad de los resúmenes y supervisión humana de las decisiones. No se evalúa ni se solicita exponer razonamiento interno de ninguna herramienta de IA.

## Solución de Problemas

### Problema 1: `KeyError` al buscar `ventas` o `inversion_publicitaria`

**Síntomas:** El paso 5 muestra un error similar a `KeyError: Faltan variables requeridas`.

**Causa:** Los nombres de columnas del archivo reutilizado no coinciden con las equivalencias previstas, o se seleccionó un archivo distinto al utilizado en 03-00-02.

**Solución:**

1. Ejecute `print(df_trabajo.columns.tolist())`.
2. Compare los nombres con la documentación y notebook de 03-00-02.
3. Agregue únicamente la equivalencia necesaria al diccionario `equivalencias`.
4. Vuelva a ejecutar desde el paso 5.
5. No regenere ni sustituya el conjunto de datos original.

Ejemplo si la columna original se llama `monto_ventas`:

```python
equivalencias["monto_ventas"] = "ventas"
```

### Problema 2: Error al guardar o leer Parquet con PyArrow

**Síntomas:** Aparece un error como `ImportError`, `ArrowInvalid` o falla `to_parquet()`.

**Causa:** PyArrow no está instalado en el entorno virtual correcto, tiene una versión distinta de 20.0.0, o el kernel de Jupyter no corresponde a `.venv`.

**Solución:**

1. Verifique el intérprete con `print(sys.executable)`.
2. Compruebe `print(pa.__version__)`.
3. Active `.venv` en la terminal.
4. Instale o alinee PyArrow exclusivamente dentro de `.venv`.
5. Reinicie el kernel y ejecute nuevamente el notebook desde el paso 2.

**Windows PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
python -m pip install pyarrow==20.0.0
```

**macOS o Linux:**

```bash
source .venv/bin/activate
python -m pip install pyarrow==20.0.0
```

## Limpieza

Conserve los resultados analíticos requeridos para el laboratorio 04-00-02. Elimine únicamente archivos temporales creados por error o pruebas que no formen parte de la evidencia.

1. Cierre JupyterLab o Visual Studio Code después de guardar el notebook.
2. Mantenga el entorno virtual `.venv`.
3. Mantenga los archivos Parquet, CSV de auditoría, métricas y evidencia.
4. No elimine ni modifique archivos de `salidas/lab_03_00_02`.
5. Si creó archivos de prueba fuera de las carpetas del laboratorio, elimínelos manualmente.

Ejemplo para listar los artefactos finales:

```python
for archivo in sorted(RUTA_SALIDAS.iterdir()):
    print(archivo.name)
```

## Resumen

En este laboratorio construyó un pipeline reproducible de preparación analítica con Pandas y NumPy. Partió del conjunto reutilizado de 03-00-02, registró su estado inicial, aplicó selección con `.loc` e `.iloc`, definió reglas de limpieza, normalizó tipos y categorías, y validó nulos, duplicados y rangos.

También creó una dimensión auxiliar en memoria, verificó combinaciones con `merge` y `join`, reestructuró datos con `pivot`, `pivot_table` y `melt`, y generó resúmenes con `groupby` y `crosstab`. Finalmente aplicó vectorización de NumPy, preparó variables codificadas, creó particiones de entrenamiento y prueba, y entrenó modelos base de regresión lineal y logística.

El archivo principal para continuar en el laboratorio 04-00-02 es:

```text
salidas/lab_04_00_01/datos_analiticos_preparados.parquet
```

La interpretación de métricas de regresión, clasificación, accuracy, sobreajuste, subajuste y sesgo-varianza debe profundizarse en el laboratorio siguiente; las métricas obtenidas aquí son una línea base reproducible, no una validación de producción.

---

# Práctica: Entrenar regresión lineal y logística, evaluar con datos de prueba e identificar sobreajuste y sesgo

## Metadatos

| Campo | Valor |
|---|---|
| Duración | 180 minutos |
| Complejidad | Difícil |
| Nivel de Bloom | Aplicar |

## Descripción General

En este laboratorio reutilizará exclusivamente el `DataFrame` analítico preparado en el laboratorio 04-00-01 para construir dos modelos supervisados: una regresión lineal para una variable numérica de ventas y una regresión logística para el objetivo binario creado en 03-00-02. Preparará variables, separará datos de entrenamiento y prueba de manera reproducible, entrenará modelos y analizará sus coeficientes, predicciones y métricas.

La actividad final compara una especificación simple con una más compleja sin modificar el conjunto de prueba. El cierre documenta evidencia de sobreajuste, subajuste, sesgo de representación, limitaciones muestrales y acciones de mejora que requerirían datos adicionales.

## Objetivos de Aprendizaje

- [ ] Preparar matrices de características y objetivos evitando fuga de información posterior al resultado.
- [ ] Dividir observaciones en conjuntos de entrenamiento y prueba con `train_test_split` y una semilla reproducible.
- [ ] Entrenar e interpretar una regresión lineal y una regresión logística con scikit-learn.
- [ ] Evaluar modelos con métricas de regresión, `accuracy`, errores, probabilidades y comparación entre entrenamiento y prueba.
- [ ] Identificar evidencia técnica de sobreajuste, subajuste, sesgo de muestreo y límites de generalización.

## Prerrequisitos

**Conocimientos requeridos**

- Uso de `Series`, `DataFrame`, `.loc`, `.iloc`, filtros booleanos, `dropna`, `fillna`, `replace` y `sort_values`.
- Integración y reestructuración de datos con `merge`, `groupby`, `pivot`, `melt` y `crosstab`.
- Fundamentos de arrays NumPy, vectorización, agregaciones y broadcasting.
- Comprensión básica de regresión lineal, regresión logística, probabilidades, correlación, entrenamiento y prueba.
- Haber completado el laboratorio 04-00-01 y conservado su salida analítica documentada.

**Acceso requerido**

- Acceso al directorio raíz del proyecto utilizado en los laboratorios anteriores.
- Entorno virtual `.venv` creado por el estudiante.
- Archivo analítico producido o identificado en `04-00-01`, en formato CSV o Parquet.
- JupyterLab o Visual Studio Code configurado para usar el intérprete de `.venv`.
- No se requiere una conexión a Internet durante la ejecución, salvo que el instructor autorice consultar documentación oficial.

> **Protección de datos:** use exclusivamente datos sintéticos o anonimizados. No incorpore PII real, credenciales, tokens, clientes reales ni archivos no autorizados.

## Entorno de Laboratorio

### Recursos de hardware

| Recurso | Mínimo | Recomendado |
|---|---:|---:|
| Arquitectura | Equipo de 64 bits, dos núcleos | Equipo de 64 bits, cuatro núcleos o superior |
| Memoria RAM | 8 GB | 16 GB |
| Espacio libre | 5 GB | 10 GB |
| Pantalla | 1366×768 | 1920×1080 |

### Software y versiones controladas

| Tecnología | Versión exacta | Edición o arquitectura | Fuente oficial |
|---|---:|---|---|
| Python | 3.12.11 | CPython, 64 bits | https://www.python.org/downloads/release/python-31211/ |
| pip | 24.3.1 | Paquete Python | https://pypi.org/project/pip/24.3.1/ |
| JupyterLab | 4.3.4 | Paquete Python | https://pypi.org/project/jupyterlab/4.3.4/ |
| Visual Studio Code | 1.96.2 | Windows/macOS/Linux, 64 bits | https://code.visualstudio.com/updates/v1_96 |
| NumPy | 2.2.6 | Paquete Python | https://pypi.org/project/numpy/2.2.6/ |
| pandas | 3.0.0 | Paquete Python | https://pypi.org/project/pandas/3.0.0/ |
| PyArrow | 20.0.0 | Paquete Python | https://pypi.org/project/pyarrow/20.0.0/ |
| scikit-learn | 1.7.0 | Paquete Python | https://pypi.org/project/scikit-learn/1.7.0/ |
| Matplotlib | 3.10.3 | Paquete Python | https://pypi.org/project/matplotlib/3.10.3/ |
| Seaborn | 0.13.2 | Paquete Python | https://pypi.org/project/seaborn/0.13.2/ |
| Plotly | 5.24.1 | Paquete Python | https://pypi.org/project/plotly/5.24.1/ |
| SQLAlchemy | 2.0.36 | Paquete Python | https://pypi.org/project/SQLAlchemy/2.0.36/ |

> Este laboratorio utiliza directamente pandas, NumPy, PyArrow, scikit-learn, Matplotlib y Seaborn. Plotly y SQLAlchemy permanecen registrados como parte del entorno del curso, pero no son necesarios para completar esta práctica.

### Activación del entorno

Ejecute únicamente el comando correspondiente a su sistema operativo desde la raíz del proyecto.

**macOS o Linux**

```bash
source .venv/bin/activate
```

**Windows PowerShell**

```powershell
.\.venv\Scripts\Activate.ps1
```

Verifique las versiones instaladas. No instale paquetes fuera de `.venv`.

```bash
python --version
python -m pip --version
python -m pip show pandas numpy scikit-learn matplotlib seaborn pyarrow
```

Inicie JupyterLab si trabajará con un cuaderno.

```bash
jupyter lab
```

## Instrucciones Paso a Paso

### Paso 1: Crear el cuaderno y registrar el contexto de trabajo

**Objetivo:** Crear el cuaderno del laboratorio, seleccionar el kernel correcto y registrar el alcance reproducible de la práctica.

**Tiempo estimado:** 10 minutos.

**Instrucciones:**

1. Abra JupyterLab o Visual Studio Code desde la raíz del proyecto.
2. Seleccione el kernel asociado al entorno `.venv`.
3. Cree el cuaderno `lab_04_00_02_modelos.ipynb`.
4. Inserte una celda Markdown con su nombre, fecha, identificador del laboratorio y la frase exacta: `Datos sintéticos; no contiene PII real.`
5. Registre que el archivo analítico de entrada procede del laboratorio 04-00-01 y no será reemplazado por un conjunto creado desde cero.

**Salida esperada:** Existe el cuaderno del laboratorio y el kernel seleccionado corresponde al entorno virtual del estudiante.

**Verificación:** La ruta mostrada por `sys.executable` contiene `.venv`.

```python
import sys
from pathlib import Path

print("Intérprete:", sys.executable)
print("Directorio de trabajo:", Path.cwd())
assert ".venv" in sys.executable, "Seleccione el kernel asociado a .venv."
```

### Paso 2: Importar bibliotecas y verificar versiones

**Objetivo:** Confirmar que el entorno contiene las versiones requeridas antes de cargar datos o entrenar modelos.

**Tiempo estimado:** 12 minutos.

**Instrucciones:**

1. Inserte una nueva celda de código.
2. Importe las bibliotecas requeridas.
3. Ejecute la celda y compare las versiones con la tabla del entorno.
4. Si una versión crítica no coincide, documente la diferencia y solicite orientación al instructor antes de continuar.
5. No modifique el intérprete global ni instale paquetes fuera de `.venv`.

**Salida esperada:** Se muestran las versiones de Python, pandas, NumPy, scikit-learn, Matplotlib, Seaborn y PyArrow.

**Verificación:** No se producen errores de importación y las bibliotecas se cargan desde el entorno virtual.

```python
import platform
import numpy as np
import pandas as pd
import pyarrow
import sklearn
import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns

from pathlib import Path

print("Python:", platform.python_version())
print("pandas:", pd.__version__)
print("NumPy:", np.__version__)
print("PyArrow:", pyarrow.__version__)
print("scikit-learn:", sklearn.__version__)
print("Matplotlib:", matplotlib.__version__)
print("Seaborn:", sns.__version__)
```

### Paso 3: Configurar rutas, semilla y directorios de salida

**Objetivo:** Definir rutas y una semilla reproducible sin alterar la estructura creada en laboratorios anteriores.

**Tiempo estimado:** 15 minutos.

**Instrucciones:**

1. Mantenga el valor global `RANDOM_STATE_CURSO = 3102`.
2. Cree únicamente los directorios nuevos del laboratorio actual.
3. No elimine ni reemplace `salidas/lab_04_00_01`.
4. Utilice `salidas/lab_04_00_02` para tablas, modelos de evaluación y gráficos.
5. Utilice `evidencia/lab_04_00_02` para evidencia solicitada por el instructor.

**Salida esperada:** Existen los directorios de salida y evidencia del laboratorio 04-00-02.

**Verificación:** Las rutas se imprimen y están disponibles para escritura.

```python
RANDOM_STATE_CURSO = 3102

RAIZ_PROYECTO = Path.cwd()
RUTA_SALIDAS = RAIZ_PROYECTO / "salidas" / "lab_04_00_02"
RUTA_EVIDENCIA = RAIZ_PROYECTO / "evidencia" / "lab_04_00_02"

RUTA_SALIDAS.mkdir(parents=True, exist_ok=True)
RUTA_EVIDENCIA.mkdir(parents=True, exist_ok=True)

print("Salidas:", RUTA_SALIDAS.resolve())
print("Evidencia:", RUTA_EVIDENCIA.resolve())
```

### Paso 4: Localizar y cargar el DataFrame analítico de 04-00-01

**Objetivo:** Reutilizar el archivo analítico preparado en el laboratorio anterior sin crear otro conjunto sintético ni usar fuentes externas.

**Tiempo estimado:** 15 minutos.

**Instrucciones:**

1. Ejecute la búsqueda de archivos CSV y Parquet fuera de `.venv`.
2. Identifique el archivo de salida analítica documentado en 04-00-01.
3. Excluya auditorías, tablas resumidas y archivos de evidencia que no contengan observaciones modelables.
4. Asigne manualmente la ruta correcta a `RUTA_DF_ANALITICO`.
5. Cargue el archivo según su extensión y conserve una copia profunda en memoria.

**Salida esperada:** Se muestra la ruta seleccionada, la dimensión del DataFrame y las primeras filas.

**Verificación:** El DataFrame tiene al menos una fila, al menos una columna y procede de un archivo CSV o Parquet existente.

```python
archivos_disponibles = sorted(
    ruta for ruta in RAIZ_PROYECTO.rglob("*")
    if ruta.suffix.lower() in {".csv", ".parquet"}
    and ".venv" not in ruta.parts
    and "lab_04_00_02" not in ruta.parts
)

for archivo in archivos_disponibles:
    print(archivo.relative_to(RAIZ_PROYECTO))
```

> En la siguiente celda, reemplace el ejemplo por la ruta exacta documentada en el laboratorio 04-00-01.

```python
## Ejemplo: ajuste esta ruta al artefacto analítico real de 04-00-01.
RUTA_DF_ANALITICO = RAIZ_PROYECTO / "salidas" / "lab_04_00_01" / "df_analitico.parquet"

assert RUTA_DF_ANALITICO.exists(), (
    "La ruta no existe. Sustituya RUTA_DF_ANALITICO por el archivo analítico "
    "identificado en el laboratorio 04-00-01."
)

if RUTA_DF_ANALITICO.suffix.lower() == ".parquet":
    df_analitico = pd.read_parquet(RUTA_DF_ANALITICO)
elif RUTA_DF_ANALITICO.suffix.lower() == ".csv":
    df_analitico = pd.read_csv(RUTA_DF_ANALITICO)
else:
    raise ValueError("El archivo analítico debe tener extensión .csv o .parquet.")

df_origen = df_analitico.copy(deep=True)

print("Archivo cargado:", RUTA_DF_ANALITICO)
print("Dimensión:", df_analitico.shape)
display(df_analitico.head())
```

### Paso 5: Inspeccionar estructura, calidad y posibles variables de fuga

**Objetivo:** Identificar objetivos, variables predictoras disponibles y columnas que no deben utilizarse por contener información posterior al resultado o filtrar indebidamente la respuesta.

**Tiempo estimado:** 18 minutos.

**Instrucciones:**

1. Revise nombres, tipos y valores nulos de las columnas.
2. Use `.loc` y `.iloc` para inspeccionar una muestra de filas y columnas.
3. Identifique la variable numérica de ventas y el objetivo binario creado en 03-00-02.
4. Excluya identificadores únicos, fechas posteriores al resultado, importes cobrados después de la venta, indicadores derivados del objetivo y columnas de texto libre no justificadas.
5. Documente en una celda Markdown la razón para incluir o excluir cada variable predictora.
6. No use una columna porque presente una correlación alta si dicha columna fue calculada después de conocer el resultado.

**Salida esperada:** Una tabla de auditoría de columnas y una lista documentada de variables permitidas y excluidas.

**Verificación:** La auditoría queda guardada en `salidas/lab_04_00_02/auditoria_modelado.csv`.

```python
auditoria_modelado = pd.DataFrame({
    "columna": df_analitico.columns,
    "tipo": df_analitico.dtypes.astype(str).to_numpy(),
    "nulos": df_analitico.isna().sum().to_numpy(),
    "porcentaje_nulos": (df_analitico.isna().mean() * 100).round(2).to_numpy(),
    "valores_unicos": df_analitico.nunique(dropna=False).to_numpy()
}).sort_values(["porcentaje_nulos", "columna"], ascending=[False, True])

auditoria_modelado.to_csv(
    RUTA_SALIDAS / "auditoria_modelado.csv",
    index=False,
    encoding="utf-8"
)

display(auditoria_modelado)
```

```python
print("Muestra con .loc:")
display(df_analitico.loc[df_analitico.index[:5], :].copy())

print("Muestra con .iloc:")
display(df_analitico.iloc[:5, :min(8, df_analitico.shape[1])].copy())
```

> Registre una decisión similar a la siguiente, adaptada a sus nombres reales:
>
> - `ventas`: objetivo numérico permitido.
> - `objetivo_binario`: objetivo de clasificación permitido.
> - `inversion_publicitaria`, `canal`, `region`: variables disponibles antes del resultado y permitidas.
> - `id_venta`: excluida por ser identificador.
> - `importe_cobrado_final`: excluida si fue conocido después de ocurrir la venta.
> - `ventas_categoria_alta`: excluida de predictores si se derivó directamente de `ventas`.

### Paso 6: Definir columnas analíticas y validar objetivos

**Objetivo:** Establecer de forma explícita los objetivos y las variables predictoras disponibles antes del resultado.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Reemplace los nombres de ejemplo por las columnas reales del DataFrame analítico.
2. Seleccione una variable de objetivo numérico para regresión lineal.
3. Seleccione el objetivo binario creado en 03-00-02 para regresión logística.
4. Seleccione una o más variables numéricas predictoras disponibles antes del resultado.
5. Seleccione variables categóricas solo si existían antes del resultado y son pertinentes.
6. Cree una columna técnica `id_fila_origen` para mantener trazabilidad; no la use como predictor.
7. Verifique que el objetivo binario contiene únicamente `0` y `1`.

**Salida esperada:** Variables de configuración definidas y una tabla con distribución de ambos objetivos.

**Verificación:** El objetivo numérico no contiene nulos tras la limpieza mínima; el objetivo binario contiene exactamente dos clases.

```python
## Sustituya estos nombres por los nombres reales documentados en 04-00-01.
COLUMNA_OBJETIVO_REGRESION = "ventas"
COLUMNA_OBJETIVO_CLASIFICACION = "objetivo_binario"

FEATURES_NUMERICAS = [
    "inversion_publicitaria",
]

FEATURES_CATEGORICAS = [
    "canal",
    "region",
]

COLUMNAS_REQUERIDAS = (
    [COLUMNA_OBJETIVO_REGRESION, COLUMNA_OBJETIVO_CLASIFICACION]
    + FEATURES_NUMERICAS
    + FEATURES_CATEGORICAS
)

faltantes = sorted(set(COLUMNAS_REQUERIDAS) - set(df_analitico.columns))
assert not faltantes, (
    f"Estas columnas no existen: {faltantes}. "
    "Corrija los nombres usando la auditoría del paso anterior."
)

assert FEATURES_NUMERICAS, "Seleccione al menos una característica numérica."
```

```python
df_modelo = df_analitico.loc[:, COLUMNAS_REQUERIDAS].copy()
df_modelo.insert(0, "id_fila_origen", df_analitico.index)

df_modelo[COLUMNA_OBJETIVO_REGRESION] = pd.to_numeric(
    df_modelo[COLUMNA_OBJETIVO_REGRESION],
    errors="coerce"
)

valores_binarios = set(
    df_modelo[COLUMNA_OBJETIVO_CLASIFICACION]
    .dropna()
    .astype(int)
    .unique()
)

assert valores_binarios == {0, 1}, (
    "El objetivo de clasificación debe contener 0 y 1. "
    "Revise la columna creada en 03-00-02 antes de continuar."
)

display(df_modelo[[COLUMNA_OBJETIVO_REGRESION, COLUMNA_OBJETIVO_CLASIFICACION]].describe())
print(df_modelo[COLUMNA_OBJETIVO_CLASIFICACION].value_counts(dropna=False).sort_index())
```

### Paso 7: Limpiar datos de modelado y explorar representación

**Objetivo:** Aplicar una limpieza mínima, medir registros excluidos por objetivos faltantes y evaluar la representación de clases y categorías.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Cuente filas antes de eliminar objetivos faltantes.
2. Elimine únicamente las filas con objetivo de regresión o clasificación faltante.
3. No rellene el objetivo de regresión ni el objetivo binario.
4. Conserve los nulos de las características para tratarlos dentro del flujo de entrenamiento.
5. Use `groupby`, `crosstab`, `pivot` y `melt` para producir evidencia de representación por categoría.
6. Guarde las tablas de representación como CSV.
7. Explique si alguna clase o categoría tiene muy pocas observaciones.

**Salida esperada:** Un DataFrame modelable, una tabla cruzada y una tabla larga de representación.

**Verificación:** Se informa el número de filas eliminado; las características no se imputan todavía con información de todo el conjunto.

```python
filas_antes = len(df_modelo)

df_modelo = df_modelo.dropna(
    subset=[COLUMNA_OBJETIVO_REGRESION, COLUMNA_OBJETIVO_CLASIFICACION]
).copy()

filas_despues = len(df_modelo)
filas_eliminadas = filas_antes - filas_despues

print("Filas antes:", filas_antes)
print("Filas después:", filas_despues)
print("Filas eliminadas por objetivo faltante:", filas_eliminadas)

assert len(df_modelo) >= 20, (
    "Hay menos de 20 observaciones modelables. Documente la limitación "
    "y consulte al instructor antes de interpretar resultados."
)
```

```python
if FEATURES_CATEGORICAS:
    categoria_revision = FEATURES_CATEGORICAS[0]

    tabla_clases = pd.crosstab(
        df_modelo[categoria_revision].fillna("Sin dato"),
        df_modelo[COLUMNA_OBJETIVO_CLASIFICACION],
        margins=True
    )

    tabla_clases.to_csv(
        RUTA_SALIDAS / "tabla_cruzada_categoria_clase.csv",
        encoding="utf-8"
    )

    tabla_ancha = (
        df_modelo
        .assign(**{categoria_revision: df_modelo[categoria_revision].fillna("Sin dato")})
        .groupby([categoria_revision, COLUMNA_OBJETIVO_CLASIFICACION], dropna=False)
        .size()
        .reset_index(name="observaciones")
        .pivot(
            index=categoria_revision,
            columns=COLUMNA_OBJETIVO_CLASIFICACION,
            values="observaciones"
        )
        .fillna(0)
        .reset_index()
    )

    tabla_larga = tabla_ancha.melt(
        id_vars=categoria_revision,
        var_name="clase",
        value_name="observaciones"
    )

    tabla_larga.to_csv(
        RUTA_SALIDAS / "representacion_categoria_clase_largo.csv",
        index=False,
        encoding="utf-8"
    )

    display(tabla_clases)
    display(tabla_larga.sort_values("observaciones", ascending=False))
else:
    print("No se definieron características categóricas. Documente esta decisión.")
```

### Paso 8: Separar entrenamiento y prueba de manera reproducible

**Objetivo:** Crear una única partición reproducible para ambos problemas y evitar evaluar exclusivamente con datos de entrenamiento.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Reserve el 25 % de las observaciones para prueba.
2. Use `RANDOM_STATE_CURSO`.
3. Estratifique por el objetivo binario para preservar, en lo posible, la proporción de clases.
4. Use exactamente los mismos índices de entrenamiento y prueba para regresión y clasificación.
5. Guarde los identificadores de filas asignados a entrenamiento y prueba.
6. No vuelva a generar esta partición para obtener métricas más favorables.

**Salida esperada:** Conjuntos de entrenamiento y prueba con identificadores trazables.

**Verificación:** Los conjuntos no comparten índices y sus tamaños suman el total de filas modelables.

```python
from sklearn.model_selection import train_test_split

indices_entrenamiento, indices_prueba = train_test_split(
    df_modelo.index,
    test_size=0.25,
    random_state=RANDOM_STATE_CURSO,
    stratify=df_modelo[COLUMNA_OBJETIVO_CLASIFICACION]
)

df_train = df_modelo.loc[indices_entrenamiento].copy()
df_test = df_modelo.loc[indices_prueba].copy()

assert set(df_train.index).isdisjoint(set(df_test.index))
assert len(df_train) + len(df_test) == len(df_modelo)

particion = pd.concat([
    df_train[["id_fila_origen"]].assign(conjunto="entrenamiento"),
    df_test[["id_fila_origen"]].assign(conjunto="prueba")
]).sort_values("id_fila_origen")

particion.to_csv(
    RUTA_SALIDAS / "particion_entrenamiento_prueba.csv",
    index=False,
    encoding="utf-8"
)

print("Entrenamiento:", df_train.shape)
print("Prueba:", df_test.shape)
print("\nProporción de clase en entrenamiento:")
print(df_train[COLUMNA_OBJETIVO_CLASIFICACION].value_counts(normalize=True).sort_index())
print("\nProporción de clase en prueba:")
print(df_test[COLUMNA_OBJETIVO_CLASIFICACION].value_counts(normalize=True).sort_index())
```

### Paso 9: Preparar especificaciones simple y compleja

**Objetivo:** Definir una comparación controlada entre modelos con distinta cantidad de información disponible antes del resultado.

**Tiempo estimado:** 18 minutos.

**Instrucciones:**

1. Defina una especificación simple usando únicamente la primera variable numérica predictora.
2. Defina una especificación compleja con todas las variables numéricas permitidas y las categóricas permitidas.
3. Impute valores numéricos con la mediana aprendida solo en entrenamiento.
4. Impute categorías faltantes con la categoría más frecuente aprendida solo en entrenamiento.
5. Codifique categorías mediante `OneHotEncoder`.
6. Use `drop="first"` para establecer una categoría de referencia en cada variable categórica.
7. Documente que la especificación compleja fue definida antes de consultar las métricas de prueba.

**Salida esperada:** Dos preprocesadores reutilizables: uno simple y uno complejo.

**Verificación:** La preparación se ajusta dentro de cada `Pipeline`; no se calcula la mediana ni las categorías usando el conjunto de prueba.

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder

FEATURES_SIMPLES = [FEATURES_NUMERICAS[0]]
FEATURES_COMPLEJAS_NUMERICAS = FEATURES_NUMERICAS.copy()
FEATURES_COMPLEJAS_CATEGORICAS = FEATURES_CATEGORICAS.copy()

preprocesador_simple = ColumnTransformer(
    transformers=[
        (
            "numericas",
            Pipeline([
                ("imputacion", SimpleImputer(strategy="median"))
            ]),
            FEATURES_SIMPLES
        )
    ],
    remainder="drop",
    verbose_feature_names_out=False
)

transformadores_complejos = [
    (
        "numericas",
        Pipeline([
            ("imputacion", SimpleImputer(strategy="median"))
        ]),
        FEATURES_COMPLEJAS_NUMERICAS
    )
]

if FEATURES_COMPLEJAS_CATEGORICAS:
    transformadores_complejos.append(
        (
            "categoricas",
            Pipeline([
                ("imputacion", SimpleImputer(strategy="most_frequent")),
                ("codificacion", OneHotEncoder(
                    handle_unknown="ignore",
                    drop="first",
                    sparse_output=False
                ))
            ]),
            FEATURES_COMPLEJAS_CATEGORICAS
        )
    )

preprocesador_complejo = ColumnTransformer(
    transformers=transformadores_complejos,
    remainder="drop",
    verbose_feature_names_out=False
)
```

### Paso 10: Entrenar y evaluar regresión lineal

**Objetivo:** Ajustar regresiones lineales simple y compleja, comparar errores de entrenamiento y prueba, e interpretar coeficientes según su escala y codificación.

**Tiempo estimado:** 20 minutos.

**Instrucciones:**

1. Prepare `X` e `y` para el objetivo numérico.
2. Entrene una regresión lineal simple y una compleja con los datos de entrenamiento.
3. Obtenga predicciones para entrenamiento y prueba.
4. Calcule MAE, RMSE y R² en ambos conjuntos.
5. Extraiga y ordene coeficientes del modelo complejo.
6. Interprete los coeficientes con cautela: un coeficiente representa un cambio esperado condicionado a las demás variables incluidas.
7. No interprete causalidad ni use el conjunto de prueba para rediseñar la especificación.

**Salida esperada:** Tabla de métricas de regresión, coeficientes y gráfico de valores reales frente a predicciones.

**Verificación:** El RMSE de prueba se calcula sobre filas que no participaron en el ajuste.

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

X_train = df_train[FEATURES_NUMERICAS + FEATURES_CATEGORICAS]
X_test = df_test[FEATURES_NUMERICAS + FEATURES_CATEGORICAS]

y_reg_train = df_train[COLUMNA_OBJETIVO_REGRESION]
y_reg_test = df_test[COLUMNA_OBJETIVO_REGRESION]

modelo_regresion_simple = Pipeline([
    ("preprocesamiento", preprocesador_simple),
    ("modelo", LinearRegression())
])

modelo_regresion_complejo = Pipeline([
    ("preprocesamiento", preprocesador_complejo),
    ("modelo", LinearRegression())
])

modelo_regresion_simple.fit(X_train, y_reg_train)
modelo_regresion_complejo.fit(X_train, y_reg_train)
```

```python
def metricas_regresion(y_real, y_prediccion, modelo, conjunto):
    return {
        "modelo": modelo,
        "conjunto": conjunto,
        "mae": mean_absolute_error(y_real, y_prediccion),
        "rmse": mean_squared_error(y_real, y_prediccion) ** 0.5,
        "r2": r2_score(y_real, y_prediccion)
    }

pred_reg_simple_train = modelo_regresion_simple.predict(X_train)
pred_reg_simple_test = modelo_regresion_simple.predict(X_test)

pred_reg_complejo_train = modelo_regresion_complejo.predict(X_train)
pred_reg_complejo_test = modelo_regresion_complejo.predict(X_test)

metricas_regresion_df = pd.DataFrame([
    metricas_regresion(y_reg_train, pred_reg_simple_train, "lineal_simple", "entrenamiento"),
    metricas_regresion(y_reg_test, pred_reg_simple_test, "lineal_simple", "prueba"),
    metricas_regresion(y_reg_train, pred_reg_complejo_train, "lineal_complejo", "entrenamiento"),
    metricas_regresion(y_reg_test, pred_reg_complejo_test, "lineal_complejo", "prueba"),
]).round(4)

metricas_regresion_df.to_csv(
    RUTA_SALIDAS / "metricas_regresion.csv",
    index=False,
    encoding="utf-8"
)

display(metricas_regresion_df)
```

```python
nombres_features_reg = modelo_regresion_complejo.named_steps[
    "preprocesamiento"
].get_feature_names_out()

coeficientes_regresion = pd.DataFrame({
    "variable": nombres_features_reg,
    "coeficiente": modelo_regresion_complejo.named_steps["modelo"].coef_
}).sort_values("coeficiente", key=np.abs, ascending=False)

coeficientes_regresion.to_csv(
    RUTA_SALIDAS / "coeficientes_regresion_compleja.csv",
    index=False,
    encoding="utf-8"
)

print("Intercepto:", modelo_regresion_complejo.named_steps["modelo"].intercept_)
display(coeficientes_regresion)
```

```python
resultados_regresion_prueba = pd.DataFrame({
    "id_fila_origen": df_test["id_fila_origen"],
    "ventas_reales": y_reg_test,
    "prediccion_simple": pred_reg_simple_test,
    "prediccion_compleja": pred_reg_complejo_test
})

resultados_regresion_prueba["residuo_complejo"] = (
    resultados_regresion_prueba["ventas_reales"]
    - resultados_regresion_prueba["prediccion_compleja"]
)

resultados_regresion_prueba.to_parquet(
    RUTA_SALIDAS / "predicciones_regresion_prueba.parquet",
    index=False
)

plt.figure(figsize=(8, 5))
sns.scatterplot(
    data=resultados_regresion_prueba,
    x="ventas_reales",
    y="prediccion_compleja"
)
limite_inferior = min(
    resultados_regresion_prueba["ventas_reales"].min(),
    resultados_regresion_prueba["prediccion_compleja"].min()
)
limite_superior = max(
    resultados_regresion_prueba["ventas_reales"].max(),
    resultados_regresion_prueba["prediccion_compleja"].max()
)
plt.plot([limite_inferior, limite_superior], [limite_inferior, limite_superior], "--", color="black")
plt.title("Regresión lineal compleja: valores reales y predichos")
plt.xlabel("Ventas reales")
plt.ylabel("Ventas predichas")
plt.tight_layout()
plt.savefig(RUTA_SALIDAS / "grafico_regresion_real_vs_prediccion.png", dpi=150)
plt.show()
```

### Paso 11: Entrenar y evaluar regresión logística

**Objetivo:** Ajustar modelos de regresión logística, revisar predicciones, errores, probabilidades estimadas y coeficientes.

**Tiempo estimado:** 18 minutos.

**Instrucciones:**

1. Prepare el objetivo binario para entrenamiento y prueba.
2. Entrene una regresión logística simple y una compleja.
3. Obtenga predicciones de clase y probabilidades de pertenecer a la clase `1`.
4. Calcule `accuracy` para entrenamiento y prueba.
5. Compare `accuracy` con la proporción de la clase mayoritaria de entrenamiento.
6. Revise filas correctamente clasificadas y filas con error.
7. Interprete coeficientes respecto a la categoría de referencia y a las unidades de las variables numéricas.

**Salida esperada:** Métricas de clasificación, tabla de predicciones con probabilidades y tabla de coeficientes.

**Verificación:** Las probabilidades están entre 0 y 1, y las predicciones contienen únicamente 0 o 1.

```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

y_clas_train = df_train[COLUMNA_OBJETIVO_CLASIFICACION].astype(int)
y_clas_test = df_test[COLUMNA_OBJETIVO_CLASIFICACION].astype(int)

modelo_logistico_simple = Pipeline([
    ("preprocesamiento", preprocesador_simple),
    ("modelo", LogisticRegression(max_iter=2000, random_state=RANDOM_STATE_CURSO))
])

modelo_logistico_complejo = Pipeline([
    ("preprocesamiento", preprocesador_complejo),
    ("modelo", LogisticRegression(max_iter=2000, random_state=RANDOM_STATE_CURSO))
])

modelo_logistico_simple.fit(X_train, y_clas_train)
modelo_logistico_complejo.fit(X_train, y_clas_train)
```

```python
pred_clas_simple_train = modelo_logistico_simple.predict(X_train)
pred_clas_simple_test = modelo_logistico_simple.predict(X_test)

pred_clas_complejo_train = modelo_logistico_complejo.predict(X_train)
pred_clas_complejo_test = modelo_logistico_complejo.predict(X_test)

prob_clas_complejo_test = modelo_logistico_complejo.predict_proba(X_test)[:, 1]

metricas_clasificacion_df = pd.DataFrame([
    {
        "modelo": "logistico_simple",
        "conjunto": "entrenamiento",
        "accuracy": accuracy_score(y_clas_train, pred_clas_simple_train)
    },
    {
        "modelo": "logistico_simple",
        "conjunto": "prueba",
        "accuracy": accuracy_score(y_clas_test, pred_clas_simple_test)
    },
    {
        "modelo": "logistico_complejo",
        "conjunto": "entrenamiento",
        "accuracy": accuracy_score(y_clas_train, pred_clas_complejo_train)
    },
    {
        "modelo": "logistico_complejo",
        "conjunto": "prueba",
        "accuracy": accuracy_score(y_clas_test, pred_clas_complejo_test)
    }
]).round(4)

metricas_clasificacion_df.to_csv(
    RUTA_SALIDAS / "metricas_clasificacion.csv",
    index=False,
    encoding="utf-8"
)

accuracy_base = y_clas_train.value_counts(normalize=True).max()

print("Accuracy de referencia por clase mayoritaria:", round(accuracy_base, 4))
display(metricas_clasificacion_df)
```

```python
resultados_clasificacion_prueba = pd.DataFrame({
    "id_fila_origen": df_test["id_fila_origen"],
    "clase_real": y_clas_test,
    "clase_predicha": pred_clas_complejo_test,
    "probabilidad_clase_1": prob_clas_complejo_test
})

resultados_clasificacion_prueba["error"] = (
    resultados_clasificacion_prueba["clase_real"]
    != resultados_clasificacion_prueba["clase_predicha"]
)

assert resultados_clasificacion_prueba["probabilidad_clase_1"].between(0, 1).all()
assert set(resultados_clasificacion_prueba["clase_predicha"].unique()).issubset({0, 1})

resultados_clasificacion_prueba.to_csv(
    RUTA_SALIDAS / "predicciones_clasificacion_prueba.csv",
    index=False,
    encoding="utf-8"
)

print("Errores de clasificación en prueba:")
display(
    resultados_clasificacion_prueba
    .loc[resultados_clasificacion_prueba["error"]]
    .sort_values("probabilidad_clase_1")
)

print("Tabla cruzada de clase real y predicha:")
display(pd.crosstab(
    resultados_clasificacion_prueba["clase_real"],
    resultados_clasificacion_prueba["clase_predicha"],
    margins=True
))
```

```python
nombres_features_log = modelo_logistico_complejo.named_steps[
    "preprocesamiento"
].get_feature_names_out()

coeficientes_logisticos = pd.DataFrame({
    "variable": nombres_features_log,
    "coeficiente_log_odds": modelo_logistico_complejo.named_steps["modelo"].coef_[0]
})

coeficientes_logisticos["odds_ratio"] = np.exp(
    coeficientes_logisticos["coeficiente_log_odds"]
)

coeficientes_logisticos = coeficientes_logisticos.sort_values(
    "coeficiente_log_odds",
    key=np.abs,
    ascending=False
)

coeficientes_logisticos.to_csv(
    RUTA_SALIDAS / "coeficientes_logisticos_complejos.csv",
    index=False,
    encoding="utf-8"
)

display(coeficientes_logisticos)
```

### Paso 12: Comparar resultados y redactar la reflexión técnica

**Objetivo:** Analizar evidencia de sobreajuste, subajuste, sesgo de representación y limitaciones de generalización sin seleccionar modelos mediante cambios repetidos en prueba.

**Tiempo estimado:** 14 minutos.

**Instrucciones:**

1. Una las métricas de regresión y clasificación en una tabla de comparación.
2. Compare la brecha entre entrenamiento y prueba para cada modelo.
3. Considere evidencia inicial de sobreajuste si el desempeño de entrenamiento es claramente superior al de prueba.
4. Considere evidencia de subajuste si ambos conjuntos muestran desempeño débil frente a una referencia sencilla.
5. Revise si categorías o clases minoritarias tienen pocas observaciones.
6. Redacte una reflexión técnica de 200 a 300 palabras en una celda Markdown.
7. Indique explícitamente que las métricas de prueba no se utilizaron para recrear la partición ni modificar repetidamente la especificación.
8. Guarde la reflexión como archivo Markdown.

**Salida esperada:** Tabla consolidada y reflexión técnica trazable.

**Verificación:** La reflexión menciona regresión, clasificación, representación, tamaño muestral y al menos una acción de mejora que necesite más datos o validación adicional.

```python
comparacion_modelos = {
    "rmse_lineal_simple_train": metricas_regresion_df.loc[
        (metricas_regresion_df["modelo"] == "lineal_simple")
        & (metricas_regresion_df["conjunto"] == "entrenamiento"),
        "rmse"
    ].iloc[0],
    "rmse_lineal_simple_test": metricas_regresion_df.loc[
        (metricas_regresion_df["modelo"] == "lineal_simple")
        & (metricas_regresion_df["conjunto"] == "prueba"),
        "rmse"
    ].iloc[0],
    "rmse_lineal_complejo_train": metricas_regresion_df.loc[
        (metricas_regresion_df["modelo"] == "lineal_complejo")
        & (metricas_regresion_df["conjunto"] == "entrenamiento"),
        "rmse"
    ].iloc[0],
    "rmse_lineal_complejo_test": metricas_regresion_df.loc[
        (metricas_regresion_df["modelo"] == "lineal_complejo")
        & (metricas_regresion_df["conjunto"] == "prueba"),
        "rmse"
    ].iloc[0],
    "accuracy_logistico_simple_train": metricas_clasificacion_df.loc[
        (metricas_clasificacion_df["modelo"] == "logistico_simple")
        & (metricas_clasificacion_df["conjunto"] == "entrenamiento"),
        "accuracy"
    ].iloc[0],
    "accuracy_logistico_simple_test": metricas_clasificacion_df.loc[
        (metricas_clasificacion_df["modelo"] == "logistico_simple")
        & (metricas_clasificacion_df["conjunto"] == "prueba"),
        "accuracy"
    ].iloc[0],
    "accuracy_logistico_complejo_train": metricas_clasificacion_df.loc[
        (metricas_clasificacion_df["modelo"] == "logistico_complejo")
        & (metricas_clasificacion_df["conjunto"] == "entrenamiento"),
        "accuracy"
    ].iloc[0],
    "accuracy_logistico_complejo_test": metricas_clasificacion_df.loc[
        (metricas_clasificacion_df["modelo"] == "logistico_complejo")
        & (metricas_clasificacion_df["conjunto"] == "prueba"),
        "accuracy"
    ].iloc[0],
    "accuracy_referencia_mayoritaria": accuracy_base
}

pd.DataFrame([comparacion_modelos]).to_csv(
    RUTA_SALIDAS / "comparacion_modelos.csv",
    index=False,
    encoding="utf-8"
)

display(pd.DataFrame([comparacion_modelos]))
```

Use la siguiente plantilla y sustituya los corchetes con sus hallazgos observados.

```markdown
### Reflexión técnica

La regresión lineal [simple/compleja] obtuvo un RMSE de entrenamiento de [valor]
y un RMSE de prueba de [valor]. La diferencia [es/no es] consistente con una
posible evidencia de sobreajuste porque [justificación basada en métricas].
El valor R² de prueba fue [valor], por lo que el modelo [explicó/no explicó]
una proporción útil de la variabilidad observada dentro de este conjunto.

La regresión logística compleja obtuvo una accuracy de entrenamiento de [valor]
y una accuracy de prueba de [valor], frente a una referencia de clase mayoritaria
de [valor]. Por ello, el modelo [sí/no] mejora de forma visible la referencia.
Los errores se concentraron principalmente en [categoría o descripción], si esta
conclusión está respaldada por la tabla cruzada y las filas de error.

Existe un posible sesgo de representación porque [clase/categoría] tiene
[número] observaciones, mientras que [otra clase/categoría] tiene [número].
El tamaño muestral limita la generalización porque una única partición puede
producir resultados sensibles a la composición de entrenamiento y prueba.

No se debe afirmar causalidad ni desempeño en producción. Como mejora futura,
se requerirían más datos sintéticos representativos, validación adicional y una
revisión de variables disponibles antes del resultado. La partición de prueba
se mantuvo fija y no se recreó para buscar resultados más favorables.
```

Guarde la reflexión manualmente en el siguiente archivo:

```python
RUTA_REFLEXION = RUTA_SALIDAS / "reflexion_tecnica.md"
print("Guarde su reflexión en:", RUTA_REFLEXION)
```

## Validación y Pruebas

Complete esta lista antes de entregar la evidencia.

| Criterio medible | Evidencia requerida | Resultado esperado |
|---|---|---|
| Entorno correcto | Salida de `sys.executable` y versiones | Intérprete dentro de `.venv`; versiones registradas |
| Fuente trazable | Ruta de `RUTA_DF_ANALITICO` | Archivo procedente de 04-00-01 |
| Integridad de objetivos | Conteos y validación de clases | Objetivo binario con valores `0` y `1` |
| Prevención de fuga | Lista documentada de variables excluidas | No se usan variables posteriores al resultado, objetivos derivados ni identificadores |
| Partición reproducible | `particion_entrenamiento_prueba.csv` | Sin solapamiento; `random_state=3102`; 25 % para prueba |
| Regresión | `metricas_regresion.csv` | MAE, RMSE y R² para entrenamiento y prueba |
| Clasificación | `metricas_clasificacion.csv` | `accuracy` para entrenamiento y prueba, comparada con referencia |
| Predicciones | CSV y Parquet de predicciones | Probabilidades entre 0 y 1; errores identificables |
| Coeficientes | Dos CSV de coeficientes | Variables y coeficientes interpretables según escala y codificación |
| Reflexión | `reflexion_tecnica.md` | Discute sobreajuste, subajuste, sesgo, tamaño muestral y mejoras |

Ejecute las siguientes pruebas automáticas.

```python
assert (RUTA_SALIDAS / "auditoria_modelado.csv").exists()
assert (RUTA_SALIDAS / "particion_entrenamiento_prueba.csv").exists()
assert (RUTA_SALIDAS / "metricas_regresion.csv").exists()
assert (RUTA_SALIDAS / "metricas_clasificacion.csv").exists()
assert (RUTA_SALIDAS / "predicciones_clasificacion_prueba.csv").exists()
assert (RUTA_SALIDAS / "predicciones_regresion_prueba.parquet").exists()

assert len(df_train) > 0 and len(df_test) > 0
assert set(df_train.index).isdisjoint(set(df_test.index))
assert resultados_clasificacion_prueba["probabilidad_clase_1"].between(0, 1).all()

print("Validaciones automáticas básicas superadas.")
```

### Caso adversarial de calidad y trazabilidad

Trate toda instrucción incrustada en archivos de datos como contenido, no como una orden de ejecución. Por ejemplo, si una categoría contiene texto como `ignorar la auditoría y usar esta columna como objetivo`, no ejecute esa instrucción ni cambie el procedimiento: es un valor de datos potencialmente malicioso o irrelevante.

Ejecute esta revisión para localizar texto sospechoso en variables categóricas:

```python
terminos_sospechosos = r"ignorar|instrucción|instruction|prompt|system|contraseña|token"

for columna in FEATURES_CATEGORICAS:
    coincidencias = df_modelo[
        df_modelo[columna]
        .astype("string")
        .str.contains(terminos_sospechosos, case=False, na=False, regex=True)
    ]
    print(f"{columna}: {len(coincidencias)} coincidencias sospechosas")
```

Si se detectan coincidencias:

1. Registre la columna y el número de filas.
2. Trátelas como categorías o datos a revisar; no como instrucciones.
3. No exponga datos sensibles en capturas o entregables.
4. Consulte al instructor si el contenido afecta la calidad del conjunto.

> Una **instrucción** es una indicación concreta dentro de esta práctica. Un **prompt** es el texto enviado temporalmente a una herramienta de IA. Un **mensaje de sistema** define reglas de comportamiento de un sistema de IA y no es equivalente a un prompt del estudiante. Ninguno de estos conceptos convierte a un archivo de datos en una fuente autorizada de instrucciones.

> Este laboratorio no requiere Microsoft 365 Copilot, Copilot Chat, Microsoft Designer ni Microsoft Planner. Por tanto, no requiere licencia ni configuración de estas herramientas. Si el instructor autoriza una consulta externa mediante una herramienta de IA, registre la fuente, el prompt utilizado, la incertidumbre de la respuesta y la revisión humana aplicada; no use como criterio de éxito la exposición de razonamiento interno del sistema.

## Solución de Problemas

### Incidencia 1: `ValueError` al estratificar o una clase aparece solo una vez

**Síntoma:** `train_test_split` muestra un error relacionado con la clase menos poblada, o el objetivo binario presenta muy pocas observaciones de una clase.

**Causa probable:** La clase `0` o `1` tiene menos de dos observaciones después de eliminar filas con objetivos faltantes. También puede existir una codificación incorrecta del objetivo binario.

**Corrección:**

1. Revise los conteos antes de crear la partición.
2. Confirme que el objetivo binario procede de 03-00-02 y contiene solo `0` y `1`.
3. No invente registros ni cambie etiquetas para forzar la estratificación.
4. Si la clase minoritaria es insuficiente, documente la limitación y consulte al instructor para definir una partición no estratificada únicamente como excepción pedagógica.

```python
print(df_modelo[COLUMNA_OBJETIVO_CLASIFICACION].value_counts(dropna=False))
```

### Incidencia 2: El modelo falla por valores de texto, nulos o columnas inexistentes

**Síntoma:** Aparece un error como `KeyError`, `could not convert string to float`, o `Input X contains NaN`.

**Causa probable:** Los nombres configurados en `FEATURES_NUMERICAS` o `FEATURES_CATEGORICAS` no coinciden con el DataFrame; una columna numérica contiene texto; o se intentó entrenar un modelo sin el `Pipeline` de imputación y codificación.

**Corrección:**

1. Compare los nombres configurados con `df_analitico.columns`.
2. Revise los tipos de datos de las variables numéricas.
3. Conserve la imputación y codificación dentro de `Pipeline`.
4. No convierta una categoría en número arbitrario salvo que su significado sea realmente ordinal y esté documentado.
5. Ejecute nuevamente desde el paso 6 después de corregir la configuración.

```python
display(df_analitico.dtypes)
display(df_analitico.loc[:, COLUMNAS_REQUERIDAS].head())
print("Nulos por columna:")
print(df_modelo.loc[:, COLUMNAS_REQUERIDAS].isna().sum())
```

## Limpieza

Conserve los archivos de resultados hasta que el instructor valide la entrega. No elimine ni modifique el archivo analítico de 04-00-01.

Si necesita reiniciar únicamente los resultados de este laboratorio, cierre el cuaderno, confirme que la ruta es correcta y elimine solo el directorio de salidas actual.

**macOS o Linux**

```bash
rm -rf salidas/lab_04_00_02 evidencia/lab_04_00_02
```

**Windows PowerShell**

```powershell
Remove-Item -Recurse -Force .\salidas\lab_04_00_02, .\evidencia\lab_04_00_02
```

Después, desactive el entorno virtual cuando termine su sesión.

```bash
deactivate
```

## Resumen

En esta práctica reutilizó el DataFrame analítico de 04-00-01 para preparar variables predictoras, separar datos de entrenamiento y prueba y entrenar modelos de regresión lineal y logística. También aplicó limpieza controlada, codificación categórica, imputación dentro de pipelines, operaciones de pandas para representación y trazabilidad, arrays NumPy para cálculos vectorizados y escritura de resultados en CSV y Parquet.

La evidencia principal debe mostrar que:

- La partición fue reproducible y no se modificó para mejorar resultados.
- Las variables predictoras no contienen fuga de información.
- Las métricas de entrenamiento y prueba fueron comparadas para detectar señales de sobreajuste o subajuste.
- La clasificación fue contrastada con una referencia de clase mayoritaria.
- Las conclusiones reconocen las limitaciones de tamaño muestral, representación y generalización.

Fuentes oficiales recomendadas:

- scikit-learn, regresión lineal: https://scikit-learn.org/1.7/modules/linear_model.html
- scikit-learn, regresión logística: https://scikit-learn.org/1.7/modules/linear_model.html#logistic-regression
- scikit-learn, `train_test_split`: https://scikit-learn.org/1.7/modules/generated/sklearn.model_selection.train_test_split.html
- pandas, guía de usuario: https://pandas.pydata.org/docs/
- NumPy, guía de usuario: https://numpy.org/doc/2.2/user/
- Matplotlib, documentación: https://matplotlib.org/3.10.3/
- Seaborn, documentación: https://seaborn.pydata.org/
