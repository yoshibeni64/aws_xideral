# Práctica AWS: S3, Boto3, Jupyter, Streamlit, Docker y GitHub Actions

## 1. Objetivo de la práctica

En esta práctica se integraron varios servicios y herramientas para construir un flujo completo de trabajo en AWS:

1. Preparar una instancia **EC2** con Ubuntu.
2. Instalar y configurar herramientas de Linux y AWS.
3. Configurar el **AWS CLI** para comunicarse con la cuenta de AWS.
4. Crear y utilizar un bucket de **Amazon S3**.
5. Subir, consultar, sincronizar y eliminar archivos de S3 mediante AWS CLI.
6. Utilizar **Python + Boto3** para acceder a S3 desde código.
7. Descargar un dataset de Spotify desde S3 y cargarlo en un **DataFrame de pandas**.
8. Crear una aplicación sencilla con **Streamlit**.
9. Ejecutar la aplicación dentro de EC2.
10. Crear un repositorio en GitHub para la práctica.
11. Configurar un **self-hosted runner** de GitHub Actions dentro de EC2.
12. Crear un flujo de despliegue utilizando **Docker, Docker Compose y GitHub Actions**.
13. Acceder finalmente a la aplicación desde el puerto `8501` de la instancia EC2.

---

# 2. Arquitectura general

El flujo que se construyó puede entenderse de la siguiente manera:

```text
                 AWS
                  │
        ┌─────────┴─────────┐
        │                   │
       EC2                  S3
        │                   │
        │             spotify-2023.csv
        │                   │
        │                   ▼
        │              Boto3 / S3
        │                   │
        ▼                   ▼
   Jupyter / Python ──► Pandas DataFrame
        │
        ▼
     Streamlit
        │
        ▼
      Docker
        │
        ▼
 Docker Compose
        │
        ▼
 GitHub Actions
        │
        ▼
 Self-hosted Runner
        │
        ▼
      EC2
        │
        ▼
   Puerto 8501
        │
        ▼
 Aplicación Streamlit
```

La idea principal es que **EC2 funciona como el servidor donde se ejecuta la aplicación**, mientras que **S3 funciona como almacenamiento de archivos/datos**.

---

# 3. Preparación de EC2

La práctica se realizó sobre una instancia EC2 con Ubuntu.

Desde la terminal de la instancia se ejecutan los comandos necesarios para preparar el entorno.

---

## 3.1. Instalar `unzip`

```bash
sudo apt install unzip
```

### ¿Qué hace?

Instala la utilidad `unzip`, que permite descomprimir archivos `.zip`.

El comando está compuesto por:

- `sudo`: ejecuta el comando con permisos administrativos.
- `apt`: administrador de paquetes de Ubuntu.
- `install`: indica que queremos instalar un paquete.
- `unzip`: paquete que queremos instalar.

Una forma más completa y habitual sería:

```bash
sudo apt update
sudo apt install unzip
```

`apt update` actualiza primero la información de los paquetes disponibles.

---

# 4. Configuración de PATH

Se utilizó la variable de entorno `PATH` para indicar al sistema dónde buscar ejecutables.

```bash
export PATH="/usr/local/bin:$PATH"
```

Esto agrega:

```text
/usr/local/bin
```

al inicio de la variable `PATH`.

La variable `$PATH` contiene diferentes directorios separados por `:`.

Por ejemplo:

```text
/usr/local/bin:/usr/bin:/bin
```

Cuando escribimos:

```bash
aws
```

Linux busca el ejecutable `aws` dentro de los directorios definidos en `PATH`.

---

## 4.1. Agregar una ruta al PATH permanentemente

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

Este comando agrega la configuración al archivo:

```text
~/.zshrc
```

Ese archivo contiene configuraciones que se cargan al iniciar una sesión de **Zsh**.

La ruta:

```text
$HOME/.local/bin
```

se agrega al `PATH`.

El operador:

```bash
>>
```

significa **agregar contenido al final de un archivo**.

No reemplaza el contenido existente.

---

# 5. Verificar AWS CLI

Se utilizó:

```bash
aws --version
```

Esto permite verificar que AWS CLI esté instalado y disponible desde la terminal.

> Nota: debe utilizarse `--version` con dos guiones y la palabra `version` en inglés.

Ejemplo de resultado:

