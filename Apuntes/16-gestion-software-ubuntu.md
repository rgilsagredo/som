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
Es un conjunto de programas/apps, aorganizadas en paquetes, accesibles 
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