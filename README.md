# Evidencias del curso de AWS

## 1. Creación y publicación de un "Hola Mundo"

Creamos un repositorio en GitHub con un archivo `index.html` que mostraba un mensaje de **"Hola Mundo"**.

Posteriormente, utilizamos **GitHub Pages** para realizar el deployment y publicar la página en Internet mediante una URL.

## 2. Creación del portafolio de evidencias

Actualizamos el archivo `index.html` y lo convertimos en un **portafolio de evidencias** para organizar nuestros trabajos, ejercicios y certificaciones realizados durante el curso.


## 3. Creación de una instancia EC2

En nuestra cuenta de AWS creamos una instancia **EC2** con las siguientes características:

* **Región:** US West (N. California)
* **Sistema operativo:** Ubuntu
* **Tipo de instancia:** `t3.micro`
* **Key pair:** `rodrigo`
* **Security Group:** `launch-wizard-1`

## 4. Acceso a la instancia EC2

Una vez creada la instancia, nos conectamos a ella utilizando el usuario:

```
ubuntu
```

De esta manera pudimos acceder a la terminal de nuestra instancia EC2 y comenzar a trabajar con Linux.

## 5. Práctica de comandos de Linux

En nuestra instancia EC2 practicamos diferentes comandos básicos de Linux.

### Limpiar la terminal
```
clear
```
### Crear un archivo

```
touch hola.txt
```

### Listar archivos

```
ls
```

### Eliminar un archivo
```
rm -rf hola.txt
```

### Escribir contenido en un archivo

```
echo "hola mundo" > hola.txt
```

### Mostrar texto en la terminal

```
echo "hola mundo"
```

### Mostrar el contenido de un archivo

```
cat hola.txt
```
### Copiar un archivo

```
cp hola.txt hola_copia.txt
```
### Modificar el contenido de un archivo

```
echo "hola mundo" > hola_copia.txt
```
Posteriormente modificamos nuevamente su contenido:

```
echo "adios mundo" > hola_copia.txt
```
Comprobamos el contenido:

```
cat hola_copia.txt
```

### Consultar el historial de comandos

```
history
```

## 6. Búsqueda de comandos en el historial

Utilizamos `history` junto con `grep` para buscar comandos específicos que habíamos ejecutado anteriormente.

Por ejemplo, para buscar los comandos relacionados con `rm`:

```
history | grep "rm"
```

Esto permite localizar rápidamente comandos específicos dentro del historial de la terminal.

## 7. Actualización del sistema

Actualizamos los paquetes disponibles y posteriormente instalamos sus actualizaciones:

```
sudo apt update
```

```
sudo apt upgrade
```

`apt update` actualiza la información de los repositorios, mientras que `apt upgrade` instala las actualizaciones disponibles.

## 8. Instalación de Zsh y Oh My Zsh

Instalamos **Zsh**, un shell alternativo a Bash, y posteriormente instalamos **Oh My Zsh** para facilitar su configuración y personalización.

Instalamos Zsh con:

```
sudo apt install zsh
```

Podemos comprobar que Zsh se instaló correctamente utilizando:

```
zsh --version
```

Después instalamos Oh My Zsh mediante:

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 9. Instalación de Docker

Instalamos **Docker Engine** en nuestra instancia EC2 siguiendo la documentación oficial de Docker para Ubuntu.

Docker nos permitirá posteriormente crear y ejecutar **contenedores** dentro de nuestra instancia.

## 10. Instalación de dependencias para Python

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

Estas dependencias proporcionan las herramientas y librerías necesarias para que `pyenv` pueda compilar diferentes versiones de Python.

Para comprobar que Git está disponible, ya que será utilizado para clonar el repositorio de `pyenv`, podemos utilizar:

```
git --version
```

En caso de no contar con Git, podemos instalarlo mediante:

```
sudo apt install -y git
```

## 11. Instalación de pyenv

Clonamos el repositorio de **pyenv**, una herramienta que permite instalar y administrar diferentes versiones de Python:

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```



Comprobamos que el repositorio se haya descargado correctamente:

```
ls ~/.pyenv
```



Al ejecutar este comando debemos poder observar los archivos y directorios correspondientes a `pyenv`.

## 12. Configuración de pyenv

Agregamos las configuraciones necesarias para que `pyenv` pueda ser utilizado desde nuestra terminal Zsh:

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
```

```
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
```

```
echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc
```

Recargamos la configuración de Zsh para aplicar los cambios:

```
source ~/.zshrc
```

Finalmente, comprobamos que `pyenv` esté instalado correctamente:

```
pyenv --version
```

Si el comando devuelve la versión instalada de `pyenv`, significa que la configuración se realizó correctamente.

## 13. Instalación y configuración de Python 3.14.7

Utilizamos `pyenv` para instalar la versión **Python 3.14.7**:

```
pyenv install 3.14.7
```

Una vez instalada, podemos comprobar que la versión se encuentra disponible mediante:

```
pyenv versions
```

Configuramos esta versión como la versión global de Python:

```
pyenv global 3.14.7
```

Comprobamos que la versión configurada sea la correcta:

```
python --version
```

El resultado esperado es:

```
Python 3.14.7
```

También podemos comprobar qué versión está siendo utilizada actualmente por `pyenv` mediante:

```
pyenv version
```
De esta manera, Python 3.14.7 queda configurado como la versión global de Python para nuestro usuario.



## 14. Creación de un entorno virtual de Python

Creamos un entorno virtual utilizando el módulo `venv` de Python:

```
python -m venv .venv
```
El entorno virtual se crea dentro de la carpeta `.venv`, permitiendo mantener las dependencias de este proyecto aisladas del resto del sistema.

Posteriormente activamos el entorno virtual:

```
source .venv/bin/activate
```

Al activarlo, la terminal muestra el nombre del entorno al inicio de la línea de comandos:

```
(.venv) usuario@ubuntu:~/jupyter$
```

Esto indica que actualmente estamos trabajando dentro del entorno virtual `.venv`.

Podemos comprobar que el entorno virtual está utilizando la versión correcta de Python mediante:

```
python --version
```

El resultado esperado es:

```
Python 3.14.7
```

## 15. Instalación de Jupyter Notebook


Con el entorno virtual `.venv` activado, instalamos **Jupyter Notebook** y `ipykernel`:

```
pip install notebook
```
```
pip install ipykernel
```

Al realizar la instalación con el entorno virtual activo, estas dependencias se instalan dentro de `.venv`, manteniéndolas aisladas del Python global.

Podemos comprobar la instalación de Jupyter mediante:

```
jupyter --version
```
Esto mostrará las versiones de los componentes instalados de Jupyter.

## 16. Ejecución de Jupyter Notebook

Con el entorno virtual `.venv` activado, iniciamos Jupyter Notebook:

```
jupyter notebook
```
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
