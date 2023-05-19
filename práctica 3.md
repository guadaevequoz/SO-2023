# File Systems

1. Un file system es una abstracción que permite la creación, eliminación, modificación y búsqueda de archivos y su organización en directorios. Es la parte del SO que se encarga del manejo de archivos.
2. Tanto los sistemas de archivos ext (ext2, ext3, ext4) como XFS son sistemas de archivos utilizados comúnmente en sistemas operativos Linux. Las principales diferencias y similitudes entre ellos son:
   - **Journaling**: Tanto ext3 como ext4 son sistemas de archivos con registro (_journaling_), lo que significa que mantienen un registro de las transacciones pendientes antes de escribirlas en el disco, lo que garantiza una mayor integridad de los datos en caso de un cierre inesperado del sistema. Por otro lado, XFS también es un sistema de archivos con registro y utiliza una técnica llamada "journaling por metadatos" para garantizar la consistencia de los metadatos, pero no realiza un registro completo de todas las transacciones como en ext3 y ext4.
   - **Tamaño máximo del sistema de archivos**: ext2 y ext3 tienen un límite de tamaño máximo para el sistema de archivos, que depende del tamaño del bloque y de otros factores. En ext2, el tamaño máximo del sistema de archivos es de 32 terabytes, mientras que en ext3 el límite es de 16 terabytes. Por otro lado, tanto ext4 como XFS tienen límites mucho más altos. Ext4 puede admitir sistemas de archivos de hasta 1 exabyte y XFS tiene un límite teórico de 8 exabytes.
   - **Fragmentación**: La fragmentación es la dispersión de archivos en fragmentos no contiguos en el disco. En este aspecto, ext2 y ext3 pueden sufrir más fragmentación a medida que se utilizan y se eliminan archivos en el sistema de archivos. Ext4 y XFS tienen mejores algoritmos de asignación de espacio en disco, lo que reduce la fragmentación y mantiene un rendimiento más consistente a medida que se añaden y eliminan archivos.
   - **Rendimiento y escalabilidad**: XFS se ha diseñado pensando en el rendimiento y la escalabilidad en sistemas con grandes volúmenes de datos. Proporciona una alta concurrencia y buen rendimiento en escritura y lectura de archivos grandes. Ext4 también ha mejorado su rendimiento en comparación con ext3 y es adecuado para la mayoría de las cargas de trabajo. Sin embargo, para aplicaciones que requieren un alto rendimiento y escalabilidad, XFS es generalmente preferido.
   - **Características avanzadas**: Ext4 y XFS incluyen características avanzadas que no están presentes en ext2 y ext3. Ext4 admite extensiones del sistema de archivos, lo que permite un mayor número de inodos y tamaño máximo de archivo, así como asignación diferida y asignación rápida para mejorar el rendimiento. XFS ofrece instantáneas (snapshots) y gestión avanzada de cuotas de disco, lo que permite crear copias de seguridad en el tiempo y controlar el uso del espacio en disco de manera más granular.
3. Estas características de ext4 (extents, multiblock allocation, delayed allocation y persistent pre-allocation) se han introducido para mejorar el rendimiento, reducir la fragmentación y proporcionar una asignación de bloques más eficiente en comparación con las versiones anteriores de los sistemas de archivos ext.
   - **Extents**: Extents es una característica de ext4 que reemplaza el esquema de asignación tradicional de bloques individuales. En lugar de ello, ext4 utiliza "extents" para asignar bloques contiguos de manera más eficiente. Un extent es un rango de bloques contiguos que se asignan a un archivo. Esto reduce la fragmentación y mejora el rendimiento al reducir la cantidad de operaciones de búsqueda necesarias para acceder a los bloques de un archivo.
   - **Multiblock allocation**: Esta característica se refiere a la capacidad de ext4 para asignar múltiples bloques de manera eficiente en una sola operación. En lugar de asignar un bloque a la vez, ext4 puede asignar varios bloques consecutivos en una sola transacción. Esto mejora el rendimiento al reducir la sobrecarga de operaciones de asignación de bloques y minimizar la fragmentación.
   - **Delayed allocation**: Delayed allocation es una técnica utilizada por ext4 para mejorar el rendimiento al retrasar la asignación real de bloques en el disco hasta que sea necesario. En lugar de escribir inmediatamente los datos en el disco, ext4 retiene los datos en una especie de caché interna y los retrasa hasta que sea necesario realizar la asignación física de bloques. Esto permite una mejor consolidación de datos y reduce la fragmentación al asignar bloques contiguos en lugar de dispersarlos en diferentes ubicaciones del disco.
   - **Persistent pre-allocation**: Esta característica permite que ext4 reserve de manera anticipada espacio en disco para un archivo antes de que se escriban los datos. Cuando se crea un archivo, ext4 puede reservar bloques en disco para el archivo, lo que evita la fragmentación y mejora el rendimiento al tener bloques contiguos disponibles. Esta reserva anticipada de espacio también es útil para aplicaciones que necesitan garantizar un espacio continuo y predecible en disco, como bases de datos.