```text
aws-cli/2.x.x Python/3.x.x Linux/...
```

El número exacto depende de la versión instalada.

---

# 6. Instalar Oh My Zsh

Se utilizó el siguiente comando:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Este comando descarga y ejecuta el instalador de **Oh My Zsh**.

### Partes importantes

```bash
curl
```

permite realizar solicitudes y descargar contenido.

```bash
-fsSL
```

son opciones de `curl` para manejar la descarga de forma silenciosa y seguir redirecciones.

Después se ejecuta el script mediante:

```bash
sh -c
```

Oh My Zsh es una configuración/framework para Zsh que facilita trabajar con la terminal mediante temas, plugins y otras herramientas.

---

# 7. Recargar la configuración

Después de modificar `.zshrc` se puede ejecutar:

```bash
source ~/.zshrc
```

`source` vuelve a cargar el archivo de configuración en la sesión actual.

Esto permite aplicar los cambios sin cerrar y volver a abrir la terminal.

---

# 8. Configuración de AWS

Para que AWS CLI pueda comunicarse con nuestra cuenta se ejecutó:

```bash
aws configure
```

AWS CLI solicita:

```text
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-west-1
Default output format [None]: json
```

---

## 8.1. AWS Access Key ID

Es el identificador de una credencial utilizada por AWS CLI para autenticarse.

Ejemplo conceptual:

```text
AKIA...
```

**Nunca debe publicarse en GitHub ni incluirse directamente en el código.**

---

## 8.2. AWS Secret Access Key

Es la parte secreta de la credencial.

Debe mantenerse privada.

**No debe escribirse dentro de archivos del proyecto ni subirse al repositorio.**

---

## 8.3. Región

Se configuró:

```text
us-west-1
```

Esta región corresponde a **US West (N. California)**.

La región determina en qué región de AWS se realizan determinadas operaciones.

---

## 8.4. Formato de salida

Se configuró:

```text
json
```

Esto hace que AWS CLI muestre muchas respuestas en formato JSON.

---

# 9. Verificar la identidad de AWS

Después de configurar AWS CLI se utilizó:

```bash
aws sts get-caller-identity
```

Este comando consulta **AWS STS** para saber qué identidad está utilizando actualmente la CLI.

La respuesta contiene información como:

```json
{
    "UserId": "...",
    "Account": "...",
    "Arn": "..."
}
```

Es una buena práctica ejecutar este comando después de configurar las credenciales porque permite comprobar que AWS CLI está utilizando la cuenta/identidad esperada.

---

# 10. Trabajo con Amazon S3

Amazon S3 es un servicio de almacenamiento de objetos.

En la práctica se utilizó un bucket llamado:

```text
xideralaws-curso-rodrigo
```

Un bucket puede imaginarse como un contenedor donde almacenamos objetos, por ejemplo:

```text
hola.txt
spotify-2023.csv
imagenes/
documentos/
```

---

# 11. Subir un archivo a S3

Para subir `hola.txt`:

```bash
aws s3 cp hola.txt s3://xideralaws-curso-rodrigo/
```

Aquí:

```text
aws s3
```

indica que estamos utilizando los comandos de S3 de AWS CLI.

```text
cp
```

significa copy.

```text
hola.txt
```

es el archivo local.

```text
s3://xideralaws-curso-rodrigo/
```

es el destino.

El resultado es:

```text
hola.txt
        ↓
S3
xideralaws-curso-rodrigo
```

---

# 12. Listar archivos del bucket

```bash
aws s3 ls s3://xideralaws-curso-rodrigo/
```

Este comando muestra los objetos almacenados en el bucket.

Por ejemplo:

```text
2026-09-09 18:00:00       12 hola.txt
```

---

# 13. Eliminar un archivo

Para eliminar `hola.txt`:

```bash
aws s3 rm s3://xideralaws-curso-rodrigo/hola.txt
```

La estructura es:

```text
aws s3 rm <ubicación-del-objeto>
```

`rm` significa remove.

---

# 14. Sincronizar una carpeta completa

También se utilizó:

```bash
aws s3 sync . s3://xideralaws-curso-rodrigo
```

El punto:

```text
.
```

representa el directorio actual.

Por lo tanto, el comando sincroniza el contenido del directorio actual con el bucket.

Conceptualmente:

