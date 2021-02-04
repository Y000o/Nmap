# Nmap - Básico 

## TEMAS

* [¿Qué es Nmap?](#¿Qué-es-Nmap?)
* [Características](#Características)
* [Instalación](#Instalación)
    * [Windows](#Windows)
    * [Linux](#Linux)
    * [Mac OS](#Mac-OS)
* [Port scanning](#Port-scanning)

    
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


