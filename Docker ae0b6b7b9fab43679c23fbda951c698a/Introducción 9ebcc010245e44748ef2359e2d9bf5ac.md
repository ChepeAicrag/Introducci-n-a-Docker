# Introducción

Una breve introducción a Docker y la diferencia que hay entre este y una VM.

## Problemas del desarrollo del software profesional

Construir: 

- Escribir código
- Los problemas complejos necesitan equipos

Problemas de construir:

- Entorno de desarrollo (Versión del lenguaje de programación)
- Dependencias (Librerías externas)
- Entorno de ejecución
- Equivalencia con entorno productivo
- Servicios externos

Distribuir: 

- Tu código tiene que transforma en un artefacto o varios que pueden ser transportados a donde tengan que ser ejecutados.
- Se refiere a portabilidad

Problemas de distribuir:

- Divergencia de repositorios
- Divergencia de artefactos
- Versionado (que versión de distribuye: el código publicado es el correcto)

Ejecutar software: 

- La maquina donde se escribe el software siempre es distinta a la máquina donde se ejecuta de manera productiva.

Problemas de ejecutar software: 

- Compatibilidad con el entorno productivo
- Dependencias
- Disponibilidad de servicios externos
- Recursos del hardware

“Docker te permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado.”

**Problemáticas del desarrollo de software**

**1. Construir -** Escribir código en la máquina del desarrollador. (Compile, que no compile, arreglar el bug, compartir código, etc. )

**Problemática:**

- Entorno de desarrollo (paquetes)
- Dependencias (Frameworks, bibliotecas)
- Versiones de entornos de ejecución (runtime, versión Node)
- Equivalencia de entornos de desarrollo (compartir el código)
- Equivalencia con entornos productivos (pasar a producción)
- Servicios externos (integración con otros servicios ejem: base de datos)

**2. Distribuir** - Llevar la aplicación donde se va a desplegar (Transformarse en un artefacto)

**Problemática:**

- Output de build heterogeo (múltiples compilaciones)
- Acceso a servidores productivos (No tenemos acceso al servidor)
- Ejecución nativa vs virtualizada
- Entornos Serverless

**3. Ejecutar** - Implementar la solución en el ambiente de producción (Subir a producción)El reto Hacer que funcione como debería funcionar

**Problemática:**

- Dependencia de aplicación (paquetes, runtime)
- Compatibilidad con el entorno productivo (sistema operativo poco amigable con la solución)
- Disponibilidad de servicios externos (Acceso a los servicios externos)
- Recursos de hardware (Capacidad de ejecución - Menos memoria, procesador más debil)

## Virtualización

... versión virtual de algún recurso tecnológico, cómo (...) hardware, un sistema operativo, un dispositivo de almacenamiento o (...) recyrsi de red. 

Es lo que nos permite atacar en simultaneo los tres problemas del desarrollo de software profesional. 

---

**Problemas de la virtualización**

- PESO: En el orden de los GBs. Repiten archivos en común. Inicio lento.
- COSTO DE ADMINISTRACION: Necesita mantenimiento igual que cualquier otra computadora.
- MULTIPLES DE FORMATO: VDI, VMDK, VHD, raw, etc

## Contenedores**🚢**

Un contenedor la unidad logica mas importande de docker que permite encapsular las dependencias de un proyecto en un entorno aislado, con esto puedes conseguir resolver el problema de “But it works in my machine” ya que te permite crear una especie de maquina virtual y pasársela a tus compañeros o colocarlo en un servidor para el despliegue de manera fácil.

¡✋Alerta! Puedes ver a un contendor como una maquina virtual pero eso no significa que lo sea, una maquina virtual puede llegar a se muy similar debido a sus funcionalidades como el aislamiento de procesos.

Entonces… **¿cuál es el beneficio de usar contenedores en lugar de maquinas virtuales?**

El mayor beneficio es que los contenedores de Docker están en el nivel de los MB eso nos da ventajas en el consumo de recursos, ya que estos corren compartiendo el host del kernel de Linux, por otro lado las maquinas virtuales son un sistema operativo(O.S) con sus propias apps que corre sobre el tuyo usando virtualizacion y que consume muchos recursos en el nivel de los GB. Una mejor forma de ver esto es observando la arquitectura de los contendores y de las maquinas virtuales

***Los beneficios de usar contenedores incluyen:***

