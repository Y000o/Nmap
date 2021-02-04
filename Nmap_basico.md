# Nmap - Básico 

## TEMAS

* [¿Qué es Nmap?](#¿Qué-es-Nmap?)
* [Características](#Características)
* [Tipos de inyección sql](#Tipos-de-inyección-sql)
    * [Inyección sql manual](#Inyección-sql-manual)
    
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