```text
Carpeta local
     │
     ├── archivo1
     ├── archivo2
     └── archivo3
             │
             ▼
           S3
```

`sync` es especialmente útil cuando tenemos varios archivos.

---

# 15. Eliminar todo el contenido del bucket

Se utilizó:

```bash
aws s3 rm s3://xideralaws-curso-rodrigo/ --recursive
```

La opción:

```text
--recursive
```

hace que la operación se aplique recursivamente a los objetos encontrados.

**Precaución:** este comando puede eliminar muchos archivos, por lo que debe ejecutarse únicamente cuando realmente se quiera vaciar el contenido correspondiente.

---

# 16. Acceso a S3 utilizando Python y Boto3

Además de AWS CLI, se utilizó Python.

La biblioteca utilizada es:

```python
import boto3
```

**Boto3** es el SDK de AWS para Python.

Permite que un programa Python interactúe con servicios de AWS como:

- S3
- EC2
- DynamoDB
- Lambda
- SQS
- SNS
- entre otros.

---

# 17. Definir el bucket

Se definió:

```python
BUCKET = "xideralaws-curso-rodrigo"
```

Guardar el nombre en una variable evita tener que escribirlo repetidamente.

---

# 18. Crear un cliente S3

En Python:

```python
import boto3

s3 = boto3.client("s3")
```

Aquí:

```python
boto3.client("s3")
```

crea un cliente para comunicarse con Amazon S3.

Después podemos utilizar:

```python
s3
```

para realizar operaciones.

---

# 19. Crear un archivo desde Jupyter

Para generar un archivo de texto desde una celda de Jupyter:

```python
!echo "hola mundo" > hola.txt
```

El signo:

```text
!
```

permite ejecutar un comando de la terminal desde una celda de Jupyter Notebook.

El comando:

```bash
echo "hola mundo"
```

imprime el texto.

El operador:

```text
>
```

redirige la salida hacia un archivo.

Por lo tanto:

```text
echo "hola mundo" > hola.txt
```

crea `hola.txt` con:

```text
hola mundo
```

---

# 20. Subir información a S3 con Boto3

Se utilizó:

```python
s3.put_object(
    Bucket=BUCKET,
    Key="hola_desde_boto3.txt",
    Body="Hola desde Python y Boto3"
)
```

Este método crea un objeto directamente en S3.

### `Bucket`

```python
Bucket=BUCKET
```

indica en qué bucket se almacenará el objeto.

### `Key`

```python
Key="hola_desde_boto3.txt"
```

es el nombre/ruta del objeto dentro de S3.

### `Body`

```python
Body="Hola desde Python y Boto3"
```

es el contenido que se guardará.

El resultado conceptual es:

```text
Python
  │
  │ boto3
  ▼
Amazon S3
  │
  └── hola_desde_boto3.txt
          │
          └── "Hola desde Python y Boto3"
```

---

# 21. Jupyter Notebook dentro de EC2

Para iniciar Jupyter se utilizó:

```bash
jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```

Cada opción tiene una función.

### `--no-browser`

Indica que Jupyter no debe intentar abrir un navegador dentro de la instancia EC2.

Esto tiene sentido porque EC2 es un servidor remoto.

### `--ip=0.0.0.0`

Hace que Jupyter escuche conexiones en las interfaces de red disponibles, en lugar de limitarse a `localhost`.

### `--port=8888`

Indica que Jupyter utilizará el puerto:

```text
8888
```

---

# 22. Acceder a Jupyter desde el navegador

Una vez iniciado Jupyter, se obtiene la dirección IP pública de la instancia EC2.

Por ejemplo:

```text
54.193.77.158
```

La aplicación se consulta mediante:

```text
http://54.193.77.158:8888
```

Para que esto funcione, el **Security Group de EC2** debe permitir tráfico entrante al puerto correspondiente.

> Para un entorno real no se recomienda dejar Jupyter expuesto públicamente de esta manera sin controles adicionales de seguridad.

---

# 23. Dataset de Spotify 2023

La siguiente parte de la práctica consistió en trabajar con un dataset de Spotify de 2023.

El objetivo fue:

```text
Dataset CSV
     ↓
Amazon S3
     ↓
Boto3
     ↓
Python
     ↓
Pandas
     ↓
DataFrame
     ↓
Streamlit
```