- Ágil creación y despliegue de aplicaciones: Mayor facilidad y eficiencia al crear imágenes de contenedor en vez de máquinas virtuales.
- Desarrollo, integración y despliegue continuo: Permite que la imagen de contenedor se construya y despliegue de forma frecuente y confiable, facilitando los rollbacks pues la imagen es inmutable.
- Separación de tareas entre Dev y Ops: Puedes crear imágenes de contenedor al momento de compilar y no al desplegar, desacoplando la aplicación de la infraestructura.
- Observabilidad: No solamente se presenta la información y métricas del sistema operativo, sino la salud de la aplicación y otras señales.
- Consistencia entre los entornos de desarrollo, pruebas y producción: La aplicación funciona igual en un laptop y en la nube.
- Portabilidad entre nubes y distribuciones: Funciona en Ubuntu, RHEL, CoreOS, tu datacenter físico, Google Kubernetes Engine y todo lo demás.
- Administración centrada en la aplicación: Eleva el nivel de abstracción del sistema operativo y el hardware virtualizado a la aplicación que funciona en un sistema con recursos lógicos.
- Microservicios distribuidos, elásticos, liberados y débilmente acoplados: Las aplicaciones se separan en piezas pequeñas e independientes que pueden ser desplegadas y administradas de forma dinámica, y no como una aplicación monolítica que opera en una sola máquina de gran capacidad
- Aislamiento de recursos: Hace el rendimiento de la aplicación más predecible.
- Utilización de recursos: Permite mayor eficiencia y densidad.

**Containerización**

El empleo de **contenedores** para construir y desplegar software. 

Las ventajas que permite:

- Flexibles: Toda aplicación se mete en un contenedor
- Livianos: Reutilizar el kernel del host
- Portables: Diseñados para correr de la misma manera en cualquier máquina o servidor
- Bajo acoplamiento: Los contenedores son autocontenidos (todo para poder correr el software contenido)
- Escalables: Muy fácil crear uno o muchos contenedores que sean iguales
- Seguros: Un contenedor solo puede acceder a las partes que necesita, no accede a otras inncesarios

**Virtualizacion vs Containerización**

- Virtualización: A diferencia de un contenedor, las máquinas virtuales ejecutan un sistema operativo completo, incluido su propio kernel.
- Containerización: Un contenedor es un silo aislado y ligero para ejecutar una aplicación en el sistema operativo host. Los contenedores se basan en el kernel del sistema operativo host (que puede considerarse la fontanería del sistema operativo), y solo puede contener aplicaciones y algunas API ligeras del sistema operativo y servicios que se ejecutan en modo de usuario.

A diferencia de una máquina virtual, que es una abstracción del hardware y emula toda una computadora (o servidor), un contenedor es una abstracción del software y éste puede empaquetar el código, el runtime necesario y las dependencias de una aplicación

![Untitled](Introduccio%CC%81n%209ebcc010245e44748ef2359e2d9bf5ac/Untitled.png)

# ¿Qué es Docker?

Docker es una plataforma de software que nos permite crear, probar e implementar rápidamente, mediante contenedores. 

Tecnología se ha convertido en un estándar para

Es importante por el tema de compatibilidad, resuelvas estos problemas muy fácilmente, genera confianza. Cómo desarrollador es una herramienta que te permite portabilidad y versatilidad 

## ¿Qué es un contenedor?

Un contenedor en la vida real es un lugar que nos sirve para almacenar cosas, que posteriormente estas cosas serán transportadas a otro destino.
Es un lugar virtual donde se almacenarán dependencia que necesita una aplicación para poder ejecutarse correctamente. En un contenedor se empaqueten una aplicación con todo sus componentes necesarios para ejecutar en cualquiera parte. 

# build once run anywhere

Es el eslogan de Docker, hace referencia a configurar un contenedor y poder ejecutarlo en cualquier parte

# VM vs Contenedor

![Introduccio%CC%81n%209ebcc010245e44748ef2359e2d9bf5ac/VM_vs_Container.png](Introduccio%CC%81n%209ebcc010245e44748ef2359e2d9bf5ac/VM_vs_Container.png)

- La arquitectura de una VM vs La arquitectura de un contenedor.
- Docker no es un VM ligera.
- En ambos casos se tienen aplicaciones ejecutándose.
- De lado derecho, no se usa Guest OS, ya que se ejecutan en la maquina HOST.
- Docker Engine es el encargado de gestionar a los recursos del SO hacia los contenedores, similar al kernel, su función es asignar lo que sea necesario para que los contenedores funcionen adecuadamente.

### Arquitectura de una VM

