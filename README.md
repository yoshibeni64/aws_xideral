# Evidencias del curso de AWS

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#evidencias-del-curso-de-aws)

## 1. Creación y publicación de un "Hola Mundo"

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#1-creaci%C3%B3n-y-publicaci%C3%B3n-de-un-hola-mundo)

Creamos un repositorio en GitHub con un archivo `index.html` que mostraba un mensaje de **"Hola Mundo"**.

Posteriormente, utilizamos **GitHub Pages** para realizar el deployment y publicar la página en Internet mediante una URL.

---

## 2. Creación del portafolio de evidencias

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#2-creaci%C3%B3n-del-portafolio-de-evidencias)

Actualizamos el archivo `index.html` y lo convertimos en un **portafolio de evidencias** para organizar nuestros trabajos, ejercicios y certificaciones realizados durante el curso.

---

## 3. Creación de una instancia EC2

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#3-creaci%C3%B3n-de-una-instancia-ec2)

En nuestra cuenta de AWS creamos una instancia **EC2** con las siguientes características:

* **Región:** US West (N. California)
* **Sistema operativo:** Ubuntu
* **Tipo de instancia:** `t3.micro`
* **Key pair:** `rodrigo`
* **Security Group:** `launch-wizard-1`

---

## 4. Acceso a la instancia EC2

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#4-acceso-a-la-instancia-ec2)

Una vez creada la instancia, nos conectamos a ella utilizando el usuario:

```
ubuntu
```

**svg**

De esta manera pudimos acceder a la terminal de nuestra instancia EC2 y comenzar a trabajar con Linux.

---

## 5. Práctica de comandos de Linux

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#5-pr%C3%A1ctica-de-comandos-de-linux)

En nuestra instancia EC2 practicamos diferentes comandos básicos de Linux.

### Limpiar la terminal

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#limpiar-la-terminal)

```
clear
```

**svg**

### Crear un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#crear-un-archivo)

```
touch hola.txt
```

**svg**

### Listar archivos

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#listar-archivos)

```
ls
```

**svg**

### Eliminar un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#eliminar-un-archivo)

```
rm -rf hola.txt
```

**svg**

### Escribir contenido en un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#escribir-contenido-en-un-archivo)

```
echo "hola mundo" > hola.txt
```

**svg**

### Mostrar texto en la terminal

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#mostrar-texto-en-la-terminal)

```
echo "hola mundo"
```

**svg**

### Mostrar el contenido de un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#mostrar-el-contenido-de-un-archivo)

```
cat hola.txt
```

**svg**

### Copiar un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#copiar-un-archivo)

```
cp hola.txt hola_copia.txt
```

**svg**

### Modificar el contenido de un archivo

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#modificar-el-contenido-de-un-archivo)

```
echo "hola mundo" > hola_copia.txt
```

**svg**

Posteriormente modificamos nuevamente su contenido:

```
echo "adios mundo" > hola_copia.txt
```

**svg**

Comprobamos el contenido:

```
cat hola_copia.txt
```

**svg**

### Consultar el historial de comandos

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#consultar-el-historial-de-comandos)

```
history
```

**svg**

---

## 6. Búsqueda de comandos en el historial

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#6-b%C3%BAsqueda-de-comandos-en-el-historial)

Utilizamos `history` junto con `grep` para buscar comandos específicos que habíamos ejecutado anteriormente.

Por ejemplo, para buscar los comandos relacionados con `rm`:

```
history | grep "rm"
```

**svg**

Esto permite localizar rápidamente comandos específicos dentro del historial de la terminal.

---

## 7. Actualización del sistema

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#7-actualizaci%C3%B3n-del-sistema)

Actualizamos los paquetes disponibles y posteriormente instalamos sus actualizaciones:

```
sudo apt update
```

**svg**

```
sudo apt upgrade
```

**svg**

`apt update` actualiza la información de los repositorios, mientras que `apt upgrade` instala las actualizaciones disponibles.

---

## 8. Instalación de Zsh y Oh My Zsh

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#8-instalaci%C3%B3n-de-zsh-y-oh-my-zsh)

Instalamos **Zsh**, un shell alternativo a Bash, y posteriormente instalamos **Oh My Zsh** para facilitar su configuración y personalización.

Instalamos Zsh con:

```
sudo apt install zsh
```

**svg**

Podemos comprobar que Zsh se instaló correctamente utilizando:

```
zsh --version
```

**svg**

Después instalamos Oh My Zsh mediante:

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**svg**

---

## 9. Instalación de Docker

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#9-instalaci%C3%B3n-de-docker)

Instalamos **Docker Engine** en nuestra instancia EC2 siguiendo la documentación oficial de Docker para Ubuntu.

Docker nos permitirá posteriormente crear y ejecutar **contenedores** dentro de nuestra instancia.

---

## 10. Instalación de dependencias para Python

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#10-instalaci%C3%B3n-de-dependencias-para-python)

