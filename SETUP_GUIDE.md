# Setup Guide General

## Análisis de Datos con Python

| Campo | Valor |
|---|---|
| Repositorio | [Netec-Mx/PYT_DA_ADV](https://github.com/Netec-Mx/PYT_DA_ADV) |
| Alcance revisado | README principal, SETUP_GUIDE.md, cinco capítulos, 12 prácticas, CODEOWNERS y recursos del repositorio |
| Rama y revisión | `main`, commit `2b14a38b48ead4b8a76ef421a775b99994991591` |
| Versión documental | 1.0 |
| Fecha de revisión | 28 de octubre de 2026 |
| Modalidad | Ejecución local con Python, entorno virtual y datos sintéticos |

## Objetivo

Preparar un único entorno local para todo el repositorio, con un intérprete Python de 64 bits, dependencias controladas, ejecución de scripts y cuadernos, SQLite y un servicio HTTP de prueba accesible desde el mismo equipo.

La preparación debe realizarse antes de iniciar y conservarse durante todo el curso. Esta guía consolida los requisitos compartidos; no contiene ejercicios ni genera los conjuntos de datos o entregables académicos.

El repositorio contiene documentación y un logotipo. No distribuye archivos `.py`, cuadernos `.ipynb`, bases de datos, archivos `requirements.txt` ejecutables ni paquetes de datos preparados. El código y los generadores de datos están incorporados en los README. Los archivos de configuración indicados a continuación deben crearse en el proyecto de trabajo.

## Requisitos de Hardware

| Recurso | Mínimo consolidado | Recomendado |
|---|---|---|
| Procesador | Arquitectura de 64 bits y dos núcleos | Cuatro núcleos o superior |
| Memoria RAM | 8 GB | 16 GB |
| Espacio libre | 10 GB para cubrir el requisito más alto del repositorio | Espacio adicional para entornos, caché de paquetes y exportaciones |
| Pantalla | 1366 × 768 | 1920 × 1080 |
| Red | Internet durante la instalación y descarga de dependencias | Paquetes preparados previamente si el aula no tiene acceso a Internet |
| Terminales | Dos terminales para combinar ejecución y servidor HTTP | Una terminal adicional si JupyterLab permanece abierto |

No se requiere GPU ni hardware de virtualización.

## Sistemas Operativos Compatibles

| Sistema | Condición de preparación |
|---|---|
| Windows | Windows 11 de 64 bits como base recomendada. Windows 10 aparece en las guías; utilizarlo únicamente si la institución mantiene una configuración con soporte vigente y valida los paquetes. |
| macOS | Versión de 64 bits compatible con CPython 3.12 y con las distribuciones binarias de las dependencias seleccionadas; arquitectura Intel o Apple Silicon según el equipo. |
| Linux | Distribución de 64 bits compatible con CPython 3.12 y las ruedas binarias disponibles. En Linux con glibc, el conjunto de paquetes utilizado en la validación requiere compatibilidad con `manylinux_2_28`. |

El repositorio no fija una versión mínima de macOS ni una distribución Linux. Registrar el sistema y la arquitectura reales. No mezclar intérpretes x86 de 32 bits con paquetes x64 o ARM64.

## Software Requerido

### Matriz única de preparación

Las versiones siguientes constituyen la línea común de esta guía. Se seleccionan versiones que ya aparecen en las prácticas y se completan las dependencias de ejecución omitidas. No representan una actualización general a las últimas versiones del mercado.

| Componente | Versión de preparación | Función |
|---|---:|---|
| CPython | **3.12.11**, 64 bits | Intérprete único del curso |
| pip | **24.3.1** | Instalación dentro del entorno virtual |
| pandas | **3.0.0** | Lectura, preparación y transformación de datos |
| NumPy | **2.2.6** | Operaciones numéricas y reproducibilidad |
| requests | **2.32.3** | Consultas al servidor HTTP local |
| Beautiful Soup 4 | **4.12.3** | Análisis de HTML mediante `bs4` |
| openpyxl | **3.1.5** | Lectura y escritura de archivos XLSX |
| PyArrow | **20.0.0** | Lectura y escritura de Parquet |
| Matplotlib | **3.10.3** | Visualización y exportación de imágenes |
| Seaborn | **0.13.2** | Visualización estadística |
| scikit-learn | **1.7.0** | Transformadores, particiones y modelos |
| Plotly | **6.0.0** | Visualizaciones interactivas y HTML autocontenido |
| SQLAlchemy | **2.0.36** | Dependencia declarada que se conserva en el entorno común; las conexiones de las prácticas utilizan principalmente `sqlite3` |
| JupyterLab | **4.4.3** | Edición y ejecución de cuadernos |
| ipykernel | **6.29.5** | Kernel Python asociado al entorno virtual |
| nbformat | **5.10.4** | Formato de cuadernos y soporte de representación de Plotly |
| joblib | **1.5.1** | Persistencia de transformadores; se importa directamente en el código |
| SciPy | **1.15.3** | Dependencia científica de scikit-learn y operaciones estadísticas |
| pyzmq | **26.4.0** | Comunicación del kernel Jupyter |
| jupyter-client | **8.6.3** | Inicio y comunicación con kernels |
| SQLite | Biblioteca incluida con el intérprete; **3.39.0 o superior** | Base local sin servidor; permite `RIGHT JOIN` y `FULL OUTER JOIN` |
| Navegador | Versión estable mantenida por su fabricante | Interfaz de JupyterLab y visualización de HTML |

Los módulos `venv`, `sqlite3`, `http.server`, `json`, `csv`, `xml.etree.ElementTree`, `pathlib`, `argparse`, `hashlib` y `datetime` forman parte de Python. No se instalan con `pip`.

### Editor opcional

Puede utilizarse Visual Studio Code de 64 bits con las extensiones oficiales **Python** (`ms-python.python`) y **Jupyter** (`ms-toolsai.jupyter`). Las guías alternan VS Code 1.96.2 y 1.102.0; no hay una dependencia del código que obligue a esas compilaciones. Utilizar una edición mantenida compatible con las extensiones y registrar su versión.

JupyterLab permite ejecutar el curso sin instalar VS Code. No se requiere Visual Studio, Node.js ni herramientas de compilación cuando las dependencias se instalan mediante ruedas binarias compatibles.

### Correcciones de versiones y continuidad

| Diferencia encontrada | Criterio único de esta guía |
|---|---|
| Python 3.12.1 y 3.12.11 | Preparar 3.12.11 desde el inicio; no cambiar de intérprete a mitad de la secuencia. |
| NumPy 2.2.0 y 2.2.6 | Utilizar 2.2.6. |
| Matplotlib 3.10.0 y 3.10.3 | Utilizar 3.10.3. |
| scikit-learn 1.6.0 y 1.7.0 | Utilizar 1.7.0 de forma consistente. |
| PyArrow 18.1.0 y 20.0.0 | Utilizar 20.0.0; no degradarlo al avanzar entre documentos. |
| JupyterLab 4.3.4 y 4.4.3 | Utilizar 4.4.3. |
| Plotly 5.24.1 y 6.0.0 | Utilizar 6.0.0. |
| SQLite 3.47.2 presentado como valor fijo | Registrar la biblioteca enlazada a Python y verificar sus capacidades. Instalar la CLI de SQLite no cambia la biblioteca que usa `sqlite3`. |
| SQLAlchemy sin versión en una tabla | Conservar 2.0.36, especificada en el archivo de requisitos descrito por el repositorio. |
| URL `Netec-Mx/261028-PYT-DA-ADV` en el SETUP_GUIDE original | Utilizar el repositorio real `Netec-Mx/PYT_DA_ADV`. |

**Antes de distribuir los cuadernos:** al copiar los bloques desde las guías, alinear únicamente sus comprobaciones e instalaciones con esta matriz. Existen aserciones explícitas que exigen Python 3.12.1, NumPy 2.2.0 o scikit-learn 1.6.0, aunque otras prácticas exigen versiones distintas. Sustituir esos valores por 3.12.11, 2.2.6 y 1.7.0, respectivamente, sin eliminar las comprobaciones. No ejecutar después los comandos antiguos que degradan el entorno.

Esta guía no modifica el repositorio remoto. La normalización anterior debe quedar aplicada en las copias de trabajo que se utilicen para la sesión.

## Herramientas y Dependencias

- Un entorno virtual `.venv` para todo el proyecto.
- Un kernel de Jupyter que utilice exactamente ese intérprete.
- Un navegador para la interfaz local y los archivos HTML de Plotly.
- Lectura y escritura en el directorio del proyecto.
- `openpyxl` para XLSX y PyArrow para Parquet; no se necesita Microsoft Excel para generar esos archivos.
- `joblib`, `ipykernel` y `nbformat`, que deben estar disponibles aunque algunas tablas del repositorio no los declaren expresamente.
- `nbconvert`, instalado como dependencia de JupyterLab/Jupyter Server, para la exportación de cuadernos a HTML. Comprobar su disponibilidad después de instalar.

SciPy, pyzmq y jupyter-client se fijan para controlar dependencias transitivas relevantes. El resto de dependencias resueltas debe registrarse en un archivo de bloqueo generado después de validar la estación.

No se requieren Docker, Kubernetes, máquinas virtuales, plataformas cloud, cuentas de bases de datos, Microsoft 365, Copilot ni servicios de IA. No se necesita un parser externo como `lxml` para el código mostrado: el HTML usa `html.parser` y el XML utiliza la biblioteca estándar.

## Instalación y Configuración

### 1. Disponer del intérprete correcto

Python 3.12.11 es una publicación de seguridad sin instaladores binarios oficiales para Windows o macOS. No buscar un instalador EXE o PKG inexistente en su página de publicación.

Si TI ya entrega CPython 3.12.11 de 64 bits, utilizar esa instalación. Si no está disponible, puede aprovisionarse con **uv**, siguiendo la [instalación oficial de Astral](https://docs.astral.sh/uv/getting-started/installation/). uv se utiliza solo para obtener el intérprete y crear el entorno; no sustituye a `pip` durante las prácticas.

En Windows con WinGet disponible:

```powershell
winget install --id astral-sh.uv --exact
```

En macOS o Linux, utilizar el instalador oficial aprobado por TI:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Abrir una terminal nueva y comprobar:

```text
uv --version
uv python install 3.12.11
uv python find 3.12.11
```

La última consulta debe mostrar una ruta al intérprete 3.12.11. Registrar la versión de uv utilizada para el aprovisionamiento. Sus distribuciones de Python proceden de `python-build-standalone`; deben estar permitidas por la política institucional.

### 2. Crear una sola raíz de trabajo

Descargar o consultar el repositorio como documentación. Los datos y resultados se guardarán en el proyecto local, no dentro del entorno virtual.

**Windows PowerShell:**

```powershell
$projectRoot = Join-Path $env:USERPROFILE 'NetecLabs\PYT_DA_ADV\lab-01-00-01'
New-Item -ItemType Directory -Force -Path $projectRoot | Out-Null
Set-Location $projectRoot
```

**macOS o Linux:**

```bash
mkdir -p "$HOME/NetecLabs/PYT_DA_ADV/lab-01-00-01"
cd "$HOME/NetecLabs/PYT_DA_ADV/lab-01-00-01"
```

El nombre `lab-01-00-01` conserva la raíz creada por la primera guía, pero se reutiliza durante todo el curso. No crear un proyecto independiente por capítulo.

### 3. Crear y activar el entorno virtual

Si se utiliza uv, ejecutar una sola vez desde la raíz del proyecto:

```text
uv venv --python 3.12.11 --seed .venv
```

Si se utiliza un intérprete ya aprovisionado por TI, ejecutar su ruta completa con `-m venv .venv`. No emplear un comando `python` ambiguo que seleccione otra instalación.

**Windows PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
python --version
python -c "import sys; print(sys.executable)"
```

**macOS o Linux:**

```bash
source .venv/bin/activate
python --version
python -c "import sys; print(sys.executable)"
```

Resultado esperado: `Python 3.12.11` y una ruta que termine en `.venv\Scripts\python.exe` o `.venv/bin/python` dentro del proyecto.

Si PowerShell bloquea `Activate.ps1`, se pueden ejecutar los comandos con la ruta explícita:

```powershell
.\.venv\Scripts\python.exe --version
.\.venv\Scripts\python.exe -m pip --version
```

En ese caso, sustituir `python` por `.\.venv\Scripts\python.exe` en los comandos siguientes. No es necesario modificar la política de ejecución de todo el equipo.

Si ya existe `.venv`, comprobar su versión antes de continuar. Reutilizarla si coincide. Si usa otro intérprete, registrar sus dependencias y preparar su sustitución controlada antes del curso; no recrearla durante la secuencia ni eliminar los datos del proyecto.

### 4. Crear el manifiesto común de dependencias

Crear **`requirements-setup.txt`** en la raíz del proyecto, en UTF-8, con el siguiente contenido:

```text
pandas==3.0.0
numpy==2.2.6
requests==2.32.3
beautifulsoup4==4.12.3
openpyxl==3.1.5
pyarrow==20.0.0
matplotlib==3.10.3
seaborn==0.13.2
scikit-learn==1.7.0
plotly==6.0.0
SQLAlchemy==2.0.36
jupyterlab==4.4.3
ipykernel==6.29.5
nbformat==5.10.4
joblib==1.5.1
scipy==1.15.3
pyzmq==26.4.0
jupyter-client==8.6.3
```

Instalar desde el entorno virtual:

```bash
python -m pip install "pip==24.3.1"
python -m pip install --only-binary=:all: -r requirements-setup.txt
python -m pip check
```

Resultado esperado de la última orden:

```text
No broken requirements found.
```

El uso de ruedas binarias evita requerir compiladores de C/C++ para la preparación. Si no existe una distribución binaria para el sistema elegido, revisar arquitectura y sistema operativo antes de cambiar versiones o instalar herramientas de compilación.

Conservar `requirements-setup.txt` aunque se cree otro `requirements.txt` durante el trabajo. La instrucción del repositorio que escribe un archivo con solo SQLAlchemy no debe reemplazar este manifiesto general.

### 5. Preparar las carpetas comunes

Con el entorno activo y desde la raíz:

```bash
python -c "from pathlib import Path; [Path(p).mkdir(parents=True, exist_ok=True) for p in ('datos','fuentes_web','scripts','notebooks','salidas','evidencia')]"
```

Estructura base:

```text
lab-01-00-01/
├── .venv/
├── datos/
├── fuentes_web/
├── scripts/
├── notebooks/
├── salidas/
├── evidencia/
└── requirements-setup.txt
```

Las carpetas pueden estar vacías al terminar el setup. Las fuentes sintéticas, scripts, cuadernos y bases se crean posteriormente con las instrucciones del repositorio.

### 6. Registrar el kernel

```bash
python -m ipykernel install --sys-prefix --name pyt-da-adv --display-name "Python PYT_DA_ADV"
python -m jupyter kernelspec list
```

Resultado esperado: aparece `pyt-da-adv` y su especificación queda dentro de `.venv`. Seleccionar **Python PYT_DA_ADV** al abrir los cuadernos.

No copiar ni distribuir una `.venv` entre equipos. Crear el entorno en su ubicación definitiva y reconstruirlo desde los manifiestos si se traslada el proyecto.

### 7. Configurar JupyterLab o Visual Studio Code

Para trabajar con JupyterLab, iniciar desde la raíz:

```bash
python -m jupyter lab --ip=127.0.0.1 --port=8888 --ServerApp.port_retries=0
```

Abrir la URL local que muestra la terminal. Mantener la autenticación local que utiliza Jupyter; no compartir su token ni deshabilitarla para resolver problemas de acceso.

Si se utiliza VS Code, abrir la raíz del proyecto y seleccionar **Python: Select Interpreter** para elegir `.venv`. Instalar las extensiones oficiales desde su catálogo o, si la CLI `code` está disponible:

```bash
code --version
code --install-extension ms-python.python
code --install-extension ms-toolsai.jupyter
code .
```

En el selector de kernel del cuaderno, elegir el entorno del curso. Comprobar `sys.executable`; la selección del intérprete de la terminal y la del kernel pueden ser distintas.

Crear los cuadernos mediante JupyterLab, VS Code o `nbformat`. No usar `touch` o `New-Item` para crear un archivo `.ipynb` vacío y asumir que ya es un cuaderno válido.

## Configuración Base para los Laboratorios

### Directorio de ejecución

Ejecutar los scripts desde la raíz del proyecto, por ejemplo con `python scripts/nombre_del_script.py`. En los cuadernos, comprobar que las rutas relativas se resuelvan desde esa misma raíz.

Si un cuaderno está guardado en `notebooks/`, el directorio de su kernel puede ser esa subcarpeta. Utilizar esta celda de configuración antes de las celdas que acceden a datos:

```python
from pathlib import Path
import os
import sys

candidatas = [Path.cwd(), *Path.cwd().parents]
raiz = next((p for p in candidatas if (p / ".venv").is_dir()), None)
if raiz is None:
    raise RuntimeError("Abra el cuaderno dentro del proyecto del curso.")
os.chdir(raiz)
assert sys.prefix != sys.base_prefix, "Seleccione el kernel de .venv."
print("Raíz:", Path.cwd())
print("Intérprete:", sys.executable)
```

### Rutas y continuidad de archivos

- Conservar la estructura y las salidas entre sesiones.
- Las guías utilizan nombres con guiones, como `lab-01-00-02`, y con guiones bajos, como `lab_03_00_01`. Respetar las rutas realmente generadas; no reemplazar todos los nombres de manera global.
- Resolver las rutas de ejemplo contra los archivos existentes antes de reutilizarlos. Un nombre terminado en `datos_reutilizados.parquet` o en `CAMBIE_POR_LA_RUTA...` no identifica un archivo distribuido por GitHub.
- La base SQLite de trabajo se crea durante la secuencia. No crear una base vacía solo para satisfacer una comprobación `exists()`.
- Los módulos propios, como `scripts/configuracion.py`, se crean con el código de las guías. No son paquetes que deban buscarse en PyPI.
- Mantener las semillas que declara cada bloque original. El valor `3102` es común en los cuadernos analíticos, pero también aparece `202501` en una transformación anterior; no uniformarlos como si fueran una variable de instalación.

### SQLite

Usar el módulo `sqlite3` del intérprete de `.venv`. No se requiere instalar un servidor, configurar un usuario ni abrir un puerto de base de datos.

Comprobar:

```bash
python -c "import sqlite3; print(sqlite3.sqlite_version); assert sqlite3.sqlite_version_info >= (3,39,0)"
```

La versión 3.39.0 incorpora los `RIGHT JOIN` y `FULL OUTER JOIN` utilizados por el repositorio. La referencia 3.47.2 no se obtiene instalando otra CLI: importa la biblioteca enlazada con Python. En la validación de esta guía, CPython 3.12.11 administrado con uv utilizó SQLite **3.50.4** en Linux x86-64.

Para bases que sí deban verificar claves foráneas, cada conexión correspondiente debe ejecutar `PRAGMA foreign_keys = ON`. Esta configuración pertenece a la conexión y no se aplica permanentemente al servidor, pues no existe un servicio SQLite independiente.

### Configuraciones y variables de entorno

No se requiere `.env`, credenciales, tokens, claves de API, `PYTHONPATH` ni variables de conexión de una base remota. La activación de `.venv` ajusta `PATH` para utilizar el intérprete del proyecto.

`RANDOM_STATE_CURSO`, las rutas de los datos y `PUERTO` son parámetros del código, no variables de entorno obligatorias del sistema. No configurar `MPLBACKEND=Agg` de forma global: se reserva para la prueba automática de exportación; los cuadernos deben conservar su representación gráfica normal.

## Servicios Externos Requeridos

### Durante la preparación

Se necesita acceso a:

- [GitHub del repositorio](https://github.com/Netec-Mx/PYT_DA_ADV), para las guías.
- [PyPI](https://pypi.org/) y sus destinos de distribución de archivos, o al índice institucional autorizado, para las dependencias.
- [Astral](https://docs.astral.sh/uv/) y los destinos de descarga de `python-build-standalone`, únicamente si se usa uv para aprovisionar Python.
- El catálogo de VS Code, únicamente si se utiliza ese editor y sus extensiones.

Durante la ejecución normal no se requiere cloud, una API pública, una cuenta externa ni conexión a Internet después de instalar los componentes. Los datos se generan localmente con los bloques del repositorio.

La mención a una API externa autorizable en la primera guía no constituye un requisito del entorno general. La ruta completa utiliza el servidor HTTP local; no deben inventarse URL ni credenciales para sustituirlo.

### Servicio HTTP local

Preparar el puerto **8000** en la interfaz de loopback. Utilizar **8001** solo si el primero no está disponible y registrar el cambio. El servidor debe servir exclusivamente `fuentes_web`, no la raíz que contiene el entorno y las evidencias.

En una terminal con `.venv` activa y ubicada en la raíz:

```bash
python -m http.server 8000 --bind 127.0.0.1 --directory fuentes_web
```

Desde otra terminal del mismo proyecto:

```bash
python -c "import requests; r=requests.get('http://127.0.0.1:8000/', timeout=5); r.raise_for_status(); print(r.status_code)"
```

Resultado esperado: `200`. Esta comprobación funciona con la carpeta vacía. Los endpoints JSON y HTML propios del trabajo solo existirán después de generar sus fuentes.

Registrar el puerto que utilizarán los scripts:

```bash
python -c "from pathlib import Path; Path('evidencia/puerto_usado.txt').write_text('8000', encoding='utf-8')"
```

Si se usa 8001, cambiar el puerto del servidor y el contenido del archivo a `8001`. El código de integración utiliza ese archivo para construir su URL base. Detener el servidor con `Ctrl+C` al terminar la comprobación.

JupyterLab utiliza el puerto **8888** y también se limita a `127.0.0.1`. No se necesita abrir estos servicios hacia Internet o hacia la red del aula.

## Validación del Entorno

Realizar todas las comprobaciones antes de iniciar la secuencia. La existencia de una carpeta `.venv` por sí sola no acredita que el intérprete y las bibliotecas sean correctos.

| Comprobación | Resultado esperado |
|---|---|
| Intérprete | Python 3.12.11 de 64 bits dentro del proyecto |
| Dependencias | Versiones del manifiesto y `pip check` sin conflictos |
| SQLite | Biblioteca ≥ 3.39.0 y consultas de unión disponibles |
| Excel y Parquet | Escritura y lectura de archivos temporales correctas |
| Modelos | Entrenamiento mínimo y persistencia con joblib funcionales |
| Visualización | PNG y HTML autocontenido generados correctamente |
| HTTP | Consulta local con `requests` y respuesta 200 |
| Cuadernos | Kernel `pyt-da-adv` disponible y ejecución satisfactoria |
| Escritura | Acceso a las carpetas de datos, salidas y evidencia |
| Revisión visual | JupyterLab abre; un PNG y un HTML interactivo se visualizan en la estación |

La prueba automática de la siguiente sección crea datos de comprobación en una carpeta temporal. No utiliza ni modifica bases o resultados de los laboratorios.

## Comandos de Verificación

### Intérprete, instalación y servicios de cuadernos

Con `.venv` activa, desde la raíz:

```bash
python --version
python -c "import sys,struct; print(sys.executable); print('bits:',struct.calcsize('P')*8); print('venv:',sys.prefix != sys.base_prefix)"
python -m pip --version
python -m pip check
python -m jupyterlab --version
python -m jupyter kernelspec list
python -m jupyter nbconvert --version
python -c "import sqlite3; print('SQLite:', sqlite3.sqlite_version)"
```

Resultados esperados: Python `3.12.11`, `bits: 64`, `venv: True`, pip `24.3.1`, JupyterLab `4.4.3`, kernel `pyt-da-adv`, versión de nbconvert disponible y SQLite compatible.

Para comprobar las versiones instaladas sin depender del nombre del módulo de importación:

```bash
python -m pip show pandas numpy requests beautifulsoup4 openpyxl pyarrow matplotlib seaborn scikit-learn plotly SQLAlchemy jupyterlab ipykernel nbformat joblib scipy pyzmq jupyter-client
```

### Prueba automática de preparación

Guardar el siguiente bloque como **`scripts/validar_setup.py`**, en UTF-8. Ejecutarlo desde la raíz después de registrar el kernel. No requiere paquetes adicionales a los ya instalados.

```python
from importlib.metadata import version
from pathlib import Path
from tempfile import TemporaryDirectory
import json
import sqlite3
import sys
import threading
from functools import partial
from http.server import ThreadingHTTPServer, SimpleHTTPRequestHandler

assert sys.version_info[:3] == (3, 12, 11), sys.version
assert sys.prefix != sys.base_prefix, 'Se requiere un entorno virtual'
expected = {}
for line in Path('requirements-setup.txt').read_text().splitlines():
    if '==' in line:
        name, pin = line.split('=='); expected[name] = pin
for name, pin in expected.items():
    assert version(name) == pin, (name, version(name), pin)
    print(name, version(name))

import pandas as pd
import numpy as np
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.io as pio
import requests
from bs4 import BeautifulSoup
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.preprocessing import OneHotEncoder
from joblib import dump, load
import nbformat

with TemporaryDirectory() as temp:
    root = Path(temp)
    frame = pd.DataFrame({'x':[1,2,3,4], 'y':[2.5,4.5,6.5,8.5], 'grupo':['a','b','a','b']})
    frame.to_excel(root/'prueba.xlsx', index=False)
    pd.testing.assert_frame_equal(frame, pd.read_excel(root/'prueba.xlsx'))
    frame.to_parquet(root/'prueba.parquet', index=False)
    pd.testing.assert_frame_equal(frame, pd.read_parquet(root/'prueba.parquet'))
    assert BeautifulSoup('<table><tr><td>OK</td></tr></table>', 'html.parser').td.text=='OK'
    model=LinearRegression().fit(frame[['x']],frame['y'])
    np.testing.assert_allclose(model.predict(pd.DataFrame({'x':[5]})),[10.5])
    LogisticRegression().fit(np.array([[0],[1],[2],[3]]),[0,0,1,1])
    encoded=OneHotEncoder(sparse_output=False).fit_transform(frame[['grupo']])
    assert encoded.shape==(4,2)
    dump(model,root/'modelo.joblib'); assert load(root/'modelo.joblib').coef_[0]>0
    fig,ax=plt.subplots(); sns.boxplot(data=frame,x='grupo',y='y',ax=ax)
    fig.savefig(root/'prueba.png');plt.close(fig)
    assert (root/'prueba.png').stat().st_size>0
    px.scatter(frame,x='x',y='y',color='grupo').write_html(root/'prueba.html',include_plotlyjs=True)
    assert (root/'prueba.html').stat().st_size>0
    rendered=pio.to_html(px.scatter(frame,x='x',y='y'),include_plotlyjs=True)
    assert '<html>' in rendered
    connection=sqlite3.connect(':memory:')
    assert sqlite3.sqlite_version_info >= (3,39,0)
    connection.executescript('CREATE TABLE a(id INTEGER PRIMARY KEY);CREATE TABLE b(id INTEGER);INSERT INTO a VALUES (1);INSERT INTO b VALUES (2);')
    assert len(connection.execute('SELECT * FROM a FULL OUTER JOIN b USING(id)').fetchall())==2
    assert connection.execute('SELECT row_number() OVER () FROM a').fetchone()==(1,)
    connection.close()
    print('SQLite',sqlite3.sqlite_version)
    (root/'ok.json').write_text(json.dumps({'estado':'OK'}),encoding='utf-8')
    handler=partial(SimpleHTTPRequestHandler,directory=str(root))
    server=ThreadingHTTPServer(('127.0.0.1',0),handler)
    thread=threading.Thread(target=server.serve_forever,daemon=True);thread.start()
    try:
        response=requests.get(f'http://127.0.0.1:{server.server_port}/ok.json',timeout=5)
        assert response.status_code==200 and response.json()=={'estado':'OK'}
    finally:
        server.shutdown();server.server_close();thread.join()
print('SETUP_OK: dependencias, formatos, SQLite, modelos, graficos y HTTP')
```

Ejecutar:

```bash
python scripts/validar_setup.py
```

Resultado esperado: se muestran las versiones comprobadas y termina con:

```text
SETUP_OK: dependencias, formatos, SQLite, modelos, graficos y HTTP
```

No ignorar una aserción fallida para marcar la estación como apta. Si el sistema impide iniciar procesos de kernel o conexiones locales, resolver esa restricción con TI y repetir la prueba en la estación del curso.

### Ejecución del kernel y comprobación visual

Abrir JupyterLab con el comando de inicio indicado, crear un cuaderno con **Python PYT_DA_ADV** y ejecutar:

```python
import sys
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly.express as px

assert sys.version_info[:3] == (3, 12, 11)
assert sys.prefix != sys.base_prefix
assert pd.__version__ == "3.0.0"
assert np.__version__ == "2.2.6"
print("KERNEL_OK", sys.executable)
plt.plot([0, 1], [0, 1])
plt.show()
px.scatter(x=[0, 1], y=[0, 1]).show()
```

Resultado esperado: `KERNEL_OK`, la ruta del intérprete de `.venv`, una figura estática y una figura interactiva visibles. El registro del kernel en una lista no sustituye esta prueba de ejecución. Si el navegador no representa Plotly, revisar el apartado de troubleshooting y comprobar también un HTML exportado mediante `write_html(..., include_plotlyjs=True)`.

### Registro del entorno validado

Después de que las comprobaciones sean satisfactorias, generar `requirements-lock.txt` en UTF-8:

```bash
python -c "from pathlib import Path; import subprocess,sys; Path('requirements-lock.txt').write_text(subprocess.check_output([sys.executable,'-m','pip','freeze'], text=True), encoding='utf-8')"
```

Conservar el manifiesto inicial y este registro de dependencias directas y transitivas. El archivo de bloqueo corresponde al sistema y arquitectura donde se generó; validar nuevamente si se utiliza en otra plataforma. No copiar el directorio `.venv` entre estaciones.

Si se requiere instalación sin Internet, preparar previamente un directorio de ruedas binarias en el mismo sistema y arquitectura de destino:

```bash
python -m pip download --only-binary=:all: --dest wheelhouse -r requirements-lock.txt "pip==24.3.1"
```

En la estación sin Internet, con el intérprete y el entorno ya disponibles:

```bash
python -m pip install --no-index --find-links wheelhouse "pip==24.3.1"
python -m pip install --no-index --find-links wheelhouse -r requirements-lock.txt
python -m pip check
```

## Troubleshooting

| Síntoma | Comprobación y corrección |
|---|---|
| `ModuleNotFoundError` aunque se instaló el paquete | Comparar `sys.executable` en la terminal y en el cuaderno. Instalar mediante `python -m pip` del entorno correcto y reiniciar el kernel. |
| `Activate.ps1` bloqueado | Usar `.\.venv\Scripts\python.exe` explícitamente. No modificar globalmente la política de ejecución para activar un entorno. |
| Una aserción pide Python 3.12.1 o NumPy 2.2.0 | La fuente conserva una comprobación antigua. Alinear el valor con la matriz única de preparación y mantener la aserción. No degradar la estación durante el curso. |
| Solo se conserva SQLAlchemy en `requirements.txt` | Mantener `requirements-setup.txt` y el bloqueo completo como referencias del entorno común. No reemplazarlos con el archivo parcial descrito en una actividad. |
| `No matching distribution found` | Revisar versión de Python, arquitectura e índice de paquetes. Confirmar que el sistema dispone de ruedas binarias compatibles antes de intentar compilar. |
| Se propone un instalador oficial de Python 3.12.11 para Windows o macOS | Esa publicación es solo de código fuente. Utilizar la distribución administrada aprobada o el procedimiento con uv. |
| Kernel no disponible o no inicia | Comprobar ipykernel, pyzmq y jupyter-client, registrar el kernel desde `.venv` y verificar permisos de procesos y red local. No usar un kernel global como sustituto silencioso. |
| `FileNotFoundError` con datos existentes | Revisar la raíz de ejecución, guiones frente a guiones bajos y las rutas configurables. Conservar los nombres reales de los artefactos. |
| SQLite no reconoce `RIGHT JOIN` o `FULL OUTER JOIN` | Revisar `sqlite3.sqlite_version`, no la versión de una CLI externa. Usar un intérprete enlazado con SQLite ≥ 3.39.0. |
| `database is locked` | Cerrar conexiones y otros procesos que mantienen abierta la misma base. Confirmar escritura en el directorio; no crear una base alternativa para ocultar el bloqueo. |
| Conexión HTTP rechazada | Iniciar el servidor desde el entorno correcto, confirmar puerto y mantener abierta su terminal. Usar 127.0.0.1; comprobar que el archivo de puerto coincide. |
| HTTP 404 en JSON o HTML | Verificar `--directory fuentes_web` y que las fuentes correspondientes ya fueron generadas. El setup no crea esos recursos de los ejercicios. |
| Puerto 8000 u 8888 ocupado | Identificar el proceso autorizado o seleccionar otro puerto. Registrar el puerto HTTP en `evidencia/puerto_usado.txt` y utilizar la URL real de Jupyter. |
| Cuaderno vacío o JSON inválido | Crear un cuaderno válido desde JupyterLab/VS Code o nbformat. Un archivo creado con `touch` no contiene la estructura de un `.ipynb`. |
| Error de XLSX o Parquet | Verificar openpyxl o PyArrow en `.venv`; confirmar que el archivo tiene un formato real y no una extensión renombrada. |
| Plotly no se muestra en el cuaderno | Verificar nbformat y el kernel seleccionado. Abrir el HTML autocontenido en el navegador para distinguir un problema de representación de un problema de generación. No se necesita Kaleido para el HTML utilizado. |
| Gráficos estáticos sin interfaz gráfica en Linux | Ejecutar desde el cuaderno. El backend Agg es válido para exportar PNG en la prueba automática, pero no abre ventanas interactivas. |

## Checklist Final

- [ ] Sistema de 64 bits y recursos de hardware suficientes.
- [ ] CPython 3.12.11 seleccionado desde el inicio.
- [ ] Una sola `.venv` en la raíz del proyecto.
- [ ] pip 24.3.1 y dependencias de la matriz instaladas.
- [ ] `python -m pip check` sin conflictos.
- [ ] Comprobaciones antiguas de versiones alineadas en las copias de trabajo.
- [ ] Kernel `pyt-da-adv` asociado al intérprete correcto.
- [ ] JupyterLab o VS Code abre los cuadernos y utiliza la raíz adecuada.
- [ ] Carpetas comunes con permisos de lectura y escritura.
- [ ] Lectura y escritura de XLSX y Parquet verificadas.
- [ ] SQLite ≥ 3.39.0 y consulta de unión probada.
- [ ] PNG y HTML interactivo comprobados en la estación.
- [ ] HTTP local responde 200 y su puerto queda registrado.
- [ ] Prueba automática finaliza con `SETUP_OK`.
- [ ] `requirements-lock.txt` conservado en UTF-8.
- [ ] No se han creado bases vacías, datos ficticios de reemplazo ni entregables académicos para suplir archivos aún no generados.

## Resumen

El entorno general consiste en un proyecto local con CPython 3.12.11 de 64 bits, una única `.venv`, las bibliotecas de análisis fijadas, un kernel Jupyter asociado y SQLite integrado. El servicio HTTP se ejecuta localmente cuando se necesita. No se requieren servicios cloud, servidores de bases de datos, credenciales externas ni licencias de Microsoft 365.

La matriz común resuelve las contradicciones de las guías y evita reinstalaciones entre capítulos. La preparación se considera completa cuando pasan las comprobaciones de dependencias, formatos, modelos, gráficos, HTTP y kernel, y se confirma la visualización en la estación real.

### Alcance de la validación técnica

Se revisó la documentación completa de los cinco capítulos y las 12 prácticas, junto con los archivos generales del repositorio. La validación técnica se realizó en Linux x86-64 con CPython 3.12.11 y la matriz indicada: instalación de dependencias, `pip check`, importaciones, lectura/escritura XLSX y Parquet, consultas SQLite con unión externa y función de ventana, entrenamiento y persistencia de modelos, exportación PNG/HTML y respuesta HTTP local.

También se comprobó la disponibilidad de las CLI de JupyterLab y nbconvert y el registro del kernel. La ejecución del kernel no pudo validarse en este entorno de revisión porque el sistema bloqueó operaciones de comunicación de ZeroMQ con `Operation not permitted`. Debe completarse la prueba `KERNEL_OK` y la revisión visual en la estación del curso antes de aprobarla. No se afirma haber ejecutado íntegramente los 12 laboratorios ni haber probado esta matriz en Windows o macOS.

### Referencias oficiales

- [Repositorio analizado en la revisión indicada](https://github.com/Netec-Mx/PYT_DA_ADV/tree/2b14a38b48ead4b8a76ef421a775b99994991591).
- [Python 3.12.11 y disponibilidad de instaladores](https://www.python.org/downloads/release/python-31211/).
- [Administración de Python con uv](https://docs.astral.sh/uv/guides/install-python/).
- [Instalación de uv](https://docs.astral.sh/uv/getting-started/installation/).
- [Entornos virtuales de Python](https://docs.python.org/3.12/library/venv.html).
- [Instalación de paquetes con pip](https://pip.pypa.io/en/stable/cli/pip_install/).
- [Dependencias y cambios de pandas 3.0.0](https://pandas.pydata.org/docs/whatsnew/v3.0.0.html).
- [Instalación de scikit-learn 1.7](https://scikit-learn.org/1.7/install.html).
- [JupyterLab 4.4](https://jupyterlab.readthedocs.io/en/4.4.x/).
- [Registro de kernels IPython](https://ipython.readthedocs.io/en/stable/install/kernel_install.html).
- [Cuadernos Jupyter en VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks).
- [SQLite integrado con Python](https://docs.python.org/3.12/library/sqlite3.html).
- [SQLite 3.39.0 y soporte de RIGHT y FULL OUTER JOIN](https://www.sqlite.org/releaselog/3_39_0.html).
- [Servidor HTTP de Python](https://docs.python.org/3.12/library/http.server.html).
- [Representación de figuras Plotly](https://plotly.com/python/renderers/).
