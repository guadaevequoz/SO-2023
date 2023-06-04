# Práctica 4

# cgroups & namespaces

## Conceptos teóricos

1. La virtualización se refiere al proceso de crear una versión virtual o simulada de un recurso tecnológico, como un sistema operativo, un servidor, una red o un dispositivo de almacenamiento. En lugar de utilizar físicamente un recurso, la virtualización permite que múltiples instancias virtuales se ejecuten en un solo hardware físico, lo que brinda flexibilidad, eficiencia y ahorro de costos.

   La primera implementación de virtualización se remonta a la década de 1960, cuando IBM desarrolló el concepto de máquinas virtuales (VM) en sus sistemas mainframe. IBM introdujo el sistema operativo VM/370 en 1972 para su uso en computadoras de la serie System/370. VM/370 permitía a los usuarios ejecutar múltiples sistemas operativos, como OS/360, como máquinas virtuales independientes.

   VM/370 fue un hito importante en la historia de la virtualización, ya que permitió a los usuarios aprovechar al máximo el hardware mainframe de IBM al ejecutar múltiples sistemas operativos en un solo sistema físico. Esto proporcionó una mayor utilización de recursos y una mayor flexibilidad para las aplicaciones y los usuarios.

   Desde entonces, la virtualización ha evolucionado significativamente y se ha extendido a diferentes plataformas, incluidos los servidores x86, donde tecnologías como VMware y Xen han ganado popularidad. Actualmente, la virtualización es una práctica común en la industria de la tecnología, utilizada en centros de datos, entornos de nube, virtualización de escritorio, redes y más.

2. La diferencia entre virtualización y emulación radica en cómo se crea y utiliza el entorno virtual.

   La virtualización se refiere a la creación de una versión virtual de un recurso tecnológico, como un sistema operativo, un servidor o una red. En la virtualización, se utiliza un software llamado hipervisor o monitor de máquina virtual para crear y administrar las máquinas virtuales. Estas máquinas virtuales son instancias aisladas que se ejecutan en un único hardware físico y comparten los recursos subyacentes. La virtualización permite que múltiples sistemas operativos o entornos se ejecuten simultáneamente en un mismo servidor, lo que mejora la eficiencia y la utilización de recursos.

   Por otro lado, la emulación implica recrear completamente el comportamiento y la funcionalidad de un sistema o dispositivo específico en otro entorno. En la emulación, se utiliza software o hardware especializado para imitar el hardware y el software de la plataforma que se está emulando. El objetivo de la emulación es permitir que un sistema o programa diseñado para una plataforma específica se ejecute en otra plataforma diferente. La emulación es útil cuando se necesita ejecutar aplicaciones o sistemas operativos antiguos en hardware moderno o cuando se desea probar software en diferentes plataformas sin tener acceso directo al hardware original.

   En resumen, la virtualización se basa en crear instancias virtuales a partir de un hardware físico para compartir recursos y ejecutar múltiples sistemas operativos, mientras que la emulación implica recrear un entorno completo para imitar el comportamiento de un sistema o dispositivo específico en otro hardware o software.

3. 1. Un hypervisor, también conocido como monitor de máquina virtual (VMM), es un software o firmware que se encarga de crear y administrar las máquinas virtuales (VM) en entornos de virtualización. Actúa como una capa de abstracción entre el hardware físico del sistema y las VM, permitiendo que múltiples sistemas operativos o entornos se ejecuten de manera aislada y simultánea en un único servidor.

      El hypervisor se encarga de asignar y administrar los recursos físicos del hardware, como CPU, memoria, almacenamiento y dispositivos de entrada/salida, entre las VM. También garantiza la seguridad y el aislamiento de las VM, evitando que una VM afecte el rendimiento o la estabilidad de otras. Además, proporciona funcionalidades como la migración en vivo de VM, el escalado dinámico de recursos y la gestión centralizada de las VM.

   2. Los hypervisores ofrecen una serie de beneficios en entornos de virtualización:

      1. Consolidación de servidores: Permite ejecutar múltiples sistemas operativos y aplicaciones en un único servidor físico, lo que reduce los costos de hardware, energía y espacio físico.
      2. Aislamiento y seguridad: Cada VM se ejecuta de forma aislada, lo que garantiza que los fallos o problemas en una VM no afecten a las demás. Además, los hypervisors suelen proporcionar mecanismos de seguridad, como firewalls virtuales y políticas de acceso, para proteger las VM.
      3. Flexibilidad y escalabilidad: Los hypervisors permiten asignar y ajustar los recursos de manera dinámica a medida que las necesidades cambian. Esto facilita la escalabilidad de los entornos virtuales, permitiendo agregar o quitar recursos según sea necesario.
      4. Alta disponibilidad y recuperación ante desastres: Los hypervisors ofrecen funcionalidades como la migración en vivo de VM, que permite mover una VM de un servidor físico a otro sin interrupciones. Esto facilita la implementación de soluciones de alta disponibilidad y recuperación ante desastres.

      Los hypervisors se clasifican en dos tipos principales:

      1. Hypervisor de tipo 1 o "nativo": También conocido como "bare-metal", se ejecuta directamente sobre el hardware físico sin requerir un sistema operativo anfitrión adicional. Proporciona un rendimiento y una eficiencia superiores y es comúnmente utilizado en entornos de servidores. Ejemplos de hypervisors de tipo 1 son VMware ESXi, Microsoft Hyper-V y Citrix XenServer.
      2. Hypervisor de tipo 2 o "hosted": Se ejecuta como una aplicación en un sistema operativo anfitrión y gestiona las VM sobre ese sistema operativo. Este tipo de hypervisor es más común en entornos de escritorio y desarrollo. Ejemplos de hypervisors de tipo 2 son VMware Workstation, Oracle VirtualBox y Microsoft Virtual PC.

      Ambos tipos de hypervisors tienen sus usos y características específicas, y la elección depende de los requisitos y el entorno de virtualización en particular.