4. No siempre es necesario tener un sistema de archivos para acceder a un disco o partición. En algunos casos, como discos sin formato o sistemas de archivos desconocidos, es posible acceder a nivel de bajo nivel o mediante herramientas especializadas. Sin embargo, para acceder y manipular los datos de manera convencional, es necesario contar con un sistema de archivos establecido en el disco o partición.
5. El área de **_swap_** en Linux es un espacio en el disco duro utilizado como extensión de la memoria RAM. Almacenando datos no utilizados temporalmente, ayuda a evitar la falta de memoria.

   En Windows, existe una característica similar llamada archivo de paginación. Cumple una función similar al área de swap de Linux, permitiendo que el sistema transfiera datos de la memoria RAM al disco duro cuando se alcanza su capacidad máxima. Esto ayuda a mantener el rendimiento del sistema y evitar bloqueos debido a la falta de memoria.

6. El directorio "lost+found" en Linux se utiliza para almacenar archivos que se recuperan después de un fallo del sistema de archivos. Cuando ocurre algún tipo de corrupción o fallo en el sistema de archivos, el sistema operativo Linux intenta repararlo durante el proceso de arranque y cualquier archivo que no pueda asociarse con un directorio existente se coloca en el directorio "lost+found". Esto permite que los administradores del sistema revisen y recuperen los archivos perdidos o dañados después de una falla.
7. En Linux, el nombre y los metadatos de los archivos se almacenan en la estructura de datos conocida como **_inodo_**. Cada archivo tiene un inodo asociado, que contiene información sobre el nombre del archivo, su tamaño, permisos, fechas de creación y modificación, así como los punteros a los bloques de datos que contienen el contenido del archivo. Los inodos se guardan en una tabla especial en el sistema de archivos y se utilizan para acceder y administrar los archivos en el sistema.
8. Entre como root y ejecute `dumpe2fs /dev/sda1` en la terminal.

   1. El comando **`dumpe2fs`** en Linux muestra información detallada sobre el sistema de archivos. Al ejecutar este comando, se obtiene información como el tamaño del sistema de archivos, el número total de inodos, el número de bloques libres, el tamaño de bloque, el número de bloques reservados, la fecha de creación del sistema de archivos, la versión del sistema de archivos, el número máximo de montajes, el número de grupos de bloques, el tamaño medio de los inodos y otras estadísticas relacionadas con el sistema de archivos. Además, también muestra información sobre el último montaje exitoso, el último chequeo realizado y cualquier error detectado en el sistema de archivos.
   2. El tamaño de bloque del file system es: `4096`.
   3. En total, el sistema contiene `2445984`inodos. En total, hay `2185007` inodos libres.
   4. Existen `9764864` bloques.
   5. Para aumentar la cantidad de inodos en un sistema de archivos en Linux, generalmente es necesario crear un nuevo sistema de archivos con una cantidad mayor de inodos y luego copiar los archivos existentes al nuevo sistema.

      Los pasos resumidos para incrementar la cantidad de inodos en un sistema de archivos son:

      1. Verificar el sistema de archivos existente: Utiliza el comando `df -T` para obtener información sobre el sistema de archivos actual, incluyendo el tipo de sistema de archivos utilizado.
      2. Hacer una copia de seguridad: Antes de realizar cualquier cambio en el sistema de archivos, es recomendable hacer una copia de seguridad de los archivos importantes para evitar pérdida de datos.
      3. Crear un nuevo sistema de archivos: Utiliza una herramienta como `mkfs` para crear un nuevo sistema de archivos con una cantidad mayor de inodos. Puedes especificar la cantidad de inodos utilizando el parámetro adecuado según el tipo de sistema de archivos.
      4. Montar el nuevo sistema de archivos: Monta el nuevo sistema de archivos en un directorio vacío utilizando el comando `mount`.
      5. Copiar los archivos: Copia los archivos desde el sistema de archivos anterior al nuevo sistema de archivos montado utilizando el comando `cp` o alguna otra herramienta de copia.

      Es importante tener en cuenta que este proceso puede ser complejo y potencialmente riesgoso.

