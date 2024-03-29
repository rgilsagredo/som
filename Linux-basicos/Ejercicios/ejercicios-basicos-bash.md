# Ejercicios de uso básico de bash

Usar los comandos indicados y buscar en help para averiguar las
opciones necesarias.

## ls
Lista los ficheros de un directorio

- listar los ficheros del directorio /bin
```console
ls /bin
```

- listar los ficheros del directorio /tmp
```console
ls /tmp
```

- listar los fichero del directorio /etc que empiecen por t en orden inverso
    (hay que usar el wildcard `*`, que quiere decir "cualquier número de
    caracteres". Por ejemplo, `ls ./a*` es lo que empiece por a y 
    detrás cualquier cosa)
```console
ls --reverse /etc/t*
```

- listar los ficheros del directorio /dev que empiecen por tty y tengan
    5 caracteres (hay que usar a wildcard `?`, que quiere decir exactamente
    un caracter. Por ejemplo `a?` encontrará `aa, ab, a0,...`)

```console
ls /dev/tty??
```

- listar todos los archivos del directorio /dev que empiecen por tty y acaben
    en 1,2,3 o 4 (usar la regex [1-4], que es cualqueira de estas opciones)
```console
ls /dev/tty*[1-4]
```

- listar ficheros del directorio /dev que empiecen por t y acaebn en 1
```console
ls /dev/t*1
```

- listar todos los archivos, incluidos ocultos, de /
```console
ls --all /
```

- listar todos los archivos de /etc que no empiecen por t (usar el regex
    [^t], que es NO a las opciones que se indican)

```console
ls /etc/[^t]*
```

- listar todos los archivos de /usr y sus subdirectorios
```console
ls --all --recursive /usr
```

## cd
para cambiar de directorio

- cabiarse a /tmp
```console
cd /tmp
```

- verificar que se ha hecho el cambio (pwd -- print working directory)
```console
pwd
```
- mostrar dia y hora (date)
```console
date
```
- ir a $HOME de un solo comando (nota: HOME es una variable [de usuario]
    y opdemos ver su alor con echo $HOME)
```console
cd
```
```console
cd $HOME
```
- mostrar los ficheros de $HOME junto con su número de inodo
```console
ls --inode $HOME
```

## mkdir
para crear directorios

- crear un directorio, en tu home, que se llame PRUEBA
```console
mkdir PRUEBA
```
- crear, dentro de PRUEBA, dir1 dir2 y dir3
```console
mkdir ./PRUEBA/dir1 ./PRUEBA/dir2 ./PRUEBA/dir3
```
- crear, en dir1, dir11
```console
mkdir ./PRUEBA/dir1/dir11
```
- crear, en dir3, dir31 y dir32, y en dir31, dir311 y dir312
```console
mkdir ./PRUEBA/dir3/dir31 ./PRUEBA/dir3/dir32
mkdir ./PRUEBA/dir3/dir31/dir311 ./PRUEBA/dir3/dir31/dir312
```

## touch
crear ficheros vacíos (y otras cosas)

- crear, en PRUEBA, un fichero que se llame hola
```console
touch ./PRUEBA/hola
```


## cp
copiar un fichero

- copiar hola a dir312, y cambiarle el nombre a adios
```console
cp ./PRUEBA/hola ./PRUEBA/dir3/dir31/dir312/adios
```
- copiar en dir311 los ficheros de /bin que tengan 'a' como segunda letra
    y su nombre sea de 4 letras
```console
cp /bin/?a?? ./PRUEBA/dir3/dir31/dir311/
```

- copiar a dir312 todo lo que esté en /dev que empiecen por t, acaben en
    un numero entre 1 y 3, y tengan 5 letras

```console
cp /dev/t???[1-3]
```
## mv
mover o renombrar un fichero o directorio

- mover dir3 y todo lo que tenga a dir2
```console
mv ./PRUEBA/dir3/dir3 ./PRUEBA/dir2
```
- hacer que el fichero 'adios' sea oculto (un fichero es oculto cuando su
    nombre empieza por '.')
```console
mv ./PRUEBA/dir2/dir3/dir31/dir312/adios ./PRUEBA/dir2/dir3/dir31/dir312/.adios
```
- mover dir312 debajo de dir3
```console
mv ./PRUEBA/dir2/dir3/dir31/dir312 ./PRUEBA/dir2/dir3/
```

## rm
remove, borrar cosas
- borrar dir1 y todo lo que tenga dentro
```console
rm -r ./PRUEBA/dir1
```
- borrar todos los ficheros de 312 que no acaben en b y su cuarta letra
    sea una q
```console
rm ./PRUEBA/dir2/dir3/dir312/???q*[^b]
```
- borrar todo lo creado hasta ahora
```console
rm -r ./PRUEBA
```
