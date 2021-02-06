# Nmap - Medio - Avanzado  

## TEMAS

* [Introducción a TCP/IP](#Introducción-a-TCP/IP)
    * [TCP/IP](#TCP/IP)
    * [Transmission Control Protocol (TCP)](#Transmission-Control-Protocol-(TCP))
    * [Estado de los puertos](#Estado-de-los-puertos)
    * [Flags](#Flags)
         * [SYN](#SYN)
         * [ACK](#ACK)
         * [RST](#RST)
         * [PSH](#PSH)
         * [URG](#URG)
         * [FIN](#FIN)
* [Listado de todos los comandos básicos](#Listado-de-todos-los-comandos-básicos)
* [Características](#Características)
* [Instalación](#Instalación)
    * [Windows](#Windows)
    * [Linux](#Linux)
    * [Mac OS](#Mac-OS)


Esta es la continuación al escrito "Nmap - Básico" en donde se habló solamente de las caracteristicas de esta herramienta, el uso que se le da, y dejamos un poco de lado la parte "practica" centrado el escrito de una forma mas "teorica" y de esa manera poder conocer "básico" para no perdernos entre las opciones que esta herramienta nos permite utilizar.

En esta continuacion se hablará de un punto de vista mas practico y se hará lo posible para avarcar lo mas posible entre las opciones que nos da esta herramienta.

## Introducción a TCP/IP

Para empezar bien esto primero vamos a aclarar unos terminos que necesitamos conocer si nos vamos a meter en el mundo de las redes, escaneos...

### TCP/IP

Transmission Control Protocol(TCP) e Internet Protocol (IP). Basicamente estos 2 hacen la base para todo el internet. TCP se encarga de controlar, ordenar, manejar los datos y de asegurarse de que llegan a su destino sin que se pierdan paquetes. Pertenece a la capa de Transporte del protocolo TCP/IP.

IP se encarga de enviar los paquetes a su destino. Pertenece a la capa Internet de TCP/IP.

### Transmission Control Protocol (TCP)

TCP actúa tanto como emisor como receptor de paquetes. El TCP emisor divide la información a transferir en paquetes y envía cada uno por separado, retransmitiéndolos si alguno no se entrega correctamente. Si es necesario el TCP receptor tendrá que reordenar los paquetes secuencialmente (cada paquete tiene un número de secuencia de 32 bits, que es un campo de la cabecera) para poder obtener la información.

Otros campos de la cabecera son Source port (Puerto fuente) y Destination port (Puerto destino) que son números sin signo (unsigned) de 16 bits. TCP usa los números de puerto para identificar la aplicación emisora y la aplicación receptora. Hay puertos TCP de 0 a 65535, sin embargo podemos distinguir tres tipos de puertos:
Puertos conocidos: De 0 al 1023, están reservados por la IANA para determinado tipo de aplicaciones (servidor HTTP, FTP, etc.) y se requiere de privilegios de administrador en una máquina para activar una aplicación en uno de estos puertos.

Un ejemplo es el puerto 80(HTTP).

Puertos registrados: de 1024 a 49151, reservados para aplicaciones concretas. Un ejemplo es el 3306(MySQL).
Puertos privados: de 49152 a 65535, estos no están reservados para ninguna aplicación concreta.

#### Estado de los puertos

Podemos decir que un puerto puede estar en tres estados:

```
Puerto filtrado: Un firewall (cortafuegos) bloquea el acceso al puerto.

Puerto cerrado: El puerto no está bloqueado pero no hay ninguna aplicación escuchando en él.

Puerto abierto: El puerto no está bloqueado y hay una aplicación escuchando en él.
```

A la técnica de detectar los puertos abiertos de una máquina se le denomina Escaneo de puertos (Port scan), que se hace con fines de seguridad pero también proporciona mucha información de utilidad a un posible atacante de nuestra máquina, ya que le permite saber qué aplicaciones tenemos activas para explotar algún fallo y conseguir el acceso.

### Flags

En la cabecera de los paquetes de TCP hay 6 flags de 1 bit, es decir, que pueden valer ó 0 ó 1 según estén desactivadas oactivadas. 
Estas banderas son: SYN, ACK, RST, PSH, URG y FIN.


#### SYN

SYN (Synchronize): se utiliza para iniciar una conexión TCP. Si enviamos un paquete con este flag a un puerto X de una máquina, pueden pasar varias cosas:

Si el puerto está abierto, la máquina nos responde con un paquete con los flags SYN y ACK.
Si el puerto está cerrado, nos responde con un paquete con los flags RST y ACK.
Si el puerto está filtrado, no recibiremos nada.

#### ACK

ACK (Acknowledgement): Este flag se utiliza para confirmaciones.

Si enviamos un paquete ACK "no solicitado" a un puerto X de una máquina, debería ser respondido con otro paquete con flag RST, sea cual fuere el estado del puerto. De hecho hay firewalls que sólo filtran los paquetes de intento de conexión con SYN, no filtrando los que llevan ACK.

#### RST

RST (Reset): Este bit se utiliza para reiniciar una conexión debido a paquetes corrompidos o a SYN duplicados, retardados, etc

#### PSH

PSH (Push): Se utiliza para forzar el enviado inmediato de los datos tan pronto como sea posible. Si el TCP
emisor envía un paquete con este flag activado, el TCP receptor sabe que tiene que entregar los datos
inmediatamente a la aplicación receptora sin ponerlo en un buffer y esperar a más datos.

#### URG

URG (Urgent): Sirve para definir un bloque de datos como "urgente". El campo Urgent Pointer de la cabecera es
un puntero que "apunta" a donde terminan estos datos urgentes; de esta manera se marca el bloque de datos.

#### FIN

FIN (Finalize): Sirve para finalizar una conexión.




## Listado de todos los comandos básicos

Vamos a repasar de nuevo cada seccion pero a diferencia del escrito pasado vamos a profundizar un poco mas en el funcionamieno de los escaneos y algunos ejemplos.

### Seleccionar objetivos

Direcciones o rangos IP, nombres de sistemas, redes, etc.

Ejemplo: scanme.nmap.org

```
-iL fichero lista en fichero 
-iR n elegir objetivos aleatoriamente, 0 nunca acaba
–exclude –excludefile fichero excluir sistemas desde fichero
```

### Descubrir sistemas 

En este apartado se utilizan comandos para descubrir los HOST que estan dentro de una red.

```
-PS n tcp syn ping
-PA n ping TCP ACK
-PU n ping UDP
-PM Netmask Req
-PP Timestamp Req
-PE Echo Req
-sL análisis de listado
-PO ping por protocolo
-PN No hacer ping
-n no hacer DNS
-R Resolver DNS en todos los sistemas objetivo
–traceroute: trazar ruta al sistema (para topologías de red)
-sP realizar ping, igual que con –PP –PM –PS443 –PA80
```







### Técnicas de análisis de puertos
```
-sS análisis utilizando TCP SYN
-sT análisis utilizando TCP CONNECT
-sU análisis utilizando UDP
-sY análisis utilizando SCTP INIT
-sZ utilizando COOKIE ECHO de SCTP
-sO protocolo IP
-sW ventana TCP -sN
–sF -sX NULL, FIN, XMAS
–sA TCP ACK
```
### Puertos a analizar y orden de análisis
```
-p n-mrango
-p– todos los puertos
-p n,m,z especificados
-p U:n-m,z T:n,m U para UDP, T para TCP
-F rápido, los 100 comunes
–top-ports n analizar los puertos más utilizados
-r no aleatorio
```
### Duración y ejecución
```
-T0 paranoico
-T1 sigiloso
-T2 sofisticado
-T3 normal
-T4 agresivo
-T5 locura
–min-hostgroup
–max-hostgroup
–min-rate
–max-rate
–min-parallelism
–max-parallelism
–min-rtt-timeout
–max-rtt-timeout
–initial-rtt-timeout
–max-retries
–host-timeout –scan-delay
```
### Detección de servicios y versiones
```
-sV: detección de la versión de servicios
–all-ports no excluir puertos
–version-all probar cada exploración
–version-trace rastrear la actividad del análisis de versión
-O activar detección del S. Operativo
–fuzzy adivinar detección del SO
–max-os-tries establecer número máximo de intentos contra el sistema objetivo
```
### Evasión de Firewalls/IDS
```
-f fragmentar paquetes
-D d1,d2 encubrir análisis con señuelos
-S ip falsear dirección origen
–g source falsear puerto origen
–randomize-hosts orden
–spoof-mac mac cambiar MAC de origen
Parámetros de nivel de detalle y depuración
-v Incrementar el nivel de detalle
–reason motivos por sistema y puerto
-d (1-9) establecer nivel de depuración
–packet-trace ruta de paquetes
```
### Otras opciones
```
–resume file continuar análisis abortado (tomando formatos de salida con -oN o -oG)
-6 activar análisis IPV6
-A agresivo, igual que con -O -sV -sC –traceroute
Opciones interactivas
v/V aumentar/disminuir nivel de detalle del análisis
d/D aumentar/disminuir nivel de depuración
p/P activar/desactivar traza de paquetes
Scripts
-sC realizar análisis con los scripts por defecto
–script file ejecutar script (o todos)
–script-args n=v proporcionar argumentos
–script-trace mostrar comunicación entrante y saliente
Formatos de salida
-oN guardar en formato normal
-oX guardar en formato XML
-oG guardar en formato para posteriormente usar Grep
-oA guardar en todos los formatos anteriores
```










