# Conceptos generales

1. Una system call (o llamada al sistema) es un mecanismo para que las aplicaciones soliciten operaciones del kernel para realizar tareas que están fuera de su alcance. Se utiliza para acceder a recursos del sistema operativo, como el acceso a archivos, la comunicación entre procesos, el manejo de la memoria, entre otros. En resumen, las system calls permiten realizar tareas avanzadas de forma segura y controlada.
2. La macro **syscall** permite crear syscalls, sirve cuando se quiere crear una syscall especial para el sistema. Además, se crea de forma genérica por lo cual la función no sabe nada en particular sobre el sistema. Cuando se define una syscall uno de los parámetros más importantes es el número de syscall llamado `sysno`, el cual es el número de system call y es único para cada syscall. El próximo parámetro es en general los argumentos propios que se le pasarán a la syscall, los argumentos varían según el sistema operativo y la llamada al sistema en particular. En general, estos argumentos son valores o punteros que se utilizan para configurar el comportamiento de la llamada al sistema. Además, existe un parámetro para definir el número de argumentos.
3. En el archivo `<kernel_code>/arch/x86/syscalls/syscall_32.tbl` se guarda la tabla de system calls para la arquitectura de x86 de 32 bits. Por otro lado, el archivo `<kernel_code>/arch/x86/syscalls/syscall_64.tbl` guarda la tabla de system calls para la arquitectura de x86 de 64 bits.
4. La macro **asmlinkage**instruye al compilador a pasar parámetros por stack y no por registros, es decir, se utiliza para indicar que una función debe ser compilada utilizando la convención de llamada especifica de kernel garantizando que la función se llame correctamente desde el kernel y que los argumentos pasen de la manera adecuada.
5. Strace es una herramienta que enlista las syscalls que realizan los programas, se puede utilizar junto a otros parámetros para obtener información personalizada; por ejemplo si se usa junto a `-f` se tiene en cuenta además las syscalls de los procesos hijos.

# System calls

1. Agregar `common rqinfo  sys_rqinfo` al final de `<kernel_code>/arch/x86/syscalls/syscall_64.tbl`. ❌ **\*ESTÁ MAL, LO ARREGLÉ EN EL PUNTO 4!!**

   ```bash
   echo "common rqinfo  sys_rqinfo" >> syscall_64.tbl
   ```

2. Incluir `asmlinkage long sys_rqinfo(unsigned long *ubuff, long len);` en `<kernel_code>/include/linux/syscalls.h`. Lo hice haciendo `nano syscalls.h` y la línea que hay que agregar es casi al final de todo.

   ![Untitled](/img/tp2-1.png)

3. Incluí el código de la syscall en `core.h`.

   ![Untitled](/img/tp2-2.png)

4. Compilando el kernel con `make -j`**.** Tiro error en la compilación, así que hice `make clean` y arregle el punto 1, en vez de hacerlo con echo lo hice con nano y lo agregué en la fila que dice tal cual dice la práctica. Aún así me volvió a tirar error el error: referencia a ‘\_\_x64_sys_rqinfo' sin definir

   ![Untitled](/img/tp2-3.png)

5. Generé con `touch prueba_inforq.c` el archivo y luego le agregué el código:

   ![Untitled](/img/tp2-4.png)

   Claramente no funcionó, estimo será porque no me compilo el kernel desde un principio entonces no puede llamar a rqinfo.

   ![Untitled](/img/tp2-5.png)

# Monitoreando System Calls

1. No me funcionaba el **`strace`**, así que ejecute el comando del punto 5 anterior e hice `strace ./mi_programa` . Me imprimió

   ![Untitled](/img/tp2-6.png)

2. La diferencia que se puede notar entre ellos es que tienen diferente `pid`.

   1. Código 1:

      ![Untitled](/img/tp2-7.png)

   2. Código 2:

      ![Untitled](/img/tp2-8.png)

# Módulos y Drivers

## Conceptos generales

1. En GNU/Linux la porción de código que se agrega al kernel en tiempo de ejecución es el modulo del kernel, estos módulos son archivos que contienen código ejecutable que se carga en el kernel en tiempo de ejecución para agregar nuevas funcionalidades al SO. La carga de módulos en tiempo de ejecución es una característica importante de Linux porque permite agregar nuevas funcionalidades al sistema operativo sin tener que reiniciar el sistema. Para poder cargar los módulos en tiempo de ejecución se utilizan los comandos `insmod` o `modprobe` seguido del módulo. Sólo es necesario reiniciar el sistema si el módulo afecta a secciones críticas del kernel.

   En caso de no poder utilizar módulos se debería agregar código al kernel y por cada cambio de código se debería reiniciar por completo el sistema.

