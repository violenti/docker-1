# Que es docker

### Es una tecnología de virtualización basada en contenedores

Las ventajas es que se evita el famoso "funciona en mi pc"; consume menos recursos; además de permitir tener N servidores.

# Cómo se instala
ir a la web de [docker ce -- community edition](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce "Title")

1. __preparo el repo para poderlo descargar__
	> sudo add-apt-repository \
	> "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
	> $(lsb_release -cs) \
	> stable"

2. __actualizo el sistema__
	> sudo apt-get update 
3. __instalo docker__ 
	> sudo apt-get install docker-ce
    
4. __verifico la instalación__ 
	> docker version
	
	_si tengo el mensaje de permission denied_ 
	
	> sudo usermod -aG docker nombreDeUsuario
	
	__cierro sesión y vuelvo a iniciar__
	verifico nuevamente con docker version
		
### Instalación en equipos legacy (mac o windows) 
[docker toolbox](https://docs.docker.com/toolbox/overview/ "Title")
en mac no usar iterm, sino terminal
docker version mostrará la ip que usaremos.

# Conceptos importantes
### Terminología


1. **Imágenes:** 
	- Plantilla solo lectura usada para crear contenedores
	- Son creadas con __docker build__ ya sea por nosotros o por otros usuarios de Docker
	- son creadas también a partir del comando __docker commit__  mientras se esté trabajando con un contenedor de modo interactivo
	- Se componen de otras imágenes (puedo combinar nginx con ubuntu para php, etc.)
	- Se almacenan en un registry de docker.
	
2. **Contenedores:**
	- Son la instancia de una clase, nace a partir de una imagen
	- Son una encapsulación ligera y portable de un entorno en donde se ejecutan aplicaciones.
	- Son creados a partir de imágenes. Dentro de un contenedor están todos los archivos binarios y dependencias para ejecutar las aplicaciones. 
	
3. **Registries y repositorios:**
	- Es donde se almacenan las imágenes de docker.
	- Existen registries privados o podemos usar el público de Docker llamado Docker Hub.
	- Dentro del registry se almacenan las imágenes y los repos.
	- Un repo de docker es una colección de diferentes imágenes con el mismo nombre pero con diferente tag, cada una de estas representando la versión de la imagen. 
	
####Se recomienda usar las imágenes oficiales, ya que están documentadas, tienen equipos dedicados a revisar el contenido de las imágenes oficiales para de esa manera realizar actualizaciones de seguridad cuando sea necesario. 

#Creación del primer contenedor

1. Vamos a [hub.docker.com](https://hub.docker.com "Title")
	- buscamos *alpine* official, la vamos a usar porque es pequeña y liviana.
	> docker run alpine echo "Hola Mundo"
	- con docker images podemos ver que se descargó la imagen de docker, además de diferentes datos adicionales, el tag, el id de imagen, cuándo se creó y el tamaño.
	> docker images
	- **docker run alpine ls /** muestra un listado de los archivos y directorios
	- también podemos crear una seccion interactiva
	 > **docker run -i -t alpine sh**
	 
	   -i input
	   -t tty (terminal nativa del contenedor)
	 > touch prueba.txt
	 > vi prueba.txt
	 > hola mundo
	 > exit
	 
	- Si creo otro contenedor no existirá lo creado anteriormente, porque cada contenedor es independiente de los demás, están aislados.
	
### Contenedores en profundidad
-d --detach crea el contenedor en el background, al crearlo me mostrará el id del contendor.
	
> docker run -d alpine sleep 1000
	 
Puedo ver los contenedores con status up
> docker ps 
	
Con este comando veo todos los contenedores, up o down	
> docker ps -a 

Con este comando creo un contenedor que se dormirá por 1 segundo y se eliminará luego de terminada su ejecución	
> docker run --rm alpine sleep 1
	
Puedo especificar el nombre del contenedor, sino docker le pone uno random
> docker run --rm -d --name Alpine alpine  sleep 1000
	
Puedo ver los contenedores que estén up o down
> docker ps -a

Puedo ver información de bajo nivel, por ejemplo la IP, configuración del servidor, etc.
> docker inspect Alpine
	
## Mapeo de Puertos

### Vamos usar un servidor web con nginx y su puerto 80 
> docker run --rm --name nginx -d -p 8080:80 nginx

Ejecuto docker ps para verificar que se esté ejecutando en el servidor
> docker ps
		
En el navegador pruebo: localhost:8080
Puedo usar cualquier puerto que desee, también puedo hacer docker inspect y acceder a la ip del contenedor.

> docker inspect nginx  >> 172.17.0.3

#### Previamente genero una carpeta para alojar index.html
> mkdir nginx/html

> nano index.html

Para enviarle contenido a nginx creo un **volumen**, nos permitirá compartir un directorio desde nuestro host a nuestro contenedor. 
>  docker run --rm --name nginx -d -p 8080:80 -v $(pwd)/nginx/html:/usr/share/nginx/html nginx

*siempre desde el directorio raíz*

Puedo modificar index.html en la carpeta nginx/html y ver los cambios reflejados en el momento.

Desde docker logs veo qué sucede con el contenedor.

> docker logs nginx

Con el parámetro -f veo qué sucede en cada momento

> docker logs -f nginx



	 
	
	