El archivo utilizado fue:

```text
spotify-2023.csv
```

---

# 24. Crear una aplicación Streamlit

Primero se creó un archivo:

```text
app.py
```

Una aplicación mínima de Streamlit:

```python
import streamlit as st

st.title("Hola mundo")
st.write("Mi primera app con streamlit")
```

---

# 25. ¿Qué es Streamlit?

**Streamlit** es una herramienta de Python que permite crear aplicaciones web interactivas principalmente para visualización y análisis de datos.

En lugar de construir manualmente:

```text
HTML
CSS
JavaScript
Backend
```

podemos crear una aplicación básica utilizando Python.

Por ejemplo:

```python
st.title("Hola mundo")
```

genera un título en la aplicación.

Y:

```python
st.write("Mi primera app con streamlit")
```

muestra contenido.

---

# 26. Aplicación conectada a S3

La aplicación completa utilizada en la práctica fue:

```python
import streamlit as st
import boto3
import pandas as pd
import io

BUCKET = "xideralaws-curso-benjamin"
KEY = "spotify-2023.csv"

s3 = boto3.client("s3")

response = s3.get_object(
    Bucket=BUCKET,
    Key=KEY
)

contenido = response["Body"].read()

df = pd.read_csv(
    io.BytesIO(contenido),
    encoding="latin-1",
    sep=","
)

st.title("Hola mundo")
st.write("Mi primera app con streamlit")
st.dataframe(df.head())
```

> En esta parte aparece un segundo bucket: `xideralaws-curso-benjamin`. Se debe utilizar el nombre real del bucket que contenga el CSV. Si el CSV está en `xideralaws-curso-rodrigo`, hay que cambiar la variable `BUCKET`.

---

# 27. Explicación del código de Streamlit

## Importar Streamlit

```python
import streamlit as st
```

Permite utilizar las funciones de Streamlit.

---

## Importar Boto3

```python
import boto3
```

Permite conectarse con AWS desde Python.

---

## Importar pandas

```python
import pandas as pd
```

Pandas permite trabajar con datos tabulares.

El resultado será un:

```text
DataFrame
```

---

## Importar io

```python
import io
```

La biblioteca `io` permite trabajar con datos en memoria como si fueran archivos.

Esto es útil porque el CSV se obtiene directamente desde S3.

---

# 28. Obtener un objeto de S3

Se define:

```python
response = s3.get_object(
    Bucket=BUCKET,
    Key=KEY
)
```

Aquí:

```python
Bucket=BUCKET
```

indica el bucket.

Y:

```python
Key=KEY
```

indica el objeto:

```text
spotify-2023.csv
```

S3 devuelve una respuesta que contiene, entre otras cosas, el cuerpo del objeto.

---

# 29. Leer el contenido

Se utilizó:

```python
contenido = response["Body"].read()
```

Esto lee los bytes del archivo.

Conceptualmente:

```text
S3
 │
 │ get_object()
 ▼
response
 │
 ▼
Body
 │
 │ read()
 ▼
contenido
```

---

# 30. Convertir el CSV a DataFrame

Se utilizó:

```python
df = pd.read_csv(
    io.BytesIO(contenido),
    encoding="latin-1",
    sep=","
)
```

`pd.read_csv()` permite leer un archivo CSV y convertirlo en un DataFrame.

---

## `io.BytesIO(contenido)`

Convierte los bytes obtenidos de S3 en un objeto que pandas puede tratar como archivo.

---

## `encoding="latin-1"`

Indica la codificación utilizada para interpretar los caracteres del CSV.

---

## `sep=","`

Indica que las columnas están separadas por comas.

---

# 31. Mostrar el DataFrame en Streamlit

Se utilizó:

```python
st.dataframe(df.head())
```

`df.head()` obtiene las primeras filas del DataFrame.

Por defecto:

```python
df.head()
```

muestra las primeras 5 filas.

`st.dataframe()` las presenta como una tabla interactiva en la aplicación web.

---

# 32. Ejecutar Streamlit

La aplicación puede ejecutarse con:

```bash
streamlit run app.py
```

Streamlit normalmente informa una dirección local y otra dirección de red.

La aplicación utiliza normalmente:

```text
8501
```

como puerto.

---

# 33. Acceso a la aplicación desde EC2

