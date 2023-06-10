# Práctica 5

# Docker Compose

## Teoría

1. Docker compose es una herramienta que facilita el despliegue de aplicaciones con muchos contenedores. Es el conjunto del archivo compose y archivos de configuración.
2. El archivo compose es un archivo escrito en el lenguaje YAML que alberga todas las características de los contenedores a desplegar. Toda la configuración se define en este archivo. A cada contenedor definido en este archivo se lo denomina “**service**”. Usualmente se lo llama `docker-compose.yml`.
3. Existen tres versiones de archivo compose. La primera está obsoleta, la tercera es la última y que recomienda docker. Las versiones 2 y 3 comparten estructura pero no algunas opciones. En general, las versiones de compose son retrocompatibles, lo que significa que los archivos Compose escritos en versiones anteriores deberían ser compatibles con versiones posteriores. Sin embargo, si utilizas características específicas de una versión más reciente, es posible que no sean reconocidas por una versión anterior.
4. La estructura básica de un archivo compose es:

   ```yaml
   version: ""
   services:
   	web:
   		build: .
   		ports: ":"
   		volumes: ":"
   		enviroment:
   			FLASK_ENV: development
   	redis:
   		image: ":"
   ```

   Algunos bloques son:

   - **`services`**: Este bloque define los servicios individuales de la aplicación. Cada servicio es un **_contenedor_** que se ejecuta como parte de la aplicación. Dentro de este bloque, se especifica el nombre del servicio y se proporcionan sus configuraciones y opciones.
   - **`build`**: Se utiliza para especificar cómo construir una imagen personalizada del contenedor para el servicio. Puede contener la ruta a un directorio con un archivo Dockerfile, o puede ser un objeto que incluya el contexto de construcción, el Dockerfile y otras opciones relacionadas con la construcción de la imagen.
   - **`image`**: En lugar de construir una imagen a partir de un Dockerfile, puedes utilizar una imagen existente de un registro de Docker. En este bloque, puedes especificar el nombre de la imagen y opcionalmente la etiqueta para el servicio.
   - **`volumes`**: Este bloque se utiliza para montar volúmenes en los contenedores del servicio. Puedes especificar un volumen existente o definir uno nuevo, indicando la ruta en el contenedor y, opcionalmente, la ruta en el host.
   - **`restart`**: Se utiliza para definir la política de reinicio del servicio en caso de fallos. Puedes especificar diferentes opciones, como "`no`" (no reiniciar), "`always`" (reiniciar siempre), "`on-failure`" (reiniciar solo si hay un fallo), entre otros.
   - **`depends_on`**: Este bloque especifica las dependencias entre los servicios. Puedes indicar qué servicios deben iniciarse antes de que se inicie el servicio actual. Esto asegura que los servicios dependientes estén disponibles antes de que se inicie un servicio específico.
   - **`environment`**: Aquí se pueden establecer variables de entorno para el servicio. Puedes proporcionar valores clave-valor para configurar variables de entorno dentro del contenedor.
   - **`ports`**: Se utiliza para exponer puertos del contenedor y vincularlos a puertos del host. Puedes especificar los puertos del host y del contenedor en el formato "`host:contenedor`".
   - **`expose`**: A diferencia de **`ports`**, el bloque **`expose`** solo expone puertos del contenedor internamente a otros servicios dentro de la misma red de Docker Compose. No se exponen al host.
   - **`networks`**: Aquí puedes especificar las redes en las que se deben conectar los servicios. Puedes utilizar redes predeterminadas o crear tus propias redes personalizadas para aislar los servicios y controlar la comunicación entre ellos.

