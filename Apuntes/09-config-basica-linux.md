# ABC de Linux
Cosas básicas para aprender a gestionar un OS de kernel Linux


## Interfaces de usuario
Son los medios que tiene un usuario de un OS de interactuar con el OS
y controlar sus funciones. Las tareas típicas que incluyen estas interfaces son
lanzar programas, gestión de fiicheros, configurar el sistema...

### Interfaz CLI
CLI (Command Line Interface), se interactúa con el OS por comandos de texto.
Es la típica de sistemas Linux

### Interfaz GUI
GUI (Graphical User Interface) usa elementos visuales como ventanas,
iconos, menús desplegables, botones, barras de desplazamiento...
Es la clásica de Windows, pero algunas distros de Linux, como Ubuntu, 
tienen también GUI.
Incluso para la propia distribución Ubuntu hay distintas versiones/variaciones
que vienen con distintos entornos gráficos predeterminados
(Ubuntu - GNOME, Xubuntu - XFCE, Lubuntu - LDE, Kubuntu - KDE)

REalemnte estas GUI son lo que se llama Entorno de Escritorio, que proporcionan
un modo gráfico de gestionar prácticamente todo.

## Antes de usar el sistema
Antes de poder usar un OS, te obliga a hacer un `login`, esto es, 
proporcionar un user+pass. Es realmente lo que hacemos al "meternos" al
ordenador.

Si estamos en un sistema con entorno gráfico, lo que hay que hacer es buscar
un emulador de terminal (en el entorno gráfico de GNOME, se llama 
gnome-terminal, que es la terminal que hemos estado usando; pero hay otras
como xterm, lxterminal...).

Si estamos en un OS sin entorno gráfico, tras el login se nos da
acceso directamente al intérprete de comandos (como era el caso de Alpine)

## Uso de CLI
La CLi o interfaz de texto lo que hacen es ejecutar un intérprete de comandos,
en el caso de Linux suele ser `bash`. Su objetivo es esperar a que el usuario
meta una orden y ejecutarla.

La CLI suele presentar una linea tipo:

```console
user@machine:/current-location$
```

que ya dice cierta info: user es el usuario que está usando el OS, 
machine es el nombre de la máquina, /current-location es el directorio
actual donde estamos; por defecto nos suele meter en `/home/user`, es decir,
en el directorio reservado a archivos/datos del usuario, que se representa 
como `~`. Además, cuando el usuario con el que estamos loggeados es el admin
del sistema, root, el símbolo no es `$` si no `#`.

Todo esto se llama `prompt`, y queda a la espera de una orden.

### Dar órdenes
Una orden esencialmente es decir qué programa queremos usar. En un entorno 
gráfico sería "hacer doble click" en un programa. En una (pseudo)termnial
es decir el nombre del programa

Ejemplo: hay un programa que se llama `whoami` que dice que usuario está 
loggeado. No ncesita más. Esdto sería lo más simple.

Otros programas necesitarán más info para funcionar. Por ejemplo, el programa
`echo` muestra por pantalla los **argumentos** que se le proporcionen,
por si mismo no hace nada `echo hola`.

Los programas suelen aceptar también `parámetros` u opciones que a veces
son necesarias para su correcto funcionamiento pero la mayoría de veces
son para cambiar su comportameinto. Por ejemplo, si al comando `echo` le
añado el parámetro `-e` entonces sabe interpretar cosas como
saltos de linea:

```console
echo "hola\ncaracola"
echo -e "hola\ncaracola"
```

En general las órdenes por CLI tienen forma
```console
<programa> [<argumentos>]
```

Los argumentos (como se indican) en ppio dependen de cada programa, pero
en general todos siguen un estandar (POSIX).

Ejemplo de uso de un programa y sus variaciones: `ls`. El objetivo de
este programa es mostrar los contenidos de un directorio.

La forma más básica es simplemente `ls`, y entiende que nos referimos al
directorio donde estemos actualemnte.

Si quieres decirle al  programa que quieres ver otro directorio, le dices
la ruta al directorio, bien de manera absoluta o de manera relativa:
`ls /dev`.