9. El sistema de archivos **_procfs_** (también conocido como `/proc`) es un sistema de archivos virtual en Linux que proporciona una interfaz para acceder a información y estadísticas del kernel y procesos en tiempo real. Se utiliza para representar información del sistema en forma de archivos y directorios, donde cada archivo o directorio representa una entidad del sistema, como procesos en ejecución, información del hardware, configuración del sistema y más. Permite a los usuarios y programas acceder a esta información mediante operaciones de lectura en los archivos correspondientes.

   El **_sysfs_** (sistema de archivos `sysfs`) es otro sistema de archivos virtual en Linux que proporciona una vista jerárquica de los dispositivos del sistema y sus controladores. Está diseñado para representar la información y configuración del kernel y los dispositivos en forma de archivos y directorios. Permite a los usuarios y programas obtener información sobre los dispositivos y sus características, así como realizar cambios en la configuración del kernel y los controladores mediante la escritura en los archivos correspondientes.

   En resumen, tanto el procfs como el sysfs son sistemas de archivos virtuales en Linux que proporcionan acceso a información del sistema y configuración del kernel y dispositivos a través de una interfaz de archivos y directorios.

10. Ingrese al directorio `/proc` .
    1. Ejecute `cat /proc/version` y la respuesta fue: `Linux version 5.16.16 (so@so2022)`.
    2. Ejecute `cat /proc/cpuinfo`y dentro de la respuesta, el procesador aparece como: `model name: Intel(R) Core(TM) i5-10600K CPU @ 4.10GHz`.
    3. Ejecute `cat /proc/meminfo`y dentro de la respuesta, la memoria disponible aparece como: `MemAvailable:315160 kB`.
    4. Para ver el mismo resultado que el comando `lsmod` es suficiente con hacer `cat /proc/modules`.
       - `lsmod`:
         ![Untitled](/img/tp3-10-4-1.png)
       - `cat /proc/modules`:
         ![Untitled](/img/tp3-10-4-2.png)
11. El comando `stat` en Linux se utiliza para mostrar información detallada sobre un archivo o directorio específico.
    1. `stat /etc/group`: la última vez que se modificó fue, según la respuesta del comando, `Modificación: 2023-04-29 19:15:11.126046887 -0300` y `Cambio: 2023-04-29 19:15:11.126046887 -0300`.
    2. "**_Cambio_**" se refiere a los cambios en los metadatos del archivo (como los permisos, el propietario, el grupo, etc.), mientras que "**_Modificación_**" se refiere a los cambios en el contenido real del archivo (ya sea añadiendo, eliminando o modificando su contenido).
    3. Ocupa el inodo `1315009` y `8` bloques.
    4. Tomando en cuenta a `etc` como el directorio raíz hice `stat /etc` y el número de inodo es: `1313281`.
    5. En el sistema de archivos ext4, no se almacena directamente la fecha de creación de un archivo. El sistema de archivos ext4 solo registra la fecha de último acceso (access), modificación (modify) y cambio de metadatos (change) en los atributos del archivo. Sin embargo, es posible que existan soluciones o herramientas específicas de terceros que puedan ofrecer información adicional o estimaciones sobre la fecha de creación de archivos en ext4. Estas herramientas pueden basarse en registros o información almacenada en otros lugares del sistema. Sin embargo, dichas soluciones no forman parte del sistema de archivos ext4 por defecto y pueden requerir una configuración o instalación adicional.