4. La full virtualization (virtualización completa) es una técnica de virtualización en la que el hypervisor permite que las máquinas virtuales (VM) se ejecuten en un entorno aislado y emulen un conjunto completo de recursos de hardware virtualizados. En la full virtualization, las VM no tienen conocimiento de que se están ejecutando en un entorno virtual y pueden ejecutar cualquier sistema operativo compatible con el hardware subyacente. El hypervisor se encarga de traducir las instrucciones de la VM para que sean compatibles con el hardware físico del servidor. Ejemplos de hypervisors de full virtualization incluyen VMware ESXi y Microsoft Hyper-V.

   Por otro lado, la virtualización asistida por hardware, también conocida como virtualización nativa o virtualización con extensiones de hardware, es una tecnología que utiliza características específicas del hardware subyacente para mejorar el rendimiento y la eficiencia de las máquinas virtuales. Los procesadores modernos suelen incluir extensiones de virtualización, como Intel VT (Virtualization Technology) o AMD-V, que ofrecen instrucciones adicionales y funcionalidades diseñadas para acelerar las operaciones de virtualización. Estas extensiones permiten que el hypervisor y las VM interactúen directamente con el hardware, evitando la necesidad de traducciones o emulaciones complejas. La virtualización asistida por hardware mejora el rendimiento de las VM y reduce la sobrecarga de la virtualización. La mayoría de los hypervisors modernos aprovechan estas extensiones para proporcionar una mayor eficiencia en entornos virtualizados.

5. La técnica de traducción binaria (binary translation) y la técnica de atrapar y emular (trap-and-emulate) son dos enfoques utilizados en la virtualización para ejecutar instrucciones de un sistema operativo invitado en un entorno virtualizado.

   La traducción binaria implica la traducción dinámica de las instrucciones del sistema operativo invitado a un conjunto de instrucciones compatibles con el hardware subyacente. En este enfoque, el hypervisor examina las instrucciones de la VM mientras se ejecutan y las traduce a un formato que el hardware físico puede entender. Esto permite que las instrucciones de la VM se ejecuten directamente en el hardware sin requerir cambios en el sistema operativo invitado. La traducción binaria puede introducir una pequeña sobrecarga debido al proceso de traducción, pero permite una ejecución eficiente de las VM.

   Por otro lado, el enfoque de atrapar y emular consiste en atrapar ciertas instrucciones o eventos generados por el sistema operativo invitado y emularlos en el hypervisor. Cuando una instrucción específica o evento no se puede ejecutar directamente en el hardware subyacente, el hypervisor "atrapa" la ejecución y la emula en su propio entorno. Por ejemplo, si una instrucción privilegiada no está permitida en la VM, el hypervisor la atrapa y emula su comportamiento. Este enfoque implica una mayor sobrecarga, ya que cada instrucción o evento atrapado requiere la intervención del hypervisor para su emulación.

   Ambos enfoques tienen sus ventajas y desventajas. La traducción binaria permite una ejecución más eficiente y directa de las instrucciones de la VM en el hardware físico, pero puede requerir un proceso de traducción inicial. Por otro lado, el enfoque de atrapar y emular proporciona una mayor flexibilidad y control, pero puede generar una mayor sobrecarga debido a la emulación de las instrucciones atrapadas.

   Los hypervisors modernos pueden utilizar una combinación de ambas técnicas, según las necesidades y las características del hardware y del sistema operativo invitado, para lograr un equilibrio entre el rendimiento y la flexibilidad en la virtualización.