Instalamos las dependencias necesarias para posteriormente utilizar `pyenv` y compilar diferentes versiones de Python:

```
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

**svg**

Estas dependencias proporcionan las herramientas y librerías necesarias para que `pyenv` pueda compilar diferentes versiones de Python.

Para comprobar que Git está disponible, ya que será utilizado para clonar el repositorio de `pyenv`, podemos utilizar:

```
git --version
```

**svg**

En caso de no contar con Git, podemos instalarlo mediante:

```
sudo apt install -y git
```

**svg**

---

## 11. Instalación de pyenv

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#11-instalaci%C3%B3n-de-pyenv)

Clonamos el repositorio de **pyenv**, una herramienta que permite instalar y administrar diferentes versiones de Python:

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

**svg**

Comprobamos que el repositorio se haya descargado correctamente:

```
ls ~/.pyenv
```

**svg**

Al ejecutar este comando debemos poder observar los archivos y directorios correspondientes a `pyenv`.

---

## 12. Configuración de pyenv

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#12-configuraci%C3%B3n-de-pyenv)

Agregamos las configuraciones necesarias para que `pyenv` pueda ser utilizado desde nuestra terminal Zsh:

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
```

**svg**

```
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
```

**svg**

```
echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc
```

**svg**

Recargamos la configuración de Zsh para aplicar los cambios:

```
source ~/.zshrc
```

**svg**

Finalmente, comprobamos que `pyenv` esté instalado correctamente:

```
pyenv --version
```

**svg**

Si el comando devuelve la versión instalada de `pyenv`, significa que la configuración se realizó correctamente.

---

## 13. Instalación y configuración de Python 3.14.7

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#13-instalaci%C3%B3n-y-configuraci%C3%B3n-de-python-3147)

Utilizamos `pyenv` para instalar la versión **Python 3.14.7**:

```
pyenv install 3.14.7
```

**svg**

Una vez instalada, podemos comprobar que la versión se encuentra disponible mediante:

```
pyenv versions
```

**svg**

Configuramos esta versión como la versión global de Python:

```
pyenv global 3.14.7
```

**svg**

Comprobamos que la versión configurada sea la correcta:

```
python --version
```

**svg**

El resultado esperado es:

```
Python 3.14.7
```

**svg**

También podemos comprobar qué versión está siendo utilizada actualmente por `pyenv` mediante:

```
pyenv version
```

**svg**

De esta manera, Python 3.14.7 queda configurado como la versión global de Python para nuestro usuario.

---

## 14. Creación de un entorno virtual de Python

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#14-creaci%C3%B3n-de-un-entorno-virtual-de-python)

Creamos un entorno virtual utilizando el módulo `venv` de Python:

```
python -m venv .venv
```

**svg**

El entorno virtual se crea dentro de la carpeta `.venv`, permitiendo mantener las dependencias de este proyecto aisladas del resto del sistema.

Posteriormente activamos el entorno virtual:

```
source .venv/bin/activate
```

**svg**

Al activarlo, la terminal muestra el nombre del entorno al inicio de la línea de comandos:

```
(.venv) usuario@ubuntu:~/jupyter$
```

**svg**

Esto indica que actualmente estamos trabajando dentro del entorno virtual `.venv`.

Podemos comprobar que el entorno virtual está utilizando la versión correcta de Python mediante:

```
python --version
```

**svg**

El resultado esperado es:

```
Python 3.14.7
```

**svg**

---

## 15. Instalación de Jupyter Notebook

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#15-instalaci%C3%B3n-de-jupyter-notebook)

Con el entorno virtual `.venv` activado, instalamos **Jupyter Notebook** y `ipykernel`:

```
pip install notebook
```

**svg**

```
pip install ipykernel
```

**svg**

Al realizar la instalación con el entorno virtual activo, estas dependencias se instalan dentro de `.venv`, manteniéndolas aisladas del Python global.

Podemos comprobar la instalación de Jupyter mediante:

```
jupyter --version
```

**svg**

Esto mostrará las versiones de los componentes instalados de Jupyter.

---

## 16. Ejecución de Jupyter Notebook

[svg](https://github.com/yoshibeni64/aws_xideral/blob/main/README.md#16-ejecuci%C3%B3n-de-jupyter-notebook)

Con el entorno virtual `.venv` activado, iniciamos Jupyter Notebook:

```
jupyter notebook
```

**svg**

Esto inicia un **servidor local de Jupyter Notebook** que permite acceder a la interfaz desde el navegador.

Jupyter funciona como un servicio porque la interfaz del navegador necesita comunicarse con el servidor para ejecutar código Python y obtener sus resultados.

El servidor permanece ejecutándose en la terminal mientras utilizamos Jupyter y normalmente utiliza el puerto `8888`.

Jupyter proporciona una URL similar a:

```
http://localhost:8888/tree?token=...
```

Desde esta interfaz podemos crear y ejecutar archivos `.ipynb` para trabajar con Python de manera interactiva.

El flujo utilizado fue:

```
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