5. - **`docker-compose create`** y **`docker-compose up`**:
     - **`docker-compose create`** crea los contenedores definidos en el archivo Compose, pero no los inicia automáticamente. Debes usar **`docker-compose start`** posteriormente para iniciar los contenedores.
     - **`docker-compose up`** crea y luego inicia los contenedores definidos en el archivo Compose.
   - **`docker-compose stop`** y **`docker-compose down`**:
     - **`docker-compose stop`** detiene los contenedores definidos en el archivo Compose sin eliminarlos. Puedes usar **`docker-compose start`** posteriormente para reiniciarlos.
     - **`docker-compose down`** detiene y elimina los contenedores definidos en el archivo Compose. También elimina las redes y los volúmenes asociados, pero no elimina las imágenes construidas.
   - **`docker-compose run`** y **`docker-compose exec`**:
     - **`docker-compose run`** crea e inicia un nuevo contenedor para ejecutar un comando específico definido en el archivo Compose. El contenedor se detendrá y se eliminará una vez que se complete el comando.
     - **`docker-compose exec`** ejecuta un comando dentro de un contenedor en ejecución definido en el archivo Compose. El contenedor sigue en ejecución después de que se complete el comando.
   - **`docker-compose ps`**: Muestra el estado de los servicios/contenedores definidos en el archivo Compose. Proporciona información sobre los contenedores, como nombres, estado, puertos expuestos y nombres de los servicios asociados.
   - **`docker-compose logs`**: Muestra los registros (logs) de los servicios definidos en el archivo Compose. Puedes utilizar opciones adicionales para filtrar registros específicos o mostrar registros en tiempo real.
6. Docker Compose admite varios tipos de volúmenes que se pueden utilizar en un archivo Compose. Algunos de los tipos comunes de volúmenes son:
   - **Volumen anónimo**: Se crea automáticamente por Docker y no se le asigna un nombre. Puede ser útil para compartir datos temporales entre contenedores. Se declara en el archivo Compose mediante la opción **`volume`**.
   - **Volumen nombrado**: Se le asigna un nombre explícito y persiste incluso cuando los contenedores se eliminan. Se puede utilizar para compartir datos persistentes entre contenedores o para realizar copias de seguridad de datos. Se declara en el archivo Compose mediante la opción **`volume`** y se proporciona un nombre.
   - **Volumen de host**: Permite montar un directorio o archivo del sistema de archivos del host en el contenedor. Puede ser útil para compartir datos entre el host y el contenedor. Se declara en el archivo Compose mediante la opción **`volume`** y se especifica la ruta en el host y en el contenedor.
   - **Volumen de tipo bind**: Similar al volumen de host, pero ofrece más flexibilidad para controlar los permisos y las opciones de montaje. Se puede utilizar para montar directorios o archivos específicos en el host en el contenedor. Se declara en el archivo Compose mediante la opción **`volume`** y se especifica la ruta en el host y en el contenedor.
7. Si utilizas el comando **`docker-compose down -v`** o **`docker-compose down --volumes`**, además de detener y eliminar los contenedores, también se eliminarán los volúmenes definidos en el archivo Compose. Esta opción se utiliza para limpiar completamente los volúmenes junto con los contenedores y las redes asociadas. Es útil cuando deseas eliminar completamente todos los datos persistentes y comenzar desde cero. Sin embargo, ten en cuenta que esta acción es irreversible y se eliminarán todos los datos almacenados en los volúmenes. Se recomienda tener cuidado al utilizar esta opción y asegurarse de hacer copias de seguridad de los datos importantes antes de ejecutar el comando.

## Taller

- Paso 1: `sudo curl -SL [https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64](https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64) -o /usr/local/bin/docker-compose`
- Paso 2: `sudo chmod +x /usr/local/bin/docker-compose`.
- Paso 3: `docker-compose --version`

### Ejercicio guiado