2. Un driver es un módulo que se utiliza para poder administrar cada dispositivo. En otras palabras, es una porción de código que permite que un dispositivo se comunique con el SO y las aplicaciones del mismo.
3. Los drivers son necesarios para que cada dispositivo pueda comunicarse con el SO y estar disponible para el mismo, esto es debido a que todos los dispositivos tienen diferentes características y resulta imposible realizar un driver general para todos. Sin un driver, el sistema operativo no puede comunicarse con el dispositivo y, por lo tanto, no puede utilizar sus funcionalidades.

   El driver actúa como un traductor entre el dispositivo y el sistema operativo, permitiendo que los comandos del sistema operativo sean entendidos y ejecutados por el dispositivo. El driver también permite que el dispositivo envíe información al sistema operativo, como notificaciones de eventos, errores y datos de estado.

   Por lo tanto, escribir drivers es esencial para que los dispositivos de hardware funcionen correctamente y puedan ser utilizados por el sistema operativo y las aplicaciones que se ejecutan en él.

4. Un driver es considerado un módulo debido a que proporciona funcionalidades extra al sistema operativo, estas son la comunicación entre el SO y el dispositivo.
5. Un bug en un driver o módulo tendrán consecuencias graves en el funcionamiento del sistema operativo y en algunos casos en la seguridad del sistema. Además, puede ocurrir perdida de datos y problemas de compatibilidad.
6. Existen drivers de bloques, de carácter y de red. Los drivers de bloques se utilizan para dispositivos que funcionan en modo de bloque y contienen conjuntos de datos persistentes, se escriben y leen datos en forma de bloques de datos. Los drivers de caracteres se utilizan con dispositivos que funcionan en modo carácter, como teclados, estos drivers leen y escriben de 1 byte a la vez. Por último, los drivers de red son los utilizados para dispositivos de red, como tarjetas de red, estos drivers controlan la transmisión y recepción de datos a través de la red.
7. En el directorio `/dev` se encuentran los archivos representativos de los dispositivos que se conectan al SO. Estos archivos son la representación de los dispositivos. En este directorio encontramos tipos de archivos como son dispositivos de caracteres, de bloques y de red. Además, en el directorio se pueden encontrar otros archivos y subdirectorios que representan recursos del sistema como el archivo random que genera números aleatorios, el archivo zero que devuelve datos vacíos o el subdirectorio shm que se utiliza para la comunicación interproceso mediante memoria compartida.
8. El archivo `/lib/modules/<version>/modules.dep` es utilizado por el comando `modprobe` en GNU/Linux para conocer las dependencias de los módulos del kernel que se deben cargar cuando se carga un módulo determinado.

   Cuando se carga un módulo de kernel con el comando `modprobe`, el sistema busca en el archivo `modules.dep` para determinar qué otros módulos dependen del módulo que se está cargando. Entonces, `modprobe` carga automáticamente estas dependencias en el orden adecuado para asegurarse de que todas las dependencias se resuelvan correctamente.

   Este archivo es generado automáticamente por el sistema y contiene información sobre las dependencias de los módulos del kernel. Cada línea en el archivo describe una dependencia entre dos módulos, donde el primer módulo depende del segundo.

## Agregando un módulo a nuestro kernel

1.  ![Untitled](/img/tp2-9.png)

2.  La principal utilidad del archivo **`Makefile`** es definir las reglas de construcción y las dependencias entre los diferentes componentes de un proyecto. Permite especificar cómo se compilan los archivos fuente, cómo se enlazan para crear ejecutables o bibliotecas, y qué acciones se deben realizar para mantener el proyecto actualizado.

    La macro **`MODULE_LICENSE`** es una directiva utilizada en el desarrollo de módulos de kernel en el sistema operativo Linux. Esta macro se utiliza para especificar la licencia bajo la cual se distribuye y se permite el uso del módulo. La licencia indicada en la macro **`MODULE_LICENSE`** es verificada por el kernel al cargar el módulo, y es parte de los requisitos legales y de licencia para los módulos de kernel. La inclusión de la macro **`MODULE_LICENSE`** en un módulo de kernel es importante para cumplir con las regulaciones y requisitos de licencia establecidos por el kernel de Linux. Sin embargo, no es obligatorio en el sentido de que el código del módulo no se ejecutará o fallará si no se especifica una licencia.

