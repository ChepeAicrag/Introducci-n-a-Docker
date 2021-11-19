# Ejecutar imágenes existentes

Para este tipo de ejemplos, se utilizará lo que es Docker Hub, que consiste en un repositorio de imagenes de Docker. 

# UNA IMAGEN DE UNA APLICACIÓN

```docker
docker run hello-world
```

- Si no tiene la imagen de hello-world localmente, Docker la extrae de su registro configurado, como si hubiera ejecutado docker pull hello-world manualmente.
- Docker crea un nuevo contenedor, como si hubiera ejecutado manualmente un comando de creación de contenedor de Docker.

# UNA IMAGEN DE UN SO

Vamos a ejecutar el siguiente comando. 

```docker
docker run -i -t ubuntu /bin/bash
```

- Cuando escribe exit para terminar el comando / bin / bash, el contenedor se detiene pero no se elimina. Puede iniciarlo de nuevo o eliminarlo.
- Docker inicia el contenedor y ejecuta / bin / bash. Debido a que el contenedor se ejecuta de forma interactiva y está adjunto a su terminal (debido a los indicadores -i y -t), puede proporcionar entrada usando su teclado mientras la salida se registra en su terminal.
- Docker crea una interfaz de red para conectar el contenedor a la red predeterminada, ya que no especificó ninguna opción de red. Esto incluye la asignación de una dirección IP al contenedor. De forma predeterminada, los contenedores pueden conectarse a redes externas mediante la conexión de red de la máquina host.
- Docker asigna un sistema de archivos de lectura y escritura al contenedor, como su capa final. Esto permite que un contenedor en ejecución cree o modifique archivos y directorios en su sistema de archivos local.
- Docker crea un nuevo contenedor, como si hubiera ejecutado manualmente un comando de creación de contenedor de Docker.
- Si no tiene la imagen de ubuntu localmente, Docker la extrae de su registro configurado, como si hubiera ejecutado docker pull ubuntu manualmente.