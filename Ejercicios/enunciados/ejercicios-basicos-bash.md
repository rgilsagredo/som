# Ejercicios de uso básico de bash

Usar los comandos indicados y buscar en help para averiguar las
opciones necesarias.

## ls
Lista los ficheros de un directorio

- listar los ficheros del directorio /bin
- listar los ficheros del directorio /tmp
- listar los fichero del directorio /etc que empiecen por t en orden inverso
    (hay que usar el wildcard `*`, que quiere decir "cualquier número de
    caracteres". Por ejemplo, `ls ./a*` es lo que empiece por a y 
    detrás cualquier cosa)
- listar los ficheros del directorio /dev que empiecen por tty y tengan
    5 caracteres (hay que usar a wildcard `?`, que quiere decir exactamente
    un caracter. Por ejemplo `a?` encontrará `aa, ab, a0,...`)

- listar todos los archivos del directorio /dev que empiecen por tty y acaben
    en 1,2,3 o 4 (usar la regex [1-4], que es cualqueira de estas opciones)

- listar ficheros del directorio /dev que empiecen por t y acaebn en C1
- listar todos los archivos, incluidos ocultos, de /
- listar todos los archivos de /etc que no empiecen por t (usar el regex
    [^t], que es NO a las opciones que se indican)

- listar todos los archivos de /usr y sus subdirectorios

## cd
para cambiar de directorio

- cabiarse a /tmp
- verificar que se ha hecho el cambio (pwd -- print working directory)
- mostrar dia y hora (date)
- ir a $HOME de un solo comando (nota: HOME es una variable [de usuario]
    y opdemos ver su alor con echo $HOME)
- mostrar los ficheros de $HOME junto con su número de inodo


## mkdir
para crear directorios

- crear un directorio, en tu home, que se llame PRUEBA
- crear, dentro de PRUEBA, dir1 dir2 y dir3
- crear, en dir1, dir11
- crear, en dir3, dir31 y dir32, y en dir31, dir311 y dir312

## touch
crear ficheros vacíos (y otras cosas)

- crear, en PRUEBA, un fichero que se llame hola


## cp
copiar un fichero

- copiar hola a dir312, y cambiarle el nombre a adios
- copiar en dir311 los ficheros de /bin que tengan 'a' como segunda letra
    y su nombre sea de 4 letras
- copiar a dir312 todo lo que esté en /dev que empiecen por t, acaben en
    una letra entre la a y la h, y tengan 5 letras
## mv
mover o renombrar un fichero o directorio
- mover dir31 y todo lo que tenga a dir2
- hacer que el fichero 'adios' sea oculto (un fichero es oculto cuando su
    nombre empieza por '.')
- mover dir312 debajo de dir3

## rm
remove, borrar cosas
- borrar dir1 y todo lo que tenga dentro
- borrar todos los ficheros de 312 que no acaben en b y su cuarta letra
    sea una q
- borrar todo lo creado hasta ahora