12. Un `enlace simbólico` (**symbolic link** o **symlink**) es un tipo de enlace en el sistema de archivos que apunta a otro archivo o directorio mediante su ruta relativa o absoluta. Es esencialmente un acceso directo que redirige a otro archivo o directorio. El enlace simbólico es un archivo independiente con su propia ubicación en el sistema de archivos. Puede apuntar a cualquier archivo o directorio, incluso en sistemas de archivos diferentes o en ubicaciones no accesibles actualmente. Si se elimina el archivo original al que apunta el enlace simbólico, el enlace simbólico queda roto y apunta a una ubicación no válida.

    Por otro lado, un `hard-link` (enlace rígido) es un enlace directo a un archivo o directorio específico. Ambos enlaces, el original y el hard-link, se refieren al mismo objeto en el sistema de archivos. Los hard-links solo pueden apuntar a archivos o directorios en el mismo sistema de archivos. Si se elimina el archivo original, el hard-link aún mantiene una referencia al objeto y sigue siendo accesible. No se puede crear un hard-link para un directorio, solo para archivos.

    En resumen, un enlace simbólico es un acceso directo a otro archivo o directorio, mientras que un hard-link es un enlace directo al mismo objeto en el sistema de archivos. Los enlaces simbólicos son archivos independientes que pueden apuntar a ubicaciones arbitrarias, mientras que los hard-links son referencias directas dentro del mismo sistema de archivos.

13. Al crear un hard-link, no se ocupa un nuevo inodo. Los hard-links comparten el mismo inodo que el archivo original al que están enlazados. En cambio, al crear un enlace simbólico (symbolic link) se crea un nuevo inodo para el archivo simbólico. El inodo del enlace simbólico contiene la ruta o ubicación del archivo o directorio al que apunta.
14. Comandos que ejecuté:

    ![Untitled](/img/tp3-14.png)

    Al crear un enlace simbólico a ese archivo, y luego eliminarlo, el enlace simbólico seguirá existiendo pero apuntará a una ubicación no válida. Se considerará un enlace roto o "link simbólico huérfano". Al intentar acceder al enlace simbólico roto resulta en un error.

    Por otro lado, el hard-link sigue apuntando al mismo archivo en el sistema de archivos y es totalmente accesible. En este caso, el archivo seguiría existiendo en el sistema hasta que se eliminen todos los enlaces (incluido el hard-link) que apunten a él.

15. Ambos archivos, "prueba2.txt" y "pruebahd.txt", comparten el mismo inodo en el sistema de archivos. Esto significa que ambos enlaces apuntan al mismo objeto de archivo en el sistema de archivos, y cualquier cambio realizado en uno de los enlaces se reflejará en el otro. Se puede observar que ambos archivos tienen el mismo tamaño, permisos, propietario y grupo. Esto se debe a que comparten los mismos metadatos y atributos del archivo.

    ![Untitled](/img/tp3-15.png)

16. Tal como pasó en el punto 14, aunque elimine el archivo “prueba2.txt” puedo acceder al hard-link del mismo puesto que cada hard-link se mantienen como entradas separadas con su propio nombre y metadatos. Cuando se elimina un archivo, como "prueba2.txt", se elimina esa entrada específica del sistema de archivos, pero los hard-links que apuntan al mismo inodo del archivo aún existen y siguen siendo válidos.

# RAID

// Para que todo funcione hice el `apt-update` y después el `apt install linux-image-amd64`.

