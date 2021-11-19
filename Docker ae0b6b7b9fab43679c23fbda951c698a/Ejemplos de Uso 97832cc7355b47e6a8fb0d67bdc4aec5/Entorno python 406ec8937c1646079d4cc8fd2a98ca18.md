# Entorno python

Configurar un entorno de desarrollo con Python para ejecutar sencillos scripts. 

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
    
- Visualizamos nuestras imágenes, y vamos a ver la que acabamos de crear.
    
    ```docker
    docker images
    ```
    
    Vamos a obtener una salida cómo la siguiente: 
    
    REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
    env_python      v1.0      f9b80f67c9d2   41 seconds ago   114MB
    
- Vamos a ejecutar nuestro contenedor.
    
    ```docker
    docker run --name env_python_01 env_python:v1.0
    ```
    
- La salida que vamos a tener en pantalla es
    
    ```bash
    Hola mundo desde un contenedor
    ```
    

# Problema

- Hasta ahora nuestro contenedor ejecuta solo el archivo que creamos llamado [hello.py](http://hello.py).
    
    Por ejemplo, vamos a crear otro script, llamado [bye.py](http://bye.py), pero ahora. ¿Cómo ejecutamos este script?
    
    ```python
    print("Adios :)")
    ```
    

# Posible acción de solución

- Tal vez la principal idea que se nos venga a la mente, es acceder al contenedor de forma iterativa.
    
    ```docker
    docker run -it --name env_python_01 env_python
    ```
    
- Para luego ejecutar desde la terminal el comando que nos permita ejecutar el script.
    
    ```bash
    python bye.py
    ```
    
- Sorpresa... Esto no va a funcionar. ¿Por qué?
    
    Básicamente porque nosotros en el Dockerfile le indicamos que ejecute un comando al momento de crearse. Y el contenedor ejecuta el comando y luego se detiene automáticamente.
    

# Solución no optima

- La solución más sencilla de realizar es reconstruir la imagen con un nuevo Dockerfile en el que agregamos una nueva instrucción de ejecución de comando y lo guardamos en el archivo con el nombre de Dockerfile-2.
    
    ```docker
    FROM python:3.8-slim-buster
    
    # Keeps Python from generating .pyc files in the container
    ENV PYTHONDONTWRITEBYTECODE=1
    
    # Turns off buffering for easier container logging
    ENV PYTHONUNBUFFERED=1
    
    WORKDIR /app
    
    COPY . /app/
    
    CMD ["python", "hello.py"]
    
    CMD ["python", "bye.py"]
    ```
    
- Construimos nuestra imagen cómo una nueva versión.
    
    ```bash
    docker build -t env_python:v2.0 -f Dockerfile-2 .
    ```
    
    - Ahora usamos la flag -f para indicar el nombre del archivo que contiene nuestras instrucciones para la imagen. Esto se usa cuando el nombre del archivo es distinto a Dockerfile. 
    Name of the Dockerfile (Default is 'PATH/Dockerfile')
    - Seguimos usando el . al final para indicar la ruta en la que se encuentra el archivo
- Vamos a listar todas las imágenes que tenemos.
    
    ```bash
    docker images
    ```
    
    Vamos a obtener una salida cómo la siguiente:
    
    REPOSITORY      TAG       IMAGE ID       CREATED              SIZE
    env_python      v2.0      b3c08e5b0731   About a minute ago   114MB
    env_python      v1.0      f9b80f67c9d2   17 minutes ago       114MB
    
- Ahora vamos a ejecutar el contenedor
    
    ```bash
    docker run --name env_python_02 env_python:v2.0
    ```
    
- Y lo que va a pasar es que se va a ejecutar el último script.
    
    ```bash
    python bye.py
    ```
    
- Y ahora tendremos la salida en consola.
    
    ```bash
    Adios :)
    ```
    
- Pero el primer script no se ha visualizado, el problema de esto es que vamos a anidar comandos para cada script sin ver la ejecución de cada uno.

# Otra solución no optima

- Pero... si necesitamos ejecutar muchos scripts y estar agregando más y más, entonces se vuelve tedioso el tema de reconstruir a cada rato nuestra imagen.
- Vamos a proponer otra solución que será similar como si se usará la **shell interactivo IDLE**
- Lo primero a realizar, es editar el Dockerfile, para indicarle que no ejecute un comando. Y vamos a crear otro Dockerfile, con el nombre de Dockerfile-3.
    
    ```docker
    FROM python:3.8-slim-buster
    
    # Keeps Python from generating .pyc files in the container
    ENV PYTHONDONTWRITEBYTECODE=1
    
    # Turns off buffering for easier container logging
    ENV PYTHONUNBUFFERED=1
    
    WORKDIR /app
    
    COPY . /app/
    ```
    
    **NOTA:** Hay que borrar el archivo [bye.py](http://bye.py) antes de realizar la construcción del Dockerfile-3 para posteriormente en otro punto explicar algo importante. 
    
    Hasta este momento la estructura que se supone debemos tener debe ser la siguiente.
    
    ```bash
    ls -l
    -rwxrwxrwx 1 chepeaicrag chepeaicrag 249 Nov 17 23:31 Dockerfile
    -rwxrwxrwx 1 chepeaicrag chepeaicrag 274 Nov 17 23:40 Dockerfile-2
    -rwxrwxrwx 1 chepeaicrag chepeaicrag 221 Nov 17 23:45 Dockerfile-3
    -rwxrwxrwx 1 chepeaicrag chepeaicrag  39 Nov 17 23:15 hello.py
    ```
    

- Vamos a construir la imagen con la configuración actualizada, utilizando el nuevo Dockerfile.
    
    ```bash
    docker build -t env_python:v3.0 -f Dockerfile-3 .
    ```
    
- Listamos las imágenes para visualizar nuestra imagen con su respectivo tag.
    
    ```bash
    docker images
    ```
    
    Vamos a obtener una salida cómo la siguiente:
    
    REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
    env_python      v3.0      39a668b9e182   19 seconds ago   114MB
    env_python      v2.0      df4745269279   6 minutes ago    114MB
    env_python      v1.0      f9b80f67c9d2   29 minutes ago   114MB
    

- Vamos a ejecutar el contenedor con nuestra nueva imagen.
    
    ```bash
    docker run --name env_python_03 -it env_python:v3.0
    ```
    
    - Usamos -it porque vamos a hacer al modo interactivo y con al terminal para ejecutar comandos.
    
    Lo que vamos a obtener es:
    
    ```bash
    Python 3.8.12 (default, Nov 17 2021, 17:26:17) 
    [GCC 8.3.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    ```
    
- Ahora estamos en el modo interactivo, pero... ¿Qué modo interactivo es? Pues el de python, justamente estamos en el interprete de python. Entonces no podemos ejecutar comandos de shell cómo los de Linux.
- Hay que intentar ejecutar nuestro script de hello.py
    
    ```bash
    python hello.py
    ```
    
- Vamos a tener una salida cómo la siguiente:
    
    ```bash
    >>> python hello.py
      File "<stdin>", line 1
        python hello.py
               ^
    SyntaxError: invalid syntax
    >>>
    ```
    
- Ahora la duda es ¿Cómo ejecutamos scripts desde el interprete? Pues hay una forma.
    
    Ejecutamos la siguiente instrucción.
    
    ```bash
    exec(open("hello.py").read())
    ```
    
- ¿Qué hace lo anterior? sencillamente es abrir nuestro archivo [hello.py](http://hello.py) y leerlo para luego ejecutar las instrucciones.
    
    La salida entonces es:
    
    ```bash
    >>> exec(open("hello.py").read())
    Hola mundo desde un contenedor
    >>>
    ```
    
- Listo, hemos podido ejecutar nuestro script [hello.py](http://hello.py)
    
    Pero ¿Qué pasa con los otros scripts? 
    
    Tendríamos que cambiar el nombre del archivo
    
- Cómo al principio de esta sección borramos el archivo [bye.py](http://bye.py), lo volvemos a crear.
    
    ```python
    print("Adios :)")
    ```
    
- Ahora, vamos a ejecutar el archivo creado recientemente.
    
    ```bash
    exec(open("bye.py").read())
    ```
    
- Vamos a obtener un error cómo el siguiente:
    
    ```python
    >>> exec(open("bye.py").read())
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    FileNotFoundError: [Errno 2] No such file or directory: 'bye.py'
    >>>
    ```
    
- Este error fue porque el archivo [bye.py](http://bye.py) se creó despues de que se creara la imagen, entonces al ejecutar un contenedor, la carpeta /app no contiene el archivo y por ello el interprete no lo encuentra.
    
    El problema sería que el contenedor no encontrará el archivo solicitado, ya que el archivo lo creamos en nuestro host pero no en el contenedor. 
    
    Esto es porque al construir la imagen copiamos los archivos que existían en ese momento hacia el contenedor, y los archivos que se creen a partir de construir la imagen no van a ser almacenados en el contenedor.
    
- Entonces, cada vez que vamos a crear nuevos archivos, vamos a tener que reconstruir la imagen, y se va a volver tedioso.
- Por ahora, la solución es buena pero no es la mejor, entonces vamos a proceder a solucionar ese problema de los archivos, crearemos nuevos archivos después de construir nuestra imagen y aún así los podemos ejecutar desde el contenedor.

# Mejor solución

- Si queremos usar más archivos y gestionarlos, así cómo el poder ejecutar sin Python interactivo ¿Se puede hacer?
- La respuesta es que sí, y para ello vamos a utilizar los bind mounts, y lo que haremos es compartir nuestra carpeta donde estaremos creando nuestros scripts cómo un volumen y que el contenedor pueda acceder a dicho volumen y tener en tiempo real los archivos que se agreguen.
- El bind mounts los vamos a crear al momento de construir y ejecutar el contenedor, de tal forma que podamos acceder al modo interactivo.
- Vamos a empezar por retomar nuestra imagen creada con la configuración de Dockerfile contenida en el archivo Dockerfile-3
    
    
    Esta es la imagen empleada:
    
    REPOSITORY      TAG       IMAGE ID       CREATED              SIZE
    
    env_python      v3.0      39a668b9e182   19 seconds ago   114MB
    
- Procedemos a crear y ejecutar nuestro contenedor.
    
    ```bash
    docker run --name env_python_real -it -v "$(pwd)":/app env_python:v3.0
    ```
    
    - Utilizamos el flag -v o --volume para enlazar-montar un archivo o directorio que aún no existe en el host de Docker, -v crea el punto final por usted. Siempre se crea como un directorio.
    - Utilizamos ${pwd} que indica la ruta actual en la que nos encontramos.
    
    También podemos utilizar la flag de --mount 
    
    ```bash
    docker run --name env_python_real -it --mount type=bind,source="$(pwd)", \
    target=/app env_python:v3.0
    ```
    
    Que es más intuitiva y es recomendada para principiantes cuando no se conoce que se pueden crear diferentes tipos de volumen.
    
    **NOTA:** Revisar la referencia que está al final de esta página.
    
- Vamos también a acceder al interprete de python.
    
    ```bash
    Python 3.8.12 (default, Nov 17 2021, 17:26:17) 
    [GCC 8.3.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    ```
    
- Y podemos hacer lo que habíamos hecho anteriormente, ejecutar nuestros scripts.
    
    ```bash
    exec(open("bye.py").read())
    ```
    
    Obteniendo el siguiente resultado:
    
    ```bash
    >>> exec(open("bye.py").read())
    Adios :)
    >>>
    ```
    
- Ahora vamos a crear un nuevo archivo desde nuestra máquina (host) y este lo llamaremos [file.py](http://file.py)
    
    ```bash
    print("Soy un archivo de un bind mount")
    ```
    
- Y la vamos a ejecutar desde nuestro interprete. Por lo cual vamos a ejecutar el comando siguiente.
    
    ```bash
    exec(open("file.py").read())
    ```
    
    Vamos a obtener de salida:
    
    ```bash
    Soy un archivo de un bind mount
    ```
    
    Y como veremos, el archivo en ningún momento lo tuvimos que agregar al contenedor de forma manual. Ni reconstruimos la imagen.
    
    Si creamos más archivos desde nuestro host en este directorio que estamos actualmente, también estarán en el contenedor, esto gracias al bind mount que hicimos.
    

### Pero... ¿Ejecutar scripts desde el interprete 😖?

Vamos a mejorar la solución para dejarla ya finalmente optima. 

La idea es acceder al interactivo del contenedor, que si observamos se sigue ejecutando a pesar de ejecutar un script dado. 

Entonces, vamos a hacer uso de otro comando muy concurrente de Docker.

- Primero vamos a crear un nuevo contenedor con un nuevo nombre.
    
    ```docker
    docker run -d --name env_python_real_opt -it -v "$(pwd)":/app env_python:v3.0
    ```
    
    - Ahora agregamos -d para indicarle a dockerd que ejecute en segundo plano al contenedor.
    
    Vamos a tener una salida cómo la siguiente:
    
    ```bash
    env_python_real_opt
    ```
    
    O una salida con el id del contenedor, que esta puede variar.
    
    ```bash
    53e596e425dc997af6d06d54144aaa4af2a5086943d557dbdb69a026f426795b
    ```
    
- Posteriormente, vamos a verificar que se está ejecutando nuestro contenedor.
    
    ```docker
    docker ps
    ```
    
    Y ahí veremos a nuestro contenedor ejecutándose. 
    
    ```markdown
    CONTAINER ID   IMAGE             COMMAND     CREATED              STATUS              PORTS     NAMES
    53e596e425dc   env_python:v3.0   "python3"   About a minute ago   Up About a minute             env_python_real_opt
    ```
    
- Ahora vamos a acceder a ese contenedor en ejecución.
    
    Podemos acceder al contenedor de otra forma a la mostrada anteriormente. Para dejar de usar el interprete de python y utilizando el bash que este contiene.
    
    ```bash
    docker exec -it env_python_real_opt /bin/bash
    ```
    
- Vamos a obtener una shell cómo la que tenemos en cualquier distro de Linux donde podremos ejecutar comandos. Esto ya es familiar para nosotros. 
Por lo general la estructura de la shell es
    
    ```markdown
    root@{id_container}:{workdir}#
    ```
    
    Por lo que en mi caso, la shell se muestra cómo la siguiente:
    
    ```bash
    root@53e596e425dc:/app#
    ```
    
- Y ahora si ejecutamos el script que deseemos.
    
    Ahora si podemos ejecutar nuestro script hello.py, cómo lo hacemos típicamente. 
    
    ```bash
    python hello.py
    ```
    
    La salida en consola será:
    
    ```bash
    root@53e596e425dc:/app# python hello.py
    Hola mundo desde un contenedor
    root@53e596e425dc:/app#
    ```
    
- Y si ahora ejecutamos el otro script [bye.py](http://bye.py).
    
    ```bash
    python bye.py
    ```
    
    La salida en consola será:
    
    ```bash
    root@53e596e425dc:/app# python bye.py
    Adios :)
    root@53e596e425dc:/app#
    ```
    
- Ejecutamos también el archivo [file.py](http://file.py)
    
    ```bash
    python file.py
    ```
    
    La salida en consola será:
    
    ```bash
    root@53e596e425dc:/app# python file.py 
    Soy un archivo de un bind mount
    root@53e596e425dc:/app#
    ```
    
- Podemos crear otro archivo desde nuestro host, un script llamado future_devs.py con el contenido de un mensaje en pantalla.
    
    ```python
    print("-------- Future Devs --------")
    ```
    
- Y ejecutarlo desde el contenedor que está corriendo, este contenedor es el del nombre **env_python_real_opt**
    
    ```bash
    python future_devs.py
    ```
    
    Y la salida en consola será:
    
    ```bash
    root@53e596e425dc:/app# python future_devs.py
    -------- Future Devs --------
    root@53e596e425dc:/app#
    ```
    
- Ahora vamos a editar desde nuestro contenedor el archivo [bye.py](http://bye.py) para ello debemos estar en la shell de nuestro contenedor.
    
    ```bash
    echo "print('Ejemplo terminado :)')" > bye.py
    ```
    
    Con el operador > remplazamos todo el contenido actual que tenga el archivo [bye.py](http://bye.py) actualmente.
    
    Si usáramos >> solamente agregaría una nueva línea al archivo y mantendría lo que contiene hasta el momento.
    
    Y el archivo ahora estará de la siguiente forma:
    
    ```python
    print("Ejemplo terminado :)")
    ```
    
- Si ahora abrimos el archivo desde nuestro host, vamos a notar que también se modificó, esto es porque ambos están trabajando con el mismo archivo.
- También lo podemos ejecutar así cómo los scripts anteriores.
    
    ```bash
    python bye.py 
    ```
    
    Y el resultado obtenido es:
    
    ```bash
    root@53e596e425dc:/app# python bye.py 
    Ejemplo terminado :)
    root@53e596e425dc:/app#
    ```
    
- Finalmente, simplemente vamos a salir del contenedor. Con el comando **exit**.
    
    ```bash
    root@53e596e425dc:/app# exit
    exit
    ```
    
- Y si comprobamos, el contenedor sigue ejecutándose.
    
    ```bash
    docker ps
    ```
    
    Y ahí estará el contenedor en ejecución en segundo plano.
    
    ```bash
    CONTAINER ID   IMAGE             COMMAND     CREATED          STATUS          PORTS     NAMES
    53e596e425dc   env_python:v3.0   "python3"   18 minutes ago   Up 18 minutes             env_python_real_opt
    ```
    
- Finalmente, detenemos nuestro contenedor.
    
    ```docker
    docker stop env_python_real_opt
    ```
    
    Y este comando nos muestra el nombre del contenedor detenido.
    
    ```bash
    env_python_real_opt
    ```
    
- Y comprobamos nuevamente, ahora no hay ningún contenedor en ejecución.
    
    ```docker
    docker ps
    ```
    
    Se mostrará lo siguiente, solo encabezados:
    
    ```docker
    CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
    ```
    
    Esta solución es la mejor para trabajar con Docker para un entorno de desarrollo básico. Esto porque no tuvimos que reconstruir la imagen, tampoco detuvimos el contenedor y lo ejecutamos desde una shell en la que se pueden realizar más cosas cómo abrir el archivo y editarlo. 
    
    Bueno, hasta ahora hemos aprendido lo básico para un entorno de programación cob python utilizando docker. Si queremos más cosas cómo git, vim y otros comandos más en el contenedor, entonces podemos crear nuestra propia imagen con todas las herramientas necesarias. 
    

# **Cómo levantar una imagen base para un proyecto de Python**

## **Quickstart: Compose and Django**

[https://docs.docker.com/samples/django/](https://docs.docker.com/samples/django/)

## **Get started with Docker Compose**

[https://docs.docker.com/compose/gettingstarted/](https://docs.docker.com/compose/gettingstarted/)

[Use bind mounts](https://docs.docker.com/storage/bind-mounts/)