1. Se instancian dos contenedores para los servicios: `db` y `wordpress`. uno para la base de datos MySQL y otro para la aplicación de WordPress.
2. En este caso, no se necesitan Dockerfiles porque se está utilizando una imagen de contenedor predefinida tanto para el servicio "db" (imagen de MySQL) como para el servicio "wordpress" (imagen de WordPress). Las imágenes se obtienen directamente desde Docker Hub, lo que significa que no es necesario definir Dockerfiles personalizados.
3. Esto es debido a que el servicio WordPress depende del servicio de la base de datos, por lo tanto requiere que se inicie primero el servicio de la BD. En otras palabras, antes de que se inicie el contenedor de WordPress, se espera que el contenedor de la base de datos ya esté en funcionamiento.
4. Tiene dos volúmenes. El contenedor de la base de datos tiene un volumen nombrado llamado "`db_data`" asociado a la ruta "`/var/lib/mysql`".

   El contenedor de WordPress tiene dos volúmenes asociados:

   - El volumen nombrado "`wordpress_data`" asociado a la ruta "`/var/www/html`".
   - Un volumen anónimo que se mapea al directorio actual (directorio de trabajo) del host en la ruta "`/data`" dentro del contenedor.

5. El uso del volumen nombrado "`db_data`" para el servicio de la base de datos se realiza para asegurar la persistencia de los datos de la base de datos. Al utilizar un volumen nombrado, los datos almacenados en la ruta "`/var/lib/mysql`" dentro del contenedor persistirán incluso si el contenedor se detiene o se elimina. Esto permite que los datos de la base de datos se conserven entre ejecuciones del contenedor.
6. La línea "`volumes: - ${PWD}:/data`" en la definición de WordPress mapea el directorio de trabajo actual (PWD, por sus siglas en inglés) del host en la ruta "/data" dentro del contenedor de WordPress. Esto permite que los archivos en el directorio de trabajo del host estén disponibles dentro del contenedor de WordPress en la ruta "/data". Es útil para, por ejemplo, compartir archivos o mantener archivos de configuración personalizados.
7. La información del bloque `enviroment` son variables de entorno para el servicio. Cada variable de entorno tiene un nombre y un valor. En el archivo de configuración proporcionado, se definen variables de entorno relacionadas con la configuración de la base de datos y de WordPress. Estas variables de entorno se utilizan en los contenedores para configurar la base de datos, el usuario, la contraseña, etc.
8. Si cambias los valores de las variables de entorno definidas en el bloque `environment` solo en uno de los contenedores, los cambios afectarán solo a ese contenedor específico. Cada contenedor se instancia de forma independiente y tiene su propia configuración de variables de entorno. En este caso, si cambias el valor de la variable "`WORDPRESS_DB_NAME`" solo en la definición de WordPress, solo el contenedor de WordPress utilizará el nuevo valor para esa variable, mientras que el contenedor de la base de datos mantendrá su valor original.
9. El contenedor de WordPress puede comunicarse con el contenedor de la base de datos utilizando el nombre del servicio como un alias de host. En el archivo de configuración, el servicio de la base de datos se llama "db". Dentro del contenedor de WordPress, se puede utilizar "db" como el nombre de host para conectarse a la base de datos. Docker Compose se encarga de la resolución de nombres y crea una red interna donde los contenedores pueden comunicarse entre sí utilizando los nombres de servicio como alias de host.
10. El archivo de configuración de Docker Compose no indica explícitamente los puertos expuestos en los Dockerfiles de las imágenes utilizadas. Sin embargo, si se consulta los Dockerfiles de las imágenes de WordPress ([https://hub.docker.com/\_/wordpress](https://hub.docker.com/_/wordpress)) y MySQL ([https://hub.docker.com/\_/mysql](https://hub.docker.com/_/mysql)) en Docker Hub, se puede determinar la información de los puertos expuestos. Según los Dockerfiles, el contenedor de WordPress expone el puerto 80 para la comunicación web, mientras que el contenedor de MySQL no expone puertos.
11. El servicio de WordPress se "publica" para ser accedido desde el exterior en el puerto 8000 del host local (`127.0.0.1:8000`). Esto se define en la línea "`ports: - 127.0.0.1:8000:80`" en el archivo de configuración. El puerto 8000 del host se mapea al puerto 80 del contenedor de WordPress. Esto permite acceder a WordPress desde un navegador en el host local utilizando la dirección IP 127.0.0.1 y el puerto 8000.