1. RAID (Redundant Array of Independent Disks) es una tecnología que combina múltiples unidades de almacenamiento en un solo grupo lógico para mejorar la confiabilidad, el rendimiento o ambos. Los diferentes niveles de RAID más comunes son:

   1. **RAID 0 (_Striping_)**: Distribuye los datos entre varias unidades en franjas (**_stripes_**) para mejorar el rendimiento. No ofrece redundancia, por lo que si una unidad falla, se pierden todos los datos del grupo.
   2. **RAID 1 (_Mirroring_)**: Duplica los datos en dos o más unidades para brindar redundancia. Si una unidad falla, los datos se pueden recuperar desde la unidad espejo. El rendimiento de lectura puede mejorar, pero el rendimiento de escritura es similar al de una sola unidad. Ocupa mucho espacio.
   3. **RAID 5 (_Striping with Parity_)**: Distribuye los datos en varias unidades al tiempo que almacena información de paridad para recuperar datos en caso de fallo de una unidad. Proporciona un equilibrio entre rendimiento, capacidad y redundancia.
   4. **RAID 6 (_Striping with Double Parity_)**: Similar a RAID 5, pero con dos conjuntos de datos de paridad, lo que brinda mayor capacidad de tolerancia a fallos. Puede recuperarse de la falla simultánea de hasta dos unidades.
   5. **RAID 10 (RAID 1+0)**: Combina la duplicación de datos de RAID 1 con el rendimiento de striping de RAID 0. Los datos se duplican en múltiples unidades y luego se distribuyen en franjas.

   Estos son solo algunos de los niveles de RAID más comunes. Cada nivel tiene sus propias ventajas y consideraciones en términos de rendimiento, capacidad y confiabilidad. La elección del nivel de RAID adecuado depende de los requisitos específicos del sistema, como la tolerancia a fallos, el rendimiento necesario y el costo.

2. ![Untitled](/img/tp3-R-2.png)

3. Creación de las particiones:

   ![Untitled](/img/tp3-R-3-1.png)

   `fdisk -l`:

   ![Untitled](/img/tp3-R-3-2.png)

4. _EN PROCESO_
5. Son las particiones.

6-14 sin hacer

# LVM - Logical Volumen Management

1. LVM, o Logical Volume Manager (Gestor de Volúmenes Lógicos), es una tecnología utilizada en sistemas operativos Linux para administrar el almacenamiento en disco. Proporciona una capa de abstracción entre los discos físicos y los sistemas de archivos, lo que permite una gestión más flexible y dinámica de los volúmenes de almacenamiento.

   La principal ventaja de LVM sobre el particionado tradicional de Linux es su capacidad para crear y administrar volúmenes lógicos que pueden abarcar varios discos físicos o particiones. Esto significa que se pueden combinar varios discos en un único volumen lógico, lo que proporciona una mayor flexibilidad en términos de gestión del espacio en disco.

   Algunas de las ventajas específicas de LVM son:

   1. Administración dinámica del espacio: Con LVM, es posible redimensionar volúmenes lógicos en tiempo real sin tener que desmontar los sistemas de archivos o interrumpir el funcionamiento del sistema. Esto permite ajustar el tamaño de los volúmenes en función de las necesidades cambiantes de almacenamiento sin interrupciones.
   2. Administración de discos en caliente: LVM permite agregar o quitar discos físicos en caliente, es decir, mientras el sistema está en funcionamiento. Esto facilita la expansión o modificación de los volúmenes lógicos sin tener que detener o reiniciar el sistema.
   3. Gestión de snapshots: LVM ofrece la capacidad de crear instantáneas (snapshots) de los volúmenes lógicos. Los snapshots son copias virtuales de un volumen en un punto determinado en el tiempo, lo que permite realizar copias de seguridad o realizar pruebas sin afectar los datos originales.
   4. Gestión de volúmenes en espejo y en RAID: LVM permite la creación de volúmenes lógicos en espejo y la configuración de RAID a nivel de volumen. Esto proporciona redundancia de datos y mayor tolerancia a fallos, lo que aumenta la disponibilidad y confiabilidad del almacenamiento.

   En resumen, LVM ofrece una mayor flexibilidad y facilidad de gestión en comparación con el particionado tradicional de Linux. Permite la administración dinámica del espacio en disco, la gestión de discos en caliente, la creación de snapshots y la configuración de volúmenes en espejo y en RAID, lo que lo convierte en una opción atractiva para aquellos que requieren una administración avanzada del almacenamiento en Linux.

2. Los "**_snapshots_**" en LVM son copias virtuales de un volumen lógico en un momento específico. Se crean capturando el estado actual del volumen y asignando espacio adicional para cambios futuros. A medida que se realizan modificaciones en el volumen original, estos cambios se registran en el snapshot, mientras que el volumen original se mantiene sin cambios. Los snapshots permiten realizar copias de seguridad, pruebas y revertir cambios sin afectar el volumen original.