Si quiero alterar el comportamiento del programa, tengo las opciones,
que suelen empezar por `-`. Por ejemplo, tengo la opción `-a` para
el programa `ls` que lo que le dice es ue muestre todo, incluido ficheros
ocultos `ls -a /dev`.

Esta opción se llama opción corta, porque es solo una letra. Las opciones cortas
suelen tener una equivalente larga, que se indica con `--`. En este caso, 
es lo mismo que `ls --all /dev`.

Se pueden añadir varias opciones a la vez; pro ejemplo,
`ls -a -l` = `ls -l -a` = `ls -al` hacen todos lo mismo. `-l` lo que hace es
mosrar el output con más detalles. Las opcines largas no 
permiten fusión. 

Algunas oipciiones necesitan a su vez parámetros. Por ejemplo, la opción `-w`
nos permite indicar el ancho de la respuesta, pero tenemos que decir ese
ancho: `ls -w 80` = `ls -w80`

En las opciones largas se suele escribir `ls --width=80`

Cada programa tiene sus cosas y no hace falta saberlas (todas), porque suelen
incluir una ayuda cn la opción `-h` o `--help`. 

También existe un manual al que se accede con `man ls` (por ejemplo).

### Comandos internos y externos
Cuando ejecutas una orden via CLI, puede ser interna (vienen con la shell)
o externo (es un programa que está en algún sitio). Los comandos
internos los podemos averiguar con `man builtins` (por ejemplo, pwd, cd, echo)

Los comandos externos se pueden encontrar en el disco duro con `which`

Para saber si es programa interno o externo puede funcionar usar `which`.
Si no devuelve nada, es interno. Pero puede que existan una evrsión interna 
y una externa; para averiguar qué es, podemos usar `type -a programa`;
en caso de ser orden interna y externa, la que s eejecuta por defecto es
la interna.

### Salir de la CLI
tienes varias opciones:
- ctrl+d o escribir exit
- logout -- cierra la sesión abierta con login
- poweroff -- apaga el equipo
- reboot -- reinicia el sistema

## Sistema de archivos
Es quien se encarga de estructurar y gestionar la info almacenada en disco.
Esa info se guarda en ficheros/archivos, que  a su vez se organizan en
directorios/carpetas.

Los ficheros son los que al final contienen algún tipo de información (una foto,
un documento...), los directorios son ficheros que "almacenan" otros ficheros.

Los archivos se organizan en modo árbol, con un directorio raiz (el origen),
que se va ramificando.

El disco lo que contiene son 0/1 (hay electricidad o no). El sistema de archivos
(el file system) es el software que traduce esos 0/1 a cosas que podemos
entender. Además les añade ciertos atributos (es decirr, no solo es información,
si no que tengo metainformación), como puede ser propietario, permisos,
fechas (creaión, modificacion...).

Los FS más usados en kernel linux son ext4, brtfs y xfs. ext4 es el nativo
(el que viene con) linux

### Estructura
Realmente cda distro de linux va a tener su propia estructura, pero todas más 
o menos se parecen y siguen un estándar llamado FHS (Filesystem Hierarchy 
Standard).

Podemos ver la estructura de arbol de un FS con el comando 
```console
tree -dL 1 /
```

Si no tenemos instalado el programa tree, podemos instalarlo con 
```console
sudo apt install tree
```

Las opciones que estamos pasando al programa son -d -> solo muestra directorios
y -L 1 -> baja solo 1 nivel respecto al directorio que te digo, que en este
caso es el directorio raíz /

![tree](./images/config-basica/tree.jpg "tree")

Lo que vemos son los directorios bajo la raiz, y esos que apuntan son
symlinks, enlaces simbólicos; eso es que se directorio realmente "no tiene
nada", solo apunta a otro directorio (es un acceso directo).

Hay que entnder qué contiene cada directorio, y algunos de sus subdirectorios:

#### /bin
de binarios; contiene programas básicos para el funcionamiento del sistema
que pueden usar todos los usuarios

