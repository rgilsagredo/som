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

## Config de red

## ordenes avanzadas