6. 1. La paravirtualización es una técnica de virtualización en la cual el sistema operativo invitado se modifica para ser consciente de que se está ejecutando en un entorno virtualizado. A diferencia de la virtualización completa (full virtualization), donde el sistema operativo invitado se ejecuta sin modificaciones, en la paravirtualización, el sistema operativo invitado se adapta para utilizar interfaces de comunicación específicas proporcionadas por el hypervisor. Estas interfaces permiten una comunicación directa y eficiente con el hypervisor y el hardware subyacente, evitando la necesidad de técnicas de traducción binaria o atrapar y emular.
   2. Un ejemplo de sistema que implementa la paravirtualización es Xen. Xen es un hypervisor de tipo 1 de código abierto que se utiliza ampliamente en entornos de virtualización. Xen permite la paravirtualización de sistemas operativos invitados, lo que implica que los sistemas operativos invitados deben modificarse para utilizar las interfaces de Xen. Algunos sistemas operativos, como Linux, han sido adaptados para admitir la paravirtualización en Xen, lo que proporciona un rendimiento y una eficiencia mejorados en comparación con la virtualización completa.
   3. La paravirtualización ofrece varios beneficios en comparación con otros modos de virtualización:
      1. Mejor rendimiento: Al ser consciente del entorno virtualizado, el sistema operativo invitado puede utilizar interfaces optimizadas proporcionadas por el hypervisor, lo que resulta en una mejor eficiencia y rendimiento en comparación con la traducción binaria o la emulación completa.
      2. Menor sobrecarga: Al evitar la necesidad de técnicas de traducción o emulación complejas, la paravirtualización reduce la sobrecarga en la comunicación entre el sistema operativo invitado y el hypervisor, lo que mejora el rendimiento general del sistema.
      3. Mayor flexibilidad: Al modificar el sistema operativo invitado, la paravirtualización permite una mayor flexibilidad y control sobre las operaciones y recursos del sistema virtualizado. Esto facilita la implementación de características avanzadas, como la migración en vivo y el escalado dinámico de recursos.
      4. Mayor compatibilidad: Dado que los sistemas operativos invitados se adaptan específicamente para utilizar las interfaces de paravirtualización del hypervisor, la paravirtualización puede ser compatible con una amplia gama de sistemas operativos, incluso aquellos que no son compatibles con la virtualización completa.
7. 1. Los containers son una forma de virtualización a nivel de sistema operativo que permite la ejecución de aplicaciones y sus dependencias en un entorno aislado y ligero. Un contenedor encapsula una aplicación con todas las bibliotecas, archivos y configuraciones necesarios, lo que facilita su implementación y ejecución en diferentes entornos sin preocuparse por las diferencias de configuración y dependencias.
   2. Los containers no dependen directamente del hardware subyacente. Utilizan características del kernel del sistema operativo, como los espacios de nombres (namespaces) y los grupos de control (cgroups), para crear un entorno aislado y seguro para la ejecución de aplicaciones. Esto significa que los containers pueden ejecutarse en diferentes sistemas operativos y arquitecturas de hardware, siempre y cuando el kernel del sistema operativo sea compatible.
   3. Lo que diferencia a los containers de otras tecnologías de virtualización es su enfoque ligero y eficiente. A diferencia de las máquinas virtuales, los containers comparten el mismo kernel del sistema operativo subyacente, lo que elimina la sobrecarga de ejecutar múltiples sistemas operativos completos. Los containers son rápidos de iniciar, tienen un menor consumo de recursos y permiten una mayor densidad de aplicaciones en un mismo servidor físico. Además, los containers son altamente portables y ofrecen una gran flexibilidad para la implementación y gestión de aplicaciones.
   4. Para poder implementar containers, se requieren ciertas funcionalidades clave:
      1. Espacios de nombres (namespaces): Permiten que los containers tengan una vista aislada y virtualizada de los recursos del sistema, como el sistema de archivos, la red, los procesos y los identificadores de usuarios.
      2. Grupos de control (cgroups): Permiten limitar y administrar los recursos asignados a los containers, como la CPU, la memoria, el ancho de banda de red y el almacenamiento, garantizando un uso eficiente de los recursos del sistema.
      3. Sistema de archivos en capas (filesystem layering): Permite que los containers compartan y reutilicen capas de sistemas de archivos base, lo que reduce el espacio de almacenamiento y el tiempo de implementación de nuevos containers.
      4. Imágenes de contenedor: Son plantillas o instantáneas que contienen todo el entorno necesario para ejecutar una aplicación en un container, incluyendo el sistema operativo, las bibliotecas y las dependencias. Las imágenes de contenedor se utilizan para crear y replicar containers.
      5. Orquestación de contenedores: Para implementar y gestionar un conjunto de containers, se utilizan herramientas de orquestación, como Docker Swarm, Kubernetes y Mesos, que automatizan tareas como el escalado, la distribución de carga y la administración de la infraestructura subyacente.

## chroot, Control Groups y Namespaces

### Chroot

1. El comando `chroot` es una herramienta utilizada en sistemas Unix y Unix-like que cambia el directorio raíz (root) para un proceso y sus procesos secundarios. La finalidad principal del comando chroot es crear un entorno aislado y restringido dentro de un sistema operativo, limitando el acceso del proceso a solo los archivos y directorios presentes dentro del nuevo directorio raíz.

   Al ejecutar el comando chroot, se especifica el directorio que se convertirá en el nuevo directorio raíz. A partir de ese punto, el proceso y todos los procesos secundarios que se ejecuten dentro de él verán ese directorio como la raíz del sistema de archivos. Esto significa que el proceso tendrá acceso solo a los archivos y subdirectorios que se encuentren dentro de ese nuevo entorno aislado, sin poder ver ni acceder a los archivos y directorios fuera de él.

   La finalidad del comando chroot es proporcionar un entorno seguro para la ejecución de aplicaciones o tareas específicas que requieren un nivel de aislamiento del resto del sistema. Esto puede ser útil en situaciones como pruebas de software, ejecución de servicios con privilegios mínimos, recuperación de sistemas dañados o configuración de entornos de desarrollo.

   En resumen, el comando chroot se utiliza para cambiar el directorio raíz de un proceso, creando así un entorno aislado y restringido donde el proceso y sus subprocesos solo tienen acceso a los archivos y directorios dentro de ese entorno. Esto proporciona una mayor seguridad y control al ejecutar aplicaciones o tareas específicas en sistemas Unix y Unix-like.