3-30 sin hacer

# BTRFS & ZFS

1.  1. Las siglas `BTRFS` significan "B-Tree File System". El término "B-Tree" se refiere a una estructura de datos utilizada para organizar y gestionar eficientemente la información en un sistema de archivos.

       Las siglas `ZFS` significan "Zettabyte File System". Un zettabyte es una unidad de medida de almacenamiento que equivale a 1 billón de gigabytes.

    2. BTRFS fue creado por Oracle Corporation y lanzado en 2007. Desde entonces, ha sido desarrollado y mantenido por la comunidad de código abierto. En cuanto a su licenciamiento, BTRFS se distribuye bajo la Licencia Pública General GNU (GNU General Public License, GPL), que es una licencia de software libre y de código abierto.

       ZFS fue creado originalmente por Sun Microsystems (ahora propiedad de Oracle Corporation) y lanzado en 2005. Actualmente, ZFS es mantenido y desarrollado por la comunidad de código abierto. En cuanto a su licenciamiento, ZFS utiliza la Licencia Pública Común de Sun (Sun Common Development and Distribution License, CDDL), que es una licencia de código abierto.

    3. Algunas características importantes de BTRFS son:

       - Administración avanzada de volúmenes y subvolúmenes: BTRFS permite la gestión flexible de volúmenes y subvolúmenes, lo que facilita la organización y administración del almacenamiento.
       - Copias instantáneas (snapshots): BTRFS ofrece la capacidad de crear instantáneas del sistema de archivos en tiempo real, lo que permite realizar copias de seguridad y revertir cambios.
       - RAID y tolerancia a fallos: BTRFS soporta diferentes niveles de RAID para la redundancia de datos y la protección contra fallos de disco.
       - Compresión y deduplicación: BTRFS incluye opciones de compresión y deduplicación de datos, lo que puede ayudar a ahorrar espacio en disco.
       - Integridad de datos: BTRFS utiliza checksums para verificar la integridad de los datos almacenados, detectando y corrigiendo errores cuando sea posible.

       Algunas características importantes de ZFS son:

       - Almacenamiento y gestión de volúmenes: ZFS proporciona un sistema de archivos combinado con la gestión de volúmenes, lo que permite un control integral del almacenamiento.
       - Integridad de datos y detección de errores: ZFS utiliza checksums para detectar y corregir errores de datos en tiempo real, brindando una mayor fiabilidad y protección contra la corrupción de datos.
       - RAID y tolerancia a fallos: ZFS incluye una variedad de niveles de RAID y ofrece opciones avanzadas de tolerancia a fallos para proteger los datos contra fallos de hardware.
       - Copias instantáneas (snapshots): ZFS permite la creación rápida de instantáneas para realizar copias de seguridad, clonar sistemas de archivos y revertir cambios.
       - Compresión y deduplicación: ZFS ofrece opciones de compresión de datos y deduplicación, lo que puede ahorrar espacio en disco al eliminar duplicados y comprimir los datos almacenados.

    4. Copy-on-Write (COW) es una técnica utilizada por sistemas de archivos como BTRFS y ZFS. En la técnica Copy-on-Write, cuando se realiza una escritura en un archivo, en lugar de modificar directamente los datos existentes, se realiza una copia de los datos en una ubicación diferente. De esta manera, los datos originales permanecen intactos y se crea una copia actualizada. Esto garantiza la integridad de los datos y permite la creación de instantáneas (snapshots) eficientes, ya que solo se copian los bloques de datos que cambian, en lugar de todo el sistema de archivos. La técnica Copy-on-Write es fundamental para la implementación de las características de instantáneas y la integridad de datos en BTRFS y ZFS.

       La técnica Copy-on-Write (COW) también se utiliza en ZFS de manera similar a BTRFS. Cuando se realiza una escritura en un archivo, ZFS crea una copia actualizada de los datos en una ubicación diferente en lugar de modificar directamente los datos originales. Esto asegura la integridad de los datos y permite la creación eficiente de instantáneas y la administración del almacenamiento.

2-18 sin hacer
