# Con archivos Dockerfile

# EJECUTAR UN SISTEMA OPERATIVO: UBUNTU

Para obtener una distro de Linux, en este caso ubuntu, lo más sencillo 

- Crear un archivo con el nombre de Dockerfile

- En el contenido del archivo colocar:
    
    Esto va a descargar la imagen base del contenedor. Por hacer una analogía el sistema operativo.
    

```docker
FROM ubuntu
```

- Construir el contenedor en base a nuestro archivo Dockerfile
    
    ```docker
    docker build -t myfirstimage:v1.0 .
    ```
    
    - El comando docker build nos permite construir imágenes.
    - La flag -t nos permite indicar el nombre que va a tener nuestra imagen y también tagearla. En este caso se llama ***myfirstimage*** y despues de los : se coloca un tag para indicar la versión de nuestra imagen.
    - El . es para indicar la ruta donde se encuentra el archivo Dockerfile. Es una ruta con el formato *PATH/Dockerfile*
- Listar las imágenes que existen actualmente.
    
    ```docker
    docker images
    ```
    
- Ejecutar el contenedor
    
    ```docker
    docker run -it myfirstimage
    ```
    
    - Utilizamos el comando docker run para crear un contenedor o dicho de otra forma, ejecutar una instancia de una imagen. La imagen en este caso es myfirstimage. 
    **Nota:** El tag de la imagen no fue colocado, por lo que Docker supondrá que es la más receiente y ejecuta el contenedor *myfirstimage:latest.* Por eso es importante colocar el tag de la versión de la imagen que vamos a usar.
    - Utilizamos -it para indicarle que lo corra en modo iterativo y podamos usar la terminal en el contenedor.
- Por la ejecución de lo anterior, vamos a obtener un mensaje de error.
    
    ```bash
    Unable to find image 'myfirsimage:latest' locally
    <3>init: (12188) ERROR: UtilConnectUnix:467: connect failed 111
    docker: Error response from daemon: pull access denied for myfirsimage, 
    repository does not exist or may require 'docker login': 
    denied: requested access to the resource is denied.
    See 'docker run --help'.
    ```
    
- Por lo que para ejecutar el contenedor vamos a tener que indicar el tag que queremos. Y al construir la imagen solo colocamos un tag.
    
    ```bash
    docker run -it myfirsimage:v1.0
    ```
    
- Vamos a visualizar los contenedores ejecutándose.
    
    ```docker
    docker ps
    ```
    

# EJECUTAR UNA PÁGINA WEB CON NGINX

- Crear el archivo de la página web. Para ello creamos un archivo index.html
    
    ```html
    <a> Hola soy una página web <a>
    ```
    
- Crear un archivo con el nombre de Dockerfile
- En el contenido del archivo colocar:
    
    ```docker
    FROM nginx:1.19.0-alpine
    COPY . /usr/share/nginx/html
    ```
    
    - Vamos a descargar una imagen base de nginx con una versión especifica.
    - Vamos a copiar nuestro archivo html a la dirección donde guarda el servidor nginx el html que muestra cómo principal.
- Construir el contenedor en base a nuestro archivo Dockerfile
    
    ```docker
    docker build -t myservernginx:v0.1 .
    ```
    
- Listar las imágenes que existen actualmente.
    
    ```docker
    docker images
    ```
    
- Ejecutar el contenedor
    
    ```html
    docker run -it -p 8080:80 myservernginx:v0.1
    ```
    
    - Vamos a utilizar el comando docker run para ejecutar contenedores a partir de una imagen.
    - Utilizamos -p para indicar la configuración del puerto, con una estructura puertoHost:puertoContenedor. Esto quiere decir, que vamos a compartir nuestro puerto 8080 con el contenedor y estará mapeado al puerto 80 del contenedor.
        
        Por default la configuración de nginx está para escuchar peticiones al puerto 80, por eso mapeamos a ese puerto en el contenedor.
        
    - Utilizamos -it para indicarle que lo corra en modo iterativo y podamos usar la terminal en el contenedor.
- Vamos a visualizar los contenedores ejecutándose.
    
    ```docker
    docker ps
    ```
    
- Accedemos a nuestro navegador y colocamos la ruta [localhost:8080](http://localhost:8080) y vamos a obtener lo siguiente.
    
    ![Untitled](Con%20archivos%20Dockerfile%20c42c14d6d6d2457a974c96b572d3abe8/Untitled.png)
    

# Referencias

[](https://hub.docker.com/_/ubuntu)

[Nginx - Official Image | Docker Hub](https://hub.docker.com/_/nginx)