2. Al ejecutar `chroot /root/sobash` me tira el error: `chroot: failed to run command ‘/bin/bash’: No such file or directory`. Este error ocurre ya que no se configuró adecuadamente el comando chroot.
3. A la biblioteca "linux-vdso.so.1" no es necesaria copiarla manualmente. Esta biblioteca es una biblioteca virtual proporcionada por el kernel de Linux y es cargada automáticamente por el sistema operativo en tiempo de ejecución. No es un archivo físico en el sistema de archivos, por lo que no es necesario copiarlo manualmente.

   ```bash
   root@so2022:~# mkdir bin
   root@so2022:~# mkdir lib
   root@so2022:~# cp /bin/bash /root/bin
   root@so2022:~# cp /lib/x86_64-linux-gnu/libtinfo.so.6 /root/sobash/lib/x86_64-linux-gnu
   root@so2022:~# cp /lib/x86_64-linux-gnu/libdl.so.2 /root/sobash/lib/x86_64-linux-gnu
   root@so2022:~# cp /lib/x86_64-linux-gnu/libc.so.6 /root/sobash/lib/x86_64-linux-gnu
   root@so2022:~# cp /lib64/ld-linux-x86-64.so.2 /root/sobash/lib64
   root@so2022:~# ls sobash
   bin  lib  lib64
   root@so2022:~/sobash# chroot /root/sobash
   bash-5.0#
   ```

4. `ls` no funciona, `cd` y `echo` sí.
5. El comando `pwd` me muestra el directorio donde estoy, en este caso `/`. Esto es debido a que toma como directorio raíz el directorio donde ejecute el comando, creando una especie de “celda”.
6. ✅

### Control Groups

1. Los **_cgroups_** (grupos de control) están montados en el sistema de archivos virtual en Linux. La ubicación común es en el directorio "`/sys/fs/cgroup`". Dentro de este directorio, se encuentran diferentes controladores de cgroups que representan diferentes recursos del sistema, como CPU, memoria, dispositivos, redes, etc.

   En cuanto a las versiones disponibles de cgroups, existen tres versiones principales:

   1. cgroups v1: Es la versión original de cgroups y ha sido ampliamente utilizada en sistemas Linux. En esta versión, los controladores de cgroups se montan en el directorio "/sys/fs/cgroup" y se utilizan archivos de configuración para establecer los límites y las políticas de recursos.
   2. cgroups v2: Es la versión más reciente y recomendada de cgroups. En esta versión, los controladores de cgroups se montan en el directorio "/sys/fs/cgroup/unified". cgroups v2 introduce un enfoque más unificado y simplificado, donde se utiliza un solo controlador llamado "unified" que reemplaza a varios controladores individuales en cgroups v1. También ofrece una mejor funcionalidad y flexibilidad en comparación con la versión anterior.
   3. Hybrid mode (modo híbrido): Algunas distribuciones de Linux, como Ubuntu, ofrecen un modo híbrido que permite utilizar tanto cgroups v1 como v2 simultáneamente. En este modo, los controladores de cgroups v1 se montan en "/sys/fs/cgroup" y los de cgroups v2 se montan en "/sys/fs/cgroup/unified".

2. Sí, en cgroups v2 existen varios controladores disponibles que permiten gestionar y limitar diferentes recursos del sistema. Para determinar qué controladores están disponibles en cgroups v2, se puede consultar el archivo "cgroup.controllers" dentro del directorio "/sys/fs/cgroup/unified".

   El archivo "cgroup.controllers" muestra una lista de controladores separados por espacios en blanco. Cada controlador representa un recurso específico que puede ser gestionado mediante cgroups v2. Algunos ejemplos de controladores comunes incluyen:

   - cpu: Controlador para limitar el uso de la CPU por los procesos en un grupo de control.
   - memory: Controlador para establecer límites de memoria para los procesos en un grupo de control.
   - io: Controlador para regular el acceso a dispositivos de E/S.
   - pids: Controlador para limitar el número máximo de procesos en un grupo de control.
   - net_cls: Controlador para etiquetar paquetes de red generados por procesos en un grupo de control específico.

   Estos son solo algunos ejemplos de controladores disponibles en cgroups v2. La lista de controladores puede variar según la configuración y el kernel de Linux utilizado.

   En resumen, para determinar qué controladores están disponibles en cgroups v2, se puede consultar el archivo "cgroup.controllers" dentro del directorio "/sys/fs/cgroup/unified". Este archivo mostrará una lista de controladores separados por espacios en blanco que representan los recursos que pueden ser gestionados mediante cgroups v2.

