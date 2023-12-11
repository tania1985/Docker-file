# Docker-file
Antes de empezar

Lo primero es ir a git y crear un nuevo repositorio. Después de esto, creamos nuestro nuevo proyecto en el Code. Una vez listo esto, creamos también un nuevo proyecto en Git y lo asociamos al repositorio creado anteriormente.
Imagen de Ubuntu 22.04

FROM ubuntu:22.04

# Instalar paquetes necesarios
RUN apt-get update
RUN apt-get install -y iputils-ping dnsutils

# Configurar comando por defecto cuando se ejecute el contenedor
CMD ["/bin/bash"]

En este Dockerfile, se utiliza la imagen base de Ubuntu 22.04 (FROM ubuntu:22.04). Luego, se actualiza el sistema y se instalan los paquetes iputils-ping (para el comando "ping") y dnsutils (para el comando "dig").

Por último, se configura el comando por defecto cuando se ejecute el contenedor (CMD ["/bin/bash"]).

Sin olvidar de primero guardar el fichero ejecutamos el siguiente comando en la terminal para construir la imagen Docker:

$ docker build -t myubuntu:22.04 .

El argumento -t define el nombre y etiqueta de la imagen (en este caso, "myubuntu" y "22.04" respectivamente). El punto al final indica que el Dockerfile se encuentra en la ubicación actual.

Para ejecutar un contenedor basado en la imagen personalizada, se utiliza el siguiente comando en la terminal:

$ docker run -it myubuntu:22.04

El argumento -it permite interactuar con el contenedor mediante la terminal. A continuación, verás la línea de comandos del contenedor Ubuntu, donde podrás ejecutar comandos como ip, ping y dig.
Repositorio en Docker Hub

Primero creamos un usuario con plan gratuito en Docker Hub usando nuestro usuario de GitHub. En la parte de repositorios seleccionamos create repository para crear un nuevo repositorio que llamaremos <cliente_ubuntu>.

Para etiquetar nuestra imagen con el repositorio recién creado usamos el siguiente comando:

$ docker tag myubuntu:22.04 jasdacoss/cliente_ubuntu:22.04

No podemos continuar sin antes ejecutar:

$ docker login

Y subimos en el anterior repositorio la imagen creada con el siguinte comando:

$ docker image push jasdacoss/cliente_ubuntu:22.04

Ahora podemos probar la imagen ejecutando los contenedores:

$ docker run -it jasdacoss/cliente_ubuntu:22.04
$ docker run -it damiannogueiras/cliente_ubuntu:red

Imagen de Apache

Primero creamos una carpeta llamada web en el directorio donde tenemos nuestro archivo Dockerfile_apache.

Dentro de la carpeta web, colocamos los archivos de nuestra página web personalizada, por ejemplo, un archivo index.html y cualquier otro archivo necesario para nuestra página (imágenes, CSS, JavaScript, etc.).

El contenido de la carpeta web se verá algo así:

web/
├── index.html
├── style.css
└── ira.jpg

Abrimos el archivo Dockerfile_apache y agregamos lo siguiente:

# Utilizar la imagen base de Apache
FROM httpd:latest

# Copiar los archivos de la carpeta 'web' al directorio '/usr/local/apache2/htdocs/' del contenedor
COPY ./web/ /usr/local/apache2/htdocs/

# Exponer el puerto 80 para acceder a la página web desde el host
EXPOSE 80

Ejecutamos el siguiente comando para construir la imagen:

$ docker build -t apache:2 -f Dockerfile_apache .

Esto creará una imagen con el nombre apache:2.

Una vez que la imagen se haya construido, podemos ejecutar un contenedor basado en esta imagen utilizando el siguiente comando:

$ docker run -d -p 8080:80 apache:2

Esto ejecutará un contenedor basado en la imagen apache:2 y mapeará el puerto 8080 del host al puerto 80 del contenedor.

Accedemos a la página web personalizada en nuestro navegador utilizando la siguiente URL:

http://localhost:8080/index.html