#### /boot
los archivos básicos para el arranque del sistema (gestor de arranque, 
el kernel...)

#### /dev
contiene archivos de bloques y caractrers. Un archivo de caracteres gestiona
el I/O secuencialemnte (tty "es" la consola, "null" es la "papelera"..).
Los archivos de bloques gestionan el I/O en bloques o por lotes; los ejemplos
son los dispositivos de almacenamiento sda, sdb, o sr0 (un CD). Son los
ficheros que permiten el interactuar con el hardware.

#### /etc
fichero de configuración global de los programas del sistema. Luego, cada
usuario puede tener sus ficheros de configuración específicos (personalizaciones)

#### /lib
de libraries o bibliotecas. Realmente son más progrtamas, pero son programas
de apoyo/compartidos por otros programas que se alojan en /bin o /sbin o
incluso las puede usar el kernel

#### /media
son los puntos de montaje (un punto de montaje es cómo voy a meterme a un
filesystem concreto) de los dispositivos extraibles (CDs, USBs..)

#### /mnt
sirve para montar manualmente algún FS (poder acceder a ese filesystem)

#### /opt
optional, es para que se instale software que no viene con el OS. Suele
copiar la jerarquíe de ficheros del software base, es decir, suele tener 
una carpeta /bin, /lib... para que los programas puedan ir dejando
los ficheros que van a nacesitar.

#### /proc
procesos; es el sitio que se reserva el OS para informarte de sus procesos
y otras cosas; por ejemplo, la info sobre la CPU está ahí y podemos verla
con 
```console
cat /proc/cpuinfo
```
también es interesante meminfo; información sobre la RAM

La utilidad de proc es que es una interfaz de monitoreo del sistema,
como pueda ser el administrador de tareas de windows.

Los ficheros de /proc son virtuales, quiere decir que no existen físicamente
en disco, si no que cuando quiero acceder a la info que tienen se generan
en RAM y luego "desaparecen"

#### /root
es el "home" del usuario admin del sistema; es donde guarda sus cosas.
En general lo que debería guardar ahí son ficheros de config, scripts,
copias de seguridad, documentación sobre la admin, logs...

#### /run
contiene datso temporales y necesarios de procesos que se están ejecutando
en el sistema (los procesos en segundo plano se llaman deamons)

#### /sbin
similar a bin, contiene programas básicos para hacer cosas, pero en este caso
se trata de los programas básicos para administrar cosas del sistema; es
decir, es lo que puede hacer root y otros superusuarios. Ejemplos: fdisk
para particionados, ifconfig para cofiguraciones de red, mount/umonut
para montajes de FS, reboot/poweroff para reinciar/apagar

#### /srv
served; si el equipo es un servidor, lo que sirve está aquí

#### /sys
simi lar a proc, otra manera de sacar datos del sistema. Más orientado al 
hardware, miestras que /proc está más orientado a procesos

#### /tmp
temporal; para ficheros temporales. No pongas nada importante aquí porque
seguramente se borrará

#### /usr
no es usuarios; es Unix System Resources, es decir, recursos que necesita el
sistema. Si vemos la imagen de antes, ralmente es ahí donde se almacenan un
montón de programas básicos

#### /var
variables, fichero que contienen datos que cambian normalmente miestras se
ejecutan procesos (por ejemplo, una foto es un documento que no debería
cambiar, no va aquí; los ficheros que componen una base de datos sí
cambian habitualmente, o los logs del sistema, o info sobre estaos de
procesos...)


Es normal usar varios FS a la vez (ya hicimos eso en la instalación,
creando varias particiones con sus FS para contener cosas). En los sistemas
UNIX existe un único árbol de directorios, y al resto de FS se accede montándolos
en algún directorio del árbol principal. Ejemplo: datos de usuario se montan
en /home, los dispositivos extraibles en /media

## hacer la comparativa con windows
como se organoiza win, qué esperar en cada cosa

### idea ejerccio
crear usuarios, tocar la config de la consola y personalizarla

## Config de red

## ordenes avanzadas