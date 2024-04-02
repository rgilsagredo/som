# Gestión de procesos en Linux
Un proceso en un programa en ejecución. La diferencia con un programa es que
el programa es "siemrpe el mismo", pero un programa puede originar muchos
procesos distintos (incluso a la vez).

Los procesos tienen un contexto, que los diferencia del mismo programa
ejecutandose a la vez (o no) en diferentes momentos. El contexto incluye
conpectos como el dueño del proceso, los permisos, si tiene un "padre"...

Linux es multitarea, es decir, que puede ejecutar varios procesos a la vez.

Los procesos tienen varias características asociados a ellos, y la más 
importante es el PID (Process IDentifier). Es único para cada proceso.

Otro concepto importante es el de **Deamon** o demonio; se trata de un
proceso en segundo plano, es decir, que se está ejecutando sin intervención
activa del usuario.

Otro concepto importante aosicado a cada proceso es la prioridad, que es 
la prefernecia que v a dar el sistema a la finalización de la ejecución de
ese proceso. Se representa con un número desde -20 (máxima) a 19 (mínima),
siendo 0 la prioridad "normal"

## Sacar info
El comando que buscamos en `ps`, pero en genral necesita varias opciones
para que la info sea relvante.

```bash
ps -e
```
muestra todos los procesos

```bash
ps -a
```
muestra todos los procesos asociados a una terminal. Entre la info que 
nos suelta, TTY es la terminal que ejecuta el proceso, y TIME es el tiempo
de ejecución usado y CMD es el nombre del programa que inicia el proceso.
Podemos sacar aún más info con la opción `-f`, aparecen PPID (PID del proceso
padre) y otros que ahora no me importan.

con la opción `-H` podemos ver la jerarquía de procesos, hace lo mismo y mejor
la opción `--forest`.

Podemos también filtrar info (no siempre quieres ver todos los procesos
y buscar por ahí). Podemos filtrar por usuario que lo lanza, con
`ps -u raul` (nota: podemos hacer `ps -l -u raul` para ver el UID entre
las columnas y estar seguros de que es el correcto).

Podemos filtrar por nombre del proceso: `ps -C systemd` o por PID con
`ps -p 1000` o por PPID con `ps --ppid 1234`, pudiendo combinar estas
opcioes de filtrado; pero ojo, que el filtrado no es "doble"
(`ps -C nombre-programa -u Piter` muestra los procesos que se llamen
`nombre-programa` y los procesos del usuario Piter). Para filtrar combinado
hay que usar `pgrep`.

Podemos también elegir qué queremos ver con la opción -o:

```bash
ps -eo user,pid,ppid,comm --forest
```

y podemos ordenar los resultados con `--sort`:

```bash
ps -eo user,pid,ppid,comm --sort ppid
```

Otra manera de visualizar procesos es usar `pstree`. Esto nos muestra por ejemplo
que hay un proceso raiz que es systemd.

Una manera más potente de buscar procesos (pero más difícil de usar) es
`pgrep`, que usa REGEXP para filtrar cosas. Lo que nos imprta es que pgrep
devuelve PIDs de procesos, atendiendo a distintos criterios de búsqueda, como
puede ser el nombre `pgrep bash`, el usuario `pgrep -u raul` o el PPID
`pgrep -P 1234`. Lo bueno de pgrep en que permite acumular filtros por ejemplo

```bash
pgrep -u raul,root bash
```

También tienes la opción `-c` para contar el número de procesos asociados a 
la búsqueda, en vez de ver los PIDs.

`top` (y htop pero hay que instalarlo) es una interfaz para ver los 
procesos en tiempo real (como el adminitrador de tareas de windows).

`uptime` muestra tiempo desde arranque, usuarios conectados y la carga media
del equipo en el último minuto, últimos 10 minutos y últmios 15 minutos.

## Manipular órdenes
Al dar un orden al sistema, se ejecuta un programa así que se crea un proceso.
Podemos manipular el comportamiento de cómo se va a ejecutar ese proceso.

### nice
Cambia la prioridad de un proceso, qe por defecto es 0 (-20 es máxima, 19 mínima).
Si no eres admin, solo puedes bjar la prioridad (dar númros positivos).

