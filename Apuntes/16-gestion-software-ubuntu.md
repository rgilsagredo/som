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