- Un sistema subyacente (hardware / infraestructura) que incluye la máquina física y su sistema operativo. Los hipervisores desnudos no requieren un sistema operativo subyacente en esta capa.
- Un hipervisor que actúa como intermediario entre el hardware y la infraestructura subyacente.
- Varias máquinas virtuales que utilizan los recursos del host comunicándose con el hipervisor.
- Aplicaciones y procesos que se ejecutan en el sistema operativo de cada invitado.
- El hipervisor debe configurarse adecuadamente antes de implementar cualquier máquina virtual.

Ventajas:

- Se pueden utilizar varios entornos de sistema operativo en la misma computadora.
- VM mejora la confiabilidad del sistema y evita fallas del sistema. Incluso si falla, el sistema operativo del host no se verá afectado debido al aislamiento.
- Proporciona una capa de seguridad, si la máquina virtual se ve afectada por algún malware, no resultará en una violación de la seguridad en el sistema operativo host.

Desventajas

- La ejecución de varias máquinas virtuales puede generar una salida inestable.
- Las máquinas virtuales son menos eficientes y lentas en comparación con una máquina física.
- Una máquina virtual puede infectarse con las debilidades de la máquina host.

### Arquitectura de un contenedor

Un contenedor requiere un sistema operativo, programas y bibliotecas de soporte, y recursos del sistema para ejecutar un programa específico. Cuando trabaje dentro de un contenedor, puede crear una plantilla de un entorno que necesite. Básicamente, el contenedor ejecuta una instantánea del sistema en un momento determinado, lo que proporciona coherencia en el comportamiento de una aplicación.

El contenedor comparte el kernel del host para ejecutar todas las aplicaciones individuales dentro del contenedor. Los únicos elementos que requiere cada contenedor son bins, bibliotecas y otros componentes de tiempo de ejecución

Ventajas:

- Los contenedores pueden ser tan pequeños como 10 MB y puede limitar fácilmente su memoria y el uso de la CPU. Entonces, son livianos.
- Dado que son de tamaño pequeño, pueden iniciarse más rápido y también se pueden escalar rápidamente.
- Los contenedores son ejemplares en lo que respecta a la implementación de integración continua y despliegue continuo (CI / CD).

Desventajas:

- Dado que los contenedores se ejecutan en el sistema operativo host, tienen una dependencia del sistema operativo host subyacente del host.
- Los contenedores no pueden por sí solos proporcionar seguridad a un nivel encomiable.
- Cuando se elimina el contenedor si se pierden los datos dentro del contenedor. Deberá agregar volúmenes de datos para almacenar los datos.

## Similitudes entre VM y Docker

- Ambos pueden tener ejecutando aplicaciones.
- Tiene una buena aislación de las aplicaciones.
- Emplean un SO.
- Los contenedores  pueden ser utilizados junto con máquinas virtuales.

## Diferencias entre VM y Docker

- Los contenedores no necesitan un sistema operativo completo sino que reutilizan el subyacente.
- Es más rápido ejecutar un contenedor que una VM.
- Los contenedores son más ligeros que las VM en cuanto a hardware empleado.
- El espacio ocupado en disco es muy inferior con Docker al no necesitar que instalemos el sistema operativo completo.
- En una VM debemos indicar de antemano cuántos recursos físicos le debemos dedicar.
- Las VM tiene una aislación mayor a un contenedor.
- Las VM emplean virtualización.  La virtualización consiste en simular a todos los componentes de un SO: la RAM, el kernel, etc.
- Las VM tienen seguridad a comparación de los contenedores.
- Los contenedores son gratuitos y de código abierto.

### Docker es una tecnología para la manipulación de contenedores.

# Desventajas de Docker

Docker no permite utilizar en un sistema operativo "host" contenedores/aplicaciones que no sean para ese mismo sistema operativo. Es decir, no podemos ejecutar un contenedor con una aplicación para Linux en Windows ni al revés. Lo cual puede suponer un impedimento en algunas ocasiones.

# ¿Por qué usar Docker?

- Se puede ejecutar en cualquiera parte. Soluciona el Ah ... es que en mi máquina funciona
- La aplicación se puede ejecutar en desarrollo y producción de la misma forma.
- Se puede usar en cualquier SO, ya sea Mac, Windows o Linux.
- Los contenedores permiten desplegar aplicaciones más rápido, arrancarlas y pararlas más rápido y aprovechar mejor los recursos de hardware.
- Existe tecnología que permite trabajar con multiples contenedores a la vez, como lo es Kubernetes, Docker-Compose, Docker Swarm.

# Referencias de apoyo

[Te lo explico con gatitos](https://teloexplicocongatitos.com/poster/tlecg16)

[](https://labs.play-with-docker.com/)