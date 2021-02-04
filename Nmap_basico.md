# Nmap - Básico 

## TEMAS

* [¿Qué es Nmap?](#¿Qué-es-Nmap?)
* [Características](#Características)
* [Instalación](#Instalación)
    * [Windows](#Windows)
    * [Linux](#Linux)
    * [Mac OS](#Mac-OS)
* [Port scanning](#Port-scanning)
* [Listado de todos los comandos básicos](#Listado-de-todos-los-comandos-básicos)
    * [Seleccionar objetivos](#Seleccionar-objetivos)
    * [Descubrir sistemas](#Descubrir-sistemas)
    * [Técnicas de análisis de puertos](#Técnicas-de-análisis-de-puertos)
    * [Puertos a analizar y orden de análisis](#Puertos-a-analizar-y-orden-de-análisis)
    * [Duración y ejecución](#Duración-y-ejecución)
    * [Detección de servicios y versiones](#Detección-de-servicios-y-versiones)
    * [Evasión de Firewalls/IDS](#Evasión-de-Firewalls/IDS)
    * [Otras opciones](#Otras-opciones)
* [Ejemplos de comandos](#Ejemplos-de-comandos)

    
## ¿Qué es Nmap?

Nmap, abreviatura de Network Mapper, es una herramienta gratuita de código abierto para la exploración de vulnerabilidades y la detección de redes. 
Los administradores de red utilizan Nmap para identificar qué dispositivos se están ejecutando en sus sistemas, 
descubrir los hosts disponibles y los servicios que ofrecen, encontrar puertos abiertos y detectar riesgos de seguridad.

Nmap puede ser utilizado para monitorear hosts individuales así como redes extensas que abarcan cientos de miles de dispositivos y multitudes de subredes.


## Características

Nmap es una de las mejores herramientas de escaneo de puertos y descubrimiento de hosts que existen. Este nos permite obtener 
una gran cantidad de información sobre los equipos de nuestra red, es capaz de escanear hosts que están levantados, comprobar si tienen algún puerto abierto,
si están filtrando los puertos (tienen un firewall activado), e incluso saber qué sistema operativo está utilizando un determinado objetivo.

Nmap nos permite detectar hosts de una red local, y también a través de Internet, de esta forma, 
podremos saber si dichos hosts (ordenadores, servidores, routers, switches, dispositivos IoT) están actualmente conectados a Internet o a la red local.
Esta herramienta también permite realizar un escaneo de puertos a los diferentes hosts, ver qué servicios tenemos activos en dichos hosts gracias a que nos dirá el estado de sus puertos,
podremos saber qué sistema operativo está utilizando un determinado equipo, e incluso podremos automatizar diferentes pruebas de pentesting para comprobar la seguridad de los equipos.


## Instalación 

Nmap es un sistema muy flexible, gracias a esto esta herramienta puede ser instalada en una gran cantidad de dispositivos, como podemos ver en su página principal:
https://nmap.org/download.html

### Windows

Para instalar la herramienta en sistemas windows tenemos que seguir los siguientes pasos:

```
1 - descargar el siguiente archivo: https://nmap.org/dist/nmap-7.91-win32.zip 
2 - Descomprimir el archivo 
3 - Ejecutar el archivo nmap.exe desde la terminal (ej. "CMD")
```
### Linux

En linux tenemos la oportunidad de instalar desde el codigo fuente o desde las paqueterías, si instalamos desde las paqueterías los comandos cambiarán dependientdo la distribucion. 

Ejemplos:

Instalando desde RPMs 
```
1- Installing Nmap from binary RPMs
`rpm -vhU https://nmap.org/dist/nmap-4.68-1.i386.rpm`
2- Building and installing Nmap from source RPMs
`rpmbuild --rebuild https://nmap.org/dist/nmap-4.68-1.src.rpm`

Installing Nmap from a system Yum repository
`yum install nmap`

Installing Nmap from a system Apt repository
`apt-get install nmap.`
```

https://nmap.org/book/inst-linux.html#inst-rpm 


### Mac OS

pasos para instalar nmap en mac:

```
1 - descargar el siguiente archivo: https://nmap.org/dist/nmap-7.91.dmg
2 - instalar.
```
https://nmap.org/book/inst-macosx.html

## Port scanning

Los paquetes que Nmap envía devuelven direcciones IP y una gran cantidad de otros datos, lo que le permite identificar todo tipo de atributos de la red, permite crear un perfil o mapa de la red y un inventario de hardware y software.

Diferentes protocolos utilizan diferentes tipos de estructuras de paquetes. Nmap emplea protocolos de capa de transporte que incluyen TCP (Transmission Control Protocol), UDP (User Datagram Protocol) y SCTP (Stream Control Transmission Protocol), así como protocolos de soporte como ICMP (Internet Control Message Protocol), utilizados para enviar mensajes de error.

Los distintos protocolos sirven para diferentes propósitos y puertos del sistema. Por ejemplo, la sobrecarga de recursos de UDP es adecuada para la transmisión de vídeo en tiempo real, en la que se sacrifican algunos paquetes perdidos a cambio de velocidad, mientras que los vídeos de transmisión en tiempo no real en YouTube se almacenan en búferes y utilizan el protocolo TCP más lento, aunque más fiable.

## Listado de todos los comandos básicos

Este programa es realmente completo, hasta el momento hemos utilizado los comandos básicos para descubrir hosts y también para ver si tiene los puertos abiertos, sin embargo, esto no se queda así, y tenemos un gran listado de comandos para exprimir al máximo esta herramienta.

### Seleccionar objetivos

Direcciones o rangos IP, nombres de sistemas, redes, etc.

Ejemplo: scanme.nmap.org

-iL fichero lista en fichero -iR n elegir objetivos aleatoriamente, 0 nunca acaba
–exclude –excludefile fichero excluir sistemas desde fichero

### Descubrir sistemas

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

### Técnicas de análisis de puertos

-sS análisis utilizando TCP SYN
-sT análisis utilizando TCP CONNECT
-sU análisis utilizando UDP
-sY análisis utilizando SCTP INIT
-sZ utilizando COOKIE ECHO de SCTP
-sO protocolo IP
-sW ventana TCP -sN
–sF -sX NULL, FIN, XMAS
–sA TCP ACK

### Puertos a analizar y orden de análisis

-p n-mrango
-p– todos los puertos
-p n,m,z especificados
-p U:n-m,z T:n,m U para UDP, T para TCP
-F rápido, los 100 comunes
–top-ports n analizar los puertos más utilizados
-r no aleatorio

### Duración y ejecución

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

### Detección de servicios y versiones

-sV: detección de la versión de servicios
–all-ports no excluir puertos
–version-all probar cada exploración
–version-trace rastrear la actividad del análisis de versión
-O activar detección del S. Operativo
–fuzzy adivinar detección del SO
–max-os-tries establecer número máximo de intentos contra el sistema objetivo

### Evasión de Firewalls/IDS

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

### Otras opciones

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


Principalmente estos son los comandos de que dispone Nmap. Antes de terminar, debemos decir que Nmap dispone de multitud de opciones con las que poder realizar completos análisis de redes. Podemos consultar todas las opciones disponibles tecleando:

nmap --help


## Ejemplos de comandos 

Para todos los ejemplos que vamos a ver, usarémos la página http://scanme.nmap.org/

#### Escaneo mediante el nombre de host

`nmap scanme.nmap.org`

#### Escanear utilizando la opción “-v” 

Se puede ver que el siguiente comando con la opción “-v” está dando una información más detallada acerca de la máquina remota.

`nmap -v scanme.nmap.org`


#### Analizar la lista de los ejércitos de un archivo

`nmap -iL nmaptest.txt`

#### Información Scan OS y Traceroute

Con Nmap, puede detectar qué sistema operativo y la versión que está ejecutando en el host remoto. Para habilitar la detección de sistema operativo y versión, la exploración de la escritura y la Ruta de seguimiento, podemos usar la opción “-A” con NMAP.

`nmap -A scanme.nmap.org`

#### Activar la Detección de OS con Nmap

Utilice la opción “-O” y “-osscan-guess” también ayuda a descubrir la información del sistema operativo.
 
`nmap -O scanme.nmap.org`

#### Escanear un anfitrión para Detectar Firewall 

El siguiente comando realizará una búsqueda en un host remoto para detectar si los filtros de paquetes o Firewall se utiliza por el anfitrión.

`nmap -sA scanme.nmap.org`

#### Escanear un anfitrión para comprobar su protección por Firewall

`nmap -PN scanme.nmap.org`

#### Encontrar versión Host Services Números

Podemos encontrar versiones de servicios que se ejecutan en máquinas remotas con la opción “-sV”.

`nmap -sV scanme.nmap.org`

#### Analizar hosts remotos a través de TCP ACK (PA) y TCP Syn (PS)

A veces, los cortafuegos de filtrado de paquetes bloquea las solicitudes de ping ICMP estándar, en ese caso, podemos utilizar métodos TCP ACK y TCP Syn para escanear hosts remotos.

`nmap -PS scanme.nmap.org`

#### Escaneo FIN de Nmap

Escaneo FIN agresivo

`nmap -sF -T4 scanme.nmap.org`

Escaneo insanamente agresivo de tipo FIN contra un dispositivo.

`nmap -sF -T5 scanme.nmap.org`

------------------------------

Espero que este escrito sea de su agrado, fue pensado para intruducirnos en el tema de los escaneos en NMAP.