Al ejecutar Streamlit en EC2, la aplicación puede quedar disponible mediante:

```text
http://IP_PUBLICA:8501
```

Por ejemplo:

```text
http://54.193.77.158:8501/
```

La estructura es:

```text
http://
   │
   ├── IP pública de EC2
   │             │
   │             └── 54.193.77.158
   │
   └── puerto 8501
```

Para que sea accesible desde Internet, el Security Group debe permitir el tráfico entrante al puerto `8501`.

---

# 34. Crear el repositorio de GitHub

Se creó un repositorio nuevo llamado:

```text
practicas-streamlit
```

La idea es almacenar ahí el código de la aplicación y los archivos necesarios para construir y desplegar el proyecto.

Una estructura posible es:

```text
practicas-streamlit/
│
├── app.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
│
└── .github/
    └── workflows/
        └── Docker.compose.yml
```

---

# 35. `requirements.txt`

El archivo:

```text
requirements.txt
```

contiene las dependencias Python que necesita la aplicación.

Para este proyecto, como mínimo se requieren:

```text
streamlit
boto3
pandas
```

Esto permite que Docker instale las mismas dependencias al construir la aplicación.

---

# 36. Dockerfile

Se creó un:

```text
Dockerfile
```

El Dockerfile define cómo construir la imagen de la aplicación.

Una versión sencilla podría ser:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.address=0.0.0.0", "--server.port=8501"]
```

---

# 37. Explicación del Dockerfile

## Imagen base

```dockerfile
FROM python:3.12-slim
```

Utiliza una imagen de Python como base.

---

## Directorio de trabajo

```dockerfile
WORKDIR /app
```

Define:

```text
/app
```

como directorio de trabajo dentro del contenedor.

---

## Copiar dependencias

```dockerfile
COPY requirements.txt .
```

Copia el archivo `requirements.txt` al contenedor.

---

## Instalar dependencias

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

Instala las librerías necesarias.

---

## Copiar aplicación

```dockerfile
COPY app.py .
```

Copia el código de Streamlit.

---

## Exponer puerto

```dockerfile
EXPOSE 8501
```

Indica que la aplicación utiliza el puerto:

```text
8501
```

`EXPOSE` documenta el puerto que utiliza el contenedor; el acceso real desde EC2 depende también de la publicación del puerto y de la configuración de red.

---

## Ejecutar Streamlit

```dockerfile
CMD ["streamlit", "run", "app.py", "--server.address=0.0.0.0", "--server.port=8501"]
```

Inicia Streamlit cuando se ejecuta el contenedor.

El parámetro:

```text
--server.address=0.0.0.0
```

es importante para que Streamlit pueda recibir conexiones desde fuera del contenedor.

---

# 38. Docker Compose

También se creó:

```text
docker-compose.yml
```

Un ejemplo:

```yaml
services:
  streamlit:
    build: .
    ports:
      - "8501:8501"
    restart: unless-stopped
```

La parte:

```yaml
ports:
  - "8501:8501"
```

conecta:

```text
Puerto 8501 de EC2
        │
        ▼
Puerto 8501 del contenedor
```

Por eso podemos acceder mediante:

```text
http://IP_PUBLICA:8501
```

---

# 39. Variables de entorno y credenciales

Durante la práctica también se definieron variables relacionadas con AWS:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

Sin embargo, es importante distinguir entre **configuración local** y **secretos de GitHub**.

Las claves secretas no deben colocarse directamente en:

```text
app.py
Dockerfile
docker-compose.yml
requirements.txt
GitHub
```

ni deben escribirse directamente en el repositorio.

---

# 40. GitHub Actions

GitHub Actions permite automatizar tareas cuando ocurren eventos en un repositorio.

En esta práctica se utilizó para automatizar el despliegue.

El flujo conceptual es:

```text
GitHub
   │
   │ push
   ▼
GitHub Actions
   │
   ▼
Self-hosted Runner
   │
   ▼
EC2
   │
   ▼
Docker Compose
   │
   ▼
Streamlit
```

---

# 41. Self-hosted Runner

Un **self-hosted runner** es una máquina que ejecuta los trabajos de GitHub Actions utilizando infraestructura propia.

En este caso:

```text
EC2 = máquina que ejecuta el runner
```

Por lo tanto, GitHub puede enviarle instrucciones como:

```text
descargar código
construir imagen
ejecutar Docker Compose
reiniciar aplicación
```

---

# 42. Crear el runner en EC2

En GitHub se entra al repositorio y posteriormente a:

```text
Settings
    ↓
