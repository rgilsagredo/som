# Uso avanzado de la shell
Necesitaremos usar la shell mejor si queremos ser eefectivo en el trabajo;
por ejemplo, intentar buscar info de ficheros de registro sin manejarse
bien con las shell es in infierno.

## Concatenar órdenes
De momento cuando estamos lanzando órdenes a la shell lo hacemos de
1 en 1 poniendo el nombre del programa, por ejemplo

```bash
cp -r este-fichero /home/user/dir/esta-copia
```

Muchas veces querrás dar más de una orden en ligar de ir de 1 en 1.
La primera manera básica de hacer eso es usar el `;`; a efectos,
lo que hace es ejecutar la primera orden, y falle o no, ejecutar 
la segunda después. Ejemplos:

```bash
echo "hola" ; echo "adios"
cp ; echo "lo anterior ha fallado pero yo me ejecuto"
```

Otra cosa que querremos hacer es dar órdenes largas, que son incómodas de ver 
en la shell. Para que se vea mejor, podemos usar el `\` para que la shell
entienda que es una misma orden escrita en distintas lineas:

```bash
sudo adduser manolo --gecos "Manolo el del bombo" \
> --shell /bin/sh
```

### Operadores lógicos
Tenemos 2 operadores lógicos, `y == &&` y `o == ||`, que también sirven
para concatener órdenes. El operador `&&` ejecuta la segunda orden solo
si la primera tiene éxito; el operador `||` ejecuta la segunda orden solo
si la primera falla.

Un ejemplo clásico del operador `&&` es la actualización del sistema:

```bash
sudo apt update && sudo apt upgrade
```

Para ver bien cómo funcionan estos operadores, tenemos un par de 
comandos: `true` y `false`. `true` siempre tiene éxito, `false` siempre
falla. Entonces si hacemos:

```bash
true && echo "el primero comando ha funcionado"
false && echo "no voy a salir por pantalla"
true || echo "no voy a salir por pantalla"
false || echo "el primero comando ha fallado"
```

vemos exactamente qué está pasando.

### Precedencia
Al evaluar las expresiones que le metemos a la shell, la evaluación
va siempre de izda a dcha y ambos operadores lógicos tienen la 
misma precedencia. Ver este ejemplo:

```bash
true || echo "Uno" && echo "Dos"
```

Sería equivalente a:

```bash
(true || echo "Uno") && echo "Dos"
```

#### Un poco de I/O
Supongo que lo veremos más adelante en más profundidad, pero
en la shell hay un par de conceptos que son la entrada estandar (stdin)
y la salida estandar (stdout); en realidad son 2 chorradas, la stdin es
el teclado, es el sitio desde donde la shell espera un comando;
la stdout es la "pantalla", es donde la shell espera devolver la info.

Estas entradas se pueden modificar, usando (entre otras cosas) las *piplines*
(tuberías). Es una redirección de salida junto a una de entrada.

Para ver esto con un ejemplo: quiero saber la antepenúltima linea del
fichero de usuarios `/etc/passwd`.

El programa `tail` permite mostrar el final de un fichero, por ejemplo,

```bash
tail -n3 /etc/passwd
```

mostrará las 3 últimas lineas del fichero (por stdout), 
en particular tenemos ahí la linea que queremos, y otras 2 lineas.

Entonces querríamos, sobre esta ``stdout``, sacar solo la primera linea.
Si fuese un fichero, harías `head -n1 nombre-del-fichero`; pero para
no crear un fichero que luego hay que borrar, puedes usar un pipline
que a lo que se traduce es "usa esta stdout como stdin en el siguiente comando":

```bash
tail -n3 /etc/passwd | head -n1
```

El operador `|` es la pipeline; y solo lo meto aquí de momento porque
tiene más precedencia que `&&` y `||`

### Alterar precedencia
Es igual que en matemáticas, puedo usar paréntesis para decir que quiero que se
ejecuta primero. No es lo mismo:

$$
1+2*3=7
$$

que 

$$
(1+2)*3=9
$$

Igualmente, no es lo mismo

```bash
true || ( echo "Uno" && echo "Dos" )
```

que 

```bash
true ||  echo "Uno" && echo "Dos" 
```

Pero ojo, usar paréntesis le dice a la shell que debe ejecutar lo que
está entre paréntesis en una subshell. Si quiero que todo se ejecute en la
misma shell hay que usar `{}`

Para ver esto, podemos hacer:

```bash
echo $BASHPID ; ( echo $BASHPID )
```

y 

```bash
echo $BASHPID ; { echo $BASHPID }
```

## Entender la shell
En general vamos a tratar solo `bash` y ninguna otra shell.
Esto va de entender  qué entiende ``bash`` cuando le decimos cosas.

### Proteger caracteres espaciales
Hay ciertos carateres que tienen un significado especial si se los decimos
a bash, por ejemplo `*` suele significar "todo"; si hacemos

```bash
echo *
```

no printea por stdout `*`; si queremos que printee `*`, tenemos que 
protegerlo, hay 3 maneras:

- usar `\` (escape)
- usar `""` (comillas dobles)
- usar `''` (comillas simples)

Ojo que no son equivalentes.

### Espacios (o tabs)
Normalmente los espacios sirven a bash para que entienda que le estamos
pasando argumentos distintos (al programa que sea). Ya lo hemos hecho varias 
veces:

```bash
ls --all --directory
```

cada argumento se separa y bash sabe que son argumentos distintos.
Pero a veces queremos que bash interprete literalmente un espacio,
para ello "escapamos" el espacio:

```bash
cat x y
cat x\ y
```

### historial
bash almancena los comandos que le has pasado, miestras está corriendo, 
en memoria, y cuando sales en el fichero `~\.bash_history`. Ya se habrá visto
que la manera más rápida de repetir un comando es con las flechas
irlo buscando; si quiero saber todos los comandos, tengo el programa

```bash
history
```

Pero tengo más shortcuts para ir más rápido:

- `!!` - reejecuta la última orden
- `!N` - reejecuta la orden N
- `!-M` - reejecuta la orden M contando desde abajo
- `!se` - reejecuta la última orden que empezaba por `se`

Puede ser que quieras repetir una orden pero cambiar algún argumento
(los comando anteriores simplemente repiten la orden). Para ello tienes
la sintaxis `:`.

Para ver cóm funciona, crear 3 ficheros y meter en cada uno de ellos
simplemente su nombre. Hacerles `cat` a los 3, debería mostrrnos sus nombres.

Si ahora uso `!!:1` lo que digo es que repitas el comando anterior (cat)
pero usando solo el primer argumento (el número se conrresponde con el argumento
a usar).

Si digo `!!:1-2` quiero un rango de argumentos; puedes dejar el final "abierto"
tipo `!!:2-`, para que vaya hasta el final (desde el 2); idem el ppio.

### Comodines
Ya vimos algo, pero es muy últil, lo repetimos. Permiten hacer sustituciones en
los nombres de los fcheros. Tienes:

#### ?
significa exactamente 1 caratcer:

```bash
ls -d /s??
```

**NOTA**: la interpretación de `??` la hace bash, no ls

#### *
significa 0 o más caracteres:

```bash
ls -d /s*
```

#### []
Permite indicar 1 o más carateres, de los cuales uno debe estar presente en el
nombre:

```bash
ls -d /s[by]*
```

Puedes meter un `^` antes de los carateres para decir que NO son esos: `[^by]`.
Puedes meter rangos con `-`: `[a-z]`

#### {} expandibles
puedes hacer esto:

```bash
echo "prefijo-{1,2,3}-sufijo"
```

y te crea todas las cadenas. Puesdes usar `..` para referirte a lso acarateres
ascii: `echo {A..a}`^, e incluso dar un paso ``echo {A..a..2}``

Se pueden combinar con los anteriores:

```bash
ls -d /{s*,li*}
```

#### ?(lista-patrones)
los patrones aparecen 1 o 0 veces:
```bash
ls *?(.tar).gz
ls -r ./raul/?(f[0-9])
```

#### +(lista-patrones)
los patrones aparecen 1 o más veces
```bash
ls *+(.tar).gz
```

#### *(lista-patrones)
los patrones aparecen 0 o mas veces

#### @(lista-patrones)
se contiene al menos 1 de los patrones. Se separan con `|`

#### !(lista-patrones)
no se contienen los patrones

## subshells
Ya vimos que los ``()`` generan una subshell, es decir, se invoca
otra shell, se ejecuta ahí la orden. Puedo poner `$` para que la stdout
de la subshell se sustituya en la expresió que tengo en mi shell.

Por ejemplo, quiero saber quién es el paquete que instala `python3`.
Eso se hace con `dpkg -S python3`, pero ese `python3` es un patrón de
búsqueda que va a devolver muchos resultados (todos en donde aparezca
python3). Yo quiero solo el paquete que instala `which python3`, que
es el intérprete. Es decir, quiero "pegar" la salida de ese comando
en la entrada del otro. Con subshells, puedo hacerlo con

```bash
dpkg -S "$(which  python3)"
```

## Variables
La shell tiene una serie de variables que determinan su comportamiento.
Se pueden modificar con `set`, de momento no lo necesitamos.

El comando `env` nos dice las variables de entorno y su valor. Las variables
de entorno son valores disponibles para todos los procesos (en un entorno)
del OS.

También se pueden entender las variables de entorno como aquellas "heredables"
por subprocesos de un proceso. Por ejemplo:

```bash
sh -c 'echo valor=$HOME'
variable="No soy de entorno"
sh -c 'echo valor=$aa'
```

sh es unvocar a una subshell y que ejecute un comando; la variable $HOME es de
entorno así que se hereda, la variabla `variable` solo está en el entorno
de la shelll actual así que no se hereda.

Para recuperar el valor de una variable, le pones el `$` delante.
Puedes hacerlo también así, si se va a incluir entre más texto:

```bash
echo "texto${HOME}mastexto"
```

### Predefinidas
Hay muchas: https://tldp.org/LDP/abs/html/internalvariables.html
Que nos puedan resultar útiles:
 
#### $$
dice el PID del proceso actual

#### $?
dice el resultado del último programa ejecutado. Los programas, al ejecutarse,
devuleven 1 byte; normalemnte 0 es ejecutado con éxito; otro número es un
código de error que nos ayuda a entender qué ha fallado.

#### $!
es el PID del último programa ejecutado en backgrouod

#### $HISTFILE
dice donde se almacena el historial de bash

#### $HOME
dice el home del usuario actual

#### $HOSTNAME
dice el nombre de la máquina donde estamos loggeados

#### $OLDPWD
el antiguo pwd

#### $PATH
rutas donde buscar ejecutables. Se separan por `:`. Esto es que
cuando tu ejecutas un programa que se llame `asdf`, ese fichero se va a
buscar en las rutas que diga $PATH

### de usuario
puedes definir tus propias variables (no tiene mucho sentido en sesión
interactiva), con:

```bash
VAR=42
```

Las variables en bash suelen ser todo mayus. Puedes inclusi redefinir
variables predefinidas:
```bash
PATH="~/bin/:$PATH"
```

Eso sí, estas variables no son de entorno (no se hereedan), si quiero que 
se herenden, hay que hacer un export:

```bash
MIVAR=4
export MIVAR
```

puedes hacer lo anterior en una misma linea. Si quiees desexportar, la
opcion es `-n`: `export -n MIVAR`. El listado de las exportadas está en
`export -p`

## Operaciones aritméticas
si quieres hacer operaciones matemáticas, hay que usar ``(())``

```bash
SUMA=$((1+2))
echo $SUMA
((A == 3)) || echo "A no vale 3"
```

## Alias
es un truco para escribir menos, o para hacerte las cosas más fáciles.
Se suelen usar para cosas repetitivas. 
Por ejemplo, el comando ls en realiad es un alias, basta ejecutar
`alias ls` y nos dice qué se está ejecutando de verdad.

Si quiero ejecutar el comando sin alias, hay que usar `\ls` o 
`command ls`.

`alias` a pincho nos dice todos los alias que tenemos definidos.

Si queremos crear alias, lo hacemos con `alias nombre-alias=comando`.

Ojo que esto hace que los alias se "borren" cuando cerramos sesión.

Si quiero que sean permantes (idem para crear variables de entorno),
tengo que hacer la info persistente en los ficheros que lee bash. Son
- /etc/profile. Este fichero se lee independientemente del usuario que abra
    sesión, pero luego se leerá el fichero concreto de usuario `/etc/bash.bashrc`
    que podría machacar el cambio. En cq caso, estos ficheros son de distro
    y es mejor no tocarlos, se proporciona el espacio `/etc/profile.d/` como
    directorio para meter lo que queramos y que se lea (debe ser un fichero
    .sh)
- `~/.bash_profile`, `~/.bash_login`, `~/.profile`. Son archivos de usuario,
    lo normal es que se lea el profile que va a llamar al siguiente:
- `~/.bashrc` -- es el de sesión interactiva, pero no login. Aqui es donde
    deberíamos añadir los alias, o variables de entorno. Pero de hecho no,
    porque convertimos este fichero en un infierno si empezmos a meter
    cosas. Por elo, mejor crear el dir `~.bashrc.d/`, y ahí meter cosas.
    Para que una shell lea comandos de u fichero, necesito el programa
    `source` (a veces puede ser un punto). Si dices `source fichero`
    ejecutará lo que esté escrito en el fichero.
    Con esto, lo que hay que añadir a bashrc es:

```bash
if [ -d ~/.bashrc.d ]; then
   for conf in ~/.bashrc.d/*.sh; do
      source "$conf"
   done
fi
```

entonces eso ejecuta los ficheros .sh y cargo mis alias y variables.

## Redireccionamiento de sdtin/stdout/stderr
Cuando abrimos una shell, tenemos 3 ficheros abiertos de manera
predeterminada: `/dev/stdin, /dev/stdout, /dev/stderr`.

Se corresponden con teclado, pantalla y pantalla.

Para probar que funcionan: el stdout es fácil, podemos hacer `cat` a un
fichero, lo que hce este programa es coger el contenido de un fichero
y sacarlo por stdout (y por eso lo vemos por pantalla).

Si quiero probar la sdtin, también puedo usar `cat`, en este caso sin 
argumentos; este modo de funcionamiento lo que hace es que espera
input (por teclado, ie, stdin), y cuanod lo tiene lo muestra por stdout
(para salir ``ctrl+d``).

Para probar la stderr, basta con cometer un error. Aunque se muetsra por el
mismo dispositivo, no es la misma salida.

Las stds tienen números asociados, 0-in, 1-out, 2-err.

### Redirección de stdin/stderr
Consiste en que la salida no vaya a (la pantalla) si no a otro sitio
(normalemnte un ficheor u otro programa).

Por ejemplo te crear un fichero `/tmp/redireccion-stdout`, y si quieres
que la stdin de un programa no vaya a pantalla si no a ese fichero, dices:

```bash
cat > /tmp/redireccion-stdout
```

Podremos ver (con cat de nuevo) que en vez de escribirse por pantalla,
se alamcena en el fichero.

En general, si rediriges, por defecto machaca lo que había en el fichero;
a veces querrás añadir contenido, para lo cual dices:

```bash
cat >> /tmp/redireccion-stdout
```

También entre aquí en juego el fichero `/dev/null`, que es un agujero negro.
Si no quiero saber la stdout, redirijo aquí.

Ahora, para redirigir la stderr (hasta ahora solo hemos redirigido stdout;
para comprobarlo, hacer `adadfd > /dev/null`), se usa la misma notación
pero dices '2' para que la shell sepa que te refieres explicitamente a la
stderr:

```bash
afffad 2> /dev/null
```

Si quieres redirigir ambas salidas a la vez, tienes el símbolo `&>`. En realidad
lo puedes hacer también con una doble redirección:

```bash
asdadasd > /tmp/redireccion-stdout 2>&1
```

Eso de arriba se lee como: redirige la stout al fichero, y redirige la
stderr a la stdout (hay que poner el & para que se entere). Entonces el 
resultado final es que se escribe el error en el fichero.

Si haces

```bash
asdadasd 2>&1 > /tmp/redireccion-stdout 
```

no funciona igual, porque aquí dices: redirigela stderr a stdout (que
es la pantalla), y luego redirige stdout al fichero (como he dicho primero
lo del error, el error no se escribe en el fichero)

### Redireccionamiento stdin
En ppio un programa (cat, por ejemplo) espera recibir datos por teclado, 
redireccionar la stdin consiste en que reciba esos dtaos por otra via.

Sin que sea una sorpresa, el simbolo es `<`:

```bash
cat < fichero-con-cosas
```

Otra cosa (que ya hicimos) es hacer una redirección con lo que se llama
"here document", que es un "texto largo" de varias lineas. Para eso se 
define un "fin" (normalmente EOF - End Of File) para que la stdin
sepa cuando termina el input:

```bash
cat <<EOF
> linea 1
> linea 2
> linea 3
> EOF
```

Cosas que ocurren aquí: si al EOF lo pongo entre comillas, no se 
interpreta nada:

```bash
cat <<"EOF"
1 + 2 = $((1+2))
EOF
```

```bash
cat <<EOF
1 + 2 = $((1+2))
EOF
```



### pipelines
Ya vimos arriba lo que son, es un redireccionamientdo de stdin y
stdout ambos a la vez. Ojo que no redirecciona stderr, si quieres que lo
haga debes hacer:

```bash
ls /g* 2>&1 | head -n1
```

Hay una orden que se usa mucho con las redirecciones:

#### tee
redirecciona su entrada a stdout y a un fichero que indiquemos:

```bash
ls / | tee /tmp/listado.txt
```

#### named pipelines
Todo lo de las tuberías asume que tanto el programa primero puede escribir
en stdout como que el programa segundo puede leer de stdin; lo cual no 
es siempre cierto. Si tenemos alguno de esos casos, en vez de crear un fichero
de en medio para que uno escriba u el otro lea, puedes crear una named pipeline
que es un fichero especial que hace de "ese fichero del medio". Entocnes haces:

```bash
mkfifo /tmp/pipeline
```

y ahora que he creado la pipeline, si es el primer programa quien necesita
escribir a fichero (no puede escribir a stdout), haremos esta redirección:

```bash
programa1 /tmp/pipeline & programa2 < /tmp/pipeline
```

Si es el programa2 quien no sabe leer de stdin, haces:

```bash
programa1 > /tmp/pipeline & programa2 /tmp/pipeline
```