3.  Cuando ejecuto: `make -C $HOME/kernel/linux-5.16` me salta esto:

    ![Untitled](/img/tp2-10.png)

    Traté de ejecutarlo de nuevo con la dirección que decía en la explicación y la salida cambió:

    ![Untitled](/img/tp2-11.png)

    Se generan: archivos objeto (`memory.o`) , archivo del módulo (`memory.ko`), archivo de configuración (`Module.symvvers`) y `memory.mod.o`, que tiene metadata del módulo.

4.  El comando `insmod` trata de cargar el módulo especificado mientras que `modprobe` emplea la información generada por `depmod` e información de `/etc/modules.conf` para cargar el módulo especificado.

    Ejecutar `insmod memory.ko` me tiraba error así que tuve que entrar y ejecutarlo como root y todo ok.

5.  La salida del comando es:

    ![Untitled](/img/tp2-12.png)

    La utilidad de `lsmod` es listar los módulos cargados.

    El archivo **`/proc/modules`** en Linux contiene información sobre los módulos del kernel cargados en el sistema. Cada línea del archivo muestra detalles sobre un módulo específico, como su nombre, tamaño, cuenta de referencias, dependencias y estado. Esta información es útil para obtener una visión general de los módulos cargados y para el diagnóstico y monitoreo del kernel en tiempo real.

    En el ejemplo dado podemos observar que, tomando la primera línea del ejemplo, primero está el nombre del módulo (`parport_pc`), luego el tamaño (`37412`), número de referencias del módulo que están siendo utilizadas (`0`), dependencias (`-`) y el estado (`Live`).

    Para eliminar un módulo del kernel en Linux, se utiliza el comando **`rmmod`**. Este comando permite descargar un módulo previamente cargado y liberar los recursos asociados a él.

6.  ![Untitled](/img/tp2-13.png)

7.  Las funciones **`module_init`** y **`module_exit`** son utilizadas en los módulos del kernel de Linux para especificar el punto de entrada y salida del módulo, respectivamente.

    - **`module_init`**: Se utiliza para definir la función que se ejecuta al cargar el módulo en el kernel, generalmente para realizar inicializaciones necesarias.
    - **`module_exit`**: Se utiliza para definir la función que se ejecuta al descargar el módulo del kernel, generalmente para realizar limpieza o liberación de recursos.

    Para ver la información de registro (log) generada por estas funciones, se puede utilizar el comando **`dmesg`** en la terminal, que muestra el registro del kernel. Los mensajes correspondientes a la inicialización (**`module_init`**) y salida (**`module_exit`**) del módulo se pueden encontrar al ejecutar el comando **`dmesg`**.

    En Linux, los dispositivos se clasifican en tres categorías principales:

    1. **Dispositivos de bloque**: Proporcionan acceso a nivel de bloque a los datos. Son dispositivos de almacenamiento, como discos duros y unidades flash. Permiten operaciones de lectura y escritura en bloques de datos.
    2. **Dispositivos de caracteres**: Proporcionan acceso a nivel de carácter a los datos. Se utilizan para la entrada/salida básica y la comunicación con dispositivos periféricos. Permiten operaciones de lectura y escritura de caracteres individuales o secuencias de caracteres.
    3. **Dispositivos de red**: Se utilizan para la comunicación en red. Proporcionan interfaces de red para la transferencia de datos a través de Ethernet, Wi-Fi u otras tecnologías de red. Permiten operaciones de envío y recepción de paquetes de red.

    Cabe destacar que existen otros tipos de dispositivos especializados, como dispositivos de sonido y dispositivos de video, que no se incluyen en estas categorías principales.

## Desarrollando un Driver