Actions
    ↓
Runners
    ↓
New self-hosted runner
```

GitHub proporciona instrucciones y comandos específicos para registrar la máquina.

Se ejecutan en EC2.

Es importante seguir los comandos que proporciona GitHub para la arquitectura y sistema operativo correspondientes.

---

# 43. Servicio del runner

Después de descargar y configurar el runner, se utilizaron comandos como:

```bash
sudo ./svc.sh install
```

y:

```bash
sudo ./svc.sh start
```

---

## `svc.sh install`

Instala el runner como un servicio del sistema.

Esto permite que el runner pueda ejecutarse como servicio.

---

## `svc.sh start`

Inicia el servicio.

Después GitHub puede mostrar el runner como disponible/activo.

---

# 44. Docker en EC2

Para preparar Docker se utilizaron comandos como:

```bash
sudo apt-get update
```

Esto actualiza la información de los paquetes disponibles.

Después:

```bash
sudo apt-get install docker-compose-plugin
```

instala el plugin de Docker Compose proporcionado por los repositorios disponibles para el sistema.

La disponibilidad exacta del paquete depende de la configuración y versión de Ubuntu.

---

# 45. Docker Compose

Docker Compose permite definir varios aspectos de la aplicación en un archivo YAML.

En lugar de ejecutar manualmente múltiples comandos de Docker, podemos definir:

```yaml
services:
  streamlit:
    ...
```

y posteriormente utilizar:

```bash
docker compose up -d
```

El `-d` significa que los contenedores se ejecutan en segundo plano.

Para detenerlos:

```bash
docker compose down
```

---

# 46. Workflow de GitHub Actions

Dentro del repositorio se creó:

```text
.github/workflows/Docker.compose.yml
```

Este archivo define qué debe hacer GitHub Actions.

Un ejemplo simplificado sería:

```yaml
name: Deploy Streamlit

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Obtener código
        uses: actions/checkout@v4

      - name: Construir y ejecutar
        run: docker compose up -d --build
```

---

# 47. ¿Qué significa `runs-on: self-hosted`?

Esta línea:

```yaml
runs-on: self-hosted
```

indica que el workflow debe ejecutarse en un runner administrado por nosotros.

En esta práctica:

```text
runs-on: self-hosted
          │
          ▼
Runner instalado en EC2
```

Por lo tanto, los comandos del workflow se ejecutan directamente en la instancia EC2.

---

# 48. ¿Qué hace `actions/checkout`?

Esta acción:

```yaml
uses: actions/checkout@v4
```

permite que el runner descargue el código del repositorio.

Así, dentro de EC2 estará disponible:

```text
app.py
Dockerfile
docker-compose.yml
requirements.txt
.github/
```

y Docker podrá construir la aplicación.

---

# 49. Construcción y despliegue

Con:

```bash
docker compose up -d --build
```

Docker Compose:

1. Lee `docker-compose.yml`.
2. Construye la imagen si es necesario.
3. Ejecuta el contenedor.
4. Publica el puerto configurado.
5. Mantiene el proceso en segundo plano.

El flujo es:

```text
GitHub
  ↓
Actions
  ↓
EC2 Runner
  ↓
docker compose up -d --build
  ↓
Docker
  ↓
Streamlit
  ↓
8501
```

---

# 50. El check verde de GitHub Actions

Después de hacer un `push`, GitHub Actions ejecuta el workflow.

En el repositorio se puede entrar a:

```text
Actions
```

y observar la ejecución.

Si todo termina correctamente, GitHub muestra una marca:

```text
✓
```

Esto significa que el workflow terminó correctamente.

Es importante recordar que un workflow exitoso significa que **los pasos definidos por el workflow finalizaron sin error**; aun así, una aplicación podría tener problemas de ejecución que deben comprobarse por separado.

---

# 51. Acceder a Streamlit después del despliegue

Una vez que:

```text
GitHub Actions
      ↓
EC2
      ↓
Docker Compose
      ↓
Contenedor
      ↓