3. Si se elimina un controlador de cgroups v1, puede tener diferentes consecuencias según el entorno y las configuraciones específicas del sistema. Aquí hay algunos posibles efectos que podrían ocurrir:

   1. Pérdida de funcionalidad: Si un controlador de cgroups v1 se elimina y se estaba utilizando para gestionar y limitar los recursos de los grupos de control (cgroups), se perderá la capacidad de controlar y limitar esos recursos. Esto puede llevar a un uso descontrolado de recursos como CPU, memoria, E/S, etc.
   2. Errores y fallos del sistema: La eliminación de un controlador de cgroups v1 puede causar errores y fallos en los procesos y aplicaciones que dependen de la gestión de recursos proporcionada por ese controlador. Los programas que estaban ajustados a los límites y restricciones de ese controlador podrían comportarse de manera inesperada o incluso terminar abruptamente.
   3. Impacto en otras características o servicios: Algunos controladores de cgroups v1 están interrelacionados y tienen dependencias con otros componentes del sistema. La eliminación de un controlador podría afectar el funcionamiento de otros controladores, servicios o características que dependen de él, lo que podría causar un mal funcionamiento general del sistema.

   Yo ejecute el `umount /sys/fs/cgroup/rdma` y no pasó nada.

4. ```bash
   root@so2022:~# mkdir /sys/fs/cgroup/cpu/cpualta
   root@so2022:~# mkdir /sys/fs/cgroup/cpu/cpubaja
   ```

5. Esta usando un cgroup versión 1 ya que manda a hacer las carpetas en `/sys/fs/cgroup/cpu` y no en `/sys/fs/cgroup/unified/cpu`.
6. ✅
7. ✅ so es termalta y so2 es termbaja
8. a
   - so: `[1] 2729`
   - so2: `[1] 2730`
9. Ambos van variando entre los porcentajes 49% y 50%.
10. No puedo por los permisos.
    - so: `echo 2729 > /sys/fs/cgroup/cpu/cpualta/cgroup.procs`
    - so2: `echo 2730 > /sys/fs/cgroup/cpu/cpubaja/cgroup.procs`
11. Desde otra terminal podemos ver que ahora el proceso `2729` ocupa 70% y el proceso `2730` ocupa 30% aproximadamente.
12. ```bash
    root@so2022:~# jobs
    [1]+  Ejecutando              taskset -c 0 md5sum /dev/urandom &
    root@so2022:~# kill %1
    root@so2022:~# jobs
    [1]+  Terminado               taskset -c 0 md5sum /dev/urandom
    ```

13. Ídem lo de arriba pero en la otra terminal.
14. ✅
15. Literalmente lo mismo!!! termalta 70% y termbaja 30%.
16. termalta sigue ocupando 670% pero en termbaja cada proceso ocupa aproximadamente 15%, sumando entre ambos 30%.

### Namespaces

1. Los namespaces son un concepto clave en Linux que se utiliza para proporcionar aislamiento y separación entre procesos y recursos del sistema. Un namespace es un mecanismo que permite que un conjunto de procesos vea un conjunto limitado y aislado de recursos del sistema, como procesos, sistemas de archivos, redes y otros recursos, como si fueran los únicos procesos en ejecución en el sistema.

   Cada namespace proporciona un espacio de nombres independiente para los recursos asociados, lo que significa que los procesos que se ejecutan en diferentes namespaces pueden tener vistas y accesos diferentes a los recursos del sistema. Esto permite que los procesos se ejecuten en entornos aislados y no vean o interactúen con los recursos y procesos fuera de su namespace.

2. Los namespaces se utilizan para varios propósitos, como:

   - Namespace de proceso (PID namespace): Proporciona aislamiento de procesos, de modo que cada proceso ve su propio espacio de nombres de ID de proceso. Esto significa que los procesos pueden tener su propio conjunto de ID de proceso, evitando colisiones de ID y proporcionando un aislamiento efectivo.
   - Namespace de sistema de archivos (Mount namespace): Permite que los procesos tengan su propio sistema de archivos raíz, lo que proporciona aislamiento de los sistemas de archivos del host. Los procesos pueden montar sistemas de archivos diferentes y tener su propia vista aislada del sistema de archivos.

   Estos son solo algunos ejemplos de los namespaces disponibles en Linux. Cada namespace proporciona aislamiento y separación entre los procesos y recursos asociados, lo que permite la creación de entornos aislados y seguros para diferentes aplicaciones y casos de uso. Los namespaces son fundamentales para la virtualización, la contenerización y otras tecnologías de aislamiento en Linux.

3. ```bash
   so@so2022:~$ lsns
           NS TYPE   NPROCS   PID USER COMMAND
   4026531835 cgroup     65   926 so   /lib/systemd/systemd --user
   4026531836 pid        65   926 so   /lib/systemd/systemd --user
   4026531837 user       65   926 so   /lib/systemd/systemd --user
   4026531838 uts        65   926 so   /lib/systemd/systemd --user
   4026531839 ipc        65   926 so   /lib/systemd/systemd --user
   4026531840 mnt        65   926 so   /lib/systemd/systemd --user
   4026531992 net        65   926 so   /lib/systemd/systemd --user
   ```