1.  1. La estructura **`ssize_t`** y **`memory_fops`** son utilizadas en el desarrollo de controladores de dispositivos en el kernel de Linux.

       1. **`ssize_t`**:
          - Propósito: **`ssize_t`** es un tipo de datos utilizado para representar el tamaño o la cantidad de bytes que se leen o escriben en una operación de lectura o escritura en un dispositivo.
          - Características: **`ssize_t`** es un tipo de datos con signo que se utiliza para manejar tamaños y resultados de operaciones de lectura/escritura en dispositivos en el kernel de Linux. Puede tener valores negativos para indicar errores, cero para indicar el final de un archivo o un valor positivo para indicar el número de bytes leídos o escritos.
       2. **`memory_fops`**:
          - Propósito: **`memory_fops`** es una estructura que contiene las operaciones y funciones de manejo para un controlador de dispositivo específico. Se utiliza para definir cómo el controlador de dispositivo interactúa con las operaciones de lectura, escritura, apertura, cierre, etc., realizadas en el dispositivo.
          - Características: La estructura **`memory_fops`** es parte de la implementación de un controlador de dispositivo y contiene punteros a las funciones que se deben llamar cuando se realiza una operación específica en el dispositivo, como lectura o escritura. Estas funciones son implementadas por el programador del controlador y definen el comportamiento del dispositivo en respuesta a las operaciones realizadas en él.

       **`register_chrdev`** y **`unregister_chrdev`** se utilizan en el proceso de desarrollo de controladores de dispositivos en Linux para registrar y desregistrar el número de dispositivo correspondiente y establecer las operaciones y funciones de manejo asociadas al controlador.

       - **`register_chrdev`**: Esta función se utiliza para registrar un número de dispositivo de caracteres en el kernel. Recibe como argumentos el número mayor y el nombre del controlador de dispositivo, así como una estructura **`file_operations`** que define las operaciones del controlador.
       - **`unregister_chrdev`**: Esta función se utiliza para desregistrar un número de dispositivo de caracteres previamente registrado en el kernel. Recibe como argumento el número mayor del dispositivo.

    2. El kernel de Linux sabe qué funciones del controlador de dispositivo invocar para leer y escribir en un dispositivo mediante el uso de la estructura **`file_operations`**. Esta estructura contiene punteros a las funciones que se deben llamar cuando se realiza una operación específica en el dispositivo.

       Cuando se realiza una operación de lectura o escritura en el dispositivo, el kernel identifica el controlador de dispositivo correspondiente y accede a la estructura **`file_operations`** asociada a ese controlador. Luego, utiliza los punteros a funciones en la estructura para invocar la función apropiada, como **`read`** para una operación de lectura o **`write`** para una operación de escritura.

       El programador del controlador de dispositivo es responsable de implementar estas funciones referenciadas en la estructura **`file_operations`** de acuerdo con la funcionalidad requerida para el dispositivo. Estas funciones se encargan de manejar la lógica específica para leer y escribir en el dispositivo, interactuando con él a través de llamadas a otras funciones y manipulando los datos según sea necesario.

       En resumen, el kernel utiliza la estructura **`file_operations`** y sus punteros a funciones para determinar qué funciones del controlador de dispositivo invocar al realizar operaciones de lectura y escritura en el dispositivo.

    3. En Linux, se accede a los dispositivos desde el espacio de usuario utilizando archivos especiales de dispositivo ubicados en el directorio **`/dev`**. Estos archivos proporcionan una interfaz para comunicarse con los controladores de dispositivo y realizar operaciones de lectura, escritura y control.

       El acceso a los dispositivos desde el espacio de usuario se realiza a través de los siguientes pasos:

       1. Identificar el archivo de dispositivo correspondiente al dispositivo deseado.
       2. Abrir el archivo de dispositivo utilizando la función **`open()`** u otra llamada similar. Esto crea una conexión entre el proceso y el dispositivo y devuelve un descriptor de archivo.
       3. Realizar operaciones de lectura/escritura utilizando funciones como **`read()`** y **`write()`**, que transfieren datos entre el espacio de usuario y el dispositivo a través del descriptor de archivo.
       4. Si es necesario, realizar operaciones de control adicionales utilizando llamadas al sistema específicas o ioctl para configurar parámetros o enviar comandos al controlador de dispositivo.
       5. Cerrar el archivo de dispositivo utilizando la función **`close()`** para liberar los recursos asociados al descriptor de archivo.

       Es importante tener en cuenta que se pueden requerir permisos adecuados para acceder a los dispositivos, lo que puede implicar privilegios de administrador (root) o la configuración de permisos específicos para los archivos de dispositivo.

    4. Para asociar un módulo que implementa un controlador de dispositivo con el dispositivo correspondiente en Linux, se siguen los siguientes pasos:

       1. Implementar el módulo del controlador de dispositivo: El programador crea un módulo en el kernel de Linux que contiene el código del controlador de dispositivo. Este módulo debe implementar las funciones necesarias para interactuar con el dispositivo, como operaciones de lectura, escritura y control.
       2. Registrar el número mayor del dispositivo: Utilizando la función **`register_chrdev()`** o una función similar, se registra un número mayor único para el dispositivo. Esto establece una asociación entre el número mayor y el módulo del controlador de dispositivo.
       3. Configurar la estructura **`file_operations`**: Dentro del módulo del controlador de dispositivo, se define y configura la estructura **`file_operations`**. Esta estructura contiene los punteros a las funciones que implementan las operaciones del dispositivo, como **`read()`**, **`write()`**, etc.
       4. Asociar el número mayor y la estructura **`file_operations`**: Se establece la asociación entre el número mayor registrado y la estructura **`file_operations`** definida en el módulo del controlador de dispositivo.
       5. Cargar el módulo del controlador de dispositivo: El módulo del controlador de dispositivo se carga en el kernel utilizando el comando **`insmod`** u otro método adecuado.

       Una vez que se completa este proceso, el kernel de Linux tiene conocimiento del número mayor del dispositivo y la estructura **`file_operations`** asociada. Cuando se accede al dispositivo desde el espacio de usuario, el kernel utiliza esta asociación para invocar las funciones apropiadas del módulo del controlador de dispositivo según las operaciones realizadas en el dispositivo, como lectura, escritura o control.

    5. Las funciones **`copy_to_user`** y **`copy_from_user`** son funciones utilizadas en el kernel de Linux para transferir datos entre el espacio de kernel y el espacio de usuario de forma segura. Sus principales propósitos son:

       1. **`copy_to_user`**:
          - Propósito: La función **`copy_to_user`** se utiliza para copiar datos desde el espacio de kernel al espacio de usuario.
          - Funcionalidad: Esta función toma como argumentos un puntero al espacio de usuario donde se copiarán los datos, un puntero al espacio de kernel que contiene los datos a copiar y el tamaño de los datos. Copia los datos desde el espacio de kernel al espacio de usuario de manera segura, manejando las restricciones y protecciones de acceso de memoria.
       2. **`copy_from_user`**:
          - Propósito: La función **`copy_from_user`** se utiliza para copiar datos desde el espacio de usuario al espacio de kernel.
          - Funcionalidad: Esta función toma como argumentos un puntero al espacio de kernel donde se copiarán los datos, un puntero al espacio de usuario que contiene los datos a copiar y el tamaño de los datos. Copia los datos desde el espacio de usuario al espacio de kernel de manera segura, teniendo en cuenta las restricciones y protecciones de acceso de memoria.

       Ambas funciones son importantes para garantizar la seguridad y la integridad de los datos al transferir información entre el espacio de usuario y el espacio de kernel en Linux. Ayudan a prevenir problemas como la lectura o escritura de datos no autorizados o acceder a áreas de memoria prohibidas. Estas funciones garantizan que los datos se copien correctamente y siguiendo las políticas de seguridad del kernel.

