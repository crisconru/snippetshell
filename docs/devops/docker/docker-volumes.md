# Docker Volumes

Información oficial -> [aquí](https://docs.docker.com/storage/volumes/)

Existen 3 tipos de volumenes:

* Host Volumes -> Una carpeta del Host.
* Named Volumes -> Un volumen con nombre creado con Docker
    * No deja de ser un Host Volume pero en la carpeta `/../docker/volumes/<nombre-volumen>`
* Anonymous Volumes -> Es igual que Named Volumes solo que el nombre es un hash que asigna docker.

## Manipular (crear, listar, destruir...) volumenes con `docker volume`

```bash
# Crear volumen
docker volume create <nombre-volumen>
# Listar volumenes
docker volume ls
# Borrar volumen
docker volume rm <nombre-volumen>
```

Se recomienda asignar nombre a los volúmenes, ya que sino serán volúmenes anónimos y es más complicado nombrarlos (hay que usar su id).

## Montar volumen con `docker run`

```bash
# Ejecutar contenedor con un volumen host montado en el contenedor
docker run -v [<directorio-host-o-volumen-definido>:]<directorio-contenedor> -t <nombre-contenedor>
# Borrar tanto el contendor como su volumen
docker rm -v <nombre-contenedor>
```

**OJO** -> Si queremos montar una carpeta local específica en un contenedor, solo podemos hacerlo con este comando. No se puede montar una carpeta local específica desde el Dockerfile ya que el path podría cambiar. Por ejemplo si queremos montar la propia carpeta en la que estamos haríamos

```bash
docker run -v $(pwd):<directorio-contenedor> <contenedor>
```

Si no indica el `<directorio-host-o-volumen-definido>`, se creará un volumen anónimo.  

## Montar volumen con `Dockerfile`

Desde el Dockerfile podemos hacer lo mismo, pero solo crea volumenes anónimos. Es decir, que no podemos montar una carpeta local específica.

```bash
VOLUME <directorio-contenedor>
```

## Dangling Volumes

Si al borrar los contenedores, no especificamos que borre sus volumenes con `-v`, quedarán los llamados ***Dangling Volumes***. Para verlos y borrarlos

```bash
# Ver todos los volumenes dangling
docker volume ls -f dangling=true
# Listar todos los ids de los dangling volumes | Borrarlos
docker volume ls -f dangling=true -q | xargs docker volume rm
```
