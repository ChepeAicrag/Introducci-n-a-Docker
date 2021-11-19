# Entorno Node js

Configurar un entorno de desarrollo con Node JS para ejecutar sencillos scripts, es muy fácil y seguiremos lo mismo realizado en Node JS. 

- Vamos a crear un script de python hello.py
    
    ```python
    print("Hola mundo desde un contenedor")
    ```
    
- Vamos a crear un archivo Dockerfile
    
    ```docker
    FROM python:3.8-slim-buster
    
    # Keeps Python from generating .pyc files in the container
    ENV PYTHONDONTWRITEBYTECODE=1
    
    # Turns off buffering for easier container logging
    ENV PYTHONUNBUFFERED=1
    
    WORKDIR /app
    
    COPY . /app/
    
    CMD ["python", "hello.py"]
    ```
    
- Vamos a construir la imagen de nuestro entorno.
    
    ```docker
    docker build -t env_python:v1.0 .
    ```
    
- Vamos a ejecutar nuestro contenedor.
    
    ```docker
    docker run --name env_python_01 env_python
    ```
    
- La salida que vamos a tener en pantalla es
    
    ```bash
    Hola mundo desde un contenedor
    ```
    

# Problema

- Hasta ahora nuestro contenedor ejecuta solo el archivo que creamos llamado [hello.py](http://hello.py). Por ejemplo, vamos a crear otro script, llamado [bye.py](http://bye.py), pero ahora. ¿Cómo ejecutamos este script?
    
    ```python
    print("Adios :)")
    ```
    

# Posible solución

- Tal vez la principal idea que se nos venga a la mente, es acceder al contenedor de forma iterativa.
    
    ```docker
    docker run -it --name env_python_01 env_python
    ```
    
- Para luego ejecutar desde la terminal comando que nos permita ejecutar el script.
    
    ```bash
    python bye.py
    ```
    
- Sorpresa... Esto no va a funcionar. ¿Por qué?
    
    Básicamente porque el contenedor no encuentra el archivo solicitado, ya que el archivo lo creamos en nuestro host pero no en el contenedor. 
    
    Esto es porque al construir la imagen copiamos los archivos que existían en ese momento hacia el contenedor, y los archivos que se creen a partir de construir la imagen no van a ser almacenados en el contenedor.
    

# Solución no optima

- La solución más sencilla de realizar es reconstruir la imagen.
    
    ```bash
    docker build -t env_python:v2.0 .
    ```
    
- Ahora vamos a ejecutar el contenedor
    
    ```bash
    docker run -it --name env_python_02 env_python:v2.0
    ```
    
- Listamos los archivos que hay en el contenedor, ahí vamos a visualizar los dos archivos.
    
    ```bash
    ls -l
    ```
    
    Lo que se mostrará en pantalla es:
    
    ```bash
    
    ```
    
- Para luego ejecutar el script.
    
    ```bash
    python bye.py
    ```
    
- Y ahora tendremos la salida en consola.
    
    ```bash
    Adios :)
    ```
    

# Solución Real

- Pero... si necesitamos ejecutar muchos scripts y estar agregando más y más, entonces se vuelve tedioso el tema de reconstruir a cada rato nuestra imagen.
- Para ello vamos a utilizar los volúmenes, y lo que haremos en compartir nuestra carpeta donde estaremos creando nuestros scripts cómo un volumen y que el contenedor pueda acceder a dicho volumen y tener en tiempo real los archivos que se agregan.
- Lo primero a realizar, es editar el Dockerfile, para indicarle que no ejecute un comando. Y vamos a crear otro Dockerfile, con el nombre de Dockerfile-2
    
    ```docker
    FROM python:3.8-slim-buster
    
    # Keeps Python from generating .pyc files in the container
    ENV PYTHONDONTWRITEBYTECODE=1
    
    # Turns off buffering for easier container logging
    ENV PYTHONUNBUFFERED=1
    
    WORKDIR /app
    
    COPY . /app/
    
    ```
    
- Vamos a construir la imagen con la figuración actualizada, utilizando el nuevo Dockerfile.
    
    ```bash
    docker build -t env_python:v3.0 Dockerfile-2
    ```
    
- El volumen los vamos a crear al momento de construir y ejecutar el contenedor, de tal forma que podamos acceder al modo interactivo.
    
    ```bash
    docker run --name env_python_real -it -v ./:/app/scripts  env_python:v3.0
    ```
    
- Ahora si podemos ejecutar nuestro script hello.py, cómo lo hacemos típicamente.
    
    ```bash
    python hello.py
    ```
    
    La salida en consola será:
    
    ```bash
    Hola mundo desde un contenedor
    ```
    
- Y si ahora ejecutamos el otro script [bye.py](http://bye.py).
    
    ```bash
    python bye.py
    ```
    
    La salida en consola será:
    
    ```bash
    Adios :)
    ```
    
- Ahora veremos algo extra. Podemos crear otro archivo desde nuestro host, un script llamado future_devs.py
    
    ```python
    print("-------- Future Devs --------")
    ```
    
- Y ejecutar desde el contenedor que está corriendo, este contenedor es el del nombre **env_python_real**
    
    ```bash
    python future_devs.py
    ```
    
    Y la salida en consola será:
    
    ```bash
    Adios :)
    ```
    
    Notando que no tuvimos que reconstruir la imagen, y tampoco detuvimos el contenedor. Bueno, hasta ahora hemos aprendido lo básico para un entorno de programación cob python utilizando docker.