4. El proceso **`cron`** no crea sus propios namespaces de forma predeterminada. El cron es un demonio del sistema que se ejecuta en el namespace del sistema principal y no tiene namespaces específicos asignados.

   Por lo tanto, en el caso del proceso **`cron`**, los namespaces net, ipc y uts serían los mismos que los del sistema principal. Estos namespaces son compartidos por todos los procesos en el sistema, incluido el proceso **`cron`**.

   En comparación con el punto anterior, donde se mencionaban los namespaces activados en el sistema, los namespaces net, ipc y uts para el proceso **`cron`** serían los mismos que los namespaces net, ipc y uts del sistema principal, ya que el proceso **`cron`** se ejecuta en ese mismo entorno y no crea namespaces adicionales.

5. a

   1. `unshare --uts sh`
   2. ```bash
      # hostname
      so2022
      ```

   3. Aparece la línea `4026532233 uts         2  2598 root   sh` así que supongo que aparece el nuevo hostname.

      ```bash
      # lsns
              NS TYPE   NPROCS   PID USER   COMMAND
      4026531835 cgroup    167     1 root   /sbin/init
      4026531836 pid       167     1 root   /sbin/init
      4026531837 user      167     1 root   /sbin/init
      4026531838 uts       165     1 root   /sbin/init
      4026531839 ipc       167     1 root   /sbin/init
      4026531840 mnt       159     1 root   /sbin/init
      4026531860 mnt         1    14 root   kdevtmpfs
      4026531992 net       166     1 root   /sbin/init
      4026532158 mnt         1   255 root   /lib/systemd/systemd-udevd
      4026532231 mnt         1   479 root   /usr/sbin/ModemManager --filter-policy=strict
      4026532232 mnt         1   501 root   /usr/sbin/NetworkManager --no-daemon
      4026532233 uts         2  2598 root   sh
      4026532296 net         1   754 rtkit  /usr/lib/rtkit/rtkit-daemon
      4026532347 mnt         1   754 rtkit  /usr/lib/rtkit/rtkit-daemon
      4026532401 mnt         1   763 root   /usr/lib/upower/upowerd
      4026532403 mnt         1   870 colord /usr/lib/colord/colord
      4026532404 mnt         1  1303 root   /usr/lib/fwupd/fwupd
      ```

   4. ```bash
      # hostname so2023
      # hostname
      so2023
      ```

   5. ```bash
      so@so2022:~$ hostname
      so2022
      ```

   6. Después de salir, el nombre del host anfitrión vuelve a ser el mismo que en el sistema principal, ya que el nuevo namespace UTS se ha abandonado y los cambios realizados en él ya no se aplican.

6. 1. `unshare --ipc sh`
   2. En el namespace el PID es `2681` y en el host anfitrión es `2609`.
   3. `unshare --pid --fork --mount-proc`
   4. - Anfitrión:
        ```bash
        so@so2022:~$ ps
          PID TTY          TIME CMD
         2609 pts/5    00:00:00 bash
         2694 pts/5    00:00:00 ps
        ```
      - Namespace:
        ```bash
        # ps
          PID TTY          TIME CMD
            1 pts/4    00:00:00 sh
            2 pts/4    00:00:00 ps
        ```
   5. `exit`

---

# Docker

1. Docker es una plataforma de contenedores que permite crear, distribuir y ejecutar aplicaciones de manera eficiente y aislada. Se basa en la tecnología de contenedores de Linux y proporciona una capa adicional de abstracción y herramientas para simplificar el desarrollo, despliegue y gestión de aplicaciones.

   Beneficios de los contenedores en Docker:

   1. Portabilidad y compatibilidad: Los contenedores Docker ofrecen una forma consistente de empaquetar una aplicación junto con todas sus dependencias y configuraciones en un único objeto autónomo. Esto facilita la portabilidad de la aplicación, ya que se puede ejecutar en cualquier entorno que tenga Docker instalado, independientemente del sistema operativo o la infraestructura subyacente. Además, los contenedores proporcionan una mayor compatibilidad entre diferentes sistemas, evitando problemas de dependencias y conflictos.
   2. Aislamiento y eficiencia: Los contenedores proporcionan un alto nivel de aislamiento entre las aplicaciones y su entorno. Cada contenedor tiene su propio espacio de nombres, lo que significa que no pueden interferir entre sí ni con el sistema host. Esto permite ejecutar múltiples aplicaciones de forma segura y aislada en el mismo host sin que interactúen o afecten negativamente a otras aplicaciones. Además, los contenedores son ligeros en términos de recursos y se pueden iniciar y detener rápidamente, lo que los hace eficientes en cuanto a consumo de memoria y tiempo de arranque.

   Otros beneficios adicionales de los contenedores Docker incluyen la escalabilidad, la gestión simplificada de las dependencias, la automatización del despliegue y la facilidad para compartir y distribuir aplicaciones. Estas características hacen que Docker sea ampliamente utilizado en el desarrollo ágil, la integración continua, la entrega continua y la orquestación de contenedores a gran escala.