Streamlit
```

está funcionando, la aplicación puede consultarse mediante:

```text
http://IP_PUBLICA:8501/
```

Por ejemplo, durante la práctica se utilizó una IP como:

```text
http://54.193.77.158:8501/
```

La IP pública de una instancia EC2 puede cambiar, por lo que no debe considerarse permanente a menos que se utilice una dirección reservada, como una Elastic IP.

---

# 52. Flujo completo de la práctica

El proceso completo puede resumirse así:

```text
1. Crear / utilizar EC2
        │
        ▼
2. Preparar Ubuntu
        │
        ├── unzip
        ├── PATH
        ├── AWS CLI
        └── Zsh
        │
        ▼
3. Configurar AWS
        │
        ├── Access Key
        ├── Secret Key
        ├── Region
        └── Output
        │
        ▼
4. Verificar identidad
        │
        └── aws sts get-caller-identity
        │
        ▼
5. Trabajar con S3
        │
        ├── cp
        ├── ls
        ├── rm
        └── sync
        │
        ▼
6. Utilizar Boto3
        │
        ▼
7. Obtener CSV desde S3
        │
        ▼
8. Leer CSV con Pandas
        │
        ▼
9. Crear app.py
        │
        ▼
10. Ejecutar Streamlit
        │
        ▼
11. Crear Dockerfile
        │
        ▼
12. Crear docker-compose.yml
        │
        ▼
13. Crear repositorio GitHub
        │
        ▼
14. Crear Self-hosted Runner
        │
        ▼
15. Configurar GitHub Actions
        │
        ▼
16. Push al repositorio
        │
        ▼
17. GitHub Actions ejecuta Deploy
        │
        ▼
18. EC2 ejecuta Docker Compose
        │
        ▼
19. Streamlit queda disponible
        │
        ▼
http://IP_PUBLICA:8501/
```

---

# 53. Conceptos principales aprendidos

## EC2

Servicio de AWS que proporciona máquinas virtuales en la nube.

En esta práctica se utilizó como servidor para:

- Jupyter
- Python
- Streamlit
- Docker
- GitHub Actions Runner

---

## S3

Servicio de almacenamiento de objetos.

En esta práctica se utilizó para almacenar:

- archivos de texto
- archivos CSV
- dataset de Spotify

---

## AWS CLI

Herramienta de línea de comandos para interactuar con AWS.

Ejemplos:

```bash
aws configure
aws sts get-caller-identity
aws s3 ls
aws s3 cp
aws s3 rm
aws s3 sync
```

---

## Boto3

SDK de AWS para Python.

Permite interactuar con AWS desde programas Python.

Ejemplo:

```python
s3 = boto3.client("s3")
```

---

## Pandas

Biblioteca de Python para análisis y manipulación de datos.

En la práctica:

```python
df = pd.read_csv(...)
```

permitió convertir el CSV de Spotify en un DataFrame.

---

## Streamlit

Framework/herramienta para crear aplicaciones web de datos utilizando Python.

Ejemplo:

```python
st.title("Hola mundo")
st.dataframe(df.head())
```

---

## Docker

Permite empaquetar la aplicación y sus dependencias dentro de un contenedor.

Esto facilita que la aplicación tenga un entorno reproducible.

---

## Docker Compose

Permite definir y administrar la ejecución de contenedores mediante un archivo YAML.

---

## GitHub Actions

Permite automatizar procesos como:

- pruebas
- construcción
- despliegues
- tareas de mantenimiento

En esta práctica se utilizó para automatizar el despliegue de Streamlit.

---

## Self-hosted Runner

Es una máquina administrada por nosotros que ejecuta los jobs de GitHub Actions.

En este caso:

```text
EC2
  └── GitHub Actions Runner
```

---

# 54. Diferencia entre AWS CLI y Boto3

Aunque ambos permiten trabajar con AWS, funcionan de manera diferente.

### AWS CLI

Se utiliza desde la terminal:

```bash
aws s3 ls
```

### Boto3

Se utiliza desde Python:

```python
s3 = boto3.client("s3")
s3.get_object(...)
```

La relación puede visualizarse así:

```text
                Amazon S3
                ▲       ▲
                │       │
             AWS CLI  Boto3
                │       │
             Terminal Python
