# ¿Qué es Docker?

Docker es una herramienta que permite la creación y ejecución de contenedores. Pero… ¿qué es un contenedor?

## Contenedores

Es más importante hacerse una idea de lo que es un contenedor saber lo que es Docker. Un contenedor es un concepto bastante antiguo, que viene del mundo de Linux. Sin querer profundizar en como funciona, hay que pensar que un contenedor es una forma de encapsular una aplicación software junto con las librerías, dependencias, aplicaciones de terceros, … que necesita para funcionar. Se suele hacer un símil con los típicos contenedores de los barcos (de ahí el logo de Docker) ya que podríamos ver estos contenedores como cajas donde metemos todo lo que necesitamos:

![contenedor](/figures/contenedor.png)

Pero creo que podríamos darle una pequeña vuelta más a esta comparación y decir que son algo más como esto:

![bar](/figures/bar.png)

Donde como vemos, todo lo que metemos dentro tiene un propósito. 

Estos contenedores son… Dios. Permiten ser llevados a cualquier máquina y ejecutar la aplicación que guardan sin tener que preocuparnos de las posibles dependencias que esta pueda necesitar. Además, son ligeros y herramientas como Docker permiten que las operaciones sobre ellos sean extremadamente fáciles de realizar.

Es importante, aunque solo sea mencionarlo, que un contenedor no es una máquina virtual como las que se pueden crear con VirtualBox o VMWare. Se parecen mucho, y prácticamente, solventan los mismos problemas. Pero mientras que estas máquinas virtuales funcionan virtualizando un sistema operativo diferente al anfitrión, y sobre ese sistema ejecutan la aplicación; un contenedor se ejecuta directamente sobre el anfitrión. Es decir, el kernel que utiliza es el mismo. Es una de las razones de porque los contenedores son mas ligeros que las máquinas virtuales. **NO LLEVAN EL SISTEMA OPERATIVO ENCIMA.**

![bar](/figures/docker_vs_vm.jpg)

Y con esto, tenemos la idea básica de lo que es un contenedor. No necesitamos saber más, eso se lo dejamos a los informáticos. 

# Vuelta a lo de Docker

Ya sabiendo lo que es un contenedor y lo maravilloso que son, queda más claro porque nuestras almas anhelan hacer uso de ellos. Para eso tenemos Docker, que como se dijo arriba, es una herramienta que permite ‘jugar’ con ellos. Señalar que Docker está disponible en prácticamente todas las plataformas que podamos llegar a usar, pudiendo compartir contenedores entre ellas: Ubuntu, Debian, CentOS, Windows, relojes Casio, …  ¿a qué es maravilloso? 
(pequeño apunte: no en todas funciona por debajo igual, pero a nivel de usuario esto no nos tiene que preocupar)

Antes de ponernos al lio y ver que se puede hacer con Docker, hay algunos conceptos, algunos servicios que son necesarios mencionar:
* Una imagen es un template, una fotografía, a partir de la cual se puede crear un contenedor. Puedes tener una imagen, y a partir de ella crear todos los contenedores que quieras, sabiendo que esos nuevos contenedores serán iguales a esta imagen. 
* Existen repositorios especiales para estas imágenes, conocidos como hub. El oficial es https://hub.docker.com/. En estos repositorios pueden subirse las imágenes y compartirlas.

## Usar docker