Para usar nice, `nice -n7 nombre-del-programa` es decir, antepones nice y
la prioridad que quieres dar y luego ejecutas el programa (con su nombre, 
argumentos, opciones...). PRO TIP: procesos costosos (por nivel de cómputo)
pero que no son prioritarios hay que darles prioridad baja (por ejemplo transformar
un video de calidad tal a calidad cual), pero procesos asociados a programas
críticos (una basa de datos) hay que darles prioridad máxima

### renice
modifica la priorridad de un proceso que ya se está ejecutando

```bash
renice -n-4 -p 1234
```

donde 1234 es el PID del proceso que queremos cambiar la prioridad (a -4)

### kill
Manda señales a los procesos (normalmente matarlos, ie, terminarlos). Tienes
muchas opciones que puedes ver con `kill -L`, se usa en general con
```bash
kill -n PID1 PID2 ...
```

donde n es el número de  la señal que quieres mandar.

#### SIGTERM (15)
Es una manera de decir al proceso que intente apagarse liberando recusos
abiertos que esté usando, todo de manera muy polite.

#### SIGHUP (1)
Sobre un proceso en primer plano, es esencialmente igual que SIGTERM.
Pero en un daemon, esencialmente es que se vuelva a leer la config y continúe
corriedno.

#### SIGINT (2)
Equivalente a ctrl+c, es decir, para el proceso de una. Pero el proceso
puede ignorarlo a veces.

#### SIGKILL (9)
Como SIGINT pero el programa no puede ignorarlo, esd ecir, se carga el proceso.

La idea será siempre primero SIGTERM y si eso no hace nada, SIGKILL

#### SIGTSTP (20)
Suspende un proceso (no se ejecutar hasta que le digamos que vuelva a
ejecutarse)

#### SIGSTOP (19)
Como SIGTSPS pero más agresiva; usar esta solo si la 20 te ignora

#### SIGCONT (18)
Resucita un proceso suspendido.

### pkill y killall
pkill es como kill (se usa igual, con las mismas señales) pero permite meter
la info como pgrep, es decir, puedo poner el nombre del proceso y filtros
acumulativos.

killall cancela procesos (con señal 15 por defecto)  identificándolos por nombre

### Como funcionan las cosas
Cuando doy una orden por terminal, la shell espera a que se acabe la orden
para liberar el prompt. Es porque se ejecuta en primer plano. Si quiero que se 
ejecute en segundo plano, tengo que hacer `orden &`.

Además, para una orden en 1er plano, crtl+c es igual que kill, y crlt+z es
pararla. Probar a ejecutar esto:

```bash
(for i in {1..10}; do sleep 1; echo $i; done)
```

y suspenderlo.

### jobs
dice los trabajos activos en la shell actual. Si vemos ordenes supendidas,
podemos hacer `fg %n` donde n es el número del trabajo (que me ha dicho jobs)
para traerla de nuevo a primer plano y que siga ejecutándose.

Para llevar una orden a 2º plano, hacemos `bg %n`, o como antes, `orden &`


## Privilegios 
Los procesos pueden tener 2 niveles de privilegio: kernel y usuario.
La diferencia principal es el acceso a recursos del sistema y las acciones
que puedan realizar.

Los procesos privilegiados pueden realizar tareas que afecten el correcto
funcionamiento del OS: crear/borrar usuarios, instalar software, modificar
ficheros del OS...

Los procesos de usuario están restringidos a los privilegios que tenga el 
usuario, y están limitados también por las restricciones que pueda imponer
el OS

Para que usuarios no root (o no admin) puedan lanzar procesos privilegiados,
hay 2 estrategias:

### setuid
Como recordatorio, setuid es un permiso especial que se puede conceder a 
ficheros para que se ejecuten con los permisos del propietario del fichero

Pordemos ver con 

```bash
stat $(which su)
```

que el programa `su` lo tiene activado y el propietario es root, es decir,
cualquier usuario que use este programa lo hará como si fuese root, se 
puede replicar esta estrategia con cualquier otro programa

Otra estrategia es usar el propio comando `su`, que permite ejecutar 
cualquier otro programa. Pero requiere saber la password del admin.

Una variante de `su` es `sudo`, que es más configurable: puedes pedir passwd
de user, admin o ninguna y puedes restringir para cada usuario qué programas
puede lanzar como privilegiado