```

AWS CLI es muy práctico para operaciones rápidas desde la terminal.

Boto3 es especialmente útil cuando queremos que nuestro programa Python interactúe automáticamente con AWS.

---

# 55. Diferencia entre S3 y EC2

Es importante no confundir ambos servicios.

### EC2

Proporciona capacidad de cómputo.

Podemos ejecutar:

```text
Python
Jupyter
Docker
Streamlit
```

### S3

Proporciona almacenamiento.

Podemos guardar:

```text
CSV
TXT
JSON
imágenes
archivos
datasets
```

Por eso en esta práctica se utilizaron juntos:

```text
EC2
 │
 │ ejecuta Python
 │
 ▼
Boto3
 │
 │ solicita archivo
 ▼
S3
 │
 └── spotify-2023.csv
```

---

# 56. Seguridad

Durante esta práctica se utilizaron credenciales de AWS, por lo que es especialmente importante recordar:

## Nunca subir Access Keys a GitHub

No se debe hacer:

```python
AWS_ACCESS_KEY = "AKIA..."
AWS_SECRET_KEY = "..."
```

dentro de:

```text
app.py
```

ni subir archivos que contengan las credenciales.

---

## Variables de entorno

Cuando una aplicación necesita secretos, es preferible utilizar variables de entorno o mecanismos de identidad de AWS apropiados.

Para GitHub Actions se pueden configurar secretos en:

```text
Repository
    ↓
Settings
    ↓
Secrets and variables
    ↓
Actions
```

Los valores sensibles deben almacenarse como **Secrets**, no como texto visible dentro del repositorio.

---

# 57. Flujo de datos de Spotify

La parte de datos puede resumirse de esta forma:

```text
Spotify 2023 CSV
       │
       ▼
Amazon S3
       │
       │ boto3.get_object()
       ▼
response["Body"]
       │
       │ read()
       ▼
bytes
       │
       │ BytesIO
       ▼
pandas.read_csv()
       │
       ▼
DataFrame
       │
       ▼
Streamlit
       │
       ▼
Tabla en navegador
```

Esto demuestra una integración entre:

```text
AWS + Python + S3 + Pandas + Streamlit
```

---

# 58. Flujo de despliegue

Finalmente, el despliegue se puede representar así:

```text
Desarrollador
     │
     │ git push
     ▼
GitHub Repository
     │
     ▼
GitHub Actions
     │
     ▼
Self-hosted Runner
     │
     │ EC2
     ▼
Docker Compose
     │
     ▼
Docker Container
     │
     ▼
Streamlit
     │
     ▼
Puerto 8501
     │
     ▼
Navegador
```

Este flujo permite que una modificación del proyecto pueda llegar automáticamente a la instancia EC2 mediante GitHub Actions.

---

# 59. Resumen final

La práctica permitió construir un pequeño flujo de despliegue en la nube.

Primero se preparó una instancia **EC2** y se configuró **AWS CLI** para poder administrar recursos de AWS desde la terminal. Después se trabajó con **Amazon S3**, realizando operaciones como subir, consultar, sincronizar y eliminar archivos.

Posteriormente se utilizó **Boto3** para acceder a S3 desde Python. Esto permitió obtener el dataset `spotify-2023.csv`, procesarlo con **Pandas** y convertirlo en un DataFrame.

Con **Streamlit** se creó una aplicación web capaz de mostrar los datos.

Finalmente, la aplicación se empaquetó mediante **Docker**, se definió su ejecución con **Docker Compose** y se configuró un **self-hosted runner** en EC2 para que **GitHub Actions** pudiera automatizar el despliegue.

El resultado final fue una aplicación Streamlit ejecutándose dentro de un contenedor Docker en EC2 y accesible mediante:

```text
http://IP_PUBLICA:8501/
```

Este ejercicio integra varios conceptos fundamentales de desarrollo y nube:

```text
AWS
├── EC2       → cómputo
├── S3        → almacenamiento
└── STS       → identidad

Python
├── Boto3     → comunicación con AWS
├── Pandas    → procesamiento de datos
└── Streamlit → aplicación web

DevOps
├── Docker
├── Docker Compose
├── GitHub Actions
└── Self-hosted Runner
```

En conjunto, el ejercicio muestra cómo pasar de un archivo de datos almacenado en la nube hasta una aplicación web desplegada y automatizada sobre infraestructura de AWS.