2. Una imagen es un paquete de sólo lectura que contiene todo lo necesario para ejecutar aplicaciones (librerías, configuraciones, etc.). Un contenedor es una instancia de una imagen en ejecución. La principal diferencia entre ambos es la capa escribible. Desde una imagen se pueden crear varios contenedor. Cada uno es autónomo y ejecuta en su propio entorno aislado.
3. Union Filesystem, también conocido como UnionFS, es un tipo de sistema de archivos que permite combinar varios sistemas de archivos en una sola vista lógica. Proporciona una forma eficiente de combinar y superponer varios directorios y sistemas de archivos en una jerarquía unificada. Docker utiliza Union Filesystem como parte de su arquitectura de contenedores para implementar y administrar imágenes y contenedores de forma eficiente.

   Los archivos creados dentro de un contenedor son almacenados en una capa escribible. Los datos no persisten cuando el contenedor deja de existir. Escribir dentro del contenedor requiere de un “storage driver” que provee un “union filesystem” usando el kernel de Linux.

   En resumen, Union Filesystem es una tecnología fundamental en Docker que permite la creación de imágenes en capas y la administración eficiente de contenedores mediante la superposición de sistemas de archivos. Esto ofrece beneficios en términos de eficiencia de almacenamiento, rendimiento y facilidad de administración de contenedores.

4. Cuando se crean contenedores en Docker, por defecto, utilizan direcciones IP dentro del rango de direcciones IP privadas reservadas para redes locales. El rango de direcciones IP más comúnmente utilizado por Docker es `172.17.0.0/16`. Sin embargo, este rango de direcciones IP puede variar dependiendo de la configuración de red específica de Docker. Docker asigna automáticamente una dirección IP a cada contenedor que se crea.

   La obtención de estas direcciones IP se realiza a través de un mecanismo llamado Docker bridge network. Cuando Docker se instala, crea una interfaz de red virtual llamada "docker0" que actúa como un puente entre los contenedores y el host. Esta interfaz de red tiene una dirección IP asignada, y Docker utiliza la tecnología de NAT (Network Address Translation) para traducir las direcciones IP de los contenedores a través de esta interfaz.

   Cuando se crea un contenedor, Docker asigna una dirección IP única dentro del rango especificado a la interfaz de red del contenedor. Además, Docker configura automáticamente las reglas de reenvío de IP y puertos para permitir la comunicación entre el contenedor y el host, así como entre los contenedores.

5. Docker tiene dos opciones para almacenar datos en el host y que los datos sean persistentes:
   - **Volúmenes**: Los volúmenes en Docker son una forma de almacenar y gestionar datos de forma persistente fuera del ciclo de vida de los contenedores. Los volúmenes son directorios especiales en el sistema de archivos del host o en un área designada por Docker que se montan en los contenedores. Los datos escritos en un volumen desde un contenedor están disponibles para otros contenedores que también monten ese mismo volumen. Los volúmenes se pueden utilizar para almacenar datos de aplicaciones, bases de datos, archivos de configuración, etc. Los volúmenes son administrados por Docker y persisten incluso cuando los contenedores se detienen o se eliminan. La ventaja de los volúmenes es que son independientes de la vida útil de los contenedores, lo que permite mantener los datos intactos aunque se elimine y vuelva a crear un contenedor.
   - **Bind Mounts**: Los bind mounts, o montajes de directorios del host, son una forma de vincular un directorio o archivo específico en el host con un directorio dentro del contenedor. Esto permite que el contenedor acceda a los datos del directorio del host y los utilice como datos persistentes. Los cambios realizados en el contenedor se reflejan directamente en el directorio del host y viceversa. Los bind mounts ofrecen una gran flexibilidad para compartir datos entre el host y el contenedor, ya que cualquier cambio realizado en el directorio del host se reflejará inmediatamente en el contenedor y viceversa. Sin embargo, a diferencia de los volúmenes, los bind mounts están vinculados a la ubicación específica del directorio en el host, por lo que si se elimina el directorio del host, se perderán los datos.

## Taller