La estrategia de su/sudo tiene el problema de que se viola (en ppio) el ppio de 
minimo privilegio, que es que un usuario debe tener los privilegios mínimos
para hacer lo que tiene que hacer

### capacities 
No me voy a meter mucho, pero esencialmente consiste en que en lugar de dar
omnipotencia (actuar como root) se dividen los privilegios en miniprivielgios,
y solo se conceden los miniprivilegios necesarios al proceso para que haga
loq ue necesite

### sudo
Ya que hemos visto y usado bastante sudo, lo vemos un poco más en profundidad.
Está pensado para usarse puntualmente: `sudo comando` pero recuerda la
password durante 5 minutos (para que no se tenga que estar pidiendo la
pass continuamete que puede ser toedioso)

Por un lado está bien que no puede abrir una sesión interactiva de una shell
como root, pero por otro lado i te pillan la pass de un usuario sudoer, 
a efectos te han robado al admin.

La config de sudo está en 

```bash
/etc/sudoers
/etc/sudoers.d/
```

sudoers es el fichero principal pero cualquier fichero que esté en la carpeta
sudoers.d también se considera de config; es útil para distribuir configs
específicas y sobre tod para no tocar ficheros que pueden romper cosas

Para tocar estos ficheros puedes hacerlo conun editor de textos pero mejor usar
el programa diseñado para ello que es `visudo`:

```bash
visudo # esto abre /etc/sudoers
visudo -f /etc/sudoers.d/fichero-de-config # esto abre un fichero concreto 
```

Escribir en estos ficheros requiere saber cómo se debe escribir en ellos.

#### alias
son nombres que damos a conjuntos de usuarios/grupos, máquinas u órdenes.

##### Cmnd_Alias
Define alias para un comando o grupo de comandos, por ejemplo:

```bash
Cmnd_Alias BACKUP_CMDS = /bin/tar, /usr/bin/rsync, /bin/cp
```

define esos comandos bajo el alias BACKUP_CMND.

Se puede usar también una expresión tipo `Cmnd_Alias NETEXEC = /sbin/if* eth*`
que permite usar un comando que empiece por if pero solo si luego hay
un argumentno que empiece por eth

Se pueden también usar alias en alias tipo: 

```bash
Cmnd_Alias IFUPDOWN = /sbin/if* eth*
Cmnd_Alias NETEXEC = IFUPDOWN, /sbin/route
```
Y se pueden denegar comandos que no queremos que se ejecuten:

```bash
Cmnd_Alias IFUPDOWN = /sbin/if*, !/sbin/ifconfig
```

Exite un alias predefinido, `ALL`, que es "ejecutar cualquier cosa"

##### User_alias
Define grupos de usuarios y grupos:

```bash
User_Alias COLEGUILLAS = pepe, paco, %amigospepe, %amigospaco
User_Alias CASTA = ALL, !apestado, !%parias
```

el % quiere decir que estoy incluyendo un grupo, ! y ALL como antes

##### Host_alias
define las maquinas que podrán ejecutar `sudo`
```bash
Host_Alias LAN = 172.22.0.0/16, 192.168.0.0/255.255.255.0, 192.168.1.1
```
ALL también existe aquí

#### Reglas de acceso
Es cómo definir qué puede hacer cada usuario, el formato es

```bash
<usuario> <maquina> = [(<poderdante>)] <comando1>[, <comando2>, ...]
```
- usuario es un usuario, grupo o alias. Es a quien se dan los privilegios
- máquina es desde donde se puede hacer el sudo
- poderante es el usuario del que se toman los privilegios. Si no se dice, es 
    root
- lo último son los comandos o alias

Ejemplo:
```bash
COLEGUILLAS ALL = (root) NOPASSWD: NETEXEC
```
El palabro NOPASSWD es para que no se pida password

Por ejemplo esto:
```bash
# /etc/sudoers.d/updaters
# Command alias
Cmnd_Alias APT_UPDATE = /usr/bin/apt update
Cmnd_Alias APT_UPGRADE = /usr/bin/apt upgrade
Cmnd_Alias APT_UPDATE_SYS = APT_UPDATE, APT_UPGRADE

# User alias
User_Alias SYS_UPDATERS = raul

# Access rule
SYS_UPDATERS ALL = (root) NOPASSWD: APT_UPDATE_SYS
```
Hace que podamos ejecutar el sistema sin que nos pida password