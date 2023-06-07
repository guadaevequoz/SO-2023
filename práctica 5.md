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
    
    - **`services`**: Este bloque define los servicios individuales de la aplicación. Cada servicio es un ***contenedor*** que se ejecuta como parte de la aplicación. Dentro de este bloque, se especifica el nombre del servicio y se proporcionan sus configuraciones y opciones.
    - **`build`**: Se utiliza para especificar cómo construir una imagen personalizada del contenedor para el servicio. Puede contener la ruta a un directorio con un archivo Dockerfile, o puede ser un objeto que incluya el contexto de construcción, el Dockerfile y otras opciones relacionadas con la construcción de la imagen.
    - **`image`**: En lugar de construir una imagen a partir de un Dockerfile, puedes utilizar una imagen existente de un registro de Docker. En este bloque, puedes especificar el nombre de la imagen y opcionalmente la etiqueta para el servicio.
    - **`volumes`**: Este bloque se utiliza para montar volúmenes en los contenedores del servicio. Puedes especificar un volumen existente o definir uno nuevo, indicando la ruta en el contenedor y, opcionalmente, la ruta en el host.
    - **`restart`**: Se utiliza para definir la política de reinicio del servicio en caso de fallos. Puedes especificar diferentes opciones, como "`no`" (no reiniciar), "`always`" (reiniciar siempre), "`on-failure`" (reiniciar solo si hay un fallo), entre otros.
    - **`depends_on`**: Este bloque especifica las dependencias entre los servicios. Puedes indicar qué servicios deben iniciarse antes de que se inicie el servicio actual. Esto asegura que los servicios dependientes estén disponibles antes de que se inicie un servicio específico.
    - **`environment`**: Aquí se pueden establecer variables de entorno para el servicio. Puedes proporcionar valores clave-valor para configurar variables de entorno dentro del contenedor.
    - **`ports`**: Se utiliza para exponer puertos del contenedor y vincularlos a puertos del host. Puedes especificar los puertos del host y del contenedor en el formato "`host:contenedor`".
    - **`expose`**: A diferencia de **`ports`**, el bloque **`expose`** solo expone puertos del contenedor internamente a otros servicios dentro de la misma red de Docker Compose. No se exponen al host.
    - **`networks`**: Aquí puedes especificar las redes en las que se deben conectar los servicios. Puedes utilizar redes predeterminadas o crear tus propias redes personalizadas para aislar los servicios y controlar la comunicación entre ellos.
5. 
    - **`docker-compose create`** y **`docker-compose up`**:
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