Como ya se ha dicho, Docker puede ser instalado en cualquier plataforma. En la página oficial (https://docs.docker.com/get-docker/) podemos encontrar como instalarla para la nuestra. Existen aplicaciones de escritorio como la de Windows, pero eso es para peasants, es mejor preferible usar la consola de comandos. 

A continuación, se van a repasar las operaciones y comandos más habituales. Hay bastantes más, pero por lo general no es necesario conocerlas.

### Listar las imágenes

```
docker image
```
### Listar contenedores

```
docker ps (opciones)

-a: Muestra también los contenedores que están parados.
```

### Crear y ejecutar un nuevo contenedor

Busca la imagen en local. Si no la encuentra, busca en el repositorio oficial (o cualquiera que tengamos configurado). Cuando lo encuentra, lo descarga y crea un contenedor con a partir de la imagen.

```
docker run (opciones) [nombre de la imagen]

-t: Permite usar la terminal de dentro del contenedor.
-i: Activa la entrada estandar (STDIN).
-d: Ejecuta el contenedor en segundo plano.
--name: Permite especificar el nombre del contenedor.
-p: Permite mapear puertos del sistema anfitrión al contenedor.
-v: Crea y une un volumen al contenedor.
```

### Start / Stop / Restart

```
docker start / stop / restart [nombre o ID del contenedor]
```

### Ver logs

```
docker logs (nombre o ID del contenedor)
```

### Ejecutar un comando

Ejecuta un comando **DENTRO** del contenedor.

```
docker exec [nombre o ID del contenedor] [cmd]

-t: Permite usar la terminal de dentro del contenedor.
-i: Activa la entrada estandar (STDIN).
-w: Directorio donde se quiere ejecutar el comando.
```

Este comando se puede aprovechar para acceder al propio docker si se ejecuta bash o sh.

```
docker exec -it [nombre o ID del contenedor] bash
```

### Las tripas de la imagen y el contenedor

```
docker history [nombre o ID de la imagen]
```
```
docker inspect [nombre o ID del contenedor]
```

### Eliminar contenedores e imágenes

Para eliminar un contenedor que esté parado.
```
docker rm [nombre o ID del contenedor]

-f: Fuerza la eliminación, aunque este corriendo el contenedor en ese momento.
```

El equivalente para imagenes.

```
docker rmi [nombre o ID de la imagen]
```

## Crear una imagen

Hemos visto que los contenedores se crean a partir de una imagen, pero... ¿cómo se crea esta imagen?. 

Las imagenes se pueden crear a partir de un contenedor ya creado o a partir de un fichero *Dockerfile*. Estos ficheros dan la receta para concinar nuestra imagen, como un buen cocido casero. 

Estos documentos estan formados por una serie de comandos que se ejecutan secuencialmente. A continuación se muestra un ejemplo de este fichero:

```
# El comando FROM indica la imagen para usar como imagen base.
FROM python:3

# El comando WORKDIR establece el directorio en los que se va a ejecutar los comandos ADD, RUN, ... Como un cd
WORKDIR /

# Los comandos ADD añade un fichero del sistema local al sistema de ficheros de la imagen.
ADD my_script.py /
COPY my_script_2.py /

# El comando RUN ejecuta el comando indicado. Otro ejemplo sería pip install requirements.txt
RUN pip install pystrich

# El comando EXPOSE informa de que la imagen escucha por cierto puerto. SOLO ES INFORMATIVO, NO PUBLICA
EXPOSE 80

# El comando ENV configura una variable de entorno.
ENV host 1.2.3.4

# El comando que se ejecuta cada vez que se arranca el contenedor.
ENTRYPOINT [ "python"]
CMD [ "./my_script.py" ]
```

Una vez se tiene el fichero preparado, se puede crear la imagen con el siguiente comando:

```
docker build [dockerfile]

-t: El tag de la imagen, el nombre
```

Una vez creada, ya la podemos usar como cualquier otra imagen, o incluso subirla al repositorio. Si este repositorio es el hub oficial, será necesario autenticarnos anteriormente usando siguiente comando:

```
docker login
```

IMPORTANTE: Si se quiere subir la imagen al hub oficial, el nombre de la imagen debe terner el formato (usuario)/(tag). Por ejemplo, si el nombre de nuestra imagen es *test*.

```
docker test username/test
```

Con el nombre nuevo, ya podremos subir la imagen con el comando *push*.

```
docker push [nombre o ID de la imagen]
``` 


# docker-compose

Compose es una herramienta para definir aplicaciones que utilicen múltiples contenedores. Al igual que ocurre con Docker, Compose está disponible para múltiples plataformas (https://docs.docker.com/compose/install/).

Compose se basa en un fichero de configuración, al que se le suele dar el nombre de *docker-compose.yml*. En este fichero se escribe la configuración de nuestros contenedores, no son comandos como ocurría en el caso del *Dockerfile*. A continuación un ejemplo de este fichero en el que se crean tres contenedores, a saber: uno ejecuta ES, otro Kibana y el tercero un contenedor dummy con python. Como se ve, es un fichero con formato *YAML*, por lo que la estructura queda marcada por identaciones, y los miembros de las listas por guiones (-).

```
# La primera línea indica la version de la configuración. 
version: '3'

# Los servicios que se van a levantar, los contenedores.
services:
  # El primero es el ES. Se define el nombre y después su configuración. 
  # Cumple una función parecida a la opción --name de docker run.
  elastic:
    # Imagen a partir de la cual se crea el contenedor. 
	# En este caso, la url de esta imagen. 
	# Es análogo al parámetro que indica la imagen al usar docker run.
    image: docker.elastic.co/elasticsearch/elasticsearch:7.3.1
    # Define las variables de entorno dentro del contenedor.
	# Es análogo a la opción -e del comando docker run.
    environment: 
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - discovery.type=single-node
    # Define los volumenes.
	# Es análogo a la opción -v del comando docker run.
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data
	# Define el mapping de los puertos. 
	# Es análogo a la opción -p del comando docker run.
    ports:
      - 9299:9200
      - 9399:9300
  kibana:
    image: docker.elastic.co/kibana/kibana:7.3.1
    environment:
      SERVER_NAME: localhost
      ELASTICSEARCH_HOSTS: http://elastic:9200/
    ports:
      - 8008:5601
  python:
	# Se puede usar un Dockerfile en vez de una imagen.
	# En este caso se indicaría el fichero a usar y Compose se encarga del resto.
    build: .
	# También se puede pasar el comando como se haría en docker run.
    command:
      ['tail', '-f', '/dev/null'] 

# Se definen los volumenes que se van a usar.
volumes:
  elasticsearch_data:
```

Una vez tenemos listo el fichero *docker-compose.yml*, podemos levantar los servicios. A continuación se explican algunos de los comandos más utilizados. Muchos de ellos son análogos a los que se utilizan con Docker. Recordad, que los contenedores creados de este modo siguen siendo contenedores, así que **TODO** lo que se puede hacer con la herramienta docker, también se puede aplicar a estos. Estos comandos deben ser ejecutados dentro de la carpeta que contiene el fichero, de esta forma, docker-compose solo 'verá' los contenedores pertenecientes a este.

### Puesta en marcha

Crea y arranca los contenedores. Si es necesario, construye la imagen.

```
docker-compose up (opciones) 

-d: Ejecuta el contenedor en segundo plano.
```

### Listar contenedores

Solo los que hayan sido creados usando el fichero compose.

```
docker-compose ps
```

### Start / Stop / Restart

El nombre del servicio es el indicado en el fichero compose.

```
docker-compose start / stop / restart [nombre del servicio]
```

### Ejecutar un comando

Ejecuta un comando **DENTRO** del contenedor.

```
docker-compose exec [nombre servicio] [cmd]

-t: Permite usar la terminal de dentro del contenedor.
-i: Activa la entrada estandar (STDIN).
-w: Directorio donde se quiere ejecutar el comando.
```

### Eliminar los contenedores

```
docker-compose rm
```

# ¿Cómo y cuándo usar todo esto?

Siempre... Pero con cabeza.

Cuando se necesite levantar algún servicio tipo una base de datos, un nginx, ... evitas tener que instalarlo en tu máquina. Además, permite que la versión de este servicio sea la que necesitas.

A la hora de desarrollar, sobretodo si hay más gente trabajando en el mismo proyecto. 

Si es necesario, crea una nueva imagen, un nuevo contenedor, ... **IT'S FREE**. Mejor empezar de cero que intentar chapucear y parchear.

Mejor tirar a lo fácil. Antes de crear una imagen, busca en internet. Seguro que un@ más list@ y más loc@ ya lo ha hecho antes.

Si estás haciendo una función sencilla de Python, un análisis sencillo en Jupyter, ... piensa si necesitas usar contenedore. Los entornos de Python/anaconda están ahí para eso.


