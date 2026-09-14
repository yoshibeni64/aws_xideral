# WordCount con Hadoop y HDFS

En este ejercicio utilizamos **Docker, Hadoop, HDFS y MapReduce** para contar las palabras de un archivo de texto (`Frankenstein`).

El flujo general es:

```text
Docker
  ↓
Contenedor Hadoop
  ↓
HDFS
  ↓
pg84.txt
  ↓
MapReduce - WordCount
  ↓
/output/part-r-00000
```

---

## 1. Crear la red de Docker

```bash
sudo docker network create --driver=bridge hadoop
```

Crea una red de Docker llamada `hadoop`.

### ¿Para qué sirve?

Los contenedores que estén conectados a esta red pueden comunicarse entre ellos.

El parámetro:

```bash
--driver=bridge
```

indica que se utilizará una red tipo **bridge**, que es la forma común de comunicación entre contenedores Docker.

---

## 2. Entrar a la carpeta de Hadoop

```bash
cd hadoop
```

Cambia la ubicación actual de la terminal a la carpeta `hadoop`.

Esta carpeta contiene los archivos necesarios para levantar el entorno de Hadoop.

---

## 3. Iniciar el contenedor de Hadoop

```bash
sudo ./start-container.sh
```

Ejecuta el script `start-container.sh`.

Este script se encarga de iniciar el contenedor Docker preparado para trabajar con Hadoop.

Después de ejecutarlo, estamos trabajando dentro del entorno del contenedor, por ejemplo:

```text
root@hadoop-master:~#
```

---

## 4. Iniciar Hadoop

```bash
./start-hadoop.sh
```

Inicia los servicios necesarios de Hadoop.

Entre ellos se encuentran componentes de **HDFS**, como:

- NameNode
- DataNode
- SecondaryNameNode

Podemos comprobar los procesos con:

```bash
jps
```

Por ejemplo:

```text
NameNode
DataNode
SecondaryNameNode
```

### ¿Por qué es importante?

HDFS necesita que el **NameNode** y los **DataNodes** estén funcionando para poder guardar y leer archivos.

---

# 5. Descargar Frankenstein

```bash
wget https://www.gutenberg.org/cache/epub/84/pg84.txt
```

Descarga el texto de *Frankenstein* desde Project Gutenberg.

El archivo descargado se llama:

```text
pg84.txt
```

En este momento el archivo está en el **sistema de archivos local del contenedor**, todavía no en HDFS.

Podemos comprobarlo con:

```bash
ls
```

---

# 6. Crear una carpeta local llamada input

```bash
mkdir input
```

Crea una carpeta llamada:

```text
input
```

Esta carpeta pertenece al sistema de archivos normal del contenedor.

Es importante distinguir:

```text
input local
```

de:

```text
/input en HDFS
```

No son la misma carpeta.

---

# 7. Crear `/input` en HDFS

```bash
hdfs dfs -mkdir -p /input
```

Crea una carpeta llamada `/input` dentro de **HDFS**.

### `hdfs dfs`

Es el comando utilizado para trabajar con archivos y directorios de HDFS.

Es parecido a utilizar:

```bash
ls
mkdir
cp
```

en Linux, pero aplicado al sistema de archivos distribuido de Hadoop.

### `-mkdir`

Crea un directorio.

### `-p`

Permite crear los directorios necesarios aunque los directorios superiores todavía no existan.

---

# 8. Copiar Frankenstein a la carpeta local `input`

```bash
cp pg84.txt input/
```

Copia:

```text
pg84.txt
```

a:

```text
input/pg84.txt
```

Todavía estamos trabajando en el sistema de archivos local.

Podemos comprobarlo:

```bash
ls input
```

Resultado esperado:

```text
pg84.txt
```

---

# 9. Subir el archivo a HDFS

```bash
hdfs dfs -put -f input/pg84.txt /input/
```

Este es uno de los pasos más importantes.

Copia el archivo desde el sistema de archivos local:

```text
input/pg84.txt
```

hacia HDFS:

```text
/input/pg84.txt
```

### `-put`

Sirve para subir/copiar archivos desde el sistema de archivos local hacia HDFS.

### `-f`

Indica que se puede sobrescribir el archivo si ya existe.

Por ejemplo:

```text
Local
└── input
    └── pg84.txt
```

se convierte en:

```text
HDFS
└── input
    └── pg84.txt
```

Podemos comprobarlo con:

```bash
hdfs dfs -ls /input
```

---

# 10. Ejecutar WordCount

```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.7.2-sources.jar \
org.apache.hadoop.examples.WordCount /input /output
```

Este comando ejecuta el programa **WordCount de MapReduce**.

Su objetivo es:

> Leer un archivo de texto, contar cuántas veces aparece cada palabra y guardar los resultados.

---

## ¿Qué significa cada parte?

### `hadoop jar`

Ejecuta un programa Java empaquetado dentro de un archivo `.jar` utilizando Hadoop.

---

### `$HADOOP_HOME`

Es una variable de entorno que apunta a la instalación de Hadoop.

Por ejemplo, podría apuntar a:

```text
/usr/local/hadoop
```

Por lo tanto:

```bash
$HADOOP_HOME/share/hadoop/...
```

permite localizar los archivos de Hadoop sin escribir toda la ruta manualmente.

---

### `hadoop-mapreduce-examples-2.7.2-sources.jar`

Es un archivo `.jar` que contiene diferentes ejemplos de MapReduce.

Entre ellos está:

```text
org.apache.hadoop.examples.WordCount
```

---

### `org.apache.hadoop.examples.WordCount`

Es la clase Java que ejecuta el algoritmo WordCount.

Conceptualmente hace algo parecido a:

```text
Frankenstein Frankenstein monster
```

y genera pares:

```text
Frankenstein 1
Frankenstein 1
monster 1
```

Después MapReduce combina los valores:

```text
Frankenstein 2
monster 1
```

---

### `/input`

Es la carpeta de **entrada en HDFS**.

WordCount leerá los archivos que se encuentran ahí.

En nuestro caso:

```text
/input/pg84.txt
```

---

### `/output`

Es la carpeta de **salida en HDFS**.

Aquí Hadoop guardará los resultados.

Por ejemplo:

```text
/output
├── _SUCCESS
└── part-r-00000
```

---

# 11. Mostrar el resultado

```bash
hdfs dfs -cat /output/part-r-00000
```

Lee el archivo de resultado que generó WordCount.

`-cat` muestra el contenido de un archivo de HDFS en la terminal.

El resultado tendrá una estructura similar a:

```text
a       1234
about   53
after   87
...
monster 15
...
```

El formato es:

```text
palabra    cantidad
```

Por ejemplo:

```text
monster    15
```

significa que WordCount encontró la palabra `monster` **15 veces**.

---

# 12. Buscar solamente una palabra

Si queremos consultar únicamente `monster`:

```bash
hdfs dfs -cat /output/part-r-00000 | grep -i "^monster[[:space:]]"
```

### `|`

El operador pipe pasa la salida del primer comando al segundo.

Es decir:

```text
hdfs dfs -cat
       ↓
      grep
```

### `grep`

Busca texto dentro de la salida.

### `-i`

Ignora mayúsculas y minúsculas.

Por lo tanto:

```text
monster
Monster
MONSTER
```

pueden coincidir.

### `^monster`

El símbolo `^` indica que `monster` debe aparecer al inicio de la línea.

### `[[:space:]]`

Indica que después de `monster` debe existir un espacio o carácter de separación.

Así evitamos coincidir accidentalmente con palabras como:

```text
monsters
monstering
```

---

# Resumen del proceso

Los comandos realizan este flujo:

```text
1. Crear red Docker
       ↓
2. Iniciar contenedor Hadoop
       ↓
3. Iniciar servicios Hadoop
       ↓
4. Descargar Frankenstein
       ↓
5. Crear carpeta local
       ↓
6. Crear /input en HDFS
       ↓
7. Copiar Frankenstein a input local
       ↓
8. Subir Frankenstein a HDFS
       ↓
9. Ejecutar WordCount
       ↓
10. Guardar resultado en /output
       ↓
11. Leer part-r-00000
```

La diferencia fundamental que hay que recordar es:

```text
Sistema de archivos local
        ↓
input/pg84.txt
        ↓
hdfs dfs -put
        ↓
HDFS
        ↓
/input/pg84.txt
```

Y posteriormente:

```text
/input/pg84.txt
        ↓
    WordCount
        ↓
/output/part-r-00000
        ↓
palabra + cantidad
```