1. Instale docker desde `https://docs.docker.com/engine/install/debian/#set-up-the-repository`.
2. 1. El tamaño lo encontramos en la columna: `SIZE` y es 77,8 MB. 1. La imagen descargada aún no se considera un contenedor, sino solo una plantilla para crear contenedores basados en esa imagen. Un contenedor es una instancia en ejecución de una imagen, sólo descargue la imagen. 1. "`Using default tag: latest`" significa que Docker utiliza la etiqueta "**latest**" por defecto al descargar una imagen si no se especifica ninguna otra etiqueta. La etiqueta "latest" suele referirse a la versión más reciente de una imagen.

      ```bash
      so@so2022:~$ docker pull ubuntu
      Using default tag: latest
      latest: Pulling from library/ubuntu
      837dd4791cdc: Pull complete
      Digest: sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd032e2246d30a722fa348fd799a5
      Status: Downloaded newer image for ubuntu:latest
      docker.io/library/ubuntu:latest
      so@so2022:~$ docker images
      REPOSITORY                                  TAG       IMAGE ID       CREATED         SIZE
      ubuntu                                      latest    1f6ddc1b2547   12 days ago     77.8MB
      hello-world                                 latest    9c7a54a9a43c   4 weeks ago     13.3kB
      ```

   2. Para realizar esto ejecute el comando: `docker run ubuntu ls -l`.
   3. Al ejecutar `docker run ubuntu /bin/bash`no sucede nada, no puedo utilizar la shell Bash del contenedor.

      1. Para poder iniciar el contenedor con una terminal interactiva se ejecuta el comando: `docker run -it ubuntu /bin/bash`. Al agregar la opción **`-it`**, se inicia el contenedor en modo interactivo con una terminal, lo que te permite interactuar directamente con la shell Bash del contenedor.
      2. El PID dentro del contenedor es 1 y fuera 2129.

         ```bash
         root@6c5b7031a1e7:/# echo $$
         1
         root@6c5b7031a1e7:/# exit
         exit
         so@so2022:~$ echo $$
         2129
         ```

      3. La salida del comando `lsns` muestra información sobre los diferentes namespaces del contenedor.

         ```bash
         root@a58e44380338:/# lsns
                 NS TYPE   NPROCS PID USER COMMAND
         4026531834 time        2   1 root /bin/bash
         4026531837 user        2   1 root /bin/bash
         4026532389 mnt         2   1 root /bin/bash
         4026532390 uts         2   1 root /bin/bash
         4026532391 ipc         2   1 root /bin/bash
         4026532392 pid         2   1 root /bin/bash
         4026532394 net         2   1 root /bin/bash
         4026532580 cgroup      2   1 root /bin/bash
         ```

      4. `touch /sistemas-operativos`.
      5. Los contenedores de Docker tienen su propio sistema de archivos aislado, por lo que los cambios realizados dentro del contenedor no se reflejan en el sistema operativo anfitrión.

   4. Al ejecutar nuevamente el contenedor se puede observar que no se encuentra el archivo creado anteriormente, esto es debido a que cada vez que se inicia un contenedor se crea una nueva instancia del mismo basada en la imagen Ubuntu original, lo que significa que cualquier cambio realizado dentro del contenedor se perderá cuando se detenga y vuelva a iniciarse.
   5. 1. El container_id se obtiene con el comando:

         ```bash
         so@so2022:~$ docker ps -a --filter "ancestor=ubuntu" --format "{{.ID}}\t{{.Image}}\t{{.Command}}\t{{.CreatedAt}}"
         1cea4e261d84	ubuntu	"/bin/bash"	2023-06-04 11:17:30 -0300 -03
         a58e44380338	ubuntu	"/bin/bash"	2023-06-04 11:13:32 -0300 -03
         6c5b7031a1e7	ubuntu	"/bin/bash"	2023-06-04 11:10:01 -0300 -03
         0b7f5bec4343	ubuntu	"/bin/bash"	2023-06-04 11:08:37 -0300 -03
         26208fb91f70	ubuntu	"ls -l"	2023-06-04 11:06:46 -0300 -03
         ```

      2. Efectivamente se encuentra el archivo.

         ```bash
         so@so2022:~$ docker start -ia a58e44380338
         root@a58e44380338:/# ls
         bin   dev  home  lib32  libx32  mnt  proc  run   sistemas-operativos  sys  usr
         boot  etc  lib   lib64  media   opt  root  sbin  srv                  tmp  var
         ```

   6. Para poder verificar los contenedores actualmente en ejecución y su estado se ejecuta el comando `docker ps -a`.

      ![Untitled](/img/tp4.png)

   7. Primero hay que detener todos los contenedores en ejecución con el comando `docker stop $(docker ps -aq)`. Luego, para eliminar todos los contenedores se ejecuta:

3. 1. `docker run -it ubuntu /bin/bash`
   2. ✅
   3. Si no se especifica nombre por defecto se genera sin un nombre específico y se le asigna un identificador único.

      ```bash
      so@so2022:~$ docker commit 05da9ea4b2f6
      sha256:3ccf2ef51c14897a78c2b0bd44a68365ecc7a109ca5fd5db5f1b55c68e559c43
      ```

   4. `docker tag 3ccf2ef51c14 nginx-so:v1`.
   5. 1. Ruta absoluta: `$HOME/so/nginx-so`.
      2. ✅
      3. `docker run -p 8080:80 -v $HOME/so/nginx-so:/var/www/html nginx-so:v1 nginx -g 'daemon off;’.`
   6. No me funciona!!
   7. No lo puedo hacer porque no me funciona.
   8. Idem g.

4. 1. ```bash
      FROM ubuntu
      EXPOSE 80
      RUN apt-get update && apt-get install -y nginx
      COPY index.html /var/www/html
      CMD ["nginx", "-g", "daemon off;"]
      ```

   2. `docker build -t nginx-so:v2 $HOME/so/nginx-so`
   3. ✅
   4. No se ven reflejados los cambios. Esto se debe a que el contenido del archivo se copió en el momento de la construcción de la imagen y no se actualiza automáticamente.
   5. No se ven reflejados los cambios porque la construcción del contenedor no incluía los nuevos cambios.
   6. Ahora si se ven los cambios!! Porque se guardan con la construcción de la imagen.
