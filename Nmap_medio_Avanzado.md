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
    * [Seleccionar objetivos](#Seleccionar-objetivos)
    * [Descubrir sistemas](#Descubrir-sistemas)
    * [Técnicas de análisis de puertos](#Técnicas-de-análisis-de-puertos)
    * [Puertos a analizar y orden de análisis](#Puertos-a-analizar-y-orden-de-análisis)
    * [Duración y ejecución](#Duración-y-ejecución)
    * [Detección de servicios y versiones](#Detección-de-servicios-y-versiones)
    * [Evasión de Firewalls/IDS](#Evasión-de-Firewalls/IDS)
    * [Otras opciones](#Otras-opciones)
    * [Ejemplos de comandos utiles](#Ejemplos-de-comandos-utiles)
* [Nmap Scripting Engine](#Nmap-Scripting-Engine)
    
    



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

Vamos a ver algunos ejemplos:

los siguientes son parecidos, ya que tienen un comportamiento similar pero cambia la flag que se utiliza:
```
-PS n tcp syn ping
-PA n ping TCP ACK
-PU n ping UDP
```

#### -PS

-PS n tcp syn ping


Esta opción envía un paquete TCP vacío con la flag SYN activada. El puerto de destino predeterminado es 80. Los puertos alternativos se pueden especificar como parámetro. La sintaxis es la misma que para -p excepto que no se permiten especificadores de tipo de puerto como T :. Los ejemplos son -PS22 y -PS22-25,80,113,1050,35000.

La frag SYN sugiere al sistema remoto que está intentando establecer una conexión. Normalmente, el puerto de destino se cerrará y se enviará un paquete RST (restablecimiento). Si el puerto está abierto, el destino dará el segundo paso de un protocolo de enlace de tres vías de TCP respondiendo con un paquete SYN / ACK TCP. La máquina que ejecuta Nmap luego rompe la conexión naciente respondiendo con un RST en lugar de enviar un paquete ACK que completaría el protocolo de enlace de tres vías y establecería una conexión completa. El paquete RST es enviado por el kernel de la máquina que ejecuta Nmap en respuesta al SYN / ACK inesperado, no por Nmap en sí.

ejemplo: `nmap -PS scanme.nmap.org`

##### -PO

-PO ping por protocolo


El ping del protocolo IP, envía paquetes IP con el número de protocolo especificado establecido en su encabezado IP. La lista de protocolos tiene el mismo formato que las listas de puertos en las opciones de detección de host TCP, UDP y SCTP discutidas anteriormente.


#### -PN

-Pn (No ping)


Esta opción omite por completo la etapa de descubrimiento de host. Normalmente, Nmap usa esta etapa para determinar las máquinas activas para un escaneo más pesado y para medir la velocidad de la red. De forma predeterminada, Nmap solo realiza un sondeo intenso, como análisis de puertos, detección de versiones o detección de SO contra hosts que se encuentran activos. Deshabilitar el descubrimiento de host con -Pn hace que Nmap intente las funciones de escaneo solicitadas contra cada dirección IP de destino especificada.

#### -Traceroute

–traceroute: trazar ruta al sistema (para topologías de red)

Personalmente me gusta mucho el output de esta opcion, no se... me siento bien hacker con la pantalla asi.


Traceroute funciona mediante el envío de paquetes con un TTL (tiempo de vida) bajo en un intento de obtener mensajes ICMP de tiempo excedido de saltos intermedios entre el escáner y el host de destino. Las implementaciones de traceroute estándar comienzan con un TTL de 1 y aumentan el TTL hasta que se alcanza el host de destino. El traceroute de Nmap comienza con un TTL alto y luego disminuye el TTL hasta que llega a cero. Hacerlo al revés permite a Nmap emplear algoritmos de almacenamiento en caché inteligentes para acelerar los seguimientos en múltiples hosts. En promedio, Nmap envía de 5 a 10 paquetes menos por host, según las condiciones de la red.



### Técnicas de análisis de puertos

Esta sección documenta la docena de técnicas de escaneo de puertos compatibles con Nmap. De forma predeterminada, Nmap realiza un SYN Scan, aunque sustituye a un connect scan si el usuario no tiene los privilegios adecuados para enviar paquetes sin procesar (requiere acceso de root en Unix).

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

#### -sS

-sS análisis utilizando TCP SYN

El análisis SYN es la opción de análisis predeterminada y más popular por buenas razones. Se puede realizar rápidamente, escaneando miles de puertos por segundo en una red rápida no obstaculizada por firewalls restrictivos. También es relativamente discreto y sigiloso, ya que nunca completa las conexiones TCP. El escaneo SYN funciona contra cualquier pila TCP compatible en lugar de depender de la idiosincrasia de plataformas específicas como lo hacen los escaneos FIN / NULL / Xmas, Maimon e inactivos de Nmap. También permite una diferenciación clara y confiable entre los estados abierto, cerrado y filtrado.


#### -sT

-sT análisis utilizando TCP CONNECT

El escaneo de conexión TCP es el tipo de escaneo TCP predeterminado cuando el escaneo SYN no es una opción. Este es el caso cuando un usuario no tiene privilegios de paquetes sin procesar. En lugar de escribir paquetes sin procesar como hacen la mayoría de los otros tipos de escaneo, Nmap solicita al sistema operativo subyacente que establezca una conexión con la máquina y el puerto de destino emitiendo la llamada al sistema de conexión. Esta es la misma llamada al sistema de alto nivel que utilizan los navegadores web, los clientes P2P y la mayoría de las otras aplicaciones habilitadas para la red para establecer una conexión. Es parte de una interfaz de programación conocida como Berkeley Sockets API. En lugar de leer las respuestas de paquetes sin procesar, Nmap usa esta API para obtener información de estado en cada intento de conexión.

#### -sU

-sU análisis utilizando UDP

Si bien los servicios más populares en Internet se ejecutan sobre el protocolo TCP, los servicios UDP se implementan ampliamente. DNS, SNMP y DHCP (puertos registrados 53, 161/162 y 67/68) son tres de los más comunes. Debido a que el escaneo UDP es generalmente más lento y más difícil que TCP, algunos auditores de seguridad ignoran estos puertos. Esto es un error, ya que los servicios UDP explotables son bastante comunes y los atacantes ciertamente no ignoran todo el protocolo. Afortunadamente, Nmap puede ayudar a inventariar puertos UDP.

#### -sY

-sY análisis utilizando SCTP INIT

SCTP es una alternativa relativamente nueva a los protocolos TCP y UDP, que combina la mayoría de las características de TCP y UDP, y también agrega nuevas características como multi-homing y multi-streaming. Se usa principalmente para servicios relacionados con SS7 / SIGTRAN, pero también tiene el potencial de usarse para otras aplicaciones. El escaneo SCTP INIT es el equivalente SCTP de un escaneo TCP SYN. Se puede realizar rápidamente, escaneando miles de puertos por segundo en una red rápida no obstaculizada por firewalls restrictivos. Al igual que el análisis SYN, el análisis INIT es relativamente discreto y sigiloso, ya que nunca completa las asociaciones SCTP. También permite una diferenciación clara y confiable entre los estados abierto, cerrado y filtrado.

#### -sZ

-sZ utilizando COOKIE ECHO de SCTP

El escaneo SCTP COOKIE ECHO es un escaneo SCTP más avanzado. Aprovecha el hecho de que las implementaciones de SCTP deben eliminar silenciosamente los paquetes que contienen fragmentos COOKIE ECHO en los puertos abiertos, pero enviar un ABORT si el puerto está cerrado. La ventaja de este tipo de escaneo es que no es tan obvio un escaneo de puertos como un escaneo INIT. Además, puede haber conjuntos de reglas de firewall sin estado que bloqueen los fragmentos INIT, pero no los fragmentos COOKIE ECHO. No se deje engañar pensando que esto hará que un escaneo de puertos sea invisible; un buen IDS también podrá detectar escaneos SCTP COOKIE ECHO. La desventaja es que los escaneos SCTP COOKIE ECHO no pueden diferenciar entre puertos abiertos y filtrados, dejándolo con el estado abierto | filtrado en ambos casos.

#### -sO

-sO protocolo IP

El escaneo de protocolo IP le permite determinar qué protocolos IP (TCP, ICMP, IGMP, etc.) son compatibles con las máquinas de destino. Técnicamente, esto no es un escaneo de puertos, ya que recorre los números de protocolo IP en lugar de los números de puerto TCP o UDP. Sin embargo, todavía usa la opción -p para seleccionar números de protocolo escaneados, informa sus resultados dentro del formato de tabla de puertos normal e incluso usa el mismo motor de escaneo subyacente que los métodos de escaneo de puertos verdaderos. Así que está lo suficientemente cerca de un escaneo de puertos que pertenece aquí.

#### -sW 

-sW ventana TCP -sN

El escaneo de ventana es exactamente lo mismo que el escaneo ACK, excepto que aprovecha un detalle de implementación de ciertos sistemas para diferenciar los puertos abiertos de los cerrados, en lugar de imprimir siempre sin filtrar cuando se devuelve un RST. Para ello, examina el campo de ventana TCP de los paquetes RST devueltos. En algunos sistemas, los puertos abiertos usan un tamaño de ventana positivo (incluso para paquetes RST) mientras que los cerrados tienen una ventana cero. Entonces, en lugar de enumerar siempre un puerto como sin filtrar cuando recibe un RST, el escaneo de Windows enumera el puerto como abierto o cerrado si el valor de la ventana TCP en ese restablecimiento es positivo o cero, respectivamente.

#### –sF -sX NULL, FIN, XMAS

–sF -sX NULL, FIN, XMAS

Estos tres tipos de escaneo (incluso más son posibles con la opción --scanflags que se describe en la siguiente sección) aprovechan una laguna sutil en el RFC de TCP para diferenciar entre puertos abiertos y cerrados. La página 65 de RFC 793 dice que "si el estado del puerto [de destino] está CERRADO ... un segmento entrante que no contiene un RST hace que se envíe un RST en respuesta". Luego, la página siguiente analiza los paquetes enviados a puertos abiertos sin los bits SYN, RST o ACK configurados, indicando que: "es poco probable que llegue aquí, pero si lo hace, elimine el segmento y regrese".

Al escanear sistemas que cumplen con este texto RFC, cualquier paquete que no contenga bits SYN, RST o ACK dará como resultado un RST devuelto si el puerto está cerrado y no habrá respuesta si el puerto está abierto. Siempre que no se incluya ninguno de esos tres bits, cualquier combinación de los otros tres (FIN, PSH y URG) está bien.

#### –sA 

–sA TCP ACK

Este escaneo es diferente a los otros discutidos hasta ahora en que nunca determina puertos abiertos (o incluso abiertos | filtrados). Se utiliza para mapear conjuntos de reglas de firewall, determinando si tienen estado o no y qué puertos se filtran.



### Puertos a analizar y orden de análisis

Nmap ofrece opciones para especificar qué puertos se escanean y si el orden de escaneo es aleatorio o secuencial. De forma predeterminada, Nmap escanea los 1000 puertos más comunes para cada protocolo.

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

#### -T 

-T paranoid|sneaky|polite|normal|aggressive|insane (Set a timing template)

Estas plantillas le permiten al usuario especificar qué tan agresivo desea ser, mientras deja que Nmap elija los valores de tiempo exactos. Las plantillas también realizan algunos ajustes de velocidad menores para los que actualmente no existen opciones de control detalladas. Por ejemplo, -T4 prohíbe que el retardo de exploración dinámica exceda los 10 ms para los puertos TCP y -T5 limita ese valor a 5 ms. Las plantillas se pueden usar en combinación con controles detallados, y los controles detallados que especifique tendrán prioridad sobre la plantilla de tiempo predeterminada para ese parámetro. Recomiendo usar -T4 al escanear redes razonablemente modernas y confiables. Mantenga esa opción incluso cuando agregue controles detallados para que se beneficie de esas optimizaciones menores adicionales que habilita.

Puede especificarlos con la opción -T y su número (0–5) o su nombre. Los nombres de las plantillas son paranoico (0), furtivo (1), educado (2), normal (3), agresivo (4) y loco (5). Los dos primeros son para la evasión de IDS. El modo educado ralentiza el análisis para utilizar menos ancho de banda y menos recursos de la máquina de destino. El modo normal es el predeterminado y, por lo tanto, -T3 no hace nada. Las velocidades del modo agresivo aumentan asumiendo que se encuentra en una red razonablemente rápida y confiable. Finalmente, el modo loco asume que estás en una red extraordinariamente rápida o que estás dispuesto a sacrificar algo de precisión por velocidad.


#### --Min/Max-hostgroup 

--min-hostgroup <numhosts>; --max-hostgroup <numhosts> 

Nmap tiene la capacidad de escanear puertos o escanear versiones de múltiples hosts en paralelo. Nmap hace esto dividiendo el espacio de IP de destino en grupos y luego escaneando un grupo a la vez. En general, los grupos más grandes son más eficientes. La desventaja es que los resultados del host no se pueden proporcionar hasta que todo el grupo haya terminado. Entonces, si Nmap comenzó con un tamaño de grupo de 50, el usuario no recibiría ningún informe (excepto las actualizaciones que se ofrecen en modo detallado) hasta que se completen los primeros 50 hosts.


#### --Min/Max-parallelism

--min-parallelism <numprobes>; --max-parallelism <numprobes>

Estas opciones controlan el número total de sondas que pueden estar pendientes para un grupo de hosts. Se utilizan para escanear puertos y descubrir hosts. De forma predeterminada, Nmap calcula un paralelismo ideal en constante cambio basado en el rendimiento de la red. Si se descartan paquetes, Nmap se ralentiza y permite menos sondeos pendientes. El número de sonda ideal aumenta lentamente a medida que la red demuestra su valía. Estas opciones colocan límites mínimos o máximos en esa variable. De forma predeterminada, el paralelismo ideal puede reducirse a uno si la red resulta poco fiable y aumentar a varios cientos en perfectas condiciones.

### Detección de servicios y versiones

Apunte Nmap a una máquina remota y podría decirle que los puertos 25 / tcp, 80 / tcp y 53 / udp están abiertos. Utilizando su base de datos nmap-services de unos 2.200 servicios bien conocidos, Nmap informaría que esos puertos probablemente corresponden a un servidor de correo (SMTP), servidor web (HTTP) y servidor de nombres (DNS) respectivamente. Esta búsqueda suele ser precisa: la gran mayoría de los demonios que escuchan en el puerto TCP 25 son, de hecho, servidores de correo. Sin embargo, ¡no debes apostar tu seguridad a esto! La gente puede ejecutar servicios en puertos extraños.

```
-sV: detección de la versión de servicios
–all-ports no excluir puertos
–version-all probar cada exploración
–version-trace rastrear la actividad del análisis de versión
-O activar detección del S. Operativo
–fuzzy adivinar detección del SO
–max-os-tries establecer número máximo de intentos contra el sistema objetivo
```

#### -sV

Habilita la detección de versiones, como se discutió anteriormente. Alternativamente, puede usar -A, que habilita la detección de versiones, entre otras cosas.

#### –O 

Habilita la detección del sistema operativo, como se discutió anteriormente. Alternativamente, puede usar -A para habilitar la detección del sistema operativo junto con otras cosas.

#### -Fuzzy

Cuando Nmap no puede detectar una coincidencia perfecta de sistema operativo, a veces ofrece posibilidades similares. La coincidencia tiene que ser muy cercana para que Nmap haga esto por defecto. Cualquiera de estas opciones (equivalentes) hace que Nmap adivine de manera más agresiva. Nmap aún le dirá cuando se imprima una coincidencia imperfecta y mostrará su nivel de confianza (porcentaje) para cada conjetura.

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

#### -F

-f fragmentar paquetes

La opción -f hace que el análisis solicitado (incluidos los análisis de descubrimiento de host) utilice pequeños paquetes IP fragmentados. La idea es dividir el encabezado TCP en varios paquetes para que sea más difícil para los filtros de paquetes, los sistemas de detección de intrusos y otras molestias detectar lo que está haciendo.

##### -D

-D d1,d2 encubrir análisis con señuelos

Realiza un sondeo con señuelos. Esto hace creer que el/los equipo/s que utilice como señuelos están también haciendo un sondeo de la red. De esta manera sus IDS pueden llegar a informar de que se están realizando de 5 a 10 sondeos de puertos desde distintas direcciones IP, pero no sabrán qué dirección IP está realizando el análisis y cuáles son señuelos inocentes. Aunque esta técnica puede vencerse mediante el seguimiento del camino de los encaminadores, descarte de respuesta («response-dropping», N. del T.), y otros mecanismos activos, generalmente es una técnica efectiva para esconder su dirección IP.

#### -S

-S ip falsear dirección origen

Nmap puede que no sea capaz de determinar tu dirección IP en algunas ocasiones (Nmap se lo dirá si pasa). En esta situación, puede utilizar la opción -S con la dirección IP de la interfaz a través de la cual quieres enviar los paquetes.

Otro uso alternativo de esta opción es la de falsificar la dirección para que los objetivos del análisis piensen que algún otro los está sondeando


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

## Ejemplos de comandos utiles

#### Lea la lista de hosts / redes desde un archivo

`nmap -iL /ruta/prueba.txt`

#### Exclusión de hosts / redes

`nmap 192.168.1.0/24 - exclude 192.168.1.5,192.168.1.254`

#### Para saber si un host / red están protegidos por un firewall

`nmap -sA scanme.nmap.org`

#### Escanear un host si está protegido por el firewall

`nmap -PN scanme.nmap.org`

#### Escanear una red y averiguar qué servidores y dispositivos están en marcha

`nmap -sP 192.168.1.0/24`

#### Mostrar únicamente los puertos abiertos

`nmap -open scanme.nmap.org`

#### Muestra todos los paquetes enviados y recibidos

`nmap -packet-trace scanme.nmap.org`

#### ¿Cómo puedo escanear puertos específicos?

```
 nmap-p [puerto] hostName
 # # Escanear el puerto 80
 nmap-p 80 192.168 0,1 0,1

 # # Escanear el puerto TCP 80
 nmap -p T: 80 192.168 0,1 0,1

 # # Scan UDP puerto 53
 nmap -p U: 53 192.168 0,1 0,1

 # # Escanear dos puertos # #
 nmap -p 80, 443 192.168 0,1 0,1

 # # Escanea rangos de puertos # #
 nmap -p 80 -200 192.168 0,1 0,1

 # # Combinar todas las opciones # #
 nmap -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 192.168 0,1 0,1
 nmap -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 server1.cyberciti.biz
 nmap -v -sU -sT -p U: 53, 111, 137, T: 21 -25, 80, 139, 8080 192.168 0,1 0.254

 # # Analizar todos los puertos con comodín * # #
 nmap -p "*" 192.168 0,1 0,1

 # # Escanear puertos principales, es decir $ escanear puertos numéricos más comunes # #
 nmap - TOP-puertos 5 192.168 0,1 0,1
 nmap - top-ports 10 192.168 0,1 0,1
```
#### ¿Cómo puedo detectar el sistema operativo remoto?

```
 nmap -O 192.168 0,1 0,1
 nmap -O -osscan-Supongo 192.168 0,1 0,1
 nmap -v -O -osscan-Supongo 192.168 0,1 0,1
```

#### Escanear un host que utiliza TCP ACK (PA) y TCP Syn (PS) de ping

```
 nmap -PS 192.168.1.1
 nmap -PS 80,21,443 192.168.1.1
 nmap -PA 192.168.1.1
 nmap -PA 80,21,200-512 192.168.1.1
```

#### ¿Cómo puedo detectar servicios remotos (servidor / daemon) números de versión?

```
nmap-sV 192.168.1.1
```

#### Escanear un host para servicios UDP

`nmap -sU scanme.nmap.org`

#### Digitalizar un firewall para la debilidad en la seguridad

```
# # TCP Null Scan para engañar a un servidor de seguridad para generar una respuesta # #
 # # No establece ningún bit (encabezado TCP bandera es 0) # #
 nmap -sN scanme.nmap.org

 # # TCP Fin scan para comprobar firewall # #
 # # Establece sólo el bit FIN TCP # #
 nmap -sF scanme.nmap.org

 # # TCP Xmas escaneado para comprobar firewall # #
 # # Establece el FIN, PSH, URG y banderas, encendiendo el paquete como un árbol de Navidad # #
 nmap -sX scanme.nmap.org
```

#### Escaneo completo de puertos TCP usando detección de versión de servicio.








## Nmap Scripting Engine

Despues de ver las opciones básicas que esta herramienta nos ofrece, ahora vamos a enfocarnos en los scripts que tenemos para mejorar nuestros escaneos.

NSE tiene una implementación compleja para la eficiencia, es sorprendentemente fácil de usar. Simplemente especifique -sC para habilitar los scripts más comunes. O especifique la opción --script para elegir sus propios scripts para ejecutar proporcionando categorías, nombres de archivos de script o el nombre de directorios llenos de scripts que desea ejecutar. Puede personalizar algunos scripts proporcionándoles argumentos a través de las opciones --script-args y --script-args-file. --Script-help muestra una descripción de lo que hace cada script seleccionado. Las dos opciones restantes, --script-trace y --script-updatedb, generalmente solo se usan para la depuración y el desarrollo de scripts. El análisis de secuencias de comandos también se incluye como parte de la opción -A (análisis agresivo)

El escaneo de scripts se realiza normalmente en combinación con un escaneo de puertos, porque los scripts pueden ejecutarse o no, dependiendo de los estados de los puertos encontrados por el escaneo. Con la opción -sn es posible ejecutar un escaneo de script sin un escaneo de puerto, solo descubrimiento de host. En este caso, solo se podrán ejecutar los scripts de host. Para ejecutar un escaneo de script sin un descubrimiento de host ni un escaneo de puerto, use las opciones -Pn -sn junto con -sC o --script. Se asumirá que todos los hosts están activos y aún así solo se ejecutarán los scripts de host. Esta técnica es útil para scripts como whois-ip que solo usan la dirección del sistema remoto y no requieren que esté activo.

Estos se clasifican en las siguientes categorias:

```
Auth: ejecuta todos sus scripts disponibles para autenticación

Default: ejecuta los scripts básicos por defecto de la herramienta

Discovery: recupera información del target o víctima

External: script para utilizar recursos externos

Intrusive: utiliza scripts que son considerados intrusivos para la víctima o target

Malware: revisa si hay conexiones abiertas por códigos maliciosos o backdoors (puertas traseras)

Safe: ejecuta scripts que no son intrusivos

Vuln: descubre las vulnerabilidades más conocidas

All: ejecuta absolutamente todos los scripts con extensión NSE disponibles
```

Para este escrito vamos a analizar el uso de algunos de estos scripts, en total son 604 y los pueden encontrar aqui: https://nmap.org/nsedoc/

Ejemplos de scripts por categorias:









