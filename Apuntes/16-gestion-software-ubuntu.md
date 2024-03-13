# Gestión software
*NOTA IMPORTANTE:*

La gestión del software en un OS en general es siempre concreta de ese OS.
Quiero decir, que aunque hasta este momento hemos usado cosas que son
generales para cualqueir districbución de Linux, para este apartado
lo que vamos a ver es específico de la distro Debian (y subdistros, como
Ubuntu).

## Conceptos de paquetes
Conceptos necesarios para entender la gestión del software.

### Sistema de paquetes
Es un conjunto de programas/apps, organizadas en paquetes, accesibles 
via servidores, estructuradas para que sea fácil la instalación/desinstalación
y actualización. 

### Paquete
Es un conjunto de ficheros binarios, docs y configs, relacionados, que
constituyen un programa o parte de un programa.

### Repo(sitorio)
Es un almacén de paquetes. Puede ser un CD, server web, server FTP. La idea
es que los paquetes quedan accesibles a sistemas cliente para que pueda
instalar/acualizar.

### Gestor de paquetes
Es un programa que se encarga de instalar/desinstalar/actualizar paquetes

## Tipos de paquetería
Hay 2 tipos: la propia (de la distro) y la universal. Realmente hay otro tipo
de "paquetería" pero que no es paquetería en sí: es el hacer tú todo el
trabajo de la paquetería, ie, instalar a mano.

La paquetería universal intentará instalar todo en de tal manera que
no interfiera con el sistema (ejemplo tonto: si el software que estoy instalando
necesita un programa que copie archivos, no va a reciclar el `cp` que ya
tengo de mi distro, si no que va a instalar su propio programa y usar ese),
la paquetería de distro intentará reciclar lo que ya tiene en sistema. Además,
instalará las cosas donde se espera que se instalen (ie un ejecutable irá
en `/usr/bin/`, un fichero de config irá en `/etc/`). 

No se pueden mezclar paqueterías entre distros, pero sís e puede mezclar
la de la distro con la universal.

**Importante**: los paquetes suelen tener muchas interdependencias. Al final
es porque en el desarrollo de software siempre construyes cosas nuevas basándote
en cosas que ya se han hecho, o un mismo software básico puede ser usando
como "materia prima" de muchos otros programas, por tanto es compartido.

Se intenta siempre que se separen las dependencias de lo que es el programa
final, así se evitan duplicidades. Por tanto lo que se suele hacer es
que no se incluyen en el paquete las dependencias, si no una dependencia del
paquete que instala las dependencias.

Así el sistema de paquetería evoluciona en conjunto: se actualiza una librería:
de gratis todos los programas que cuelgan de esa librería están atualizados.

## Maneras de instalar software
Para poder usar un programa concreto, primero tenemos que instalarlo en el
disco. Tenemos distintas maneras de hacer eso:

### Paquetería propia
La propia distro proporciona un sistema de paquetes. El software en las
distros de linux suele ser libre; los que mantienen la distro se encargan
de seleccionar qué software se inlcuye en la apquetería,
lo adpatan a la distro y lo ponen a disposición de los usuarios.

La paquetería propioa de la distro debe ser siempre el sistema preferente 
de instalación de software. Asegura que el software que voy a instalar es 
compatible con el resto del sistema, minimiza la duplicidad de librerías
(ya que cada software sabrá donde encontrar las dependencias que necesita
dentro del propio sistema), y las actualizaciones son cómodas.

### Paquetería universal
En este tipo de paquetería lo que se suele hacer es propiocionar
todo el software necesario (ie app+dependencias) sin tener en cuenta lo que
ya tiene el OS. Además, la instalación se hace de tal manera que no interfiera
con lo que ya hay en el OS (puede que tengamos duplicidad de software).

Se suele proporcionar una manera de gestionar los pquetes y sus actualizaciones,
pero la integración con el resto del sistema es menor y tendremos duplicidad
de librerías. Pero es la opción a usar si la distro no tiene el paquete
que queremos  o neceistamos una visrión diferente a la que da la distro
de un programa concreto;  también que la distro no propiociona las dependencias
para la app.

Hay varios sitemas de paquetería universal: Flatpak (ventaja: sus paquetes
tienen librerías comunes, entonces el primer paquete suele constar bastante
instalarlo, porque va a ir acompañado de muchas cosas, pero los siguientes ya
tendrán casi tdo lo que necesitan disponible), Snap (es la asociada con Ubuntu),
Applmage (sigue la filosofía de Apple: no instala nada, si no que es 
un fichero que se monta, tiene todo lo que se necesita para correr la app,
y cuando no lo quieres usar, se desmonta), Docker (técnicamente no instala
software, lo que hace es que crea un "OS" aislado donde se instala el software)

### Instalación manual
No aconsejable, ya que te tienes que encargar de todo: instalar (que te va a dar
problemas de dependencias), mantener y actualizar.

Si tengo que hacer instalaciópn manual, a veces tengo el paquete en la paquetería
propia o algo univeresal, a veces tengo un script (des)instalador (cons: posibles
incompatibilidades de dependencias, los gestores no saben de este software,
mantener y actualizar es manual), a veces tengo el source code y te toca
compilar.

## Ejemplos de instalación de software

### Paquetería propia
Vamos a instalar el JDK (Java Development Kit, es el software que se neceista
para desarrollar programas en Java), que se proporciona con la propia 
distro de Ubuntu.

Primero, vemos sí ya está instalado con

```bash
which javac
javac -version
```

Estos comandos deberían decir que no está instalado, y decirnos cómo instalarlo.
Entonces, solo hay que ejecutar el comando:

```bash
sudo apt update && sudo apt install default-jdk -y
```

Esto nos ha instalado la versión 11 del JDK (bastante desactualizado).

### Paquetería universal
Vamos a usar flatpak para el ejemplo; primero hay que ver si está instañlado:

```bash
which flatpak
```

y si no está, instalarlo (flatpak está en la distro):

```bash
sudo apt install flatpak -y
```

Luego, tenemos que decirle a flatpak dónde encontrar software, es decir,
su repo. Para ello:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

**NOTA**: parece que hay algún error con esta versión del software que no
permite buscar; la solución es la siguiente:

```bash
sudo add-apt-repository ppa:flatpak/stable
sudo apt update && sudo apt upgrade -y
```

que esencialmente lo que hace es decirle a apt que la versión buena de flatpak
está en ese repo.

Ahora podemos buscar software en ese repo, por ejemplo

```bash
flatpak search chrome
```

E instalar con:

```bash
flatpak install flathub com.google.Chrome
```

Ver que lo hemos instalado con

```bash
flatpak list
```

Y ejecutarlo con

```bash
flatpak run com.google.Chrome
```

### Instalación manual con paquetes de la distro
Vamos a instalar ahora manualmente Chrome usando paquetes de la distro
(.deb). Primero tenemos que encontrar y descargar esos paquetes; basta
saber hacer una búsqueda por Google o tu buscador favorito.

Te desacragas el .deb, vemos que lo tenemos en `~/Downloads`.
Una vez que estamos ahí, basta con usar el gestor `dpkg`:

```bash
sudo dpkg --install nombre-del-paquete.deb
```

cuando termine, podemos eliminar el .deb, y comprobar que el programa
se ha instalado con `which google-chrome`


### Instalación manual desde source code
Primero tenemos que descaragr el código. Para el ejemplo, vamos a instalar
VIM manualmente. Buscamos el repo de github con el SC. Descargamos,
descomprimimos (debería haberse descargado un .zip, para descomprimir,
tenemos la utilidad `unzip`)

Una vez descargado, vamos a la carpeta unzippeada, y si se han hecho
las cosas bien, suele venir un fichero ejecutable llamado `config` o similar
que nos prepara el equipo para hacer el build.

En mi caso, al ejecutar `./configure`, me informa de que me falta una 
librería por intalar, la instal con:

```bash
sudo apt install libncurses-dev
```

Tras que el fichero de config me de el OK, podemos pasar a compilar el
código fuente con

```bash
sudo make
```

o podemos usar `sudo make -j $(nproc)` para que use todos los procesadores
y vaya más rápido. Cuando ha compilado, instalamos con

```bash
sudo make install
```

Y si no hay problemas, lo tendremos instalado. Podemos verlo con `which`


## Gestores de paquetes
Son aplicaciones que gestionan un sistema de paquetes concretos. Por ejemplo,
apt es la que gestiona los paquetes .deb, flatpak es la que gestiona los
paquetes tipo .flatpak. Existen algunas apps que son "multipaquetería".

Ahora vamos a ser muy *Debian* (y derivados, como Ubuntu) específicos.
Los gestores que tenemos son 

### dpkg
más que un gestor, es la base para instalar, actualizar y borrar paquetes .deb
Es lo que usan los gestores de aquí abajo

### apt
Antes existía una familia de app `apt-*`, se simplificó en una única que es la
`apt`.

#### Básicos
Suponiendo que tenemos un repo válido, lo primero es siemrpe actualizar
el listado de paquetes con `apt update`. Siempre es recomendable hacer esto
pues lo normal es que los paquetes se actualicen a menudo.

Tras actualizar el listado, podemos actualizar en nuestro sistema un 
paquete o todos a su nueva versión con `apt upgrade`. Esta es la versión
no traumática, ie, cuando no hay un cambio en la estructura de paquetes
significativo. Pero puede que sí lo haya (el equipo que mantiene el
sistema de paquetes ha decidido cambiar cosas; tengo que borrar o mover
ciertos paquetes). Para eso tienes `apt dist-upgrade` (ojo que en distros
estables como Ubuntu20.04, esto no tiene sentido, las actualizaciones serán
siemrpe de seguridad o fix)

Para instalr software, basta con `apt intall <nombre-del-programa>`

Para desinatalar, `apt remove` o `apt purge`. La diferencia es que remove
deja los fichero sde config del programa por si se vuelve a reinstalar en el 
futuro, no tener quw volver a hacer la config; pero si quiero elminiar todo
lo sociado con el programa purge es tu opción.  

Ojo que al desinatalr un programa puede que se nos quede algún paquete del 
que se dependía por ahí suelto. El gestor sabe de esto, ya que marca 
los paquetes instalados como `manual` o `auto`, los auto son justamente los que
se instalan solo para satisfacer dependencias.

También puede que tengamos huérfanos por ahí, para elminiarlos, `apt autoremove`,
pero podemos incluir esta opción en las operaciones de install/upgrade/remove/purge
para no tener que andar haciendo esto todo el rato.

Para saber el listado de paquetes instalados, `apt list`, que puedes combinar con
patronesd e búsqueda como `apt list pyt?on*`. Tienes las opciones de 
`--installed` o `--upgradable` para filtrar más.

### otras de Debian
puede que se encuentre en algún sistema antiguo cosas tipo `dselect`, `aptitude`
o `synaptic`

### otras de otras distros
yum para RedHat, zypper para Suse, pacman para Arch...

## Repos
Lo habitual es que los paquetes se obtengan de internet. Para que apt
o tu gestor favorito sepa donde buscar, suele haber definido
un fichero con las URL de los repos. En caso de apt, está en
`/etc/apt/sources.list`

Cada linea del fichero es un repo donde buscar paquetes, la info se almacena
de la siguiente manera:
lo primero es `deb` o `deb-src`; en el seundo caso queire decir que es
SC y tocará compilar. Lo siguiente es la URL del repo. El siguiente campo
es el nombre de la distro (la 20.04 es Focal Fossa) quizás con nombre de 
rama (por ejemplo updates). Lo último se llama componente, suele ser siempre
`main`.

Este fichero es de la distro y no se debe tocar, si quiero añadir mis
propios repos, lo hago en un fichero en esta ruta:
```bash
/etc/apt/sources.list.d/nombre-fichero.list
```

### Ejemplo
Vamos a instalar VSCode añadiendo el repo de la deescarga. Ignorar de momento
las cosas extra. Primero, necesitamos ciertos paquetes:

```bash
sudo apt install software-properties-common apt-transport-https wget -y
```

Segundo, necesitamos la GPG:

```bash
wget -O- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/vscode.gpg
```

Finalente, estamos donde queremos. Vamos a añadir el repo donde está el programa
a un listado para apt con:

```bash
echo deb [arch=amd64 signed-by=/usr/share/keyrings/vscode.gpg] https://packages.microsoft.com/repos/vscode stable main | sudo tee /etc/apt/sources.list.d/vscode.list
```

Tras esto apt ya sabe buscar el programa (primero hay que apt update), 
y podremos instalarlo con `apt install code`


### Repos extraoficiales
Como el de MS que hemos usado antes, hemos tenido que añadir una PubKey GPG
para que sea confiable. Es una operación bastante común, simplemente añade
la PubKey a listado de claves de apt y listo

## Acciones sobre paquetes
Los ficheros de un paquete se obtienen con `dpkg -L <paquete>`.
Podemos ir al revés, ver qué paquete instala un programa concreto:

```bash
dpkg -S $(which python3)
```

*NOTA*: como `/bin` dejó de existir y es solo un symlink a `/usr/bin`,
a veces parece que no podemos encontrar qué paquetes instalan
cierto software (por ejemplo ls, pq el paquete que lo instala dice
/bin y no /usr/bin), entonces se suele añadir `which -a` para que busque mejor.

dpkg también ayuda a instalar un .deb descargado a mano con
```bash
dpkg --install pak.deb
```

Ojo que esta opción es normal que me instale cosas inutilizables porque 
el paquete tenía unas dependencias que no tnego instaladas; para solocionar ese
problema, `apt -f intall` se encargará de instalarnos las dependencias.
Pero en general es mejor no hacer eso y usar `apt install pak.deb` que
ya hará todo de una.

Para saber las dependencias de un paquete, `apt depends <nombre-paquete>`,
y sus dependentes con `apt rdepends <nombre-paquete>` (los que dice aquí
son todos sus dependeientes, no tengo por qué tenerlos instalados)

Si quiero saber por qué un paquete está instalado, usa este comando:

```bash
apt rdepends --installed --no-suggests --no-breaks --no-replaces --recurse <paquete>
```

*Podemos hacer el ejemplo con policykit-1*

y nos dará un listado de todas las reverse-dependencies (es decir, las cosas
instaladas que al final del día dependen de esto).

Además con `apt list <paquete>` ppodemos ver si el apquete concreto se
instaló manual o auto. Eso se llaman marcas. Los marcados como auto se instalan
para satisfacer dependencias. Si quitamos un paquete que es una dependencia
de un programa que usemos, el programa ya no va a funcionar.

Los marcados
 como manual es que dijomos explícitamente que queríamos instalarlos.

También se puede marcar con paquete como `holded` y nunca se actualizará
ni borrará.

Para marcar, usamos `apt-mark auto/manual/hold/unhold <paquete>`

Tenemos también más opciones como `apt-mark showmanual/showauto/showhold`
para obtener listados. También existen `showinstall/showremove/showpurge`

