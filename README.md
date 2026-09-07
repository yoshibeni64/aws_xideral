# Evidencias del curso de AWS

## 1. Creación y publicación de un "Hola Mundo"

Creamos un repositorio en GitHub con un archivo `index.html` que mostraba un mensaje de **"Hola Mundo"**.

Posteriormente, utilizamos **GitHub Pages** para realizar el deployment y publicar la página en Internet mediante una URL.

---

## 2. Creación del portafolio de evidencias

Actualizamos el archivo `index.html` y lo convertimos en un **portafolio de evidencias** para organizar nuestros trabajos, ejercicios y certificaciones realizados durante el curso.

---

## 3. Creación de una instancia EC2

En nuestra cuenta de AWS creamos una instancia **EC2** con las siguientes características:

* **Región:** US West (N. California)
* **Sistema operativo:** Ubuntu
* **Tipo de instancia:** `t3.micro`
* **Key pair:** `rodrigo`
* **Security Group:** `launch-wizard-1`

---

## 4. Acceso a la instancia EC2

Una vez creada la instancia, nos conectamos a ella utilizando el usuario:

```bash
ubuntu
```

De esta manera pudimos acceder a la terminal de nuestra instancia EC2 y comenzar a trabajar con Linux.

---

## 5. Práctica de comandos de Linux

En nuestra instancia EC2 practicamos diferentes comandos básicos de Linux.

### Limpiar la terminal

```bash
clear
```

### Crear un archivo

```bash
touch hola.txt
```

### Listar archivos

```bash
ls
```

### Eliminar un archivo

```bash
rm -rf hola.txt
```

### Escribir contenido en un archivo

```bash
echo "hola mundo" > hola.txt
```

### Mostrar texto en la terminal

```bash
echo "hola mundo"
```

### Mostrar el contenido de un archivo

```bash
cat hola.txt
```

### Copiar un archivo

```bash
cp hola.txt hola_copia.txt
```

### Modificar el contenido de un archivo

```bash
echo "hola mundo" > hola_copia.txt
```

Posteriormente modificamos nuevamente su contenido:

```bash
echo "adios mundo" > hola_copia.txt
```

Comprobamos el contenido:

```bash
cat hola_copia.txt
```

### Consultar el historial de comandos

```bash
history
```

---

## 6. Búsqueda de comandos en el historial

Utilizamos `history` junto con `grep` para buscar comandos específicos que habíamos ejecutado anteriormente.

Por ejemplo, para buscar los comandos relacionados con `rm`:

```bash
history | grep "rm"
```

Esto permite localizar rápidamente comandos específicos dentro del historial de la terminal.

---

## 7. Actualización del sistema

Actualizamos los paquetes disponibles y posteriormente instalamos sus actualizaciones:

```bash
sudo apt update
```

```bash
sudo apt upgrade
```

`apt update` actualiza la información de los repositorios, mientras que `apt upgrade` instala las actualizaciones disponibles.

---

## 8. Instalación de Zsh y Oh My Zsh

Instalamos **Zsh**, un shell alternativo a Bash, y posteriormente instalamos **Oh My Zsh** para facilitar su configuración y personalización.

Instalamos Zsh con:

```bash
sudo apt install zsh
```

Después instalamos Oh My Zsh mediante:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 9. Instalación de Docker

Instalamos **Docker Engine** en nuestra instancia EC2 siguiendo la documentación oficial de Docker para Ubuntu.

Docker nos permitirá posteriormente crear y ejecutar **contenedores** dentro de nuestra instancia.

---

## 10. Instalación de dependencias para Python

Instalamos las dependencias necesarias para posteriormente utilizar `pyenv` y compilar diferentes versiones de Python:

```bash
sudo apt install -y \
    make \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    curl \
    llvm \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libxml2-dev \
    libxmlsec1-dev \
    libffi-dev \
    liblzma-dev
```

---

## 11. Instalación de pyenv

Clonamos el repositorio de **pyenv**, una herramienta que permite instalar y administrar diferentes versiones de Python:

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

Comprobamos que el repositorio se haya descargado correctamente:

```bash
ls ~/.pyenv
```

---

## 12. Configuración de pyenv

Agregamos las configuraciones necesarias para que `pyenv` pueda ser utilizado desde nuestra terminal Zsh:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
```

```bash
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
```

```bash
echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc
```

Recargamos la configuración de Zsh para aplicar los cambios:

```bash
source ~/.zshrc
```

Finalmente, comprobamos que `pyenv` esté instalado correctamente:

```bash
pyenv --version
```

Si el comando devuelve la versión instalada de `pyenv`, significa que la configuración se realizó correctamente.

---

## 13. Instalación y configuración de Python 3.14.7

Utilizamos `pyenv` para instalar la versión **Python 3.14.7**:

```bash
pyenv install 3.14.7
```

Una vez instalada, configuramos esta versión como la versión global de Python:

```bash
pyenv global 3.14.7
```

Comprobamos que la versión configurada sea la correcta:

```bash
python --version
```

El resultado esperado es:

```text
Python 3.14.7
```

De esta manera, Python 3.14.7 queda configurado como la versión global de Python para nuestro usuario.

---

## 14. Creación de un entorno virtual de Python

Creamos un entorno virtual utilizando el módulo `venv` de Python:

```bash
python -m venv .venv
```

El entorno virtual se crea dentro de la carpeta `.venv`, permitiendo mantener las dependencias de este proyecto aisladas del resto del sistema.

Posteriormente activamos el entorno virtual:

```bash
source .venv/bin/activate
```

Al activarlo, la terminal muestra el nombre del entorno al inicio de la línea de comandos:

```text
(.venv) usuario@ubuntu:~/jupyter$
```

Esto indica que actualmente estamos trabajando dentro del entorno virtual `.venv`.

---

## 15. Instalación de Jupyter Notebook

Con el entorno virtual `.venv` activado, instalamos **Jupyter Notebook** y `ipykernel`:

```bash
pip install notebook
```

```bash
pip install ipykernel
```

Al realizar la instalación con el entorno virtual activo, estas dependencias se instalan dentro de `.venv`, manteniéndolas aisladas del Python global.

---

## 16. Ejecución de Jupyter Notebook

Con el entorno virtual `.venv` activado, iniciamos Jupyter Notebook:

```bash
jupyter notebook
```

Esto inicia el servidor de **Jupyter Notebook** utilizando el entorno virtual y proporciona una URL para acceder a la interfaz desde el navegador.

Desde esta interfaz podemos crear y ejecutar archivos `.ipynb` para trabajar con Python de manera interactiva.

El flujo utilizado fue:

```text
Python 3.14.7
      ↓
pyenv global
      ↓
python -m venv .venv
      ↓
source .venv/bin/activate
      ↓
pip install notebook
      ↓
jupyter notebook
```
