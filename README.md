# ¿Qué es Docker?

Docker es una herramienta que permite la creación y ejecución de contenedores. Pero… ¿qué es un contenedor?

## Contenedores

Es más importante hacerse una idea de lo que es un contenedor saber lo que es Docker. Un contenedor es un concepto bastante antiguo, que viene del mundo de Linux. Sin querer profundizar en como funciona, hay que pensar que un contenedor es una forma de encapsular una aplicación software junto con las librerías, dependencias, aplicaciones de terceros, … que necesita para funcionar. Se suele hacer un símil con los típicos contenedores de los barcos (de ahí el logo de Docker) ya que podríamos ver estos contenedores como cajas donde metemos todo lo que necesitamos:

![contenedor](/figures/contenedor.png)

Pero creo que podríamos darle una pequeña vuelta más a esta comparación y decir que son algo más como esto:

![bar](/figures/bar.png)

Donde como vemos, todo lo que metemos dentro tiene un propósito. 

Estos contenedores son… Dios. Permiten ser llevados a cualquier máquina y ejecutar la aplicación que guardan sin tener que preocuparnos de las posibles dependencias que esta pueda necesitar. Además, son ligeros y herramientas como Docker permiten que las operaciones sobre ellos sean extremadamente fáciles de realizar.

Es importante, aunque solo sea mencionarlo, que un contenedor no es una máquina virtual como las que se pueden crear con VirtualBox o VMWare. Se parecen mucho, y prácticamente, solventan los mismos problemas. Pero mientras que estas máquinas virtuales funcionan virtualizando un sistema operativo diferente al anfitrión, y sobre ese sistema ejecutan la aplicación; un contenedor se ejecuta directamente sobre el anfitrión. Es decir, el kernel que utiliza es el mismo. Es una de las razones de porque los contenedores son mas ligeros que las máquinas virtuales. NO LLEVAN EL SISTEMA OPERATIVO ENCIMA.

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

A continuación se van a repasar las operaciones y comandos más habituales. Hay bastantes más, pero por lo general no es necesario conocerlas.

### Listar las imágenes

```
docker image
```

### Crear y ejecutar un nuevo contenedor

```
docker run
```

### Listar contenedores

```
docker ps
```

### Start / Stop / Restart

```
docker start / stop / restart
```

### Ver logs

```
docker logs
```

### Ejecutar un comando

```
docker exec
```

### Las tripas del contenedor

```
docker history
```
```
docker inspect
```

### Eliminar contenedores e imágenes

```
docker rm
```
```
docker rmi
```