2.  ![Untitled](/img/tp2-14.png)

3.  El comando **`mknod`** en Linux se utiliza para crear nodos de dispositivo en el sistema de archivos. Estos nodos de dispositivo son archivos especiales que representan dispositivos en el directorio **`/dev`**. A través de **`mknod`**, se puede crear un nodo de dispositivo para un dispositivo específico, estableciendo el tipo de archivo y los números de mayor y menor.

    Los parámetros del comando **`mknod`** son los siguientes:

    1. Ruta del archivo: Especifica la ubicación y el nombre del archivo de nodo de dispositivo a crear.
    2. Tipo de archivo: Especifica el tipo de archivo que se creará. Puede ser **`b`** para un archivo de bloque (como un disco) o **`c`** para un archivo de caracteres (como una impresora).
    3. Número mayor: Especifica el número mayor del dispositivo. El número mayor identifica el controlador de dispositivo al que está asociado el nodo de dispositivo. Es un número único asignado al controlador de dispositivo.
    4. Número menor: Especifica el número menor del dispositivo. El número menor identifica un dispositivo específico dentro del controlador de dispositivo. Puede haber múltiples dispositivos asociados al mismo controlador, y el número menor los distingue entre sí.

    El número mayor y el número menor son referencias numéricas utilizadas por el kernel de Linux para identificar y asociar el nodo de dispositivo con el controlador y el dispositivo correspondientes. El número mayor indica el controlador de dispositivo, mientras que el número menor identifica el dispositivo dentro del controlador. Estos números son asignados y definidos por el desarrollador del controlador de dispositivo.

    En resumen, el comando **`mknod`** se utiliza para crear nodos de dispositivo en Linux. Los parámetros especifican la ruta del archivo, el tipo de archivo (bloque o caracteres) y los números de mayor y menor. El número mayor identifica el controlador de dispositivo y el número menor identifica un dispositivo específico dentro del controlador.

4.  me tira error!!!

    ![Untitled](/img/tp2-15.png)

5.  Como no puedo crear o modificar el archivo tampoco puedo ejecutar este comando!!!!! ☠️

## Crashing

Estos ejercicios no me salieron, no se si es a propósito o qué 🥺

1. No me deja hacer el make :/

   ![Untitled](/img/tp2-16.png)

2. ![Untitled](/img/tp2-17.png)
